# Diretório D:\gestao-condominio\templates\
## despesas.html
```html
<div class="cards">
  <div class="card"><span>💵</span><h3>R$ {{ '%.2f'|format(total) }}</h3><p>Total acumulado</p></div>
  <div class="card destaque"><span>🧮</span><h3>R$ {{ '%.2f'|format(cota) }}</h3><p>Cota por unidade</p></div>
</div>
```
Adicionei os cartões em despesas.html:

Total acumulado exibe a soma das despesas, recebida como total.
Cota por unidade exibe o valor calculado por unidade, recebido como cota. A classe destaque permite dar a esse cartão um estilo especial.
Ambos formatam os valores como reais com duas casas decimais.
<hr>

```html
<form method="post" action="{{ url_for('add_despesa') }}" class="form">
  <input name="descricao" placeholder="Descrição" required>
  <input name="valor" type="number" step="0.01" placeholder="Valor (R$)" required>
  <input name="mes" placeholder="Mês (ex: 2025-01)" required>
  <select name="categoria">
    <option>Água</option><option>Luz</option><option>Limpeza</option>
    <option>Portaria</option><option>Manutenção</option><option>Outros</option>
  </select>
  <button>Registrar</button>
</form>
```
Adicionei o formulário em despesas.html. Ele envia os dados via POST para a rota add_despesa:

Descrição, Valor e Mês são obrigatórios.
O campo de valor aceita números com até duas casas decimais (step="0.01").
A categoria pode ser escolhida na lista.
Ao clicar em Registrar, os dados são enviados para serem salvos como despesa.
<hr>

```html
<table>
  <tr><th>Data</th><th>Descrição</th><th>Categoria</th><th>Mês</th><th>Valor</th><th></th></tr>
  {% for d in despesas %}
  <tr>
    <td>{{ (d.data or d.mes)[:10] }}</td><td>{{ d.descricao }}</td><td>{{ d.categoria }}</td>
    <td>{{ d.mes }}</td><td>R$ {{ '%.2f'|format(d.valor) }}</td>
    <td><a class="del" href="{{ url_for('del_despesa', did=d.id) }}">Excluir</a></td>
  </tr>
  {% else %}<tr><td colspan="6">Nenhuma despesa.</td></tr>{% endfor %}
</table>
```
Adicionei a tabela de despesas em despesas.html. Ela lista data, descrição, categoria, mês, valor e um link para excluir cada despesa. Se não houver registros, exibe “Nenhuma despesa.”

O trecho usa d.data, mas a tabela despesas ainda não tinha essa coluna. Para que a página funcione, também adicionei data ao esquema e uma migração em database.py para bancos existentes. A rota de cadastro em app.py agora grava a data atual. Para despesas antigas sem data, a tabela mostra o mês como alternativa.
<hr>
