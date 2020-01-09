---
title: REST-Azure AD kullanarak Azure VMSS 'de Yönetilen kimlikler yapılandırma
description: REST API çağrısı yapmak için bir Azure VMSS 'de bir sistem ve Kullanıcı tarafından atanan Yönetilen kimlikler yapılandırmaya yönelik adım adım yönergeler.
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
ms.openlocfilehash: dce9894b26d03c351a2209792cc076de91feba54
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75429988"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-virtual-machine-scale-set-using-rest-api-calls"></a>REST API çağrılarını kullanarak bir sanal makine ölçek kümesindeki Azure kaynakları için Yönetilen kimlikler yapılandırma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Azure kaynakları için Yönetilen kimlikler, Azure Active Directory ' de otomatik olarak yönetilen bir sistem kimliğiyle Azure hizmetleri sağlar. Bu kimliği, kodunuzda kimlik bilgileri olmadan Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmak için kullanabilirsiniz. 

Bu makalede, Azure Resource Manager REST uç noktasına çağrı yapmak için KıVRıMLı kullanarak, sanal makine ölçek kümesindeki Azure kaynakları işlemleri için aşağıdaki yönetilen kimliklerin nasıl gerçekleştirileceğini öğreneceksiniz:

- Azure sanal makine ölçek kümesi üzerinde sistem tarafından atanan yönetilen kimliği etkinleştirme ve devre dışı bırakma
- Azure sanal makine ölçek kümesine Kullanıcı tarafından atanan yönetilen kimlik ekleme ve kaldırma

