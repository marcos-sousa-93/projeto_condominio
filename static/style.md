# Diretório D:\gestao-condominio\static\
## style.css
```css
* {
box-sizing: border-box;
margin: 0;
padding: 0;
}
```
Adicionei as regras em style.css.

O seletor * aplica box-sizing: border-box e remove margens e espaçamentos padrão de todos os elementos.
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
Adicionei as regras em style.css.

body define a fonte, o fundo azul-claro e a cor do texto.

display: flex habilita o layout flexível da página.

min-height: 100vh garante que o corpo ocupe pelo menos toda a altura da janela do navegador.
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
Adicionei as regras .sidebar em style.css. 

Elas dão à barra lateral largura de 230 px, fundo azul-escuro, texto branco e espaçamento interno. 

position: sticky com top: 0 mantém a barra visível no topo ao rolar a página, e height: 100vh faz com que ela ocupe a altura da janela.
<hr>

```css
.sidebar h1 { 
    font-size: 20px; 
    margin-bottom: 28px; 
    letter-spacing: .5px; 
}
```
Ela estiliza os títulos <h1> dentro da barra lateral: define o tamanho do texto em 20 px, adiciona 28 px de espaço abaixo do título e aumenta levemente o espaçamento entre as letras.
<hr>

```css
.sidebar h1 span { 
    color: #38bdf8; 
}
```
Ela aplica um tom azul-claro ao <span> que estiver dentro do título <h1> da barra lateral. 

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
Adicionei a regra .sidebar nav a em style.css. 

Ela estiliza os links de navegação na barra lateral: define a cor do texto, remove o sublinhado padrão, adiciona espaçamento interno e cantos arredondados, separa os links verticalmente e configura uma transição suave de 0,2 segundos para mudanças visuais.
<hr>

```css
.sidebar nav a:hover { 
    background: #334155; 
    color: #fff; 
}
```
O seletor :hover aplica esses estilos enquanto o ponteiro está sobre um link da barra lateral: o fundo fica azul-acinzentado e o texto branco. 

A transição já definida nos links suaviza essa mudança.
<hr>

```css
main { 
    flex: 1; 
    padding: 30px 40px; 
}
```
Como o body usa display: flex, flex: 1 faz o conteúdo principal ocupar o espaço disponível ao lado da barra lateral. padding acrescenta 30 px de espaço vertical e 40 px horizontal ao redor do conteúdo.
<hr>

```css
h2 { 
    margin-bottom: 20px; 
    color: #1e293b; 
}
```
Ela estiliza todos os títulos <h2>: define 20 px de espaço abaixo e aplica a cor azul-escura #1e293b.
<hr>

```css
h3 { 
    margin-bottom: 12px; 
}
```
Ela acrescenta 12 px de espaço abaixo de todos os títulos <h3>, separando-os do conteúdo que vem em seguida.
<hr>

```css
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  gap: 16px;
  margin-bottom: 24px;
}
```
Adicionei a regra .cards em style.css. Ela organiza os cartões em uma grade responsiva:

display: grid ativa o layout em grade.

repeat(auto-fit, minmax(180px, 1fr)) ajusta automaticamente o número de colunas, mantendo cada cartão com pelo menos 180 px e distribuindo o espaço disponível.

gap: 16px define o espaçamento entre cartões.

margin-bottom: 24px separa a grade do conteúdo abaixo.
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
Adicionei a regra .card em style.css. Ela dá aos cartões fundo branco e espaçamento interno, arredonda os cantos, aplica uma sombra suave e acrescenta uma borda azul-clara à esquerda para destacá-los.
<hr>

