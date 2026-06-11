# 📑 Orquestração de Workflows Serverless com AWS Step Functions

Análise prática e documentação de arquitetura desenvolvida para o desafio de projeto da plataforma **DIO (Digital Innovation One)**. O objetivo deste laboratório é consolidar o entendimento sobre o **AWS Step Functions** como ferramenta central de orquestração de microsserviços, integrando componentes essenciais do ecossistema AWS de forma visual, resiliente e baseada em estados.

---

## 🎯 Cenário de Negócio Proposto
Para aplicar os conceitos do Step Functions, foi desenhada a arquitetura de um **Pipeline Automatizado de Processamento de Pedidos e Validação de Arquivos**, simulando um ambiente real de e-commerce e faturamento industrial. 

O fluxo garante que cada etapa (upload do arquivo, validação de dados, processamento de pagamento e notificações) aconteça na ordem correta, tratando falhas de forma isolada.

---

## 🏗️ Arquitetura do Workflow & Serviços Integrados

O Step Functions coordena uma máquina de estados (*State Machine*) que gerencia os seguintes componentes:

1. **Amazon S3 (Camada de Armazenamento):** Ponto de entrada onde os relatórios ou arquivos de pedidos são depositados.
2. **AWS Lambda (Computação Serverless):** Funções efêmeras e rápidas responsáveis por processar as regras de negócio e validar a estrutura do arquivo.
3. **Amazon SQS (Mensageria/Fila):** Utilizado para desacoplar e enfileirar as tarefas de faturamento pesado ou integrações externas que não podem sobrecarregar o sistema.
4. **Amazon SNS (Notificação):** Dispara alertas imediatos (e-mail/SMS) para os operadores e clientes finais dependendo do status de sucesso ou falha crítica do workflow.

### 🗺️ Visualização Lógica do Fluxo de Estados

### 🗺️ Visualização Lógica do Fluxo de Estados

graph TD
    Start([Início do Workflow]) --> S3{⭐ Verificar Arquivo S3}
    
    S3 -->|fileStatus == 'ok'| Valid[📄 Arquivo Válido]
    S3 -->|fileStatus == 'error'| Corrupt[❌ Arquivo Corrompido]
    
    Valid --> Lambda[⚙️ Invocar Lambda <br> Processar Dados]
    Lambda --> SQS[📥 Enviar para SQS <br> Fila de Faturamento]
    SQS --> Success([🏁 Fim com Sucesso])
    
    Corrupt --> SNS[🚨 Publicar SNS: Erro]
    SNS --> Fail([🛑 Parada Imediata / Fail])
    
    %% Customização de Estilos para o GitHub
    style Start fill:#f9f9f9,stroke:#333,stroke-width:2px
    style Success fill:#d4edda,stroke:#28a745,stroke-width:2px
    style Fail fill:#f8d7da,stroke:#dc3545,stroke-width:2px
    style S3 fill:#fff3cd,stroke:#ffc107,stroke-width:2px

### 🎨 Por que essa versão é muito melhor?
* **Renderização Nativa:** O GitHub vai transformar esse bloco de texto em um gráfico real com setas perfeitas e caixas formatadas.
* **Cores Semânticas:** Adicionei estilos nas bordas e fundos para que o início/fim com sucesso fique **verde**, o fluxo de erro fique **vermelho**, e as tomadas de decisão fiquem em **amarelo**.
* **Responsivo:** Ele se ajusta automaticamente ao modo escuro (*Dark Mode*) ou claro (*Light Mode*) do GitHub de quem estiver visitando o seu perfil.



