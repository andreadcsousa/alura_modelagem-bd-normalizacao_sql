# Modelagem de banco de dados relacional: normalização

Normalização de banco de dados, diretrizes de qualidade de um modelo relacional, análise de anomalias a serem evitadas em um banco de dados, dependências funcionais, projeto de normalização no Sheets.

1. [Normalização de Dados](#1-normalização-de-dados)
2. [Dependências, 1FN e 2FN](#2-dependências-1fn-e-2fn)
3. [3FN e Boyce-Codd](#3-3fn-e-boyce-codd)
4. [Quarta Forma Normal - 4FN](#4-quarta-forma-normal---4fn)
5. [Quinta Forma Normal - 5FN](#5-quinta-forma-normal---5fn)

Saiba mais sobre o curso [aqui](https://cursos.alura.com.br/course/modelagem-banco-dados-relacional-normalizacao) ou acompanhe minhas anotações abaixo. ⬇️

## 1. Normalização de Dados

### **Clube do Livro e Mormalização**

> O Clube do Livro está disponível agora no e-commerce. Além de livros, vende equipamentos de informática. Com essa expansão, o sistema e o banco de dados que foram feitos de forma básica, para que atendessem as demandas temporariamente, passaram a sofrer alguns problemas de armazenagem.

`Problema principal:`  
Espaço em disco cheio.

`Problema de manutenção:`  
Mistura e redundância de dados.

`Causa:`  
Anomalias de inserção, remoção e alteração tornam as tabelas do banco pesadas.

### **Diretrizes informais**

> Conjunto de regras que ajudam a identificar a qualidade de um projeto de banco de dados.

1. **Semântica clara com esquemas fáceis de explicar:** Compatibilidade na interpretação entre os atributos de uma mesma relação (cada esquema de relação deve representar um tipo de entidade ou de relacionamento);
2. **Evitar informações redundantes nas relações:** Tornar inexistentes as anomalias de inserção, remoção e alteração dos dados na atualização das relações no banco de dados;
3. **Impossibilitar valores NULL nas tuplas:** Valores nulos causam desperdício de espaço de armazenamento, gerando problemas de entendimento de significado dos atributos e do resultado de operações:  
    3.1. O atributo não se aplica;  
    3.2. O valor do atributo é desconhecido;  
    3.3. O valor do atributo está ausente (não foi registrado);
4. **Atenção ao surgimento de tuplas falsas:** Para que as operações de junção possam ser formuladas por meio de condições de igualdade definidas sobre atributos que sejam chaves primárias ou estrangeiras, deve-se impossibilitar valores ilegítimos ao se recompor relações decompostas, evitando perdas.

### **Anomalias**

> Anomalias são mudanças em dados que podem gerar Inconsistências dentro de um banco de dados relacionais. A inconsistência se refere a valores que deveriam ser iguais, mas que estão diferentes.

CPF    | nome_colaborador | end_colaborador | cod_depto | nome_depto
------ | ---------------- | --------------- | --------- | ----------
123456 | Marta            | Rua das Flores  | 15        | Tecnologia
147896 | Robledo          | Rua Pio XXI     | 23        | Vendas
965537 | Helena           | Rua ABC         | 15        | Tecnologia

- `Inserção` levam a repetição dos dados dentro do banco relacional (redundância)

A tabela acima contém dados que se repetem (redundância) e que na inserção de novos dados podem ocorrer entradas nulas (NULL), no caso de inserir colaborador sem departamento ou departamento sem colaborador.

Sendo a chave primária o CPF que corresponde ao colaborador, não ao departamento, o correto seria manter os dados do colaborador e criar uma nova tabela apenas para o departamento.

Definindo então uma chave estrangeira do código do depatarmento na tabela de colaborador.

- `Alteração` levam a inconsistências e exigem esforço maior para atualização dos dados

Ao alterar um departamento, teria que alterar todos os registros também, o que geraria retrabalho e uma inconsistência dentro do banco de dados.

- `Remoção` causa remoção de um dado necessário na tabela que não deveria ser removido

Ao remover um departamento ou colaborador, deve-se remover uma tupla inteira. Isso gera uma inconsistência de dados pela ausência (perda) deles no registro.

## 2. Dependências, 1FN e 2FN

### **Dependência funcional**

> Dependências Funcionais são restrições aplicadas sobre os atributos da tabela, ou seja, quando um atributo depende de outro para que a sua existência faça sentido. A dependência funcional estabelece uma relação de atributos na tabela. Por exemplo, a chave primária que é um ID e define outra coluna na tabela.

- Determinante: CPF
- Dependente: nome e endereço do colaborador

CPF    | nome_colaborador | end_colaborador
------ | ---------------- | --------------------
123456 | Marta            | Rua das Flores 
147896 | Robledo          | Rua Pio XXI    
965537 | Helena           | Rua ABC        
437627 | Marta            | Rua Floriano Peixoto

As formas normais satisfazem as propriedades de uma dependência, então:

- A `primeira forma normal` deve satisfazer as propriedades baseadas na dependência funcional.
- A `segunda forma normal` deve satisfazer as propriedades baseadas na dependência funcional parcial.
- A `terceira forma normal` deve satisfazer as propriedades baseadas na dependência transitiva.
- A `forma normal de Boyce-Codd` deve satisfazer as propriedades baseadas na dependência funcional trivial.
- A `quarta forma normal` deve satisfazer as propriedades baseadas na dependência Multivalorada.
- A `quinta forma normal` deve satisfazer as propriedades baseadas na dependência junção.

### **Primeira Forma Normal - 1FN**

> Uma tabela está na 1FN quando ela não possui: Atributos multivalorados e atributos compostos.

Quais cuidados devem ser tomados para que uma tabela esteja na primeira forma normal?

    Evitar ter mais de um assunto em uma tabela;  
    Não admitir repetições ou campos que tenham mais de um valor.

Procedimentos para aplicar a regra da 1FN

    Identificar a chave primária da tabela;  
    Identificar o grupo repetitivo e removê-lo da tabela.

> O objetivo da primeira forma normal é eliminar o aninhamento de tabelas para que cada tabela tenha informações de um único assunto e sem campos com valores multivalorados.

### **Dependência funcional parcial e total**

Na `dependência funcional parcial` os atributos que não possuem chave dependem de parte da chave primária. No exemplo abaixo, C depende de A e B para existir.

    (A) cod_colaborador 🔑 | (B) cod_projeto 🔑 | (C) qtd_horas_trabalhadas

Na `dependência funcional total` os atributos que não possuem chave dependem totalmente da chave primária. No exemplo abaixo, C depende apenas de B para existir.

    (A) cod_colaborador 🔑 | (B) cod_projeto 🔑 | (C) nome_projeto

### **Segunda Forma Normal - 2FN**

> Uma tabela está na 2FN quando ela já está a 1FN e quando não existe dependência funcional parcial.

Quando uma tabela está na segunda forma normal?

    Quando ela já está na primeira forma normal;  
    Quando os atributos que não possuem chave, dependem da chave composta em sua totalidade.

Procedimentos para aplicar a regra da 2FN

    Identificar se a tabela tem chave primária composta;  
    Identificr os atributos que dependem parcialmente dessa chave e criar uma nova tabela com eles.

Esse é o momento de dividir a tabela em duas, aplicando as regras e separando o que está relacionado a cada uma. Mantendo a chave estrangeira que irá relacionar as duas tabelas.

***Tabela de ALOCAÇÃO:***

    cod_colaborador 🔑 | cod_projeto 🔑 | qtd_horas_trabalhadas

***Tabela de PROJETO:***

    cod_projeto 🔑 | nome_projeto

## 3. 3FN e Boyce-Codd

### **Dependência transitiva**

> A Dependência Funcional Transitiva ocorre quando um atributo não-chave não depende da chave primária, nem parcialmente, mas depende de outro atributo não-chave.

Um atributo é dependente funcional transitivo quando ele é dependente de outro atributo (que não é a chave primária) e este é dependente da chave primária.

    (A) cod_colaborador 🔑 | (B) nome_colaborador | (C) adm_colaborador |  (D) col_projeto | (E) fim_projeto

No exemplo acima, a data de término do projeto (E) depende apenas do colaborador do projeto (D) e este do código do colaborador (A).

***Tabela de COLABORADOR:***

    cod_colaborador 🔑 | nome_colaborador | adm_colaborador | col_projeto

***Tabela de PROJETO:***

    col_projeto 🔑 | fim_projeto

### **Dependência Transitiva X Parcial**

> Quando há `dependência transitiva`, pelo menos um atributo depende de outro que não seja chave primária.
>
> Quando há `dependência parcial`, os atributos dependem parcialmente da chave primária composta.

### **Terceira Forma Normal - 3FN**

> Uma tabela está na 3FN quando ela já está a 2FN e quando nenhuma coluna possui dependência transitiva em relação a outra coluna que não participe da chave primária.

Quando uma tabela está na terceira forma normal?

    Quando ela já estiver na segunda forma normal;  
    Quando não existir dependência funcional transitiva entre atributos não chave.

Procedimentos para aplicar a regra da 3FN

    Identificar todos os atributos que são funcionalmente dependentes de outros atributos não chave e removê-los.

### **Aplicando a 3FN**

Dada a tabela abaixo, para aplicar a terceira forma normal, deve-se:

Numero_pedido | Codigo_produto | Quantidade | Valor_unitario | Subtotal
------------- | -------------- | ---------- | -------------- | --------
0001          | 12348          | 5          | 350,00         | 1.750,00
0002          | 58269          | 3          | 350,00         | 765,00
0003          | 45623          | 1          | 350,00         | 110,00
0004          | 75264          | 4          | 350,00         | 580,00
0005          | 88526          | 5          | 350,00         | 2.000,00

Remover o atributo subtotal, pois ele depende de outros atributos não-chave da tabela. O subtotal é resultado da multiplicação dos atributos Quantidade X Valor_unitario.

### **Boyce-Codd**

> Uma tabela de banco de dados está no BCNF se, e somente se, não houver dependências funcionais não triviais de atributos em algo que não seja um superconjunto de uma chave candidata.

Quando uma tabela está na Boyce-Codd forma normal?

    Quando ela já estver na terceira forma normal;  
    Todos os dados dependem apenas da chave primária e não de outro campo da tabela.

Procedimentos para aplicar a regra da FNBC

    Identificar todos os atributos que sejam determinados por outro atributo que não seja uma chave candidata,removê-los e levar para outra tabela.

- `Dependência funcional não trivial` leva em consideração todas as chaves candidatas em uma relação
- `Superconjunto de uma chave candidata` atributo determinado por outros que não sejam chave candidata

> Para deixar uma tabela na FNBC podemos aplicar o conceito de decomposição sem perdas. Uma decomposição é sem perdas se for possível reconstruir a tabela original fazendo a junção das novas tabelas criadas.

## 4. Quarta Forma Normal - 4FN

### **Dependência multivalorada**

A dependência multivalorada é uma extensão da dependência funcional. Visto que na dependência funcional o valor de uma atributo determina o valor de outro atributo. Na dependência multivalorada, o valor de um atributo determina um conjunto de valores de outro atributo.

- A dependência funcional é representada por `X -> Y` (X determina Y)
- A dependência multivalorada é representada por `X ->> Y` (X determina múltiplos Y)

***Exemplo de dependência funcional:***

    CPF -> Nome (um nome por cpf)

***Exemplo de dependência multivalorada:***

    CPF ->> Nome Dependente (pode ter vários dependentes por pessoa)
    CPF ->> Idade Dependente (dependentes têm idades diferentes)

### **Quarta Forma Normal - 4FN**

> Uma tabela está na 4FN quando ela já está a 3FN e quando não pode existir dependência multivalorada.

Quando uma tabela está na quarta forma normal?

    Quando já está na terceira forma normal;  
    Quando não possui dependência multivalorada.

Procedimentos para aplicar a regra da FNBC

    Identificar se existe um atributo multi determinante que aponte para mais de um multi dependente;  
    Identificar se existe independência entre esses multi dependentes.

Na tabela abaixo é possível identificar que o CPF é um atributo independente, pois não depende do código do fornecedor, nem do código do produto.

cod_fornecedor | cod_produto | cpf
-------------- | ----------- | ------
2              | 7           | 315264
1              | 1           | 842679
4              | 5           | 214786
5              | 4           | 975631
1              | 1           | 214786

Para normalizar na quarta forma, pode-se dividir a tabela em duas, com ambas herdando o código do fornecedor:

***Tabela de produto:***

    cod_fornecedor 🔑 | cod_produto

***Tabela de cpf:***

    cod_fornecedor 🔑 | cpf

## 5. Quinta Forma Normal - 5FN

### **Dependência de junção**

> A dependência de junção utiliza os conceitos de decomposição sem perdas da 4FN e de junção. As tabelas podem ser decompostas em mais de duas, sem perda de informação (denominado tabela n-decomponível, onde “n” é maior que dois). - [Bráulio Ferreira de Carvalho, Devmedia](https://www.devmedia.com.br/artigo-sql-magazine-7-formas-normais-superiores/7474)

***Importante saber:***

A dependência de junção não existe quando não temos a restrição cíclica. E é cíclico quando todos os passos se conectam, por exemplo:

- Se a loja A vende o carro B;
- A loja A representa a montadora C;
- E a montadora C fabrica o carro B;
- Então a loja A vende o carro B da montadora C.

> A dependência de junção ocorre quando realizamos a decomposição de uma tabela em outras três relações. Tais relações são novamente compostas, caso não sejam geradas tuplas falsas, isto é, quando os dados originais são preservados na tabela.

### **Quinta Forma Normal - 5FN**

> Uma tabela está na 5FN quando ela já está a 4FN e quando não há dependência de junção.

Quando uma tabela está na quinta forma normal?

    Quando já está na quarta forma normal;  
    Quando não há decomposição sem perda, nem de junção.

Procedimentos para aplicar a regra da FNBC

    Identificar se existe dependência de junção;  
    Reconstruir a partir de informações menores combinadas.

>Para aplicarmos a quinta forma normal, precisamos conhecer os conceitos de decomposição sem perdas e de junção. Levando assim a ser conhecida como a forma normal de projeção-junção.

⬆️ [Voltar ao topo](#modelagem-de-banco-de-dados-relacional-normalização) ⬆️