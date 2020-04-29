---
title: Yönetilen kimlik VM uzantısını kullanmayı Durdur-Azure AD
description: VM uzantısını kullanmayı durdurmak ve kimlik doğrulaması için Azure Instance Metadata Service (ıMDS) kullanmaya başlamak için adım adım yönergeler.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80049058"
---
# <a name="how-to-stop-using-the-virtual-machine-managed-identities-extension-and-start-using-the-azure-instance-metadata-service"></a>Sanal makine tarafından yönetilen kimlikler uzantısını kullanmayı durdurma ve Azure Instance Metadata Service kullanmaya başlama

## <a name="virtual-machine-extension-for-managed-identities"></a>Yönetilen kimlikler için sanal makine uzantısı

Yönetilen kimlikler için sanal makine uzantısı, sanal makine içinde yönetilen bir kimlik için belirteç istemek üzere kullanılır. İş akışı aşağıdaki adımlardan oluşur:

1. İlk olarak, kaynak içindeki iş yükü, bir erişim belirteci `http://localhost/oauth2/token` istemek için Yerel uç noktasını çağırır.
2. Daha sonra sanal makine uzantısı, Azure AD 'den bir erişim belirteci istemek için yönetilen kimliğin kimlik bilgilerini kullanır. 
3. Erişim belirteci, çağırana döndürülür ve Azure Key Vault veya Azure depolama gibi Azure AD kimlik doğrulamasını destekleyen hizmetlerde kimlik doğrulaması yapmak için kullanılabilir.

Bir sonraki bölümde özetlenen çeşitli sınırlamalar nedeniyle, yönetilen kimlik VM uzantısı, Azure Instance Metadata Service (ıMDS) içindeki eşdeğer uç noktayı kullanmanın kullanım dışı bırakılmıştır.

### <a name="provision-the-extension"></a>Uzantıyı sağlama 

Bir sanal makineyi veya sanal makine ölçek kümesini yönetilen bir kimliğe sahip olacak şekilde yapılandırdığınızda, isteğe bağlı olarak `-Type` [set-Azvmexgercmdlet](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension) 'teki PARAMETRESINI kullanarak Azure kaynakları VM uzantısı için Yönetilen kimlikler sağlamayı tercih edebilirsiniz. Sanal makine türüne bağlı `ManagedIdentityExtensionForWindows` olarak `ManagedIdentityExtensionForLinux`ya da ' i geçirebilir ve `-Name` parametresini kullanarak adını verebilirsiniz. `-Settings` Parametresi, OAuth belirteci bitiş noktası tarafından belirteç alımı için kullanılan bağlantı noktasını belirtir:

```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
```

