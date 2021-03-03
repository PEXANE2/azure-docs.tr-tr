---
title: Sürekli tümleştirme ve teslim için otomatik yayımlama
description: Sürekli tümleştirme ve teslim için otomatik olarak yayımlama hakkında bilgi edinin.
ms.service: data-factory
author: nabhishek
ms.author: abnarain
ms.reviewer: maghan
ms.topic: conceptual
ms.date: 02/02/2021
ms.openlocfilehash: 49ec43e59989f3fdad8f5731867953cc7cbb5757
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101699717"
---
# <a name="automated-publishing-for-continuous-integration-and-delivery"></a>Sürekli tümleştirme ve teslim için otomatik yayımlama

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

## <a name="overview"></a>Genel Bakış

Sürekli tümleştirme, kod tabanınızda yapılan her değişikliği otomatik olarak test etme ve olası sürekli dağıtım, sürekli tümleştirme sırasında gerçekleşen teste ve bir hazırlama veya üretim sistemine yapılan değişiklikleri geri gönderme yöntemidir.

Azure Data Factory'de sürekli tümleştirme ve sürekli teslim (CI/CD) Data Factory işlem hatlarını bir ortamdan (geliştirme, test, üretim) diğerine taşıma anlamına gelir. Azure Data Factory, çeşitli ADF varlıklarınızın yapılandırmasını depolamak için [Azure Resource Manager şablonlarından](../azure-resource-manager/templates/overview.md) yararlanır (işlem hatları, veri kümeleri, veri akışları vb.). Bir veri fabrikasını başka bir ortama yükseltmek için önerilen iki yöntem vardır:

- Data Factory [Azure Pipelines](/azure/devops/pipelines/get-started/what-is-azure-pipelines)tümleştirme kullanılarak otomatik dağıtım.
- Azure Resource Manager ile Data Factory UX tümleştirmesi kullanarak Kaynak Yöneticisi şablonunu el ile karşıya yükleyin.

Daha fazla bilgi için bkz. [Azure Data Factory sürekli tümleştirme ve teslim](continuous-integration-deployment.md).

Bu makalede, sürekli dağıtım geliştirmeleri ve CI/CD için otomatik yayımlama özelliği üzerine odaklanıyoruz.

## <a name="continuous-deployment-improvements"></a>Sürekli dağıtım geliştirmeleri

"Otomatik yayımlama" özelliği, ADF UX 'den *Tümünü doğrula* ve dışarı aktar *Azure Resource Manager (ARM) şablonu* özelliklerini doğrular ve mantığın genel kullanıma açık bir NPM paketiyle kullanılabilmesini sağlar [@microsoft/azure-data-factory-utilities](https://www.npmjs.com/package/@microsoft/azure-data-factory-utilities) . Bu, ADF Kullanıcı arabirimine gidip bir düğme tıklamasına gerek kalmadan, bu eylemleri programlı bir şekilde tetiklemeniz sağlar. Bu, CI/CD işlem hatlarına bir truer sürekli tümleştirme deneyimi sağlar.

### <a name="current-cicd-flow"></a>Geçerli CI/CD akışı

1. Her kullanıcı özel dallarında değişiklikler yapar.
2. Ana öğesine gönderim yasak, kullanıcıların değişiklik yapması için ana için bir PR oluşturması gerekir.
3. Kullanıcılar ADF Kullanıcı arabirimini yükledikten sonra Data Factory değişiklikleri dağıtmak ve Yayımla dalında ARM şablonlarını oluşturmak için Yayımla ' ya tıklamalıdır.
4. DevOps yayın işlem hattı yeni bir sürüm oluşturmak ve yayınlama dalına her yeni değişiklik gönderildiğinde ARM şablonunu dağıtmak üzere yapılandırılır.

![Geçerli CI/CD akışı](media/continuous-integration-deployment-improvements/current-ci-cd-flow.png)

### <a name="manual-step"></a>El ile adım

Geçerli CI/CD akışında UX, ARM şablonunu oluşturmak için gereken bir ara bu nedenle, bir kullanıcının, ADF şablonu oluşturmayı başlatması ve bunu bir hack 'ın bir bit olan Yayımla dalında bırakması için Yayımla ' ya tıklayın.

### <a name="the-new-cicd-flow"></a>Yeni CI/CD akışı

1. Her kullanıcı özel dallarında değişiklikler yapar.
2. Ana öğesine gönderim yasak, kullanıcıların değişiklik yapması için ana için bir PR oluşturması gerekir.
3. **Azure DevOps işlem hattı derlemesi, ana öğe üzerinde her yeni bir işleme yapıldığında kaynakları doğrular ve doğrulama başarılı olursa yapıt olarak bir ARM şablonu oluşturur.**
4. DevOps yayın işlem hattı yeni bir yayın oluşturacak ve yeni bir derleme kullanılabilir her seferinde ARM şablonunu dağıtan şekilde yapılandırılmıştır. 

