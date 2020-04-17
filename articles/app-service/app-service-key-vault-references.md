---
title: Anahtar Kasası referanslarını kullanma
description: Azure Anahtar Kasası başvurularını kullanmak için Azure Uygulama Hizmeti ve Azure Işlevlerini nasıl ayarlayabilirsiniz öğrenin. Anahtar Kasa sısırlar uygulama kodunuz için kullanılabilir olun.
author: mattchenderson
ms.topic: article
ms.date: 10/09/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: dd0a03ea76d517486bb9bda6d9628fb529166dd8
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81453736"
---
# <a name="use-key-vault-references-for-app-service-and-azure-functions"></a>Uygulama Hizmeti ve Azure İşlevleri için Anahtar Kasa başvurularını kullanma

Bu konu, herhangi bir kod değişikliği gerektirmeden Uygulama Hizmetinizdeki veya Azure İşlevler uygulamanızdaki Azure Key Vault'un sırlarıyla nasıl çalışacağınızı gösterir. [Azure Key Vault,](../key-vault/general/overview.md) erişim ilkeleri ve denetim geçmişi üzerinde tam denetime sahip merkezi sırlar yönetimi sağlayan bir hizmettir.

## <a name="granting-your-app-access-to-key-vault"></a>Uygulamanızın Key Vault'a erişimini verme

Key Vault'un sırlarını okumak için bir kasa oluşturmanız ve uygulamanıza erişim izni vermeniz gerekir.

