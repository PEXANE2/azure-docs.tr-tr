---
title: REST kullanarak Azure VM 'de yönetilen kimlikleri Yapılandırma-Azure AD
description: REST API çağrısı yapmak için KıVRıMLı kullanarak bir Azure VM 'de sistem ve Kullanıcı tarafından atanan Yönetilen kimlikler yapılandırmaya yönelik adım adım yönergeler.
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
ms.date: 06/25/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5d78ddaaae886a33b4d22e8724ade04ab63508f1
ms.sourcegitcommit: a678f00c020f50efa9178392cd0f1ac34a86b767
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74547331"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-rest-api-calls"></a>REST API çağrılarını kullanarak Azure VM 'de Azure kaynakları için Yönetilen kimlikler yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için Yönetilen kimlikler, Azure Active Directory ' de otomatik olarak yönetilen bir sistem kimliğiyle Azure hizmetleri sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager REST uç noktasına çağrı yapmak için KıVRıMLı kullanarak, Azure VM 'de Azure kaynakları işlemleri için aşağıdaki yönetilen kimlikleri nasıl gerçekleştireceğinizi öğreneceksiniz:

- Azure VM 'de sistem tarafından atanan yönetilen kimliği etkinleştirme ve devre dışı bırakma
- Azure VM 'ye Kullanıcı tarafından atanan yönetilen kimlik ekleme ve kaldırma

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için Yönetilen kimlikler hakkında bilginiz varsa [genel bakış bölümüne](overview.md)bakın. **[Sistem tarafından atanan ve Kullanıcı tarafından atanan yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)gözden geçirdiğinizden emin**olun.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Windows kullanıyorsanız, [Linux Için Windows alt sistemini](https://msdn.microsoft.com/commandline/wsl/about) yükledikten sonra Azure Portal [Azure Cloud Shell](../../cloud-shell/overview.md) kullanın.
- [Linux Için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya [Linux dağıtım Işletim SISTEMI](/cli/azure/install-azure-cli-apt?view=azure-cli-latest)kullanıyorsanız [Azure CLI yerel konsolunu yükleyebilirsiniz](/cli/azure/install-azure-cli).
- Azure CLı yerel Konsolu kullanıyorsanız, sistem veya Kullanıcı tarafından atanan yönetilen kimlikleri yönetmek istediğiniz Azure aboneliğiyle ilişkili bir hesapla `az login` kullanarak Azure 'da oturum açın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, Azure Resource Manager REST uç noktasına çağrı yapmak için KıVRıMLı kullanarak bir Azure VM 'de sistem tarafından atanan yönetilen kimliğin nasıl etkinleştirileceğini ve devre dışı bırakılacağını öğreneceksiniz.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Azure VM oluşturma sırasında sistem tarafından atanan yönetilen kimliği etkinleştirme

Sistem tarafından atanan yönetilen kimlik etkin bir Azure VM 'si oluşturmak için hesabınızın [sanal makine katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rolü ataması gerekir.  Ek Azure AD dizin rolü ataması gerekli değildir.

1. VM'nizin ve onunla ilgili kaynakların kapsaması ve dağıtımı için, [az group create](/cli/azure/group/#az-group-create) komutunu kullanarak bir [kaynak grubu](../../azure-resource-manager/resource-group-overview.md#terminology) oluşturun. Bunun yerine kullanmak istediğiniz bir kaynak grubunuz varsa, bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. SANAL ağınız için bir [ağ arabirimi](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) oluşturun:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3. VM 'nizi sistem tarafından atanan bir yönetilen kimlikle oluşturmak için yetkilendirme üstbilgisindeki bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alın.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Azure Resource Manager REST uç noktasını çağırmak için KıVRıMLı kullanarak bir VM oluşturun. Aşağıdaki örnek, `"identity":{"type":"SystemAssigned"}`değere göre istek gövdesinde tanımlandığı şekilde, sistem tarafından atanan yönetilen kimlik ile *Myvm* ADLı bir VM oluşturur. `<ACCESS TOKEN>`, bir taşıyıcı erişim belirteci istediğinizde ve ortamınız için uygun olan `<SUBSCRIPTION ID>` değerini, önceki adımda aldığınız değerle değiştirin.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PUT -d '{"location":"westus","name":"myVM","identity":{"type":"SystemAssigned"},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"myVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"<SECURE PASSWORD STRING>"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   
   **İstek üst bilgileri**
   
   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın.        | 
   
   **İstek gövdesi**

   ```JSON
     {
       "location":"westus",
       "name":"myVM",
       "identity":{
          "type":"SystemAssigned"
       },
       "properties":{
          "hardwareProfile":{
             "vmSize":"Standard_D2_v2"
          },
          "storageProfile":{
             "imageReference":{
                "sku":"2016-Datacenter",
                "publisher":"MicrosoftWindowsServer",
                "version":"latest",
                "offer":"WindowsServer"
             },
             "osDisk":{
                "caching":"ReadWrite",
                "managedDisk":{
                   "storageAccountType":"Standard_LRS"
                },
                "name":"myVM3osdisk",
                "createOption":"FromImage"
             },
             "dataDisks":[
                {
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":0
                },
                {
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":1
                }
             ]
          },
          "osProfile":{
             "adminUsername":"azureuser",
             "computerName":"myVM",
             "adminPassword":"myPassword12"
          },
          "networkProfile":{
             "networkInterfaces":[
                {
                   "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
                   "properties":{
                      "primary":true
                   }
                }
             ]
          }
       }
    }  
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Mevcut bir Azure VM 'de sistem tarafından atanan kimliği etkinleştirme

Başlangıçta sağlanan bir VM 'de sistem tarafından atanan yönetilen kimliği etkinleştirmek için hesabınızın [sanal makine katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rolü ataması gerekir.  Ek Azure AD dizin rolü ataması gerekli değildir.

1. VM 'nizi sistem tarafından atanan bir yönetilen kimlikle oluşturmak için yetkilendirme üstbilgisindeki bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2. SANAL makinenizde sistem tarafından atanan yönetilen kimliği, *myvm*ADLı bir VM için `{"identity":{"type":"SystemAssigned"}` değer ile belirtilen şekilde, istek gövdesinde tanımlandığı şekilde etkinleştirmek üzere Azure Resource Manager REST uç noktasını çağırmak IÇIN aşağıdaki kıvrımlı komutunu kullanın.  `<ACCESS TOKEN>`, bir taşıyıcı erişim belirteci istediğinizde ve ortamınız için uygun olan `<SUBSCRIPTION ID>` değerini, önceki adımda aldığınız değerle değiştirin.
   
   > [!IMPORTANT]
   > VM 'ye atanmış olan, Kullanıcı tarafından atanan yönetilen kimliklerin hiçbirini silmemenizi sağlamak için, bu KıVRıMLı komutunu kullanarak Kullanıcı tarafından atanan yönetilen kimlikleri listeetmeniz gerekir: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Yanıtta `identity` değerinde tanımlandığı şekilde VM 'ye atanan kullanıcı tarafından atanan yönetilen kimlikleriniz varsa, VM 'niz üzerinde sistem tarafından atanan yönetilen kimliği etkinleştirirken Kullanıcı tarafından atanan yönetilen kimliklerin nasıl tutulacağını gösteren 3. adıma atlayın.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın.        | 
   
   **İstek gövdesi**
    
   ```JSON
    {  
       "identity":{  
          "type":"SystemAssigned"
       }
    }
   ```

3. Kullanıcı tarafından atanan yönetilen kimlikleri olan bir VM 'de sistem tarafından atanan yönetilen kimliği etkinleştirmek için, `type` değerine `SystemAssigned` eklemeniz gerekir.  
   
   Örneğin, sanal makinenizde Kullanıcı tarafından atanan Yönetilen kimlikler varsa `ID1` ve buna atanmış `ID2` ve sanal makineye sistem tarafından atanan yönetilen kimlik eklemek istiyorsanız aşağıdaki KıVRıMLı çağrıyı kullanın. `<ACCESS TOKEN>` ve `<SUBSCRIPTION ID>`, ortamınız için uygun değerlerle değiştirin.

   API sürümü `2018-06-01`, Kullanıcı tarafından atanan yönetilen kimlikleri, API `2017-12-01`sürümünde kullanılan bir dizi biçimindeki `identityIds` değerine karşılık gelen `userAssignedIdentities` değerindeki bir sözlük biçiminde depolar.
   
   **APı SÜRÜMÜ 2018-06-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {  
       "identity":{  
          "type":"SystemAssigned, UserAssigned",
          "userAssignedIdentities":{  
             "/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{  
    
             },
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{  
    
             }
          }
       }
    }
   ```

   **APı SÜRÜMÜ 2017-12-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "identityIds":["/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01 HTTP/1.1
   ```
    
   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {  
       "identity":{  
          "type":"SystemAssigned, UserAssigned",
          "identityIds":[  
             "/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1",
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"
          ]
       }
    }
   ```   

### <a name="disable-system-assigned-managed-identity-from-an-azure-vm"></a>Azure VM 'den sistem tarafından atanan yönetilen kimliği devre dışı bırakma

Bir VM 'de sistem tarafından atanan yönetilen kimliği devre dışı bırakmak için hesabınızın [sanal makine katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rolü ataması gerekir.  Ek Azure AD dizin rolü ataması gerekli değildir.

1. VM 'nizi sistem tarafından atanan bir yönetilen kimlikle oluşturmak için yetkilendirme üstbilgisindeki bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Sistem tarafından atanan yönetilen kimliği devre dışı bırakmak için Azure Resource Manager REST uç noktasını çağırmak üzere sanal makineyi KıVRıMLı kullanarak güncelleştirin.  Aşağıdaki örnek, *myvm*ADLı bir VM 'den `{"identity":{"type":"None"}}` değere göre istek gövdesinde tanımlanan sistem tarafından atanan yönetilen kimliği devre dışı bırakır.  `<ACCESS TOKEN>`, bir taşıyıcı erişim belirteci istediğinizde ve ortamınız için uygun olan `<SUBSCRIPTION ID>` değerini, önceki adımda aldığınız değerle değiştirin.

   > [!IMPORTANT]
   > VM 'ye atanmış olan, Kullanıcı tarafından atanan yönetilen kimliklerin hiçbirini silmemenizi sağlamak için, bu KıVRıMLı komutunu kullanarak Kullanıcı tarafından atanan yönetilen kimlikleri listeetmeniz gerekir: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Yanıtta `identity` değerinde tanımlandığı şekilde VM 'ye atanan kullanıcı tarafından atanan yönetilen kimlikleriniz varsa, VM 'niz üzerinde sistem tarafından atanan yönetilen kimliği devre dışı bırakırken, Kullanıcı tarafından atanan yönetilen kimliklerin nasıl tutulacağını gösteren 3. adıma atlayın.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {  
       "identity":{  
          "type":"None"
       }
    }
   ```

   Kullanıcı tarafından atanan yönetilen kimlikleri olan bir sanal makineden sistem tarafından atanan yönetilen kimliği kaldırmak için, **apı 2018-06-01**kullanıyorsanız `UserAssigned` değerini ve `userAssignedIdentities` sözlük değerlerini tutarken `{"identity":{"type:" "}}` değerden `SystemAssigned` kaldırın. **API sürüm 2017-12-01** veya önceki bir sürümünü kullanıyorsanız `identityIds` diziyi saklayın.

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, Azure Resource Manager REST uç noktasına çağrı yapmak için bir Azure VM üzerinde kullanıcı tarafından atanan yönetilen kimliğin nasıl ekleneceğini ve kaldırılacağını öğrenirsiniz.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-an-azure-vm"></a>Azure VM oluşturma sırasında Kullanıcı tarafından atanan yönetilen kimlik atama

Bir VM 'ye Kullanıcı tarafından atanan bir kimlik atamak için hesabınızın [sanal makine katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamalarına ihtiyacı vardır. Ek Azure AD dizin rolü ataması gerekli değildir.

1. VM 'nizi sistem tarafından atanan bir yönetilen kimlikle oluşturmak için yetkilendirme üstbilgisindeki bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2. SANAL ağınız için bir [ağ arabirimi](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) oluşturun:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3. VM 'nizi sistem tarafından atanan bir yönetilen kimlikle oluşturmak için yetkilendirme üstbilgisindeki bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alın.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Burada bulunan yönergeleri kullanarak Kullanıcı tarafından atanan bir yönetilen kimlik oluşturun: [Kullanıcı tarafından atanan yönetilen kimlik oluşturma](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

5. Azure Resource Manager REST uç noktasını çağırmak için KıVRıMLı kullanarak bir VM oluşturun. Aşağıdaki örnek, bir kullanıcı tarafından atanan yönetilen kimlik `ID1`kaynak grubunda *myvm* *adlı bir* VM oluşturur `"identity":{"type":"UserAssigned"}`değer tarafından tanımlanan istek gövdesinde tanımlanmıştır. `<ACCESS TOKEN>`, bir taşıyıcı erişim belirteci istediğinizde ve ortamınız için uygun olan `<SUBSCRIPTION ID>` değerini, önceki adımda aldığınız değerle değiştirin.
 
   **APı SÜRÜMÜ 2018-06-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PUT -d '{"location":"westus","name":"myVM","identity":{"type":"UserAssigned","identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"myVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```
    
   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {  
       "location":"westus",
       "name":"myVM",
       "identity":{  
          "type":"UserAssigned",
          "identityIds":[  
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       },
       "properties":{  
          "hardwareProfile":{  
             "vmSize":"Standard_D2_v2"
          },
          "storageProfile":{  
             "imageReference":{  
                "sku":"2016-Datacenter",
                "publisher":"MicrosoftWindowsServer",
                "version":"latest",
                "offer":"WindowsServer"
             },
             "osDisk":{  
                "caching":"ReadWrite",
                "managedDisk":{  
                   "storageAccountType":"Standard_LRS"
                },
                "name":"myVM3osdisk",
                "createOption":"FromImage"
             },
             "dataDisks":[  
                {  
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":0
                },
                {  
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":1
                }
             ]
          },
          "osProfile":{  
             "adminUsername":"azureuser",
             "computerName":"myVM",
             "adminPassword":"myPassword12"
          },
          "networkProfile":{  
             "networkInterfaces":[  
                {  
                   "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
                   "properties":{  
                      "primary":true
                   }
                }
             ]
          }
       }
    }

   ```
  
   **APı SÜRÜMÜ 2017-12-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PUT -d '{"location":"westus","name":"myVM","identity":{"type":"UserAssigned","identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"myVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {
       "location":"westus",
       "name":"myVM",
       "identity":{
          "type":"UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       },
       "properties":{
          "hardwareProfile":{
             "vmSize":"Standard_D2_v2"
          },
          "storageProfile":{
             "imageReference":{
                "sku":"2016-Datacenter",
                "publisher":"MicrosoftWindowsServer",
                "version":"latest",
                "offer":"WindowsServer"
             },
             "osDisk":{
                "caching":"ReadWrite",
                "managedDisk":{
                   "storageAccountType":"Standard_LRS"
                },
                "name":"myVM3osdisk",
                "createOption":"FromImage"
             },
             "dataDisks":[
                {
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":0
                },
                {
                   "diskSizeGB":1023,
                   "createOption":"Empty",
                   "lun":1
                }
             ]
          },
          "osProfile":{
             "adminUsername":"azureuser",
             "computerName":"myVM",
             "adminPassword":"myPassword12"
          },
          "networkProfile":{
             "networkInterfaces":[
                {
                   "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
                   "properties":{
                      "primary":true
                   }
                }
             ]
          }
       }
    }
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Mevcut bir Azure VM 'sine Kullanıcı tarafından atanan bir yönetilen kimlik atama

Bir VM 'ye Kullanıcı tarafından atanan bir kimlik atamak için hesabınızın [sanal makine katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) ve [yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol atamalarına ihtiyacı vardır. Ek Azure AD dizin rolü ataması gerekli değildir.

1. VM 'nizi sistem tarafından atanan bir yönetilen kimlikle oluşturmak için yetkilendirme üstbilgisindeki bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Burada bulunan yönergeleri kullanarak Kullanıcı tarafından atanan bir yönetilen kimlik oluşturun, [Kullanıcı tarafından atanan bir yönetilen kimlik oluşturun](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

3. VM 'ye atanan mevcut kullanıcı veya sistem tarafından atanan yönetilen kimlikleri silmemenizi sağlamak için aşağıdaki KıVRıMLı komutunu kullanarak VM 'ye atanan kimlik türlerini listeetmeniz gerekir. Sanal makine ölçek kümesine atanmış Yönetilen kimlikler varsa, bunlar `identity` değerinde listelenir.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
   ```

   ```HTTP
   GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01 HTTP/1.1
   ```
   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın.

    Yanıttaki `identity` değerde tanımlandığı şekilde VM 'ye atanan kullanıcı veya sistem tarafından atanan yönetilen kimlikleriniz varsa, VM 'nize Kullanıcı tarafından atanan yönetilen kimlik eklerken, sistem tarafından atanan yönetilen kimliğin nasıl tutulacağını gösteren 5. adıma atlayın.

4. Sanal makinenize atanan kullanıcı tarafından atanan yönetilen kimliğiniz yoksa, VM 'ye ilk Kullanıcı tarafından atanan yönetilen kimliği atamak için Azure Resource Manager REST uç noktasını çağırmak üzere aşağıdaki KıVRıMLı komutunu kullanın.

   Aşağıdaki örneklerde, *Myresourcegroup*kaynak grubundaki *MYVM* adlı bir VM 'ye `ID1` Kullanıcı tarafından atanan yönetilen bir kimlik atanır.  `<ACCESS TOKEN>`, bir taşıyıcı erişim belirteci istediğinizde ve ortamınız için uygun olan `<SUBSCRIPTION ID>` değerini, önceki adımda aldığınız değerle değiştirin.

   **APı SÜRÜMÜ 2018-06-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın.        |
 
   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
    
             }
          }
       }
    }
   ```

   **APı SÜRÜMÜ 2017-12-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01 HTTP/1.1
   ```
   
   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"userAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       }
    }
   ```

5. VM 'nize atanan, Kullanıcı tarafından atanan veya sistem tarafından atanan bir yönetilen Kimliğiniz varsa:
   
   **APı SÜRÜMÜ 2018-06-01**

   Kullanıcı tarafından atanan yönetilen kimliği `userAssignedIdentities` sözlük değerine ekleyin.
    
   Örneğin, sistem tarafından atanan yönetilen kimlik ve Kullanıcı tarafından atanan yönetilen kimlik `ID1` sanal makinenizde Şu anda atanmış olan ve Kullanıcı tarafından atanan yönetilen kimlik `ID2` eklemek istiyorsanız:

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```
   
   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned, UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
    
             },
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{
    
             }
          }
       }
    }
   ```

   **APı SÜRÜMÜ 2017-12-01**

   Kullanıcı tarafından atanan yeni yönetilen kimliği eklerken `identityIds` dizi değerinde tutmak istediğiniz kullanıcı tarafından atanan yönetilen kimlikleri koruyun.

   Örneğin, sistem tarafından atanan yönetilen kimlik ve Kullanıcı tarafından atanan yönetilen kimlik `ID1` sanal makinenizde Şu anda atanmış olan ve Kullanıcı tarafından atanan yönetilen kimlik `ID2` eklemek istiyorsanız: 

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned,UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1",
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"
          ]
       }
    }
   ```   

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Kullanıcı tarafından atanan yönetilen kimliği bir Azure VM 'den kaldırma

