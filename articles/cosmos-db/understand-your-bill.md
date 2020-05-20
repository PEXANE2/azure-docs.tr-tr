---
title: Azure Cosmos DB Faturanızı Anlama
description: Bu makalede, Azure Cosmos DB faturanızı bazı örneklerle nasıl anlayabileceğiniz açıklanır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/01/2019
ms.reviewer: sngun
ms.openlocfilehash: 9384b974463c963cc130e7ca0d4a9ee815a92e53
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83647730"
---
# <a name="understand-your-azure-cosmos-db-bill"></a>Azure Cosmos DB faturanızı anlama

Tamamen yönetilen bir bulutta yerel veritabanı hizmeti olarak, Azure Cosmos DB yalnızca sağlanan aktarım hızı ve tüketilen depolama alanı için ücretlendirme yaparak faturalandırmayı basitleştirir. Şirket içi veya IaaS ile barındırılan diğer yöntemler ile karşılaştırıldığında ek lisans ücretleri, donanım, yardımcı program maliyetleri veya tesis maliyetleri yoktur. Azure Cosmos DB çoklu bölge yeteneklerini dikkate aldığınızda, veritabanı hizmeti mevcut şirket içi veya IaaS çözümleriyle karşılaştırıldığında maliyetlerde önemli bir düşüş sağlar.

Azure Cosmos DB, sağlanan verimlilik ve tüketilen depolama alanı temelinde saatlik olarak faturalandırılırsınız. Sağlanan aktarım hızı için, faturalandırma birimi saat başına 100 RU/sn, standart genel fiyatlandırmayı kabul eden bir saatte $0,008 ile ücretlendirilir, [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/)bakın. Tüketilen depolama alanı için aylık $0,25 GB depolama alanı üzerinden faturalandırılırsınız, [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/)bakın. 

Bu makale, aylık faturada gördüğünüz ayrıntıları anlamanıza yardımcı olacak örnekleri içerir. Azure Cosmos kapsayıcılarınızda farklı miktarda aktarım hızı sağlandıysa, bunları birden fazla bölgeye yaydıysanız veya farklı bir öğe için bir aydan daha fazla süre boyunca çalışıyorlarsa örneklerde gösterilen sayılar farklı olabilir.

> [!NOTE]
> Faturalandırma, 60 dakikalık bir süre değil, bir duvar saati saatinin herhangi bir bölümü için tasarlanmıştır.

## <a name="billing-examples"></a>Faturalandırma örnekleri

### <a name="billing-example---throughput-on-a-container-full-month"></a>Faturalandırma örneği-bir kapsayıcıda işleme (tam ay)

* Bir kapsayıcıda 1.000 RU/sn aktarım hızı kullandığınızı ve bu süre için 24 saat * 30 720 gün boyunca mevcut olduğunu varsayalım.  

* 1.000 RU/sn, kapsayıcıların mevcut olduğu her saat için saat başına 100 RU/sn 'dir (yani, 1000/100 = 10). 

* Saat başına 10 birimi, $0,008 maliyeti (saat başına 100 RU/sn başına $0,08) ile çarpar. 

* Saat başına $0,08, ay içindeki saat sayısı $0,08 * 24 saat * 30 gün = $57,60.  

* Toplam aylık faturada 7.200 birim (/100 ru) gösterilecektir ve bu maliyet, $57,60.

### <a name="billing-example---throughput-on-a-container-partial-month"></a>Faturalandırma örneği-bir kapsayıcıda aktarım hızı (kısmi ay)

* Sağlanan 2.500 RU/sn için üretilen iş hızına sahip bir kapsayıcı oluşturduğumuz varsayın. Kapsayıcı ayda 24 saat sürer (örneğin, oluşturduktan sonra 24 saat sonra silinir).  

* Ardından, faturada 600 birim (2.500 RU/sn/100 RU/sn/Unit * 24 saat) göreceksiniz. Maliyet $4,80 (600 birim * $0.008/Unit) olacaktır.

* Aylık toplam fatura $4,80 olacaktır.

### <a name="billing-rate-if-storage-size-changes"></a>Depolama boyutu değişirse faturalandırma oranı

Depolama kapasitesi, aylık bir dönemde depolanan en fazla saatlik veri miktarı için GB cinsinden faturalandırılır. Örneğin, ayın yarısı için 100 GB depolama ve ayın ikinci yarısında 50 GB kullandıysanız, bu ay için 75 GB depolama alanının bir eşdeğeri için faturalandırılırsınız.

