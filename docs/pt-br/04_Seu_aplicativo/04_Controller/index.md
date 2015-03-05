<br>

O Controller é a classe que é acessada pela URL requisitada pelo usuário que por sua vez executa um *action* que está dentro dela.

Basicamente o controller serve para receber os dados do usuário, fazer algum tipo de ação com aquele dado e responder ao usuário o resultado daquela ação

Um controller **DEVE** seguir os seguintes padrões
* Ficar na pasta <code>src/Controller/</code>
* Herdar a classe <code>AppController</code>
* Ter o nome em <a href="http://en.wikipedia.org/wiki/CamelCase" target="_blank">CamelCase</a> seguido de controller

Exemplo:

	//src/Controller/ArtigosController.php

	namespace App\Controller;

	class ArtigosController extends AppController
	{
	}

### Request

* <code>action</code> - Nome da action atual
* <code>controller</code> - Nome do controller atual
* <code>params</code> - <em>Array</em> com o valor passado por qualquer método em formato serializado
* <code>get</code> - Valores passados por <code>GET</code>
* <code>post</code> - Valores passados por <code>POST</code> que estiverem em formato serializado
* <code>delete</code> - Valores passados por <code>DELETE</code> que estiverem em formato serializado
* <code>headerBodyJson</code> - JSON passado pelo corpo do cabeçalho, o <code>$http</code> do <a href="https://angularjs.org/" target="_blank">Angularjs</a> por exemplo sempre passa dados através este formato.

Exemplo de uso: <code>$this->request->get['nome']</code> ou <code>$this->request->post['nome']</code>.

### Response
Toda requisição feita ao seu <em>Webservice</em> espera uma resposta contento um código de status e alguns dados informativos no seu cabeçalho.

### Resposta de sucesso
Este método recebe apenas um parametro que é aonde você deverá colocar os dados da resposta, ele automaticamente retorna o <code>status</code> igual a <code>200 </code>
	
	return $this->Response->success($artigos);

### Resposta de erro
Este método recebe três parametros sendo eles, o <code>código de status</code>, <code>descrição</code> e <code>tipo de erro</code> sendo que o último é opcional tendo <code>error</code> como o seu valor<code>default</code>.


	return $this->Response->error(400, 'Não foi possível salvar as suas informações');

Exemplo dos dois:
	//src/Controller/ArtigosController.php
	public function add()
	{
		if ($this->Artigo->save($data)) {
			return $this->Response->success('ok');
			//Status Code:
			//200
			//Header Body:
			//"ok"
		} else {
			$this->Response->error(400, 'Não foi possível salvar as suas informações');
			//Status Code:
			//400
			//Header Body:
			//{"message":"Não foi possível salvar as suas informações", "type":"error", "code":400}
		}
	}