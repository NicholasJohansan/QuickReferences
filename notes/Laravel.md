### Notes
The following notes about Laravel are about using Laravel as an API backend.  
The notes here may also not be 100% accurate.

## Getting Started
1. Create Laravel Project: `composer create-project laravel/laravel project_name`
2. Edit DB details in `.env`

## Contents
- [Models](#models)
- [Controllers](#controllers)
- [Migrations](#migrations)
- Middlewares *To be written*
- Routing *To be written*
- Factories *To be written*
- Seeders *To be written*
- [Eloquent Operations](#eloquent-operations)

### Models
Creation: `php artisan make:model ModelName`  
*- creates app/Models/ModelName.php*

#### Flags
- `-m` *creates corresponding migration file at database/migrations/*
- `-f` *creates corresponding factory at database/factories/ModelFactory.php*
- `-s` *creates corresponding seeder at database/seeders/ModelSeeder.php*
- `-c` *creates corresponding controller at app/Http/Controllers/ModelsController.php*

Importing: `use App\Models\ModelName;`

By default *primary key* is ModelName in snake_case.  
To change, specify `protected $primaryKey = 'model_id';` in the model file

By default *timestamps* fields are enabled (*created_at*, *updated_at*)  
To disable, specify `public $timestamps = false;` in the model file

### Controllers
Creation: `php artisan make:controller ModelsController`  
*- creates app/Http/Controllers/ModelsController.php*  
Convention is to name the controllers as ModelName(plural)Controller

Importing: `use App\Http\Controllers\ModelsController;`

#### Flags
- `--resource` *creates `index`, `create`, `store`, `show`, `edit`, `update`, `destroy` methods*
- `--api` *creates `index`, `show`, `update`, `destroy` methods*
- `--model=ModelName` *binds ModelName to controller*

### Migrations
Creation: `php artisan make:migration migration_name`  
*- creates database/migrations/timestamp_migration_name.php*

#### Operations:  
Create Table: `Schema::create('models', function (Blueprint $table) {});`  
Drop Table: `Schema::dropIfExists('models');`  
Update Table: `Schema::table('models, function (Blueprint $table) {});`  
Add field: `$table->field_type_func(field_name);` *refer to below*  
Modify field: `field_type_func(field_name)->modifier();` *refer to below*  
Foreign field: `$table->foreign(field_name)->references(table_id_field_name)->on(table_name);`  
Unique index: `$table->field_type_func(field_name)->unique();`  
Primary key: `$table->primary(field_name);`

#### Field Types (Common)
<table>
	<tr>
		<th>Type</th>
		<th>Function</th>
		<th>Notes</th>
	</tr>
	<tr>
		<td>VARCHAR(length)</td>
		<td><pre lang="php">string(field_name, length)</pre></td>
		<td>&nbsp;</td>
	</tr>
	<tr>
		<td>CHAR(length)</td>
		<td><pre lang="php">char(field_name)</pre></td>
		<td>&nbsp;</td>
	</tr>
	<tr>
		<td>TEXT</td>
		<td><pre lang="php">text(field_name)</pre></td>
		<td>&nbsp;</td>
	</tr>
	<tr>
		<td>UNSIGNED INTEGER (primary key)</td>
		<td><pre lang="php">increments(field_name)</pre></td>
		<td>Incrementing unsigned integer primary key <br/> BIGINT: bigIncrements, SMALLINT: smallIncrements</td>
	</tr>
	<tr>
		<td>INTEGER</td>
		<td><pre lang="php">integer(field_name)</pre></td>
		<td>BIGINT: bigInteger, SMALLINT: smallInteger</td>
	</tr>
	<tr>
		<td>FLOAT</td>
		<td><pre lang="php">float(field_name, precision, scale)</pre></td>
		<td>precision is total digits, scale is decimal digits <br/> DOUBLE: double, DECIMAL: decimal</td>
	</tr>
	<tr>
		<td>BOOLEAN</td>
		<td><pre lang="php">boolean(field_name)</pre></td>
		<td>&nbsp;</td>
	</tr>
	<tr>
		<td>ENUM</td>
		<td><pre lang="php">enum(field_name, [allowed_values])</pre></td>
		<td>&nbsp;</td>
	</tr>
	<tr>
		<td>BLOB</td>
		<td><pre lang="php">binary(field_name)</pre></td>
		<td>&nbsp;</td>
	</tr>
	<tr>
		<td>DATE</td>
		<td><pre lang="php">date(field_name)</pre></td>
		<td>YYYY-MM-DD</td>
	</tr>
	<tr>
		<td>TIME</td>
		<td><pre lang="php">time(field_name, precision)</pre></td>
		<td>HH:MM:SS</td>
	</tr>
	<tr>
		<td>DATETIME</td>
		<td><pre lang="php">dateTime(field_name, precision)</pre></td>
		<td>YYYY-MM-DD HH:MM:SS<br/>1000-01-01 00:00:00 to 9999-12-31 23:59:59</td>
	</tr>
	<tr>
		<td>TIMESTAMP</td>
		<td><pre lang="php">timestamp(field_name, precision)</pre></td>
		<td>YYYY-MM-DD HH:MM:SS<br/>1970-01-01 00:00:01 to 2038-01-19 03:14:07</td>
	</tr>
	<tr>
		<td>TIMESTAMPS</td>
		<td><pre lang="php">timestamps()</pre></td>
		<td>created_at and updated_at columns</td>
	</tr>
</table>

#### Field Modifiers (Common)
<table>
	<tr>
		<th>Modifier</th>
		<th>Notes</th>
	</tr>
	<tr>
		<td><pre lang="php">nullable()</pre></td>
		<td>Make field optional</td>
	</tr>
	<tr>
		<td><pre lang="php">default(value)</pre></td>
		<td>Specify default value for field</td>
	</tr>
	<tr>
		<td><pre lang="php">unsigned()</pre></td>
		<td>Make integer field unsigned</td>
	</tr>
	<tr>
		<td><pre lang="php">autoIncrement()</pre></td>
		<td>Field auto increments (primary key)</td>
	</tr>
	<tr>
		<td><pre lang="php">useCurrent()</pre></td>
		<td>Assign current timestamp</td>
	</tr>
</table>

### Eloquent Operations
<table>
	<tr>
		<th>Operation</th>
		<th>Code</th>
	</tr>
	<tr>
		<td>Get All</td>
		<td>
			<pre lang="php">Model::all();</pre>
		</td>
	</tr>
	<tr>
		<td>Get By Primary Key</td>
		<td>
			<pre lang="php">Model::find($primaryKey);</pre>
		</td>
	</tr>
	<tr>
		<td>Get By Criterion</td>
		<td>
			<pre lang="php">Model::where($col_name, $operator, $value)->get();</pre>
		</td>
	</tr>
	<tr>
		<td>Insert New Model</td>
		<td>
			<pre lang="php">$model = new Model;
const MODEL_FIELDS = ['field1', 'field2', 'field3'];
foreach (MODEL_FIELDS as $field) {
	$model->$field = $request->$field;
}
if ($model->save()) {
	return $model; // or response()->json([keyed_array], status_code)
}</pre>
		</td>
	</tr>
	<tr>
		<td>Update Model</td>
		<td>
			<pre lang="php">$model = Model::find($id); // may use way to retrieve model
const MODEL_FIELDS = ['field1', 'field2', 'field3'];
foreach (MODEL_FIELDS as $field) {
	if ($request->$field) {
		$model->$field = $request->$field
	}
}
if ($model->save()) {
	return $model; // or response()->json([keyed_array], status_code)
}</pre>
		</td>
	</tr>
	<tr>
		<td>Delete Model</td>
		<td>
			<pre lang="php">$model = Model::find($id); // may use way to retrieve model
if ($model->delete()) {
	return $model; // or response()->json([keyed_array], status_code)
}</pre>
		</td>
	</tr>
</table>