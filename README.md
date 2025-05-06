# Projeto DIO: Prevendo Vendas de Sorvete com Azure Machine Learning 🍦☀️

Projeto desenvolvido para o desafio "Prevendo Vendas de Sorvete com Machine Learning" da [Digital Innovation One (DIO)](https://web.dio.me/). O objetivo é demonstrar a criação de um modelo de regressão para prever vendas de sorvete com base na temperatura, utilizando o Azure Machine Learning.

## 📄 Contexto

Imagine ser o proprietário de uma sorveteria fictícia chamada "Gelato Mágico" em uma cidade litorânea. As vendas disparam no verão, mas percebe-se uma forte correlação entre a temperatura do dia e a quantidade de sorvetes vendidos. Para otimizar a produção, evitar desperdícios e garantir estoque, decidiu-se usar Machine Learning para prever as vendas futuras com base na temperatura.

## 🎯 Objetivo

Desenvolver um modelo de regressão preditiva que permita:
*   Treinar um modelo de Machine Learning para prever as vendas de sorvete com base na temperatura do dia.
*   Registrar e gerenciar o modelo usando MLflow (integrado ao Azure ML).
*   Criar um pipeline estruturado para treinar e testar o modelo (usando o Designer).
*   Tentar implantar o modelo para previsões em tempo real em um ambiente de nuvem (Azure).

## 🛠️ Ferramentas e Tecnologias Utilizadas

*   Microsoft Azure
*   Azure Machine Learning Studio
    *   Datasets (Ativos de Dados)
    *   Compute Instances (Instâncias de Computação)
    *   Compute Clusters (Clusters de Computação)
    *   Automated ML (ML Automatizado)
    *   Designer
    *   Models (Modelos Registrados)
    *   Endpoints (Pontos de Extremidade) - *Tentativa*
*   MLflow (para rastreamento e registro, integrado ao Azure ML)
*   GitHub (para versionamento e documentação)

## ⚙️ Passos do Projeto

1.  **Configuração do Ambiente Azure:**
    *   Criação do Grupo de Recursos (`rg-dio-projetoum`).
        *   _(Print da criação do grupo de recursos aqui)_
    *   Criação do Workspace do Azure Machine Learning (`workspacedio`) na região Brazil South.
        *   _(Print da criação/visão geral do workspace aqui)_
    *   Criação de Recursos de Computação:
        *   Instância de Computação (`CpuInstanciaZero`) para desenvolvimento/testes.
            *   _(Print da configuração/status da instância aqui)_
        *   Cluster de Computação (`CpuCluster`) para treinamento.
            *   _(Print da configuração/status do cluster aqui)_

2.  **Preparação dos Dados:**
    *   Criação de um dataset simulado com 100 linhas contendo Data, Vendas (target) e Temperatura (feature). (Arquivo: `vendas_sorvete.csv`)
    *   Upload do CSV e criação de um Ativo de Dados ("Sorvetes") no Azure ML Studio.
        *   _(Print da configuração/visão geral do Data Asset "Sorvetes" aqui)_

3.  **Treinamento com ML Automatizado (AutoML):**
    *   Configuração de um job de AutoML para tarefa de Regressão.
    *   Seleção do dataset "Sorvetes", coluna alvo "Vendas".
    *   Definição de limites de tempo e métrica primária. (Foco no algoritmo XGBoost).
        *   _(Print da configuração do job AutoML aqui)_
    *   Execução do job AutoML (`great_owl_rvfvh9bvks`).
        *   _(Print da execução concluída e visão geral do melhor modelo encontrado aqui)_
    *   Registro do melhor modelo encontrado pelo AutoML: `modelo-sorvete-automl-xgboost:1`.
        *   _(Print do modelo registrado na seção Modelos aqui)_

4.  **Treinamento com Designer:**
    *   Criação de um pipeline visual no Azure ML Designer.
    *   Componentes utilizados: Dataset "Sorvetes", Select Columns, Split Data, Linear Regression, Train Model (definindo "Vendas" como label), Score Model, Evaluate Model.
        *   _(Print do pipeline desenhado no Designer aqui)_
    *   Execução do pipeline (Job: `ExperimentDesigner / Pipeline-Created-on-05-05-2025`).
        *   _(Print da execução bem-sucedida do pipeline aqui)_
    *   Registro do modelo treinado pelo Designer: `modelo-sorvete-designer:1`.
        *   _(Print do modelo registrado na seção Modelos aqui)_

5.  **Tentativa de Implantação:**
    *   Objetivo: Implantar o modelo `modelo-sorvete-automl-xgboost:1` em um Ponto de Extremidade Online Gerenciado para previsões em tempo real.
    *   Configuração da implantação via Azure ML Studio.
        *   _(Print da tela de configuração da implantação, mostrando a VM escolhida, etc.)_
    *   **Falha na Implantação:** O processo de implantação falhou repetidamente.
        *   Erro inicial: `SubscriptionNotRegistered` para um provedor não especificado (`[N/A]`).
            *   _(Print do primeiro erro detalhado aqui)_
        *   Erro persistente após registro de provedores: `ResourceDeploymentFailure`, ainda com a mensagem `SubscriptionNotRegistered` nos detalhes.
            *   _(Print do segundo erro detalhado aqui)_

6.  **Troubleshooting (Solução de Problemas) da Implantação:**
    *   Verificação e Registro/Re-registro dos provedores de recursos via Portal do Azure (`Microsoft.MachineLearningServices`, `Microsoft.Compute`, `Microsoft.Network`, `Microsoft.ContainerRegistry`, `Microsoft.Insights`, etc.).
    *   Tentativas de implantação com diferentes tamanhos de VM (incluindo `Standard_DS1_v2`).
    *   Exclusão de endpoints falhos antes de tentar novamente.
    *   Verificação dos logs de atividade do Azure e logs de implantação (sem informações conclusivas adicionais sobre o provedor ausente).
    *   *(A causa raiz do erro `SubscriptionNotRegistered` não pôde ser identificada via Studio, apesar dos provedores parecerem registrados).*

## 📊 Resultados e Insights

*   Os dados simulados mostraram uma clara tendência de aumento das vendas com o aumento da temperatura.
*   Tanto o AutoML (com XGBoost) quanto o Designer (com Regressão Linear) conseguiram treinar modelos de regressão com sucesso. *(Você pode adicionar métricas específicas aqui, como R², se as anotou dos jobs concluídos).*
*   O processo de registro de modelos via MLflow integrado funcionou sem problemas para ambos os métodos.

## ✨ Aprendizados

*   Compreensão do fluxo de trabalho de Machine Learning ponta a ponta no Azure ML Studio (Dados -> Treinamento -> Registro).
*   Utilização prática do AutoML para encontrar rapidamente modelos performáticos.
*   Utilização do Designer para criar pipelines de ML visualmente.
*   Importância do MLflow para rastrear experimentos e versionar modelos.
*   Experiência com os desafios e a depuração de erros durante a implantação de modelos na nuvem, especialmente relacionados a configurações da assinatura (provedores de recursos, cotas).

## 📁 Arquivos no Repositório

*   `README.md`: Este arquivo de documentação.
*   `inputs/`: Pasta contendo arquivos de entrada.
    *   `inputs/cenario.txt`: Breve descrição do cenário.
*   `data/vendas_sorvete.csv`: Os dados simulados de vendas de sorvete (adicione o CSV a uma pasta `data` ou na raiz).

---
*Desenvolvido por: Fábio Jorge de Nazaré Ferreira*
