---
title: Azure DevTest Labs'daki laboratuvarınıza yapı deposu ekleme | Microsoft Dokümanlar
description: Azure DevTest laboratuarlarında laboratuvarınıza nasıl bir yapı deposu ekleyeceğinizi öğrenin.
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2019
ms.author: spelluru
ms.openlocfilehash: 2bb871119bece71c705ad9621a7c76c4b5ed0bc7
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770250"
---
# <a name="add-an-artifact-repository-to-your-lab-in-devtest-labs"></a>DevTest Labs'daki laboratuvarınıza bir yapı deposu ekleme
DevTest Labs, VM'nin oluşturulması sırasında veya VM oluşturulduktan sonra VM'ye eklenecek bir yapı belirtmenize olanak tanır. Bu yapı, VM'ye yüklemek istediğiniz bir araç veya uygulama olabilir. Yapılar, GitHub veya Azure DevOps Git deposundan yüklenen json dosyasında tanımlanır.

DevTest Labs tarafından sürdürülen [ortak yapı deposu,](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts)hem Windows hem de Linux için birçok yaygın araç sağlar. Bu deponun bağlantısı otomatik olarak laboratuvarınıza eklenir. Ortak yapı deposunda bulunmayan belirli araçlarla kendi yapı deponuzu oluşturabilirsiniz. Özel yapılar oluşturma hakkında bilgi edinmek için [bkz.](devtest-lab-artifact-author.md)

Bu makalede, Azure portalı, Azure Kaynak Yönetimi şablonları ve Azure PowerShell kullanarak özel yapı deponuzu nasıl ekleyeceğiniz hakkında bilgi verilmektedir. PowerShell veya CLI komut dosyaları yazarak laboratuvara yapı tevkiyonu eklemeyi otomatikleştirebilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar
Laboratuvarınıza bir depo eklemek için öncelikle deponuzdan önemli bilgileri alın. Aşağıdaki bölümlerde **GitHub** veya **Azure DevOps'te**barındırılan depolar için gerekli bilgilerin nasıl alınılması açıklanmaktadır.

### <a name="get-the-github-repository-clone-url-and-personal-access-token"></a>GitHub depo klon URL'sini ve kişisel erişim jetonunu alın

1. Yapı veya Kaynak Yöneticisi şablon tanımlarını içeren GitHub deposunun ana sayfasına gidin.
2. **Clone or download**'u (Kopyala veya indir) seçin.
3. URL'yi panoya kopyalamak için **HTTPS klon url** düğmesini seçin. URL'yi daha sonra kullanmak üzere kaydedin.
4. GitHub'ın sağ üst köşesinde profil görüntüsünü seçin ve ardından **Ayarlar'ı**seçin.
5. Soldaki **Kişisel ayarlar** menüsünde **Geliştirici Ayarları'nı**seçin.
6. Sol menüde **Kişisel erişim belirteçleri'ni** seçin.
7. **Yeni belirteç oluştur'u**seçin.
8. Yeni **kişisel erişim belirteci** sayfasında, **Belirteç açıklamasıaltında,** bir açıklama girin. Varsayılan öğeleri **Select kapsamları**altında kabul edin ve ardından **Token Oluştur'u**seçin.
9. Oluşturulan belirteci kaydedin. Jetonu daha sonra kullanırsın.
10. GitHub'ı kapatın.   