### <a name="billing-rate-when-container-or-a-set-of-containers-are-active-for-less-than-an-hour"></a>Kapsayıcının veya kapsayıcı kümesinin bir saatten daha az etkin olduğu faturalandırma oranı

Kapsayıcının veya veritabanının mevcut olduğu her saat için, kullanım veya kapsayıcının veya veritabanının bir saatten daha az etkin olduğu durumlarda, her saat için sabit bir fiyat üzerinden faturalandırılırsınız. Örneğin, bir kapsayıcı veya veritabanı oluşturup 5 dakika sonra silerseniz faturanızda bir saat yer alır.

### <a name="billing-rate-when-throughput-on-a-container-or-database-scales-updown"></a>Bir kapsayıcı veya veritabanı üzerinde üretilen iş miktarı yukarı/aşağı ölçeklenirken faturalandırma oranı

9:30:400 RU/sn 'den 1.000 RU/sn 'ye kadar olan üretilen iş verimini artırdıysanız ve daha sonra 10:45 tarihinde 400 ru/sn 'ye geri dönmek istiyorsanız, iki saatlik 1.000 RU/sn üzerinden ücretlendirilirsiniz. 

Bir kapsayıcı için sağlanan aktarım hızını veya 9:30:100-K RU/sn ile 200-K RU/sn arası bir kapsayıcı kümesini artırırsanız, daha sonra 10:45 $-K RU/sn ' ye kadar ücret100 lendirilirsiniz.

### <a name="billing-example-multiple-containers-each-with-dedicated-provisioned-throughput-mode"></a>Faturalandırma örneği: her biri adanmış sağlanan verimlilik moduna sahip birden çok kapsayıcı

* Doğu ABD 2 içinde bir Azure Cosmos hesabını, sağlanan 500 RU/sn ve 700 RU/sn hızında üretilen iş hızına sahip iki kapsayıcı ile birlikte oluşturursanız, toplam 1.200 sağlanan% RU/sn hızına sahip olursunuz.  

* Ücretlendirilirsiniz/100 * $0,008 = $0.096/saat. 

* İşleme gereksinimleriniz değiştiyse ve her kapsayıcının kapasitesini 500 RU/sn ile artırdıysanız, ayrıca 20.000 RU/sn ile yeni bir sınırsız kapsayıcı oluştururken, genel olarak sağlanan kapasiteniz 22.200 RU/sn (1.000 RU/sn + 1.200 RU/sn + 20, 000RU/sn) olacaktır.  

* Faturanız daha sonra şu şekilde değişir: $0,008 x 222 = $1.776/Hour. 

* 720 saatlik bir ayda (24 saat * 30 gün), 500 saat için sağlanan aktarım hızı 1.200 RU/sn ise ve kalan 220 saat için sağlanan aktarım hızı 22.200 RU/sn ise aylık faturanız şu şekilde görünür: 500 x $0.096/saat + 220 x $1.776/saat = $438.72/ay.

![Adanmış üretilen iş faturası örneği](./media/understand-your-bill/bill-example1.png)

### <a name="billing-example-containers-with-shared-throughput-mode"></a>Faturalandırma örneği: paylaşılan işleme modundaki kapsayıcılar

* İki Azure Cosmos veritabanı ile Doğu ABD 2 içinde bir Azure Cosmos hesabı oluşturursanız (veritabanı düzeyinde üretilen işi paylaşan bir kapsayıcı kümesi ile), sırasıyla 50-K RU/sn ve 70-K RU/sn için sağlanan aktarım hızı ile, 120 en fazla K RU/sn için sağlanan toplam işlem hızına sahip olursunuz.  

* 1200 x $0,008 = $9.60/saat ücretlendirilirsiniz. 

* Aktarım gereksinimleriniz değiştiyse ve her bir veritabanı için her bir veritabanının sağlanan verimini 10.000 RU/sn olarak artırdıysanız, paylaşılan aktarım hızı veritabanınıza 15-K RU/sn adanmış aktarım hızı moduna sahip ilk veritabanına yeni bir kapsayıcı eklerseniz, genel olarak sağlanan kapasiteniz 155-K RU/sn (60 K RU/sn + 80 K RU/sn + 15 K RU/sn) olacaktır.  

