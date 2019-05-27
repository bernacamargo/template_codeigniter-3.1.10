# Template Codeigniter 3.1.10

Esse é um template com otimizações voltadas para a reutilização de código.

## Instalação
Faça o download como ZIP ou clone o repósitorio em seu ambiente local, em seguida basta realizar a configuração da aplicação e começar a usa-la.

## Configuração
### Config.php
Configure o base url em application/config/config.php

- Troque 'yourapp' pelo nome do seu aplicativo local

```php
$config['base_url'] = 'http://localhost/yourapp'
```

### Conectando ao Banco de Dados

Credenciais do banco configuradas em `application/config/database.php`

- Troque os valores `YOURHOST`, `DB_USERNAME`, `DB_PASSWORD` e `DB_NAME` pelo host, username, senha e nome do banco de dados respectivamente

```php
$db['default'] = array(
    'dsn'   => '',
    'hostname' => 'YOURHOST',
    'username' => 'DB_USERNAME',
    'password' => 'DB_PASSWORD',
    'database' => 'DB_NAME',
    'dbdriver' => 'mysqli',
    'dbprefix' => '',
    'pconnect' => FALSE,
    'db_debug' => (ENVIRONMENT !== 'production'),
    'cache_on' => FALSE,
    'cachedir' => '',
    'char_set' => 'utf8',
    'dbcollat' => 'utf8_general_ci',
    'swap_pre' => '',
    'encrypt' => FALSE,
    'compress' => FALSE,
    'stricton' => FALSE,
    'failover' => array(),
    'save_queries' => TRUE
);
```

### Assets

Configure a source de todos os import `.js` e `.css` em `application/config/assets.php`

- Defina a ordem, da esquerda para a direita, em que os assets serão carregados. Note que os arquivos `functions.js` e `style.css` devem ser carregados por último

```php
$config['default'] = ['bootstrap', 'vendors', 'pnotify', 'custom'];
```

- Defina o caminho dos plugins a serem utilizados sempre respeitando a estrutura dos arrays

```php
// Bootstrap 4.3.1
$config['bootstrap'] = [
    'css' => [ 
        'https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css',
        site_url('assets/vendors/bootstrap-datepicker-1.9.0-dist/css/bootstrap-datepicker.min.css')
    ],
    'js'  => [
        'https://unpkg.com/popper.js/dist/umd/popper.min.js',
        'https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js',
        site_url('assets/vendors/bootstrap-datepicker-1.9.0-dist/js/bootstrap-datepicker.min.js')
    ]
];

// Plugins localizados na pasta assets/vendors
$config['vendors'] = [
    'css' => [

    ],

    'js' => [

    ]
];


// Notificação carregada por default
$config['pnotify'] = [
    'css'   =>  [
        site_url('assets/vendors/pnotify/dist/pnotify.css'), 
        site_url('assets/vendors/pnotify/dist/pnotify.buttons.css'),         
        site_url('assets/vendors/pnotify/dist/pnotify.nonblock.css'),         
        site_url('assets/vendors/pnotify/dist/pnotify.material.css'),         
        site_url('assets/vendors/pnotify/dist/pnotify.mobile.css'),         
        site_url('assets/vendors/pnotify/dist/pnotify.brighttheme.css'),         
        site_url('assets/vendors/pnotify/dist/pnotify.history.css'),         

    ],
    'js'    => [
        site_url('assets/vendors/pnotify/dist/pnotify.js'),         
        site_url('assets/vendors/pnotify/dist/pnotify.buttons.js'),         
        site_url('assets/vendors/pnotify/dist/pnotify.nonblock.js'),         
        site_url('assets/vendors/pnotify/dist/pnotify.animate.js'),         
        site_url('assets/vendors/pnotify/dist/pnotify.callbacks.js'),         
        site_url('assets/vendors/pnotify/dist/pnotify.confirm.js'),         
        site_url('assets/vendors/pnotify/dist/pnotify.history.js'),         
        site_url('assets/vendors/pnotify/dist/pnotify.mobile.js'),         
        site_url('assets/vendors/pnotify/dist/pnotify.reference.js'),         
        site_url('assets/vendors/pnotify/dist/pnotify.tooltip.js'),         

    ]
];

// Ultimo módule a ser carregado
$config['custom'] = [
    'css' => [  
        site_url('assets/css/style.css'),
    ],
    'js' => [ 
        site_url('assets/js/functions.js'),         
    ]
];
```

