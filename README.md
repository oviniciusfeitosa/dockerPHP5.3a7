Docker: Migrando Versões do PHP
===============================

Quais os topicos serão abordados?
---------------------------------

- O que para de funcionar com a mudança do PHP 5.3 para o 5.6
- Quais as melhorias entre as versões
- Qual SO é mais eficaz em aspecto de segurança, performance e compatibilidade com PHP

Docker
------

É uma ferramenta que descentraliza e melhor distribui a utilização de recursos que são necessários para executar sua aplicação. O docker trabalha com Imagens, que são um conjunto ou template, e trabalha com Containers, que são implementações das imagens mencionadas.

DockerFile
----------

Esse arquivo pode ser utilizado de duas maneiras na geração de imagens para o Docker são elas:
- Quando executado um comando `docker build` e já estando na raiz da aplicação onde está localizado o DockerFile ou quando utilizado o comando `docker build -f {caminho_para_dockerfile} .`
- Quando executado o comando `docker-compose` em conjunto com uma ação, por exemplo: `docker-compose up --build`

Abaixo temos um exemplo de DockerFile que utiliza a imagem "nimmis/apache-php5" como referência.

  #Dockerfile Example 1

	FROM nimmis/apache-php5

	MAINTAINER SemaphoreCI <dev@semaphoreci.com>

	COPY 000-default.conf /etc/apache2/sites-available/000-default.conf

	EXPOSE 80
	EXPOSE 443

	CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]


