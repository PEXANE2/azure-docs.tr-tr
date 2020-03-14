---
title: Azure Cosmos DB'de destek akış değişiklik ile çalışma
description: Belgelerdeki değişiklikleri izlemek için Azure Cosmos DB akış desteğini değiştirin, tetikleyiciler gibi olay tabanlı işleme ve önbellekleri ve analitik sistemleri güncel tutun
author: TheovanKraay
ms.author: thvankra
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/25/2019
ms.reviewer: sngun
ms.custom: seodec18
ms.openlocfilehash: 898dfe7a619981b93af98effa942fdecbeb42dde
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79368137"
---
# <a name="change-feed-in-azure-cosmos-db---overview"></a>Azure Cosmos DB akışı değiştirme-genel bakış

Azure Cosmos DB’de değişiklik akışı desteği, Azure Cosmos kapsayıcısında yapılan değişiklikleri dinleyerek çalışır. Ardından çıkış olarak, değiştirilen belgelerin değiştirilme zamanına göre sıralandığı bir belge listesi oluşturur. Değişiklikler kalıcı hale getirilir, zaman uyumsuz ve artırımlı olarak işlenebilir ve çıkış, paralel işleme için bir veya daha fazla tüketiciye dağıtılabilir. 

Azure Cosmos DB, perakende, oyun, IOT ve işlem günlüğü uygulamalar için uygundur. Bu uygulamalar bir ortak tasarım modelinde, ek eylemleri tetiklemek için verilerde yapılan değişiklikleri kullanmaktır. Ek eylem örnekleri şunlardır:

* Bir öğe eklendiğinde veya bir bildirim ya da bir API çağrısına tetikleniyor.
* Gerçek Zamanlı Akış, IOT veya gerçek zamanlı analiz işlem verilerinde işleme için işleme.
* Ek veri taşıma, bir önbellek veya bir arama motoru veya veri ambarı eşitleme veya soğuk depolama için veri arşivleme.

Değişiklik Azure Cosmos DB'de akışı, aşağıdaki görüntüde gösterildiği gibi her biri bu desenleri için etkili ve ölçeklenebilir çözümler oluşturmanıza olanak sağlar:

![Güç gerçek zamanlı analiz ve olay temelli bilgi işlem senaryoları kullanarak Azure Cosmos DB değişiklik akışı](./media/change-feed/changefeedoverview.png)

## <a name="supported-apis-and-client-sdks"></a>Desteklenen API'ları ve istemci SDK'ları

Bu özellik şu anda aşağıdaki Azure Cosmos DB API'ları ve istemci SDK'ları tarafından desteklenmektedir.

| **İstemci sürücüleri** | **Azure CLI** | **SQL APı 'SI** | **Cassandra için Azure Cosmos DB API 'SI** | **MongoDB için Azure Cosmos DB API 'SI** | **Gremlin API 'SI**|**Tablo API’si** |
| --- | --- | --- | --- | --- | --- | --- |
| .NET | NA | Yes | Yes | Yes | Yes | Hayır |
|Java|NA|Yes|Yes|Yes|Yes|Hayır|
|Python|NA|Yes|Yes|Yes|Yes|Hayır|
|Düğüm/JS|NA|Yes|Yes|Yes|Yes|Hayır|

## <a name="change-feed-and-different-operations"></a>Değişiklik akışı ve farklı işlemler

Bugün, değişiklik akışı tüm işlemlerde bakın. Burada denetleyebilirsiniz işlevi yalnızca güncelleştirmeler ve değil ekler gibi henüz kullanılabilir akış, belirli işlemler için değiştirin. Değişiklik akışındaki öğeleri işlerken bu öğeye bağlı olarak, güncelleştirmeler ve filtre için öğe üzerinde "geçici işaretleyici" ekleyebilirsiniz. Şu anda değişiklik akışında silme günlüğü yok. Önceki örneğe benzer, yumuşak bir işaretçi, silinen öğeleri ekleyebilirsiniz, örneğin, böylece otomatik olarak silinebilir "silindi" adlı "true" olarak ayarlayın ve öğe üzerinde bir TTL ayarlamak öğesindeki bir öznitelik ekleyebilirsiniz. Geçmiş öğeler için değişiklik akışını okuyabilir (öğeye karşılık gelen en son değişiklik, ara değişiklikleri içermez), örneğin, beş yıl önce eklenmiş olan öğeler. Öğe silinmedi, değişiklik okuyabilirsiniz kapsayıcınızı kaynağı sunulan ürünün kendinde akış.

### <a name="sort-order-of-items-in-change-feed"></a>Değişiklik akışı öğelerinin sıralama

Değişiklik akışı öğelerini değiştirme zamanlarının sırasına göre gelir. Bu sıralama düzeni mantıksal bölüm anahtarı başına garanti edilir.

### <a name="consistency-level"></a>Tutarlılık düzeyi