* Faturanız daha sonra şu şekilde değişir: 1.550 * $0,008 = $12.40/Hour.  

* 720 saatlik bir ayda saat 300 için sağlanan aktarım hızı 120-K RU/sn ve kalan 420 saat için sağlanan aktarım hızı 155-K RU/sn ise aylık faturanız şu şekilde görünür: 300 x $9.60/saat + 420 x $12.40/saat = $2.880 + $5.208 = $8088/ay. 

![Paylaşılan verimlilik faturası örneği](./media/understand-your-bill/bill-example2.png)

## <a name="billing-examples-with-geo-replication-and-multi-master"></a>Coğrafi çoğaltma ve çoklu yönetici ile faturalandırma örnekleri  

Dünyanın her yerindeki Azure bölgelerini Azure Cosmos veritabanı hesabınıza dilediğiniz zaman ekleyebilir/kaldırabilirsiniz. Çeşitli Azure Cosmos veritabanları ve kapsayıcıları için yapılandırdığınız aktarım hızı, Azure Cosmos veritabanı hesabınızla ilişkili Azure bölgelerinin her birine ayrılır. Azure Cosmos veritabanı hesabınızdaki (saat başına sağlanan) tüm veritabanları ve kapsayıcılar üzerinde yapılandırılan üretilen iş hacmi (RU/sn) toplamı T ise ve veritabanı hesabınızla ilişkili Azure bölgelerinin sayısı N ise, daha sonra, tek bir yazma bölgesi ile yapılandırılan Azure Cosmos veritabanı hesabınız (a) için verilen toplam sağlanan aktarım hızı T x N RU/sn 'ye eşittir ve (b) yazmaları işleme yeteneğine sahip tüm bölgeler sırasıyla T x (N + 1) RU/sn 'ye eşittir. Sağlanan aktarım hızı (tek yazma bölgesi) maliyetleri $0.008/saat başına 100 RU/sn ve birden çok yazılabilir bölge (çok yöneticili yapılandırma) maliyetleriyle sağlanan aktarım hızı, 100 RU/sn başına (bkz. [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/)bakın). Tek bir yazma bölgesinin veya birden fazla yazma bölgesinin Azure Cosmos DB, herhangi bir bölgeden veri okumanızı sağlar.

### <a name="billing-example-multi-region-azure-cosmos-account-single-region-writes"></a>Faturalandırma örneği: çok bölgeli Azure Cosmos hesabı, tek bölgede yazma işlemleri

Batı ABD ' de bir Azure Cosmos Kapsayıcınız olduğunu varsayalım. Kapsayıcı 10.000 RU/sn aktarım hızı ile oluşturulur ve bu ay 1 TB veri depoladığınızda. Azure Cosmos hesabınıza her biri aynı depolama ve aktarım hızı ile üç bölge (Doğu ABD, Kuzey Avrupa ve Doğu Asya) eklediğinizi varsayalım. Toplam aylık faturanız olacaktır (bir ayda 30 gün varsayılır). Faturanız aşağıdaki gibi olacaktır: 

|**Öğe** |**Kullanım (ay)** |**Fiyat** |**Aylık maliyet** |
|---------|---------|---------|-------|
|Batı ABD kapsayıcı için üretilen iş faturası      | 10.000 RU/sn * 24 * 30    |$0,008/saat başına 100 RU/sn   |$576|
|3 ek bölge için üretilen iş faturası Doğu ABD, Kuzey Avrupa ve Doğu Asya       | 3 * 10.000 RU/sn * 24 * 30    |$0,008/saat başına 100 RU/sn  |$1.728|
|Batı ABD kapsayıcı için depolama faturası      | 250 GB    |$0,25/GB  |$62,50|
|3 ek bölge için depolama faturası Doğu ABD, Kuzey Avrupa ve Doğu Asya      | 3 * 250 GB    |$0,25/GB  |$187,50|
|**Toplam**     |     |  |**$2.554**|

*Ayrıca, verileri Doğu ABD, Kuzey Avrupa ve Doğu Asya çoğaltmak için Batı ABD kapsayıcıdaki her ay 100 GB veri çıkışı olduğunu da varsayalım. Veri aktarımı ücretleri itibariyle çıkış için faturalandırılırsınız.*

### <a name="billing-example-multi-region-azure-cosmos-account-multi-region-writes"></a>Faturalandırma örneği: çok bölgeli Azure Cosmos hesabı, çok bölgeli yazma işlemleri

