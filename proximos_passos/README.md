# Planejamento de Evolução: Automação de Pipeline de Investimentos com n8n

Este documento apresenta o planejamento estratégico e a arquitetura técnica para a próxima fase deste projeto. O objetivo central é realizar a transição da análise executada de forma manual em ambiente de desenvolvimento (Jupyter Notebook) para um ecossistema produtivo, resiliente e 100% automatizado, utilizando o n8n como orquestrador central de fluxos.

Ao implementar esta arquitetura orientada a eventos (event-driven), o sistema passa a operar de forma autônoma, eliminando a necessidade de qualquer intervenção humana para carregar scripts, processar dados ou gerar relatórios.

---

## Detalhamento das Fases do Fluxo Automatizado

O fluxo de trabalho dentro do orquestrador n8n será segmentado em quatro camadas operacionais independentes e integradas:

### 1. Camada de Gatilho (Trigger Layer)
A automação não depende de agendamento fixo, mas sim da chegada de novos dados ao mercado. O gatilho para o início do pipeline pode ser configurado de três maneiras distintas, dependendo da necessidade do time de análise:
* Gatilho de Armazenamento: Monitoramento ativo de um diretório compartilhado em nuvem. No momento em que o PDF da transcrição da chamada de resultados é depositado na pasta, o webhook do n8n captura o arquivo imediatamente.
* Gatilho de Comunicação: Integração com servidores de e-mail corporativos. O fluxo é disparado ao receber um e-mail com uma estrutura de assunto específica contendo o relatório em anexo.
* Gatilho de API: Um endpoint HTTP exposto pelo n8n que pode receber requisições de outros sistemas internos da empresa.

### 2. Camada de Processamento de Linguagem Natural (LLM Layer)
Uma vez capturado o arquivo binário do PDF pela etapa anterior, o n8n gerencia o envio do documento para os modelos de linguagem avançados utilizando os nós nativos de inteligência artificial (Advanced AI Nodes).
* Engenharia de Prompt em Produção: O prompt estruturado e testado no MVP é injetado diretamente no nó do orquestrador, mantendo a parametrização de temperatura baixa para garantir a acurácia factual.
* Estruturação de Contexto: O orquestrador gerencia a carga do arquivo completo, garantindo que a persona de analista de Equity Research filtre o ruído institucional e foque nas métricas de CAPEX, custos de extração e dinâmica de refino.

### 3. Camada de Execução de Código e Visualização (Code & Graphics Layer)
O output retornado pelo motor de inteligência artificial é um objeto JSON estruturado. Para transformar esses dados em insights visuais rápidos para os tomadores de decisão, o n8n direciona as informações para um nó de execução de código (Code Node) configurado em ambiente Python:
* Processamento Dinâmico: O script Python interno lê o dicionário de dados gerado pela IA.
* Geração de Gráficos: Utilizando rotinas automatizadas de visualização de dados, o script calcula o volume de alertas linguísticos por severidade (Baixo, Médio e Alto) e renderiza os gráficos de barras e indicadores de score.
* Persistência de Mídia: Os gráficos gerados são convertidos de volta em arquivos binários de imagem diretamente na memória do fluxo do n8n, prontos para serem anexados às mensagens de saída.

### 4. Camada de Distribuição e Destino (Delivery Layer)
A última etapa do fluxo consiste em pulverizar a inteligência gerada para as ferramentas de consumo do time de negócios e estratégia:
* Notificação de Alta Prioridade: Envio do relatório executivo resumido diretamente para os canais de comunicação interna de analistas, permitindo a leitura em dispositivos móveis no início do pregão.
* Relatório Executivo Formal: Disparo de e-mails estruturados em HTML contendo a análise do Q&A e as imagens dos gráficos em anexo para os gestores de portfólio.
* Governança e Histórico: Alimentação automática de bancos de dados relacionais ou planilhas de controle, registrando o histórico de surpresas de mercado e red flags ao longo dos trimestres para análises comparativas futuras.

---

## Benefícios Estratégicos da Automação

* Redução drástica do Time-to-Market: O tempo entre o encerramento de uma chamada de resultados e a distribuição dos insights visuais para a mesa de operações cai de dias ou horas para escassos segundos.
* Eliminação de Erros Manuais: O fluxo padronizado garante que nenhum passo de validação técnica seja ignorado, aplicando as regras estritas de verificação de dados de forma homogênea para qualquer documento inserido.
* Eficiência e Escalabilidade: A estrutura modular permite reaproveitar 100% da lógica deste pipeline para monitorar e analisar de forma simultânea dezenas de outras empresas listadas na bolsa de valores, escalando a capacidade analítica sem a necessidade de contratação de novos recursos operacionais.