![Yeni CI/CD akışı](media/continuous-integration-deployment-improvements/new-ci-cd-flow.png)

### <a name="what-changed"></a>Ne değişti?

- Artık bir DevOps derleme işlem hattı kullanan bir yapı sürecimiz var.
- Derleme işlem hattı ADFUtilities NPM paketini kullanır, bu, tüm kaynakları doğrular ve ARM şablonları (tek ve bağlantılı şablonlar) oluşturur.
- Derleme işlem hattı ADF kaynaklarını doğrulamadan ve ADF Kullanıcı arabirimi (Yayımla düğmesi) yerine ARM şablonu oluşturmaya sorumludur.
- DevOps yayın tanımı artık git yapıtı yerine bu yeni derleme işlem hattını kullanacaktır.

> [!NOTE]
> Mevcut mekanizmayı (adf_publish dalı) kullanmaya devam edebilir veya yeni akışı kullanabilirsiniz. Her ikisi de desteklenir. 

## <a name="package-overview"></a>Pakete genel bakış

Şu anda pakette bulunan iki komut vardır:
- ARM şablonunu dışarı aktarma
- Doğrulama

### <a name="export-arm-template"></a>ARM şablonunu dışarı aktarma

<rootFolder> <factoryId> Belirli bir klasörün kaynaklarını kullanarak ARM şablonunu dışarı aktarmak için NPM Run start Export [outputfolder] öğesini çalıştırın. Bu komut, ARM şablonunu oluşturmadan önce bir doğrulama denetimi ve de çalışır. Aşağıda bir örnek verilmiştir:

```
npm run start export C:\DataFactories\DevDataFactory /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/testResourceGroup/providers/Microsoft.DataFactory/factories/DevDataFactory ArmTemplateOutput
```

- RootFolder, Data Factory kaynaklarının nerede bulunduğunu temsil eden zorunlu bir alandır.
- Factoryıd, Data Factory kaynak KIMLIĞINI şu biçimde temsil eden zorunlu bir alandır: "/Subscriptions/ <subId> /ResourceGroups/ <rgName> /providers/Microsoft.DataFactory/Factories/ <dfName> ".
- OutputFolder, oluşturulan ARM şablonunu kaydetmek için göreli yolu belirten isteğe bağlı bir parametredir.
 
> [!NOTE]
> Oluşturulan ARM şablonu, `Live` fabrika sürümünde yayımlanmaz. Dağıtım bir CI/CD işlem hattı kullanılarak yapılmalıdır. 
 

### <a name="validate"></a>Doğrulama

<rootFolder> <factoryId> Belirli bir klasörün tüm kaynaklarını doğrulamak için NPM çalıştırma başlatma doğrulaması ' nı çalıştırın. Aşağıda bir örnek verilmiştir:
    
```
npm run start validate C:\DataFactories\DevDataFactory /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/testResourceGroup/providers/Microsoft.DataFactory/factories/DevDataFactory
```

- RootFolder, Data Factory kaynaklarının nerede bulunduğunu temsil eden zorunlu bir alandır.
- Factoryıd, Data Factory kaynak KIMLIĞINI şu biçimde temsil eden zorunlu bir alandır: "/Subscriptions/ <subId> /ResourceGroups/ <rgName> /providers/Microsoft.DataFactory/Factories/ <dfName> ".


## <a name="create-an-azure-pipeline"></a>Azure işlem hattı oluşturma

