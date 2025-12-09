# Aplicação: Sistema de Controle de Garantia de Equipamentos

## Descrição Geral

O problema proposto é recorrente: usuários perdem notas fiscais e certificados de garantia.
A aplicação deverá permitir o cadastro de equipamentos, armazenamento dos documentos de
compra e aviso sobre o vencimento da garantia.
---

## Estrutura do Projeto

```
ed005_garantia_Weverton/
│
├── sql/
│   ├── schema.sql          
│   ├── inserts.sql         
│
├── src/
│   ├── main.py             
│   ├── database.py        
│   ├── models/
│   │   ├── equipamento.py
│   │   ├── garantia.py
│   │   ├── loja.py
│   │   ├── documentos.py
│   │   ├── usuarios.py
├── prints/
│   ├── modelo_logico.png           # Diagrama lógico do banco
│   ├── consultas_dbeaver.png       # Resultado da execução no DBeaver
│   ├── execucao_terminal.png       # Evidência de execução no terminal
│
└── README.md
```

---


### Entidades Principais

| Entidade | Descrição | Atributos principais |
|-----------|------------|----------------------|
| **Loja** | Representa uma loja de vendas | `id_loja`, `nome`, `cnpj`, `endereco`, `telefone` |
| **Equipamento** | Produto vendido pela loja | `id_equipamento`, `nome`, `preco`, `data_venda`, `id_loja` |
| **Garantia** | Período de cobertura do equipamento | `id_garantia`, `data_inicio`, `data_fim`, `tipo`, `id_equipamento` |
| **Documentos** | Nota fiscal associada a uma venda | `id_doc`, `numero_nota` |
| **Usuários** | Pessoa ou cliente do sistema | `id_usuario`, `cpf_usuario` |

---

## Justificativa Técnica das Relações

### 1. Loja e Equipamento
* **Relação:** 1:N (Um para Muitos)
* **Justificativa:** Uma **Loja** (agente de venda) pode vender **vários Equipamentos**. No entanto, cada Equipamento foi vendido por apenas uma Loja específica. A chave estrangeira (`id_loja`) na tabela Equipamento garante esse vínculo.

### 2. Equipamento e Garantia
* **Relação:** 1:N (Um para Muitos)
* **Justificativa:** Um **Equipamento** específico (identificado pelo seu número de série/id) pode ter **várias Garantias** ao longo do tempo (Ex: Garantia Padrão seguida por uma Garantia Estendida). Cada Garantia, no entanto, é associada a apenas um Equipamento. A chave estrangeira (`id_equipamento`) na tabela Garantia garante esse vínculo.
    * *Nota:* Se a regra de negócio fosse que cada equipamento só pode ter **uma única** garantia, a relação seria 1:1, e a chave estrangeira seria colocada em uma das tabelas (ou a PK da Garantia poderia ser a própria FK de Equipamento, dependendo do design). A modelagem 1:N é mais flexível para o cenário de múltiplas coberturas (ex: estendida, de fábrica, etc.).
---

## Modelo Lógico 

```
Loja (id_loja PK, nome, cnpj, endereco, telefone)
Equipamento (id_equipamento PK, nome, preco, data_venda, id_loja FK)
Garantia (id_garantia PK, data_inicio, data_fim, tipo, id_equipamento FK)
Documentos (id_doc PK, numero_nota)
Usuários (id_usuario PK, cpf_usuario)
```

---

## Diagrama Lógico

O diagrama representando as entidades e relações está salvo em:
```
prints/modelo_logico.png
```

---

## Execução dos Scripts

1. Abra o **DBeaver** (ou outro cliente SQL).
2. Execute o script `schema.sql` para criar as tabelas.
3. Execute o script `inserts.sql` para inserir os dados.
4. Faça consultas como:

```sql
SELECT * FROM loja;
SELECT * FROM equipamento;
SELECT * FROM garantia;
SELECT * FROM documentos;
SELECT * FROM usuarios;
```


## Diferença entre Modelos

| Modelo | Descrição |
|--------|------------|
| **Conceitual** | Mostra as entidades e relacionamentos em alto nível, sem tipos de dados. |
| **Lógico**     | Define tabelas, chaves primárias e estrangeiras, ainda sem detalhes do SGBD. |
| **Físico**     | Implementação real do banco com tipos e restrições específicos. |

