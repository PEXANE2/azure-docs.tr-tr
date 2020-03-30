---
title: Azure Cosmos DB faturanızı anlama
description: Bu makalede, Azure Cosmos DB faturanızı bazı örneklerle nasıl anlayacağınızı açıklanmaktadır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/01/2019
ms.reviewer: sngun
ms.openlocfilehash: 5954c8eda370c0734985c47cfff6d073f5d76d17
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80258031"
---
# <a name="understand-your-azure-cosmos-db-bill"></a>Azure Cosmos DB faturanızı anlama

Azure Cosmos DB, tam olarak yönetilen bir bulut tabanlı veritabanı hizmeti olarak, yalnızca sağlanan iş ortası ve tüketilen depolama için şarj ederek faturalandırmayı kolaylaştırır. Şirket içi veya IaaS tarafından barındırılan alternatiflerle karşılaştırıldığında ek lisans ücreti, donanım, hizmet maliyeti veya tesis maliyeti yoktur. Azure Cosmos DB'nin çok bölgeli özelliklerini göz önünde bulundurursak, veritabanı hizmeti mevcut şirket içi veya IaaS çözümlerine kıyasla maliyetlerde önemli bir azalma sağlar.

Azure Cosmos DB ile, sağlanan iş başına ve tüketilen depolama alanına göre saatlik olarak faturalandırılırsınız. Sağlanan üretim için, faturalandırma birimi saat başına 100 RU/sn olup, standart genel fiyatlandırmayı varsayarsak saat başına 0,008 ABD doları olarak ücretlendirilir, [Fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/)bakın. Tüketilen depolama için, ayda 1 GB depolama başına 0,25 ABD doları faturalandırılır, [Fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/)bakın. 

Bu makale, aylık faturada gördüğünüz ayrıntıları anlamanıza yardımcı olacak örnekleri içerir. Azure Cosmos kapsayıcılarınızda farklı miktarda aktarım hızı sağlandıysa, bunları birden fazla bölgeye yaydıysanız veya farklı bir öğe için bir aydan daha fazla süre boyunca çalışıyorlarsa örneklerde gösterilen sayılar farklı olabilir.

> [!NOTE]
> Faturalandırma, 60 dakikalık bir süre için değil, duvar saati saatinin herhangi bir bölümü içindir.

## <a name="billing-examples"></a>Faturalama örnekleri

### <a name="billing-example---throughput-on-a-container-full-month"></a>Faturalama örneği - bir kapsayıcıda iş ortası (tam ay)

* Bir kapsayıcıda 1.000 RU/sn'lik bir iş bünyesini yapılandırdığınızı varsayalım ve bu iş 24 saat * ay = 720 saat için 30 gün boyunca bulunur.  

* 1.000 RU/sn, konteynerlerin var olduğu her saat için saatte 10 00 RU/sn 10 birimdir (yani 1.000/100 = 10). 

* Saat başına 10 birimin $0,008 (saat başına 100 RU/sn başına) = 0,08 $/saat maliyetiyle çarpılması. 

* Saat başına 0,08 TL'yi aydaki saat sayısıyla çarpmak 0,08 TL * 24 saat * 30 gün = ay için 57,60 TL'ye eşittir.  

* Toplam aylık fatura 7.200 adet (100 RUs), hangi 57,60 $ mal olacak gösterecektir.

### <a name="billing-example---throughput-on-a-container-partial-month"></a>Faturalama örneği - bir kapsayıcıda iş ortası (kısmi ay)

* 2.500 RU/sn'lik tedarik li bir konteyner oluşturduğumuzvarsayalım. Kapsayıcı ay boyunca 24 saat yaşar (örneğin, oluşturduktan 24 saat sonra siliyoruz).  

* Sonra tasarı (2.500 RU / sn / 100 RU / sn / birim * 24 saat) üzerinde 600 adet görürsünüz. Maliyet 4,80 $ (600 adet * 0,008 $ / birim) olacaktır.

* Ayın toplam faturası 4,80 $ olacaktır.

### <a name="billing-rate-if-storage-size-changes"></a>Depolama boyutu değişirse faturalandırma oranı

Depolama kapasitesi, aylık bir dönemde GB'de depolanan maksimum saatlik veri miktarı nın birimleri olarak faturalandırılır. Örneğin, ayın yarısı için 100 GB ve ayın ikinci yarısı için 50 GB depolama alanı kullandıysanız, o ay boyunca 75 GB'lık eşdeğer bir depolama alanı için faturalandırılırsınız.

