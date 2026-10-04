# Diretório D:\gestao-condominio\templates\
## index.html
```html
{% extends "base.html" %}
```

Adicionei {% extends "base.html" %} em index.html.

Essa instrução do Jinja faz com que a página inicial herde a estrutura comum definida em base.html, incluindo o menu lateral, os avisos e o layout principal. O conteúdo próprio de index.html deve ser escrito em blocos, como {% block content %}...{% endblock %}.
<hr>

```html
{% block content %}
{% endblock %}
```

Como essa página herda de base.html, o conteúdo que você colocar entre essas tags será exibido no espaço {% block content %} do template base. Por enquanto, o bloco está vazio.
<hr>

```html
<h2>Dashboard</h2>
```

Adicionei <h2>Dashboard</h2> dentro do bloco content em index.html.

A tag <h2> cria um título de segundo nível. Ele aparecerá como título principal do conteúdo da página inicial, abaixo da estrutura compartilhada de base.html.
<hr>

```html
<div class="cards"></div>
```

Adicionei `<div class="cards"></div>` em index.html, logo após o título do Dashboard.

A <div> cria um contêiner para agrupar conteúdo. A classe cards permite estilizar esse espaço no CSS; por enquanto, ele está vazio e pode receber os cartões com os dados do painel.
<hr>

```html
<div class="card"><span>🏠</span><h3>{{ total_unidades }}</h3><p>Unidades</p></div>
<div class="card"><span>👥</span><h3>{{ total_moradores }}</h3><p>Moradores</p></div>
<div class="card"><span>💰</span><h3>R$ {{ '%.2f'|format(total_despesas) }}</h3><p>Despesas totais</p></div>
<div class="card destaque"><span>🧮</span><h3>R$ {{ '%.2f'|format(cota) }}</h3><p>Cota sugerida / unidade</p></div>
<div class="card"><span>🔧</span><h3>{{ pendentes }}</h3><p>Chamados abertos</p></div>
```
Adicionei os três cartões dentro de <div class="cards"> em index.html:

Unidades mostra total_unidades.
Moradores mostra total_moradores.
Despesas totais mostra total_despesas formatado com duas casas decimais e o prefixo R$.
Cota sugerida / unidade exibe cota como valor em reais, com duas casas decimais. A classe extra destaque permite dar a esse cartão um estilo visual diferenciado no CSS.
Chamados abertos exibe o valor de pendentes, que a rota envia ao template.
As expressões {{ ... }} são do Jinja e inserem no HTML os valores enviados pela rota Flask.
<hr>

```html
<div class="grid2"></div>
```
Adicionei <div class="grid2"></div> abaixo dos cartões em index.html.

Essa <div> cria um contêiner vazio para agrupar conteúdo em uma grade de duas colunas. A classe grid2 permite definir esse layout no CSS.
<hr>

```html
<section class="box"></section>
```
Adicionei <section class="box"></section> dentro de grid2 em index.html.

<section> representa uma seção temática da página. A classe box permite aplicar estilos visuais a esse bloco; ele está vazio por enquanto e pode receber conteúdo do painel.
<hr>

```html
<h3>📢 Últimos Avisos</h3>
{% for a in avisos %}
  <div class="item"><strong>{{ a.titulo }}</strong><small>{{ a.data[:16] }}</small>
  <p>{{ a.mensagem }}</p></div>
{% else %}<p class="vazio">Nenhum aviso.</p>{% endfor %}
```
Adicionei o conteúdo à seção box em index.html. Ela agora exibe o título “Últimos Avisos” e percorre a lista avisos recebida da rota:

Para cada aviso, mostra o título, a data (limitada aos primeiros 16 caracteres) e a mensagem.
Se não houver avisos, mostra “Nenhum aviso.”.
<hr>

```html
<section class="box">
    <h3>🔧 Chamados em andamento</h3>
    {% for m in manutencoes %}
      <div class="item">
        <strong>{{ m.titulo }}</strong> <span class="tag {{ m.prioridade|lower }}">{{ m.prioridade }}</span>
        <small>{% if m.bloco %}Bloco {{ m.bloco }} - Ap {{ m.numero }}{% endif %} · {{ m.status }}</small>
      </div>
    {% else %}<p class="vazio">Nada por aqui 🎉</p>{% endfor %}
  </section>
```
Adicionei a seção “Chamados em andamento” dentro de grid2 em index.html. Ela percorre a lista manutencoes enviada pela rota e mostra o título, a prioridade e o status de cada chamado. Quando há bloco associado, também exibe o bloco e o número do apartamento. Se a lista estiver vazia, aparece “Nada por aqui 🎉”.

A classe da etiqueta de prioridade usa o valor em minúsculas (m.prioridade|lower), permitindo estilos CSS específicos por prioridade.
<hr>

```html
<section class="box">
  <h3>📈 Despesas por categoria</h3>
  <table>
    <tr><th>Categoria</th><th>Total</th></tr>
    {% for c in por_categoria %}
      <tr><td>{{ c.categoria or 'Sem categoria' }}</td><td>R$ {{ '%.2f'|format(c.v) }}</td></tr>
    {% else %}<tr><td colspan="2">Nenhuma despesa cadastrada.</td></tr>{% endfor %}
  </table>
</section>
```
Adicionei a seção “Despesas por categoria” logo após grid2 em index.html.

Ela apresenta uma tabela com cada categoria e o total correspondente, formatado em reais com duas casas decimais. Despesas sem categoria aparecem como “Sem categoria”. Se não houver registros, a tabela exibe “Nenhuma despesa cadastrada.”
<hr>

