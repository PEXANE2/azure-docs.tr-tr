---
title: Denetim günlüğü - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: PostgreSQL için Azure Veritabanında pgAudit denetim günlüğü için kavramlar - Tek Sunucu.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/28/2020
ms.openlocfilehash: 45490e398abd8b5bd3c10adb95b56e1019d2bb94
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76842478"
---
# <a name="audit-logging-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanında Denetim günlüğü - Tek Sunucu

PostgreSQL - Single Server için Azure Veritabanı'ndaki veritabanı etkinliklerinin denetim günlüğü, PostgreSQL Denetim Uzantısı üzerinden kullanılabilir: [pgAudit](https://www.pgaudit.org/). pgAudit ayrıntılı oturum ve/veya nesne denetimi günlüğü sağlar.

> [!NOTE]
> pgAudit, PostgreSQL için Azure Veritabanı'nda önizlemededir.
> Uzantı yalnızca Genel Amaç ve Bellek Optimize Edilmiş sunucularda etkinleştirilebilir.

Bilgi işlem ve depolama ölçekleme gibi işlemler için Azure kaynak düzeyi günlükleri istiyorsanız, [Azure Etkinlik Günlüğü'ne](../azure-monitor/platform/platform-logs-overview.md)bakın.

## <a name="usage-considerations"></a>Kullanım da göz önünde bulundurulması gerekenler
Varsayılan olarak, Postgres'in standart günlük özelliği kullanılarak pgAudit günlük deyimleri normal günlük deyimlerinizle birlikte yayılır. PostgreSQL için Azure Veritabanı'nda bu .log dosyaları Azure portalı veya CLI aracılığıyla indirilebilir. Dosyaların toplanması için maksimum depolama alanı 1 GB'dır ve her dosya en fazla yedi gün kullanılabilir (varsayılan değer üç gündür). Bu hizmet kısa süreli depolama seçeneğidir.

Alternatif olarak, tüm günlükleri Azure Monitor'un tanıgünlüğü hizmetine yayılacak şekilde yapılandırabilirsiniz. Azure Monitor tanı günlüğe kaydetmeyi etkinleştiriseniz, günlükleriniz otomatik olarak (JSON formatında) seçtiğinize bağlı olarak Azure Depolama, Etkinlik Hub'ları ve/veya Azure Monitor günlüklerine gönderilir.

pgAudit'in etkinleştirilmesi sunucuda çok büyük hacimli bir günlük oluşturur, bu da performansı ve günlük depolamasını etkiler. Hem daha uzun vadeli depolama seçenekleri hem de analiz ve uyarı özellikleri sunan Azure tanılama günlüğü hizmetini kullanmanızı öneririz. Ek günlüğün performans üzerindeki etkisini azaltmak için standart günlüğü kapatmanızı öneririz:

   1. Parametreyi `logging_collector` OFF olarak ayarlayın. 
   2. Bu değişikliği uygulamak için sunucuyu yeniden başlatın.

Azure Depolama, Etkinlik Hub'ları veya Azure Monitor günlüklerinde günlüğe kaydetmeyi öğrenmek için [sunucu günlükleri makalesinin](concepts-server-logs.md)tanıgünlükleri bölümünü ziyaret edin.

## <a name="installing-pgaudit"></a>pgAudit yükleme

pgAudit'i yüklemek için sunucunun paylaşılan ön yükleme kitaplıklarına eklemeniz gerekir. Postgres'in `shared_preload_libraries` parametresindeki değişiklik, bir sunucunun yeniden başlatılmasını gerektirir. [Azure portalı, Azure](howto-configure-server-parameters-using-portal.md) [CLI](howto-configure-server-parameters-using-cli.md)veya REST [API'yi](/rest/api/postgresql/configurations/createorupdate)kullanarak parametreleri değiştirebilirsiniz.

Azure [portalını](https://portal.azure.com)kullanma:

   1. PostgreSQL için Azure Veritabanı sunucunuzu seçin.
   2. Kenar çubuğunda **Sunucu Parametreleri'ni**seçin.
   3. Parametreyi `shared_preload_libraries` arayın.
   4. **pgaudit'i**seçin.
   5. Değişikliği uygulamak için sunucuyu yeniden başlatın.

   6. Bir istemci (psql gibi) kullanarak sunucunuza bağlanın ve pgAudit uzantısını etkinleştirin
      ```SQL
      CREATE EXTENSION pgaudit;
      ```

> [!TIP]
> Bir hata görürseniz, kaydedindikten `shared_preload_libraries`sonra sunucunuzu yeniden başlattığınızı onaylayın.

## <a name="pgaudit-settings"></a>pgAudit ayarları

pgAudit oturum veya nesne denetim günlüğü yapılandırmak için izin verir. [Oturum denetim günlüğü,](https://github.com/pgaudit/pgaudit/blob/master/README.md#session-audit-logging) yürütülen ifadelerin ayrıntılı günlüklerini yayır. [Nesne denetim günlüğü,](https://github.com/pgaudit/pgaudit/blob/master/README.md#object-audit-logging) belirli ilişkilere göre denetlenir. Bir veya her iki günlük türünü ayarlamayı seçebilirsiniz. 

> [!NOTE]
> pgAudit ayarları gloabally belirtilir ve bir veritabanı veya rol düzeyinde belirtilemez.

[pgAudit'i yükledikten](#installing-pgaudit)sonra, parametrelerini günlüğe kaydetmeye başlamak için yapılandırabilirsiniz. [pgAudit belgeleri](https://github.com/pgaudit/pgaudit/blob/master/README.md#settings) her parametrenin tanımını sağlar. Önce parametreleri test edin ve beklenen davranışı aldığınızı onaylayın.

> [!NOTE]
> A'ya ayar, `pgaudit.log_client` günlükleri dosyaya yazılmak yerine istemci işlemine (psql gibi) yönlendirir. Bu ayar genellikle devre dışı bırakılmalıdır. <br> <br>
> `pgaudit.log_level`yalnızca alırken `pgaudit.log_client` etkinleştirilir.

> [!NOTE]
> PostgreSQL için Azure `pgaudit.log` Veritabanı'nda, `-` pgAudit belgelerinde açıklandığı gibi (eksi) işaret kısayolu kullanılarak ayarlanamaz. Tüm gereken deyim sınıfları (READ, WRITE vb.) tek tek belirtilmelidir.

### <a name="audit-log-format"></a>Denetim günlüğü biçimi
Her denetim girişi, `AUDIT:` günlük satırının başına yakın olarak gösterilir. Girişin geri kalanının biçimi [pgAudit belgelerinde](https://github.com/pgaudit/pgaudit/blob/master/README.md#format)ayrıntılı olarak açıklanır.

Denetim gereksinimlerinizi karşılamak için başka alanlara ihtiyacınız varsa, `log_line_prefix`Postgres parametresini kullanın. `log_line_prefix`her Postgres günlük satırının başında çıktı olan bir dizedir. Örneğin, aşağıdaki `log_line_prefix` ayar zaman damgası, kullanıcı adı, veritabanı adı ve işlem kimliği sağlar:

```
t=%m u=%u db=%d pid=[%p]:
```

Hakkında `log_line_prefix`daha fazla bilgi edinmek için [PostgreSQL belgelerini](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-LINE-PREFIX)ziyaret edin.

### <a name="getting-started"></a>Başlarken
Hızlı bir şekilde başlamak `pgaudit.log` `WRITE`için, ''yi ayarlayın ve çıktıyı gözden geçirmek için günlüklerinizi açın. 

## <a name="viewing-audit-logs"></a>Denetim günlüklerini görüntüleme
.log dosyalarını kullanıyorsanız, denetim günlükleriniz PostgreSQL hata günlüklerinizle aynı dosyaya dahil edilir. Azure [portalından](howto-configure-server-logs-in-portal.md) veya [CLI'den](howto-configure-server-logs-using-cli.md)günlük dosyalarını indirebilirsiniz. 

Azure tanılama günlüğünü kullanıyorsanız, günlüklere erişme şekliniz seçtiğiniz bitiş noktasına bağlıdır. Azure Depolama için [günlükdepolama hesabı](../azure-monitor/platform/resource-logs-collect-storage.md) makalesine bakın. Olay Hub'ları için [akış Azure günlükleri](../azure-monitor/platform/resource-logs-stream-event-hubs.md) makalesine bakın.

Azure Monitör Günlükleri için günlükler seçtiğiniz çalışma alanına gönderilir. Postgres günlükleri Azure Diagnostics toplama modunu kullanarak **AzureDiagnostics** tablosundan sorgulanabilirler. Tablodaki alanlar aşağıda açıklanmıştır. [Azure Monitör Günlükleri sorgusuna](../azure-monitor/log-query/log-query-overview.md) genel bakışta sorgulama ve uyarı hakkında daha fazla bilgi edinin.

Başlamak için bu sorguyı kullanabilirsiniz. Uyarıları sorgulara göre yapılandırabilirsiniz.

Son gün içinde belirli bir sunucu için tüm Postgres günlüklerini arama
```
AzureDiagnostics
| where LogicalServerName_s == "myservername"
| where TimeGenerated > ago(1d) 
| where Message contains "AUDIT:"
```

## <a name="next-steps"></a>Sonraki adımlar
- [PostgreSQL için Azure Veritabanı'nda oturum açma hakkında bilgi edinin](concepts-server-logs.md)
- [Azure portalı, Azure](howto-configure-server-parameters-using-portal.md) [CLI](howto-configure-server-parameters-using-cli.md)veya [REST API'yi](/rest/api/postgresql/configurations/createorupdate)kullanarak parametreleri nasıl ayarlayabilirsiniz öğrenin.
