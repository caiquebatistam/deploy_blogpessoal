<h1>Deploy do Back-end Spring no Heroku</h1>



Siga as etapas abaixo:

1. <a href="#cfhrk">Criar conta grátis no Heroku</a>

2. <a href="#node">Instalar o Node no seu computador</a>

3. <a href="#hrkcli">Instalar o Heroku Client</a>

4. <a href="#sprop">Criar o arquivo <code>system.properties</code> no seu projeto</a>

5. <a href="#pom01">Configurar a versão do Java no arquivo <code>pom.xml</code> do seu projeto</a>

6. <a href="#pom02">Trocar a dependência do MySQL pela dependência do PostgreSQL no arquivo <code>pom.xml</code> do seu projeto</a>

7. <a href="#approp">Configurar a conexão com o Banco de Dados no arquivo <code>application.properties</code> do seu projeto</a>

8. <a href="#user">Criar um usuário padrão em memória</a>

9. <a href="#home">Criar uma rota padrão para o Swagger no seu projeto</a>

10. <a href="#git">Preparar o seu projeto para o Deploy com o Git</a>

11. <a href="#login">Fazer login no Heroku</a>

12. <a href="#projeto">Criar um novo projeto no Heroku</a>

13. <a href="#postgre">Adicionar o Banco de dados (PostgreSQL) no Heroku</a>

14. <a href="#deploy">Efetuar o Deploy</a>

15. <a href="#testar">Testar o link e a API</a>

16. <a href="#update">Atualizar o Deploy</a>

    

<h2 id="cfhrk">Criar uma conta grátis no Heroku</h2>



O **Heroku é** uma  plataforma na nuvem que permite efetuar o Deploy (em termos práticos, significa colocar no ar uma aplicação que teve seu desenvolvimento concluído) de várias aplicações Back End seja para  hospedagem, testes em produção ou escalar as suas aplicações. 

O grande diferencial do Heroku são as contas gratuitas, que permitem implantar até 5 Aplicações na mesma conta com Banco de Dados PostgreSQL incluso. 

1) Acesse o endereço: **https://www.heroku.com**

<div align="center"><img src="https://i.imgur.com/9lFOzru.png" title="source: imgur.com" /></div>

2) Crie a sua conta grátis no Heroku clicando no botão **SIGN UP FOR FREE** e siga as instruções.



<h2 id="node">Instalação do Node.js</h2>



O Node.js pode ser definido como um **ambiente de execução Javascript \*server-side\***. Isso significa que com o Node.js é possível criar aplicações Javascript para rodar como uma aplicação local em uma máquina, não dependendo de um browser para a execução, como estamos acostumados.

No Módulo 03 do Bootcamp (Front End), o Node é ferramenta indispensável para criar projetos no Angular.

1) Acesse o endereço: **https://nodejs.org/en/**

<div align="center"><img src="https://i.imgur.com/8xKcp6h.png" title="source: imgur.com" /></div>

2) Faça o download da versão 14 do Node.js e instale no seu computador.

Em caso de dúvidas, acesse o Guia de instalação do Node.js clicando aqui



<h2 id="hrkcli">Instalação do Heroku Client</h2>



O Heroku Client é uma Interface de linha de comando (CLI) do Heroku, que facilita a criação e o gerenciamento de seus aplicativos Heroku diretamente do terminal. É uma parte essencial do uso do Heroku. 

Para instalar e executar os comandos do Heroku Client usaremos o Prompt de comando do Windows. 

1) Para instalar, execute o atalho <img width="80" src="https://i.imgur.com/JpqKaVh.png" title="source: imgur.com" /> para abrir a janela Executar

<div align="center"><img src="https://i.imgur.com/ISBwaaK.png" title="source: imgur.com" /></div>

2) Digite o comando **cmd** para abrir o **Prompt de comando do Windows**

3) No Prompt de comando do Windows, se você estiver usando o **STS** digite o comando: ***cd Documents\workspace-spring-tool-suite-4-4.10.0.RELEASE***  (a versão pode ser diferente de 4-4.10.0). Se você estiver utilizando o **Eclipse**, digite o comando ***cd eclipse-workspace***.

4) Antes de instalar o **Heroku Client**, verifique se o Node já está instalado através do comando: ***npm -version*** ou ***npm -v*** (A versão pode ser diferente da imagem)

<div align="center"><img src="https://i.imgur.com/sfHThTC.png" title="source: imgur.com" /></div>

5) Para instalar o **Heroku Client** digite o comando: ***npm i -g heroku*** 

<div align="center"><img src="https://i.imgur.com/rcsDAZ0.png" title="source: imgur.com" /></div>

6) Confirme a instalação do Heroku Client através do comando: ***heroku version*** (A versão pode ser diferente da imagem)

<div align="center"><img src="https://i.imgur.com/QAtGCn7.png" title="source: imgur.com" /></div>



<div align="center"> <h1>*** Importante ***</h1></div>