### <a name="billing-rate-when-container-or-a-set-of-containers-are-active-for-less-than-an-hour"></a>Konteyner veya kapsayıcı kümesi bir saatten kısa bir süre etkin olduğunda faturalandırma oranı

Kullanım ne olursa olsun veya kapsayıcı veya veritabanı bir saatten kısa bir süre için etkin sayılsa da, kapsayıcının veya veritabanının bulunduğu her saat için sabit ücret faturalandırılırsınız. Örneğin, bir kapsayıcı veya veritabanı oluşturur ve 5 dakika sonra silerseniz, faturanız bir saat içerir.

### <a name="billing-rate-when-throughput-on-a-container-or-database-scales-updown"></a>Bir kapsayıcı veya veritabanındaki iş çıktıları yukarı/aşağı ölçeklendiğinde faturalandırma oranı

Sağlanan iş bünyesini 09:30'da 400 RU/sn'den 1.000 RU/sn'ye ve daha sonra 10:45'te 400 RU/sn'ye düşürürseniz, 1.000 RU/sn'den iki saat ücretlendirilirsiniz. 

Bir konteyner veya konteyner kümesi için verilen iş belini 100-K RU/sn'den 200-K RU/sn'ye kadar 9:30'da artırırsanız ve daha sonra 10:45'te 100-K RU/sn'ye düşürürseniz, 200 K RU/sn'den iki saat boyunca ücretlendirilirsiniz.

### <a name="billing-example-multiple-containers-each-with-dedicated-provisioned-throughput-mode"></a>Faturalandırma örneği: her biri özel olarak sağlanan iş elde etme moduna sahip birden çok kapsayıcı

* Doğu ABD 2'de sırasıyla 500 RU/sn ve 700 RU/sn'lik iki kapsayıcıiçeren bir Azure Cosmos hesabı oluşturursanız, toplam 1.200 RU/sn'lik bir toplam iş ortası elde etmiş olursunuz.  

* 1.200/100 * $0.008 = $0.096/saat ücretlendirilirsiniz. 

* İş girdiliği ihtiyaçlarınız değiştiyse ve her konteynerin kapasitesini 500 RU/sn artırırken aynı zamanda 20.000 RU/sn içeren yeni bir sınırsız kapsayıcı oluşturduysanız, toplam sağlanan kapasiteniz 22.200 RU/sn (1.000 RU/sn + 1.200 RU/sn + 20.000 RU/sn) olacaktır.  

* Faturanız daha sonra: $0.008 x 222 = $1.776/hour. 

* 720 saat (24 saat * 30 gün) bir ay içinde, 500 saat için sağlanan iş 1.200 RU / sn ve kalan 220 saat için girdi 22.200 RU / sn, aylık fatura gösterir: 500 x $ 0.096/hour + 220 x $ 1.776/saat = $ 438.72/month.

![Özel iş girdisi faturası örneği](./media/understand-your-bill/bill-example1.png)

### <a name="billing-example-containers-with-shared-throughput-mode"></a>Faturalama örneği: paylaşılan iş elde etme moduna sahip kapsayıcılar

* Doğu ABD 2'de, sırasıyla 50-K RU/sn ve 70-K RU/sn'lik iki Azure Cosmos veritabanıyla (veri tabanında çıktıyı paylaşan bir kapsayıcı kümesiyle) bir Azure Cosmos hesabı oluşturursanız, toplam 120 K RU/sn'lik bir veri girişine sahip olursunuz.  

* Saat 1200 x $0,008 = $9,60/saat ücretlendirilirsiniz. 

* İş ortası gereksinimleriniz değiştiyse ve her veritabanının sağlanan iş sayısını her veritabanı için 10K RU/sn artırdıysanız ve paylaşılan iş veri tabanınıza 15-K RU/sn özel iş verme moduna sahip ilk veritabanına yeni bir kapsayıcı eklerseniz, genel sağlanan kapasiteniz 155-K RU/sn (60 K RU/sn + 15 K sn/ RU) olacaktır.  

* Faturanız daha sonra değişecektir: 1.550 * $0.008 = $12.40/hour.  

