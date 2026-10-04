# Diretório D:\gestao-condominio\static\
## style.css
```css
* {
box-sizing: border-box;
margin: 0;
padding: 0;
}
```
Adicionei as regras em `style.css`.

O seletor `*` aplica `box-sizing: border-box` e remove margens e espaçamentos padrão de todos os elementos.
<hr>

```css
body {
  font-family: 'Segoe UI', system-ui, sans-serif;
  background: #f3f5fa;
  color: #2c3e50;
  display: flex;
  min-height: 100vh;
}
```
Adicionei as regras em `style.css`.

`body` define a fonte, o fundo azul-claro e a cor do texto.

`display: flex` habilita o layout flexível da página.

`min-height: 100vh` garante que o corpo ocupe pelo menos toda a altura da janela do navegador.
<hr>

```css
.sidebar {
  width: 230px;
  background: #1e293b;
  color: #fff;
  padding: 24px 16px;
  position: sticky;
  top: 0;
  height: 100vh;
}
```
Adicionei as regras `.sidebar` em style.css. 

Elas dão à barra lateral largura de 230 px, fundo azul-escuro, texto branco e espaçamento interno. 

`position: sticky` com `top: 0` mantém a barra visível no topo ao rolar a página, e `height: 100vh` faz com que ela ocupe a altura da janela.
<hr>

```css
.sidebar h1 { 
    font-size: 20px; 
    margin-bottom: 28px; 
    letter-spacing: .5px; 
}
```
Ela estiliza os títulos `<h1>` dentro da barra lateral: define o tamanho do texto em 20 px, adiciona 28 px de espaço abaixo do título e aumenta levemente o espaçamento entre as letras.
<hr>

```css
.sidebar h1 span { 
    color: #38bdf8; 
}
```
Ela aplica um tom azul-claro ao `<span>` que estiver dentro do título `<h1>` da barra lateral. 

No título “CondoGest”, por exemplo, destaca “Gest” em azul.
<hr>

```css
.sidebar nav a {
  display: block;
  color: #cbd5e1;
  text-decoration: none;
  padding: 10px 12px;
  border-radius: 8px;
  margin-bottom: 6px;
  transition: .2s;
}
```
Adicionei a regra `.sidebar` nav a em `style.css`. 

Ela estiliza os links de navegação na barra lateral: define a cor do texto, remove o sublinhado padrão, adiciona espaçamento interno e cantos arredondados, separa os links verticalmente e configura uma transição suave de 0,2 segundos para mudanças visuais.
<hr>

```css
.sidebar nav a:hover { 
    background: #334155; 
    color: #fff; 
}
```
O seletor `:hover` aplica esses estilos enquanto o ponteiro está sobre um link da barra lateral: o fundo fica azul-acinzentado e o texto branco. 

A transição já definida nos links suaviza essa mudança.
<hr>

```css
main { 
    flex: 1; 
    padding: 30px 40px; 
}
```
Como o `body` usa `display: flex`, `flex: 1` faz o conteúdo principal ocupar o espaço disponível ao lado da barra lateral. padding acrescenta 30 px de espaço vertical e 40 px horizontal ao redor do conteúdo.
<hr>

```css
h2 { 
    margin-bottom: 20px; 
    color: #1e293b; 
}
```
Ela estiliza todos os títulos `<h2>`: define 20 px de espaço abaixo e aplica a cor azul-escura #1e293b.
<hr>

```css
h3 { 
    margin-bottom: 12px; 
}
```
Ela acrescenta 12 px de espaço abaixo de todos os títulos `<h3>`, separando-os do conteúdo que vem em seguida.
<hr>

```css
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 16px;
  margin-bottom: 24px;
}
```
Adicionei a regra `.cards` em `style.css`. Ela organiza os cartões em uma grade responsiva:

`display: grid` ativa o layout em grade.

`repeat(auto-fit, minmax(180px, 1fr))` ajusta automaticamente o número de colunas, mantendo cada cartão com pelo menos 180 px e distribuindo o espaço disponível.

`gap: 16px` define o espaçamento entre cartões.

`margin-bottom: 24px` separa a grade do conteúdo abaixo.
<hr>

```css
.card {
  background: #fff;
  padding: 20px;
  border-radius: 14px;
  box-shadow: 0 2px 10px rgba(0,0,0,.05);
  border-left: 5px solid #38bdf8;
}
```
Adicionei a regra `.card` em `style.css`. Ela dá aos cartões fundo branco e espaçamento interno, arredonda os cantos, aplica uma sombra suave e acrescenta uma borda azul-clara à esquerda para destacá-los.
<hr>

```css
.card span { 
    font-size: 22px; 
}
```
Ela define o tamanho da fonte de qualquer <span> dentro de um cartão como 22 px. 

No painel, isso aumenta o tamanho dos ícones exibidos nesses elementos.
<hr>

```css
.card h3 { 
    font-size: 22px; 
    margin: 6px 0 4px; 
    color: #0f172a; 
}
```
Ela estiliza os títulos `<h3>` dentro dos cartões: define o tamanho do texto em 22 px, ajusta o espaçamento acima e abaixo e aplica uma cor azul-escura.
<hr>

