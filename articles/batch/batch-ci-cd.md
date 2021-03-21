---
title: HPC çözümlerini derlemek & dağıtmak için Azure Pipelines kullanma
description: Azure Batch üzerinde çalışan bir HPC uygulaması için derleme/sürüm ardışık düzeni dağıtmayı öğrenin.
author: chrisreddington
ms.author: chredd
ms.date: 03/04/2021
ms.topic: how-to
ms.openlocfilehash: 7170044af58a508ff5a43751cc376f8b8d498444
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102435554"
---
# <a name="use-azure-pipelines-to-build-and-deploy-hpc-solutions"></a>HPC çözümleri derlemek ve dağıtmak için Azure Pipelines kullanma

Azure DevOps tarafından sunulan araçlar, yüksek performanslı bilgi işlem (HPC) çözümlerinin otomatik olarak oluşturulmasına ve test edilmesine çevrilebilir. [Azure Pipelines](/azure/devops/pipelines/get-started/what-is-azure-pipelines) , yazılım oluşturmaya, dağıtmaya, test etmeye ve izlemeye yönelik bir dizi modern sürekli TÜMLEŞTIRME (CI) ve sürekli DAĞıTıM (CD) işlemi sağlar. Bu işlemler, yazılım teslimatını hızlandırarak altyapıyı ve işlemlerini desteklemek yerine kodunuza odaklanmanızı sağlar.

