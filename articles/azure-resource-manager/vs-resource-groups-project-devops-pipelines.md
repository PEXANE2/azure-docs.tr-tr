---
title: Azure Pipelines ve Kaynak Yöneticisi şablonlarıyla CI/CD
description: Kaynak Yöneticisi şablonlarını dağıtmak üzere Visual Studio 'da Azure Kaynak grubu dağıtım projelerini kullanarak Azure Pipelines sürekli tümleştirmenin nasıl ayarlanacağını açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: tomfitz
ms.openlocfilehash: ae896fa0820fbd25ed3f2d29c89fbcd56e7fd6f5
ms.sourcegitcommit: 6d2a147a7e729f05d65ea4735b880c005f62530f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69982453"
---
# <a name="integrate-resource-manager-templates-with-azure-pipelines"></a>Kaynak Yöneticisi şablonlarını Azure Pipelines ile tümleştirme

Visual Studio, şablon oluşturmak ve bunları Azure aboneliğinize dağıtmak için Azure Kaynak grubu projesi sağlar. Bu projeyi sürekli tümleştirme ve sürekli dağıtım (CI/CD) için Azure Pipelines ile tümleştirebilirsiniz.

Azure Pipelines şablonları dağıtmanın iki yolu vardır:

* **Azure PowerShell betiği çalıştıran görev ekleyin**. Visual Studio projesinde (Deploy-AzureResourceGroup. ps1) bulunan aynı betiği kullandığınız için, bu seçeneğin geliştirme yaşam döngüsü genelinde tutarlılık sağlama avantajı vardır. Betik aşamaları, projenizdeki Kaynak Yöneticisi erişebileceği bir depolama hesabına ait yapıtlardan oluşur. Yapıtlar, projenizdeki bağlantılı şablonlar, betikler ve uygulama ikilileri gibi öğelerdir. Ardından, komut dosyası şablonu dağıtır.

* **Görevleri kopyalamak ve dağıtmak için görevler ekleyin**. Bu seçenek, proje betiğine uygun bir alternatif sağlar. Ardışık düzende iki görev yapılandırırsınız. Bir görev, yapıtları ve diğer görevi şablonu dağıtır.

Bu makalede her iki yaklaşım da gösterilmektedir.

## <a name="prepare-your-project"></a>Projenizi hazırlama

Bu makalede, Visual Studio projenizin ve Azure DevOps kuruluşunuzun işlem hattını oluşturmaya hazırlanma varsayılmaktadır. Aşağıdaki adımlarda, nasıl hazırlandığınızı nasıl yapacağınız nasıl emin olduğunuz gösterilmektedir:

* Bir Azure DevOps kuruluşunuza sahipsiniz. Yoksa, [ücretsiz olarak bir tane oluşturun](/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops). Takımınızın zaten bir Azure DevOps kuruluşu varsa, kullanmak istediğiniz Azure DevOps projesinin yöneticisi olduğunuzdan emin olun.