Aşağıdaki JSON `resources` 'ı şablona ekleyerek (Linux sürümü için ad ve tür öğeleri için kullanın `ManagedIdentityExtensionForLinux` ), VM uzantısını sağlamak için Azure Resource Manager Dağıtım şablonunu da kullanabilirsiniz.

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
    
    
Sanal makine ölçek kümeleriyle çalışıyorsanız, [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) cmdlet 'Ini kullanarak Azure kaynakları sanal makine ölçek kümesi uzantısı için Yönetilen kimlikler de sağlayabilirsiniz. Sanal makine ölçek kümesinin `ManagedIdentityExtensionForWindows` türüne `ManagedIdentityExtensionForLinux`bağlı olarak ya da ' i geçirebilir ve `-Name` parametresini kullanarak adını verebilirsiniz. `-Settings` Parametresi, OAuth belirteci bitiş noktası tarafından belirteç alımı için kullanılan bağlantı noktasını belirtir:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzVmss
   Add-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```
Sanal makine ölçek kümesi uzantısını Azure Resource Manager dağıtım şablonuyla sağlamak için, aşağıdaki JSON 'ı `extensionpProfile` bölümüne ekleyin (Linux sürümü için ad ve tür öğeleri için kullanın `ManagedIdentityExtensionForLinux` ).

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

Sanal makine uzantısının sağlanması DNS arama hatalarından dolayı başarısız olabilir. Bu durumda, sanal makineyi yeniden başlatın ve yeniden deneyin. 

### <a name="remove-the-extension"></a>Uzantıyı kaldırma 
Uzantıyı kaldırmak için, [az VM Extension Delete](https://docs.microsoft.com/cli/azure/vm/)ile (sanal makine türüne bağlı olarak) veya Azure CLI kullanan sanal makine ölçek kümeleri için [az VMSS Extension Delete](https://docs.microsoft.com/cli/azure/vmss) ' i veya PowerShell `Remove-AzVMExtension` için şunu kullanın: `-n ManagedIdentityExtensionForWindows` `-n ManagedIdentityExtensionForLinux`

```azurecli-interactive
az vm identity --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
```

```azurecli-interactive
az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
```

```powershell
Remove-AzVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
```

### <a name="acquire-a-token-using-the-virtual-machine-extension"></a>Sanal makine uzantısını kullanarak belirteç alma

Aşağıda Azure kaynakları için Yönetilen kimlikler VM uzantı uç noktası kullanılarak örnek bir istek verilmiştir:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Öğe | Açıklama |
| ------- | ----------- |
| `GET` | Uç noktadan veri almak istediğinizi gösteren HTTP fiili. Bu durumda, bir OAuth erişim belirteci. | 
| `http://localhost:50342/oauth2/token` | Azure kaynakları için Yönetilen kimlikler uç noktası, burada 50342 varsayılan bağlantı noktasıdır ve yapılandırılabilir. |
| `resource` | Hedef kaynağın uygulama KIMLIĞI URI 'sini gösteren bir sorgu dizesi parametresi. Ayrıca, verilen belirtecin `aud` (hedef kitle) talebinde de görüntülenir. Bu örnek, uygulama KIMLIĞI URI 'SI olan Azure Resource Manager erişmek için bir belirteç ister `https://management.azure.com/`. |
| `Metadata` | Sunucu tarafı Isteği forgery (SSRF) saldırılarına karşı risk azaltma olarak Azure kaynakları için Yönetilen kimlikler için gereken bir HTTP istek üst bilgisi alanı. Bu değer, tüm küçük durumlarda "true" olarak ayarlanmalıdır.|
| `object_id` | Seçim Belirteci istediğiniz yönetilen kimliğin object_id belirten bir sorgu dizesi parametresi. SANAL makinenizde birden çok kullanıcı tarafından atanan yönetilen kimlik varsa, gereklidir.|
| `client_id` | Seçim Belirteci istediğiniz yönetilen kimliğin client_id belirten bir sorgu dizesi parametresi. SANAL makinenizde birden çok kullanıcı tarafından atanan yönetilen kimlik varsa, gereklidir.|


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
| `access_token` | İstenen erişim belirteci. Güvenli bir REST API çağrılırken, belirteç `Authorization` istek üst bilgisi alanına bir "taşıyıcı" belirteci olarak katıştırılır ve bu da API 'nin çağıranın kimliğini doğrulamasına izin verir. | 
| `refresh_token` | Azure kaynakları için Yönetilen kimlikler tarafından kullanılmıyor. |
| `expires_in` | Erişim belirtecinin, verme zamanından önce geçerli olmaya devam etmesi için geçmesi gereken saniye sayısı. Verme süresi belirtecin `iat` talebinde bulunabilir. |
| `expires_on` | Erişim belirtecinin süresi dolduğu zaman aralığı. Tarih, "1970-01-01T0:0: 0Z UTC" (belirtecin `exp` talebine karşılık gelir) için saniye sayısı olarak gösterilir. |
| `not_before` | Erişim belirteci yürürlüğe girer ve kabul edilebilir. Tarih, "1970-01-01T0:0: 0Z UTC" (belirtecin `nbf` talebine karşılık gelir) için saniye sayısı olarak gösterilir. |
| `resource` | İsteğin `resource` sorgu dizesi parametresiyle eşleşen erişim belirtecinin istendiği kaynak. |
| `token_type` | "Taşıyıcı" erişim belirteci olan belirtecin türü, bu, kaynağın bu belirtecin taşıyıcının erişim izni verebileceği anlamına gelir. |


### <a name="troubleshoot-the-virtual-machine-extension"></a>Sanal makine uzantısının sorunlarını giderme 

#### <a name="restart-the-virtual-machine-extension-after-a-failure"></a>Bir hatadan sonra sanal makine uzantısını yeniden Başlat

Windows ve Linux 'un belirli sürümlerinde, uzantı durdurulduğunda aşağıdaki cmdlet 'i el ile yeniden başlatmak için kullanılabilir:

```powershell
Set-AzVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Konumlar: 
- Windows için uzantı adı ve tür:`ManagedIdentityExtensionForWindows`
- Linux için uzantı adı ve türü:`ManagedIdentityExtensionForLinux`

#### <a name="automation-script-fails-when-attempting-schema-export-for-managed-identities-for-azure-resources-extension"></a>"Otomasyon betiği", Azure kaynakları uzantısı için Yönetilen kimlikler için şema dışarı aktarma denemesi sırasında başarısız oluyor

Bir sanal makinede Azure kaynakları için Yönetilen kimlikler etkinleştirildiğinde, sanal makine veya kaynak grubu için "Otomasyon betiği" özelliğini kullanmaya çalışırken aşağıdaki hata görüntülenir:

![Azure kaynakları için Yönetilen kimlikler Otomasyon betiği dışarı aktarma hatası](./media/howto-migrate-vm-extension/automation-script-export-error.png)

Azure kaynakları için Yönetilen kimlikler sanal makine uzantısı Şu anda, şemasını bir kaynak grubu şablonuna aktarma özelliğini desteklememektedir. Sonuç olarak, oluşturulan şablon kaynaktaki Azure kaynakları için yönetilen kimlikleri etkinleştirmek üzere yapılandırma parametrelerini göstermez. Bu bölümler, [bir Azure sanal makinesinde şablon kullanarak Azure kaynakları için yönetilen kimlikleri yapılandırma](qs-configure-template-windows-vm.md)içindeki örnekleri izleyerek el ile eklenebilir.

Şema dışa aktarma işlevselliği, Azure kaynakları sanal makine uzantısının yönetilen kimlikleri için kullanılabilir hale geldiğinde (2019 Ocak 'ta kullanımdan kaldırma için planlanmıştır), [VM uzantıları Içeren kaynak gruplarını dışarı aktarma](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions)bölümünde listelenecektir.

## <a name="limitations-of-the-virtual-machine-extension"></a>Sanal makine uzantısının sınırlamaları 

Sanal makine uzantısını kullanmanın bazı önemli sınırlamaları vardır. 

 * En ciddi kısıtlama, belirteçleri istemek için kullanılan kimlik bilgilerinin sanal makinede depolanmasıdır. Sanal makineye başarılı bir şekilde ulaşan bir saldırgan, kimlik bilgilerini alabilir. 
 * Ayrıca, bu dağıtımların her birinde uzantıyı değiştirmek, derlemek ve test etmek için büyük bir geliştirme maliyetiyle, sanal makine uzantısı birkaç Linux dağıtımı tarafından yine de desteklenmeye devam etmektedir. Şu anda yalnızca aşağıdaki Linux dağıtımları desteklenir: 
    * CoreOS kararlı
    * CentOS 7,1 
    * Red hat 7,2 
    * Ubuntu 15,04 
    * Ubuntu 16.04
 * Sanal makine uzantısının da sağlanması gerektiği için, yönetilen kimliklerle sanal makineler dağıtmaya yönelik bir performans etkisi vardır. 
 * Son olarak, sanal makine uzantısı yalnızca, sanal makine başına 32 Kullanıcı tarafından atanan yönetilen kimlik olmasını destekleyebilir. 

## <a name="azure-instance-metadata-service"></a>Azure Instance Metadata Service

[Azure Instance Metadata Service (IMDS)](/azure/virtual-machines/windows/instance-metadata-service) , sanal makinelerinizi yönetmek ve yapılandırmak için kullanılabilecek çalışan sanal makine örnekleri hakkında bilgi sağlayan bir REST uç noktasıdır. Uç nokta, yalnızca sanal makine içinden erişilebilen, iyi bilinen yönlendirilemeyen IP adresinde`169.254.169.254`() kullanılabilir.

Belirteçleri istemek için Azure ıMDS kullanmanın çeşitli avantajları vardır. 

1. Bu hizmet sanal makinenin dışında olduğundan, Yönetilen kimlikler tarafından kullanılan kimlik bilgileri artık sanal makinede mevcut değildir. Bunun yerine, Azure sanal makinesinin ana makinesinde barındırılır ve güvenirler.   
2. Azure IaaS 'de desteklenen tüm Windows ve Linux işletim sistemleri, yönetilen kimlikleri kullanabilir.
3. VM uzantısının artık sağlanması gerekmediğinden dağıtım daha hızlı ve daha kolaydır.
4. IMDS uç noktasıyla, en fazla 1000 Kullanıcı tarafından atanan yönetilen kimlik tek bir sanal makineye atanabilir.
5. Sanal makine uzantısını kullananlara karşılık ıMDS kullanan isteklerde önemli bir değişiklik yoktur, bu nedenle sanal makine uzantısını kullanan mevcut dağıtımlar üzerinde bağlantı noktası oldukça basittir.

Bu nedenlerden dolayı, sanal makine uzantısı kullanım dışı olduktan sonra Azure ıMDS hizmeti belirteçleri istemek için erteleme yoludur. 


## <a name="next-steps"></a>Sonraki Adımlar

* [Erişim belirteci almak için bir Azure sanal makinesinde Azure kaynakları için Yönetilen kimlikler kullanma](how-to-use-vm-token.md)
* [Azure Instance Metadata Service](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)
