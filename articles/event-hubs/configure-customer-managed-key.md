---
title: Azure Etkinlik Hub'ları verilerini şifrelemek için kendi anahtarınızı yapılandırma
description: Bu makalede, Azure Olay Hub'ları veri geri kalanı şifrelemek için kendi anahtarınızı yapılandırma hakkında bilgi verilmektedir.
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: spelluru
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: spelluru
ms.openlocfilehash: f515d3ad832db7f78f98111ab67628a2874033ff
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81459143"
---
# <a name="configure-customer-managed-keys-for-encrypting-azure-event-hubs-data-at-rest-by-using-the-azure-portal"></a>Azure portalını kullanarak Azure Event Hub'ları verilerini yeniden şifrelemek için müşteri tarafından yönetilen anahtarları yapılandırın
Azure Etkinlik Hub'ları, Azure Depolama Hizmeti Şifrelemesi (Azure SSE) ile veri şifrelemesini sağlar. Olay Hub'ları verileri depolamak için Azure Depolama'ya güvenir ve varsayılan olarak Azure Depolama ile depolanan tüm veriler Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. 

## <a name="overview"></a>Genel Bakış
Azure Etkinlik Hub'ları artık verileri Microsoft tarafından yönetilen anahtarlarla veya müşteri tarafından yönetilen anahtarlarla (Kendi Anahtarınızı Getir – BYOK) ile birlikte şifreleme seçeneğini destekler. Bu özellik, Azure Etkinlik Hub'ları verilerini kolaylaştırmak için kullanılan müşteri tarafından yönetilen anahtarlara erişimi oluşturmanıza, döndürmenize, devre dışı etmenizi ve iptal etmenizi sağlar.

BYOK özelliğini etkinleştirmek, ad alanınızda tek seferlik bir kurulum işlemidir.