* Azure aboneliğiniz için bir [hizmet bağlantısı](/azure/devops/pipelines/library/connect-to-azure?view=azure-devops) yapılandırdınız. İşlem hattındaki görevler hizmet sorumlusu kimliği altında yürütülür. Bağlantıyı oluşturma adımları için bkz. [DevOps projesi oluşturma](resource-manager-tutorial-use-azure-pipelines.md#create-a-devops-project).

* **Azure Kaynak grubu** Starter şablonundan oluşturulmuş bir Visual Studio projenize sahipsiniz. Bu tür bir projeyi oluşturma hakkında daha fazla bilgi için bkz. [Azure kaynak gruplarını Visual Studio aracılığıyla oluşturma ve dağıtma](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

* Visual Studio projeniz [bir Azure DevOps projesine bağlandı](/azure/devops/repos/git/share-your-code-in-git-vs-2017?view=azure-devops).

## <a name="create-pipeline"></a>İşlem hattı oluşturma

1. Daha önce bir işlem hattı eklemediyseniz yeni bir işlem hattı oluşturmanız gerekir. Azure DevOps kuruluşunuzdan işlem **hatları** ve **Yeni işlem hattı**' nı seçin.

   ![Yeni işlem hattı Ekle](./media/vs-resource-groups-project-devops-pipelines/new-pipeline.png)

1. Kodunuzun depolanacağı yeri belirtin. Aşağıdaki görüntüde **Git Azure Repos**seçme gösterilmektedir.

   ![Kod kaynağı seç](./media/vs-resource-groups-project-devops-pipelines/select-source.png)

1. Bu kaynaktan, projenizin koduna sahip depoyu seçin.

   ![Depo seçin](./media/vs-resource-groups-project-devops-pipelines/select-repo.png)

1. Oluşturulacak işlem hattının türünü seçin. **Başlatıcı işlem hattı**' nı seçebilirsiniz.

   ![İşlem hattı seçin](./media/vs-resource-groups-project-devops-pipelines/select-pipeline.png)

Azure PowerShell bir görev ya da Dosya Kopyala ve görevleri dağıt 'a hazırsınız.

## <a name="azure-powershell-task"></a>Azure PowerShell görev

Bu bölümde, projenizde PowerShell betiğini çalıştıran tek bir görev kullanılarak sürekli dağıtımın nasıl yapılandırılacağı gösterilmektedir. Aşağıdaki YAML dosyası bir [Azure PowerShell görevi](/azure/devops/pipelines/tasks/deploy/azure-powershell?view=azure-devops)oluşturur:

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

Görevi olarak `AzurePowerShell@3`ayarladığınızda, işlem hattı, bağlantının kimliğini doğrulamak için azurerd modülünden komutlar kullanır. Varsayılan olarak, Visual Studio projesindeki PowerShell betiği Azurerd modülünü kullanır. Betiğinizi [az Module](/powershell/azure/new-azureps-module-az)kullanacak şekilde güncelleştirdiyseniz, görevi olarak `AzurePowerShell@4`ayarlayın.

```yaml
steps:
- task: AzurePowerShell@4
```

İçin `azureSubscription`, oluşturduğunuz hizmet bağlantısının adını belirtin.

```yaml
inputs:
    azureSubscription: '<your-connection-name>'
```

İçin `scriptPath`, işlem hattı dosyasından betiğe göreli yol belirtin. Yolu görmek için deponuza bakabilirsiniz.

```yaml
ScriptPath: '<your-relative-path>/<script-file-name>.ps1'
```

Yapıtlar için gerekli değilse, dağıtım için kullanmak üzere bir kaynak grubunun adını ve konumunu geçirmeniz yeterlidir. Visual Studio betiği, zaten mevcut değilse kaynak grubunu oluşturur.

```yaml
ScriptArguments: -ResourceGroupName '<resource-group-name>' -ResourceGroupLocation '<location>'
```

Yapıtları var olan bir depolama hesabına hazırlama gereksinimi varsa şunu kullanın:

```yaml
ScriptArguments: -ResourceGroupName '<resource-group-name>' -ResourceGroupLocation '<location>' -UploadArtifacts -ArtifactStagingDirectory '$(Build.StagingDirectory)' -StorageAccountName '<your-storage-account>'
```

Şimdi, görevi nasıl oluşturacağınız hakkında anladığınıza göre, işlem hattını düzenleme adımlarında ilerlim.

1. İşlem hattınızı açın ve içeriği YAML 'ınızla değiştirin:

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

1. **Kaydet**’i seçin.

   ![İşlem hattını kaydetme](./media/vs-resource-groups-project-devops-pipelines/save-pipeline.png)

1. Kayıt için bir ileti sağlayın ve doğrudan **ana öğe**için yürütün.

1. **Kaydet**' i seçtiğinizde, derleme işlem hattı otomatik olarak çalıştırılır. Derleme işlem hattınızla ilgili özete dönün ve durumu izleyin.

   ![Sonuçları görüntüleme](./media/vs-resource-groups-project-devops-pipelines/view-results.png)

Görevlerle ilgili ayrıntıları görmek için şu anda çalışan ardışık düzeni seçebilirsiniz. Tamamlandığında, her adımın sonuçlarını görürsünüz.

## <a name="copy-and-deploy-tasks"></a>Görevleri Kopyala ve dağıt

Bu bölümde, yapıtları hazırlamak ve şablonu dağıtmak için iki görevi kullanarak sürekli dağıtımın nasıl yapılandırılacağı gösterilmektedir. 

Aşağıdaki YAML, [Azure dosya kopyalama görevini](/azure/devops/pipelines/tasks/deploy/azure-file-copy?view=azure-devops)göstermektedir:

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

Bu görevin, ortamınız için gözden geçirmek için birkaç bölümü vardır. , `SourcePath` İşlem hattı dosyasına göre yapıtların konumunu gösterir. Bu örnekte, dosyalar, projenin adı olan adlı `AzureResourceGroup1` bir klasörde bulunur.

```yaml
SourcePath: '<path-to-artifacts>'
```

İçin `azureSubscription`, oluşturduğunuz hizmet bağlantısının adını belirtin.

```yaml
azureSubscription: '<your-connection-name>'
```

Depolama ve kapsayıcı adı için, yapıtları depolamak üzere kullanmak istediğiniz depolama hesabının ve kapsayıcının adlarını belirtin. Depolama hesabının mevcut olması gerekir.

```yaml
storage: '<your-storage-account-name>'
ContainerName: '<container-name>'
```

Aşağıdaki YAML, [Azure Kaynak grubu dağıtım görevini](/azure/devops/pipelines/tasks/deploy/azure-resource-group-deployment?view=azure-devops)göstermektedir:

```yaml
- task: AzureResourceGroupDeployment@2
  displayName: 'Deploy template'
  inputs:
    azureSubscription: 'demo-deploy-sp'
    resourceGroupName: 'demogroup'
    location: 'centralus'
    templateLocation: 'URL of the file'
    csmFileLink: '$(artifactsLocation)WebSite.json$(artifactsLocationSasToken)'
    csmParametersFileLink: '$(artifactsLocation)WebSite.parameters.json$(artifactsLocationSasToken)'
    overrideParameters: '-_artifactsLocation $(artifactsLocation) -_artifactsLocationSasToken "$(artifactsLocationSasToken)"'
```

Bu görevin, ortamınız için gözden geçirmek için birkaç bölümü vardır. İçin `azureSubscription`, oluşturduğunuz hizmet bağlantısının adını belirtin.

```yaml
azureSubscription: '<your-connection-name>'
```

`resourceGroupName` Ve`location`için, dağıtmak istediğiniz kaynak grubunun adını ve konumunu belirtin. Görev, mevcut değilse kaynak grubunu oluşturur.

```yaml
resourceGroupName: '<resource-group-name>'
location: '<location>'
```

Dağıtım görevi, adlı `WebSite.json` bir şablona ve Web sitesi. Parameters. JSON adlı parametreler dosyasına bağlanır. Şablon ve parametre dosyalarınızın adlarını kullanın.

Artık görevlerin nasıl oluşturulduğunu anladığınıza göre, işlem hattını düzenleme adımlarını inceleyelim.

1. İşlem hattınızı açın ve içeriği YAML 'ınızla değiştirin:

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
       azureSubscription: 'demo-deploy-sp'
       resourceGroupName: demogroup
       location: 'centralus'
       templateLocation: 'URL of the file'
       csmFileLink: '$(artifactsLocation)WebSite.json$(artifactsLocationSasToken)'
       csmParametersFileLink: '$(artifactsLocation)WebSite.parameters.json$(artifactsLocationSasToken)'
       overrideParameters: '-_artifactsLocation $(artifactsLocation) -_artifactsLocationSasToken "$(artifactsLocationSasToken)"'
   ```

1. **Kaydet**’i seçin.

1. Kayıt için bir ileti sağlayın ve doğrudan **ana öğe**için yürütün.

1. **Kaydet**' i seçtiğinizde, derleme işlem hattı otomatik olarak çalıştırılır. Derleme işlem hattınızla ilgili özete dönün ve durumu izleyin.

   ![Sonuçları görüntüleme](./media/vs-resource-groups-project-devops-pipelines/view-results.png)

Görevlerle ilgili ayrıntıları görmek için şu anda çalışan ardışık düzeni seçebilirsiniz. Tamamlandığında, her adımın sonuçlarını görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

Kaynak Yöneticisi şablonlarla Azure Pipelines kullanma hakkında adım adım işlemler için bkz [. Öğretici: Azure Pipelines](resource-manager-tutorial-use-azure-pipelines.md)olan Azure Resource Manager şablonlarının sürekli tümleştirilmesi.
