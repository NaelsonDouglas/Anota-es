# Fonte
 Este markdown é uma versão extremamente resumida do tutorial encontrado aqui

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

```bash
mvn dependency:copy-dependencies
```

## Cria um CLASSPATH para dizer ao java onde as dependências estão.

```bash
export CLASSPATH=target/classes:evosuite-standalone-runtime-1.0.6.jar:evosuite-tests:target/dependency/junit-4.12.jar:target/dependency/hamcrest-core-1.3.jar

```


## Compila
```bash
javac evosuite-tests/tutorial/*.java
```

## Executa
```bash
java org.junit.runner.JUnitCore tutorial.Stack_ESTest
```
