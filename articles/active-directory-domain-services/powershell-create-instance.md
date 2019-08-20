---
title: PowerShell 'i kullanarak Azure Active Directory Domain Services etkinleştirme | Microsoft Docs
description: PowerShell kullanarak Azure Active Directory Domain Services etkinleştirme
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: d4bc5583-6537-4cd9-bc4b-7712fdd9272a
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: iainfou
ms.openlocfilehash: c6572ab8bc2a10039f327233f983c2e822fba3b0
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69617226"
---
# <a name="enable-azure-active-directory-domain-services-using-powershell"></a>PowerShell kullanarak Azure Active Directory Domain Services etkinleştirme
Bu makalede, PowerShell kullanarak Azure Active Directory (AD) etki alanı hizmetlerinin nasıl etkinleştirileceği gösterilmektedir.

[!INCLUDE [updated-for-az.md](../../includes/updated-for-az.md)]

## <a name="task-1-install-the-required-powershell-modules"></a>1\. Görev: Gerekli PowerShell modüllerini yükler

### <a name="install-and-configure-azure-ad-powershell"></a>Azure AD PowerShell 'i yükleyip yapılandırma
[Azure AD PowerShell modülünü yüklemek ve Azure AD 'ye bağlanmak](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?toc=%2fazure%2factive-directory-domain-services%2ftoc.json)için makalesindeki yönergeleri izleyin.

### <a name="install-and-configure-azure-powershell"></a>Azure PowerShell'i yükleyip yapılandırma
[Azure PowerShell modülünü yüklemek ve Azure aboneliğinize bağlanmak](https://docs.microsoft.com/powershell/azure/install-az-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json)için makalesindeki yönergeleri izleyin.


## <a name="task-2-create-the-required-service-principal-in-your-azure-ad-directory"></a>2\. Görev: Azure AD dizininizde gerekli hizmet sorumlusunu oluşturma
Azure AD dizininizde Azure AD Domain Services için gereken hizmet sorumlusunu oluşturmak için aşağıdaki PowerShell komutunu yazın.
```powershell
# Create the service principal for Azure AD Domain Services.
New-AzureADServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
```

## <a name="task-3-create-and-configure-the-aad-dc-administrators-group"></a>3\. Görev: ' AAD DC Administrators ' grubunu oluşturun ve yapılandırın
Sonraki görev, yönetilen etki alanında yönetim görevlerinin temsilciliğini oluşturmak için kullanılacak yönetici grubunu oluşturmaktır.
```powershell
# Create the delegated administration group for AAD Domain Services.
New-AzureADGroup -DisplayName "AAD DC Administrators" `
  -Description "Delegated group to administer Azure AD Domain Services" `
  -SecurityEnabled $true -MailEnabled $false `
  -MailNickName "AADDCAdministrators"
```

Grubu oluşturduktan sonra gruba birkaç kullanıcı ekleyin.
```powershell
# First, retrieve the object ID of the newly created 'AAD DC Administrators' group.
$GroupObjectId = Get-AzureADGroup `
  -Filter "DisplayName eq 'AAD DC Administrators'" | `
  Select-Object ObjectId

# Now, retrieve the object ID of the user you'd like to add to the group.
$UserObjectId = Get-AzureADUser `
  -Filter "UserPrincipalName eq 'admin@contoso.onmicrosoft.com'" | `
  Select-Object ObjectId

# Add the user to the 'AAD DC Administrators' group.
Add-AzureADGroupMember -ObjectId $GroupObjectId.ObjectId -RefObjectId $UserObjectId.ObjectId
```

## <a name="task-4-register-the-azure-ad-domain-services-resource-provider"></a>4\. Görev: Azure AD Domain Services kaynak sağlayıcısını kaydetme
Azure AD Domain Services için kaynak sağlayıcısını kaydetmek için aşağıdaki PowerShell komutunu yazın:
```powershell
# Register the resource provider for Azure AD Domain Services with Resource Manager.
Register-AzResourceProvider -ProviderNamespace Microsoft.AAD
```

## <a name="task-5-create-a-resource-group"></a>5\. Görev: Kaynak grubu oluşturma
Bir kaynak grubu oluşturmak için aşağıdaki PowerShell komutunu yazın:
```powershell
$ResourceGroupName = "ContosoAaddsRg"
$AzureLocation = "westus"

# Create the resource group.
New-AzResourceGroup `
  -Name $ResourceGroupName `
  -Location $AzureLocation
```

Sanal ağı ve Azure AD Domain Services yönetilen etki alanını bu kaynak grubunda oluşturabilirsiniz.


## <a name="task-6-create-and-configure-the-virtual-network"></a>Görev 6: Sanal ağ oluşturma ve yapılandırma
Şimdi Azure AD Domain Services etkinleştirdiğiniz sanal ağı oluşturun. Azure AD Domain Services için ayrılmış alt ağ oluşturduğunuzdan emin olun. Bu ayrılmış alt ağa iş yükü VM 'Leri dağıtmayın.

Azure AD Domain Services için ayrılmış alt ağa sahip bir sanal ağ oluşturmak için aşağıdaki PowerShell komutlarını yazın.

```powershell
$ResourceGroupName = "ContosoAaddsRg"
$VnetName = "DomainServicesVNet_WUS"

# Create the dedicated subnet for AAD Domain Services.
$AaddsSubnet = New-AzVirtualNetworkSubnetConfig `
  -Name DomainServices `
  -AddressPrefix 10.0.0.0/24

$WorkloadSubnet = New-AzVirtualNetworkSubnetConfig `
  -Name Workloads `
  -AddressPrefix 10.0.1.0/24

# Create the virtual network in which you will enable Azure AD Domain Services.
$Vnet=New-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location westus `
  -Name $VnetName `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $AaddsSubnet,$WorkloadSubnet
```


## <a name="task-7-provision-the-azure-ad-domain-services-managed-domain"></a>Görev 7: Azure AD Domain Services yönetilen etki alanını sağlama
Dizininiz için Azure AD Domain Services etkinleştirmek üzere aşağıdaki PowerShell komutunu yazın:

```powershell
$AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
$ManagedDomainName = "contoso.com"
$ResourceGroupName = "ContosoAaddsRg"
$VnetName = "DomainServicesVNet_WUS"
$AzureLocation = "westus"

# Enable Azure AD Domain Services for the directory.
New-AzResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
  -Location $AzureLocation `
  -Properties @{"DomainName"=$ManagedDomainName; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
  -ApiVersion 2017-06-01 -Force -Verbose
```

> [!WARNING]
> **Yönetilen etki alanınızı sağlamaktan sonra ek yapılandırma adımlarını unutmayın.**
> Yönetilen etki alanınız sağlandıktan sonra yine de aşağıdaki görevleri gerçekleştirmeniz gerekir:
> * Sanal makinelerin, etki alanına katılması veya kimlik doğrulaması için yönetilen etki alanını bulabileceği şekilde sanal ağ için DNS ayarlarını güncelleştirin. DNS 'yi yapılandırmak için portalda Azure AD DS yönetilen etki alanınızı seçin. **Genel bakış** penceresinde, bu DNS ayarlarını otomatik olarak yapılandırmanız istenir.
> * Yönetilen etki alanı için gelen trafiği kısıtlamak üzere gerekli ağ güvenlik grubu kurallarını oluşturun. Ağ güvenlik grubu kuralları oluşturmak için portalda Azure AD DS yönetilen etki alanınızı seçin. **Genel bakış** penceresinde, uygun ağ güvenlik grubu kurallarını otomatik olarak oluşturmanız istenir.
> * **[Azure AD Domain Services için parola eşitlemesini etkinleştirin](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds)** , böylece son kullanıcılar şirket kimlik bilgilerini kullanarak yönetilen etki alanında oturum açabilirler.

## <a name="powershell-script"></a>PowerShell betiği
Bu makalede listelenen tüm görevleri gerçekleştirmek için kullanılan PowerShell betiği aşağıda verilmiştir. Betiği kopyalayın ve '. ps1 ' uzantılı bir dosyaya kaydedin. Betiği PowerShell 'de veya PowerShell Tümleşik komut dosyası ortamı (ıSE) kullanarak yürütün.

> [!NOTE]
> **Bu betiği çalıştırmak için gereken izinler** Azure AD Domain Services etkinleştirmek için Azure AD dizini için genel yönetici olmanız gerekir. Ayrıca, Azure AD Domain Services etkinleştiren sanal ağ üzerinde en az "katkıda bulunan" ayrıcalıklarınız olması gerekir.
>

```powershell
# Change the following values to match your deployment.
$AaddsAdminUserUpn = "admin@contoso.onmicrosoft.com"
$AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
$ManagedDomainName = "contoso.com"
$ResourceGroupName = "ContosoAaddsRg"
$VnetName = "DomainServicesVNet_WUS"
$AzureLocation = "westus"

# Connect to your Azure AD directory.
Connect-AzureAD

# Login to your Azure subscription.
Connect-AzAccount

# Create the service principal for Azure AD Domain Services.
New-AzureADServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"

# Create the delegated administration group for AAD Domain Services.
New-AzureADGroup -DisplayName "AAD DC Administrators" `
  -Description "Delegated group to administer Azure AD Domain Services" `
  -SecurityEnabled $true -MailEnabled $false `
  -MailNickName "AADDCAdministrators"

# First, retrieve the object ID of the newly created 'AAD DC Administrators' group.
$GroupObjectId = Get-AzureADGroup `
  -Filter "DisplayName eq 'AAD DC Administrators'" | `
  Select-Object ObjectId

# Now, retrieve the object ID of the user you'd like to add to the group.
$UserObjectId = Get-AzureADUser `
  -Filter "UserPrincipalName eq '$AaddsAdminUserUpn'" | `
  Select-Object ObjectId

# Add the user to the 'AAD DC Administrators' group.
Add-AzureADGroupMember -ObjectId $GroupObjectId.ObjectId -RefObjectId $UserObjectId.ObjectId

# Register the resource provider for Azure AD Domain Services with Resource Manager.
Register-AzResourceProvider -ProviderNamespace Microsoft.AAD

# Create the resource group.
New-AzResourceGroup `
  -Name $ResourceGroupName `
  -Location $AzureLocation

# Create the dedicated subnet for AAD Domain Services.
$AaddsSubnet = New-AzVirtualNetworkSubnetConfig `
  -Name DomainServices `
  -AddressPrefix 10.0.0.0/24

$WorkloadSubnet = New-AzVirtualNetworkSubnetConfig `
  -Name Workloads `
  -AddressPrefix 10.0.1.0/24

# Create the virtual network in which you will enable Azure AD Domain Services.
$Vnet=New-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location $AzureLocation `
  -Name $VnetName `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $AaddsSubnet,$WorkloadSubnet

# Enable Azure AD Domain Services for the directory.
New-AzResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
  -Location $AzureLocation `
  -Properties @{"DomainName"=$ManagedDomainName; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
  -ApiVersion 2017-06-01 -Force -Verbose
```

> [!WARNING]
> **Yönetilen etki alanınızı sağlamaktan sonra ek yapılandırma adımlarını unutmayın.**
> Yönetilen etki alanınız sağlandıktan sonra yine de aşağıdaki görevleri gerçekleştirmeniz gerekir:
> * Sanal makinelerin, etki alanına katılması veya kimlik doğrulaması için yönetilen etki alanını bulabileceği şekilde sanal ağ için DNS ayarlarını güncelleştirin. DNS 'yi yapılandırmak için portalda Azure AD DS yönetilen etki alanınızı seçin. **Genel bakış** penceresinde, bu DNS ayarlarını otomatik olarak yapılandırmanız istenir.
> * Yönetilen etki alanı için gelen trafiği kısıtlamak üzere gerekli ağ güvenlik grubu kurallarını oluşturun. Ağ güvenlik grubu kuralları oluşturmak için portalda Azure AD DS yönetilen etki alanınızı seçin. **Genel bakış** penceresinde, uygun ağ güvenlik grubu kurallarını otomatik olarak oluşturmanız istenir.
> * **[Azure AD Domain Services için parola eşitlemesini etkinleştirin](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds)** , böylece son kullanıcılar şirket kimlik bilgilerini kullanarak yönetilen etki alanında oturum açabilirler.

## <a name="next-steps"></a>Sonraki adımlar
Yönetilen etki alanınız oluşturulduktan sonra, yönetilen etki alanını kullanmak için aşağıdaki yapılandırma görevlerini gerçekleştirin:

* [Sanal ağın DNS sunucusu ayarlarını, yönetilen etki alanınızı işaret etmek üzere güncelleştirin](tutorial-create-instance.md#update-dns-settings-for-the-azure-virtual-network)
* [Yönetilen etki alanınız için parola eşitlemeyi etkinleştirme](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds)
