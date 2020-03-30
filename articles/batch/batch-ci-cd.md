---
title: HPC çözümleri oluşturmak & dağıtmak için Azure Ardışık Hatlar'ı kullanın - Azure Toplu İş
description: Azure Toplu İş'te çalışan bir HPC uygulaması için yapı/sürüm ardışık hattını nasıl dağıtlayacağınızı öğrenin.
author: chrisreddington
ms.author: chredd
ms.date: 03/28/2019
ms.topic: conceptual
ms.custom: fasttrack-new
services: batch
ms.service: batch
ms.openlocfilehash: 50cb711dfd16c2a8718d13ba9255ace1e7e3e26d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79533139"
---
# <a name="use-azure-pipelines-to-build-and-deploy-hpc-solutions"></a>HPC çözümleri oluşturmak ve dağıtmak için Azure Ardışık Hatları'nı kullanın

Azure DevOps hizmetleri, geliştirme ekipleri tarafından özel bir uygulama yaparken kullanılan çeşitli araçlar sağlar. Azure DevOps tarafından sağlanan araçlar, yüksek performanslı bilgi işlem çözümlerinin otomatik olarak oluşturulması ve test edilmesi anlamına gelebilir. Bu makalede, Azure Toplu İş'te dağıtılan yüksek performanslı bir işlem çözümü için Azure Pipelines kullanarak sürekli bir tümleştirme (CI) ve sürekli dağıtım (CD) nasıl kurulansüreceğini gösterilmiştir.

Azure Pipelines, yazılım oluşturma, dağıtma, test etme ve izleme için çeşitli modern CI/CD işlemleri sağlar. Bu işlemler yazılım tesliminizi hızlandırarak altyapıve işlemleri desteklemek yerine kodunuza odaklanmanızı sağlar.

## <a name="create-an-azure-pipeline"></a>Azure Ardışık Bir Yol Oluşturma

Bu örnekte, bir Azure Toplu Iş altyapısı dağıtmak ve bir uygulama paketi serbest bırakmak için bir yapı ve sürüm ardışık yapı oluşturacağız. Kodun yerel olarak geliştirildiğini varsayarsak, bu genel dağıtım akışıdır:

![Boru Hattımızdaki dağıtım akışını gösteren diyagram](media/batch-ci-cd/DeploymentFlow.png)

### <a name="setup"></a>Kurulum

Bu makaledeki adımları izlemek için bir Azure DevOps kuruluşuna ve bir ekip projesine ihtiyacınız var.

