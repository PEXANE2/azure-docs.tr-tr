---
title: Azure Pipelines ve şablonlar ile CI/CD
description: Azure Resource Manager şablonları kullanarak Azure Pipelines sürekli tümleştirmenin nasıl yapılandırılacağını açıklar. Bir PowerShell betiğini kullanmayı veya dosyaları bir hazırlama konumuna kopyalamayı ve buradan dağıtmayı gösterir.
ms.topic: conceptual
ms.date: 10/01/2020
ms.openlocfilehash: 6784df30340e4c54b8b1d6e82b45046666824315
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91653409"
---
# <a name="integrate-arm-templates-with-azure-pipelines"></a>ARM şablonlarını Azure Pipelines ile tümleştirme

Sürekli tümleştirme ve sürekli dağıtım (CI/CD) için, Azure Resource Manager şablonlarını (ARM şablonları) Azure Pipelines tümleştirebilirsiniz. [ARM şablonlarının sürekli tümleştirmesi Azure Pipelines](deployment-tutorial-pipeline.md) , GitHub deponuzdan bir şablon dağıtmak için [ARM şablonu dağıtım görevinin](https://github.com/microsoft/azure-pipelines-tasks/blob/master/Tasks/AzureResourceManagerTemplateDeploymentV3/README.md) nasıl kullanılacağını gösterir. Bu yaklaşım, bir şablonu doğrudan bir depodan dağıtmak istediğinizde işe yarar.

Bu makalede, Azure Pipelines şablonları dağıtmanın iki yolunu öğreneceksiniz. Bu makalede nasıl yapılacağı gösterilmektedir:

* **Azure PowerShell betiği çalıştıran görev ekleyin**. Yerel testleri çalıştırırken kullandığınız betiği kullanabilmeniz için, bu seçeneğin geliştirme yaşam döngüsü genelinde tutarlılık sağlama avantajı vardır. Betiğinizin şablonu dağıtır ancak parametre olarak kullanılacak değerleri alma gibi başka işlemler de gerçekleştirebilir.

   Visual Studio, bir PowerShell betiği içeren [Azure Kaynak grubu projesi](create-visual-studio-deployment-project.md) sağlar. Betik aşamaları, projenizdeki Kaynak Yöneticisi erişebileceği bir depolama hesabına ait yapıtlardan oluşur. Yapıtlar, projenizdeki bağlantılı şablonlar, betikler ve uygulama ikilileri gibi öğelerdir. Projeden betiği kullanmaya devam etmek istiyorsanız, bu makalede gösterilen PowerShell betiği görevini kullanın.

* **Görevleri kopyalamak ve dağıtmak için görevler ekleyin**. Bu seçenek, proje betiğine uygun bir alternatif sağlar. Ardışık düzende iki görev yapılandırırsınız. Bir görev yapıtları erişilebilir bir konuma göre aşamalar. Diğer görev, şablonu o konumdan dağıtır.

## <a name="prepare-your-project"></a>Projenizi hazırlama

Bu makalede, ARM şablonunuz ve Azure DevOps kuruluşunuzun işlem hattını oluşturmaya hazırız varsayılmaktadır. Aşağıdaki adımlarda, nasıl hazırlandığınızı nasıl yapacağınız nasıl emin olduğunuz gösterilmektedir:

* Bir Azure DevOps kuruluşunuza sahipsiniz. Yoksa, [ücretsiz olarak bir tane oluşturun](/azure/devops/pipelines/get-started/pipelines-sign-up). Takımınızın zaten bir Azure DevOps kuruluşu varsa, kullanmak istediğiniz Azure DevOps projesinin yöneticisi olduğunuzdan emin olun.

* Azure aboneliğiniz için bir [hizmet bağlantısı](/azure/devops/pipelines/library/connect-to-azure) yapılandırdınız. İşlem hattındaki görevler hizmet sorumlusu kimliği altında yürütülür. Bağlantıyı oluşturma adımları için bkz. [DevOps projesi oluşturma](deployment-tutorial-pipeline.md#create-a-devops-project).

* Projeniz için altyapıyı tanımlayan bir [ARM şablonunuz](quickstart-create-templates-use-visual-studio-code.md) vardır.

## <a name="create-pipeline"></a>İşlem hattı oluşturma

1. Daha önce bir işlem hattı eklemediyseniz yeni bir işlem hattı oluşturmanız gerekir. Azure DevOps kuruluşunuzdan işlem **hatları** ve **Yeni işlem hattı**' nı seçin.

   ![Yeni işlem hattı Ekle](./media/add-template-to-azure-pipelines/new-pipeline.png)

1. Kodunuzun depolanacağı yeri belirtin. Aşağıdaki görüntüde **Git Azure Repos**seçme gösterilmektedir.

   ![Kod kaynağı seç](./media/add-template-to-azure-pipelines/select-source.png)

1. Bu kaynaktan, projenizin koduna sahip depoyu seçin.

   ![Depo seçin](./media/add-template-to-azure-pipelines/select-repo.png)

1. Oluşturulacak işlem hattının türünü seçin. **Başlatıcı işlem hattı**' nı seçebilirsiniz.

   ![İşlem hattı seçin](./media/add-template-to-azure-pipelines/select-pipeline.png)

Azure PowerShell bir görev ya da Dosya Kopyala ve görevleri dağıt 'a hazırsınız.

## <a name="azure-powershell-task"></a>Azure PowerShell görev

Bu bölümde, projenizde PowerShell betiğini çalıştıran tek bir görev kullanılarak sürekli dağıtımın nasıl yapılandırılacağı gösterilmektedir. Şablon dağıtan bir PowerShell betiğine ihtiyacınız varsa, bkz. [Deploy-AzTemplate.ps1](https://github.com/Azure/azure-quickstart-templates/blob/master/Deploy-AzTemplate.ps1) veya [Deploy-AzureResourceGroup.ps1](https://github.com/Azure/azure-quickstart-templates/blob/master/Deploy-AzureResourceGroup.ps1).

Aşağıdaki YAML dosyası bir [Azure PowerShell görevi](/azure/devops/pipelines/tasks/deploy/azure-powershell)oluşturur:

```yml
trigger:
- master

pool:
  vmImage: 'ubuntu-latest'

steps:
- task: AzurePowerShell@5
  inputs:
    azureSubscription: 'script-connection'
    ScriptType: 'FilePath'
    ScriptPath: './Deploy-Template.ps1'
    ScriptArguments: -Location 'centralus' -ResourceGroupName 'demogroup' -TemplateFile templates\mainTemplate.json
    azurePowerShellVersion: 'LatestVersion'
```

Görevi olarak ayarladığınızda `AzurePowerShell@5` , işlem hattı [az Module](/powershell/azure/new-azureps-module-az)kullanır. Betiğinizdeki Azurere modülünü kullanıyorsanız, görevi olarak ayarlayın `AzurePowerShell@3` .

```yaml
steps:
- task: AzurePowerShell@3
```

İçin `azureSubscription` , oluşturduğunuz hizmet bağlantısının adını belirtin.

```yaml
inputs:
    azureSubscription: '<your-connection-name>'
```

İçin `scriptPath` , işlem hattı dosyasından betiğe göreli yol belirtin. Yolu görmek için deponuza bakabilirsiniz.

```yaml
ScriptPath: '<your-relative-path>/<script-file-name>.ps1'
```

İçinde `ScriptArguments` , betiğinizi gereken tüm parametreleri sağlayın. Aşağıdaki örnek, bir komut dosyası için bazı parametreleri gösterir, ancak betiğinizin parametrelerini özelleştirmeniz gerekir.

```yaml
ScriptArguments: -Location 'centralus' -ResourceGroupName 'demogroup' -TemplateFile templates\mainTemplate.json
```

**Kaydet**' i seçtiğinizde, derleme işlem hattı otomatik olarak çalıştırılır. Derleme işlem hattınızla ilgili özete dönün ve durumu izleyin.

![Sonuçları görüntüleme](./media/add-template-to-azure-pipelines/view-results.png)

Görevlerle ilgili ayrıntıları görmek için şu anda çalışan ardışık düzeni seçebilirsiniz. Tamamlandığında, her adımın sonuçlarını görürsünüz.

## <a name="copy-and-deploy-tasks"></a>Görevleri Kopyala ve dağıt

Bu bölümde, iki görevi kullanarak sürekli dağıtımın nasıl yapılandırılacağı gösterilmektedir. İlk görev yapıtları bir depolama hesabına, ikinci görev ise şablonu dağıtır.

Dosyaları bir depolama hesabına kopyalamak için, hizmet bağlantısı için hizmet sorumlusuna Depolama Blobu veri katılımcısı veya Depolama Blobu veri sahibi rolü atanmalıdır. Daha fazla bilgi için bkz. [AzCopy ile çalışmaya başlama](../../storage/common/storage-use-azcopy-v10.md).

Aşağıdaki YAML, [Azure dosya kopyalama görevini](/azure/devops/pipelines/tasks/deploy/azure-file-copy)göstermektedir.

```yml
trigger:
- master

pool:
  vmImage: 'windows-latest'

steps:
- task: AzureFileCopy@4
  inputs:
    SourcePath: 'templates'
    azureSubscription: 'copy-connection'
    Destination: 'AzureBlob'
    storage: 'demostorage'
    ContainerName: 'projecttemplates'
  name: AzureFileCopy
```

Bu görevin, ortamınız için gözden geçirmek için birkaç bölümü vardır. , İşlem `SourcePath` hattı dosyasına göre yapıtların konumunu gösterir.

```yaml
SourcePath: '<path-to-artifacts>'
```

İçin `azureSubscription` , oluşturduğunuz hizmet bağlantısının adını belirtin.

```yaml
azureSubscription: '<your-connection-name>'
```

Depolama ve kapsayıcı adı için, yapıtları depolamak üzere kullanmak istediğiniz depolama hesabının ve kapsayıcının adlarını belirtin. Depolama hesabının mevcut olması gerekir.

```yaml
storage: '<your-storage-account-name>'
ContainerName: '<container-name>'
```

Dosya Kopyala görevini oluşturduktan sonra, hazırlanan şablonu dağıtmak için görevi eklemeye hazırsınız demektir.

Aşağıdaki YAML, [Azure Resource Manager şablonu dağıtım görevini](https://github.com/microsoft/azure-pipelines-tasks/blob/master/Tasks/AzureResourceManagerTemplateDeploymentV3/README.md)göstermektedir:

```yaml
- task: AzureResourceManagerTemplateDeployment@3
  inputs:
    deploymentScope: 'Resource Group'
    azureResourceManagerConnection: 'copy-connection'
    subscriptionId: '00000000-0000-0000-0000-000000000000'
    action: 'Create Or Update Resource Group'
    resourceGroupName: 'demogroup'
    location: 'West US'
    templateLocation: 'URL of the file'
    csmFileLink: '$(AzureFileCopy.StorageContainerUri)templates/mainTemplate.json$(AzureFileCopy.StorageContainerSasToken)'
    csmParametersFileLink: '$(AzureFileCopy.StorageContainerUri)templates/mainTemplate.parameters.json$(AzureFileCopy.StorageContainerSasToken)'
    deploymentMode: 'Incremental'
    deploymentName: 'deploy1'
```

Bu görevin daha ayrıntılı olarak incelemesinin birkaç bölümü vardır.

- `deploymentScope`:, Ve seçeneklerinden dağıtım kapsamını seçin: `Management Group` , `Subscription` , ve `Resource Group` . Kapsamlar hakkında daha fazla bilgi edinmek için bkz. [dağıtım kapsamları](deploy-rest.md#deployment-scope).

- `azureResourceManagerConnection`: Oluşturduğunuz hizmet bağlantısının adını belirtin.

- `subscriptionId`: Hedef abonelik KIMLIĞINI belirtin. Bu özellik yalnızca kaynak grubu dağıtım kapsamı ve abonelik Dağıtım kapsamı için geçerlidir.

- `resourceGroupName` ve `location` : dağıtmak istediğiniz kaynak grubunun adını ve konumunu belirtin. Görev, mevcut değilse kaynak grubunu oluşturur.

   ```yml
   resourceGroupName: '<resource-group-name>'
   location: '<location>'
   ```

- `csmFileLink`: Hazırlanan şablon için bağlantı sağlayın. Değer ayarlanırken, dosya kopyalama görevinden döndürülen değişkenleri kullanın. Aşağıdaki örnek, üzerinde mainTemplate.jsadlı bir şablona bağlantı sağlar. Dosya kopyalama görevinin dosyayı kopyalandığı konum olduğu için **şablon** adlı klasör dahil edilmiştir. İşlem hattınızda şablonunuzun yolunu ve şablonunuzun adını belirtin.

   ```yml
   csmFileLink: '$(AzureFileCopy.StorageContainerUri)templates/mainTemplate.json$(AzureFileCopy.StorageContainerSasToken)'
   ```

İşlem hattınızda şöyle görünür:

```yml
trigger:
- master

pool:
  vmImage: 'windows-latest'

steps:
- task: AzureFileCopy@4
  inputs:
    SourcePath: 'templates'
    azureSubscription: 'copy-connection'
    Destination: 'AzureBlob'
    storage: 'demostorage'
    ContainerName: 'projecttemplates'
  name: AzureFileCopy
- task: AzureResourceManagerTemplateDeployment@3
  inputs:
    deploymentScope: 'Resource Group'
    azureResourceManagerConnection: 'copy-connection'
    subscriptionId: '00000000-0000-0000-0000-000000000000'
    action: 'Create Or Update Resource Group'
    resourceGroupName: 'demogroup'
    location: 'West US'
    templateLocation: 'URL of the file'
    csmFileLink: '$(AzureFileCopy.StorageContainerUri)templates/mainTemplate.json$(AzureFileCopy.StorageContainerSasToken)'
    csmParametersFileLink: '$(AzureFileCopy.StorageContainerUri)templates/mainTemplate.parameters.json$(AzureFileCopy.StorageContainerSasToken)'
    deploymentMode: 'Incremental'
    deploymentName: 'deploy1'
```

**Kaydet**' i seçtiğinizde, derleme işlem hattı otomatik olarak çalıştırılır. Derleme işlem hattınızla ilgili özete dönün ve durumu izleyin.

## <a name="next-steps"></a>Sonraki adımlar

GitHub eylemleri ile ARM şablonlarını kullanma hakkında bilgi edinmek için bkz. [GitHub eylemlerini kullanarak Azure Resource Manager şablonlarını dağıtma](deploy-github-actions.md).
