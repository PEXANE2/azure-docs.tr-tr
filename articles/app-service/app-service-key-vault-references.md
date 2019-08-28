---
title: Key Vault başvuruları-Azure App Service | Microsoft Docs
description: Azure App Service ve Azure Işlevlerinde Azure Key Vault başvuruları için kavramsal başvuru ve Kurulum Kılavuzu
services: app-service
author: mattchenderson
manager: jeconnoc
editor: ''
ms.service: app-service
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/20/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: 30bd7c68ae1c88aba288b515d0ec32581f90b868
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70088178"
---
# <a name="use-key-vault-references-for-app-service-and-azure-functions-preview"></a>App Service ve Azure Işlevleri için Key Vault başvurularını kullanma (Önizleme)

> [!NOTE] 
> Key Vault başvurular Şu anda önizleme aşamasındadır.

Bu konu başlığı altında, herhangi bir kod değişikliğine gerek kalmadan App Service veya Azure Işlevleri uygulamanızda Azure Key Vault gizliliklerle nasıl çalışılacağı gösterilmektedir. [Azure Key Vault](../key-vault/key-vault-overview.md) , erişim ilkeleri ve denetim geçmişi üzerinde tam denetim ile merkezi gizli dizi yönetimi sağlayan bir hizmettir.

## <a name="granting-your-app-access-to-key-vault"></a>Uygulamanıza Key Vault erişim verme

Key Vault parolaları okumak için bir kasasının oluşturulmuş olması ve uygulamanıza erişim izni vermeniz gerekir.

1. [Key Vault hızlı](../key-vault/quick-create-cli.md)başlangıcı ' nı izleyerek bir Anahtar Kasası oluşturun.

1. Uygulamanız için [sistem tarafından atanan bir yönetilen kimlik](overview-managed-identity.md) oluşturun.

   > [!NOTE] 
   > Key Vault başvurular Şu anda yalnızca sistem tarafından atanan yönetilen kimlikleri destekler. Kullanıcı tarafından atanan kimlikler kullanılamaz.

