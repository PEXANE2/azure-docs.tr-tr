---
title: PostgreSQL için dağıtım sütunlarını seçin – Hyperscale (Citus) - Azure Veritabanı
description: PostgreSQL için Azure Veritabanı'nda ortak hiper ölçekli senaryolarda dağıtım sütunlarını nasıl seçeceğinizi öğrenin.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 8ced9767d81affceef851820ee587f4f3dd24deb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74975678"
---
# <a name="choose-distribution-columns-in-azure-database-for-postgresql--hyperscale-citus"></a>PostgreSQL - Hyperscale (Citus) için Azure Veritabanı'nda dağıtım sütunlarını seçin

Her tablonun dağıtım sütununun seçilmesi, yapacağınız en önemli modelleme kararlarından biridir. PostgreSQL için Azure Veritabanı – Hyperscale (Citus), satırların dağıtım sütununun değerine bağlı olarak satırları kırıklar halinde depolar.

Doğru seçim, ilgili verileri aynı fiziksel düğümlerde bir araya getirir ve sorguları hızlı yapar ve tüm SQL özellikleri için destek ekler. Yanlış bir seçim, sistemin yavaş çalışmasını sağlar ve düğümler boyunca tüm SQL özelliklerini desteklemez.

Bu makalede, en yaygın iki Hyperscale (Citus) senaryoları için dağıtım sütunu ipuçları verir.

### <a name="multi-tenant-apps"></a>Çok kiracılı uygulamalar

Çok kiracılı mimari, sunucu grubundaki düğümler arasında sorguları dağıtmak için hiyerarşik veritabanı modelleme biçimi kullanır. Veri hiyerarşisinin üst bölümü kiracı *kimliği* olarak bilinir ve her tablodaki bir sütunda depolanması gerekir.

Hyperscale (Citus), hangi kiracı kimliğini içerdiklerini görmek için sorguları inceler ve eşleşen tablo parçasını bulur. Sorguyu, parçayı içeren tek bir alt düğüme yönlendirir. Aynı düğüme yerleştirilen tüm ilgili verileri içeren bir sorguyu çalıştırmaya colocation denir.

Aşağıdaki diyagram, çok kiracılı veri modelinde birlikte konumu göstermektedir. Her biri `account_id`. Gölgeli kutular kırıkları temsil eder. Yeşil kırıklar bir işçi düğümünde birlikte depolanır ve mavi kırıklar başka bir işçi düğümünde depolanır. Her iki tablo da aynı hesap\_kimliğiyle sınırlı yken, Hesaplar ve Kampanyalar arasındaki birleştirme sorgusunun tek bir düğümde gerekli tüm verileri nasıl bir arada oluşturduğuna dikkat edin.

![Çok kiracılı birlikte konumlandırma](media/concepts-hyperscale-choosing-distribution-column/multi-tenant-colocation.png)

Bu tasarımı kendi şemanızda uygulamak için, uygulamanızda kiracıyı neyin oluşturduğunu belirleyin. Yaygın örnekler şirket, hesap, kuruluş veya müşteriyi içerir. Sütun adı gibi `company_id` bir `customer_id`şey olacak ya da . Sorgularınızın her birini inceleyin ve kendinize sorun, aynı kiracı kimliğine sahip satırları içeren tüm tabloları kısıtlamak için ek WHERE yan tümceleri olsaydı işe yarar mıydı?
Çok kiracılı modeldeki sorgular kiracıya kadar kapsamlıdır. Örneğin, satış veya stoksorguları belirli bir mağaza içinde kapsama.

#### <a name="best-practices"></a>En iyi uygulamalar

-   **Dağıtılmış tabloları ortak\_bir kiracı kimliği sütununa göre bölme.** Örneğin, kiracıların şirket olduğu bir SaaS uygulamasında, kiracı\_kimliği büyük\_olasılıkla şirket kimliği olabilir.
-   **Küçük çapraz kiracı tablolarını başvuru tablolarına dönüştürün.** Birden çok kiracı küçük bir bilgi tablosunu paylaştığında, bunu başvuru tablosu olarak dağıtın.
-   **Tüm uygulama sorgularını kiracı\_kimliğine göre filtreleyin.** Her sorgu, aynı anda bir kiracı için bilgi istemelidir.

Bu tür bir uygulamanın nasıl oluşturulabildiğini öğrenmek için [çok kiracılı öğreticiyi](./tutorial-design-database-hyperscale-multi-tenant.md) okuyun.

### <a name="real-time-apps"></a>Gerçek zamanlı uygulamalar

