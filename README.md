# Tutorial Django REST 01 – Criando Uma Aplicação Web para Biblioteca e sua API

Neste tutorial vamos construir um Web Site básico para uma biblioteca, e então estendê-lo para uma API **Django REST Framework**. Para isto, siga atentamente os passos abaixo.


**Observação Importante: somente o faça depois de fazer o Tutorial 01 - Django**

##**Passo 1: Configure seu ambiente de desenvolvimento**

Sempre que você estiver iniciando um projeto de desenvolvimento web, é uma boa ideia configurá-lo primeiro.

1.1) Abra o Terminal no VS Code. Primeiro digite (CTRL+Shift+P) e use a opção “**Ver: Toggle Terminal**” ou “**Ver: Alternar Terminal**”.

1.2) Digite na linha de comando do Terminal:

```
cd Django_Tutoriais
mkdir Tutorial_REST_01
cd Tutorial_REST_01
```

1.3) Uma vez dentro do diretório `Tutorial_REST_01`, crie um ambiente virtual. Para isto, digite o comando a seguir:

```
python3 -m venv bibenv
```

1.4) Agora você precisa ativar o ambiente virtual criado no item anterior, executando o comando abaixo:

**Linux/Mac**

```
source bibenv/bin/activate
```

**Windows**

```
.\bibenv\Scripts\activate.bat
```

Ou

```
.\bibenv\Scripts\Activate.ps1
```

Você saberá que seu ambiente virtual foi ativado, porque o _prompt_ do console no Terminal mudará. Deve ser assim:

```
(bibenv) $
```

1.5) Agora que você criou um ambiente virtual, é hora de instalar o **Django**. Digite na linha de comando:

```
(bibenv) $ pip install django
```

##**Passo 2: Criando seu projeto em Django**

2.1) Certifique-se de que você está dentro do diretório `Tutorial_REST_01` e o ambiente virtual ativado. Agora, digite o comando abaixo para criar um projeto.

```
(bibenv) $ django-admin startproject config .
```

>**Observação: ao criar um novo projeto Django chamado “config” certifique-se de incluir o ponto (.) no final do comando para que ele seja instalado no diretório atual.**

O comando acima irá criar uma pasta chamada “**config**” contendo alguns arquivos. No painel esquerdo do **VS Code**, você verá uma estrutura de diretório que se parece com a figura abaixo.

![pasta website](../img_readme/pasta_website.png)


##**Passo 3: Testando seu servidor Django**

3.1) Depois que sua estrutura de arquivos estiver configurada, você pode iniciar o servidor de desenvolvimento que já vem embutido no Django. Para verificar se a configuração foi bem-sucedida, execute o seguinte comando no console do Terminal:

```
python manage.py runserver
```
Ao posicionar o mouse no link “**http://127.0.0.1:8000/**” você verá a seguinte mensagem:

3.2) Observe no console do Terminal as mensagens da figura abaixo:

![console terminal](../img_readme/console_terminal.png)


No **Windows** irá aparecer “**Seguir o link (ctrl + click)**”. Ao efetuar esta operação, você será direcionado para uma aba do seu browser, e, se tudo estiver correto, você verá uma página da web como a da figura abaixo.

![tela django](../img_readme/tela_django.png)

3.3) Parabéns, você acabou de criar um projeto, nossa configuração está correta e você o testou no servidor de desenvolvimento. Agora o Django está pronto para começarmos a desenvolver.

##**Passo 4: Criando uma aplicação em Django**

Para esta parte do tutorial, criaremos uma “**app**” chamada “**books**”.

4.1) Para criar uma “**app**”, execute o seguinte comando:

```
python manage.py startapp books
```

Este comando irá criar um diretório chamado “**books**” com vários arquivos. Veja a estrutura na figura abaixo.

![pasta hello_world](../img_readme/pasta_books.png)


4.2) Agora que você criou a “**app**”, temos que “instalá-la” no seu projeto. Abra o arquivo (`config/settings.py`) e adicione a seguinte linha de código destacada em INSTALLED_APPS: (**Não se esqueça de colocar a vírgula após a _string_**).