## <a name="prerequisites"></a>Ön koşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-the-managed-identities-for-azure-resources-work)** .
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makaledeki yönetim işlemlerini gerçekleştirmek için, hesabınız aşağıdaki Azure rol tabanlı erişim denetimi atamalarına ihtiyaç duyuyor:

    > [!NOTE]
    > Ek Azure AD dizin rolü ataması gerekli değildir.

    - Sanal makine [katılımcısı](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) bir sanal makine ölçek kümesi oluşturmak ve sistem ve/veya Kullanıcı tarafından atanan yönetilen kimliği bir sanal makine ölçek kümesinden etkinleştirmek ve kaldırmak için.
    - Kullanıcı tarafından atanan yönetilen kimlik oluşturmak için [yönetilen kimlik katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü.
    - Kullanıcı tarafından atanan bir kimliği ve sanal makine ölçek kümesine atamak ve kaldırmak için [yönetilen kimlik operatörü](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rolü.
- Windows kullanıyorsanız, [Linux Için Windows alt sistemini](https://msdn.microsoft.com/commandline/wsl/about) yükledikten sonra Azure Portal [Azure Cloud Shell](../../cloud-shell/overview.md) kullanın.
- [Linux Için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about) veya [Linux dağıtım Işletim SISTEMI](/cli/azure/install-azure-cli-apt?view=azure-cli-latest)kullanıyorsanız [Azure CLI yerel konsolunu yükleyebilirsiniz](/cli/azure/install-azure-cli).
- Azure CLı yerel Konsolu kullanıyorsanız, sistem veya Kullanıcı tarafından atanan yönetilen kimlikleri yönetmek istediğiniz Azure aboneliğiyle ilişkili bir hesapla `az login` kullanarak Azure 'da oturum açın.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-managed-identity"></a>Sistem tarafından atanan yönetilen kimlik

Bu bölümde, Azure Resource Manager REST uç noktasına çağrı yapmak için KıVRıMLı kullanarak bir sanal makine ölçek kümesinde sistem tarafından atanan yönetilen kimliğin nasıl etkinleştirileceğini ve devre dışı bırakılacağını öğrenirsiniz.

### <a name="enable-system-assigned-managed-identity-during-creation-of-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi oluştururken sistem tarafından atanan yönetilen kimliği etkinleştir

Sistem tarafından atanan yönetilen kimlik etkin bir sanal makine ölçek kümesi oluşturmak için, bir sanal makine ölçek kümesi oluşturmanız ve Kaynak Yöneticisi uç noktasını sistem tarafından atanan yönetilen kimlik türü değeriyle çağırmak için bir erişim belirteci almanız gerekir.

1. [Az Group Create](/cli/azure/group/#az-group-create)kullanılarak sanal makine ölçek kümesinin ve ilgili kaynaklarının kapsama ve dağıtımı için bir [kaynak grubu](../../azure-resource-manager/management/overview.md#terminology) oluşturun. Bunun yerine kullanmak istediğiniz bir kaynak grubunuz varsa, bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. Sanal makine ölçek kümesi için bir [ağ arabirimi](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) oluşturun:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3. Sistem tarafından atanan yönetilen kimlik ile sanal makine ölçek kümesini oluşturmak için yetkilendirme üstbilgisindeki bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alın.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Azure Resource Manager REST uç noktasını çağırmak için KıVRıMLı kullanarak bir sanal makine ölçek kümesi oluşturun. Aşağıdaki örnek, `"identity":{"type":"SystemAssigned"}`değere göre istek gövdesinde tanımlandığı gibi, *Myresourcegroup* Içindeki *myvmss* adlı bir sanal makine ölçek kümesi oluşturur ve sistem tarafından atanan yönetilen kimlik. `<ACCESS TOKEN>`, bir taşıyıcı erişim belirteci istediğinizde ve ortamınız için uygun olan `<SUBSCRIPTION ID>` değerini, önceki adımda aldığınız değerle değiştirin.

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus","identity":{"type":"SystemAssigned"},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Content-Type*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. | 

   **İstek gövdesi**

   ```JSON
    {
       "sku":{
          "tier":"Standard",
          "capacity":3,
          "name":"Standard_D1_v2"
       },
       "location":"eastus",
       "identity":{
          "type":"SystemAssigned"
       },
       "properties":{
          "overprovision":true,
          "virtualMachineProfile":{
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
                   "createOption":"FromImage"
                }
             },
             "osProfile":{
                "computerNamePrefix":"myVMSS",
                "adminUsername":"azureuser",
                "adminPassword":"myPassword12"
             },
             "networkProfile":{
                "networkInterfaceConfigurations":[
                   {
                      "name":"myVMSS",
                      "properties":{
                         "primary":true,
                         "enableIPForwarding":true,
                         "ipConfigurations":[
                            {
                               "name":"myVMSS",
                               "properties":{
                                  "subnet":{
                                     "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
                                  }
                               }
                            }
                         ]
                      }
                   }
                ]
             }
          },
          "upgradePolicy":{
             "mode":"Manual"
          }
       }
    }  
   ```  

### <a name="enable-system-assigned-managed-identity-on-an-existing-virtual-machine-scale-set"></a>Mevcut bir sanal makine ölçek kümesi üzerinde sistem tarafından atanan yönetilen kimliği etkinleştir

Mevcut bir sanal makine ölçek kümesi üzerinde sistem tarafından atanan yönetilen kimliği etkinleştirmek için bir erişim belirteci edinmeniz ve ardından, kimlik türünü güncelleştirmek üzere Kaynak Yöneticisi REST uç noktasını çağırmak için KıVRıMLı kullanmanız gerekir.

1. Sistem tarafından atanan yönetilen kimlik ile sanal makine ölçek kümesini oluşturmak için yetkilendirme üstbilgisindeki bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Sanal makine ölçek kümesinde, *Myvmss*adlı bir sanal makine ölçek kümesi için `{"identity":{"type":"SystemAssigned"}` değer tarafından tanımlanan değere göre, sistem tarafından atanan yönetilen kimliği etkinleştirmek üzere Azure Resource Manager REST uç noktasını çağırmak IÇIN aşağıdaki kıvrımlı komutunu kullanın.  `<ACCESS TOKEN>`, bir taşıyıcı erişim belirteci istediğinizde ve ortamınız için uygun olan `<SUBSCRIPTION ID>` değerini, önceki adımda aldığınız değerle değiştirin.
   
   > [!IMPORTANT]
   > Sanal makine ölçek kümesine atanmış olan, Kullanıcı tarafından atanan yönetilen kimlikleri silmemenizi sağlamak için, bu KıVRıMLı komutunu kullanarak Kullanıcı tarafından atanan yönetilen kimlikleri listeetmeniz gerekir: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Yanıttaki `identity` değerde tanımlandığı şekilde sanal makine ölçek kümesine atanan kullanıcı tarafından atanan yönetilen kimlikleriniz varsa, sanal makine ölçek kümesinde sistem tarafından atanan yönetilen kimliği etkinleştirirken Kullanıcı tarafından atanan yönetilen kimliklerin nasıl tutulacağını gösteren 3. adıma atlayın.

   ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Content-Type*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned"
       }
    }
   ```

3. Kullanıcı tarafından atanan yönetilen kimlikleri olan bir sanal makine ölçek kümesinde sistem tarafından atanan yönetilen kimliği etkinleştirmek için, `type` değerine `SystemAssigned` eklemeniz gerekir.  
   
   Örneğin, sanal makine ölçek kümesinde Kullanıcı tarafından atanan Yönetilen kimlikler `ID1` ve buna atanmış `ID2` varsa ve sanal makine ölçek kümesine sistem tarafından atanan yönetilen kimlik eklemek istiyorsanız aşağıdaki KıVRıMLı çağrısını kullanın. `<ACCESS TOKEN>` ve `<SUBSCRIPTION ID>`, ortamınız için uygun değerlerle değiştirin.

   API sürümü `2018-06-01`, Kullanıcı tarafından atanan yönetilen kimlikleri, API `2017-12-01`sürümünde kullanılan bir dizi biçimindeki `identityIds` değerine karşılık gelen `userAssignedIdentities` değerindeki bir sözlük biçiminde depolar.
   
   **APı SÜRÜMÜ 2018-06-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Content-Type*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. |
 
   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned,UserAssigned",
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

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Content-Type*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. | 

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

### <a name="disable-system-assigned-managed-identity-from-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesinden sistem tarafından atanan yönetilen kimliği devre dışı bırak

Var olan bir sanal makine ölçek kümesinde sistem tarafından atanan bir kimliği devre dışı bırakmak için, bir erişim belirteci edinmeniz ve sonra Kaynak Yöneticisi REST uç noktasını çağırmak için, kimlik türünü `None`olarak güncellemek üzere KıVRıMLı kullanmanız gerekir.

1. Sistem tarafından atanan yönetilen kimlik ile sanal makine ölçek kümesini oluşturmak için yetkilendirme üstbilgisindeki bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Sistem tarafından atanan yönetilen kimliği devre dışı bırakmak için Azure Resource Manager REST uç noktasını çağırmak üzere sanal makine ölçek kümesini KıVRıMLı kullanarak güncelleştirin.  Aşağıdaki örnek, *Myvmss*adlı bir sanal makine ölçek kümesinden `{"identity":{"type":"None"}}` değere göre istek gövdesinde tanımlanan sistem tarafından atanan yönetilen kimliği devre dışı bırakır.  `<ACCESS TOKEN>`, bir taşıyıcı erişim belirteci istediğinizde ve ortamınız için uygun olan `<SUBSCRIPTION ID>` değerini, önceki adımda aldığınız değerle değiştirin.

   > [!IMPORTANT]
   > Sanal makine ölçek kümesine atanmış olan, Kullanıcı tarafından atanan yönetilen kimlikleri silmemenizi sağlamak için, bu KıVRıMLı komutunu kullanarak Kullanıcı tarafından atanan yönetilen kimlikleri listeetmeniz gerekir: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Sanal makine ölçek kümesine atanan kullanıcı tarafından atanan yönetilen Kimliğiniz varsa, sanal makine ölçek kümesinden sistem tarafından atanan yönetilen kimliği kaldırırken Kullanıcı tarafından atanan yönetilen kimliklerin nasıl tutulacağını gösteren 3. adıma atlayın.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Content-Type*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"None"
       }
    }
   ```

   Kullanıcı tarafından atanan yönetilen kimlikleri olan bir sanal makine ölçek kümesinden sistem tarafından atanan yönetilen kimliği kaldırmak için, **apı 2018-06-01**kullanıyorsanız `UserAssigned` değerini ve `userAssignedIdentities` sözlük değerlerini tutarken `{"identity":{"type:" "}}` değerden `SystemAssigned` kaldırın. **API sürüm 2017-12-01** veya önceki bir sürümünü kullanıyorsanız `identityIds` diziyi saklayın.

## <a name="user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik

Bu bölümde, Azure Resource Manager REST uç noktasına çağrı yapmak için KıVRıMLı kullanarak bir sanal makine ölçek kümesinde Kullanıcı tarafından atanan yönetilen kimliğin nasıl ekleneceğini ve kaldırılacağını öğrenirsiniz.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi oluşturulurken Kullanıcı tarafından atanan yönetilen kimlik atama

1. Sistem tarafından atanan yönetilen kimlik ile sanal makine ölçek kümesini oluşturmak için yetkilendirme üstbilgisindeki bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Sanal makine ölçek kümesi için bir [ağ arabirimi](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) oluşturun:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3. Sistem tarafından atanan yönetilen kimlik ile sanal makine ölçek kümesini oluşturmak için yetkilendirme üstbilgisindeki bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alın.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Burada bulunan yönergeleri kullanarak Kullanıcı tarafından atanan bir yönetilen kimlik oluşturun: [Kullanıcı tarafından atanan yönetilen kimlik oluşturma](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

5. Azure Resource Manager REST uç noktasını çağırmak için KıVRıMLı kullanarak bir sanal makine ölçek kümesi oluşturun. Aşağıdaki örnek, bir kullanıcı tarafından atanan yönetilen kimlik `ID1`*kaynak grubunda* *myvmss* adlı bir sanal makine ölçek kümesi oluşturur `"identity":{"type":"UserAssigned"}`değer tarafından tanımlanan istek gövdesinde tanımlanmıştır. `<ACCESS TOKEN>`, bir taşıyıcı erişim belirteci istediğinizde ve ortamınız için uygun olan `<SUBSCRIPTION ID>` değerini, önceki adımda aldığınız değerle değiştirin.
 
   **APı SÜRÜMÜ 2018-06-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus","identity":{"type":"UserAssigned","userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Content-Type*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. | 

   **İstek gövdesi**

   ```JSON
    {
       "sku":{
          "tier":"Standard",
          "capacity":3,
          "name":"Standard_D1_v2"
       },
       "location":"eastus",
       "identity":{
          "type":"UserAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
    
             }
          }
       },
       "properties":{
          "overprovision":true,
          "virtualMachineProfile":{
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
                   "createOption":"FromImage"
                }
             },
             "osProfile":{
                "computerNamePrefix":"myVMSS",
                "adminUsername":"azureuser",
                "adminPassword":"myPassword12"
             },
             "networkProfile":{
                "networkInterfaceConfigurations":[
                   {
                      "name":"myVMSS",
                      "properties":{
                         "primary":true,
                         "enableIPForwarding":true,
                         "ipConfigurations":[
                            {
                               "name":"myVMSS",
                               "properties":{
                                  "subnet":{
                                     "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
                                  }
                               }
                            }
                         ]
                      }
                   }
                ]
             }
          },
          "upgradePolicy":{
             "mode":"Manual"
          }
       }
    }
   ```   

   **APı SÜRÜMÜ 2017-12-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus","identity":{"type":"UserAssigned","identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Content-Type*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. |
 
   **İstek gövdesi**

   ```JSON
    {
       "sku":{
          "tier":"Standard",
          "capacity":3,
          "name":"Standard_D1_v2"
       },
       "location":"eastus",
       "identity":{
          "type":"UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       },
       "properties":{
          "overprovision":true,
          "virtualMachineProfile":{
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
                   "createOption":"FromImage"
                }
             },
             "osProfile":{
                "computerNamePrefix":"myVMSS",
                "adminUsername":"azureuser",
                "adminPassword":"myPassword12"
             },
             "networkProfile":{
                "networkInterfaceConfigurations":[
                   {
                      "name":"myVMSS",
                      "properties":{
                         "primary":true,
                         "enableIPForwarding":true,
                         "ipConfigurations":[
                            {
                               "name":"myVMSS",
                               "properties":{
                                  "subnet":{
                                     "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
                                  }
                               }
                            }
                         ]
                      }
                   }
                ]
             }
          },
          "upgradePolicy":{
             "mode":"Manual"
          }
       }
    }
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Mevcut bir Azure sanal makine ölçek kümesine Kullanıcı tarafından atanan bir yönetilen kimlik atama

1. Sistem tarafından atanan yönetilen kimlik ile sanal makine ölçek kümesini oluşturmak için yetkilendirme üstbilgisindeki bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Burada bulunan yönergeleri kullanarak Kullanıcı tarafından atanan bir yönetilen kimlik oluşturun, [Kullanıcı tarafından atanan bir yönetilen kimlik oluşturun](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

3. Sanal makine ölçek kümesine atanan mevcut kullanıcı veya sistem tarafından atanan yönetilen kimlikleri silmemenizi sağlamak için, aşağıdaki KıVRıMLı komutunu kullanarak sanal makine ölçek kümesine atanan kimlik türlerini listeetmeniz gerekir. Sanal makine ölçek kümesine atanmış Yönetilen kimlikler varsa, bunlar `identity` değerinde listelenir.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. |   
 

4. Sanal makine ölçek kümesine atanan kullanıcı veya sistem tarafından atanan yönetilen kimliğiniz yoksa, sanal makineye ilk Kullanıcı tarafından atanan yönetilen kimliği atamak için Azure Resource Manager REST uç noktasını çağırmak üzere aşağıdaki KıVRıMLı komutunu kullanın ölçek kümesi.  Sanal makine ölçek kümesine atanan bir kullanıcı veya sistem tarafından atanan yönetilen Kimliğiniz varsa, bir sanal makine ölçek kümesine birden çok kullanıcı tarafından atanan yönetilen kimlik ekleme işlemini gösteren 5. adıma atlayın ve ayrıca sistem tarafından atanan yönetilen IDENTITY.

   Aşağıdaki örnek, *Myresourcegroup*kaynak grubundaki *myvmss* adlı bir sanal makine ölçek kümesine `ID1` Kullanıcı tarafından atanan yönetilen bir kimlik atar.  `<ACCESS TOKEN>`, bir taşıyıcı erişim belirteci istediğinizde ve ortamınız için uygun olan `<SUBSCRIPTION ID>` değerini, önceki adımda aldığınız değerle değiştirin.

   **APı SÜRÜMÜ 2018-06-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-12-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Content-Type*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"userAssigned",
          "userAssignedIdentities":{
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{
    
             }
          }
       }
    }
   ``` 
    
   **APı SÜRÜMÜ 2017-12-01**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Content-Type*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. | 

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

5. Sanal makine ölçek kümesine atanmış olan, Kullanıcı tarafından atanan veya sistem tarafından atanan bir yönetilen Kimliğiniz varsa:
   
   **APı SÜRÜMÜ 2018-06-01**

   Kullanıcı tarafından atanan yönetilen kimliği `userAssignedIdentities` sözlük değerine ekleyin.

   Örneğin, sistem tarafından atanan yönetilen kimlik ve Kullanıcı tarafından atanan yönetilen kimlik `ID1`, şu anda sanal makine ölçeklendirmenize atandı ve Kullanıcı tarafından atanan yönetilen kimlik `ID2` eklemek istiyorsanız:

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Content-Type*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. | 

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

   Örneğin, şu anda sanal makine ölçek kümesine atanmış olan ve Kullanıcı tarafından atanan yönetilen kimlik `ID1`, sistem tarafından atanan bir kimliğiniz ve Kullanıcı tarafından atanan yönetilen kimlik `ID2` eklemek istiyorsanız:

    ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
   ```

    **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Content-Type*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned, UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1",
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"
          ]
       }
    }
   ```

### <a name="remove-a-user-assigned-managed-identity-from-a-virtual-machine-scale-set"></a>Kullanıcı tarafından atanan yönetilen kimliği bir sanal makine ölçek kümesinden kaldırma

1. Sistem tarafından atanan yönetilen kimlik ile sanal makine ölçek kümesini oluşturmak için yetkilendirme üstbilgisindeki bir sonraki adımda kullanacağınız bir taşıyıcı erişim belirteci alın.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Sanal makine ölçek kümesine atanmasını veya sistem tarafından atanan yönetilen kimliği kaldırmayı istediğiniz mevcut kullanıcı tarafından atanan yönetilen kimlikleri silmemenizi sağlamak için, aşağıdaki KıVRıMLı komutunu kullanarak yönetilen kimlikleri listemeniz gerekir :

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
   ```

   ```HTTP
   GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. |
   
   VM 'ye atanmış Yönetilen kimlikler varsa, bu değerler `identity` değerindeki yanıtta listelenir. 
    
   Örneğin, Kullanıcı tarafından atanan yönetilen kimliklere `ID1` ve sanal makine ölçek kümesine atanan `ID2` ve yalnızca `ID1` atanmasını ve sistem tarafından atanan yönetilen kimliği tutmayı istiyorsanız:

   **APı SÜRÜMÜ 2018-06-01**

   Kaldırmak istediğiniz kullanıcı tarafından atanan yönetilen kimliğe `null` ekleyin:

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":null}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Content-Type*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. | 

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
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```   

   ```HTTP
   PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01 HTTP/1.1
   ```

   **İstek üst bilgileri**

   |İstek üst bilgisi  |Açıklama  |
   |---------|---------|
   |*Content-Type*     | Gereklidir. `application/json` olarak ayarlayın.        |
   |*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. | 

   **İstek gövdesi**

   ```JSON
    {
       "identity":{
          "type":"SystemAssigned,UserAssigned",
          "identityIds":[
             "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"
          ]
       }
    }
   ```

Sanal makine ölçek kümesinde hem sistem tarafından atanan hem de Kullanıcı tarafından atanan Yönetilen kimlikler varsa, aşağıdaki komutu kullanarak yalnızca sistem tarafından atanan ' i kullanmaya geçerek Kullanıcı tarafından atanan tüm yönetilen kimlikleri kaldırabilirsiniz:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

```HTTP
PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
```

**İstek üst bilgileri**

|İstek üst bilgisi  |Açıklama  |
|---------|---------|
|*Content-Type*     | Gereklidir. `application/json` olarak ayarlayın.        |
|*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. | 

**İstek gövdesi**

```JSON
{
   "identity":{
      "type":"SystemAssigned"
   }
}
```
    
Sanal makine ölçek ayarlandıysa yalnızca Kullanıcı tarafından atanan Yönetilen kimlikler varsa ve bunları tümünü kaldırmak istiyorsanız aşağıdaki komutu kullanın:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

```HTTP
PATCH https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01 HTTP/1.1
```

**İstek üst bilgileri**

|İstek üst bilgisi  |Açıklama  |
|---------|---------|
|*Content-Type*     | Gereklidir. `application/json` olarak ayarlayın.        |
|*Yetkilendirme*     | Gereklidir. Geçerli bir `Bearer` erişim belirtecine ayarlayın. | 

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

- [REST API çağrılarını kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturma, listeleme veya silme](how-to-manage-ua-identity-rest.md)
