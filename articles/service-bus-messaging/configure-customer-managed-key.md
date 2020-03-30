---
title: Azure Hizmet Veri Çaracı verilerini şifrelemek için kendi anahtarınızı yapılandırma
description: Bu makalede, Azure Hizmet Veri Tos veri geri kalanı şifrelemek için kendi anahtarı yapılandırmak hakkında bilgi sağlar.
services: service-bus-messaging
ms.service: service-bus
documentationcenter: ''
author: axisc
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: aschhab
ms.openlocfilehash: aeb9a9730ddc61793e49c9e042906457e0068d9a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77624080"
---
# <a name="configure-customer-managed-keys-for-encrypting-azure-service-bus-data-at-rest-by-using-the-azure-portal"></a>Azure Hizmet Veri Servisi verilerini şifrelemek için müşteri tarafından yönetilen anahtarları Azure portalını kullanarak yapılandırma
Azure Hizmet Veri Hizmeti Premium, Azure Depolama Hizmeti Şifrelemesi (Azure SSE) ile veri şifrelemesini sağlar. Service Bus Premium verileri depolamak için Azure Depolama'ya güvenir ve varsayılan olarak Azure Depolama ile depolanan tüm veriler Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. 

## <a name="overview"></a>Genel Bakış
Azure Hizmet Veri Servisi artık verileri Microsoft tarafından yönetilen anahtarlarla veya müşteri tarafından yönetilen anahtarlarla (Kendi Anahtarınızı Getir - BYOK) ile birlikte şifreleme seçeneğini destekler. Bu özellik, Azure Hizmet Veri Veri Tos'u şifrelemek için kullanılan müşteri tarafından yönetilen anahtarlara erişimi oluşturmanıza, döndürmenize, devre dışı etmenizi ve iptal etmenizi sağlar.

BYOK özelliğini etkinleştirmek, ad alanınızda tek seferlik bir kurulum işlemidir.

> [!NOTE]
> Hizmet tarafı şifreleme si için müşteri tarafından yönetilen anahtara yönelik bazı uyarılar vardır. 
>   * Bu özellik Azure [Service Bus Premium](service-bus-premium-messaging.md) katmanı tarafından desteklenir. Standart katman Hizmet Veri Servisi ad alanları için etkinleştirilemez.
>   * Şifreleme yalnızca yeni veya boş ad alanları için etkinleştirilebilir. Ad alanı veri içeriyorsa, şifreleme işlemi başarısız olur.

Anahtarlarınızı yönetmek ve anahtar kullanımınızı denetlemek için Azure Key Vault'u kullanabilirsiniz. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya anahtar oluşturmak için Azure Key Vault API'lerini kullanabilirsiniz. Azure Anahtar Kasası hakkında daha fazla bilgi için Azure [Anahtar Kasası nedir?](../key-vault/key-vault-overview.md)