1. [Key Vault hızlı başlat'ı](../key-vault/secrets/quick-create-cli.md)izleyerek bir anahtar kasa oluşturun.

1. Uygulamanız için [sistem tarafından atanmış yönetilen](overview-managed-identity.md) bir kimlik oluşturun.

   > [!NOTE] 
   > Anahtar Vault başvuruları şu anda yalnızca sistem tarafından atanan yönetilen kimlikleri destekler. Kullanıcı tarafından atanan kimlikler kullanılamaz.

1. Daha önce oluşturduğunuz uygulama kimliği için [Key Vault'ta](../key-vault/general/secure-your-key-vault.md#key-vault-access-policies) bir erişim ilkesi oluşturun. Bu ilkede "Get" gizli iznini etkinleştirin. Yönetilen bir kimlikle uyumlu olmadığı `applicationId` için "yetkili uygulama" veya ayarları yapılandırmayın.

    > [!NOTE]
    > Anahtar Vault başvuruları şu anda [ağ kısıtlamaları](../key-vault/general/overview-vnet-service-endpoints.md)olan anahtar kasasında depolanan sırları çözemez.

## <a name="reference-syntax"></a>Referans sözdizimi

Anahtar Kasa başvurusu, `@Microsoft.KeyVault({referenceString})` `{referenceString}` aşağıdaki seçeneklerden biriyle değiştirilen formdan dır:

> [!div class="mx-tdBreakAll"]
> | Başvuru dizesi                                                            | Açıklama                                                                                                                                                                                 |
> |-----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | SecretUri=_secretUri_                                                       | **SecretUri** Key Vault bir sır tam veri düzlemi URI olmalıdır, bir sürümü de dahil olmak üzere, örneğin,https://myvault.vault.azure.net/secrets/mysecret/ec96f02080254f109c51a1f14cdb1931  |
> | VaultName=_vaultName_; SecretName=_secretName_; SecretVersion=_secretVersion_ | **VaultName,** Key Vault kaynağınızın adı olmalıdır. **SecretName** hedef sırrın adı olmalıdır. **SecretVersion** kullanmak için gizli sürümü olmalıdır. |

Örneğin, Sürüm ile tam bir başvuru aşağıdaki gibi görünür:

```
@Microsoft.KeyVault(SecretUri=https://myvault.vault.azure.net/secrets/mysecret/ec96f02080254f109c51a1f14cdb1931)
```
Alternatif olarak:

```
@Microsoft.KeyVault(VaultName=myvault;SecretName=mysecret;SecretVersion=ec96f02080254f109c51a1f14cdb1931)
```


## <a name="source-application-settings-from-key-vault"></a>Key Vault'tan Kaynak Uygulama Ayarları

Anahtar Vault referansları Uygulama [Ayarları](configure-common.md#configure-app-settings)için değer olarak kullanılabilir, site config yerine Key Vault sırlarını tutmak için izin. Uygulama Ayarları istirahatte güvenli bir şekilde şifrelenir, ancak gizli yönetim yeteneklerine ihtiyacınız varsa, Anahtar Kasa'ya gitmelidir.

Bir uygulama ayarı için Key Vault başvurusunu kullanmak için, başvuruyu ayarın değeri olarak ayarlayın. Uygulamanız sırrı anahtarı yla normal olarak referans verebilir. Kod değişikliği gerekmez.

> [!TIP]
> Key Vault başvurularını kullanan çoğu uygulama ayarı, her ortam için ayrı kasalara sahip olması gerektiğinden, yuva ayarları olarak işaretlenmelidir.

### <a name="azure-resource-manager-deployment"></a>Azure Resource Manager dağıtımı

Azure Kaynak Yöneticisi şablonları aracılığıyla kaynak dağıtımlarını otomatikleştirirken, bu özelliğin çalışması için bağımlılıklarınızı belirli bir sırada sıralamanız gerekebilir. Ayrıca, site tanımında bir `siteConfig` özellik kullanmak yerine uygulama ayarlarınızı kendi kaynakları olarak tanımlamanız gerekir. Bunun nedeni, sistem tarafından atanan kimliğin onunla oluşturulabilmesi ve erişim ilkesinde kullanılabilmesi için önce sitenin tanımlanması gerekir.

Bir işlev uygulaması için örnek bir psuedo şablonu aşağıdaki gibi görünebilir:

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
> Bu örnekte, kaynak denetimi dağıtımı uygulama ayarlarına bağlıdır. Uygulama ayarı güncelleştirmesi eşsenkronize olarak davranınca, bu normalde güvenli olmayan bir davranıştır. Ancak, uygulama ayarını `WEBSITE_ENABLE_SYNC_UPDATE_SITE` dahil ettiğimizden, güncelleştirme eşzamanlıdır. Bu, kaynak denetimi dağıtımının yalnızca uygulama ayarları tam olarak güncelleştirildikten sonra başlayacağı anlamına gelir.

## <a name="troubleshooting-key-vault-references"></a>Sorun Giderme Anahtar KasaSı Referansları

Bir başvuru düzgün çözülmezse, başvuru değeri bunun yerine kullanılır. Bu, uygulama ayarları için değeri sözdizimi olan `@Microsoft.KeyVault(...)` bir ortam değişkeni oluşturulacağı anlamına gelir. Bu, belirli bir yapının sırrını beklediği için uygulamanın hata lar atmasına neden olabilir.

En yaygın olarak, bu [Key Vault erişim ilkesinin](#granting-your-app-access-to-key-vault)bir yanlış yapılandırma kaynaklanmaktadır. Ancak, artık var olmayan bir gizli veya başvurunun kendisinde bir sözdizimi hatası ndan da kaynaklanıyor olabilir.

Sözdizimi doğruysa, portaldaki geçerli çözüm durumunu denetleyerek diğer hata nedenlerini görüntüleyebilirsiniz. Uygulama Ayarları'na gidin ve söz konusu başvuru için "Edit" seçeneğini belirleyin. Ayar yapılandırmasının altında, hatalar da dahil olmak üzere durum bilgilerini görmeniz gerekir. Bunların yokluğu, başvuru sözdiziminin geçersiz olduğu anlamına gelir.

Ek bilgi almak için dahili dedektörlerden birini de kullanabilirsiniz.

### <a name="using-the-detector-for-app-service"></a>Uygulama Hizmeti için dedektörü kullanma

1. Portalda, uygulamanıza gidin.
2. **Sorunları tanılama ve çözme** seçeneğini belirleyin.
3. **Kullanılabilirlik ve Performans'ı** seçin ve **Web uygulamasını aşağı seçin.**
4. **Anahtar Vault Uygulama Ayarları Tanıbulun** ve daha fazla **bilgi**tıklayın.


### <a name="using-the-detector-for-azure-functions"></a>Azure Fonksiyonları için dedektörü kullanma

1. Portalda, uygulamanıza gidin.
2. Platform **özelliklerine gidin.**
3. **Sorunları tanılama ve çözme** seçeneğini belirleyin.
4. **Kullanılabilirlik ve Performans'ı** seçin ve **Fonksiyon uygulamasını aşağı veya raporlama hatalarını seçin.**
5. Key **Vault Uygulama Ayarları Tanılama'ya tıklayın.**
