# Diretório D:\gestao-condominio\templantes\
## base.html
```html
<!DOCTYPE html>
```
Adicionei <!DOCTYPE html> ao arquivo base.html.

Essa declaração informa ao navegador que o documento usa HTML5. Ela deve ficar no início do arquivo, antes do restante do conteúdo HTML, para que o navegador interprete a página no modo padrão.
<hr>

```html
<html lang="pt-BR">
```
Adicionei <html lang="pt-BR"> em base.html, logo após <!DOCTYPE html>.

Essa tag inicia o documento HTML. O atributo lang="pt-BR" informa que o idioma principal da página é português do Brasil, o que ajuda leitores de tela, navegadores e mecanismos de busca. É necessário fechá-la mais adiante com </html>.
<hr>

```html
<head>
    <meta charset="UTF-8">
</head>
```
Adicionei <meta charset="UTF-8"> dentro de <head> em base.html.

Essa declaração define a codificação de caracteres da página como UTF-8, permitindo que letras acentuadas e outros caracteres sejam exibidos corretamente.
<hr>

```html
<title>{% block title %}Gestão de Condomínio{% endblock %}</title>
```
<title> define o texto exibido na aba do navegador. As tags {% block title %} e {% endblock %} são do Jinja, o mecanismo de templates do Flask: páginas que herdam de base.html podem substituir esse título. Se não substituírem, o padrão será “Gestão de Condomínio”.
<hr>

```html
<link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
```
Ela carrega o arquivo de estilos style.css da pasta static. url_for('static', filename='style.css') gera o endereço correto do arquivo usando o Flask, e o navegador aplica essas regras visuais à página.
<hr>

```html
<aside class="sidebar">
```
Adicionei <aside class="sidebar"> dentro do <body> em base.html.

<aside> cria uma área de conteúdo complementar — aqui, provavelmente uma barra lateral. A classe sidebar permite aplicar estilos específicos a essa área no CSS. Como esta linha apenas abre o elemento, será preciso adicionar </aside> depois do conteúdo da barra lateral.
<hr>

```html
<h1>🏢 Condo<span>Gest</span></h1>
  <nav>
      <a href="{{ url_for('index') }}">📊 Dashboard</a>
      <a href="{{ url_for('unidades') }}">🏠 Unidades</a>
      <a href="{{ url_for('moradores') }}">👥 Moradores</a>
      <a href="{{ url_for('despesas') }}">💰 Despesas</a>
      <a href="{{ url_for('manutencao') }}">🔧 Manutenção</a>
      <a href="{{ url_for('avisos') }}">📢 Avisos</a>
  </nav>
```
Adicionei o título e o menu de navegação dentro de <aside class="sidebar"> em base.html.

<h1> exibe a marca “CondoGest”; o texto “Gest” está dentro de <span> para poder receber um estilo diferente no CSS.
<nav> agrupa os links de navegação.
Cada url_for(...) gera o endereço da rota correspondente no Flask: dashboard, unidades, moradores, despesas, manutenção e avisos.
Assim, a barra lateral fornece acesso às principais páginas do sistema.
<hr>

```html
<main>
    {% with msgs = get_flashed_messages(with_categories=true) %}
        {% for cat, m in msgs %}
            <div class="flash {{ cat }}">{{ m }}</div>
        {% endfor %}
    {% endwith %}
    {% block content %}{% endblock %}
</main>
```
Adicionei o bloco <main> em base.html.

Ele define a área principal das páginas e:

Recupera as mensagens temporárias do Flask criadas com flash(), incluindo suas categorias.
Percorre as mensagens e exibe cada uma em uma <div>. A categoria (ok, erro etc.) é incluída como classe CSS para permitir estilos diferentes.
Define {% block content %} como espaço reservado para o conteúdo específico de cada página que herda esse template.
<hr>