NPM paketleri çeşitli yollarla tüketilirken, birincil avantajlardan biri bir [Azure Işlem hattı](https://nam06.safelinks.protection.outlook.com/?url=https:%2F%2Fdocs.microsoft.com%2F%2Fazure%2Fdevops%2Fpipelines%2Fget-started%2Fwhat-is-azure-pipelines%3Fview%3Dazure-devops%23:~:text%3DAzure%2520Pipelines%2520is%2520a%2520cloud%2Cit%2520available%2520to%2520other%2520users.%26text%3DAzure%2520Pipelines%2520combines%2520continuous%2520integration%2Cship%2520it%2520to%2520any%2520target.&data=04%7C01%7Cabnarain%40microsoft.com%7C5f064c3d5b7049db540708d89564b0bc%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C637423607000268277%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C1000&sdata=jo%2BkIvSBiz6f%2B7kmgqDN27TUWc6YoDanOxL9oraAbmA%3D&reserved=0)aracılığıyla tüketilecektir. İşbirliği dalınızdaki her bir birleştirmede, önce kodun tümünü doğrulayan ve sonra ARM şablonunu bir yayın işlem hattı tarafından tüketilen bir [Yapı yapıtına](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2F%2Fazure%2Fdevops%2Fpipelines%2Fartifacts%2Fbuild-artifacts%3Fview%3Dazure-devops%26tabs%3Dyaml%23how-do-i-consume-artifacts&data=04%7C01%7Cabnarain%40microsoft.com%7C5f064c3d5b7049db540708d89564b0bc%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C637423607000278113%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C1000&sdata=dN3t%2BF%2Fzbec4F28hJqigGANvvedQoQ6npzegTAwTp1A%3D&reserved=0) dışarı aktaran bir işlem hattı tetiklenebilir. **Geçerli CI/CD işleminden farklı olarak, yayın işlem hattınızı mevcut dal yerine bu yapıta işaret edecek `adf_publish` .**

Başlamak için aşağıdaki adımları izleyin:

1.  Bir Azure DevOps projesi açın ve "işlem hatları" bölümüne gidin. "Yeni işlem hattı" seçeneğini belirleyin.

    ![Yeni İşlem Hattı](media/continuous-integration-deployment-improvements/new-pipeline.png)
    
2.  Ardışık düzen YAML betiğinizi kaydetmek istediğiniz depoyu seçin. ADF kaynaklarınızın aynı deposundaki bir *Yapı* klasörüne kaydedilmesini öneririz. Depodaki dosyada **package.js** olduğundan ve paket adının (aşağıdaki örnekte gösterildiği gibi) bulunduğundan emin olun.

    ```json
    {
        "scripts":{
            "build":"node node_modules/@microsoft/azure-data-factory-utilities/lib/index"
        },
        "dependencies":{
            "@microsoft/azure-data-factory-utilities":"^0.1.3"
        }
    } 
    ```
    
3.  *Başlatıcı işlem hattı* seçin. YAML dosyasını karşıya yüklediyseniz veya birleştirdiyseniz (aşağıda gösterildiği gibi), doğrudan bu dosyayı da işaret edebilir ve düzenleyebilirsiniz. 

    ![Başlatıcı işlem hattı](media/continuous-integration-deployment-improvements/starter-pipeline.png) 

    ```yaml
    # Sample YAML file to validate and export an ARM template into a Build Artifact
    # Requires a package.json file located in the target repository
    
    trigger:
    - main #collaboration branch
    
    pool:
      vmImage: 'ubuntu-latest'
    
    steps:
    
    # Installs Node and the npm packages saved in your package.json file in the build
    
    - task: NodeTool@0
      inputs:
        versionSpec: '10.x'
      displayName: 'Install Node.js'
    
    - task: Npm@1
      inputs:
        command: 'install'
        verbose: true
      displayName: 'Install npm package'
    
    # Validates all of the ADF resources in the repository. You will get the same validation errors as when "Validate All" is clicked
    # Enter the appropriate subscription and name for the source factory 
    
    - task: Npm@1
      inputs:
        command: 'custom'
        customCommand: 'run build validate $(Build.Repository.LocalPath) /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/testResourceGroup/providers/Microsoft.DataFactory/factories/yourFactoryName'
      displayName: 'Validate'
    
    # Validate and then generate the ARM template into the destination folder. Same as clicking "Publish" from UX
    # The ARM template generated is not published to the ‘Live’ version of the factory. Deployment should be done using a CI/CD pipeline. 
    
    - task: Npm@1
      inputs:
        command: 'custom'
        customCommand: 'run build export $(Build.Repository.LocalPath) /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/testResourceGroup/providers/Microsoft.DataFactory/factories/yourFactoryName "ArmTemplate"'
      displayName: 'Validate and Generate ARM template'
    
    # Publish the Artifact to be used as a source for a release pipeline
    
    - task: PublishPipelineArtifact@1
      inputs:
        targetPath: '$(Build.Repository.LocalPath)/ArmTemplate'
        artifact: 'ArmTemplates'
        publishLocation: 'pipeline'
    ```

4.  YAML kodunuzu girin. YAML dosyasını ve bir başlangıç noktası olarak kullanmayı öneririz.
5.  Kaydet ve Çalıştır. YAML kullanılıyorsa, "ana" dal her güncelleştirildiği zaman tetiklenir.

## <a name="next-steps"></a>Sonraki adımlar

Data Factory 'de sürekli tümleştirme ve teslim hakkında daha fazla bilgi edinin: 

- [Azure Data Factory 'de sürekli tümleştirme ve teslim](continuous-integration-deployment.md).