<div align="justify"><b>Até este ponto fizemos apenas configurações no nosso Sistema Operacional para trabalhar com o Heroku. Agora faremos ajustes no nosso projeto Blog Pessoal para efetuar o Deploy da API. Após realizarmos todas as alterações, o seu projeto não executará mais localmente. Para rodar localmente, será necessário desfazer as configurações dos itens 5 e 6 (arquivo pom.xml), e 7 (arquivo application.properties).</b></div>



<h2 id="sprop"> Criação do arquivo system.properties</h2>

1) Na raiz do seu projeto (em nosso exemplo, na pasta blogpessoal), crie o arquivo **system.properties**.

<div align="center"><img src="https://i.imgur.com/G70Oset.png" title="source: imgur.com" /></div>

2) No lado esquerdo superior, na Guia **Package explorer**, na pasta do projeto, clique com o botão direito do mouse e clique na opção **New->File**.

3) Em **File name**, digite o nome do arquivo (**system.properties**) e clique no botão **Finish**.

<div align="center"><img src="https://i.imgur.com/gffhZoF.png" title="source: imgur.com" /></div>

4) No arquivo **system.properties** indique a versão do Java que será utilizada pela API no Heroku:

```properties
java.runtime.version=15
```


<h2 id="pom01"> Configuração da versão do Java no arquivo pom.xml</h2>

No arquivo, **pom.xml**, vamos alterar as linhas:

```xml
<properties>
	<java.version>16</java.version>
</properties>
```

Para:

```xml
<properties>
	<java.version>15</java.version>
</properties>
```



<h2 id="pom02"> Configuração do PostgreSQL no arquivo pom.xml</h2>


No arquivo, **pom.xml**, vamos alterar as linhas:

```xml
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
	<scope>runtime</scope>
</dependency>
```

Para:

```xml
<!-- Dependência do MySQL -->
<!-- <dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
	<scope>runtime</scope>
</dependency>-->

<!-- Dependência PostgreSQL -->
 <dependency>
	<groupId>org.postgresql</groupId>
	<artifactId>postgresql</artifactId>
</dependency> 

```



<h2 id="approp"> Configuração do Banco de Dados no arquivo application.properties</h2>


No arquivo, **application.properties**, vamos alterar as linhas:

```properties
spring.jpa.hibernate.ddl-auto=update
spring.jpa.database=mysql
spring.datasource.url=jdbc:mysql://localhost/db_blogpessoal?createDatabaseIfNotExist=true&serverTimezone=America/Sao_Paulo&useSSl=false
spring.datasource.username=root
spring.datasource.password=root
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL8Dialect

spring.jpa.show-sql=true

spring.jackson.date-format=yyyy-MM-dd HH:mm:ss
spring.jackson.time-zone=Brazil/East
```

Para:

```properties
#Configuração MySQL
#spring.jpa.hibernate.ddl-auto=update
#spring.jpa.database=mysql
#spring.datasource.url=jdbc:mysql://localhost/db_blogpessoal?createDatabaseIfNotExist=true&serverTimezone=America/Sao_Paulo&useSSl=false
#spring.datasource.username=root
#spring.datasource.password=root
#spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL8Dialect

#spring.jpa.show-sql=true

#Configuração PostgreSQL
spring.jpa.generate-ddl=true
spring.datasource.url=${JDBC_DATASOURCE_URL}
spring.jpa.show-sql=true

spring.jackson.date-format=yyyy-MM-dd HH:mm:ss
spring.jackson.time-zone=Brazil/East
```



<h2 id="user">Criação do usuário em memória</h2>



Vamos criar um usuário padrão em memória para simplificar o acesso a nossa API. O usuário em memória é um usuário para testes, que dispensa o cadastro no Banco de Dados. Em produção é altamente recomendado que este usuário seja desabilitado.

Na camada Security, abra o arquivo **BasicSecurityConfig**

Vamos alterar o método **protected void configure(AuthenticationManagerBuilder auth) throws Exception** de:

```java
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userDetailsService);	
    }
```

Para:

```java
	@Override
	protected void configure(AuthenticationManagerBuilder auth) throws Exception {
		auth.userDetailsService(userDetailsService);
		
		auth.inMemoryAuthentication()
		.withUser("root")
		.password(passwordEncoder().encode("root"))
		.authorities("ROLE_USER");
	
	}
```



<h2 id="home">Criar uma rota padrão para o Swagger</h2>



Vamos configurar o **Swagger** como página inicial da nossa API, ou seja, ao digitarmos o endereço principal da nossa API, ao invés de abrir a página abaixo:

<div align="center"><img src="https://i.imgur.com/wE5e4hZ.png" title="source: imgur.com" /></div>

Abriremos a página do Swagger:

<div align="center"><img src="https://i.imgur.com/RgYPeBg.png" title="source: imgur.com" /></div>



Na camada principal do projeto (**br.org.generation.blogpessoal**) vamos abrir o arquivo **BlogpessoalApplication**

Vamos alterar o arquivo de:

```java
package br.org.generation.blogpessoal;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class BlogpessoalApplication {

	public static void main(String[] args) {
		SpringApplication.run(BlogpessoalApplication.class, args);
	}

}

```