* [Azure DevOps Organizasyonu Oluşturma](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization?view=azure-devops)
* [Azure DevOps'lerde proje oluşturma](https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops)

### <a name="source-control-for-your-environment"></a>Ortamınız için kaynak denetimi

Kaynak denetimi, ekiplerin kod tabanında yapılan değişiklikleri izlemesine ve kodun önceki sürümlerini incelemesine olanak tanır.

Genellikle, kaynak denetimi yazılım kodu ile el ele düşünülür. Peki ya altta yatan altyapı? Bu, bizi Temel Altyapımızı bildirimsel olarak tanımlamak için Azure Kaynak Yöneticisi şablonlarını veya diğer açık kaynak alternatiflerini kullanacağımız Kod Olarak Altyapı'ya getirir.

Bu örnek, büyük ölçüde kaynak yöneticisi şablonları (JSON Belgeleri) ve varolan ikili bir dizi dayanır. Bu örnekleri deponuza kopyalayabilir ve Azure DevOps'a taşıyabilirsiniz.

Bu örnekte kullanılan codebase yapısı aşağıdakilere benzer;

* Bir dizi Azure Kaynak Yöneticisi şablonu içeren **bir kol şablonları** klasörü. Şablonlar bu makalede açıklanmıştır.
* Ffmpeg örnekli [Azure Toplu .NET Dosya İşleme'nin](https://github.com/Azure-Samples/batch-dotnet-ffmpeg-tutorial) bir kopyası olan **istemci uygulama** klasörü. Bu makale için gerekli değildir.
* [Ffmpeg 3.4](https://ffmpeg.zeranoe.com/builds/win64/static/ffmpeg-3.4-win64-static.zip)Windows 64-bit sürümü olan **bir hpc-uygulama** klasörü.
* Bir **boru hattı** klasörü. Bu, yapı işlemimizin ana hatlarını özetleyen bir YAML dosyası içerir. Bu makalede ele alınmıştır.

Bu bölüm, sürüm denetimi ve Kaynak Yöneticisi şablonları tasarlama aşina olduğunuzu varsayar. Bu kavramlara aşina değilseniz, daha fazla bilgi için aşağıdaki sayfalara bakın.

* [Kaynak denetimi nedir?](https://docs.microsoft.com/azure/devops/user-guide/source-control?view=azure-devops)
* [Azure Kaynak Yöneticisi Şablonlarının yapısını ve sözdizimini anlama](../azure-resource-manager/templates/template-syntax.md)

#### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Bu örnek, çözümümüzü dağıtmak için birden çok Kaynak Yöneticisi şablonundan yararlanır. Bunu yapmak için, belirli bir işlevsellik parçasını uygulayan bir dizi yetenek şablonu (birimler veya modüllere benzer) kullanırız. Ayrıca, altta yatan yetenekleri bir araya getirmekten sorumlu uçtan uca bir çözüm şablonu da kullanıyoruz. Bu yaklaşımın birkaç faydası vardır:

* Temel yetenek şablonları tek tek birim test edilebilir.
* Temel yetenek şablonları, bir kuruluşun içinde bir standart olarak tanımlanabilir ve birden çok çözümde yeniden kullanılabilir.

Bu örnekte, üç şablon dağıtan uçtan uca bir çözüm şablonu (deployment.json) vardır. Altta yatan şablonlar, çözümün belirli bir yönünü dağıtmaktan sorumlu yetenek şablonlarıdır.

![Azure Kaynak Yöneticisi şablonlarını kullanarak Bağlantılı Şablon Yapısı örneği](media/batch-ci-cd/ARMTemplateHierarchy.png)

Bakacağız ilk şablon bir Azure Depolama Hesabı içindir. Çözümümüz, uygulamayı Toplu Hesabımıza dağıtmak için bir depolama hesabı gerektirir. Depolama Hesapları için Kaynak Yöneticisi şablonları yaparken [Microsoft.Storage kaynak türleri için Kaynak Yöneticisi şablonu başvuru kılavuzunu](https://docs.microsoft.com/azure/templates/microsoft.storage/allversions) bilmek te önemlidir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string",
            "metadata": {
                 "description": "Name of the Azure Storage Account"
             }
         }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('accountName')]",
            "sku": {
                "name": "Standard_LRS"
            },
            "apiVersion": "2018-02-01",
            "location": "[resourceGroup().location]",
            "properties": {}
        }
    ],
    "outputs": {
        "blobEndpoint": {
          "type": "string",
          "value": "[reference(resourceId('Microsoft.Storage/storageAccounts', parameters('accountName'))).primaryEndpoints.blob]"
        },
        "resourceId": {
          "type": "string",
          "value": "[resourceId('Microsoft.Storage/storageAccounts', parameters('accountName'))]"
        }
    }
}
```

Ardından, Azure Toplu İş Hesabı şablonuna bakacağız. Azure Toplu İş Hesabı, havuzlar arasında çok sayıda uygulamayı (makine gruplandırmaları) çalıştırmak için bir platform görevi görür. Toplu Hesaplar için Kaynak Yöneticisi şablonları yaparken [Microsoft.Batch kaynak türleri için Kaynak Yöneticisi şablonu başvuru kılavuzunu](https://docs.microsoft.com/azure/templates/microsoft.batch/allversions) bilmek te önemlidir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "batchAccountName": {
           "type": "string",
           "metadata": {
                "description": "Name of the Azure Batch Account"
            }
        },
        "storageAccountId": {
           "type": "string",
           "metadata": {
                "description": "ID of the Azure Storage Account"
            }
        }
    },
    "variables": {},
    "resources": [
        {
            "name": "[parameters('batchAccountName')]",
            "type": "Microsoft.Batch/batchAccounts",
            "apiVersion": "2017-09-01",
            "location": "[resourceGroup().location]",
            "properties": {
              "poolAllocationMode": "BatchService",
              "autoStorage": {
                  "storageAccountId": "[parameters('storageAccountId')]"
              }
            }
          }
    ],
    "outputs": {}
}
```

