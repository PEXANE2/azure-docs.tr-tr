---
title: Azure DevOps'leri kullanarak işlev uygulama kodunu sürekli güncelleştirin
description: Azure İşlevlerini hedefleyen bir Azure DevOps ardışık hattını nasıl ayarladığınızı öğrenin.
author: craigshoemaker
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: cshoe
ms.openlocfilehash: 5e2fc8fb06248e2cdad9067c56647da6d9626b50
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78255763"
---
# <a name="continuous-delivery-by-using-azure-devops"></a>Azure DevOps kullanarak sürekli teslim

Azure [Ardışık Hatlar'ı](/azure/devops/pipelines/)kullanarak işlevinizi otomatik olarak bir Azure İşlevler uygulamasına dağıtabilirsiniz.

Ardışık hattınızı tanımlamak için iki seçeneğiniz var:

- **YAML dosyası**: Bir YAML dosyası boru hattını açıklar. Dosyada yapı adımları bölümü ve sürüm bölümü olabilir. YAML dosyası uygulamayla aynı repoda olmalıdır.
- **Şablon**: Şablonlar, uygulamanızı oluşturan veya dağıtan hazır görevlerdir.

## <a name="yaml-based-pipeline"></a>YAML tabanlı boru hattı

YAML tabanlı bir boru hattı oluşturmak için önce uygulamanızı oluşturun ve ardından uygulamayı dağıtın.

### <a name="build-your-app"></a>Uygulamanızı oluşturma

Uygulamanızı Azure Ardışık Programlar'da nasıl oluşturduğunuz uygulamanızın programlama diline bağlıdır. Her dil, bir dağıtım artifakı oluşturan belirli yapı adımları vardır. İşlev uygulamanızı Azure'da dağıtmak için bir dağıtım arteişi kullanılır.

# <a name="c"></a>[C\#](#tab/csharp)

Bir .NET uygulaması oluşturmak için bir YAML dosyası oluşturmak için aşağıdaki örneği kullanabilirsiniz:

```yaml
pool:
      vmImage: 'VS2017-Win2016'
steps:
- script: |
    dotnet restore
    dotnet build --configuration Release
- task: DotNetCoreCLI@2
  inputs:
    command: publish
    arguments: '--configuration Release --output publish_output'
    projects: '*.csproj'
    publishWebProjects: false
    modifyOutputPath: false
    zipAfterPublish: false
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)/publish_output"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    artifactName: 'drop'
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

Bir JavaScript uygulaması oluşturmak için bir YAML dosyası oluşturmak için aşağıdaki örneği kullanabilirsiniz:

```yaml
pool:
      vmImage: ubuntu-16.04 # Use 'VS2017-Win2016' if you have Windows native +Node modules
steps:
- bash: |
    if [ -f extensions.csproj ]
    then
        dotnet build extensions.csproj --output ./bin
    fi
    npm install 
    npm run build --if-present
    npm prune --production
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    artifactName: 'drop'
```

# <a name="python"></a>[Python](#tab/python)

Belirli bir Python sürümü için bir uygulama oluşturmak için bir YAML dosyası oluşturmak için aşağıdaki örneklerden birini kullanabilirsiniz. Python yalnızca Linux üzerinde çalışan işlev uygulamaları için desteklenir.

**Sürüm 3.7**

```yaml
pool:
  vmImage: ubuntu-16.04
steps:
- task: UsePythonVersion@0
  displayName: "Setting python version to 3.7 as required by functions"
  inputs:
    versionSpec: '3.7'
    architecture: 'x64'
- bash: |
    if [ -f extensions.csproj ]
    then
        dotnet build extensions.csproj --output ./bin
    fi
    pip install --target="./.python_packages/lib/site-packages" -r ./requirements.txt
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    artifactName: 'drop'
```

**Sürüm 3.6**

```yaml
pool:
  vmImage: ubuntu-16.04
steps:
- task: UsePythonVersion@0
  displayName: "Setting python version to 3.6 as required by functions"
  inputs:
    versionSpec: '3.6'
    architecture: 'x64'
- bash: |
    if [ -f extensions.csproj ]
    then
        dotnet build extensions.csproj --output ./bin
    fi
    pip install --target="./.python_packages/lib/python3.6/site-packages" -r ./requirements.txt
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    artifactName: 'drop'
```

# <a name="powershell"></a>[Powershell](#tab/powershell)

PowerShell uygulamasını paketlemek için bir YAML dosyası oluşturmak için aşağıdaki örneği kullanabilirsiniz. PowerShell yalnızca Windows Azure İşlevleri için desteklenir.

```yaml
pool:
      vmImage: 'VS2017-Win2016'
steps:
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    artifactName: 'drop'
```

---

### <a name="deploy-your-app"></a>Uygulamanızı dağıtma

Barındırma işletim sistemi bağlı olarak, YAML dosyanıza aşağıdaki YAML örneklerinden birini eklemeniz gerekir.

#### <a name="windows-function-app"></a>Windows fonksiyon uygulaması

Bir Windows işlev uygulaması dağıtmak için aşağıdaki snippet'i kullanabilirsiniz:

```yaml
steps:
- task: AzureFunctionApp@1
  inputs:
    azureSubscription: '<Azure service connection>'
    appType: functionApp
    appName: '<Name of function app>'
    #Uncomment the next lines to deploy to a deployment slot
    #deployToSlotOrASE: true
    #resourceGroupName: '<Resource Group Name>'
    #slotName: '<Slot name>'
```

#### <a name="linux-function-app"></a>Linux fonksiyon uygulaması

Bir Linux işlev uygulaması dağıtmak için aşağıdaki snippet'i kullanabilirsiniz:

```yaml
steps:
- task: AzureFunctionApp@1
  inputs:
    azureSubscription: '<Azure service connection>'
    appType: functionAppLinux
    appName: '<Name of function app>'
    #Uncomment the next lines to deploy to a deployment slot
    #Note that deployment slots is not supported for Linux Dynamic SKU
    #deployToSlotOrASE: true
    #resourceGroupName: '<Resource Group Name>'
    #slotName: '<Slot name>'
```

## <a name="template-based-pipeline"></a>Şablon tabanlı ardışık lık

Azure DevOps'teki şablonlar, bir uygulama oluşturan veya dağıtan önceden tanımlanmış görev gruplarıdır.

### <a name="build-your-app"></a>Uygulamanızı oluşturma

Uygulamanızı Azure Ardışık Programlar'da nasıl oluşturduğunuz uygulamanızın programlama diline bağlıdır. Her dil, bir dağıtım artifakı oluşturan belirli yapı adımları vardır. Azure'daki işlev uygulamanızı güncelleştirmek için bir dağıtım yapı kullanılır.

Yerleşik yapı şablonlarını kullanmak için, yeni bir yapı denetim hattı oluşturduğunuzda, tasarımcı şablonlarını kullanarak bir ardışık yol üzerinde yardımcı oluşturmak için **klasik düzenleyiciyi kullanın'ı** seçin.

![Azure Ardışık İşleri klasik düzenleyicisini seçin](media/functions-how-to-azure-devops/classic-editor.png)

Kodunuzu yapılandırdıktan sonra Azure İşlevleri şablonları oluşturun. Uygulama dilinize uyan şablonu seçin.

![Azure İşlevler oluşturma şablonu seçin](media/functions-how-to-azure-devops/build-templates.png)

Bazı durumlarda yapı yapılarının belirli bir klasör yapısı vardır. **Yolları arşivlemek için Prepend kök klasör adını** seçmeniz gerekebilir.

![Kök klasör adını hazırlama seçeneği](media/functions-how-to-azure-devops/prepend-root-folder.png)

#### <a name="javascript-apps"></a>JavaScript uygulamaları

JavaScript uygulamanızın Windows yerel modüllerine bağımlılığı varsa, aracı havuzu sürümünü **Hosted VS2017**olarak güncelleştirmeniz gerekir.

![Aracı havuzu sürümünü güncelleştirme](media/functions-how-to-azure-devops/change-agent.png)

### <a name="deploy-your-app"></a>Uygulamanızı dağıtma

Yeni bir sürüm ardışık alanı oluşturduğunuzda, Azure İşlevleri sürüm şablonunu arayın.

![Azure İşlevleri sürüm şablonunu arama](media/functions-how-to-azure-devops/release-template.png)

Dağıtım yuvasına dağıtım, sürüm şablonunda desteklenmez.

## <a name="create-a-build-pipeline-by-using-the-azure-cli"></a>Azure CLI'yi kullanarak yapı ardışık yapısı oluşturma

Azure'da yapı ardışık bir `az functionapp devops-pipeline create` yapı oluşturmak için [komutu](/cli/azure/functionapp/devops-pipeline#az-functionapp-devops-pipeline-create)kullanın. Yapı ardışık hattı, repo'nuzda yapılan kod değişikliklerini oluşturmak ve serbest bırakmak için oluşturulur. Komut, yapı ve sürüm ardışık hattını tanımlayan ve sonra repo'nuza adatan yeni bir YAML dosyası oluşturur. Bu komutun ön koşulları kodunuzun konumuna bağlıdır.

- Kodunuz GitHub'daysa:

    - Aboneliğiniz için **yazma** izinleriniz olmalıdır.

    - Azure DevOps'te proje yöneticisi olmalısınız.

    - Yeterli izinlere sahip bir GitHub kişisel erişim jetonu (PAT) oluşturmak için izinlere sahip olmalısınız. Daha fazla bilgi için [GitHub PAT izin gereksinimlerine bakın.](https://aka.ms/azure-devops-source-repos)

    - Otomatik olarak oluşturulmuş YAML dosyasını işleyebilmeniz için GitHub deponuzdaki ana şubeye bağlanmak için izinleriniz olmalıdır.

- Kodunuz Azure Deposu'ndaysa:

    - Aboneliğiniz için **yazma** izinleriniz olmalıdır.

    - Azure DevOps'te proje yöneticisi olmalısınız.

## <a name="next-steps"></a>Sonraki adımlar

- Azure [İşlevleri genel görünümünü](functions-overview.md)gözden geçirin.
- Azure [DevOps genel görünümünü](/azure/devops/pipelines/)inceleyin.
