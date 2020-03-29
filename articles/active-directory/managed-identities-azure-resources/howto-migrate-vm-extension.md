---
title: Yönetilen kimlik VM uzantısını kullanmayı durdurma - Azure AD
description: VM uzantısını kullanmayı durdurmak ve kimlik doğrulaması için Azure Örneği Meta Veri Hizmeti'ni (IMDS) kullanmaya başlamak için adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/25/2018
ms.author: markvi
ms.openlocfilehash: 01b8e1dbc290bed86ccfc3c7016e8bd9168e427a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80049058"
---
# <a name="how-to-stop-using-the-virtual-machine-managed-identities-extension-and-start-using-the-azure-instance-metadata-service"></a>Sanal makine yönetilen kimlik uzantısını kullanmayı bırakma ve Azure Örneği Meta veri Hizmetini kullanmaya başlama

## <a name="virtual-machine-extension-for-managed-identities"></a>Yönetilen kimlikler için sanal makine uzantısı

Yönetilen kimlikler için sanal makine uzantısı, sanal makine içinde yönetilen bir kimlik için belirteçleri istemek için kullanılır. İş akışı aşağıdaki adımlardan oluşur:

1. İlk olarak, kaynak içindeki iş yükü, bir erişim belirteci istemek için yerel bitiş noktasını `http://localhost/oauth2/token` çağırır.
2. Sanal makine uzantısı daha sonra, Azure AD'den bir erişim belirteci istemek için yönetilen kimlik bilgilerini kullanır. 
3. Erişim belirteci arayana döndürülür ve Azure Anahtar Vault veya Azure Depolama gibi Azure AD kimlik doğrulamasını destekleyen hizmetlerde kimlik doğrulaması yapmak için kullanılabilir.

Bir sonraki bölümde özetlenen çeşitli sınırlamalar nedeniyle, yönetilen kimlik VM uzantısı Azure Örneği Meta veri hizmetinde (IMDS) eşdeğer bitiş noktasının kullanılması lehine amortismana konulmuştur

### <a name="provision-the-extension"></a>Uzatmayı sağlama 

Sanal bir makineyi veya sanal makine ölçeğikümesini yönetilen bir kimliğe sahip olacak şekilde yapılandırdığınızda, isteğe bağlı olarak `-Type` [Set-AzVMExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension) cmdlet'teki parametreyi kullanarak Azure kaynakları VM uzantısı için yönetilen kimlikleri sağlamayı seçebilirsiniz. Sanal makinenin `ManagedIdentityExtensionForWindows` `ManagedIdentityExtensionForLinux`türüne bağlı olarak geçiş yapabilir ve parametreyi `-Name` kullanarak adını verebilirsiniz. Parametre belirteç `-Settings` edinimi için OAuth belirteç bitiş noktası tarafından kullanılan bağlantı noktasını belirtir:

```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
```

Şablona `resources` aşağıdaki JSON'u ekleyerek VM uzantısını sağlamak için Azure Kaynak Yöneticisi dağıtım şablonu'nu da kullanabilirsiniz (Linux sürümü için ad ve tür öğelerini kullanın). `ManagedIdentityExtensionForLinux`

    ```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
        "apiVersion": "2018-06-01",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
        ],
        "properties": {
            "publisher": "Microsoft.ManagedIdentity",
            "type": "ManagedIdentityExtensionForWindows",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "port": 50342
            }
        }
    }
    ```
    
    
