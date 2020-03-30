---
title: Veri alma boru hattı için DevOps
titleSuffix: Azure Machine Learning
description: DevOps uygulamalarını, bir model eğitimi için veri hazırlamak için kullanılan bir veri alma boru hattı uygulamasına nasıl uygulayacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: iefedore
author: eedorenko
manager: davete
ms.reviewer: larryfr
ms.date: 01/30/2020
ms.openlocfilehash: d987171d41bd6d80bab4cce91ef9ecec1f0dc7a4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247189"
---
# <a name="devops-for-a-data-ingestion-pipeline"></a>Veri alma boru hattı için DevOps

Çoğu senaryoda, veri alma çözümü komut dosyalarının, hizmet çağrılarının ve tüm etkinlikleri düzenleyen bir ardışık yapının bileşimidir. Bu makalede, DevOps uygulamalarını ortak bir veri alma ardışık ardışık ardışık ardışık yapının geliştirme yaşam döngüsüne nasıl uygulayacağınızı öğreneceksiniz. Boru hattı, Verileri Machine Learning modeli eğitimi için hazırlar.

## <a name="the-solution"></a>Çözüm

Aşağıdaki veri alma iş akışını göz önünde bulundurun:

![veri alma-boru hattı](media/how-to-cicd-data-ingestion/data-ingestion-pipeline.png)

Bu yaklaşımda, eğitim verileri bir Azure blob depolama sında depolanır. Azure Veri Fabrikası ardışık hattı, verileri bir giriş blob kapsayıcısından alır, dönüştürür ve verileri çıktı blob kapsayıcısına kaydeder. Bu kapsayıcı, Azure Machine Learning hizmeti için bir [veri depolama](concept-data.md) alanı görevi görehizmet eder. Verilerin hazırlanmasından sonra, Veri Fabrikası boru hattı bir modeli eğitmek için bir eğitim Machine Learning boru hattı çağırır. Bu özel örnekte veri dönüşümü, Azure Veri Tuğlaları kümesinde çalışan bir Python dizüstü bilgisayarı tarafından gerçekleştirilir. 

## <a name="what-we-are-building"></a>Ne inşa ediyoruz

Herhangi bir yazılım çözümünde olduğu gibi, üzerinde çalışan bir ekip (örneğin, Veri Mühendisleri) vardır. 

![cicd-veri-yutma](media/how-to-cicd-data-ingestion/cicd-data-ingestion.png)