Değişiklik akışı nihai tutarlılık düzeyinde kullanılırken, sonraki değişiklik akışı okuma işlemleri arasında (bir okuma işleminin son olayı, sonraki ilk olarak görüntülenir) içinde yinelenen olaylar olabilir.

### <a name="change-feed-in-multi-region-azure-cosmos-accounts"></a>Değişiklik akışı çok bölgeli Azure Cosmos hesaplar

Çok bölgeli bir Azure Cosmos hesabında üzerine yazma bölgesi başarısız olursa, değişiklik akışı el ile yük devretme işlemi çalışır ve bitişik olacaktır.

### <a name="change-feed-and-time-to-live-ttl"></a>Değişiklik akışı ve yaşam süresi (TTL)

Değişiklik akışı, bir TTL (yaşam süresi) özelliği bir öğe üzerinde -1 olarak ayarlarsanız, her zaman açık kalır. Veriler silinmez, değişiklik akışı kalır.  

### <a name="change-feed-and-_etag-_lsn-or-_ts"></a>Değişiklik akışı ve _etag, _lsn veya _ts

_Etag biçimi dahili kullanım içindir ve dilediğiniz zaman değiştirebilirsiniz çünkü, bağımlılık üzerinde almamalıdır. _ts bir değişiklik ya da oluşturma zaman damgası ' dir. _Ts kronolojik bir karşılaştırması için kullanabilirsiniz. _lsn yalnızca değişiklik akışı için eklenen bir toplu iş KIMLIĞIDIR; işlem KIMLIĞINI temsil eder. Birçok öğe aynı _lsn olabilir. ETag FeedResponse üzerinde öğede gördüğünüz _etag farklıdır. _etag dahili bir tanımlayıcıdır ve eşzamanlılık için kullanılan denetim öğesi sürümü hakkında akışın sıralama için ETag kullanılırken söyler.

## <a name="change-feed-use-cases-and-scenarios"></a>Kullanım örnekleri ve senaryoları değişiklik akışı

Yüksek hacimli yazma ile büyük veri kümeleri işlem verimli etkinleştirir değişiklik akışı. Ayrıca, değişiklik akışı nelerin değiştiğini belirlemek için bir veri kümesinin tamamında sorgulama için bir alternatif sunar.

### <a name="use-cases"></a>Uygulama alanları

Örneğin, değişiklik akışı ile aşağıdaki görevleri verimli bir şekilde gerçekleştirebilirsiniz:

* Bir önbellek güncelleştirme, bir arama dizinini güncelleştirin veya Azure Cosmos DB'de depolanan verilerle bir veri ambarı'nı güncelleştirin.

* Uygulama düzeyinde veri katmanlama ve arşivleme uygulayın, örneğin, [Azure Blob depolama](../storage/common/storage-introduction.md)gibi diğer depolama sistemlerine "soğuk verileri" Azure Cosmos DB ve yaş dışına "sık verileri" depolayın.

* Başka bir Azure Cosmos hesabı ya da başka bir Azure Cosmos kapsayıcı sıfır kapalı kalma süresini geçişler, farklı bir mantıksal bölüm anahtarı ile gerçekleştirin.

* Azure Cosmos DB gerçek zamanlı, toplu işlem ve sorgu özelliklerini desteklediği Azure Cosmos DB kullanarak [lambda mimarisini](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) uygulayın. bu sayede, düşük TCO ile Lambda mimarisini etkinleştirir.

* Cihazlardan, sensörlerden, altyapıdan ve uygulamalardan olay verilerini alın ve depolayın ve örneğin [Spark](../hdinsight/spark/apache-spark-overview.md)kullanarak bu olayları gerçek zamanlı olarak işleyin.  Aşağıdaki görüntüde, değişiklik akışı ile Azure Cosmos DB kullanarak lambda mimarisi nasıl uygulayacağınıza dair gösterilmektedir:

![Kesintisiz alım ve sorgu için Azure Cosmos DB tabanlı lambda işlem hattı](./media/change-feed/lambda.png)

### <a name="scenarios"></a>Senaryolar

Değişiklik akışı ile kolayca uygulayabilirsiniz senaryolardan bazıları şunlardır:

