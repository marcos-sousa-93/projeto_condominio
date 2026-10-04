# Diretório D:\gestao-condominio\templates\
## unidades.html
```html
<form method="post" action="{{ url_for('add_unidade') }}" class="form">
  <input name="bloco" placeholder="Bloco" required>
  <input name="numero" placeholder="Número / Ap" required>
  <input name="proprietario" placeholder="Proprietário">
  <input name="telefone" placeholder="Telefone">
  <input name="email" placeholder="E-mail" type="email">
  <button>Cadastrar</button>
</form>
```
Adicionei o formulário em unidades.html. Ele envia os dados por POST para a rota add_unidade:

Bloco e Número / Ap são obrigatórios (required).
Proprietário, Telefone e E-mail são opcionais.
type="email" pede ao navegador uma validação básica do formato do e-mail.
A classe form permite estilizar o formulário no CSS.
Ao clicar em Cadastrar, os dados são enviados à rota para serem gravados no banco.
<hr>

```html
<table>
  <tr><th>Bloco</th><th>Nº</th><th>Proprietário</th><th>Contato</th><th></th></tr>
  {% for u in unidades %}
  <tr>
    <td>{{ u.bloco }}</td><td>{{ u.numero }}</td><td>{{ u.proprietario or '-' }}</td>
    <td>{{ u.telefone or '' }} {{ u.email or '' }}</td>
    <td><a class="del" href="{{ url_for('del_unidade', uid=u.id) }}">Excluir</a></td>
  </tr>
  {% else %}<tr><td colspan="5">Nenhuma unidade cadastrada.</td></tr>{% endfor %}
</table>
```
Adicionei a tabela em unidades.html. Ela percorre a lista unidades enviada pela rota e mostra bloco, número, proprietário e contatos. Se não houver proprietário, exibe -; se não houver unidades cadastradas, mostra uma mensagem na tabela.

O link Excluir chama a rota del_unidade com o ID da unidade. Como essa rota atualmente exclui por GET, abrir esse link apaga o registro; idealmente, essa ação deve ser alterada para usar POST.
<hr>

```html
<form method="post" action="{{ url_for('add_morador') }}" class="form">
  <input name="nome" placeholder="Nome" required>
  <select name="unidade_id">
    <option value="">— Sem unidade —</option>
    {% for u in unidades %}
      <option value="{{ u.id }}">Bloco {{ u.bloco }} - Ap {{ u.numero }}</option>
    {% endfor %}
  </select>
  <select name="tipo">
    <option>Morador</option><option>Proprietário</option>
    <option>Inquilino</option><option>Funcionário</option>
  </select>
  <input name="telefone" placeholder="Telefone">
  <input name="email" placeholder="E-mail" type="email">
  <button>Cadastrar</button>
</form>
```
Adicionei o formulário em moradores.html. Ele envia os dados por POST para a rota add_morador:

Nome é obrigatório.
O seletor Unidade é preenchido com as unidades cadastradas; também é possível escolher “Sem unidade”.
O seletor Tipo oferece Morador, Proprietário, Inquilino e Funcionário.
Telefone e E-mail são opcionais; o navegador faz uma validação básica do formato do e-mail.
Ao clicar em Cadastrar, os campos são enviados para gravação no banco.
<hr>

```html
<table>
  <tr><th>Nome</th><th>Unidade</th><th>Tipo</th><th>Contato</th><th></th></tr>
  {% for m in moradores %}
  <tr>
    <td>{{ m.nome }}</td>
    <td>{% if m.bloco %}Bloco {{ m.bloco }} - Ap {{ m.numero }}{% else %}-{% endif %}</td>
    <td>{{ m.tipo }}</td>
    <td>{{ m.telefone or '' }} {{ m.email or '' }}</td>
    <td><a class="del" href="{{ url_for('del_morador', mid=m.id) }}">Excluir</a></td>
  </tr>
  {% else %}<tr><td colspan="5">Sem moradores.</td></tr>{% endfor %}
</table>
```
Adicionei a tabela em moradores.html. Ela lista cada morador com nome, unidade (bloco e apartamento), tipo e contatos. Caso não haja unidade associada, mostra -; se a lista estiver vazia, exibe “Sem moradores.”

O link Excluir chama a rota del_morador usando o ID do morador. Como essa rota atualmente exclui por GET, recomendo alterá-la para POST para que a exclusão não aconteça apenas por abrir um link.
<hr>

