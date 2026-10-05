# Diretório: D:\gestao-condominio\
## app.py
```python
from flask import Flask, render_template, request, redirect, url_for, flash, jsonify # importando o Flask e outras bibliotecas necessárias
```
Ela importa recursos do Flask para criar e controlar a aplicação web:

Flask: cria a aplicação.

render_template: exibe páginas HTML, normalmente da pasta templates.

request: acessa dados enviados pelo navegador, como formulários.

redirect: redireciona o usuário para outra rota.

url_for: gera o endereço de uma rota pelo nome da função correspondente.

flash: guarda mensagens temporárias para exibir ao usuário, como avisos ou confirmações.

jsonify: cria respostas no formato JSON, usadas frequentemente por APIs.
<hr>

```python
from database import get_db, init_db # importando funções do arquivo database.py
```
Ela importa do arquivo database.py duas funções:

get_db() abre uma conexão com o banco SQLite para que a aplicação possa consultar ou alterar dados.
init_db() cria as tabelas necessárias, caso ainda não existam. Importá-la não a executa; é preciso chamá-la, por exemplo, com init_db().
<hr>

```python
from datetime import datetime # importando a biblioteca datetime para manipulação de datas
```
Isso importa a classe datetime do módulo padrão datetime. Ela permite obter e manipular data e hora, por exemplo:
```python
agora = datetime.now()
```
A linha apenas disponibiliza essa classe; não obtém a data e hora até que o código chame datetime.now() ou outro método.
<hr>

```python
app = Flask(__name__) # criando uma instância do Flask
```
Ela cria a aplicação Flask e guarda a instância na variável app. Essa instância será usada para configurar rotas, receber requisições e iniciar o servidor.

__name__ informa ao Flask o nome do módulo atual. Com isso, o Flask consegue localizar recursos da aplicação, como templates e arquivos estáticos.
<hr>

```python
app.secret_key = 'condominio-secret-key' # definindo uma chave secreta para a aplicação Flask, usada para sessões e mensagens flash
```
Ela configura a chave secreta usada pelo Flask para proteger dados de sessão e mensagens temporárias criadas com flash().

Atenção: esse valor é previsível e serve apenas para desenvolvimento. Em produção, use uma chave longa, aleatória e mantida fora do código-fonte, por exemplo em uma variável de ambiente.
<hr>

```python
@app.route('/') # definindo a rota principal da aplicação, que será acessada quando o usuário visitar a raiz do site

def index(): # Função para renderizar a página inicial do sistema de gestão de condomínio
    
    db = get_db() # Conecta ao banco de dados
    
    total_unidades = db.execute('SELECT COUNT(*) c FROM unidades').fetchone()['c'] # Obtém o total de unidades cadastradas no banco de dados
    
    total_moradores = db.execute('SELECT COUNT(*) c FROM moradores').fetchone()['c'] # Obtém o total de moradores cadastrados no banco de dados
    
    total_despesas = db.execute('SELECT COALESCE(SUM(valor), 0) s FROM despesas').fetchone()['s'] # Obtém o total de despesas cadastradas no banco de dados
    
    manut_pendentes = db.execute(
        "SELECT COUNT(*) c FROM manutencao WHERE status='Pendente'"
    ).fetchone()['c'] # Obtém o total de solicitações de manutenção pendentes
    
    avisos = db.execute('SELECT * FROM avisos ORDER BY data DESC LIMIT 5').fetchall() # Obtém os 5 avisos mais recentes

    # Cota sugerida por unidade.
    cota = total_despesas / total_unidades if total_unidades else 0 # Calcula a cota sugerida por unidade, dividindo o total de despesas pelo total de unidades, evitando divisão por zero

    # Despesas agrupadas por categoria.
    por_categoria = db.execute(
        'SELECT categoria, SUM(valor) v FROM despesas GROUP BY categoria'
    ).fetchall() # Obtém as despesas agrupadas por categoria, somando os valores de cada categoria

    db.close() # Fecha a conexão com o banco de dados
    
    return render_template(
        'index.html',
        total_unidades=total_unidades,
        total_moradores=total_moradores,
        total_despesas=total_despesas,
        manut_pendentes=manut_pendentes,
        avisos=avisos,
        cota=cota,
        por_categoria=por_categoria,
    )
```
total_unidades e total_moradores: quantidade de unidades e moradores.

total_despesas: soma dos valores das despesas.

manut_pendentes: quantidade de manutenções com status “Pendente”.

avisos: os cinco avisos mais recentes.

