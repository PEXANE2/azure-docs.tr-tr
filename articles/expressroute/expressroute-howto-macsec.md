---
title: "Azure ExpressRoute: MACsec'i yapılandırın"
description: Bu makale, kenar yönlendiricileriniz ve Microsoft'un kenar yönlendiricileri arasındaki bağlantıları güvence altına almak için MACsec'i yapılandırmanıza yardımcı olur.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: cherylmc
ms.openlocfilehash: 626302845dfb4b19deb921675601818b35ab8edb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74083541"
---
# <a name="configure-macsec-on-expressroute-direct-ports"></a>ExpressRoute Direct bağlantı noktalarında MACsec'i yapılandırın

Bu makale, PowerShell'i kullanarak kenar yönlendiricileriniz ile Microsoft'un kenar yönlendiricileri arasındaki bağlantıları güvence altına alacak şekilde MACsec'i yapılandırmanıza yardımcı olur.

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmayı başlatmadan önce aşağıdakileri onaylayın:

* [ExpressRoute Direct sağlama iş akışlarını](expressroute-erdirect-about.md)anlıyorsunuz.
* Bir [ExpressRoute Direct bağlantı noktası kaynağı](expressroute-howto-erdirect.md)oluşturdunuz.
* PowerShell'i yerel olarak çalıştırmak istiyorsanız, Azure PowerShell'in en son sürümünün bilgisayarınızda yüklü olduğunu doğrulayın.

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

### <a name="sign-in-and-select-the-right-subscription"></a>Oturum açın ve doğru aboneliği seçin

Yapılandırmayı başlatmak için Azure hesabınızda oturum açın ve kullanmak istediğiniz aboneliği seçin.

   [!INCLUDE [sign in](../../includes/expressroute-cloud-shell-connect.md)]

## <a name="1-create-azure-key-vault-macsec-secrets-and-user-identity"></a>1. Azure Key Vault, MACsec sırları ve kullanıcı kimliği oluşturun

1. MACsec sırlarını yeni bir kaynak grubunda depolamak için bir Anahtar Kasa örneği oluşturun.

    ```azurepowershell-interactive
    New-AzResourceGroup -Name "your_resource_group" -Location "resource_location"
    $keyVault = New-AzKeyVault -Name "your_key_vault_name" -ResourceGroupName "your_resource_group" -Location "resource_location" -EnableSoftDelete 
    ```

    Zaten bir anahtar kasanız veya kaynak grubunuz varsa, bunları yeniden kullanabilirsiniz. Ancak, varolan anahtar kasanızdaki [ **yumuşak silme** özelliğini](../key-vault/key-vault-ovw-soft-delete.md) etkinleştirmeniz çok önemlidir. Yumuşak silme etkin değilse, etkinleştirmek için aşağıdaki komutları kullanabilirsiniz:

    ```azurepowershell-interactive
    ($resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName "your_existing_keyvault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"
    Set-AzResource -resourceid $resource.ResourceId -Properties $resource.Properties
    ```
2. Kullanıcı kimliği oluşturun.

    ```azurepowershell-interactive
    $identity = New-AzUserAssignedIdentity  -Name "identity_name" -Location "resource_location" -ResourceGroupName "your_resource_group"
    ```

    New-AzUserAssignedIdentity geçerli bir PowerShell cmdlet olarak tanınmıyorsa, aşağıdaki modülü (Yönetici modunda) yükleyin ve yukarıdaki komutu yeniden çalıştırın.

    ```azurepowershell-interactive
    Install-Module -Name Az.ManagedServiceIdentity
    ```
3. Bir bağlantı ilişkisi anahtarı (CAK) ve bir bağlantı ilişkilendirme anahtar adı (CKN) oluşturun ve bunları anahtar kasasında saklayın.

    ```azurepowershell-interactive
    $CAK = ConvertTo-SecureString "your_key" -AsPlainText -Force
    $CKN = ConvertTo-SecureString "your_key_name" -AsPlainText -Force
    $MACsecCAKSecret = Set-AzKeyVaultSecret -VaultName "your_key_vault_name" -Name "CAK_name" -SecretValue $CAK
    $MACsecCKNSecret = Set-AzKeyVaultSecret -VaultName "your_key_vault_name" -Name "CKN_name" -SecretValue $CKN
    ```
4. Kullanıcı kimliğine GET izni atayın.

    ```azurepowershell-interactive
    Set-AzKeyVaultAccessPolicy -VaultName "your_key_vault_name" -PermissionsToSecrets get -ObjectId $identity.PrincipalId
    ```

   Şimdi bu kimlik, örneğin CAK ve CKN gibi sırları anahtar kasasından alabilir.
5. Bu kullanıcı kimliğini ExpressRoute tarafından kullanılacak şekilde ayarlayın.

    ```azurepowershell-interactive
    $erIdentity = New-AzExpressRoutePortIdentity -UserAssignedIdentityId $identity.Id
    ```
 
## <a name="2-configure-macsec-on-expressroute-direct-ports"></a>2. ExpressRoute Direct bağlantı noktalarında MACsec'i yapılandırın

