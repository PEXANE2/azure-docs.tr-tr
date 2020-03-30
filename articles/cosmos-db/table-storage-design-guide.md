---
title: Ölçeklendirme ve performans için Azure Cosmos DB tabloları tasarla
description: 'Azure Tablo depolama tasarım kılavuzu: Azure Cosmos DB ve Azure Tablo depolama alanında ölçeklenebilir ve performans tabloları'
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: conceptual
ms.date: 05/21/2019
author: sakash279
ms.author: akshanka
ms.custom: seodec18
ms.openlocfilehash: 166076d366cbbf7bef24648772beaba9b3a88253
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246480"
---
# <a name="azure-table-storage-table-design-guide-scalable-and-performant-tables"></a>Azure Tablo depolama tablo tasarım kılavuzu: Ölçeklenebilir ve performant tablolar

[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

Ölçeklenebilir ve performansa yönelik tablolar tasarlamak için maliyet de dahil olmak üzere çeşitli faktörleri göz önünde bulundurmanız gerekir. Daha önce ilişkisel veritabanları için şema tasarladıysanız dikkat edilmesi gereken bu yönler size tanıdık gelir. Azure Tablo depolaması ve ilişkisel modeller arasında bazı benzerlikler olsa da birçok önemli farklılıklar da bulunur. Bu farklılıklar, ilişkisel veritabanlarına aşina olan bir kullanıcıya genelde sezgisel olmayan veya yanlış olarak gözükebilir. Ancak, Tablo depolaması gibi bir NoSQL anahtar/değer deposu için tasarım yapıyorsanız bu anlamlı olur.

Tablo depolama, milyarlarca varlığı ("ilişkisel veritabanı terminolojisinde satırlar") içerebilen bulut ölçekli uygulamaları veya yüksek işlem hacimlerini desteklemesi gereken veri kümelerini desteklemek üzere tasarlanmıştır. Bu nedenle verilerinizi nasıl depoladığınızı farklı düşünmeniz ve Tablo depolamanın nasıl çalıştığını anlamanız gerekir. İyi tasarlanmış bir NoSQL veri deposu, çözümünüzün ilişkisel veritabanı kullanan bir çözümden çok daha fazla (ve daha düşük maliyetle) ölçeklemesini sağlayabilir. Bu kılavuz, bu konularda size yardımcı olur.  

## <a name="about-azure-table-storage"></a>Azure Tablo depolama alanı hakkında
Bu bölümde, Tablo depolama alanının özellikle performans ve ölçeklenebilirlik için tasarımla ilgili bazı temel özellikleri vurgulanmaktadır. Azure Depolama ve Tablo depolama alanında yeniyseniz, bu makalenin geri kalanını okumadan önce [.NET'i kullanarak](table-storage-how-to-use-dotnet.md) [Microsoft Azure Depolamasına Giriş](../storage/common/storage-introduction.md) ve Azure Tablo depolama alanına başlayın' a bakın. Bu kılavuzun odak noktası Tablo depolama alanı olsa da, Azure Kuyruk depolama ve Azure Blob depolama alanı ve bunları bir çözümde Tablo depolamasıyla birlikte nasıl kullanabileceğiniz hakkında bazı tartışmaları içerir.  

Tablo depolama verileri depolamak için bir tablo biçimi kullanır. Standart terminolojide, tablonun her satırı bir varlığı temsil eder ve sütunlar bu varlığın çeşitli özelliklerini depolar. Her varlığın benzersiz olarak tanımlamak için bir çift anahtarı ve Tablo depolama kuruluşunun varlığın en son ne zaman güncelleştirildiğini izlemek için kullandığı bir zaman damgası sütunu vardır. Zaman damgası alanı otomatik olarak eklenir ve zaman damgasını rasgele bir değerle el ile üzerine yazamazsınız. Tablo depolama iyimser eşzamanlılığı yönetmek için bu son değiştirilmiş zaman damgası (LMT) kullanır.  

> [!NOTE]
> Tablo depolama REST API `ETag` işlemleri de LMT türetilen bir değer döndürmektedir. Bu belgede, ETag ve LMT terimleri birbirinin yerine kullanılır, çünkü bunlar aynı temel verilere başvururlar.  
> 
> 

Aşağıdaki örnek, çalışan ve departman varlıklarını depolamak için basit bir tablo tasarımıgösterir. Bu kılavuzda daha sonra gösterilen örneklerin çoğu bu basit tasarıma dayanmaktadır.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Zaman damgası</th>
<th></th>
</tr>
<tr>
<td>Pazarlama</td>
<td>00001</td>
<td>2014-08-22T00:50:32Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Don</td>
<td>Salon</td>
<td>34</td>
<td>donh@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Pazarlama</td>
<td>00002</td>
<td>2014-08-22T00:50:34Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Haz</td>
<td>Cao</td>
<td>47</td>
<td>junc@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Pazarlama</td>
<td>Bölüm</td>
<td>2014-08-22T00:50:30Z</td>
<td>
<table>
<tr>
<th>DepartmentName</th>
<th>Çalışan Sayısı</th>
</tr>
<tr>
<td>Pazarlama</td>
<td>153</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>Satışlar</td>
<td>00010</td>
<td>2014-08-22T00:50:44Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Ken</td>
<td>Kwok</td>
<td>23</td>
<td>kenk@contoso.com</td>
</tr>
</table>
</td>
</tr>
</table>


Şimdiye kadar, bu tasarım ilişkisel bir veritabanındaki tabloya benzer görünüyor. Temel farklar, zorunlu sütunlar ve aynı tabloda birden çok varlık türünü depolama yeteneğidir. Buna ek olarak, **FirstName** veya **Age**gibi kullanıcı tanımlı özelliklerin her biri, ilişkisel veritabanındaki bir sütun gibi tamsayı veya dize gibi bir veri türüne sahiptir. Ancak, ilişkisel bir veritabanının aksine, Tablo depolamanın şemasız yapısı, bir özelliğin her varlıkta aynı veri türüne sahip olmaması gerektiği anlamına gelir. Karmaşık veri türlerini tek bir özellikte depolamak için JSON veya XML gibi serileştirilmiş bir biçim kullanmanız gerekir. Daha fazla bilgi için Tablo [depolama veri modelini anlama konusuna](https://msdn.microsoft.com/library/azure/dd179338.aspx)bakın.

Seçiminiz `PartitionKey` ve `RowKey` iyi tablo tasarımı için temel. Bir tabloda depolanan her varlık benzersiz `PartitionKey` `RowKey`bir kombinasyonu na sahip olmalıdır ve. İlişkisel veritabanı tablosundaki anahtarlarda `PartitionKey` olduğu `RowKey` gibi, ve değerleri hızlı arama sağlayan kümelenmiş bir dizin oluşturmak için dizine alınır. Ancak tablo depolama, ikincil dizinler oluşturmaz, bu nedenle bunlar yalnızca iki dizine eklenmiş özelliktir (daha sonra açıklanan desenlerden bazıları bu görünür sınırlamayı nasıl çözebileceğinizi gösterir).  

Bir tablo bir veya daha fazla bölümden oluşur ve yaptığınız tasarım kararlarının çoğu `PartitionKey` uygun `RowKey` bir seçim yapmak ve çözümünüzü optimize etmek için olacaktır. Bir çözüm, tüm varlıklarınızı bölümlere ayırarak içeren tek bir tablodan oluşabilir, ancak genellikle bir çözümün birden çok tablosu vardır. Tablolar, varlıklarınızı mantıksal olarak düzenlemenize ve erişim denetim listelerini kullanarak verilere erişimi yönetmenize yardımcı olur. Tek bir depolama işlemi kullanarak tüm tabloyu bırakabilirsiniz.  

### <a name="table-partitions"></a>Tablo bölümleri
Hesap adı, tablo adı `PartitionKey` ve birlikte Tablo depolama varlığını depoladığı depolama hizmeti içindeki bölümü tanımlar. Varlıklar için adresleme şemasının bir parçası olmanın yanı sıra, bölümler hareketler için bir kapsam tanımlar (bu makalenin ilerleyen bölümünde, [Varlık grubu hareketleribölümüne](#entity-group-transactions)bakın) ve Tablo depolamanın nasıl ölçeklendirildiğinin temelini oluşturur. Tablo bölümleri hakkında daha fazla bilgi için Tablo [depolama için Performans ve ölçeklenebilirlik denetim listesine](../storage/tables/storage-performance-checklist.md)bakın.  

Tablo depolamada, tek tek bir düğüm bir veya daha fazla tam bölüme hizmet eder ve hizmet düğümleri arasında dinamik olarak yük dengeleme bölümleri ile ölçeklenir. Bir düğüm yük altındaysa, Tablo depolama, bu düğüm tarafından hizmet edilen bölüm aralığını farklı düğümlere bölebilir. Trafik yatıştığında, Tablo depolama sessiz düğümlerden tek bir düğüme geri bölüm aralıklarını birleştirilebilir.  

Tablo depolamanın iç ayrıntıları ve özellikle bölümleri nasıl yönettiği hakkında daha fazla bilgi için [Microsoft Azure Depolama: Güçlü tutarlılığa sahip yüksek oranda kullanılabilir bir bulut depolama hizmeti.](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)  

### <a name="entity-group-transactions"></a>Varlık grubu hareketleri
Tablo depolamada, varlık grubu hareketleri (EGTs) birden çok varlık arasında atomik güncelleştirmeler gerçekleştirmek için tek yerleşik mekanizmadır. EGT'ler *toplu işlem*olarak da adlandırılır. EGT'ler yalnızca aynı bölümde depolanan varlıklarda çalışabilir (belirli bir tabloda aynı bölüm anahtarını paylaşma), bu nedenle birden çok varlık arasında atomik işlem davranışına ihtiyaç duyduğunuzda, bu varlıkların aynı bölümde olduğundan emin olun. Bu genellikle aynı tabloda (ve bölüm) birden çok varlık türleri tutmak ve farklı varlık türleri için birden çok tablo kullanmamak için bir nedendir. Tek bir EGT en fazla 100 varlıkta çalışabilir.  İşleme için birden çok eşzamanlı EGT gönderirseniz, bu EGT'lerin EGT'ler arasında yaygın olan varlıklar üzerinde çalışmadığından emin olmak önemlidir. Aksi takdirde, işleme geciktirme riski.

EGT'ler ayrıca tasarımınızda değerlendirmeniz için potansiyel bir denge sağlar. Azure'un düğümler arasında yük dengeleme istekleri için daha fazla fırsatı olduğundan, daha fazla bölüm kullanmak uygulamanızın ölçeklenebilirliğini artırır. Ancak bu, uygulamanızın atomik işlemler gerçekleştirme ve verileriniz için güçlü tutarlılık sağlama yeteneğini sınırlayabilir. Ayrıca, tek bir düğüm için bekleyebilirsiniz hareketlerin iş düzeyini sınırlayabilir bir bölüm düzeyinde belirli ölçeklenebilirlik hedefleri vardır.

Azure depolama hesapları için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi [için, standart depolama hesapları için Ölçeklenebilirlik hedeflerine](../storage/common/scalability-targets-standard-account.md)bakın. Tablo depolama için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için Tablo [depolama için ölçeklenebilirlik ve performans hedefleri](../storage/tables/scalability-targets.md)ne bakın. Bu kılavuzun sonraki bölümleri, bu gibi dengeleri yönetmenize yardımcı olan çeşitli tasarım stratejilerini tartışır ve istemci uygulamanızın özel gereksinimlerine göre bölüm anahtarınızı en iyi nasıl seçeceğinizi tartışır.  

### <a name="capacity-considerations"></a>Kapasiteye ilişkin önemli noktalar
Aşağıdaki tablo, Tablo depolama çözümü tasarlarken dikkat edilmesi gereken bazı temel değerleri içerir:  

| Azure depolama hesabının toplam kapasitesi | 500 TB |
| --- | --- |
| Azure depolama hesabındaki tablo sayısı |Yalnızca depolama hesabının kapasitesiyle sınırlıdır. |
| Tablodaki bölüm sayısı |Yalnızca depolama hesabının kapasitesiyle sınırlıdır. |
| Bir bölümdeki varlıkların sayısı |Yalnızca depolama hesabının kapasitesiyle sınırlıdır. |
| Tek bir varlığın boyutu |En fazla 1 MB, en fazla 255 `PartitionKey`özellikleri `RowKey`(dahil , , ve `Timestamp`). |
| Boyutu`PartitionKey` |Boyutu 1 KB kadar bir dize. |
| Boyutu`RowKey` |Boyutu 1 KB kadar bir dize. |
| Varlık grubu hareketinin boyutu |Bir hareket en fazla 100 varlık içerebilir ve taşıma kapasitesi 4 MB'dan az olmalıdır. EGT bir varlığı yalnızca bir kez güncelleyebilir. |

Daha fazla bilgi için tablo [hizmeti veri modelini anlama](https://msdn.microsoft.com/library/azure/dd179338.aspx)konusuna bakın.  

### <a name="cost-considerations"></a>Maliyetle ilgili konular
Tablo depolama sı nispeten ucuzdur, ancak Tablo depolamasını kullanan herhangi bir çözümün değerlendirilmesinin bir parçası olarak hem kapasite kullanımı hem de hareket miktarı için maliyet tahminleri eklemeniz gerekir. Ancak birçok senaryoda, çözümünüzün performansını veya ölçeklenebilirliğini artırmak için normalize edilmiş veya yinelenen verileri depolamak geçerli bir yaklaşımdır. Fiyatlandırma hakkında daha fazla bilgi için Azure [Depolama fiyatlandırması'na](https://azure.microsoft.com/pricing/details/storage/)bakın.  

## <a name="guidelines-for-table-design"></a>Tablo tasarımı için yönergeler
Bu listeler, tablolarınızı tasarlarken göz önünde bulundurmanız gereken bazı temel yönergeleri özetler. Bu kılavuz daha sonra hepsini daha ayrıntılı olarak ele alar. Bu yönergeler, ilişkisel veritabanı tasarımı için genellikle izleyeceğiniz yönergelerden farklıdır.  

Tablo depolamanızın verimli *okunacak* şekilde tasarlanması:

* **Okuma ağırlıklı uygulamalarda sorgulama için tasarım.** Tablolarınızı tasarlarken, varlıklarınızı nasıl güncelleştireceğiniz hakkında düşünmeden önce çalıştıracağınız sorguları (özellikle gecikmeye duyarlı olanları) düşünün. Bu genellikle verimli ve performant bir çözüm le sonuçlanır.  
* **Hem `PartitionKey` de `RowKey` sorgularınızda belirtin.** Bu gibi *nokta sorguları* en verimli Tablo depolama sorgularıdır.  
* **Varlıkların yinelenen kopyalarını depolamayı düşünün.** Tablo depolama ucuz, bu nedenle daha verimli sorgular etkinleştirmek için aynı varlığı birden çok kez (farklı anahtarlarla) depolamayı düşünün.  
* **Verilerinizin normalinden arındırmayı düşünün.** Tablo depolama ucuz, bu nedenle verilerinizi denormalleştirme düşünün. Örneğin, özet varlıkları, toplu veri sorgularının yalnızca tek bir varlığa erişebilmeleri için depolayın.  
* **Bileşik anahtar değerlerini kullanın.** Sahip olduğun tek `PartitionKey` anahtar. `RowKey` Örneğin, varlıklara alternatif anahtarlı erişim yollarını etkinleştirmek için bileşik anahtar değerlerini kullanın.  
* **Sorgu projeksiyonu kullanın.** Yalnızca gereksinim duyduğunuz alanları seçen sorguları kullanarak ağ üzerinden aktardığınız veri miktarını azaltabilirsiniz.  

Tablo depolamanızın verimli *yazılması* için tasarlanması:  

* **Sıcak bölümler oluşturmayın.** İsteklerinizi herhangi bir zamanda birden çok bölüme yaymanızı sağlayan anahtarları seçin.  
* **Trafikte ani artışlardan kaçının.** Trafiği makul bir süre boyunca dağıtın ve trafikteki ani artışlardan kaçının.
* **Her varlık türü için ayrı bir tablo oluşturma.** Varlık türleri arasında atomik hareketler ekidirseniz, bu birden çok varlık türünü aynı tabloda aynı bölümde depolayabilirsiniz.
* **Başarması gereken maksimum verimi göz önünde bulundurun.** Tablo depolama için ölçeklenebilirlik hedeflerinin farkında olmalı ve tasarımınızın bunları aşmanıza neden olmayacağından emin olmalısınız.  

Daha sonra bu kılavuzda, tüm bu ilkeleri uygulamaya koyan örnekler görürsünüz.  

## <a name="design-for-querying"></a>Sorgulama için tasarım
Tablo depolama yoğun okunabilir, yoğun yazılabilir veya ikisinin bir karışımı. Bu bölümde, okuma işlemlerini verimli bir şekilde desteklemek için tasarım yapmayı düşünmektedir. Genellikle, okuma işlemlerini verimli bir şekilde destekleyen bir tasarım yazma işlemleri için de etkilidir. Ancak, yazma işlemlerini desteklemek için tasarlarken göz önünde bulundurulması gereken ek noktalar vardır. Bu sonraki bölümde ele alınmıştır, [Veri modifikasyonu için Tasarım](#design-for-data-modification).

Verileri verimli bir şekilde okumanızı sağlayan iyi bir başlangıç noktası, "Uygulamamın ihtiyacı olan verileri almak için hangi sorguları çalıştırması gerekir?" diye sormaktır.  

> [!NOTE]
> Tablo depolama ile, tasarımı önceden doğru hale getirmek önemlidir, çünkü daha sonra değiştirmek zor ve pahalıdır. Örneğin, ilişkisel bir veritabanında, yalnızca varolan bir veritabanına dizinler ekleyerek performans sorunlarını gidermek genellikle mümkündür. Bu Tablo depolama ile bir seçenek değildir.  

### <a name="how-your-choice-of-partitionkey-and-rowkey-affects-query-performance"></a>Seçtiğiniz `PartitionKey` ve `RowKey` sorgu performansını nasıl etkiler
Aşağıdaki örnekler, Tablo depolamanın çalışan varlıklarını aşağıdaki yapıyla depolayabettiğini `Timestamp` varsayar (örneklerin çoğu özelliği netlik açısından atlar):  

| Sütun adı | Veri türü |
| --- | --- |
| `PartitionKey`(Bölüm adı) |Dize |
| `RowKey`(Çalışan Kimliği) |Dize |
| `FirstName` |Dize |
| `LastName` |Dize |
| `Age` |Tamsayı |
| `EmailAddress` |Dize |

Tablo depolama sorguları tasarlamak için bazı genel yönergeler aşağıda verilmiştir. Aşağıdaki örneklerde kullanılan filtre sözdizimi Tablo depolama REST API'dendir. Daha fazla bilgi için sorgu [varlıklarına](https://msdn.microsoft.com/library/azure/dd179421.aspx)bakın.  

* *Nokta sorgusu* kullanılacak en verimli aramadır ve en düşük gecikme yi gerektiren yüksek hacimli aramalar veya aramalar için önerilir. Böyle bir sorgu, hem değerleri hem de `PartitionKey` `RowKey` değerleri belirterek tek bir varlığı verimli bir şekilde bulmak için dizinleri kullanabilir. Örneğin: `$filter=(PartitionKey eq 'Sales') and (RowKey eq '2')`.  
* İkinci en iyi *aralık sorgusudur.* Birden fazla `PartitionKey`varlık döndürmek için bir `RowKey` dizi değer üzerinde 'yi ve filtreleri kullanır. Değer `PartitionKey` belirli bir bölümü tanımlar `RowKey` ve değerler bu bölümdeki varlıkların bir alt kümesini tanımlar. Örneğin: `$filter=PartitionKey eq 'Sales' and RowKey ge 'S' and RowKey lt 'T'`.  
* Üçüncü en iyi *bir bölüm tcan*. Başka bir `PartitionKey`anahtar olmayan özellik üzerinde ,, ve filtreler kullanır ve birden fazla varlık döndürebilir. Değer `PartitionKey` belirli bir bölümü tanımlar ve özellik değerleri bu bölümdeki varlıkların bir alt kümesi için seçilir. Örneğin: `$filter=PartitionKey eq 'Sales' and LastName eq 'Smith'`.  
* *Tablo taramalari* `PartitionKey`, 'yi içermez ve eşleşen varlıklar için tablonuzu oluşturan tüm bölümleri aradığından verimsizdir. Filtrenizin .'yi `RowKey`kullanıp kullanmadığına bakılmaksızın tablo tonu yapar. Örneğin: `$filter=LastName eq 'Jones'`.  
* Birden çok varlığı döndüren Azure Tablo `PartitionKey` `RowKey` depolama sorguları bunları sıralar ve sırayla sıralar. İstemcideki varlıklara başvurmamak için, `RowKey` en yaygın sıralama sırasını tanımlayan bir sıralama seçin. Azure Cosmos DB'deki Azure Tablo API'si tarafından döndürülen sorgu sonuçları bölüm anahtarına veya satır tuşuna göre sıralanmaz. Özellik farklılıklarının ayrıntılı bir listesi için Azure [Cosmos DB'deki Tablo API'si ile Azure Tablo depolama alanı arasındaki farklara](faq.md#where-is-table-api-not-identical-with-azure-table-storage-behavior)bakın.

Değerlere**or**dayalı `RowKey` bir filtre belirtmek için " veya " kullanmak, bir bölüm taramayla sonuçlanır ve aralık sorgusu olarak kabul edilmez. Bu nedenle, gibi filtreler kullanan sorguları kaçının: `$filter=PartitionKey eq 'Sales' and (RowKey eq '121' or RowKey eq '322')`.  

Verimli sorgular çalıştırmak için Depolama İstemci Kitaplığı'nı kullanan istemci tarafı kodu örnekleri için bkz:  

* [Depolama İstemci Kitaplığı'nı kullanarak bir nokta sorgusu çalıştırma](#run-a-point-query-by-using-the-storage-client-library)
* [LINQ kullanarak birden fazla varlığı alma](#retrieve-multiple-entities-by-using-linq)
* [Sunucu tarafı projeksiyonu](#server-side-projection)  

Aynı tabloda depolanan birden çok varlık türünü işleyebilen istemci tarafı kodu örnekleri için bkz:  

* [Heterojen varlık türleri ile çalışma](#work-with-heterogeneous-entity-types)  

### <a name="choose-an-appropriate-partitionkey"></a>Uygun bir seçim yapın`PartitionKey`
Seçtiğiniz egts kullanımını etkinleştirmek için ihtiyacı dengelemek `PartitionKey` gerekir (tutarlılık sağlamak için) birden fazla bölüm (ölçeklenebilir bir çözüm sağlamak için) arasında varlıkları dağıtmak için gereksinimi karşı.  

Bir uçta, tüm varlıklarınızı tek bir bölümde depolayabilirsiniz. Ancak bu, çözümünüzün ölçeklenebilirliğini sınırlayabilir ve Tablo depolamanın istekleri ni yükleyebilmelerini engelleyebilir. Diğer uçta, bölüm başına bir varlık depolayabilirsiniz. Bu son derece ölçeklenebilir ve Tablo depolamanın istekleri yük dengesini sağlamasına olanak tanır, ancak varlık grubu hareketlerini kullanmanızı engeller.  

İdeal, `PartitionKey` verimli sorgular kullanmanıza olanak sağlar ve çözümünüzün ölçeklenebilir olmasını sağlamak için yeterli bölüme sahiptir. Genellikle, varlıklarınızın varlıklarınızı yeterli bölümlere dağıtan uygun bir özelliğe sahip olacağını göreceksiniz.

> [!NOTE]
> Örneğin, kullanıcılar veya çalışanlar hakkında bilgi depolayan bir `UserID` `PartitionKey`sistemde, iyi bir . Belirli `UserID` bir bölümü anahtar olarak kullanan birkaç varlığınız olabilir. Kullanıcı hakkında veri depolayan her varlık tek bir bölüm halinde gruplandırılır. Bu varlıklara EGT'ler üzerinden erişilebilirken, hala yüksek ölçeklenebilir.
> 
> 

Seçtiğiniz `PartitionKey` de varlıkları ekleme, güncelleştirme ve silme ile ilgili ek hususlar vardır. Daha fazla bilgi [için,](#design-for-data-modification) bu makalenin ilerleyen saatlerinde veri değişikliği için Tasarım bölümüne bakın.  

### <a name="optimize-queries-for-table-storage"></a>Tablo depolama için sorguları optimize edin
Tablo depolama, tek bir kümelenmiş `PartitionKey` dizindeki değerleri ve değerleri `RowKey` kullanarak varlıklarınızı otomatik olarak dizine verir. Nokta sorgularının kullanımı nın en verimli olmasının nedeni budur. Ancak, kümelenmiş dizin üzerinde bundan başka dizin `PartitionKey` `RowKey`yoktur ve .

Birçok tasarım, birden çok ölçüte dayalı varlıkların görünümünü etkinleştirmek için gereksinimleri karşılamalıdır. Örneğin, çalışan varlıklarını e-postaya, çalışan kimliğine veya soyadına göre bulma. Bölüm Tablo tasarım [desenleri](#table-design-patterns) aşağıdaki desenler gereksinimleri bu tür adresi. Desenler, Tablo depolamaikincil dizinler sağlamadığı gerçeği etrafında çalışma yollarını da açıklar.  

* [Bölüm içi ikincil dizin deseni](#intra-partition-secondary-index-pattern): Her varlığın birden çok kopyasını farklı `RowKey` değerler kullanarak (aynı bölümde) saklayın. Bu, hızlı ve verimli aramalar ve farklı `RowKey` değerler kullanarak siparişleri alternatif sıralama sağlar.  
* [Bölümler arası ikincil dizin deseni](#inter-partition-secondary-index-pattern): Her varlığın birden çok kopyasını ayrı bölümlerde veya ayrı tablolarda farklı `RowKey` değerler kullanarak saklayın. Bu, hızlı ve verimli aramalar ve farklı `RowKey` değerler kullanarak siparişleri alternatif sıralama sağlar.  
* [Dizin varlıkları deseni](#index-entities-pattern): Varlıkların listelerini döndüren verimli aramaları etkinleştirmek için dizin varlıklarını koruyun.  

### <a name="sort-data-in-table-storage"></a>Tablo depolamasında verileri sıralama

Tablo depolama, artan sırada sıralanmış sorgu `PartitionKey` sonuçlarını, `RowKey`'ye göre ve sonra da .

> [!NOTE]
> Azure Cosmos DB'deki Azure Tablo API'si tarafından döndürülen sorgu sonuçları bölüm anahtarına veya satır tuşuna göre sıralanmaz. Özellik farklılıklarının ayrıntılı bir listesi için Azure [Cosmos DB'deki Tablo API'si ile Azure Tablo depolama alanı arasındaki farklara](faq.md#where-is-table-api-not-identical-with-azure-table-storage-behavior)bakın.

Tablo depolamasındaki anahtarlar dize değerleridir. Sayısal değerlerin doğru sıraladığından emin olmak için, bunları sabit bir uzunluğa dönüştürmeli ve sıfırlarla bürünmelisiniz. Örneğin, kullandığınız çalışan kimliği değeri bir `RowKey` integer değeri yse, çalışan kimliğini **123** ile **00000123**arasında dönüştürmeniz gerekir. 

Birçok uygulamanın farklı siparişlerde sıralanmış verileri kullanmak için gereksinimleri vardır: örneğin, çalışanları ada göre sıralama veya tarihe katılma. Bölüm Tablo tasarım [desenleri](#table-design-patterns) aşağıdaki desenler nasıl varlıklarınız için sipariş alternatif sıralama adresi:  

* [Bölüm içi ikincil dizin deseni](#intra-partition-secondary-index-pattern): Her varlığın birden çok kopyasını farklı `RowKey` değerler kullanarak (aynı bölümde) saklayın. Bu, hızlı ve verimli aramalar ve farklı `RowKey` değerler kullanarak siparişleri alternatif sıralama sağlar.  
* [Bölümler arası ikincil dizin deseni](#inter-partition-secondary-index-pattern): Ayrı tablolarda farklı bölümlerde farklı `RowKey` değerler kullanarak her varlığın birden çok kopyasını saklayın. Bu, hızlı ve verimli aramalar ve farklı `RowKey` değerler kullanarak siparişleri alternatif sıralama sağlar.
* [Günlük kuyruk deseni](#log-tail-pattern): Bir bölüme en son eklenen `RowKey` *n* varlıklarını, ters tarih ve saat sırasına göre sıralayan bir değer kullanarak alın.  

## <a name="design-for-data-modification"></a>Veri değişikliği için tasarım
Bu bölümde, ekler, güncelleştirmeler ve silmeleri en iyi duruma sokmak için tasarım konuları üzerinde duruluyor. Bazı durumlarda, veri modifikasyonu için en iyi duruma getirici tasarımlara karşı sorgulama için en iyi duruma gelen tasarımlar arasındaki dengeyi değerlendirmeniz gerekir. Bu değerlendirme, ilişkisel veritabanları için tasarımlarda yaptığınıza benzer (tasarım dengelerini yönetme teknikleri ilişkisel bir veritabanında farklı olsa da). Bölüm [Tablo tasarım desenleri](#table-design-patterns) Tablo depolama için bazı ayrıntılı tasarım desenleri açıklar ve bu dengelerin bazılarını vurgular. Uygulamada, varlıkları sorgulamak için optimize edilmiş birçok tasarım da varlıkları değiştirmek için iyi çalışır bulacaksınız.  

### <a name="optimize-the-performance-of-insert-update-and-delete-operations"></a>Ekleme, güncelleştirme ve silme işlemlerinin performansını optimize edin
Bir varlığı güncelleştirmek veya silmek için, bu `PartitionKey` varlığı `RowKey` ve değerleri kullanarak tanımlayabilmelisin. Bu bağlamda, seçtiğiniz `PartitionKey` ve `RowKey` varlıkları değiştirmek için nokta sorguları desteklemek için seçtiğiniz benzer ölçütleri takip etmelidir. Varlıkları mümkün olduğunca verimli bir şekilde tanımlamak istiyorsunuz. Güncelleştirmeniz veya silmeniz gereken değerleri ve `PartitionKey` bir `RowKey` varlığı bulmak için verimsiz bir bölüm veya tablo tonu kullanmak istemezsiniz.  

Bölüm Tablo tasarım [desenleri](#table-design-patterns) aşağıdaki desenler ekleme, güncelleme ve silme işlemleri performansını optimize adresi:  

* [Yüksek hacimli silme deseni](#high-volume-delete-pattern): Tüm varlıkları kendi ayrı tablolarında eşzamanlı silme için depolayarak yüksek hacimli varlıkların silinmesini etkinleştirin. Tabloyu silerek varlıkları silersiniz.  
* [Veri serisi deseni](#data-series-pattern): Yaptığınız istek sayısını en aza indirmek için tüm veri serilerini tek bir varlıkta saklayın.  
* [Geniş varlıklar deseni](#wide-entities-pattern): 252'den fazla özelliyi olan mantıksal varlıkları depolamak için birden çok fiziksel varlık kullanın.  
* [Büyük varlıklar deseni](#large-entities-pattern): Büyük özellik değerlerini depolamak için blob depolamasını kullanın.  

### <a name="ensure-consistency-in-your-stored-entities"></a>Depolanan varlıklarınızda tutarlılık sağlayın
Veri modifikasyonlarını optimize etmek için seçtiğiniz anahtarları etkileyen bir diğer önemli faktör, atomik işlemleri kullanarak tutarlılığı nasıl sağlayacağınızdır. EGT'yi yalnızca aynı bölümde depolanan varlıklar üzerinde çalışmak için kullanabilirsiniz.  

Bölüm Tablo tasarım [desenleri](#table-design-patterns) aşağıdaki desenler tutarlılık yönetimi adresi:  

* [Bölüm içi ikincil dizin deseni](#intra-partition-secondary-index-pattern): Her varlığın birden çok kopyasını farklı `RowKey` değerler kullanarak (aynı bölümde) saklayın. Bu, hızlı ve verimli aramalar ve farklı `RowKey` değerler kullanarak siparişleri alternatif sıralama sağlar.  
* [Bölümler arası ikincil dizin deseni](#inter-partition-secondary-index-pattern): Her varlığın birden çok kopyasını ayrı bölümlerde veya ayrı tablolarda farklı `RowKey` değerler kullanarak saklayın. Bu, hızlı ve verimli aramalar ve farklı `RowKey` değerler kullanarak siparişleri alternatif sıralama sağlar.  
* [Sonunda tutarlı hareketler deseni](#eventually-consistent-transactions-pattern): Azure kuyruklarını kullanarak bölüm sınırları veya depolama sistemi sınırları arasında sonunda tutarlı davranış lar etkinleştirin.
* [Dizin varlıkları deseni](#index-entities-pattern): Varlıkların listelerini döndüren verimli aramaları etkinleştirmek için dizin varlıklarını koruyun.  
* [Denormalization pattern](#denormalization-pattern): Tek bir nokta sorgusu ile ihtiyacınız olan tüm verileri almak için sağlamak için, tek bir varlık birlikte ilgili verileri birleştirin.  
* [Veri serisi deseni](#data-series-pattern): Yaptığınız istek sayısını en aza indirmek için tam veri serisini tek bir varlıkta saklayın.  

Daha fazla bilgi için, bu makalenin ilerleyen saatlerinde [Varlık grubu hareketlerine](#entity-group-transactions) bakın.  

### <a name="ensure-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>Verimli modifikasyonlar için tasarımınızın verimli sorguları kolaylaştırdığından emin olun
Çoğu durumda, verimli sorgulama sonuçları için bir tasarım verimli değişiklikler, ancak her zaman bu belirli senaryo için durum olup olmadığını değerlendirmek gerekir. Bölüm [Tablo tasarım desenleri](#table-design-patterns) bazı açıkça sorgulama ve varlıkları değiştirme arasındaki dengeleri değerlendirmek ve her zaman her tür işlem sayısını dikkate almalıdır.  

Bölüm Tablo tasarım [desenleri](#table-design-patterns) aşağıdaki desenler verimli sorgular için tasarım ve verimli veri modifikasyonu için tasarım arasındaki dengeleri gidermek:  

* [Bileşik anahtar deseni](#compound-key-pattern): İstemcinin ilgili verileri tek bir nokta sorgusuyla aramasını sağlamak için bileşik `RowKey` değerleri kullanın.  
* [Günlük kuyruk deseni](#log-tail-pattern): Bir bölüme en son eklenen `RowKey` *n* varlıklarını, ters tarih ve saat sırasına göre sıralayan bir değer kullanarak alın.  

## <a name="encrypt-table-data"></a>Tablo verilerini şifreleme
.NET Azure Depolama istemcikitap, ekleme ve değiştirme işlemleri için dize varlık özelliklerinin şifrelemesini destekler. Şifrelenmiş dizeleri ikili özellikleri olarak hizmette depolanır ve şifre çözme sonra dizeleri geri dönüştürülür.    

Tablolar için, şifreleme ilkesine ek olarak, kullanıcıların şifrelenecek özellikleri belirtmesi gerekir. Bir `EncryptProperty` öznitelik belirtin (bunlardan `TableEntity`türeyen POCO varlıkları için), veya istek seçeneklerinde bir şifreleme çözümleyicisi belirtin. Şifreleme çözümleyicisi, bölüm anahtarı, satır anahtarı ve özellik adı alan ve bu özelliğin şifrelenip şifrelenmemesi gerektiğini belirten bir Boolean döndüren bir temsilcidir. Şifreleme sırasında, istemci kitaplığı, bir özelliğin kabloya yazarken şifrelenip şifrelenmemesi gerektiğine karar vermek için bu bilgileri kullanır. Temsilci ayrıca özelliklerin nasıl şifrelendirilenleri hakkında mantık olasılığını da sağlar. (Örneğin, X ise, a özelliğini şifreleyin; aksi takdirde A ve B özelliklerini şifreleyin.) Varlıkları okurken veya sorgularken bu bilgileri sağlamak gerekli değildir.

Birleştirme şu anda desteklenmiyor. Özelliklerin bir alt kümesi daha önce farklı bir anahtar kullanılarak şifrelenmiş olabileceğinden, yeni özellikleri birleştirmek ve meta verileri güncelleştirmek veri kaybına neden olur. Birleştirme, hizmetten önceden varolan varlığı okumak için ek hizmet çağrıları yapmayı veya mülk başına yeni bir anahtar kullanmayı gerektirir. Bunların hiçbiri performans nedenleriyle uygun değildir.     

Tablo verilerini şifreleme hakkında bilgi için, [Microsoft Azure Depolama için İstemci tarafı şifreleme ve Azure Anahtar Kasası'na](../storage/common/storage-client-side-encryption.md)bakın.  

## <a name="model-relationships"></a>Model ilişkileri
Etki alanı modelleri oluşturmak karmaşık sistemlerin tasarımında önemli bir adımdır. Genellikle, iş alanını anlamak ve sisteminizin tasarımını bilgilendirmek için varlıkları ve aralarındaki ilişkileri tanımlamak için modelleme işlemini kullanırsınız. Bu bölümde, etki alanı modellerinde bulunan ortak ilişki türlerinden bazılarını Tablo depolama tasarımlarına nasıl çevirebileceğiniz üzerinde duruluyor. Mantıksal bir veri modelinden fiziksel NoSQL tabanlı veri modeline eşleme işlemi, ilişkisel bir veritabanı tasarlarken kullanılandan farklıdır. İlişkisel veritabanları tasarımı genellikle artıklığı en aza indirmek için optimize edilmiş bir veri normalleştirme işlemi varsayar. Bu tür tasarım, veritabanının nasıl çalıştığını özetleyen bir bildirimsel sorgu lama özelliği de varsayar.  

### <a name="one-to-many-relationships"></a>Bir-çok ilişkileri
İş alanı nesneleri arasında bir-çok ilişkisi sık sık oluşur: örneğin, bir bölümün çok çalışanı vardır. Tablo depolama alanında, her biri belirli senaryoyla alakalı olabilecek artıları ve eksileri olan bir-çok ilişkileri uygulamanın çeşitli yolları vardır.  

On binlerce departman ve çalışan varlığı olan çok uluslu büyük bir şirket örneğini düşünün. Her bölümün çok sayıda çalışanı vardır ve her çalışan belirli bir departmanla ilişkilidir. Bir yaklaşım, aşağıdaki gibi ayrı departman ve çalışan varlıkları depolamaktır:  

![Bir departman varlığını ve çalışan tüzel kişiliğini gösteren grafik][1]

Bu örnek, `PartitionKey` değere dayalı türler arasında örtülü bir-çok ilişkisi gösterir. Her bölümün birçok çalışanı olabilir.  

Bu örnek, aynı bölümde bir departman varlığı ve ilgili çalışan varlıklarını da gösterir. Farklı varlık türleri için farklı bölümler, tablolar ve hatta depolama hesapları kullanmayı seçebilirsiniz.  

Alternatif bir yaklaşım, verilerinizi normalden arındırmak ve aşağıdaki örnekte gösterildiği gibi yalnızca normalden arındırılmış departman verilerine sahip çalışan varlıkları depolamaktır. Bu özel senaryoda, bir departman yöneticisinin ayrıntılarını değiştirebilme gereksiniminiz varsa, bu normaldışı yaklaşım en iyisi olmayabilir. Bunu yapmak için departmandaki tüm çalışanları güncelleştirmeniz gerekir.  

![Çalışan varlığının grafiği][2]

Daha fazla bilgi için, daha sonra bu kılavuzda [Denormalization deseni](#denormalization-pattern) bakın.  

Aşağıdaki tablo, bir-çok ilişkisi olan çalışan ve departman varlıklarını depolamak için yaklaşımların her birinin artılarını ve eksilerini özetler. Ayrıca, çeşitli işlemleri ne sıklıkta gerçekleştirmeyi beklediğiniz de göz önünde bulundurmalısınız. Bu işlem yalnızca seyrek gerçekleşirse, pahalı bir işlem içeren bir tasarıma sahip olmak kabul edilebilir.  

<table>
<tr>
<th>Yaklaşım</th>
<th>Artıları</th>
<th>Simgeler</th>
</tr>
<tr>
<td>Ayrı varlık türleri, aynı bölüm, aynı tablo</td>
<td>
<ul>
<li>Bir departman varlığını tek bir işlemle güncelleştirebilirsiniz.</li>
<li>Bir çalışan varlığını güncellediğinizde/eklediğinizde/sildiğinizde bir departman varlığını değiştirme gereksiniminiz varsa tutarlılığı korumak için Bir EGT kullanabilirsiniz. Örneğin, her departman için bir departman çalışan sayısı tutarsanız.</li>
</ul>
</td>
<td>
<ul>
<li>Bazı istemci etkinlikleri için hem çalışan hem de departman varlığını almanız gerekebilir.</li>
<li>Depolama işlemleri aynı bölümde gerçekleşir. Yüksek işlem birimlerinde bu bir etkin nokta neden olabilir.</li>
<li>EGT kullanarak bir çalışanı yeni bir departmana taşıyamazsınız.</li>
</ul>
</td>
</tr>
<tr>
<td>Ayrı varlık türleri, farklı bölümler veya tablolar veya depolama hesapları</td>
<td>
<ul>
<li>Bir departman varlığını veya çalışan tüzel kişiliğini tek bir işlemle güncelleştirebilirsiniz.</li>
<li>Yüksek işlem birimlerinde, bu yükün daha fazla bölüme yayılmasına yardımcı olabilir.</li>
</ul>
</td>
<td>
<ul>
<li>Bazı istemci etkinlikleri için hem çalışan hem de departman varlığını almanız gerekebilir.</li>
<li>Bir çalışanı güncellediğinizde/eklediğinizde/sildiğinizde ve bir bölümü güncellediğinizde tutarlılığı korumak için EGT'leri kullanamazsınız. Örneğin, bir departman varlığındaki çalışan sayısını güncelleştirme.</li>
<li>EGT kullanarak bir çalışanı yeni bir departmana taşıyamazsınız.</li>
</ul>
</td>
</tr>
<tr>
<td>Tek varlık türüne normalleştirme</td>
<td>
<ul>
<li>Tek bir istekle ihtiyacınız olan tüm bilgileri alabilirsiniz.</li>
</ul>
</td>
<td>
<ul>
<li>Departman bilgilerini güncelleştirmeniz gerekiyorsa tutarlılığı korumak pahalıya mal olabilir (bu, bir departmandaki tüm çalışanları güncelleştirmenizi gerektirir).</li>
</ul>
</td>
</tr>
</table>

Bu seçenekler arasından nasıl seçim yaptığınız ve artıları ve eksileri en önemli olan, belirli uygulama senaryolarınıza bağlıdır. Örneğin, departman varlıklarını ne sıklıkta değiştirebilirsiniz? Tüm çalışan sorgularınızın ek departman bilgilerine ihtiyacı var mı? Bölümlerinizdeki veya depolama hesabınızdaki ölçeklenebilirlik sınırlarına ne kadar yakınsınız?  

### <a name="one-to-one-relationships"></a>Bire bir ilişkiler
Etki alanı modelleri, varlıklar arasında bire bir ilişkiler içerebilir. Tablo depolama alanında bire bir ilişki uygulamanız gerekiyorsa, her ikisini de almanız gerektiğinde ilgili iki varlığı nasıl bağlayacaklarını da seçmeniz gerekir. Bu bağlantı, her varlıktaki bir bağlantıyı ilgili varlığa biçim `PartitionKey` ve `RowKey` değer biçiminde depolayarak, anahtar değerlerdeki bir kurala dayalı olarak örtülü olabilir. İlgili varlıkları aynı bölümde depolamanız gerekip gerekmediği ne tartışmaya yönelik bir tartışma için, [birden çok'a ilişki](#one-to-many-relationships)bölümüne bakın.  

Tablo depolama alanında bire bir ilişkileri uygulamanıza yol açabilecek uygulama konuları da vardır:  

* Büyük varlıkları işleme (daha fazla bilgi için bkz. [Büyük varlıklar deseni).](#large-entities-pattern)  
* Erişim denetimlerini uygulama (daha fazla bilgi için [bkz. paylaşılan erişim imzalarıyla erişimi denetle).](#control-access-with-shared-access-signatures)  

### <a name="join-in-the-client"></a>İstemciye katılın
Tablo depolama alanında ilişkileri modellemenin yolları olsa da, Tablo depolamasını kullanmanın başlıca iki nedeninin ölçeklenebilirlik ve performans olduğunu unutmayın. Çözümünüzün performansını ve ölçeklenebilirliğini tehlikeye sosan birçok ilişkiyi modellediğinizi fark ederseniz, tablo tasarımınızda tüm veri ilişkilerini oluşturmanın gerekli olup olmadığını kendinize sormalısınız. İstemci uygulamanızın gerekli birleştirmeleri gerçekleştirmesine izin verirsen, tasarımı basitleştirebilir ve çözümünüzün ölçeklenebilirliğini ve performansını artırabilirsiniz.  

Örneğin, sık sık değişmeyen veriler içeren küçük tablolarınız varsa, bu verileri bir kez alabilir ve istemciüzerinde önbelleğe alabilirsiniz. Bu, aynı verileri almak için tekrarlanan gidiş dönüşleri önleyebilir. Bu kılavuzda baktığımız örneklerde, küçük bir kuruluştaki departmanlar kümesinin küçük olması ve seyrek olarak değişmesi olasıdır. Bu, istemci uygulamasının bir kez indirebileceği ve arama verisi olarak önbelleğe alabildiği veriler için iyi bir aday olmasını sağlar.  

### <a name="inheritance-relationships"></a>Kalıtım ilişkileri
İstemci uygulamanız, iş varlıklarını temsil etmek için devralma ilişkisinin bir parçasını oluşturan bir sınıf kümesi kullanıyorsa, bu varlıkları Tablo depolama alanında kolayca devam edebilirsiniz. Örneğin, istemci uygulamanızda soyut bir sınıf olan `Person` aşağıdaki sınıflar kümesi tanımlanabilir.

![Kalıtım ilişkilerinin diyagramı][3]

Tablo depolamadaki iki somut sınıfın örneklerini tek `Person` bir tablo kullanarak devam edebilirsiniz. Aşağıdaki gibi görünen varlıkları kullanın:  

![Müşteri varlığını ve çalışan varlığını gösteren grafik][4]

İstemci kodunda aynı tabloda birden çok varlık türüyle çalışma hakkında daha fazla bilgi için, daha sonra bu kılavuzda [heterojen varlık türleri ile Çalışma'ya](#work-with-heterogeneous-entity-types) bakın. Bu, istemci kodundaki varlık türünü nasıl tanıyacağınıza ilgili örnekler sağlar.  

## <a name="table-design-patterns"></a>Tablo tasarımı desenleri
Önceki bölümlerde, hem sorguları kullanarak hem de varlık verilerini ekleme, güncelleştirme ve silme için tablo tasarımınızı en iyi duruma getirmehakkında bilgi edinilmişsinizdir. Bu bölümde, Tablo depolama ile kullanıma uygun bazı desenler açıklanmaktadır. Buna ek olarak, bu kılavuzda daha önce gündeme getirilen bazı sorunları ve dengeleri pratik olarak nasıl ele alabileceğinizi göreceksiniz. Aşağıdaki diyagram, farklı desenler arasındaki ilişkileri özetler:  

![Tablo tasarım desenleri diyagramı][5]

Desen eşlemi, bu kılavuzda belgelenen desenler (mavi) ve anti-desenler (turuncu) arasındaki bazı ilişkileri vurgular. Tabii ki dikkate değer birçok diğer desenler vardır. Örneğin, Tablo depolama için önemli senaryolardan biri komut sorgusu sorumluluk [ayırma](https://msdn.microsoft.com/library/azure/jj554200.aspx) deseninden [maddeleştirilmiş görünüm deseni](https://msdn.microsoft.com/library/azure/dn589782.aspx) kullanmaktır.  

### <a name="intra-partition-secondary-index-pattern"></a>Bölüm içi ikincil dizin deseni
Her varlığın birden çok kopyasını `RowKey` farklı değerler kullanarak (aynı bölümde) depolayın. Bu, hızlı ve verimli aramalar ve farklı `RowKey` değerler kullanarak siparişleri alternatif sıralama sağlar. Kopyalar arasındaki güncellemeler EGT'ler kullanılarak tutarlı tutulabilir.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tablo depolama otomatik olarak ve `PartitionKey` `RowKey` değerleri kullanarak varlıkları dizinler. Bu, istemci uygulamasının bu değerleri kullanarak bir varlığı verimli bir şekilde almasına olanak tanır. Örneğin, aşağıdaki tablo yapısını kullanarak, istemci uygulaması departman adını ve çalışan kimliğini (ve `PartitionKey` `RowKey` değerleri) kullanarak tek bir çalışan varlığını almak için bir nokta sorgusu kullanabilir. İstemci, her departmanda çalışan kimliğine göre sıralanmış varlıkları da alabilir.

![Çalışan varlığının grafiği][6]

Ayrıca, e-posta adresi gibi başka bir özelliğin değerine dayalı bir çalışan varlığı bulmak istiyorsanız, eşleşme bulmak için daha az verimli bir bölüm taramayı kullanmanız gerekir. Bunun nedeni Tablo depolama ikincil dizinler sağlamamasıdır. Ayrıca, `RowKey` sırayla farklı bir sırada sıralanmış çalışanların listesini isteme seçeneği de yoktur.  

#### <a name="solution"></a>Çözüm
İkincil dizinlerin eksikliğini aşmak için, her varlığın farklı bir `RowKey` değer kullanarak her kopyayla birlikte birden çok kopyasını depolayabilirsiniz. Aşağıdaki yapılara sahip bir varlığı depolarsanız, e-posta adresine veya çalışan kimliğine göre çalışan varlıklarını verimli bir şekilde alabilirsiniz. Bir dizi e-posta adresi veya çalışan tonu kullanarak tek bir çalışan veya çeşitli çalışan için önek değerleri `RowKey` `empid_`ve sorgulama yapmanızı `email_` sağlar.  

![Farklı RowKey değerlerine sahip çalışan varlığını gösteren grafik][7]

Aşağıdaki iki filtre ölçütü (biri çalışan kimliğine, diğeri e-posta adresine göre bakar) her ikisi de nokta sorgularını belirtir:  

* $filter=(PartitionKey eq 'Satış') ve (RowKey eq 'empid_000223')  
* $filter=(PartitionKey eq 'Satış') ve (RowKey eq 'email_jonesj@contoso.com')  

Bir dizi çalışan varlıkları için sorgu larsanız, çalışan kimliği siparişinde sıralanmış bir aralık veya e-posta adresi siparişinde sıralanmış bir aralık belirtebilirsiniz. `RowKey`'de uygun önek olan varlıklar için sorgula  

* Satış departmanında 000100 ile 000199 arasında çalışan kimliği olan tüm çalışanları bulmak için şunları kullanın: $filter=(PartitionKey eq 'Satış') ve (RowKey ge 'empid_000100') ve (RowKey le 'empid_000199')  
* Satış departmanındaki tüm çalışanları "a" harfiile başlayan bir e-posta adresiyle bulmak için kullanın: $filter=(PartitionKey eq 'Sales') ve (RowKey ge 'email_a') ve (RowKey lt 'email_b')  
  
Önceki örneklerde kullanılan filtre sözdizimi Tablo depolama REST API'den gelir. Daha fazla bilgi için sorgu [varlıklarına](https://msdn.microsoft.com/library/azure/dd179421.aspx)bakın.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tablo depolama kullanımı nispeten ucuzolduğundan, yinelenen verileri depolama maliyeti yükü önemli bir sorun olmamalıdır. Ancak, her zaman beklenen depolama gereksinimlerinize göre tasarımınızın maliyetini değerlendirmeli ve yalnızca istemci uygulamanızın çalıştıracağı sorguları desteklemek için yinelenen varlıklar eklemeniz gerekir.  
* İkincil dizin varlıkları özgün varlıklarla aynı bölümde depolandığı için, tek bir bölüm için ölçeklenebilirlik hedeflerini aşmadığınızdan emin olun.  
* Varlığın iki kopyasını atomik olarak güncelleştirmek için EGT'leri kullanarak yinelenen varlıklarınızı birbiriyle tutarlı tutabilirsiniz. Bu, bir varlığın tüm kopyalarını aynı bölümde depolamanız gerektiği anlamına gelir. Daha fazla bilgi için [bkz.](#entity-group-transactions)  
* Için kullanılan değer `RowKey` her varlık için benzersiz olmalıdır. Bileşik anahtar değerlerini kullanmayı düşünün.  
* Sayısal değerleri `RowKey` doldurma (örneğin, çalışan kimliği 000223) üst ve alt sınırlara göre doğru sıralama ve filtreleme sağlar.  
* Varlığınızın tüm özelliklerini yinelemeniz gerekmez. Örneğin, çalışanın yaşına `RowKey` gerek yokken e-posta adresini kullanarak varlıkları arayan sorgular aşağıdaki yapıya sahip olabilir:

  ![Çalışan varlığının grafiği][8]

* Genellikle, yinelenen verileri depolamak ve bir varlığı bulmak için bir sorgu ve gerekli verileri aramak için başka bir sorgu kullanmak yerine, tek bir sorgu ile ihtiyacınız olan tüm verileri alabilirsiniz emin olmak daha iyidir.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Bu düzeni aşağıdaki durumlarda kullanın:

- İstemci uygulamanızın çeşitli farklı anahtarlar kullanarak varlıkları alması gerekir.
- Müşterinizin farklı tür siparişlerdeki varlıkları alması gerekir.
- Her varlığı çeşitli benzersiz değerler kullanarak tanımlayabilirsiniz.

Ancak, farklı `RowKey` değerleri kullanarak varlık aramaları gerçekleştirirken bölüm ölçeklenebilirlik sınırlarını aşmadığınızdan emin olun.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bölümler arası ikincil dizin deseni](#inter-partition-secondary-index-pattern)
* [Bileşik anahtar deseni](#compound-key-pattern)
* [Varlık grubu hareketleri](#entity-group-transactions)
* [Heterojen varlık türleri ile çalışma](#work-with-heterogeneous-entity-types)

### <a name="inter-partition-secondary-index-pattern"></a>Bölümler arası ikincil dizin deseni
Ayrı bölümlerde veya ayrı tablolarda farklı `RowKey` değerler kullanarak her varlığın birden çok kopyasını depolayın. Bu, hızlı ve verimli aramalar ve farklı `RowKey` değerler kullanarak siparişleri alternatif sıralama sağlar.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tablo depolama otomatik olarak ve `PartitionKey` `RowKey` değerleri kullanarak varlıkları dizinler. Bu, istemci uygulamasının bu değerleri kullanarak bir varlığı verimli bir şekilde almasına olanak tanır. Örneğin, aşağıdaki tablo yapısını kullanarak, istemci uygulaması departman adını ve çalışan kimliğini (ve `PartitionKey` `RowKey` değerleri) kullanarak tek bir çalışan varlığını almak için bir nokta sorgusu kullanabilir. İstemci, her departmanda çalışan kimliğine göre sıralanmış varlıkları da alabilir.  

![Çalışan varlığının grafiği][9]

Ayrıca, e-posta adresi gibi başka bir özelliğin değerine dayalı bir çalışan varlığı bulmak istiyorsanız, eşleşme bulmak için daha az verimli bir bölüm taramayı kullanmanız gerekir. Bunun nedeni Tablo depolama ikincil dizinler sağlamamasıdır. Ayrıca, `RowKey` sırayla farklı bir sırada sıralanmış çalışanların listesini isteme seçeneği de yoktur.  

Bu varlıklara karşı yüksek hacimli işlem bekliyorsunuz ve müşterinizi sınırlayan Tablo depolama oranı riskini en aza indirmek istiyorsunuz.  

#### <a name="solution"></a>Çözüm
İkincil dizinlerin eksikliğini aşmak için, her varlığın farklı `PartitionKey` ve `RowKey` değerleri kullanarak her kopyayla birlikte birden çok kopyasını depolayabilirsiniz. Aşağıdaki yapılara sahip bir varlığı depolarsanız, e-posta adresine veya çalışan kimliğine göre çalışan varlıklarını verimli bir şekilde alabilirsiniz. `PartitionKey`Önkekleri , `empid_`ve `email_` bir sorgu için kullanmak istediğiniz dizini belirlemenizi sağlar.  

![Birincil dizinve ikincil dizinile çalışan varlık gösteren grafik][10]

Aşağıdaki iki filtre ölçütü (biri çalışan kimliğine, diğeri e-posta adresine göre bakar) her ikisi de nokta sorgularını belirtir:  

* $filter=(PartitionKey eq 'empid_Sales') ve (RowKey eq '000223')
* $filter=(PartitionKey eq 'email_Sales') ve (RowKey eq 'jonesj@contoso.com')  

Bir dizi çalışan varlıkları için sorgu larsanız, çalışan kimliği siparişinde sıralanmış bir aralık veya e-posta adresi siparişinde sıralanmış bir aralık belirtebilirsiniz. `RowKey`'de uygun önek olan varlıklar için sorgula  

* **000100** ile **000199**aralığında çalışan kimliği olan Satış departmanındaki tüm çalışanları bulmak için, çalışan kimliği sırasına göre sıralanır: $filter=(PartitionKey eq 'empid_Sales') ve (RowKey ge '000100') ve (RowKey le '000199')  
* Satış departmanındaki tüm çalışanları e-posta adresi sırasına göre sıralanmış "a" ile başlayan bir e-posta adresiyle bulmak için: $filter=(PartitionKey eq 'email_Sales') ve (RowKey ge 'a') ve (RowKey lt 'b')  

Önceki örneklerde kullanılan filtre sözdiziminin Tablo depolama REST API'sinden olduğunu unutmayın. Daha fazla bilgi için sorgu [varlıklarına](https://msdn.microsoft.com/library/azure/dd179421.aspx)bakın.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Birincil ve ikincil dizin varlıklarını korumak için [Son olarak tutarlı hareketler modelini](#eventually-consistent-transactions-pattern) kullanarak yinelenen varlıklarınızı sonunda birbiriyle tutarlı tutabilirsiniz.  
* Tablo depolama kullanımı nispeten ucuzolduğundan, yinelenen verileri depolama maliyeti yükü önemli bir sorun olmamalıdır. Ancak, her zaman beklenen depolama gereksinimlerinize göre tasarımınızın maliyetini değerlendirin ve yalnızca istemci uygulamanızın çalıştıracağı sorguları desteklemek için yinelenen varlıklar ekleyin.  
* Için kullanılan değer `RowKey` her varlık için benzersiz olmalıdır. Bileşik anahtar değerlerini kullanmayı düşünün.  
* Sayısal değerleri `RowKey` doldurma (örneğin, çalışan kimliği 000223) üst ve alt sınırlara göre doğru sıralama ve filtreleme sağlar.  
* Varlığınızın tüm özelliklerini yinelemeniz gerekmez. Örneğin, çalışanın yaşına `RowKey` gerek yokken e-posta adresini kullanarak varlıkları arayan sorgular aşağıdaki yapıya sahip olabilir:
  
  ![İkincil dizinli çalışan varlığını gösteren grafik][11]
* Genellikle, ikincil dizini kullanarak bir varlığı bulmak için bir sorgu kullanmak ve birincil dizinde gerekli verileri aramak için başka bir sorgu kullanmak yerine, yinelenen verileri depolamak ve tek bir sorgu ile ihtiyacınız olan tüm verileri alabilirsiniz emin olmak daha iyidir.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Bu düzeni aşağıdaki durumlarda kullanın:

- İstemci uygulamanızın çeşitli farklı anahtarlar kullanarak varlıkları alması gerekir.
- Müşterinizin farklı tür siparişlerdeki varlıkları alması gerekir.
- Her varlığı çeşitli benzersiz değerler kullanarak tanımlayabilirsiniz.

Farklı `RowKey` değerleri kullanarak varlık aramaları gerçekleştirirken bölüm ölçeklenebilirlik sınırlarını aşmak tan kaçınmak istediğinizde bu deseni kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Sonunda tutarlı hareketler deseni](#eventually-consistent-transactions-pattern)  
* [Bölüm içi ikincil dizin deseni](#intra-partition-secondary-index-pattern)  
* [Bileşik anahtar deseni](#compound-key-pattern)  
* [Varlık grubu hareketleri](#entity-group-transactions)  
* [Heterojen varlık türleri ile çalışma](#work-with-heterogeneous-entity-types)  

### <a name="eventually-consistent-transactions-pattern"></a>Sonunda tutarlı hareketler deseni
Azure kuyruklarını kullanarak bölüm sınırları veya depolama sistemi sınırları arasında en geç tutarlı davranışı etkinleştirin.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
EGT'ler, aynı bölüm anahtarını paylaşan birden çok kuruluş arasında atomik işlemlere olanak tanır. Performans ve ölçeklenebilirlik nedenleriyle, tutarlılık gereksinimleri olan varlıkları ayrı bölümlerde veya ayrı bir depolama sisteminde depolamaya karar verebilirsiniz. Böyle bir senaryoda, tutarlılığı korumak için EGT'leri kullanamazsınız. Örneğin, arasında nihai tutarlılığı korumak için bir gereksinim olabilir:  

* Aynı tabloda, farklı tablolarda veya farklı depolama hesaplarında iki farklı bölümde depolanan varlıklar.  
* Tablo depolamasında depolanan bir varlık ve Blob depolamada depolanan bir leke.  
* Tablo depolama alanında depolanan bir varlık ve dosya sistemindeki bir dosya.  
* Tablo depolama alanında depolanan, ancak Azure Bilişsel Arama kullanılarak dizine eklenmiş bir varlık.  

#### <a name="solution"></a>Çözüm
Azure kuyruklarını kullanarak, iki veya daha fazla bölüm veya depolama sisteminde nihai tutarlılık sağlayan bir çözüm uygulayabilirsiniz.

Bu yaklaşımı göstermek için, eski çalışan varlıklarını arşivleme gereksiniminiz olduğunu varsayalım. Eski çalışan varlıkları nadiren sorgulanır ve geçerli çalışanlarla ilgili tüm etkinliklerin dışında tutulmalıdır. Bu gereksinimi uygulamak için, etkin çalışanları **Geçerli** tabloda ve eski çalışanları **Arşiv** tablosunda saklarsınız. Bir çalışanın arşivleme geçerli **tablodan** varlığı silmek ve **arşiv** tablosuna varlık eklemek gerektirir.

Ancak bu iki işlemi gerçekleştirmek için EGT kullanamazsınız. Bir hatanın bir varlığın her iki tabloda veya her iki tabloda da görünmesine neden olma riskini önlemek için, arşiv işlemi sonunda tutarlı olmalıdır. Aşağıdaki sıralı diyagram, bu işlemdeki adımları özetleyin.  

![Nihai tutarlılık için çözüm diyagramı][12]

İstemci, bir Azure kuyruğuna ileti yerleştirerek (bu örnekte, çalışan #456 arşivlemek için) arşiv işlemini başlatır. Bir işçi rolü yeni iletiler için sırayı yoklar; bir tane bulduğunda, iletiyi okur ve kuyrukta gizli bir kopyasını bırakır. Alt rol sonraki **geçerli** tablodan varlığın bir kopyasını getirir, **Arşiv** tablosuna bir kopyasını ekler ve ardından **Özgün** tablodan siler. Son olarak, önceki adımlardan hata yoksa, alt rol gizli iletiyi kuyruktan siler.  

Bu örnekte, diyagramdaki 4 adım, çalışanı **Arşiv** tablosuna ekler. Çalışan, Blob depolamasındaki bir blob'a veya dosya sistemindeki bir dosyaya ekleyebilir.  

#### <a name="recover-from-failures"></a>Hatalardan kurtarma
Diyagramdaki 4-5 adımdaki işlemlerin, işçi rolünün arşiv işlemini yeniden başlatması gerektiğinde *iktidara* gitmesi önemlidir. Tablo depolama alanını kullanıyorsanız, adım 4 için bir "ekle veya değiştir" işlemi kullanmalısınız; adım 5 için, kullanmakta olduğunuz istemci kitaplığında "varsa sil" işlemini kullanmalısınız. Başka bir depolama sistemi kullanıyorsanız, uygun bir idempotent işlem kullanmanız gerekir.  

Alt rol diyagramdaki 6. İşçi rolü, kuyruktaki bir iletinin kaç kez okunduğunu denetleyebilir ve gerekirse, ayrı bir kuyruğa göndererek onu soruşturma için "zehir" iletisi olarak işaretleyebilir. Sıra iletilerini okuma ve sıra silme sayısını denetleme hakkında daha fazla bilgi için ileti [leri al'a](https://msdn.microsoft.com/library/azure/dd179474.aspx)bakın.  

Tablo depolama ve Sıra depolama bazı hatalar geçici hatalar ve istemci uygulama bunları işlemek için uygun yeniden deneme mantığı içermelidir.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Bu çözüm işlem yalıtımı sağlamaz. Örneğin, bir istemci, alt rol diyagramdaki 4-5 adımları arasındayken **Geçerli** ve **Arşiv** tablolarını okuyabilir ve verilerin tutarsız bir görünümünü görebilir. Veriler eninde sonunda tutarlı olacaktır.  
* Nihai tutarlılık sağlamak için 4-5 adımlarının iktidara olduğundan emin olmalısınız.  
* Birden çok kuyruk ve alt rol örnekleri kullanarak çözümü ölçeklendirebilirsiniz.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Farklı bölümlerde veya tablolarda bulunan varlıklar arasında nihai tutarlılığı garanti etmek istediğinizde bu deseni kullanın. Tablo depolama ve Blob depolama ve veritabanı veya dosya sistemi gibi diğer Azure olmayan Depolama veri kaynakları genelinde işlemler için nihai tutarlılık sağlamak için bu deseni genişletebilirsiniz.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Varlık grubu hareketleri](#entity-group-transactions)  
* [Birleştirme veya değiştirme](#merge-or-replace)  

> [!NOTE]
> İşlem yalıtımı çözümünüz için önemliyse, EGT'leri kullanabilmeniz için tablolarınızı yeniden tasarlamayı düşünün.  
> 
> 

### <a name="index-entities-pattern"></a>Dizin varlıkları deseni
Varlıkların listelerini döndüren verimli aramaları etkinleştirmek için dizin varlıklarını koruyun.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tablo depolama otomatik olarak ve `PartitionKey` `RowKey` değerleri kullanarak varlıkları dizinler. Bu, istemci uygulamasının bir nokta sorgusu kullanarak bir varlığı verimli bir şekilde almasına olanak tanır. Örneğin, aşağıdaki tablo yapısını kullanarak, istemci uygulaması departman adını ve çalışan kimliğini (ve) `PartitionKey` `RowKey`kullanarak tek bir çalışan varlığını verimli bir şekilde alabilir.  

![Çalışan varlığının grafiği][13]

Ayrıca, soyadı gibi benzersiz olmayan başka bir özelliğin değerini temel alan çalışan varlıklarının listesini de almak istiyorsanız, daha az verimli bir bölüm taramayı kullanmanız gerekir. Bu tarayın, doğrudan aramak için bir dizin kullanmak yerine eşleşmeleri bulur. Bunun nedeni Tablo depolama ikincil dizinler sağlamamasıdır.  

#### <a name="solution"></a>Çözüm
Önceki varlık yapısıyla soyadına göre aramayı etkinleştirmek için çalışan adlarının listesini tutmanız gerekir. Jones gibi belirli bir soyadı olan çalışan varlıklarını almak istiyorsanız, önce Jones'un soyadı olan çalışanların çalışan kimliklerini bulmanız ve ardından bu çalışan varlıklarını almanız gerekir. Çalışan iT'lerinin listelerini depolamak için üç ana seçenek vardır:  

* Blob depolama alanını kullanın.  
* Dizin varlıklarını çalışan varlıklarla aynı bölümde oluşturun.  
* Dizin varlıklarını ayrı bir bölüm veya tabloda oluşturun.  

Seçenek 1: Blob depolama alanını kullanma  

Her benzersiz soyadı için bir blob oluşturun ve her blob'da bu soyadı olan çalışanlar için `PartitionKey` (departman) ve `RowKey` (çalışan kimliği) değerlerinin bir listesini saklayın. Bir çalışan eklediğinizde veya sildiğinizde, ilgili blob içeriğinin sonunda çalışan varlıklarıyla tutarlı olduğundan emin olun.  

Seçenek 2: Aynı bölümde dizin varlıkları oluşturma  

Aşağıdaki verileri depolayan dizin varlıklarını kullanın:  

![Aynı soyadına sahip çalışan adlarının listesini içeren dize ile çalışan varlığını gösteren grafik][14]

Özellik, `EmployeeIDs` son adı depolanan çalışanlar için çalışan ların listesini `RowKey`içerir.  

Aşağıdaki adımlar, yeni bir çalışan eklerken izlemeniz gereken işlemi ana hatlar. Bu örnekte, Satış departmanına Kimliği 000152 ve soyadı Jones olan bir çalışan ekliyoruz:  

1. "Satışlar" `PartitionKey` ve "Jones" `RowKey` değeri yle dizin varlığını alın. Adım 2'de kullanmak üzere bu varlığın ETag kaydedin.  
2. Yeni çalışan varlığını`PartitionKey` ("Satışlar" değeri ve `RowKey` değeri "000152" olarak) ekleyen ve dizin varlığını (değer "Satışlar" ve`PartitionKey` `RowKey` "Jones" değeri) güncelleyen bir varlık grubu hareketi (yani toplu işlem) oluşturun. EGT bunu, Çalışan Kimlikleri alanındaki listeye yeni çalışan kimliğini ekleyerek yapar. EGT'ler hakkında daha fazla bilgi için [Entity grup hareketlerine](#entity-group-transactions)bakın.  
3. EGT iyimser bir eşzamanlılık hatası nedeniyle başarısız olursa (diğer bir başkası dizin varlığını değiştirdi), o zaman adım 1'den baştan başlamanız gerekir.  

İkinci seçeneği kullanıyorsanız, bir çalışanı silerken benzer bir yaklaşım kullanabilirsiniz. Çalışan varlığı, eski soyadıiçin dizin varlığı ve yeni soyadıiçin dizin varlığı: üç varlığı güncelleyen bir EGT çalıştırmanız gerektiğinden, bir çalışanın soyadını değiştirmek biraz daha karmaşıktır. Daha sonra iyimser eşzamanlılık kullanarak güncelleştirmeleri gerçekleştirmek için kullanabileceğiniz ETag değerlerini almak için, herhangi bir değişiklik yapmadan önce her varlığı almak gerekir.  

Aşağıdaki adımlar, bir departmanda belirli bir soyadı olan tüm çalışanları aramanız gerektiğinde izlemeniz gereken işlemi ana hatlar. Bu örnekte, Satış departmanında soyadı Jones olan tüm çalışanları arıyoruz:  

1. "Satışlar" `PartitionKey` ve "Jones" `RowKey` değeri yle dizin varlığını alın.  
2. `EmployeeIDs` Alandaki çalışan ların listesini ayrıştırın.  
3. Bu çalışanların her biri hakkında ek bilgilere (e-posta adresleri gibi) ihtiyacınız varsa, "Satışlar" `PartitionKey` `RowKey` değerini ve 2.  

Seçenek 3: Ayrı bir bölüm veya tabloda dizin varlıkları oluşturma  

Bu seçenek için, aşağıdaki verileri depolayan dizin varlıklarını kullanın:  

![Aynı soyadına sahip çalışan adlarının listesini içeren dize ile çalışan varlığını gösteren grafik][15]

Özellik, `EmployeeIDs` son adı depolanan çalışanlar için çalışan ların listesini `RowKey`içerir.  

Dizin varlıkları çalışan varlıklarından ayrı bir bölüm olduğundan, tutarlılığı korumak için EGT'leri kullanamazsınız. Dizin varlıklarının sonunda çalışan varlıklarla tutarlı olduğundan emin olun.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Bu çözüm, eşleşen varlıkları almak için en az iki sorgu gerektirir: bir `RowKey` dizin varlıkları değerleri listesini almak için sorgulamak için, ve sonra listedeki her varlığı almak için sorgular.  
* Tek bir varlığın en fazla 1 MB boyutu olduğundan, çözümdeki seçenek 2 ve seçenek 3, belirli bir soyadıiçin çalışan iAd'ler listesinin hiçbir zaman 1 MB'dan fazla olmadığını varsayalım. Çalışan iAd'leri listesinin boyutu 1 MB'dan fazla ysa, seçenek 1'i kullanın ve dizin verilerini Blob depolama alanında saklayın.  
* Seçenek 2 kullanıyorsanız (çalışan ekleme ve silme işlemlerini işlemek için EGT'leri kullanıyorsanız ve bir çalışanın soyadını değiştirmek), işlem hacminin belirli bir bölümdeki ölçeklenebilirlik sınırlarına yaklaşıp yaklaşacağını değerlendirmeniz gerekir. Bu durumda, sonunda tutarlı bir çözüm düşünmelisiniz (seçenek 1 veya seçenek 3). Bunlar güncelleştirme isteklerini işlemek için kuyrukları kullanır ve dizin varlıklarınızı çalışan varlıklarından ayrı bir bölümde depolamanızı sağlar.  
* Bu çözümdeki Seçenek 2, bir departman içinde soyadına göre bakmak istediğinizi varsayar. Örneğin, Satış departmanında soyadı Jones olan çalışanların listesini almak istiyorsunuz. Tüm kuruluş genelinde Jones soyadıolan tüm çalışanları aramak istiyorsanız, seçenek 1'i veya seçenek 3'u kullanın.
* Nihai tutarlılık sağlayan sıra tabanlı bir çözüm uygulayabilirsiniz. Daha fazla ayrıntı için, [Eventually tutarlı hareketler deseni](#eventually-consistent-transactions-pattern)bakın.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Jones soyadına sahip tüm çalışanlar gibi ortak bir özellik değerini paylaşan bir varlık kümesini aramak istediğinizde bu deseni kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bileşik anahtar deseni](#compound-key-pattern)  
* [Sonunda tutarlı hareketler deseni](#eventually-consistent-transactions-pattern)  
* [Varlık grubu hareketleri](#entity-group-transactions)  
* [Heterojen varlık türleri ile çalışma](#work-with-heterogeneous-entity-types)  

### <a name="denormalization-pattern"></a>Denormalizasyon deseni
Tek bir nokta sorgusuyla ihtiyacınız olan tüm verileri alabilmeniz için ilgili verileri tek bir varlıkta birleştirin.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
İlişkisel bir veritabanında, sorgular birden çok tablodan veri aldığında oluşan yinelemeyi kaldırmak için genellikle verileri normalleştirirsiniz. Verilerinizi Azure tablolarında normalleştirirseniz, ilgili verilerinizi almak için istemciden sunucuya birden çok gidiş dönüş seyahati yapmalısınız. Örneğin, aşağıdaki tablo yapısıyla, bir bölümün ayrıntılarını almak için iki gidiş-dönüş seyahate ihtiyacınız vardır. Bir gezi, yöneticinin kimliğini içeren departman kuruluşunu getirir ve ikinci gezi yöneticinin ayrıntılarını çalışan bir varlıkta getirir.  

![Departman tüzel kişiliğinin ve çalışan tüzel kişiliğinin grafiği][16]

#### <a name="solution"></a>Çözüm
Verileri iki ayrı varlıkta depolamak yerine, verileri normalden arındırın ve yöneticinin ayrıntılarının bir kopyasını departman varlığında tutun. Örnek:  

![Normalize edilmiş ve birleştirilmiş departman varlığının grafiği][17]

Bu özelliklerle depolanan departman varlıklarıyla, artık bir nokta sorgusu kullanarak bir departman hakkında ihtiyacınız olan tüm ayrıntıları alabilirsiniz.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Bazı verilerin iki kez depolanmasıyla ilişkili bazı maliyet yükü vardır. Tablo depolamasına daha az istekten kaynaklanan performans avantajı genellikle depolama maliyetlerindeki marjinal artıştan daha ağır basar. Ayrıca, bu maliyet kısmen bir bölümün ayrıntılarını getirmek için gereken işlem sayısındaki bir azalma ile dengelenir.  
* Yöneticiler hakkında bilgi depolayan iki varlığın tutarlılığını korumanız gerekir. Tek bir atomik işlemde birden çok varlığı güncelleştirmek için EGT'leri kullanarak tutarlılık sorununu işleyebilirsiniz. Bu durumda, departman tüzel kişiliği ve departman yöneticisinin çalışan varlığı aynı bölümde depolanır.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
İlgili bilgileri sık sık aramanız gerektiğinde bu deseni kullanın. Bu desen, istemcinizin gerektirdiği verileri almak için yapması gereken sorgu sayısını azaltır.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bileşik anahtar deseni](#compound-key-pattern)  
* [Varlık grubu hareketleri](#entity-group-transactions)  
* [Heterojen varlık türleri ile çalışma](#work-with-heterogeneous-entity-types)

### <a name="compound-key-pattern"></a>Bileşik anahtar deseni
İstemcinin ilgili verileri tek bir nokta sorgusuyla aramasını sağlamak için bileşik `RowKey` değerleri kullanın.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
İlişkisel bir veritabanında, ilgili veri parçalarını tek bir sorguda istemciye döndürmek için sorgularda birleştirmeler kullanmak doğaldır. Örneğin, çalışan kimliğini, söz sahibi çalışanın performans ve inceleme verilerini içeren ilgili varlıkların listesini aramak için kullanabilirsiniz.  

Aşağıdaki yapıyı kullanarak çalışan varlıklarını Tablo depolama alanında depoladığınızı varsayalım:  

![Çalışan varlığının grafiği][18]

Ayrıca, çalışanın kuruluşunuz için çalıştığı her yıl için incelemeler ve performansla ilgili geçmiş verileri depolamanız ve bu bilgilere her yıl erişebilmeniz gerekir. Seçeneklerden biri, varlıkları aşağıdaki yapıya sahip depolayan başka bir tablo oluşturmaktır:  

![Çalışan inceleme varlığının grafiği][19]

Bu yaklaşımla, verilerinizi tek bir istekle almanızı sağlamak için yeni varlıktaki bazı bilgileri (ad ve soyadı gibi) çoğaltmaya karar verebilirsiniz. Ancak, iki varlığı atomik olarak güncelleştirmek için Bir EGT kullanamadığınız için güçlü tutarlılık koruyamazsınız.  

#### <a name="solution"></a>Çözüm
Aşağıdaki yapıya sahip varlıkları kullanarak özgün tablonuzda yeni bir varlık türünü depolayın:  

![Bileşik anahtarlı çalışan varlığının grafiği][20]

Çalışan kimliği `RowKey` ve gözden geçirme verilerinin yılı oluşturan bileşik anahtarın nasıl artık nasıl olduğuna dikkat edin. Bu, çalışanın performansını almanızı ve tek bir varlık için tek bir istekle verileri gözden geçirmenizi sağlar.  

Aşağıdaki örnekte, belirli bir çalışanın (Satış departmanındaki çalışan 000123 gibi) tüm gözden geçirme verilerini nasıl alabileceğiniz özetlenebilir:  

$filter=(PartitionKey eq 'Satış') ve (RowKey ge 'empid_000123') ve (RowKey lt 'empid_000124')&$select=RowKey,Yönetici Derecelendirmesi,Eş Değerlendirme,Yorumlar  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* `RowKey` Değeri ayrıştırmayı kolaylaştıran uygun bir ayırıcı karakter kullanmalısınız: örneğin, **000123_2012**.  
* Ayrıca, bu varlığı aynı çalışan için ilgili verileri içeren diğer varlıklarla aynı bölümde de depolayabilirsiniz. Bu, güçlü tutarlılığı korumak için EGT'leri kullanabileceğiniz anlamına gelir.
* Bu desenin uygun olup olmadığını belirlemek için verileri ne sıklıkta sorgulayacağınızı düşünmelisiniz. Örneğin, gözden geçirme verilerine seyrek olarak ve ana çalışan verilerine sık sık erişirseniz, bunları ayrı varlıklar olarak tutmanız gerekir.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Sık sorguladığınız bir veya daha fazla ilgili varlığı depolamanız gerektiğinde bu deseni kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Varlık grubu hareketleri](#entity-group-transactions)  
* [Heterojen varlık türleri ile çalışma](#work-with-heterogeneous-entity-types)  
* [Sonunda tutarlı hareketler deseni](#eventually-consistent-transactions-pattern)  

### <a name="log-tail-pattern"></a>Kütük kuyruk deseni
En son bir bölüme eklenen *n* varlıklarını, ters tarih ve saat sırasına göre sıralayan bir `RowKey` değer kullanarak alın.  

> [!NOTE]
> Azure Cosmos DB'deki Azure Tablo API'si tarafından döndürülen sorgu sonuçları bölüm anahtarına veya satır tuşuna göre sıralanmaz. Bu nedenle, bu desen Tablo depolama için uygun olsa da, Azure Cosmos DB için uygun değildir. Özellik farklılıklarının ayrıntılı bir listesi için Azure [Cosmos DB'deki Tablo API'si ile Azure Tablo Depolama'daki farklara](faq.md#where-is-table-api-not-identical-with-azure-table-storage-behavior)bakın.

#### <a name="context-and-problem"></a>Bağlam ve sorun
Yaygın bir gereksinim, en son oluşturulan varlıkları (örneğin, bir çalışan tarafından gönderilen on en son gider talebi) alabilmektir. Tablo sorguları, `$top` ilk *n* varlıklarını bir kümeden döndürmek için bir sorgu işlemini destekler. Bir kümedeki son *n* varlıklarını döndürmek için eşdeğer bir sorgu işlemi yoktur.  

#### <a name="solution"></a>Çözüm
Varlıkları, doğal olarak `RowKey` ters tarih/saat sırasına göre sıralayan bir madde kullanarak depolayın, böylece en son giriş her zaman tablodaki ilk giriştir.  

Örneğin, bir çalışan tarafından gönderilen on en son gider talebinin alınabilmesi için geçerli tarihten/saatten türetilen bir ters onay değeri kullanabilirsiniz. Aşağıdaki C# kodu örneği, en sondan en eskiye doğru sıralayan bir değer için uygun bir `RowKey` "ters kene" değeri oluşturmanın bir yolunu gösterir:  

`string invertedTicks = string.Format("{0:D19}", DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks);`  

Aşağıdaki kodu kullanarak tarih/saat değerine geri dönebilirsiniz:  

`DateTime dt = new DateTime(DateTime.MaxValue.Ticks - Int64.Parse(invertedTicks));`  

Tablo sorgusu aşağıdaki gibi görünür:  

`https://myaccount.table.core.windows.net/EmployeeExpense(PartitionKey='empid')?$top=10`  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Dize değerinin beklendiği gibi sıraladığından emin olmak için, önde gelen sıfırlarla ters kene değerini deftere bağlamanız gerekir.  
* Bir bölüm düzeyinde ölçeklenebilirlik hedefleri farkında olmalıdır. Sıcak nokta bölümleri oluşturmamaya dikkat edin.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Bu deseni, ters tarih/saat sırasına göre varlıklara erişmeniz gerektiğinde veya en son eklenen varlıklara erişmeniz gerektiğinde kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Prepend / ek anti-desen](#prepend-append-anti-pattern)  
* [Varlıkları alma](#retrieve-entities)  

### <a name="high-volume-delete-pattern"></a>Yüksek hacimli silme deseni
Tüm varlıkları kendi ayrı tablolarında eşzamanlı silme için depolayarak yüksek hacimli varlıkların silinmesini etkinleştirin. Tabloyu silerek varlıkları silersiniz.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Birçok uygulama, istemci uygulaması için artık kullanılabilir olması gerekmeyen veya uygulamanın başka bir depolama ortamına arşivlenmiş eski verileri siler. Bu tür verileri genellikle bir tarihe göre tanımlarsınız. Örneğin, 60 günden eski olan tüm oturum açma isteklerinin kayıtlarını silme niz gerekir.  

Olası bir tasarım oturum açma isteğinin tarih ve saatini `RowKey`şu şekilde kullanmaktır:  

![Giriş denemesi varlığının grafiği][21]

Uygulama ayrı bir bölümdeki her kullanıcı için oturum açma varlıkları ekleyip silebileceğinden, bu yaklaşım bölüm etkin noktalarını önler. Ancak, çok sayıda varlığınız varsa, bu yaklaşım maliyetli ve zaman alıcı olabilir. İlk olarak, silmek için tüm varlıkları tanımlamak için bir tablo tayini gerçekleştirmek ve daha sonra her eski varlık silmeniz gerekir. EGT'lere birden çok silme isteği ekleyerek eski varlıkları silmek için gereken sunucuya yapılan gidiş dönüş sayısını azaltabilirsiniz.  

#### <a name="solution"></a>Çözüm
Oturum açma denemelerinin her günü için ayrı bir tablo kullanın. Varlıkları eklerken etkin noktalardan kaçınmak için önceki varlık tasarımını kullanabilirsiniz. Eski varlıkları silen, her gün yüzlerce ve binlerce oturum açma varlığı bulmak ve salmak yerine, her gün bir tabloyu (tek bir depolama işlemi) silme meselesidir.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tasarımınız, uygulamanızın belirli varlıkları arama, diğer verilerle bağlantı kurma veya toplu bilgi oluşturma gibi verileri kullanma nın diğer yollarını destekliyor mu?  
* Yeni varlıklar eklerken tasarımınız etkin noktalardan kaçınıyor mu?  
* Silen sonra aynı tablo adını yeniden kullanmak istiyorsanız bir gecikme bekleyin. Her zaman benzersiz tablo adları kullanmak daha iyidir.  
* Tablo depolama erişim desenlerini öğrenir ve düğümler arasında bölümleri dağıtır iken, yeni bir tablo yu ilk kullandığınızda bazı oran sınırlamaları bekleyin. Yeni tablolar oluşturmanız için ne sıklıkta yapmanız gerektiğini göz önünde bulundurmalısınız.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Aynı anda silmeniz gereken yüksek hacimli varlıklar varsa bu deseni kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Varlık grubu hareketleri](#entity-group-transactions)
* [Varlıkları değiştirme](#modify-entities)  

### <a name="data-series-pattern"></a>Veri serisi deseni
Yaptığınız istek sayısını en aza indirmek için tüm veri serilerini tek bir varlıkta depolayın.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Yaygın bir senaryo, bir uygulamanın genellikle aynı anda alınması gereken bir dizi veriyi depolamasıdır. Örneğin, uygulamanız her çalışanın her saat kaç IM iletisi gönderdiğini kaydedebilir ve daha sonra bu bilgileri, her kullanıcının önceki 24 saat içinde kaç ileti gönderdiğini çizmek için kullanabilir. Bir tasarım her çalışan için 24 varlıkları depolamak için olabilir:  

![İleti istatistikleri varlığının grafiği][22]

Bu tasarımla, uygulamanın ileti sayısı değerini güncelleştirmesi gerektiğinde varlığı her çalışan için güncelleştirmek üzere kolayca bulabilir ve güncelleştirebilirsiniz. Ancak, önceki 24 saat için etkinliğin bir grafik çizmek için bilgi almak için, 24 varlıklar almanız gerekir.  

#### <a name="solution"></a>Çözüm
İleti sayısını her saat için depolamak için ayrı bir özellik içeren aşağıdaki tasarımı kullanın:  

![Ayrılmış özelliklere sahip ileti istatistikleri varlığını gösteren grafik][23]

Bu tasarımla, bir çalışanın ileti sayısını belirli bir saat için güncelleştirmek için birleştirme işlemini kullanabilirsiniz. Şimdi, tek bir varlık için bir istek kullanarak grafiği çizmek için gereken tüm bilgileri alabilirsiniz.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tam veri seriniz tek bir varlığa uymuyorsa (bir varlığın 252'ye kadar özelliği olabilir), blob gibi alternatif bir veri deposu kullanın.  
* Bir varlığı aynı anda güncelleyen birden çok istemciniz varsa, iyimser eşzamanlılık uygulamak için **ETag'ı** kullanın. Çok sayıda müşteriniz varsa, yüksek çekişme ler yaşayabilirsiniz.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Tek bir varlıkla ilişkili bir veri serisini güncelleştirmeniz ve almanız gerektiğinde bu deseni kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Büyük varlıklar deseni](#large-entities-pattern)  
* [Birleştirme veya değiştirme](#merge-or-replace)  
* [Sonunda tutarlı işlemler deseni](#eventually-consistent-transactions-pattern) (veri serisini bir blob'da depolıyorsanız)  

### <a name="wide-entities-pattern"></a>Geniş varlıklar deseni
252'den fazla özelliyi olan mantıksal varlıkları depolamak için birden çok fiziksel varlık kullanın.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tek bir varlık en fazla 252 özelliye (zorunlu sistem özellikleri hariç) sahip olabilir ve toplamda 1 MB'dan fazla veri depolayamaz. İlişkisel bir veritabanında, genellikle yeni bir tablo ekleyerek ve aralarında bire bir ilişki uygulayarak satırBoyutundaki herhangi bir sınır üzerinde çalışırsınız.  

#### <a name="solution"></a>Çözüm
Tablo depolama alanını kullanarak, 252'den fazla özelliği olan tek bir büyük iş nesnesini temsil edecek şekilde birden çok varlığı depolayabilirsiniz. Örneğin, son 365 gün içinde her çalışan tarafından gönderilen IM iletilerinin sayısını depolamak istiyorsanız, farklı şemalara sahip iki varlığı kullanan aşağıdaki tasarımı kullanabilirsiniz:  

![Rowkey 01 ve Satır Anahtarı 02 ile ileti istatistikleri varlık gösteren grafik][24]

Birbirleri ile eşitleme lerini sağlamak için her iki varlığı da güncelleştirmeyi gerektiren bir değişiklik yapmanız gerekiyorsa, EGT kullanabilirsiniz. Aksi takdirde, ileti sayısını belirli bir gün için güncelleştirmek için tek bir birleştirme işlemi kullanabilirsiniz. Tek bir çalışanın tüm verilerini almak için her iki varlığı da almanız gerekir. Bunu hem a hem de bir `PartitionKey` `RowKey` değeri kullanan iki verimli istekle yapabilirsiniz.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu deseni nasıl uygulayacağınız konusunda karar verirken aşağıdaki noktayı göz önünde bulundurun:  

* Tam bir mantıksal varlık alma en az iki depolama hareketleri içerir: her fiziksel varlık almak için bir.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Tablo depolama alanında tek bir varlığın sınırlarının üzerinde olan varlıkları depolamanız gerektiğinde bu deseni kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Varlık grubu hareketleri](#entity-group-transactions)
* [Birleştirme veya değiştirme](#merge-or-replace)

### <a name="large-entities-pattern"></a>Büyük varlıklar deseni
Büyük özellik değerlerini depolamak için Blob depolama alanını kullanın.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tek bir varlık toplamda 1 MB'dan fazla veri depolayamaz. Özelliklerinizden biri veya birkaçı, varlığınızın toplam boyutunun bu değeri aşması için değer depolarsa, tüm varlığı Tablo depolama alanında depolayamazsınız.  

#### <a name="solution"></a>Çözüm
Bir veya daha fazla özellik büyük miktarda veri içerdiğinden, varlığınız boyutu 1 MB'ı aşarsa, verileri Blob depolama alanında depolayabilir ve ardından blob adresini varlıktaki bir özellikte depolayabilirsiniz. Örneğin, bir çalışanın fotoğrafını Blob depolama alanında saklayabilir ve fotoğrafın bağlantısını `Photo` çalışan varlığınızın mülkünde saklayabilirsiniz:  

![Blob depolamasını gösteren Fotoğraf dizesi ile çalışan varlığını gösteren grafik][25]

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tablo depolamadaki varlık la Blob depolamadaki veriler arasında nihai tutarlılığı korumak için, varlıklarınızı korumak için [Eventually tutarlı hareketler modelini](#eventually-consistent-transactions-pattern) kullanın.
* Tam bir varlığın alınması en az iki depolama işlemi içerir: biri varlığı almak için, diğeri blob verilerini almak için.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Boyutu Tablo depolamasında tek bir varlığın sınırlarını aşan varlıkları depolamanız gerektiğinde bu deseni kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Sonunda tutarlı hareketler deseni](#eventually-consistent-transactions-pattern)  
* [Geniş varlıklar deseni](#wide-entities-pattern)

<a name="prepend-append-anti-pattern"></a>

### <a name="prependappend-anti-pattern"></a>Prepend/ek anti-desen
Yüksek hacimli kesici uçlara sahipseniz, kesici uçları birden çok bölüme yayarak ölçeklenebilirliği artırın.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Depolanan varlıklarınıza ön bekleyen veya eklenen varlıklar genellikle uygulamanın bir dizi bölümün ilk veya son bölümüne yeni varlıklar eklemesine neden olur. Bu durumda, belirli bir zamanda tüm ekler aynı bölüm içinde yer alıyor, bir hotspot oluşturma. Bu, Tablo depolamanın birden çok düğüm üzerinde yük dengeleme uçlarını önler ve büyük olasılıkla uygulamanızın bölüm için ölçeklenebilirlik hedeflerini vurmasına neden olur. Örneğin, çalışanların ağ ve kaynak erişimini günlüğe kaydeden bir uygulama nın durumunu göz önünde bulundurun. Aşağıdaki gibi bir varlık yapısı, hareketlerin hacmi tek bir bölüm için ölçeklenebilirlik hedefine ulaşırsa, geçerli saatin bölünmesinin etkin nokta haline gelmesine neden olabilir:  

![Çalışan varlığının grafiği][26]

#### <a name="solution"></a>Çözüm
Aşağıdaki alternatif varlık yapısı, uygulama olayları günlüğe kaydeder gibi, belirli bir bölüm üzerinde bir hotspot önler:  

![RowKey'in Yıl, Ay, Gün, Saat ve Olay Kimliğini biraraya getiren çalışan varlığını gösteren grafik][27]

Bu örnekte hem `PartitionKey` hem `RowKey` de bileşik anahtarların nasıl olduğuna dikkat edin. Günlüğe `PartitionKey` kaydetmeyi birden çok bölüme dağıtmak için hem departman hem de çalışan kimliği kullanır.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Eklerde sıcak bölümler oluşturmayı önleyen alternatif anahtar yapısı istemci uygulamanızın yaptığı sorguları verimli bir şekilde destekliyor mu?  
* Beklenen işlem hacminiz, tek bir bölüm için ölçeklenebilirlik hedeflerine ulaşma ve Tablo depolama tarafından daraltılmış olma olasılığınız anlamına mı geliyor?  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Sıcak bir bölüme erişdiğinizde, işlem hacminiz Tablo depolama tarafından hız sınırlamasına neden olacaksa, hazırlık/ek anti-desenkaçının.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bileşik anahtar deseni](#compound-key-pattern)  
* [Kütük kuyruk deseni](#log-tail-pattern)  
* [Varlıkları değiştirme](#modify-entities)  

### <a name="log-data-anti-pattern"></a>Günlük veri anti-desen
Genellikle, günlük verilerini depolamak için Tablo depolama alanı yerine Blob depolama sını kullanmanız gerekir.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Günlük verileri için yaygın kullanım örneği, belirli bir tarih/saat aralığı için günlük girişlerinin seçimini almaktır. Örneğin, uygulamanızın belirli bir tarihte 15:04 ile 15:06 arasında günlüğe kaydettiği tüm hatayı ve kritik iletileri bulmak istiyorsunuz. Günlük varlıklarını kaydettiğiniz bölümü belirlemek için günlük iletisinin tarih ve saatini kullanmak istemezsiniz. Bu da sıcak bir bölümle sonuçlanır, çünkü belirli bir zamanda `PartitionKey` tüm günlük varlıkları aynı değeri paylaşır [(Prepend/ek anti-desenbölümüne](#prepend-append-anti-pattern)bakın). Örneğin, günlük iletisi için aşağıdaki varlık şeması sıcak bir bölümle sonuçlanır, çünkü uygulama tüm günlük iletilerini geçerli tarih ve saat için bölüme yazar:  

![Günlük ileti sahihinin grafiği][28]

Bu örnekte, `RowKey` günlük iletilerinin tarih/saat sırasına göre sıralandığından emin olmak için günlük iletisinin tarih ve saatini içerir. Birden `RowKey` çok günlük iletisinin aynı tarih ve saati paylaşması durumunda, bir ileti kimliği de içerir.  

Başka bir `PartitionKey` yaklaşım, uygulamanın çeşitli bölümler arasında ileti ler yazmasını sağlayan bir uygulama kullanmaktır. Örneğin, günlük iletisinin kaynağı birçok bölüm arasında ileti dağıtmak için bir yol sağlıyorsa, aşağıdaki varlık şemasını kullanabilirsiniz:  

![Günlük ileti sahihinin grafiği][29]

Ancak, bu şema ile ilgili sorun, belirli bir zaman aralığı için tüm günlük iletileri almak için, tablodaki her bölümü aramak gerekir.

#### <a name="solution"></a>Çözüm
Önceki bölümde, günlük girişlerini depolamak için Tablo depolama sını kullanmaya çalışma sorunu vurgulanmış ve iki tatmin edici olmayan tasarım önerilmiştir. Bir çözüm kötü performans günlük mesajları yazma riski ile sıcak bir bölümyol açtı. Diğer çözüm, belirli bir zaman aralığı için günlük iletileri almak için tablodaki her bölümü tarama gereksinimi nedeniyle, kötü sorgu performansı ile sonuçlandı. Blob depolama, bu tür senaryolar için daha iyi bir çözüm sunar ve Azure Depolama analitiği topladığı günlük verilerini bu şekilde depolar.  

Bu bölümde, Depolama analitiği, genellikle aralıklara göre sorguladığınız verileri depolamaya ilişkin bu yaklaşımın bir örneği olarak, verileri Blob depolama alanında nasıl depoladığını özetlemektedir.  

Depolama analitiği günlük iletilerini birden çok blob'da sınırlı bir biçimde saklar. Sınırlı biçim, istemci uygulamasının günlük iletisindeki verileri ayrışdırmasını kolaylaştırır.  

Depolama analitiği, arama yaptığınız günlük iletilerini içeren blob 'u (veya blobs)'ı bulmanızı sağlayan blob'lar için bir adlandırma kuralı kullanır. Örneğin, "queue/2014/07/31/1800/000001.log" adlı bir blob, 31 Temmuz 2014 tarihinde saat 18:00'den itibaren sıra hizmetiyle ilgili günlük iletileri içerir. "000001" bu dönemin ilk günlük dosyası olduğunu gösterir. Depolama analitiği ayrıca, blob'un meta verilerinin bir parçası olarak dosyada depolanan ilk ve son günlük iletilerinin zaman damgalarını da kaydeder. Blob depolama için API, bir ad önekine dayalı bir kapsayıcıda lekeler bulmanızı sağlar. Saat 18:00'den itibaren saat için sıra günlüğü verilerini içeren tüm lekeleri bulmak için "queue/2014/07/31/1800" önekini kullanabilirsiniz.  

Depolama analitiği arabellekleri iletileri dahili olarak günlüğe kaydeder ve ardından uygun blob'u düzenli aralıklarla güncelleştirir veya en son günlük girişleri yle yeni bir tane oluşturur. Bu, Blob depolamasına gerçekleştirmesi gereken yazma sayısını azaltır.  

Kendi uygulamanızda benzer bir çözüm uyguluyorsanız, güvenilirlik, maliyet ve ölçeklenebilirlik arasındaki dengeyi nasıl yönetacağınızı düşünün. Başka bir deyişle, uygulamanızdaki güncelleştirmeleri arabelleğe alma ve toplu olarak Blob depolama ya yazma ile karşılaştırıldığında, blob depolama ya her günlük girişi yazma etkisini değerlendirir.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Günlük verilerinin nasıl depolanmasına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Olası sıcak bölümleri önleyen bir tablo tasarımı oluşturursanız, günlük verilerinize verimli bir şekilde erişemediğinizi görebilirsiniz.  
* Günlük verilerini işlemek için istemcinin genellikle çok sayıda kayıt yüklemesi gerekir.  
* Günlük verileri genellikle yapılandırılmış olsa da, Blob depolama daha iyi bir çözüm olabilir.  

### <a name="implementation-considerations"></a>Uygulama konuları
Bu bölümde, önceki bölümlerde açıklanan desenleri uygularken göz önünde bulundurulması gereken bazı hususlar açıklanmaktadır. Bu bölümün çoğu, Depolama İstemci Kitaplığı'nı kullanan C# ile yazılmış örnekleri kullanır (yazım sırasında sürüm 4.3.0).  

### <a name="retrieve-entities"></a>Varlıkları alma
[Sorgulama için Tasarım](#design-for-querying)bölümünde anlatıldığı gibi, en verimli sorgu bir nokta sorgusudur. Ancak, bazı senaryolarda birden çok varlık almanız gerekebilir. Bu bölümde, Depolama İstemci Kitaplığı'nı kullanarak varlıkları almak için bazı yaygın yaklaşımlar açıklanmaktadır.  

#### <a name="run-a-point-query-by-using-the-storage-client-library"></a>Depolama İstemci Kitaplığı'nı kullanarak bir nokta sorgusu çalıştırma
Bir nokta sorgusunu çalıştırmanın en kolay yolu **TabloYu Al** işlemini kullanmaktır. Aşağıdaki C# kod snippet'inde gösterildiği gibi, bu `PartitionKey` işlem "Satış" değeri `RowKey` olan bir varlık ve "212" değeri olan bir varlığı alır:  

```csharp
TableOperation retrieveOperation = TableOperation.Retrieve<EmployeeEntity>("Sales", "212");
var retrieveResult = employeeTable.Execute(retrieveOperation);
if (retrieveResult.Result != null)
{
    EmployeeEntity employee = (EmployeeEntity)retrieveResult.Result;
    ...
}  
```

Bu örnek, almakta olduğu varlığın türünde `EmployeeEntity`olmasını nasıl beklediğine dikkat edin.  

#### <a name="retrieve-multiple-entities-by-using-linq"></a>LINQ kullanarak birden fazla varlığı alma
Depolama İstemci Kitaplığı ile LINQ kullanarak ve **bir where** clause ile bir sorgu belirterek birden çok varlık ları alabilirsiniz. Tablo taramasından kaçınmak için, her `PartitionKey` zaman nerede yan tümcesi `RowKey` değeri ve mümkünse değer tablo ve bölüm taramaları önlemek için içermelidir. Tablo depolama, where yan tümcesinde kullanılacak sınırlı bir karşılaştırma işleci kümesini (büyük, büyük veya eşit, daha az, daha az veya eşit, eşit ve eşit olmayan) destekler. Aşağıdaki C# kodu snippet, soyadı "B" ile başlayan (soyadını depoladığını `RowKey` varsayarsak) Satış departmanında (mağaza adını `PartitionKey` varsayarsak) tüm çalışanları bulur:  

```csharp
TableQuery<EmployeeEntity> employeeQuery = employeeTable.CreateQuery<EmployeeEntity>();
var query = (from employee in employeeQuery
            where employee.PartitionKey == "Sales" &&
            employee.RowKey.CompareTo("B") >= 0 &&
            employee.RowKey.CompareTo("C") < 0
            select employee).AsTableQuery();
var employees = query.Execute();  
```

Daha iyi performans sağlamak için `RowKey` sorgunun hem a hem de a'yı `PartitionKey` nasıl belirlediğine dikkat edin.  

Aşağıdaki kod örneği akıcı API'yi kullanarak eşdeğer işlevselliği gösterir (genel olarak akıcı API'ler hakkında daha fazla bilgi [için, akıcı BIR API tasarlamak için en iyi uygulamalara](https://visualstudiomagazine.com/articles/2013/12/01/best-practices-for-designing-a-fluent-api.aspx)bakın):  

```csharp
TableQuery<EmployeeEntity> employeeQuery = new TableQuery<EmployeeEntity>().Where(
    TableQuery.CombineFilters(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition(
    "PartitionKey", QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.GenerateFilterCondition(
    "RowKey", QueryComparisons.GreaterThanOrEqual, "B")
),
TableOperators.And,
TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "C")
    )
);
var employees = employeeTable.ExecuteQuery(employeeQuery);  
```

> [!NOTE]
> Örnek, üç `CombineFilters` filtre koşullarını içerecek şekilde birden çok yöntem eyer.  
> 
> 

#### <a name="retrieve-large-numbers-of-entities-from-a-query"></a>Sorgudan çok sayıda varlık alma
En iyi sorgu, bir değer `PartitionKey` ve `RowKey` bir değere dayalı olarak tek bir varlığı döndürür. Ancak, bazı senaryolarda aynı bölümden, hatta birçok bölümden birçok varlığı döndürme gereksiniminiz olabilir. Bu tür senaryolarda uygulamanızın performansını her zaman tam olarak test etmelidir.  

Tablo depolamasına karşı bir sorgu aynı anda en fazla 1.000 varlık döndürebilir ve en fazla beş saniye çalıştırılabilir. Tablo depolama, aşağıdakilerden herhangi biri doğruysa, istemci uygulamasının bir sonraki varlık kümesini istemesini sağlamak için bir devamı belirteci döndürür:

- Sonuç kümesi 1.000'den fazla varlık içerir.
- Sorgu beş saniye içinde tamamlanmadı.
- Sorgu bölüm sınırını geçer. 

Devam belirteçlerinin nasıl çalıştığı hakkında daha fazla bilgi için sorgu [zaman aşım ve pagination'a](https://msdn.microsoft.com/library/azure/dd135718.aspx)bakın.  

Depolama İstemci Kitaplığı kullanıyorsanız, Tablo depolama varlıklarından varlıkları döndürür gibi sizin için devam belirteçleri otomatik olarak işleyebilir. Örneğin, Aşağıdaki C# kodu örneği, Tablo depolama alanı bunları yanıt olarak döndürürse, devam belirteçlerini otomatik olarak işler:  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

var employees = employeeTable.ExecuteQuery(employeeQuery);
foreach (var emp in employees)
{
        ...
}  
```

Aşağıdaki C# kodu, devam belirteçlerini açıkça işler:  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

TableContinuationToken continuationToken = null;

do
{
        var employees = employeeTable.ExecuteQuerySegmented(
        employeeQuery, continuationToken);
    foreach (var emp in employees)
    {
    ...
    }
    continuationToken = employees.ContinuationToken;
} while (continuationToken != null);  
```

Devam belirteçlerini açıkça kullanarak, uygulamanızın bir sonraki veri kesimini ne zaman aldığını denetleyebilirsiniz. Örneğin, istemci uygulamanız kullanıcıların bir tabloda depolanan varlıklar aracılığıyla sayfa uygulamasına olanak sağlıyorsa, kullanıcı sorgu tarafından alınan tüm varlıklar arasında sayfa uygulamamaya karar verebilir. Uygulamanız yalnızca kullanıcı geçerli segmentteki tüm varlıklar arasında sayfalama bittiğinde bir sonraki kesimi almak için bir devamı belirteci kullanır. Bu yaklaşımın birkaç faydası vardır:  

* Tablo depolamadan alınacak ve ağ üzerinde hareket ettirecek veri miktarını sınırlayabilirsiniz.  
* .NET'te eşzamanlı G/Ç gerçekleştirebilirsiniz.  
* Kalıcı depolama için devamı belirteci serileştirebilirsiniz, böylece bir uygulama kilitlenme durumunda devam edebilirsiniz.  

> [!NOTE]
> Bir devam belirteci genellikle 1.000 varlık içeren bir kesim döndürür, ancak daha az içerebilir. Bu durum, bir sorgunun geri dönen giriş sayısını, arama ölçütlerinizle eşleşen ilk n varlıklarını döndürmek için **Al'ı** kullanarak sınırlandırıyorsanız da durumdur. Tablo depolama, kalan varlıkları almanızı sağlamak için bir devamı belirteciyle birlikte n'den az varlık içeren bir segment döndürebilir.  
> 
> 

Aşağıdaki C# kodu, bir segment içinde döndürülen varlıkların sayısını nasıl değiştirebilirsiniz gösterir:  

```csharp
employeeQuery.TakeCount = 50;  
```

#### <a name="server-side-projection"></a>Sunucu tarafı projeksiyonu
Tek bir varlık en fazla 255 özellüğe sahip olabilir ve 1 MB boyutuna kadar olabilir. Tabloyu sorgulayıp varlıkları geri aldığınızda, tüm özelliklere ihtiyaç duymayabilirsiniz ve verileri gereksiz yere aktarmaktan kaçınabilirsiniz (gecikme süresini ve maliyeti azaltmaya yardımcı olmak için). İhtiyacınız olan özellikleri aktarmak için sunucu tarafı projeksiyonu kullanabilirsiniz. Aşağıdaki örnek, sorgu `Email` tarafından seçilen `PartitionKey`varlıklardan `Timestamp`yalnızca `ETag`özelliği (, , `RowKey`, ve ) alır.  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
List<string> columns = new List<string>() { "Email" };
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter).Select(columns);

var entities = employeeTable.ExecuteQuery(employeeQuery);
foreach (var e in entities)
{
        Console.WriteLine("RowKey: {0}, EmployeeEmail: {1}", e.RowKey, e.Email);
}  
```

Alınacak özellikler `RowKey` listesine dahil olmasa bile değerin nasıl kullanılabilir olduğuna dikkat edin.  

### <a name="modify-entities"></a>Varlıkları değiştirme
Depolama İstemci Kitaplığı, tablo depolama alanında depolanan varlıklarınızı, varlıkları ekleyerek, silerek ve güncelleştirerek değiştirmenize olanak tanır. EGT'leri, gerekli gidiş-dönüş sayısını azaltmak ve çözümünüzünüzün performansını artırmak için birden fazla eklemeyi toplu olarak kullanmak, işlemleri güncellemek ve silmek için kullanabilirsiniz.  

Depolama İstemci Kitaplığı bir EGT çalıştırdığında atılan özel durumlar genellikle toplu iş başarısız lığa neden olan varlığın dizin içerir. Bu, EGT'leri kullanan kod hata ayıklama yaparken yararlıdır.  

Ayrıca, tasarımınızın istemci uygulamanızın eşzamanlılık ve güncelleştirme işlemlerini nasıl ele adadığını da göz önünde bulundurmalısınız.  

#### <a name="managing-concurrency"></a>Eşzamanlılığı yönetme
Varsayılan olarak, Tablo depolama ekleme, birleştirme ve silme işlemleri için tek tek varlıklar düzeyinde iyimser eşzamanlılık denetimleri uygular, ancak bir istemcinin Tablo depolamasını bu denetimleri atlayarak zorlayabilir. Daha fazla bilgi için microsoft [Azure Depolama'da eşzamanlılık yönetme'ye](../storage/common/storage-concurrency.md)bakın.  

#### <a name="merge-or-replace"></a>Birleştirme veya değiştirme
`TableOperation` Sınıfın `Replace` yöntemi her zaman Tablo depolama tam varlık değiştirir. Bu özellik depolanan varlıkta bulunduğunda isteğe bir özellik eklemezseniz, istek bu özelliği depolanan varlıktan kaldırır. Bir özelliği depolanmış bir varlıktan açıkça kaldırmak istemiyorsanız, istekteki tüm mülkleri eklemeniz gerekir.  

Bir varlığı `Merge` güncelleştirmek `TableOperation` istediğinizde Tablo depolamasına gönderdiğiniz veri miktarını azaltmak için sınıfın yöntemini kullanabilirsiniz. Yöntem, `Merge` depolanan varlıktaki tüm özellikleri, isteğe dahil olan varlığın özellik değerleriyle değiştirir. Bu yöntem, depolanan varlıkta isteğe dahil olmayan tüm özellikleri bozulmadan bırakır. Bu, büyük varlıklarınız varsa ve yalnızca istekteki az sayıda özelliği güncelleştirmeniz gerekiyorsa yararlıdır.  

> [!NOTE]
> Varlık `*Replace` `Merge` yoksa ve yöntemler başarısız olur. Alternatif olarak, yoksa yeni `InsertOrReplace` `InsertOrMerge` bir varlık oluşturan ve yöntemleri kullanabilirsiniz.  
> 
> 

### <a name="work-with-heterogeneous-entity-types"></a>Heterojen varlık türleri ile çalışma
Tablo depolama *şemasız* bir tablo deposudur. Bu, tek bir tablonun birden çok türdeki varlıkları depolayarak tasarımınızda büyük esneklik sağabileceği anlamına gelir. Aşağıdaki örnekte, hem çalışan hem de departman varlıklarını depolayan bir tablo gösterilebilir:  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Zaman damgası</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>DepartmentName</th>
<th>Çalışan Sayısı</th>
</tr>
<tr>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

Her `PartitionKey`varlığın hala `RowKey`, `Timestamp` , ve değerleri olmalıdır, ancak özellikleri herhangi bir kümesi olabilir. Ayrıca, bu bilgileri bir yerde depolamayı seçmediğiniz sürece varlığın türünü gösteren hiçbir şey yoktur. Varlık türünü tanımlamak için iki seçenek vardır:  

* Varlık türünü `RowKey` (veya muhtemelen) `PartitionKey`hazırlayın. Örneğin, `EMPLOYEE_000123` ya `DEPARTMENT_SALES` `RowKey` da değerler olarak.  
* Aşağıdaki tabloda gösterildiği gibi varlık türünü kaydetmek için ayrı bir özellik kullanın.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Zaman damgası</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Employee (Çalışan)</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Employee (Çalışan)</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>DepartmentName</th>
<th>Çalışan Sayısı</th>
</tr>
<tr>
<td>Bölüm</td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Employee (Çalışan)</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

Varlık türünü `RowKey`, farklı türlerde iki varlığın aynı anahtar değerine sahip olma olasılığı varsa, ilk seçenek yararlıdır. Ayrıca, aynı türdeki varlıkları bölümiçinde gruplayır.  

Bu bölümde tartışılan teknikler özellikle[Kalıtımı olan ilişkiler](#inheritance-relationships)le ilgili tartışmalarla ilgilidir.  

> [!NOTE]
> İstemci uygulamalarının POCO nesnelerini geliştirmesini ve farklı sürümlerle çalışmasını sağlamak için varlık türü değerine bir sürüm numarası dahil etmeyi düşünün.  
> 
> 

Bu bölümün geri kalanı, Depolama İstemci Kitaplığı'nda aynı tablodaki birden çok varlık türüyle çalışmayı kolaylaştıran bazı özellikleri açıklar.  

#### <a name="retrieve-heterogeneous-entity-types"></a>Heterojen varlık türlerini alma
Depolama İstemci Kitaplığı kullanıyorsanız, birden çok varlık türüyle çalışmak için üç seçeneğiniz vardır.  

Belirli `RowKey` ve `PartitionKey` değerlerle depolanan varlığın türünü biliyorsanız, varlığı alırken varlık türünü belirtebilirsiniz. Bunu önceki iki örnekte gördün: `EmployeeEntity`Depolama [İstemci Kitaplığı'nı kullanarak bir nokta sorgusu çalıştırın](#run-a-point-query-by-using-the-storage-client-library) ve [LINQ kullanarak birden çok varlığı alın.](#retrieve-multiple-entities-by-using-linq)  

İkinci seçenek, somut `DynamicTableEntity` bir POCO varlık türü yerine türü (özellik torbası) kullanmaktır. Varlığı .NET türlerine seri hale getirmek ve deserialize etmeye gerek olmadığından, bu seçenek performansı da artırabilir. Aşağıdaki C# kodu, tablodan farklı türlerde birden çok varlık alabilir, `DynamicTableEntity` ancak tüm varlıkları örnek olarak döndürür. Daha sonra `EntityType` her varlığın türünü belirlemek için özelliği kullanır:  

```csharp
string filter = TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("PartitionKey",
    QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("RowKey",
                    QueryComparisons.GreaterThanOrEqual, "B"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey",
        QueryComparisons.LessThan, "F")
    )
);
TableQuery<DynamicTableEntity> entityQuery =
    new TableQuery<DynamicTableEntity>().Where(filter);
var employees = employeeTable.ExecuteQuery(entityQuery);

IEnumerable<DynamicTableEntity> entities = employeeTable.ExecuteQuery(entityQuery);
foreach (var e in entities)
{
EntityProperty entityTypeProperty;
if (e.Properties.TryGetValue("EntityType", out entityTypeProperty))
{
    if (entityTypeProperty.StringValue == "Employee")
    {
        // Use entityTypeProperty, RowKey, PartitionKey, Etag, and Timestamp
        }
    }
}  
```

Diğer özellikleri almak için, `TryGetValue` `Properties` `DynamicTableEntity` sınıfın özelliği üzerinde yöntemi kullanmanız gerekir.  

Üçüncü bir seçenek `DynamicTableEntity` türü ve bir `EntityResolver` örneği kullanarak birleştirmektir. Bu, aynı sorguda birden çok POCO türüne çözüm sağlamanızı sağlar. Bu örnekte, `EntityResolver` temsilci sorgunun döndürdettiği iki varlık türünü ayırt etmek için `EntityType` özelliği kullanır. Yöntem, `Resolve` örnekleri `resolver` örneklere `DynamicTableEntity` çözmek `TableEntity` için temsilciyi kullanır.  

```csharp
EntityResolver<TableEntity> resolver = (pk, rk, ts, props, etag) =>
{

        TableEntity resolvedEntity = null;
        if (props["EntityType"].StringValue == "Department")
        {
        resolvedEntity = new DepartmentEntity();
        }
        else if (props["EntityType"].StringValue == "Employee")
        {
        resolvedEntity = new EmployeeEntity();
        }
        else throw new ArgumentException("Unrecognized entity", "props");

        resolvedEntity.PartitionKey = pk;
        resolvedEntity.RowKey = rk;
        resolvedEntity.Timestamp = ts;
        resolvedEntity.ETag = etag;
        resolvedEntity.ReadEntity(props, null);
        return resolvedEntity;
};

string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<DynamicTableEntity> entityQuery =
        new TableQuery<DynamicTableEntity>().Where(filter);

var entities = employeeTable.ExecuteQuery(entityQuery, resolver);
foreach (var e in entities)
{
        if (e is DepartmentEntity)
        {
    ...
        }
        if (e is EmployeeEntity)
        {
    ...
        }
}  
```

#### <a name="modify-heterogeneous-entity-types"></a>Heterojen varlık türlerini değiştirme
Silmek için varlığın türünü bilmeniz gerekmez ve eklediğinizde varlığın türünü her zaman bilirsiniz. Ancak, `DynamicTableEntity` türü, bir varlığı türünü bilmeden ve bir POCO varlık sınıfı kullanmadan güncelleştirmek için kullanabilirsiniz. Aşağıdaki kod örneği tek bir varlığı alır `EmployeeCount` ve özelliği güncelleştirmeden önce var olduğunu denetler.  

```csharp
TableResult result =
        employeeTable.Execute(TableOperation.Retrieve(partitionKey, rowKey));
DynamicTableEntity department = (DynamicTableEntity)result.Result;

EntityProperty countProperty;

if (!department.Properties.TryGetValue("EmployeeCount", out countProperty))
{
        throw new
        InvalidOperationException("Invalid entity, EmployeeCount property not found.");
}
countProperty.Int32Value += 1;
employeeTable.Execute(TableOperation.Merge(department));  
```

### <a name="control-access-with-shared-access-signatures"></a>Paylaşılan erişim imzalarıyla erişimi denetleme
Paylaşılan erişim imzası (SAS) belirteçlerini, istemci uygulamalarının tablo varlıklarını doğrudan tablo depolama alanıyla doğrulamaya gerek kalmadan doğrudan değiştirmesine (ve sorgulayabilmesini) etkinleştirmek için kullanabilirsiniz. Genellikle, uygulamanızda SAS kullanmanın üç ana avantajı vardır:  

* Depolama alanının Tablo depolamadaki varlıklara erişmesine ve varlıkları değiştirmesine izin vermek için depolama hesabı anahtarınızı güvenli olmayan bir platforma (mobil aygıt gibi) dağıtmanız gerekmez.  
* Web ve çalışan rollerinin varlıklarınızı yönetmede gerçekleştirdiği çalışmaların bir kısmını boşaltabilirsiniz. Son kullanıcı bilgisayarları ve mobil aygıtlar gibi istemci aygıtlarına yük yükleyebilirsiniz.  
* Bir istemciye kısıtlı ve zaman sınırlı bir izin kümesi atayabilirsiniz (örneğin, belirli kaynaklara salt okunur erişime izin vermek gibi).  

Tablo depolama alanıyla SAS belirteçlerini kullanma hakkında daha fazla bilgi için [bkz.](../storage/common/storage-dotnet-shared-access-signature-part-1.md)  

Ancak, Yine de Tablo depolama varlıklara bir istemci uygulaması veren SAS belirteçleri oluşturmanız gerekir. Bunu, depolama hesabı anahtarlarınıza güvenli erişimi olan bir ortamda yapın. Genellikle, SAS belirteçlerini oluşturmak ve bunları varlıklarınıza erişması gereken istemci uygulamalarına teslim etmek için bir web veya çalışan rolü kullanırsınız. Müşterilere SAS belirteçleri oluşturma ve sunma da dahil olmak üzere hala bir ek yükü olduğundan, özellikle yüksek hacimli senaryolarda bu yükü en iyi nasıl azaltabileceğinizi düşünmelisiniz.  

Tablodaki varlıkların bir alt kümesine erişim sağlayan bir SAS belirteci oluşturmak mümkündür. Varsayılan olarak, tüm tablo için bir SAS belirteci oluşturursunuz. Ancak, SAS belirteci nin bir dizi `PartitionKey` değere veya çeşitli `PartitionKey` `RowKey` değerlere erişim izni verdiğini de belirtmek mümkündür. Sisteminizin tek tek kullanıcıları için, her kullanıcının SAS belirteci yalnızca Tablo depolama kendi varlıklarına erişim sağlar gibi SAS belirteçleri oluşturmak için seçebilirsiniz.  

### <a name="asynchronous-and-parallel-operations"></a>Asynchronous ve paralel işlemler
İsteklerinizi birden çok bölüme yayıyorsanız, eşzamanlı veya paralel sorgular kullanarak iş ve istemci yanıt verme yeteneğini artırabilirsiniz.
Örneğin, tablolarınıza paralel olarak erişen iki veya daha fazla alt rol örneğiniz olabilir. Belirli bölüm kümelerinden sorumlu tek tek alt rollere sahip olabilir veya her biri bir tablodaki tüm bölümlere erişebilen birden çok alt rol örneği olabilir.  

İstemci örneğinde, depolama işlemlerini eşit bir şekilde çalıştırarak iş yerini artırabilirsiniz. Depolama İstemci Kitaplığı, eşzamanlı sorgular ve değişiklikler yazmayı kolaylaştırır. Örneğin, aşağıdaki C# kodunda gösterildiği gibi, bir bölümdeki tüm varlıkları alan senkron yöntemiyle başlayabilirsiniz:  

```csharp
private static void ManyEntitiesQuery(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

        TableContinuationToken continuationToken = null;

        do
        {
        var employees = employeeTable.ExecuteQuerySegmented(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
    {
        ...
    }
        continuationToken = employees.ContinuationToken;
        } while (continuationToken != null);
}  
```

Bu kodu, sorgunun aşağıdaki gibi eşit bir şekilde çalıştırılabilmeleri için kolayca değiştirebilirsiniz:  

```csharp
private static async Task ManyEntitiesQueryAsync(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);
        TableContinuationToken continuationToken = null;

        do
        {
        var employees = await employeeTable.ExecuteQuerySegmentedAsync(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
        {
            ...
        }
        continuationToken = employees.ContinuationToken;
            } while (continuationToken != null);
}  
```

Bu eşzamanlı örnekte, senkron sürümden aşağıdaki değişiklikleri görebilirsiniz:  

* Yöntem imzası şimdi `async` değiştirici içerir ve `Task` bir örnek döndürür.  
* Yöntem, sonuçları `ExecuteSegmented` almak için yöntemi çağırmak yerine, yöntemi şimdi çağırır. `ExecuteSegmentedAsync` Yöntem, sonuçları `await` eşit olarak almak için değiştirici kullanır.  

İstemci uygulaması bu yöntemi `department` parametre için farklı değerlerle birden çok kez arayabilir. Her sorgu ayrı bir iş parçacığı üzerinde çalışır.  

Arabirim asynchronous numaralandırmayı desteklemediği `Execute` `TableQuery` için, yöntemin sınıfta eşzamanlı bir sürümü yoktur. `IEnumerable`  

Ayrıca varlıkları eşit bir şekilde ekleyebilir, güncelleyebilir ve silebilirsiniz. Aşağıdaki C# örneği, çalışan varlığını eklemek veya değiştirmek için basit, eşzamanlı bir yöntem gösterir:  

```csharp
private static void SimpleEmployeeUpsert(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = employeeTable
        .Execute(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

Güncelleştirmenin aşağıdaki gibi eşsenkronize olarak çalıştırılabilmeleri için bu kodu kolayca değiştirebilirsiniz:  

```csharp
private static async Task SimpleEmployeeUpsertAsync(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = await employeeTable
        .ExecuteAsync(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

Bu eşzamanlı örnekte, senkron sürümden aşağıdaki değişiklikleri görebilirsiniz:  

* Yöntem imzası şimdi `async` değiştirici içerir ve `Task` bir örnek döndürür.  
* Varlığı güncelleştirmek `Execute` için yöntemi çağırmak yerine, yöntem `ExecuteAsync` artık yöntemi çağırır. Yöntem, sonuçları `await` eşit olarak almak için değiştirici kullanır.  

İstemci uygulaması bunun gibi birden çok eşzamanlı yöntem çağırabilir ve her yöntem çağırma ayrı bir iş parçacığı üzerinde çalışır.  


[1]: ./media/storage-table-design-guide/storage-table-design-IMAGE01.png
[2]: ./media/storage-table-design-guide/storage-table-design-IMAGE02.png
[3]: ./media/storage-table-design-guide/storage-table-design-IMAGE03.png
[4]: ./media/storage-table-design-guide/storage-table-design-IMAGE04.png
[5]: ./media/storage-table-design-guide/storage-table-design-IMAGE05.png
[6]: ./media/storage-table-design-guide/storage-table-design-IMAGE06.png
[7]: ./media/storage-table-design-guide/storage-table-design-IMAGE07.png
[8]: ./media/storage-table-design-guide/storage-table-design-IMAGE08.png
[9]: ./media/storage-table-design-guide/storage-table-design-IMAGE09.png
[10]: ./media/storage-table-design-guide/storage-table-design-IMAGE10.png
[11]: ./media/storage-table-design-guide/storage-table-design-IMAGE11.png
[12]: ./media/storage-table-design-guide/storage-table-design-IMAGE12.png
[13]: ./media/storage-table-design-guide/storage-table-design-IMAGE13.png
[14]: ./media/storage-table-design-guide/storage-table-design-IMAGE14.png
[15]: ./media/storage-table-design-guide/storage-table-design-IMAGE15.png
[16]: ./media/storage-table-design-guide/storage-table-design-IMAGE16.png
[17]: ./media/storage-table-design-guide/storage-table-design-IMAGE17.png
[18]: ./media/storage-table-design-guide/storage-table-design-IMAGE18.png
[19]: ./media/storage-table-design-guide/storage-table-design-IMAGE19.png
[20]: ./media/storage-table-design-guide/storage-table-design-IMAGE20.png
[21]: ./media/storage-table-design-guide/storage-table-design-IMAGE21.png
[22]: ./media/storage-table-design-guide/storage-table-design-IMAGE22.png
[23]: ./media/storage-table-design-guide/storage-table-design-IMAGE23.png
[24]: ./media/storage-table-design-guide/storage-table-design-IMAGE24.png
[25]: ./media/storage-table-design-guide/storage-table-design-IMAGE25.png
[26]: ./media/storage-table-design-guide/storage-table-design-IMAGE26.png
[27]: ./media/storage-table-design-guide/storage-table-design-IMAGE27.png
[28]: ./media/storage-table-design-guide/storage-table-design-IMAGE28.png
[29]: ./media/storage-table-design-guide/storage-table-design-IMAGE29.png

