---
title: Azure Monitor kaynak günlüklerinde biçim değişikliğine hazırlanın
description: 1 Kasım 2018'de ek blobs kullanmak üzere değiştirilen yeni Azure kaynak günlüklerini işlemek için aracınızın etkisini ve nasıl güncelleştirilmeye başlandığını açıklar.
author: johnkemnetz
services: monitoring
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: d30652d4e068cbceb79e6da60b48176b9de64647
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670449"
---
# <a name="prepare-for-format-change-to-azure-monitor-resource-logs-archived-to-a-storage-account"></a>Depolama hesabına arşivlenmiş Azure Monitor kaynak günlüklerinde biçim değişikliğine hazırlanma

> [!WARNING]
> [Günlük profillerini kullanarak](./../../azure-monitor/platform/archive-activity-log.md)bir depolama hesabına kaynak tanılama ayarlarını veya etkinlik günlüklerini kullanarak bir depolama [hesabına Azure kaynak günlükleri veya ölçümleri](./../../azure-monitor/platform/archive-diagnostic-logs.md) gönderiyorsanız, depolama hesabındaki verilerin biçimi 1 Kasım 2018'de JSON Satırları olarak değişecektir. Aşağıdaki yönergelerde, yeni biçimi işlemek için aracınızın etkisi ve nasıl güncelleştirilen açıklanmıştır. 
>
> 

## <a name="what-is-changing"></a>Neler değişiyor

Azure Monitor, kaynak tanılama verileri ve etkinlik günlüğü verilerini Bir Azure depolama hesabına, Olay Hub'ları ad alanına veya Azure Monitor'daki Günlük Analizi çalışma alanına göndermenize olanak tanıyan bir özellik sunar. Bir sistem performansı sorununu gidermek için, **1 Kasım 2018 tarihinde saat 12:00'de UTC'de** blob depolamaya gönderilen günlük verilerinin formatı değişecektir. Blob depolama dan veri okuma araç varsa, yeni veri biçimini anlamak için araç güncelleştirmeniz gerekir.

