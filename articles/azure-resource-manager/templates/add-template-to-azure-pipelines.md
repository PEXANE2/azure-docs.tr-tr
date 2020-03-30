---
title: Azure Ardışık Hatlar ve şablonları ile CI/CD
description: Kaynak Yöneticisi şablonlarını dağıtmak için Visual Studio'daki Azure Kaynak Grubu dağıtım projelerini kullanarak Azure Altyapı Hatları'nda sürekli tümleştirmenin nasıl ayarlanır olduğunu açıklar.
ms.topic: conceptual
ms.date: 10/17/2019
ms.openlocfilehash: 7617bf47595fce7baa533b0f7cc94a1803ddd349
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80153463"
---
# <a name="integrate-arm-templates-with-azure-pipelines"></a>ARM şablonlarını Azure Pipelines ile tümleştir

Visual Studio, Azure Kaynak Yöneticisi (ARM) şablonları oluşturmak ve bunları Azure aboneliğinize dağıtmak için Azure Kaynak Grubu projesini sağlar. Sürekli tümleştirme ve sürekli dağıtım (CI/CD) için bu projeyi Azure Pipelines ile tümleştirebilirsiniz.

Azure Ardışık Hatları ile şablondağıtmanın iki yolu vardır:

* **Azure PowerShell komut dosyası çalıştıran görev ekleyin.** Visual Studio projesinde (Deploy-AzureResourceGroup.ps1) yer alan aynı komut dosyasını kullandığınızdan, bu seçenek geliştirme yaşam döngüsü boyunca tutarlılık sağlama avantajına sahiptir. Komut dosyası, yapıtları projenizden Kaynak Yöneticisi'nin erişebileceği bir depolama hesabına aşamalar. Yapı, bağlı şablonlar, komut dosyaları ve uygulama ikilileri gibi projenizdeki öğelerdir. Ardından, komut dosyası şablonu dağır.

* **Görevleri kopyalamak ve dağıtmak için görevler ekleyin.** Bu seçenek, proje komut dosyasına uygun bir alternatif sunar. Ardışık alanda iki görevi yapılandırırsınız. Bir görev yapıları aşamalı olarak, diğer görev şablonu dağıtır.

Bu makalede, her iki yaklaşım gösterir.

## <a name="prepare-your-project"></a>Projenizi hazırlayın

Bu makalede, Visual Studio projeniz ve Azure DevOps kuruluşunuzun ardışık yapı oluşturmaya hazır olduğu varsayar. Aşağıdaki adımlar, nasıl hazır olduğunuzdan emin olun:

* Bir Azure DevOps kuruluşunuz var. Eğer yoksa, [ücretsiz bir oluşturun.](/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops) Ekibinizin zaten bir Azure DevOps kuruluşu varsa, kullanmak istediğiniz Azure DevOps projesinin yöneticisi olduğunuzdan emin olun.