Batı ABD içinde bir Azure Cosmos kapsayıcısı oluşturduğunuzu varsayalım. Kapsayıcı 10.000 RU/sn aktarım hızı ile oluşturulur ve bu ay 1 TB veri depoladığınızda. Her biri aynı depolama ve aktarım hızı ve Azure Cosmos hesabınızla ilişkili tüm bölgelerdeki kapsayıcılara yazmak istediğiniz üç bölge (Doğu ABD, Kuzey Avrupa ve Doğu Asya) eklediğinizi varsayalım. Toplam aylık faturanız şu şekilde olacaktır: (bir ayda 30 gün varsayılır):

|**Öğe** |**Kullanım (ay)**|**Fiyat** |**Aylık maliyet** |
|---------|---------|---------|-------|
|Batı ABD kapsayıcı için üretilen iş faturası (tüm bölgeler yazılabilir)       | 10.000 RU/sn * 24 * 30    |$0,016/saat başına 100 RU/sn    |$1.152 |
|3 ek bölge için üretilen iş faturası Doğu ABD, Kuzey Avrupa ve Doğu Asya (tüm bölgeler yazılabilir)        | (3 + 1) * 10.000 RU/sn * 24 * 30    |$0,016/saat başına 100 RU/sn   |$4.608 |
|Batı ABD kapsayıcı için depolama faturası      | 250 GB    |$0,25/GB  |$62,50|
|3 ek bölge için depolama faturası Doğu ABD, Kuzey Avrupa ve Doğu Asya      | 3 * 250 GB    |$0,25/GB  |$187,50|
|**Toplam**     |     |  |**$6.010**|

*Ayrıca, verileri Doğu ABD, Kuzey Avrupa ve Doğu Asya çoğaltmak için Batı ABD kapsayıcıdaki her ay 100 GB veri çıkışı olduğunu da varsayalım. Veri aktarımı ücretleri itibariyle çıkış için faturalandırılırsınız.*

### <a name="billing-example-azure-cosmos-account-with-multi-master-database-level-throughput-including-dedicated-throughput-mode-for-some-containers"></a>Faturalandırma örneği: bazı kapsayıcılar için adanmış aktarım hızı modu dahil olmak üzere çok yöneticili, veritabanı düzeyinde aktarım hızı olan Azure Cosmos hesabı

Tüm bölgelerin yazılabilir olduğu çok bölgeli bir Azure Cosmos hesabımız olan aşağıdaki örneği ele alalım (çok yöneticili yapılandırma). Basitlik sağlamak için, depolama boyutunun sabit kalır ve bu örneği daha basit tutmak için burada değişmez ve yok sayacağız. Ay boyunca sağlanan aktarım hızı aşağıdaki gibi farklılık gösterir (30 gün veya 720 saat varsayılır): 

[0-100 saat]:  

* Tüm bölgelerin yazılabilir olduğu üç bölge Azure Cosmos hesabı (Batı ABD, Doğu ABD, Kuzey Avrupa) oluşturduk 

* Paylaşılan verimlilik 10.000 RU/sn ile bir veritabanı (D1) oluşturduk 

* Paylaşılan işleme 30-K RU/sn ile bir veritabanı (D2) oluşturdunuz ve  

* Adanmış aktarım hızı 20 K RU/sn ile bir kapsayıcı (C1) oluşturduk 

[101-200 saat]:  

* Veritabanını (D1) 50 K RU/sn 'ye ölçeklendirdik 

* Veritabanını (D2) 70 K RU/sn 'ye ölçeklendirdik  

* Kapsayıcıyı sildik (C1)  

[201-300 saat]:  

* Özel işleme 20 K RU/sn ile kapsayıcı (C1) oluşturdunuz 

[301-400 saat]:  

