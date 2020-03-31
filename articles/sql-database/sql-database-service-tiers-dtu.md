---
title: Servis katmanları - DTU tabanlı satın alma modeli
description: Bilgi işlem ve depolama boyutları sağlamak için tek ve havuzlu veritabanları için DTU tabanlı satın alma modelindeki hizmet katmanları hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 11/26/2019
ms.openlocfilehash: 2f316e57e407a0588e77f56d6e1fbe8c19ba5fee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75562128"
---
# <a name="service-tiers-in-the-dtu-based-purchase-model"></a>DTU tabanlı satın alma modelindeki hizmet katmanları

DTU tabanlı satın alma modelindeki hizmet katmanları, sabit miktarda dahil edilmiş depolama alanı, yedeklemeler için sabit saklama süresi ve sabit fiyatiçeren bir dizi işlem boyutuyla ayırt edilir. DTU tabanlı satın alma modelindeki tüm hizmet katmanları, en az [kapalı kalma süresiyle](https://azure.microsoft.com/support/legal/sla/sql-database/v1_2/)bilgi işlem boyutlarını değiştirme esnekliği sağlar; ancak, bağlantının kısa bir süre için veritabanına kaybolduğu ve yeniden deneme mantığı kullanılarak azaltılabilen bir geçiş süresi boyunca vardır. Tek veritabanları ve elastik havuzlar, servis katmanı ve bilgi işlem boyutuna göre saatlik olarak faturalandırılır.

> [!IMPORTANT]
> SQL Database yönetilen örnek dtu tabanlı bir satın alma modelini desteklemez. Daha fazla bilgi için Azure [SQL Veritabanı Yönetilen Örneği'ne](sql-database-managed-instance.md)bakın.
> [!NOTE]
> vCore tabanlı hizmet katmanları hakkında bilgi için [vCore tabanlı hizmet katmanlarına](sql-database-service-tiers-vcore.md)bakın. DTU tabanlı hizmet katmanları ve vCore tabanlı hizmet katmanları nın farklılaştısı hakkında bilgi için [Azure SQL Veritabanı satın alma modellerine](sql-database-purchase-models.md)bakın.

## <a name="compare-the-dtu-based-service-tiers"></a>DTU tabanlı hizmet katmanlarını karşılaştırın

Bir hizmet katmanı seçmek öncelikle iş sürekliliğine, depolamaya ve performans gereksinimlerine bağlıdır.

||Temel|Standart|Premium|
| :-- | --: |--:| --:|
|Hedef iş yükü|Geliştirme ve üretim|Geliştirme ve üretim|Geliştirme ve üretim|
|Çalışma Süresi SLA|%99,99|%99,99|%99,99|
|Maksimum yedekleme tutma|7 gün|35 gün|35 gün|
|CPU|Düşük|Düşük, Orta, Yüksek|Orta, Yüksek|
|IO iş mesuliyeti (yaklaşık) |DTU başına 1-5 IOPS| DTU başına 1-5 IOPS | DTU başına 25 IOPS|
|IO gecikmesi (yaklaşık)|5 ms (okuma), 10 ms (yazma)|5 ms (okuma), 10 ms (yazma)|2 ms (okuma/yazma)|
|Sütun mağaza dizin oluşturma |Yok|S3 ve üzeri|Destekleniyor|
|Hafıza içi OLTP|Yok|Yok|Destekleniyor|
|||||

> [!IMPORTANT]
> Temel, Standart S0, S1 ve S2 hizmet katmanları birden az vCore (CPU) sağlar.  CPU yoğun iş yükleri için, S3 veya daha büyük bir hizmet katmanı önerilir. 
>
>Veri depolama yla ilgili olarak, Temel, Standart S0 ve S1 hizmet katmanları Standart Sayfa Blobs'a yerleştirilir. Standart Sayfa Blob'ları sabit disk sürücüsü (HDD) tabanlı depolama ortamını kullanır ve performans değişkenliğine daha az duyarlı geliştirme, sınama ve seyrek erişilen diğer iş yükleri için en uygun olanlardır.
>

> [!NOTE]
> Azure'u keşfetmek için Bir Azure ücretsiz hesabıyla birlikte Temel hizmet katmanında ücretsiz bir Azure SQL veritabanı alabilirsiniz. Bilgi için bkz. [Azure ücretsiz hesabınızla yönetilen bir bulut veritabanı oluşturun.](https://azure.microsoft.com/free/services/sql-database/)

## <a name="single-database-dtu-and-storage-limits"></a>Tek veritabanı DTU ve depolama sınırları

İşlem boyutları, tek veritabanları için Veritabanı İşlem Birimleri (DTS) ve elastik havuzlar için elastik Veritabanı İşlem Birimleri (elastik Veri Tabanı İşlem Birimleri) açısından ifade edilir. DTU ve eDTU'lar hakkında daha fazla şey için [DTU tabanlı satın alma modeline](sql-database-purchase-models.md#dtu-based-purchasing-model)bakın.

||Temel|Standart|Premium|
| :-- | --: | --: | --: |
| Maksimum depolama boyutu | 2 GB | 1 TB | 4 TB  |
| Maksimum DTUs | 5 | 3000 | 4000 | 
|||||

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri almak için bir veritabanını küçültmeniz gerekebilir. Daha fazla bilgi için Azure [SQL Veritabanı'nda dosya alanını yönet'e](sql-database-file-space-management.md)bakın.

## <a name="elastic-pool-edtu-storage-and-pooled-database-limits"></a>Elastik havuz eDTU, depolama ve havuzlu veritabanı sınırları

| | **Temel** | **Standart** | **Premium** |
| :-- | --: | --: | --: |
| Veritabanı başına maksimum depolama boyutu  | 2 GB | 1 TB | 1 TB |
| Havuz başına maksimum depolama boyutu | 156 GB | 4 TB | 4 TB |
| Veritabanı başına maksimum eDTUs | 5 | 3000 | 4000 |
| Havuz başına maksimum eDTUs | 1600 | 3000 | 4000 |
| Havuz başına maksimum veritabanları sayısı | 500  | 500 | 100 |
|||||

> [!IMPORTANT]
> Premium katmanında 1 TB'den fazla depolama şu anda Çin Doğu, Çin Kuzey, Almanya Orta, Almanya Kuzeydoğu, Batı Orta ABD, ABD DoD bölgeleri ve ABD Hükümeti Merkezi hariç tüm bölgelerde kullanılabilir. Bu bölgelerde, Premium katmandaki depolama limiti 1 TB ile sınırlıdır.  Daha fazla bilgi için [P11-P15 geçerli sınırlamaları'na](sql-database-single-database-scale.md#p11-and-p15-constraints-when-max-size-greater-than-1-tb)bakın.  
> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri almak için bir veritabanını küçültmeniz gerekebilir. Daha fazla bilgi için [Azure SQL Veritabanı'ndaki dosya alanını yönet'e](sql-database-file-space-management.md)bakın.

## <a name="dtu-benchmark"></a>DTÜ Kıyaslama

Her DTU ölçüsüyle ilişkili fiziksel özellikler (CPU, bellek, IO) gerçek veritabanı iş yükünü taklit eden bir kıyaslama kullanılarak kalibre edilir.

### <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Kıyaslama sonuçlarının gerçek dünya veritabanı performansıyla ilişkilendirilmesi

Tüm kriterlerin sadece temsili ve gösterge niteliğinde olduğunu anlamak önemlidir. Kıyaslama uygulaması ile elde edilen işlem oranları, diğer uygulamalarla elde edilebilenlerle aynı olmayacaktır. Kıyaslama, çeşitli tablolar ve veri türleri içeren bir şema ile çalışan farklı işlem türlerinin bir koleksiyonunu kapsar. Kıyaslama, tüm OLTP iş yüklerinde ortak olan aynı temel işlemleri uygularken, belirli bir veritabanı veya uygulama sınıfını temsil etmez. Kıyaslama nın amacı, bilgi işlem boyutları arasında yukarı veya aşağı ölçekleme yaparken beklenebilecek bir veritabanının göreli performansına makul bir kılavuz sağlamaktır. Gerçekte, veritabanları farklı boyutlarda ve karmaşıklıkta, iş yüklerinin farklı karışımları karşılaşma ve farklı şekillerde yanıt verecektir. Örneğin, IO yoğun bir uygulama IO eşiklerine daha erken çıkabilir veya CPU yoğun bir uygulama CPU sınırlarını daha erken vurabilir. Herhangi bir veritabanı artan yük altında kriter olarak aynı şekilde ölçeklenecek hiçbir garantisi yoktur.

Kıyaslama ve metodolojisi aşağıda daha ayrıntılı olarak açıklanmıştır.

### <a name="benchmark-summary"></a>Kıyaslama özeti

Kıyaslama, çevrimiçi işlem işleme (OLTP) iş yüklerinde en sık gerçekleşen temel veritabanı işlemlerinin bir karışımının performansını ölçer. Kıyaslama bulut bilgi işlem göz önünde bulundurularak tasarlanmış olsa da, veritabanı şeması, veri popülasyonu ve işlemler OLTP iş yüklerinde en sık kullanılan temel öğeleri geniş ölçüde temsil edecek şekilde tasarlanmıştır.

### <a name="schema"></a>Şema

Şema, geniş bir operasyon yelpazesini destekleyecek kadar çeşitlilik ve karmaşıklığa sahip olacak şekilde tasarlanmıştır. Kıyaslama altı tablodan oluşan bir veritabanına karşı çalışır. Tablolar üç kategoriye ayrılır: sabit boyutlu, ölçekleme ve büyüme. İki sabit boyutlu tablo vardır; üç ölçekleme tabloları; ve büyüyen bir masa. Sabit boyutlu tablolarda sabit sayıda satır vardır. Ölçekleme tabloları veritabanı performansıyla orantılı bir kardinalliğe sahiptir, ancak kıyaslama sırasında değişmez. Büyüyen tablo ilk yükte bir ölçekleme tablosu gibi boyutlandırılır, ancak satırlar eklenir ve silindikçe kıyaslama çalışma sırasında ki kardinallik değişiklikleri.

Şema, tamsayı, sayısal, karakter ve tarih/saat gibi veri türlerinin bir karışımını içerir. Şema birincil ve ikincil anahtarları içerir, ancak herhangi bir yabancı anahtarlar - yani, tablolar arasında hiçbir başvuru bütünlüğü kısıtlamaları vardır.

Bir veri oluşturma programı ilk veritabanı için veri oluşturur. İnteger ve sayısal veriler çeşitli stratejilerle oluşturulur. Bazı durumlarda, değerler bir aralık üzerinde rasgele dağıtılır. Diğer durumlarda, belirli bir dağılım korunur sağlamak için değerler kümesi rasgele permuted. Metin alanları gerçekçi görünümlü veriler üretmek için ağırlıklı bir sözcük listesinden oluşturulur.

Veritabanı bir "ölçek faktörü" temel alının. Ölçek faktörü (SF olarak kısaltılır) ölçeklendirme ve büyüyen tabloların önem düzeyibelirler. Aşağıda açıklandığı gibi Kullanıcılar ve Pacing bölümünde, veritabanı boyutu, kullanıcı sayısı ve maksimum performans hepsi birbiriyle orantılı olarak ölçeklendirilir.

### <a name="transactions"></a>İşlemler

İş yükü, aşağıdaki tabloda gösterildiği gibi dokuz işlem türünden oluşur. Her işlem, veritabanı altyapısıve sistem donanımındaki belirli bir sistem karakteristikkümesini vurgulamak ve diğer işlemlerden yüksek kontrastla birlikte tasarlanmıştır. Bu yaklaşım, farklı bileşenlerin genel performansa etkisini değerlendirmeyi kolaylaştırır. Örneğin, "Ağır Oku" hareketi diskten önemli sayıda okuma işlemi üretir.

| İşlem Türü | Açıklama |
| --- | --- |
| Lite'ı Oku |SEÇ; bellekte; salt okunur |
| Orta Oku |SEÇ; çoğunlukla bellekte; salt okunur |
| Ağır Oku |SEÇ; çoğunlukla bellekte değil; salt okunur |
| Lite'ı Güncelle |GÜNCELLEME; bellekte; okuma-yazma |
| Ağır'ı Güncelleştir |GÜNCELLEME; çoğunlukla bellekte değil; okuma-yazma |
| Lite Ekle |EKLE; bellekte; okuma-yazma |
| Ağır Ekle |EKLE; çoğunlukla bellekte değil; okuma-yazma |
| Sil |SIL; bellek tespin değil, bellek tespin karışımı; okuma-yazma |
| Cpu Ağır |SEÇ; bellekte; nispeten ağır CPU yükü; salt okunur |

### <a name="workload-mix"></a>İş yükü karışımı

İşlemler aşağıdaki genel karışımla birlikte ağırlıklı bir dağılımdan rasgele seçilir. Genel karışım yaklaşık 2:1 okuma/yazma oranına sahiptir.

| İşlem Türü | Karışım'ın %'si |
| --- | --- |
| Lite'ı Oku |35 |
| Orta Oku |20 |
| Ağır Oku |5 |
| Lite'ı Güncelle |20 |
| Ağır'ı Güncelleştir |3 |
| Lite Ekle |3 |
| Ağır Ekle |2 |
| Sil |2 |
| Cpu Ağır |10 |

### <a name="users-and-pacing"></a>Kullanıcılar ve pacing

Kıyaslama iş yükü, eşzamanlı kullanıcıların davranışlarını simüle etmek için bir dizi bağlantı boyunca hareketler gönderen bir araçtan yönlendirilir. Tüm bağlantılar ve hareketler makine tarafından oluşturulsa da, basitlik için bu bağlantılara "kullanıcı" diyoruz. Her kullanıcı diğer tüm kullanıcılardan bağımsız olarak faaliyet gösterse de, tüm kullanıcılar aşağıda gösterilen aynı adım döngüsünü gerçekleştirir:

1. Bir veritabanı bağlantısı kurun.
2. Çıkış sinyali verilene kadar tekrarlayın:
   - Rasgele bir hareket seçin (ağırlıklı bir dağıtımdan).
   - Seçili hareketi gerçekleştirin ve yanıt süresini ölçün.
   - Bir pacing gecikme bekleyin.
3. Veritabanı bağlantısını kapatın.
4. Çıkış.

Pacing gecikmesi (adım 2c) rasgele seçilir, ancak ortalama 1,0 saniye olan bir dağılım ile. Böylece her kullanıcı, ortalama olarak, saniyede en fazla bir işlem oluşturabilir.

### <a name="scaling-rules"></a>Ölçekleme kuralları

Kullanıcı sayısı veritabanı boyutuna göre belirlenir (ölçek-faktör birimlerinde). Her beş ölçek faktörlü birim için bir kullanıcı vardır. Pacing gecikmesi nedeniyle, bir kullanıcı ortalama olarak saniyede en fazla bir işlem oluşturabilir.

Örneğin, 500 (SF=500) veritabanında bir ölçek faktörü 100 kullanıcıya sahip olacak ve maksimum 100 TPS oranına ulaşabilir. Daha yüksek bir TPS oranı nı kullanmak için daha fazla kullanıcı ve daha büyük bir veritabanı gerektirir.

### <a name="measurement-duration"></a>Ölçüm süresi

Geçerli bir kıyaslama çalışması, en az bir saatlik sabit durum ölçüm süresi gerektirir.

### <a name="metrics"></a>Ölçümler

Kıyaslamadaki temel ölçümler, iş ve yanıt süresidir.

- İş mesuliyonu, kıyaslamadaki temel performans ölçüsüdür. Tüm işlem türleri sayma, zaman birimi başına hareketlerde işlenir.
- Yanıt süresi performans öngörülebilirliğinin bir ölçüsüdür. Yanıt süresi kısıtlaması hizmet sınıfına göre değişir ve daha yüksek hizmet sınıfları aşağıda gösterildiği gibi daha sıkı yanıt süresi gereksinimine sahip tir.

| Hizmet Sınıfı | Elde İbiş Ölçümü | Yanıt Süresi Gereksinimi |
| --- | --- | --- |
| Premium |Saniyedeki işlemler |0,5 saniyede 95. |
| Standart |Dakika başına işlemler |1.0 saniyede 90. |
| Temel |Saat başına işlemler |2.0 saniyede 80. |

## <a name="next-steps"></a>Sonraki adımlar

- Tek veritabanları için kullanılabilen belirli bilgi işlem boyutları ve depolama boyutu seçenekleri hakkında ayrıntılar için, [tek veritabanları için SQL Database DTU tabanlı kaynak sınırlarına](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes)bakın.
- Elastik havuzlar için kullanılabilen belirli bilgi işlem boyutları ve depolama boyutu seçenekleri hakkında ayrıntılı bilgi için SQL [Database DTU tabanlı kaynak sınırlarına](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes)bakın.
