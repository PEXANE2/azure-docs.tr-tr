---
title: Günlükler - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: PostgreSQL için Azure Veritabanında günlük yapılandırması, depolama ve analiz açıklar - Tek Sunucu
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/25/2019
ms.openlocfilehash: 2636e9a225002148e4cd79bb2176e0883aed623a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280501"
---
# <a name="logs-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanında Oturum Açma - Tek Sunucu
PostgreSQL için Azure Veritabanı, Postgres'in standart günlüklerini yapılandırmanızı ve erişmenizi sağlar. Günlükler, yapılandırma hatalarını ve düşük performans belirlemek, sorun gidermek ve onarmak için kullanılabilir. Yapılandırabileceğiniz ve erişebileceğiniz günlük bilgileri hatalar, sorgu bilgileri, otomatik vakum kayıtları, bağlantılar ve denetim noktaları içerir. (Hareket günlüklerine erişim kullanılamıyor).

Denetim günlüğü bir Postgres uzantısı, pgaudit aracılığıyla kullanılabilir hale getirilir. Daha fazla bilgi edinmek için denetim kavramları makalesini ziyaret [edin.](concepts-audit.md)


## <a name="configure-logging"></a>Günlük işlemlerini yapılandırma 
Günlük sunucusu parametrelerini kullanarak sunucunuzda Postgres standart günlüğe kaydetmeyi yapılandırabilirsiniz. PostgreSQL sunucusu için her `log_checkpoints` Azure `log_connections` Veritabanı'nda varsayılan olarak açıktır. Günlük gereksinimlerinize göre ayarlayabildiğiniz ek parametreler vardır: 

![PostgreSQL için Azure Veritabanı - Günlük parametreleri](./media/concepts-server-logs/log-parameters.png)

