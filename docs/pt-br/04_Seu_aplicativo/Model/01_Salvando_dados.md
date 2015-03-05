<br>
### Salvando

Para salvar o seus dados você deverá usar o método <code>save</code> para <code>criar</code> e o método <code>update</code> para <code>editar</code> os seus dados. Ambos devem receber um <em>array</em> com os dados a serem salvos como primeiro paramêtros com a diferença que o <code>update</code> deverá conter a chave primária da tabela neste campo.

	//src/Controller/ArtigoController.php
	...
	public function add() {
		$data = ['titulo' => 'Um título bem legal'];
		$this->Artigo->save($data, ['validate' => false]);
	}

	public function edit() {
		$data = ['id' => 1, 'titulo' => 'Um título mais legal ainda'];
		$this->Artigo->update($data, ['validate' => false]);
	}
	...
<br>

### Custom update
O método <code>update</code> salva os seus dados fazendo a edição baseado no valor da chave primário que você passar para ele. Caso você precise de condições mais específicas você deverá usar um <code>custom update</code>.

> É importante lembrar que <code>customUpdate</code> não executa a validação dos dados, você deverá acioná-la explicitamente.

### Desabilitando a validação

Caso os métodos <code>defaultRules</code> ou <code>customRules</code> estejam presentes no seu model eles serão executados automaticamente antes dos dados serem salvos com os métodos <code>save</code> ou <code>update</code>. Você pode desabilitar a validação passando a chave <code>validate</code> igual a <code>false</code> no array de opção no controller.

	//src/Controller/ArtigoController.php
	...
	public function add() {
		$this->Artigo->save($data, ['validate' => false]);
	}

	public function edit() {
		$this->Artigo->update($data, ['validate' => false]);
	}
	...

<br>

### Evitando ataques de massa com a opção allowedFields
Os métodos <code>save</code> ou <code>update</code> montam a sua <em>query</em> usando o nome das chaves do <em>array</em> que você passa como primeiro paramêtro. O trabalho de escrever <em>querys</em> enormes para salvar formulários igualmente enormes é poupado. O problema é que um usuário mal intencionado pode facilmente injetar um novo campo do formulário com o nome de um campo que você <strong>não quer ou não pode salvar</strong> neste momento. Suponha que você tem uma tabela chamada <code>users</code> e tem um campo chamado <code>facebook_id</code> que carrega o <code>id</code> da conta do <em>Facebook</em> do usuário. Agora imagine que em uma determinada parte do seu <em>app</em> você precisa atualizar o nome deste usuário.

	//src/Controller/UsersController.php
	...
	public function editaNome()
	{
		// $data->Request->post carrega os dados vindos de um formulário contendo ['id' => 1, 'nome' => 'Novo Nome']
		$this->User->update($data->Request->post);
	}
	...

<br>

### Validação
A validação dos dados é feita através do método <code>defaultRules</code> que deverá retornar um array com os campos e as regras que serão validadas.


A validação do Mayhem Framework foi criada no topo da biblioteca <a href="https://github.com/vlucas/valitron" target="_blank">Valitron</a> e todas as regras default poderão ser acompanhdas <a href="https://github.com/vlucas/valitron#built-in-validation-rules" target="_blank">aqui!</a>

	//src/Mode/Artigo.php
	namespace App\Model;

	class Artigo extends AppModel
	{
		public function defaultRules()
		{
			$rules = [
				'titulo' =>	[
					'required', // Regra sem parametros
					['lengthMin', 1] // Regra com parametros
				]
			];

			return $rules;
		}
	}

<br>

Você também pode especificar através da chave <code>on</code> se a regra será executada apenas quando for salvar <code>create</code> ou na edição <code>update</code>. Caso a não seja especificada a regra será executada em ambos os casos.

	//src/Mode/Artigo.php
	public function defaultRules()
	{
		$rules = [
			'titulo' =>	[
				['required', 'on' => 'create']
			]
		];

		return $rules;
	}

<br>
Também é possível passar uma mensagem customizada caso a regra falhe.

	//src/Model/Artigo.php
	public function defaultRules()
	{
		$rules = [
			'titulo' =>	[
				['required', 'message' => 'Você deve preencher este campo =]']
			]
		];

		return $rules;
	}

<br>

No exemplo acima você está fazendo um <code>update</code> do nome do usuário com <em>id</em> igual 1, agora acompanhe como seria o ataque.

	//src/Controller/UsersController.php
	...
	public function editaNome()
	{
		// $data->Request->post carrega os dados vindos de um formulário contendo um campo injetado chamado facebook_id ['id' => 1, 'nome' => 'Novo Nome', 'facebook_id' => '123456']
		$this->User->update($data->Request->post); 
	}
	...
<br>

No exemplo acima o atacante conseguiria trocar o seu próprio <em>facebook_id</em> para o que ele bem quisesse, esse é só um exemplo dentro tantos outros ataques mais criativos que poderiam colocar o seu app em cheque.
A forma simples de resolver esse problema é usando a opção <em>allowedFields</em>.

	//src/Controller/UsersController.php
	...
		public function editaNome()
		{
			// $data->Request->post carrega os dados vindos de um formulário contendo um campo injetado chamado facebook_id ['id' => 1, 'nome' => 'Novo Nome', 'facebook_id' => '123456']
			// Porém somente o que estiver especificado na opção allowedFields será salvo
			$this->User->update($data->Request->post, ['allowedFields' => ['nome']]);
		}
	...
<br>

### Regras customizadas

Caso você tenha algum campo que possua uma lógica de validação bem particular é possível criar regras customizadas, para isso você deverá usar o método <code>customRules</code> que deverá retornar um array com as regras que por sua vez deverá retornar valores boleanos.

	//src/Model/Artigo.php
	...
	public function customRules()
	{
		$rules = [
			'regraCustomizada' => [
				'rule' => function($field, $value, array $params){
					return true;
				}, 
				'message' => 'Mensagem aqui' // Mensagem que será exibida caso a regra retorne false
			]
		];

		return $rules;
	}
	...