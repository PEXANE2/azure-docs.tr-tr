---
title: Cosmos DB için Azure Fonksiyonlarını kullanırken sorun giderme sorunları
description: Cosmos DB için Azure Fonksiyonlar tetikleyicisini kullanırken sık karşılaşılan sorunlar, geçici çözüm ve tanılama adımları
author: ealsur
ms.service: cosmos-db
ms.date: 03/13/2020
ms.author: maquaran
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 7bf7d418e3f2680b32f61e42cffc76c921068508
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79365517"
---
# <a name="diagnose-and-troubleshoot-issues-when-using-azure-functions-trigger-for-cosmos-db"></a>Cosmos DB için Azure Fonksiyonlarını tetiklerken sorunları tanılama ve sorun giderme

Bu makalede, [Cosmos DB için Azure İşlevler tetikleyicisi](change-feed-functions.md)kullandığınızda sık karşılaşılan sorunlar, geçici çözüm ve tanılama adımları ele alındı.

## <a name="dependencies"></a>Bağımlılıklar

Cosmos DB için Azure İşlevleri tetikleyicisi ve bağlamaları, Temel Azure İşlevleri çalışma süresi üzerindeki uzantı paketlerine bağlıdır. Karşılaşabileceğiniz olası sorunları giderebilecek düzeltmeler ve yeni özellikler içerebileceğiiçin bu paketleri her zaman güncel tutun:

* Azure İşlevler V2 için [Bkz. Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB).
* Azure İşlevler V1 için [Bkz. Microsoft.Azure.WebJobs.Extensions.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB).

Bu makalede, çalışma zamanı açıkça belirtilmediği sürece her zaman Azure İşlevler V2'ye atıfta bulunulacaktır.

## <a name="consume-the-azure-cosmos-db-sdk-independently"></a>Azure Cosmos DB SDK'yı bağımsız olarak tüketin

