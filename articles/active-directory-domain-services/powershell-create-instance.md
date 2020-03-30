---
title: PowerShell kullanarak Azure DS Etki Alanı Hizmetlerini etkinleştirin | Microsoft Dokümanlar
description: Azure AD PowerShell ve Azure PowerShell'i kullanarak Azure Active Directory Domain Services'ı nasıl yapılandırabileceğinizi ve etkinleştirmeyi öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: d4bc5583-6537-4cd9-bc4b-7712fdd9272a
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: iainfou
ms.openlocfilehash: ee85002aea962dfa675ac6c09a6bfbaeba8e9e79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77613224"
---
# <a name="enable-azure-active-directory-domain-services-using-powershell"></a>PowerShell'i kullanarak Azure Active Directory Etki Alanı Hizmetlerini etkinleştirme

Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS), Windows Server Active Directory ile tam uyumlu etki alanı birleştirme, grup ilkesi, LDAP, Kerberos/NTLM kimlik doğrulaması gibi yönetilen etki alanı hizmetleri sağlar. Etki alanı denetleyicilerini dağıtmadan, yönetmeden ve düzeltmeden bu etki alanı hizmetlerini tüketirsiniz. Azure AD DS, mevcut Azure AD kiracınızla tümleşir. Bu tümleştirme, kullanıcıların şirket kimlik bilgilerini kullanarak oturum açmalarına olanak tanır ve kaynaklara erişimi güvence altına almak için varolan grupları ve kullanıcı hesaplarını kullanabilirsiniz.

Bu makalede, PowerShell kullanarak Azure AD DS'yi nasıl etkinleştirdiğinizi gösterilmektedir.

