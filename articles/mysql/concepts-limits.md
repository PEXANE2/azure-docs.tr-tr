---
title: Sınırlamalar-MySQL için Azure veritabanı
description: Bu makalede, bağlantı ve depolama altyapısı seçenekleri sayısı gibi bir MySQL için Azure veritabanı'nda sınırlamalar açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/9/2019
ms.openlocfilehash: 757a061bff72ca9fc34d408cd94cec9966d1157f
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77191109"
---
# <a name="limitations-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı'nda sınırlamaları
Aşağıdaki bölümlerde, kapasitesi, depolama altyapısı desteği, destek ayrıcalığına, veri işleme ifadesi desteği ve veritabanı hizmeti işlevsel sınırları açıklanmaktadır. Ayrıca bkz. MySQL veritabanı altyapısı için geçerli olan [genel sınırlamalar](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) .

## <a name="maximum-connections"></a>Bağlantı sayısı üst sınırı
Fiyatlandırma katmanı ve sanal çekirdek başına bağlantıları sayısı aşağıdaki gibidir: 

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**| **En fazla bağlantı sayısı**|
|---|---|---|
|Temel| 1| 50|
|Temel| 2| 100|
|Genel Amaçlı| 2| 600|
|Genel Amaçlı| 4| 1250|
|Genel Amaçlı| 8| 2500|
|Genel Amaçlı| 16| 5000|
|Genel Amaçlı| 32| 10000|
|Genel Amaçlı| 64| 20000|
|Bellek için İyileştirilmiş| 2| 1250|
|Bellek için İyileştirilmiş| 4| 2500|
|Bellek için İyileştirilmiş| 8| 5000|
|Bellek için İyileştirilmiş| 16| 10000|
|Bellek için İyileştirilmiş| 32| 20000|

Bağlantı sınırı aştıklarında aşağıdaki hata iletisini alabilirsiniz:
> 1040 (08004). hata: Çok fazla sayıda bağlantı

> [!IMPORTANT]
> En iyi deneyim için, bağlantıları verimli bir şekilde yönetmek üzere ProxySQL gibi bir bağlantı havuzlayıcı kullanmanızı öneririz.

MySQL 'e yeni istemci bağlantıları oluşturma zaman alır ve bir kez kurulduktan sonra bile bu bağlantılar veritabanı kaynaklarını kaplar. Çoğu uygulama, bu durumu çözer birçok kısa süreli bağlantı ister. Sonuç olarak gerçek iş yükünüz için daha az kaynak kullanılabilir ve performansı azaltıldı. Boştaki bağlantıları azaltan ve var olan bağlantıları yeniden kullanan bir bağlantı havuzlayıcı bunun önlenmesine yardımcı olur. ProxySQL 'i ayarlama hakkında bilgi edinmek için [Blog gönderimizi](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042)ziyaret edin.

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
- DBA rol: birçok sunucu parametreleri ve ayarları yanlışlıkla sunucu performansının düşmesine neden veya DBMS ACID özelliklerini negate. Bu nedenle, bir ürün düzeyinde SLA ve hizmet bütünlüğü korumak için bu hizmeti DBA rol kullanıma sunmuyor. Yeni bir veritabanı örneği oluşturulduğunda bu oluşturulur, varsayılan kullanıcı hesabı, veritabanı yönetilen örneğine DDL ve DML deyimleri çoğunu gerçekleştirmek bu kullanıcı sağlar. 
- Süper ayrıcalık: benzer [süper ayrıcalık](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) da kısıtlıdır.
- DEFINER: oluşturmak için süper ayrıcalıklar gerektirir ve kısıtlıdır. Bir yedekleme kullanarak veri içeri aktardıysanız, bir mysqldump gerçekleştirirken `CREATE DEFINER` komutlarını el ile veya `--skip-definer` komutunu kullanarak kaldırın.

## <a name="data-manipulation-statement-support"></a>Veri işleme ifadesi desteği

### <a name="supported"></a>Destekleniyor
- `LOAD DATA INFILE` desteklenir, ancak `[LOCAL]` parametresi belirtilmelidir ve bir UNC yoluna (SMB üzerinden bağlanmış Azure depolama) yönlendirilmelidir.

### <a name="unsupported"></a>Desteklenmeyen
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>İşlev sınırlamaları

### <a name="scale-operations"></a>Ölçeklendirme işlemleri
- Temel fiyatlandırma katmanları gelen ve giden dinamik ölçeklendirme şu anda desteklenmiyor.
- Sunucu depolama boyutunu küçültme desteklenmiyor.

### <a name="server-version-upgrades"></a>Sunucu sürümü yükseltme
- Ana veritabanı altyapısı sürümleri arasında otomatik geçişi şu anda desteklenmiyor. Bir sonraki ana sürüme yükseltmek isterseniz, bir dökümünü alın ve yeni altyapı sürümüyle oluşturulmuş bir sunucuya [geri yükleyin](./concepts-migrate-dump-restore.md) .

### <a name="point-in-time-restore"></a>belirli bir noktaya geri yükleme
- PITR özelliğini kullanırken, yeni sunucuya bağlı olduğu sunucusuyla aynı yapılandırmaları ile oluşturulur.
- Silinen bir sunucuya geri yükleme desteklenmiyor.

### <a name="vnet-service-endpoints"></a>Sanal ağ hizmet uç noktaları
- Yalnızca genel amaçlı ve bellek için iyileştirilmiş sunucuları için sanal ağ hizmet uç noktaları desteğidir.

### <a name="storage-size"></a>Depolama boyutu
- Fiyatlandırma Katmanı başına depolama boyutu sınırları için lütfen [fiyatlandırma katmanlarına](concepts-pricing-tiers.md) bakın.

## <a name="current-known-issues"></a>Bilinen geçerli sorunlar
- Bağlantı kurulduktan sonra MySQL sunucu örneğinde yanlış sunucu sürümünü görüntüler. Doğru sunucu örneği altyapısı sürümünü almak için `select version();` komutunu kullanın.

## <a name="next-steps"></a>Sonraki adımlar
- [Her hizmet katmanında kullanılabilen özellikler](concepts-pricing-tiers.md)
- [Desteklenen MySQL veritabanı sürümleri](concepts-supported-versions.md)
