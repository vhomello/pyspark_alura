# Input e Output (io)

## Exite outras formas de passar os parametros de leitura
```python
spark.read.option("header", True).option("sep", ";").csv(path)

spark.read.options(header=True, sep=";").csv(path)
```

## Formatos de Data aceito no dateFormat
| Component | Pattern | Example |
| :--- | :--- | :--- |
| Day | dd | "01, 15, 31" |
| Month (Number) | MM | "01, 12" |
| Month (Abbrev) | MMM | "Jan, Dec" |
| Year (4-digit) | yyyy | 2024 |
| Year (2-digit) | yy | 24 |
| Hour (24h) | HH | "13, 23" |
| Minutes | mm | "05, 59" |

## Mais detalhes sobre os tipos de dados no PySpark
O PySpark (e o Spark em geral) possui seu próprio sistema de tipos, localizado em `pyspark.sql.types`. É importante conhecer esses tipos para definir schemas corretamente.

### Tipos Primitivos Comuns
- **StringType**: Textos.
- **IntegerType**: Números inteiros (32-bit).
- **LongType**: Números inteiros longos (64-bit).
- **DoubleType**: Números decimais de ponto flutuante (dupla precisão).
- **BooleanType**: Verdadeiro ou Falso.
- **DateType**: Datas (sem hora).
- **TimestampType**: Data e Hora.

### Tipos Complexos
- **ArrayType**: Lista de elementos do mesmo tipo.
- **MapType**: Pares chave-valor.
- **StructType**: Estrutura aninhada (objeto), similar a uma linha de tabela ou um JSON aninhado.

## Modes de escrita
- append: adiciona os dados ao arquivo
- overwrite: sobreescreve o arquivo
- ignore: ignora a escrita se o arquivo já existir
- error (default): lança um erro se o arquivo já existir

## Convertendo Para Pandas

É possível converter um DataFrame Spark para Pandas usando `.toPandas()`.

### Impacto em Ambientes Distribuídos

Quando você executa `.toPandas()`, o Spark **coleta todos os dados** de todos os nós do cluster e os envia para a memória do **Driver** (o computador que está rodando o script principal).

**Impactos:**
1. **Estouro de Memória (OOM)**: Se o dataset for maior que a memória RAM do Driver, o processo vai falhar
2. **Gargalo de Rede**: Transferir terabytes de dados pela rede para uma única máquina é lento e ineficiente.

Use `.toPandas()` apenas para dados pequenos e agregados (ex: resultado final de um relatório), nunca para o dataset inteiro
