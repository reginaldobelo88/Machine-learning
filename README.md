# Machine-learning
Teste Azure
Entre no portal do Azure usando https://portal.azure.comsuas credenciais da Microsoft.

Selecione + Criar um recurso , pesquise Machine Learning e crie um novo recurso do Azure Machine Learning com as seguintes configurações:
Assinatura : sua assinatura do Azure .
Grupo de recursos : Crie ou selecione um grupo de recursos .
Nome : Insira um nome exclusivo para seu espaço de trabalho .
Região : Selecione a região geográfica mais próxima .
Conta de armazenamento : observe a nova conta de armazenamento padrão que será criada para seu espaço de trabalho .
Cofre de chaves : Observe o novo cofre de chaves padrão que será criado para seu espaço de trabalho .
Insights de aplicativo : observe o novo recurso padrão de insights de aplicativo que será criado para seu espaço de trabalho .
Registro de contêiner : Nenhum ( um será criado automaticamente na primeira vez que você implantar um modelo em um contêiner ).
Selecione Revisar + criar e selecione Criar . Aguarde a criação do seu espaço de trabalho (pode demorar alguns minutos) e, em seguida, vá para o recurso implantado.

Selecione Launch Studio (ou abra uma nova guia do navegador e navegue até https://ml.azure.com e entre no Azure Machine Learning Studio usando sua conta da Microsoft). Feche todas as mensagens exibidas.

No estúdio Azure Machine Learning, você deverá ver seu espaço de trabalho recém-criado. Caso contrário, selecione Todos os espaços de trabalho no menu à esquerda e selecione o espaço de trabalho que você acabou de criar.
Use aprendizado de máquina automatizado para treinar um modelo
O aprendizado de máquina automatizado permite que você experimente vários algoritmos e parâmetros para treinar vários modelos e identificar o melhor para seus dados. Neste exercício, você usará um conjunto de dados de detalhes históricos de aluguel de bicicletas para treinar um modelo que prevê o número de aluguel de bicicletas esperado em um determinado dia, com base em características sazonais e meteorológicas.
No Azure Machine Learning Studio , veja a página Automated ML (em Authoring ).

Crie um novo trabalho de ML automatizado com as seguintes configurações, usando Next conforme necessário para avançar pela interface do usuário:

Configurações básicas :

Nome do trabalho : mslearn-bike-automl
Novo nome do experimento : mslearn-bike-rental
Descrição : Aprendizado de máquina automatizado para previsão de aluguel de bicicletas
Marcadores : nenhum
Tipo de tarefa e dados :

Selecione o tipo de tarefa : Regressão
Selecionar conjunto de dados : crie um novo conjunto de dados com as seguintes configurações:
Tipo de dados :
Nome : aluguel de bicicletas
Descrição : dados históricos de aluguel de bicicletas
Tipo : Tabular
Fonte de dados :
Selecione Dos arquivos da web
URL da Web :
URL da Web :https://aka.ms/bike-rentals
Ignorar validação de dados : não selecionar
Configurações :
Formato de arquivo : Delimitado
Delimitador : Vírgula
Codificação : UTF-8
Cabeçalhos de coluna : apenas o primeiro arquivo possui cabeçalhos
Pular linhas : Nenhum
O conjunto de dados contém dados multilinhas : não selecione
Esquema :
Incluir todas as colunas exceto Caminho
Revise os tipos detectados automaticamente
Selecione Criar . Após a criação do conjunto de dados, selecione o conjunto de dados de aluguel de bicicletas para continuar a enviar o trabalho de ML automatizado.

Configurações de tarefa :

Tipo de tarefa : Regressão
Conjunto de dados : aluguel de bicicletas
Coluna de destino : Aluguéis (inteiro)
Configurações adicionais :
Métrica primária : raiz do erro quadrático médio normalizado
Explique o melhor modelo : Não selecionado
Usar todos os modelos suportados : Desmarcado . Você restringirá o trabalho para tentar apenas alguns algoritmos específicos.
Modelos permitidos : Selecione apenas RandomForest e LightGBM — normalmente você gostaria de tentar o máximo possível, mas cada modelo adicionado aumenta o tempo necessário para executar o trabalho.
Limites : expanda esta seção
Máximo de testes : 3
Máximo de testes simultâneos : 3
Máximo de nós : 3
Limite de pontuação da métrica : 0,085 ( para que, se um modelo atingir uma pontuação da métrica de erro quadrático médio normalizado de 0,085 ou menos, o trabalho termina. )
Tempo limite : 15
Tempo limite de iteração : 15
Habilitar rescisão antecipada : selecionado
Validação e teste :
Tipo de validação : divisão de validação de trem
Porcentagem de dados de validação : 10
Conjunto de dados de teste : Nenhum
Calcular :

Selecione o tipo de computação : sem servidor
Tipo de máquina virtual : CPU
Camada de máquina virtual : Dedicada
Tamanho da máquina virtual : Standard_DS3_V2*
Número de instâncias : 1
* Se a sua assinatura restringir os tamanhos de VM disponíveis para você, escolha qualquer tamanho disponível.

Envie o trabalho de treinamento. Ele inicia automaticamente.

Espere o trabalho terminar. Pode demorar um pouco – agora pode ser um bom momento para uma pausa para o café!

Avalie o melhor modelo
Quando o trabalho automatizado de aprendizado de máquina for concluído, você poderá revisar o melhor modelo treinado.

Na guia Visão geral do trabalho automatizado de aprendizado de máquina, observe o melhor resumo do modelo.
Selecione o texto em Nome do algoritmo do melhor modelo para visualizar seus detalhes.

Selecione a guia Métricas e selecione os gráficos residuais e predito_true se eles ainda não estiverem selecionados.

Revise os gráficos que mostram o desempenho do modelo. O gráfico de resíduos mostra os resíduos (as diferenças entre os valores previstos e reais) como um histograma. O gráfico predito_true compara os valores previstos com os valores verdadeiros.

Implantar e testar o modelo
Na guia Modelo do melhor modelo treinado pelo seu trabalho automatizado de machine learning, selecione Implantar e use a opção de serviço Web para implantar o modelo com as seguintes configurações:
Nome : prever-aluguéis
Descrição : Prever aluguel de bicicletas
Tipo de computação : Instância de Contêiner do Azure
Habilitar autenticação : selecionado
Aguarde o início da implantação – isso pode levar alguns segundos. O status de implantação do endpoint de previsão de aluguel será indicado na parte principal da página como Running .
Aguarde até que o status da implantação mude para Succeeded . Isso pode levar de 5 a 10 minutos.
Testar o serviço implantado
Agora você pode testar seu serviço implantado.

No estúdio Azure Machine Learning, no menu esquerdo, selecione Endpoints e abra o ponto final em tempo real de previsão de alugueres .

Na página do endpoint em tempo real de previsão de aluguel, visualize a guia Teste .

No painel Dados de entrada para testar o endpoint , substitua o modelo JSON pelos seguintes dados de entrada:
Limpar
O serviço web que você criou está hospedado em uma instância de contêiner do Azure . Se não pretender experimentá-lo ainda mais, deverá eliminar o ponto final para evitar acumular utilização desnecessária do Azure.

No estúdio Azure Machine Learning , na guia Endpoints , selecione o ponto de extremidade de previsão de aluguel . Em seguida, selecione Excluir e confirme que deseja excluir o endpoint.

Excluir sua computação garante que sua assinatura não será cobrada por recursos de computação. No entanto, será cobrada uma pequena quantia pelo armazenamento de dados, desde que o espaço de trabalho do Azure Machine Learning exista na sua assinatura. Se tiver terminado de explorar o Azure Machine Learning, poderá eliminar o espaço de trabalho Azure Machine Learning e os recursos associados.

Para excluir seu espaço de trabalho:

No portal Azure , na página Grupos de recursos , abra o grupo de recursos que especificou ao criar o seu espaço de trabalho Azure Machine Learning.
Clique em Excluir grupo de recursos , digite o nome do grupo de recursos para confirmar que deseja excluí-lo e selecione Excluir .


{
    "runId": "mslearn-bike-automl",
    "runUuid": "067ed5db-7f47-4d77-92a7-f963efded2d2",
    "parentRunUuid": null,
    "rootRunUuid": "067ed5db-7f47-4d77-92a7-f963efded2d2",
    "target": "Serverless",
    "status": "Completed",
    "parentRunId": null,
    "dataContainerId": "dcid.mslearn-bike-automl",
    "createdTimeUtc": "2024-04-29T20:06:46.8992925+00:00",
    "startTimeUtc": "2024-04-29T20:07:03.419Z",
    "endTimeUtc": "2024-04-29T20:17:37.958Z",
    "error": null,
    "warnings": null,
    "tags": {
        "_aml_system_automl_mltable_data_json": "{\"Type\":\"MLTable\",\"TrainData\":{\"Uri\":\"azureml://locations/brazilsouth/workspaces/6372fd10-0f55-49ae-9503-26fb9a46c7c1/data/alugueldebicicletas/versions/1\",\"ResolvedUri\":null,\"AssetId\":null},\"TestData\":null,\"ValidData\":null}",
        "model_explain_run": "best_run",
        "_aml_system_automl_run_workspace_id": "6372fd10-0f55-49ae-9503-26fb9a46c7c1",
        "_aml_system_azureml.automlComponent": "AutoML",
        "pipeline_id_000": "faf12f74cf9bbd358ca5525682c5030d36f7be7c;b76be6b5846772ee1128c4d415381c1e9fed455e;__AutoML_Ensemble__",
        "score_000": "0.09357675586470318;0.0958428095003268;0.088008071660423",
        "predicted_cost_000": "0;0.5;0",
        "fit_time_000": "0.064852;0.02261;1",
        "training_percent_000": "100;100;100",
        "iteration_000": "0;1;2",
        "run_preprocessor_000": "MaxAbsScaler;MaxAbsScaler;",
        "run_algorithm_000": "LightGBM;RandomForest;VotingEnsemble",
        "automl_best_child_run_id": "mslearn-bike-automl_2"
    },
    "properties": {
        "num_iterations": "3",
        "training_type": "TrainFull",
        "acquisition_function": "EI",
        "primary_metric": "normalized_root_mean_squared_error",
        "train_split": "0",
        "acquisition_parameter": "0",
        "num_cross_validation": "",
        "target": "Serverless",
        "AMLSettingsJsonString": "{\"is_subgraph_orchestration\":false,\"is_automode\":true,\"path\":\"./sample_projects/\",\"subscription_id\":\"bd56cfdb-bae8-48d8-ba30-0fa1941585a3\",\"resource_group\":\"Laboratórioteste\",\"workspace_name\":\"laboratorio-AI900\",\"iterations\":3,\"primary_metric\":\"normalized_root_mean_squared_error\",\"task_type\":\"regression\",\"IsImageTask\":false,\"IsTextDNNTask\":false,\"validation_size\":0.1,\"n_cross_validations\":null,\"preprocess\":true,\"is_timeseries\":false,\"time_column_name\":null,\"grain_column_names\":null,\"max_cores_per_iteration\":-1,\"max_concurrent_iterations\":3,\"max_nodes\":3,\"iteration_timeout_minutes\":15,\"enforce_time_on_windows\":false,\"experiment_timeout_minutes\":15,\"exit_score\":\"NaN\",\"experiment_exit_score\":\"NaN\",\"whitelist_models\":[\"RandomForest\",\"LightGBM\"],\"blacklist_models\":null,\"blacklist_algos\":[\"TensorFlowDNN\",\"TensorFlowLinearRegressor\"],\"auto_blacklist\":false,\"blacklist_samples_reached\":false,\"exclude_nan_labels\":false,\"verbosity\":20,\"model_explainability\":false,\"enable_onnx_compatible_models\":false,\"enable_feature_sweeping\":false,\"send_telemetry\":true,\"enable_early_stopping\":true,\"early_stopping_n_iters\":20,\"distributed_dnn_max_node_check\":false,\"enable_distributed_featurization\":true,\"enable_distributed_dnn_training\":true,\"enable_distributed_dnn_training_ort_ds\":false,\"ensemble_iterations\":3,\"enable_tf\":false,\"enable_cache\":false,\"enable_subsampling\":false,\"metric_operation\":\"minimize\",\"enable_streaming\":false,\"use_incremental_learning_override\":false,\"force_streaming\":false,\"enable_dnn\":false,\"is_gpu_tmp\":false,\"enable_run_restructure\":false,\"featurization\":\"auto\",\"vm_type\":\"Standard_DS3_v2\",\"vm_priority\":\"dedicated\",\"label_column_name\":\"rentals\",\"weight_column_name\":null,\"miro_flight\":\"default\",\"many_models\":false,\"many_models_process_count_per_node\":0,\"automl_many_models_scenario\":null,\"enable_batch_run\":true,\"save_mlflow\":true,\"track_child_runs\":true,\"test_include_predictions_only\":false,\"enable_mltable_quick_profile\":\"True\",\"has_multiple_series\":false,\"_enable_future_regressors\":false,\"enable_ensembling\":true,\"enable_stack_ensembling\":false,\"ensemble_download_models_timeout_sec\":300.0,\"stack_meta_learner_train_percentage\":0.2}",
        "DataPrepJsonString": null,
        "EnableSubsampling": "False",
        "runTemplate": "AutoML",
        "azureml.runsource": "automl",
        "_aml_internal_automl_best_rai": "False",
        "ClientType": "Mfe",
        "_aml_system_scenario_identification": "Remote.Parent",
        "PlatformVersion": "DPV2",
        "environment_cpu_name": "AzureML-AutoML",
        "environment_cpu_label": "prod",
        "environment_gpu_name": "AzureML-AutoML-GPU",
        "environment_gpu_label": "prod",
        "root_attribution": "automl",
        "attribution": "AutoML",
        "Orchestrator": "AutoML",
        "CancelUri": "https://brazilsouth.api.azureml.ms/jasmine/v1.0/subscriptions/bd56cfdb-bae8-48d8-ba30-0fa1941585a3/resourceGroups/Laboratórioteste/providers/Microsoft.MachineLearningServices/workspaces/laboratorio-AI900/experimentids/6c2445a0-80e0-4e0a-9f2f-51b4da4674bc/cancel/mslearn-bike-automl",
        "mltable_data_json": "{\"Type\":\"MLTable\",\"TrainData\":{\"Uri\":\"azureml://locations/brazilsouth/workspaces/6372fd10-0f55-49ae-9503-26fb9a46c7c1/data/alugueldebicicletas/versions/1\",\"ResolvedUri\":\"azureml://locations/brazilsouth/workspaces/6372fd10-0f55-49ae-9503-26fb9a46c7c1/data/alugueldebicicletas/versions/1\",\"AssetId\":\"azureml://locations/brazilsouth/workspaces/6372fd10-0f55-49ae-9503-26fb9a46c7c1/data/alugueldebicicletas/versions/1\"},\"TestData\":null,\"ValidData\":null}",
        "ClientSdkVersion": null,
        "snapshotId": "00000000-0000-0000-0000-000000000000",
        "SetupRunId": "mslearn-bike-automl_setup",
        "SetupRunContainerId": "dcid.mslearn-bike-automl_setup",
        "FeaturizationRunJsonPath": "featurizer_container.json",
        "FeaturizationRunId": "mslearn-bike-automl_featurize",
        "ProblemInfoJsonString": "{\"dataset_num_categorical\": 0, \"is_sparse\": true, \"subsampling\": false, \"has_extra_col\": true, \"dataset_classes\": 552, \"dataset_features\": 64, \"dataset_samples\": 657, \"single_frequency_class_detected\": false}"
    },
    "parameters": {},
    "services": {},
    "inputDatasets": null,
    "outputDatasets": [],
    "runDefinition": null,
    "logFiles": {},
    "jobCost": {
        "chargedCpuCoreSeconds": null,
        "chargedCpuMemoryMegabyteSeconds": null,
        "chargedGpuSeconds": null,
        "chargedNodeUtilizationSeconds": null
    },
    "revision": 13,
    "runTypeV2": {
        "orchestrator": "AutoML",
        "traits": [
            "automl",
            "Remote.Parent"
        ],
        "attribution": null,
        "computeType": null
    },
    "settings": {},
    "computeRequest": null,
    "compute": {
        "target": "Serverless",
        "targetType": "AmlCompute",
        "vmSize": "Standard_DS3_v2",
        "instanceType": "Standard_DS3_v2",
        "instanceCount": 1,
        "gpuCount": null,
        "priority": "Dedicated",
        "region": null,
        "armId": null,
        "properties": null
    },
    "createdBy": {
        "userObjectId": "80b0cd6a-3eb1-4d4e-b651-631372f32626",
        "userPuId": "10032003778B4E46",
        "userIdp": "live.com",
        "userAltSecId": "1:live.com:00060000A6A619FC",
        "userIss": "https://sts.windows.net/4362a6b9-871f-4832-a7e0-2ffd05aed9ac/",
        "userTenantId": "4362a6b9-871f-4832-a7e0-2ffd05aed9ac",
        "userName": "Reginaldo Filho",
        "upn": null
    },
    "computeDuration": "00:10:34.5389959",
    "effectiveStartTimeUtc": null,
    "runNumber": 1714421206,
    "rootRunId": "mslearn-bike-automl",
    "experimentId": "6c2445a0-80e0-4e0a-9f2f-51b4da4674bc",
    "userId": "80b0cd6a-3eb1-4d4e-b651-631372f32626",
    "statusRevision": 3,
    "currentComputeTime": null,
    "lastStartTimeUtc": null,
    "lastModifiedBy": {
        "userObjectId": "80b0cd6a-3eb1-4d4e-b651-631372f32626",
        "userPuId": "10032003778B4E46",
        "userIdp": "live.com",
        "userAltSecId": "1:live.com:00060000A6A619FC",
        "userIss": "https://sts.windows.net/4362a6b9-871f-4832-a7e0-2ffd05aed9ac/",
        "userTenantId": "4362a6b9-871f-4832-a7e0-2ffd05aed9ac",
        "userName": "Reginaldo Filho",
        "upn": null
    },
    "lastModifiedUtc": "2024-04-29T20:17:37.6893658+00:00",
    "duration": "00:10:34.5389959",
    "inputs": {
        "training_data": {
            "assetId": "azureml://locations/brazilsouth/workspaces/6372fd10-0f55-49ae-9503-26fb9a46c7c1/data/alugueldebicicletas/versions/1",
            "type": "MLTable"
        }
    },
    "outputs": {
        "best_model": {
            "assetId": "azureml://locations/brazilsouth/workspaces/6372fd10-0f55-49ae-9503-26fb9a46c7c1/models/azureml_mslearn-bike-automl_2_output_mlflow_log_model_278589041/versions/1",
            "type": "MLFlowModel"
        }
    },
    "currentAttemptId": 1
}