Bu makalede, Azure portalını kullanarak müşteri tarafından yönetilen anahtarlarla anahtar kasasının nasıl yapılandırılabildiğini gösterilmektedir. Azure portalını kullanarak önemli bir kasa oluşturmayı öğrenmek için [Quickstart: Azure portalını kullanarak Azure Key Vault'tan bir sır ayarlayın ve alın.](../key-vault/quick-create-portal.md)

> [!IMPORTANT]
> Azure Hizmet Veri Servisi ile müşteri tarafından yönetilen anahtarları kullanmak, anahtar kasasının iki gerekli özellisini yapılandırmasını gerektirir. Bunlar: **Yumuşak Silme** ve **Temizleme etmeyin**. Azure portalında yeni bir anahtar kasası oluşturduğunuzda, bu özellikler varsayılan olarak etkinleştirilir. Ancak, bu özellikleri varolan bir anahtar kasasında etkinleştirmeniz gerekiyorsa, PowerShell veya Azure CLI'yi kullanmanız gerekir.

## <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları etkinleştirme
Azure portalında müşteri tarafından yönetilen anahtarları etkinleştirmek için aşağıdaki adımları izleyin:

1. Service Bus Premium ad alanınıza gidin.
2. Hizmet Veri Aracı ad alanınızın **Ayarlar** sayfasında **Şifreleme'yi**seçin.
3. Aşağıdaki resimde gösterildiği **gibi, Müşteri tarafından yönetilen anahtar şifrelemesini istirahatte** seçin.

    ![Müşteri yönetilen anahtarını etkinleştirme](./media/configure-customer-managed-key/enable-customer-managed-key.png)


## <a name="set-up-a-key-vault-with-keys"></a>Anahtarlarla anahtar kasası ayarlama

Müşteri tarafından yönetilen anahtarları etkinleştirdikten sonra, müşteri yönetilen anahtarını Azure Servis Veri Veri Kurumu ad alanınızla ilişkilendirmeniz gerekir. Servis Veri Servisi yalnızca Azure Anahtar Kasası'nı destekler. Önceki bölümde **müşteri tarafından yönetilen anahtar seçeneğiyle Şifreleme'yi** etkinleştiriseniz, anahtarın Azure Anahtar Kasası'na aktarılmasını sağlamanız gerekir. Ayrıca, anahtarlar **Yumuşak Silme** ve **Temizleme anahtarı** için yapılandırılan etmeyin olmalıdır. Bu ayarlar [PowerShell](../key-vault/key-vault-soft-delete-powershell.md) veya [CLI](../key-vault/key-vault-soft-delete-cli.md#enabling-purge-protection)kullanılarak yapılandırılabilir.

1. Yeni bir anahtar kasası oluşturmak için Azure Key Vault [Quickstart'ı](../key-vault/key-vault-overview.md)izleyin. Varolan anahtarları alma hakkında daha fazla bilgi için [anahtarlar, sırlar ve sertifikalar hakkında](../key-vault/about-keys-secrets-and-certificates.md)bilgi edinebilirsiniz.
1. Kasa oluştururken hem yumuşak silme hem de temizleme korumasını açmak için [az keyvault oluşturma](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) komutunu kullanın.

    ```azurecli-interactive
    az keyvault create --name contoso-SB-BYOK-keyvault --resource-group ContosoRG --location westus --enable-soft-delete true --enable-purge-protection true
    ```    
1. Varolan bir kasaya temizleme koruması eklemek için (zaten yumuşak silme etkinleştirilmiş), [az keyvault güncelleştirme](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update) komutunu kullanın.

    ```azurecli-interactive
    az keyvault update --name contoso-SB-BYOK-keyvault --resource-group ContosoRG --enable-purge-protection true
    ```
1. Aşağıdaki adımları izleyerek anahtarlar oluşturun:
    1. Yeni bir anahtar oluşturmak için **Ayarlar**menüsünden **Ayarlar** menüsünden **Oluştur/İçe Aktar'ı** seçin.
        
        ![Oluştur/İçe'yi seç düğmesini seçin](./media/configure-customer-managed-key/select-generate-import.png)

    1. **Seçenekler** **Oluşturmak** ve anahtara bir ad vermek için ayarlama.

        ![Bir anahtar oluşturma](./media/configure-customer-managed-key/create-key.png) 

    1. Artık açılan listeden şifrelemek için Hizmet Veri Servisi ad alanıyla ilişkilendirmek için bu anahtarı seçebilirsiniz. 

        ![Anahtar kasasından anahtarı seçin](./media/configure-customer-managed-key/select-key-from-key-vault.png)
        > [!NOTE]
        > Fazlalık için en fazla 3 tuş ekleyebilirsiniz. Anahtarlardan birinin süresinin dolması veya erişilememesi durumunda, diğer anahtarlar şifreleme için kullanılır.
        
    1. Anahtarın ayrıntılarını doldurun ve **Seç'e**tıklayın. Bu, müşteri yönetilen bir anahtarla ad alanında veri şifrelemesini sağlar. 


    > [!IMPORTANT]
    > Geo felaket kurtarma ile birlikte Müşteri yönetilen anahtarı kullanmak için arıyorsanız, aşağıdaki gözden geçirin - 
    >
    > Müşteri yönetilen anahtarla şifrelemeyi etkinleştirmek için, belirtilen Azure KeyVault'ta Hizmet Veri Yolu'nun yönetilen kimliği için bir [erişim ilkesi](../key-vault/key-vault-secure-your-key-vault.md) ayarlanır. Bu, Azure Hizmet Veri Veri Tos ad alanından Azure KeyVault'a kontrollü erişim sağlar.
    >
    > Bu nedenle:
    > 
    >   * Hizmet Veri Yolu ad alanı için [Coğrafi durum kurtarma](service-bus-geo-dr.md) zaten etkinse ve müşteri yönetilen anahtarını etkinleştirmek istiyorsanız, 
    >     * Eşleştirmeyi boz
    >     * Anahtar kasasına hem birincil hem de ikincil ad alanları için yönetilen kimlik için [erişim ilkesini ayarlayın.](../key-vault/managed-identity.md)
    >     * Birincil ad alanında şifreleme ayarlayın.
    >     * Birincil ve ikincil ad alanlarını yeniden eşleştirin.
    > 
    >   * Müşteri yönetilen anahtarının zaten ayarlandığı bir Hizmet Veri Yolundan ad alanında Geo-DR'yi etkinleştirmek istiyorsanız,
    >     * Anahtar kasasına ikincil ad alanı için yönetilen kimlik için [erişim ilkesini ayarlayın.](../key-vault/managed-identity.md)
    >     * Birincil ve ikincil ad alanlarını eşleştirin.


## <a name="rotate-your-encryption-keys"></a>Şifreleme anahtarlarınızı döndürme

Azure Anahtar Kasaları döndürme mekanizmasını kullanarak anahtarınızı anahtar kasasında döndürebilirsiniz. Daha fazla bilgi için [bkz.](../key-vault/key-vault-key-rotation-log-monitoring.md) Etkinleştirme ve son kullanma tarihleri de anahtar döndürmeyi otomatikleştirmek için ayarlanabilir. Servis Veri Servisi yeni anahtar sürümlerialgılar ve otomatik olarak kullanmaya başlar.

## <a name="revoke-access-to-keys"></a>Anahtarlara erişimi iptal etme

Şifreleme anahtarlarına erişimi iptal etmek, verileri Servis Veri Servisi'nden temizlemez. Ancak, verilere Hizmet Veri Kurumu ad alanından erişilemez. Şifreleme anahtarını erişim ilkesi yle veya anahtarı silerek iptal edebilirsiniz. Erişim ilkeleri ve anahtar kasanızı Güvenli kasadan [anahtar kasasına](../key-vault/key-vault-secure-your-key-vault.md)güvenli hale alma hakkında daha fazla bilgi edinin.

Şifreleme anahtarı iptal edildikten sonra, şifrelenmiş ad alanındaki Servis Veri Servisi çalışamaz hale gelir. Anahtara erişim etkinleştirilmişse veya silinen anahtar geri yüklenirse, şifreli Servis Veri Servisi ad alanından verilere erişebilmeniz için Servis Veri Servisi anahtarı seçer.

## <a name="use-resource-manager-template-to-enable-encryption"></a>Şifrelemeyi etkinleştirmek için Kaynak Yöneticisi şablonunu kullanma
Bu bölümde, **Azure Kaynak Yöneticisi şablonlarını**kullanarak aşağıdaki görevlerin nasıl yapılacağını gösterilmektedir. 

1. Yönetilen hizmet **kimliğine**sahip **birinci sınıf** bir Hizmet Veri Yolu ad alanı oluşturun.
2. Anahtar **kasası** oluşturun ve servis kimliğine anahtar kasasına erişim hakkı tanıyın. 
3. Servis Veri Servisi ad alanını anahtar kasa bilgileriyle (anahtar/değer) güncelleştirin. 


### <a name="create-a-premium-service-bus-namespace-with-managed-service-identity"></a>Yönetilen hizmet kimliğiyle birinci sınıf hizmet veri yolu ad alanı oluşturun
Bu bölümde, Azure Kaynak Yöneticisi şablonu ve PowerShell kullanarak yönetilen hizmet kimliğine sahip bir Azure Hizmet Veri Yolu ad alanı nasıl oluşturulacaksınız gösterilmektedir. 

1. Yönetilen hizmet kimliğine sahip bir Hizmet Veri Yolu premium katman ad alanı oluşturmak için bir Azure Kaynak Yöneticisi şablonu oluşturun. Dosyaadı: **CreateServiceBusPremiumNamespace.json**: 

    ```json
    {
       "$schema":"https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion":"1.0.0.0",
       "parameters":{
          "namespaceName":{
             "type":"string",
             "metadata":{
                "description":"Name for the Namespace."
             }
          },
          "location":{
             "type":"string",
             "defaultValue":"[resourceGroup().location]",
             "metadata":{
                "description":"Specifies the Azure location for all resources."
             }
          }
       },
       "resources":[
          {
             "type":"Microsoft.ServiceBus/namespaces",
             "apiVersion":"2018-01-01-preview",
             "name":"[parameters('namespaceName')]",
             "location":"[parameters('location')]",
             "identity":{
                "type":"SystemAssigned"
             },
             "sku":{
                "name":"Premium",
                "tier":"Premium",
                "capacity":1
             },
             "properties":{
    
             }
          }
       ],
       "outputs":{
          "ServiceBusNamespaceId":{
             "type":"string",
             "value":"[resourceId('Microsoft.ServiceBus/namespaces',parameters('namespaceName'))]"
          }
       }
    }
    ```
2. Adlı bir şablon parametre dosyası **oluşturun: CreateServiceBusPremiumNamespaceParams.json**. 

    > [!NOTE]
    > Aşağıdaki değerleri değiştirin: 
    > - `<ServiceBusNamespaceName>`- Servis Veri Günü ad alanınızın adı
    > - `<Location>`- Servis Veri Günü ad alanınızın konumu

    ```json
    {
       "$schema":"https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion":"1.0.0.0",
       "parameters":{
          "namespaceName":{
             "value":"<ServiceBusNamespaceName>"
          },
          "location":{
             "value":"<Location>"
          }
       }
    }
    ```
3. Bir premium Hizmet Veri Merkezi ad alanı oluşturmak için şablonu dağıtmak için aşağıdaki PowerShell komutunu çalıştırın. Ardından, daha sonra kullanmak üzere Hizmet Veri Servisi ad alanının kimliğini alın. Komutu çalıştırmadan önce kaynak grubunun adı ile değiştirin. `{MyRG}`  

    ```powershell
    $outputs = New-AzResourceGroupDeployment -Name CreateServiceBusPremiumNamespace -ResourceGroupName {MyRG} -TemplateFile ./CreateServiceBusPremiumNamespace.json -TemplateParameterFile ./CreateServiceBusPremiumNamespaceParams.json
    
    $ServiceBusNamespaceId = $outputs.Outputs["serviceBusNamespaceId"].value
    ```
 
### <a name="grant-service-bus-namespace-identity-access-to-key-vault"></a>Grant Service Bus namespace kimlik erişimi anahtar kasa

1. **Temizleme koruması** ve **yumuşak silme** etkin leştirilmiş anahtar kasası oluşturmak için aşağıdaki komutu çalıştırın. 

    ```powershell
    New-AzureRmKeyVault -Name "{keyVaultName}" -ResourceGroupName {RGName}  -Location "{location}" -EnableSoftDelete -EnablePurgeProtection    
    ```
    
    (VEYA)
    
    Varolan bir anahtar **kasasını**güncelleştirmek için aşağıdaki komutu çalıştırın. Komutu çalıştırmadan önce kaynak grubu ve anahtar kasa adları için değerleri belirtin. 
    
    ```powershell
    ($updatedKeyVault = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -ResourceGroupName {RGName} -VaultName {keyVaultName}).ResourceId).Properties| Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"-Force | Add-Member -MemberType "NoteProperty" -Name "enablePurgeProtection" -Value "true" -Force
    ``` 
2. Hizmet Veri Yolu ad alanının yönetilen kimliğinin anahtar kasasındaki anahtar değerine erişebileceği şekilde anahtar kasa erişim ilkesini ayarlayın. Önceki bölümdeki Hizmet Veri Servisi ad alanının kimliğini kullanın. 

    ```powershell
    $identity = (Get-AzureRmResource -ResourceId $ServiceBusNamespaceId -ExpandProperties).Identity
    
    Set-AzureRmKeyVaultAccessPolicy -VaultName {keyVaultName} -ResourceGroupName {RGName} -ObjectId $identity.PrincipalId -PermissionsToKeys get,wrapKey,unwrapKey,list
    ```

### <a name="encrypt-data-in-service-bus-namespace-with-customer-managed-key-from-key-vault"></a>Servis Veri Servisi ad alanında ki verileri anahtar kasasından müşteri tarafından yönetilen anahtarla şifreleme
Şimdiye kadar aşağıdaki adımları yaptınız: 

1. Yönetilen bir kimliğe sahip bir premium ad alanı oluşturdu.
2. Anahtar kasası oluşturun ve anahtar kasasına yönetilen kimlik erişimi ne zaman verildi. 

Bu adımda, Servis Veri Servisi ad alanını anahtar kasa bilgileriyle güncelleştireceksiniz. 

1. Aşağıdaki içeriği içeren **UpdateServiceBusNamespaceWithEncryption.json** adlı bir JSON dosyası oluşturun: 

    ```json
    {
       "$schema":"https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion":"1.0.0.0",
       "parameters":{
          "namespaceName":{
             "type":"string",
             "metadata":{
                "description":"Name for the Namespace to be created in cluster."
             }
          },
          "location":{
             "type":"string",
             "defaultValue":"[resourceGroup().location]",
             "metadata":{
                "description":"Specifies the Azure location for all resources."
             }
          },
          "keyVaultUri":{
             "type":"string",
             "metadata":{
                "description":"URI of the KeyVault."
             }
          },
          "keyName":{
             "type":"string",
             "metadata":{
                "description":"KeyName."
             }
          }
       },
       "resources":[
          {
             "type":"Microsoft.ServiceBus/namespaces",
             "apiVersion":"2018-01-01-preview",
             "name":"[parameters('namespaceName')]",
             "location":"[parameters('location')]",
             "identity":{
                "type":"SystemAssigned"
             },
             "sku":{
                "name":"Premium",
                "tier":"Premium",
                "capacity":1
             },
             "properties":{
                "encryption":{
                   "keySource":"Microsoft.KeyVault",
                   "keyVaultProperties":[
                      {
                         "keyName":"[parameters('keyName')]",
                         "keyVaultUri":"[parameters('keyVaultUri')]"
                      }
                   ]
                }
             }
          }
       ]
    }
    ``` 

2. Şablon parametre dosyası oluşturun: **UpdateServiceBusNamespaceWithEncryptionParams.json**.

    > [!NOTE]
    > Aşağıdaki değerleri değiştirin: 
    > - `<ServiceBusNamespaceName>`- Servis Veri Günü ad alanınızın adı
    > - `<Location>`- Servis Veri Günü ad alanınızın konumu
    > - `<KeyVaultName>`- Anahtar kasanızın adı
    > - `<KeyName>`- Anahtar kasasındaki anahtarın adı  

    ```json
    {
       "$schema":"https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion":"1.0.0.0",
       "parameters":{
          "namespaceName":{
             "value":"<ServiceBusNamespaceName>"
          },
          "location":{
             "value":"<Location>"
          },
          "keyName":{
             "value":"<KeyName>"
          },
          "keyVaultUri":{
             "value":"https://<KeyVaultName>.vault.azure.net"
          }
       }
    }
    ```             
3. Kaynak Yöneticisi şablonunu dağıtmak için aşağıdaki PowerShell komutunu çalıştırın. Komutu çalıştırmadan önce kaynak grubunuzun adı ile değiştirin. `{MyRG}` 

    ```powershell
    New-AzResourceGroupDeployment -Name UpdateServiceBusNamespaceWithEncryption -ResourceGroupName {MyRG} -TemplateFile ./UpdateServiceBusNamespaceWithEncryption.json -TemplateParameterFile ./UpdateServiceBusNamespaceWithEncryptionParams.json
    ```
    

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:
- [Servis Veri Servisi'ne genel bakış](service-bus-messaging-overview.md)
- [Key Vault'a genel bakış](../key-vault/key-vault-overview.md)


