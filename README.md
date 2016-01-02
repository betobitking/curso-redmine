# Sobre o Redmine


# Sobre o livro
Neste livro veremos como desenvolver plugins para Redmine 3.1. Com foco na teoria e técnicas de programação da framework Rails e como se aplica ao Redmine.

<sub><sup>Não é um livro de Ruby on Rails</sup></sub>

### Fluxo de Dados


### Estrutura de Pastas

#### Subpastas importantes
##### App

##### Public

[Lista de temas da Comunidade](http://www.redmine.org/projects/redmine/wiki/Theme_List)

### Plugins
#### Quando devemos desenvolver um plugin?


#### Como criar um plugin

#### Criando um modelo


#### Criando um Controller

#### Adicionando rota



#### Adicionando um link no menu

O redmine possui diversos menus que você pode adicionar links para seus controllers, são eles:

* :top_menu - menu superior esquerdo
* :account_menu - menu superior direito, onde ficam os links de login e sair
* :application_menu - menu principal fora de projetos
* :project_menu - menu principal dentro de projetos
* :admin_menu - menu dos administradores

Para adicionar um menu, é necessário editar o arquivo init.rb e dentro do registro do plugin usar o método menu que tem a sintaxe:

```rb
menu(menu_name, item_name, url, options={})
```

No nosso exemplo ficaria:

```rb
  menu :application_menu, :polls, { controller: 'polls', action: 'index' }
```

As opções que podemos passar são:

* :param - the parameter key that is used for the project id (default is :id)
* :if - a Proc that is called before rendering the item, the item is displayed only if it returns true
* :caption - the menu caption that can be:
  * a localized string Symbol
  * a String
  * a Proc that can take the project as argument
* :before, :after - specify where the menu item should be inserted (eg. :after => :activity)
* :first, :last - if set to true, the item will stay at the beginning/end of the menu (eg. :last => true)
* :html - a hash of html options that are passed to link_to when rendering the menu item

Com isso, criaremos um link para a nossa action no menu principal fora dos projetos

#### Internacionalização

A última coisa que falta para o menu, é que nossos clientes são Brasileiros e falam português, eles não querem polls e sim enquetes escrito no menu. Para fazer essa alteração, iremos criar um arquivo chamado pt-BR.yml dentro do config/locates do plugin e preencher com o seguinte conteúdo:

```yml
pt-BR:
  label_polls: Enquetes
```

Caso a opção caption não seja passada, **e ela não deve ser**, o redmine usa o _menu_name_ préfixado com _label_ para fazer a internacionalização do menu.

Para saber mais sobre internacionalização no rails: http://guides.rubyonrails.org/i18n.html

#### Criando uma View

Agora que já temos uma rota, um controller e um botão que o usuário pode clicar para acessar nosso controller, precisamos codificar o que o usuário vai ver quando acessar nossa action. Como falado anteriormente o Rails vai procurar dentro da pasta com o mesmo nome do controller, _polls_, uma view com o mesmo nome da action, _index_.

Então criaremos o arquivo index.html.erb dentro da pasta app/views/polls do plugin e colocaremos o seguinte conteúdo:

```erb
<h1>Polls</h1>
<ul>
  <% @polls.each do |polls| %>
    <li><%= polls.question %></li>
  <% end %>
</ul>
```

> O que diabos é .erb depois do .html?

erb ou Embedded Ruby ou mesmo eRuby é o template padrão do Rails, existem diversos outros e a comunidade é bem dividida nesse ponto. O rails permite adicionar qualquar preprocessador de arquivo estático, sempre lendo as extensões da direita para esquerda. Se tivéssemos um javascript que quiséssemos que antes dele ser enviado para o cliente, rodasse código ruby, poderíamos criar o arquivo _file.js.erb_.

Para entender mais sobre views, que é um dos pontos mais completos do Rails. Ele aceita partials, layouts, content, e várias outras features que facilitam o **dry**, leia o http://guides.rubyonrails.org/layouts_and_rendering.html

#### Assets

Vale ressaltar que view feia não agrada cliente e muitas vezes precisamos escrever css e javascript específicos para um plugin. Para isso o redmine permite adicionar assets da seguinte forma:

```erb
<% content_for :header_tags do %>
  <%= stylesheet_link_tag 'css_name', plugin: 'plugin_name' %>
  <%= javascript_link_tag 'js_name' , plugin: 'plugin_name' %>
<% end %>
```

Se você leu o guia sobre layout como sugerido, saberá que este _content_for_ rodará o bloco de código passado por ele na posição onde tiver um yield(:header_tags), que no nosso caso fica dentro da tag

```html
<header></header>
```

O método _stylesheet_link_tag_, adiciona um link para o css com nome _css_name_ que se encontra dentro do plugin com nome igual _plugin_name_, assim como o _javascript_link_tag_ fará para o javascript.

Com isso você não precisará modificar o código direto dos assets do redmine para fazer uma modificação específica da sua view.

#### Permissões
#### Módulos
#### Hooks
##### Hooks nas Views
##### Hooks nos controllers
#### Fazendo um plugin ser configurável
#### Sobrescrevendo um Modelo
#### Sobrescrevendo um Controller
#### Sobrescrevendo uma View

### Fonte de Estudo

1. http://guides.rubyonrails.org/
2. http://www.redmine.org/projects/redmine/wiki/Plugin_Tutorial
3. http://www.redmine.org/projects/redmine/wiki/Hooks
4. http://www.redmine.org/projects/redmine/wiki/Plugin_Internals

### Futuro
A ideia de desenvolver o curso no github é deixar ele colaborativo e expansível, assim como o Redmine. Gerando assim uma apostila completa sobre o assunto, que se mantenha sempre atualizada.

### Colaboradores
Quem contribuir com esse material, peço que mande um pull request adicionado o seu nome na lista abaixo.
- Victor Lima Campos(victorlcampos)
- Annanda Sousa (annanda)

### Alunos
Quem utilizar esse material para estudo, peço que mande um pull request adicionado o seu nome na lista abaixo.
- Victor Lima Campos (victorlcampos)
- Annanda Sousa (annanda)
- Julio Nascimento (juliocesarnrocha)
- Gabriel Rodrigues (gabrieldesar)