* Perşembe günü, Kasım 1, 2018 at 12:00 gece yarısı UTC, blob biçimi [JSON Lines](http://jsonlines.org/)olarak değişecektir. Bu, her kaydın bir yeni satırla sınırlandırılacak, dış kayıt dizisi ve JSON kayıtları arasında virgül olmayacağı anlamına gelir.
* Blob biçimi, tüm abonelikler genelinde aynı anda tüm tanılama ayarları için değişir. 1 Kasım için yayılan ilk PT1H.json dosyası bu yeni biçimi kullanacaktır. Leke ve kapsayıcı adları aynı kalır.
* Şimdi ve 1 Kasım arasında bir tanıa ayar 1 Kasım'a kadar geçerli biçimde veri yayarlar.
* Bu değişiklik, tüm genel bulut bölgelerinde aynı anda gerçekleşir. Değişiklik, 21Vianet, Azure Almanya veya Azure Devlet bulutları tarafından işletilen Microsoft Azure'da henüz gerçekleşmez.
* Bu değişiklik aşağıdaki veri türlerini etkiler:
  * [Azure kaynak günlükleri](archive-diagnostic-logs.md) ([kaynak listesine bakın)](diagnostic-logs-schema.md)
  * [Azure kaynak ölçümleri tanılama ayarlarıyla dışa aktarılıyor](diagnostic-settings.md)
  * [Azure Etkinlik günlük verileri günlük profilleri tarafından dışa aktarılıyor](archive-activity-log.md)
* Bu değişiklik etkilemez:
  * Ağ akış günlükleri
  * Azure hizmet günlükleri henüz Azure Monitor üzerinden kullanıma sunulmadı (örneğin, Azure App Service kaynak günlükleri, depolama analizi günlükleri)
  * Azure kaynak günlüklerinin ve etkinlik günlüklerinin diğer hedeflere yönlendirmesi (Olay Hub'ları, Günlük Analizi)

### <a name="how-to-see-if-you-are-impacted"></a>Etkilenip etkilenmediğinizi görme

Bu değişiklikyalnızca şu şekilde etkilenirseniz:
1. Kaynak tanılama ayarını kullanarak günlük verilerini bir Azure depolama hesabına gönderiyor ve
2. Depolama bu günlükleri JSON yapısına bağlı takım var.
 
Bir Azure depolama hesabına veri gönderen kaynak tanılama ayarlarınız olup olmadığını belirlemek için portalın **Monitör** bölümüne gidebilir, **Tanılama Ayarları'nı**tıklatabilir ve **Etkin'e** **Tanıdurumu** ayarlanan kaynakları tanımlayabilirsiniz:

![Azure Monitör Tanı Ayarları blade](./media/diagnostic-logs-append-blobs/portal-diag-settings.png)

Tanılama Durumu etkin olarak ayarlanmışsa, bu kaynakta etkin bir tanılama ayarı var. Herhangi bir tanılama ayarlarının bir depolama hesabına veri gönderip göndermedığını görmek için kaynağa tıklayın:

![Depolama hesabı etkin](./media/diagnostic-logs-append-blobs/portal-storage-enabled.png)

Bu kaynak tanılama ayarlarını kullanarak bir depolama hesabına veri gönderen kaynaklarınız varsa, bu depolama hesabındaki verilerin biçimi bu değişiklikten etkilenir. Bu depolama hesaplarının dışında çalışan özel takımlarınız yoksa, biçim değişikliği sizi etkilemez.

### <a name="details-of-the-format-change"></a>Biçim değişikliğinin ayrıntıları

Azure blob depolamasındaki PT1H.json dosyasının geçerli biçimi, bir JSON kayıt dizisi kullanır. Burada şimdi bir KeyVault günlük dosyasının bir örneği:

```json
{
    "records": [
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
            "identity": {
                "claim": {
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com",
                    "appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"
                }
            },
            "properties": {
                "clientInfo": "azure-resource-manager/2.0",
                "requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01",
                "id": "https://contosokeyvault.vault.azure.net/",
                "httpStatusCode": 200
            }
        },
        {
            "time": "2016-01-05T01:33:56.5264523Z",
            "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
            "operationName": "VaultGet",
            "operationVersion": "2015-06-01",
            "category": "AuditEvent",
            "resultType": "Success",
            "resultSignature": "OK",
            "resultDescription": "",
            "durationMs": "83",
            "callerIpAddress": "104.40.82.76",
            "correlationId": "",
            "identity": {
                "claim": {
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com",
                    "appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"
                }
            },
            "properties": {
                "clientInfo": "azure-resource-manager/2.0",
                "requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01",
                "id": "https://contosokeyvault.vault.azure.net/",
                "httpStatusCode": 200
            }
        }
    ]
}
```

Yeni biçim, her olayın bir satır olduğu ve yeni çizgi karakterinin yeni bir olayı gösterdiği [JSON satırlarını](http://jsonlines.org/)kullanır. Yukarıdaki örnek, değişiklikten sonra PT1H.json dosyasında aşağıdaki gibidir:

```json
{"time": "2016-01-05T01:32:01.2691226Z","resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT","operationName": "VaultGet","operationVersion": "2015-06-01","category": "AuditEvent","resultType": "Success","resultSignature": "OK","resultDescription": "","durationMs": "78","callerIpAddress": "104.40.82.76","correlationId": "","identity": {"claim": {"http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com","appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},"properties": {"clientInfo": "azure-resource-manager/2.0","requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id": "https://contosokeyvault.vault.azure.net/","httpStatusCode": 200}}
{"time": "2016-01-05T01:33:56.5264523Z","resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT","operationName": "VaultGet","operationVersion": "2015-06-01","category": "AuditEvent","resultType": "Success","resultSignature": "OK","resultDescription": "","durationMs": "83","callerIpAddress": "104.40.82.76","correlationId": "","identity": {"claim": {"http://schemas.microsoft.com/identity/claims/objectidentifier": "d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "live.com#username@outlook.com","appid": "1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},"properties": {"clientInfo": "azure-resource-manager/2.0","requestUri": "https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id": "https://contosokeyvault.vault.azure.net/","httpStatusCode": 200}}
```

Bu yeni biçim, Azure Monitor'un yeni olay verilerini sürekli olarak eklemede daha verimli olan [ek blob'ları](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-append-blobs)kullanarak günlük dosyalarını itmesini sağlar.

## <a name="how-to-update"></a>Nasıl güncellenir?

Yalnızca daha fazla işlem için bu günlük dosyalarını yutacak özel bir araç varsa güncelleştirmeler yapmanız gerekir. Harici bir günlük analitiği veya SIEM aracı kullanıyorsanız, [bunun yerine bu verileri yutmak için olay hub'larını kullanmanızı](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)öneririz. Olay hub'ları tümleştirmesi, birçok hizmetten günlük işleme ve belirli bir günlükteki yer imi konumu açısından daha kolaydır.

Özel araçlar, hem geçerli biçimi hem de yukarıda açıklanan JSON Satırları biçimini işlemek için güncelleştirilmelidir. Bu, verilerin yeni biçimde görünmeye başladığında araçlarınızın kırılmamasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* Kaynak [günlüklerini depolama hesabına arşivleme](./../../azure-monitor/platform/archive-diagnostic-logs.md) hakkında bilgi edinin
* Etkinlik [günlüğü verilerini depolama hesabına arşivleme](./../../azure-monitor/platform/archive-activity-log.md) hakkında bilgi edinin