Conforme esse link ( https://github.com/kost/docker-alpine/blob/master/alpine-apache-php/Dockerfile ) temos outro exemplo utilizando PHP + Apache:

  # Dockerfile Example 2

	FROM gliderlabs/alpine
	MAINTAINER kost - https://github.com/kost

	RUN apk --update add php-apache2 curl php-cli php-json php-phar php-openssl && rm -f /var/cache/apk/* && \
	curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
	mkdir /app && chown -R apache:apache /app && \
	sed -i 's#^DocumentRoot ".*#DocumentRoot "/app"#g' /etc/apache2/httpd.conf && \
	sed -i 's#AllowOverride none#AllowOverride All#' /etc/apache2/httpd.conf && \
	echo "Success"

	ADD scripts/run.sh /scripts/run.sh
	RUN mkdir /scripts/pre-exec.d && \
	mkdir /scripts/pre-init.d && \
	chmod -R 755 /scripts

	EXPOSE 80

	# VOLUME /app
	WORKDIR /app

	ENTRYPOINT ["/scripts/run.sh"]

Changelog : Principais diferenças entre versões do php 5.3 até a versão 5.6
---------------------------------------------------------------------------

De tempos em tempos, temos o conhecimento de que são lançadas novas versões do PHP e cada versão tem suas particularidade sque vão desde o ganho em performance até novas funções, recursos e descontinuidade de algumas funções.

Como a lista que contém todas as diferenças entre as versões do php é muito extensa, seguem abaixo os links que contém todas as diferenças entre versões do php.

- http://php.net/manual/en/migration54.php
- http://php.net/manual/en/migration55.php
- http://php.net/manual/en/migration56.php
- http://php.net/manual/en/migration70.php

PHP 7
-----

PHP 7 é a versão mais atual e mais revolucionária do PHP.

Já sabemos que o antecessor do php 7 não foi a versão 6, mas sabe qual foi o principal motivo para não ter existido a versão 6? Foi porque a versão não estava apresentando resultados satisfatórios comparando com a versão 5.6.

http://magazine.softerize.com.br/wp-content/uploads/2015/12/wp-php7-performance.jpg

Como podemos observar no gráfico acima, extraído do site "magazine.softerize.com.br" é póssível observar a grande quantidade de requisições por segundo que o PHP7 consegue suportar em relação a versão 5.6 ao utilizar um CMS como o Wordpress, por exemplo. 

Suporte : PHP versions
----------------------

De acordo com o próprio site do php temos suporte ativo para o php 5.6 até 31/12/2016 e suporte de segurança até 31/12/2018. Já a versão 7 do php tem suporte ativo até "3/12/2017" e suporte de segurança até "3/12/2018". Mais informações no link à seguir: http://php.net/supported-versions.php)


Docker Vs. PHP : Qual o melhor S.O.?
------------------------------------

Atualmente existe existem repositórios no hub.docker do "PHP" ( https://hub.docker.com/_/php/ ) com configurações prontas para a execução do php de acordo com o sistema operacional desejado.
Existem várias imagens disponívels para trabalhar com sistemas operacionais já com o PHP instalado ou instalando manualmente em um sistema operacional cru, como o debian ou ubuntu, por exemplo. 

Um exemplo de utilização de micro imagem para Docker, como Alpine, em conjunto com PHP, pode ser visualizada aqui: https://github.com/kost/docker-alpine/tree/master/alpine-apache-php

Através do Docker, temos a possibilidade de descentralizar recursos, criando serviços distintos para cada recurso desejado. Por exemplo, podemos ter um serviço que trabalha com um container somente para a execução do PHP 7 + NGINX ou mesmo PHP 5.6 + Apache, um somente para trabalhar com um banco de dados como o Postgres e outro somente com MySQL.


Migrar : PHP 5.3 p/ PHP 7?
--------------------------

A versão 5.3 do php entrou no chamado "end-of-life" desde Agosto de 2014. Isso significa que regras de segurança e conteúdo legado não será mais reparado ou mantido.

A mudança da versão 5.3 comparando com a versão versão 5.4 passou a ser mais adotada principalmente pelo ganho de performance, que consequentemente ocasionou mais economia de recursos e investimentos em hardware pelo melhor uso dos recursos. O melhor manejo do cache da aplicação também favoreceu mais agilidade no processamento da aplicação. Mas se compararmos o ganho de recursos que houveram da versão 5.3 até a versão 7, veremos que houve uma diferença enorme!

Vários sites mostram que à partir da versão "5.3" quando comparada com a "5.4" tem um grande ganho de performance e consumo de memória.

Migrar : Melhorias
------------------

Abaixo listarei algumas novidades que tivemos em cada versão o php até a 7.

Desde a versão 5.3 temos recursos como:
- Suporte a namespaces;
- Suporte a Closures;
- Dois novos métodos mágicos "__callStatic()" e "__invoke()";
- Constantes podem ser declaradas fora da classe usando o palavra chave const;
- Operador ternário tem a opção "?";
- Acesso dinâmico a métodos mágicos;
- Exceptions podem ser aninhadas.

Alguns recursos novos também foram implantados na versão 5.4, como por exemplo:
- Simplificação do uso de arrays por "[]";
- Suporte a Traits
- WebServer CLI embutido;
- Entre outras coisas (etc).

Da versão 5.4 para 5.5 também houveram grandes melhorias, como por exemplo:
- Possibilidade de utilizar "finally" ao final de um bloco de verificação de exceções ( try ... catch );
- Função "foreach" agora suporta receber como parâmetro uma "list()";
- O método "empty" agora valida operadores arbitráriso ( true / false );
- Agora é possível trabalhar com arrays em strings como operador literal. Exemplo: http://php.net/manual/pt_BR/migration55.new-features.php#migration55.new-features.const-dereferencing
- Quando realizadas chamadas estáricas como "MyClass::class" agora é retornado o nome da classe, inclusive se já faz parte de um namespace ou não.
- Adição da extensão OPcache
- Agora a função "foreach" consegue trabalhar com chaves não escalares, podendo iterar Tipos, pode exemplo.
- Melhorias no tratamento de imagem

Algumas novidade dos php versão "5.4" podem ser encontradas aqui: http://www.phpit.com.br/artigos/o-que-ha-de-novo-no-php-5-4-conheca-as-mudancas-mais-significativas.phpit

Já a versão 5.5 para 5.6 é possível listar as seguintes melhorias:
- Possibilidade de aplicar valores escaláveis para constantes. Exemplo: `const ONE_THIRD = ONE / self::THREE;` ;
- Também é possível definir uma constante como um array. Exemplo: `const ARR = ['a', 'b']; echo ARR[0];` ;
- Agora é possível utilizar "Variadic functions", que antigamente era utilizada através da função "func_get_args()" e agora foi substituído pelo operador "...". Exemplo: http://php.net/manual/pt_BR/migration56.new-features.php#migration56.new-features.variadics
- Operador "**" pode ser utilizado como exponencial para operações. Exemplo: `2 ** 3 == 8 `
- Possibilidade de utilizar o "use function" e "use const" sem o uso de herança ou mesmo implementação de interface para o uso do recurso presente no namespace;
- A função "default_charset" veio para substituir funções como "iconv" e "mbstring" e agora é utilizada por funções como  "htmlentities", "html_entity_decode" e "htmlspecialchars";
- Uploads de arquivos maiores que 2 GB agora são suportados;
- Adição da função "hash_equals" que compara duas strings com a constante de tempo;
- Grandes melhorias foram realizadas relacionadas ao suporte a "SSL/TLS", como validações de fingerprint, TLS renegotiation attacks e novos recursos que permitem trabalhar com stream criptografada e controlada pelo protocolo.

A versão 7 foi a que mais trouxe recursos novos. Listarei alguns abaixo:
- Declarações de tipos escalares;
- Declarações de tipo de retorno;
- Operador de coalescência nula ( ?? ). Exemplo: `$username = $_GET['user'] ?? 'nobody';` ;
- Declaração de arrays constantes;
- Suporte a Classes Anônimas;
- Uso de Expectations, que possui retrocompatibilidade a função "assert()";
- Agrupamento de declarações use;
- Adição da função "intdiv" que faz a divisão inteira de seus operandos e as retorna.
- Mais opções para trabalhar com sessões
- A nova função preg_replace_callback_array() permite que o código seja escrito de forma mais clara com a utilização da função preg_replace_callback().

Entre as diferenças mais da mudança da versão "5.6" para "7" do php, a mais notória são :
- Ganho de velocidade na execução da aplicação 
- Eficácia no consumo de memória

Migrar : Incompatibilidade?
---------------------------

Abaixo é possível acessar a lista com todas as incompatibilidades que serão geradas à partir das mudanças de versões do php:
- http://php.net/manual/pt_BR/migration54.incompatible.php
- http://php.net/manual/pt_BR/migration55.incompatible.php
- http://php.net/manual/pt_BR/migration56.incompatible.php
- http://php.net/manual/pt_BR/migration70.incompatible.php


Para que possamos ter mais precisão antes de fazer a migração de uma versão para outra do PHP, precisamos utilizar ferramentas que testam nosso código-fonte, verificando se os recursos utilizados atualmente são compatíveis com a versão desejada.

Enquanto não executarmos isso, não teremos a visibilidade do que é necessário alterar para que comporte a versão desejada.

Frameworks : Compatibilidade
----------------------------

Cada framework possui uma versão minima estável e necessária para que o framework possa funcionar em seu perfeito estado. A versão mínima do php para que o ZF1 funcione corretamente é a 5.2. O link do changelog da Zend à seguir sobre o ZF1, mostra que desde a versão "1-12-12" o framework já tem compatibilidade com o PHP7, conforme mencionado neste artigo da Zend: https://framework.zend.com/blog/zend-framework-1-12-12-released.html

Uma outra possibilidade para verificar se é possível utilizar o PHP 7 e inclusive testar sua compatibilidade é utilizando a ferramenta "PHPCompability" ou "PHP7CC".

O repositório "PHPCompatibility" localizado no github, utiliza o PHPCode_Sniffer e tem como responsabilidade fazer validações de compatibilidade inclusive de versões do php. Essa ferramenta pode ser muito útil para validarmos se o que temos realizado é compatível com o que atualmente suportado pelo servidor. Vale à pena conferi-lo: https://github.com/wimg/PHPCompatibility

Já o repositório PHP7CC, apesar de ser similar ao "PHPCompability", possui o foco na versão 7 do PHP e pode ser acessado aqui:  https://github.com/sstalle/php7cc .

PHP : Versões sob demanda
-------------------------

Uma outra possibilidade de trabalhar com versões sob demanda utilizando o Docker, Vagrant ou o que achar melhor, é trabalhar com alguma das ferramentas abaixo: 

https://github.com/phpbrew/phpbrew
https://github.com/phpenv/phpenv

Dicas de Leitura
----------------

Os links mencionados são muito interessante, recomendo a leitura de todos. Segue abaixo um compilado de todos os links utilizados:
- http://www.newmediacampaigns.com/blog/docker-for-php-developers
- https://github.com/kost/docker-alpine/blob/master/alpine-apache-php/Dockerfile
- http://rberaldo.com.br/php-7-9-vezes-mais-rapido-que-php-5-6/
- http://php.net/manual/en/migration54.php
- http://php.net/manual/en/migration55.php
- http://php.net/manual/en/migration56.php
- http://php.net/manual/en/migration70.php
- http://php.net/supported-versions.php
- https://framework.zend.com/blog/zend-framework-1-12-12-released.html
- https://github.com/wimg/PHPCompatibility
- https://github.com/sstalle/php7cc
- http://www.phpit.com.br/artigos/o-que-ha-de-novo-no-php-5-4-conheca-as-mudancas-mais-significativas.phpit
- https://updraftplus.com/php-version-matter/
- https://hub.docker.com/_/php/
- https://github.com/kost/docker-alpine/tree/master/alpine-apache-php
- https://github.com/phpbrew/phpbrew
- https://github.com/phpenv/phpenv