* Azure Cosmos hesabından bölgelerden birini kaldırdık (yazılabilir bölge sayısı artık 2 ' dir) 

* Veritabanını (D1) 10.000 RU/sn 'ye ölçeklendirdik 

* Veritabanını (D2) 80 K RU/sn 'ye ölçeklendirdik  

* Kapsayıcıyı (C1) yeniden sildiniz 

[401-500 saat]:  

* Veritabanını (D2) 10.000 RU/sn 'ye ölçeklendirdik  

* Özel işleme 20 K RU/sn ile kapsayıcı (C1) oluşturdunuz 

[501-700 saat]:  

* Veritabanını (D1) 20 K RU/sn 'ye ölçeklendirdik  

* Veritabanını (D2) 100 K RU/sn 'ye ölçeklendirdik  

* Kapsayıcıyı (C1) yeniden sildiniz  

[701-720 saat]:  

* Veritabanı (D2) ile 50 K RU/sn arasında ölçeklendirildi  

Ayda 720 saat boyunca toplam sağlanan aktarım hızı içindeki değişiklikler aşağıdaki şekilde gösterilmiştir: 

![Gerçek yaşam örneği](./media/understand-your-bill/bill-example3.png)

Toplam aylık fatura şöyle olacaktır (bir ayda 30 gün/720 saat varsayılır), aşağıdaki şekilde hesaplanır:

|**Saat**  |**RU/s** |**Öğe** |**Kullanım (saatlik)** |**Maliyet** |
|---------|---------|---------|-------|-------|
|[0-100] |D1:10.000 <br/>D2:30K <br/>C1:20K |Batı ABD kapsayıcı için üretilen iş faturası (tüm bölgeler yazılabilir)  | `D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br/>`D2: 30 K RU/sec/100 * $0.016 * 100 hours = $480` <br/>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |$960  |
| | |2 ek bölge için üretilen iş faturası: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(2 + 1) * (60 K RU/sec /100 * $0.016) * 100 hours = $2,880`  |$2.880  |
|[101-200] |D1:50K <br/>D2:70K <br/>C1:-- |Batı ABD kapsayıcı için üretilen iş faturası (tüm bölgeler yazılabilir)  |`D1: 50 K RU/sec/100 * $0.016 * 100 hours = $800` <br/>`D2: 70 K RU/sec/100 * $0.016 * 100 hours = $1,120` |$1920  |
| | |2 ek bölge için üretilen iş faturası: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(2 + 1) * (120 K RU/sec /100 * $0.016) * 100 hours = $5,760`  |$5.760  |
|[201-300]  |D1:50K <br/>D2:70K <br/>C1:20K |Batı ABD kapsayıcı için üretilen iş faturası (tüm bölgeler yazılabilir)  |`D1: 50 K RU/sec/100 * $0.016 * 100 hours = $800` <br/>`D2: 70 K RU/sec/100 * $0.016 * 100 hours = $1,120` <br/>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |$2.240  |
| | |2 ek bölge için üretilen iş faturası: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(2 + 1) * (140 K RU/sec /100 * $0.016-) * 100 hours = $6,720` |$6.720 |
|[301-400] |D1:10.000 <br/>D2:80K <br/>C1:-- |Batı ABD kapsayıcı için üretilen iş faturası (tüm bölgeler yazılabilir)  |`D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br/>`D2: 80 K RU/sec/100 * $0.016 * 100 hours = $1,280`  |$1.440   |
| | |2 ek bölge için üretilen iş faturası: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(1 + 1) * (90 K RU/sec /100 * $0.016) * 100 hours = $2,880`  |$2.880  |
|[401-500] |D1:10.000 <br>D2:10.000 <br>C1:20K |Batı ABD kapsayıcı için üretilen iş faturası (tüm bölgeler yazılabilir)  |`D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br>`D2: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |$640  |
| | |2 ek bölge için üretilen iş faturası: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(1 + 1) * (40 K RU/sec /100 * $0.016) * 100 hours = $1,280`  |$1.280  |
|[501-700] |D1:20K <br>D2:100K <br>C1:-- |Batı ABD kapsayıcı için üretilen iş faturası (tüm bölgeler yazılabilir)  |`D1: 20 K RU/sec/100 * $0.016 * 200 hours = $640` <br>`D2: 100 K RU/sec/100 * $0.016 * 200 hours = $3,200` |$3.840  |
| | |2 ek bölge için üretilen iş faturası: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(1 + 1) * (120 K RU/sec /100 * $0.016) * 200 hours = $1,280`  |$7.680  |
|[701-720] |D1:20K <br/>D2:50K <br/>C1:-- |Batı ABD kapsayıcı için üretilen iş faturası (tüm bölgeler yazılabilir)  |`D1: 20 K RU/sec/100 *$0.016 * 20 hours = $64` <br/>`D2: 50 K RU/sec/100 *$0.016 * 20 hours = $160` |$224  |
| | |2 ek bölge için üretilen iş faturası: Doğu ABD, Kuzey Avrupa (tüm bölgeler yazılabilir)  |`(1 + 1) * (70 K RU/sec /100 * $0.016) * 20 hours = $448`  |$224  |
|| |**Aylık toplam maliyet**  | |**$38.688**   |

## <a name="billing-examples-with-free-tier-accounts"></a>Ücretsiz katman hesaplarıyla faturalandırma örnekleri
Azure Cosmos DB ücretsiz katman sayesinde hesapta ücretsiz olarak ilk 400 RU/sn ve 5 GB depolama alanı, hesap düzeyinde uygulanır. 400 RU/s ve 5 GB 'ın ötesinde tüm RU/s ve depolama alanı, fiyatlandırma sayfası başına normal fiyatlandırma ücretlerine göre faturalandırılır. Faturanızda ücretsiz 400 ru/s ve 5 GB için bir ücret veya satır öğesi görmezsiniz, yalnızca RU/s ve boş katmanın kapsamına giren depolama alanı görüntülenir. 400 RU/s, her RU/sn tarafından sağlanan aktarım hızı, otomatik ölçeklendirme ve çoklu yönetici türleri için geçerlidir.  

### <a name="billing-example---container-or-database-with-provisioned-throughput"></a>Faturalandırma örneği-sağlanan aktarım hızı ile kapsayıcı veya veritabanı
- Ücretsiz bir katman hesabında 400 RU/sn ve 5 GB depolama alanı ile bir veritabanı ya da kapsayıcı oluşturduğumuz varsayın.
- Faturanızda bu kaynak için herhangi bir ücret gösterilmeyecektir. Saatlik ve aylık maliyetiniz $0 olacaktır.
- Şimdi aynı hesapta yer alalım, 1000 RU/s ve 10 GB depolama ile başka bir veritabanı ya da kapsayıcı ekleyeceğiz.
- Faturanız artık 1000 RU/sn ve 10 GB depolama alanı için ücret gösterecektir. 

### <a name="billing-example---container-with-autoscale-throughput"></a>Faturalandırma örneği-otomatik ölçeklendirme işleme ile kapsayıcı
- Ücretsiz bir katman hesabında, ölçek etkin olan bir kapsayıcı oluşturduğumuz ve 4000 RU/sn 'nin en fazla RU/sn olduğunu varsayalım. Bu kaynak, 400 RU/s-4000 RU/s arasında otomatik olarak ölçeklendirilir. 
- Saat 1 ile saat 10 arasında, kaynağın en az 400 RU/sn olduğunu varsayalım. Saat 11 ' de, kaynak 1000 RU/sn 'ye kadar ölçeklenirken saat içinde 400 RU/s olarak yeniden açılır.
- 1 ile 10 arasındaki saatlerde $0, 400 RU/s, ücretsiz katmanda ele alınmıştır. 
- Saat 11 ' de, saat içinde en yüksek RU/sn olduğu için geçerli 1000 RU/s-400 RU/s = 600 RU/s için faturalandırılırsınız. Bu, saat boyunca 6 birimlik 100 RU/sn olacaktır, bu nedenle saat için toplam verimlilik maliyeti 6 birim * $0,012 = $0,072 olur. 
- İlk 5 GB 'ın ötesinde herhangi bir depolama alanı, normal depolama ücretlerine göre faturalandırılır. 

### <a name="billing-example---multi-region-single-write-region-account"></a>Faturalandırma örneği-çok bölgeli, tek bir yazma bölgesi hesabı
- Ücretsiz bir katman hesabında, 1200 RU/s ve 10 GB depolama ile bir veritabanı ya da kapsayıcı oluşturuyoruz. Hesabı 3 bölgeye çoğalttık ve tek yöneticili (tek bir yazma bölgesi) hesabınız var.
- Toplam katman olmadan, ücretsiz katman olmadan 3 * 1200 RU/s = 3600 RU/s ve 3 * 10 GB = 30 GB depolama alanı için faturalandırılırsınız.
- Ücretsiz katman indirimiyle, 400 RU/s ve 5 GB depolama alanı kaldırıldıktan sonra, tek bir yazma bölgesi fiyatı ve 25 GB depolama alanı üzerinde sağlanan aktarım hızı için geçerli bir 3200 RU/sn (32 birimi) faturalandırılırsınız.
- RU/s için aylık maliyet Şu şekilde olacaktır: 32 birim * $0,008 * 24 saat * 31 gün = $190,46. Depolama için aylık maliyet: 25 GB * 0,25/GB = $6,25 olacaktır. Toplam maliyet $190,46 + $6,25 = $196,71 olur.
- Note: RU/s veya Storage birim fiyatı bölgelerde farklıysa, 400 RU/s ve 5 GB ücretsiz katman, hesabın oluşturulduğu bölgenin oranlarını yansıtır.

### <a name="billing-example---multi-region-multi-master-multiple-write-region-account"></a>Faturalandırma örneği-çok bölgeli, çoklu yönetici (birden fazla yazma bölgesi) hesabı

Bu örnek, 1 Aralık 2019 ' den sonra oluşturulan hesaplar için [çok yöneticili fiyatlandırmayı](https://azure.microsoft.com/pricing/details/cosmos-db/) yansıtır. 
- Ücretsiz bir katman hesabında, 1200 RU/s ve 10 GB depolama ile bir veritabanı ya da kapsayıcı oluşturuyoruz. Hesabı 3 bölgeye çoğalttık ve birden çok ana (çoklu yazma bölgesi) hesabınız var. 
- Toplam katman olmadan, ücretsiz katman olmadan 3 * 1200 RU/s = 3600 RU/s ve 3 * 10 GB = 30 GB depolama alanı için faturalandırılırsınız.
- Ücretsiz katman indirimiyle, 400 RU/s ve 5 GB depolama alanı kaldırıldıktan sonra, birden fazla yazma bölgesi fiyatı ve 25 GB depolama alanı üzerinde sağlanan aktarım hızı için geçerli bir 3200 RU/sn (32 birimi) faturalandırılırsınız.
- RU/s için aylık maliyet Şu şekilde olacaktır: 32 birim * $0,016 * 24 saat * 31 gün = $380,93. Depolama için aylık maliyet: 25 GB * 0,25/GB = $6,25 olacaktır. Toplam maliyet $380,93 + $6,25 = $387,18 olur.

## <a name="proactively-estimating-your-monthly-bill"></a>Aylık faturanızı önceden tahmin etme  

Daha sonra faturanızı ayın sonundan önce önceden tahmin etmek istediğiniz başka bir örneği ele alalım. Faturanızı şu şekilde tahmin edebilirsiniz:

|**Depolama maliyeti** | |
|----|----|
|Ort kayıt boyutu (KB) |1 |
|Kayıt sayısı  |100.000.000  |
|Toplam depolama alanı (GB)  |100 |
|GB başına aylık maliyet  |$0,25  |
|Depolama için beklenen aylık maliyet   |$25,00  |

<br>

|**Verimlilik maliyeti** | | | |
|----|----|----|----|
|İşlem türü| İstek/sn| Ort. RU/istek| RUs gerekli|
|Yazma| 100 | 5 | 500|
|Okuma| 400| 1| 400|

Toplam RU/sn: 500 + 400 = 900 saatlik maliyet: 900/100 * $0,008 = $0,072 Işleme için beklenen aylık maliyet (31 gün varsayılıyor): $0,072 * 24 * 31 = $53,57

**Aylık toplam maliyet**

Aylık toplam maliyet = depolama için aylık maliyet ve toplam verimlilik için aylık maliyet = $25,00 + $53,57 = $78,57

*Fiyatlandırma bölgeye göre farklılık gösterebilir. Güncel fiyatlandırma için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/).*

