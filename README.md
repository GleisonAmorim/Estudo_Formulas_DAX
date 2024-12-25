
# Medidas e Funções Iteradoras no Power BI

Este repositório contém exemplos de medidas e funções iteradoras criadas no Power BI para análise de dados de transporte. As medidas ajudam a transformar dados brutos em insights valiosos e podem ser usadas em relatórios dinâmicos.

## Medidas Criadas

### 1. Percentual de Viagens no Prazo
Calcula o percentual de viagens entregues no prazo:

```DAX
%ViagensNoPrazo = [Viagens no prazo] / [QtdViagens]
```

**Explicação**: Divide as medidas `[Viagens no prazo]` e `[QtdViagens]` para obter o percentual.

---

### 2. Média de Receita por Viagem
Calcula a média do valor líquido do frete por viagem:

```DAX
MédiaReceitaPorViagem = AVERAGE(BaseTransporte[Valor do Frete Líquido])
```

**Explicação**: A função `AVERAGE` calcula a média dos valores na coluna.

---

### 3. Quantidade de Estados (UFs)
Conta quantos estados (UFs) diferentes aparecem na base:

```DAX
QtdUfs = DISTINCTCOUNT(BaseTransporte[UF])
```

**Explicação**: `DISTINCTCOUNT` conta apenas valores únicos.

---

### 4. Quantidade Total de Viagens
Conta o total de viagens registradas:

```DAX
QtdViagens = COUNTROWS(BaseTransporte)
```

**Explicação**: `COUNTROWS` conta o número total de linhas na tabela.

---

### 5. Total de Quilômetros Rodados
Soma o total de quilômetros registrados:

```DAX
TotalKm = SUM(BaseTransporte[Km])
```

**Explicação**: `SUM` soma todos os valores na coluna `Km`.

---

### 6. Viagens Entregues no Prazo
Conta as viagens entregues no prazo:

```DAX
Viagens no prazo = CALCULATE([QtdViagens], BaseTransporte[StatusEntrega] = "No prazo")
```

**Explicação**: `CALCULATE` aplica o filtro `StatusEntrega = "No prazo"` para contar apenas as viagens no prazo.

---

## Funções Iteradoras no Power BI

As funções iteradoras permitem cálculos linha por linha e retornam um resultado agregado. São úteis para criar métricas personalizadas.

### Exemplos de Medidas com Iteradoras

#### Receita RJ, SP e MG (IN)
Calcula a média do frete líquido para os estados RJ, SP e MG com entregas no prazo:

```DAX
Receita RJ e SP = 
    CALCULATE(
        AVERAGE(BaseTransporte[Valor do Frete Líquido]),
        BaseTransporte[UF] IN {"RJ", "SP", "MG"},
        BaseTransporte[StatusEntrega] = "No prazo"
    )
```

---

#### Cálculo Personalizado (Soma de Colunas)
Soma os valores de Combustível, Manutenção e Custos Fixos para cada linha:

```DAX
Calcular (soma) = 
    SUMX(
        BaseTransporte,
        BaseTransporte[Combustível] + BaseTransporte[Manutenção] + BaseTransporte[Custos Fixos]
    )
```

---

#### Cálculo com Filtros
Conta as linhas onde a marca é "Volkswagen":

```DAX
CALCULATE = 
    CALCULATE(
        COUNTROWS(BaseTransporte),
        BaseTransporte[Marca] = "Volkswagen"
    )
```

---

#### Cálculo com Filtros Adicionais
Aplica múltiplos filtros simultaneamente:

```DAX
CALCULATE com mais filtros = 
    CALCULATE(
        COUNTROWS(BaseTransporte),
        BaseTransporte[Marca] = "Volkswagen",
        BaseTransporte[StatusEntrega] = "Com atraso",
        ROUND(BaseTransporte[Combustível], 2) = 17.01
    )
```

---

## Outros Exemplos de Funções Iteradoras

- **Frete Bruto**: Calcula o frete bruto dividindo o valor líquido por 0.8:
    ```DAX
    FreteBruto = SUMX(BaseTransporte, BaseTransporte[Valor do Frete Líquido] / 0.8)
    ```

- **Frete Líquido**: Calcula o frete líquido multiplicando o valor por 0.8:
    ```DAX
    FreteLiquido = SUMX(BaseTransporte, BaseTransporte[Valor do Frete Líquido] * 0.8)
    ```

- **Valor Máximo**: Exibe o maior valor de frete bruto:
    ```DAX
    MAXIMO = MAXX(BaseTransporte, BaseTransporte[Valor do Frete Líquido] / 0.8)
    ```

- **Média de Custos Fixos**:
    ```DAX
    Média = AVERAGEX(BaseTransporte, BaseTransporte[Combustível] + BaseTransporte[Manutenção] + BaseTransporte[Custos Fixos])
    ```

- **Média de Custo de Combustível**:
    ```DAX
    MediaCustoCombustível = AVERAGEX(BaseTransporte, BaseTransporte[Combustível])
    ```

- **Valor Mínimo**:
    ```DAX
    mínimo = MINX(BaseTransporte, BaseTransporte[Valor do Frete Líquido] / 0.8)
    ```

- **Multiplicar e Arredondar**:
    ```DAX
    multiplicar e arredondar = SUMX(BaseTransporte, ROUND(BaseTransporte[Milhas] * 1.6, 2))
    ```

---

## Resumo

Estas medidas e funções iteradoras são projetadas para criar relatórios dinâmicos e explorar dados com flexibilidade. Utilize os exemplos para monitorar KPIs, analisar desempenho e tomar decisões baseadas em dados.