## Utilitários
### Librarie Template

```php
class Template {

    // instancia do codeigniter
    public $ci;

    // arquivos de css para carregar na página
    public $css;

    // arquivos de js para carregar na página
    public $js;

    // as views que serão carregadas
    public $views = array();

    // modulos para carregar
    public $modules = array();

    // adiciona uma variavel a ser carregada na view
    public $data = array();

    // pagina a ser carregada
    public $p_page = 'home';

    // guard
    public $guard;

    // titulo da pagina
    public $title = '';

    // método construtor
    public function __construct() {

        // pega a instancia do ci
        $this->ci =& get_instance();

        // pega a biblioteca de configuração
        $this->ci->config->load( 'assets' );

        // pega a biblioteca de guard
        $this->ci->load->library( 'Guard' );
        $this->guard = $this->ci->guard;

        // carrega os módulos padrão
        $this->loadDefault();
    }
```

#### Métodos

- set_title
```php
    /**
     * set_title
     *
     * Define o titulo da página html
     * 
     * @param String $title [Titulo da página]
     * @return void
     */
    public function set_title( $title ) {
        $this->title = $title;
    }
```

- print_title
```php
    /**
     * print_title
     *
     * Exibe o titulo atual
     * 
     * @return void
     */
    public function print_title() {
        echo $this->title;
    }
```

- loadDefault
```php
    /**
     * loadDefault
     *
     * carrega os módulos padrão
     * 
     * @return void
     */    
    public function loadDefault() {

        // pega os módulos setados no arquivo de configuracao
        $default = $this->ci->config->item('default');

        // junta com o que já tem guardado
        $this->modules = array_merge( $this->modules, $default );
    }
```

- use_module
```php
    /**
     * use_module
     *
     * adiciona um novo modulo a ser carregado
     * 
     * @return void
     */        
    public function use_module( $module ) {

        // adiciona o módulo
        $this->modules[] = $module;
    }
```

- addCss
```php
    /**
     * addCss
     *
     * adiciona o css
     *
     * @param String $css [SRC to a css file]     
     * @return void
     */            
    public function addCss( $css ) {
        $this->css[] = $css;
    }
```
- addJs
```php
    /**
     * addJs
     *
     * adiciona o css
     *
     * @param String $js [SRC to a js file]
     * @return void
     */            
    public function addJs( $js ) {
        $this->js[] = $js;
    }
```
- view
```php
    /**
     * view
     *
     * adiciona uma nova view
     * 
     * @param  String
     * @param  String
     * @return void
     */
    public function view( $chave, $view ) {
        $this->view[$chave] = $view;
    }
```
- set
```php
    /**
     * set
     *
     * seta o valor para uma variavel
     * 
     * @param String $chave
     * @param String $valor
     * @return void
     */
    public function set( $chave, $valor ) {
        $this->data[$chave] = $valor;
    }
```

- item
```php
    /**
     * item
     *
     * pega o valor de uma varivel
     * 
     * @param  String
     * @return mixed [Retorna o objeto do array indicado pela $chave ou null caso não exista]
     */
    public function item( $chave ) {
        return ( isset( $this->data[$chave] ) ) ? $this->data[$chave] : null;
    }
```

- print_view
```php
    /**
     * print_view
     *
     * Carrega uma view através do nome do seu arquivo
     * 
     * @param  String $view
     * @return void
     */
    public function print_view( $view ) {
        $this->ci->load->view( $view );
    } 
```

- page
```php
    /**
     * page
     *
     * seta a pagina a ser carregada
     * 
     * @param  String
     * @return void
     */
    public function page( $page ) {
        $this->p_page = $page;
    }
```

- print_component
```php
    /**
     * page
     *
     * carrega um componente
     * 
     * @param  String
     * @param Array $var [Array de dados a serem enviados para a view]
     * @return void
     */
    public function print_component( $component , $var = false) {
        
        // carrega a pagina
        $this->ci->load->view( 'components/'.$component, $var);
    }
```

