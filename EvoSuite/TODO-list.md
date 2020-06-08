# Anotações feitas durante a leitura do artigo base do Evosuite
* https://dl.acm.org/doi/pdf/10.1145/2025113.2025179

# Pergunta científica em mente durante a leitura:
>### Qual o comportamento dos mantenedores de repositórios diante de pull requests com testes unitários gerados por geradores automáticos de testes? Em quais cenários este comportamento muda? E como ele muda?

### Alguns dos possíveis cenários (clusterização dos experimentos):
 * Repositórios previamente pouco ou muito testados.
 * Quantidade de contrubuidores.
 * Idade do repositório.
 * Quantidade de commits?
 * Quem mantém? (fundação, empresa, pessoa)
 * Área da aplicação? (Ciêncida de dados, desenvolvimento web, etc...)

# Idéias aplicáveis à dissertação
 * Evosuite utiliza mutação baseada na cobertura da suite de testes. As suites mutam de forma a elevar a cobertura.
   * Proposta: Pegar repositórios com cobertura parcial e "completar" eles com Evosuite. Segundo a documentação do Evosuite, é possível "importar" testes pre-existentes do JUnit em uma suite do Evosuite. (checar como fazer isto vai no TODOLIST abaixo)
     * Qual seria a taxa de aceitação destes PRs "completadores"?
       * **Hipótese:** Esperasse que repositórios com taxas mais elevadas de compleção sejam repositórios melhor cuidados, uma vez que existe aí um indício de que "alguém está testando".
       * **Hipótese:** Repositórios com poucos ou nenhum testes dão indício de que não ligam para os testes, logo estes aí seriam mais propensos a aceitar "qualquer coisa".
 * _EvoSuite can be used as a command line tool that produces test suites for individual classes or entire packages, and produces HTML reports for further analysis._
   * A inclusão destes relatórios agradaria os DEVS?
     * Como criar eles vai no TODOLIST
 * Finally, EvoSuite can also be used through a webservice,allowing simple experimentation without any installation.
   * Dá para fazer muita coisa com isto. Inclusive serviços pagos.
 * Na última aula de testes de SW (que também foi a primeira de todas), foi citado que cobertura de código é **uma das** métricas existentes.
   * Evosuite utiliza cobertura de código para guiar a evolução das suites de testes.
   * Pesquisar outras métricas existentes.
     * E se a ferramenta utilizasse outra métrica ao invés de cobertura de código?
       * Melhor desempenho na geração dos testes?
       * Melhor alguma outra coisa?
___
-[ ] Conferir como importar testes existentes no Evosuite.
 * Tem algo sobre aqui: http://www.evosuite.org/documentation/tutorial-part-2/

-[ ] Conferir como que gera os relatórios de teste em HTML

-[ ] Conferir os tópicos citados no artigo
>EvoSuite uses a search-based approach integrating state-of-the-art techniques such as for example 

* dynamic symbolic execution [7] 
* and testability transformation [8]
* hybrid search [9]

>[7] P. Godefroid, N. Klarlund, and K. Sen. DART:
directed automated random testing. In PLDI’05:
Proc. of the 2005 ACM SIGPLAN Conference on
Programming Language Design and Implementation,
pages 213–223, New York, USA, 2005. ACM.

>[8] M. Harman, L. Hu, R. Hierons, J. Wegener,
H. Sthamer, A. Baresel, and M. Roper. Testability
transformation. IEEE Transactions on Software
Engineering, 30(1):3–16, 2004.

>[9] M. Harman and P. McMinn. A theoretical and
empirical study of search based testing: Local, global
and hybrid search. IEEE Transactions on Software
Engineering, 36(2):226–247, 2010
___
