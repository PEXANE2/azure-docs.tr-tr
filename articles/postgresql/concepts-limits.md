---
title: Limitler - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL için Azure Veritabanı'ndaki bağlantı sayısı ve depolama motoru seçenekleri gibi Tek Sunucu sınırları açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/28/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: 047e722a0e0ade60d1eb93a48e37333fffafd674
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76836465"
---
# <a name="limits-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanında Sınırlar - Tek Sunucu
Aşağıdaki bölümlerde veritabanı hizmetinde kapasite ve işlevsel sınırlar açıklanmıştır. Kaynak (bilgi işlem, bellek, depolama) katmanları hakkında bilgi edinmek istiyorsanız, [fiyatlandırma katmanları](concepts-pricing-tiers.md) makalesine bakın.


## <a name="maximum-connections"></a>En fazla bağlantı sayısı
Fiyatlandırma katmanı ve vCore'lar başına maksimum bağlantı sayısı aşağıda gösterilmiştir. Azure sistemi, PostgreSQL sunucusu için Azure Veritabanı'nı izlemek için beş bağlantı gerektirir. 

|**Fiyatlandırma Katmanı**| **vCore(lar)**| **En Çok Bağlantı** | **Max Kullanıcı Bağlantıları** |
|---|---|---|---|
|Temel| 1| 55 | 50|
|Temel| 2| 105 | 100|
|Genel Amaçlı| 2| 150| 145|
|Genel Amaçlı| 4| 250| 245|
|Genel Amaçlı| 8| 480| 475|
|Genel Amaçlı| 16| 950| 945|
|Genel Amaçlı| 32| 1500| 1495|
|Genel Amaçlı| 64| 1900| 1895|
|Bellek İçin İyileştirilmiş| 2| 300| 295|
|Bellek İçin İyileştirilmiş| 4| 500| 495|
|Bellek İçin İyileştirilmiş| 8| 960| 955|
|Bellek İçin İyileştirilmiş| 16| 1900| 1895|
|Bellek İçin İyileştirilmiş| 32| 1987| 1982|

Bağlantılar sınırı aştığında, aşağıdaki hatayı alabilirsiniz:
> FATAL: üzgünüm, zaten çok fazla müşteri

> [!IMPORTANT]
> En iyi deneyim için, bağlantıları verimli bir şekilde yönetmek için pgBouncer gibi bir bağlantı havuzu kullanmanızı öneririz.

Bir PostgreSQL bağlantısı, hatta boşta, bellek yaklaşık 10MB kaplayabilir. Ayrıca, yeni bağlantılar oluşturmak zaman alır. Çoğu uygulama, bu durumu biraraya getiren birçok kısa süreli bağlantı isteğinde dir. Sonuç, performansın düşmesine yol açan gerçek iş yükünüz için daha az kaynak kullanılabilir. Boşta kalan bağlantıları azaltan ve varolan bağlantıları yeniden kullanan bir bağlantı havuzu bunu önlemeye yardımcı olur. Daha fazla bilgi için [blog gönderimizi](https://techcommunity.microsoft.com/t5/azure-database-for-postgresql/not-all-postgres-connection-pooling-is-equal/ba-p/825717)ziyaret edin.

## <a name="functional-limitations"></a>Fonksiyonel sınırlamalar
### <a name="scale-operations"></a>Ölçek işlemleri
- Temel fiyatlandırma katmanlarına ve temel fiyatlandırma katmanlarından dinamik ölçeklendirme şu anda desteklenmez.
- Azalan sunucu depolama boyutu şu anda desteklenmiyor.

### <a name="server-version-upgrades"></a>Sunucu sürümü yükseltmeleri
- Ana veritabanı altyapısı sürümleri arasında otomatik geçiş şu anda desteklenmez. Bir sonraki ana sürüme yükseltmek istiyorsanız, [bir döküm](./howto-migrate-using-dump-and-restore.md) alın ve yeni motor sürümü ile oluşturulan bir sunucuya geri yükleyin.

> PostgreSQL sürüm 10'dan önce, [PostgreSQL sürüm lendirme ilkesinin](https://www.postgresql.org/support/versioning/) birinci _veya_ ikinci sayıda bir artış olarak önemli bir _sürüm_ yükseltmesi olarak kabul edildiğini unutmayın (örneğin, 9,5 ile 9,6 önemli _bir_ sürüm yükseltmesi olarak kabul edilir).
> Sürüm 10 itibariyle, yalnızca ilk sayıdaki bir değişiklik ana sürüm yükseltmesi olarak kabul edilir (örneğin, 10.0 ila 10.1 _küçük_ bir sürüm yükseltmesidir ve 10'dan 11'e önemli _bir_ sürüm yükseltmesidir).

### <a name="vnet-service-endpoints"></a>Sanal Ağ hizmet uç noktaları
- VNet hizmet uç noktaları için destek yalnızca Genel Amaç ve Bellek Optimize Edilmiş sunucular içindir.

### <a name="restoring-a-server"></a>Sunucunun geri dinlenmesi
- PITR özelliğini kullanırken, yeni sunucu dayandığı sunucuyla aynı fiyatlandırma katmanı yapılandırmalarıyla oluşturulur.
- Geri yükleme sırasında oluşturulan yeni sunucu, özgün sunucuda var olan güvenlik duvarı kurallarına sahip değildir. Bu yeni sunucu için güvenlik duvarı kurallarının ayrı olarak ayarlanılması gerekir.
- Silinen bir sunucunun geri dinlenmesi desteklenmez.

### <a name="utf-8-characters-on-windows"></a>Windows'da UTF-8 karakterleri
- Bazı senaryolarda UTF-8 karakterleri, Windows'daki açık kaynak postgreSQL'de tam olarak desteklenmez ve bu da PostgreSQL için Azure Veritabanı'nı etkiler. Daha fazla bilgi için [postgresql-arşivinde #15476 Hata'daki](https://www.postgresql-archive.org/BUG-15476-Problem-on-show-trgm-with-4-byte-UTF-8-characters-td6056677.html) iş parçacığına bakın.

## <a name="next-steps"></a>Sonraki adımlar
- [Her fiyatlandırma katmanında nelerin mevcut olduğunu](concepts-pricing-tiers.md) anlama
- Desteklenen [PostgreSQL Veritabanı Sürümleri](concepts-supported-versions.md) hakkında bilgi edinin
- [Azure portalını kullanarak PostgreSQL için Azure Veritabanı'ndaki bir sunucuyu nasıl yedekleyip geri yükleyin](howto-restore-server-portal.md)