Çok kiracılı mimari hiyerarşik bir yapı sunar ve kiracı başına sorguları yönlendirmek için veri birliş kullanır. Buna karşılık, gerçek zamanlı mimariler son derece paralel işleme elde etmek için verilerinin belirli dağıtım özelliklerine bağlıdır.

"Entity ID"i, gerçek zamanlı modeldeki dağıtım sütunları için bir terim olarak kullanırız. Tipik varlıklar kullanıcılar, ana bilgisayarlar veya aygıtlardır.

Gerçek zamanlı sorgular genellikle tarihe veya kategoriye göre gruplanmış sayısal toplamlar ister. Hyperscale (Citus) bu sorguları kısmi sonuçlar için her parçaya gönderir ve koordinatör düğümde son yanıtı bir araya getirer. Sorgular, mümkün olduğunca çok düğüm katkıda bulununca ve tek bir düğüm orantısız miktarda çalışma yapmak zorunda olmadığında en hızlı şekilde çalışır.

#### <a name="best-practices"></a>En iyi uygulamalar

-   **Dağıtım sütunu olarak yüksek kardinallik olan bir sütun seçin.** Karşılaştırma için, Yeni, Ücretli ve Sevk değerleri olan bir sipariş tablosundaki Durum alanı, dağıtım sütununun zayıf bir seçimdir. Yalnızca verileri tutabilecek parça sayısını ve onu işleyebilir düğüm sayısını sınırlayan birkaç değeri varsayar. Yüksek kardinallik li sütunlar arasında, grup bazında veya birleştirme anahtarları olarak sık lıkla kullanılan sütunları seçmek de iyidir.
-   **Eşit dağılımlı bir sütun seçin.** Belirli ortak değerlere eğrilenmiş bir sütunda bir tablo dağıtırsanız, tablodaki veriler belirli kırıklarda birikir. Bu parçaları tutan düğümler diğer düğümlerden daha fazla iş yapıyor.
-   **Olgu ve boyut tablolarını ortak sütunlarına dağıtın.**
    Durum tablonuzda yalnızca bir dağıtım anahtarı olabilir. Başka bir anahtarda birleşen tablolar gerçek tablosuyla birlikte yer almaz. Ne sıklıkta birleştiğine ve birleşen satırların boyutuna bağlı olarak bir araya gelmek için bir boyut seçin.
-   **Bazı boyut tablolarını başvuru tablolarına dönüştürün.** Bir boyut tablosu olgu tablosuyla birlikte bulunamazsa, boyut tablosunun kopyalarını başvuru tablosu biçimindeki düğümlerin tümüne dağıtarak sorgu performansını artırabilirsiniz.

Bu tür bir uygulamanın nasıl oluşturulabildiğini öğrenmek için [gerçek zamanlı pano öğreticisini](./tutorial-design-database-hyperscale-realtime.md) okuyun.

### <a name="time-series-data"></a>Zaman serisi verileri

Zaman serisi iş yükünde, uygulamalar eski bilgileri arşivlerken son bilgileri sorgular.

Hyperscale'de (Citus) zaman serisi bilgilerini modellemede en yaygın hata zaman damgasının kendisini dağıtım sütunu olarak kullanmaktır. Zamana dayalı karma dağılım, zaman aralıklarını parçalar halinde bir arada tutmak yerine rastgele görünen zamanları farklı parçalara dağıtır. Zaman içeren sorgular genellikle zaman aralıkları başvuru, örneğin, en son veriler. Bu tür karma dağıtım, ağ yüküne yol açar.

#### <a name="best-practices"></a>En iyi uygulamalar

-   **Dağıtım sütunu olarak bir zaman damgası seçmeyin.** Farklı bir dağıtım sütunu seçin. Çok kiracılı bir uygulamada, kiracı kimliğini veya gerçek zamanlı bir uygulamada varlık kimliğini kullanın.
-   **Bunun yerine postgreSQL tablo bölümleme kullanın.** Her tabloda farklı zaman aralıkları içeren birden çok devralınan tabloya sıralı büyük bir zaman sıralı veri tablosunu kırmak için tablo bölümlemesini kullanın. Hiperölçek 'de (Citus) Postgres bölümlü bir tablo dağıtmak, devralınan tablolar için kırıklar oluşturur.

Bu tür bir uygulamanın nasıl oluşturulabildiğini öğrenmek için [zaman serisi öğreticisini](https://aka.ms/hyperscale-tutorial-timeseries) okuyun.

## <a name="next-steps"></a>Sonraki adımlar
- Dağıtılmış veriler arasında [birlikte konumlandırmanın](concepts-hyperscale-colocation.md) sorguların hızlı çalışmasına nasıl yardımcı olduğunu öğrenin.