- print_page
```php
    /**
     * print_page
     *
     * Carrega uma view salva em views/pages/[view].php
     * 
     * @param  String
     * @return void
     */
    public function print_page( $page = false ){

        // verifica se o usuário deseja carregar uma pagina em especifico
        $this->p_page = $page ? $page : $this->p_page;

        // carrega a pagina
        $this->ci->load->view( 'pages/'.$this->p_page );
    }
```

- loadModules
```php
    /**
     * loadModules
     *
     * Carrega os modulos
     * 
     * @return void
     */
    public function loadModules(){

        // pega os modulos
        $modules = array_unique( $this->modules );

        // percorre todos os modulos
        foreach( $modules as $module ) {

            // carrega os arquivos de configuração
            $config = $this->ci->config->item( $module );

            // verifica se existem css
            if ( isset( $config['css'] ) ) {
                foreach ( $config['css'] as $css ) {
                    $this->addCss( $css );
                }
            }

            // verifica se existem js
            if ( isset( $config['js'] ) ) {
                foreach ( $config['js'] as $js ) {
                    $this->addJs( $js );
                }
            }
        }
    }
```

- print_js
```php
    /**
     * print_js
     *
     * Imprime o js
     * 
     * @return void
     */
    public function print_js() {
        foreach( $this->js as $js ) echo '<script src="'.$js.'?version='.time().'" type="text/javascript"></script>';
    }
```

- print_css
```php
    /**
     * print_css
     *
     * Imprime o css
     * 
     * @return void
     */
    public function print_css() {
        foreach( $this->css as $css ) echo '<link href="'.$css.'?version='.time().'" rel="stylesheet" media="screen"></script>';
    }
```

- render
```php
    /**
     * render
     *
     * Renderiza a página escolhida em um layout escolhido (master.php)
     * 
     * @param  String $layout
     * @param  String $page
     * @return void
     */
    public function render( $layout = false, $page = false ) {

        // carrega os modulos
        $this->loadModules();

        // verifica se o usuário deseja carregar uma pagina em especifico
        $this->p_page = $page ? $page : $this->p_page;

        // carrega a view
        $this->ci->load->view( $layout, [ 'template' => $this ] );
    }
```



### Librarie Guard

Essa biblioteca tem como função facilitar a manipulação da variável de sessão do usuário.

```php
class Guard {

    // instancia do codeigniter
    public $ci;

    // dados do usuario logado
    public $user = false;

    // método construtor
    public function __construct() {

        // pega a instancia do ci
        $this->ci =& get_instance();

        // carrega a librarie de sessao
        $this->ci->load->library( 'session' );

        // pega os dados do usuario
        if ( $user = $this->ci->session->userdata( 'user' ) ) {
            $this->user = $user;
        }
    }
```

#### Métodos

- logged

```php
    /**
     * logged
     *
     * Verifica se o usuário está logado ou não
     * 
     * @return Boolean
     */
    public function logged(){
        return $this->user ? true : false;
    }
```
Exemplo:
  ```php
  if($this->guard->logged()){
    echo 'Usuário logado';
  }
  else{
    echo 'Usuário deslogado';
  }
  ```

- item

```php
    /**
     * item
     *
     * Retorna um item do array $user salvo na sessão
     * 
     * @param  String $key [Campo do array $user]
     * @return Object or NULL
     */
    public function item( $key ) {
        return isset( $this->user[$key] ) ? $this->user[$key] : null;
    }
```

Exemplo:
  ```php
$nome = $this->guard->item('nome');
echo $nome;
// Bernardo Pinheiro Camargo
```

- login

```php
    /**
     * login
     *
     * Tenta fazer o login do usuario através de um e-mail e senha inseridos
     * 
     * @param  String $email [E-mail para logar]
     * @param  String $senha [Senha para logar]
     * @return Boolean
     */
    public function login( $email, $senha ) {

        // carrega a model de usuários
        $this->ci->load->model( 'Usuarios_model' );

        // faz o login
        if ( $user = $this->ci->Usuarios_model->validate( $email, $senha ) ) {
            
            // guarda na sessao
            $this->ci->session->set_userdata( 'user', $user );            
            
            // guarda no atributo
            $this->user = $user;

            return true;
        } else return false;
    }
```

Exemplo:

```php
$email = 'bernardopcamargo@gmail.com';
$senha = '123456';

if($this->guard->login($email, $senha)){
    echo 'Bem-vindo!';
}
else{
    echo 'Credenciais inválidas';
}
```

- update

```php
    /**
     * update
     *
     * Busca o usuario no banco de dados e atualiza a sessão com os dados 'novos'
     * 
     * @return Boolean
     */
    public function update() {

        // verifica se existe um usuário logado
        if ( !$this->user ) return false;

        // carrega a model de usuários
        $this->ci->load->model( 'Usuarios_model' );

        // pega os dados do perfil do usuario logado
        if ( $user = $this->ci->Usuarios_model->getById( $this->user['id_usuario'] ) ) {

            // seta a sessao
            $this->ci->session->set_userdata( 'user', $user );

            // seta o usuario
            $this->user = $user;

            return true;

        } else return false;
    }
```

Exemplo:

```php
echo $this->guard->item('nome'); // Bernardo Pinheiro Camargo

$dados = ['id' => $id_usuario, 'nome' => 'Bernardo'];  //Seta os dados para o update

$this->Usuarios_model->update($dados); // Atualiza o nome do usuario com id = $id_usuario

echo $this->guard->item('nome'); // Bernardo Pinheiro Camargo

$this->guard->update(); // Atualiza a sessão com os valores do banco

echo $this->guard->item('nome'); // Bernardo
```


- getShortName
```php
    /**
     * getShortName
     *
     * @param String $nome [Nome completo do usuario]
     * @return String [Concatena o primeiro e o ultimo nome do usuario]
     */
    public function getShortName($nome = $this->user['nome']){
        $nomes = explode(" ", $nome);

        if(count($nomes) > 1)
            $nome = $nomes[0] . " " . $nomes[count($nomes)-1];        
        else
            $nome = $nomes[0];

        return $nome;
    }
```

Exemplo:

```php
echo $this->guard->item('nome'); // Bernardo Pinheiro Camargo

echo $this->guard->getShortName(); // Bernardo Camargo
```

- logout
```php
    /**
     * logout
     *
     * Limpa a sessão 'user'
     * 
     * @return void
     */
    public function logout() {
        $this->ci->session->unset_userdata('user');
    }
```

 * Exemplo:

```php
$this->guard->logout();
```


### Classe MY_Model
*Localizada em application/core/MY_Model.php*

Essa classe é responsável por permitir a reutilização de funções que são comuns em aplicações web que utilizam SQL para manipular um banco de dados.

```php
class MY_Model extends CI_Model {
    
    /**
    * table
    *
    * nome da tabela no model
    *
    * @protected
    */
    protected $table;

    /**
    * table_id
    *
    * chave da tabela no model
    *
    * @protected
    */
    protected $table_id;

    public function __construct() {
        parent::__construct();
    }
}
```

Basicamente a mágica acontece ao utilizar-se do conceito de herança da *Programação Orientada a Objetos*. Dessa forma os models a serem criados pelo desenvolvedor devem herdar essa classe através da palavra chave `extends`. O `$table`(nome da tabela) e `$table_id`(chave primária) são atributos essenciais para isso, pois essas duas representam as variáveis de qualquer consulta SQL básica, dessa forma podemos reutilizar funções como getAll, create, update e delete para quase todos os models.

> É obrigatório a inicialização destas em todos os models que herdarem esta classe.


- Exemplo: Model para tabela de `usuarios` cuja a chave primária é `id_usuario`

```php
class Usuarios_model extends MY_Model {

    /**
    * table
    *
    * nome da tabela no model
    *
    * @protected
    */
    protected $table = 'usuarios';

    /**
    * table_id
    *
    * chave da tabela no model
    *
    * @protected
    */
    protected $table_id = 'id_usuario';

    // metodo construtor
    public function __construct() {
        parent::__construct();
    }
}
```


