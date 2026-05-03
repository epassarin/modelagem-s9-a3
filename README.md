# modelagem-s9-a3
Aula Modelagem Semana 9 A3

***

# 🧠 AULA COMPLETA – ATÉ 35 MINUTOS

## Cláusulas SELECT Avançadas

### Consultas Complexas com SQL

**Curso:** Técnico em Desenvolvimento de Sistemas  
**Unidade:** Cláusulas SELECT  
**Base:** Documento `[SIS]ANO2C4B2S9A3`

***

## 🎯 OBJETIVOS DA AULA (2 minutos)

***

> “Nesta aula, vamos avançar no uso do SQL, aprendendo a construir **consultas complexas**, que combinam JOINs, subconsultas, filtros, agregações e técnicas de otimização.  
> Essas consultas são exatamente as que aparecem em cenários reais de e‑commerce e sistemas corporativos.  
> Ao final, vocês não só entenderão a sintaxe, mas também a **lógica por trás das decisões**, tanto técnicas quanto profissionais.”

Objetivos técnicos:

*   Construir consultas SQL complexas
*   Combinar JOINs, GROUP BY, HAVING e subconsultas
*   Otimizar consultas para desempenho

Objetivos socioemocionais:

*   Resolver problemas sob pressão
*   Comunicar resultados técnicos de forma clara
*   Trabalhar de forma colaborativa

***

## 🔹 PARTE 1 – PONTO DE PARTIDA (7 minutos)

### CENÁRIO DO DOCUMENTO

A empresa possui 3 tabelas principais:

*   **Clientes**
*   **Pedidos**
*   **Produtos**

### PERGUNTAS-CHAVE (do material)

1.  Como relacionar Clientes, Pedidos e Produtos?
2.  Por que usar uma **tabela intermediária**?

***

### ✅ RESPOSTA EXPLICADA (script + conteúdo)

> “Em bancos de dados relacionais, raramente colocamos tudo em uma única tabela.  
> Para relacionar clientes, pedidos e produtos, usamos **chaves primárias e estrangeiras**.  
> Além disso, como um pedido pode ter vários produtos e um produto pode aparecer em vários pedidos, precisamos de uma **tabela intermediária**.”

### MODELO CORRETO DE RELACIONAMENTO

    Clientes (id_cliente)
           ↓
    Pedidos (id_pedido, id_cliente)
           ↓
    ItensPedidos (id_pedido, id_produto, quantidade, valor_unitario)
           ↓
    Produtos (id_produto)

### VANTAGENS DA TABELA INTERMEDIÁRIA (resposta oficial)

*   Permite relacionamento N:N
*   Armazena quantidade e preço no momento da compra
*   Facilita consultas complexas
*   Garante integridade referencial

✅ **Esta resposta resolve integralmente as questões do Slide 6 do material.**

***

## 🔹 PARTE 2 – CONSTRUINDO O CONCEITO: CONSULTAS COMPLEXAS (10 minutos)

### O QUE É UMA CONSULTA COMPLEXA?

> “Uma consulta complexa não é apenas uma consulta longa.  
> Ela é complexa porque envolve **lógica**, divisão do problema em partes e combinação de resultados.”

Normalmente envolve:

*   JOINs
*   Subconsultas
*   GROUP BY
*   HAVING
*   WHERE estratégico

***

### ✅ EXEMPLO CENTRAL DO DOCUMENTO

**Objetivo:**

> Recuperar o nome do cliente e a quantidade total de produtos comprados, **apenas para clientes que gastaram mais de R$ 500**.

```sql
SELECT c.nome,
       SUM(p.quantidade) AS total_produtos
FROM Clientes c
JOIN Pedidos pe ON c.id_cliente = pe.id_cliente
JOIN Produtos p ON pe.id_produto = p.id_produto
WHERE c.id_cliente IN (
    SELECT id_cliente
    FROM Pedidos
    GROUP BY id_cliente
    HAVING SUM(preco_total) > 500
)
GROUP BY c.nome;
```