Uzantı paketinin temel işlevi, Azure İşlevleri tetikleyicisi ve Cosmos DB için bağlamadesteği sağlamaktır. Ayrıca, tetikleyici ve bağlamaları kullanmadan Azure Cosmos DB ile programlı olarak etkileşimde kalmak istiyorsanız yararlı olan [Azure Cosmos DB .NET SDK'yı](sql-api-sdk-dotnet-core.md)da içerir.

Azure Cosmos DB SDK'yı kullanmak istiyorsanız, projenize başka bir NuGet paketi başvurusu eklemediğinizden emin olun. Bunun yerine, **SDK başvurusunun Azure İşlevlerinin Uzantı paketi aracılığıyla çözülmesine izin verin.** Azure Cosmos DB SDK'yı tetikleyici den ve bağlamalardan ayrı olarak tüketin

Ayrıca, [Azure Cosmos DB SDK istemcisinin](./sql-api-sdk-dotnet-core.md)kendi örneğini el ile oluşturuyorsanız, [Singleton desen yaklaşımını kullanarak](../azure-functions/manage-connections.md#documentclient-code-example-c)istemcinin yalnızca bir örneğine sahip olma desenini izlemeniz gerekir. Bu işlem, operasyonlarınızdaki olası soket sorunlarını önler.

## <a name="common-scenarios-and-workarounds"></a>Sık karşılaşılan senaryolar ve geçici çözüm

### <a name="azure-function-fails-with-error-message-collection-doesnt-exist"></a>Azure İşlevi hata iletisi toplama yok ile başarısız olur

Azure İşlevi hata iletisi ile başarısız olur "Ya kaynak koleksiyonu 'toplama adı' (veritabanı 'veritabanı adı') veya kira koleksiyonu 'collection2-name' (veritabanı 'database2-name') yok. Dinleyici başlamadan önce her iki koleksiyon da bulunmalıdır. Kira koleksiyonunu otomatik olarak oluşturmak için 'CreateLeaseCollectionIfNotExists'u 'true' olarak ayarlayın."

Bu, tetikleyicinin çalışması için gereken Azure Cosmos kapsayıcılarından birinin veya her ikisinin bulunmadığı veya Azure İşlevi'ne erişilemeyecek anlamına gelir. Hatanın kendisi, yapılandırmanıza bağlı olarak **hangi Azure Cosmos veritabanının ve kapsayıcısının tetikleyici olduğunu söyleyecektir.**

1. Özniteliği `ConnectionStringSetting` doğrulayın ve **Azure İşlev Uygulamanızda bulunan bir ayara başvuruyor.** Bu öznitelikteki değer Bağlantı Dizesinin kendisi değil, Yapılandırma Ayarı'nın adı olmalıdır.
2. Azure Cosmos hesabınızda bulunduğunu `databaseName` ve `collectionName` var olduğunu doğrulayın. Otomatik değer değiştirme (desenleri `%settingName%` kullanarak) kullanıyorsanız, Azure İşlev Uygulamanızda ayarın adının bulunduğundan emin olun.
3. Bir `LeaseCollectionName/leaseCollectionName`, varsayılan "kiralama" belirtmezseniz. Bu tür kapsayıcının var olduğunu doğrulayın. İsteğe bağlı olarak, tetikleyicinizdeki `CreateLeaseCollectionIfNotExists` özniteliği otomatik olarak oluşturacak şekilde `true` ayarlayabilirsiniz.
4. Azure İşlevi'ni engellemediğini görmek için [Azure Cosmos hesabınızın Güvenlik Duvarı yapılandırmasını](how-to-configure-firewall.md) doğrulayın.

### <a name="azure-function-fails-to-start-with-shared-throughput-collection-should-have-a-partition-key"></a>Azure İşlevi "Paylaşılan iş ortası toplamanın bir bölüm anahtarı olmalı" ile başlayamazsa

Azure Cosmos DB Uzantısı'nın önceki [sürümleri, paylaşılan](./set-throughput.md#set-throughput-on-a-database)bir iş veri tabanında oluşturulan bir kiralama kapsayıcısı kullanmayı desteklemedi. Bu sorunu gidermek için, en son sürümü almak için [Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB) uzantısını güncelleyin.

### <a name="azure-function-fails-to-start-with-partitionkey-must-be-supplied-for-this-operation"></a>Azure İşlevi "Bu işlem için PartitionKey sağlanmalıdır" ile başlayamaz.

Bu hata, şu anda eski bir [uzantı bağımlılığı](#dependencies)ile bölümlenmiş bir kira koleksiyonu kullanıyorsanız anlamına gelir. Kullanılabilir en son sürüme yükseltin. Şu anda Azure İşlevleri V1 üzerinde çalışıyorsanız, Azure İşlevler V2'ye yükseltmeniz gerekir.

### <a name="azure-function-fails-to-start-with-the-lease-collection-if-partitioned-must-have-partition-key-equal-to-id"></a>Azure İşlevi "Kira koleksiyonu, bölümlenmişse, id'e eşit bölüm anahtarına sahip olmalıdır" ile başlayamaz.

Bu hata, geçerli kira kapsayıcınızın bölümlenmiş olduğu, ancak `/id`bölüm anahtarı yolunun . Bu sorunu gidermek için, bölme anahtarı `/id` olarak kira kapsayıcısını yeniden oluşturmanız gerekir.

### <a name="you-see-a-value-cannot-be-null-parameter-name-o-in-your-azure-functions-logs-when-you-try-to-run-the-trigger"></a>"Değer null olamaz. Parametre adı: Tetikleyiciyi Çalıştırmayı denediğinizde Azure İşlevlerinizgünlüklerinizde o"

Bu sorun, Azure portalını kullanıyorsanız ve tetikleyiciyi kullanan bir Azure Işlevini incelerken ekrandaki **Çalıştır** düğmesini seçmeye çalışıyorsanız görünür. Tetikleyici, başlamak için Çalıştır'ı seçmeniz gerekmez, Azure İşlevi dağıtıldığında otomatik olarak başlar. Azure portalında Azure İşlevi'nin günlük akışını denetlemek, izlenen kapsayıcınıza gidin ve bazı yeni öğeler eklemek isterseniz, Tetikleyici'nin çalıştırAn tetikleyicisini otomatik olarak görürsünüz.

### <a name="my-changes-take-too-long-to-be-received"></a>Değişikliklerimin alınması çok uzun sürüyor

Bu senaryonun birden çok nedeni olabilir ve bunların tümü denetlenmelidir:

1. Azure İşleviniz Azure Cosmos hesabınızla aynı bölgeye mi dağıtıldı? En iyi ağ gecikme süresi için hem Azure İşlevinin hem de Azure Cosmos hesabınızın aynı Azure bölgesinde birlikte bulunması gerekir.
2. Azure Cosmos kapsayıcınızda gerçekleşen değişiklikler sürekli mi yoksa aralıklı mı?
Aralıklıysa, değişikliklerin depolanması ile Azure İşlevinin bunları alması arasında biraz gecikme olabilir. Bunun nedeni dahili olarak tetikleyici Azure Cosmos kapsayıcınızda değişiklikleri denetleyip hiç okunmayı bekleyen değişiklik olmadığını bulduğunda, yeni değişiklikler için denetleme yapmadan önce yapılandırılabilir bir süre (varsayılan olarak 5 saniye) uykuya geçer (yüksek RU tüketimini önlemek için). Bu uyku süresini tetikleyicinizin [yapılandırmasındaki](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md#configuration)`FeedPollDelay/feedPollDelay` ayarıyla yapılandırabilirsiniz (değerin milisaniye cinsinden olması beklenir).
3. Azure Cosmos kapsayıcınız [hız sınırlı](./request-units.md)olabilir.
4. Özel tercih `PreferredLocations` edilen bağlantı sırasını tanımlamak için virgülle ayrılmış Azure bölgelerinin listesini belirtmek için tetikleyicinizdeki özniteliği kullanabilirsiniz.

### <a name="some-changes-are-repeated-in-my-trigger"></a>Tetikleyicimde bazı değişiklikler yinelenir

"Değişiklik" kavramı, belge üzerinde yapılan bir işlemdir. Aynı belgeiçin olayların alındığı en yaygın senaryolar şunlardır:
* Hesap Nihai tutarlılık kullanıyor. Değişiklik akışını Nihai tutarlılık düzeyinde tüketirken, sonraki değişiklik akışı okuma işlemleri arasında yinelenen olaylar olabilir (bir okuma işleminin son olayı bir sonrakiilk olay olarak görünür).
* Belge güncelleştiriliyor. Değişiklik Akışı aynı belgeler için birden çok işlem içerebilir, bu belge güncelleştirme alıyorsa, birden çok olay alabilir (her güncelleştirme için bir tane). Aynı belge için farklı işlemler arasında ayırt etmek `_lsn` için kolay bir yolu [her değişiklik için özelliği](change-feed.md#change-feed-and-_etag-_lsn-or-_ts)izlemektir. Eşleşmezlerse, bunlar aynı belge üzerinde farklı değişikliklerdir.
* Belgeleri sadece olarak `id`tanımlıyorsanız, bir belgenin `id` benzersiz tanımlayıcısının ve bölüm anahtarı olduğunu unutmayın (aynı `id` ancak farklı bölüm anahtarına sahip iki belge olabilir).

### <a name="some-changes-are-missing-in-my-trigger"></a>Tetikleyicimde bazı değişiklikler eksik

Azure Cosmos kapsayıcınızda gerçekleşen bazı değişikliklerin Azure İşlevi tarafından alınmadığını fark ederseniz, gerçekleşmesi gereken bir ilk araştırma adımı vardır.

Azure İşleviniz değişiklikleri aldığında, genellikle bunları işler ve isteğe bağlı olarak sonucu başka bir hedefe gönderebilir. Eksik değişiklikleri araştırırken, hangi **değişikliklerin hedefte** değil, yutma noktasında (Azure İşlevi başladığında) alındığını ölçtüğünden emin olun.

Hedefte bazı değişiklikler eksikse, bu, değişiklikler alındıktan sonra Azure İşlevi yürütmesi sırasında bazı hatalar meydana geldiğini anlamına gelebilir.

Bu senaryoda, en iyi eylem `try/catch` yolu, kodunuzda ve değişiklikleri işleyen döngülerin içine bloklar eklemek, belirli bir öğe alt kümesi için herhangi bir hata algılamak ve bunları buna göre işlemektir (daha fazla analiz veya yeniden deneme için başka bir depolama alanına göndermek). 

> [!NOTE]
> Cosmos DB için Azure İşlevleri tetikleyicisi, kod yürütmeniz sırasında işlenmemiş bir özel durum oluştuğunda varsayılan olarak bir toplu değişiklik grubunu denemeyecektir. Bu, değişikliklerin hedefe ulaşmamasının nedeninin, bunları işlemekoyamadığınız anlamına gelir.

Bazı değişikliklerin tetikleyiciniz tarafından hiç alınmadığını fark ederseniz, en yaygın senaryo **başka bir Azure İşlevi'nin çalışıyor**olmasıdır. Azure'da dağıtılan başka bir Azure İşlevi veya bir geliştiricinin makinesinde yerel olarak çalışan ve **tam olarak aynı yapılandırmaya** (aynı izlenen ve kira kapsayıcıları) çalışan bir Azure İşlevi olabilir ve bu Azure İşlevi, Azure İşlevinizin işlemesini beklediğiniz değişikliklerin bir alt kümesini çalıyor olabilir.

Ayrıca, kaç tane Azure İşi Uygulaması örneği çalıştırdığınızı biliyorsanız, senaryo doğrulanabilir. Kira kabınızı inceler ve içindeki kira maddelerinin sayısını sayarsanız, bu alandaki `Owner` özelliğin farklı değerleri İşlev Uygulamanızın örnek sayısına eşit olmalıdır. Bilinen Azure İşlevi Uygulaması örneklerinden daha fazla sahip varsa, bu ek sahiplerin değişiklikleri "çalan" kişiler olduğu anlamına gelir.

Bu durumu aşmanın kolay bir yolu, `LeaseCollectionPrefix/leaseCollectionPrefix` Yeni/farklı bir değerle İşlevinize bir uygulama veya alternatif olarak yeni bir kiralama kapsayıcısıyla test etmektir.

### <a name="need-to-restart-and-reprocess-all-the-items-in-my-container-from-the-beginning"></a>En başından itibaren kabımdaki tüm öğeleri yeniden başlatmanız ve yeniden işlemem gerekiyor 
Bir kaptaki tüm öğeleri baştan yeniden işlemek için:
1. Şu anda çalışıyorsa Azure işlevinizi durdurun. 
1. Kira koleksiyonundaki belgeleri silme (veya boş olması için kira koleksiyonunu silmek ve yeniden oluşturmak)
1. İşlevinizdeki [StartFromBeginning](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md#configuration) CosmosDBTrigger özniteliğini doğru olarak ayarlayın. 
1. Azure işlevini yeniden başlatın. Şimdi tüm değişiklikleri baştan okuyacak ve işleyecek. 

[Başlangıç Başlangıç'ı](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md#configuration) doğru ayarlamak, Azure işlevine geçerli saat yerine koleksiyonun geçmişinin başlangıcından itibaren değişiklikleri okumaya başlamasını söyler. Bu yalnızca zaten oluşturulmuş kiralamalar olmadığında (diğer bir deyişle, kiralama koleksiyonundaki belgeler) çalışır. Zaten oluşturulmuş kiralar olduğunda bu özelliğin doğru ayarlanması hiçbir etkisi yoktur; Bu senaryoda, bir işlev durdurulup yeniden başlatıldığında, kiralama koleksiyonunda tanımlandığı gibi son denetim noktasından okumaya başlar. Baştan yeniden işlemek için yukarıdaki adımları 1-4 izleyin.  

### <a name="binding-can-only-be-done-with-ireadonlylistdocument-or-jarray"></a>Bağlama yalnızca IReadOnlyList\<Belge> veya JArray ile yapılabilir

Bu hata, Azure İşlevler projeniz (veya başvurulan herhangi bir proje) [Azure İşleçleri Cosmos DB Uzantısı](./troubleshoot-changefeed-functions.md#dependencies)tarafından sağlanandan farklı bir sürümle Azure Cosmos DB SDK'ya manuel NuGet başvurusu içeriyorsa ortaya çıkar.

Bu durumu çözmek için eklenen manuel NuGet başvurusunu kaldırın ve Azure Cosmos DB SDK başvurusunun Azure İşlemi Cosmos DB Uzantısı paketi aracılığıyla çözülmesine izin verin.

### <a name="changing-azure-functions-polling-interval-for-the-detecting-changes"></a>Algılama değişiklikleri için Azure İşlevinin yoklama aralığını değiştirme

Değişikliklerim için daha önce açıklandığı [gibi, değişikliklerin alınması çok uzun sürer,](./troubleshoot-changefeed-functions.md#my-changes-take-too-long-to-be-received)Azure işlevi yeni değişiklikleri kontrol etmeden önce (yüksek RU tüketimini önlemek için) yapılandırılabilir bir süre (varsayılan olarak 5 saniye) uyku moduna geçecektir. Bu uyku süresini tetikleyicinizin [yapılandırmasındaki](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md#configuration)`FeedPollDelay/feedPollDelay` ayarıyla yapılandırabilirsiniz (değerin milisaniye cinsinden olması beklenir).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İşlevleriniz için izlemeyi etkinleştirme](../azure-functions/functions-monitoring.md)
* [Azure Cosmos DB .NET SDK Sorun Giderme](./troubleshoot-dot-net-sdk.md)