Azure Veri Fabrikası, Azure Veri Tuğlaları, Azure Depolama hesabı gibi aynı Azure kaynaklarını işbirliği yaparlar ve paylaşırlar. Bu kaynakların toplanması bir Geliştirme ortamıdır. Veri mühendisleri aynı kaynak kod tabanına katkıda bulunur. Sürekli Tümleştirme işlemi kodu bir araya getirer, kod kalitesi testleri, birim testleri ile denetler ve test kodu ve Azure Kaynak Yöneticisi şablonları gibi yapılar üretir. Sürekli Teslim işlemi, yapıları akış aşağı ortamlarına dağıtir. Bu makalede, [Azure Ardışık Lıklar](https://azure.microsoft.com/services/devops/pipelines/)ile CI ve CD işlemlerinin nasıl otomatikleştirilen gösterildiği gösterilmiştir.

## <a name="source-control-management"></a>Kaynak kontrol yönetimi

Ekip üyeleri Python not defteri kaynak kodu ve Azure Veri Fabrikası kaynak kodu üzerinde işbirliği yapmak için biraz farklı şekillerde çalışır. Ancak, her iki durumda da kod bir kaynak denetim deposunda depolanır (örneğin, Azure DevOps, GitHub, GitLab) ve işbirliği normalde bazı dallandırma modeline [(örneğin, GitFlow)](https://datasift.github.io/gitflow/IntroducingGitFlow.html)dayanır.

### <a name="python-notebook-source-code"></a>Python Notebook Kaynak Kodu

Veri mühendisleri Python not defteri kaynak koduyla yerel olarak bir IDE'de (örneğin, [Visual Studio Code)](https://code.visualstudio.com)veya doğrudan Databricks çalışma alanında çalışır. İkincisi, geliştirme ortamındaki kodu hata ayıklama olanağı verir. Her durumda, kod bir dallanma ilkesini izleyerek depoyla birleştirilecektir. Kodun Jupyter dizüstü bilgisayar `.py` biçiminde değil de dosyalarda depolaması önerilir. `.ipynb` Kod okunabilirliğini artırır ve CI işleminde otomatik kod kalitesi denetimleri sağlar.

### <a name="azure-data-factory-source-code"></a>Azure Veri Fabrikası Kaynak Kodu

Azure Veri Fabrikası ardışık hatlarının kaynak kodu, bir çalışma alanı tarafından oluşturulan json dosyaları topluluğudur. Normalde veri mühendisleri doğrudan kaynak kod dosyaları yerine Azure Veri Fabrikası çalışma alanında görsel bir tasarımcıyla çalışır. [Çalışma alanını, Azure Veri Fabrikası belgelerinde](https://docs.microsoft.com/azure/data-factory/source-control#author-with-azure-repos-git-integration)açıklandığı gibi bir kaynak denetim deposuyla yapılandırın. Bu yapılandırma nın devreye sayılması yla, veri mühendisleri tercih edilen dallanma iş akışını takiben kaynak kodu üzerinde işbirliği yapabilecekler.    

## <a name="continuous-integration-ci"></a>Sürekli entegrasyon (CI)

Sürekli Tümleştirme işleminin nihai amacı, ortak ekip çalışmasını kaynak kodundan toplamak ve alt akış ortamlarına dağıtımiçin hazırlamaktır. Kaynak kod yönetiminde olduğu gibi bu işlem Python dizüstü bilgisayarlar ve Azure Veri Fabrikası ardışık hatları için farklıdır. 

### <a name="python-notebook-ci"></a>Python Notebook CI

Python Notebook'larının CI işlemi kodu işbirliği dalından alır (örneğin, ***ana bilgisayar*** veya ***geliştirme)*** ve aşağıdaki etkinlikleri gerçekleştirir:
* Kod linting
* Birim testi
* Kodu artefakt olarak kaydetme

Aşağıdaki kod parçacığı, azure DevOps ***veyaml*** ardışık bir ardışık alanda aşağıdaki adımların uygulanmasını gösterir:

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

Ardışık hatlar Python kod linting yapmak için ***flake8*** kullanır. Kaynak kodunda tanımlanan birim testlerini çalıştırır ve azure pipeline yürütme ekranında kullanılabilir olmaları için linting ve test sonuçlarını yayımlar:

![linting-birim-testleri](media/how-to-cicd-data-ingestion/linting-unit-tests.png)

Linting ve birim sınama başarılı olursa, ardışık ardışık ardışık sonraki dağıtım adımları tarafından kullanılmak üzere yapı deposuna kaynak kodu kopyalar.

### <a name="azure-data-factory-ci"></a>Azure Veri Fabrikası CI

Azure Veri Fabrikası ardışık bir ardışık için CI işlemi, veri alma ardışık bir ardışık ardışık ardışık ardışık ardışık ardışık ardışık ardışık laştırma için tüm CI/CD hikayesinde bir darboğazdır. ***Sürekli*** Entegrasyon diye bir şey yok. Azure Veri Fabrikası için dağıtılabilir yapı, Azure Kaynak Yöneticisi şablonlarının bir koleksiyonudur. Bu şablonları oluşturmanın tek yolu Azure Veri Fabrikası çalışma alanında ***yayımla*** düğmesini tıklatmaktır. Burada otomasyon yok.
Veri mühendisleri, özellik dallarındaki kaynak kodunu, örneğin ***ana*** bilgisayar veya ***geliştirme***gibi işbirliği dalında birleştirir. Ardından, izin verilen bir kişi, işbirliği dalındaki kaynak koddan Azure Kaynak Yöneticisi şablonları oluşturmak için ***yayımlama*** düğmesini tıklatır. Düğme tıklandığında, çalışma alanı ardışık yapıları doğrular (linting ve birim sınama olarak düşünün), Azure Kaynak Yöneticisi şablonları oluşturur (bina olarak düşünün) ve oluşturulan şablonları aynı kod deposunda ***adf_publish*** teknik bir dala kaydeder (yapıyı yayımlarken düşünün). Bu dal, Azure Veri Fabrikası çalışma alanı tarafından otomatik olarak oluşturulur. Bu işlem, [Azure Veri Fabrikası belgelerinde](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment)ayrıntılı olarak açıklanmıştır.

Oluşturulan Azure Kaynak Yöneticisi şablonlarının ortam agnostik olduğundan emin olmak önemlidir. Bu, ortamlar arasında farklılık gösteren tüm değerlerin parametrize olduğu anlamına gelir. Azure Veri Fabrikası, parametreler gibi değerlerin çoğunu ortaya çıkaracak kadar akıllıdır. Örneğin, aşağıdaki şablonda Bir Azure Machine Learning çalışma alanının bağlantı özellikleri parametre olarak ortaya çıkarır:

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

Ancak, varsayılan olarak Azure Veri Fabrikası çalışma alanı tarafından işlenmemiş özel özelliklerinizi ortaya çıkarmak isteyebilirsiniz. Bu makalenin senaryosunda, Azure Veri Fabrikası ardışık bir veri işleme python dizüstü çağırır. Not defteri, giriş veri dosyasının adını içeren bir parametre kabul eder.

```Python
import pandas as pd
import numpy as np

data_file_name = getArgument("data_file_name")
data = pd.read_csv(data_file_name)

labels = np.array(data['target'])
...
```

Bu ad ***Dev,*** ***QA***, ***UAT***ve ***PROD*** ortamları için farklıdır. Birden çok etkinlik içeren karmaşık bir ardışık ardışık ardışık alanda, birkaç özel özellik olabilir. Tüm bu değerleri tek bir yerde toplamak ve bunları boru hattı ***değişkenleri***olarak tanımlamak iyi bir uygulamadır:

![adf değişkenleri](media/how-to-cicd-data-ingestion/adf-variables.png)

Boru hattı etkinlikleri, bunları kullanırken ardışık hatlar değişkenlerine başvurabilir:

![adf-notebook-parametreleri](media/how-to-cicd-data-ingestion/adf-notebook-parameters.png)

Azure Veri Fabrikası çalışma alanı, varsayılan olarak Azure Kaynak Yöneticisi parametreleri şablonu olarak boru hattı değişkenlerini ortaya ***çıkarmaz.*** Çalışma alanı, Azure Kaynak Yöneticisi şablon parametreleri olarak hangi alt iş ne anlama gelen özelliklerin açığa çıkarılması gerektiğini belirleyen [Varsayılan Parametrelendirme](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#default-parameterization-template) Şablonu'nu kullanır. Listeye ardışık hatlar değişkenleri eklemek için Varsayılan [Parametrelendirme Şablonu'nun](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#default-parameterization-template) "Microsoft.DataFactory/factorys/pipelines" bölümünü aşağıdaki parçacıkla güncelleyin ve sonuç json dosyasını kaynak klasörün köküne yerleştirin:

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

Bunu yapmak, Azure Veri Fabrikası çalışma alanını ***yayımlama*** düğmesi tıklandığında değişkenleri parametreler listesine eklemeye zorlar:

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

json dosyasındaki değerler, ardışık yapı tanımında yapılandırılan varsayılan değerlerdir. Azure Kaynak Yöneticisi şablonu dağıtıldığında hedef ortam değerleriyle geçersiz kılınması beklenir.

## <a name="continuous-delivery-cd"></a>Sürekli teslimat (CD)

Sürekli Teslim işlemi yapıları alır ve ilk hedef ortamına dağır. Bu, çözüm testleri çalıştırarak çalıştığından emin olun. Başarılı olursa, bir sonraki ortama devam eder. CD Azure Ardışık Alanı, ortamları temsil eden birden çok aşamadan oluşur. Her aşama, aşağıdaki adımları gerçekleştiren [dağıtımlar](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) ve [işler](https://docs.microsoft.com/azure/devops/pipelines/process/phases?view=azure-devops&tabs=yaml) içerir:
* Python Not Defterini Azure Databricks çalışma alanına dağıtma
* Azure Veri Fabrikası ardışık hattını dağıtma 
* İşlem hattını çalıştırma
* Veri alma sonucunu kontrol edin

Boru hattı aşamaları, dağıtım işleminin ortamlar zinciri aracılığıyla nasıl geliştiği konusunda ek denetim sağlayan [onaylar](https://docs.microsoft.com/azure/devops/pipelines/process/approvals?view=azure-devops&tabs=check-pass) ve [kapılarla](https://docs.microsoft.com/azure/devops/pipelines/release/approvals/gates?view=azure-devops) yapılandırılabilir.

### <a name="deploy-a-python-notebook"></a>Python Not Defteri Dağıtma

Aşağıdaki kod snippet' ı, Python not defterini Databricks kümesine kopyalayan bir Azure Ardışık Hattı [dağıtımı](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) tanımlar:

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

CI tarafından üretilen eserler otomatik olarak dağıtım aracısına kopyalanır ve ***$(Pipeline.Workspace)*** klasöründe kullanılabilir. Bu durumda, dağıtım görevi Python not defterini içeren ***di-not defteri*** yapısını ifade eder. Bu [dağıtım,](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) not defteri dosyalarını Databricks çalışma alanına kopyalamak için [Databricks Azure DevOps uzantısını](https://marketplace.visualstudio.com/items?itemName=riserrad.azdo-databricks) kullanır.
***Deploy_to_QA*** aşamasında Azure DevOps projesinde tanımlanan ***devops-ds-qa-vg*** değişken grubuna bir başvuru içerir. Bu aşamadaki adımlar, bu değişken grubundaki değişkenleri (örneğin, $(DATABRICKS_URL), $(DATABRICKS_TOKEN)) ifade eder. Fikir, bir sonraki aşamanın (örneğin, ***Deploy_to_UAT)*** kendi UAT kapsamlı değişken grubunda tanımlanan aynı değişken adlarıyla çalışacağıdır.

### <a name="deploy-an-azure-data-factory-pipeline"></a>Azure Veri Fabrikası ardışık hattını dağıtma

Azure Veri Fabrikası için dağıtılabilir yapı, Azure Kaynak Yöneticisi şablonudur. Bu nedenle, aşağıdaki parçacıkta gösterildiği gibi ***Azure Kaynak Grubu Dağıtım*** göreviyle dağıtılacak:

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
Veri dosya adı parametresinin değeri, QA aşaması değişken grubunda tanımlanan $(DATA_FILE_NAME) değişkeninden gelir. Benzer şekilde, ***ARMTemplateForFactory.json'da*** tanımlanan tüm parametreler geçersiz kılınabilir. Bunlar değilse, varsayılan değerler kullanılır.

### <a name="run-the-pipeline-and-check-the-data-ingestion-result"></a>Ardışık hattı çalıştırın ve veri alma sonucunu kontrol edin

Bir sonraki adım, dağıtılan çözümün çalıştığından emin olmaktır. Aşağıdaki iş tanımı, [PowerShell komut dosyasıyla](https://github.com/microsoft/DataOps/tree/master/adf/utils) birlikte bir Azure Veri Fabrikası ardışık hattını çalıştırAr ve Bir Azure Veri Tuğlaları kümesinde python not defterini çalıştırar. Not defteri, verilerin doğru yutulup alınıp alınamaydığını denetler ve sonuç veri dosyasını $(bin_FILE_NAME) adı ile doğrular.

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

İşteki son görev, not defteri yürütmesinin sonucunu denetler. Bir hata döndürürse, denetim hattı yürütme durumunu başarısız olarak ayarlar.

## <a name="putting-pieces-together"></a>Parçaları bir araya getirmek

Bu makalenin sonucu, aşağıdaki aşamalardan oluşan bir CI/CD Azure Ardışık
* CI
* QA'ya Dağılın
    * Databricks'e Dağıtma + ADF'ye dağıt
    * Entegrasyon Testi

Sahip olduğunuz hedef ortam sayısına eşit bir dizi ***Dağıtım*** aşaması içerir. Her ***Dağıtım*** aşaması, paralel olarak çalışan iki [dağıtım](https://docs.microsoft.com/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) ve çözümü ortamda sınamak için dağıtımlardan sonra çalışan bir [iş](https://docs.microsoft.com/azure/devops/pipelines/process/phases?view=azure-devops&tabs=yaml) içerir.

Boru hattının örnek bir uygulaması aşağıdaki ***yaml*** snippet monte edilir:

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
* [Azure Veri Fabrikası'nda sürekli entegrasyon ve teslimat](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment)
* [Azure Databricks için DevOps](https://marketplace.visualstudio.com/items?itemName=riserrad.azdo-databricks)