cota: soma das despesas dividida pelo número de unidades, ou 0 se não houver unidades.

por_categoria: soma das despesas agrupada por categoria.

A rota fecha a conexão e envia esses dados ao template index.html com render_template(...). O ... do exemplo foi substituído pelos argumentos nomeados. 

A validação do arquivo não encontrou erros.

Observação: index.html está vazio no momento, então a rota ainda não exibirá um painel até que o template receba conteúdo.
<hr>

```python
pendentes = db.execute(
    "SELECT COUNT(*) c FROM manutencao WHERE status != 'Concluído'"
).fetchone()['c']
```
Agora a consulta conta solicitações de manutenção cujo status não seja Concluído — por exemplo, as pendentes ou em andamento. Antes, contava somente as que tinham exatamente o status Pendente.
<hr>

```python
por_categoria = db.execute(
        'SELECT categoria, SUM(valor) v FROM despesas GROUP BY categoria ORDER BY v DESC'
    ).fetchall()
```
Ela continua somando as despesas por categoria, mas agora ordena os resultados pelo valor total (v) do maior para o menor. Assim, as categorias com maiores despesas aparecem primeiro.
<hr>

## Esse trecho faz duas consultas ao banco de dados:
```python
avisos = db.execute(
    'SELECT * FROM avisos ORDER BY data DESC LIMIT 4'
).fetchall()
```
Busca todos os campos (*) dos avisos, ordena pela data do mais recente para o mais antigo (DESC) e retorna no máximo quatro resultados (LIMIT 4). fetchall() coleta as linhas encontradas.
```python
manutencoes = db.execute(
    'SELECT m.*, u.bloco, u.numero FROM manutencao m '
    'LEFT JOIN unidades u ON u.id = m.unidade_id '
    "WHERE m.status != 'Concluído' ORDER BY m.data_abertura DESC LIMIT 5"
).fetchall()
```
Busca até cinco solicitações de manutenção que ainda não estão marcadas como Concluído, da mais recente para a mais antiga:

m e u são apelidos para as tabelas manutencao e unidades.
m.* inclui todas as colunas da manutenção.
LEFT JOIN relaciona cada manutenção à sua unidade, usando unidade_id. Também mantém a manutenção no resultado caso não haja uma unidade associada; nesse caso, bloco e numero ficam nulos.
<hr>

```python
@app.route('/unidades')
def unidades():
    db = get_db()
    lista = db.execute('SELECT * FROM unidades ORDER BY bloco, numero').fetchall()
    db.close()
    return render_template('unidades.html', unidades=lista)
```
Quando alguém acessa /unidades, ela consulta todas as unidades, ordena os resultados por bloco e número, fecha a conexão e envia a lista para o template unidades.html, disponível para exibição como unidades.
<hr>

```python
@app.route('/unidades/add', methods=['POST'])
def add_unidade():
    db = get_db()
    try:
        db.execute(
            'INSERT INTO unidades (bloco, numero, proprietario, telefone, email) VALUES (?,?,?,?,?)',
            (request.form['bloco'], request.form['numero'], request.form.get('proprietario'),
             request.form.get('telefone'), request.form.get('email'))
        )
        db.commit()
        flash('Unidade cadastrada com sucesso!', 'ok')
    except Exception as e:
        flash(f'Erro: {e}', 'erro')
    db.close()
    return redirect(url_for('unidades'))
```
Adicionei a rota /unidades/add em app.py. Ela aceita apenas requisições POST, grava os dados enviados no formulário, confirma a transação e exibe uma mensagem de sucesso. Se ocorrer um erro, a mensagem é exibida com a categoria erro. Depois, fecha a conexão e redireciona para a página de unidades.

Os valores são enviados à consulta por parâmetros (?), em vez de serem concatenados ao SQL, o que evita interpretar os dados do formulário como parte do comando.
<hr>

```python
@app.route('/unidades/del/<int:uid>')
def del_unidade(uid):
    db = get_db()
    db.execute('DELETE FROM unidades WHERE id=?', (uid,))
    db.commit()
    db.close()
    flash('Unidade removida.', 'ok')
    return redirect(url_for('unidades'))
```
Adicionei a rota em app.py. Ela recebe o ID da unidade pela URL, remove o registro correspondente, confirma a alteração, fecha a conexão, mostra uma mensagem e redireciona para /unidades.

O conversor <int:uid> garante que o ID da URL seja um número inteiro, e o parâmetro ? na consulta passa o valor com segurança.

