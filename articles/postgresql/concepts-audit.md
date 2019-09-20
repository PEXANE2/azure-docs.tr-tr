---
title: PostgreSQL için Azure veritabanı 'nda pgAudit kullanarak günlüğü denetleme-tek sunucu
description: PostgreSQL için Azure veritabanı 'nda pgAudit denetim günlüğü için kavramlar-tek sunucu.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/18/2019
ms.openlocfilehash: 198ab5f567652a76d209168041f305b9da4d0b43
ms.sourcegitcommit: b03516d245c90bca8ffac59eb1db522a098fb5e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71147180"
---
# <a name="audit-logging-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'nda denetim günlüğü-tek sunucu

PostgreSQL için Azure veritabanı 'nda veritabanı etkinliklerini denetleme-tek sunucu, PostgreSQL Denetim Uzantısı aracılığıyla kullanılabilir: [pgaudit](https://www.pgaudit.org/). pgAudit ayrıntılı oturum ve/veya nesne denetim günlüğü sağlar.

> [!NOTE]
> pgAudit, PostgreSQL için Azure veritabanı 'nda önizlemededir.
> Uzantı yalnızca Genel Amaçlı ve bellek için Iyileştirilmiş sunucularda etkinleştirilebilir.

## <a name="usage-considerations"></a>Kullanım konuları
Varsayılan olarak, pgAudit günlük deyimleri, Postgres 'nin standart günlüğe kaydetme tesis kullanılarak normal günlük deyimleriyle birlikte yayınlanır. PostgreSQL için Azure veritabanı 'nda, bu. log dosyaları Azure portal veya CLı aracılığıyla indirilebilir. Dosya koleksiyonu için maksimum depolama alanı 1 GB ve her dosya en fazla yedi gün için kullanılabilir (varsayılan değer üç gündür). Bu hizmet, kısa süreli bir depolama seçeneğidir.

Alternatif olarak, tüm günlüklerin Azure Izleyici 'nin tanılama günlüğü hizmetine yayınlanacağını yapılandırabilirsiniz. Azure Izleyici tanılama günlüğünü etkinleştirirseniz, sizin seçiminize bağlı olarak günlüklerinizi Azure depolama, Event Hubs ve/veya Azure Izleyici günlüklerine otomatik olarak (JSON biçiminde) gönderilir.

PgAudit 'in etkinleştirilmesi, bir sunucuda, performans ve günlük depolama üzerinde bir etkisi olan büyük bir günlük kaydı oluşturur. Daha uzun vadeli depolama seçenekleri sunan Azure tanılama günlüğü hizmetini ve analiz ve uyarı özelliklerini kullanmanızı öneririz. Ek günlüğe kaydetmenin performans etkisini azaltmak için standart günlüğe kaydetmeyi kapatmanızı öneririz:

   1. Parametreyi `logging_collector` kapalı olarak ayarlayın. 
   2. Bu değişikliği uygulamak için sunucuyu yeniden başlatın.

Azure depolama, Event Hubs veya Azure Izleyici günlüklerine günlük kaydı ayarlamayı öğrenmek için [sunucu günlükleri makalesinin](concepts-server-logs.md)tanılama günlükleri bölümünü ziyaret edin.

## <a name="installing-pgaudit"></a>PgAudit yükleniyor

PgAudit 'ı yüklemek için sunucunun paylaşılan önyükleme kitaplıklarına dahil etmeniz gerekir. Postgres 'nin `shared_preload_libraries` parametresinin bir değişikliği, sunucu yeniden başlatmanın etkili olmasını gerektirir. [Azure Portal](howto-configure-server-parameters-using-portal.md), [Azure CLI](howto-configure-server-parameters-using-cli.md)veya [REST API](/rest/api/postgresql/configurations/createorupdate)kullanarak parametreleri değiştirebilirsiniz.

