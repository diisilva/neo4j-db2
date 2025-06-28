
# README: Workflow Neo4j no Docker

Este documento foi desenvolvido no âmbito do Curso de Ciência da Computação da Universidade do Vale do Itajaí, sob a disciplina de Banco de Dados II, do Prof. Maurício Pasetto de Freitas. 
Os autores – Flávia Schnaider, Gregori da Luz Maciel, Miruna Onofrei, Joshua Leite e Diego Silva – apresentam um guia prático para implantação e uso do Neo4j, um Sistema Gerenciador de Banco de Dados NoSQL orientado a grafos.
Local: Itajaí | Ano: 2025. Siga a ordem:

## Requisitos

- **Sistema Operacional**: Ubuntu 22.04 LTS  
- **Docker**: Docker Engine e Docker CLI instalados 


## 1. Iniciar container

```bash
export NEO4J_PASSWORD=minhaSenhaSegura
docker compose up -d
```

## 2. Acessar o Cypher Shell

```bash
docker exec -it meu-neo4j bin/cypher-shell -u neo4j -p minhaSenhaSegura
```

## 3. Criar _schema_ (constraint e índice)

```cypher
// Constraint de unicidade em cpf
CREATE CONSTRAINT unicaPessoaCpf IF NOT EXISTS
  FOR (p:Pessoa)
  REQUIRE p.cpf IS UNIQUE;

// Índice em nome
CREATE INDEX idx_pessoa_nome IF NOT EXISTS
  FOR (p:Pessoa)
  ON (p.nome);
```

## 4. Operações CRUD

### CREATE

```cypher
CREATE
  (a:Pessoa {nome: 'Alice', cpf: '123'}),
  (b:Pessoa {nome: 'Bob',   cpf: '456'}),
  (a)-[:AMIGO_DE {desde: 2015}]->(b);
```

### READ

```cypher
MATCH (a:Pessoa {nome:'Alice'})-[:AMIGO_DE]->(amigo)
RETURN amigo.nome AS NomeDoAmigo;
```

### UPDATE

```cypher
MATCH (a:Pessoa {nome:'Alice'})
SET a.idade = 30;
```

### DELETE

```cypher
MATCH (b:Pessoa {nome:'Bob'})
DETACH DELETE b;
```

## 5. Verificações

```cypher
SHOW CONSTRAINTS;
SHOW INDEXES;
MATCH (p:Pessoa) RETURN count(p) AS TotalPessoas;
```