Kullanıcı tarafından atanan kimliği bir VM 'ye kaldırmak için hesabınızın [sanal makine katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) rolü ataması gerekir.

1. VM 'nizi sistem tarafından atanan bir yönetilen kimlikle oluşturmak için yetkilendirme üstbilgisindeki bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2. VM 'ye atanmasını veya sistem tarafından atanan yönetilen kimliği kaldırmayı istediğiniz var olan kullanıcı tarafından atanan yönetilen kimlikleri silmemenizi sağlamak için aşağıdaki KıVRıMLı komutunu kullanarak yönetilen kimlikleri listeetmeniz gerekir: 

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```
 
   ```HTTP
   GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın.
 
   VM 'ye atanmış Yönetilen kimlikler varsa, bu değerler `identity` değerindeki yanıtta listelenir.

   Örneğin, Kullanıcı tarafından atanan yönetilen kimliklere `ID1` ve `ID2` sanal makinenize atandı ve yalnızca `ID1` atanmasını ve sistem tarafından atanan kimliği tutmayı istiyorsanız:
   
   **APı SÜRÜMÜ 2018-06-01**

   Kaldırmak istediğiniz kullanıcı tarafından atanan yönetilen kimliğe `null` ekleyin:

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":null}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned, UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":null
          }
       }
    }
   ```

   **APı SÜRÜMÜ 2017-12-01**

   `identityIds` dizide tutmak istediğiniz yalnızca Kullanıcı tarafından atanan yönetilen kimlikleri koru:

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*İçerik türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın.        | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned, UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       }
    }
   ```