```css
.card p { 
    color: #64748b; 
    font-size: 13px; 
}
```
Ela define os parágrafos dentro dos cartões com texto cinza-azulado e tamanho de 13 px, diferenciando as descrições dos valores e ícones.
<hr>

```css
.card.destaque { 
    border-left-color: #10b981; 
    background: linear-gradient(135deg,#ecfdf5,#fff); 
}
```
Adicionei a regra `.card.destaque` em `style.css`. 

Ela diferencia cartões que têm as classes card e destaque: muda a borda esquerda para verde e aplica um degradê de verde-claro para branco.
<hr>

```css
.grid2 { 
    display: grid; 
    grid-template-columns: 1fr 1fr; 
    gap: 20px; 
    margin-bottom: 24px; 
}
```
Adicionei a regra `.grid2` em `style.css`. 

Ela organiza os elementos dentro dessa classe em duas colunas de larguras iguais (1fr 1fr), com 20 px de espaço entre elas e 24 px de margem abaixo da grade.
<hr>

```css
@media (max-width: 900px) { 
    .grid2 { 
        grid-template-columns: 1fr; 
    } 
}
```
Quando a tela tiver até 900 px de largura, os elementos de `.grid2` passam de duas colunas para uma, facilitando a visualização em telas menores. 

Em telas mais largas, continua valendo o layout de duas colunas.
<hr>

```css
.box {
  background: #fff;
  border-radius: 14px;
  padding: 20px;
  box-shadow: 0 2px 10px rgba(0,0,0,.05);
  margin-bottom: 20px;
}
```
Adicionei a regra `.box` em `style.css`. Ela dá às seções com essa classe um fundo branco, cantos arredondados, espaçamento interno, uma sombra discreta e 20 px de espaço abaixo, criando um visual de painel.
<hr>

```css
.item {
  padding: 12px 0;
  border-bottom: 1px solid #eef1f6;
}
```
Adicionei a regra `.item` em `style.css`. 

Ela aplica 12 px de espaçamento vertical aos itens e adiciona uma linha divisória fina e clara na parte inferior, separando visualmente avisos e chamados.
<hr>

```css
.item:last-child { 
    border-bottom: none; 
}
```
Ela remove a borda inferior do último elemento com a classe item, evitando uma linha divisória depois do último item da lista.
<hr>

```css
.item small { 
    color: #94a3b8; 
    margin-left: 8px; 
    font-size: 12px; 
}
```
Ela estiliza os elementos `<small>` dentro de itens: usa texto cinza-claro, tamanho de 12 px e adiciona 8 px de espaço à esquerda, separando informações secundárias do restante do conteúdo.
<hr>

```css
.item p { 
    margin-top: 4px; 
    color: #475569; 
    font-size: 14px; 
}
```
Ela estiliza os parágrafos dentro dos itens: cria 4 px de espaço acima, define uma cor cinza-azulada para o texto e usa tamanho de 14 px.
<hr>

```css
.vazio { 
    color: #94a3b8; 
    font-style: italic; 
}
```
Ela estiliza elementos com a classe vazio — como mensagens exibidas quando não há avisos ou chamados — com texto cinza-claro e em itálico.
<hr>

```css
.form {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  background: #fff;
  padding: 16px;
  border-radius: 12px;
  margin-bottom: 20px;
  box-shadow: 0 2px 8px rgba(0,0,0,.04);
}
```
Adicionei a regra `.form` em `style.css`. 

Ela organiza os elementos do formulário em uma linha flexível, permite que passem para a linha seguinte quando faltar espaço e mantém 10 px entre eles. 

Também aplica fundo branco, espaçamento interno, cantos arredondados, margem inferior e uma sombra leve.
<hr>

```css
.form input, .form select {
  padding: 10px 12px;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  font-size: 14px;
  flex: 1;
  min-width: 150px;
}
```
Adicionei a regra em `style.css`. 

Ela estiliza os campos `<input>` e `<select>` dentro dos formulários: define o espaçamento interno, uma borda cinza-clara, cantos arredondados e texto de 14 px. `flex: 1` permite que os campos ocupem o espaço disponível, enquanto `min-width: 150px` evita que fiquem estreitos demais.
<hr>

```css
.form button {
  padding: 10px 20px;
  background: #0ea5e9;
  color: #fff;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  transition: .2s;
}
```
Adicionei a regra `.form button` em `style.css`. 

Ela estiliza os botões dos formulários com espaçamento interno, fundo azul e texto branco, cantos arredondados e texto em negrito. 

cursor: pointer indica que o botão é clicável, e transition: .2s suaviza mudanças visuais.
<hr>

```css
.form button:hover { 
    background: #0284c7; 
}
```
Quando o ponteiro passa sobre um botão de formulário, o fundo muda para um azul um pouco mais escuro. A transição de 0,2 segundos definida no botão suaviza essa mudança.
<hr>

```css
table {
  width: 100%;
  border-collapse: collapse;
  background: #fff;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0,0,0,.05);
}
```
Adicionei a regra table em `style.css`. 

Ela faz as tabelas ocuparem toda a largura disponível, une as bordas das células e aplica fundo branco, cantos arredondados e uma sombra leve. 

`overflow: hidden` mantém o conteúdo dentro dos cantos arredondados.
<hr>

