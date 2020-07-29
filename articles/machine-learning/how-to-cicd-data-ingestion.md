---
title: Veri alma işlem hattı için DevOps
titleSuffix: Azure Machine Learning
description: Azure Machine Learning ile kullanım için veri hazırlamak üzere kullanılan bir veri alma işlem hattı oluşturmak için DevOps uygulamalarını uygulamayı öğrenin. Alma işlem hattı Azure Data Factory ve Azure Databricks kullanır. Azure işlem hattı, alma işlem hattı için sürekli tümleştirme ve teslim süreci oluşturmak üzere kullanılır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to, tracking-python
ms.author: iefedore
author: eedorenko
manager: davete
ms.reviewer: larryfr
ms.date: 06/23/2020
ms.openlocfilehash: 2f101226040c6266a53e8c15697dccb7554e70a7
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87321539"
---
# <a name="devops-for-a-data-ingestion-pipeline"></a>Veri alma işlem hattı için DevOps

Çoğu senaryoda, bir veri alma çözümü, betikleri, hizmet etkinleştirmeleri ve tüm etkinlikleri düzenleyen bir işlem hattı oluşturma çözümüdür. Bu makalede, Machine Learning model eğitimi için veri hazırlayan ortak bir veri alma işlem hattının geliştirme yaşam döngüsüne DevOps uygulamalarını nasıl uygulayacağınızı öğreneceksiniz. İşlem hattı aşağıdaki Azure Hizmetleri kullanılarak oluşturulmuştur:

* __Azure Data Factory__: ham verileri okur ve veri hazırlanmasını düzenler.
* __Azure Databricks__: verileri dönüştüren bir Python Not defteri çalıştırır.
* __Azure Pipelines__: sürekli tümleştirme ve geliştirme sürecini otomatikleştirir.

## <a name="data-ingestion-pipeline-workflow"></a>Veri alımı ardışık düzeni iş akışı

Veri alma işlem hattı aşağıdaki iş akışını uygular:

1. Ham veriler Azure Data Factory (ADF) ardışık düzenine göre okunurdur.
1. ADF ardışık düzeni, verileri dönüştürmek için bir Python Not defteri çalıştıran bir Azure Databricks kümesine verileri gönderir.
1. Veriler bir blob kapsayıcısına depolanır ve burada, bir modeli eğitebilmeniz için Azure Machine Learning tarafından kullanılabilir.

![veri alımı ardışık düzeni iş akışı](media/how-to-cicd-data-ingestion/data-ingestion-pipeline.png)

## <a name="continuous-integration-and-delivery-overview"></a>Sürekli tümleştirme ve teslime genel bakış

Birçok yazılım çözümlerinde olduğu gibi, üzerinde çalışan bir ekip (örneğin, veri mühendisleri) vardır. Azure Data Factory, Azure Databricks ve Azure depolama hesapları gibi Azure kaynaklarını işbirliği ve paylaşma. Bu kaynakların koleksiyonu bir geliştirme ortamıdır. Veri mühendisleri aynı kaynak kodu tabanına katkıda bulunur.

Sürekli tümleştirme ve teslim sistemi, çözümü oluşturma, test etme ve sunma (dağıtma) sürecini otomatikleştirir. Sürekli tümleştirme (CI) işlemi aşağıdaki görevleri gerçekleştirir:

* Kodu ayrıştırır
* Kod kalitesi testleriyle onu denetler
* Birim testlerini çalıştırır
* Sınanan kod ve Azure Resource Manager şablonları gibi yapıtlar üretir

Sürekli teslim (CD) işlemi, yapıtları aşağı akış ortamlarına dağıtır.

![cıcd veri alımı diyagramı](media/how-to-cicd-data-ingestion/cicd-data-ingestion.png)

Bu makalede, [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/)ile CI ve CD işlemlerini otomatikleştirme gösterilmektedir.

## <a name="source-control-management"></a>Kaynak denetimi yönetimi

