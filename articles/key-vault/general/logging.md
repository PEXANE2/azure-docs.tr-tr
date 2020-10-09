---
title: Günlüğe kaydetme Azure Key Vault | Microsoft Docs
description: Bilgileri sağladığınız bir Azure depolama hesabına kaydeden Azure Key Vault için günlüğe kaydetmeyi etkinleştirerek anahtar kasalarınıza erişimi nasıl izleyeceğinizi öğrenin.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 08/12/2019
ms.author: mbaldwin
ms.openlocfilehash: a51e9a628f67269357d42bd1d3af10c1d86f301a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91739791"
---
# <a name="azure-key-vault-logging"></a>Azure Key Vault günlüğü

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bir veya daha fazla Anahtar Kasası oluşturduktan sonra muhtemelen anahtar kasalarınızın nasıl ve ne zaman erişildiğini ve kim tarafından yapılacağını izlemek isteyeceksiniz. Bu, bilgileri, sağladığınız bir Azure depolama hesabına kaydeden Azure Key Vault için günlüğe kaydetmeyi etkinleştirerek yapabilirsiniz. Belirtilen depolama hesabınız için **Öngörüler-logs-auditevent** adlı yeni bir kapsayıcı otomatik olarak oluşturulur. Birden çok Anahtar Kasası için günlükleri toplamak üzere bu depolama hesabını kullanabilirsiniz.

Kayıt bilgilerinizi 10 dakika (en çok) Anahtar Kasası işleminden sonra erişebilirsiniz. Çoğu durumda, bundan daha hızlı olacaktır.  Depolama hesabınızdaki günlüklerinizi yönetmek size bağlıdır:

* Günlüklerinize erişebilecek kişileri kısıtlayarak güvenliklerini sağlamak için standart Azure erişim denetimi yöntemlerini kullanın.
* Artık depolama hesabınızda tutmak istemediğiniz günlükleri silin.

Key Vault hakkında genel bilgi için bkz. [Azure Key Vault nedir?](overview.md)). Key Vault nerede kullanılabildiği hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/key-vault/)bakın. [Key Vault Için Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/insights/key-vault-insights-overview)kullanma hakkında bilgi için.

## <a name="interpret-your-key-vault-logs"></a>Anahtar Kasası günlüklerinizi yorumlama

Tek tek bloblar JSON blobu olarak biçimlendirilip metin olarak depolanır. Bir örnek günlük girişine bakalım. 

```json
    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }
```

Aşağıdaki tabloda alan adları ve açıklamaları listelenmektedir:

| Alan adı | Açıklama |
| --- | --- |
| **ışınızda** |UTC olarak tarih ve saat. |
| **RESOURCEID** |Azure Resource Manager kaynak KIMLIĞI. Key Vault günlükleri için, her zaman Key Vault kaynak KIMLIĞI olur. |
| **operationName** |Sonraki tabloda belirtildiği gibi işlemin adı. |
| **operationVersion** |İstemci tarafından istenen sürümü REST API. |
| **alan** |Sonuç türü. Key Vault günlükleri için, `AuditEvent` tek, kullanılabilir değerdir. |
| **'ı** |REST API isteğinin sonucu. |
| **resultSignature** |HTTP durumu. |
| **resultDescription** |Kullanılabilir olduğunda sonuç hakkında ek açıklama. |
| **Ort** |Milisaniye cinsinden REST API'si isteğini sunmak için geçen süre. Buna ağ gecikme süresi dahil değildir; bu nedenle istemci tarafında ölçtüğünüz süre bu süreyle eşleşmeyebilir. |
| **callerIpAddress** |İsteği yapan istemcinin IP adresi. |
| **ID** |İstemci tarafı günlüklerini hizmet tarafı (Anahtar Kasası) günlükleriyle ilişkilendirmek için istemcinin geçirebileceği isteğe bağlı bir GUID. |
| **IDENTITY** |REST API isteğinde sunulan belirteçten kimlik. Bu, genellikle bir "Kullanıcı," bir "hizmet sorumlusu" veya "Kullanıcı + AppID" birleşimidir ve bir Azure PowerShell cmdlet 'inin sonucu olan bir istekte bulunur. |
| **özelliklerinin** |İşleme göre farklılık gösteren bilgiler (**OperationName**). Çoğu durumda bu alan istemci bilgilerini (istemci tarafından geçirilen kullanıcı aracısı dizesi), tam REST API istek URI 'sini ve HTTP durum kodunu içerir. Ayrıca, bir nesne bir isteğin sonucu olarak döndürüldüğünde (örneğin, **Keycreate** veya **vaultget**), anahtar URI (as `id` ), kasa URI 'si veya gizli URI 'yi de içerir. |

**OperationName** alan değerleri *objectverb* biçimindedir. Örneğin:

* Tüm Anahtar Kasası işlemleri `Vault<action>` , ve gibi biçimdedir `VaultGet` `VaultCreate` .
* Tüm anahtar işlemleri `Key<action>` , ve gibi biçimdedir `KeySign` `KeyList` .
* Tüm gizli işlemler `Secret<action>` , ve gibi biçimdedir `SecretGet` `SecretListVersions` .

Aşağıdaki tabloda, **OperationName** değerleri ve karşılık gelen REST API komutları listelenmektedir:

### <a name="operation-names-table"></a>İşlem adları tablosu