[Azure Portal](https://portal.azure.com)kullanarak:

   1. PostgreSQL için Azure Veritabanı sunucunuzu seçin.
   2. Kenar çubuğunda **sunucu parametreleri**' ni seçin.
   3. `shared_preload_libraries` Parametresi için arama yapın.
   4. **Pgaudit**öğesini seçin.
   5. Değişikliği uygulamak için sunucuyu yeniden başlatın.

   6. Bir istemciyi (psql gibi) kullanarak sunucunuza bağlanın ve pgAudit uzantısını etkinleştirin
      ```SQL
      CREATE EXTENSION pgaudit;
      ```

> [!TIP]
> Bir hata görürseniz, kaydettikten `shared_preload_libraries`sonra sunucunuzu yeniden başlattığınızdan emin olun.

## <a name="pgaudit-settings"></a>pgAudit ayarları

pgAudit oturum veya nesne denetim günlüğünü yapılandırmanızı sağlar. [Oturum denetim günlüğü](https://github.com/pgaudit/pgaudit/blob/master/README.md#session-audit-logging) , yürütülen deyimlerin ayrıntılı günlüklerini yayar. [Nesne denetim günlüğü](https://github.com/pgaudit/pgaudit/blob/master/README.md#object-audit-logging) , belirli ilişkilerin kapsama alınmış bir denetim. Günlük kayıt türlerinin birini veya ikisini birden ayarlamayı seçebilirsiniz. 

> [!NOTE]
> pgAudit ayarları, genel olarak belirtilmiştir ve bir veritabanı veya rol düzeyinde belirtilemez.

[PgAudit](#installing-pgaudit)' i yükledikten sonra, parametrelerini günlüğe kaydetmeyi başlatacak şekilde yapılandırabilirsiniz. [Pgaudit belgeleri](https://github.com/pgaudit/pgaudit/blob/master/README.md#settings) her parametrenin tanımını sağlar. Önce parametreleri test edin ve beklenen davranışı aldığınızı onaylayın.

> [!NOTE]
> Olarak `pgaudit.log_client` ayarlandığında, günlükleri dosyaya yazılması yerine bir istemci işlemine (psql gibi) yönlendirir. Bu ayar genellikle devre dışı bırakılmalıdır.

> [!NOTE]
> `pgaudit.log_level`yalnızca açık olduğunda `pgaudit.log_client` etkindir. Ayrıca, Azure Portal Şu anda bir hata `pgaudit.log_level`var: bir Birleşik giriş kutusu gösterilmekte ve birden çok düzeyin seçilediğine engel olabilir. Ancak yalnızca bir düzey seçilmelidir. 

> [!NOTE]
> PostgreSQL için Azure veritabanı 'nda, `pgaudit.log` pgaudit belgelerinde açıklandığı şekilde `-` (eksi) işareti kısayolu kullanılarak ayarlanamaz. Tüm gerekli ifade sınıfları (okuma, yazma vb.) tek tek belirtilmelidir.

### <a name="audit-log-format"></a>Denetim günlüğü biçimi
Her denetim girişi, günlük satırının `AUDIT:` başlangıcında yakınında gösterilir. Girişin geri kalanının biçimi [Pgaudit belgelerinde](https://github.com/pgaudit/pgaudit/blob/master/README.md#format)ayrıntılıdır.

Denetim gereksinimlerinizi karşılamak için başka bir alana ihtiyacınız varsa, Postgres parametresini `log_line_prefix`kullanın. `log_line_prefix`Her Postgres günlük satırının başlangıcında çıktı olan bir dizedir. Örneğin, aşağıdaki `log_line_prefix` ayar zaman damgası, Kullanıcı adı, veritabanı adı ve işlem kimliği sağlar:

```
t=%m u=%u db=%d pid=[%p]:
```

Hakkında `log_line_prefix`daha fazla bilgi edinmek için [PostgreSQL belgelerini](https://www.postgresql.org/docs/current/runtime-config-logging.html#GUC-LOG-LINE-PREFIX)ziyaret edin.

### <a name="getting-started"></a>Başlarken
Hızlıca kullanmaya başlamak için, olarak `pgaudit.log` `WRITE`ayarlayın ve çıktıyı gözden geçirmek için günlüklerinizi açın. 


## <a name="next-steps"></a>Sonraki adımlar
- [PostgreSQL için Azure veritabanı 'nda günlüğe kaydetme hakkında bilgi edinin](concepts-server-logs.md)
- [Azure Portal](howto-configure-server-parameters-using-portal.md), [Azure CLI](howto-configure-server-parameters-using-cli.md)veya [REST API](/rest/api/postgresql/configurations/createorupdate)kullanarak parametrelerin nasıl ayarlanacağını öğrenin.