Bu makalede, Azure Batch üzerinde dağıtılan HPC çözümleri için [Azure Pipelines](/azure/devops/pipelines/get-started/what-is-azure-pipelines) kullanarak CI/CD işlemlerinin nasıl ayarlanacağı açıklanır.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları izlemek için bir [Azure DevOps kuruluşunun](/azure/devops/organizations/accounts/create-organization)olması gerekir. Ayrıca, [Azure DevOps 'da bir proje oluşturmanız](/azure/devops/organizations/projects/create-project)gerekecektir.

Başlamadan önce [kaynak denetimi](/azure/devops/user-guide/source-control) ve [Azure Resource Manager şablonu sözdiziminin](../azure-resource-manager/templates/template-syntax.md) temel olarak anlaşılmasına yardımcı olur.

## <a name="create-an-azure-pipeline"></a>Azure işlem hattı oluşturma

Bu örnekte, bir Azure Batch altyapısını dağıtmak ve bir uygulama paketini serbest bırakmak için derleme ve sürüm işlem hattı oluşturacaksınız. Kodun yerel olarak geliştirildiği varsayıldığında, bu genel dağıtım akışdır:

![İşlem hattının dağıtım akışını gösteren diyagram,](media/batch-ci-cd/DeploymentFlow.png)

Bu örnek, çeşitli Azure Resource Manager şablonları ve var olan ikilileri kullanır. Bu örnekleri deponuza kopyalayabilir ve bunları Azure DevOps 'a gönderebilirsiniz.

### <a name="understand-the-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını anlayın

Bu örnek, çözümü dağıtmak için çeşitli Azure Resource Manager şablonları kullanır. Üç yetenek şablonu (birimlere veya modüllere benzer şekilde) belirli bir işlev parçasını uygulamak için kullanılır. Uçtan uca bir çözüm şablonu (deployment.json), bu temel yetenek şablonlarını dağıtmak için kullanılır. Bu [bağlantılı şablon yapısı ](../azure-resource-manager/templates/deployment-tutorial-linked-template.md) , her bir yetenek şablonunun çözümler arasında tek tek test ve yeniden kullanılabilir olmasını sağlar.

![Azure Resource Manager şablonları kullanarak bağlantılı şablon yapısını gösteren diyagram.](media/batch-ci-cd/ARMTemplateHierarchy.png)

Bu şablon, uygulamayı Batch hesabına dağıtmak için gerekli olan bir Azure depolama hesabı tanımlar. Ayrıntılı bilgi için bkz. [Microsoft. Storage kaynak türleri için Kaynak Yöneticisi şablonu başvuru kılavuzu](/azure/templates/microsoft.storage/allversions).

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

Sonraki şablon bir [Azure Batch hesabını](accounts.md)tanımlar. Batch hesabı, [havuzlar](nodes-and-pools.md#pools)genelinde çok sayıda uygulama çalıştırmak için bir platform işlevi görür. Ayrıntılı bilgi için, [Microsoft.Batch kaynak türleri için Kaynak Yöneticisi şablonu başvuru kılavuzuna](/azure/templates/microsoft.batch/allversions)bakın.

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

Sonraki şablon Batch hesabında bir Batch havuzu oluşturur. Ayrıntılı bilgi için, [Microsoft.Batch kaynak türleri için Kaynak Yöneticisi şablonu başvuru kılavuzuna](/azure/templates/microsoft.batch/allversions)bakın.

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

Son şablon, temel alınan üç yetenek şablonunu dağıtan bir Orchestrator işlevi görür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "templateContainerUri": {
           "type": "string",
           "metadata": {
                "description": "URI of the Blob Storage Container containing the Azure Resource Manager templates"
            }
        },
        "templateContainerSasToken": {
           "type": "string",
           "metadata": {
                "description": "The SAS token of the container containing the Azure Resource Manager templates"
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

### <a name="understand-the-hpc-solution"></a>HPC çözümünü anlama

Daha önce belirtildiği gibi, bu örnek birkaç Azure Resource Manager şablonu ve var olan ikilileri kullanır. Bu örnekleri deponuza kopyalayabilir ve bunları Azure DevOps 'a gönderebilirsiniz.

Bu çözüm için, FFmpeg uygulama paketi olarak kullanılır. Henüz yoksa [FFmpeg paketini indirebilirsiniz](https://github.com/GyanD/codexffmpeg/releases/tag/4.3.1-2020-11-08) .

![Depo yapısının ekran görüntüsü.](media/batch-ci-cd/git-repository.jpg)

Bu deponun dört ana bölümü vardır:

- Azure Resource Manager şablonlarını içeren bir **ARM-Templates** klasörü
- [FFmpeg 4.3.1](https://github.com/GyanD/codexffmpeg/releases/tag/4.3.1-2020-11-08)'ın Windows 64 bit sürümünü içeren bir **HPC-Application** klasörü.
- Yapı işlem hattı işlemini tanımlayan bir YAML dosyası içeren bir **ardışık düzen** klasörü.
- İsteğe bağlı: [FFmpeg örneğiyle Azure Batch .NET dosya işlemenin](https://github.com/Azure-Samples/batch-dotnet-ffmpeg-tutorial) bir kopyası olan bir **istemci-uygulama** klasörü. Bu uygulama bu makale için gerekli değildir.


> [!NOTE]
> Bu, bir kod temelinin yapısına yalnızca bir örnektir. Bu yaklaşım, uygulamanın, altyapının ve işlem hattı kodunun aynı depoda depolandığını gösteren amaçlar için kullanılır.

Kaynak kodu ayarlandığına göre, ilk derlemeyi başlatabilirsiniz.

## <a name="continuous-integration"></a>Sürekli tümleştirme

Azure DevOps Services içinde [Azure Pipelines](/azure/devops/pipelines/get-started/), uygulamalarınız için bir derleme, test ve dağıtım işlem hattı uygulamanıza yardımcı olur.

İşlem hattının bu aşamasında, testler genellikle kodu doğrulamak ve yazılımın uygun parçalarını derlemek için çalıştırılır. Testlerin sayısı ve türleri ve çalıştırdığınız ek görevler, daha geniş derleme ve yayın stratejinize göre değişir.

## <a name="prepare-the-hpc-application"></a>HPC uygulamasını hazırlama

Bu bölümde **HPC-Application** klasörüyle çalışacaksınız. Bu klasör Azure Batch hesabı içinde çalışacak yazılımı (FFmpeg) içerir.

1. Azure DevOps kuruluşunuzda Azure Pipelines yapılar bölümüne gidin. Yeni bir işlem **hattı** oluşturun.

    ![Yeni ardışık düzen ekranının ekran görüntüsü.](media/batch-ci-cd/new-build-pipeline.jpg)

1. Derleme işlem hattı oluşturmak için iki seçeneğiniz vardır:

    a. [Görsel tasarımcıyı kullanın](/azure/devops/pipelines/get-started-designer). Bunu yapmak için **Yeni işlem hattı** sayfasında "görsel tasarımcıyı kullan" ı seçin.

    b. [YAML derlemelerini kullanın](/azure/devops/pipelines/get-started-yaml). Yeni bir YAML işlem hattı oluşturarak yeni bir işlem **hattı** sayfasında Azure Repos veya GitHub seçeneğine tıklayabilirsiniz. Alternatif olarak, aşağıdaki örneği kaynak denetilinizden saklayabilir ve Visual Designer ' ı seçip YAML şablonunu kullanarak var olan bir YAML dosyasına başvurabilirsiniz.

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

1. Yapı gerektiğinde yapılandırıldıktan sonra **& kuyruğu kaydet**' i seçin. Sürekli tümleştirme etkinse ( **Tetikleyiciler** bölümünde), depoya yeni bir kayıt yapıldığında derleme sırasında ayarlanan koşullara uyan yapı otomatik olarak tetiklenir.

    ![Mevcut derleme işlem hattının ekran görüntüsü.](media/batch-ci-cd/existing-build-pipeline.jpg)

1. Azure Pipelines **Build** bölümüne giderek, Azure DevOps 'daki yapınızı sürmekte olan canlı güncelleştirmeleri görüntüleyin. Derleme tanımınızdan uygun derlemeyi seçin.

    ![Azure DevOps 'daki derlemeden canlı çıktıların ekran görüntüsü.](media/batch-ci-cd/Build-1.jpg)

> [!NOTE]
> HPC çözümünüzü yürütmek için bir istemci uygulaması kullanıyorsanız, bu uygulama için ayrı bir derleme tanımı oluşturmanız gerekir. [Azure Pipelines](/azure/devops/pipelines/get-started/index) belgelerinde çeşitli nasıl yapılır kılavuzlarından ulaşabilirsiniz.

## <a name="continuous-deployment"></a>Sürekli dağıtım

Azure Pipelines, uygulamanızı ve temel altyapıyı dağıtmak için de kullanılır. [Yayın işlem hatları](/azure/devops/pipelines/release) sürekli dağıtımı etkinleştirir ve yayın işleminizi otomatik hale getirir.

### <a name="deploy-your-application-and-underlying-infrastructure"></a>Uygulamanızı ve temel altyapıyı dağıtın

Altyapıyı dağıtmaya yönelik birkaç adım vardır. Bu çözüm [bağlantılı şablonlar](../azure-resource-manager/templates/linked-templates.md)kullandığından, Bu şablonların ortak bir uç noktadan (http veya https) erişilebilir olması gerekir. Bu bir GitHub veya bir Azure Blob depolama hesabı ya da başka bir depolama konumunda bir depo olabilir. Karşıya yüklenen şablon yapıtları, özel bir modda tutulacağından ve paylaşılan erişim imzası (SAS) belirteci kullanılarak erişilen için güvenli durumda kalabilir.

Aşağıdaki örnek, bir Azure Storage blobundan şablonlar içeren bir altyapının nasıl dağıtılacağını göstermektedir.

1. Yeni bir **yayın tanımı** oluşturun ve boş bir tanım seçin. Yeni oluşturulan ortamı, işlem hattınızla ilgili bir şekilde yeniden adlandırın.

    ![İlk yayın işlem hattının ekran görüntüsü.](media/batch-ci-cd/Release-0.jpg)

1. HPC uygulamasının çıkışını almak için derleme ardışık düzeninde bir bağımlılık oluşturun.

    > [!NOTE]
    > **Kaynak diğer** adını, bu, sürüm tanımının içinde görevler oluşturulduğunda gerekli olacağı için bir yere göz atın.

    ![Uygun derleme ardışık düzeninde HPCApplicationPackage yapıt bağlantısını gösteren ekran görüntüsü.](media/batch-ci-cd/Release-1.jpg)

1. Başka bir yapıtın, bu kez bir Azure deposunun bağlantısını oluşturun. Bu, deponuzda depolanan Kaynak Yöneticisi şablonlarına erişmek için gereklidir. Kaynak Yöneticisi şablonlar derleme gerektirirken, bunları bir derleme işlem hattı aracılığıyla göndermeniz gerekmez.

    > [!NOTE]
    > Daha sonra gerekli olacağı için, bir kez daha sonra **kaynak diğer adına** göz önünde bulunmanız gerekir.

    ![Azure Repos yapıt bağlantısını gösteren ekran görüntüsü.](media/batch-ci-cd/Release-2.jpg)

1. **Değişkenler** bölümüne gidin. Aynı bilgileri birden çok göreve yeniden girmeniz gerekmiyorsa, işlem hattınızda birkaç değişken oluşturmak isteyeceksiniz. Bu örnek aşağıdaki değişkenleri kullanır:

   - **Applicationstorageaccountname**: HPC uygulama ikililerini tutan depolama hesabının adı
   - **Batchaccountapplicationname**: toplu iş hesabındaki uygulamanın adı
   - **Batchaccountname**: Batch hesabının adı
   - **Batchaccountpoolname**: Işlemeyi yapan VM havuzunun adı
   - **Batchapplicationıd**: Batch uygulaması IÇIN benzersiz kimlik
   - **Batchapplicationversion**: Batch uygulamanızın anlamsal sürümü (yani, FFmpeg ikilileri)
   - **konum**: dağıtılacak Azure kaynakları için konum
   - **Resourcegroupname**: oluşturulacak kaynak grubunun adı ve kaynaklarınızın dağıtılacağı konum
   - **storageAccountName**: bağlı kaynak yöneticisi şablonlarını tutan depolama hesabının adı

   ![Azure Pipelines sürümü için ayarlanan değişkenleri gösteren ekran görüntüsü.](media/batch-ci-cd/Release-4.jpg)

1. Geliştirme ortamı görevlerine gidin. Aşağıdaki anlık görüntüde altı görevi görebilirsiniz. Bu görevler: iç içe geçmiş Kaynak Yöneticisi şablonlarını barındırmak için bir depolama hesabı dağıtma, bu Kaynak Yöneticisi şablonlarını depolama hesabına kopyalama, Batch hesabını ve gerekli bağımlılıkları dağıtma, Azure Batch hesabında bir uygulama oluşturma ve uygulama paketini Azure Batch hesabına yükleme.

    ![Azure Batch HPC uygulamasını serbest bırakmak için kullanılan görevleri gösteren ekran görüntüsü.](media/batch-ci-cd/Release-3.jpg)

1. Indirme işlem **hattı yapıtı (Önizleme)** görevini ekleyin ve aşağıdaki özellikleri ayarlayın:
    - **Görünen ad:** ApplicationPackage 'i aracıya indir
    - **İndirilecek yapıt adı:** HPC-Application
    - **Indirilecek yol**: $ (System. DefaultWorkingDirectory)

1. Azure Resource Manager şablonlarınızı depolamak için bir depolama hesabı oluşturun. Çözümdeki mevcut bir depolama hesabı kullanılabilir, ancak bu otomatik olarak içerilen Bu örnek ve içerik yalıtımının desteklenmesi için, ayrılmış bir depolama hesabı oluşturacaksınız.

    **Azure Kaynak grubu dağıtım** görevini ekleyin ve aşağıdaki özellikleri ayarlayın:
    - **Görünen ad:** Kaynak Yöneticisi şablonları için depolama hesabı dağıtma
    - **Azure aboneliği:** Uygun Azure aboneliğini seçin
    - **Eylem**: kaynak grubu oluştur veya güncelleştir
    - **Kaynak grubu**: $ (resourcegroupname)
    - **Konum**: $ (konum)
    - **Şablon**: $ (System. ArtifactsDirectory)/**{Yourazurerepoartifactsourcealias}**/ARM-Templates/storageAccount.json
    - **Geçersiz kılma şablonu parametreleri**:-AccountName $ (storageAccountName)

1. Azure Pipelines kullanarak yapıtları kaynak denetiminden depolama hesabına yükleyin. Bu Azure Pipelines görevinin bir parçası olarak, depolama hesabı kapsayıcı URI 'SI ve SAS belirteci, Azure Pipelines bir değişkene alınabilir ve bu da bu aracı aşamasında yeniden kullanılabilmelerini sağlar.

    **Azure dosya kopyalama** görevini ekleyin ve aşağıdaki özellikleri ayarlayın:
    - **Kaynak:** $ (System. ArtifactsDirectory)/**{Yourazurerepoartifactsourcealias}**/ARM-Templates/
    - **Azure bağlantı türü**: Azure Resource Manager
    - **Azure aboneliği:** Uygun Azure aboneliğini seçin
    - **Hedef türü**: Azure blobu
    - **RM depolama hesabı**: $ (storageAccountName)
    - **Kapsayıcı adı**: Şablonlar
    - **Depolama kapsayıcısı URI 'si**: templatecontaineruri
    - **Depolama KAPSAYıCıSı SAS belirteci**: templatecontainersastoken

1. Orchestrator şablonunu dağıtın. Bu şablon, depolama hesabı kapsayıcı URI 'SI ve SAS belirteci için parametreler içerir. Kaynak Yöneticisi şablonunda gereken değişkenler, yayın tanımının değişkenler bölümünde tutulur veya başka bir Azure Pipelines görevinden (örneğin, Azure Blob kopyalama görevinin bir parçası) ayarlanmış olabilir.

    **Azure Kaynak grubu dağıtım** görevini ekleyin ve aşağıdaki özellikleri ayarlayın:
    - **Görünen ad:** Azure Batch dağıt
    - **Azure aboneliği:** Uygun Azure aboneliğini seçin
    - **Eylem**: kaynak grubu oluştur veya güncelleştir
    - **Kaynak grubu**: $ (resourcegroupname)
    - **Konum**: $ (konum)
    - **Şablon**: $ (System. ArtifactsDirectory)/**{Yourazurerepoartifactsourcealias}**/ARM-Templates/deployment.json
    - **Şablon parametrelerini geçersiz kıl**: `-templateContainerUri $(templateContainerUri) -templateContainerSasToken $(templateContainerSasToken) -batchAccountName $(batchAccountName) -batchAccountPoolName $(batchAccountPoolName) -applicationStorageAccountName $(applicationStorageAccountName)`

   Azure Key Vault görevleri kullanmak yaygın bir uygulamadır. Azure aboneliğinize bağlı hizmet sorumlusu uygun bir erişim ilkeleri ayarlandıysa, bir Azure Key Vault parolaları indirebilir ve işlem hattınızda değişken olarak kullanılabilir. Gizli anahtar adı, ilişkili değerle ayarlanır. Örneğin, sürüm tanımında sshPassword 'ın gizli anahtarı $ (sshPassword) ile birlikte başvurulmalıdır.

1. Sonraki adımlar Azure CLı 'yı çağırır. Birincisi, Azure Batch bir uygulama oluşturmak ve ilişkili paketleri karşıya yüklemek için kullanılır.

    **Azure CLI** görevini ekleyin ve aşağıdaki özellikleri ayarlayın:
    - **Görünen ad:** Azure Batch hesapta uygulama oluştur
    - **Azure aboneliği:** Uygun Azure aboneliğini seçin
    - **Betik konumu**: satır içi betik
    - **Satır Içi betik**: `az batch application create --application-id $(batchApplicationId) --name $(batchAccountName) --resource-group $(resourceGroupName)`

1. İkinci adım, ilişkili paketleri uygulamaya yüklemek için kullanılır (Bu durumda, FFmpeg dosyaları).

    **Azure CLI** görevini ekleyin ve aşağıdaki özellikleri ayarlayın:
    - **Görünen ad:** Paketi Azure Batch hesaba yükle
    - **Azure aboneliği:** Uygun Azure aboneliğini seçin
    - **Betik konumu**: satır içi betik
    - **Satır Içi betik**: `az batch application package create --application-id $(batchApplicationId)  --name $(batchAccountName)  --resource-group $(resourceGroupName) --version $(batchApplicationVersion) --package-file=$(System.DefaultWorkingDirectory)/$(Release.Artifacts.{YourBuildArtifactSourceAlias}.BuildId).zip`

    > [!NOTE]
    > Uygulama paketinin sürüm numarası bir değişkene ayarlı. Bu, paketin önceki sürümlerinin üzerine yazılmasına izin verir ve Azure Batch gönderilen paketin sürüm numarasını el ile denetlemenizi sağlar.

1. **Yeni bir yayın oluşturmak > yayın**' i seçerek yeni bir yayın oluşturun. Tetiklendikten sonra, durumu görüntülemek için yeni sürüme yönelik bağlantıyı seçin.

1. Ortamınızın altındaki **Günlükler** düğmesini seçerek aracıdan canlı çıktıyı görüntüleyin.

    ![Yayın durumunu gösteren ekran görüntüsü.](media/batch-ci-cd/Release-5.jpg)

## <a name="test-the-environment"></a>Ortamı test etme

Ortam kurulduktan sonra, aşağıdaki testlerin başarıyla tamamlandıklarını onaylayın.

PowerShell komut isteminden Azure CLı kullanarak yeni Azure Batch hesabına bağlanın.

- Azure hesabınızda ile oturum açın `az login` ve kimlik doğrulaması için yönergeleri izleyin.
- Şu anda Batch hesabının kimliğini doğrulayın: `az batch account login -g <resourceGroup> -n <batchAccount>`

#### <a name="list-the-available-applications"></a>Kullanılabilir uygulamaları listeleyin

```azurecli
az batch application list -g <resourcegroup> -n <batchaccountname>
```

#### <a name="check-the-pool-is-valid"></a>Havuzun geçerli olduğunu denetleme

```azurecli
az batch pool list
```

`currentDedicatedNodes`Bu komutun çıktısından değerini aklınızda edin. Bu değer bir sonraki testte ayarlanır.

#### <a name="resize-the-pool"></a>Havuzu yeniden boyutlandır

Havuzu yeniden boyutlandır iş ve görev testi için kullanılabilir işlem düğümleri olacak şekilde, yeniden boyutlandırma tamamlanana ve kullanılabilir düğümler bulunduğundan geçerli durumu görmek için havuz listesi komutuyla denetleyin

```azurecli
az batch pool resize --pool-id <poolname> --target-dedicated-nodes 4
```

## <a name="next-steps"></a>Sonraki adımlar

Basit bir uygulama aracılığıyla Batch hesabıyla nasıl etkileşim kuracağınızı öğrenmek için bu öğreticilere bakın.

- [Python API 'sini kullanarak Azure Batch ile paralel iş yükü çalıştırma](tutorial-parallel-python.md)
- [.NET API’si kullanarak Azure Batch ile paralel iş yükü çalıştırma](tutorial-parallel-dotnet.md)
