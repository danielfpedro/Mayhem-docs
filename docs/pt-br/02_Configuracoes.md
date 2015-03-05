Você poderá obter acesso a configurações básicas do seu app acessando <code>config/app.php</code>.

* <code>debug</code> - Indicado usar <code>true</code> para desenvolvimento e <code>false</code> para produção, quando true retorna descrição dos erros .
* <code>responeType</code> - Diz em qual formato a classe <code>Response</code> deverá retornar os dados para os usuários do seu <em>app</em>, por enquanto apenas <code>JSON</code> foi implementado.
* <code>defaultTimezone</code> - Especifica a sua <code>default timezone</code>, você pode ver <a href="http://php.net/manual/en/timezones.php" target="_blank">aqui</a> todas as outras aceitas pelo PHP.

	$config	= ['debug' => true,
		'responseType' => 'JSON',
		'defaultTimezone' => 'America/Sao_Paulo',];