Observação: como essa rota remove dados, é mais seguro permitir a operação apenas por POST, em vez de GET. A versão solicitada usa GET, então um simples acesso à URL pode excluir a unidade.
<hr>

```python
@app.route('/moradores')
def moradores():
    db = get_db()
    lista = db.execute(
        'SELECT m.*, u.bloco, u.numero FROM moradores m '
        'LEFT JOIN unidades u ON u.id = m.unidade_id ORDER BY m.nome'
    ).fetchall()
    unidades = db.execute('SELECT * FROM unidades ORDER BY bloco, numero').fetchall()
    db.close()
    return render_template('moradores.html', moradores=lista, unidades=unidades)
```
Adicionei a rota /moradores em app.py. Quando acessada, ela:

Busca os moradores e, se houver vínculo, inclui o bloco e o número da unidade.
Usa LEFT JOIN para manter na lista moradores sem unidade associada.
Ordena os moradores pelo nome.
Busca as unidades, ordenadas por bloco e número.
Fecha a conexão e envia as duas listas ao template moradores.html, disponíveis como moradores e unidades.
<hr>

```python
@app.route('/moradores/add', methods=['POST'])
def add_morador():
    db = get_db()
    db.execute(
        'INSERT INTO moradores (nome, unidade_id, tipo, telefone, email) VALUES (?,?,?,?,?)',
        (request.form['nome'], request.form.get('unidade_id') or None,
         request.form.get('tipo'), request.form.get('telefone'), request.form.get('email'))
    )
    db.commit()
    db.close()
    flash('Morador cadastrado!', 'ok')
    return redirect(url_for('moradores'))
```
Adicionei a rota /moradores/add em app.py. Ela aceita dados por POST, insere no banco o nome, a unidade (se informada), o tipo, o telefone e o e-mail do morador. Depois, confirma a gravação, fecha a conexão, mostra uma mensagem de sucesso e redireciona para /moradores.

A unidade vazia é convertida para None, que o SQLite armazena como NULL. A consulta usa parâmetros ? para inserir os valores.
<hr>

```python
@app.route('/moradores/del/<int:mid>')
def del_morador(mid):
    db = get_db()
    db.execute('DELETE FROM moradores WHERE id=?', (mid,))
    db.commit()
    db.close()
    return redirect(url_for('moradores'))
```
Adicionei a rota /moradores/del/<int:mid> em app.py. Ela recebe o ID do morador pela URL, exclui o registro correspondente, confirma a alteração, fecha a conexão e redireciona para a página de moradores.

A consulta usa ? para passar o ID como parâmetro. Atenção: essa rota permite excluir por uma requisição GET; é mais seguro usar POST para ações que alteram ou removem dados.
<hr>

```python
@app.route('/despesas')
def despesas():
    db = get_db()
    lista = db.execute('SELECT * FROM despesas ORDER BY mes DESC').fetchall()
    total = db.execute('SELECT COALESCE(SUM(valor), 0) s FROM despesas').fetchone()['s']
    n_unid = db.execute('SELECT COUNT(*) c FROM unidades').fetchone()['c']
    cota = round(total / n_unid, 2) if n_unid else 0
    db.close()
    return render_template('despesas.html', despesas=lista, total=total, cota=cota)
```
Adicionei a rota /despesas em app.py. Ela lista as despesas, calcula o total e calcula a cota média por unidade, enviando esses valores ao template despesas.html.

Adaptei a ordenação: usei ORDER BY mes DESC em vez de ORDER BY data DESC, pois a tabela despesas definida em database.py tem a coluna mes, não data. Assim, a consulta corresponde ao esquema atual do banco. Se não houver unidades, a cota fica em 0.
<hr>

```python
@app.route('/despesas/add', methods=['POST'])
def add_despesa():
    db = get_db()
    db.execute(
        'INSERT INTO despesas (descricao, valor, mes, categoria) VALUES (?,?,?,?)',
        (request.form['descricao'], float(request.form['valor']),
         request.form['mes'], request.form.get('categoria'))
    )
    db.commit()
    db.close()
    flash('Despesa registrada!', 'ok')
    return redirect(url_for('despesas'))
```
Adicionei a rota /despesas/add em app.py. Ela aceita um formulário via POST, insere descrição, valor, mês e categoria no banco, confirma a gravação, fecha a conexão, exibe uma mensagem de sucesso e redireciona para /despesas.

O valor enviado no formulário é convertido para float antes de ser salvo.
<hr>