* [Sunucusuz](https://azure.microsoft.com/solutions/serverless/) Web veya mobil uygulamalarınız içinde, müşterinizin profilinde, tercihlerinde veya konumlarından yapılan tüm değişiklikler gibi olayları izleyebilir ve örneğin [Azure işlevleri](change-feed-functions.md)'ni kullanarak cihazlarına anında iletme bildirimleri gönderebilirsiniz.

* Bir oyun oluşturmak için Azure Cosmos DB kullanıyorsanız, şunları yapabilirsiniz, örneğin, kullanım değişiklik akışı tamamlanmış oyunlardan puanları göre gerçek zamanlı puan tabloları uygulamak için.


## <a name="working-with-change-feed"></a>Değişiklik akışı ile çalışma

Aşağıdaki seçenekleri kullanarak değişiklik akışı ile çalışabilirsiniz:

* [Azure Işlevleri ile değişiklik akışını kullanma](change-feed-functions.md)
* [Değişiklik akışı işlemcisi ile değişiklik akışını kullanma](change-feed-processor.md) 

Değişiklik akışı kapsayıcıdaki her bir mantıksal bölüm anahtarı için kullanılabilir ve, paralel işleme için bir veya daha fazla tüketicileri arasında aşağıdaki resimde gösterildiği gibi dağıtılabilir.

![Azure Cosmos DB değişiklik akışı, dağıtılan işleme](./media/change-feed/changefeedvisual.png)

## <a name="features-of-change-feed"></a>Değişiklik akışı özellikleri

* Değişiklik akışı, tüm Azure Cosmos hesaplar için varsayılan olarak etkindir.

* Azure Cosmos veritabanınız ile ilişkili bölgelerin herhangi birinde olduğu Azure Cosmos DB gibi, değişiklik akışından okumak için [sağlanan aktarım hızını](request-units.md) kullanabilirsiniz.

* Değişiklik akışı, ekler ve kapsayıcı içindeki öğelerde yapılan güncelleştirme işlemlerini içerir. Siler yakalayabilirsiniz öğelerinizi (örneğin, belgeleri) içinde "geçici silme" bayrak ayarlayarak yerine siler. Alternatif olarak, [TTL özelliği](time-to-live.md)olan öğeleriniz için sonlu bir süre sonu süresi ayarlayabilirsiniz. Örneğin, 24 saat ve kullanım yakalamak için bu özelliğin değerini siler. Bu çözüm sayesinde, TTL sona erme süresinden daha kısa bir süre içinde değişiklikleri işleme gerekir. 

* Her değişiklik için bir öğe değişiklik akışı tam bir kez görünür ve istemcilerin denetim noktası oluşturma mantığı yönetmeniz gerekir. Kontrol noktalarını yönetme karmaşıklığına engel olmak istiyorsanız, değişiklik akışı işlemcisi otomatik onay işareti ve "en az bir kez" semantiği sağlar. Bkz. [değişiklik akışı işlemcisi ile değişiklik akışını kullanma](change-feed-processor.md).

* Yalnızca belirli bir öğe en son değişikliğin değişiklik günlüğünde bulunur. Ara değişikliklerin kullanılamayabilir.

* Değişiklik akışı, her bir mantıksal bölüm anahtarı değeri içinde değişiklik sıraya göre sıralanır. Bölüm anahtarı değerlerine arasında yürütülme sırası yoktur.

* Değişiklikleri tüm-değişiklikler kullanılabilir sabit veri saklama dönemi yoktur olan belirli bir noktaya, eşitlenebilir.

* Değişiklikler, paralel bir Azure Cosmos kapsayıcının tüm mantıksal bölüm anahtarları için kullanılabilir. Bu özellik paralel olarak birden fazla tüketici tarafından işlenmek üzere büyük kapsayıcıları değişikliklerini tanır.

* Uygulamalar aynı kapsayıcıda aynı anda birden fazla değişiklik akışı talep edebilir. ChangeFeedOptions.StartTime ilk bir başlangıç noktası sağlamak için kullanılabilir. Örneğin, belirli bir saatin karşılık gelen devamlılık belirteci bulunamadı. ContinuationToken belirtilmişse StartTime ve StartFromBeginning değerlerin kazanır. ~ 5 saniye duyarlığını ChangeFeedOptions.StartTime olur. 

## <a name="change-feed-in-apis-for-cassandra-and-mongodb"></a>Cassandra ve MongoDB API 'Lerinde akışı değiştirme

Değişiklik akışı işlevselliği, MongoDB API 'sinde değişiklik akışı olarak ve Cassandra API ' de koşul ile sorgulama olarak ortaya çıkmış. MongoDB API 'SI için uygulama ayrıntıları hakkında daha fazla bilgi edinmek için bkz. [MongoDB için Azure Cosmos DB API 'sindeki akışları değiştirme](mongodb-change-streams.md).

Native Apache Cassandra değişiklik verilerini yakalama (CDC), belirli tabloları arşivleme için bayrakla işaretleme ve CDC günlüğü için yapılandırılabilir bir diske ulaşıldığında bu tablolara yazmaları reddetme mekanizması sağlar. Cassandra için Azure Cosmos DB API 'sindeki akış değiştirme özelliği, değişiklikleri CQL aracılığıyla koşullu olarak sorgulama yeteneğini geliştirir. Uygulama ayrıntıları hakkında daha fazla bilgi edinmek için bkz. [Cassandra için Azure Cosmos DB API 'sindeki akışı değiştirme](cassandra-change-feed.md).

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler de akış değiştirme hakkında daha fazla bilgi edinmek için şimdi geçebilirsiniz:

* [Değişiklik akışını okuma seçenekleri](read-change-feed.md)
* [Azure Işlevleri ile değişiklik akışını kullanma](change-feed-functions.md)
* [Değişiklik akışı işlemcisini kullanma](change-feed-processor.md)
