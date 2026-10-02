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