![INSTALLED_APPS](../img_readme/installed_apps.png)

Essa linha de código indica que seu projeto agora sabe que o aplicativo que você acabou de criar existe.

Surge uma pergunta: de onde obtivemos a referência a “hello_world”?

Resposta: quando criamos uma nova “app”, no **Passo 4.1**, o Django gerou um arquivo chamado “apps.py” na pasta “hello_world”. E dentro desse arquivo, ele criou uma classe chamada “HelloWorldConfig”. Essa classe nos permite fazer referência ao aplicativo (i.e. “app”) para o projeto. O conteúdo do arquivo “hello_world/apps.py” está abaixo.

```python
from django.apps import AppConfig
class HelloWorldConfig(AppConfig):
	default_auto_field = 'django.db.models.BigAutoField'
	name = 'hello_world'
```

4.3) Neste mesmo arquivo (`config/settings.py`) procure pelo comentário “**#Internationalization**” e altere as configurações para `LANGUAGE_CODE` e `TIME_ZONE`. Veja a figura abaixo. 

![Internationalization](../img_readme/internationalization.png)

##**Passo 5: Criando um Banco de Dados**

5.1)	Na linha de comando do VS Code, digite o seguinte comando:

```python
python manage.py migrate
```

O comando “**migrate**” acima serve para criar um Banco de Dados (BD) inicial com base nas configurações padrões do Django.

Se você olhar dentro do nosso diretório com o comando “ls”, verá que agora existe um arquivo chamado “**db.sqlite3**” que representa esse BD. Por padrão, o Django utiliza o SQLite.

[SQLite Website](https://www.sqlite.org/index.html)
 

Tecnicamente, um arquivo “**db.sqlite3**” é criado na primeira vez que você executa o comando `python manage.py migrate` (**Passo 5.1** acima) ou o comando `python manage.py runserver`. (**Passo 3.1** também acima).

Entretanto, “**migrate**” irá sincronizar o Banco de Dados com o estado atual de quaisquer modelos (“models”) de BD contidos no projeto e listados em “INSTALLED_APPS” (**Passo 4.2**). 

Em outras palavras, para garantir que o BD reflita o estado atual do seu projeto, você precisará executar o comando “migrate” (e também o comando “**makemigrations**”) cada vez que atualizar um “model”. Este último, veremos em um passo futuro.

5.2) 	Agora, para confirmar se tudo está funcionando corretamente, reinicie nosso servidor no Terminal (`python manage.py runserver`), e visite “**http://127.0.0.1:8000**”. Você deverá a seguinte página da web:

![tela django](../img_readme/tela_django.png)

## **Passo 6: Criando um Modelo (“Model”) de Banco de Dados**

6.1)	Abra o arquivo `models.py` no diretório (`books/models.py`). Já existe uma linha de código lá que importa um módulo chamado models”. Este módulo nos ajuda a construir novos modelos, os quais irão “modelar” as características dos dados no nosso BD. 

```python
from django.db import models
# Create your models here
```


Agora adicione o seguinte código a este arquivo (em destaque):

```python
class Book(models.Model):
	title = models.CharField(max_length=250)
	subtitle = models.CharField(max_length=250) 
	author = models.CharField(max_length=100)
	isbn = models.CharField(max_length=13) 

	def __str__(self):
		return self.title 
```

> Observe que criamos um “model” de BD chamado “**Book**”, que contém quatro campos: **title** (título do livro), **subtitle** (subtítulo do livro), **author** (autor) e **isbn**. Também incluímos um método mágico (__str__), que irá mostrar o título do livro no módulo “**admin**”. Django fornece muitos campos de “**model**" suportando tipos comuns de conteúdo como caracteres, datas, inteiros, emails e assim por diante.
> Visite o link abaixo:
> 
> [Model Field Reference](https://docs.djangoproject.com/pt-br/4.2/ref/models/fields/)

Agora que nosso novo modelo (“model”) foi criado, precisamos ativá-lo. No futuro, sempre que criarmos ou modificarmos um modelo existente, precisaremos atualizar o Django em um processo de duas etapas:

* Criamos um arquivo de migração com o comando “makemigrations”. Este arquivo cria uma referência de quaisquer alterações nos modelos do Banco de Dados, o que significa que podemos rastrear alterações e depurar erros caso necessário.

* Construímos o Banco de Dados real com o comando “**migrate**”, que executa as instruções no arquivo criado na etapa acima.


6.2)	Certifique-se de que o servidor local esteja parado digitando “**Control+C**” na linha de comando e, em seguida, execute os comandos abaixo: 

