<br>

O Controller é a classe que é acessada pela URL requisitada pelo usuário que por sua vez executa um *action* que está dentro dela.

Basicamente o controller serve para receber os dados do usuário, fazer algum tipo de ação com aquele dado e responder ao usuário o resultado daquela ação

Um controller **DEVE** seguir os seguintes padrões
* Ficar na pasta <code>src/Controller/</code>
* Herdar a classe <code>AppController</code>
* Ter o nome em <a href="http://en.wikipedia.org/wiki/CamelCase" target="_blank">CamelCase</a> seguido de controller

Exemplo:

	// src/Controller/ArtigosController.php

	namespace App\Controller;

	class ArtigosController extends AppController
	{
	}