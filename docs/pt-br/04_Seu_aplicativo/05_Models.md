<br>
Os models são as classes aonde ficam a lógica de negócio do seu app. Eles serão responsáveis por todo tipo de interação dos seu dados.

Um model **DEVE** seguir os seguintes padrões
* Ficar na pasta <code>src/Model/</code>
* Herdar a classe <code>AppModel</code>
* Ter o nome em <a href="http://en.wikipedia.org/wiki/CamelCase" target="_blank">CamelCase</a>. Caso ele esteja fazendo referencia a alguma tabela do banco de dados 


Exemplo:

	// src/Model/Artigo.php

	namespace App\Model;

	class Artigo extends AppModel
	{
		public $tableName = 'artigos';
	}

## App model

A classe <code>AppModel</code> é uma *parent class* de todos os seus models, ou seja, deverá conter toda a lógica em que você queira que seja visível para todos os models do seu app.

	// src/Model/AppModel.php
	namespace App\Model;

	use Mayhem\Model\Model;

	class AppModel extends Model
	{

	}

### Atributos

* <code>$this->tableName</code>
* <code>$this->pkName</code>
* <code>$this->connection</code>

### $this->tableName

Por padrão os nomes dos models são no singular e os nomes das tabelas no plural. Para evitar o uso de uma classe inflector para pluralizar o nome do model, o nome da tabela deverá obrigatoriamente ser declarado.

### $this->pkName

Este atributo é usado para alterar o nome da sua chave primária que por padrão é <code>id</code>.

### $this->connection

Este atributo é usado para alterar o nome da conexão usada pelo model que por padrão é <code>default</code>.

### Alterando a conexão do Model no criação da instância.
Como já tido acima, a conexão a ser usada pelo model é referenciada pelo Framework através do atributo <code>$this->connection</code>. Imagine que em um determinado lugar do seu app você precise usar uma outra, mas somente naquele ponto. Você pode fazer isso explicitando uma conexão diferente na criação da instância do model, exemplo:

	$this->$Artigo = new Artigo('outra_conexao');