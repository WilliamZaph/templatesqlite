# Template e suas características

## Esse templete tem como objetivo trazer uma estrutura mínima de projeto em Django, com o banco de dados para configuração sendo o SQLite. Os comandos a seguir são para rodar o projeto no Windows, para outros sistemas operacionais, os procedimentos podem ser diferentes. A seguir, temos a sequência de pré-requisitos para rodar um projeto Django em sua máquina:

### 1. Verificar se o Python está instalado:


```bash
py --version 
```

- Caso não esteja com o Python instaldo em sua máquina, vá no site e baixe a versão mais atualizada: https://www.python.org/downloads/

### 2. Com o python instalado faça a instalação do pip, que é o gerenciador de pacotes do python:

- #### Faça o download do script https://bootstrap.pypa.io/get-pip.py
- #### Abra seu terminal na pasta que você baixou o arquivo `get-pip.py` e rode o comando:

```bash
py get-pip.py
```

### Com essas ferramentas instaladas, agora é possível instalar o Django:


```bash
django-admin startproject myproject

```

#### Sendo "myproject" o nome que você deseja dar ao seu projeto.

#### A partir daqui, caso queira deixar o projeto criado manualmente igual ao do template, basta alterar o conteúdo dos arquivos.

## Criar a aplicação

```bash
django-admin startproject myproject

```

#### Sendo myapp o nome que se deseja dar a aplicação

## Configurar `urls.py`
### Altere o arquivo `myapp/urls.py`, que vai definir as rotas da aplicação:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
]

```

## Criar uma view

### No arquivo `myapp/views.py`:


```python
from django.http import HttpResponse

def home(request):
    return HttpResponse("Olá, Django!")
```

## Incluir URLs 

#### Altere o arquivo `myproject/urls.py`:

```python
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('myapp.urls')),  # Inclui as rotas da sua aplicação
]

```

## Iniciar o servidor Django:

 ```python
 py managa.py runserver
 ```