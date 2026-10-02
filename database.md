# Diretório: D:\gestao-condominio\
## 1 - database.py

```python
import sqlite3 # Biblioteca para trabalhar com bancos de dados SQLite
```
import sqlite3

O que isso significa:

sqlite3 é a biblioteca nativa do Python para trabalhar com bancos de dados SQLite.
SQLite é um banco leve, que geralmente fica em um arquivo local, sem precisar de um servidor separado.
Com essa importação, o código futuro pode criar conexões, executar comandos SQL, criar tabelas, inserir dados e consultar registros.
Exemplo de uso futuro:

conectar ao banco: sqlite3.connect("condominio.db")
criar uma tabela: CREATE TABLE moradores (...)
inserir registros: INSERT INTO moradores ...
consultar dados: SELECT * FROM moradores.
<hr>

```python
import os
```
import os

Importa o módulo os, que fornece funções para interagir com o sistema operacional.
Ele ajuda a trabalhar com caminhos de arquivos, pastas, criação e leitura de diretórios, checagem de existência, entre outras coisas.
Exemplo:
os.getcwd() → mostra o diretório atual
os.path.join("pastas", "arquivo.db") → monta um caminho de arquivo
os.path.exists("arquivo.db") → verifica se o arquivo existe.
<hr>

```python
DB_PATH = os.path.join(os.path.dirname(__file__), 'condominio.db') # D:\gestao-condominio\condominio.db
```
os.path.dirname(file)

pega o diretório da própria pasta do arquivo database.py
por exemplo: se o arquivo estiver em C:\gestao-condominio\database.py, então isso devolve C:\gestao-condominio
os.path.join(...)

junta esse diretório com o nome do arquivo do banco:
C:\gestao-condominio\condominio.db
Ou seja, DB_PATH aponta para o banco dentro da mesma pasta do projeto, e não para uma pasta dados que pode não existir.
<hr>

```python
def get_db(): # Função para obter a conexão com o banco de dados
    conn = sqlite3.connect(DB_PATH) # Conecta ao banco de dados
    conn.row_factory = sqlite3.Row # Permite acessar os resultados das consultas como dicionários
    return conn # Retorna a conexão com o banco de dados
```
sqlite3.connect(DB_PATH)

abre ou cria o arquivo do banco SQLite em DB_PATH
se o arquivo ainda não existe, o SQLite cria automaticamente
conn.row_factory = sqlite3.Row

faz com que os resultados das consultas venham como linhas com acesso por nome
<hr>

```python
def init_db(): # Função para inicializar o banco de dados
    conn = get_db() # Conecta ao banco de dados
    cur = conn.cursor() # Cria um cursor para executar comandos SQL
```
A função abre uma conexão com o banco usando get_db() e cria um cursor, que poderá executar comandos SQL. Por enquanto, ela não executa comandos nem fecha a conexão; isso pode ser acrescentado quando você definir o que a inicialização do banco deve fazer.
<hr>

```python
cur.executescript('''
    CREATE TABLE IF NOT EXISTS unidades (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        bloco TEXT NOT NULL,
        numero TEXT NOT NULL,
        proprietario TEXT,
        telefone TEXT,
        email TEXT,
        UNIQUE(bloco, numero)
    );
    
    CREATE TABLE IF NOT EXISTS moradores (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        nome TEXT NOT NULL,
        unidade_id INTEGER,
        tipo TEXT,
        telefone TEXT,
        email TEXT,
        FOREIGN KEY(unidade_id) REFERENCES unidades(id) ON DELETE SET NULL
    );
    
    CREATE TABLE IF NOT EXISTS despesas (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        descricao TEXT NOT NULL,
        valor REAL NOT NULL,
        mes TEXT NOT NULL,
        categoria TEXT
    );
    
    CREATE TABLE IF NOT EXISTS manutencao (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        titulo TEXT NOT NULL,
        descricao TEXT,
        unidade_id INTEGER,
        status TEXT DEFAULT 'Pendente',
        prioridade TEXT DEFAULT 'Normal',
        data_abertura TEXT DEFAULT CURRENT_TIMESTAMP,
        FOREIGN KEY(unidade_id) REFERENCES unidades(id) ON DELETE SET NULL
    );
    
    CREATE TABLE IF NOT EXISTS avisos (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        titulo TEXT NOT NULL,
        mensagem TEXT,
        data TEXT DEFAULT CURRENT_TIMESTAMP
    );
    ''')
```
executescript() para criar as tabelas, caso ainda não existam:
unidades: bloco, número e dados do proprietário. A combinação bloco/número não pode se repetir.
moradores: nome e contato dos moradores, com vínculo opcional a uma unidade.
despesas: descrição, valor, mês e categoria.
manutencao: solicitações de manutenção, incluindo status, prioridade, data de abertura e unidade opcional.
avisos: título, mensagem e data dos avisos.
IF NOT EXISTS impede que o script tente recriar uma tabela que já existe.
<hr>

```python
conn.commit()
```
commit() confirma e grava as alterações pendentes da conexão no banco. Assim, quando a função também fizer operações como inserir ou atualizar dados, elas serão persistidas. Neste caso, o SQLite pode confirmar a criação das tabelas automaticamente, mas deixar o commit() explícito é útil para garantir a confirmação das alterações.
<hr>

```python
conn.close()
```
conn.close() fecha a conexão com o banco depois de confirmar as alterações. Isso libera os recursos usados e evita deixar conexões abertas sem necessidade. A validação do arquivo não encontrou erros.
<hr>

