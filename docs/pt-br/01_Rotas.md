Para uma maior repidez na resolução das rotas o mayhem não usa nenhum tipo de rota personalizada.

O roteameto padrão leva em consideração duas regras:
1. REST
2. Controller/action/variaveis

Após o framework buscar a <code>action</code> desejada baseado nessas duas regras e nada for encontrado o usuário recebe a seguinte resposta <code>{"status": "errors", "message": "Page not found"}</code>.

O roteamento padrão busca como primeiro argumento da URL o controller, o segundo a action deste controller e tudo que vem depois é tratado como variavel <code>http(s)://seuhost.com/:controller/:action/:var/:var ...</code>, veja o exemplo:
	

	// Aqui você acessaria o Controller Articles e a action Index.
	http://mydomain.com/articles/index

### Regra do REST

<table>
	<thead>
		<tr>
			<th>
				Method
			</th>
			<th>
				URL
			</th>
			<th>
				Controller e Action chamados
			</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>
				GET
			</td>
			<td>
				/artigos
			</td>
			<td>
				Artigos::index()
			</td>
		</tr>
		<tr>
			<td>
				GET
			</td>
			<td>
				/artigos/1
			</td>
			<td>
				Artigos::view(1)
			</td>
		</tr>
		<tr>
			<td>
				POST
			</td>
			<td>
				/artigos
			</td>
			<td>
				Artigos::add()
			</td>
		</tr>
		<tr>
			<td>
				PUT
			</td>
			<td>
				/artigos/1
			</td>
			<td>
				Artigos::edit(1)
			</td>
		</tr>
		<tr>
			<td>
				DELETE
			</td>
			<td>
				/artigos/1
			</td>
			<td>
				Artigos::delete(1)
			</td>
		</tr>
	</tbody>
</table>

## Regra do Contoller e action

<table>
	<thead>
		<tr>
			<th>
				Method
			</th>
			<th>
				URL
			</th>
			<th>
				Controller e Action chamados
			</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>
				Qualquer
			</td>
			<td>
				/artigos/ver_ultimos_artigosrtigos/1/2
			</td>
			<td>
				Artigos::ver_ultimos_artigos(1, 2)
			</td>
		</tr>
	</tbody>
</table>

	/artigos/action_qualquer/alpha/beta

	namespace App\Controller;

	class ArtigosController extends AppController
	{
		public function action_qualquer($arg1, $arg2)
		{
			echo $arg1;		// Saída: alpha
			echo $arg2;		// Saída: beta
		}
	}