| operationName | REST API komutu |
| --- | --- |
| **Kimlik Doğrulaması** |Azure Active Directory uç noktası aracılığıyla kimlik doğrulaması |
| **VaultGet** |[Bir anahtar kasası hakkında bilgi edinme](https://msdn.microsoft.com/library/azure/mt620026.aspx) |
| **VaultPut** |[Bir anahtar kasası oluşturma veya güncelleştirme](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| **VaultDelete** |[Bir anahtar kasasını silme](https://msdn.microsoft.com/library/azure/mt620022.aspx) |
| **VaultPatch** |[Bir anahtar kasasını güncelleştirme](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| **VaultList** |[Bir kaynak grubundaki tüm anahtar kasalarını listeleme](https://msdn.microsoft.com/library/azure/mt620027.aspx) |
| **KeyCreate** |[Bir anahtar oluşturma](https://msdn.microsoft.com/library/azure/dn903634.aspx) |
| **KeyGet** |[Bir anahtar hakkında bilgi edinme](https://msdn.microsoft.com/library/azure/dn878080.aspx) |
| **KeyImport** |[Bir kasaya bir anahtar aktarma](https://msdn.microsoft.com/library/azure/dn903626.aspx) |
| **KeyBackup** |[Anahtar yedekleme](https://msdn.microsoft.com/library/azure/dn878058.aspx) |
| **KeyDelete** |[Bir anahtarı silme](https://msdn.microsoft.com/library/azure/dn903611.aspx) |
| **KeyRestore** |[Bir anahtarı geri yükleme](https://msdn.microsoft.com/library/azure/dn878106.aspx) |
| **KeySign** |[Bir anahtar ile oturum açma](https://msdn.microsoft.com/library/azure/dn878096.aspx) |
| **KeyVerify** |[Bir anahtar ile doğrulama](https://msdn.microsoft.com/library/azure/dn878082.aspx) |
| **KeyWrap** |[Bir anahtarı sarmalama](https://msdn.microsoft.com/library/azure/dn878066.aspx) |
| **KeyUnwrap** |[Bir anahtarı kaydırma](https://msdn.microsoft.com/library/azure/dn878079.aspx) |
| **KeyEncrypt** |[Bir anahtar ile şifreleme](https://msdn.microsoft.com/library/azure/dn878060.aspx) |
| **KeyDecrypt** |[Bir anahtar ile şifre çözme](https://msdn.microsoft.com/library/azure/dn878097.aspx) |
| **KeyUpdate** |[Bir anahtarı güncelleştirme](https://msdn.microsoft.com/library/azure/dn903616.aspx) |
| **KeyList** |[Bir kasadaki anahtarları listeleme](https://msdn.microsoft.com/library/azure/dn903629.aspx) |
| **KeyListVersions** |[Bir anahtarın sürümlerini listeleme](https://msdn.microsoft.com/library/azure/dn986822.aspx) |
| **SecretSet** |[Gizli anahtar oluşturma](https://msdn.microsoft.com/library/azure/dn903618.aspx) |
| **SecretGet** |[Gizli dizi alın](https://msdn.microsoft.com/library/azure/dn903633.aspx) |
| **SecretUpdate** |[Gizli anahtarı güncelleştirme](https://msdn.microsoft.com/library/azure/dn986818.aspx) |
| **SecretDelete** |[Gizli anahtarı silme](https://msdn.microsoft.com/library/azure/dn903613.aspx) |
| **SecretList** |[Bir kasadaki gizli anahtarları listeleme](https://msdn.microsoft.com/library/azure/dn903614.aspx) |
| **SecretListVersions** |[Bir gizli anahtarın sürümlerini listeleme](https://msdn.microsoft.com/library/azure/dn986824.aspx) |
| **VaultAccessPolicyChangedEventGridNotification** | Kasa erişimi ilkesi değiştirilmiş olayı yayınlandı |
| **Secretyaklaştığında Expiryeventgridnotification** |Süre sonu olayı yayımlanan gizli dizi |
| **Secretexpio Ventgridnotification** |Gizli zaman aşımına uğradı olayı yayımlandı |
| **Keyyaklaştığında Expiryeventgridnotification** |Süre sonu olayını yayımlayan anahtar |
| **KeyExpiredEventGridNotification** |Anahtarın süre dolduğunda etkinlik yayınlandı |
| **CertificateNearExpiryEventGridNotification** |Süre sonu olayı yayımlanan sertifika |
| **CertificateExpiredEventGridNotification** |Sertifikanın zaman aşımına uğradığı olay yayımlandı |

## <a name="use-azure-monitor-logs"></a><a id="loganalytics"></a>Azure Izleyici günlüklerini kullanma

Key Vault günlüklerini gözden geçirmek için Azure Izleyici günlüklerinde Key Vault çözümünü kullanabilirsiniz `AuditEvent` . Azure Izleyici günlüklerinde, verileri analiz etmek ve ihtiyacınız olan bilgileri almak için günlük sorgularını kullanırsınız. 

Bunu ayarlama dahil daha fazla bilgi için bkz. [Azure izleyici 'de Azure Key Vault](../../azure-monitor/insights/key-vault-insights-overview.md).

## <a name="next-steps"></a><a id="next"></a>Sonraki adımlar

.NET Web uygulamasında Azure Key Vault kullanan bir öğretici için bkz. [bir Web uygulamasından Azure Key Vault kullanma](tutorial-net-create-vault-azure-web-app.md).

Programlama başvuruları için bkz. [Azure Anahtar Kasası geliştirici kılavuzu](developers-guide.md).

Azure Key Vault için Azure PowerShell 1,0 cmdlet 'lerinin bir listesi için bkz. [Azure Key Vault cmdlet 'leri](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault).