> [!NOTE]
> BYOK özelliği, tek [kiracılı kümeler için özel Etkinlik Hub'ları](event-hubs-dedicated-overview.md) tarafından desteklenir. Standart Olay Hub'ları ad alanları için etkinleştirilenemez.

Anahtarlarınızı yönetmek ve anahtar kullanımınızı denetlemek için Azure Key Vault'u kullanabilirsiniz. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya anahtar oluşturmak için Azure Key Vault API'lerini kullanabilirsiniz. Azure Anahtar Kasası hakkında daha fazla bilgi için Azure [Anahtar Kasası nedir?](../key-vault/general/overview.md)

Bu makalede, Azure portalını kullanarak müşteri tarafından yönetilen anahtarlarla anahtar kasasının nasıl yapılandırılabildiğini gösterilmektedir. Azure portalını kullanarak önemli bir kasa oluşturmayı öğrenmek için [Quickstart: Azure portalını kullanarak Azure Key Vault'tan bir sır ayarlayın ve alın.](../key-vault/secrets/quick-create-portal.md)

> [!IMPORTANT]
> Azure Etkinlik Hub'ları ile müşteri tarafından yönetilen anahtarları kullanmak, anahtar kasasının iki gerekli özellisini yapılandırmasını gerektirir. Bunlar: **Yumuşak Silme** ve **Temizleme etmeyin**. Azure portalında yeni bir anahtar kasası oluşturduğunuzda, bu özellikler varsayılan olarak etkinleştirilir. Ancak, bu özellikleri varolan bir anahtar kasasında etkinleştirmeniz gerekiyorsa, PowerShell veya Azure CLI'yi kullanmanız gerekir.

## <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları etkinleştirme
Azure portalında müşteri tarafından yönetilen anahtarları etkinleştirmek için aşağıdaki adımları izleyin:

1. Etkinlik Hub'larınıza Özel kümenize gidin.
1. BYOK'u etkinleştirmek istediğiniz ad alanını seçin.
1. Olay Hub'larınızın ad alanının **Ayarlar** sayfasında **Şifreleme'yi**seçin. 
1. Aşağıdaki resimde gösterildiği **gibi, Müşteri tarafından yönetilen anahtar şifrelemesini istirahatte** seçin. 

    ![Müşteri yönetilen anahtarını etkinleştirme](./media/configure-customer-managed-key/enable-customer-managed-key.png)

## <a name="set-up-a-key-vault-with-keys"></a>Anahtarlarla anahtar kasası ayarlama
Müşteri tarafından yönetilen anahtarları etkinleştirdikten sonra, müşteri yönetilen anahtarını Azure Etkinlik Hub'larınız ad alanınızla ilişkilendirmeniz gerekir. Etkinlik Hub'ları yalnızca Azure Anahtar Kasası'nı destekler. Önceki bölümde **müşteri tarafından yönetilen anahtar seçeneğiyle Şifreleme'yi** etkinleştiriseniz, anahtarın Azure Anahtar Kasası'na aktarılmasını sağlamanız gerekir. Ayrıca, anahtarlar **Yumuşak Silme** ve **Temizleme anahtarı** için yapılandırılan etmeyin olmalıdır. Bu ayarlar [PowerShell](../key-vault/general/soft-delete-powershell.md) veya [CLI](../key-vault/general/soft-delete-cli.md#enabling-purge-protection)kullanılarak yapılandırılabilir.

1. Yeni bir anahtar kasası oluşturmak için Azure Key Vault [Quickstart'ı](../key-vault/general/overview.md)izleyin. Varolan anahtarları alma hakkında daha fazla bilgi için [anahtarlar, sırlar ve sertifikalar hakkında](../key-vault/about-keys-secrets-and-certificates.md)bilgi edinebilirsiniz.
1. Kasa oluştururken hem yumuşak silme hem de temizleme korumasını açmak için [az keyvault oluşturma](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) komutunu kullanın.

    ```azurecli-interactive
    az keyvault create --name ContosoVault --resource-group ContosoRG --location westus --enable-soft-delete true --enable-purge-protection true
    ```    
1. Varolan bir kasaya temizleme koruması eklemek için (zaten yumuşak silme etkinleştirilmiş), [az keyvault güncelleştirme](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update) komutunu kullanın.

    ```azurecli-interactive
    az keyvault update --name ContosoVault --resource-group ContosoRG --enable-purge-protection true
    ```
1. Aşağıdaki adımları izleyerek anahtarlar oluşturun:
    1. Yeni bir anahtar oluşturmak için **Ayarlar**menüsünden **Ayarlar** menüsünden **Oluştur/İçe Aktar'ı** seçin.
        
        ![Oluştur/İçe'yi seç düğmesini seçin](./media/configure-customer-managed-key/select-generate-import.png)
    1. **Seçenekler** **Oluşturmak** ve anahtara bir ad vermek için ayarlama.

        ![Bir anahtar oluşturma](./media/configure-customer-managed-key/create-key.png) 
    1. Artık açılan listeden şifrelemek için Olay Hub'ları ad alanıyla ilişkilendirmek için bu anahtarı seçebilirsiniz. 

        ![Anahtar kasasından anahtarı seçin](./media/configure-customer-managed-key/select-key-from-key-vault.png)
    1. Anahtarın ayrıntılarını doldurun ve **Seç'e**tıklayın. Bu, müşteri yönetilen bir anahtarla ad alanında veri şifrelemesini sağlar. 


## <a name="rotate-your-encryption-keys"></a>Şifreleme anahtarlarınızı döndürme
Azure Anahtar Kasaları döndürme mekanizmasını kullanarak anahtarınızı anahtar kasasında döndürebilirsiniz. Daha fazla bilgi için [bkz.](../key-vault/secrets/key-rotation-log-monitoring.md) Etkinleştirme ve son kullanma tarihleri de anahtar döndürmeyi otomatikleştirmek için ayarlanabilir. Olay Hub'ları hizmeti yeni anahtar sürümlerini algılar ve otomatik olarak kullanmaya başlar.

## <a name="revoke-access-to-keys"></a>Anahtarlara erişimi iptal etme
Şifreleme anahtarlarına erişimi iptal etmek, verileri Olay Hub'larından temizlemez. Ancak, verilere Olay Hub'ları ad alanından erişilemez. Şifreleme anahtarını erişim ilkesi yle veya anahtarı silerek iptal edebilirsiniz. Erişim ilkeleri ve anahtar kasanızı Güvenli kasadan [anahtar kasasına](../key-vault/general/secure-your-key-vault.md)güvenli hale alma hakkında daha fazla bilgi edinin.

Şifreleme anahtarı iptal edildikten sonra, şifrelenmiş ad alanındaki Olay Hub'ları hizmeti çalışamaz hale gelir. Anahtara erişim etkinleştirilmişse veya silme anahtarı geri yüklenirse, şifreli Olay Hub'ları ad alanından verilere erişebilmeniz için Olay Hub'ları hizmeti anahtarı seçer.

## <a name="set-up-diagnostic-logs"></a>Tanılama günlükleri ayarlama 
BYOK etkin ad alanları için tanılama günlükleri ayarlama, bir ad alanı müşteri tarafından yönetilen anahtarlarla şifrelendiğinde işlemler hakkında gerekli bilgileri verir. Bu günlükler etkinleştirilebilir ve daha sonra bir olay hub'ına akabilir veya günlük analitiği aracılığıyla analiz edilebilir veya özelleştirilmiş analitik gerçekleştirmek için depolama alanına aktarılabilir. Tanılama günlükleri hakkında daha fazla bilgi edinmek için Azure [Tanı günlüklerine genel bakış](../azure-monitor/platform/platform-logs-overview.md)bölümüne bakın.

## <a name="enable-user-logs"></a>Kullanıcı günlüklerini etkinleştirme
Müşteri tarafından yönetilen anahtarlar için günlükleri etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure portalında, BYOK etkinleştirilmiş ad alanına gidin.
1. **İzleme**altında **Tanılama ayarlarını** seçin.

    ![Tanılama ayarlarını seçin](./media/configure-customer-managed-key/select-diagnostic-settings.png)
1. **+Tanı ayarını ekle'yi**seçin. 

    ![Tanılama ayarı ekle'yi seçin](./media/configure-customer-managed-key/select-add-diagnostic-setting.png)
1. Bir **ad** sağlayın ve günlükleri nereye aktarmak istediğinizi seçin.
1. **CustomerManagedKeyUserLogs'u** seçin ve **Kaydedin.** Bu eylem, ad alanında BYOK günlükleri sağlar.

    ![Müşteri tarafından yönetilen anahtar kullanıcı günlükleri seçeneğini seçin](./media/configure-customer-managed-key/select-customer-managed-key-user-logs.png)

## <a name="log-schema"></a>Günlük şema 
Tüm günlükler JavaScript Nesne Gösterimi (JSON) biçiminde depolanır. Her giriş, aşağıdaki tabloda açıklanan biçimi kullanan dize alanları vardır. 

| Adı | Açıklama |
| ---- | ----------- | 
| Görevadı | Başarısız olan görevin açıklaması. |
| Activityıd | İzleme için kullanılan dahili kimlik. |
| category | Görevin sınıflandırını tanımlar. Örneğin, anahtar kasanızdaki anahtar devre dışı ediliyorsa, bu bir bilgi kategorisi olur veya bir anahtar paketten çıkarılamazsa hata altında kalabilir. |
| resourceId | Azure Kaynak Yöneticisi kaynak kimliği |
| keyVault | Anahtar kasasının tam adı. |
| anahtar | Olay Hub'ları ad alanını şifrelemek için kullanılan anahtar adı. |
| version | Kullanılan anahtarın sürümü. |
| Işlem | Anahtar kasanızdaki anahtara yapılan işlem. Örneğin, anahtarı, kaydırmayı veya açma |
| kod | İşlemle ilişkili kod. Örnek: Hata kodu, 404 bu anahtarın bulunamadı anlamına gelir. |
| message | İşlemle ilişkili herhangi bir hata iletisi |

Müşteri yönetilen bir anahtar için günlük örneği aşağıda verilmiştir:

```json
{
   "TaskName": "CustomerManagedKeyUserLog",
   "ActivityId": "11111111-1111-1111-1111-111111111111",
   "category": "error"
   "resourceId": "/SUBSCRIPTIONS/11111111-1111-1111-1111-11111111111/RESOURCEGROUPS/DEFAULT-EVENTHUB-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/FBETTATI-OPERA-EVENTHUB",
   "keyVault": "https://mykeyvault.vault-int.azure-int.net",
   "key": "mykey",
   "version": "1111111111111111111111111111111",
   "operation": "wrapKey",
   "code": "404",
   "message": "Key not found: ehbyok0/111111111111111111111111111111",
}



{
   "TaskName": "CustomerManagedKeyUserLog",
   "ActivityId": "11111111111111-1111-1111-1111111111111",
   "category": "info"
   "resourceId": "/SUBSCRIPTIONS/111111111-1111-1111-1111-11111111111/RESOURCEGROUPS/DEFAULT-EVENTHUB-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/FBETTATI-OPERA-EVENTHUB",
   "keyVault": "https://mykeyvault.vault-int.azure-int.net",
   "key": "mykey",
   "version": "111111111111111111111111111111",
   "operation": "disable" | "restore",
   "code": "",
   "message": "",
}
```

## <a name="use-resource-manager-template-to-enable-encryption"></a>Şifrelemeyi etkinleştirmek için Kaynak Yöneticisi şablonunu kullanma
Bu bölümde, **Azure Kaynak Yöneticisi şablonlarını**kullanarak aşağıdaki görevlerin nasıl yapılacağını gösterilmektedir. 

1. Yönetilen hizmet kimliğine sahip bir **Olay Hub'ları ad alanı** oluşturun.
2. Anahtar **kasası** oluşturun ve servis kimliğine anahtar kasasına erişim hakkı tanıyın. 
3. Olay Hub'ları ad alanını anahtar kasa bilgileriyle (anahtar/değer) güncelleştirin. 


### <a name="create-an-event-hubs-cluster-and-namespace-with-managed-service-identity"></a>Yönetilen hizmet kimliğiyle Olay Hub'ları kümesi ve ad alanı oluşturma
Bu bölümde, Azure Kaynak Yöneticisi şablonu ve PowerShell kullanarak yönetilen hizmet kimliğine sahip bir Azure Etkinlik Hub'ları ad alanı nasıl oluşturulacaksınız gösterilmektedir. 

1. Yönetilen hizmet kimliğine sahip bir Olay Hub'ları ad alanı oluşturmak için Azure Kaynak Yöneticisi şablonu oluşturun. Dosyayı adlandırın: **CreateEventHubClusterAndNamespace.json**: 

    ```json
    {
       "$schema":"https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion":"1.0.0.0",
       "parameters":{
          "clusterName":{
             "type":"string",
             "metadata":{
                "description":"Name for the Event Hub cluster."
             }
          },
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
          }
       },
       "resources":[
          {
             "type":"Microsoft.EventHub/clusters",
             "apiVersion":"2018-01-01-preview",
             "name":"[parameters('clusterName')]",
             "location":"[parameters('location')]",
             "sku":{
                "name":"Dedicated",
                "capacity":1
             }
          },
          {
             "type":"Microsoft.EventHub/namespaces",
             "apiVersion":"2018-01-01-preview",
             "name":"[parameters('namespaceName')]",
             "location":"[parameters('location')]",
             "identity":{
                "type":"SystemAssigned"
             },
             "sku":{
                "name":"Standard",
                "tier":"Standard",
                "capacity":1
             },
             "properties":{
                "isAutoInflateEnabled":false,
                "maximumThroughputUnits":0,
                "clusterArmId":"[resourceId('Microsoft.EventHub/clusters', parameters('clusterName'))]"
             },
             "dependsOn":[
                "[resourceId('Microsoft.EventHub/clusters', parameters('clusterName'))]"
             ]
          }
       ],
       "outputs":{
          "EventHubNamespaceId":{
             "type":"string",
             "value":"[resourceId('Microsoft.EventHub/namespaces',parameters('namespaceName'))]"
          }
       }
    }
    ```
2. Adlı bir şablon parametre dosyası oluşturun: **CreateEventHubClusterAndNamespaceParams.json**. 

    > [!NOTE]
    > Aşağıdaki değerleri değiştirin: 
    > - `<EventHubsClusterName>`- Olay Hub'larınızın adı    
    > - `<EventHubsNamespaceName>`- Etkinlik Hub'larınızın ad alanının adı
    > - `<Location>`- Etkinlik Hub'larınızın ad alanının konumu

    ```json
    {
       "$schema":"https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion":"1.0.0.0",
       "parameters":{
          "clusterName":{
             "value":"<EventHubsClusterName>"
          },
          "namespaceName":{
             "value":"<EventHubsNamespaceName>"
          },
          "location":{
             "value":"<Location>"
          }
       }
    }
    
    ```
3. Olay Hub'ları ad alanı oluşturmak için şablonu dağıtmak için aşağıdaki PowerShell komutunu çalıştırın. Ardından, daha sonra kullanmak üzere Olay Hub'larının ad alanının kimliğini alın. Komutu çalıştırmadan önce kaynak grubunun adı ile değiştirin. `{MyRG}`  

    ```powershell
    $outputs = New-AzResourceGroupDeployment -Name CreateEventHubClusterAndNamespace -ResourceGroupName {MyRG} -TemplateFile ./CreateEventHubClusterAndNamespace.json -TemplateParameterFile ./CreateEventHubClusterAndNamespaceParams.json

    $EventHubNamespaceId = $outputs.Outputs["eventHubNamespaceId"].value
    ```
 
### <a name="grant-event-hubs-namespace-identity-access-to-key-vault"></a>Grant Event Hubs ad alanı kimlik erişimi anahtar kasa

1. **Temizleme koruması** ve **yumuşak silme** etkin leştirilmiş anahtar kasası oluşturmak için aşağıdaki komutu çalıştırın. 

    ```powershell
    New-AzureRmKeyVault -Name {keyVaultName} -ResourceGroupName {RGName}  -Location {location} -EnableSoftDelete -EnablePurgeProtection    
    ```     
    
    (VEYA)    
    
    Varolan bir anahtar **kasasını**güncelleştirmek için aşağıdaki komutu çalıştırın. Komutu çalıştırmadan önce kaynak grubu ve anahtar kasa adları için değerleri belirtin. 
    
    ```powershell
    ($updatedKeyVault = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -ResourceGroupName {RGName} -VaultName {keyVaultName}).ResourceId).Properties| Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"-Force | Add-Member -MemberType "NoteProperty" -Name "enablePurgeProtection" -Value "true" -Force
    ``` 
2. Olay Hub'larının yönetilen kimliğinin anahtar kasasındaki anahtar değerine erişebileceği şekilde anahtar kasa erişim ilkesini ayarlayın. Önceki bölümdeki Olay Hub'ları ad alanının kimliğini kullanın. 

    ```powershell
    $identity = (Get-AzureRmResource -ResourceId $EventHubNamespaceId -ExpandProperties).Identity
    
    Set-AzureRmKeyVaultAccessPolicy -VaultName {keyVaultName} -ResourceGroupName {RGName} -ObjectId $identity.PrincipalId -PermissionsToKeys get,wrapKey,unwrapKey,list
    ```

### <a name="encrypt-data-in-event-hubs-namespace-with-customer-managed-key-from-key-vault"></a>Olay Hub'ları ad alanında ki verileri anahtar kasasından müşteri tarafından yönetilen anahtarla şifreleme
Şimdiye kadar aşağıdaki adımları yaptınız: 

1. Yönetilen bir kimliğe sahip bir premium ad alanı oluşturdu.
2. Anahtar kasası oluşturun ve anahtar kasasına yönetilen kimlik erişimi ne zaman verildi. 

Bu adımda, Olay Hub'ları ad alanını anahtar kasa bilgileriyle güncelleştireceksiniz. 

1. **CreateEventHubClusterAndNamespace.json** adlı bir JSON dosyası oluşturun: 

    ```json
    {
       "$schema":"https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion":"1.0.0.0",
       "parameters":{
          "clusterName":{
             "type":"string",
             "metadata":{
                "description":"Name for the Event Hub cluster."
             }
          },
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
             "type":"Microsoft.EventHub/namespaces",
             "apiVersion":"2018-01-01-preview",
             "name":"[parameters('namespaceName')]",
             "location":"[parameters('location')]",
             "identity":{
                "type":"SystemAssigned"
             },
             "sku":{
                "name":"Standard",
                "tier":"Standard",
                "capacity":1
             },
             "properties":{
                "isAutoInflateEnabled":false,
                "maximumThroughputUnits":0,
                "clusterArmId":"[resourceId('Microsoft.EventHub/clusters', parameters('clusterName'))]",
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

2. Şablon parametre dosyası oluşturun: **UpdateEventHubClusterAndNamespaceParams.json**. 

    > [!NOTE]
    > Aşağıdaki değerleri değiştirin: 
    > - `<EventHubsClusterName>`- Olay Hub'larınızın adı.        
    > - `<EventHubsNamespaceName>`- Etkinlik Hub'larınızın ad alanının adı
    > - `<Location>`- Etkinlik Hub'larınızın ad alanının konumu
    > - `<KeyVaultName>`- Anahtar kasanızın adı
    > - `<KeyName>`- Anahtar kasasındaki anahtarın adı

    ```json
    {
       "$schema":"https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion":"1.0.0.0",
       "parameters":{
          "clusterName":{
             "value":"<EventHubsClusterName>"
          },
          "namespaceName":{
             "value":"<EventHubsNamespaceName>"
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
    New-AzResourceGroupDeployment -Name UpdateEventHubNamespaceWithEncryption -ResourceGroupName {MyRG} -TemplateFile ./UpdateEventHubClusterAndNamespace.json -TemplateParameterFile ./UpdateEventHubClusterAndNamespaceParams.json 
    ```

## <a name="troubleshoot"></a>Sorun giderme
En iyi uygulama olarak, her zaman önceki bölümde gösterildiği gibi günlükleri etkinleştirin. BYOK şifrelemesi etkinleştirildiğinde etkinlikleri izlemede yardımcı olur. Aynı zamanda sorunları aşağı inme de yardımcı olur.

BYOK şifrelemesi etkinleştirildiğinde aranacak yaygın hata kodları aşağıda verilmiştir.

| Eylem | Hata kodu | Ortaya çıkan veri durumu |
| ------ | ---------- | ----------------------- | 
| Anahtar kasasından kaydırma/açma iznini kaldırma | 403 |    Erişile -mez |
| Kaydırma/açma izni veren bir AAD müdüründen AAD rol üyeliğini kaldırma | 403 |  Erişile -mez |
| Anahtar kasasından şifreleme anahtarını silme | 404 | Erişile -mez |
| Anahtar kasasını silme | 404 | Erişilemez (gerekli ayar olan yumuşak silmenin etkin olduğunu varsayar.) |
| Şifreleme anahtarının son kullanma süresini, süresi nin dolması gibi değiştirme | 403 |   Erişile -mez  |
| Anahtar şifreleme anahtarının etkin olmaması gibi NBF'yi (daha önce değil) değiştirme | 403 | Erişile -mez  |
| Anahtar kasa güvenlik duvarı için **MSFT Hizmetlerine İzin Ver** seçeneğini seçmek veya şifreleme anahtarına sahip anahtar kasasına ağ erişimini engellemek | 403 | Erişile -mez |
| Anahtar kasasını farklı bir kiracıya taşıma | 404 | Erişile -mez |  
| Aralıklı ağ sorunu veya DNS/AAD/MSI kesintisi |  | Önbelleğe alınmış veri şifreleme anahtarı kullanılarak erişilebilir |

> [!IMPORTANT]
> BYOK şifrelemesini kullanan bir ad alanında Geo-DR'yi etkinleştirmek için, eşleştirme için ikincil ad alanının özel bir kümede olması ve üzerinde yönetilen kimlik atanmış bir sistem olması gerekir. Daha fazla bilgi için Azure [Kaynakları için Yönetilen Kimlikler'e](../active-directory/managed-identities-azure-resources/overview.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:
- [Olay Hub'larına genel bakış](event-hubs-about.md)
- [Key Vault'a genel bakış](../key-vault/general/overview.md)




