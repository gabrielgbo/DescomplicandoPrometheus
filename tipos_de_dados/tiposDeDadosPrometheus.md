## Tipos de Dados usados no Prometheus



#### **gauge: Medidor**
  - O tipo de dado `gauge` é o tipo de dado utilizado para criar métricas que podem ter seus valores alterados para cima ou para baixo, por exemplo, a ultilização de memória ou cpu. 
  Um exemplo de métrica do tipo `gauge` é a métrica `memory_usage`, que é uma métrica que mostra a utilização de memória.

      ```PROMQL
      memory_usage{instance="localhost:8899",job="Primeiro Exporter"}
      ```

&nbsp;

#### **counter: Contador**
  - O tipo de dado `counter` é o tipo de dado utilizado que vai ser incrementado no decorrer do tempo, por exemplo, quando eu quero contar os erros em uma aplicação no decorrer da última hora.
  O valor atual do `counter` quase nunca é importante, pois o que queremos dele são os valores durante uma janela de tempo, por exemplo, quantas vezes a minha aplicação falhou durante o final de semana.
  Normalmente as métricas `counter` possuem o sufixo `_total` para indicar que é o total de valores que foram contados, por exemplo:

    ```PROMQL
    requests_total{instance="localhost:8899",job="Primeiro Exporter"}
    ```

&nbsp;

#### **histogram: Histograma**
  - O tipo de dado `histogram` é o tipo de dado que te permite especificar o seu valor através de buckets predefinidos, por exemplo, o tempo de execução de uma aplicação. Com o `histogram` eu consigo contar todas as requisições que minha aplicação respondeu entre 0 e 0,5 segundos, ou então as requisições que tiveram respostas entre 1,0 e 2,5 e assim por diante.
  Por padrão, os buckets predefinidos são até no máximo 10 segundos, se você quiser mais, você pode criar seus próprios buckets personalizados.
  Um coisa super importante, o Prometheus irá contar cada item em cada bucket, e também a soma dos valores.
  Uma métrica do tipo `histogram` inclui alguns itens importantes são adicionados ao final do nome da métrica para indicar o tipo de dado e o tamanho do bucket, por exemplo:

    ```PROMQL
    requests_duration_seconds_bucket{le="0.5"}
    ```

    Onde `le` é o valor limite do bucket, o valor `0.5` indica que o valor do bucket é até 0,5 segundos, ou seja, aqui nesse bucket poderiam estar os requisições que tiveram respostas entre 0 e 0,5 segundos.

    O `_bucket` é um sufixo que indica que o valor é um bucket.

    Ainda temos alguns sufixos que são importantes e que podem ser úteis para nós:

    - _count: Contador
        - O sufixo `_count` indica que o valor é um contador, ou seja, o valor é incrementado a cada vez que a métrica é atualizada.

    - _sum: Soma
       - O sufixo `_sum` indica que o valor é uma soma, ou seja, o valor é somado a cada vez que a métrica é atualizada.

  - _bucket: Bucket
      - O sufixo `_bucket` indica que o valor é um bucket, ou seja, o valor é um bucket.

        O ponto alto do `histogram` é a excelente flexibilidades, pois percentuais e as janelas de tempos podem definidas durante a criação das queries, o ponto negativo é a precisão é um pouco inferior quando comparado com o `summary`.

&nbsp;

#### **summary: Resumo**
  - O tipo de dado `summary` é bem parecido com o `histogram`, com a diferença que os buckets, aqui chamados de `quantiles`, são definidos por um valor entre 0 e 1, ou seja, o valor do bucket é o valor que está entre os quantiles.

    Da mesma forma como no `histogram`, podemos criar métricas do tipo `summary` com alguns itens importantes adicionados ao final do nome da métrica, por exemplo:
  
    ```PROMQL
    requests_duration_seconds_sum{instance="localhost:8899",job="Primeiro Exporter"}
    ```

    Utilizamos o sufixo `_sum` indica que o valor é uma soma, ou seja, o valor é somado a cada vez que a métrica é atualizada e o sufixo `_count` para indicar que o valor é um contador, ou seja, o valor é incrementado a cada vez que a métrica é atualizada.

    O ponto alto do `summary` é a excelente precisão e o ponto baixo é a baixa flexibilidades, pois percentuais e as janelas de tempos precisam ser definidos durante a criação da métrica e não é possível agregar métricas do tipo `summary` com outras métricas do tipo `summary` durante a criação das queries.