Para:

```java
package br.org.generation.blogpessoal;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.servlet.ModelAndView;

@SpringBootApplication
@RestController
@RequestMapping("/")
public class BlogpessoalApplication {

	@GetMapping
	public ModelAndView swaggerUi() {
		
		return new ModelAndView("redirect:/swagger-ui/");
		
	}
	
	public static void main(String[] args) {
		SpringApplication.run(BlogpessoalApplication.class, args);
	}

}

```

As alterações acima transformam a classe principal da nossa API (**BlogpessoalApplication**) em uma classe do tipo **RestController**, que responderá à todas as requisições do tipo **Get** para o **endpoint "/"** (raiz do nosso projeto) e fará o redirecionamento para a página inicial do Swagger, ou seja, o Swagger será a página home do nosso projeto.



<h2 id="git">Deploy com o Git</h2>



Vamos preparar o nosso repositório local para subir a aplicação para o Heroku utilizando o Git. Para realizar estas operações, utilizaremos o **Prompt de Comandos do Windows**.

1) Para abrir o Prompt de Comando, execute o atalho <img width="80" src="https://i.imgur.com/JpqKaVh.png" title="source: imgur.com" /> para abrir a janela Executar

<div align="center"><img src="https://i.imgur.com/ISBwaaK.png" title="source: imgur.com" /></div>

2) Digite o comando ***cmd***

3) No Prompt de comando do Windows, se você estiver usando o STS digite o comando: ***cd Documents\workspace-spring-tool-suite-4-4.10.0.RELEASE***  (a versão pode ser diferente de 4-4.10.0). Se você estiver utilizando o Eclipse, digite o comando ***cd eclipse-workspace***.

4) Acesse a pasta da API: ***cd blogpessoal*** (o nome da pasta pode ser diferente)

5) Digite a sequência de comandos abaixo:

***git init***
***git add .***
***git commit -m “Deploy inicial - Blog Pessoal”***



<h2 id="login">Login no Heroku</h2>



1) Digite o comando: ***heroku login***

<div><img src="https://i.imgur.com/DWeaqfP.png" title="source: imgur.com" /></div>

2) Será aberta a janela abaixo. Clique no botão **Log in**

<div align="center"><img src="https://i.imgur.com/PXR6hFW.png" title="source: imgur.com" /></div>

3) Após efetuar o login na sua conta, será exibida a janela abaixo. 

<div align="center"><img src="https://i.imgur.com/i6VMoMp.png" title="source: imgur.com" /></div>

4) Volte para o Prompt de comando para continuar o Deploy.

<div align="center"><img src="https://i.imgur.com/2CRuGnz.png" title="source: imgur.com" /></div>



<h2 id="projeto">Criar um novo projeto no Heroku</h2>



Para criar um novo projeto na sua conta do Heroku, digite o comando: ***heroku create nomedoprojeto***

<div align="center"> <h1>*** Importante ***</h1></div>
**O NOME DO PROJETO NÃO PODE TER LETRAS MAIUSCULAS, NUMEROS OU CARACTERES ESPECIAIS. ALÉM DISSO ELE PRECISA SER UNICO DENTRO DA PLATAFORMA HEROKU.**

Se o nome escolhido já existir, será exibida a mensagem abaixo:

<div><img src="https://i.imgur.com/L7ayFaz.png" title="source: imgur.com" /></div>

Se o nome escolhido for aceito, será exibida a mensagem abaixo:

<div><img src="https://i.imgur.com/thMnmlP.png" title="source: imgur.com" /></div>



<h2 id="postgre">Adicionar o Banco de dados (PostgreSQL) no Heroku</h2>



Para adicionar um Banco de Dados PostgreSQL no seu projeto, digite o comando: ***heroku addons:create heroku-postgresql:hobby-dev***

<div><img src="https://i.imgur.com/rPIrhXs.png" title="source: imgur.com" /></div>



<h2 id="deploy">Efetuar o Deploy</h2>



Para concluir o Deploy, digite o comando: ***git push heroku master***

Se tudo deu certo, será exibida a mensagem **BUILD SUCESS** (destacado em verde na imagem) e será exibido o endereço (**https://nomedoprojeto.heroku.com**) para acessar a API na Internet (destacado em amarelo na imagem)

<div align="center"><img src="https://i.imgur.com/gEUe301.png" title="source: imgur.com" /></div>



<h2 id="testar">Testar o link e a API</h2>



1) Abra o navegador e digite o endereço a sua API

2) Será solicitado o usuário e a senha. Digite **root** para ambos.

3) Sua API abrirá o Swagger. 

<div align="center"><img src="https://i.imgur.com/VYkEvqP.png" title="source: imgur.com" /></div>

4) Faça alguns testes via Swagger para certificar-se de que tudo está funcionando



<h2 id="update">Atualizar o Deploy</h2>



Uma vez que o Deploy foi feito, assim como no Github, basta efetuar a sequência de comandos abaixo na pasta do projeto:

***git add .***
***git commit -m “Atualização do Deploy - Blog Pessoal”***
***git push heroku master***

