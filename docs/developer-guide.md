Guia do Desenvolvedor
=====================

- [Arquivo de Configura��o](#arquivo-de-configuracao)
- [Model](#model)
- [Controller](#controller)
- [EntityController](#entitycontroller)
- [View](#view)
    - [Temas](#temas)
      - [theme.php](theme-php)
      - [Estrutura de pastas](#estrutura-de-pastas)
    - [P�ginas](#P�ginas)
    - [Layouts](#Layouts)
    - [Vis�es](#Vis�es)
    - [Partes](#Partes)
    - [Assets](#Assets)
    - [Vari�veis Acess�veis](#vari�veis-acess�veis)
    - [Verificando se um usu�rio est� logado](#verificando-se-um-usu�rio-est�-logado)
- [Autentica��o]()
- [Log]()
- [Cache]()
- [Outputs da API]()


## Arquivo de Configura��o

## Model
As classes de modelo ficam no namespace **MapasCulturais\Entities** e seus arquivos dentro da pasta [src/protected/application/lib/MapasCulturais/Entities](../src/protected/application/lib/MapasCulturais/Entities). 

Estas classes devem estender a classe abstrata [MapasCulturais\Entity](#classe-entity) e usar os [Docblock Annotations](http://docs.doctrine-project.org/en/latest/reference/annotations-reference.html) do [Doctrine](http://docs.doctrine-project.org/en/latest/index.html) para fazer o [mapeamento](http://docs.doctrine-project.org/en/latest/reference/basic-mapping.html) com a representa��o desta entidade no banco de dados (geralmente uma tabela). 

Estas podem tamb�m usar os [traits criados para entidades](#traits-das-entidades) (os que t�m o prefixo **Entity** no nome, como por exmplo o _**Entity**Files_, que � para ser usado em entidades que t�m arquivos anexos).

### Classe Entity
A classe abstrata **MapasCulturais\Entity** � a classe que deve servir de base para todoas as entidades do sistema

### Traits das Entidades

#### EntityAgentRelation
Este trait � para ser usado em entidades que podem ter agentes relacionados.

#### EntityAvatar
#### EntityFiles
#### EntityGeoLocation
#### EntityMetadata
#### EntityMetaLists
#### EntityNested
#### EntityOwnerAgent
#### EntitySoftDelete
#### EntityTaxonomies
#### EntityTypes
#### EntityVerifiable

### Verifica��o de Permiss�es

## Controller

### Actions
### M�todo render
### M�todo partial
### Retornando um JSON
### Requisitando autentica��o
### Checando permiss�o



## EntityController

## View

### Temas
Por enquanto ainda n�o temos resolvida a estrutura para m�ltiplos temas. O que temos � um tema �nico dentro da pasta **src/protected/application/themes/active**, que ser� modificado para aceitar configura��es.

#### theme.php
Este arquivo fica na pasta ra�z do tema (**src/protected/application/themes/active**) e � usado para colocar fun��es helpers usadas dentro do tema e para estender o sistema utilizando a [API de plugins](api.md).

#### Estrutura de pastas
dentro da pasta ra�z do tema
- **assets/** - *onde deve ficar tudo que � acess�vel pelo p�blico dentro da url **/public** do site*
  - **css/**
  - **fonts/**
  - **img/**
  - **vendor/**
- **layouts/** - *onde ficam os layouts do site*
    - **parts/** - *onde ficam os template parts utilizados pelo tema*
- **views/** - *onde ficam as vi�es dos controles*
- **pages/** - onde ficam os arquivos de p�ginas

### P�ginas
As p�ginas do sistema s�o arquivos **.md** (Markdown) salvos dentro da pasta **pages/** do tema. Para criar uma nova p�gina basta criar um novo arquivo **.md** dentro desta pasta. Estes arquivos s�o renderizadas pela biblioteca [PHP Markdown Extra](https://michelf.ca/projects/php-markdown/extra/).

#### Url da p�gina
Para uma p�gina cujo nome de arquivo � **nome-da-pagina.md**, a url de acesso ser� **http://mapasculturais/page/site/nome-da-pagina/**


#### T�tulo da p�gina
O texto do **primeiro h1** do conte�do da p�gina ser� utilizado como t�tulo da p�gina (tag <title>). Isto � feito via javascript.


No exemplo a seguir o t�tulo da p�gina ser� **T�tulo da P�gna**
```Markdown
# T�tulo da P�gina

Conte�do da p�gina ....

```

#### Sidebars
O Conte�do das sidebars est�o nos arquivos **_right.md** e **_left.md**

#### Substituindo uma sidebar
Voc� pode substituir uma sidebar envolvendo o conte�do que voc� deseja que substitua o conte�do padr�o com as tags **<%left left%>** para a sidebar da esquerda e **<%right right%>** para a sidebar da direita.

No exemplo a seguir substituimos a sidebar da direita por um menu com tr�s links:
```Markdown
<%right 
- [Primeiro link](#primeiro)
- [Segundo link](#segundo)
- [Terceiro link](#terceiro)
right%>

# T�tulo da P�gina

Conte�do da p�gina ....
```

#### Extendendo uma sidebar
Voc� pode extender uma sidebar, adicionando conte�do antes ou depois do conte�do padr�o, colocando um **:after** ou **:before** logo depois da tag de abertura.

No exemplo a seguir extendemos a sidebar da esquerda adicionando um menu com 2 links no final da sidebar:
```Markdown
<%left:after
## submenu da p�gina

- [Primeiro Link](#primeiro)
- [Segundo Link](#segundo)
left%>

# T�tulo da P�gina

Conte�do da p�gina ....
```


### Layouts
O layout � a "moldura" do conte�do de uma vis�o. A estrutura m�nima de um layout � a seguinte:

```HTML+PHP
<html>
    <head>
        <title><?php echo isset($entity) ? $this->getTitle($entity) : $this->getTitle() ?></title>
        <?php mapasculturais_head(isset($entity) ? $entity : null); ?>
    </head>
    <body>
        <?php body_header(); ?>
        <?php echo $TEMPLATE_CONTENT; /* aqui entra o conte�do da view */ ?>
        <?php body_footer(); ?>
    </body>
</html>
```

Por padr�o as vis�es usam o arquivo de layout **default.php**, mas voc� pode definir qual layout elas usar�o colocando a seguinte linha na primeira linha do seu arquivo de vis�o:
```PHP
$this->layout = 'nome-do-layout'; // n�o precisa do .php no nome do template
```

### Vis�es
As vis�es s�o chamadas de dentro das [actions do controller](#actions) atrav�s do [m�todo render](#m�todo-render), que inclui o [layout](#layouts) definido na view, ou do [m�todo partial](#m�todo-partial), que **n�o** inclui o layout. 

Quando a vis�o � chamada pelo m�todo render, o conte�do renderizado da vis�o � guardado na vari�vel **$TEMPLATE_CONTENT** e enviado para o layout.

#### Vis�es das actions single, create e edit
Os arquivos de vis�o **single.php**, **create.php** e **edit.php** dos controladores **agent**, **space**, **event** e **project** s�o, n�o relidade, o mesmo arquivo. O arquivo *real* � o **single.php** e os dois outros s�o *links simb�licos* para o primeiro.

Para saber, de dentro de um destes arquivos, em qual action voc� est�, voc� pode usar a propriedade **$this->controller->action**:

```HTML+PHP
<?php if($this->controller->action == 'single'): ?>
    <p>voc� est� visualizando a entidade</p>
<?php elseif($this->controller->action == 'edit'): ?>
    <p>voc� est� editando a entidade</p>
<?php else: ?>
    <p>voc� est� criando uma nova entidade<p>
<?php endif; ?>
```

Se voc� s� deseja saber se est� no modo de edi��o use a fun��o **is_editable()**:
```HTML+PHP
<?php if(is_editable(): ?>
    <p> voc� est� em modo de edi��o (edit ou create). </p>
<?php else: ?>
    <p> voc� est� somente visualizando a entidade. <p>
<?php endif; ?>
```

### Partes
As partes s�o blocos de c�digo que podem ser incluidos em diferentes views, layouts ou mesmo dentro de outras partes. Estes blocos de c�digo devem ficar, por padr�o, na pasta **layouts/parts/** do tema.

Para usar uma parte cujo nome de arquivo � **uma-parte.php** basta chamar o m�todo **part** da seguinte forma:

```HTML+PHP
<div> A parte ser� incluida a seguir: </div>
<?php $this->part('uma-parte'); ?>
```

#### Enviando vari�veis para dentro das partes
Voc� pode enviar vari�veis para usar dentro das partes. Isto � �til em v�rias situa��es, por exmplo quando voc� quer que uma parte seja usada dentro de um loop e voc� tem que enviar o item atual do loop para usar dentro da parte.

No exemplo a seguir, passamos uma vari�vel chamada **user_name**, com o valor **"Fulano de Tal"**, para dentro da parte **uma-parte**.
```PHP
// dentro de algum arquivo de view, layout ou mesmo outra parte
$this->part('uma-parte', ['user_name' => 'Fulano de Tal']);
```

```HTML+PHP
<!-- dentro do arquivo layouts/parts/uma-parte.php -->
<span>Nome de usu�rio: <?php echo $user_name; ?></span>
```


### Assets
Os assets s�o arquivos est�ticos (.css, .js, imagens, etc.) utilizados pelo tema. 

Para imprimir a url de um asset use a fun��o **$this->asset()**. J� se voc� deseja adicionar um js ou css use as fun��es **$app->enqueueScript()** e **$app->enqueueStyle()**.

#### M�todo Asset
O M�todo **asset** do objeto de fun��o serve para imprimir ou somente retornar a url de um asset. Este m�todo aceita dois par�metros: 

O primeiro, **$file**, � o caminho do arquivo deseja dentro da pasta assets do tema, como exemplo a string "img/uma-image.jpg".

O Segundo, **$print**, � opcional e tem como padr�o *true*. Se for passado *false* a fun��o somente retornar� a url, mas n�o imprimir� nada.

##### Adicionando uma imagem
O exemplo a seguir usa uma imagem chamada **logo.png** que est� na pasta **assets/img/** do tema.
```HTML+PHP
<img src="<?php $this->asset('img/logo.png'); ?>" />
```

##### Criando um link para um asset
O exemplo a seguir cria um link para o arquivo **documento.pdf** que est� na pasta **asset/** do tema.
```HTML+PHP
<a href="<?php $this->asset('documento.pdf'); ?>" >Documento</a>
```

#### M�todo enqueueStyle
Este m�todo � utilizado para adicionar arquivos .css que ser�o utilizados pela vis�o, layout ou parte. Este m�todo aceitas 5 par�metros (**$group**, **$script_name**, **$script_filename**, *array* **$dependences**, **$media**), sendo os dois �ltimo opcional.

H� tr�s grupos de estilos no sistema: **vendor**, que s�o estilos utilizados pelas bibliotecas, **fonts** que s�o as fontes utilizadas, e **app**, que s�o os estilos escritos exclusivamente para o tema. 

##### Adicionando um estilo
O exemplo a seguir adiciona um estilo chamado **um-estilo.css** escrito para a aplica��o.

```PHP
$app->enqueueStyle('app', 'um-estilo', 'css/um-estilo.css');
```

#### M�todo enqueueScript
Este m�todo � utilizado para adicionar arquivos .js que ser�o utilizados pela vis�o, layout ou parte. Este m�todo aceitas 4 par�metros (**$group**, **$script_name**, **$script_filename**, *array* **$dependences**), sendo o �ltimo opcional.

H� dois grupos de scripts no sistema: **vendor**, que s�o as bibliotecas utilizadas, e **app**, que s�o os scripts escritos exclusivamente para o tema. 

##### Adicionando um script
O exemplo a seguir adiciona um script chamado **um-script.js** escrito para a aplica��o.

```PHP
$app->enqueueScript('app', 'um-script', 'js/um-script.js');
```

##### Adicionando um script que depende de outro
O exemplo a seguir adiciona uma biblioteca que depende de outra biblioteca.

```PHP
$app->enqueueScript('vendor', 'jquery-ui-datepicker', '/vendor/jquery-ui.datepicker.js', array('jquery'));
$app->enqueueScript('vendor', 'jquery', '/vendor/jquery/jquery-2.0.3.min.js');
```

#### Ordem de impress�o das tags de estilos e scripts
Os grupos de estilos e scripts ser�o impressos na seguinte ordem e dentro dos grupos os estilos/scripts ser�o ordenados conforme suas depend�ncias:
- Estilos do grupo **vendor**
- Estilos do grupo **font**
- Estilos do grupo **app**
- Scripts do grupo **vendor**
- Scripts do grupo **app**

### Vari�veis Acess�veis
De dentro dos arquivos das vis�es (views, layouts e parts) as seguintes vari�veis est�o acess�veis:
- **$this** - inst�ncia da classe *MapasCulturais\View*.
    - **$this->assetUrl** - url dos assets.
    - **$this->baseUrl** - url da ra�z do site.
    - **$this->controller** - o controller que mandou renderizar a vis�o.
    - **$this->controller->action** - a action que mandou renderizar a vis�o.
- **$app** - inst�ncia da classe *MapasCulturais\App*.
- **$app->user** - o usu�rio que est� vendo o site. Este objeto � inst�ncia da classe *MapasCulturais\Entities\User*, se o usu�rio estiver logado, ou inst�ncia da classe *MapasCulturais\GuestUser*, se o usu�rio n�o estiver logado.
- **$app->user->profile** - o agente padr�o do usu�rio. Inst�ncia da classe *MapasCulturais\Entities\Agent*. *(somente para usu�rios logados)*
- **$entity** - � a entidade que est� sendo visualizada, editada ou criada. *(somente para as actions single, edit e create dos controladores das entidades agent, space, project e event. Dentro das partes somente se esta foi [enviada](#enviando-vari�veis-para-dentro-das-partes))*

### Verificando se um usu�rio est� logado
Para saber se um usu�rio est� logado voc� pode verificar se o usu�rio n�o � *guest*. 
```HTML+PHP
<?php if( ! $app->user->is('guest') ): ?>
    <p>O usu�rio est� logado e o nome do agente padr�o dele � <?php echo $app->user->profile->name; ?> ?></p>
<?php else: ?>
    <p>O usu�rio n�o est� logado</p>
<?php endif; ?>
```