* 720 saatlik bir ay içinde, 300 saat için sağlanan iş 120-K RU/sn ve kalan 420 saat için sağlanan iş 155-K RU /sn ise, aylık faturanız: 300 x $9,60/saat + 420 x $12,40/saat = $2,880 + $5,208 = $8,08/month olacaktır. 

![Paylaşılan iş artışı faturası örneği](./media/understand-your-bill/bill-example2.png)

## <a name="billing-examples-with-geo-replication-and-multi-master"></a>Coğrafi çoğaltma ve çoklu ana ile faturalama örnekleri  

Azure Cosmos veritabanı hesabınıza dünyanın herhangi bir yerinde Azure bölgeleri ekleyebilir/kaldırabilirsiniz. Çeşitli Azure Cosmos veritabanları ve kapsayıcılar için yapılandırdığınız iş, Azure Cosmos veritabanı hesabınızla ilişkili Azure bölgelerinin her birinde rezerve edilir. Azure Cosmos veritabanı hesabınızdaki tüm veritabanları ve kapsayıcılar arasında yapılandırılan (RU/sn) sağlanan iş toplamı (saat başına sağlanan) T ise ve veritabanı hesabınızla ilişkili Azure bölgesi sayısı N ise, daha sonra, Azure Cosmos veritabanı hesabınız için belirli bir saat için sağlanan toplam iş ortası, (a) tek bir yazma bölgesiyle yapılandırılan T x N RU/sn'ye eşittir ve (b) yazma işlem yeteneğine sahip tüm bölgelerle yapılandırılır si, sırasıyla T x (N+1) RU/sn'ye eşittir. Sağlanan iş ortası (tek yazma bölgesi) 100 RU/sn başına $0,008/saat maliyeti ve birden çok yazılabilir bölge (çok ustalı config) ile sağlanan iş başına $0,016/saat başına 100 RU/sn [(Fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/)bakın). Azure Cosmos DB, ister tek yazma bölgesi olsun ister birden çok yazma bölgesi olsun, herhangi bir bölgeden gelen verileri okumanızı sağlar.

### <a name="billing-example-multi-region-azure-cosmos-account-single-region-writes"></a>Faturalandırma örneği: çok bölgeli Azure Cosmos hesabı, tek bölge yazıyor

Batı ABD'de bir Azure Cosmos konteynerinizin olduğunu varsayalım. Kapsayıcı iş kaynağı 10K RU/sn ile oluşturulur ve bu ay 1 TB veri depolarsınız. Azure Cosmos hesabınıza her biri aynı depolama alanına ve iş metoduna sahip üç bölge (Doğu ABD, Kuzey Avrupa ve Doğu Asya) eklediğinizi varsayalım. Toplam aylık faturanız (bir ay içinde 30 gün varsayarak) olacaktır. Faturanız aşağıdaki gibi olacaktır: 

|**Öğe** |**Kullanım (ay)** |**Fiyat** |**Aylık Maliyet** |
|---------|---------|---------|-------|
|Batı ABD'de konteyner için iş mesuliyebi faturası      | 10K RU/sn * 24 * 30    |$0.008 /100 RU/sn/saat   |576 dolar|
|Doğu ABD, Kuzey Avrupa ve Doğu Asya - 3 ek bölge için iş mesuliyei faturası       | 3 * 10K RU/sn * 24 * 30    |$0.008 /100 RU/sn/saat  |1.728 dolar|
|Batı ABD'de konteyner için depolama faturası      | 250 GB    |$0.25/TR  |62,50 $|
|Doğu ABD, Kuzey Avrupa ve Doğu Asya - 3 ek bölge için depolama faturası      | 3 * 250 GB    |$0.25/TR  |187,50 $|
|**Toplam**     |     |  |**2.554 dolar**|

*Ayrıca, Doğu ABD, Kuzey Avrupa ve Doğu Asya'ya veri çoğaltmak için Batı ABD'deki konteynerden her ay 100 GB veri çıkarttuğunuz varsayalım. Veri aktarım hızlarına göre çıkış için faturalandırılırsınız.*

### <a name="billing-example-multi-region-azure-cosmos-account-multi-region-writes"></a>Faturalandırma örneği: çok bölgeli Azure Cosmos hesabı, çok bölgeli yazıyor