## <a name="billing-with-azure-cosmos-db-reserved-capacity"></a>Azure Cosmos DB ayrılmış kapasite ile faturalama

Azure Cosmos DB ayrılmış kapasite, tüm Azure bölgelerindeki tüm Azure Cosmos veritabanlarına ve kapsayıcılara (API veya veri modeli için) uygulanabilen, sağlanan aktarım hızını önceden (bir ayrılmış kapasite veya bir ayırma) satın almanıza olanak sağlar. Sağlanan aktarım hızı fiyatı bölge başına değiştiğinden, ayrılmış kapasiteyi indirimle satın aldığınız Parasal kredi olarak düşünmenize yardımcı olur. Bu, her bölgedeki ilgili fiyata sağlanan aktarım hızı için üzerinden çizilebilirler. Örneğin, 50-K RU/sn ile sağlanan tek bir kapsayıcıya sahip bir Azure Cosmos hesabınız olduğunu ve küresel olarak çoğaltılan iki bölgeyi Doğu ABD ve Japonya Doğu. Kullandıkça Öde seçeneğini belirlerseniz, ödeme yaparsınız:  

* Doğu ABD: 50-K RU/sn için, bu bölgedeki her 100 RU/sn hızında $0,008 

* Japonya Doğu: 50-K RU/sn için, bu bölgedeki her 100 RU/sn hızında $0,009