* Azure aboneliğinize bir [hizmet bağlantısı](/azure/devops/pipelines/library/connect-to-azure?view=azure-devops) yapılandırıldınız. Ardışık işteki görevler, hizmet sorumlusunun kimliği altında yürütülür. Bağlantıyı oluşturmak için adımlar için [bkz.](template-tutorial-use-azure-pipelines.md#create-a-devops-project)

* **Azure Kaynak Grubu** başlangıç şablonundan oluşturulmuş bir Visual Studio projeniz var. Bu tür bir proje oluşturma hakkında bilgi için [Visual Studio aracılığıyla Azure kaynak grupları oluşturma ve dağıtma](create-visual-studio-deployment-project.md)bölümüne bakın.

* Visual Studio projeniz [bir Azure DevOps projesine bağlıdır.](/azure/devops/repos/git/share-your-code-in-git-vs-2017?view=azure-devops)

## <a name="create-pipeline"></a>İşlem hattı oluşturma

1. Daha önce bir ardışık hatlar eklemediyseniz, yeni bir ardışık yol hattı oluşturmanız gerekir. Azure DevOps kuruluşunuzdan, **Boru Hatları** ve **Yeni ardışık hatlar'ı**seçin.

   ![Yeni ardışık hat lar ekleme](./media/add-template-to-azure-pipelines/new-pipeline.png)

1. Kodunuzun nerede depolandığınızı belirtin. Aşağıdaki resimde Azure **Repos Git'i**seçili görüntüleilmektedir.

   ![Kod kaynağını seçin](./media/add-template-to-azure-pipelines/select-source.png)

1. Bu kaynaktan, projenizin koduna sahip depoyu seçin.

   ![Depo seçin](./media/add-template-to-azure-pipelines/select-repo.png)

1. Oluşturmak için boru hattı türünü seçin. **Başlangıç ardışık hattını**seçebilirsiniz.

   ![Boru hattını seçin](./media/add-template-to-azure-pipelines/select-pipeline.png)

Bir Azure PowerShell görevi veya kopyalama dosyası ekleyip görevleri dağıtmaya hazırsınız.

## <a name="azure-powershell-task"></a>Azure PowerShell görevi

Bu bölümde, projenizdeki PowerShell komut dosyasını çalıştıran tek bir görev kullanarak sürekli dağıtımı nasıl yapılandırıştırılatırınız gösterilmektedir. Aşağıdaki YAML dosyası bir [Azure PowerShell görevi](/azure/devops/pipelines/tasks/deploy/azure-powershell?view=azure-devops)oluşturur:

```yaml
pool:
  name: Hosted Windows 2019 with VS2019
  demands: azureps

steps:
- task: AzurePowerShell@3
  inputs:
    azureSubscription: 'demo-deploy-sp'
    ScriptPath: 'AzureResourceGroupDemo/Deploy-AzureResourceGroup.ps1'
    ScriptArguments: -ResourceGroupName 'demogroup' -ResourceGroupLocation 'centralus'
    azurePowerShellVersion: LatestVersion
```

Görevi `AzurePowerShell@3`ayarladığınızda, ardışık iş, bağlantının kimliğini doğrulamak için AzureRM modülünden komutlar kullanır. Varsayılan olarak, Visual Studio projesindeki PowerShell komut dosyası AzureRM modüllerini kullanır. Az [modüllerini](/powershell/azure/new-azureps-module-az)kullanmak için komut dosyanızı güncellediyseniz, görevi ' ye `AzurePowerShell@4`ayarlayın.

```yaml
steps:
- task: AzurePowerShell@4
```

Bunun `azureSubscription`için oluşturduğunuz hizmet bağlantısının adını sağlayın.

```yaml
inputs:
    azureSubscription: '<your-connection-name>'
```

Bunun `scriptPath`için, komut dosyasına boru hattı dosyasından göreli yolu sağlayın. Yolu görmek için deponuza bakabilirsiniz.

```yaml
ScriptPath: '<your-relative-path>/<script-file-name>.ps1'
```

Yapıtları sahnelemeniz gerekmiyorsa, dağıtım için kullanmak üzere bir kaynak grubunun adını ve konumunu geçirmeniz gerekir. Visual Studio komut dosyası, kaynak grubunu zaten yoksa oluşturur.

```yaml
ScriptArguments: -ResourceGroupName '<resource-group-name>' -ResourceGroupLocation '<location>'
```

Yapıları varolan bir depolama hesabına aşamalı olarak düzenlemeniz gerekiyorsa, şunları kullanın:

```yaml
ScriptArguments: -ResourceGroupName '<resource-group-name>' -ResourceGroupLocation '<location>' -UploadArtifacts -ArtifactStagingDirectory '$(Build.StagingDirectory)' -StorageAccountName '<your-storage-account>'
```

Şimdi, görevin nasıl oluşturulabileceğinizi anladığınıza göre, boru hattını oluşturmak için gereken adımları atalım.

1. Ardışık hattınızı açın ve içeriği YAML'niz ile değiştirin:

   ```yaml
   pool:
     name: Hosted Windows 2019 with VS2019
     demands: azureps

   steps:
   - task: AzurePowerShell@3
     inputs:
       azureSubscription: 'demo-deploy-sp'
       ScriptPath: 'AzureResourceGroupDemo/Deploy-AzureResourceGroup.ps1'
       ScriptArguments: -ResourceGroupName 'demogroup' -ResourceGroupLocation 'centralus' -UploadArtifacts -ArtifactStagingDirectory '$(Build.StagingDirectory)' -StorageAccountName 'stage3a4176e058d34bb88cc'
       azurePowerShellVersion: LatestVersion
   ```

1. **Kaydet'i**seçin.

   ![İşlem hattını kaydetme](./media/add-template-to-azure-pipelines/save-pipeline.png)

1. Commit için bir ileti sağlayın ve doğrudan **ana**söze bağla.

1. **Kaydet'i**seçtiğinizde, yapı ardışık hattı otomatik olarak çalıştırılır. Yapı ardınız için özete geri dön ve durumu izleyin.

   ![Sonuçları görüntüleme](./media/add-template-to-azure-pipelines/view-results.png)

Görevlerle ilgili ayrıntıları görmek için şu anda çalışan ardışık hattı seçebilirsiniz. Bittiğinde, her adımIçin sonuçları görürsünüz.

## <a name="copy-and-deploy-tasks"></a>Görevleri kopyalama ve dağıtma

Bu bölümde, yapıları sahnelemek ve şablonu dağıtmak için iki görev kullanarak sürekli dağıtımı nasıl yapılandırıştırılatırın.

Aşağıdaki YAML [Azure dosya kopyalama görevini](/azure/devops/pipelines/tasks/deploy/azure-file-copy?view=azure-devops)gösterir:

```yaml
- task: AzureFileCopy@3
  displayName: 'Stage files'
  inputs:
    SourcePath: 'AzureResourceGroup1'
    azureSubscription: 'demo-deploy-sp'
    Destination: 'AzureBlob'
    storage: 'stage3a4176e058d34bb88cc'
    ContainerName: 'democontainer'
    outputStorageUri: 'artifactsLocation'
    outputStorageContainerSasToken: 'artifactsLocationSasToken'
    sasTokenTimeOutInMinutes: '240'
```

Bu görevin ortamınız için gözden geçirilmesi gereken birkaç bölümü vardır. Ardışık `SourcePath` yapı dosyasına göre yapıtların konumunu gösterir. Bu örnekte, dosyalar projenin adı `AzureResourceGroup1` olan bir klasörde bulunur.

```yaml
SourcePath: '<path-to-artifacts>'
```

Bunun `azureSubscription`için oluşturduğunuz hizmet bağlantısının adını sağlayın.

```yaml
azureSubscription: '<your-connection-name>'
```

Depolama ve kapsayıcı adı için, yapıtları depolamak için kullanmak istediğiniz depolama hesabının ve kapsayıcının adlarını sağlayın. Depolama hesabı nın bulunması gerekir.

```yaml
storage: '<your-storage-account-name>'
ContainerName: '<container-name>'
```

Aşağıdaki YAML, [Azure Kaynak Yöneticisi şablon dağıtım görevini](https://github.com/microsoft/azure-pipelines-tasks/blob/master/Tasks/AzureResourceManagerTemplateDeploymentV3/README.md)gösterir:

```yaml
- task: AzureResourceGroupDeployment@2
  displayName: 'Deploy template'
  inputs:
    deploymentScope: 'Resource Group'
    ConnectedServiceName: 'demo-deploy-sp'
    subscriptionName: '01234567-89AB-CDEF-0123-4567890ABCDEF'
    action: 'Create Or Update Resource Group'
    resourceGroupName: 'demogroup'
    location: 'Central US'
    templateLocation: 'URL of the file'
    csmFileLink: '$(artifactsLocation)WebSite.json$(artifactsLocationSasToken)'
    csmParametersFileLink: '$(artifactsLocation)WebSite.parameters.json$(artifactsLocationSasToken)'
    overrideParameters: '-_artifactsLocation $(artifactsLocation) -_artifactsLocationSasToken "$(artifactsLocationSasToken)"'
    deploymentMode: 'Incremental'
```

Bu görevin ortamınız için gözden geçirilmesi gereken birkaç bölümü vardır.

- `deploymentScope`: Dağıtım kapsamını seçeneklerden `Management Group`seçin: `Subscription` `Resource Group`, ve . Bu yürüyüşte **Kaynak Grubu'ni** kullanın. Kapsamlar hakkında daha fazla bilgi edinmek için [Dağıtım kapsamlarına](deploy-rest.md#deployment-scope)bakın.

- `ConnectedServiceName`: Oluşturduğunuz servis bağlantısının adını sağlayın.

    ```yaml
    ConnectedServiceName: '<your-connection-name>'
    ```

- `subscriptionName`: Hedef abonelik kimliğini sağlayın. Bu özellik yalnızca Kaynak Grubu dağıtım kapsamı ve abonelik dağıtım kapsamı için geçerlidir.

- `resourceGroupName`ve `location`: dağıtmak istediğiniz kaynak grubunun adını ve konumunu sağlayın. Görev, yoksa kaynak grubunu oluşturur.

    ```yaml
    resourceGroupName: '<resource-group-name>'
    location: '<location>'
    ```

Dağıtım görevi, adlı `WebSite.json` şablona ve WebSite.parameters.json adlı bir parametre dosyasına bağlanır. Şablonve parametre dosyalarınızın adlarını kullanın.

Şimdi, görevleri nasıl oluşturabileceğinizi anladığınıza göre, boru hattını oluşturmak için gereken adımları atalım.

1. Ardışık hattınızı açın ve içeriği YAML'niz ile değiştirin:

   ```yaml
   pool:
     name: Hosted Windows 2019 with VS2019

   steps:
   - task: AzureFileCopy@3
     displayName: 'Stage files'
     inputs:
       SourcePath: 'AzureResourceGroup1'
       azureSubscription: 'demo-deploy-sp'
       Destination: 'AzureBlob'
       storage: 'stage3a4176e058d34bb88cc'
       ContainerName: 'democontainer'
       outputStorageUri: 'artifactsLocation'
       outputStorageContainerSasToken: 'artifactsLocationSasToken'
       sasTokenTimeOutInMinutes: '240'
    - task: AzureResourceGroupDeployment@2
      displayName: 'Deploy template'
      inputs:
        deploymentScope: 'Resource Group'
        ConnectedServiceName: 'demo-deploy-sp'
        subscriptionName: '01234567-89AB-CDEF-0123-4567890ABCDEF'
        action: 'Create Or Update Resource Group'
        resourceGroupName: 'demogroup'
        location: 'Central US'
        templateLocation: 'URL of the file'
        csmFileLink: '$(artifactsLocation)WebSite.json$(artifactsLocationSasToken)'
        csmParametersFileLink: '$(artifactsLocation)WebSite.parameters.json$(artifactsLocationSasToken)'
        overrideParameters: '-_artifactsLocation $(artifactsLocation) -_artifactsLocationSasToken "$(artifactsLocationSasToken)"'
        deploymentMode: 'Incremental'
   ```

1. **Kaydet'i**seçin.

1. Commit için bir ileti sağlayın ve doğrudan **ana**söze bağla.

1. **Kaydet'i**seçtiğinizde, yapı ardışık hattı otomatik olarak çalıştırılır. Yapı ardınız için özete geri dön ve durumu izleyin.

   ![Sonuçları görüntüleme](./media/add-template-to-azure-pipelines/view-results.png)

Görevlerle ilgili ayrıntıları görmek için şu anda çalışan ardışık hattı seçebilirsiniz. Bittiğinde, her adımIçin sonuçları görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

ARM şablonları ile Azure Ardışık Hatlar'ı kullanma yla ilgili adım adım işlem için [Bkz.](template-tutorial-use-azure-pipelines.md)
