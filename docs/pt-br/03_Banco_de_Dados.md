### Configurando
Você poderá especificar em <code>config/datasource.php</code> quantas conexões você desejar.

	$datasource = [
		'default'=> [
			'type'=> 'mysql',
			'host'=> 'localhost',
			'dbname'=> 'blog',
			'user'=> 'meuuser',
			'password'=> 'senhasupersegura',
			'charset'=> 'utf8'
		],
	];

Por padrão o <code>Model</code> usa a conexão <code>default</code> como padrão, para usar alguma outra você simplesmente precisa passar o nome quando instancia-lo.

	$Artigo = new Artigo; // Usando a conexão default
	$Artigo = new Artigo('outra'); // Usando outra conexão

### Obtendo conexão do PHP PDO
O <code>Model</code> cuida de toda a parte chata de conexão com o banco porém se por algum motivo você precisar usar o <a href="http://php.net/manual/en/book.pdo.php" target="_blank">PDO</a> do zero, você poderá faze-lo usando a classe <code>Datasource</code>

	//src/Controller/ArtigoController.php
	namespace App\Controller

	use Mayhem\Datasource\Datasource;

	class ArtigoController extends AppController 
	{
		public function index()
		{
			$conn = Datasource::getConnection('default'); // Daqui para baixo você poderá usar o PDO normamente usando a variavel $conn
		}
	}