Değişiklikleri izlemek ve takım üyeleri arasında işbirliğini etkinleştirmek için kaynak denetimi yönetimi gereklidir.
Örneğin, kod bir Azure DevOps, GitHub veya GitLab deposunda depolanır. İşbirliği iş akışı, dallanan bir modeli temel alır. Örneğin, [GitFlow](https://datasift.github.io/gitflow/IntroducingGitFlow.html).

### <a name="python-notebook-source-code"></a>Python Not defteri kaynak kodu

Veri mühendisleri, Python Not defteri kaynak koduyla yerel olarak bir IDE 'de (örneğin, [Visual Studio Code](https://code.visualstudio.com)) veya doğrudan Databricks çalışma alanında çalışır. Kod değişiklikleri tamamlandıktan sonra, bir dallanma ilkesi takip eden depoya birleştirilir.

> [!TIP] 
> Kodu `.py` `.ipynb` Jupyter Not defteri biçiminde değil dosyalarında depolamanızı öneririz. Kod okunabilirliğini geliştirir ve CI işleminde otomatik kod kalitesi denetimleri sağlar.

### <a name="azure-data-factory-source-code"></a>Azure Data Factory kaynak kodu

Azure Data Factory işlem hatlarının kaynak kodu, bir Azure Data Factory çalışma alanı tarafından oluşturulan JSON dosyaları koleksiyonudur. Normalde veri mühendisleri, kaynak kodu dosyaları doğrudan yerine Azure Data Factory çalışma alanında bir görsel tasarımcı ile çalışır. 

Çalışma alanını bir kaynak denetimi deposu kullanacak şekilde yapılandırmak için, bkz. [Azure Repos git tümleştirmesi Ile yazma](../data-factory/source-control.md#author-with-azure-repos-git-integration).   

## <a name="continuous-integration-ci"></a>Sürekli tümleştirme (CI)

Sürekli tümleştirme işleminin Ultimate hedefi, kaynak koddan ortak ekip işini toplamaktır ve bunu aşağı akış ortamlarına dağıtıma hazırlar. Kaynak kodu yönetiminde olduğu gibi, bu işlem Python Not defterleri ve Azure Data Factory işlem hatları için farklıdır. 

### <a name="python-notebook-ci"></a>Python Not defteri CI

Python Not defterleri için CI işlemi, işbirliği dalından (örneğin, ***ana*** veya ***geliştirme***) kodu alır ve aşağıdaki etkinlikleri gerçekleştirir:
* Kod ele
* Birim testi
* Kodu yapıt olarak kaydetme

Aşağıdaki kod parçacığı, bu adımların bir Azure DevOps ***YAML*** ardışık düzeninde uygulanmasını göstermektedir:

```yaml
steps:
- script: |
   flake8 --output-file=$(Build.BinariesDirectory)/lint-testresults.xml --format junit-xml  
  workingDirectory: '$(Build.SourcesDirectory)'
  displayName: 'Run flake8 (code style analysis)'  
  
- script: |
   python -m pytest --junitxml=$(Build.BinariesDirectory)/unit-testresults.xml $(Build.SourcesDirectory)
  displayName: 'Run unit tests'

- task: PublishTestResults@2
  condition: succeededOrFailed()
  inputs:
    testResultsFiles: '$(Build.BinariesDirectory)/*-testresults.xml'
    testRunTitle: 'Linting & Unit tests'
    failTaskOnFailedTests: true
  displayName: 'Publish linting and unit test results'

- publish: $(Build.SourcesDirectory)
    artifact: di-notebooks
```

İşlem hattı, Python kodunu almak için [flake8](https://pypi.org/project/flake8/) kullanır. Kaynak kodda tanımlanan birim testlerini çalıştırır ve Azure işlem hattı yürütme ekranında kullanılabilir hale gelecek şekilde, ve test sonuçlarını yayımlar:

![birim testlerini azaltma](media/how-to-cicd-data-ingestion/linting-unit-tests.png)

İşlem ve birim testi başarılı olursa işlem hattı, kaynak kodu sonraki dağıtım adımları tarafından kullanılacak yapıt deposuna kopyalar.

### <a name="azure-data-factory-ci"></a>Azure Data Factory CI

Azure Data Factory işlem hattı için CI işlemi, veri alma işlem hattı için bir darboğazdır. Sürekli tümleştirme yoktur. Azure Data Factory için dağıtılabilir yapıt, bir Azure Resource Manager şablonları koleksiyonudur. Bu şablonları oluşturmanın tek yolu Azure Data Factory çalışma alanındaki ***Yayınla*** düğmesine tıklamalıdır.

1. Veri mühendisleri, kaynak kodunu Özellik dallarından, ***ana*** veya ***geliştirme***gibi işbirliği dalında birleştirir. 
1. Verilen izinlere sahip birisi, işbirliği dalında kaynak kodundan Azure Resource Manager şablonları oluşturmak için ***Yayımla*** düğmesine tıklar. 
1. Çalışma alanı, işlem hatlarını doğrular (bunu, oluşturma ve birim testi olarak düşünün), Azure Resource Manager şablonlar oluşturur (derleme olarak düşünün) ve oluşturulan şablonları aynı kod deposundaki bir teknik dala ***adf_publish*** kaydeder (yayımlama yapıtları olarak düşünün). Bu dal, Azure Data Factory çalışma alanı tarafından otomatik olarak oluşturulur. 

Bu işlem hakkında daha fazla bilgi için [Azure Data Factory ' de sürekli tümleştirme ve teslim](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment)konusuna bakın.

Oluşturulan Azure Resource Manager şablonlarının ortam belirsiz olduğundan emin olmak önemlidir. Bu, ortamlar arasında farklı olabilecek tüm değerlerin parametrized olduğu anlamına gelir. Azure Data Factory, bu değerlerin büyük çoğunluğunu parametre olarak göstermek için yeterince akıllı bir değer. Örneğin, aşağıdaki şablonda bir Azure Machine Learning çalışma alanına yönelik bağlantı özellikleri parametre olarak sunulur:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "factoryName": {
            "value": "devops-ds-adf"
        },
        "AzureMLService_servicePrincipalKey": {
            "value": ""
        },
        "AzureMLService_properties_typeProperties_subscriptionId": {
            "value": "0fe1c235-5cfa-4152-17d7-5dff45a8d4ba"
        },
        "AzureMLService_properties_typeProperties_resourceGroupName": {
            "value": "devops-ds-rg"
        },
        "AzureMLService_properties_typeProperties_servicePrincipalId": {
            "value": "6e35e589-3b22-4edb-89d0-2ab7fc08d488"
        },
        "AzureMLService_properties_typeProperties_tenant": {
            "value": "72f988bf-86f1-41af-912b-2d7cd611db47"
        }
    }
}
```

Ancak, varsayılan olarak Azure Data Factory çalışma alanı tarafından işlenmeyen özel özelliklerinizi göstermek isteyebilirsiniz. Bu makalenin senaryosunda, Azure Data Factory işlem hattı verileri işleyen bir Python Not defterini çağırır. Not defteri, bir giriş veri dosyası adına sahip bir parametreyi kabul eder.

```Python
import pandas as pd
import numpy as np

data_file_name = getArgument("data_file_name")
data = pd.read_csv(data_file_name)

labels = np.array(data['target'])
...
```

Bu ad ***dev***, ***qa***, ***UAT***ve ***Üretim*** ortamları için farklıdır. Birden çok etkinliği olan karmaşık bir işlem hattında, birkaç özel özellik olabilir. Bu değerlerin tümünü tek bir yerde toplamak ve ardışık düzen ***değişkenleri***olarak tanımlamak iyi bir uygulamadır:

![ADF-değişkenler](media/how-to-cicd-data-ingestion/adf-variables.png)

İşlem hattı etkinlikleri, gerçekten kullanılırken ardışık düzen değişkenlerine başvurabilir:

![ADF-Not defteri-parametreler](media/how-to-cicd-data-ingestion/adf-notebook-parameters.png)

Azure Data Factory çalışma alanı, işlem hattı değişkenlerini varsayılan olarak Azure Resource Manager şablonları parametresi olarak ***kullanıma sunmaz.*** Çalışma alanı [varsayılan Parameterleştirme şablonunu](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#default-parameterization-template) kullanır Azure Resource Manager şablon parametresi olarak hangi ardışık düzen özelliklerinin gösterilmesini gerektiğini dikte eder. Listeye işlem hattı değişkenleri eklemek için, `"Microsoft.DataFactory/factories/pipelines"` [varsayılan Parameterleştirme şablonunun](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#default-parameterization-template) bölümünü aşağıdaki kod parçacığıyla güncelleştirin ve sonuç json dosyasını kaynak klasörün köküne yerleştirin:

```json
"Microsoft.DataFactory/factories/pipelines": {
        "properties": {
            "variables": {
                "*": {
                    "defaultValue": "="
                }
            }
        }
    }
```

Bunun yapılması, ***Yayımla*** düğmesine tıklandığında Azure Data Factory çalışma alanını parametreler listesine eklemek için zorlayacaktır:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "factoryName": {
            "value": "devops-ds-adf"
        },
        ...
        "data-ingestion-pipeline_properties_variables_data_file_name_defaultValue": {
            "value": "driver_prediction_train.csv"
        }        
    }
}
```

JSON dosyasındaki değerler, işlem hattı tanımında yapılandırılmış varsayılan değerlerdir. Azure Resource Manager şablonu dağıtıldığında, hedef ortam değerleriyle geçersiz kılınmaları beklenir.

## <a name="continuous-delivery-cd"></a>Sürekli teslim (CD)

Sürekli teslim süreci yapıtları alır ve ilk hedef ortama dağıtır. Bu, çözümün test çalıştırması yoluyla çalıştığından emin olmanızı sağlar. Başarılı olursa, sonraki ortama devam eder. 

CD Azure işlem hattı, ortamları temsil eden birden çok aşamadan oluşur. Her aşama, aşağıdaki adımları gerçekleştiren [dağıtımlar](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) ve [işler](https://docs.microsoft.com/azure/devops/pipelines/process/phases?view=azure-devops&tabs=yaml) içerir:

* Azure Databricks çalışma alanına Python Not defteri dağıtma
* Azure Data Factory işlem hattı dağıtma 
* İşlem hattını çalıştırma
* Veri alma sonucunu denetleme

İşlem hattı aşamaları, dağıtım işleminin ortam zinciri aracılığıyla nasıl geliştikçe daha fazla denetim sağlayan [onaylar](https://docs.microsoft.com/azure/devops/pipelines/process/approvals?view=azure-devops&tabs=check-pass) ve [kapıları](https://docs.microsoft.com/azure/devops/pipelines/release/approvals/gates?view=azure-devops) ile yapılandırılabilir.

### <a name="deploy-a-python-notebook"></a>Python Not defteri dağıtma

Aşağıdaki kod parçacığı, bir Python Not defterini Databricks kümesine kopyalayan bir Azure işlem hattı [dağıtımını](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) tanımlar:

```yaml
- stage: 'Deploy_to_QA'
  displayName: 'Deploy to QA'
  variables:
  - group: devops-ds-qa-vg
  jobs:
  - deployment: "Deploy_to_Databricks"
    displayName: 'Deploy to Databricks'
    timeoutInMinutes: 0
    environment: qa
    strategy:
      runOnce:
        deploy:
          steps:
            - task: UsePythonVersion@0
              inputs:
                versionSpec: '3.x'
                addToPath: true
                architecture: 'x64'
              displayName: 'Use Python3'

            - task: configuredatabricks@0
              inputs:
                url: '$(DATABRICKS_URL)'
                token: '$(DATABRICKS_TOKEN)'
              displayName: 'Configure Databricks CLI'    

            - task: deploynotebooks@0
              inputs:
                notebooksFolderPath: '$(Pipeline.Workspace)/di-notebooks'
                workspaceFolder: '/Shared/devops-ds'
              displayName: 'Deploy (copy) data processing notebook to the Databricks cluster'       
```            

CI tarafından üretilen yapıtlar otomatik olarak dağıtım aracısına kopyalanır ve `$(Pipeline.Workspace)` klasöründe kullanılabilir. Bu durumda, dağıtım görevi `di-notebooks` Python Not defterini içeren yapıtı ifade eder. Bu [dağıtım](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) , Not defteri dosyalarını databricks çalışma alanına kopyalamak Için [Databricks Azure DevOps uzantısını](https://marketplace.visualstudio.com/items?itemName=riserrad.azdo-databricks) kullanır.

`Deploy_to_QA`Aşama, `devops-ds-qa-vg` Azure DevOps projesinde tanımlanan değişken grubuna yönelik bir başvuru içerir. Bu aşamadaki adımlar, bu değişken grubundaki değişkenlere (örneğin, `$(DATABRICKS_URL)` ve `$(DATABRICKS_TOKEN)` ) başvurur. Fikir bir sonraki aşamanın (örneğin, `Deploy_to_UAT` ) kendı uıın kapsamlı değişken grubunda tanımlanan aynı değişken adlarıyla çalışacağı bir fikirdir.

### <a name="deploy-an-azure-data-factory-pipeline"></a>Azure Data Factory işlem hattı dağıtma

Azure Data Factory için dağıtılabilir yapıt Azure Resource Manager şablonudur. Aşağıdaki kod parçacığında gösterildiği gibi ***Azure Kaynak grubu dağıtım*** göreviyle birlikte dağıtılacak:

```yaml
  - deployment: "Deploy_to_ADF"
    displayName: 'Deploy to ADF'
    timeoutInMinutes: 0
    environment: qa
    strategy:
      runOnce:
        deploy:
          steps:
            - task: AzureResourceGroupDeployment@2
              displayName: 'Deploy ADF resources'
              inputs:
                azureSubscription: $(AZURE_RM_CONNECTION)
                resourceGroupName: $(RESOURCE_GROUP)
                location: $(LOCATION)
                csmFile: '$(Pipeline.Workspace)/adf-pipelines/ARMTemplateForFactory.json'
                csmParametersFile: '$(Pipeline.Workspace)/adf-pipelines/ARMTemplateParametersForFactory.json'
                overrideParameters: -data-ingestion-pipeline_properties_variables_data_file_name_defaultValue "$(DATA_FILE_NAME)"
```
Veri dosya adı parametresinin değeri, `$(DATA_FILE_NAME)` qa aşama değişkeni grubunda tanımlanan değişkenden gelir. Benzer şekilde, ***üzerindeARMTemplateForFactory.js*** tanımlanmış tüm parametreler geçersiz kılınabilir. Aksi takdirde, varsayılan değerler kullanılır.

### <a name="run-the-pipeline-and-check-the-data-ingestion-result"></a>İşlem hattını çalıştırma ve veri alma sonucunu denetleme

Sonraki adım, dağıtılan çözümün çalıştığından emin olmak. Aşağıdaki iş tanımı bir [PowerShell betiği](https://github.com/microsoft/DataOps/tree/master/adf/utils) ile bir Azure Data Factory işlem hattı çalıştırır ve bir Azure Databricks kümesinde Python Not defteri yürütür. Not defteri, verilerin doğru şekilde alınıp alınmış olup olmadığını denetler ve sonuç veri dosyasını `$(bin_FILE_NAME)` adla doğrular.

```yaml
  - job: "Integration_test_job"
    displayName: "Integration test job"
    dependsOn: [Deploy_to_Databricks, Deploy_to_ADF]
    pool:
      vmImage: 'ubuntu-latest'
    timeoutInMinutes: 0
    steps:
    - task: AzurePowerShell@4
      displayName: 'Execute ADF Pipeline'
      inputs:
        azureSubscription: $(AZURE_RM_CONNECTION)
        ScriptPath: '$(Build.SourcesDirectory)/adf/utils/Invoke-ADFPipeline.ps1'
        ScriptArguments: '-ResourceGroupName $(RESOURCE_GROUP) -DataFactoryName $(DATA_FACTORY_NAME) -PipelineName $(PIPELINE_NAME)'
        azurePowerShellVersion: LatestVersion
    - task: UsePythonVersion@0
      inputs:
        versionSpec: '3.x'
        addToPath: true
        architecture: 'x64'
      displayName: 'Use Python3'

    - task: configuredatabricks@0
      inputs:
        url: '$(DATABRICKS_URL)'
        token: '$(DATABRICKS_TOKEN)'
      displayName: 'Configure Databricks CLI'    

    - task: executenotebook@0
      inputs:
        notebookPath: '/Shared/devops-ds/test-data-ingestion'
        existingClusterId: '$(DATABRICKS_CLUSTER_ID)'
        executionParams: '{"bin_file_name":"$(bin_FILE_NAME)"}'
      displayName: 'Test data ingestion'

    - task: waitexecution@0
      displayName: 'Wait until the testing is done'
```

İşteki son görev, Not defteri yürütme sonucunu denetler. Bir hata döndürürse, işlem hattı yürütmenin durumunu başarısız olarak ayarlar.

## <a name="putting-pieces-together"></a>Parçaları birlikte yerleştirme

Tüm CI/CD Azure işlem hattı aşağıdaki aşamalardan oluşur:
* CI
* QA 'e dağıtın
    * Databricks + ADF 'ye dağıt
    * Tümleştirme testi

Sahip olduğunuz hedef ortam sayısına eşit sayıda ***dağıtım*** aşaması içerir. Her ***dağıtım*** aşaması paralel olarak çalışan iki [dağıtım](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) ve bir ortamda çözümü test etmek için dağıtımlardan sonra çalışan bir [iş](https://docs.microsoft.com/azure/devops/pipelines/process/phases?view=azure-devops&tabs=yaml) içerir.

İşlem hattının örnek bir uygulama aşağıdaki ***YAML*** kod parçacığında toplanır:

```yaml
variables:
- group: devops-ds-vg

stages:
- stage: 'CI'
  displayName: 'CI'
  jobs:
  - job: "CI_Job"
    displayName: "CI Job"
    pool:
      vmImage: 'ubuntu-latest'
    timeoutInMinutes: 0
    steps:
    - task: UsePythonVersion@0
      inputs:
        versionSpec: '3.x'
        addToPath: true
        architecture: 'x64'
      displayName: 'Use Python3'
    - script: pip install --upgrade flake8 flake8_formatter_junit_xml
      displayName: 'Install flake8'
    - checkout: self
    - script: |
       flake8 --output-file=$(Build.BinariesDirectory)/lint-testresults.xml --format junit-xml  
    workingDirectory: '$(Build.SourcesDirectory)'
    displayName: 'Run flake8 (code style analysis)'  
    - script: |
       python -m pytest --junitxml=$(Build.BinariesDirectory)/unit-testresults.xml $(Build.SourcesDirectory)
    displayName: 'Run unit tests'
    - task: PublishTestResults@2
    condition: succeededOrFailed()
    inputs:
        testResultsFiles: '$(Build.BinariesDirectory)/*-testresults.xml'
        testRunTitle: 'Linting & Unit tests'
        failTaskOnFailedTests: true
    displayName: 'Publish linting and unit test results'    

    # The CI stage produces two artifacts (notebooks and ADF pipelines).
    # The pipelines Azure Resource Manager templates are stored in a technical branch "adf_publish"
    - publish: $(Build.SourcesDirectory)/$(Build.Repository.Name)/code/dataingestion
      artifact: di-notebooks
    - checkout: git://${{variables['System.TeamProject']}}@adf_publish    
    - publish: $(Build.SourcesDirectory)/$(Build.Repository.Name)/devops-ds-adf
      artifact: adf-pipelines

- stage: 'Deploy_to_QA'
  displayName: 'Deploy to QA'
  variables:
  - group: devops-ds-qa-vg
  jobs:
  - deployment: "Deploy_to_Databricks"
    displayName: 'Deploy to Databricks'
    timeoutInMinutes: 0
    environment: qa
    strategy:
      runOnce:
        deploy:
          steps:
            - task: UsePythonVersion@0
              inputs:
                versionSpec: '3.x'
                addToPath: true
                architecture: 'x64'
              displayName: 'Use Python3'

            - task: configuredatabricks@0
              inputs:
                url: '$(DATABRICKS_URL)'
                token: '$(DATABRICKS_TOKEN)'
              displayName: 'Configure Databricks CLI'    

            - task: deploynotebooks@0
              inputs:
                notebooksFolderPath: '$(Pipeline.Workspace)/di-notebooks'
                workspaceFolder: '/Shared/devops-ds'
              displayName: 'Deploy (copy) data processing notebook to the Databricks cluster'             
  - deployment: "Deploy_to_ADF"
    displayName: 'Deploy to ADF'
    timeoutInMinutes: 0
    environment: qa
    strategy:
      runOnce:
        deploy:
          steps:
            - task: AzureResourceGroupDeployment@2
              displayName: 'Deploy ADF resources'
              inputs:
                azureSubscription: $(AZURE_RM_CONNECTION)
                resourceGroupName: $(RESOURCE_GROUP)
                location: $(LOCATION)
                csmFile: '$(Pipeline.Workspace)/adf-pipelines/ARMTemplateForFactory.json'
                csmParametersFile: '$(Pipeline.Workspace)/adf-pipelines/ARMTemplateParametersForFactory.json'
                overrideParameters: -data-ingestion-pipeline_properties_variables_data_file_name_defaultValue "$(DATA_FILE_NAME)"
  - job: "Integration_test_job"
    displayName: "Integration test job"
    dependsOn: [Deploy_to_Databricks, Deploy_to_ADF]
    pool:
      vmImage: 'ubuntu-latest'
    timeoutInMinutes: 0
    steps:
    - task: AzurePowerShell@4
      displayName: 'Execute ADF Pipeline'
      inputs:
        azureSubscription: $(AZURE_RM_CONNECTION)
        ScriptPath: '$(Build.SourcesDirectory)/adf/utils/Invoke-ADFPipeline.ps1'
        ScriptArguments: '-ResourceGroupName $(RESOURCE_GROUP) -DataFactoryName $(DATA_FACTORY_NAME) -PipelineName $(PIPELINE_NAME)'
        azurePowerShellVersion: LatestVersion
    - task: UsePythonVersion@0
      inputs:
        versionSpec: '3.x'
        addToPath: true
        architecture: 'x64'
      displayName: 'Use Python3'

    - task: configuredatabricks@0
      inputs:
        url: '$(DATABRICKS_URL)'
        token: '$(DATABRICKS_TOKEN)'
      displayName: 'Configure Databricks CLI'    

    - task: executenotebook@0
      inputs:
        notebookPath: '/Shared/devops-ds/test-data-ingestion'
        existingClusterId: '$(DATABRICKS_CLUSTER_ID)'
        executionParams: '{"bin_file_name":"$(bin_FILE_NAME)"}'
      displayName: 'Test data ingestion'

    - task: waitexecution@0
      displayName: 'Wait until the testing is done'                

```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Factory'de Kaynak Denetimi](https://docs.microsoft.com/azure/data-factory/source-control)
* [Azure Data Factory sürekli tümleştirme ve teslim](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment)
* [Azure Databricks için DevOps](https://marketplace.visualstudio.com/items?itemName=riserrad.azdo-databricks)
