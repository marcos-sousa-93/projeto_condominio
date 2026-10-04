# Diretótrio D:\gestao-condominio\templates\
## manutencao.html
```html
<form method="post" action="{{ url_for('add_manutencao') }}" class="form">
  <input name="titulo" placeholder="Título do chamado" required>
  <input name="descricao" placeholder="Descrição">
  <select name="unidade_id">
    <option value="">Área comum</option>
    {% for u in unidades %}
      <option value="{{ u.id }}">Bloco {{ u.bloco }} - Ap {{ u.numero }}</option>
    {% endfor %}
  </select>
  <select name="prioridade">
    <option>Baixa</option><option selected>Normal</option><option>Alta</option><option>Urgente</option>
  </select>
  <button>Abrir chamado</button>
</form>
```
Adicionei o formulário em manutencao.html. Ele envia os dados por POST para a rota add_manutencao:

Título é obrigatório; descrição é opcional.
O seletor de unidade permite associar o chamado a uma unidade ou deixá-lo como “Área comum”.
A prioridade pode ser Baixa, Normal, Alta ou Urgente; “Normal” é a opção selecionada inicialmente.
Ao clicar em Abrir chamado, os dados são enviados para cadastro.
<hr>

```html
<table>
  <tr><th>Chamado</th><th>Local</th><th>Prioridade</th><th>Status</th><th>Ações</th></tr>
  {% for m in manutencoes %}
  <tr>
    <td><strong>{{ m.titulo }}</strong><br><small>{{ m.descricao or '' }}</small></td>
    <td>{% if m.bloco %}Bloco {{ m.bloco }} - Ap {{ m.numero }}{% else %}Área comum{% endif %}</td>
    <td><span class="tag {{ m.prioridade|lower }}">{{ m.prioridade }}</span></td>
    <td><span class="tag status {{ m.status|lower|replace('í','i') }}">{{ m.status }}</span></td>
    <td>
      <a href="{{ url_for('update_status', mid=m.id, status='Em andamento') }}">▶</a>
      <a href="{{ url_for('update_status', mid=m.id, status='Concluído') }}">✔</a>
      <a class="del" href="{{ url_for('del_manutencao', mid=m.id) }}">✖</a>
    </td>
  </tr>
  {% else %}<tr><td colspan="5">Sem chamados.</td></tr>{% endfor %}
</table>
```
Adicionei a tabela em manutencao.html. Ela mostra cada chamado com descrição, local, prioridade e status. Se não houver unidade associada, o local aparece como “Área comum”; se não houver chamados, a tabela exibe “Sem chamados.”

As ações usam os links para atualizar o status para “Em andamento” ou “Concluído”, ou excluir o chamado. Atenção: como essas rotas alteram dados via GET, recomenda-se migrá-las para POST, evitando alterações por simples acesso a um link.
<hr>
