---
title: PowerShell ile Azure portal panosu oluşturma
description: Azure PowerShell kullanarak Azure portal Pano oluşturmayı öğrenin.
author: mgblythe
ms.service: azure-portal
ms.topic: quickstart
ms.custom: devx-track-azurepowershell
ms.author: mblythe
ms.date: 07/24/2020
ms.openlocfilehash: 6b7a4f6d4ad7f5e94d19b9d531992f54ff13fec0
ms.sourcegitcommit: 14bf4129a73de2b51a575c3a0a7a3b9c86387b2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87440787"
---
# <a name="quickstart-create-an-azure-portal-dashboard-with-powershell"></a>Hızlı başlangıç: PowerShell ile Azure portal panosu oluşturma

Azure portal bir Pano, bulut kaynaklarınızın odaklanmış ve düzenlenmiş bir görünümüdür. Bu makalede, bir pano oluşturmak için az. Portal PowerShell modülünü kullanma işlemine odaklanılır.
Pano, bir sanal makinenin (VM) ve bazı statik bilgi ve bağlantıların performansını gösterir.

## <a name="requirements"></a>Gereksinimler

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

PowerShell 'i yerel olarak kullanmayı seçerseniz, bu makale az PowerShell modülünü yüklemenizi ve [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) cmdlet 'Ini kullanarak Azure hesabınıza bağlanmanızı gerektirir. Az PowerShell modülünü yükleme hakkında daha fazla bilgi için bkz. [yükleme Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

> [!IMPORTANT]
> **Az. Portal** PowerShell modülü önizlemedeyken, cmdlet 'Ini kullanarak az PowerShell modülünden ayrı olarak yüklemelisiniz `Install-Module` . Bu PowerShell modülü genel kullanıma sunulduğunda, bu, gelecekteki az PowerShell modülü sürümlerinin bir parçası haline gelir ve Azure Cloud Shell içinden yerel olarak kullanılabilir.

```azurepowershell-interactive
Install-Module -Name Az.Portal
```

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

## <a name="choose-a-specific-azure-subscription"></a>Belirli bir Azure aboneliği seçin

Birden çok Azure aboneliğiniz varsa, kaynakların faturalandırılması gereken uygun aboneliği seçin. [Set-AzContext](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext) cmdlet 'ini kullanarak belirli bir abonelik seçin.

```azurepowershell-interactive
Set-AzContext -SubscriptionId 00000000-0000-0000-0000-000000000000
```

## <a name="define-variables"></a>Değişkenleri tanımlama

Birkaç bilgi parçasını sürekli olarak kullanacaksınız. Bilgileri depolamak için değişkenler oluşturun.

```azurepowershell-interactive
# Name of resource group used throughout this article
$resourceGroupName = 'myResourceGroup'

# Azure region
$location = 'centralus'

# Dashboard Title
$dashboardTitle = 'Simple VM Dashboard'

# Dashboard Name
$dashboardName = $dashboardTitle -replace '\s'

# Your Azure Subscription ID
$subscriptionID = (Get-AzContext).Subscription.Id

# Name of test VM
$vmName = 'SimpleWinVM'
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) cmdlet 'ini kullanarak bir [Azure Kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek, değişkeninde belirtilen bölgedeki ada göre bir kaynak grubu oluşturur `$resourceGroupName` `$location` .

```azurepowershell-interactive
New-AzResourceGroup -Name $resourceGroupName -Location $location
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Bu hızlı başlangıç bölümünde oluşturduğunuz Pano, mevcut bir VM gerektirir. Bu adımları izleyerek bir VM oluşturun.

VM için oturum açma kimlik bilgilerini bir değişkende depolayın. Parolanın karmaşık olması gerekir. Bu yeni bir Kullanıcı adı ve paroladır; Örneğin, Azure 'da oturum açmak için kullandığınız hesap değildir. Daha fazla bilgi için bkz. [Kullanıcı adı gereksinimleri](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm) ve [Parola gereksinimleri](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

```azurepowershell-interactive
$Cred = Get-Credential
```

VM 'yi oluşturun.

```azurepowershell-interactive
$AzVmParams = @{
  ResourceGroupName = $resourceGroupName
  Name = $vmName
  Location = $location
  Credential = $Cred
}
New-AzVm @AzVmParams
```

VM dağıtımı artık başlar ve genellikle birkaç dakika sürer. Dağıtım tamamlandıktan sonra bir sonraki bölüme geçin.

## <a name="download-the-dashboard-template"></a>Pano şablonunu indirme

Azure panoları kaynak olduğundan, bu değerler JSON olarak temsil edilebilir. Aşağıdaki kod, örnek bir panonun JSON temsilini indirir. Daha fazla bilgi için bkz. [Azure panoları yapısı](/azure/azure-portal/azure-portal-dashboards-structure).

```azurepowershell-interactive
$myPortalDashboardTemplateUrl = 'https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/azure-portal/portal-dashboard-template-testvm.json'

$myPortalDashboardTemplatePath = "$env:TEMP\portal-dashboard-template-testvm.json"

Invoke-WebRequest -Uri $myPortalDashboardTemplateUrl -OutFile $myPortalDashboardTemplatePath -UseBasicParsing
```

## <a name="customize-the-template"></a>Şablonu özelleştirme

Aşağıdaki kodu çalıştırarak indirilen şablonu özelleştirin.

```azurepowershell
$Content = Get-Content -Path $myPortalDashboardTemplatePath -Raw
$Content = $Content -replace '<subscriptionID>', $subscriptionID
$Content = $Content -replace '<rgName>', $resourceGroupName
$Content = $Content -replace '<vmName>', $vmName
$Content = $Content -replace '<dashboardTitle>', $dashboardTitle
$Content = $Content -replace '<location>', $location
$Content | Out-File -FilePath $myPortalDashboardTemplatePath -Force
```

Daha fazla bilgi için bkz. [Microsoft Portal panoları şablon başvurusu](/azure/templates/microsoft.portal/dashboards).

## <a name="deploy-the-dashboard-template"></a>Pano şablonunu dağıtma

`New-AzPortalDashboard`Şablonu doğrudan PowerShell 'den dağıtmak Için az. Portal modülünün bir parçası olan cmdlet 'ini kullanabilirsiniz.

```azurepowershell
$DashboardParams = @{
  DashboardPath = $myPortalDashboardTemplatePath
  ResourceGroupName = $resourceGroupName
  DashboardName = $dashboardName
}
New-AzPortalDashboard @DashboardParams
```

## <a name="review-the-deployed-resources"></a>Dağıtılan kaynakları gözden geçirin

Panonun başarıyla oluşturulduğunu denetleyin.

```azurepowershell
Get-AzPortalDashboard -Name $dashboardName -ResourceGroupName $resourceGroupName
```

VM hakkındaki verileri Azure portal içinden görebildiğinizi doğrulayın.

1. Azure portalında **Pano**’yu seçin.

   ![Panoya gezinti Azure portal](media/quickstart-portal-dashboard-powershell/navigate-to-dashboards.png)

1. Pano sayfasında, **basıt VM panosu**' nu seçin.

   ![Basit VM panosuna git](media/quickstart-portal-dashboard-powershell/select-simple-vm-dashboard.png)

1. Panoyu gözden geçirin. İçeriğin bazılarının statik olduğunu, ancak VM 'nin performansını gösteren grafikler de olduğunu görebilirsiniz.

   ![Basit VM panosunu gözden geçirme](media/quickstart-portal-dashboard-powershell/review-simple-vm-dashboard.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

VM 'yi ve ilişkili panoyu kaldırmak için, bunları içeren kaynak grubunu silin.

> [!CAUTION]
> Aşağıdaki örnek, belirtilen kaynak grubunu ve içinde yer alan tüm kaynakları siler.
> Bu makalenin kapsamı dışındaki kaynaklar belirtilen kaynak grubunda mevcutsa, bunlar da silinir.

```azurepowershell-interactive
Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Az. Portal PowerShell modülünde bulunan cmdlet 'ler hakkında daha fazla bilgi için bkz.:

> [!div class="nextstepaction"]
> [Microsoft Azure PowerShell: Portal panosu cmdlet 'leri](https://docs.microsoft.com/powershell/module/Az.Portal/)