SANAL makinenizde hem sistem tarafından atanan hem de Kullanıcı tarafından atanan Yönetilen kimlikler varsa, aşağıdaki komutu kullanarak yalnızca sistem tarafından atanan yönetilen kimliği kullan ' a geçerek Kullanıcı tarafından atanan tüm yönetilen kimlikleri kaldırabilirsiniz:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

```HTTP
PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
```

**İstek üst bilgileri**

|İstek üst bilgisi  |Açıklama  |
|---------|---------|
|*İçerik türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
|*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. | 

**İstek gövdesi**

```JSON
{
   "identity":{
      "type":"SystemAssigned"
   }
}
```
    
SANAL makinenizin yalnızca Kullanıcı tarafından atanan yönetilen kimlikleri varsa ve bunların tümünü kaldırmak istiyorsanız aşağıdaki komutu kullanın:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

```HTTP
PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2018-06-01 HTTP/1.1
```

**İstek üst bilgileri**

|İstek üst bilgisi  |Açıklama  |
|---------|---------|
|*İçerik türü*     | Gereklidir. `application/json` olarak ayarlayın.        |
|*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın.| 

**İstek gövdesi**

```JSON
{
   "identity":{
      "type":"None"
   }
}
```

## <a name="next-steps"></a>Sonraki adımlar

REST kullanarak Kullanıcı tarafından atanan Yönetilen kimlikler oluşturma, listeleme veya silme hakkında bilgi için bkz.:

- [REST API çağrılarını kullanarak Kullanıcı tarafından atanan Yönetilen kimlikler oluşturma, listeleme veya silme](how-to-manage-ua-identity-rest.md)