Batı ABD'de bir Azure Cosmos kapsayıcısı oluşturduğunuzu varsayalım. Kapsayıcı iş kaynağı 10K RU/sn ile oluşturulur ve bu ay 1 TB veri depolarsınız. Her biri aynı depolama alanına ve iş metoduna sahip üç bölge (Doğu ABD, Kuzey Avrupa ve Doğu Asya) eklediğinizi ve Azure Cosmos hesabınızla ilişkili tüm bölgelerdeki kapsayıcılara yazma olanağı istediğinizi varsayalım. Toplam aylık faturanız aşağıdaki gibi olacaktır (bir ay içinde 30 gün varsayarak:

|**Öğe** |**Kullanım (ay)**|**Fiyat** |**Aylık Maliyet** |
|---------|---------|---------|-------|
|Batı ABD'de konteyner için iş mesuliyet faturası (tüm bölgeler yazılabilir)       | 10K RU/sn * 24 * 30    |$0.016 /100 RU/sn/saat    |1.152 dolar |
|Doğu ABD, Kuzey Avrupa ve Doğu Asya (tüm bölgeler yazılabilir)        | (3 + 1) * 10K RU/sn * 24 * 30    |$0.016 /100 RU/sn/saat   |4.608 $ |
|Batı ABD'de konteyner için depolama faturası      | 250 GB    |$0.25/TR  |62,50 $|
|Doğu ABD, Kuzey Avrupa ve Doğu Asya - 3 ek bölge için depolama faturası      | 3 * 250 GB    |$0.25/TR  |187,50 $|
|**Toplam**     |     |  |**6.010 $**|

*Ayrıca, Doğu ABD, Kuzey Avrupa ve Doğu Asya'ya veri çoğaltmak için Batı ABD'deki konteynerden her ay 100 GB veri çıkarttuğunuz varsayalım. Veri aktarım hızlarına göre çıkış için faturalandırılırsınız.*

### <a name="billing-example-azure-cosmos-account-with-multi-master-database-level-throughput-including-dedicated-throughput-mode-for-some-containers"></a>Faturalandırma örneği: Azure Cosmos hesabı, bazı kapsayıcılar için özel iş ortası modu da dahil olmak üzere çok ustalı, veritabanı düzeyinde iş düzeyi nezlesi

Tüm bölgelerin yazılabilir olduğu çok bölgeli bir Azure Cosmos hesabımız (çok büyük config) olduğu aşağıdaki örneği ele alalım. Basitlik için, depolama boyutunun sabit kaldığını ve örneği basit tutmak için burada değişmediğini ve atlamadığını varsayacağız. Ay boyunca sağlanan iş şu şekilde değişir (30 gün veya 720 saat varsayarak): 

[0-100 saat]:  

* Tüm bölgelerin yazılabilir olduğu üç bölgeli Azure Cosmos hesabı (Batı ABD, Doğu ABD, Kuzey Avrupa) oluşturduk 

* Paylaşılan iş sırrı 10K RU/sn olan bir veritabanı (D1) oluşturduk 

* Paylaşılan iş ortası 30-K RU/sn ile bir veritabanı (D2) oluşturduk ve  

* 20 K RU/sn özel iş sırrına sahip bir konteyner (C1) oluşturduk 

[101-200 saat]:  

* Veritabanını (D1) 50 K RU/sn'ye ölçeklendirdik 

* Veritabanını (D2) 70 K RU/sn'ye ölçeklendirdik  

* Konteyneri (C1) sildik  

[201-300 saat]:  

* Konteyneri (C1) yine 20 K RU/sn özel iş sırrı ile oluşturduk 

[301-400 saat]:  

