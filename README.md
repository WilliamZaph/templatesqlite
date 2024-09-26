# Template e suas características

## Esse template tem como objetivo trazer uma estrutura mínima de projeto em Django, com o banco de dados para configuração sendo o SQLite. Os comandos a seguir são para rodar o projeto no Windows, para outros sistemas operacionais, os procedimentos podem ser diferentes (normalmente, usar "python manage.py" para algumas versões do Windows e "python3 manage.py" para Linux). A seguir, temos a sequência de pré-requisitos para rodar um projeto Django em sua máquina:

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
py manage.py startapp myapp

```

#### Sendo myapp o nome que se deseja dar a aplicação

## Incluir a aplicação no `myproject/settings.py`
### Dentro do settings.py, haverá algo como:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

### Inclua a sua aplicação no INSTALLED_APPS:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myapp',
]
```

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

### Altere o arquivo `myproject/urls.py`:

```python
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('myapp.urls')),  # Inclui as rotas da sua aplicação
]

```

## Criar super usuário:

### O super usuário é quem tem acesso ao Django Admin

```bash
py manage.py createsuperuser
```

## Iniciar o servidor Django:

 ```bash
 py manage.py runserver
 ```

 ### Com o servidor rodando, é possível abrir o Django admin colocando "/admin" ao final da URL

## Modelar banco de dados com exemplo

### No arquivo `myapp/models.py`:

```python
class Genero(models.Model):
    # cada classe é uma tabela no banco de dados e herda de uma classe "models.Model"
    # cada variável é um campo da tabela
    # caso não especificado a criação de uma primary key na classe,
    # o Django cria uma primary key "id" automaticamente.
    # no nosso exemplo, pra fins de compreensão, criamos um campo id_livro

    # "primary_key=True" indica que esse campo será uma chave primária,
    # e o AutoField indica que será feito um autoincremento para cada inserção no banco
    id_genero = models.AutoField(primary_key=True)

    # CharField indica que esse campo é de caracteres
    # "max_length = 50" limita o tamanho máximo para 50
    # "unique" é equivalente ao UNIQUE do SQL
    nome = models.CharField(max_length=50, unique=True)

    # essa função é para mostrar cada gênero no Django admin no formato "id - nomedolivro"
    def __str__(self):
        return f"{self.id_genero} - {self.nome}"

class Livro(models.Model):
    id_livro = models.AutoField(primary_key=True)

    # "null = False" equivale ao "NOT NULL" do SQL
    autor = models.CharField(max_length=120, null=False)
    titulo = models.CharField(max_length=120, null=False)

    # "models.ForeignKey" sinaliza que a relação entre Livro e Gênero é 1 para muitos,
    # e, nesse caso, cria um campo "genero" na tabela "Livro",
    # referenciando o id do gênero da tabela "Gênero",
    # considerando uma aplicação no qual cada livro tem apenas um gênero.
    # "on_delete=models.CASCADE" equivale ao "ON DELETE CASCADE" do SQL
    # "on_delete=models.SET_NULL" equivale ao "ON DELETE NULL" do SQL
    # é importante ressaltar que temos que colocar null=True, para que o campo possa ser nulo
    genero = models.ForeignKey(Genero, on_delete=models.SET_NULL, null=True)

    def __str__(self):
        return f"{self.id_livro} - {self.titulo}"
```

## Jogando as mudanças no banco de dados

### Criar os arquivos de migração com base nas mudanças feitas nos models do projeto:

```bash
py manage.py makemigrations
```

#### Quando usar: Sempre que você alterar ou criar novos models, adicionar campos ou modificar os existentes.

#### O que faz: Analisa os modelos definidos no projeto e gera um arquivo de migração correspondente às mudanças detectadas. Este arquivo de migração é um script que descreve as operações necessárias para modificar a estrutura do banco de dados (criar tabelas, adicionar colunas, etc.).

### Aplicar as migrações criadas ao banco de dados:

```bash
py manage.py migrate
```

#### Quando usar: Após gerar os arquivos de migração com makemigrations, ou quando houver novas migrações a serem aplicadas no banco de dados.

#### O que faz: Executa as alterações descritas nos arquivos de migração no banco de dados real, criando ou alterando tabelas, colunas, índices, etc., para sincronizar o banco de dados com o estado atual dos models do projeto.

## Checando o banco de dados, será possível ver as tabelas criadas a partir do models.py

### As tabelas criadas recebem o prefixo "myapp_nomedatabela", pela configuração padrão do Django que cria algo como "nomedoapp_nomedatabela".

## Tornando as tabelas visíveis no Django Admin:

### No arquivo `myapp/admin.py`, faça:

```python
from django.contrib import admin
from .models import Genero, Livro

admin.site.register(Genero)
admin.site.register(Livro)
```

#### Com isso, você pode fazer manipulações no banco de dados usando a interface do Django Admin

## Realizando operações CRUD com Django

### No terminal, navegue até a pasta onde está o manage.py e abra o shell do django:
```bash
py manage.py shell
```

#### Com o shell, você pode realizar as operações CRUD (após cada operação, verifique no Django Admin se ela foi realizada com sucesso)

```python
# primeiro, você deve incluir os models para poder fazer as operações
from .models import Genero, Livro

# Create
novo_genero = "Ficção Científica"
Genero.objects.create(nome=novo_genero)
print(Genero.objects.get(nome=novo_genero))

# Read
Genero.objects.get(nome=novo_genero)

# Update
# Resgatando o livro pelo nome
atualiza_genero = Genero.objects.get(nome=novo_genero2)
# Atualizando o nome
atualiza_genero.nome = "Acadêmico"
atualiza_genero.save()

# Delete
# Resgatando o livro pelo nomes
genero = Genero.objects.get(nome=novo_genero2)
# Deletando o livro
genero.delete()
```