Toplam faturanız (ayrılmış kapasite olmadan) olacaktır (30 gün veya 720 saat varsayılır): 

|**Bölge**| **100 RU/sn başına saatlik fiyat**|**Birimler (RU/s)**|**Faturalandırılan tutar (saatlik)**| **Faturalandırılan tutar (aylık)**|
|----|----|----|----|----|
|Doğu ABD|$0,008 |50 K|$4|$2.880 |
|Doğu Japonya|$0,009 |50 K| $4,50 |$3.240 |
|Toplam|||$8,50|$6.120 |

Bunun yerine ayrılmış kapasite satın aldığınızı göz önünde bulundurun. 100-K RU/sn için ayrılan kapasiteyi bir yıl (%20 indirimli) veya saat başına $6,40 olan $56.064 oranında satın alabilirsiniz. Bkz. [fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/cosmos-db/)ayrılmış kapasite fiyatlandırması.  

* Verimlilik maliyeti (Kullandıkça öde): 100.000 RU/sn/100 * $0.008/saat * 8760 saat, yıl içinde = $70.080 

* Verimlilik maliyeti (ayrılmış kapasiteye sahip) $70.080 indirimli ve %20 = $56.064 

Kullanım dışı olarak satın almış olduğunuz miktar, $8 KB/sn 100 için saat başına, saat başına $6,40 fiyat üzerinden Doğu ABD. Daha sonra, aboneliğiniz için ayarlanan bölgesel liste fiyatlarındaki herhangi bir genel Azure bölgesindeki sağlanan aktarım hızı kapasitesi için saatlik olarak bu ön ödemeli üretilen iş hacmi üzerinden aşağı doğru çizebilirsiniz. Bu örnekte, Doğu ABD her biri için 50 K RU/sn sağladığınızda ve Japonya Doğu, saat başına sağlanan aktarım hızı $8,00 ' i çizecek ve saat başına $0,50 yaş (veya $360/ay) üzerinden faturalandırılırsınız. 