* Bölgelerden birini Azure Cosmos hesabından kaldırdık (# yazılı bölgeler artık 2' dir) 

* Veritabanını (D1) 10K RU/sn'ye küçültttüm 

* Veritabanını (D2) 80 K RU/sn'ye ölçeklendirdik  

* Konteyneri (C1) yine sildik 

[401-500 saat]:  

* Veritabanını (D2) 10K RU/sn'ye küçültttüm  

* Konteyneri (C1) yine 20 K RU/sn özel iş sırrı ile oluşturduk 

[501-700 saat]:  

* Veritabanını (D1) 20 K RU/sn'ye ölçeklendirdik  

* Veritabanını (D2) 100 K RU/sn'ye ölçeklendirdik  

* Konteyneri (C1) yine sildik  

[701-720 saat]:  

* Veritabanını (D2) 50 K RU/sn'ye küçültttüm  

Ay için 720 saat boyunca toplam sağlanan toplam iş teki değişiklikler aşağıdaki şekilde gösterilmiştir: 

![Gerçek yaşam örneği](./media/understand-your-bill/bill-example3.png)

Toplam aylık fatura (bir ay içinde 30 gün/720 saat varsayarak) aşağıdaki gibi hesaplanacaktır olacaktır:

|**Saat**  |**RU/s** |**Öğe** |**Kullanım (saatlik)** |**Maliyet** |
|---------|---------|---------|-------|-------|
|[0-100] |D1:10K <br/>D2:30K <br/>C1:20K |Batı ABD'de konteyner için iş mesuliyet faturası (tüm bölgeler yazılabilir)  | `D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br/>`D2: 30 K RU/sec/100 * $0.016 * 100 hours = $480` <br/>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |960 $  |
| | |2 ek bölge için iş mesuliyet faturası: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(2 + 1) * (60 K RU/sec /100 * $0.016) * 100 hours = $2,880`  |2.880 $  |
|[101-200] |D1:50K <br/>D2:70K <br/>C1: -- |Batı ABD'de konteyner için iş mesuliyet faturası (tüm bölgeler yazılabilir)  |`D1: 50 K RU/sec/100 * $0.016 * 100 hours = $800` <br/>`D2: 70 K RU/sec/100 * $0.016 * 100 hours = $1,120` |1920 $  |
| | |2 ek bölge için iş mesuliyet faturası: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(2 + 1) * (120 K RU/sec /100 * $0.016) * 100 hours = $5,760`  |5.760 $  |
|[201-300]  |D1:50K <br/>D2:70K <br/>C1:20K |Batı ABD'de konteyner için iş mesuliyet faturası (tüm bölgeler yazılabilir)  |`D1: 50 K RU/sec/100 * $0.016 * 100 hours = $800` <br/>`D2: 70 K RU/sec/100 * $0.016 * 100 hours = $1,120` <br/>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |2.240 dolar  |
| | |2 ek bölge için iş mesuliyet faturası: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(2 + 1) * (140 K RU/sec /100 * $0.016-) * 100 hours = $6,720` |6.720 dolar |
|[301-400] |D1:10K <br/>D2:80K <br/>C1: -- |Batı ABD'de konteyner için iş mesuliyet faturası (tüm bölgeler yazılabilir)  |`D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br/>`D2: 80 K RU/sec/100 * $0.016 * 100 hours = $1,280`  |1.440 dolar   |
| | |2 ek bölge için iş mesuliyet faturası: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(1 + 1) * (90 K RU/sec /100 * $0.016) * 100 hours = $2,880`  |2.880 $  |
|[401-500] |D1:10K <br>D2:10K <br>C1:20K |Batı ABD'de konteyner için iş mesuliyet faturası (tüm bölgeler yazılabilir)  |`D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br>`D2: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |640 $  |
| | |2 ek bölge için iş mesuliyet faturası: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(1 + 1) * (40 K RU/sec /100 * $0.016) * 100 hours = $1,280`  |1.280 dolar  |
|[501-700] |D1:20K <br>D2:100K <br>C1: -- |Batı ABD'de konteyner için iş mesuliyet faturası (tüm bölgeler yazılabilir)  |`D1: 20 K RU/sec/100 * $0.016 * 200 hours = $640` <br>`D2: 100 K RU/sec/100 * $0.016 * 200 hours = $3,200` |3.840 dolar  |
| | |2 ek bölge için iş mesuliyet faturası: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(1 + 1) * (120 K RU/sec /100 * $0.016) * 200 hours = $1,280`  |7.680 dolar  |
|[701-720] |D1:20K <br/>D2:50K <br/>C1: -- |Batı ABD'de konteyner için iş mesuliyet faturası (tüm bölgeler yazılabilir)  |`D1: 20 K RU/sec/100 *$0.016 * 20 hours = $64` <br/>`D2: 50 K RU/sec/100 *$0.016 * 20 hours = $160` |224 DOLAR  |
| | |2 ek bölge için iş mesuliyet faturası: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(1 + 1) * (70 K RU/sec /100 * $0.016) * 20 hours = $448`  |224 DOLAR  |
|| |**Toplam Aylık Maliyet**  | |**38.688 $**   |

## <a name="billing-examples-with-free-tier-accounts"></a>Ücretsiz katman hesaplarıyla faturalandırma örnekleri
Azure Cosmos DB ücretsiz katmanı ile, hesabınızda ilk 400 RU/s ve 5 GB depolama alanını hesap düzeyinde uygulanan ücretsiz olarak alırsınız. 400 RU/s ve 5 GB'ın dışındaki tüm RU/s ve depolama lar, fiyatlandırma sayfası başına normal fiyatlandırma fiyatlarıyla faturalandırılır. Tasarıda, ücretsiz 400 Ru/s ve 5 GB için bir ücret veya satır öğesi görmezsiniz, sadece RU/s ve ücretsiz katmanın kapsadığının ötesindeki depolamayı görebilirsiniz. 400 RU/s, her türlü RU/s için geçerlidir - tedarik edilen iş, otomatik pilot (önizleme) ve çoklu ana.  

### <a name="billing-example---container-or-database-with-provisioned-throughput"></a>Faturalama örneği - sağlanan iş ortası ile konteyner veya veritabanı
- 400 RU/s ve 5 GB depolama alanına sahip ücretsiz bir katman hesabında bir veritabanı veya kapsayıcı oluşturduğumu varsayalım.
- Faturanız bu kaynak için herhangi bir ücret göstermez. Saatlik ve aylık maliyetiniz $0 olacaktır.
- Şimdi, aynı hesapta, 1000 RU/s ve 10 GB depolama alanına sahip başka bir veritabanı veya kapsayıcı eklediğimizi varsayalım.
- Faturanız artık 1000 RU/s ve 10 GB depolama alanı için bir ücret gösterecektir. 

### <a name="billing-example---container-or-database-with-autopilot-throughput-preview"></a>Faturalama örneği - otomatik pilot verime sahip kapsayıcı veya veritabanı (önizleme)
- Ücretsiz bir katman hesabında, maksimum 4000 RU/s RU/s'ye sahip otomatik pilot etkin leştirilmiş bir veritabanı veya kapsayıcı oluşturduğumuzvarsayalım. Bu kaynak otomatik olarak 400 RU/s - 4000 RU/s arasında ölçeklendirilir. 
- 1 saat 1 ile 10 arasında, kaynak en az 400 RU/s olduğunu varsayalım. 11. saat boyunca kaynak 1000 RU/s'ye kadar ölçeklendirilir ve bir saat içinde 400 RU/s'ye geri döner.
- 400 RU/s ücretsiz katman tarafından karşılandığı için, 1 ile 10 saat içinde, iş için $0 faturalandırılırsınız. 
- Saat 11'de, etkili bir 1000 RU/s - 400 RU/s = 600 RU/s için faturalandırılırsınız, çünkü bu saatin en yüksek RU/s's'dir. Bu saat için 100 RU /s 6 birim olacak, bu nedenle saat için toplam işlem maliyeti 6 birim * 0,012 $ = 0,072 $ olacaktır. 
- İlk 5 GB'ın dışındaki tüm depolama lar normal depolama hızlarında faturalandırılır. 

### <a name="billing-example---multi-region-single-write-region-account"></a>Faturalama örneği - çok bölgeli, tek yazma bölgesi hesabı
- Ücretsiz bir katman hesabında 1200 RU/s ve 10 GB depolama alanına sahip bir veritabanı veya kapsayıcı oluşturduğumuzvarsayalım. Hesabı 3 bölgeye çoğaltıyoruz ve tek ana (tek yazma bölgesi) hesabımız var.
- Toplamda, ücretsiz katman olmadan, biz 3 * 1200 RU / s = 3600 RU / s ve 3 * 10 GB = 30 GB depolama için fatura olacaktır.
- Ücretsiz katman indirimi ile, 400 RU/s ve 5 GB depolama alanı kaldırdıktan sonra, tek yazma bölge fiyatı ve 25 GB depolama alanı üzerinden 3200 RU/s (32 birim) kullanılabilir iş bölümü için faturalandırılırız.
- RU / s için aylık maliyet olacaktır: 32 adet * $ 0.008 * 24 saat * 31 gün = 190,46 $ olacaktır. Depolama için aylık maliyet olacaktır: 25 GB * 0,25 / GB = 6,25 $ olacaktır. Toplam maliyet 190,46 $ + 6,25 $ = 196,71 $ olacaktır.
- Not: RU/s veya depolama nın birim fiyatı bölgelerde farklılık gösterirse, serbest katman 400 RU/s ve 5 GB hesabın oluşturulduğu bölgenin fiyatlarını yansıtacaktır.

### <a name="billing-example---multi-region-multi-master-multiple-write-region-account"></a>Faturalama örneği - çok bölgeli, çok büyük (çoklu yazma bölgesi) hesabı

Bu örnek, 1 Aralık 2019'dan sonra oluşturulan hesaplar için [çok yönlü fiyatlandırmayı](https://azure.microsoft.com/pricing/details/cosmos-db/) yansıtmaktadır. 
- Ücretsiz bir katman hesabında 1200 RU/s ve 10 GB depolama alanına sahip bir veritabanı veya kapsayıcı oluşturduğumuzvarsayalım. Hesabı 3 bölgeye çoğaltıyoruz ve çok büyük (çoklu yazma bölgesi) hesabımız var. 
- Toplamda, ücretsiz katman olmadan, biz 3 * 1200 RU / s = 3600 RU / s ve 3 * 10 GB = 30 GB depolama için fatura olacaktır.
- Ücretsiz katman indirimi ile, 400 RU/s ve 5 GB depolama alanı kaldırdıktan sonra, birden fazla yazma bölgesi fiyatıve 25 GB depolama alanı üzerinden 3200 RU/s (32 birim) kullanılabilir iş bölümü için faturalandırılırız.
- RU/s için aylık maliyet: 32 adet * $0.016 * 24 saat * 31 gün = $380.93. Depolama için aylık maliyet olacaktır: 25 GB * 0,25 / GB = 6,25 $ olacaktır. Toplam maliyet 380,93 $ + 6,25 $ = 387,18 $ olacaktır.

## <a name="proactively-estimating-your-monthly-bill"></a>Aylık faturanızı proaktif olarak tahmin etme  

Ay sonundan önce faturanızı proaktif olarak tahmin etmek istediğiniz başka bir örnek düşünelim. Faturanızı aşağıdaki gibi tahmin edebilirsiniz:

|**Depolama Maliyeti** | |
|----|----|
|Avg Kayıt Boyutu (KB) |1 |
|Kayıt Sayısı  |100,000,000  |
|Toplam Depolama Alanı (GB)  |100 |
|GB başına aylık maliyet  |0,25 dolar  |
|Depolama için Beklenen Aylık Maliyet   |25,00 TL  |

<br>

|**İş İbzama Maliyeti** | | | |
|----|----|----|----|
|İşlem Türü| İstekler/sn| Avg. RU/istek| RUs gerekli|
|Yazma| 100 | 5 | 500|
|Okuma| 400| 1| 400|

Toplam RU/sn: 500 + 400 = 900 Saatlik maliyet: 900/100 * $0,008 = $0.072 Beklenen Aylık Maliyet (31 gün varsayarsak): $0.072 * 24 * 31 = 53.57 $

**Toplam Aylık Maliyet**

Toplam Aylık Maliyet = Depolama için Aylık Maliyet + Aylık Maliyet Toplam Aylık Maliyet = 25,00 $ + 53,57 $ = 78,57 $

*Fiyatlandırma bölgeye göre değişebilir. Güncel fiyatlandırma için [Fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/)bakın.*

## <a name="billing-with-azure-cosmos-db-reserved-capacity"></a>Azure Cosmos DB ayrılmış kapasiteile faturalandırma

Azure Cosmos DB rezerve edilmiş kapasite, tüm Azure cosmos veritabanlarına ve kapsayıcılarına (herhangi bir API veya veri modeli için) uygulanabilecek, önceden sağlanan iş ortasını (ayrılmış kapasite veya rezervasyon) satın almanızı sağlar. Sağlanan iş başı fiyatı bölgeye göre değiştiğinden, ayrılmış kapasitenin, her bölgedeki ilgili fiyattan sağlanan iş başına çekilebilen, indirimli olarak satın aldığınız bir parasal kredi olarak düşünülmesine yardımcı olur. Örneğin, 50-K RU/sn ile birlikte tek bir kapsayıcıiçeren ve küresel olarak çoğaltılmış iki bölge olan Doğu ABD ve Japonya Doğu'ya sahip bir Azure Cosmos hesabınız olduğunu varsayalım. Gittikçe öde seçeneğini seçerseniz, şunları ödlersiniz:  

* Doğu ABD'de: 50-K RU/sn için o bölgede 100 RU/sn başına 0,008 $ 

* Japonya Doğu: 50-K RU / sn için 0,009 $ 100 RU / sn başına bu bölgede oranında

Toplam faturanız (ayrılmış kapasite olmadan) olacaktır (30 gün veya 720 saat varsayarak): 

|**Bölge**| **100 RU/s başına saatlik fiyat**|**Birimler (RU/s)**|**Faturalanan Tutar (saatlik)**| **Faturalanan Tutar (aylık)**|
|----|----|----|----|----|
|Doğu ABD|0,008 $ |50 K|4 dolar|2.880 $ |
|Doğu Japonya|0,009 $ |50 K| 4.50 dolar |3.240 dolar |
|Toplam|||8.50 dolar|6.120 dolar |

Bunun yerine ayrılmış kapasite satın aldığınızı varsayalım. Bir yıl için 56.064 $ fiyata 100-K RU / sn için ayrılmış kapasite satın alabilirsiniz (20% indirimli), ya da 6,40 $ saat başına. [Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/cosmos-db/)rezerve edilen kapasite fiyatlandırması sayfasına bakın).  