```python
python manage.py makemigrations books
```

Você verá a seguinte mensagem:

```python
Migrations for 'books':
  books/migrations/0001_initial.py
    - Create model Book
```

Agora execute este comando:

```python
python manage.py migrate
```

As mensagens serão estas:

```python
Operations to perform:
  Apply all migrations: admin, auth, books, contenttypes, sessions
Running migrations:
  Applying books.0001_initial... OK
```

## **Passo 7: Configurando o Módulo `Admin` do Django**

Um dos recursos matadores do Django é sua interface de administração já embutida que fornece uma maneira visual de interagir com os dados. Ela surgiu porque o Django foi originalmente construído como um CMS (_Content Management System_ – Sistema de Gerenciamento de Conteúdo) de um jornal. 

A ideia era que os jornalistas pudessem escrever e editar suas histórias no módulo “**admin**” sem precisar mexer no "código". 

Com o tempo, este aplicativo administrativo integrado evoluiu para uma ferramenta fantástica e pronta para uso para gerenciar todos os aspectos de um projeto Django. 


7.1) Para usar o Django “**admin**”, primeiro precisamos criar um super usuário (“**superuser**”) que possa fazer login. No console da linha de comando, digite o comando abaixo e responda aos prompts de nome de usuário (“**Username**”), e-mail (“**Email**”) e senha (“**Password**)”:
 
```python
python manage.py createsuperuser
```

Os _prompts_ estão descritos abaixo. Os valores digitados (em destaque) são ilustrativos. Escolha o mais adequado ao seu caso:

```python
Username (leave blank to use 'wsv'): wsv
Email: will@learndjango.com
Password:
Password (again):
Superuser created successfully.
```

> **Observação importante: Ao digitar sua senha, ela não aparecerá visível no console da linha de comando por motivos de segurança.**

7.2) Agora, reinicie o servidor no Terminal (`python manage.py runserver`), e no seu browser visite “**http://127.0.0.1:8000/admin**”. Você deverá ver a tela de login do administrador (“**admin**”) que está na figura abaixo:

![tela login admin](../img_readme/tela_login_admin.png)


7.3) Faça o login inserindo o nome de usuário (“**Username**”) e a senha (“Password”) que você acabou de criar no Passo 7.1. Você verá a página inicial do Django “**admin**” a seguir:

![tela admin logada](../img_readme/tela_admin_logada.png)

Surge uma pergunta: onde está nosso “**app**” de “**books**”? Ele não é exibido na página principal do administrador! Assim como devemos adicionar explicitamente novos aplicativos à configuração “**INSTALLED_APPS**”, também devemos atualizar o arquivo `admin.py` do nosso aplicativo para que ele apareça no módulo administrador (i.e. “Django admin”).

7.4) Abra o arquivo `admin.py` no diretório (`books/admin.py`), e adicione o seguinte código para que o “**model**” “**Book**” seja exibido.

```python
# books/admin.py
from django.contrib import admin
from .models import Book

admin.site.register(Book)
```

Agora, o Django sabe que deve exibir nosso “app” de “**books**” e o seu “model” (modelo) de BD chamado “**Book**” na página do “admin”. Se você atualizar seu browser (navegador), verá que ele aparece:

![tela admin logada book](../img_readme/tela_logada_admin_book.png)

7.5) Agora vamos criar nosso primeiro livro da biblioteca para nosso Banco de Dados. Clique no botão “**+Add**” (“+Adicionar”) que está do lado oposto de “**Books**” e insira os dados nos campos indicados na figura abaixo. Você pode entrar com quaisquer valores.

