---
title: Sınırlamalar-MySQL için Azure veritabanı
description: Bu makalede, MySQL için Azure veritabanı 'nda bağlantı ve depolama motoru seçeneklerinin sayısı gibi sınırlamalar açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 6/25/2020
ms.openlocfilehash: c562b8a82ef21e78eccad2c2ed6159251056f4fc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85392701"
---
# <a name="limitations-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı sınırlamaları
Aşağıdaki bölümlerde kapasiteyi, depolama altyapısı desteğini, ayrıcalık desteğini, veri işleme ekstresi desteğini ve veritabanı hizmetindeki işlev sınırlarını anlatmaktadır. Ayrıca bkz. MySQL veritabanı altyapısı için geçerli olan [genel sınırlamalar](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) .

## <a name="server-parameters"></a>Sunucu parametreleri

> [!NOTE]
> Ve gibi sunucu parametreleri için Min/Max değerlerini arıyorsanız `max_connections` `innodb_buffer_pool_size` , bu bilgiler **[sunucu parametreleri](./concepts-server-parameters.md)** makalesine taşınır.

MySQL için Azure veritabanı, sunucu parametrelerinin değerlerini ayarlamayı destekler. Bazı parametrelerin en küçük ve en büyük değeri (örn. `max_connections`, `join_buffer_size` , `query_cache_size` ), sunucunun fiyatlandırma katmanı ve sanal çekirdekleri tarafından belirlenir. Bu sınırlar hakkında daha fazla bilgi için [sunucu parametrelerine](./concepts-server-parameters.md) bakın.

İlk dağıtımdan sonra, MySQL için Azure Server, saat dilimi bilgileri için sistem tabloları içerir, ancak bu tablolar doldurulmaz. Saat dilimi tabloları, `mysql.az_load_timezone` MySQL komut satırı veya MySQL çalışma ekranı gibi bir araçtan saklı yordam çağırarak doldurulabilirler. Saklı yordamı çağırma ve küresel veya oturum düzeyi saat dilimlerini ayarlama hakkında [Azure Portal](howto-server-parameters.md#working-with-the-time-zone-parameter) veya [Azure CLI](howto-configure-server-parameters-using-cli.md#working-with-the-time-zone-parameter) makalelerine bakın.

## <a name="storage-engine-support"></a>Depolama altyapısı desteği

### <a name="supported"></a>Destekleniyor
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [BELLEK](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Desteklenmeyen
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [KARA DELIK](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARŞIVLIYORSANıZ](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [Federasyon](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Ayrıcalık desteği

### <a name="unsupported"></a>Desteklenmeyen
- DBA rolü: çok sayıda sunucu parametresi ve ayarı, DBMS 'nin sunucu performansını veya Negate ACID özelliklerini yanlışlıkla düşürebilir. Bu nedenle, hizmet bütünlüğünü ve SLA 'yı bir ürün düzeyinde sürdürmek için, bu hizmet DBA rolünü kullanıma sunmaz. Yeni bir veritabanı örneği oluşturulduğunda oluşturulan varsayılan kullanıcı hesabı, bu kullanıcının yönetilen veritabanı örneğinde DDL ve DML deyimlerinin çoğunu gerçekleştirmesini sağlar. 
- Süper ayrıcalık: benzer [süper ayrıcalık](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) da kısıtlıdır.
- DEFINER: oluşturmak için süper ayrıcalıklar gerektirir ve kısıtlıdır. Bir yedekleme kullanarak veri içeri aktardıysanız, `CREATE DEFINER` komutları el ile veya `--skip-definer` bir mysqldump gerçekleştirirken komutunu kullanarak kaldırın.


## <a name="data-manipulation-statement-support"></a>Veri işleme ekstresi desteği

### <a name="supported"></a>Destekleniyor
- `LOAD DATA INFILE`desteklenir, ancak `[LOCAL]` parametresi belirtilmelidir ve bır UNC yoluna (SMB üzerinden bağlanmış Azure Storage) yönlendirilmelidir.

### <a name="unsupported"></a>Desteklenmeyen
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>İşlevsel sınırlamalar

### <a name="scale-operations"></a>Ölçeklendirme işlemleri
- Temel fiyatlandırma katmanlarına ve bu katmanlara dinamik ölçeklendirme Şu anda desteklenmiyor.
- Sunucu depolama boyutunun düşürülmesi desteklenmiyor.

### <a name="server-version-upgrades"></a>Sunucu sürümü yükseltmeleri
- Ana veritabanı altyapısı sürümleri arasında otomatik geçiş Şu anda desteklenmiyor. Bir sonraki ana sürüme yükseltmek isterseniz, bir dökümünü alın ve yeni altyapı sürümüyle oluşturulmuş bir sunucuya [geri yükleyin](./concepts-migrate-dump-restore.md) .

### <a name="point-in-time-restore"></a>belirli bir noktaya geri yükleme
- INR özelliği kullanılırken yeni sunucu, temel aldığı sunucuyla aynı yapılandırmalara sahip olarak oluşturulur.
- Silinen bir sunucunun geri yüklenmesi desteklenmez.

### <a name="vnet-service-endpoints"></a>Sanal Ağ hizmet uç noktaları
- VNet hizmet uç noktaları için destek yalnızca Genel Amaçlı ve bellek için Iyileştirilmiş sunucular içindir.

### <a name="storage-size"></a>Depolama boyutu
- Fiyatlandırma Katmanı başına depolama boyutu sınırları için lütfen [fiyatlandırma katmanlarına](concepts-pricing-tiers.md) bakın.

## <a name="current-known-issues"></a>Bilinen geçerli sorunlar
- MySQL Server örneği bağlantı kurulduktan sonra yanlış sunucu sürümünü görüntülüyor. Doğru sunucu örneği altyapısı sürümünü almak için `select version();` komutunu kullanın.

## <a name="next-steps"></a>Sonraki adımlar
- [Her hizmet katmanında kullanılabilen özellikler](concepts-pricing-tiers.md)
- [Desteklenen MySQL veritabanı sürümleri](concepts-supported-versions.md)