[!INCLUDE [updated-for-az.md](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için aşağıdaki kaynaklara ihtiyacınız var:

* Azure PowerShell'i yükleyip yapılandırın.
    * Gerekirse, [Azure PowerShell modülünü yüklemek ve Azure aboneliğinize bağlanmak için](/powershell/azure/install-az-ps)yönergeleri izleyin.
    * [Connect-AzAccount][Connect-AzAccount] cmdlet'i kullanarak Azure aboneliğinizde oturum açtığınızdan emin olun.
* Azure AD PowerShell'i yükleyin ve yapılandırın.
    * Gerekirse, Azure AD [PowerShell modülünü yüklemek ve Azure AD'ye bağlanmak için](/powershell/azure/active-directory/install-adv2)yönergeleri izleyin.
    * [Connect-AzureAD][Connect-AzureAD] cmdlet'ini kullanarak Azure AD kiracınızda oturum açtığınızdan emin olun.
* Azure AD DS'yi etkinleştirmek için Azure AD kiracınızda *genel yönetici* ayrıcalıklarına ihtiyacınız vardır.
* Gerekli Azure AD DS kaynaklarını oluşturmak için Azure aboneliğinizde *Katılımcı* ayrıcalıklarına ihtiyacınız vardır.

## <a name="create-required-azure-ad-resources"></a>Gerekli Azure AD kaynaklarını oluşturma

Azure AD DS bir hizmet sorumlusu ve bir Azure REKLAM grubu gerektirir. Bu kaynaklar, Azure AD DS yönetilen etki alanının verileri eşitlemasına ve yönetilen etki alanında hangi kullanıcıların yönetim izinlerine sahip olduğunu tanımlamasına izin verir.

İlk olarak, iletişim kurmak ve kimliğini doğrulamak için Azure AD DS için bir Azure REKLAM hizmeti ilkesi oluşturun. Özel bir uygulama kimliği *2565bd9d-da50-47d4-8b85-4c97f669dc36*kimliği ile *Etki Alanı Denetleyici Hizmetleri* olarak adlandırılır. Bu uygulama kimliğini değiştirmeyin.

[Yeni AzureADServicePrincipal][New-AzureADServicePrincipal] cmdlet'ini kullanarak bir Azure AD hizmet ilkesi oluşturun:

```powershell
New-AzureADServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
```

Şimdi *AAD DC Yöneticileri*adında bir Azure AD grubu oluşturun. Bu gruba eklenen kullanıcılara Azure AD DS yönetilen etki alanında yönetim görevlerini gerçekleştirme izni verilir.

[Yeni AzureADGroup][New-AzureADGroup] cmdlet'ini kullanarak *AAD DC Yöneticileri* grubunu oluşturun:

```powershell
New-AzureADGroup -DisplayName "AAD DC Administrators" `
  -Description "Delegated group to administer Azure AD Domain Services" `
  -SecurityEnabled $true -MailEnabled $false `
  -MailNickName "AADDCAdministrators"
```

Oluşturulan *AAD DC Yöneticileri* grubuyla, [Ekle-AzureADGroupMember][Add-AzureADGroupMember] cmdlet'ini kullanarak gruba bir kullanıcı ekleyin. *Önce AAD DC Yöneticileri* grup nesne kimliğini Get-AzureADGroup cmdlet'ini, ardından [Get-AzureADUser][Get-AzureADGroup] cmdlet'ini kullanarak istediğiniz kullanıcının nesne kimliğini alırsınız. [Get-AzureADUser][Get-AzureADUser]

Aşağıdaki örnekte, bir UPN ile hesabın kullanıcı `admin@aaddscontoso.onmicrosoft.com`nesne kimliği . Bu kullanıcı hesabını *AAD DC Yöneticileri* grubuna eklemek istediğiniz kullanıcının UPN'iyle değiştirin:

```powershell
# First, retrieve the object ID of the newly created 'AAD DC Administrators' group.
$GroupObjectId = Get-AzureADGroup `
  -Filter "DisplayName eq 'AAD DC Administrators'" | `
  Select-Object ObjectId

# Now, retrieve the object ID of the user you'd like to add to the group.
$UserObjectId = Get-AzureADUser `
  -Filter "UserPrincipalName eq 'admin@aaddscontoso.onmicrosoft.com'" | `
  Select-Object ObjectId

# Add the user to the 'AAD DC Administrators' group.
Add-AzureADGroupMember -ObjectId $GroupObjectId.ObjectId -RefObjectId $UserObjectId.ObjectId
```

## <a name="create-supporting-azure-resources"></a>Destekleyici Azure kaynakları oluşturma

İlk olarak, [Register-AzResourceProvider][Register-AzResourceProvider] cmdlet'ini kullanarak Azure AD Etki Alanı Hizmetleri kaynak sağlayıcısına kaydolun:

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.AAD
```

Ardından, [Yeni Kaynak Grubu][New-AzResourceGroup] cmdlet'ini kullanarak bir kaynak grubu oluşturun. Aşağıdaki örnekte, kaynak grubu *myResourceGroup* olarak adlandırılır ve *westus* bölgesinde oluşturulur. Kendi adınızı ve istediğiniz bölgeyi kullanın:

```powershell
$ResourceGroupName = "myResourceGroup"
$AzureLocation = "westus"

# Create the resource group.
New-AzResourceGroup `
  -Name $ResourceGroupName `
  -Location $AzureLocation
```

Azure AD Etki Alanı Hizmetleri için sanal ağ ve alt ağlar oluşturun. Biri *DomainServices,* diğeri *İş Yükleri*için olmak üzere iki alt ağ oluşturulur. Azure AD DS, özel *DomainServices* alt ağına dağıtılır. Bu alt ağa diğer uygulamaları veya iş yüklerini dağıtmayın. Diğer *VM'leriniz* için ayrı İş Yük lerini veya diğer alt ağları kullanın.

[Yeni-AzVirtualNetworkSubnetConfig][New-AzVirtualNetworkSubnetConfig] cmdlet kullanarak alt ağları oluşturun, sonra [New-AzVirtualNetwork][New-AzVirtualNetwork] cmdlet kullanarak sanal ağ oluşturun.

```powershell
$VnetName = "myVnet"

# Create the dedicated subnet for AAD Domain Services.
$AaddsSubnet = New-AzVirtualNetworkSubnetConfig `
  -Name DomainServices `
  -AddressPrefix 10.0.0.0/24

$WorkloadSubnet = New-AzVirtualNetworkSubnetConfig `
  -Name Workloads `
  -AddressPrefix 10.0.1.0/24

# Create the virtual network in which you will enable Azure AD Domain Services.
$Vnet= New-AzVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location westus `
  -Name $VnetName `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $AaddsSubnet,$WorkloadSubnet
```

## <a name="create-an-azure-ad-ds-managed-domain"></a>Azure AD DS yönetilen etki alanı oluşturma

Şimdi Azure AD DS yönetilen bir etki alanı oluşturalım. Azure abonelik kimliğinizi ayarlayın ve ardından yönetilen etki alanı için *aaddscontoso.com*gibi bir ad sağlayın. Abonelik kimliğinizi [Get-AzSubscription][Get-AzSubscription] cmdlet'i kullanarak alabilirsiniz.

Kullanılabilirlik Bölgelerini destekleyen bir bölge seçerseniz, Azure AD DS kaynakları ek fazlalık için bölgeler e göre dağıtılır.

Kullanılabilirlik Alanları, Azure bölgesi içinde fiziksel olarak benzersiz konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Esnekliği sağlamak için, tüm etkin bölgelerde en az üç ayrı bölge vardır.

Azure AD DS'nin bölgelere dağıtılması için yapılandırabileceğiniz hiçbir şey yoktur. Azure platformu, kaynakların bölge dağıtımını otomatik olarak işler. Daha fazla bilgi ve bölgenin kullanılabilirliğini görmek için [Azure'daki Kullanılabilirlik Bölgeleri nelerdir?][availability-zones]

```powershell
$AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
$ManagedDomainName = "aaddscontoso.com"

# Enable Azure AD Domain Services for the directory.
New-AzResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
  -Location $AzureLocation `
  -Properties @{"DomainName"=$ManagedDomainName; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
  -Force -Verbose
```

Kaynağı oluşturmak ve denetimi PowerShell istemine döndürmek birkaç dakika sürer. Azure AD DS yönetilen etki alanı arka planda sağlanmaya devam eder ve dağıtımı tamamlamak bir saat kadar sürebilir. Azure portalında, Azure AD DS yönetilen etki alanınız için **Genel Bakış** sayfası, dağıtım aşaması boyunca geçerli durumu gösterir.

Azure portalı Azure AD DS yönetilen etki alanının sağlanmasını tamamladığını gösterdiğinde, aşağıdaki görevlerin tamamlanması gerekir:

* Sanal makinelerin etki alanı birleştirme veya kimlik doğrulama için yönetilen etki alanını bulabilmesi için sanal ağ için DNS ayarlarını güncelleştirin.
    * DNS'yi yapılandırmak için portaldaki Azure AD DS yönetilen etki alanınızı seçin. Genel **Bakış** penceresinde, bu DNS ayarlarını otomatik olarak yapılandırmanız istenir.
* Kullanılabilirlik Bölgelerini destekleyen bir bölgede Bir Azure AD DS yönetilen etki alanı oluşturduysanız, Azure AD DS yönetilen etki alanının sanal ağındaki trafiği kısıtlamak için bir ağ güvenlik grubu oluşturun. Bu kuralların yerine koymasını gerektiren bir Azure standart yük dengeleyicisi oluşturulur. Bu ağ güvenlik grubu Azure AD DS güvenliğini sağlar ve yönetilen etki alanının doğru çalışması için gereklidir.
    * Ağ güvenlik grubu ve gerekli kurallar oluşturmak için portaldaki Azure AD DS yönetilen etki alanınızı seçin. Genel **Bakış** penceresinde, ağ güvenlik grubunu otomatik olarak oluşturmanız ve yapılandırmanız istenir.
* Son kullanıcıların kurumsal kimlik bilgilerini kullanarak yönetilen etki alanında oturum açabilmesi [için Azure AD Etki Alanı Hizmetlerine parola eşitlemesini etkinleştirin.](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds)

## <a name="complete-powershell-script"></a>PowerShell komut dosyasını tamamla

Aşağıdaki tam PowerShell komut dosyası, bu makalede gösterilen tüm görevleri birleştirir. Komut dosyasını kopyalayın ve `.ps1` uzantılı bir dosyaya kaydedin. Komut dosyasını yerel bir PowerShell konsolunda veya [Azure Bulut BulutU Kabuğu'nda][cloud-shell]çalıştırın.

> [!NOTE]
> Azure AD DS'yi etkinleştirmek için Azure AD kiracısı için genel bir yönetici olmalısınız. Ayrıca Azure aboneliğinde en az *Katılımcı* ayrıcalıklarına ihtiyacınız vardır.

```powershell
# Change the following values to match your deployment.
$AaddsAdminUserUpn = "admin@aaddscontoso.onmicrosoft.com"
$ResourceGroupName = "myResourceGroup"
$VnetName = "myVnet"
$AzureLocation = "westus"
$AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
$ManagedDomainName = "aaddscontoso.com"

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
  -Force -Verbose
```

Kaynağı oluşturmak ve denetimi PowerShell istemine döndürmek birkaç dakika sürer. Azure AD DS yönetilen etki alanı arka planda sağlanmaya devam eder ve dağıtımı tamamlamak bir saat kadar sürebilir. Azure portalında, Azure AD DS yönetilen etki alanınız için **Genel Bakış** sayfası, dağıtım aşaması boyunca geçerli durumu gösterir.

Azure portalı Azure AD DS yönetilen etki alanının sağlanmasını tamamladığını gösterdiğinde, aşağıdaki görevlerin tamamlanması gerekir:

* Sanal makinelerin etki alanı birleştirme veya kimlik doğrulama için yönetilen etki alanını bulabilmesi için sanal ağ için DNS ayarlarını güncelleştirin.
    * DNS'yi yapılandırmak için portaldaki Azure AD DS yönetilen etki alanınızı seçin. Genel **Bakış** penceresinde, bu DNS ayarlarını otomatik olarak yapılandırmanız istenir.
* Kullanılabilirlik Bölgelerini destekleyen bir bölgede Bir Azure AD DS yönetilen etki alanı oluşturduysanız, Azure AD DS yönetilen etki alanının sanal ağındaki trafiği kısıtlamak için bir ağ güvenlik grubu oluşturun. Bu kuralların yerine koymasını gerektiren bir Azure standart yük dengeleyicisi oluşturulur. Bu ağ güvenlik grubu Azure AD DS güvenliğini sağlar ve yönetilen etki alanının doğru çalışması için gereklidir.
    * Ağ güvenlik grubu ve gerekli kurallar oluşturmak için portaldaki Azure AD DS yönetilen etki alanınızı seçin. Genel **Bakış** penceresinde, ağ güvenlik grubunu otomatik olarak oluşturmanız ve yapılandırmanız istenir.
* Son kullanıcıların kurumsal kimlik bilgilerini kullanarak yönetilen etki alanında oturum açabilmesi [için Azure AD Etki Alanı Hizmetlerine parola eşitlemesini etkinleştirin.](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds)

## <a name="next-steps"></a>Sonraki adımlar

Azure AD DS yönetilen etki alanını iş başında görmek için, [bir Windows VM'ye katılabilir,][windows-join] [güvenli LDAP'yi yapılandırabilir][tutorial-ldaps]ve [parola karma eşitlemeyi yapılandırabilirsiniz.][tutorial-phs]

<!-- INTERNAL LINKS -->
[windows-join]: join-windows-vm.md
[tutorial-ldaps]: tutorial-configure-ldaps.md
[tutorial-phs]: tutorial-configure-password-hash-sync.md

<!-- EXTERNAL LINKS -->
[Connect-AzAccount]: /powershell/module/Az.Accounts/Connect-AzAccount
[Connect-AzureAD]: /powershell/module/AzureAD/Connect-AzureAD
[New-AzureADServicePrincipal]: /powershell/module/AzureAD/New-AzureADServicePrincipal
[New-AzureADGroup]: /powershell/module/AzureAD/New-AzureADGroup
[Add-AzureADGroupMember]: /powershell/module/AzureAD/Add-AzureADGroupMember
[Get-AzureADGroup]: /powershell/module/AzureAD/Get-AzureADGroup
[Get-AzureADUser]: /powershell/module/AzureAD/Get-AzureADUser
[Register-AzResourceProvider]: /powershell/module/Az.Resources/Register-AzResourceProvider
[New-AzResourceGroup]: /powershell/module/Az.Resources/New-AzResourceGroup
[New-AzVirtualNetworkSubnetConfig]: /powershell/module/Az.Network/New-AzVirtualNetworkSubnetConfig
[New-AzVirtualNetwork]: /powershell/module/Az.Network/New-AzVirtualNetwork
[Get-AzSubscription]: /powershell/module/Az.Accounts/Get-AzSubscription
[cloud-shell]: /azure/cloud-shell/cloud-shell-windows-users
[availability-zones]: ../availability-zones/az-overview.md
