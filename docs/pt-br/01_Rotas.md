Para uma maior repidez na resolução das rotas o mayhem não usa nenhum tipo de rota personalizada.

O roteameto padrão leva em consideração duas regras:
1. Restful
2. Controller/action/variaveis

Após o framework buscar a <code>action</code> desejada baseado nessas duas regras e nada for encontrado o usuário recebe a seguinte resposta <code>{"status": "errors", "message": "Page not found"}</code>.

O roteamento padrão busca como primeiro argumento da URL o controller, o segundo a action deste controller e tudo que vem depois é tratado como variavel <code>http(s)://seuhost.com/:controller/:action/:var/:var ...</code>, veja o exemplo:
	

	// Aqui você acessaria o Controller Articles e a action Index.
	http://mydomain.com/articles/index

Porém antes de casasr