### EXPLICAÇÃO PASSO A PASSO

1.  **Subconsulta (WHERE):**
    *   Identifica clientes que gastaram mais de R$ 500
2.  **Consulta principal:**
    *   Junta clientes, pedidos e produtos
3.  **GROUP BY:**
    *   Agrupa por cliente
4.  **SUM:**
    *   Calcula quantidade total de produtos

***
> “Percebam que resolvemos primeiro *quem* interessa e depois *o que* vamos mostrar. Essa é a essência de uma boa consulta complexa.”

***

## 🔹 PARTE 3 – CASOS DE USO AVANÇADOS (8 minutos)

### ✅ EXEMPLO 1 – MÉDIA DE VENDAS POR CATEGORIA

```sql
SELECT categoria,
       AVG(valor_venda) AS media_venda
FROM Produtos
WHERE categoria IN (
    SELECT categoria
    FROM Produtos
    GROUP BY categoria
    HAVING SUM(quantidade_vendida) > 100
)
GROUP BY categoria;
```

**Explicação**

*   Subconsulta filtra categorias relevantes
*   Consulta externa calcula média
*   Evita cálculos desnecessários

***

### ✅ EXEMPLO 2 – CLIENTES INATIVOS

```sql
SELECT c.id_cliente, c.nome
FROM Clientes c
LEFT JOIN Pedidos p
ON c.id_cliente = p.id_cliente
AND p.data_pedido > DATE_SUB(NOW(), INTERVAL 6 MONTH)
WHERE p.id_pedido IS NULL;
```

****

> “Aqui usamos LEFT JOIN para procurar ausências.  
> Quando o pedido não existe nos últimos seis meses, o valor vem como NULL — e é isso que filtramos.”

✅ Consulta correta e mais eficiente do que subconsulta.

***

## 🔹 PARTE 4 – OTIMIZAÇÃO DE CONSULTAS (5 minutos)

### BOAS PRÁTICAS (do documento)

✅ Criar índices

```sql
CREATE INDEX idx_funcionario ON Tarefas(id_funcionario);
```

✅ Evitar `SELECT *`

✅ Preferir JOINs a subconsultas

✅ Usar `EXPLAIN`

```sql
EXPLAIN
SELECT p.nome_projeto,
       COUNT(t.id_tarefa)
FROM Projetos p
JOIN Tarefas t ON p.id_projeto = t.id_projeto
WHERE t.status_tarefa = 'Completa'
GROUP BY p.nome_projeto;
```

**Script**

> “O EXPLAIN mostra como o banco pensa.  
> Ele é essencial para identificar gargalos de desempenho.”

***

## 🔹 PARTE 5 – ATIVIDADE SOCIOEMOCIONAL (Ser Sempre +) (5 minutos)

### SITUAÇÃO: CONFLITO EM PROJETO DE E‑COMMERCE

### ✅ RESPOSTAS ESPERADAS (TOTALMENTE DESENVOLVIDAS)

**Principais causas**

*   Mudança constante de prioridades
*   Falta de comunicação
*   Pressão por prazos

**Soluções propostas**

*   Reuniões diárias de alinhamento
*   Uso de ferramentas como Trello/Slack
*   Mediação de conflitos
*   Definição clara de responsabilidades
*   Pausas e cuidado com estresse

**fechamento**

> “Assim como em SQL, problemas grandes são resolvidos quando dividimos em partes menores e tratamos cada uma com clareza.”

***

## 🔹 PARTE 6 – FECHAMENTO: O QUE APRENDEMOS HOJE? (3 minutos)

### ✅ SÍNTESE (do próprio documento)

*   Aprendemos a construir consultas complexas
*   Entendemos a importância da indexação
*   Compreendemos o uso correto de JOINs, subconsultas e EXPLAIN
*   Desenvolvemos visão técnica e profissional

**final**

> “Consultas complexas não são apenas código.  
> Elas são ferramentas de tomada de decisão.  
> Saber escrever SQL eficiente é uma competência técnica e estratégica.”

***


