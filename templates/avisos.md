# Diretório D:\gestao-condominio\templates\
## avisos.html
```html
<form method="post" action="{{ url_for('add_aviso') }}" class="form">
  <input name="titulo" placeholder="Título" required>
  <input name="mensagem" placeholder="Mensagem">
  <button>Publicar</button>
</form>
```
Adicionei o formulário em avisos.html. Ele envia os dados por POST para a rota add_aviso.

O título é obrigatório, a mensagem é opcional e o botão Publicar envia o aviso para ser salvo e publicado no mural.
<hr>

```html
{% for a in avisos %}
  <div class="box aviso">
    <strong>{{ a.titulo }}</strong>
    <small>{{ a.data[:16] }}</small>
    <p>{{ a.mensagem }}</p>
    <a class="del" href="{{ url_for('del_aviso', aid=a.id) }}">Excluir</a>
  </div>
{% else %}<p class="vazio">Sem avisos.</p>{% endfor %}
```
Adicionei a listagem em avisos.html. O template percorre a lista avisos e, para cada aviso, exibe título, data (até 16 caracteres), mensagem e um link Excluir. Se não houver avisos, mostra “Sem avisos.”

Observação: o link de exclusão chama uma rota que altera dados via GET. É mais seguro usar um formulário POST para essa ação, evitando que o aviso seja excluído por um simples acesso ao link.
<hr>