### <a name="get-the-azure-repos-clone-url-and-personal-access-token"></a>Azure Repos klon URL'sini ve kişisel erişim jetonunu alın
1. Takım koleksiyonunuzun ana sayfasına gidin `https://contoso-web-team.visualstudio.com`(örneğin), ve sonra projenizi seçin.
2. Proje ana sayfasında **Kod'u**seçin.
3. Proje **Kodu** sayfasında klon URL'sini görüntülemek için **Klon'u**seçin.
4. URL'yi kaydedin. URL'yi daha sonra kullanırsınız.
5. Kişisel erişim jetonu oluşturmak için, kullanıcı hesabı açılır menüsünde **Profilim'i**seçin.
6. Profil bilgileri sayfasında **Güvenlik'i**seçin.
7. Güvenlik **> Kişisel erişim belirteçleri** sekmesinde **+ Yeni Belirteç'i**seçin.
8. Yeni **bir kişisel erişim belirteci** sayfası oluşturun:
   1. Belirteci için bir **Ad** girin.
   2. **Kuruluş** listesinde, **tüm erişilebilir kuruluşları**seçin.
   3. Sona **Erme (UTC)** listesinde **90 gün**veya özel tanımlı bir son kullanma süresi seçin.
   4. Kapsamlar için **Tam erişim** seçeneğini seçin.
   5. **Oluştur**’u seçin.
9. Yeni belirteç Kişisel **Erişim Belirteçleri** listesinde görünür. **Token'i**kopyala'yı seçin ve ardından belirteç değerini daha sonra kullanmak üzere kaydedin.
10. Laboratuvarınızı depo bölümüne bağlayın.

## <a name="use-azure-portal"></a>Azure portalı kullanma
Bu bölümde, Azure portalındaki bir laboratuvara yapı deposu eklemek için adımlar sağlanmaktadır.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Daha Fazla Hizmet**seçin ve ardından hizmetler listesinden **DevTest Labs'ı** seçin.
3. Laboratuvarlar listesinden, laboratuvarınızı seçin.
4. Sol menüde **Yapılandırma ve ilkeler'i** seçin.
5. Sol menüde **Dış kaynaklar** bölümünün altındaki **Depolar'ı** seçin.
6. Araç çubuğuna **+ Ekle'yi** seçin.

    ![Depo Ekle düğmesi](./media/devtest-lab-add-repo/devtestlab-add-repo.png)
5. **Depolar** sayfasında aşağıdaki bilgileri belirtin:
   1. **Adı**. Depo için bir ad girin.
   2. **Git Klon Url**. Daha önce GitHub veya Azure DevOps Hizmetlerinden kopyaladığınız Git HTTPS klon URL'sini girin.
   3. **Şube**. Tanımlarınızı almak için şubeyi girin.
   4. **Kişisel Erişim Belirteci**. GitHub veya Azure DevOps Hizmetleri'nden daha önce aldığınız kişisel erişim jetonunu girin.
   5. **Klasör Yolları**. Yapı veya Kaynak Yöneticisi şablon tanımlarınızı içeren klon URL'sine göre en az bir klasör yolu girin. Bir alt dizini belirttiğiniz zaman, klasör yoluna ileri eğik çizgieklediğinizden emin olun.

        ![Depolar alanı](./media/devtest-lab-add-repo/devtestlab-repo-blade.png)
6. **Kaydet**’i seçin.

## <a name="use-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanma
Azure Kaynak Yönetimi (Azure Kaynak Yöneticisi) şablonları, Azure'da oluşturmak istediğiniz kaynakları açıklayan JSON dosyalarıdır. Bu şablonlar hakkında daha fazla bilgi için [bkz.](../azure-resource-manager/templates/template-syntax.md)

Bu bölümde, Azure Kaynak Yöneticisi şablonu kullanarak bir laboratuvara yapı deposu eklemek için adımlar sağlanmaktadır.  Şablon, zaten yoksa laboratuarı oluşturur.

### <a name="template"></a>Şablon
Bu makalede kullanılan örnek şablon parametreler aracılığıyla aşağıdaki bilgileri toplar. Parametrelerin çoğu akıllı varsayılanlara sahiptir, ancak belirtilmesi gereken birkaç değer vardır. Yapı deposu için laboratuvar adını, URI'yi ve deponun güvenlik belirteci belirtmeniz gerekir.