#### Métodos
- create()
 ```php
    /**
    * create
    * 
    * insere um novo dado
    *
    * @param Array $dados [Dados a serem inseridos na tabela]
    * @return Boolean [Retorna true caso os dados tenham sido inseridos e false caso contrario]
    */
    public function create( $dados ){
        return $this->db->insert( $this->table, $dados );
    }
```
Exemplo:

 ```php
$this->load->model('Usuarios_model');

$dados = [
    'nome'          => 'Bernardo Pinheiro Camargo',
    'email'         => 'bernardopcamargo@gmail.com',
    'senha'         => '123456',
    'sexo'          => 'masculino',
    'data_nasc'     => '1996-04-03'
]; 

if($this->Usuarios_model->create($dados)){
    echo 'Usuário cadastrado com sucesso!';
}
else{
    echo 'Houve um erro no servidor.';
}

```

- update()
 ```php
   /**
    * update
    * 
    * atualiza um dado
    *
    * @param Array $dados [Dados a serem atualizados. *O campo 'id' deve ser passado obrigatoriamente]
    * @return Boolean [Retorna true caso os dados tenham sido atualizados e false caso tenha algum erro de lógica no SQL]
    */
    public function update( $dados ) {

        // prepara os dados
        $this->db->where( $this->table_id, $dados['id']);

        // deleta o id
        unset( $dados['id'] );
        if ( isset( $dados[$this->table_id] ) ) unset( $dados[$this->table_id] );

        // faz o update
        return $this->db->update($this->table, $dados); 
    }
```
Exemplo:

 ```php
$this->load->model('Usuarios_model');

$dados = [
    'id'            => 1,
    'nome'          => 'Bernardo Pinheiro Camargo',
    'email'         => 'bernardopcamargo@gmail.com',
    'senha'         => '123456',
    'sexo'          => 'masculino',
    'data_nasc'     => '1996-04-03'
]; 

if($this->Usuarios_model->update($dados)){
    echo 'Usuário atualizado com sucesso!';
}
else{
    echo 'Houve um erro no servidor.';
}

```

> O campo `id` do array é **obrigatório**.

- delete()
 ```php
   /**
    * delete
    * 
    * deleta um dado
    *
    * @param mixed $id [Chave primária da tabela]
    * @return Boolean [Retorna true caso remova a linha ou false caso contrario]
    */
    public function delete( $id ) {
        $this->db->where( $this->table_id, $id );
        return $this->db->delete( $this->table ); 
    }
```

Exemplo

```php
$this->load->model('Usuarios_model');

if($this->Usuarios_model->delete(1)){
    echo 'Usuário deletado com sucesso!';
}
else{
    echo 'Houve um erro no servidor.';
}
```

- getById()
 ```php
    /**
    * getById
    * 
    * pega um dado por id
    *
    * @param  $id [Chave primária da tabela]
    * @return mixed [Retorna um array com os dados requisitados ou false caso não encontre nada]
    */
    public function getById( $id ){
        
        // faz a busca
        $this->db->select( '*' )
        ->from( $this->table )
        ->where( [$this->table_id => $id ] );
        $query = $this->db->get();

        // verifica se existem resultados
        return ( $query->num_rows() > 0 ) ? $query->result_array()[0] : false;
    }
```
Exemplo

```php
$this->load->model('Usuarios_model');

$user = $this->Usuarios_model->getById(1);

if($user){
    echo $user['nome']; //Bernardo Pinheiro Camargo
}
else{
    echo 'Usuário não encontrado';
}
```


- getAll()
 ```php
    /**
    * getAll
    * 
    * pega todos os registros
    *
    * @param mixed $where [Opcional: Condições da consulta]
    * @param String $fields [Opcional: Campos do SELECT da consulta]
    * @param mixed $orderby [Opcional: Ordenação da consulta]
    * @return mixed [Retorna a coleção de dados requisitadas em uma matriz]
    */
    public function getAll( $where = false, $fields = '*', $orderby = false) {
        
        if($orderby){
            $orderby = explode(" ", $orderby);
            $order = $orderby[1];
            $order_colum = $orderby[0];
        }
        else{
            $order = 'asc';
            $order_colum = $this->table_id;
        }

        // monta a busca
        $this->db->select( $fields );
        $this->db->from( $this->table );

        //verifica se existe um where
        if ( $where ) $this->db->where( $where );

        $this->db->order_by($order_colum, $order);

        // pega os dados do banco
        $query = $this->db->get();

        // verifica se existem resultados
        return ( $query->num_rows() > 0 ) ? $query->result_array() : false;
    }   
```

