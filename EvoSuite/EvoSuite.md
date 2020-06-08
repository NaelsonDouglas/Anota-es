# Fonte
 Este markdown é uma versão extremamente resumida do tutorial encontrado aqui
  * Todavia, introduzi algumas melhorias não existentes no tutorial oficial.

>http://www.evosuite.org/documentation/tutorial-part-1/

# Dependências
* Evosuite funciona APENAS com o jdk-8
```bash
ndc@PC:~$> java -version
openjdk version "1.8.0_252"
OpenJDK Runtime Environment (build 1.8.0_252-8u252-b09-1~19.10-b09)
OpenJDK 64-Bit Server VM (build 25.252-b09, mixed mode)
```
```bash
ndc@PC:~$> mvn -version
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: /opt/apache-maven-3.6.3
Java version: 1.8.0_252, vendor: Private Build, runtime: /usr/lib/jvm/java-8-openjdk-amd64/jre
Default locale: pt_BR, platform encoding: UTF-8
```
## IMPORTANTE!
 * O projeto DEVE ter o JUnit como dependência adicionada no pom.xml
   * Caso a dependência não seja adicionada, o ```mvn dependency:copy-dependencies``` não irá baixar os .jar necessários (JUnit e Hamcrest).

Esta dependência pode ser ao inserir o seguint xml no pom.xml do projeto:
```xml
<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
```

# Tutorial

### Compilar
Evosuite trabalha focado nos bytecodes e não nas classes .java. Então o primeiro passo para gerar os testes é compilar o código.

```bash
mvn compile
```
### Baixar os .jar
```bash
wget https://github.com/EvoSuite/evosuite/releases/download/v1.0.6/evosuite-1.0.6.jar
wget https://github.com/EvoSuite/evosuite/releases/download/v1.0.6/evosuite-standalone-runtime-1.0.6.jar
```

### Criar um ENV para facilitar as coisas
```bash
export EVOSUITE="java -jar $(pwd)/evosuite-1.0.6.jar"
```
* Na minha máquina eu salvei no .bashrc como um alias
```bash
 alias evosuite="java -jar $HOME/evosuite/evosuite-1.0.6.jar"
```

### Gerar testes

Para gerar os testes é necessário dizer duas coisas ao Evosuite
* Qual classe está sendo testada?
* Qual é o classpath do bytecode da classe a ser testada e a as dependências dele?

```bash
$EVOSUITE -class tutorial.Stack -projectCP target/classes
```
* Neste ponto deve ser criado a classe de teste e uma classe Test_scaffolding

# Executando os testes
## Baixando as dependências do projeto maven

Para compilar os testes são necessárias algumas dependências no classpath, as quais são:


>* **target/classes**: Diretório raíz onde estarão as classes;

>* **evosuite-standalone-runtime-1.0.6.jar**: Lib do runtime do Evosuite;

>* **evosuite-tests**: Diretório onde colocaremos as classes de teste;

>* **junit-4.12.jar** e **hamcrest-core-1.3.jar**: JUnit para poder executar os testes.
>   * Baixaremos estes .jar pelo Maven.

Como citado, precisamos baixar as dependências (hamcrest e JUnit). O Maven pode ajudar nisto com:
```bash
mvn dependency:copy-dependencies
```

## Criando um CLASSPATH para dizer ao java onde as dependências estão.
* O Evosuite precisa de um CLASSPATH que indica a localização das dependências. Este CLASSPATH pode ser salvo como uma variável de ambiente ou como um arquivo **evosuite.properties**

### CLASSPATH como variável de ambiente
* A variável pode ser gerada no seguinte formato:
```bash
export CLASSPATH=target/classes:evosuite-standalone-runtime-1.0.6.jar:evosuite-tests:target/dependency/junit-4.12.jar:target/dependency/hamcrest-core-1.3.jar

```
* Uma alternativa automatizada seria montar o CLASSPATH em partes utilizando o snipet abaixo.
  * No caso abaixo os .jar do evosuite foram salvos em ```$HOME/evosuite/```
  * Para baixa-los:
> ```wget https://github.com/EvoSuite/evosuite/releases/download/v1.0.6/evosuite-1.0.6.jar  ```
> 
> ```wget https://github.com/EvoSuite/evosuite/releases/download/v1.0.6/evosuite-standalone-runtime-1.0.6.jar```

```bash
export PROJECT_ROOT=$(pwd)
export JAVA_CLASSES_ROOT=$PROJECT_ROOT"/target/classes"
export EVOSUITERUNTIME="$HOME/evosuite/evosuite-standalone-runtime-1.0.6.jar"
export TESTS_OUTPUT=$PROJECT_ROOT"/evosuite-tests"
export JUNIT=$PROJECT_ROOT"/target/dependency/junit-4.12.jar"
export HAMCREST=$PROJECT_ROOT"/target/dependency/hamcrest-core-1.3.jar"

export CLASSPATH=$JAVA_CLASSES_ROOT:$EVOSUITERUNTIME:$TESTS_OUTPUT:$JUNIT:$HAMCREST:$PROJECT_ROOT
echo $CLASSPATH
```
### CLASSPATH como arquivo
* Podemos utilizar um arquivo **evosuite.properties** que pode ser gerado utilizando ```evosuite -setup target/classes```
* Isto irá gerar um arquivo template ```evosuite-files/evosuite.properties```, o qual pode ser editado a fim de inserir as dependências antes inseridas no CLASSPATH.
* Caso o seu projeto tenha um arquivo de propriedades, então é possível executar os testes sem definir o CLASSPATH, uma vez que o Evosuite automaticamente acessa o arquivo.
```bash
evosuite -class tutorial.Stack
```

## Compila
```bash
javac evosuite-tests/tutorial/*.java
```

## Executa
```bash
java org.junit.runner.JUnitCore tutorial.Stack_ESTest
```