|**Bölge**| **100 RU/sn başına saatlik fiyat**|**Birimler (RU/s)**| **Faturalandırılan tutar (saatlik)**| **Faturalandırılan tutar (aylık)**|
|----|----|----|----|----|
|Doğu ABD|$0,008 |50 K|$4|$2.880 |
|Doğu Japonya|$0,009 |50 K| $4,50 |$3.240 |
|||Kullandıkça öde|$8,50|$6120|
|Ayrılmış kapasite satın alındı|$0,0064 (%20 indirim) |100 RU/sn veya $8 kapasite önceden satın alındı |-$8|-$5.760 |
|NET fatura|||$0,50 |$360 |

## <a name="next-steps"></a>Sonraki Adımlar

Ardından, aşağıdaki makalelerle Azure Cosmos DB maliyet iyileştirmesi hakkında daha fazla bilgi edinebilirsiniz:

* [Fiyatlandırma modelinin müşteriler için uygun maliyetli Cosmos DB](total-cost-ownership.md) hakkında daha fazla bilgi edinin
* [Geliştirme ve test Için iyileştirme](optimize-dev-test.md) hakkında daha fazla bilgi edinin
* [Verimlilik maliyetini iyileştirme](optimize-cost-throughput.md) hakkında daha fazla bilgi edinin
* [Depolama maliyetini iyileştirme](optimize-cost-storage.md) hakkında daha fazla bilgi edinin
* [Okuma ve yazma maliyetlerini iyileştirme](optimize-cost-reads-writes.md) hakkında daha fazla bilgi edinin
* [Sorguların maliyetini En Iyi duruma getirme](optimize-cost-queries.md) hakkında daha fazla bilgi edinin
* [Çok bölgeli Azure Cosmos hesaplarının maliyetini En Iyi duruma getirme](optimize-cost-regions.md) hakkında daha fazla bilgi edinin
