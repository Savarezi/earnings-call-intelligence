# Earnings Call Intelligence Tracker — Petrobras (1Q26)

Este repositório contém uma ferramenta automatizada em Python desenvolvida para a ingestão, processamento e extração de sinais estratégicos e insights analíticos a partir da transcrição da earnings call da Petrobras referente ao primeiro trimestre de 2026 (1Q26). O objetivo principal é acelerar o processo de Equity Strategy, transformando uma apresentação de 60 a 90 minutos em inteligência estruturada e acionável em poucos minutos.


## Documentação Completa do Case

Para uma análise detalhada da governança do projeto, matriz de riscos linguísticos e justificativas de arquitetura, acesse o documento oficial do projeto:

* [Documentação Executiva do Case — Google Docs](https://docs.google.com/document/d/1z1J2jwPOSI_OIxhX2u40frQb-pEm0cfwHHzsO0X_2OA/edit?usp=sharing)

---

## Estrutura do Repositório

Todos os arquivos principais do projeto estão listados abaixo. Você pode navegar diretamente clicando nos links correspondentes:

* [Cópia de earnings_call_tracker3.ipynb](Cópia%20de%20earnings_call_tracker3.ipynb): Notebook Jupyter contendo todo o fluxo de desenvolvimento, instalação das dependências, chamadas de API e testes de prompt engineering.
* [analise_petrobras.json](analise_petrobras.json): Output estruturado final em formato JSON contendo o tom da gestão, evidências, mudanças de guidance, análise de perguntas críticas, red flags linguísticos e o surprise score.
* [relatorio.pdf](relatorio.pdf): Transcrição oficial em formato PDF utilizada como dado bruto de entrada para o pipeline de dados.
* [relatorio_executivo.md](relatorio_executivo.md): Relatório executivo inicial em Markdown otimizado para leitura rápida de até 2 minutos por analistas de mercado.
* [relatorio_executivo_v2.md](relatorio_executivo_v2.md): Versão atualizada e aprofundada do relatório executivo, incorporando trechos literais da transcrição e maior detalhamento do Q&A.

---

## Arquitetura do Sistema

O pipeline foi estruturado de forma linear e modular para garantir consistência e auditabilidade dos dados:

1. Ingestão e Extração de Texto: O arquivo `relatorio.pdf` é processado programaticamente por meio da biblioteca `pypdf`, consolidando o texto bruto e limpando quebras de linha e cabeçalhos redundantes.
2. Camada de Orquestração de LLM: O texto limpo é segmentado e enviado à API do Groq, utilizando o modelo open-source `llama-3.3-70b-versatile`. Este modelo foi selecionado devido à sua grande janela de contexto, baixa latência e excelente capacidade de interpretação de nuances financeiras.
3. Validação e Estruturação de Dados: Os dados brutos gerados pela IA são moldados e validados utilizando schemas do Pydantic (`BaseModel`), assegurando que a estrutura do arquivo `analise_petrobras.json` permaneça estrita e previsível para integrações com bancos de dados ou ferramentas de BI.

---

## Decisões de Prompt Engineering

Para mitigar alucinações e extrair o máximo valor analítico da transcrição, foram aplicadas as seguintes técnicas:

* Atribuição de Persona Estrita (Role-Play): O modelo foi instruído a agir estritamente como um Analista de Equity Research / Sell-Side especializado em Petrobras e no setor de Óleo e Gás. Isso direcionou o vocabulário para conceitos fundamentais como CAPEX, Margem de Refino, Upstream e spreads do Brent.
* Controle de Temperatura: Configurada uma temperatura baixa (0.2), forçando o modelo a priorizar a exatidão fatual e a extração literal de trechos ao invés de inferências criativas.
* Prompt em Camadas (Chaining): Em vez de solicitar toda a análise em uma única chamada maciça, dividiu-se a extração em tarefas focadas (Análise de Tom, Identificação de Red Flags, Rastreamento de Q&A e Surprise Score). Isso evitou a degradação de contexto e garantiu citações textuais reais e sem distorções.
* Loop de Autocrítica (Self-Critique Loop): Implementação de uma camada de validação secundária para forçar o modelo a revisar o primeiro rascunho do Q&A. Isso eliminou respostas corporativas genéricas e trouxe à tona os questionamentos reais levantados por analistas de grandes bancos.

---

## Limitações Identificadas

* Truncamento de Contexto Manual: O fatiamento preliminar de caracteres para evitar estouro de tokens pode omitir detalhes de interações ocorridas no final da sessão de perguntas e respostas em calls muito extensas.
