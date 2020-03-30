---
title: Azure DevTest Labs'da VM'ye yapı ekleme | Microsoft Dokümanlar
description: Azure DevTest Labs'daki bir laboratuvarda sanal makineye yapı eklemeyi öğrenin
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: 27fec279582d845972b87ac635c87c16c239924e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73901321"
---
# <a name="add-an-artifact-to-a-vm"></a>VM’ye yapıt ekleme
VM oluştururken, varolan yapıları ekleyebilirsiniz. Bu eserler ya kamu [DevTest Labs Git deposundan](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) veya kendi Git deposundan olabilir. Bu makalede, Azure portalına ve Azure PowerShell'i kullanarak yapıların nasıl ekleyeceğiniz gösterilmektedir. 

Azure DevTest Labs *yapıları,* Windows PowerShell komutları çalıştırma, Bash komutlarını çalıştırma ve yazılım yükleme gibi VM sağlandığında gerçekleştirilen *eylemleri* belirtmenize izin tanır. Yapı *parametreleri,* belirli senaryonuz için yapıyı özelleştirmenize izin sağlar.

Özel yapılar nasıl oluşturulacak hakkında bilgi edinmek için makaleye bakın: [Özel yapılar oluşturun.](devtest-lab-artifact-author.md)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="use-azure-portal"></a>Azure portalı kullanma 
1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.
1. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.
1. Laboratuvarlar listesinden, çalışmak istediğiniz VM'yi içeren laboratuarı seçin.  
1. **Sanal makinelerimi**seçin.
1. İstediğiniz VM'yi seçin.
1. **Yapıtları Yönet'i**seçin. 
1. **Yapıtları Uygula'yı**seçin.
1. **Yapıtları Uygula** bölmesine, VM'ye eklemek istediğiniz yapıyı seçin.
1. Yapı **ekle** bölmesine, gerekli parametre değerlerini ve ihtiyacınız olan isteğe bağlı parametreleri girin.  
1. Yapıyı eklemek ve **yapıtları uygula** bölmesine dönmek için **Ekle'yi** seçin.
1. VM'iniz için gerektiği gibi yapı eklemeye devam edin.
1. Yapıtlarınızı ekledikten sonra, [yapıtların çalıştırılan sırasını değiştirebilirsiniz.](#change-the-order-in-which-artifacts-are-run) Ayrıca [bir yapıyı görüntülemek veya değiştirmek](#view-or-modify-an-artifact)için geri dönebilirsiniz.
1. Yapı eklemeyi bitirdiğinizde Uygula'yı **seçin**

### <a name="change-the-order-in-which-artifacts-are-run"></a>Yapıtların çalıştırılme sırasını değiştirme
Varsayılan olarak, yapıtların eylemleri VM'ye eklendikleri sırada yürütülür. Aşağıdaki adımlar, yapıtların çalıştırılan sırayı nasıl değiştireceğini gösterir.

1. **Yapıtları Uygula** bölmesinin üst kısmında, VM'ye eklenen yapıların sayısını belirten bağlantıyı seçin.
   
    ![VM'ye eklenen yapı sayısı](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. **Seçili eserler** bölmesindeki, yapıları sürükleyin ve istenilen sıraya bırakın. Yapıyı sürüklemede sorun yaşıyorsanız, yapının sol tarafından sürüklediğinizden emin olun. 
1. Tamamladığınızda **Tamam**’ı seçin.  

### <a name="view-or-modify-an-artifact"></a>Bir yapıyı görüntüleme veya değiştirme
Aşağıdaki adımlar, bir yapının parametrelerini nasıl görüntülenebildiğini veya değiştirilebildiğini gösterir:

1. **Yapıtları Uygula** bölmesinin üst kısmında, VM'ye eklenen yapıların sayısını belirten bağlantıyı seçin.
   
    ![VM'ye eklenen yapı sayısı](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. **Seçili yapılar** bölmesinde, görüntülemek veya görüntülemek istediğiniz yapıyı seçin.  
1. Yapı **ekle** bölmesine, gerekli değişiklikleri yapın ve yapı **ekle** bölmesini kapatmak için **Tamam'ı** seçin.
1. **Seçili yapılar** bölmesini kapatmak için **Tamam'ı** seçin.

## <a name="use-powershell"></a>PowerShell kullanma
Aşağıdaki komut dosyası belirtilen yapıyı belirtilen VM'ye uygular. [Invoke-AzResourceAction](/powershell/module/az.resources/invoke-azresourceaction) komutu işlemi gerçekleştiren komutudur.  

```powershell
#Requires -Module Az.Resources

param
(
[Parameter(Mandatory=$true, HelpMessage="The ID of the subscription that contains the lab")]
   [string] $SubscriptionId,
[Parameter(Mandatory=$true, HelpMessage="The name of the lab containing the virtual machine")]
   [string] $DevTestLabName,
[Parameter(Mandatory=$true, HelpMessage="The name of the virtual machine")]
   [string] $VirtualMachineName,
[Parameter(Mandatory=$true, HelpMessage="The repository where the artifact is stored")]
   [string] $RepositoryName,
[Parameter(Mandatory=$true, HelpMessage="The artifact to apply to the virtual machine")]
   [string] $ArtifactName,
[Parameter(ValueFromRemainingArguments=$true)]
   $Params
)

# Set the appropriate subscription
Set-AzContext -SubscriptionId $SubscriptionId | Out-Null
 
# Get the lab resource group name
$resourceGroupName = (Get-AzResource -ResourceType 'Microsoft.DevTestLab/labs' | Where-Object { $_.Name -eq $DevTestLabName}).ResourceGroupName
if ($resourceGroupName -eq $null) { throw "Unable to find lab $DevTestLabName in subscription $SubscriptionId." }

# Get the internal repo name
$repository = Get-AzResource -ResourceGroupName $resourceGroupName `
                    -ResourceType 'Microsoft.DevTestLab/labs/artifactsources' `
                    -ResourceName $DevTestLabName `
                    -ApiVersion 2016-05-15 `
                    | Where-Object { $RepositoryName -in ($_.Name, $_.Properties.displayName) } `
                    | Select-Object -First 1

if ($repository -eq $null) { "Unable to find repository $RepositoryName in lab $DevTestLabName." }

# Get the internal artifact name
$template = Get-AzResource -ResourceGroupName $resourceGroupName `
                -ResourceType "Microsoft.DevTestLab/labs/artifactSources/artifacts" `
                -ResourceName "$DevTestLabName/$($repository.Name)" `
                -ApiVersion 2016-05-15 `
                | Where-Object { $ArtifactName -in ($_.Name, $_.Properties.title) } `
                | Select-Object -First 1

if ($template -eq $null) { throw "Unable to find template $ArtifactName in lab $DevTestLabName." }

# Find the virtual machine in Azure
$FullVMId = "/subscriptions/$SubscriptionId/resourceGroups/$resourceGroupName`
                /providers/Microsoft.DevTestLab/labs/$DevTestLabName/virtualmachines/$virtualMachineName"

$virtualMachine = Get-AzResource -ResourceId $FullVMId

# Generate the artifact id
$FullArtifactId = "/subscriptions/$SubscriptionId/resourceGroups/$resourceGroupName`
                        /providers/Microsoft.DevTestLab/labs/$DevTestLabName/artifactSources/$($repository.Name)`
                        /artifacts/$($template.Name)"

# Handle the inputted parameters to pass through
$artifactParameters = @()

# Fill artifact parameter with the additional -param_ data and strip off the -param_
$Params | ForEach-Object {
   if ($_ -match '^-param_(.*)') {
      $name = $_.TrimStart('^-param_')
   } elseif ( $name ) {
      $artifactParameters += @{ "name" = "$name"; "value" = "$_" }
      $name = $null #reset name variable
   }
}

# Create structure for the artifact data to be passed to the action

$prop = @{
artifacts = @(
    @{
        artifactId = $FullArtifactId
        parameters = $artifactParameters
    }
    )
}

# Check the VM
if ($virtualMachine -ne $null) {
   # Apply the artifact by name to the virtual machine
   $status = Invoke-AzResourceAction -Parameters $prop -ResourceId $virtualMachine.ResourceId -Action "applyArtifacts" -ApiVersion 2016-05-15 -Force
   if ($status.Status -eq 'Succeeded') {
      Write-Output "##[section] Successfully applied artifact: $ArtifactName to $VirtualMachineName"
   } else {
      Write-Error "##[error]Failed to apply artifact: $ArtifactName to $VirtualMachineName"
   }
} else {
   Write-Error "##[error]$VirtualMachine was not found in the DevTest Lab, unable to apply the artifact"
}

```

## <a name="next-steps"></a>Sonraki adımlar
Eserler le ilgili aşağıdaki makalelere bakın:

- [Laboratuvarınız için zorunlu yapıları belirtin](devtest-lab-mandatory-artifacts.md)
- [Özel yapıtlar oluşturma](devtest-lab-artifact-author.md)
- [Laboratuvara yapıt deposu ekleme](devtest-lab-artifact-author.md)
- [Yapıt hatalarını tanılama](devtest-lab-troubleshoot-artifact-failure.md)
