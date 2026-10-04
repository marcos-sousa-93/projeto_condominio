# Diretório D:\gestao-condominio\templates\
## moradores.html
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
