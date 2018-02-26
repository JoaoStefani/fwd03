## Passos que foram executados

### Inserir pacotes composer.json
    "infyomlabs/laravel-generator": "5.5.x-dev",
    "laravelcollective/html": "^5.5.0",
    "infyomlabs/adminlte-templates": "5.5.x-dev",
	"infyomlabs/swagger-generator": "dev-master",
    "jlapp/swaggervel": "dev-master",
    "cagartner/correios-consulta": "0.1.*",
    "yajra/laravel-datatables-buttons": "^3.0",
    "yajra/laravel-datatables-html": "^3.1",
    "yajra/laravel-datatables-oracle": "~8.0,
    "doctrine/dbal": "~2.3"


### Publicar vendor

    php artisan vendor:publish

### Atualizar arquivo RouteServiceProvider -> método mapApiRoutes

    Route::prefix('api')
    ->middleware('api')
    ->as('api.')
    ->namespace($this->namespace."\\API")
    ->group(base_path('routes/api.php'));  

### Publicar arquivos de apoio do Infyom

    php artisan infyom:publish

### publica o layout

    php artisan infyom.publish:layout

### Ajustar .env e laravel_generator.php


### Abrir Workbench exporta migrations, cria migration geral e copia tabelas, adicionando timestamps e softdeletes, e rodar as migrations
    php artisan migrate
    

### Criar o seeder e rodar
    \App\User::create([
        'id'=>1,
        'name'=> 'Admin',
        'email'=> 'admin@admin.com',
        'password' => bcrypt('123456')
    ]);

### Rodar customização dos templates
    php artisan infyom.publish:templates


### Rodar scaffold dos clientes
    php artisan infyom:scaffold Client --fromTable

### Ajustar rules do Client.php 
RULES (unique CPF), campo cast date birthday
Edita rules do UpdateClientRequest

    $rules = Client::$rules;
    $rules['cpf'] = 'required|unique:clients,cpf,'.collect(request()->segments())->last();
    return $rules;


### Controller (passa clientes por array)
	$clients = [''=>'Choose...'] + Client::pluck('name','id')->toArray();
	
### DataTables
Adiciona join na query
	
	 ->select([
    'orders.id',
    'orders.value',
    'orders.description',
    'orders.paid_at',
    'orders.created_at',
    'clients.name as client'])
    ->join('clients', 'clients.id', 'orders.client_id')

### Editar colunas 
	$dataTable->editColumn('paid_at', function($obj){
            return $obj->paid_at? $obj->paid_at->format('d/m/Y') : "";
        });
        $dataTable->filterColumn('paid_at', function($query, $keyword){
           $query->whereRaw("DATE_FORMAT(paid_at,'%d/%m/%Y') like ?", ["%$keyword%"]);
        });

### Ajustar getColumns
    return [
            'client'=>['name'=>'clients.name','data'=>'client'],
            'value',
            'description',
            'paid_at',
            'created_at'
        ];