Sanal makine ölçeği kümeleriyle çalışıyorsanız, [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) cmdlet'ini kullanarak Azure kaynakları nın yönetilen kimliklerini sanal makine ölçeği kümesi uzantısı olarak da sağlayabilirsiniz. Sanal makine `ManagedIdentityExtensionForWindows` ölçeği `ManagedIdentityExtensionForLinux`kümesinin türüne bağlı olarak geçiş yapabilir ve `-Name` parametreyi kullanarak adlandırabilirsiniz. Parametre belirteç `-Settings` edinimi için OAuth belirteç bitiş noktası tarafından kullanılan bağlantı noktasını belirtir:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzVmss
   Add-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```
Azure Kaynak Yöneticisi dağıtım şablonuyla sanal makine ölçeği kümesi uzantısını `extensionpProfile` sağlamak için şablona `ManagedIdentityExtensionForLinux` aşağıdaki JSON'u ekleyin (Linux sürümü için ad ve tür öğelerini kullanın).

    ```json
    "extensionProfile": {
        "extensions": [
            {
                "name": "ManagedIdentityWindowsExtension",
                "properties": {
                    "publisher": "Microsoft.ManagedIdentity",
                    "type": "ManagedIdentityExtensionForWindows",
                    "typeHandlerVersion": "1.0",
                    "autoUpgradeMinorVersion": true,
                    "settings": {
                        "port": 50342
                    },
                    "protectedSettings": {}
                }
            }
    ```

Sanal makine uzantısının sağlanması, DNS arama hataları nedeniyle başarısız olabilir. Bu durumda, sanal makineyi yeniden başlatın ve yeniden deneyin. 

### <a name="remove-the-extension"></a>Uzantıyı kaldırma 
[Az vm uzantısı silme](https://docs.microsoft.com/cli/azure/vm/) `-n ManagedIdentityExtensionForWindows` veya `-n ManagedIdentityExtensionForLinux` Az vmss uzantısı silme ile uzantıyı kaldırmak için (sanal makine türüne bağlı olarak) `Remove-AzVMExtension` veya Az [vmss uzantısı silme,](https://docs.microsoft.com/cli/azure/vmss) Azure CLI kullanarak sanal makine ölçek kümeleri için veya Powershell için:

```azurecli-interactive
az vm identity --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
```

```azurecli-interactive
az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
```

```powershell
Remove-AzVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
```

### <a name="acquire-a-token-using-the-virtual-machine-extension"></a>Sanal makine uzantısını kullanarak bir belirteç edinme

Aşağıda, Azure kaynakları VM Uzantı Bitiş Noktası için yönetilen kimlikleri kullanan örnek bir istek vereme:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Öğe | Açıklama |
| ------- | ----------- |
| `GET` | Son noktadan veri almak istediğinizi belirten HTTP fiili. Bu durumda, bir OAuth erişim belirteci. | 
| `http://localhost:50342/oauth2/token` | 50342 varsayılan bağlantı noktası olan ve yapılandırılabilir olan Azure kaynakları bitiş noktası için yönetilen kimlikler. |
| `resource` | Hedef kaynağın App ID URI'sini gösteren bir sorgu dize parametresi. Ayrıca verilen belirteci `aud` (hedef kitle) iddiasıgörünür. Bu örnek, Bir Uygulama Kimliği URI'si olan Azure `https://management.azure.com/`Kaynak Yöneticisi'ne erişmek için bir belirteç istiyor. |
| `Metadata` | Sunucu Tarafı İstek Sahteciliği (SSRF) saldırısına karşı bir azaltma olarak Azure kaynakları için yönetilen kimlikler tarafından gerekli bir HTTP istek üstbilgi alanı. Bu değer, tüm küçük harfle "true" olarak ayarlanmalıdır.|
| `object_id` | (İsteğe bağlı) Belirteci istediğiniz yönetilen kimliğin object_id gösteren bir sorgu dize parametresi. VM'nizde birden çok kullanıcı tarafından atanmış yönetilen kimlik varsa gereklidir.|
| `client_id` | (İsteğe bağlı) Bir sorgu dize parametresi, için belirteç istediğiniz yönetilen kimliğin client_id gösteren. VM'nizde birden çok kullanıcı tarafından atanmış yönetilen kimlik varsa gereklidir.|


Örnek yanıt:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "refresh_token": "",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
}
```

| Öğe | Açıklama |
| ------- | ----------- |
| `access_token` | İstenen erişim jetonu. Güvenli bir REST API'yi ararken, belirteç `Authorization` istek üstbilgi alanına "taşıyıcı" belirteci olarak katıştırılmış ve API'nin arayanın kimliğini doğrulaması sağlar. | 
| `refresh_token` | Azure kaynakları için yönetilen kimlikler tarafından kullanılmaz. |
| `expires_in` | Erişim belirteci nin verildiği tarihten itibaren süresi dolmadan önce geçerli olmaya devam eden saniye sayısı. Verme zamanı belirteç `iat` iddiasında bulunabilir. |
| `expires_on` | Erişim belirteci süresi dolduğunda zaman aşımı. Tarih, "1970-01-01T0:0:0Z UTC" saniye sayısı olarak temsil edilir (belirteç `exp` talebine karşılık gelir). |
| `not_before` | Erişim belirteci etkin olduğunda zaman alanı ve kabul edilebilir. Tarih, "1970-01-01T0:0:0Z UTC" saniye sayısı olarak temsil edilir (belirteç `nbf` talebine karşılık gelir). |
| `resource` | Erişim belirteci için istenen ve isteğin `resource` sorgu dize parametresi ile eşleşen kaynak. |
| `token_type` | "Taşıyıcı" erişim belirteci olan belirteci türü, hangi kaynak bu belirteç taşıyıcısı erişim verebilir anlamına gelir. |


### <a name="troubleshoot-the-virtual-machine-extension"></a>Sanal makine uzantısını giderme 

#### <a name="restart-the-virtual-machine-extension-after-a-failure"></a>Bir hatadan sonra sanal makine uzantısını yeniden başlatın

Windows'da ve Linux'un belirli sürümlerinde, uzantı durursa, aşağıdaki cmdlet el ile yeniden başlatmak için kullanılabilir:

```powershell
Set-AzVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Konumlar: 
- Windows için uzantı adı ve türü:`ManagedIdentityExtensionForWindows`
- Linux için uzantı adı ve türü:`ManagedIdentityExtensionForLinux`

#### <a name="automation-script-fails-when-attempting-schema-export-for-managed-identities-for-azure-resources-extension"></a>Azure kaynakları uzantısı için yönetilen kimlikler için şema dışa aktarma girişiminde "otomasyon komut dosyası" başarısız olur

Sanal bir makinede Azure kaynakları için yönetilen kimlikler etkinleştirildiğinde, sanal makine veya kaynak grubu için "Otomasyon komut dosyası" özelliğini kullanmaya çalışırken aşağıdaki hata gösterilir:

![Azure kaynakları otomasyonu komut dosyası dışa aktarma hatası için yönetilen kimlikler](./media/howto-migrate-vm-extension/automation-script-export-error.png)

Azure kaynakları sanal makine uzantısı için yönetilen kimlikler şu anda şemasını bir kaynak grubu şablonuna dışa aktarma özelliğini desteklemiyor. Sonuç olarak, oluşturulan şablon, kaynaktaki Azure kaynakları için yönetilen kimlikleri etkinleştirmek için yapılandırma parametrelerini göstermez. Bu bölümler, [bir Azure sanal makinesinde Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-template-windows-vm.md)örneklerinde şablonlar kullanılarak el ile eklenebilir.

Şema dışa aktarma işlevi Azure kaynakları sanal makine uzantısı için yönetilen kimlikler için kullanılabilir olduğunda (Ocak 2019'da amortisman için planlanan), [VM uzantıları içeren Kaynak Grupları Dışa Aktarma'da](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions)listelenir.

## <a name="limitations-of-the-virtual-machine-extension"></a>Sanal makine uzantısı sınırlamaları 

Sanal makine uzantısını kullanmanın birkaç önemli sınırlaması vardır. 

 * En ciddi sınırlama, belirteçleri istemek için kullanılan kimlik bilgilerinin sanal makinede depolanmış olmasıdır. Sanal makineyi başarıyla ihlal eden bir saldırgan kimlik bilgilerini boşaltabilir. 
 * Ayrıca, sanal makine uzantısı hala değiştirmek için büyük bir geliştirme maliyeti ile, çeşitli Linux dağıtımları tarafından desteklenmeyen, inşa ve bu dağıtımların her biri üzerinde uzantısı test. Şu anda yalnızca aşağıdaki Linux dağıtımları desteklenir: 
    * CoreOS Kararlı
    * CentOS 7.1 
    * Kırmızı Şapka 7.2 
    * Ubuntu 15.04 
    * Ubuntu 16.04
 * Sanal makine uzantısı da sağlanması gerekir gibi yönetilen kimliklerle sanal makineleri dağıtmak için bir performans etkisi vardır. 
 * Son olarak, sanal makine uzantısı yalnızca sanal makine başına 32 kullanıcı tarafından atanan yönetilen kimliklerin olmasını destekleyebilir. 

## <a name="azure-instance-metadata-service"></a>Azure Örneği Meta veri Hizmeti

[Azure Örneği Meta Veri Hizmeti (IMDS),](/azure/virtual-machines/windows/instance-metadata-service) sanal makinelerinizi yönetmek ve yapılandırmak için kullanılabilecek sanal makine örneklerini çalıştırma hakkında bilgi sağlayan bir REST bitiş noktasıdır. Bitiş noktası, yalnızca sanal makine nin içinden erişilebilen, iyi bilinen, routable olmayan BIR IP adresinde`169.254.169.254`kullanılabilir.

Belirteçleri istemek için Azure IMDS'yi kullanmanın çeşitli avantajları vardır. 

1. Hizmet sanal makinenin dışında olduğundan, yönetilen kimlikler tarafından kullanılan kimlik bilgileri artık sanal makinede bulunmaz. Bunun yerine, Azure sanal makinesinin ana bilgisayar makinesinde barındırılır ve güvenli hale sunulur.   
2. Azure IaaS'da desteklenen tüm Windows ve Linux işletim sistemleri yönetilen kimlikleri kullanabilir.
3. VM uzantısı artık sağlanması gerekmediğinden dağıtım daha hızlı ve daha kolaydır.
4. IMDS bitiş noktası ile, tek bir sanal makineye en fazla 1000 kullanıcı tarafından atanan yönetilen kimlik ler atanabilir.
5. Sanal makine uzantısını kullananların aksine IMDS kullanan isteklerde önemli bir değişiklik yoktur, bu nedenle şu anda sanal makine uzantısını kullanan varolan dağıtımlar üzerinde bağlantı noktası olması oldukça basittir.

Bu nedenlerden dolayı, sanal makine uzantısı devre dışı kaldıktan sonra Azure IMDS hizmeti belirteçleri istemenin fiili yolu olacaktır. 


## <a name="next-steps"></a>Sonraki Adımlar

* [Bir erişim jetonu edinmek için Azure sanal makinesinde Azure kaynakları için yönetilen kimlikler nasıl kullanılır?](how-to-use-vm-token.md)
* [Azure Örneği Meta veri Hizmeti](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)
