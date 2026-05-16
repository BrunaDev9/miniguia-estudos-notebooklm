
# 📚 Miniguia de Engenharia de Dados: Ecossistema ENEM 2024
> **Projeto desenvolvido como desafio prático na plataforma DIO, utilizando a inteligência contextual do Google NotebookLM para documentação e estruturação de conhecimento de engenharia.**

---

## 🎯 Contexto e Objetivos
O objetivo deste repositório é documentar a arquitetura, as decisões técnicas e as "cicatrizes" de infraestrutura enfrentadas durante o processamento analítico dos microdados do **ENEM 2024**. 

Buscamos demonstrar como gerenciar bases massivas (mais de 4,3 milhões de registros) saindo do absoluto zero no tratamento de dados com Python, passando pela orquestração de containers com Docker e armazenamento em PostgreSQL, até a entrega de valor de negócio com Storytelling no Power BI.

---

## 🔍 Curadoria de Fontes (Insumos do NotebookLM)
Para garantir a precisão analítica do nosso modelo de IA no NotebookLM, foram estruturadas e auditadas 3 fontes principais de conhecimento:
1. **Script_Python_ETL:** Documentação da lógica de particionamento de arquivos em Chunks (Pandas) e estratégias de redução de memória.
2. **Infraestrutura_Docker_SQL:** Parâmetros de provisionamento do container PostgreSQL e scripts de queries nativas (psql) para auditoria.
3. **Apresentacao_PowerBI_UX:** Diretrizes de integração de dados e o compilado de feedbacks e planos de ação em UI/UX recebidos da comunidade técnica do LinkedIn.

---

## 🛠️ Engenharia de Prompts & "Cicatrizes" de Bastidores
O desenvolvimento de software e a engenharia de dados são feitos de troubleshootings reais. Abaixo, destacamos os desafios superados:

### ⚡ Cicatriz 1: Estouro de Memória RAM na Origem
* **O Problema:** Os microdados brutos do INEP ultrapassam a escala de Gigabytes, tornando inviável o carregamento direto em dataframes convencionais ou ferramentas de BI locais.
* **A Solução via Prompt:** Estruturamos uma leitura fragmentada (`chunksize=100000`) no Pandas. O dado foi limpo, filtrado e convertido para o formato **Parquet** (compactação colunar via PyArrow).
* **Ganho Técnico:** O arquivo foi reduzido em mais de 70%, permitindo performance local e o versionamento do arquivo compactado direto no GitHub.

### 🔑 Cicatriz 2: O Erro "column does not exist" no PostgreSQL
* **O Problema:** Ao tentar rodar queries analíticas no terminal do Docker via `psql`, o banco retornava erro de sintaxe nas colunas, mesmo elas existindo fisicamente na tabela.
* **A Solução via Prompt:** Identificamos que o interpretador do PostgreSQL é estritamente *case-sensitive* (diferencia maiúsculas e minúsculas) para tabelas e colunas geradas automaticamente por ORMs ou dataframes Python.
* **Ganho Técnico:** Correção imediata das queries aplicando aspas duplas `""` nas colunas (Ex: `SELECT AVG("NU_NOTA_MT")`) e aspas simples `'` para filtros condicionais de strings.

---

## 📖 Miniguia de Estudo Consolidador (Entrega Final)

### 📌 Resumos Estruturados do Ecossistema
* **Camada de Engenharia (Python -> Parquet):** O dado bruto deve ser tratado o mais próximo da origem possível. Criar colunas calculadas de negócio (como a `MEDIA_GERAL`) na camada de ETL poupa o poder de processamento da camada visual de BI.
* **Camada de Infraestrutura (Docker Compose):** Utilizar volumes persistentes (`/var/lib/postgresql/data`) impede a perda de dados voláteis. Configurar o espelhamento de portas (`5432:5432`) viabiliza a comunicação externa segura entre ferramentas distintas (Power BI e Containers).
* **Camada de Apresentação & UI/UX:** A engenharia garante a performance, mas o design garante a usabilidade. Feedbacks reais de mercado pontuaram a necessidade de fundos escuros/fechados em dashboards para aumentar o contraste de cards brancos, refinando a experiência do usuário final.

### 🗂️ Glossário Técnico de Bolso
* **Docker Container:** Ambiente isolado e portátil que empacota uma aplicação e suas dependências, garantindo que o sistema funcione igual em qualquer máquina.
* **Volume Persistente:** Mecanismo do Docker que vincula um diretório virtual do container ao disco rígido físico, salvando os dados mesmo se o container for deletado.
* **Parquet File:** Formato de armazenamento colunar aberto, focado em alta compactação e performance para leitura de grandes volumes de dados.
* **psql / Docker Exec:** Comando de terminal que permite invocar e executar códigos SQL diretamente dentro de um container de banco de dados ativo.

### 🎯 Prompts Reutilizáveis para Validação e Auditoria
Copie e cole estes prompts no seu dia a dia para acelerar análises semelhantes:
1. *"Atuando como Engenheiro de Dados sênior, revise o arquivo docker-compose.yml buscando vulnerabilidades de persistência de dados ou configurações errôneas de portas relacionais."*
2. *"Com base nas colunas de notas X, Y e Z geradas pelo Pandas, escreva uma query in PostgreSQL que calcule a média agrupada por município, tratando problemas de case-sensitivity com aspas duplas."*

---
🎨 *Conhecimento estruturado com o apoio do Google NotebookLM e comunidade DIO.*