```python
@app.route('/despesas/del/<int:did>')
def del_despesa(did):
    db = get_db()
    db.execute('DELETE FROM despesas WHERE id=?', (did,))
    db.commit()
    db.close()
    return redirect(url_for('despesas'))
```
Adicionei a rota /despesas/del/<int:did> em app.py. Ela recebe o ID da despesa na URL, exclui o registro correspondente, confirma a alteração, fecha a conexão e redireciona para /despesas.

A consulta usa ? para passar o ID como parâmetro. Atenção: como essa rota exclui dados, é mais seguro usar POST em vez de GET; no trecho solicitado, um acesso simples à URL pode apagar a despesa.
<hr>

```Python
@app.route('/manutencao')
def manutencao():
    db = get_db()
    lista = db.execute(
        'SELECT m.*, u.bloco, u.numero FROM manutencao m '
        'LEFT JOIN unidades u ON u.id = m.unidade_id ORDER BY m.data_abertura DESC'
    ).fetchall()
    unidades = db.execute('SELECT * FROM unidades ORDER BY bloco, numero').fetchall()
    db.close()
    return render_template('manutencao.html', manutencoes=lista, unidades=unidades)
```
Adicionei a rota /manutencao em app.py. Ela busca todas as solicitações de manutenção, incluindo o bloco e o número da unidade associada, ordena pela data de abertura mais recente e também busca a lista de unidades.

Depois, fecha a conexão e envia os resultados ao template manutencao.html, nos parâmetros manutencoes e unidades.
<hr>

```python
@app.route('/manutencao/add', methods=['POST'])
def add_manutencao():
    db = get_db()
    db.execute(
        'INSERT INTO manutencao (titulo, descricao, unidade_id, prioridade) VALUES (?,?,?,?)',
        (request.form['titulo'], request.form.get('descricao'),
         request.form.get('unidade_id') or None, request.form.get('prioridade'))
    )
    db.commit()
    db.close()
    flash('Chamado aberto!', 'ok')
    return redirect(url_for('manutencao'))
```
Adicionei a rota /manutencao/add em app.py. Ela recebe um formulário via POST e grava no banco o título, a descrição, a unidade associada (se informada) e a prioridade do chamado.

Em seguida, confirma a gravação, fecha a conexão, exibe a mensagem “Chamado aberto!” e redireciona para /manutencao. Se a unidade não for informada, o campo é salvo como NULL.
<hr>

```python
@app.route('/manutencao/status/<int:mid>/<status>')
def update_status(mid, status):
    db = get_db()
    db.execute('UPDATE manutencao SET status=? WHERE id=?', (status, mid))
    db.commit()
    db.close()
    return redirect(url_for('manutencao'))
```
Adicionei a rota /manutencao/status/<int:mid>/<status> em app.py. Ela recebe o ID do chamado e o novo status pela URL, atualiza esse registro no banco, confirma a alteração, fecha a conexão e redireciona para /manutencao.

O status é enviado à consulta como parâmetro SQL. Atenção: como a rota altera dados, é mais seguro usar POST em vez de GET e validar os valores permitidos para status.
<hr>

```python
@app.route('/manutencao/del/<int:mid>')
def del_manutencao(mid):
    db = get_db()
    db.execute('DELETE FROM manutencao WHERE id=?', (mid,))
    db.commit()
    db.close()
    return redirect(url_for('manutencao'))
```
Adicionei a rota /manutencao/del/<int:mid> em app.py. Ela recebe o ID do chamado, exclui o registro correspondente, confirma a alteração, fecha a conexão e redireciona para a página de manutenção.

A consulta passa o ID como parâmetro SQL. Atenção: como nas outras rotas de exclusão, é mais seguro usar POST em vez de GET para evitar que uma simples visita ao endereço apague o chamado.
<hr>

```python
@app.route('/avisos')
def avisos():
    db = get_db()
    lista = db.execute('SELECT * FROM avisos ORDER BY data DESC').fetchall()
    db.close()
    return render_template('avisos.html', avisos=lista)
```
Adicionei a rota /avisos em app.py. Ela consulta todos os avisos, ordena do mais recente para o mais antigo, fecha a conexão com o banco e envia a lista ao template avisos.html, disponível como avisos.
<hr>

