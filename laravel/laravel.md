# Laravel

Framework de desenvolvimento rápido para PHP, permite um reaproveitamento de código muito bom usando técnicas MVC e outros helpers.

<!-- TOC -->

- [Laravel](#laravel)
  - [Modelo MVC](#modelo-mvc)
  - [Instalação](#instala%C3%A7%C3%A3o)
  - [Novo projeto](#novo-projeto)
    - [Configurações](#configura%C3%A7%C3%B5es)
    - [Estrutura de pastas](#estrutura-de-pastas)
    - [Artisan Serve](#artisan-serve)
      - [Artisan Name](#artisan-name)
    - [Rotas](#rotas)
  - [Controllers](#controllers)
  - [Conexões com banco de dados](#conex%C3%B5es-com-banco-de-dados)
    - [Conectando com o banco de dados](#conectando-com-o-banco-de-dados)
    - [Modo Debug](#modo-debug)
  - [Views](#views)
    - [Incluindo parâmetros com magic methods](#incluindo-par%C3%A2metros-com-magic-methods)
    - [Passando um array de dados para a view](#passando-um-array-de-dados-para-a-view)
    - [Métodos: exists e file](#m%C3%A9todos-exists-e-file)
    - [Incluindo CSS](#incluindo-css)
  - [QueryString](#querystring)
    - [Request](#request)
      - [Outros métodos da Request](#outros-m%C3%A9todos-da-request)
  - [Recursos](#recursos)
    - [Alguns cuidados necessários com recursos de rotas](#alguns-cuidados-necess%C3%A1rios-com-recursos-de-rotas)
  - [Blade](#blade)
    - [Templates](#templates)
    - [Echo](#echo)
    - [Loops](#loops)
    - [Ternários condicionais](#tern%C3%A1rios-condicionais)
    - [Condicionais completos](#condicionais-completos)
  - [Outros métodos HTTP](#outros-m%C3%A9todos-http)
    - [Buscando informações do formulário](#buscando-informa%C3%A7%C3%B5es-do-formul%C3%A1rio)
    - [Inserindo valores no DB](#inserindo-valores-no-db)
    - [Método POST](#m%C3%A9todo-post)
    - [O método match](#o-m%C3%A9todo-match)
  - [Delegação de resposta](#delega%C3%A7%C3%A3o-de-resposta)
      - [Escolhendo quais valores manter](#escolhendo-quais-valores-manter)
    - [Outros tipos de redirect](#outros-tipos-de-redirect)
    - [Linkando para ações](#linkando-para-a%C3%A7%C3%B5es)
    - [Outros tipos de resposta](#outros-tipos-de-resposta)

<!-- /TOC -->

## Modelo MVC

A grande ideia desse padrão arquitetural é que você separe suas regras de negócio em 3 camadas, cada uma com sua responsabilidade muito bem definida:

- __Model__ é a camada onde ficam nossas regras de negócio, nossas entidades e classes de acesso ao banco de dados.
- __View__ é a responsável por apresentar as páginas e outros tipos de resultado para o usuário (ou mesmo para outros sistemas, que se comunicam). É a resposta que o framework envia para o navegador, que normalmente é um HTML.
- __Controller__ é quem cuida de receber as requisições web e decidir o que fazer com elas. Nessa camada definimos quais modelos devem ser executados para determinada ação e para qual view vamos encaminhar a resposta. Em outras palavras, essa camada quem faz o link entre todas as outras.

![](http://s3.amazonaws.com/caelum-online-public/laravel/mvc-com-laravel.png)

Repare que, quando nosso cliente envia uma requisição pelo navegador, primeiramente temos um arquivo PHP, que é o routes.php, que está frente de todos. Ele cuida de atender as requisições e enviá-las para o local correto, no caso os nossos controllers.

Os controllers, por sua vez, decidem o que fazer com as requisições, passando pela camada de model (que fazem acesso ao banco, executam as regras de negócio etc.) e logo em seguida delegam pra ::view:: que será exibida como resposta no navegador do cliente.

## Instalação

Para instalar basta executar o comando `composer global require "laravel/installer"` (como está descrito no site oficial) e aguardar a instalação finalizar. 

Após esse processo o comando `laravel` deverá estar habilitado.

## Novo projeto

Para criar um novo projeto executamos `laravel new <projeto>`, isto vai criar uma estrutura de pastas dentro da nossa pasta atual com o nome do nosso projeto.

> Nesta nota vou estar usando a versão 5 do framework, para forçar a criação de um projeto usando o Laravel 5.0 use: `composer create-project laravel/laravel <nome> "5.0."`.

### Configurações

O framework faz todo o possível para que você não precise ficar configurando nada, mas em alguns momentos isso pode ser necessário. Se algo de que você precisa não está configurado por default, como por exemplo o locale, você pode fazer isso facilmente pelo arquivo de configurações presente em `<projeto>/config/app.php`.

### Estrutura de pastas

Esta é a estrutura de pastas do sistema (as mais importantes pelo menos):

- __app__: nela ficam seus modelos, views e controllers, que serão bem detalhados na próxima aula. Em poucas palavras, é onde boa parte do seu código vai ficar. Ela possui uma série de subdiretórios, como Commands, Console, Http, Events, entre outros. Não se preocupe em entender o significado de cada um deles agora, vamos vê-los melhor conforme formos precisando.
- __config__: como o nome já indica, é onde ficam os arquivos de configuração do seu projeto. Se você precisar alterar as configurações de cache, e-mail, banco de dados, entre outras, já sabe onde encontrar.
- __public__: é a pasta pra onde seu web server vai apontar. Lá fica o arquivo index.php, que aponta para sua aplicação. Além disso, é comum colocarmos os arquivos css, imagens, javascript e todos os demais arquivos públicos nesse diretório.
- __vendor__: é onde fica o source code do Laravel, plugins e outras dependências. Tudo que você usar de terceiros (bibliotecas, frameworks etc.) deve ficar nela.

O resto da estrutura está [aqui](http://laravel.com/docs/5.0/structure).

### Artisan Serve

Pra subir o server e testar, usamos o `php artisan serve`. O Artisan é uma ferramenta de linha de comando já inclusa no framework. Ela nos oferece uma série de comandos úteis para tornar nosso desenvolvimento mais produtivo.

O comando `serve` pode ser executado com `php artisan serve` e vai criar um servidor local na porta 8000 para rodar a aplicação.

#### Artisan Name

O namespace padrão de toda aplicação com Laravel é App, mas é muito comum e bastante recomendado que você altere o namespace para o nome da sua aplicação. Como fazer isso? É muito fácil, basta rodar um simples comando e pronto.

Para mudar o namespace, por exemplo, podemos usar o php artisan app:name. Vamos mudá-lo para o nome do projeto. Basta executar o seguinte comando pelo terminal, dentro da pasta de seu projeto:

`php artisan app:name <nome>`

### Rotas

O arquivo de rotas é o principal arquivo para onde o Laravel vai apontar as ações dos controllers.

Queremos ensinar o framework como queremos que ele reaja quando alguém acessar determinada URL, isto é, criar as nossas próprias rotas.

Quando acessamos `http://localhost:8000/`, ou seja, a URL `/` da nossa aplicação, em algum lugar foi configurado que a página padrão do Laravel deveria ser exibida, não é? Esse trabalho é feito no o arquivo routes.php. O arquivo de rotas se encontra em `routes/web.php` e contem algo assim:

> Na versão 5 o caminho é `app/Http/routes.php`

```php
<?php

/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/

Route::get('/', function () {
    return view('welcome');
});
```

O objeto `Route` é um provider que realiza o proxy para o direcionamento de rotas.

As rotas podem retornar uma view ou qualquer tipo de texto ou html válido como em:

```php
<?php

/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/

Route::get('/', function () {
    return '<h1>Produtos</h1>';
});
```

Em caso de ambiguidade sempre a última rota é quem será registrada. Há outras formas de lidar com ambiguidade, como quando usamos diferentes métodos HTTP, mas entraremos nesse assunto um pouco mais à frente.

## Controllers

Um controller é um gerenciador de ações e lógica da aplicação, por exemplo, um controller para mostrar todos os produtos é o método que vai listar tudo que existe no banco.

No lugar de definir todas as lógicas do nosso sistema nesse arquivo único, o `routes.php`, vamos organizá-las desde o início em Controllers distintos.

Vamos criar um novo arquivo chamado `ProdutoController`, dentro da pasta `app/Http/Controllers`, que é o diretório padrão para esse tipo de classe. Dentro do controller, crie um método chamado lista. O arquivo `ProdutoController.php` ficará assim.


```php
<?php

class ProdutoController {

}
```

Como nosso controller está em uma outra pasta, vamos ter que criar um novo namespace e mostrar aonde nosso controlador está.

```php
<?php

namespace <nome da app>\Http\Controllers;

class ProdutoController {

}
```

Todos os controles do Laravel __precisam__ herdar de uma classe padrão do Laravel chamado `Controller`

```php
<?php

namespace <nome da app>\Http\Controllers;

class ProdutoController extends Controller {

}
```

Criamos uma função aqui que retornaria o mesmo texto que temos nas rotas:

```php
<?php

namespace <nome da app>\Http\Controllers;

class ProdutoController extends Controller {
  public function lista() {
    return 'Um texto aqui';
  }
}
```

Depois precisamos editar o nosso arquivo de rotas para incluir nossa lógica do controller:

```php
<?php

Route::get('/', 'ProdutoController@lista');
```

Sempre utilizamos a lógica de nomenclatura `<nomeController>@<Método>`.

## Conexões com banco de dados

As Conexões com bancos de dados no Laravel são extremamente simples.

Primeiramente temos que mostrar ao framework qual é a base de dados que vamos utilizar e também qual é o tipo de cliente que estaremos utilizando.

Inicialmente vamos apenas mudar o arquivo `config/database.php`:

```php
    'mysql' => [
      'driver'    => 'mysql',
      'host'      => env('DB_HOST', 'localhost'),
      'database'  => env('DB_DATABASE', 'forge'),
      'username'  => env('DB_USERNAME', 'forge'),
      'password'  => env('DB_PASSWORD', ''),
      'charset'   => 'utf8',
      'collation' => 'utf8_unicode_ci',
      'prefix'    => '',
      'strict'    => false,
    ],
```

Vamos apenas editar os dados padrões:

```php
    'mysql' => [
      'driver'    => 'mysql',
      'host'      => env('DB_HOST', '<Host do banco>'),
      'database'  => env('DB_DATABASE', '<base de dados>'),
      'username'  => env('DB_USERNAME', '<username>'),
      'password'  => env('DB_PASSWORD', '<senha>'),
      'charset'   => 'utf8',
      'collation' => 'utf8_unicode_ci',
      'prefix'    => '',
      'strict'    => false,
    ],
```

### Conectando com o banco de dados

No nosso controller vamos editar uma linha para podermos realizar uma nova consulta no banco:

```php
<?php

namespace <nome da app>\Http\Controllers;

use Illuminate\Support\Facades\DB;

class ProdutoController extends Controller {
  public function lista() {

    $produtos = DB::select('<query>');
    return 'Um texto aqui';
  }
}
```

A classe `DB` é um helper para conexões de banco de dados. Note que temos que importar o namespace da classe para que possamos usá-la no projeto.

> Para exibir, podemos utilizar a instrução `dd($produtos)` que é um **D**ata **D**ump dos produtos, o equivalente a um `print_r` mais bonito.

Para acessarmos os objetos do banco de dados, podemos usar a notação de objetos:

```php
<?php

foreach $produtos as $p {
  echo $p->coluna;
}
```

### Modo Debug

Quando o Laravel encontra um erro, uma tela genérica é apresentada, porém precisamos de mais descrições deste erro. Para isso, basta editarmos o arquivo `.env` que é o responsável pela definição das variáveis de ambiente.

Inicialmente ele estará assim:

```env
APP_ENV=local
APP_DEBUG=true
APP_KEY=GEoF8Hb5gBZbDVbSIZhahB1tmBo5Gwa6

DB_HOST=localhost
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret

CACHE_DRIVER=file
SESSION_DRIVER=file
```

Vamos remover tudo deixando apenas:

```
APP_DEBUG=true
```

## Views

As views de exibição para o usuário ficam dentro da pasta `resources/views`. Estas views podem ser criadas usando HTML normal, ou então o template engine chamado _blade_.

Com o html + php podemos utilizar algo do tipo:

```php
<table>
  <?php foreach($produtos as $p): ?>
  <tr>
    <td> <?= $p->nome ?> </td>
  </tr>
  <?php endforeach ?>
</table>
```

Agora que temos nossas views, vamos avisar o Controller que não queremos mais printar o HTML, mas sim a view que criamos:

```php
class ProdutoController extends Controller {
  public function lista() {

    $produtos = DB::select('<query>');
    return view('<nome da view>');
  }
}
```

Mas vamos ter um problema quando precisarmos renderizar isso, porque a variável `$produto` não existe na view, então vamos enviar esse dado como parâmetro, mudando a linha `return view('<nome da view>');` para `return view('<nome da view>')->with('<nome da variável na view>, $produtos);`.

### Incluindo parâmetros com magic methods

Uma curiosidade é que, no lugar de escrever:

```php
view('listagem')->with('produtos', $produtos);
```

Você pode chamar um método `withProdutos`:

```php
view('listagem')->withProdutos($produtos);
```

O nome da variável na view seria como ela seria chamada quando for enviada para a mesma, e o segundo parametro seria o conteudo.

### Passando um array de dados para a view

Existem diversas outras formas de passar os dados para a view, além do método with. Uma das mais conhecidas e utilizadas é passando um array como segundo parâmetro do método view. Em vez de fazer:

`view('listagem')->with('produtos', $produtos);`

Você faria algo como:

`return view('listagem', ['produtos' => $produtos]);`
Ou extraindo o array para uma variável:

```php
$data = ['produtos' => $produtos];
return view('listagem', $data);
```

Ou ainda criando o array e adicionando cada item manualmente:

```php
$data = [];
$data['produtos'] = $produtos;
return view('listagem', $data);
```

### Métodos: exists e file

Outro ponto é que você pode verificar a existência de uma view com o método exists:

```php
if (view()->exists('listagem'))
{
    return view('listagem');
}
```

Ou mesmo usar o método file para gerar a view a partir de um caminho/diretório diferente:

`view()->file('/caminho/para/sua/view');`

### Incluindo CSS

Para incluirmos o CSS podemos apenas incluir na tag `link` o caminho `/css/app.css`.

> O Laravel por padrão já vem com o Bootstrap instalado internamente no arquivo app.css

## QueryString

Quando quisermos exibir apenas um produto, por exemplo, exibir o produto. Primeiro vamos criar um novo método no nosso controller de produtos que vai chamar uma nova view que exibirá apenas um produto

```php
class ProdutoController extends Controller {
  public function detalhe() {

    $produtos = DB::select('<query> WHERE <filtro>');
    return view('<nome da view>')->with('p', $produto[0]);
  }
}
```

Inclusive podemos enviar um parâmetro no where, para que possamos passá-lo mais facilmente:

```php
$produtos = DB::select('<query> WHERE <campo> = ?', [<campoValor>]);
```

Para cada `?` adicionado uma nova posição do array pode ser criada.

E na view:

```php
<h1>Detalhes do produto <?= $p->nome ?></h1>
. . .
```

Mas e como fazemos para buscar o QueryString?

### Request

Estamos setando um valor fixo no filtro da query, para podermos mandar esse valor dinamicamente, podemos simplesmente alterar nosso HTML na view para que ele atualize com o ID do produto:

```php
<table>
  <tr>
    <td> <?= $p->nome ?> </td>
    <td> <a href="/produto?id="<?= $p->id ?>>
        Visualizar
      </a>
    </td>
  </tr>
</table>
```

E então precisamos pegar o query string da URL, para isso vamos editar nosso controller para que possamos pegar essa variável.

Em nossa função, vamos editar o método que mostra o detalhe do produto:

```php
use Request;

class ProdutoController extends Controller {
  public function detalhe() {

    $id = Request::input('id');
    $produtos = DB::select('<query> WHERE <filtro> = ?', [$id]);
    return view('<nome da view>')->with('p', $produto[0]);
  }
}
```

Note que temos que utilizar o `use` para importarmos o pacote do namespace. Bem como utilizar o método `input` da classe para podermos utiliza-lo como parâmetro.

> De forma mais simples podemos criar um valor padrão para o input utilizando `Request::input('nome', 'default')`

#### Outros métodos da Request

a Request tem uma variedade bem grande de métodos que nos ajudam em trabalhos como esse. Por exemplo, quer saber se existe um parâmetro específico na requisição? Que tal fazer:

```php
<?php
if (Request::has('id'))
{
  // faz alguma coisa
}
```

Há ainda o método all, que retorna um array com todos os parâmetros da requisição, os métodos only e except, com que você pode restringir quais parâmetros quer listar.

```php
<?php
// lista todos os params
$input = Request::all();

// apenas nome e id
$input = Request::only('nome', 'id');

// todos os params, menos o id
$input = Request::except('id');
```

Há também métodos como url, que retorna a URL da request atual, ou o path que retorna a URI. Por exemplo, em uma requisição para o método mostra, ao fazer:

```php
<?php
$url = Request::url();
O valor da $url seria http://localhost:8000/produtos/mostra, mas já no caso do path:

$uri = Request::path();
```

E toda a informação pode ser encontrada na [documentação](https://laravel.com/docs/5.3/requests).

## Recursos

Da mesma forma da query string, podemos acessar um recurso como `url/produto/id`, para ficar um pouco mais semantico.

Para isso funcionar vamos precisar editar nosso arquivo de rotas para adicionar um novo parâmetro depois do recurso.

```php
<?php
Route::get('/produto/{id}', 'ProdutoController@detalhe');
```

Agora precisamos trocar o input do request no nosso controller:

```php
use Request;

class ProdutoController extends Controller {
  public function detalhe() {

    $id = Request::route('id');
    $produtos = DB::select('<query> WHERE <filtro> = ?', [$id]);
    return view('<nome da view>')->with('p', $produto[0]);
  }
}
```

> Note que mudamos de `input` para `route`.

E quanto ao valor default? Quando estávamos usando o método input, nosso código estava assim:

```php
$id = Request::input('id', '0');
```

Por que não fizemos o mesmo com o route? A resposta é simples: o `{id}` da rota agora é obrigatório! Se não passar, não entra no método e ponto-final. Se você quiser que o id do final da url seja opcional, você precisará deixar isso explícito no momento de registrar sua rota.

```php
Route::get('/produtos/mostra/{id?}', 'ProdutoController@mostra');
```

Repare que há uma `?` após o id indicando que ele é opcional.

Isso não é tudo, o Laravel nos oferece uma forma ainda mais interessante de recuperar parâmetros da URL. Basta adicionar um argumento com o mesmo nome do parâmetro na assinatura do seu método, ele vai ser populado. Veja como fica o método do controller:

```php
public function mostra($id){

    $resposta = DB::select('select * from produtos where id = ?', [$id]);

    if(empty($resposta)) {
        return "Esse produto não existe";
    }
    return view('detalhes')->with('p', $resposta[0]);
}
```

Note que em nenhum momento precisamos usar a `Request`, o framework faz esse trabalho por nós.

### Alguns cuidados necessários com recursos de rotas

Quando estamos usando parâmetros na URL, sempre precisamos nos atentar a alguns detalhes. Por exemplo, em algum momento falamos que o id precisaria ser um número? Não. Isso significa que eu consigo acessar o método pela seguinte URL:

- http://localhost:8000/produto/teste

Claramente isso não deveria acontecer. Por nossa sorte, a única consequência aqui será ver a mensagem de que o produto não foi encontrado. Mas existem problemas piores, como ambiguidade entre rotas.

Em nosso caso, isso seria um pouco difícil, mas imagine que para simplificar optássemos por remover o `/produto` de nossa URL. Isso causaria sérios problemas, pois as seguintes URLs seriam equivalentes:

- http://localhost:8000/1
- http://localhost:8000/adiciona

Já que o `{id}` pode ser qualquer coisa, inclusive um texto, ele pode ser a palavra adiciona. Em outras palavras, dependendo da ordem em que eu registrar as rotas, pode acontecer de eu acessar `/produto/adiciona` e a aplicação me responder que esse produto não existe.

Nesse caso, precisaríamos de alguma forma ensinar ao Laravel que o `{id}` da rota sempre será um número. Isso pode ser feito com auxilio do método `where`, como no exemplo:

```php
Route::get(
  '/produtos/mostra/{id}', 
  'ProdutoController@mostra'
  )
  ->where('id', '[0-9]+');
```

Observe que estamos passando para o método where o nome do parâmetro e uma expressão regular com o _pattern_ que pode ser seguido.

## Blade

Blade é um template Engine que é implementado no Laravel assim como o JSX ou o EJS é implementado no Node.

Todo arquivo blade __precisa__ ter uma extensão `.blade.php`

### Templates

Para criarmos um template que será seguido por todas as páginas, podemos pegar apenas as partes repetidas das nossas views (como o cabeçalho e o rodapé) e criar um novo arquivo html na pasta de views.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Titulo</title>
</head>
<body>
<conteudo>
</body>
</html>
```

Repare que o conteúdo é apenas o miolo de nosso HTML. Vamos indicar que é nesta parte que vamos colocar o conteúdo do nosso site:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Titulo</title>
</head>
<body>
  @yield('conteudo')
</body>
</html>
```

> Podemos usar qualquer nome ao invés de 'conteudo'

Agora em nossas páginas reais (aonde está o conteúdo), vamos colocar uma marcação indicando que o arquivo estende um template, e também que ele é o conteúdo de fato

```html
@extends('<nome do template>')

@section('conteudo')
<table>
  <tr>
    <td>1</td>
    <td>2</td>
    <td>3</td>
  </tr>
</table>
@stop
```

> O nome do template é o nome que demos a view que criamos anteriormente.

Veja que o `@section` define um pedaço de código, um include que poderá ser reutilizado em outros arquivos, mas precisamos colocar o mesmo nome da variável do `@yield` no template.

> Não se esqueça de mudar os nomes das extensões das views para `.blade.php`

### Echo

Utilizamos normalmente as tags `<?= ?>` para printar os resultados das variáveis na tela em um html, no blade podemos apenas circundar com duas chaves:

```php
<table>
  <tr>
    <td> {{$p->nome}} </td>
    <td> <a href="/produto/{{$p->id}}">
        Visualizar
      </a>
    </td>
  </tr>
</table>
```

> Podemos definir valores padrões para variáveis usando o `or` (`{{$p->id or Sem valor}}`)

### Loops

Podemos substituir os loops do foreach por algo mais simples:

```php
<table>
  @foreach($produtos as $p)
  <tr>
    <td> {{ $p->nome }} </td>
  </tr>
  @endforeach
</table>
```

Além do foreach, também podemos fazer o mesmo com o for tradicional:

```php
@for ($i = 0; $i < 10; $i++)
    O indice atual é {{ $i }}
@endfor
```

Ou ainda com um while:

```php
@while (true)
    Entrando em looping infinito!
@endwhile
```

Ha ainda uma variação bastante interessante, chamada forelse. Se a lista for vazia, ele executa o código do bloco marcado com @empty:

```php
@forelse($produtos as $p)
    <li>{{ $p->nome }}</li>
@empty
    <p>Não tem nenhum produto!</p>
@endforelse
```

> Podemos usar tanto `{{ $var }}` como `{{$var}}`

### Ternários condicionais

Podemos utilizar ternários para operar sobre variáveis e devolver valores que serão printados no HTML, como, por exemplo, pintar um item se ele estiver fora de estoque:

```html
<table>
  @foreach($produtos as $p)
  <tr class='{{ $p->quantidade <= 1 ? "danger" : "" }}'>
    <td> {{ $p->nome }} </td>
  </tr>
  @endforeach
</table>
```

Neste exemplo caso a quantidade em estoque seja menor ou igual a 1 terá a classe danger.

### Condicionais completos

Podemos fazer um if verificando se a lista de produtos está vazia, e se estiver, mostrar uma mensagem de que não existe nenhum produto cadastrado. Em PHP puro, o código ficaria assim:

```php
<?php if(empty($produtos)) { ?>

<div class="alert alert-danger">
  Você não tem nenhum produto cadastrado.
</div>

<?php } else { ?>

  <h1>Listagem de produtos</h1>
  <table>

  <? foreach ($produtos as $p) { ?>
?>
<!-- continuação do código -->

<?php } ?> <!-- fechando o foreach -->
<?php } ?> <!-- fechando o else -->
```

Com Blade:

```html
@if(empty($produtos))

<div class="alert alert-danger">
  Você não tem nenhum produto cadastrado.
</div>

@else

  <h1>Listagem de produtos</h1>
  <table>

  @foreach ($produtos as $p)
  <!-- continuação do código -->
  @endforeach

@endif
```

A ausência das chaves e tags do PHP ajuda bastante, nosso código fica mais simples e legível. O arquivo listagem.blade.php completo pode ficar assim:

```html
@extends('principal')

@section('conteudo')

 @if(empty($produtos))
  <div>Você não tem nenhum produto cadastrado.</div>

 @else
  <h1>Listagem de produtos</h1>
  <table class="table table-striped table-bordered table-hover">
    @foreach ($produtos as $p)
    <tr>
      <td> {{$p->nome}} </td>
      <td> {{$p->valor}} </td>
      <td> {{$p->descricao}} </td>
      <td> {{$p->quantidade}} </td>
      <td>
        <a href="/produtos/mostra/{{$p->id}}>">
          <span class="glyphicon glyphicon-search"></span>
        </a>
      </td>
    </tr>
    @endforeach
  </table>

  @endif
@stop
```

Além do `@if` e `@else`, você também pode usar `@elseif` ou mesmo o `@unless`, que faz a condição inversa. Por exemplo, no caso a seguir, o valor condicionado sempre será exibido:

```html
    @unless (1 == 2)
      Esse texto sempre será exibido! 
    @endunless
```

## Outros métodos HTTP

Podemos implementar outras rotas além do `get`. Primeiramente vamos criar a rota que vai nos direcionar para o formulário que iremos preencher:

```php
<?php
Route::get('/produto/novo', 'ProdutoController@novo');
```

E no controler vamos criar nosso método novo:

```php
<?
public function novo() {
  return view('formulario');
}
```

Feito isso, vamos criar nossa view de formulário, utilizando o blade podemos criar nosso formulário do modo que queremos. Chamaremos essa view de `formulario.blade.php` em respeito a nosso nome dado para o controle, nesta view teremos apenas um formulário simples.

### Buscando informações do formulário

Para adicionar um novo produto vamos criar uma nova rota para esta ação:

```php
<?php
Route::get('/produto/adiciona', 'ProdutoController@adiciona');
```

E no controler vamos criar nosso método adiciona:

```php
<?
public function adiciona() {
  return view('adiciona');
}
```

Temos duas formas de adicionar uma variável de um formulário, um deles é setar o método do `<form>` como `GET` e dar um nome para cada campo, depois podemos pegar esses valores no método `adiciona` com o código `Request::input('campo');`.

### Inserindo valores no DB

Da mesma forma como usamos `DB::select` vamos usar `DB::insert('insert into <tabela> (campos) values (?,?,?)', [1,2,3])` para podermos inserir o valor dentro do banco de dados.

### Método POST

Utilizando o método POST do formulário, podemos esconder os valores na requisição.

Um dos problemas que isso gera é o tratamento do CSRF, que evita que requests não permitidas sejam feitas para o app. Para podermos tratar esse tipo de requisição, vamos adicionar o token CSRF que o próprio framework gera através de um input simples:

```html
<input type="hidden" name="_token" value=" {{ csrf_token() }}"/>
```

Dentro do nosso formulário, atente-se que o nome do token __deve__ ser `_token`.

Após isso vamos ter que adicionar uma modificação nas nossas rotas para que ela identifique o método POST.

```php
<?php
Route::post('/produto/adiciona', 'ProdutoController@adiciona');
```

Assim podemos buscar as informações ainda por `input` da request.

### O método match

Além de get, post e any, a interface Route ainda nos oferece um método chamado match, que nos permite passar um array especificando exatamente quais métodos HTTP devem ser aceitos. Um exemplo seria:

```php
  Route::match(array('GET', 'POST'),
    '/produtos/adiciona',
    'ProdutoController@adiciona');
```

Neste caso, requisições de tipo GET ou POST seriam aceitas.

## Delegação de resposta

Podemos redirecionar os métodos para outras rotas inicialmente, ou seja, para que elas ajam como se fossem chamadas pelo browser, podemos utilizar simplesmente o método `Redirect`, utilizado da seguinte maneira:

```php
return redirect('/rota');
```

Porém isso vai zerar todos os nossos dados da requisição anterior, ou seja, perderemos a capacidade de exibir uma mensagem na listagem de produtos, informando que o produto foi cadastrado com sucesso. Para que isso seja possível vamos usar o método `withInput` do Laravel:

```php
return redirect('/rota')->withInput();
```

Mas isso sozinho não é suficiente, precisamos também, na nossa view de listagem, verificar se essa variável (da requisição anterior) existe e, se existir, mostrar a mensagem de informação cadastrada com sucesso. Para isto vamos usar um if com o método `old()`:

```html
@if(old('nome do input'))
  <h1>O produto {{ old('nome do input') }} foi adicionado com sucesso</h1>
@endif
```

#### Escolhendo quais valores manter

O problema de usar o withInput dessa forma é que todos os parâmetros são mantidos, mas isso é desnecessário, já que estamos usando apenas o nome. Para evitar que isso aconteça, você pode definir explicitamente quais parâmetros devem ser mantidos:

```php
return redirect('/produtos')
    ->withInput(Request::only('nome'));
```

O mesmo vale para os outros métodos da Request, como por exemplo o except. No caso a seguir, todos os parâmetros exceto a senha serão mantidos na próxima requisição.

```php
return redirect('/usuarios')
    ->withInput(Request::except('senha'));
```

### Outros tipos de redirect

Além de como fizemos, existem diversas possibilidades interessantes para o método redirect. Uma delas é redirecionar para uma ação do controller e não uma URI.

Redirecionando para uma action
Quer ver como é simples? Como o método adiciona quer redirecionar para o método lista, da classe `ProdutoController`, em vez de usar o `redirect('/produtos')`, ou seja, com a URI, ele pode fazer:

```php
return redirect()
    ->action('ProdutoController@lista')
    ->withInput(Request::only('nome'));
```

Assim ele está dizendo explicitamente qual o método que quer redirecionar, independente de sua URI. Se decidirmos no futuro mudar a rota desse método, precisaríamos lembrar de mudar todos os lugares que faziam redirect. Já quando fazemos dessa forma, apontando para uma action específica, tudo continua funcionado.

Claro, se alguém mudar o nome do método o redirect também vai parar de funcionar, mas esse tipo de mudança acontece com uma frequência bem menor do que a mudança de URI.

Mas oferecer essa possibilidade apenas para o redirect não ajudaria muito, pois, todos os links quebrariam se uma rota fosse alterada.

É justo, afinal, ele vai continuar apontando para a URI anterior já que estamos deixando isso fixo em nosso HTML. Repare no `navbar` do layout principal.blade.php:

```html
<ul class="nav navbar-nav navbar-right">
  <li><a href="/produtos">Listagem</a></li>
  <li><a href="/produtos/novo">Novo</a></li>
</ul>
```

### Linkando para ações

Em vez de linkar para uma URI, é sempre mais interessante linkar para uma ação do controller, assim como fizemos no `redirect`. A mudança será simples, basta chamar o método auxiliar action de dentro das chaves duplas do blade. Os links devem ficar assim:

```html
<ul class="nav navbar-nav navbar-right">
  <li>
      <a href="{{action('ProdutoController@lista')}}">
          Listagem
      </a>
  </li>
  <li>
      <a href="{{action('ProdutoController@novo')}}">
          Novo
      </a>
  </li>
</ul>
```

###  Outros tipos de resposta

Além de retornar uma view, ou redirecionar para outra lógica, existem momentos em que queremos enviar outros tipos de resposta, por exemplo quando estamos trabalhando com serviços ou comunicação entre sistemas.

Um formato bem comum e muito utilizado atualmente é o JSON, e por esse motivo ele é o formato padrão de resposta do Laravel. Você em algum momento já experimentou retornar um objeto ou variável no lugar de uma view? Se não fez, com certeza vai gostar de testar.

Podemos criar um novo método no `ProdutoController` chamado `listaJson`, que pode fazer o seguinte:

```php
public function listaJson(){
    $produtos = DB::select('select * from produtos');
    return $produtos;
}
```

Não se esqueça de que a cada novo método uma rota deve ser criada no routes.php, para que ele fique acessível pelo navegador.

```php
Route::get('/produtos/json', 'ProdutoController@listaJson');
```

Agora que já temos o método e ele está devidamente mapeado, podemos acessar sua URL pelo navegador:

O resultado foram todos os produtos _serializados_ em formato json. Mas também podemos fazer isso explicitamente. Em vez de retornar a lista de produtos, poderíamos fazer:

```php
public function listaJson(){
    $produtos = DB::select('select * from produtos');
    return response()->json($produtos);
}
```

Executando no navegador, o resultado será o mesmo.

No caso de o JSON retornar o objeto ou lista de objetos diretamente seria muito mais simples, mas além do método json existem diversos outros que podem ser muito úteis em nosso dia a dia. Um exemplo seria:

```php
return response()
    ->download($caminhoParaUmArquivo);
```

Acessar um método com esse retorno resultaria no download do arquivo presente no caminho especificado.