Sonraki şablon, Azure Toplu Birleştirme Havuzu (uygulamalarımızı işlemek için arka uç makineleri) oluşturan bir örneği gösterir. Toplu Hesap Havuzları için Kaynak Yöneticisi şablonları yaparken [Microsoft.Batch kaynak türleri için Kaynak Yöneticisi şablonu başvuru kılavuzunu](https://docs.microsoft.com/azure/templates/microsoft.batch/allversions) bilmek te önemlidir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "batchAccountName": {
           "type": "string",
           "metadata": {
                "description": "Name of the Azure Batch Account"
           }
        },
        "batchAccountPoolName": {
            "type": "string",
            "metadata": {
                 "description": "Name of the Azure Batch Account Pool"
             }
         }
    },
    "variables": {},
    "resources": [
        {
            "name": "[concat(parameters('batchAccountName'),'/', parameters('batchAccountPoolName'))]",
            "type": "Microsoft.Batch/batchAccounts/pools",
            "apiVersion": "2017-09-01",
            "properties": {
                "deploymentConfiguration": {
                    "virtualMachineConfiguration": {
                        "imageReference": {
                            "publisher": "Canonical",
                            "offer": "UbuntuServer",
                            "sku": "18.04-LTS",
                            "version": "latest"
                        },
                        "nodeAgentSkuId": "batch.node.ubuntu 18.04"
                    }
                },
                "vmSize": "Standard_D1_v2"
            }
          }
    ],
    "outputs": {}
}
```

Son olarak, bir orkestratör benzer davranan bir şablon var. Bu şablon, yetenek şablonlarını dağıtmakla yükümlüdür.

[Ayrıca, bağlantılı Azure Kaynak Yöneticisi şablonları oluşturma](../azure-resource-manager/templates/template-tutorial-create-linked-templates.md) hakkında daha fazla bilgi bulabilirsiniz.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "templateContainerUri": {
           "type": "string",
           "metadata": {
                "description": "URI of the Blob Storage Container containing the Azure Resouce Manager templates"
            }
        },
        "templateContainerSasToken": {
           "type": "string",
           "metadata": {
                "description": "The SAS token of the container containing the Azure Resouce Manager templates"
            }
        },
        "applicationStorageAccountName": {
            "type": "string",
            "metadata": {
                 "description": "Name of the Azure Storage Account"
            }
         },
        "batchAccountName": {
            "type": "string",
            "metadata": {
                 "description": "Name of the Azure Batch Account"
            }
         },
         "batchAccountPoolName": {
             "type": "string",
             "metadata": {
                  "description": "Name of the Azure Batch Account Pool"
              }
          }
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "storageAccountDeployment",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                  "uri": "[concat(parameters('templateContainerUri'), '/storageAccount.json', parameters('templateContainerSasToken'))]",
                  "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "accountName": {"value": "[parameters('applicationStorageAccountName')]"}
                }
            }
        },  
        {
            "apiVersion": "2017-05-10",
            "name": "batchAccountDeployment",
            "type": "Microsoft.Resources/deployments",
            "dependsOn": [
                "storageAccountDeployment"
            ],
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                  "uri": "[concat(parameters('templateContainerUri'), '/batchAccount.json', parameters('templateContainerSasToken'))]",
                  "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "batchAccountName": {"value": "[parameters('batchAccountName')]"},
                    "storageAccountId": {"value": "[reference('storageAccountDeployment').outputs.resourceId.value]"}
                }
            }
        },
        {
            "apiVersion": "2017-05-10",
            "name": "poolDeployment",
            "type": "Microsoft.Resources/deployments",
            "dependsOn": [
                "batchAccountDeployment"
            ],
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                  "uri": "[concat(parameters('templateContainerUri'), '/batchAccountPool.json', parameters('templateContainerSasToken'))]",
                  "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "batchAccountName": {"value": "[parameters('batchAccountName')]"},
                    "batchAccountPoolName": {"value": "[parameters('batchAccountPoolName')]"}
                }
            }
        }
    ],
    "outputs": {}
}
```