![add book](../img_readme/add_book.png)

7.6) Em seguida, clique no botão “**Save**” (“Salvar”), que o redirecionará para a página principal do “**Book**”. 
A figura abaixo irá aparecer.

![book saved](../img_readme/book_saved.png)

## **Passo 8: Criando nossas Views/Templates/URLs**

Para exibir o conteúdo do nosso Banco de Dados em nossa página inicial, temos que conectar nossas “**views**”, “**templates**” e as “**URLs**” (também são chamadas de “**URLConfs**”). 

Queremos listar o conteúdo do nosso “model” do BD. Felizmente, esta também é uma tarefa comum no desenvolvimento web e o Django vem equipado com uma classe genérica chamada “[**ListView**](https://docs.djangoproject.com/pt-br/4.2/ref/class-based-views/generic-display/#listview)”.

8.1) Abra o arquivo `views.py` no diretório (`books/views.py`). Adicione o código abaixo nele. 

```python
# books/views.py
from django.views.generic import ListView 
from .models import Book 

class BookListView(ListView):
	model = Book 
   template_name = 'book_list.html'
```

Na primeira linha, importamos “**ListView**” e na segunda linha importamos o “model” “**Book**”. Na “**View**” que chamamos de “**BookPageView**”, criamos uma classe filha de “**ListView**” e especificamos o “model” e o “template” corretos.

Nossa “View” está completa, mas ainda precisamos configurar nossos URLs e criar nosso “template”. 

8.2) Em seguida, atualize o campo “**DIRS**” em nosso arquivo `config/settings.py` para que o Django saiba como olhar para este diretório de “templates”.

```python
# config/settings.py
TEMPLATES = [
   {
      ...
      'DIRS': [str(BASE_DIR.joinpath('templates'))],
      ...
   }, 
] 
```

8.3) Crie um diretório chamado `templates` (`mkdir templates`) dentro da pasta `books`. Neste diretório (`templates`) crie arquivo chamado `book_list.html`. Dentro dele coloque o seguinte código:

```html
<!-- templates/book_list.html -->
<h1>Todos os livros</h1>
{% for book in object_list %}
   <ul>
		<li>Título: {{ book.title }}</li> 
		<li>Subtítulo: {{ book.subtitle }}</li> 
		<li>Autor: {{ book.author }}</li> 
		<li>ISBN: {{ book.isbn }}</li>
  </ul>
{% endfor %}
```

O Django vem com uma [**Linguagem de Template**](https://docs.djangoproject.com/pt-br/4.2/ref/templates/language/) que permite um tipo de lógica básica. Aqui usamos a [**tag for**](https://docs.djangoproject.com/pt-br/4.2/ref/templates/builtins/#std:templatetag-for) para percorrer todos os livros disponíveis. 

As tags de “**template**” devem ser incluídas entre chaves de abertura/fechamento. Então o formato é sempre `{% for ... %}` e para fechar nosso loop usamos `{% endfor %}`.

O _looping_ que estamos executando é no objeto que contém todos os livros disponíveis em nosso modelo, que é uma cortesia da “**ListView**”. O nome deste objeto é `object_list`. Portanto, para fazer um _loop_ sobre cada livro, escrevemos `{% for book in object_list %}`. E, em seguida, exibir cada campo do nosso “**model**”.

Adicionar um nome explícito desta forma torna mais fácil para outros membros de uma equipe, por exemplo, um designer, entender e raciocinar sobre o que está disponível no contexto do “**template**”. Agora temos de atualizá-lo, para que faça referência a `all_books_list` em vez de `object_list` (**Passo 8.3**).

8.4) Abra o arquivo `views.py` no diretório (`books/views.py`). Agora adicione o seguinte código a este arquivo (em destaque):

```python
# books/views.py
from django.views.generic import ListView 
from .models import Book 

class BookListView(ListView):
	model = Book 
   template_name = 'book_list.html'
   context_object_name = 'all_books_list'
```

8.5) Abra arquivo `book_list.html` no diretório `templates/book_list.html`. Dentro dele altere o seguinte código:

```html
<!-- templates/book_list.html -->
<h1>Todos os livros</h1>
{% for book in all_books_list %}
   <ul>
		<li>Título: {{ book.title }}</li> 
		<li>Subtítulo: {{ book.subtitle }}</li> 
		<li>Autor: {{ book.author }}</li> 
		<li>ISBN: {{ book.isbn }}</li>
  </ul>
{% endfor %}
```

8.6) O último passo é configurar nosso URLConfs. Vamos começar com o arquivo `config/urls.py` onde incluímos nosso “app” de “books” e adicionamos o  “include” na segunda linha. Para isto, abra o arquivo `config/urls.py`. O seu conteúdo deve ser assim.

```python
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```

Agora precisamos dizer ao Django explicitamente que precisamos ativar uma “View” para uma URL específica. Altere o conteúdo do arquivo acima para:

```python
from django.contrib import admin
from django.urls import path, include
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('books.urls')),
]
```

8.7) Crie um arquivo na pasta `books` chamado `books/urls.py`. Adicione o código abaixo.

```python
from django.urls import path
from .views import BookListView

urlpatterns = [
    path('', BookListView.as_view(), name='home'),
]
```

8.8) Agora, ao reiniciar o servidor no Terminal (`python manage.py runserver`), visite “**http://127.0.0.1:8000**”. Você deverá a seguinte página da web:

![all books listed](../img_readme/all_books_listed.png)


## **Passo 9: Adicionando Novos Livros (Books)**

Para adicionar novos livros à nossa biblioteca, volte ao módulo “**admin**” (digite “**http://127.0.0.1:8000/admin**” na linha de endereço do seu browser) e crie mais dois livros. Minha tela se parece com a figura abaixo.

![more books included admin](../img_readme/more_books_included_admin.png)

Se você retornar à página inicial no seu browser (“**http://127.0.0.1:8000/**”), verá que ela exibe automaticamente nossos livros formatados.

![more books listed](../img_readme/more_books_listed.png)

Por meio de todos esses passos anteriores criamos um site tradicional com o Django. Agora vamos adicionar uma API a ele!

---


## **Passo 10: Incluindo o Django REST Framework**

Django REST Framework é adicionado como qualquer outro aplicativo adicional. Certifique-se de encerrar o servidor local com “**Control+C**” se ele ainda estiver em execução. 

10.1) Em seguida, na linha de comando, digite os três comandos abaixo:

```python
(bibenv) $ pip install djangorestframework
(bibenv) $ pip install markdown
(bibenv) $ pip install django-filter
```

10.2) Abra o arquivo (`config/settings.py`) e adicione a seguinte linha de código abaixo em **INSTALLED_APPS**: (Não se esqueça de colocar a vírgula após a string).

![DRF installed](../img_readme/installed_apps_rest_framework.png)

Por fim, nossa API irá expor um único “**endpoint**” que lista todos os livros em JSON. Portanto, precisaremos de uma nova rota de URL, uma nova “**view**” e um novo arquivo “**serializer**” (veremos mais sobre isso em breve nos próximos passos).

Existem várias maneiras de organizar esses arquivos, mas uma abordagem ideal é criar um aplicativo de API dedicado (API app). Dessa forma, mesmo que adicionemos mais aplicativos no futuro, cada aplicativo pode conter os “models”, “views”, “templates” e URLs necessários para páginas da Web dedicadas, mas todos os arquivos específicos da API para todo o projeto ficarão em um aplicativo de API dedicado. Vamos primeiro criar um outro aplicativo de API.

10.3) Para criá-lo, execute o seguinte comando:

```python
python manage.py startapp api
```

10.4) Abra o arquivo (`config/settings.py`) e adicione as seguintes linhas de código em **INSTALLED_APPS**: (Não se esqueça de colocar a vírgula após a _string_).

![installed apps api](../img_readme/installed_apps_api.png)

O aplicativo “**api**” não terá seus próprios “models” de banco de dados, portanto, não há necessidade de criar um arquivo de migração e executar o comando de migração (“**run migrate**”) para atualizar o BD.

## **Passo 11: Adicionando os URLs da API**

