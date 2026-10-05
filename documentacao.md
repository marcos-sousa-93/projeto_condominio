# 📘 Documentação — Sistema de Gestão de Condomínio

> Última atualização: (data)

## 🏗️ Estrutura de arquivos

| Arquivo / Pasta           | Responsabilidade                                      |
|---------------------------|-------------------------------------------------------|
| `app.py`                  | Servidor Flask, rotas, lógica de negócio              |
| `database.py`             | Conexão SQLite, criação de tabelas                    |
| `servidor.py`             | Execução em produção (Waitress)                       |
| `requirements.txt`        | Dependências Python                                   |
| `condominio.db`           | Banco de dados SQLite (gerado automaticamente)        |
| `templates/base.html`     | Layout base (sidebar + flash messages)                |
| `templates/index.html`    | Dashboard com cards e estatísticas                    |
| `templates/unidades.html` | CRUD de unidades                                      |
| `templates/moradores.html`| CRUD de moradores                                     |
| `templates/despesas.html` | Cadastro e rateio de despesas                         |
| `templates/manutencao.html`| Chamados com prioridade/status                       |
| `templates/avisos.html`   | Mural de avisos                                       |
| `static/style.css`        | Estilos visuais                                       |

## 🗄️ Esquema do Banco

### Tabela `unidades`
| Campo         | Tipo    | Descrição                     |
|---------------|---------|-------------------------------|
| id            | INTEGER | PK autoincremento             |
| bloco         | TEXT    | Bloco/torre                   |
| numero        | TEXT    | Número do apartamento         |
| proprietario  | TEXT    | Nome do proprietário          |
| telefone      | TEXT    | Contato                       |
| email         | TEXT    | Contato                       |

*(repita o padrão para moradores, despesas, manutencao, avisos)*

## 🛣️ Rotas da API

| Método | Rota                       | Descrição                       |
|--------|----------------------------|---------------------------------|
| GET    | `/`                        | Dashboard                       |
| GET    | `/unidades`                | Lista unidades                  |
| POST   | `/unidades/add`            | Cadastra unidade                |
| GET    | `/unidades/del/<id>`       | Remove unidade                  |
| GET    | `/moradores`               | Lista moradores                 |
| POST   | `/moradores/add`           | Cadastra morador                |
| GET    | `/moradores/del/<id>`      | Remove morador                  |
| GET    | `/despesas`                | Lista despesas + cota           |
| POST   | `/despesas/add`            | Cadastra despesa                |
| GET    | `/despesas/del/<id>`       | Remove despesa                  |
| GET    | `/manutencao`              | Lista chamados                  |
| POST   | `/manutencao/add`          | Abre chamado                    |
| GET    | `/manutencao/status/<id>/<status>` | Atualiza status         |
| GET    | `/manutencao/del/<id>`     | Remove chamado                  |
| GET    | `/avisos`                  | Lista avisos                    |
| POST   | `/avisos/add`              | Publica aviso                   |
| GET    | `/avisos/del/<id>`         | Remove aviso                    |

## 📝 Histórico de mudanças

### v1.0 — Versão inicial
- [x] Dashboard com totais e cota sugerida
- [x] CRUD de unidades
- [x] CRUD de moradores
- [x] Cadastro de despesas com rateio
- [x] Chamados de manutenção com prioridade/status
- [x] Mural de avisos
- [x] Servidor acessível na rede local (0.0.0.0)

### v1.1 — (próxima)
- [ ] Login de síndico/morador
- [ ] Geração de boleto PDF
- [ ] Envio de e-mail
- [ ] Backup automático do banco