- Laboratuvar adı.
- DevTest Labs kullanıcı arabiriminde (UI) yapı deposunun görüntü adı. Varsayılan değer: `Team Repository`.
- Uri deposuna (Örnek: `https://github.com/<myteam>/<nameofrepo>.git` veya `"https://MyProject1.visualstudio.com/DefaultCollection/_git/TeamArtifacts"`.
- Yapıtları içeren depodaki dal. Varsayılan değer: `master`.
- Yapıtları içeren klasörün adı. Varsayılan değer: `/Artifacts`.
- Deponun türü. İzin verilen `VsoGit` `GitHub`değerler veya .
- Depo için giriş belirteci.

    ```json
    {

        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "labName": {
                "type": "string"
            },
            "artifactRepositoryDisplayName": {
                "type": "string",
                "defaultValue": "Team Repository"
            },
            "artifactRepoUri": {
                "type": "string"
            },
            "artifactRepoBranch": {
                "type": "string",
                "defaultValue": "master"
            },
            "artifactRepoFolder": {
                "type": "string",
                "defaultValue": "/Artifacts"
            },
            "artifactRepoType": {
                "type": "string",
                "allowedValues": ["VsoGit", "GitHub"]
            },
            "artifactRepoSecurityToken": {
                "type": "securestring"
            }
        },
        "variables": {
            "artifactRepositoryName": "[concat('Repo-', uniqueString(subscription().subscriptionId))]"
        },
        "resources": [{
                "apiVersion": "2016-05-15",
                "type": "Microsoft.DevTestLab/labs",
                "name": "[parameters('labName')]",
                "location": "[resourceGroup().location]",
                "resources": [
                    {
                        "apiVersion": "2016-05-15",
                        "name": "[variables('artifactRepositoryName')]",
                        "type": "artifactSources",
                        "dependsOn": [
                            "[resourceId('Microsoft.DevTestLab/labs', parameters('labName'))]"
                        ],
                        "properties": {
                            "uri": "[parameters('artifactRepoUri')]",
                            "folderPath": "[parameters('artifactRepoFolder')]",
                            "branchRef": "[parameters('artifactRepoBranch')]",
                            "displayName": "[parameters('artifactRepositoryDisplayName')]",
                            "securityToken": "[parameters('artifactRepoSecurityToken')]",
                            "sourceType": "[parameters('artifactRepoType')]",
                            "status": "Enabled"
                        }
                    }
                ]
            }
        ]
    }
    ```


### <a name="deploy-the-template"></a>Şablonu dağıtma
Şablonu Azure'a dağıtmanın ve varsa kaynağın oluşturulmasını veya yoksa güncelleştirilen birkaç yolu vardır. Ayrıntılar için aşağıdaki makalelere bakın:

- [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../azure-resource-manager/templates/deploy-powershell.md)
- [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../azure-resource-manager/templates/deploy-cli.md)
- [Kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](../azure-resource-manager/templates/deploy-portal.md)
- [Kaynakları Resource Manager şablonları ve Resource Manager REST API’si ile dağıtma](../azure-resource-manager/templates/deploy-rest.md)

PowerShell'de şablonun nasıl dağıtılanabildiğini görelim. Şablonu dağıtmak için kullanılan cmdlet'ler içeriğe özgüolduğundan, geçerli kiracı ve geçerli abonelik kullanılır. Bağlamı değiştirmek için şablonu dağıtmadan önce [Set-AzContext'ı](/powershell/module/az.accounts/set-azcontext) kullanın.

İlk olarak, [Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)kullanarak bir kaynak grubu oluşturun. Kullanmak istediğiniz kaynak grubu zaten varsa, bu adımı atlayın.

```powershell
New-AzResourceGroup -Name MyLabResourceGroup1 -Location westus
```

Ardından, [Yeni-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment)kullanarak kaynak grubuna bir dağıtım oluşturun. Bu cmdlet, kaynak değişikliklerini Azure'a uygular. Belirli bir kaynak grubuna çeşitli kaynak dağıtımları yapılabilir. Aynı kaynak grubuna birkaç kez dağıtıyorsanız, her dağıtımın adının benzersiz olduğundan emin olun.

```powershell
New-AzResourceGroupDeployment `
    -Name MyLabResourceGroup-Deployment1 `
    -ResourceGroupName MyLabResourceGroup1 `
    -TemplateFile azuredeploy.json `
    -TemplateParameterFile azuredeploy.parameters.json
```

Yeni-AzResourceGroupDeployment başarıyla çalıştırıldıktan sonra, komut sağlama durumu (başarılı olmalıdır) ve şablon için herhangi bir çıktı gibi önemli bilgiler çıktıları.

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma
Bu bölümde, bir laboratuvara bir yapı deposu eklemek için kullanılabilecek örnek bir PowerShell komut dosyası sağlar. Azure PowerShell'inyoksa, yüklemek için ayrıntılı talimatlar için [Azure PowerShell'i nasıl yükleyip yapılandıracağınıza](/powershell/azure/overview?view=azps-1.2.0) bakın.

### <a name="full-script"></a>Tam betik
Burada bazı ayrıntılı mesajlar ve yorumlar da dahil olmak üzere tam komut dosyası:

**Yeni-DevTestLabArtifactRepository.ps1**:

```powershell

<#

.SYNOPSIS
This script creates a new custom repository and adds it to an existing DevTest Lab.

.PARAMETER LabName
The name of the lab.

.PARAMETER LabResourceGroupName
The name of the resource group that contains the lab.

.PARAMETER ArtifactRepositoryName
Name for the new artifact repository.
Script creates a random name for the respository if it is not specified.

.PARAMETER ArtifactRepositoryDisplayName
Display name for the artifact repository.
This is the name that shows up in the Azure portal (https://portal.azure.com) when viewing all the artifact repositories for a lab.

.PARAMETER RepositoryUri
Uri to the repository.

.PARAMETER RepositoryBranch
Branch in which artifact files can be found. Defaults to 'master'.

.PARAMETER FolderPath
Folder under which artifacts can be found. Defaults to '/Artifacts'

.PARAMETER PersonalAccessToken
Security token for access to GitHub or VSOGit repository.
See https://azure.microsoft.com/documentation/articles/devtest-lab-add-artifact-repo/ for instructions to get personal access token

.PARAMETER SourceType
Whether artifact is VSOGit or GitHub repository.

.EXAMPLE
Set-AzContext -SubscriptionId 11111111-1111-1111-1111-111111111111
.\New-DevTestLabArtifactRepository.ps1 -LabName "mydevtestlab" -LabResourceGroupName "mydtlrg" -ArtifactRepositoryName "MyTeam Repository" -RepositoryUri "https://github.com/<myteam>/<nameofrepo>.git" -PersonalAccessToken "1111...." -SourceType "GitHub"

.NOTES
Script uses the current Az context. To set the context, use the Set-AzContext cmdlet

#>


[CmdletBinding()]
Param(

    [Parameter(Mandatory=$true)]
    $LabName,

    [Parameter(Mandatory=$true)]
    $LabResourceGroupName,
    $ArtifactRepositoryName,
    $ArtifactRepositoryDisplayName  = 'Team Artifact Repository',

    [Parameter(Mandatory=$true)]
    $RepositoryUri,
    $RepositoryBranch = 'master',
    $FolderPath = '/Artifacts',

    [Parameter(Mandatory=$true)]
    $PersonalAccessToken ,

    [Parameter(Mandatory=$true)]
    [ValidateSet('VsoGit', 'GitHub')]
    $SourceType
)


#Set artifact repository internal name,
# if not set by user.

if ($ArtifactRepositoryName -eq $null){
    $ArtifactRepositoryName = "PrivateRepo" + (Get-Random -Maximum 999)
}

# Sign in to Azure
Connect-AzAccount


#Get Lab Resource
$LabResource = Get-AzResource -ResourceType 'Microsoft.DevTestLab/labs' -ResourceName $LabName -ResourceGroupName $LabResourceGroupName

Write-Verbose "Lab Name: $($LabResource.Name)"
Write-Verbose "Lab Resource Group Name: $($LabResource.ResourceGroupName)"
Write-Verbose "Lab Resource Location: $($LabResource.Location)"

Write-Verbose "Artifact Repository Internal Name: $ArtifactRepositoryName"

#Prepare properties object for call to New-AzResource
$propertiesObject = @{
    uri = $RepositoryUri;
    folderPath = $FolderPath;
    branchRef = $RepositoryBranch;
    displayName = $ArtifactRepositoryDisplayName;
    securityToken = $PersonalAccessToken;
    sourceType = $SourceType;
    status = 'Enabled'
}

Write-Verbose @"Properties to be passed to New-AzResource:$($propertiesObject | Out-String)"

#Resource will be added to current subscription.
$resourcetype = 'Microsoft.DevTestLab/labs/artifactSources'
$resourceName = $LabName + '/' + $ArtifactRepositoryName
Write-Verbose "Az ResourceType: $resourcetype"
Write-Verbose "Az ResourceName: $resourceName"

Write-Verbose "Creating artifact repository '$ArtifactRepositoryDisplayName'..."
$result = New-AzResource -Location $LabResource.Location -ResourceGroupName $LabResource.ResourceGroupName -properties $propertiesObject -ResourceType $resourcetype -ResourceName $resourceName -ApiVersion 2016-05-15 -Force


#Alternate implementation:
# Use resourceId rather than resourcetype and resourcename parameters.
# Using resourceId allows you to specify the $SubscriptionId rather than using the
# subscription id of Get-AzContext.
#$resourceId = "/subscriptions/$SubscriptionId/resourceGroups/$($LabResource.ResourceGroupName)/providers/Microsoft.DevTestLab/labs/$LabName/artifactSources/$ArtifactRepositoryName"
#$result = New-AzResource -properties $propertiesObject -ResourceId $resourceId -ApiVersion 2016-05-15 -Force


# Check the result
if ($result.Properties.ProvisioningState -eq "Succeeded") {
    Write-Verbose ("Successfully added artifact repository source '$ArtifactRepositoryDisplayName'")
}
else {
    Write-Error ("Error adding artifact repository source '$ArtifactRepositoryDisplayName'")
}

#Return the newly created resource so it may be used in subsequent scripts
return $result
```

### <a name="run-the-powershell-script"></a>PowerShell betiğini çalıştırma
Aşağıdaki örnekte komut dosyasınasıl çalıştırılasınız gösterilmektedir:

```powershell
Set-AzContext -SubscriptionId <Your Azure subscription ID>

.\New-DevTestLabArtifactRepository.ps1 -LabName "mydevtestlab" -LabResourceGroupName "mydtlrg" -ArtifactRepositoryName "MyTeam Repository" -RepositoryUri "https://github.com/<myteam>/<nameofrepo>.git" -PersonalAccessToken "1111...." -SourceType "GitHub"
```


### <a name="parameters"></a>Parametreler
Bu makaledeki örnek PowerShell komut dosyası aşağıdaki parametreleri alır:

| Parametre | Açıklama |
| --------- | ----------- |
| Laboratuvar Adı | Laboratuvarın adı. |
| ArtifactRepositoryName | Yeni eser deposunun adı. Komut dosyası belirtilmemişse depo için rasgele bir ad oluşturur. |
| ArtifactRepositoryDisplayName | Yapı deposunun görüntü adı. Bu, Azure portalında (bir laboratuvarhttps://portal.azure.com) için tüm yapı depolarını görüntülerken) görünen addır. |
| DepoUri | Uri depoya. Örnekler: `https://github.com/<myteam>/<nameofrepo>.git` `"https://MyProject1.visualstudio.com/DefaultCollection/_git/TeamArtifacts"`veya .|
| DepoŞubesi | Yapı dosyalarının bulunabileceği dal. Varsayılan olarak 'ana' olarak verilir. |
| FolderPath | Yapıtların bulunabileceği klasör. Varsayılan olarak '/Artefakt' |
| PersonalAccessToken | GitHub veya VSOGit deposuna erişmek için güvenlik belirteci. Kişisel erişim jetonuna erişim için talimatlar için ön koşullar bölümüne bakın |
| KaynakTürü | Artifakı VSOGit veya GitHub deposu olup olmadığı. |

Deponun tanımlama için kendi iç adı gerekir ve bu da Azure portalında görülen görüntü adının farklı olmasıdır. Azure portalını kullanarak dahili adı göremezsiniz, ancak Azure REST API'lerini veya Azure PowerShell'i kullanırken görürsünüz. Komut dosyası, komut dosyamızın kullanıcısı tarafından belirtilmemişse, bir ad sağlar.

```powershell
#Set artifact repository name, if not set by user
if ($ArtifactRepositoryName -eq $null){
    $ArtifactRepositoryName = "PrivateRepo" + (Get-Random -Maximum 999)
}
```

### <a name="powershell-commands-used-in-the-script"></a>Komut dosyasında kullanılan PowerShell komutları

| PowerShell komutu | Notlar |
| ------------------ | ----- |
| [Al-AzResource](/powershell/module/az.resources/get-azresource) | Bu komut, konumu gibi laboratuar hakkında ayrıntıları almak için kullanılır. |
| [Yeni Kaynak](/powershell/module/az.resources/new-azresource) | Yapı depoları eklemek için özel bir komut yoktur. Genel [Yeni-AzResource](/powershell/module/az.resources/new-azresource) cmdlet işi yapar. Bu cmdlet oluşturmak için kaynak türünü bilmek için **ResourceId** veya **ResourceName** ve **ResourceType** çifti gerekir. Bu örnek komut dosyası kaynak adı ve kaynak türü çiftini kullanır. <br/><br/>Yapı deposu kaynağını aynı konumda ve laboratuarla aynı kaynak grubu altında oluşturduğunuzu unutmayın.|

Komut dosyası, geçerli aboneüye yeni bir kaynak ekler. Bu bilgileri görmek için [Get-AzContext'ı](/powershell/module/az.accounts/get-azcontext) kullanın. Geçerli kiracıyı ve aboneliği ayarlamak için [Set-AzContext'ı](/powershell/module/az.accounts/set-azcontext) kullanın.

Kaynak adı ve kaynak türü bilgilerini keşfetmenin en iyi yolu [Test Sürüşü Azure REST API'leri](https://azure.github.io/projects/apis/) web sitesini kullanmaktır. DevTest Labs sağlayıcısı için mevcut REST API'lerini görmek için [DevTest Labs – 2016-05-15](https://aka.ms/dtlrestapis) sağlayıcısına göz atın. Komut dosyası aşağıdaki kaynak kimliğini kullansın.

```powershell
"/subscriptions/$SubscriptionId/resourceGroups/$($LabResource.ResourceGroupName)/providers/Microsoft.DevTestLab/labs/$LabName/artifactSources/$ArtifactRepositoryName"
```

Kaynak türü, kıvırcık paranteziçinde listelenen öğeler dışında URI'de 'sağlayıcılar'dan sonra listelenen her şeydir. Kaynak adı kıvırcık parantez içinde görülen her şey. Kaynak adı için birden fazla öğe bekleniyorsa, her öğeyi bizim yaptığımız gibi bir kesikle ayırın.

```powershell
$resourcetype = 'Microsoft.DevTestLab/labs/artifactSources'
$resourceName = $LabName + '/' + $ArtifactRepositoryName
```


## <a name="next-steps"></a>Sonraki adımlar
- [Azure DevTest Labs'daki laboratuvarınız için zorunlu yapıları belirtin](devtest-lab-mandatory-artifacts.md)
- [DevTest Labs sanal makineniz için özel yapılar oluşturun](devtest-lab-artifact-author.md)
- [Yapı tonularını laboratuarda tanılama](devtest-lab-troubleshoot-artifact-failure.md)