Exemplo 1:

```php
$this->load->model('Usuarios_model');

$users = $this->Usuarios_model->getAll(); // Busca todas as tuplas da tabela `usuarios`

if($users){
    foreach ($users as $user) {
        echo $user['nome'] . '<br>';
    }
}
else{
    echo 'Usuário não encontrado';
}
```

Exemplo 2:

```php
$this->load->model('Usuarios_model');

$users = $this->Usuarios_model->getAll('sexo = masculino', 'usuarios.nome', 'nome asc'); // Será feita a consulta na tabela `usuarios` atrás das tuplas que possuem o sexo definido como masculino e retornará todos os nomes em ordem crescente.

if($users){
    foreach ($users as $user) {
        echo $user['nome'] . '<br>';
    }
}
else{
    echo 'Usuário não encontrado';
}
```


Exemplo 3:

```php
$this->load->model('Usuarios_model');

$users = $this->Usuarios_model->getAll(false, 'usuarios.nome', 'nome asc'); // Será feita a consulta na tabela `usuarios` e retornara todos os nomes dos usuarios ordenados pelo nome crescente.

if($users){
    foreach ($users as $user) {
        echo $user['nome'] . '<br>';
    }
}
else{
    echo 'Usuário não encontrado';
}
```


- getAllLimit()
 ```php
    /**
     * getAllLimit
     * 
     * @param int $limit [Inteiro que define a quantidade máxima de resultados da consulta]
     * @return Array[] [Retorna a coleção de dados requisitadas em uma matriz]    
     * 
     */
    public function getAllLimit($limit){
        $this->db->from($this->table)
        ->select('*')
        ->limit($limit);

        $busca = $this->db->get();

        return ($busca->num_rows() > 0) ? $busca->result_array() : array();

    }
```

Exemplo:

```php
$this->load->model('Usuarios_model');

$users = $this->Usuarios_model->getAllLimit(5); // Será feita a consulta na tabela `usuarios` e retornará as 5 primeiras linhas.

if($users){
    foreach ($users as $user) {
        echo $user['nome'] . '<br>';
    }
}
else{
    echo 'Usuário não encontrado';
}
```

> Esta função foi feita apenas para auxiliar no desenvolvimento da aplicação, para que quando você deseje testar uma listagem de dados possa limitar a quantidade de resultados, porém sem poder filtra-los.


### Plugins JS
Localizados em `assets/vendors/`

- animate.css
- animsition
- autosize
- bootstrap
- bootstrap-datepicker-1.9.0-dist
- bootstrap-daterangepicker
- bootstrap-datetimepicker
- bootstrap-progressbar
- bootstrap-wysiwyg
- Chart.js
- countdowntime
- cropper
- css-hamburgers
- datatables.net
- datatables.net-bs
- datatables.net-buttons
- datatables.net-buttons-bs
- datatables.net-fixedheader
- datatables.net-fixedheader-bs
- datatables.net-keytable
- datatables.net-responsive
- datatables.net-responsive-bs
- datatables.net-scroller
- datatables.net-scroller-bs
- DateJS
- daterangepicker
- devbridge-autocomplete
- dropzone
- echarts
- eve
- fastclick
- Flot
- flot.curvedlines
- flot.orderbars
- flot-spline
- font-awesome
- fullcalendar
- gauge.js
- google-code-prettify
- iCheck
- ion.rangeSlider
- jquery
- jquery.easy-pie-chart
- jquery.hotkeys
- jquery.inputmask
- jquery.tagsinput
- jquery-knob
- jquery-mousewheel
- jQuery-Smart-Wizard
- jquery-sparkline
- jqvmap
- kartik-v-bootstrap-fileinput
- malihu-custom-scrollbar-plugin
- mjolnic-bootstrap-colorpicker
- mocha
- moment
- morris.js
- normalize-css
- nprogress
- parsleyjs
- pdfmake
- perfect-scrollbar
- pnotify
- pnotify1
- raphael
- requirejs
- select2
- skycons
- smartWizard
- starrr
- switchery
- transitionize
- validator


## Contribuição
Desenvolvido por Bernardo Camargo [@bernacamargo](https://github.com/bernacamargo)


## License
[MIT](https://choosealicense.com/licenses/mit/)