1. Daha önce oluşturduğunuz uygulama kimliği için [Key Vault bir erişim ilkesi](../key-vault/key-vault-secure-your-key-vault.md#key-vault-access-policies) oluşturun. Bu ilkede "Get" gizli anahtarını etkinleştirin. "Yetkilendirilmiş uygulama" veya `applicationId` ayarları, yönetilen bir kimlikle uyumlu olmadığından yapılandırmayın.

    Anahtar kasasındaki bir uygulama kimliğine erişim verilmesi bir kerelik işlemidir ve tüm Azure abonelikleri için aynı kalacaktır. Bunu istediğiniz sayıda sertifika dağıtmak için kullanabilirsiniz. 

## <a name="reference-syntax"></a>Başvuru sözdizimi

Key Vault bir başvuru, aşağıdaki seçeneklerden biri `@Microsoft.KeyVault({referenceString})`tarafından değiştirildiği `{referenceString}` biçimdedir:

> [!div class="mx-tdBreakAll"]
> | Başvuru dizesi                                                            | Açıklama                                                                                                                                                                                 |
> |-----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | SecretUri =_Secreturi_                                                       | **Secreturi** , bir sürüm dahil olmak üzere Key Vault bir parolanın tam veri düzlemi URI 'si olmalıdır, örneğin, https://myvault.vault.azure.net/secrets/mysecret/ec96f02080254f109c51a1f14cdb1931  |
> | VaultName =_vaultname_; SecretName =_secretname_; SecretVersion =_Secretversion_ | **Vaultname** Key Vault kaynağınızın adı olmalıdır. **Secretname** , hedef parolanın adı olmalıdır. **Secretversion** , kullanılacak gizli dizinin sürümü olmalıdır. |

> [!NOTE] 
> Geçerli önizlemede sürümler gereklidir. Gizli dizileri döndürürken, uygulama yapılandırmanızda sürümü güncelleştirmeniz gerekir.

Örneğin, bir bütün başvuru aşağıdaki gibi görünür:

```
@Microsoft.KeyVault(SecretUri=https://myvault.vault.azure.net/secrets/mysecret/ec96f02080254f109c51a1f14cdb1931)
```

Alternatif olarak:

```
@Microsoft.KeyVault(VaultName=myvault;SecretName=mysecret;SecretVersion=ec96f02080254f109c51a1f14cdb1931)
```


## <a name="source-application-settings-from-key-vault"></a>Key Vault kaynak uygulama ayarları

Key Vault başvuruları, [uygulama ayarları](configure-common.md#configure-app-settings)için değer olarak kullanılabilir, böylece gizli dizileri site yapılandırması yerine Key Vault tutabilirsiniz. Uygulama ayarları, bekleyen olarak güvenli bir şekilde şifrelenir, ancak gizli yönetim özelliklerine ihtiyacınız varsa, Key Vault gitmeleri gerekir.

Bir uygulama ayarı için Key Vault başvurusu kullanmak için, başvuruyu ayarın değeri olarak ayarlayın. Uygulamanız gizli dizi anahtarı aracılığıyla gizli dizi başvurusu yapabilir. Kod değişikliği gerekli değildir.

> [!TIP]
> Key Vault başvurularını kullanan uygulama ayarlarının çoğu, her ortam için ayrı kasaların olması gerektiği için yuva ayarları olarak işaretlenmelidir.

### <a name="azure-resource-manager-deployment"></a>Azure Resource Manager dağıtımı

Kaynak dağıtımlarını Azure Resource Manager şablonları aracılığıyla otomatikleştirmede, bu özelliğin çalışmasını sağlamak için bağımlılıklarınızı belirli bir sırada sıraya almanız gerekebilir. , Site tanımında bir `siteConfig` özellik kullanmak yerine uygulama ayarlarınızı kendi kaynakları olarak tanımlamanız gerekecektir. Bunun nedeni, sitenin, sistem tarafından atanan kimliğin birlikte oluşturulması ve erişim ilkesinde kullanılabilmesi için önce tanımlanması gerekir.

Bir işlev uygulaması için örnek bir psuedo şablonu, aşağıdaki gibi görünebilir:

```json
{
    //...
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            //...
        },
        {
            "type": "Microsoft.Insights/components",
            "name": "[variables('appInsightsName')]",
            //...
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[variables('functionAppName')]",
            "identity": {
                "type": "SystemAssigned"
            },
            //...
            "resources": [
                {
                    "type": "config",
                    "name": "appsettings",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/secrets', variables('keyVaultName'), variables('storageConnectionStringName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/secrets', variables('keyVaultName'), variables('appInsightsKeyName'))]"
                    ],
                    "properties": {
                        "AzureWebJobsStorage": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('storageConnectionStringResourceId')).secretUriWithVersion, ')')]",
                        "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('storageConnectionStringResourceId')).secretUriWithVersion, ')')]",
                        "APPINSIGHTS_INSTRUMENTATIONKEY": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('appInsightsKeyResourceId')).secretUriWithVersion, ')')]",
                        "WEBSITE_ENABLE_SYNC_UPDATE_SITE": "true"
                        //...
                    }
                },
                {
                    "type": "sourcecontrols",
                    "name": "web",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]",
                        "[resourceId('Microsoft.Web/sites/config', variables('functionAppName'), 'appsettings')]"
                    ],
                }
            ]
        },
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[variables('keyVaultName')]",
            //...
            "dependsOn": [
                "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]"
            ],
            "properties": {
                //...
                "accessPolicies": [
                    {
                        "tenantId": "[reference(concat('Microsoft.Web/sites/',  variables('functionAppName'), '/providers/Microsoft.ManagedIdentity/Identities/default'), '2015-08-31-PREVIEW').tenantId]",
                        "objectId": "[reference(concat('Microsoft.Web/sites/',  variables('functionAppName'), '/providers/Microsoft.ManagedIdentity/Identities/default'), '2015-08-31-PREVIEW').principalId]",
                        "permissions": {
                            "secrets": [ "get" ]
                        }
                    }
                ]
            },
            "resources": [
                {
                    "type": "secrets",
                    "name": "[variables('storageConnectionStringName')]",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
                    ],
                    "properties": {
                        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountResourceId'),'2015-05-01-preview').key1)]"
                    }
                },
                {
                    "type": "secrets",
                    "name": "[variables('appInsightsKeyName')]",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.Insights/components', variables('appInsightsName'))]"
                    ],
                    "properties": {
                        "value": "[reference(resourceId('microsoft.insights/components/', variables('appInsightsName')), '2015-05-01').InstrumentationKey]"
                    }
                }
            ]
        }
    ]
}
```

> [!NOTE] 
> Bu örnekte, kaynak denetimi dağıtımı uygulama ayarlarına bağlıdır. Uygulama ayarı güncelleştirmesi zaman uyumsuz olarak davrandığı için bu durum normalde güvenli olmayan bir davranıştır. Ancak, `WEBSITE_ENABLE_SYNC_UPDATE_SITE` uygulama ayarını dahil ettiğimiz için güncelleştirme zaman uyumludur. Bu, kaynak denetimi dağıtımının yalnızca uygulama ayarları tamamen güncelleştirildikten sonra başlayacağı anlamına gelir.