Vamos começar com nossas configurações de URL. Adicionar um endpoint de API é como configurar as rotas de um aplicativo Django tradicional. Primeiro, no nível do projeto, precisamos incluir o aplicativo api e configurar sua rota de URL, que será “**api/**”.

11.1) Abra o arquivo `config/urls.py`, e altere o conteúdo dele:

```python
# config/urls.py
from django.contrib import admin 
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('books.urls')),
    path('api/', include('api.urls')),
]
```

11.2) Crie um arquivo na pasta `api` chamado `api/urls.py`. Adicione o código abaixo.

```python
# api/urls.py
from django.urls import path 
from .views import BookAPIView

urlpatterns = [
    path('', BookAPIView.as_view()),
]
```
---

## **Passo 12: Criando as Views da API**

O próximo passo é nosso arquivo “**views.py**” que depende das visualizações de classes genéricas integradas do Django REST Framework. Elas imitam deliberadamente as visões genéricas baseadas em classes tradicionais do Django em formato, mas não são a mesma coisa.
Para evitar confusão, alguns desenvolvedores chamarão um arquivo de visualizações da API de “**apiviews.py**” ou somente “**api.py**”. Como você está trabalhando em um aplicativo de API dedicado, não é confuso chamá-lo apenas de “**views.py**”.

12.1) Abra o arquivo `api/views.py` e atualize seu conteúdo.

```python
# api/views.py
from rest_framework import generics
from books.models import Book
from .serializers import BookSerializer

class BookAPIView(generics.ListAPIView):
	queryset = Book.objects.all()
 	serializer_class = BookSerializer
```

Nas primeiras linhas do arquivo editado acima, importamos a classe de visualizações genéricas do Django REST Framework, os “**models**” do nosso aplicativo (app) “**Book**”, e os serializadores (“**serializers**”) do nosso aplicativo (app) “**api**”, que serão criados no próximo passo.

Em seguida, criamos uma `BookAPIView` que herda da classe `ListAPIView` para criar um endpoint somente de leitura para todas as instâncias de “**Book**”.

Os únicos dois passos necessários em nossa “**view**” são especificar o conjunto de consultas (“**queryset**”) que contém todos os livros disponíveis e, em seguida, a “**serializer_class**” que será chamada de “**BookSerializer**”.

---

## **Passo 13: Criando os Serializers**

Um “**serializer**” converte dados em um formato fácil de usar na Internet. Normalmente o formato é **JSON**, e ele é exibido em um “**endpoint**” de API. Ou seja, ele serve para converter “**models**” do Django em **JSON**.

13.1) Crie um arquivo na pasta “**api**” chamado `api/serializers.py`. Adicione o código abaixo.

```python
# api/serializers.py
from rest_framework import serializers
from books.models import Book

class BookSerializer(serializers.ModelSerializer): class Meta:
	model = Book
	fields = ('title', 'subtitle', 'author', 'isbn')
```

---

## **Passo 14: Visualizando os Serializers no Browser**

14.1) Agora, ao reiniciar o servidor no Terminal (`python manage.py runserver`), visite “**http://127.0.0.1:8000/api**”. Você deverá a seguinte página da web:

![visualizando serializer](../img_readme/visualizando_serializer.png)

Uau olhe isso! O Django REST Framework fornece essa visualização por padrão. E há muitas funcionalidades incorporadas nesta página que exploraremos ao longo dos outros tutoriais. Por enquanto, quero que você compare esta página com o endpoint **JSON** bruto. Clique no botão “**GET**” e selecione “**json**” no menu suspenso. Você verá a figura abaixo.

![visualizando serializer json](../img_readme/visualizando_serializer_json.png)

Observe que existem três opções de visualização: “**JSON**”, “**Dados brutos**” e “**Cabeçalhos**”. É assim que o nosso endpoint da API se parece. Caso escolha a opção “**Dados brutos**”, a figura abaixo será mostrada.

![visualizando serializer json](../img_readme/visualizando_serializer_bruto.png)

Acho que podemos concordar que a versão do Django REST Framework é mais atraente.