* Maliyet maliyeti (ödeme-as-you-go): 100.000 RU/sn/100 * $0.008/saat * Bir yıl içinde 8760 saat = 70.080 $ 

* Maliyet (rezerve kapasitesi ile) 70.080 $ 20% = 56.064 $ indirimli 

Ne etkili bir şekilde satın almış olduğunuz saatte 8 $ bir kredi, 100 K RU / sn Doğu ABD'de liste fiyatı kullanarak, saatte 6,40 $ fiyata. Daha sonra, aboneliğiniz için belirlenen ilgili bölgesel liste fiyatlarından, herhangi bir global Azure bölgesinde sağlanan iş ortası kapasitesi için saatlik olarak bu ön ödemeli iş ortası rezervasyonundan aşağı çekebilirsiniz. Doğu ABD'de ve Japonya Doğu'da her biri için 50 K RU/sn sağlamanız durumunda, saat başına 8,00 ABD doları tutarında sağlanan iş ortası çizebileceksiniz ve saat başına 0,50 ABD doları (veya 360$/ay) fazlalığı faturalandırılır. 

|**Bölge**| **100 RU/s başına saatlik fiyat**|**Birimler (RU/s)**| **Faturalanan Tutar (saatlik)**| **Faturalanan Tutar (aylık)**|
|----|----|----|----|----|
|Doğu ABD|0,008 $ |50 K|4 dolar|2.880 $ |
|Doğu Japonya|0,009 $ |50 K| 4.50 dolar |3.240 dolar |
|||Öde-as-you-go|8.50 dolar|6120 $|
|Satın Alınan Ayrılmış Kapasite|$0.0064 (%20 indirim) |100 RU/sn veya $8 kapasite önceden satın alınmış |-$8|-$5,760 |
|Net Fatura|||$0,50 |360 dolar |

## <a name="next-steps"></a>Sonraki Adımlar

Daha sonra Aşağıdaki makalelerle Azure Cosmos DB'de maliyet optimizasyonu hakkında bilgi edinebilirsiniz:

* [Cosmos DB fiyatlandırma modelinin müşteriler için maliyet etkin olması](total-cost-ownership.md) hakkında daha fazla bilgi edinin
* [Geliştirme ve test için Optimizasyon](optimize-dev-test.md) hakkında daha fazla bilgi edinin
* [İş memat maliyetini optimize etme](optimize-cost-throughput.md) hakkında daha fazla bilgi edinin
* [Depolama maliyetini optimize etme](optimize-cost-storage.md) hakkında daha fazla bilgi edinin
* [Okuma ve yazma maliyetini optimize etme](optimize-cost-reads-writes.md) hakkında daha fazla bilgi edinin
* [Sorguların maliyetini optimize etme](optimize-cost-queries.md) hakkında daha fazla bilgi edinin
* [Çok bölgeli Azure Cosmos hesaplarının maliyetini optimize etme](optimize-cost-regions.md) hakkında daha fazla bilgi edinin