Postgres günlük parametreleri hakkında daha fazla bilgi edinmek için, Postgres belgelerinin [Ne Zaman Ve](https://www.postgresql.org/docs/current/runtime-config-logging.html#RUNTIME-CONFIG-LOGGING-WHEN) Ne Günlük [Ler](https://www.postgresql.org/docs/current/runtime-config-logging.html#RUNTIME-CONFIG-LOGGING-WHAT) Için bölümlerini ziyaret edin. Hepsi olmasa da çoğu Postgres günlük parametreleri, PostgreSQL için Azure Veritabanı'nda yapılandırmak için kullanılabilir.

PostgreSQL için Azure Veritabanı'nda parametreleri nasıl yapılandırıştırılabilirsiniz, [portal belgelerine](howto-configure-server-parameters-using-portal.md) veya [CLI belgelerine](howto-configure-server-parameters-using-cli.md)bakın. 

> [!NOTE]
> Yüksek hacimli günlükleri yapılandırmak , örneğin ekstre günlüğü, önemli performans yükü ekleyebilirsiniz. 

## <a name="access-log-files"></a>Erişim .log dosyaları
PostgreSQL için Azure Veritabanı'ndaki varsayılan günlük biçimi .log'dur. Bu günlükten örnek satır gibi görünür:

```
2019-10-14 17:00:03 UTC-5d773cc3.3c-LOG: connection received: host=101.0.0.6 port=34331 pid=16216
```

PostgreSQL için Azure Veritabanı,.log dosyaları için kısa süreli bir depolama konumu sağlar. Yeni bir dosya her 1 saat veya 100 MB başlar, hangisi önce gelir. Günlükler, Postgres'ten yayılan geçerli dosyaya eklenir.  

Bu kısa süreli günlük depolamaiçin bekletme süresini `log_retention_period` parametreyi kullanarak ayarlayabilirsiniz. Varsayılan değer 3 gün, en büyük değer ise 7 gündür. Kısa süreli depolama konumu en fazla 1 GB günlük dosyası tutabilir. 1 GB'dan sonra, saklama süresine bakılmaksızın en eski dosyalar yeni günlüklere yer açmak için silinir. 

Günlüklerin ve günlük çözümlemesinin uzun süreli tutulması için .log dosyalarını indirebilir ve üçüncü taraf bir hizmete taşıyabilirsiniz. Dosyaları [Azure portalı](howto-configure-server-logs-in-portal.md), Azure [CLI'yi](howto-configure-server-logs-using-cli.md)kullanarak indirebilirsiniz. Alternatif olarak, günlüklerinizi otomatik olarak (JSON biçiminde) daha uzun vadeli konumlara yayan Azure Monitor tanılama ayarlarını yapılandırabilirsiniz. Bu seçenek hakkında daha fazla bilgi için aşağıdaki bölümde yer aınıza ait. 

Parametreyi `logging_collector` OFF'a ayarlayarak .log dosyaları oluşturmayı durdurabilirsiniz. Azure Monitor tanılama ayarlarını kullanıyorsanız .log dosya oluşturmayı kapatmanız önerilir. Bu yapılandırma, ek günlüğe kaydetmenin performans etkisini azaltır.

## <a name="diagnostic-logs"></a>Tanılama günlükleri
PostgreSQL için Azure Veritabanı, Azure Monitor tanı ayarlarıyla tümleşiktir. Tanılama ayarları, GSON formatındaki Postgres günlüklerinizi analitik ve uyarı için Azure Monitor Günlükleri'ne, akış için Etkinlik Hub'larını ve arşivleme için Azure Depolama'ya göndermenize olanak tanır. 

> [!IMPORTANT]
> Sunucu günlükleri için bu tanılama özelliği yalnızca Genel Amaç ve Bellek Optimize Edilmiş [fiyatlandırma katmanlarında](concepts-pricing-tiers.md)kullanılabilir.


### <a name="configure-diagnostic-settings"></a>Tanılama ayarlarını yapılandırma
Azure portalı, CLI, REST API ve Powershell'i kullanarak Postgres sunucunuzun tanılama ayarlarını etkinleştirebilirsiniz. Seçmek için günlük kategori **PostgreSQLLogs**olduğunu. (Sorgu [Mağazası](concepts-query-store.md)kullanıyorsanız yapılandırabileceğiniz başka günlükler de vardır .)

Azure portalını kullanarak Tanılama günlüklerini etkinleştirmek için:

   1. Portalda, Postgres sunucunuzun navigasyon menüsündeki *Tanılama Ayarları'na* gidin.
   2. *Tanılama Ayarı Ekle'yi*seçin.
   3. Bu ayarı adlandırın. 
   4. Tercih ettiğiniz bitiş noktasını (depolama hesabı, etkinlik merkezi, günlük analitiği) seçin. 
   5. Günlük türü **PostgreSQLLogs**seçin.
   7. Ayarınızı kaydedin.

Powershell, CLI veya REST API'yi kullanarak Tanılama günlüklerini etkinleştirmek için [tanıayarları](../azure-monitor/platform/diagnostic-settings.md) makalesini ziyaret edin.

### <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

Günlüklere erişme şekliniz seçtiğiniz bitiş noktasına bağlıdır. Azure Depolama için [günlükdepolama hesabı](../azure-monitor/platform/resource-logs-collect-storage.md) makalesine bakın. Olay Hub'ları için [akış Azure günlükleri](../azure-monitor/platform/resource-logs-stream-event-hubs.md) makalesine bakın.

Azure Monitör Günlükleri için günlükler seçtiğiniz çalışma alanına gönderilir. Postgres günlükleri Azure Diagnostics toplama modunu kullanarak **AzureDiagnostics** tablosundan sorgulanabilirler. Tablodaki alanlar aşağıda açıklanmıştır. [Azure Monitör Günlükleri sorgusuna](../azure-monitor/log-query/log-query-overview.md) genel bakışta sorgulama ve uyarı hakkında daha fazla bilgi edinin.

Aşağıda, başlamayı deneyebileceğiniz sorgular vesorguları bulabilirsiniz. Uyarıları sorgulara göre yapılandırabilirsiniz.

Son gün içinde belirli bir sunucu için tüm Postgres günlüklerini arama
```
AzureDiagnostics
| where LogicalServerName_s == "myservername"
| where TimeGenerated > ago(1d) 
```

Tüm yerel olmayan ana bilgisayar bağlantı denemelerini arama
```
AzureDiagnostics
| where Message contains "connection received" and Message !contains "host=127.0.0.1"
| where Category == "PostgreSQLLogs" and TimeGenerated > ago(6h)
```
Yukarıdaki sorgu, bu çalışma alanında oturum açan herhangi bir Postgres sunucusu için son 6 saat içinde sonuçları gösterir.

### <a name="log-format"></a>Günlük biçimi

Aşağıdaki tabloda **PostgreSQLLogs** türüne ait alanlar açıklanmaktadır. Seçtiğiniz çıktı bitiş noktasına bağlı olarak, dahil edilen alanlar ve bunların görüntülenme sırası değişebilir. 

|**Alan** | **Açıklama** |
|---|---|
| TenantId | Kiracı kimliğiniz |
| SourceSystem | `Azure` |
| Zaman Oluşturuldu [UTC] | Günlük UTC'de kaydedildiğinde zaman damgası |
| Tür | Günlük türü. Her zaman`AzureDiagnostics` |
| SubscriptionId | Sunucunun ait olduğu abonelik için GUID |
| ResourceGroup | Sunucunun ait olduğu kaynak grubunun adı |
| ResourceProvider | Kaynak sağlayıcısının adı. Her zaman`MICROSOFT.DBFORPOSTGRESQL` |
| ResourceType | `Servers` |
| ResourceId | Kaynak URI |
| Kaynak | Sunucunun adı |
| Kategori | `PostgreSQLLogs` |
| ThrottledRequests | `LogEvent` |
| Errorlevel | Günlük düzeyi, örnek: LOG, HATA, DİkKAT |
| İleti | Birincil günlük iletisi | 
| Domain | Sunucu sürümü, örnek: postgres-10 |
| Ayrıntı | İkincil günlük iletisi (varsa) |
| ColumnName | Sütunun adı (varsa) |
| SchemaName | Şema adı (varsa) |
| DatatypeName | Veri türünün adı (varsa) |
| MantıksalSunucu Adı | Sunucunun adı | 
| _ResourceId | Kaynak URI |
| Ön ek | Günlük satırının öneki |


## <a name="next-steps"></a>Sonraki adımlar
- [Azure portalından](howto-configure-server-logs-in-portal.md) veya [Azure CLI'den](howto-configure-server-logs-using-cli.md)günlüklere erişim hakkında daha fazla bilgi edinin.
- [Azure Monitör fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/)hakkında daha fazla bilgi edinin.
- [Denetim günlükleri](concepts-audit.md) hakkında daha fazla bilgi edinin
