---
title: Ölçek ve performans için Azure Cosmos DB tabloları tasarlama
description: 'Azure Tablo depolama tasarım kılavuzu: Azure Cosmos DB ve Azure Tablo depolamadaki ölçeklenebilir ve performanslı tablolar'
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: conceptual
ms.date: 05/21/2019
author: sakash279
ms.author: akshanka
ms.custom: seodec18
ms.openlocfilehash: 166076d366cbbf7bef24648772beaba9b3a88253
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79246480"
---
# <a name="azure-table-storage-table-design-guide-scalable-and-performant-tables"></a>Azure Tablo depolama tablosu Tasarım Kılavuzu: ölçeklenebilir ve performank tabloları

[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

Ölçeklenebilir ve performansa yönelik tablolar tasarlamak için maliyet de dahil olmak üzere çeşitli faktörleri göz önünde bulundurmanız gerekir. Daha önce ilişkisel veritabanları için şema tasarladıysanız dikkat edilmesi gereken bu yönler size tanıdık gelir. Azure Tablo depolaması ve ilişkisel modeller arasında bazı benzerlikler olsa da birçok önemli farklılıklar da bulunur. Bu farklılıklar, ilişkisel veritabanlarına aşina olan bir kullanıcıya genelde sezgisel olmayan veya yanlış olarak gözükebilir. Ancak, Tablo depolaması gibi bir NoSQL anahtar/değer deposu için tasarım yapıyorsanız bu anlamlı olur.

Tablo depolaması, verilerin milyarlarca varlık (ilişkisel veritabanı terminolojisinde "satırlar") veya yüksek işlem birimlerini desteklemesi gereken veri kümeleri için içerebilen bulut ölçekli uygulamaları destekleyecek şekilde tasarlanmıştır. Bu nedenle, verilerinizi nasıl depoladığınıza ve tablo depolamanın nasıl çalıştığını nasıl anlayacağınızı düşünün. İyi tasarlanmış bir NoSQL veri deposu, çözümünüzün daha fazla (ve daha düşük bir maliyetle) ilişkisel bir veritabanı kullanan bir çözümden daha fazla ölçeklenebilmesini sağlayabilir. Bu kılavuz, şu konularda yardımcı olur.  

## <a name="about-azure-table-storage"></a>Azure Tablo depolama hakkında
Bu bölümde, özellikle performans ve ölçeklenebilirlik için tasarlanmasıyla ilgili tablo depolamanın bazı temel özellikleri vurgulanmıştır. Azure depolama ve tablo depolama 'yı yeni kullanmaya başladıysanız, bu makalenin geri kalanını okumadan önce .NET kullanarak [Microsoft Azure depolama giriş](../storage/common/storage-introduction.md) ve [Azure Tablo depolama ile çalışmaya başlama](table-storage-how-to-use-dotnet.md) bölümüne bakın. Bu kılavuzun odağı tablo depolama alanında olsa da, Azure kuyruk depolama ve Azure Blob depolama hakkında bazı tartışmalar ve bunları bir çözümde tablo depolama ile birlikte nasıl kullanabileceğinizi de kapsar.  

Tablo depolama, verileri depolamak için tablolu bir biçim kullanır. Standart terminolojisinde, tablodaki her satır bir varlığı temsil eder ve bu varlığa çeşitli özelliklerini sütunları depolayın. Her varlığın benzersiz bir şekilde tanımlanması için bir çift anahtar ve tablo depolamanın, varlığın en son ne zaman güncelleştirildiğini izlemek için kullandığı bir zaman damgası sütunu vardır. Zaman damgası alanı otomatik olarak eklenir ve zaman damgasını rastgele bir değerle el ile geçersiz kılabilirsiniz. Tablo depolaması, iyimser eşzamanlılık yönetimi için bu son değiştirilme zaman damgasını (LMT) kullanır.  

> [!NOTE]
> Tablo depolama REST API işlemler, LMT 'den türetilen bir `ETag` değeri de döndürür. Bu belgede, aynı temel verilere başvurdukları için ETag ve LMT terimleri birbirinin yerine kullanılır.  
> 
> 

Aşağıdaki örnek, çalışan ve departman varlıkları depolamak için bir basit Tablo Tasarımı gösterir. Bu kılavuzda gösterilen örneklerden birçoğu, bu basit bir tasarım üzerinde temel alır.  

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
<td>Hall</td>
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
<td>CAO</td>
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
<th>EmployeeCount</th>
</tr>
<tr>
<td>Pazarlama</td>
<td>153</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>Satış</td>
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


Şimdiye kadar, bu tasarım ilişkisel veritabanındaki bir tabloya benzer şekilde görünür. Önemli farklılıklar, zorunlu sütunlardır ve birden çok varlık türünü aynı tabloda depolayabilme özelliğidir. Ayrıca, **ad** veya **yaş**gibi Kullanıcı tanımlı özelliklerin her biri, ilişkisel veritabanındaki bir sütun gibi bir veri türüne (örneğin, bir tamsayı veya dize) sahiptir. Ancak, ilişkisel bir veritabanının aksine, tablo depolamanın şema-daha az doğası, bir özelliğin her varlıkta aynı veri türüne sahip olması gerektiği anlamına gelir. Karmaşık veri türlerini tek bir özellik depolamak için JSON veya XML gibi bir seri hale getirilmiş biçimi kullanmanız gerekir. Daha fazla bilgi için bkz. [Tablo depolama veri modelini anlama](https://msdn.microsoft.com/library/azure/dd179338.aspx).

`PartitionKey` ve `RowKey` seçiminiz, iyi tablo tasarımı için temel seçenektir. Bir tabloda depolanan her varlığın benzersiz bir `PartitionKey` ve `RowKey`birleşimi olmalıdır. İlişkisel bir veritabanı tablosundaki anahtarlarda olduğu gibi, `PartitionKey` ve `RowKey` değerleri, Hızlı görünüm sağlayan bir kümelenmiş dizin oluşturmak için dizinlenir. Ancak tablo depolama, ikincil dizinler oluşturmaz, bu nedenle yalnızca iki dizinli özellik bulunur (daha sonra açıklanan bazı desenler, daha sonra bu görünen sınırlamanın nasıl geçici bir şekilde çalışabilir).  

Bir tablo, bir veya daha fazla bölümden oluşur ve yaptığınız birçok tasarım kararı, çözümünüzü iyileştirmek için uygun bir `PartitionKey` ve `RowKey` seçmeyecektir. Bir çözüm, bölümler halinde düzenlenmiş tüm varlıklarınızı içeren tek bir tablodan oluşabilir, ancak genellikle bir çözümde birden çok tablo vardır. Tablolar, varlıklarınızı mantıksal olarak düzenlemenize ve erişim denetim listelerini kullanarak verilere erişimi yönetmenize yardımcı olur. Tek bir depolama işlemi kullanarak bir tablonun tamamını bırakabilirsiniz.  

### <a name="table-partitions"></a>Tablo bölümleri
Hesap adı, tablo adı ve `PartitionKey` birlikte, depolama hizmeti içindeki bölümü, tablo depolamanın varlığı depoladığını belirler. Varlıklar için adresleme şemasının bir parçası olan bölümler, işlemler için bir kapsam tanımlar (Bu makalenin ilerleyen bölümüne, [varlık grubu işlemlerine](#entity-group-transactions)bakın) ve tablo depolamanın nasıl ölçeklendirilebildiği temel bir şekilde oluşturur. Tablo bölümleri hakkında daha fazla bilgi için bkz. [Tablo depolama Için performans ve ölçeklenebilirlik denetim listesi](../storage/tables/storage-performance-checklist.md).  

Tablo depolamada, tek bir düğüm bir veya daha fazla tam bölüm hizmetleri ve hizmet, düğümler arasında dinamik olarak yük dengeleme bölümlerine göre ölçeklendirilir. Bir düğüm Load altındaysa tablo depolaması, bu düğüm tarafından hizmet verilen bölümlerin aralığını farklı düğümlere bölebilir. Trafik alt tarafları, tablo depolaması, Bölüm aralıklarını sessiz düğümlerden tek bir düğüm üzerine geri birleştirebilirler.  

Tablo depolamanın iç ayrıntıları hakkında daha fazla bilgi edinmek ve belirli bölümleri nasıl yönettiği hakkında daha fazla bilgi için, bkz. [Microsoft Azure depolama: güçlü tutarlılık ile yüksek oranda kullanılabilir bir bulut depolama hizmeti](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).  

### <a name="entity-group-transactions"></a>Varlık grubu işlemleri
Tablo depolamadaki varlık grubu işlemleri (Yumurlar), birden çok varlıkta atomik güncelleştirmeler gerçekleştirmeye yönelik tek yerleşik mekanizmadır. Yumurtları *toplu işlemler*olarak da adlandırılır. Yumurtları yalnızca aynı bölümde depolanan varlıklar üzerinde çalışabilir (belirli bir tabloda aynı bölüm anahtarını paylaşıyor), bu nedenle birden çok varlık genelinde atomik işlem davranışına ihtiyacınız varsa, bu varlıkların aynı bölümde olduğundan emin olun. Bu genellikle, farklı varlık türleri için birden çok tablo kullanmadan, birden çok varlık türünü aynı tabloda (ve bölümde) tutmanın bir nedenidir. Tek bir EGT en fazla 100 varlık üzerinde çalışabilir.  İşlenmek üzere birden çok eş zamanlı Yumurtları gönderirseniz, bu yumurtların Yumurtları genelinde ortak varlıklar üzerinde çalışmalarından emin olmak önemlidir. Aksi takdirde, işlemeyi erteleyerek risk alırsınız.

Yumurtları, tasarımınızda değerlendirmenize yönelik potansiyel bir ticaretle da tanıtılmaktadır. Azure 'un düğümler arasında yük dengeleme isteklerini daha fazla fırsata sahip olduğu için daha fazla bölüm kullanmak uygulamanızın ölçeklenebilirliğini artırır. Ancak bu durum, uygulamanızın Atomik işlemler gerçekleştirmesini ve verilerinize yönelik güçlü tutarlılığı korumanıza yönelik yeteneği sınırlayabilir. Ayrıca, tek bir düğüm için bekleneceğiniz işlem verimini sınırlayabilen bir bölüm düzeyinde belirli ölçeklenebilirlik hedefleri vardır.

Azure depolama hesapları için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için bkz. [Standart depolama hesapları Için ölçeklenebilirlik hedefleri](../storage/common/scalability-targets-standard-account.md). Tablo depolama için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için bkz. [Tablo Depolaması Için ölçeklenebilirlik ve performans hedefleri](../storage/tables/scalability-targets.md). Bu kılavuzun sonraki bölümlerde ele çeşitli yardımcı olacak stratejileri gibi bu stillerden yönetmek ve özel istemci uygulamanızın gereksinimlerine göre bölüm anahtarı seçmek için en iyi şekilde nasıl ele tasarım.  

### <a name="capacity-considerations"></a>Kapasite konuları
Aşağıdaki tabloda, bir tablo depolama çözümü tasarlarken farkında olacak bazı anahtar değerler yer almaktadır:  

| Bir Azure depolama hesabının toplam kapasite | 500 TB |
| --- | --- |
| Bir Azure depolama hesabındaki tablolar sayısı |Yalnızca depolama hesabının kapasitesine göre sınırlandırılır. |
| Bir tablodaki bölümlerin sayısı |Yalnızca depolama hesabının kapasitesine göre sınırlandırılır. |
| Bir bölümdeki varlıkların sayısı |Yalnızca depolama hesabının kapasitesine göre sınırlandırılır. |
| Tek bir varlık boyutu |Maksimum 255 özelliği olan 1 MB 'a kadar (`PartitionKey`, `RowKey`ve `Timestamp`dahil). |
| `PartitionKey` boyutu |Boyutu 1 KB 'a kadar olan bir dize. |
| `RowKey` boyutu |Boyutu 1 KB 'a kadar olan bir dize. |
| Bir varlık grubu işleminin boyutu |Bir işlem en fazla 100 varlığı içerebilir ve yükün boyutu 4 MB 'tan az olmalıdır. Bir EGT varlığın yalnızca bir kez güncelleştirebilirsiniz. |

Daha fazla bilgi için bkz. [Tablo hizmeti veri modelini anlama](https://msdn.microsoft.com/library/azure/dd179338.aspx).  

### <a name="cost-considerations"></a>Maliyetle ilgili konular
Tablo Depolaması nispeten ucuzdur, ancak her iki kapasite kullanımı için maliyet tahminleri ve tablo depolamayı kullanan herhangi bir çözüm değerlendirmesinin bir parçası olarak işlem miktarı dahil edilmelidir. Ancak, Çoğu senaryoda, çözümünüzün performansını veya ölçeklenebilirliğini geliştirmek için, Normalleştirilmemiş veya yinelenen verileri depolamak için geçerli bir yaklaşım vardır. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).  

## <a name="guidelines-for-table-design"></a>Tablo tasarımı için yönergeler
Bu listeler, tablolarınızı tasarlarken aklınızda bulundurmanız gereken önemli yönergelerinden bazılarını özetler. Bu kılavuz, bunların hepsini daha sonra daha ayrıntılı bir şekilde ele alır. Bu yönergeler, genellikle ilişkisel veritabanı tasarımı için takip ettiğiniz kılavuzlardan farklıdır.  

Tablo *depolamayı etkili bir* şekilde tasarlamak için tasarlama:

* **Okuma ağır uygulamalarda sorgulama için tasarım.** Tablolarınızı tasarlarken, varlıklarınızı nasıl güncelleştireceğiz konusunda düşündüğünüzden önce çalıştıracağınız sorgular (özellikle gecikme süresine duyarlı olanlar) hakkında düşünün. Bu genellikle, verimli ve yüksek performanslı bir çözümde de sonuçlanır.  
* **Sorgularınızda hem `PartitionKey` hem de `RowKey` belirtin.** Bunlar gibi *nokta sorguları* en verimli tablo depolama sorgulardır.  
* **Varlıkların yinelenen kopyalarını depolamayı göz önünde bulundurun.** Tablo Depolaması bir tek EAP olduğundan, daha verimli sorgular sağlamak için aynı varlığı birden çok kez (farklı anahtarlarla) depolamayı düşünün.  
* **Verilerinizi kabul etmeyi düşünün.** Tablo Depolaması bir şebedir, bu nedenle verilerinizi kabul etmeyi düşünün. Örneğin, veri toplama için sorgular yalnızca tek bir varlık erişmesi gereken şekilde Özet varlıkları depolayın.  
* **Bileşik anahtar değerlerini kullanın.** Sahip olduğunuz tek anahtarlar `PartitionKey` ve `RowKey`. Örneğin, varlıklara alternatif anahtarlı erişim yolları etkinleştirmek için bileşik bir anahtar değerlerini kullanın.  
* **Sorgu projeksiyonu kullanın.** Yalnızca istediğiniz alanları seçin sorgularını kullanarak ağ üzerinden aktarım veri miktarını azaltabilirsiniz.  

Tablo Depolamayı, *yazma* etkili olacak şekilde tasarlama:  

* **Etkin bölümler oluşturmayın.** Birden çok bölüm zaman herhangi bir noktada isteklerinizi yayılmış olanak tanıyan tuşlarını seçin.  
* **Trafikte ani artışlar önleyin.** Trafiği makul bir süre içinde dağıtın ve trafikte ani artışlar önleyin.
* **Her varlık türü için ayrı bir tablo oluşturmanız gerekmez.** Varlık türlerinde atomik işlemler ihtiyaç duyduğunuzda, bu varlık türleri aynı tablodaki aynı bölümde depolayabilirsiniz.
* **Elde etmeniz gereken en yüksek aktarım hızını göz önünde bulundurun.** Tablo depolaması için ölçeklenebilirlik hedeflerini bilmeniz ve tasarımınızın bu özellikleri aşmanıza neden olmamasını sağlamalısınız.  

Bu kılavuzun ilerleyen kısımlarında, tüm bu ilkeleri uygulamaya yerleştirtirecek örnekleri görürsünüz.  

## <a name="design-for-querying"></a>Sorgulama için tasarım
Tablo depolama, yoğun okuma, yoğun yazma veya ikisinin karışımı olabilir. Bu bölüm, okuma işlemlerini verimli şekilde desteklemeye yönelik tasarımı dikkate alır. Genellikle, desteklediği işlemleri verimli bir şekilde okuyun bir tasarım de yazma işlemleri için verimli olur. Bununla birlikte, yazma işlemlerini desteklemek için tasarlanırken ek hususlar vardır. Bunlar, [veri değişikliğini tasarlamak üzere](#design-for-data-modification)bir sonraki bölümde ele alınmıştır.

Verileri etkili bir şekilde okumanızı sağlamak için iyi bir başlangıç noktası, "uygulamamın gerek duyduğu verileri almak için hangi sorguları çalıştırması gerekir?" sormanız.  

> [!NOTE]
> Tablo depolamayla, daha sonra değiştirmek zor ve pahalı olduğundan tasarımın en baştan olması önemlidir. Örneğin, ilişkisel bir veritabanında, var olan bir veritabanına dizinler ekleyerek genellikle performans sorunlarını ele almak mümkündür. Bu, tablo depolamada bir seçenek değildir.  

### <a name="how-your-choice-of-partitionkey-and-rowkey-affects-query-performance"></a>`PartitionKey` ve `RowKey` seçiminiz sorgu performansını nasıl etkiler
Aşağıdaki örneklerde, tablo depolamanın, çalışan varlıklarını aşağıdaki yapıyla depoladığını varsaymaktadır (örneklerin çoğu, açıklık için `Timestamp` özelliğini atlayın):  

| Sütun adı | Veri türü |
| --- | --- |
| `PartitionKey` (departman adı) |Dize |
| `RowKey` (çalışan KIMLIĞI) |Dize |
| `FirstName` |Dize |
| `LastName` |Dize |
| `Age` |Tamsayı |
| `EmailAddress` |Dize |

Tablo depolama sorguları tasarlamak için bazı genel yönergeler aşağıda verilmiştir. Aşağıdaki örneklerde kullanılan filtre sözdizimi tablo depolama REST API. Daha fazla bilgi için bkz. [Sorgu varlıkları](https://msdn.microsoft.com/library/azure/dd179421.aspx).  

* *Nokta sorgusu* , kullanılacak en etkili aramadır ve en düşük gecikme süresini gerektiren yüksek hacimli aramalar veya aramalar için önerilir. Böyle bir sorgu, hem `PartitionKey` hem de `RowKey` değerlerini belirterek tek bir varlığı etkin bir şekilde bulmak için dizinleri kullanabilir. Örneğin: `$filter=(PartitionKey eq 'Sales') and (RowKey eq '2')`.  
* İkinci en iyi *Aralık sorgusudur*. Birden fazla varlık döndürmek için `PartitionKey`ve `RowKey` değerleri aralığındaki filtreleri kullanır. `PartitionKey` değeri belirli bir bölümü tanımlar ve `RowKey` değerler bu bölümdeki varlıkların bir alt kümesini tanımlar. Örneğin: `$filter=PartitionKey eq 'Sales' and RowKey ge 'S' and RowKey lt 'T'`.  
* Üçüncü en iyi *bölüm taramasından*. `PartitionKey`ve diğer anahtar olmayan bir özellik üzerinde filtreler kullanır ve birden fazla varlık döndürebilir. `PartitionKey` değeri belirli bir bölümü tanımlar ve özellik değerleri söz konusu bölümdeki varlıkların bir alt kümesini seçer. Örneğin: `$filter=PartitionKey eq 'Sales' and LastName eq 'Smith'`.  
* *Tablo taraması* `PartitionKey`içermez ve herhangi bir eşleşen varlık için tablonuzu oluşturan tüm bölümleri aradığı için verimsiz olur. Filtrenizin `RowKey`kullanıp kullanmadığını bakılmaksızın tablo taraması gerçekleştirir. Örneğin: `$filter=LastName eq 'Jones'`.  
* Birden çok varlık döndüren Azure Tablo depolama sorguları, `PartitionKey` ve `RowKey` sırasıyla sıralar. İstemcideki varlıkları yeniden kullanmaktan kaçınmak için en yaygın sıralama düzenini tanımlayan bir `RowKey` seçin. Azure Cosmos DB Azure Tablo API'si tarafından döndürülen sorgu sonuçları bölüm anahtarına veya satır anahtarına göre sıralanmaz. Özellik farklarının ayrıntılı bir listesi için, [Azure Cosmos DB ve Azure Tablo depolamadaki tablo API'si arasındaki farklara](faq.md#where-is-table-api-not-identical-with-azure-table-storage-behavior)bakın.

`RowKey` değerleri temel alan bir filtre belirtmek için "**or**" kullanılması, Bölüm taramasıyla sonuçlanır ve Aralık sorgusu olarak değerlendirilmez. Bu nedenle, şu gibi filtreler kullanan sorgulardan kaçının: `$filter=PartitionKey eq 'Sales' and (RowKey eq '121' or RowKey eq '322')`.  

Etkili sorgular çalıştırmak için depolama Istemci kitaplığını kullanan istemci tarafı kodu örnekleri için, bkz.:  

* [Depolama Istemci kitaplığını kullanarak bir nokta sorgusu çalıştırma](#run-a-point-query-by-using-the-storage-client-library)
* [LINQ kullanarak birden çok varlık alma](#retrieve-multiple-entities-by-using-linq)
* [Sunucu tarafı projeksiyonu](#server-side-projection)  

Aynı tabloda depolanan birden fazla varlık türleri işleyebilir istemci tarafı kod örnekleri için bkz:  

* [Heterojen varlık türleriyle çalışma](#work-with-heterogeneous-entity-types)  

### <a name="choose-an-appropriate-partitionkey"></a>Uygun bir `PartitionKey` seçin
`PartitionKey` seçiminiz, varlıklarınızı birden çok bölüme (ölçeklenebilir bir çözüm sağlamak için) dağıtmak için gereken yumurtların kullanımını etkinleştirme gereksinimini dengelemelidir (tutarlılık sağlamak için).  

Bir Extreme 'de, tüm varlıklarınızı tek bir bölümde saklayabilirsiniz. Ancak bu, çözümünüzün ölçeklenebilirliğini sınırlandırabilir ve tablo depolamanın yük dengeleme isteklerini önleyebilmesini engelliyor olabilir. Diğer bir deyişle, bölüm başına bir varlık depolayabilirler. Bu yüksek oranda ölçeklenebilir ve tablo depolamayı Yük Dengeleme isteklerine olanak sağlar, ancak varlık grubu işlemlerini kullanmanızı önler.  

İdeal bir `PartitionKey`, çözümünüzün ölçeklenebilir olduğundan emin olmak için verimli sorgular kullanmanıza ve yeterli bölüme sahip olmanızı sağlar. Genellikle, varlıklarınızın varlıklarınızı yeterli bölüm genelinde dağıtan uygun bir özelliği olacağını göreceksiniz.

> [!NOTE]
> Örneğin, kullanıcılar veya çalışanlar hakkında bilgi depolayan bir sistemde `UserID` iyi bir `PartitionKey`olabilir. Bölüm anahtarı olarak belirli bir `UserID` kullanan birkaç varlık olabilir. Bir kullanıcı hakkında veri depolayan her varlık tek bir bölümde gruplandırılır. Bu varlıklara, önemli ölçüde ölçeklenebilir olmaya devam ederken, Yumurtları aracılığıyla erişilebilir.
> 
> 

`PartitionKey` seçiminiz, varlıkları ekleme, güncelleştirme ve silme ile ilgili ek hususlar vardır. Daha fazla bilgi için bu makalenin ilerleyen kısımlarında [veri değişikliği Için tasarım](#design-for-data-modification) bölümüne bakın.  

### <a name="optimize-queries-for-table-storage"></a>Tablo depolama için sorguları iyileştirme
Tablo depolama, tek bir kümelenmiş dizindeki `PartitionKey` ve `RowKey` değerlerini kullanarak varlıklarınızı otomatik olarak dizinler. Bu, nokta sorgularının kullanım açısından en verimli olmasının nedenidir. Ancak, `PartitionKey` ve `RowKey`kümelenmiş dizinde başka bir dizin yoktur.

Birçok tasarımı birden çok ölçüte bağlı varlıkların aramasını etkinleştirmek için gereksinimleri karşılaması gerekir. Örneğin, e-posta, çalışan KIMLIĞI veya soyadı temelinde çalışan varlıklarını bulma. Bölüm [tablosu tasarım desenlerinde](#table-design-patterns) bulunan aşağıdaki desenler, bu tür gereksinimlerin adresleridir. Desenler, tablo depolamanın ikincil dizinler sağlamayacağından sonra çalışma yollarını da anlatmaktadır.  

* [Bölüm içi ikincil dizin kalıbı](#intra-partition-secondary-index-pattern): farklı `RowKey` değerleri kullanarak her varlığın birden çok kopyasını depolayın (aynı bölümde). Bu, hızlı ve verimli aramalar ve farklı `RowKey` değerleri kullanarak alternatif sıralama düzenleri sağlar.  
* Bölümler [arası ikincil dizin kalıbı](#inter-partition-secondary-index-pattern): ayrı bölümlerde veya ayrı tablolarda farklı `RowKey` değerleri kullanarak her varlığın birden çok kopyasını depolayın. Bu, hızlı ve verimli aramalar ve farklı `RowKey` değerleri kullanarak alternatif sıralama düzenleri sağlar.  
* [Dizin varlıkları stili](#index-entities-pattern): varlık listeleri döndüren etkili aramaları etkinleştirmek için Dizin varlıklarını koruyun.  

### <a name="sort-data-in-table-storage"></a>Tablo depolamadaki verileri sıralama

Tablo depolama, `PartitionKey` ve ardından `RowKey`göre artan düzende sıralanmış sorgu sonuçları döndürür.

> [!NOTE]
> Azure Cosmos DB Azure Tablo API'si tarafından döndürülen sorgu sonuçları bölüm anahtarına veya satır anahtarına göre sıralanmaz. Özellik farklarının ayrıntılı bir listesi için, [Azure Cosmos DB ve Azure Tablo depolamadaki tablo API'si arasındaki farklara](faq.md#where-is-table-api-not-identical-with-azure-table-storage-behavior)bakın.

Tablo depolamadaki anahtarlar dize değerleridir. Sayısal değerlerin doğru şekilde sıralanmasını sağlamak için, bunları sabit bir uzunluğa dönüştürmeniz ve bunları sıfırlarla birlikte yapmanız gerekir. Örneğin, `RowKey` olarak kullandığınız çalışan KIMLIĞI değeri bir tamsayı değeri ise, **123** çalışan kimliğini **00000123**olarak dönüştürmeniz gerekir. 

Çoğu uygulama farklı sırada sıralanmış veri kullanmak için gereksinimler vardır: Örneğin, çalışanlar ada göre ya da tarih katılarak sıralama. Bölüm [tablosu tasarım desenlerindeki](#table-design-patterns) aşağıdaki desenler, varlıklarınız için nasıl alternatif sıralama düzenleri ele alma bölümüne yöneliktir:  

* [Bölüm içi ikincil dizin kalıbı](#intra-partition-secondary-index-pattern): farklı `RowKey` değerleri kullanarak her varlığın birden çok kopyasını depolayın (aynı bölümde). Bu, hızlı ve verimli aramalar ve farklı `RowKey` değerleri kullanarak alternatif sıralama düzenleri sağlar.  
* Bölümler [arası ikincil dizin kalıbı](#inter-partition-secondary-index-pattern): ayrı tablolarda ayrı bölümlerde farklı `RowKey` değerleri kullanarak her varlığın birden çok kopyasını depolayın. Bu, hızlı ve verimli aramalar ve farklı `RowKey` değerleri kullanarak alternatif sıralama düzenleri sağlar.
* [Günlük kuyruğu düzeni](#log-tail-pattern): ters tarih ve saat düzeninde sıralama yapan bir `RowKey` değeri kullanarak bir bölüme en son eklenen *n* varlığı alın.  

## <a name="design-for-data-modification"></a>Veri değişikliği için tasarım
Bu bölümde, ekleme, güncelleştirme iyileştirmek için tasarım konuları üzerinde odaklanır ve siler. Bazı durumlarda, veri değişikliği için optimize eden tasarımlara göre sorgulama için optimize eden tasarımlar arasındaki dengelemeyi değerlendirmeniz gerekir. Bu değerlendirme, ilişkisel veritabanları için tasarımlarda yaptığınız işe benzerdir (ancak, tasarım sunmaların yönetilmesine yönelik teknikler ilişkisel bir veritabanında farklı olur). Bölüm [tablosu tasarım desenleri](#table-design-patterns) , tablo depolaması için bazı ayrıntılı tasarım düzenlerini açıklar ve bu ticaretin bazılarını vurgular. Uygulamada, varlıkları sorgulamak için en iyi duruma getirilmiş birçok tasarım, varlıkları değiştirmek için de iyi çalışır.  

### <a name="optimize-the-performance-of-insert-update-and-delete-operations"></a>INSERT, Update ve DELETE işlemlerinin performansını iyileştirin
Bir varlığı güncelleştirmek veya silmek için, `PartitionKey` ve `RowKey` değerlerini kullanarak bunu tanımlayabilmelisiniz. Bu şekilde, varlıkların değiştirilmesini sağlamak için `PartitionKey` ve `RowKey` seçiminiz, nokta sorgularını desteklemek için tercih ettiğiniz benzer ölçütlere uymalıdır. Varlıkları mümkün olduğunca verimli bir şekilde tanımlamak istiyorsunuz. Güncelleştirmek veya silmek için gereken `PartitionKey` ve `RowKey` değerlerini bulmak için bir varlığı bulmak üzere verimsiz bölüm veya tablo taraması kullanmak istemezsiniz.  

Bölüm [tablosu tasarım desenlerindeki](#table-design-patterns) aşağıdaki desenler, ekleme, güncelleştirme ve silme işlemlerinin performansını en iyi duruma getirme adresidir:  

* [Yüksek hacimli silme stili](#high-volume-delete-pattern): eşzamanlı silme için tüm varlıkları kendi ayrı tablosunda depolayarak yüksek hacimli varlıkların silinmesini etkinleştirin. Tabloyu silerek varlıkları silersiniz.  
* [Veri serisi stili](#data-series-pattern): yaptığınız istek sayısını en aza indirmek için, tüm veri serisini tek bir varlıkta depolayın.  
* [Geniş varlıklar stili](#wide-entities-pattern): 252 'den fazla özelliği olan mantıksal varlıkları depolamak için birden çok fiziksel varlık kullanın.  
* [Büyük varlıklar stili](#large-entities-pattern): büyük özellik değerlerini depolamak için BLOB depolama kullanın.  

### <a name="ensure-consistency-in-your-stored-entities"></a>Saklı varlıklarınızda tutarlılık sağlama
Seçtiğiniz veri değişiklikleri iyileştirmek için anahtarların etkileyen diğer önemli atomik bir işlem kullanarak tutarlılık sağlamak nasıl faktördür. Yalnızca bir EGT varlıklar aynı bölümde depolanır üzerinde çalışması için de kullanabilirsiniz.  

Bölüm [tablosu tasarım desenlerinde](#table-design-patterns) aşağıdaki desenler tutarlılığı yönetme adresi:  

* [Bölüm içi ikincil dizin kalıbı](#intra-partition-secondary-index-pattern): farklı `RowKey` değerleri kullanarak her varlığın birden çok kopyasını depolayın (aynı bölümde). Bu, hızlı ve verimli aramalar ve farklı `RowKey` değerleri kullanarak alternatif sıralama düzenleri sağlar.  
* Bölümler [arası ikincil dizin kalıbı](#inter-partition-secondary-index-pattern): ayrı bölümlerde veya ayrı tablolarda farklı `RowKey` değerleri kullanarak her varlığın birden çok kopyasını depolayın. Bu, hızlı ve verimli aramalar ve farklı `RowKey` değerleri kullanarak alternatif sıralama düzenleri sağlar.  
* [Sonuçta tutarlı işlem kriteri](#eventually-consistent-transactions-pattern): Azure kuyrukları kullanarak bölüm sınırları veya depolama sistemi sınırları genelinde sürekli tutarlı davranışı etkinleştirin.
* [Dizin varlıkları stili](#index-entities-pattern): varlık listeleri döndüren etkili aramaları etkinleştirmek için Dizin varlıklarını koruyun.  
* [Denormalleştirme stili](#denormalization-pattern): tek bir nokta sorgusuyla ihtiyacınız olan tüm verileri almanızı sağlamak için ilgili verileri tek bir varlıkta birleştirin.  
* [Veri serisi stili](#data-series-pattern): yaptığınız istek sayısını en aza indirmek için, tüm veri serisini tek bir varlıkta depolayın.  

Daha fazla bilgi için bu makalenin ilerleyen kısımlarında [varlık grubu işlemleri](#entity-group-transactions) konusuna bakın.  

### <a name="ensure-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>Etkili değişiklikler için tasarımınızın etkili sorgular olmasını sağlar
Bu durumda, belirli bir senaryoya yönelik olup olmadığını çoğu durumda, her zaman etkili değişiklikler, ancak verimli sorgulama sonuçları için bir tasarım değerlendirmelidir. Bölüm [tablosu tasarım desenlerindeki](#table-design-patterns) desenlerden bazıları, varlıkları sorgulama ve değiştirme arasındaki dengelemeler açıkça değerlendirir ve her bir işlem türünün sayısını her zaman dikkate almanız gerekir.  

Bölüm [tablosu tasarım desenlerinde](#table-design-patterns) aşağıdaki desenler, verimli sorgular tasarlama ve verimli veri değişikliği için tasarlama arasında denge sağlar:  

* [Bileşik anahtar stili](#compound-key-pattern): bir istemcinin tek nokta sorgusuyla ilgili verileri araması için bileşik `RowKey` değerleri kullanın.  
* [Günlük kuyruğu düzeni](#log-tail-pattern): ters tarih ve saat düzeninde sıralama yapan bir `RowKey` değeri kullanarak bir bölüme en son eklenen *n* varlığı alın.  

## <a name="encrypt-table-data"></a>Tablo verilerini şifreleme
.NET Azure depolama istemci kitaplığı, ekleme ve değiştirme işlemleri için dize varlık özelliklerinin şifrelenmesini destekler. Şifrelenmiş dizeler hizmette ikili özellikler olarak depolanır ve şifre çözme sonrasında dizelere geri dönüştürülür.    

Şifreleme İlkesi yanı sıra, tablolar için kullanıcıların şifrelenmiş özelliklerini belirtmeniz gerekir. Bir `EncryptProperty` özniteliği belirtin (`TableEntity`türevi olan POCO varlıkları için) veya istek seçeneklerinde bir şifreleme Çözümleyicisi belirtin. Şifreleme çözümleyici, Bölüm anahtarını, satır anahtarını ve özellik adını alan bir temsilcisidir ve bu özelliğin şifrelenip şifrelenmeyeceğini belirten bir Boole değeri döndürür. Şifreleme sırasında, istemci kitaplığı bu bilgileri bir özelliğin, Tel yazarken şifrelenmesi gerekip gerekmediğine karar vermek için kullanır. Temsilci özellikleri nasıl şifrelenir etrafında mantıksal olasılığı için de sağlar. (Örneğin, X ise, özelliğini şifreleyin; Aksi halde A ve B özelliklerini şifreleyin.) Varlıkları okurken veya sorgularken bu bilgilerin sağlanması gerekli değildir.

Birleştirme Şu anda desteklenmiyor. Özelliklerin bir alt kümesi daha önce farklı bir anahtar kullanılarak şifrelendiğinden, yalnızca yeni özellikleri birleştirmek ve meta verileri güncelleştirmek veri kaybına neden olur. Birleştirme, önceden var olan varlığı hizmetten okumak ya da özellik başına yeni bir anahtar kullanmak için ek hizmet çağrıları yapılmasını gerektirir. Bunların hiçbiri performans nedenleriyle uygun değildir.     

Tablo verilerini şifreleme hakkında daha fazla bilgi için bkz. [Microsoft Azure depolama Için istemci tarafı şifreleme ve Azure Key Vault](../storage/common/storage-client-side-encryption.md).  

## <a name="model-relationships"></a>Model ilişkileri
Etki alanı modellerini oluşturma karmaşık sistemlerin tasarımında önemli bir adımdır. Genellikle, iş etki alanını anlamanız ve sisteminizin tasarımını bilgilendirmenin bir yolu olarak, varlıkları ve aralarındaki ilişkileri tanımlamak için modelleme sürecini kullanırsınız. Bu bölüm, etki alanı modellerinde bulunan bazı ortak ilişki türlerinden bazılarını tablo depolaması için tasarımlara nasıl çevrilekullanabileceğinizi ele alınmaktadır. Bir mantıksal veri modelinden fiziksel NoSQL tabanlı veri modeliyle eşleme işlemi, ilişkisel bir veritabanı tasarlarken kullanılan öğesinden farklıdır. İlişkisel veritabanları tasarımı genellikle bir veri normalleştirme işleminin artıklığı en iyi duruma getirme için optimize eder. Bu tür tasarımda ayrıca, veritabanının nasıl çalıştığı ile ilgili uygulamayı soyutlayan bildirime dayalı bir sorgulama özelliği de varsayılır.  

### <a name="one-to-many-relationships"></a>Bire çok ilişkileri
İş etki alanı nesneler arasındaki bire çok ilişkileri ortaya sık: Örneğin, bir departmandaki çalışanların çoğu vardır. Tablo depolamada, her biri belirli senaryoya uygun olabilecek profesyonelleri ve dezavantajlarla bire çok ilişkiler uygulamak için birkaç yol vardır.  

On binlerce departman ve çalışan varlığı ile çok uluslu bir kuruluşun örneğini göz önünde bulundurun. Her departmanın birçok çalışanı vardır ve her çalışan, belirli bir departmanla ilişkilendirilir. Tek bir yaklaşım, aşağıdakiler gibi ayrı departmanı ve çalışan varlıklarını depomaktır:  

![Bir departman varlığını ve bir çalışan varlığını gösteren grafik][1]

Bu örnek, `PartitionKey` değerine göre türler arasındaki örtülü bir bire çok ilişkiyi gösterir. Her departmandaki çalışanların çoğu olabilir.  

Bu örnek ayrıca departmanı varlık ve ilgili çalışan varlıkları aynı bölümde gösterir. Farklı varlık türleri için farklı bölümler, tablolar veya hatta depolama hesapları kullanmayı seçebilirsiniz.  

Aşağıdaki örnekte gösterildiği gibi, verilerinizi benimseme ve yalnızca daha fazla çalışan departman verilerine sahip çalışan varlıklarını depolayan alternatif bir yaklaşım. Bu senaryoda, bir departman yöneticisinin ayrıntılarını değiştirebilmeniz için gerekli olan bu yaklaşım en iyi olmayabilir. Bunu yapmak için, departmandaki her çalışanı güncelleştirmeniz gerekir.  

![Çalışan varlığı grafiği][2]

Daha fazla bilgi için bu kılavuzun [ilerleyen kısımlarında daha sonra, bkz](#denormalization-pattern) .  

Aşağıdaki tabloda, bire çok ilişkisine sahip olan çalışan ve departman varlıklarını depolamanın her bir yaklaşımının uzmanları ve dezavantajları özetlenmektedir. Ayrıca, çeşitli işlemleri gerçekleştirmek için ne kadar sıklıkla beklediğinizi de göz önünde bulundurmanız gerekir. Bu işlem yalnızca seyrek gerçekleşseydiğinde pahalı bir işlem içeren bir tasarıma sahip olmak için kabul edilebilir.  

<table>
<tr>
<th>Yaklaşım</th>
<th>Uzmanları</th>
<th>Simgeler</th>
</tr>
<tr>
<td>Ayrı varlık türleri, aynı bölüme, aynı tabloya</td>
<td>
<ul>
<li>Tek bir işlemle bir departman varlık güncelleştirebilirsiniz.</li>
<li>Bir EGT departmanı varlığı değiştirmek için bir gereksinimi varsa tutarlılık sağlamak için kullanabileceğiniz olduğunda, güncelleştirme/ekleme/silme çalışan varlık. Örneğin, bir departman çalışan sayısı her departman için korur.</li>
</ul>
</td>
<td>
<ul>
<li>Bazı istemci etkinlikleri için hem bir çalışan hem de bir departman varlığı almanız gerekebilir.</li>
<li>Depolama işlemleri aynı bölümde gerçekleşir. Yüksek işlem birimlerinde bu, bir etkin noktaya neden olabilir.</li>
<li>Bir çalışanı EGT kullanarak yeni bir departmana taşıyamazsınız.</li>
</ul>
</td>
</tr>
<tr>
<td>Ayrı varlık türleri, farklı bölümler veya tablolar veya depolama hesapları</td>
<td>
<ul>
<li>Bir departman veya çalışan bir varlıktan tek bir işlemle güncelleştirebilirsiniz.</li>
<li>Yüksek işlem birimlerinde bu, yükü daha fazla bölüme yaymaya yardımcı olabilir.</li>
</ul>
</td>
<td>
<ul>
<li>Bazı istemci etkinlikleri için hem bir çalışan hem de bir departman varlığı almanız gerekebilir.</li>
<li>Bir çalışanı güncelleştirdiğinizde/eklediğinizde/sildiğinizde ve bir departmanı güncelleştirdiğinizde tutarlılığı sağlamak için Yumurtları kullanamazsınız. Örneğin, bir çalışan sayısı departmanı varlıklardaki güncelleştiriliyor.</li>
<li>Bir çalışanı EGT kullanarak yeni bir departmana taşıyamazsınız.</li>
</ul>
</td>
</tr>
<tr>
<td>Tek varlık türüne normalleştirmeyi geri alın</td>
<td>
<ul>
<li>Tek bir istekle gereken tüm bilgileri alabilirsiniz.</li>
</ul>
</td>
<td>
<ul>
<li>Bölüm bilgilerini güncelleştirmeniz gerekiyorsa tutarlılığı sağlamak pahalı olabilir (Bu, bir departmandaki tüm çalışanları güncelleştirmeniz gerekir).</li>
</ul>
</td>
</tr>
</table>

Bu seçenekler arasından seçim yapma ve uzmanlarının ve dezavantajlarının en önemli olduğu, belirli uygulama senaryolarınıza bağlıdır. Örneğin, departman varlıklarını ne sıklıkta değiştirirsiniz? Tüm çalışan sorgularınız ek departman bilgilerine ihtiyaç duyuyor mu? Bölümlerinizde veya depolama hesabınızda ölçeklenebilirlik limitleriniz ne kadar yakınlanıyor?  

### <a name="one-to-one-relationships"></a>Bire bir ilişkiler
Etki alanı modelleri, varlıklar arasında bire bir ilişki içerebilir. Tablo depolamada bire bir ilişki uygulamanız gerekiyorsa, her ikisini de almanız gerektiğinde iki ilişkili varlığı bağlamayı da seçmeniz gerekir. Bu bağlantı, bir bağlantıyı bir `PartitionKey` biçiminde depolayarak ve ilgili varlığına her bir varlıktaki `RowKey` değerleri depolayarak, anahtar değerlerinde bir kurala göre örtük veya açık olabilir. Aynı bölümde ilgili varlıkların depolanması gerekip gerekmediğini öğrenmek için, [tek-çok ilişkileri](#one-to-many-relationships)bölümüne bakın.  

Ayrıca, tablo depolamada bire bir ilişki uygulamanıza yol açabilecek uygulama konuları da vardır:  

* Büyük varlıkları işleme (daha fazla bilgi için bkz. [büyük varlıklar kalıbı](#large-entities-pattern)).  
* Erişim denetimleri uygulama (daha fazla bilgi için bkz. [paylaşılan erişim imzaları Ile denetim erişimi](#control-access-with-shared-access-signatures)).  

### <a name="join-in-the-client"></a>İstemci katılın
Tablo depolamadaki ilişkileri modellemekle ilgili yollar vardır, ancak tablo depolamayı kullanmanın iki ana nedeni ölçeklenebilirlik ve performans sağlar. Çözümünüzün performansını ve ölçeklenebilirliğini tehlikeye atabilecek birçok ilişki modellebileceğinizi fark ederseniz, tüm veri ilişkilerini tablo tasarımınızda oluşturmak için gerekliyse kendinize danışmalısınız. İstemci uygulamanızın gerekli birleştirmeleri gerçekleştirmesini istiyorsanız, tasarımı basitleştirebiliyor ve çözümünüzün ölçeklenebilirlik ve performansını geliştirebilirsiniz.  

Örneğin, sıklıkla değişmeyen veriler içeren küçük tablolar varsa, bu verileri bir kez alabilir ve istemcide önbelleğe alabilirsiniz. Bu, aynı verileri almak için yinelenen gidiş-dönüş önleyebilirsiniz. Bu kılavuzda incelediğimiz örneklerde küçük bir kuruluştaki bölüm kümesinin küçük olması ve seyrek olarak değiştirilmesi olasıdır. Bu, bir istemci uygulamanın bir kez indirebileceğiniz ve arama verileri olarak önbelleğe aldığı veriler için iyi bir aday sağlar.  

### <a name="inheritance-relationships"></a>Kalıtım ilişkileri
İstemci uygulamanız, iş varlıklarını temsil etmek üzere devralma ilişkisinin bir parçasını oluşturan bir sınıf kümesi kullanıyorsa, bu varlıkları tablo depolamadaki kolayca kalıcı hale getirebilirsiniz. Örneğin, istemci uygulamanızda tanımlanmış, `Person` soyut bir sınıf olan aşağıdaki sınıf kümesine sahip olabilirsiniz.

![Devralma ilişkilerinin diyagramı][3]

Tek bir `Person` tablo kullanarak tablo depolamada iki somut sınıfın örneklerini kalıcı hale getirebilirsiniz. Aşağıdaki gibi görünen varlıkları kullanın:  

![Müşteri varlığı ve çalışan varlığını gösteren grafik][4]

İstemci kodunda aynı tabloda birden fazla varlık türüyle çalışma hakkında daha fazla bilgi için bu kılavuzun ilerleyen kısımlarında bulunan [heterojen varlık türleriyle çalışma](#work-with-heterogeneous-entity-types) konusuna bakın. Bu varlık türünde istemci kodu anlamayı örnekleri sağlar.  

## <a name="table-design-patterns"></a>Tablo tasarımı desenleri
Önceki bölümlerde, sorgu kullanarak varlık verilerini almak ve varlık verilerini eklemek, güncelleştirmek ve silmek için tablo tasarımınızı nasıl iyileştirebileceğinizi öğrendiniz. Bu bölümde, tablo depolamayla kullanım için uygun bazı desenler açıklanmaktadır. Buna ek olarak, bu kılavuzda daha önce ortaya çıkan bazı sorunlar ve dengelemeler için nasıl çözüm kullanabileceğinizi göreceksiniz. Aşağıdaki diyagramda, farklı desenler arasındaki ilişkiler özetlenmektedir:  

![Tablo Tasarım desenlerinin diyagramı][5]

Desen eşleme, bu kılavuzda belgelenen desenler (mavi) ve kenar desenleri (turuncu) arasındaki ilişkileri vurgular. Elbette, dikkate değer olan diğer birçok desen vardır. Örneğin, tablo depolaması için önemli senaryolardan biri, [komut sorgu sorumluluğu](https://msdn.microsoft.com/library/azure/jj554200.aspx) ayırma düzeninden [gerçekleştirilmiş görünüm deseninin](https://msdn.microsoft.com/library/azure/dn589782.aspx) kullanılması.  

### <a name="intra-partition-secondary-index-pattern"></a>İçi bölüm ikincil dizin düzeni
Farklı `RowKey` değerleri kullanarak her varlığın birden çok kopyasını depolayın (aynı bölümde). Bu, hızlı ve verimli aramalar ve farklı `RowKey` değerleri kullanarak alternatif sıralama düzenleri sağlar. Kopyalar arasındaki güncelleştirmeler, yumurtalar kullanılarak tutarlı tutulabilir.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tablo depolama, `PartitionKey` ve `RowKey` değerlerini kullanarak varlıkları otomatik olarak dizinler. Bu, bir istemci uygulamanın bu değerleri kullanarak bir varlığı etkin bir şekilde almasını sağlar. Örneğin, aşağıdaki tablo yapısını kullanarak, bir istemci uygulaması departman adını ve çalışan KIMLIĞINI (`PartitionKey` ve `RowKey` değerlerini) kullanarak tek bir çalışan varlığını almak için bir nokta sorgusu kullanabilir. Ayrıca, bir istemci her bir departman içindeki çalışan KIMLIĞINE göre sıralanmış varlıkları alabilir.

![Çalışan varlığı grafiği][6]

Ayrıca, e-posta adresi gibi başka bir özelliğin değerine göre bir çalışan varlığı bulmak istiyorsanız, bir eşleşme bulmak için daha az verimli bir bölüm taraması kullanmanız gerekir. Bunun nedeni tablo depolamanın ikincil dizinler sağlamadır. Ayrıca, `RowKey` sırayla farklı bir sırada sıralanan çalışanların bir listesini isteme seçeneği yoktur.  

#### <a name="solution"></a>Çözüm
İkincil dizinlerin eksikliğine geçici bir çözüm bulmak için, her bir kopyanın farklı bir `RowKey` değer kullanarak her bir varlığın birden çok kopyasını saklayabilirsiniz. Aşağıdaki yapılarla bir varlık depoluysanız, e-posta adresini veya çalışan KIMLIĞINI temel alarak çalışan varlıklarını etkin bir şekilde alabilirsiniz. `RowKey`, `empid_`ve `email_` için önek değerleri, bir dizi e-posta adresini veya çalışan kimliklerini kullanarak tek bir çalışana veya bir dizi çalışana yönelik sorgulama imkanı sağlar.  

![Değişen RowKey değerleri olan çalışan varlığını gösteren grafik][7]

Aşağıdaki iki filtre ölçütü (bir çalışan KIMLIĞI tarafından bir arama ve e-posta adresiyle arama) her ikisi de nokta sorguları belirler:  

* $filter = (PartitionKey eq 'Satış') ve (RowKey eq 'empid_000223')  
* $filter = (PartitionKey EQ ' Sales ') ve (RowKey EQ 'email_jonesj@contoso.com')  

Bir dizi çalışan varlığı için sorgulama yaparsanız, çalışan KIMLIĞI sırasında sıralanmış bir Aralık veya e-posta adresi sırasında sıralanan bir Aralık belirleyebilirsiniz. `RowKey`uygun ön eki olan varlıklar için sorgu.  

* 000100 ile 000199 arasında bir çalışan KIMLIĞI olan Satış departmanındaki tüm çalışanları bulmak için, şunu kullanın: $filter = (PartitionKey EQ ' Sales ') ve (RowKey Ge ' empid_000100 ') ve (RowKey Le ' empid_000199 ')  
* "A" harfiyle başlayan bir e-posta adresine sahip satış departmanındaki tüm çalışanları bulmak için, şunu kullanın: $filter = (PartitionKey EQ ' Sales ') ve (RowKey Ge ' email_a ') ve (RowKey lt ' email_b ')  
  
Yukarıdaki örneklerde kullanılan filtre sözdizimi tablo depolama REST API. Daha fazla bilgi için bkz. [Sorgu varlıkları](https://msdn.microsoft.com/library/azure/dd179421.aspx).  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tablo depolama alanı görece bir şekilde kullanılmak üzere, yinelenen verileri depolamanın maliyet ek yükü önemli bir sorun değildir. Bununla birlikte, tahmini depolama gereksinimlerinize göre tasarımınızın maliyetini her zaman değerlendirmeli ve yalnızca istemci uygulamanızın çalışacağı sorguları destekleyecek şekilde yinelenen varlıklar ekleyebilirsiniz.  
* İkincil dizin varlıkları özgün varlıklarla aynı bölümde depolandığından, tek bir bölüm için ölçeklenebilirlik hedeflerini aşmayın emin olun.  
* Varlığın iki kopyasını atomik olarak güncelleştirmek için EGTs kullanarak yinelenen varlıklarınızı birbiriyle tutarlı kalmasını sağlayabilirsiniz. Bu, bir varlığın tüm kopyaları aynı bölümde saklamalısınız anlamına gelir. Daha fazla bilgi için bkz. [varlık grubu Işlemlerini kullanma](#entity-group-transactions).  
* `RowKey` için kullanılan değer her varlık için benzersiz olmalıdır. Bileşik anahtar değerleri kullanmayı düşünün.  
* `RowKey` sayısal değerleri doldurma (örneğin, çalışan KIMLIĞI 000223), üst ve alt sınırlara göre doğru sıralamayı ve filtrelemeyi sunar.  
* Varlığınızın tüm özelliklerini yinelememeniz gerekmez. Örneğin, varlıkları `RowKey` bir e-posta adresini kullanarak arama yapan sorguların çalışanın kullanım ömrü hiç gerekmiyorsa, bu varlıklar aşağıdaki yapıya sahip olabilir:

  ![Çalışan varlığı grafiği][8]

* Genellikle, yinelenen verileri depolamak ve bir varlık bulmak için tek bir sorgu kullanmanın yanı sıra gerekli verileri aramak için tek bir sorgu ile ihtiyacınız olan tüm verileri almanızı sağlamak daha iyidir.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Bu düzeni aşağıdaki durumlarda kullanın:

- İstemci uygulamanızın çeşitli farklı anahtarlar kullanarak varlıkları alması gerekir.
- İstemcinizin farklı sıralama emirlerindeki varlıkları alması gerekir.
- Her varlığı, çeşitli benzersiz değerler kullanarak tanımlayabilirsiniz.

Ancak, farklı `RowKey` değerlerini kullanarak varlık aramaları gerçekleştirirken bölüm ölçeklenebilirlik sınırlarını aşmayın emin olun.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bölümler arası ikincil dizin kalıbı](#inter-partition-secondary-index-pattern)
* [Bileşik anahtar stili](#compound-key-pattern)
* [Varlık grubu işlemleri](#entity-group-transactions)
* [Heterojen varlık türleriyle çalışma](#work-with-heterogeneous-entity-types)

### <a name="inter-partition-secondary-index-pattern"></a>İkincil dizin arası bölüm düzeni
Ayrı bölümlerde veya ayrı tablolarda farklı `RowKey` değerleri kullanarak her varlığın birden çok kopyasını depolayın. Bu, hızlı ve verimli aramalar ve farklı `RowKey` değerleri kullanarak alternatif sıralama düzenleri sağlar.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tablo depolama, `PartitionKey` ve `RowKey` değerlerini kullanarak varlıkları otomatik olarak dizinler. Bu, bir istemci uygulamanın bu değerleri kullanarak bir varlığı etkin bir şekilde almasını sağlar. Örneğin, aşağıdaki tablo yapısını kullanarak, bir istemci uygulaması departman adını ve çalışan KIMLIĞINI (`PartitionKey` ve `RowKey` değerlerini) kullanarak tek bir çalışan varlığını almak için bir nokta sorgusu kullanabilir. Ayrıca, bir istemci her bir departman içindeki çalışan KIMLIĞINE göre sıralanmış varlıkları alabilir.  

![Çalışan varlığı grafiği][9]

Ayrıca e-posta adresi gibi başka bir özelliğin değerini temel alan bir çalışan varlık bulamaz istiyorsanız bir eşleşme bulmak için daha az verimli bir bölüm tarama kullanmanız gerekir. Bunun nedeni tablo depolamanın ikincil dizinler sağlamadır. Ayrıca, `RowKey` sırayla farklı bir sırada sıralanan çalışanların bir listesini isteme seçeneği yoktur.  

Bu varlıklara yönelik yüksek hacimli işlemlere benimsemeyi bekleme olursunuz ve tablo depolama hızının istemcinizi sınırlayan riskini en aza indirmek istersiniz.  

#### <a name="solution"></a>Çözüm
İkincil dizinlerin eksikliğine geçici bir çözüm için, her bir kopyanın farklı `PartitionKey` ve `RowKey` değerlerini kullanarak her bir varlığın birden çok kopyasını saklayabilirsiniz. Aşağıdaki yapılarla bir varlık depoluysanız, e-posta adresini veya çalışan KIMLIĞINI temel alarak çalışan varlıklarını etkin bir şekilde alabilirsiniz. `PartitionKey`, `empid_`ve `email_` için önek değerleri, bir sorgu için kullanmak istediğiniz dizini tanımlamanızı sağlar.  

![Birincil dizin ve ikincil dizinli çalışan varlığı olan çalışan varlığını gösteren grafik][10]

Aşağıdaki iki filtre ölçütü (bir çalışan KIMLIĞI tarafından bir arama ve e-posta adresiyle arama) her ikisi de nokta sorguları belirler:  

* $filter = (PartitionKey eq ' empid_Sales') ve (RowKey eq '000223')
* $filter = (PartitionKey EQ ' email_Sales ') ve (RowKey EQ 'jonesj@contoso.com')  

Bir dizi çalışan varlığı için sorgulama yaparsanız, çalışan KIMLIĞI sırasında sıralanmış bir Aralık veya e-posta adresi sırasında sıralanan bir Aralık belirleyebilirsiniz. `RowKey`uygun ön eki olan varlıklar için sorgu.  

* Satış departmanındaki tüm çalışanları **000100** - **000199**aralığında, çalışan kimliği SıRASıYLA sıralanan bir çalışan kimliği ile bulmak için, şunu kullanın: $Filter = (partitionkey EQ ' empid_Sales ') ve (rowkey Ge ' 000100 ') ve (rowkey Le ' 000199 ')  
* Satış departmanındaki tüm çalışanları "a" ile başlayan, e-posta adresi sırasıyla sıralanmış bir e-posta adresiyle bulmak için: $filter = (PartitionKey EQ ' email_Sales ') ve (RowKey Ge ' a ') ve (RowKey lt ' b ') kullanın  

Yukarıdaki örneklerde kullanılan filtre sözdiziminin tablo depolama REST API olduğunu unutmayın. Daha fazla bilgi için bkz. [Sorgu varlıkları](https://msdn.microsoft.com/library/azure/dd179421.aspx).  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Birincil ve ikincil dizin varlıklarını sürdürmek için [sürekli tutarlı işlemler modelini](#eventually-consistent-transactions-pattern) kullanarak, yinelenen varlıklarınızın birbirleriyle sürekli tutarlı kalmasını sağlayabilirsiniz.  
* Tablo depolama alanı görece bir şekilde kullanılmak üzere, yinelenen verileri depolamanın maliyet ek yükü önemli bir sorun olmamalıdır. Ancak, tahmin edilen depolama gereksinimlerinize göre tasarımınızın maliyetini her zaman değerlendirin ve yalnızca, istemci uygulamanızın çalışacağı sorguları destekleyecek şekilde yinelenen varlıklar ekleyin.  
* `RowKey` için kullanılan değer her varlık için benzersiz olmalıdır. Bileşik anahtar değerleri kullanmayı düşünün.  
* `RowKey` sayısal değerleri doldurma (örneğin, çalışan KIMLIĞI 000223), üst ve alt sınırlara göre doğru sıralamayı ve filtrelemeyi sunar.  
* Varlığınızın tüm özelliklerini yinelememeniz gerekmez. Örneğin, varlıkları `RowKey` bir e-posta adresini kullanarak arama yapan sorguların çalışanın kullanım ömrü hiç gerekmiyorsa, bu varlıklar aşağıdaki yapıya sahip olabilir:
  
  ![İkincil dizinli çalışan varlığını gösteren grafik][11]
* Genellikle, yinelenen verileri depolamak ve tek bir sorgu ile ihtiyacınız olan tüm verileri, ikincil dizini kullanarak bir varlığı bulmak için bir sorgu kullanmanın ve birincil dizinde gerekli verileri aramak için bir sorgu kullanmaktan emin olmak daha iyidir.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Bu düzeni aşağıdaki durumlarda kullanın:

- İstemci uygulamanızın çeşitli farklı anahtarlar kullanarak varlıkları alması gerekir.
- İstemcinizin farklı sıralama emirlerindeki varlıkları alması gerekir.
- Her varlığı, çeşitli benzersiz değerler kullanarak tanımlayabilirsiniz.

Farklı `RowKey` değerlerini kullanarak varlık aramaları gerçekleştirirken bölüm ölçeklenebilirlik sınırlarını aşmamak istiyorsanız bu stili kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Sonuçta tutarlı işlem kriteri](#eventually-consistent-transactions-pattern)  
* [Bölüm içi ikincil dizin kalıbı](#intra-partition-secondary-index-pattern)  
* [Bileşik anahtar stili](#compound-key-pattern)  
* [Varlık grubu işlemleri](#entity-group-transactions)  
* [Heterojen varlık türleriyle çalışma](#work-with-heterogeneous-entity-types)  

### <a name="eventually-consistent-transactions-pattern"></a>Son tutarlılık işlemleri deseni
Azure kuyrukları kullanılarak bölüm sınırları veya depolama sistemi sınırları arasında sonunda tutarlı bir davranış etkinleştirin.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
EGTs atomik işlemler aynı bölüm anahtarı paylaşan birden fazla varlıkta etkinleştirin. Performans ve ölçeklenebilirlik nedenleriyle, tutarlılık gereksinimlerine sahip varlıkları ayrı bölümlerde veya ayrı bir depolama sisteminde depolamaya karar verebilirsiniz. Böyle bir senaryoda, tutarlılığı sağlamak için Yumurtları kullanamazsınız. Örneğin, nihai tutarlılık arasında sağlamak için bir gereksinim olabilir:  

* Varlıklar, iki farklı bölümleri aynı tabloda, farklı tablolardaki veya farklı depolama hesaplarında depolanır.  
* Tablo depolama alanında depolanan bir varlık ve BLOB depolamada depolanan bir BLOB.  
* Tablo depolama alanında depolanan bir varlık ve dosya sistemindeki bir dosya.  
* Henüz Azure Bilişsel Arama kullanılarak dizini oluşturulmuş tablo depolamada depolanan bir varlık.  

#### <a name="solution"></a>Çözüm
Azure kuyrukları kullanılarak, son tutarlılık bölümler veya depolama sistemleri arasında iki veya daha fazla sunan bir çözüm uygulayabilirsiniz.

Bu yaklaşımı göstermek için, eski çalışan varlıklarını arşivlemek için bir gereksinimin olduğunu varsayalım. Eski çalışan varlıkları nadiren sorgulanır ve geçerli çalışanlarla ilgilenen etkinliklerden çıkarılmalıdır. Bu gereksinimi uygulamak için, **geçerli** tablodaki etkin çalışanları ve **Arşiv** tablosundaki eski çalışanları depolarlar. Bir çalışanın arşivlenmesi, varlığı **geçerli** tablodan silmenizi ve varlığı **Arşiv** tablosuna eklemenizi gerektirir.

Ancak bu iki işlemi gerçekleştirmek için EGT kullanamazsınız. Bir hata görünür bir varlık veya hiçbir tablolarında neden riskini önlemek için arşiv işlemi sonunda tutarlı olması gerekir. Aşağıdaki sıralama diyagramı, bu işlem adımları açıklanmaktadır.  

![Son tutarlılık için çözüm diyagramı][12]

İstemci, Azure kuyruğuna bir ileti yerleştirerek arşiv işlemini başlatır (Bu örnekte, çalışan #456 arşivlemek için). Bir çalışan rolü, kuyruğu yeni iletiler için yoklama; bir bulduğunda, iletiyi okur ve gizli bir kopyasını kuyrukta bırakır. Çalışan rolü, **geçerli** tablodaki varlığın bir kopyasını getirir, **Arşiv** tablosuna bir kopya ekler ve ardından **geçerli** tablodan orijinali siler. Son olarak, önceki adımlarda hata varsa, çalışan rolü gizli iletiyi kuyruktan siler.  

Bu örnekte, diyagramdaki 4. adım, çalışanı **Arşiv** tablosuna ekler. Çalışan BLOB depolama alanındaki bir bloba veya dosya sistemindeki bir dosya ekleyebilir.  

#### <a name="recover-from-failures"></a>Hatalardan kurtarma
Çalışan rolünün arşiv işlemini yeniden başlatması gerektiğinden, diyagramdaki 4-5. adımlarda gerçekleştirilen işlemlerin *ıdempotent* olması önemlidir. Tablo Depolaması kullanıyorsanız, 4. adım için "Ekle veya Değiştir" işlemini kullanmanız gerekir. 5. adımda, kullanmakta olduğunuz istemci kitaplığındaki "varsa sil" işlemini kullanmanız gerekir. Başka bir depolama sistemi kullanıyorsanız, uygun bir ıdempotent işlemi kullanmanız gerekir.  

Çalışan rolü diyagramda 6. adımı hiçbir zaman tamamlarsa, bir zaman aşımından sonra ileti tekrar işlemeyi denemek için çalışan rolü için ayrılan sırada yeniden görüntülenir. Çalışan rolü, sıradaki bir iletinin kaç kez okunduğunu denetleyebilir ve gerekirse, bunu ayrı bir kuyruğa göndererek araştırma için "Poison" iletisi olarak işaretleyin. Sıra iletilerini okuma ve sıradan çıkarma sayısını denetleme hakkında daha fazla bilgi için bkz. [Iletileri alma](https://msdn.microsoft.com/library/azure/dd179474.aspx).  

Tablo depolama ve kuyruk depolamadaki bazı hatalar geçici hatalardır ve istemci uygulamanız onları işlemek için uygun yeniden deneme mantığını içermelidir.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Bu çözüm, işlem yalıtımı sağlamaz. Örneğin, bir istemci, çalışan rolü Diyagramdaki Adım 4-5 arasında olduğunda **geçerli** ve **Arşiv** tablolarını okuyabilir ve verilerin tutarsız bir görünümünü görürsünüz. Veri sonunda tutarlı olur.  
* Nihai tutarlılığı sağlamak için 4-5 adımlarının ıdempotent olduğundan emin olmanız gerekir.  
* Çözüm, birden fazla kuyruk ve çalışan rolü örnekleri kullanarak ölçeklendirebilirsiniz.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Farklı bölümler veya tablo mevcut varlıklar arasındaki son tutarlılık garantisi istediğinizde bu düzeni kullanın. Tablo depolama ve BLOB depolama genelindeki işlemlere ve veritabanı ya da dosya sistemi gibi diğer Azure dışı depolama veri kaynaklarına yönelik nihai tutarlılığı sağlamak için bu kalıbı genişletebilirsiniz.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Varlık grubu işlemleri](#entity-group-transactions)  
* [Birleştir veya Değiştir](#merge-or-replace)  

> [!NOTE]
> Çözümünüz için işlem yalıtımı önemliyse, Yumurtları kullanmanıza olanak tanımak için tablolarınızı yeniden tasarlamayı düşünün.  
> 
> 

### <a name="index-entities-pattern"></a>Dizin varlıkları kalıbı
Varlıkların listesini döndüren verimli aramalar etkinleştirmek için dizin varlıkları korur.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tablo depolama, `PartitionKey` ve `RowKey` değerlerini kullanarak varlıkları otomatik olarak dizinler. Bu, bir istemci uygulamanın bir nokta sorgusu kullanarak bir varlığı etkin bir şekilde almasını sağlar. Örneğin, aşağıdaki tablo yapısını kullanarak, bir istemci uygulaması departman adını ve çalışan KIMLIĞINI (`PartitionKey` ve `RowKey`) kullanarak bireysel bir çalışan varlığını verimli bir şekilde alabilir.  

![Çalışan varlığı grafiği][13]

Ayrıca, soyadı gibi benzersiz olmayan başka bir özelliğin değerine göre çalışan varlıklarının bir listesini alabilmek istiyorsanız, daha az verimli bir bölüm taraması kullanmanız gerekir. Bu tarama, bunları doğrudan aramak için bir dizin kullanmak yerine eşleşmeleri bulur. Bunun nedeni tablo depolamanın ikincil dizinler sağlamadır.  

#### <a name="solution"></a>Çözüm
Önceki varlık yapısıyla son ada göre aramayı etkinleştirmek için, çalışan kimliklerinin listesini korumanız gerekir. Can gibi belirli bir soyadı olan çalışan varlıklarını almak isterseniz, ilk adı olarak Jones ile çalışanlar için çalışan kimlikleri listesini bulmanız ve ardından bu çalışan varlıklarını almanız gerekir. Çalışan kimliklerinin listesini depolamak için üç ana seçenek vardır:  

* Blob depolamayı kullanın.  
* Çalışan varlıklar aynı bölümde dizin varlıklar oluşturun.  
* Dizin varlıklar bir ayrı bölüm ya da tablo oluşturun.  

Seçenek 1: blob depolamayı kullanma  

Her benzersiz soyadı için bir blob oluşturun ve her Blob deposunda, bu soyadı olan çalışanların `PartitionKey` (departman) ve `RowKey` (çalışan KIMLIĞI) değerlerinin bir listesini oluşturur. Bir çalışan eklediğinizde veya sildiğinizde, ilgili Blob içeriğinin çalışan varlıklarıyla tutarlı olduğundan emin olun.  

2\. seçenek: aynı bölümde Dizin varlıkları oluşturma  

Aşağıdaki verileri depolayan Dizin varlıklarını kullanın:  

![Aynı soyadı taşıyan çalışan kimliklerinin bir listesini içeren bir dize içeren çalışan varlığı gösteren grafik][14]

`EmployeeIDs` özelliği, `RowKey`depolanan son ada sahip çalışanlar için çalışan kimliklerinin bir listesini içerir.  

Aşağıdaki adımlarda, yeni bir çalışan eklerken izlemeniz gereken işlem ana hatlarıyla verilmiştir. Bu örnekte, satış departmanında 000152 KIMLIĞI ve soyadı Jones olan bir çalışan ekliyoruz:  

1. Bir `PartitionKey` değeri "Sales" ve "Jones" `RowKey` değeri olan dizin varlığını alın. Bu varlık Etag'i 2. adımda kullanmak üzere kaydedin.  
2. Yeni çalışan varlığını (`PartitionKey` değeri "Sales" ve `RowKey` "000152") ekleyen bir varlık grubu işlemi (bir toplu işlem) oluşturun ve Dizin varlığını (`PartitionKey` değeri "Sales" ve `RowKey` değeri "Jones") güncelleştirir. EGT bunu, yeni çalışan KIMLIĞINI Employeıdds alanındaki listeye ekleyerek yapar. Yumurtalar hakkında daha fazla bilgi için bkz. [varlık grubu işlemleri](#entity-group-transactions).  
3. EGT, iyimser bir eşzamanlılık hatası nedeniyle başarısız olursa (yani başka biri dizin varlığını değiştirmişse), 1. adımda baştan başlamanız gerekir.  

İkinci seçeneği kullanıyorsanız, bir çalışanı silmek için benzer bir yaklaşım kullanabilirsiniz. Çalışanın soyadını değiştirme işlemi, üç varlığı güncelleştiren bir EGT çalıştırmanız gerektiğinden biraz daha karmaşıktır: çalışan varlığı, eski soyadı için Dizin varlığı ve yeni soyadı için Dizin varlığı. Daha sonra iyimser eşzamanlılık kullanarak güncelleştirmeleri gerçekleştirmek için kullanabileceğiniz ETag değerlerini almak için herhangi bir değişiklik yapmadan önce her bir varlığı almalısınız.  

Aşağıdaki adımlarda, bir departmandaki belirli bir soyadı olan tüm çalışanları aramanız gerektiğinde izlemeniz gereken işlem ana hatlarıyla verilmiştir. Bu örnekte, satış departmanında en son adı Jones olan tüm çalışanları bakıyoruz:  

1. Bir `PartitionKey` değeri "Sales" ve "Jones" `RowKey` değeri olan dizin varlığını alın.  
2. `EmployeeIDs` alanındaki çalışan kimliklerinin listesini ayrıştırın.  
3. Bu çalışanların (e-posta adresleri gibi) her biri hakkında ek bilgilere ihtiyacınız varsa, "Sales" `PartitionKey` değerini kullanarak çalışan varlıklarının her birini alın ve adım 2 ' de elde ettiğiniz çalışanların listesinden `RowKey`.  

Seçenek 3: ayrı bir bölümde veya tabloda Dizin varlıkları oluşturma  

Bu seçenek için aşağıdaki verileri depolayan Dizin varlıklarını kullanın:  

![Aynı soyadı taşıyan çalışan kimliklerinin bir listesini içeren bir dize içeren çalışan varlığı gösteren grafik][15]

`EmployeeIDs` özelliği, `RowKey`depolanan son ada sahip çalışanlar için çalışan kimliklerinin bir listesini içerir.  

Dizin varlıkları çalışan varlıklarından ayrı bir bölümde olduğundan tutarlılığı sağlamak için Yumurtları kullanamazsınız. Dizin varlıklarının en sonunda çalışan varlıklarıyla tutarlı olduğundan emin olun.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Bu çözüm, eşleşen varlıkları almak için en az iki sorgu gerektirir: biri, `RowKey` değerlerinin listesini almak için Dizin varlıklarını sorgulamak ve sonra listedeki her varlığı almak için sorgular.  
* Tek bir varlık en fazla 1 MB 'lık bir değer içerdiğinden, çözümde 2. seçenek ve seçenek 3 seçeneği, belirli bir soyadı için çalışan kimlikleri listesinin hiç bir kez 1 MB olduğunu varsayar. Çalışan kimlikleri listesinin boyutu 1 MB 'tan fazla olursa, 1. seçeneği kullanın ve dizin verilerini BLOB depolama alanında depolayın.  
* Seçenek 2 ' yi (çalışanları ekleme ve silme işlemlerini ve bir çalışanın soyadını değiştirmeyi işlemek için Yumurlar kullanarak) kullanırsanız, işlem hacmi belirli bir bölümdeki ölçeklenebilirlik sınırlarına yaklaşıp yaklaşmayacak şekilde değerlendirmeniz gerekir. Bu durumda, sonunda tutarlı bir çözüm (seçenek 1 veya seçenek 3) göz önünde bulundurmanız gerekir. Bu, güncelleştirme isteklerini işlemek için kuyrukları kullanır ve Dizin varlıklarınızı çalışan varlıklarından ayrı bir bölümde depolamanıza olanak tanır.  
* Bu çözümde 2. seçenek, bir departmandaki son ada göre bakmak istediğinizi varsayar. Örneğin, satış departmanında soyadı Jones olan çalışanların bir listesini almak istiyorsunuz. Tüm kuruluş genelinde en son bir ada sahip tüm çalışanları aramak istiyorsanız 1 veya seçenek 3 ' ü kullanın.
* Nihai tutarlılığı sunan kuyruk tabanlı bir çözüm uygulayabilirsiniz. Daha fazla ayrıntı için, [sonuçta tutarlı işlemler düzenine](#eventually-consistent-transactions-pattern)bakın.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Tüm çalışanlar gibi ortak bir özellik değerini paylaşan bir varlık kümesini (can soyadı Jones olan tüm çalışanlar gibi) aramak istediğinizde bu stili kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bileşik anahtar stili](#compound-key-pattern)  
* [Sonuçta tutarlı işlem kriteri](#eventually-consistent-transactions-pattern)  
* [Varlık grubu işlemleri](#entity-group-transactions)  
* [Heterojen varlık türleriyle çalışma](#work-with-heterogeneous-entity-types)  

### <a name="denormalization-pattern"></a>Normalleştirilmişlikten çıkarma deseni
İlgili verileri içeren bir tek nokta sorgu ihtiyacınız olan tüm verileri almanıza olanak sağlamak için tek bir varlık birleştirmek.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
İlişkisel bir veritabanında, sorgular birden çok tablodan veri aldığında oluşan yinelemeyi kaldırmak için genellikle verileri normalleştirin. Verilerinizi Azure tabloları Normalleştir gerekirse, birden çok gidiş dönüş ilgili verilerinizi almak için sunucuya istemciden yapmanız gerekir. Örneğin, aşağıdaki tablo yapısıyla, bir departmanın ayrıntılarını almak için iki gidiş dönüş gerekir. Bir seyahat, yöneticinin KIMLIĞINI içeren departman varlığını getirir ve ikinci seyahat, yöneticinin ayrıntılarını bir çalışan varlığında getirir.  

![Departman varlığı ve çalışan varlığı grafiği][16]

#### <a name="solution"></a>Çözüm
İki ayrı varlık içinde veri depolama, yerine verileri normalleştirilmişlikten çıkarmak ve departman varlıkta manager'ın ayrıntılarını bir kopyasını tutun. Örnek:  

![Yoğun ve birleştirilmiş departman varlığının grafiği][17]

Bu özelliklerle depolanan departman varlıkları sayesinde, bir nokta sorgusu kullanarak bir departmanla ilgili ihtiyaç duyduğunuz tüm ayrıntıları alabilirsiniz.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Bazı verileri iki kez depolama ile ilişkili ek maliyeti yoktur. Tablo depolamaya daha az istek elde eden performans avantajı, genellikle depolama maliyetlerindeki marguinal artışa göre yapılır. Ayrıca, bu maliyet, bir departmanın ayrıntılarını getirmek için ihtiyaç duyduğunuz işlem sayısı azalmasıyla kısmen denkleştirilir.  
* Yöneticileri hakkında bilgi depolamak iki varlık tutarlılığını sürdürmeniz gerekir. Tek bir atomik işlemde birden fazla varlığı güncelleştirmek için yumurtalar kullanarak tutarlılık sorununu işleyebilirsiniz. Bu durumda, departman varlığı ve Departman Yöneticisi için çalışan varlığı aynı bölümde depolanır.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Sık ilgili bilgi aramak gerektiğinde bu düzeni kullanın. Bu düzen gerektiriyorsa verileri almak için istemcinizi yapmalısınız sorguların sayısını azaltır.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bileşik anahtar stili](#compound-key-pattern)  
* [Varlık grubu işlemleri](#entity-group-transactions)  
* [Heterojen varlık türleriyle çalışma](#work-with-heterogeneous-entity-types)

### <a name="compound-key-pattern"></a>Bileşik anahtar deseni
Bir istemcinin tek nokta sorgusuyla ilgili verileri araması için bileşik `RowKey` değerlerini kullanın.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
İlişkisel bir veritabanında, ilgili veri parçalarını tek bir sorgudaki istemciye döndürmek için sorgularda birleştirmeleri kullanmak doğal bir veritabanıdır. Örneğin, ilgili varlıkların bir listesini aramak ve bu çalışana yönelik verileri gözden geçirmek için çalışan KIMLIĞINI kullanabilirsiniz.  

Aşağıdaki yapıyı kullanarak, çalışan varlıklarını tablo depolamada depoladığını varsayın:  

![Çalışan varlığı grafiği][18]

Ayrıca, çalışanın kuruluşunuz için çalıştığı her bir yıla ait incelemeler ve performansla ilgili geçmiş verileri depolamanız ve bu bilgilere yıla göre erişebilmek için ihtiyacınız vardır. Aşağıdaki yapıya sahip varlıklar depolayan başka bir tablo oluşturma tek seçenektir:  

![Çalışan gözden geçirme varlığı grafiği][19]

Bu yaklaşımda, verileri tek bir istekle almanızı sağlamak için yeni varlıktaki bazı bilgileri (örneğin, ilk adı ve soyadı) çoğaltmaya karar verebilirsiniz. Ancak, bu iki varlığı otomatik olarak güncelleştirmek için EGT 'yi kullanamadığından güçlü tutarlılığı koruyamıyorum.  

#### <a name="solution"></a>Çözüm
Aşağıdaki yapıyla varlıkları kullanarak özgün tablonuzda yeni bir varlık türü depolayın:  

![Bileşik anahtarla çalışan varlığının grafiği][20]

`RowKey` şimdi bir bileşik anahtar, çalışan KIMLIĞI ve gözden geçirme verilerinin yılından oluşan yıldan oluşan fark. Bu, çalışanın performansını almanızı ve tek bir varlık için tek bir istekle verileri incelemenizi sağlar.  

Aşağıdaki örnek nasıl (örneğin, satış departmanında çalışan 000123) belirli bir çalışan için tüm gözden geçirme verilerini alabilirsiniz özetlenmektedir:  

$filter = (PartitionKey eq 'Satış') ve (RowKey ge 'empid_000123') ve (RowKey lt 'empid_000124') & $select RowKey, Manager değerlendirme, eş derecelendirme, açıklama =  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* `RowKey` değerini ayrıştırmayı kolaylaştıran uygun bir ayırıcı karakter kullanmanız gerekir: Örneğin, **000123_2012**.  
* Ayrıca, bu varlığı aynı çalışanla ilgili verileri içeren diğer varlıklarla aynı bölümde depoluyorsunuz. Bu, güçlü tutarlılığı sürdürmek için Yumurtları kullanabileceğiniz anlamına gelir.
* Bu düzenin uygun olup olmadığını anlamak için verileri ne sıklıkta sorgulayacağınızı düşünmeniz gerekir. Örneğin, İnceleme verilerine sık sık ve ana çalışan verileri sıklıkla eriştiğinizde, onları ayrı varlıklar olarak saklamanız gerekir.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Bu düzen, bir saklamanız gerekir veya ilgili daha fazla varlık sorguladığınız sık kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Varlık grubu işlemleri](#entity-group-transactions)  
* [Heterojen varlık türleriyle çalışma](#work-with-heterogeneous-entity-types)  
* [Sonuçta tutarlı işlem kriteri](#eventually-consistent-transactions-pattern)  

### <a name="log-tail-pattern"></a>Günlük kuyruğu deseni
Ters tarih ve saat düzeninde sıralama yapan bir `RowKey` değeri kullanarak bir bölüme en son eklenen *n* varlıklarını alın.  

> [!NOTE]
> Azure Cosmos DB Azure Tablo API'si tarafından döndürülen sorgu sonuçları bölüm anahtarına veya satır anahtarına göre sıralanmaz. Bu nedenle, bu model tablo depolaması için uygun olsa da Azure Cosmos DB için uygun değildir. Özellik farklarının ayrıntılı bir listesi için, [Azure Cosmos DB ve Azure Tablo depolamadaki tablo API'si arasındaki farklara](faq.md#where-is-table-api-not-identical-with-azure-table-storage-behavior)bakın.

#### <a name="context-and-problem"></a>Bağlam ve sorun
Sık karşılaşılan bir gereksinimdir en son oluşturulan varlıkları almak için örneğin on en son bir çalışan tarafından gönderilen talepleri gider. Tablo sorguları bir kümeden ilk *n* varlığı döndürmek için `$top` bir sorgu işlemini destekler. Bir küme içindeki son *n* varlığı döndürmek için eşdeğer bir sorgu işlemi yoktur.  

#### <a name="solution"></a>Çözüm
Bir `RowKey` kullanarak varlıkları, ters tarih/saat düzeninde sıralayarak, en son girişin her zaman tablodaki ilk bir değer olan bir kullanarak saklayın.  

Örneğin, bir çalışan tarafından gönderilen en son on gider talep alabilmesi için geçerli tarih/saat türetilmiş bir ters değer çizgisi değeri kullanabilirsiniz. Aşağıdaki C# kod örneğinde, en sonuncudan en eskiye doğru sıralama yapan bir `RowKey` için uygun bir "ters kullanım" değeri oluşturmanın bir yolu gösterilmektedir:  

`string invertedTicks = string.Format("{0:D19}", DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks);`  

Aşağıdaki kodu kullanarak tarih/saat değerine geri dönebilirsiniz:  

`DateTime dt = new DateTime(DateTime.MaxValue.Ticks - Int64.Parse(invertedTicks));`  

Tablo sorgusu şöyle görünür:  

`https://myaccount.table.core.windows.net/EmployeeExpense(PartitionKey='empid')?$top=10`  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Dize değerinin beklendiği gibi sıralandığından emin olmak için ters değer değerini önünde sıfır ile birlikte ayarlamalısınız.  
* Bir bölüm düzeyinde ölçeklenebilirlik hedefleri farkında olması gerekir. Etkin nokta bölümleri oluşturmamaya dikkat edin.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Varlıklara ters tarih/saat düzeninde erişmeniz gerektiğinde veya en son eklenen varlıklara erişmeniz gerektiğinde bu düzeni kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Prepend/Append kenar yumuşatma](#prepend-append-anti-pattern)  
* [Varlıkları al](#retrieve-entities)  

### <a name="high-volume-delete-pattern"></a>Yüksek hacimli silme deseni
Aynı anda tüm varlıkları kendi ayrı tablolarındaki eşzamanlı silme için depolayarak yüksek hacimli varlıkların silinmesini etkinleştirin. Tabloyu silerek varlıkları silersiniz.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Birçok uygulama, artık bir istemci uygulaması için kullanılabilir olması gerekir veya uygulamayı başka bir depolama ortamına arşivlenmiş eski verileri silin. Genellikle bu verileri bir tarihle belirlersiniz. Örneğin, 60 günden daha eski olan tüm oturum açma isteklerinin kayıtlarını silme gereksinimleriniz vardır.  

Olası bir tasarım, `RowKey`oturum açma isteğinin tarih ve saatini kullanmaktır:  

![Oturum açma denemesi varlığı grafiği][21]

Bu yaklaşım bölüm etkin noktalarını önler çünkü uygulama, her kullanıcı için ayrı bir bölümdeki oturum açma varlıklarını ekleyebilir ve silebilir. Ancak, çok sayıda varlık varsa bu yaklaşım maliyetli ve zaman alıcı olabilir. Öncelikle, silinecek tüm varlıkların tanımlanması için bir tablo taraması gerçekleştirmeniz ve ardından bir eski varlığı silmeniz gerekir. EGTs birden fazla delete isteğinin toplu işleme eski varlıkları silmek için gerekli sunucuya gidiş dönüş sayısını azaltabilir.  

#### <a name="solution"></a>Çözüm
Oturum açma denemesi her günü için ayrı bir tabloda kullanın. Varlıkları eklerken etkin noktaları önlemek için önceki varlık tasarımını kullanabilirsiniz. Eski varlıkların silinmesi, her gün yüzlerce ve binlerce ayrı oturum açma varlığını bulmak ve silmek yerine yalnızca bir tabloyu her gün (tek bir depolama işlemi) silme sorularından biridir.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tasarımınız, uygulamanızın belirli varlıkları aramak, diğer verilerle bağlantı kurmak veya toplu bilgi oluşturmak gibi verileri kullanacağı diğer yolları destekliyor mu?  
* Yeni varlıklar eklerken tasarımınızı etkin noktalardan kaçınacak mu?  
* Aynı tablo adı sildikten sonra yeniden kullanmak isterseniz gecikme bekler. Her zaman benzersiz tablo adları kullanılması daha iyidir.  
* Yeni bir tabloyu ilk kez kullandığınızda, tablo depolaması erişim düzenlerini öğrenirken ve bölümleri düğümler arasında dağıttığı sırada bazı hız sınırlaması beklenir. Sıklıkla yeni tablolar oluşturmak için ihtiyacınız düşünmelisiniz.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Aynı anda silmelisiniz varlıkları yüksek hacimli varsa bu düzeni kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Varlık grubu işlemleri](#entity-group-transactions)
* [Varlıkları değiştirme](#modify-entities)  

### <a name="data-series-pattern"></a>Veri serisi deseni
Tam veri serisinde yaptığınız istek sayısını en aza indirmek için tek bir varlık Store.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Bir dizi genellikle aynı anda almak için gereken verileri depolamak bir uygulama için bir yaygın senaryodur. Örneğin, uygulamanız her çalışan her saat gönderir anlık ileti sayısını kaydedin ve ardından bu bilgileri ileti sayısını çizmek için önceki 24 saat içinde gönderilen her bir kullanıcı kullanın. Her bir çalışanın 24 varlıkları depolamak için bir tasarım olabilir:  

![İleti istatistikleri varlığının grafiği][22]

Bu tasarım ile kolayca bulun ve uygulamanın ileti sayısı değerini güncelleştirmek gerektiğinde her bir çalışan için güncelleştirilecek varlık güncelleştirin. Ancak, önceki 24 saat için bir etkinlik grafiğini çizmek için bilgi almak için 24 varlıkları almanız gerekir.  

#### <a name="solution"></a>Çözüm
Her saat için ileti sayısını depolamak üzere ayrı bir özellikle aşağıdaki tasarımı kullanın:  

![Ayrılmış özelliklerle ileti istatistikleri varlığını gösteren grafik][23]

Bu tasarımla, bir çalışanın ileti sayısı için belirli bir saat güncelleştirmek için bir birleştirme işlemi kullanabilirsiniz. Şimdi, tek bir varlık için bir istek kullanarak grafiği çizmek için ihtiyacınız olan tüm bilgileri alabilirsiniz.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tüm veri seriniz tek bir varlığa sığmıyorsa (bir varlık en fazla 252 özellik içerebilir), blob gibi alternatif bir veri deposu kullanın.  
* Bir varlığı aynı anda güncelleştiren birden fazla istemciniz varsa, iyimser eşzamanlılık uygulamak için **ETag** 'i kullanın. Çok sayıda istemciniz varsa, yüksek çekişme yaşayabilirsiniz.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Güncelleştirme ve tek bir varlık ile ilişkili bir veri serisi almak gerektiğinde bu düzeni kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Büyük varlıklar kalıbı](#large-entities-pattern)  
* [Birleştir veya Değiştir](#merge-or-replace)  
* [Sonuçta tutarlı işlemler deseninin](#eventually-consistent-transactions-pattern) (veri serisini bir blob 'da depoluyorsanız)  

### <a name="wide-entities-pattern"></a>Geniş bir varlıklar deseni
Birden çok 252 özellik ile mantıksal varlıkların depolamak için birden çok fiziksel varlıkları kullanın.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tek bir varlık 252 'den fazla özelliğe sahip olabilir (zorunlu sistem özellikleri hariç) ve toplamda 1 MB 'den fazla veri depolayamıyorum. İlişkisel bir veritabanında, genellikle yeni bir tablo ekleyerek ve aralarında 1 ' den fazla ilişki zorunlu tutarak bir satırın boyutuyla ilgili her türlü sınırı geçici olarak çözebilirsiniz.  

#### <a name="solution"></a>Çözüm
Tablo Depolamayı kullanarak, birden fazla varlığı, 252 'den fazla özelliği olan tek bir büyük ölçekli bir iş nesnesini temsil etmek üzere saklayabilirsiniz. Örneğin, son 365 gün boyunca her bir çalışan tarafından gönderilen anlık ileti iletilerinin sayısını depolamak istiyorsanız, farklı şemalarla iki varlık kullanan aşağıdaki tasarımı kullanabilirsiniz:  

![Rowkey 01 ve Rowkey 02 ile ileti istatistikleri varlığı ile ileti istatistikleri varlığını gösteren grafik][24]

Bunları birbirleri ile eşitlenmiş tutmak için iki varlıkları güncelleştirme gerektiren bir değişiklik yapmanız gerekirse, bir EGT kullanabilirsiniz. Aksi takdirde, ileti sayısı için belirli bir günde güncelleştirileceği tek birleştirme işlemi kullanabilirsiniz. Tek bir çalışana ait tüm verileri almak için her iki varlığı de almalısınız. Bunu hem `PartitionKey` hem de `RowKey` değeri kullanan iki verimli istek ile yapabilirsiniz.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağını saptarken aşağıdaki noktayı göz önünde bulundurun:  

* Eksiksiz bir mantıksal varlık alma, en az iki depolama işlemleri kapsar: biri her fiziksel varlığını almak için.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Boyutu veya özellikleri olan varlıkları, tablo depolamadaki tek bir varlığın sınırlarını aşan varlıkları depolamanız gerektiğinde bu stili kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Varlık grubu işlemleri](#entity-group-transactions)
* [Birleştir veya Değiştir](#merge-or-replace)

### <a name="large-entities-pattern"></a>Büyük varlıklar deseni
Büyük özellik değerlerini depolamak için BLOB depolama kullanın.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Tek bir varlık, toplamda 1 MB 'tan fazla veri depolayamıyorum. Özelliklerden biri veya birkaçı, varlığınızın toplam boyutuna neden olan değerleri bu değere aşarsa, tüm varlığı tablo depolamadaki depokaydedemezsiniz.  

#### <a name="solution"></a>Çözüm
Bir veya daha fazla özellik büyük miktarda veri içerdiği için varlığınız 1 MB 'ı aşarsa, verileri BLOB depolama alanında saklayabilir ve sonra Blobun adresini varlıktaki bir özellikte saklayabilirsiniz. Örneğin, bir çalışanın fotoğrafını BLOB depolama alanında saklayabilir ve çalışan varlığınızın `Photo` özelliğinde fotoğrafın bağlantısını kaydedebilirsiniz:  

![Blob depolamaya işaret eden fotoğrafın bulunduğu çalışan varlığı gösteren grafik][25]

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Tablo depolamadaki varlık ve BLOB depolamada bulunan veriler arasında nihai tutarlılığı sürdürmek için, varlıklarınızı sürdürmek üzere [sonuçta tutarlı işlem düzenlerini](#eventually-consistent-transactions-pattern) kullanın.
* Eksiksiz bir varlık alma, en az iki depolama işlemleri kapsar: bir varlık ve blob verilerini almak için alınacak.  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Boyutu, tablo depolamadaki tek bir varlık için olan sınırları aşan varlıkları depolamanız gerektiğinde bu stili kullanın.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Sonuçta tutarlı işlem kriteri](#eventually-consistent-transactions-pattern)  
* [Geniş varlıklar kalıbı](#wide-entities-pattern)

<a name="prepend-append-anti-pattern"></a>

### <a name="prependappend-anti-pattern"></a>Koruma düzeni önüne ekleyin ve ekleme
Çok sayıda araya sahip olduğunuzda, eklemeleri birden fazla bölüme yayarak ölçeklenebilirliği artırın.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Bölümler bir dizi ilk veya son bölümü için yeni varlıklar ekleme uygulama eklenmesini ya da saklı varlıklarınızı varlıklar ekleme, genellikle sonuçlanır. Bu durumda, belirli bir zamanda tüm ekler aynı bölümde gerçekleşirken bir etkin nokta oluşturulur. Bu, tablo depolamanın yük dengelemeden birden çok düğüm arasında yer almasını önler ve büyük olasılıkla uygulamanızın bölüm için ölçeklenebilirlik hedeflerine ulaşmasına neden olur. Örneğin, çalışanlar tarafından ağ ve kaynak erişimini kaydeden bir uygulamanın durumunu göz önünde bulundurun. Aşağıdaki gibi bir varlık yapısı, işlem hacmi tek bir bölüm için ölçeklenebilirlik hedefine ulaşırsa, geçerli saatin bölümünün bir etkin noktaya dönüşmesine neden olabilir:  

![Çalışan varlığı grafiği][26]

#### <a name="solution"></a>Çözüm
Aşağıdaki alternatif varlık yapısı, uygulama olayları günlüğe kaydettiği için belirli bir bölümdeki etkin noktayı önler:  

![Yıl, ay, gün, saat ve olay KIMLIĞINI temel alan RowKey ile çalışan varlığı gösteren grafik][27]

Bu örneğe, hem `PartitionKey` hem de `RowKey` nasıl bileşik anahtar olduğuna dikkat edin. `PartitionKey`, günlüğü birden çok bölüme dağıtmak için hem departmanı hem de çalışan KIMLIĞINI kullanır.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Bu düzenin nasıl uygulanacağına karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Etkin bölümler ekler üzerinde verimli bir şekilde oluşturulmasını engeller alternatif anahtar yapısı, istemci uygulamanın yaptığı sorguları destekliyor mu?  
* Beklenen işlem hacmi, tek bir bölüm için ölçeklenebilirlik hedeflerine ulaşmanıza ve tablo depolaması ile kısıtlamanıza izin veriyor mu?  

#### <a name="when-to-use-this-pattern"></a>Bu düzenin kullanılacağı durumlar
Bir sıcak bölüme eriştiğinizde, işlem hackleriniz, tablo depolamaya göre sınırlama oranı ile sonuçlanırken, ön uç/ekleme önleme deseninin önüne kaçının.  

#### <a name="related-patterns-and-guidance"></a>İlgili düzenler ve kılavuzlar
Bu düzen uygulanırken aşağıdaki düzenler ve yönergeler de yararlı olabilir:  

* [Bileşik anahtar stili](#compound-key-pattern)  
* [Günlük kuyruk kalıbı](#log-tail-pattern)  
* [Varlıkları değiştirme](#modify-entities)  

### <a name="log-data-anti-pattern"></a>Günlük veri koruma deseni
Genellikle, günlük verilerini depolamak için tablo depolaması yerine BLOB depolama kullanmanız gerekir.  

#### <a name="context-and-problem"></a>Bağlam ve sorun
Günlük verileri için genel kullanım örneği, belirli bir tarih/saat aralığı için günlük girişlerinin bir seçimini almadır. Örneğin, uygulamanızın 15:04 ile 15:06 arasında günlüğe kaydedildiği tüm hata ve kritik iletileri belirli bir tarihte bulmak istiyorsunuz. Günlük varlıklarının kaydedileceği bölümü öğrenmek için günlük iletisinin tarih ve saatini kullanmak istemezsiniz. Bu, belirli bir zamanda, tüm günlük varlıklarının aynı `PartitionKey` değerini paylaştığından, bu durum, etkin bir bölüme neden olur. [](#prepend-append-anti-pattern) Örneğin, bir günlük iletisi için aşağıdaki varlık şeması, etkin bir bölüme neden olur, çünkü uygulama tüm günlük iletilerini geçerli tarih ve saat için bölüme Yazar:  

![Günlük iletisi varlığının grafiği][28]

Bu örnekte `RowKey`, günlük iletilerinin tarih/saat düzeninde sıralanmasını sağlamak üzere günlük iletisinin tarih ve saatini içerir. `RowKey`, birden çok günlük iletisinin aynı tarih ve saati paylaştığı durumlarda bir ileti KIMLIĞI de içerir.  

Başka bir yaklaşım, uygulamanın iletileri bir dizi bölüme yazmalarını sağlayan `PartitionKey` kullanmaktır. Örneğin, günlük iletisinin kaynağı birçok bölüm arasında ileti dağıtmak için bir yol sağlıyorsa aşağıdaki varlık şemasını kullanabilirsiniz:  

![Günlük iletisi varlığının grafiği][29]

Bununla birlikte, bu şemayla ilgili sorun belirli bir zaman aralığı için tüm günlük iletilerini almak için tablodaki her bölümde arama yapmanız gerekir.

#### <a name="solution"></a>Çözüm
Önceki bölümde, günlük girişlerini depolamak için Tablo Depolamayı kullanma girişimi ve önerilen iki yetersiz tasarım sorunu vurgulanmıştır. Sık erişimli bir bölüme yapılan bir çözüm, günlük iletilerinin düşük performans yazma riskiyle birlikte çalışır. Diğer çözüm, belirli bir zaman aralığı için günlük iletilerini almak üzere tablodaki her bölümü taramak için gerekli olan sorgu performansının düşmesine neden oldu. BLOB depolama, bu tür bir senaryo için daha iyi bir çözüm sunar ve Azure Storage Analytics 'in topladığı günlük verilerini depoladığı bir çözümdür.  

Bu bölümde, genellikle aralığa göre Sorgulayabileceğiniz verileri depolamada bu yaklaşımın bir açıklaması olarak depolama analizinin, blob depolamada günlük verilerini nasıl depoladığı özetlenmektedir.  

Depolama analizi, günlük iletilerini birden fazla blobda ayrılmış bir biçimde depolar. Ayrılmış biçimi için günlük verileri ayrıştırmak bir istemci uygulamasını kolaylaştırır.  

Depolama analizi, arama yaptığınız günlük iletilerini içeren Blobu (veya Blobları) bulmanızı sağlayan Bloblar için bir adlandırma kuralı kullanır. Örneğin, "Queue/2014/07/31/1800/000001. log" adlı bir blob, 31 Temmuz 2014 tarihinde 18:00 ' de başlayan saat için kuyruk hizmetiyle ilgili günlük iletileri içerir. "000001", bu ilk günlük dosyası bu döneme ait olduğunu gösterir. Depolama Analizi Ayrıca, blob 'un meta verilerinin bir parçası olarak dosyada depolanan ilk ve son günlük iletilerinin zaman damgalarını kaydeder. BLOB depolama için API, bir ad önekini temel alarak bir kapsayıcıdaki Blobları bulmanıza izin verebilir. 18:00 ile başlayan saat için kuyruk günlüğü verilerini içeren tüm Blobları bulmak için, "Queue/2014/07/31/1800" önekini kullanabilirsiniz.  

Depolama Analizi günlük iletilerini dahili olarak arabelleğe alır ve ilgili blobu düzenli aralıklarla güncelleştirir veya en son günlük girişi toplu işlem ile yeni bir tane oluşturur. Bu, BLOB depolama için gerçekleştirmesi gereken yazma sayısını azaltır.  

Kendi uygulamanızda benzer bir çözüm gerçekleştiriyorsanız, güvenilirliği ve maliyet ve ölçeklenebilirlik arasında dengelemeyi yönetmeyi göz önünde bulundurun. Diğer bir deyişle, uygulamanızdaki güncelleştirmeleri arabelleğe alma ve bunları toplu halde blob depolamaya yazma karşılaştırması ile karşılaştırıldığında, her günlük girişini blob depolamaya yazma etkisini değerlendirin.  

#### <a name="issues-and-considerations"></a>Sorunlar ve dikkat edilmesi gerekenler
Günlük verilerini depolamak nasıl karar verirken aşağıdaki noktaları göz önünde bulundurun:  

* Olası sık kullanılan bölümleri engelleyen bir tablo tasarımı oluşturursanız, günlük verilerinize verimli bir şekilde erişemeyebilirsiniz.  
* Günlük verileri işlemek için bir istemci genellikle çok sayıda kayıt yüklemesi gerekir.  
* Günlük verileri genellikle yapılandırılmış olsa da, BLOB depolama daha iyi bir çözüm olabilir.  

### <a name="implementation-considerations"></a>Uygulama konuları
Bu bölümde önceki bölümlerde açıklanan desenleri uygularken göz önünde işlenmesi için dikkat edilmesi gerekenler bazıları açıklanmaktadır. Bu bölümde çoğu C# dilinde yazılmış olan ve depolama istemci Kitaplığı'nı (sürüm 4.3.0 zaman yazma) kullanan örnekler kullanır.  

### <a name="retrieve-entities"></a>Varlıkları al
[Sorgulama için bölüm tasarımında](#design-for-querying)anlatıldığı gibi, en verimli sorgu bir nokta sorgusudur. Ancak, bazı senaryolarda birden fazla varlık almanız gerekebilir. Bu bölümde, depolama Istemci kitaplığı kullanılarak varlıkların alınması için bazı yaygın yaklaşımlar açıklanmaktadır.  

#### <a name="run-a-point-query-by-using-the-storage-client-library"></a>Depolama Istemci kitaplığını kullanarak bir nokta sorgusu çalıştırma
Nokta sorgusu çalıştırmanın en kolay yolu, tablo **Al** işlemini kullanmaktır. Aşağıdaki C# kod parçacığında gösterildiği gibi, bu Işlem "Sales" değeri `PartitionKey` bir varlık ve "212" değerinin bir `RowKey` alır:  

```csharp
TableOperation retrieveOperation = TableOperation.Retrieve<EmployeeEntity>("Sales", "212");
var retrieveResult = employeeTable.Execute(retrieveOperation);
if (retrieveResult.Result != null)
{
    EmployeeEntity employee = (EmployeeEntity)retrieveResult.Result;
    ...
}  
```

Bu örneğin, aldığı varlığın `EmployeeEntity`türünde olmasını beklediğine dikkat edin.  

#### <a name="retrieve-multiple-entities-by-using-linq"></a>LINQ kullanarak birden çok varlık alma
Depolama Istemci kitaplığı ile LINQ kullanarak birden çok varlık alabilir ve **WHERE** yan tümcesi içeren bir sorgu belirtebilirsiniz. Tablo taramasının önüne geçmek için, WHERE yan tümcesine `PartitionKey` değeri her zaman dahil etmeniz ve tablo ve bölüm taramalarından kaçınmak için `RowKey` değeri olması gerekir. Tablo depolama, WHERE yan tümcesinde kullanılmak üzere sınırlı sayıda karşılaştırma işleci (büyüktür, büyüktür veya eşittir, küçüktür, küçüktür veya eşittir, eşittir ve eşit değildir) kümesini destekler. Aşağıdaki C# kod parçacığı, son adı "B" ile başlayan tüm çalışanları bulur (`RowKey`, soyadı, Bölüm adını depoladığını `PartitionKey` varsayarak).  

```csharp
TableQuery<EmployeeEntity> employeeQuery = employeeTable.CreateQuery<EmployeeEntity>();
var query = (from employee in employeeQuery
            where employee.PartitionKey == "Sales" &&
            employee.RowKey.CompareTo("B") >= 0 &&
            employee.RowKey.CompareTo("C") < 0
            select employee).AsTableQuery();
var employees = query.Execute();  
```

Daha iyi performans sağlamak için sorgunun hem `RowKey` hem de bir `PartitionKey` nasıl belirtildiğine dikkat edin.  

Aşağıdaki kod örneği, Fluent API kullanarak eşdeğer işlevselliği gösterir (genel olarak akıcı API 'Ler hakkında daha fazla bilgi için bkz. [Fluent API tasarlamaya yönelik en iyi uygulamalar](https://visualstudiomagazine.com/articles/2013/12/01/best-practices-for-designing-a-fluent-api.aspx)):  

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
> Örnek, üç filtre koşullarını dahil etmek için birden çok `CombineFilters` yöntemi sağlar.  
> 
> 

#### <a name="retrieve-large-numbers-of-entities-from-a-query"></a>Bir sorgudan çok sayıda varlık alma
En iyi sorgu, bir `PartitionKey` değerine ve `RowKey` değerine göre tek bir varlık döndürür. Bununla birlikte, bazı senaryolarda aynı bölümden veya birçok bölümden birçok varlık döndürme gereksinimine sahip olabilirsiniz. Böyle senaryolarda, uygulamanızın performansını her zaman tam olarak test etmeniz gerekir.  

Tablo depolamaya yönelik bir sorgu, tek seferde en fazla 1.000 varlık döndürebilir ve en fazla beş saniye çalışabilir. Tablo depolama, aşağıdakilerden herhangi biri doğruysa istemci uygulamanın sonraki varlık kümesini istemesini sağlamak için bir devamlılık belirteci döndürür:

- Sonuç kümesi 1.000 'den fazla varlık içeriyor.
- Sorgu beş saniye içinde tamamlanmadı.
- Sorgu Bölüm sınırını aşar. 

Devamlılık belirteçlerinin nasıl çalıştığı hakkında daha fazla bilgi için bkz. [sorgu zaman aşımı ve sayfalandırma](https://msdn.microsoft.com/library/azure/dd135718.aspx).  

Depolama Istemci kitaplığını kullanıyorsanız, tablo depolamadan varlık döndürdüğünden, devamlılık belirteçlerini otomatik olarak işleyebilir. Örneğin, aşağıdaki C# kod örneği, tablo depolaması bunları bir yanıtta döndürürse, devamlılık belirteçlerini otomatik olarak işler:  

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

Aşağıdaki C# kodu açıkça devamlılık belirteçlerini işler:  

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

Açıkça devamlılık belirteçleri kullanarak, ne zaman uygulamanızı veri sonraki segmentini alır kontrol edebilirsiniz. Örneğin, istemci uygulamanız kullanıcıların bir tabloda depolanan varlıklar aracılığıyla sayfa oluşturmasını sağladığından, bir Kullanıcı sorgu tarafından alınan tüm varlıklarda sayfa kullanmamaya karar verebilir. Uygulamanız yalnızca bir devamlılık belirteci kullanarak Kullanıcı, geçerli kesimdeki tüm varlıklar aracılığıyla disk belleğini bitirdiğinde bir sonraki segmenti alır. Bu yaklaşım, birçok faydası vardır:  

* Tablo depolamadan alınacak veri miktarını sınırlayabilir ve ağ üzerinden geçiş yapabilirsiniz.  
* .NET 'te zaman uyumsuz g/ç gerçekleştirebilirsiniz.  
* Devamlılık belirtecini kalıcı depolama alanına seri hale getirebilirsiniz, böylece uygulama kilitlenmesi durumunda devam edebilirsiniz.  

> [!NOTE]
> Devamlılık belirteci genellikle 1.000 varlık içeren bir segmenti döndürür, ancak daha az olabilir. Bu durum, bir sorgunun döndürdüğü giriş sayısını, arama ölçütlerinizle eşleşen ilk n varlığı döndürmek için **Al** ' ı kullanarak sınırlandırdıysanız de olur. Tablo depolama, kalan varlıkları almanızı sağlamak için en az n varlık içeren bir segment döndürebilir.  
> 
> 

Aşağıdaki C# kod bir segment döndürülen varlık sayısını değiştirmek nasıl gösterir:  

```csharp
employeeQuery.TakeCount = 50;  
```

#### <a name="server-side-projection"></a>Sunucu tarafı projeksiyonu
Tek bir varlık, en fazla 255 özelliklere sahip ve en çok 1 MB boyutunda olmalıdır. Tabloyu sorgulayıp varlıkları aldığınızda, tüm özelliklere ihtiyaç duymayabilir ve verilerin gereksiz yere aktarılmasını önleyebilirsiniz (gecikme süresini ve maliyeti azaltmaya yardımcı olmak için). Sunucu tarafı projeksiyonu yalnızca gereksinim duyduğunuz özellikleri aktarmak için kullanabilirsiniz. Aşağıdaki örnek, sorgu tarafından seçilen varlıklardan yalnızca `Email` özelliğini (`PartitionKey`, `RowKey`, `Timestamp`ve `ETag`ile birlikte) alır.  

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

Alınacak özellikler listesine dahil edilmese de `RowKey` değerinin nasıl kullanılabilir olduğunu unutmayın.  

### <a name="modify-entities"></a>Varlıkları değiştirme
Depolama Istemci kitaplığı, varlıkları ekleyerek, silerek ve güncelleştirerek tablo depolamada depolanan varlıklarınızı değiştirmenize olanak sağlar. Gerekli gidiş dönüş sayısını azaltmak ve çözümünüzün performansını artırmak için, birden çok ekleme, güncelleştirme ve silme işlemini birlikte toplu olarak toplu olarak kullanabilirsiniz.  

Depolama Istemci kitaplığı bir EGT çalıştırdığında oluşturulan özel durumlar genellikle toplu işin başarısız olmasına neden olan varlığın dizinini içerir. EGTs kullanan kod hata ayıklaması yapıyorsanız, bu yararlıdır.  

Ayrıca, istemci uygulamanızın eşzamanlılık ve güncelleştirme işlemlerinin nasıl işlediğini tasarımınıza nasıl etkilediğini dikkate almalısınız.  

#### <a name="managing-concurrency"></a>Eşzamanlılığı yönetme
Varsayılan olarak, tablo depolama alanı ekleme, birleştirme ve silme işlemleri için tek tek varlıkların düzeyindeki iyimser eşzamanlılık denetimleri uygular, ancak bir istemcinin tablo depolama alanını bu denetimleri atlayacak şekilde zorlaması mümkündür. Daha fazla bilgi için bkz. [Microsoft Azure depolama eşzamanlılık yönetimi](../storage/common/storage-concurrency.md).  

#### <a name="merge-or-replace"></a>Birleştirme veya değiştirme
`TableOperation` sınıfının `Replace` yöntemi, tablo depolamadaki tüm varlığın her zaman yerini alır. Saklı varlıkta bu özellik varsa istek içine bir özellik eklemezseniz, istek bu özelliği saklı varlıktan kaldırır. Bir özelliği açıkça depolanmış bir varlıktan kaldırmak istemediğiniz sürece, istekte her bir özellik içermelidir.  

Bir varlığı güncelleştirmek istediğinizde tablo depolamaya gönderilen veri miktarını azaltmak için `TableOperation` sınıfının `Merge` yöntemini kullanabilirsiniz. `Merge` yöntemi, saklı varlıktaki tüm özellikleri, istekte yer alan varlıktaki özellik değerleriyle değiştirir. Bu yöntem, saklı varlıktaki istekte bulunmayan tüm özellikleri bozulmadan bırakır. Bu, büyük varlıklarınız varsa ve yalnızca bir istekteki az sayıda özelliği güncelleştirmeniz gerekiyorsa yararlıdır.  

> [!NOTE]
> Varlık yoksa `*Replace` ve `Merge` yöntemleri başarısız olur. Alternatif olarak, mevcut değilse yeni bir varlık oluşturan `InsertOrReplace` ve `InsertOrMerge` yöntemlerini kullanabilirsiniz.  
> 
> 

### <a name="work-with-heterogeneous-entity-types"></a>Heterojen varlık türleriyle çalışma
Tablo depolama, *şema için daha az* tablo deposudur. Bu, tek bir tablonun birden çok türden varlıkları depolayabileceği ve tasarımınızda harika esneklik sağladığı anlamına gelir. Aşağıdaki örnek, hem çalışan hem de bölüm varlıkları depolamak için bir tablo gösterir:  

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
<th>EmployeeCount</th>
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

Her varlık hala `PartitionKey`, `RowKey`ve `Timestamp` değerlerine sahip olmalıdır, ancak herhangi bir özellik kümesine sahip olabilir. Ayrıca, bu bilgileri bir yere depolamayı seçmediğiniz müddetçe bir varlığın türünü belirten bir şey yoktur. Varlık türü tanımlamak için iki seçenek vardır:  

* Varlık türünü `RowKey` (veya büyük olasılıkla `PartitionKey`) önüne ekleyin. Örneğin, `RowKey` değerler olarak `EMPLOYEE_000123` veya `DEPARTMENT_SALES`.  
* Aşağıdaki tabloda gösterildiği gibi, varlık türünü kaydetmek için ayrı bir özellik kullanın.  

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
<th>entityType</th>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Çalışan</td>
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
<th>entityType</th>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Çalışan</td>
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
<th>entityType</th>
<th>DepartmentName</th>
<th>EmployeeCount</th>
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
<th>entityType</th>
<th>FirstName</th>
<th>LastName</th>
<th>Yaş</th>
<th>Email</th>
</tr>
<tr>
<td>Çalışan</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

Varlık türü `RowKey`ön bekleyen ilk seçenek, farklı türlerde iki varlığın aynı anahtar değerine sahip olabileceği bir olasılık olduğunda yararlıdır. Ayrıca, aynı türde birlikte bölümündeki varlıkları gruplandırır.  

Bu bölümde ele alınan teknikler özellikle[Devralma ilişkileri](#inheritance-relationships)hakkındaki tartışmayla ilgilidir.  

> [!NOTE]
> İstemci uygulamalarının POCO nesnelerini ve farklı sürümlerle çalışmasını sağlamak için varlık türü değerine bir sürüm numarası dahil etmeyi göz önünde bulundurun.  
> 
> 

Bu bölümün geri kalanında bazı depolama istemci Kitaplığı'nda aynı tabloda birden fazla varlık türleri ile çalışmayı kolaylaştırmak özelliklerini açıklar.  

#### <a name="retrieve-heterogeneous-entity-types"></a>Heterojen varlık türlerini al
Depolama Istemci kitaplığını kullanıyorsanız, birden fazla varlık türüyle çalışmak için üç seçeneğiniz vardır.  

Belirli `RowKey` ve `PartitionKey` değerleriyle depolanan varlık türünü biliyorsanız, varlığı aldığınızda varlık türünü belirtebilirsiniz. Bunu, `EmployeeEntity`türündeki varlıkları alan önceki iki örnekte gördünüz: [depolama Istemci kitaplığını kullanarak bir nokta sorgusu çalıştırma](#run-a-point-query-by-using-the-storage-client-library) ve [LINQ kullanarak birden çok varlık alma](#retrieve-multiple-entities-by-using-linq).  

İkinci seçenek, somut POCO varlık türü yerine `DynamicTableEntity` türünü (bir özellik paketi) kullanmaktır. Bu seçenek, varlığı .NET türlerine serileştirmek ve serisini kaldırmak zorunda olmadığından performansı da iyileştirebilir. Aşağıdaki C# kod, tablodaki farklı türlerin birden çok varlığını potansiyel olarak alır, ancak tüm varlıkları `DynamicTableEntity` örnekleri olarak döndürür. Ardından, her bir varlığın türünü öğrenmek için `EntityType` özelliğini kullanır:  

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

Diğer özellikleri almak için, `DynamicTableEntity` sınıfının `Properties` özelliğinde `TryGetValue` yöntemini kullanmanız gerekir.  

Üçüncü bir seçenek `DynamicTableEntity` türünü ve bir `EntityResolver` örneğini kullanarak birleştirme işlemi kullanmaktır. Bu, aynı sorguda birden çok POCO türleri çözümlemeye sağlar. Bu örnekte `EntityResolver` temsilcisi, sorgunun döndürdüğü iki varlık türü arasında ayrım yapmak için `EntityType` özelliğini kullanıyor. `Resolve` yöntemi, `TableEntity` örneklerine `DynamicTableEntity` örnekleri çözümlemek için `resolver` temsilcisini kullanır.  

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
Silmek için bir varlığın türünü bilmeniz gerekmez ve onu eklediğinizde bir varlığın türünü her zaman bilirsiniz. Ancak, türünü bilmeden bir varlığı güncelleştirmek ve bir POCO varlık sınıfı kullanmadan `DynamicTableEntity` türünü kullanabilirsiniz. Aşağıdaki kod örneği tek bir varlık alır ve `EmployeeCount` özelliğinin güncelleştirmeden önce var olduğunu denetler.  

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

### <a name="control-access-with-shared-access-signatures"></a>Paylaşılan erişim imzaları ile erişimi denetleme
Doğrudan tablo depolamayla kimlik doğrulaması gerektirmeden, istemci uygulamalarının tablo varlıklarını doğrudan değiştirmesine (ve sorgulayabilmesi) için paylaşılan erişim imzası (SAS) belirteçlerini kullanabilirsiniz. Genellikle, uygulamanızda SAS kullanarak üç ana avantajları vardır:  

* Bu cihazın tablo depolamadaki varlıklara erişmesine ve bunları değiştirmesine izin vermek için depolama hesabı Anahtarınızı güvenli olmayan bir platforma (bir mobil cihaz gibi) dağıtmanız gerekmez.  
* Web ve çalışan rollerinin varlıklarınızı yönetirken gerçekleştirdiği bazı işleri devretmek için bu işlemleri çalıştırabilirsiniz. Son Kullanıcı bilgisayarları ve mobil cihazlar gibi istemci cihazlara yük devreatayabilirsiniz.  
* Bir istemciye kısıtlanmış ve zaman sınırlı bir izin kümesi atayabilirsiniz (örneğin, belirli kaynaklara salt okuma erişimine izin verme).  

Tablo depolama ile SAS belirteçlerini kullanma hakkında daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanma](../storage/common/storage-dotnet-shared-access-signature-part-1.md).  

Ancak, hala tablo depolamadaki varlıklara bir istemci uygulaması veren SAS belirteçlerini oluşturmanız gerekir. Bunu, depolama hesabı Anahtarlarınıza güvenli erişim sağlayan bir ortamda yapın. Genellikle, SAS belirteçleri oluşturmak ve varlıklarınızın erişmesi gereken istemci uygulamaları sunmak için bir web veya çalışan rolü kullanın. Olduğundan hala bir ek yük oluşturma ve SAS belirteçlerini istemciler için en iyi şekilde nasıl dikkate almanız gereken yüksek hacimli senaryolarda özellikle bu yükünü azaltmak için teslim katılan.  

Bir tablodaki varlıkların bir alt kümesine erişim veren bir SAS belirteci oluşturmak mümkündür. Varsayılan olarak, tüm tablo için bir SAS belirteci oluşturursunuz. Ancak SAS belirtecinin, bir dizi `PartitionKey` değere veya bir dizi `PartitionKey` ve `RowKey` değere erişim izni vermesini de sağlayabilirsiniz. Her kullanıcının SAS belirteci yalnızca tablo depolamadaki kendi varlıklarına erişmelerine izin verecek şekilde, sisteminizin bireysel kullanıcıları için SAS belirteçleri oluşturmayı tercih edebilirsiniz.  

### <a name="asynchronous-and-parallel-operations"></a>Zaman uyumsuz ve paralel işlemleri
Birden çok bölümde isteklerinizi yayılmasını sağlanan zaman uyumsuz veya paralel sorgular kullanarak aktarım hızı ve istemci yanıt hızını artırabilirsiniz.
Örneğin, paralel tablolara erişen iki veya daha fazla çalışan rolü örnekleri olabilir. Belirli bölüm kümelerinden sorumlu tek tek çalışan rolleriniz olabilir veya her biri bir tablodaki tüm bölümlere erişebilen birden fazla çalışan rolü örneğine sahip olabilirsiniz.  

İstemci örneği içinde, depolama işlemlerini zaman uyumsuz olarak çalıştırarak aktarım hızını artırabilirsiniz. Depolama istemcisi kitaplığı, zaman uyumsuz sorgular ve değişiklikleri yazma kolaylaştırır. Örneğin, aşağıdaki C# kodda gösterildiği gibi, bir bölümdeki tüm varlıkları alan zaman uyumlu yöntemle başlayabilirsiniz:  

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

Sorgunun zaman uyumsuz olarak çalışması için aşağıdaki gibi kolayca bu kodu değiştirebilirsiniz:  

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

Bu zaman uyumsuz bir örnekte, zaman uyumlu bir sürümünü aşağıdaki değişiklikleri görebilirsiniz:  

* Yöntem imzası artık `async` değiştiricisini içerir ve bir `Task` örneği döndürür.  
* Sonuçları almak için `ExecuteSegmented` yöntemini çağırmak yerine, yöntemi artık `ExecuteSegmentedAsync` yöntemini çağırır. Yöntemi sonuçları zaman uyumsuz olarak almak için `await` değiştiricisini kullanır.  

İstemci uygulaması, `department` parametresi için farklı değerlerle bu yöntemi birden çok kez çağırabilir. Her sorgu ayrı bir iş parçacığında çalışır.  

`TableQuery` sınıfında `Execute` yönteminin zaman uyumsuz bir sürümü yoktur, çünkü `IEnumerable` arabirimi zaman uyumsuz numaralandırmayı desteklemez.  

Ekleme, güncelleştirme ve zaman uyumsuz olarak varlıkları silin. Aşağıdaki C# örneği eklemek veya çalışan varlığı değiştirmek için basit, zaman uyumlu bir yöntemi gösterir:  

```csharp
private static void SimpleEmployeeUpsert(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = employeeTable
        .Execute(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

Bu kodu, güncelleştirmenin zaman uyumsuz olarak çalışması için, aşağıdaki gibi kolayca değiştirebilirsiniz:  

```csharp
private static async Task SimpleEmployeeUpsertAsync(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = await employeeTable
        .ExecuteAsync(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

Bu zaman uyumsuz bir örnekte, zaman uyumlu bir sürümünü aşağıdaki değişiklikleri görebilirsiniz:  

* Yöntem imzası artık `async` değiştiricisini içerir ve bir `Task` örneği döndürür.  
* Varlığı güncelleştirmek için `Execute` yöntemini çağırmak yerine, yöntemi artık `ExecuteAsync` yöntemini çağırır. Yöntemi sonuçları zaman uyumsuz olarak almak için `await` değiştiricisini kullanır.  

İstemci uygulaması, bu gibi birden çok zaman uyumsuz yöntemi çağırabilir ve her bir yöntem çağrısı ayrı bir iş parçacığında çalışır.  


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