### <a name="to-enable-macsec"></a>MACsec'i etkinleştirmek için

Her ExpressRoute Direct örneğinin iki fiziksel bağlantı noktası vardır. MaCsec'i her iki bağlantı noktasında da aynı anda etkinleştirmeyi veya aynı anda bir bağlantı noktasında MACsec'i etkinleştirmeyi seçebilirsiniz. ExpressRoute Direct'iniz zaten hizmet veriyorsa, aynı anda bir bağlantı noktası yapmak (diğer bağlantı noktasına hizmet verirken trafiği etkin bir bağlantı noktasına çevirerek) kesintiyi en aza indirmenize yardımcı olabilir.

1. ExpressRoute yönetim kodunun gerekirse MACsec sırlarına erişebilmesi için MACsec sırlarını ayarlayın ve şifreyi belirleyin ve kullanıcı kimliğini bağlantı noktasıyla ilişkilendirin.

    ```azurepowershell-interactive
    $erDirect = Get-AzExpressRoutePort -ResourceGroupName "your_resource_group" -Name "your_direct_port_name"
    $erDirect.Links[0]. MacSecConfig.CknSecretIdentifier = $MacSecCKNSecret.Id
    $erDirect.Links[0]. MacSecConfig.CakSecretIdentifier = $MacSecCAKSecret.Id
    $erDirect.Links[0]. MacSecConfig.Cipher = "GcmAes256"
    $erDirect.Links[1]. MacSecConfig.CknSecretIdentifier = $MacSecCKNSecret.Id
    $erDirect.Links[1]. MacSecConfig.CakSecretIdentifier = $MacSecCAKSecret.Id
    $erDirect.Links[1]. MacSecConfig.Cipher = "GcmAes256"
    $erDirect.identity = $erIdentity
    Set-AzExpressRoutePort -ExpressRoutePort $erDirect
    ```
2. (İsteğe bağlı) Bağlantı noktaları Administrative Down durumundaysa, bağlantı noktalarını açmak için aşağıdaki komutları çalıştırabilirsiniz.

    ```azurepowershell-interactive
    $erDirect = Get-AzExpressRoutePort -ResourceGroupName "your_resource_group" -Name "your_direct_port_name"
    $erDirect.Links[0].AdminState = "Enabled"
    $erDirect.Links[1].AdminState = "Enabled"
    Set-AzExpressRoutePort -ExpressRoutePort $erDirect
    ```

    Bu noktada, MACsec Microsoft tarafındaki ExpressRoute Direct bağlantı noktalarında etkinleştirilir. Kenar aygıtlarınızda yapılandırmadıysanız, bunları aynı MACsec sırları ve şifrelemeyle yapılandırmaya devam edebilirsiniz.

### <a name="to-disable-macsec"></a>MACsec'i devre dışı kalmak için

ExpressRoute Direct örneğinde MACsec artık istenmiyorsa, devre dışı kalmak için aşağıdaki komutları çalıştırabilirsiniz.

```azurepowershell-interactive
$erDirect = Get-AzExpressRoutePort -ResourceGroupName "your_resource_group" -Name "your_direct_port_name"
$erDirect.Links[0]. MacSecConfig.CknSecretIdentifier = $null
$erDirect.Links[0]. MacSecConfig.CakSecretIdentifier = $null
$erDirect.Links[1]. MacSecConfig.CknSecretIdentifier = $null
$erDirect.Links[1]. MacSecConfig.CakSecretIdentifier = $null
$erDirect.identity = $null
Set-AzExpressRoutePort -ExpressRoutePort $erDirect
```

Bu noktada, MACsec Microsoft tarafındaki ExpressRoute Direct bağlantı noktalarında devre dışı bırakılır.

### <a name="test-connectivity"></a>Bağlantıyı test etme
ExpressRoute Direct bağlantı noktalarınızda MACsec 'i (MACsec anahtar güncelleştirmesi dahil) yapılandırdıktan sonra, devrelerin BGP oturumlarının çalışır durumda olup olmadığını [kontrol edin.](expressroute-troubleshooting-expressroute-overview.md) Bağlantı noktalarında henüz herhangi bir devreniz yoksa, lütfen önce bir tane oluşturun ve devrenin Azure Özel Eşleme sini veya Microsoft Peering'i ayarlayın. MACsec, ağ aygıtlarınız ve Microsoft'un ağ aygıtları arasında MACsec anahtar uyuşmazlığı da dahil olmak üzere yanlış yapılandırılmışsa, Katman 2'de ARP çözünürlüğü ve katman 3'te BGP çözümünde görmezsiniz. Her şey doğru şekilde yapılandırılırsa, BGP yollarının her iki yönde de doğru şekilde duyurulduğu ve uygulama verilerinizin ExpressRoute üzerinden buna göre aktığını görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
1. [ExpressRoute Direct'te ExpressRoute devresi oluşturma](expressroute-howto-erdirect.md)
2. [ExpressRoute devresini Azure sanal ağına bağlama](expressroute-howto-linkvnet-arm.md)
3. [ExpressRoute bağlantısını doğrula](expressroute-troubleshooting-expressroute-overview.md)
