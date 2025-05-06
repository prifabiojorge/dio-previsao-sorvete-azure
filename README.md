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
    *   Criação do Workspace do Azure Machine Learning (`workspacedio`) na região Brazil South.
    *   Criação de Recursos de Computação:
        *   Instância de Computação (`CpuInstanciaZero`) para desenvolvimento/testes.
        *   Cluster de Computação (`CpuCluster`) para treinamento.

2.  **Preparação dos Dados:**
    *   Criação de um dataset simulado com 100 linhas contendo Data, Vendas (target) e Temperatura (feature). (Arquivo: `vendas_sorvete.csv`)
    *   Upload do CSV e criação de um Ativo de Dados ("Sorvetes") no Azure ML Studio.

3.  **Treinamento com ML Automatizado (AutoML):**
    *   Configuração de um job de AutoML para tarefa de Regressão.
    *   Seleção do dataset "Sorvetes", coluna alvo "Vendas".
    *   Definição de limites de tempo e métrica primária. (Foco no algoritmo XGBoost).
    *   Execução do job AutoML (`great_owl_rvfvh9bvks`).
    *   Registro do melhor modelo encontrado pelo AutoML: `modelo-sorvete-automl-xgboost:1`.

4.  **Treinamento com Designer:**
    *   Criação de um pipeline visual no Azure ML Designer.
    *   Componentes utilizados: Dataset "Sorvetes", Select Columns, Split Data, Linear Regression, Train Model (definindo "Vendas" como label), Score Model, Evaluate Model.
    *   Execução do pipeline (Job: `ExperimentDesigner / Pipeline-Created-on-05-05-2025`).
    *   Registro do modelo treinado pelo Designer: `modelo-sorvete-designer:1`.

 5. **Implantação do Modelo em Tempo Real** ✨🚀

Após os treinamentos e registros, o modelo `modelo-sorvete-automl-xgboost:1` foi selecionado para implantação como um Ponto de Extremidade Online Gerenciado no Azure Machine Learning, permitindo previsões em tempo real.

*   **Configuração Inicial e Desafios:**
    *   As primeiras tentativas de implantação encontraram falhas. O erro inicial reportado foi `SubscriptionNotRegistered` para um provedor não especificado (`[N/A]`), seguido por `ResourceDeploymentFailure` mesmo após o registro dos provedores de recursos mais comuns (`Microsoft.MachineLearningServices`, `Microsoft.Compute`, `Microsoft.Network`, `Microsoft.ContainerRegistry`, etc.).

*   **Solução do Problema de Implantação:**
    *   Após uma investigação mais aprofundada e revisão dos provedores de recursos da assinatura, identificou-se a necessidade de registrar o provedor **`Microsoft.PolicyInsights`**.
    *   Com este provedor devidamente registrado, a implantação do Ponto de Extremidade Online foi realizada com sucesso!

*   **Criação do Ponto de Extremidade Online:**
    *   Nome do Ponto de Extremidade: `workspacedio-ecrnf` 
    *   Modelo Implantado: `modelo-sorvete-automl-xgboost:1`
    *   Tipo de Computação (VM): `Standard_DS1_v2` 
    *   Autenticação: Baseada em Chave
    *   Coleta de dados e Diagnóstico do Application Insights foram habilitados.
![image](https://github.com/user-attachments/assets/5eec27b2-efbd-4f6f-a3e0-7f02d9dcc1d2)

---

<br>


 6. **Teste do Ponto de Extremidade Implantado** 🧪

O ponto de extremidade `workspacedio-ecrnf` foi testado diretamente no Azure ML Studio para verificar sua funcionalidade:

*   **Dados de Entrada (Input JSON):**
    ```json
    {
      "data": [
       ,
       ,
       
      ]
    }
    ```

*   **Resultados da Previsão (Output JSON):**
    ```json
    [
      120.4972152709961,
      148.71945190429688,
      184.60984802246094
    ]
    ```
    Isso indica que para temperaturas de 28°C, 30°C e 33°C, as vendas previstas são aproximadamente 120, 149 e 185 unidades, respectivamente.
![image (7)](https://github.com/user-attachments/assets/7a6eeaed-efe6-49b0-a33a-888cd4e21348)

---

<br>


## 📊 Resultados e Insights

*   Os dados simulados mostraram uma clara tendência de aumento das vendas com o aumento da temperatura.
*   Tanto o AutoML (com XGBoost) quanto o Designer (com Regressão Linear) conseguiram treinar modelos de regressão com sucesso.
*   O modelo `modelo-sorvete-automl-xgboost:1` foi implantado com sucesso como um serviço web para inferência em tempo real.
*   As previsões obtidas no teste do endpoint são coerentes com a relação esperada entre temperatura e vendas.

## ✨ Aprendizados

*   Compreensão do fluxo de trabalho de Machine Learning ponta a ponta no Azure ML Studio (Dados -> Treinamento -> Registro -> **Implantação** -> Teste).
*   Utilização prática do AutoML para encontrar rapidamente modelos performáticos.
*   Utilização do Designer para criar pipelines de ML visualmente.
*   Importância do MLflow para rastrear experimentos e versionar modelos.
*   Experiência com os desafios e a depuração de erros durante a implantação de modelos na nuvem.
*   **Descoberta crucial:** A necessidade de registrar provedores de recursos específicos (como `Microsoft.PolicyInsights`) que podem não ser imediatamente óbvios, mas são essenciais para a criação de certos tipos de recursos no Azure. A persistência na solução de problemas é fundamental.


## 📁 Arquivos no Repositório

*   `README.md`: Este arquivo de documentação.
*   `inputs/`: Pasta contendo arquivos de entrada.
    *   `inputs/cenario.txt`: Breve descrição do cenário.
*   `data/vendas_sorvete.csv`: Os dados simulados de vendas de sorvete (adicione o CSV a uma pasta `data` ou na raiz).

---
*Desenvolvido por: Fábio Jorge de Nazaré Ferreira*
