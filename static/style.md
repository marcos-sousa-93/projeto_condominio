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