#### <a name="the-hpc-solution"></a>HPC Çözümü

Altyapı ve yazılım kod olarak tanımlanabilir ve aynı depoda birleştirilmiş olabilir.

Bu çözüm için, ffmpeg uygulama paketi olarak kullanılır. FFMpeg paketi [buradan](https://ffmpeg.zeranoe.com/builds/win64/static/ffmpeg-3.4-win64-static.zip)indirebilirsiniz.

![Örnek Git Depo Yapısı](media/batch-ci-cd/git-repository.jpg)

Bu deponun dört ana bölümü vardır:

* Altyapımızı Kod Olarak Depolayan **kol şablonları klasörü**
* FFMpeg için ikili leri içeren **hpc-uygulama** klasörü
* Yapı **ardışık** bölgemizin tanımını içeren boru hatları klasörü.
* **İsteğe Bağlı**: .NET uygulaması için kod depolayacak **istemci uygulama** klasörü. Bunu örnekte kullanmayız, ancak kendi projenizde, bir istemci uygulaması aracılığıyla HPC Toplu Uygulama'nın çalıştırmalarını yürütmek isteyebilirsiniz.

> [!NOTE]
> Bu, kod tabanına bir yapının sadece bir örneğidir. Bu yaklaşım, uygulama, altyapı ve boru hattı kodunun aynı depoda depolandığını göstermek amacıyla kullanılır.

Artık kaynak kodu ayarlı, biz ilk yapı başlayabilirsiniz.

## <a name="continuous-integration"></a>Sürekli tümleştirme

Azure DevOps Hizmetleri içinde yer [alan Azure Ardışık Hatları,](https://docs.microsoft.com/azure/devops/pipelines/get-started/?view=azure-devops)uygulamalarınız için bir yapı, test ve dağıtım ardışık hattı uygulamanıza yardımcı olur.

Ardışık alanınızın bu aşamasında, testler genellikle kodu doğrulamak ve yazılımın uygun parçalarını oluşturmak için çalıştırılır. Testlerin sayısı ve türleri ve çalıştırdığınız ek görevler, daha geniş yapı ve sürüm stratejinize bağlıdır.

## <a name="preparing-the-hpc-application"></a>HPC uygulamasının hazırlanması

Bu örnekte, **hpc-uygulama** klasörüne odaklanacağız. **hpc-uygulama** klasörü, Azure Toplu İş hesabı nın içinden çalışacak ffmpeg yazılımıdır.

1. Azure DevOps kuruluşunuzdaki Azure Ardışık Hatlarının Yapılar bölümüne gidin. Yeni bir **ardışık hat lar**oluşturun.

    ![Yeni Yapı Ardışık Hattı Oluşturma](media/batch-ci-cd/new-build-pipeline.jpg)

1. Yapı ardışık hattı oluşturmak için iki seçeneğiniz var:

    a. [Görsel Tasarımcı'yı kullanma](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=azure-devops&tabs=new-nav). Bunu kullanmak **için, Yeni ardışık hat lar** sayfasındaki "Görsel tasarımcıyı kullan"ı tıklatın.

    b. [YAML Oluşturur kullanma](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml?view=azure-devops). Yeni ardışık hatlar sayfasında Azure Repos veya GitHub seçeneğini tıklatarak yeni bir YAML ardışık kaynak oluşturabilirsiniz. Alternatif olarak, aşağıdaki örneği kaynak denetiminizde saklayabilir ve Visual Designer'ı tıklayarak ve ardından YAML şablonunu kullanarak varolan bir YAML dosyasına başvurun.

    ```yml
    # To publish an application into Azure Batch, we need to
    # first zip the file, and then publish an artifact, so that
    # we can take the necessary steps in our release pipeline.
    steps:
    # First, we Zip up the files required in the Batch Account
    # For this instance, those are the ffmpeg files
    - task: ArchiveFiles@2
      displayName: 'Archive applications'
      inputs:
        rootFolderOrFile: hpc-application
        includeRootFolder: false
        archiveFile: '$(Build.ArtifactStagingDirectory)/package/$(Build.BuildId).zip'
    # Publish that zip file, so that we can use it as part
    # of our Release Pipeline later
    - task: PublishPipelineArtifact@0
      inputs:
        artifactName: 'hpc-application'
        targetPath: '$(Build.ArtifactStagingDirectory)/package'
    ```

1. Yapı gerektiği gibi yapılandırıldıktan **sonra, & Sırasını Kaydet'i**seçin. Sürekli tümleştirme etkinleştirilmişse **(Tetikleyiciler** bölümünde), depoya yeni bir taahhüt yapıldığında yapı otomatik olarak tetiklenir ve yapıda ayarlanan koşulları karşılar.

    ![Varolan Bir Yapı Boru Hattı örneği](media/batch-ci-cd/existing-build-pipeline.jpg)

1. Azure Ardışık Hatları'nın **Yapı** bölümüne giderek Azure DevOps'deki yapınızın ilerlemesiyle ilgili canlı güncellemeleri görüntüleyin. Yapı tanımınızdan uygun yapıyı seçin.

    ![Yapınızdan canlı çıktıları görüntüleme](media/batch-ci-cd/Build-1.jpg)

> [!NOTE]
> HPC Toplu Uygulamanızı yürütmek için bir istemci uygulaması kullanıyorsanız, bu uygulama için ayrı bir yapı tanımı oluşturmanız gerekir. [Azure Ardışık İşler](https://docs.microsoft.com/azure/devops/pipelines/get-started/index?view=azure-devops) belgelerinde bir dizi nasıl yapılacağını gösteren kılavuzlar bulabilirsiniz.

## <a name="continuous-deployment"></a>Sürekli dağıtım

Azure Pipelines, uygulamanızı ve temel altyapınızı dağıtmak için de kullanılır. [Sürüm ardışık lıkları,](https://docs.microsoft.com/azure/devops/pipelines/release) sürekli dağıtım sağlayan ve sürüm işleminizi otomatikleştiren bileşendir.

### <a name="deploying-your-application-and-underlying-infrastructure"></a>Uygulamanızı ve temel altyapınızı dağıtma

Altyapının dağıtılmasında yer alan birkaç adım vardır. [Bağlantılı şablonlar](../azure-resource-manager/templates/linked-templates.md)kullandığımız için, bu şablonların ortak bir bitiş noktasından (HTTP veya HTTPS) erişilebilir olması gerekir. Bu, GitHub'da bir depo veya Azure Blob Depolama Hesabı veya başka bir depolama konumu olabilir. Yüklenen şablon yapıları, özel bir modda tutulabildikleri, ancak bir tür paylaşılan erişim imzası (SAS) belirteci kullanılarak erişilebildikleri için güvenli kalabilir. Aşağıdaki örnek, azure depolama örneğinden şablonlarla bir altyapının nasıl dağıtılanacağını gösterir.

1. Yeni **Sürüm Tanımı**oluşturun ve boş bir tanım seçin. Daha sonra yeni oluşturulan ortamı boru hattımızla ilgili bir şeyle yeniden adlandırmamız gerekir.

    ![İlk Sürüm Boru Hattı](media/batch-ci-cd/Release-0.jpg)

1. HPC uygulamamızın çıktısını almak için Yapı Ardışık Hattı'na bağımlılık oluşturun.

    > [!NOTE]
    > Bir kez daha, Sürüm Tanımı'nın içinde görevler oluşturulduğunda bu gerekli olacağından, **Kaynak Diğer Ad'a**dikkat edin.

    ![Uygun yapı ardışık yapısında HPCApplicationPackage'a bir yapı bağlantısı oluşturma](media/batch-ci-cd/Release-1.jpg)

1. Başka bir yapıya bağlantı oluşturun, bu kez bir Azure Repo'su. Bu, deponuzda depolanan Kaynak Yöneticisi şablonlarına erişmek için gereklidir. Kaynak Yöneticisi şablonları derleme gerektirmeyin, bunları bir yapı ardışık hattı ndan geçirmeniz gerekmez.

    > [!NOTE]
    > Bir kez daha, Sürüm Tanımı'nın içinde görevler oluşturulduğunda bu gerekli olacağından, **Kaynak Diğer Ad'a**dikkat edin.

    ![Azure Deposu'na yapı bağlantısı oluşturma](media/batch-ci-cd/Release-2.jpg)

1. **Değişkenler** bölümüne gidin. Ardışık ardınızda bir dizi değişken oluşturmanız önerilir, böylece aynı bilgileri birden çok göreviçin girmezsiniz. Bunlar, bu örnekte kullanılan değişkenlerdir ve dağıtımı nasıl etkiledikleridir.

    * **applicationStorageAccountName**: HPC uygulama ikililerini tutmak için Depolama Hesabının adı
    * **batchAccountApplicationName**: Azure Toplu İş Hesabındaki uygulamanın adı
    * **batchAccountName**: Azure Toplu İş Hesabının Adı
    * **batchAccountPoolName**: İşlemi yapan VM havuzunun adı
    * **batchApplicationId**: Azure Toplu İşlem uygulaması için benzersiz kimlik
    * **batchApplicationVersion**: Toplu uygulamanızın anlamsal sürümü (yani ffmpeg ikilileri)
    * **konum**: Azure Kaynaklarının dağıtılacak konumu
    * **resourceGroupName**: Oluşturulacak Kaynak Grubunun adı ve kaynaklarınızın dağıtılanacağı yer
    * **storageAccountName**: Bağlı Kaynak Yöneticisi şablonlarını tutmak için Depolama Hesabının adı

    ![Azure Ardışık Hatları sürümü için ayarlanan değişkenler örneği](media/batch-ci-cd/Release-4.jpg)

1. Dev ortamının görevlerine gidin. Aşağıdaki anlık görüntüde altı görev görebilirsiniz. Bu görevler: sıkıştırılmış ffmpeg dosyalarını indirin, iç içe olan Kaynak Yöneticisi şablonlarını barındırmak için bir depolama hesabı dağıtın, bu Kaynak Yöneticisi şablonlarını depolama hesabına kopyalayın, toplu iş hesabını ve gerekli bağımlılıkları dağıtın, Azure Toplu İş Hesabı'nı ve uygulama paketini Azure Toplu İş Hesabı'na yükleyin.

    ![HPC Uygulamasını Azure Toplu İşlemine serbest bırakmak için kullanılan görevlere örnek](media/batch-ci-cd/Release-3.jpg)

1. **İndirme Ardışık Yapı (Önizleme)** görevini ekleyin ve aşağıdaki özellikleri ayarlayın:
    * **Görüntü Adı:** Uygulama Paketini Acenteye İndirin
    * **İndirmek için artifakı adı:** hpc-application
    * **İndirme yolu**: $(System.DefaultWorkingDirectory)

1. Yapıtlarınızı depolamak için bir Depolama Hesabı oluşturun. Çözümden varolan bir depolama hesabı kullanılabilir, ancak bağımsız örnek ve içeriğin yalıtımı için, yapıtlarımız (özellikle Kaynak Yöneticisi şablonları) için özel bir depolama hesabı oluşturuyoruz.

    Azure **Kaynak Grubu Dağıtım** görevini ekleyin ve aşağıdaki özellikleri ayarlayın:
    * **Görüntü Adı:** Kaynak Yöneticisi şablonları için Depolama Hesabı dağıtma
    * **Azure Aboneliği:** Uygun Azure Aboneliğini seçin
    * **Eylem**: Kaynak grubu oluşturma veya güncelleştirme
    * **Kaynak Grubu**: $(resourceGroupName)
    * **Yer**: $(yer)
    * **Şablon**: $(System.ArtifactsDirectory)/**{YourAzureRepoArtifactSourceAlias}**/arm-templates/storageAccount.json
    * **Geçersiz kılma şablonparametreleri**: -accountName $(storageAccountName)

1. Kaynak Denetimi'ndeki yapıları Depolama Hesabına yükleyin. Bunu gerçekleştirmek için bir Azure Pipeline görevi vardır. Bu görevin bir parçası olarak, Depolama Hesabı Kapsayıcısı URL'si ve SAS Belirteci Azure Ardışık Hatları'nda bir değişkene çıkarılabilir. Bu, aracı aşaması boyunca yeniden kullanılabileceğini zedler.

    Azure **Dosya Kopyalama** görevini ekleyin ve aşağıdaki özellikleri ayarlayın:
    * **Kaynak:** $(System.ArtifactsDirectory)/**{YourAzureRepoArtifactSourceAlias}**/arm-templates/
    * **Azure Bağlantı Türü**: Azure Kaynak Yöneticisi
    * **Azure Aboneliği:** Uygun Azure Aboneliğini seçin
    * **Hedef Türü**: Azure Blob
    * **RM Depolama Hesabı**: $(storageAccountName)
    * **Konteyner Adı**: şablonlar
    * **Depolama Konteyner URI**: templateContainerUri
    * **Depolama Konteyner SAS Belirteci**: templateContainerSasToken

1. Orkestratör şablonuna dağıtın. Daha önceki orkestratör şablonu hatırlayın, SAS belirteci ek olarak Depolama Hesabı Konteyner URL için parametreler olduğunu fark edeceksiniz. Kaynak Yöneticisi şablonunda gerekli olan değişkenlerin sürüm tanımının değişkenler bölümünde tutulduğunu veya başka bir Azure Ardışık Düzenleri görevinden (örneğin, Azure Blob Kopyalama görevinin bir parçası) ayarlandığını fark etmelisiniz.

    Azure **Kaynak Grubu Dağıtım** görevini ekleyin ve aşağıdaki özellikleri ayarlayın:
    * **Görüntü Adı:** Azure Toplu İşi Dağıtma
    * **Azure Aboneliği:** Uygun Azure Aboneliğini seçin
    * **Eylem**: Kaynak grubu oluşturma veya güncelleştirme
    * **Kaynak Grubu**: $(resourceGroupName)
    * **Yer**: $(yer)
    * **Şablon**: $(System.ArtifactsDirectory)/**{YourAzureRepoArtifactSourceAlias}**/arm-templates/deployment.json
    * **Şablon parametrelerini geçersiz kılmak:**```-templateContainerUri $(templateContainerUri) -templateContainerSasToken $(templateContainerSasToken) -batchAccountName $(batchAccountName) -batchAccountPoolName $(batchAccountPoolName) -applicationStorageAccountName $(applicationStorageAccountName)```

Azure Anahtar Kasası görevlerini kullanmak yaygın bir uygulamadır. Hizmet Sorumlusu (Azure Aboneliğinize bağlantı) uygun bir erişim ilkeleri kümesi varsa, sırları bir Azure Anahtar Kasasından karşıdan yükleyebilir ve ardışık ardınızda değişken olarak kullanılabilir. Sırrın adı ilişkili değerle ayarlanır. Örneğin, sshPassword'in bir sırrı sürüm tanımında $(sshPassword) ile başvurulabilir.

1. Sonraki adımlarAzure CLI'yi çağırır. Bunlardan ilki Azure Toplu İş'te bir uygulama oluşturmak için kullanılır. ve ilişkili paketleri yükleyin.

    Azure **CLI** görevini ekleyin ve aşağıdaki özellikleri ayarlayın:
    * **Görüntü Adı:** Azure Toplu İş Hesabı'nda uygulama oluşturma
    * **Azure Aboneliği:** Uygun Azure Aboneliğini seçin
    * **Komut Dosyası Yeri**: Satır Satır
    * **Satır Satır :**```az batch application create --application-id $(batchApplicationId) --name $(batchAccountName) --resource-group $(resourceGroupName)```

1. İkinci adım, ilişkili paketleri uygulamaya yüklemek için kullanılır. Bizim durumumuzda, ffmpeg dosyaları.

    Azure **CLI** görevini ekleyin ve aşağıdaki özellikleri ayarlayın:
    * **Görüntü Adı:** Paketi Azure Toplu İş Hesabına yükleme
    * **Azure Aboneliği:** Uygun Azure Aboneliğini seçin
    * **Komut Dosyası Yeri**: Satır Satır
    * **Satır Satır :**```az batch application package create --application-id $(batchApplicationId)  --name $(batchAccountName)  --resource-group $(resourceGroupName) --version $(batchApplicationVersion) --package-file=$(System.DefaultWorkingDirectory)/$(Release.Artifacts.{YourBuildArtifactSourceAlias}.BuildId).zip```

    > [!NOTE]
    > Uygulama paketinin sürüm numarası bir değişkenolarak ayarlanır. Bu, paketin önceki sürümlerinin üzerine yazmak sizin için çalışıyorsa ve Azure Toplu İş'e itilen paketin sürüm numarasını el ile denetlemek istiyorsanız kullanışlıdır.

1. Sürüm > Yeni sürüm oluştur'u seçerek yeni **bir sürüm oluşturun.** Tetiklendikten sonra, durumu görüntülemek için yeni sürümünüzün bağlantısını seçin.

1. Ortamınızın altındaki **Günlükler** düğmesini seçerek aracıdan canlı çıktıyı görüntüleyebilirsiniz.

    ![Sürümünüzün durumunu görüntüleme](media/batch-ci-cd/Release-5.jpg)

### <a name="testing-the-environment"></a>Çevreyi test etme

Ortam ayarlandıktan sonra, aşağıdaki testlerin başarıyla tamamlanabileceğini onaylayın.

PowerShell komut isteminden Azure CLI'yi kullanarak yeni Azure Toplu İş Hesabı'na bağlanın.

* Azure hesabınızda oturum `az login` açın ve kimlik doğrulaması için yönergeleri izleyin.
* Şimdi Toplu İşlem hesabının kimliğini doğrulayın:`az batch account login -g <resourceGroup> -n <batchAccount>`

#### <a name="list-the-available-applications"></a>Kullanılabilir uygulamaları listele

```azurecli
az batch application list -g <resourcegroup> -n <batchaccountname>
```

#### <a name="check-the-pool-is-valid"></a>Havuzun geçerli olup olmadığını kontrol edin

```azurecli
az batch pool list
```

Bu komutun `currentDedicatedNodes` çıktısından gelen değeri not edin. Bu değer bir sonraki testte ayarlanır.

#### <a name="resize-the-pool"></a>Havuzu yeniden boyutlandırma

İş ve görev sınama için kullanılabilir işlem düğümleri olacak şekilde havuzu yeniden boyutlandırın, yeniden boyutlandırma tamamlanana ve kullanılabilir düğümler bulunana kadar geçerli durumu görmek için havuz listesi komutunu kontrol edin

```azurecli
az batch pool resize --pool-id <poolname> --target-dedicated-nodes 4
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleye ek olarak, .NET ve Python kullanarak ffmpeg kullanan iki öğretici ler vardır. Basit bir uygulama aracılığıyla Toplu İşlem hesabıyla nasıl etkileşimde bulundurunizle ilgili daha fazla bilgi için bu eğitimlere bakın.

* [Python API'yi kullanarak Azure Toplu İşlemi ile paralel bir iş yükü çalıştırma](tutorial-parallel-python.md)
* [.NET API'sini kullanarak Azure Toplu İşlemile paralel bir iş yükü çalıştırma](tutorial-parallel-dotnet.md)
