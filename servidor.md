# Servidor Waitress

### Instalação
```cmd
pip install waitress
```
<hr>

### servidor.py
```python
from waitress import serve
from app import app
from database import init_db
import socket

def get_local_ip():
    s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    try:
        s.connect(('8.8.8.8', 80))
        ip = s.getsockname()[0]
    except Exception:
        ip = '127.0.0.1'
    finally:
        s.close()
    return ip

if __name__ == '__main__':
    init_db()
    ip = get_local_ip()
    print(f"\n🏢 Condomínio rodando em:")
    print(f"   Local : http://127.0.0.1:5000")
    print(f"   Rede  : http://{ip}:5000\n")
    serve(app, host='0.0.0.0', port=5000, threads=6)
```
<hr>

### Rodar o servidor
```cmd
python servidor.py
```