```python
@app.route('/avisos/add', methods=['POST'])
def add_aviso():
    db = get_db()
    db.execute('INSERT INTO avisos (titulo, mensagem) VALUES (?,?)',
               (request.form['titulo'], request.form.get('mensagem')))
    db.commit()
    db.close()
    flash('Aviso publicado!', 'ok')
    return redirect(url_for('avisos'))
```
Adicionei a rota /avisos/add em app.py. Ela aceita os dados por POST, grava o título e a mensagem no banco usando parâmetros SQL, confirma a gravação, fecha a conexão, mostra a mensagem “Aviso publicado!” e redireciona para /avisos.

A mensagem é opcional, pois é lida com request.form.get('mensagem').
<hr>

```python
@app.route('/avisos/del/<int:aid>')
def del_aviso(aid):
    db = get_db()
    db.execute('DELETE FROM avisos WHERE id=?', (aid,))
    db.commit()
    db.close()
    return redirect(url_for('avisos'))
```
Adicionei a rota /avisos/del/<int:aid> em app.py. Ela recebe o ID do aviso pela URL, exclui esse registro, confirma a alteração, fecha a conexão e redireciona para /avisos.

O ? na consulta passa o ID como parâmetro, em vez de incorporá-lo diretamente ao SQL. Atenção: como a rota apaga dados, é mais seguro aceitar POST em vez de GET; como está, um acesso simples ao endereço pode remover o aviso.
<hr>

```python
def get_local_ip():
    """Descobre o IP local da máquina na rede (sem precisar de internet)."""
    s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    try:
        # Não envia pacotes de verdade, só descobre a interface usada
        s.connect(('8.8.8.8', 80))
        ip = s.getsockname()[0]
    except Exception:
        ip = '127.0.0.1'
    finally:
        s.close()
    return ip
```
A função get_local_ip() tenta descobrir o endereço IP local que a máquina usaria para se comunicar pela rede:

socket.socket(socket.AF_INET, socket.SOCK_DGRAM) cria um socket IPv4 para comunicação UDP.

s.connect(('8.8.8.8', 80)) informa ao sistema operacional um destino externo. Em UDP, isso normalmente não envia dados; serve para o sistema escolher a interface e o endereço local apropriados. Apesar do endereço de destino ser externo, a função não precisa receber uma resposta da internet para obter o IP.

s.getsockname()[0] retorna o endereço IP local associado a essa interface.

Se ocorrer uma exceção, a função usa 127.0.0.1, que é o endereço de loopback: acessível apenas pela própria máquina.

finally: s.close() fecha o socket mesmo que tenha ocorrido erro.

return ip devolve o endereço encontrado ou o endereço de loopback.

A função depende de socket estar importado no arquivo, normalmente com import socket. O resultado pode variar conforme as interfaces e rotas de rede configuradas.
<hr>

```python
if __name__ == '__main__':
    init_db()
    ip = get_local_ip()
    port = 5000
    print("\n" + "=" * 55)
    print("  🏢  Sistema de Gestão de Condomínio - ONLINE")
    print("=" * 55)
    print(f"  💻  Neste computador : http://127.0.0.1:{port}")
    print(f"  📱  Na rede local    : http://{ip}:{port}")
    print("=" * 55)
    print("  Pressione CTRL+C para encerrar.\n")

    app.run(host='0.0.0.0', port=port, debug=False, threaded=True)
```
Esse bloco inicia a aplicação quando `app.py` é executado diretamente, por exemplo com `python app.py`:

`if __name__ == '__main__':` verifica se o arquivo foi iniciado diretamente. Se `app.py` for importado por outro módulo, o bloco não roda.

`init_db()` cria as tabelas do banco, caso ainda não existam.

`ip = get_local_ip()` obtém o endereço IP local da máquina na rede.

`port = 5000` define a porta que o servidor Flask usará.

Os comandos `print()` exibem no terminal o estado do sistema e os endereços para acessá-lo:
`http://127.0.0.1:5000` funciona neste computador.

`http://<IP local>:5000` permite o acesso por outros dispositivos na mesma rede, se a rede e o firewall permitirem.

app.run(...) inicia o servidor:
`host='0.0.0.0'` faz com que ele aceite conexões por todas as interfaces de rede da máquina.

`port=port` usa a porta 5000.

`debug=False` desativa o modo de depuração.

threaded=True permite tratar requisições em threads, possibilitando atender várias ao mesmo tempo.

`CTRL+C` interrompe o servidor no terminal.

Atenção: 0.0.0.0 pode tornar o sistema acessível a outros dispositivos da rede. O servidor integrado do Flask é apropriado para desenvolvimento, não para hospedar uma aplicação em produção. O bloco está em app.py.
<hr>
