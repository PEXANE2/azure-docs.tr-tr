---
title: Dayanıklı İşlevlerde performans ve ölçek - Azure
description: Azure İşlevler için Dayanıklı İşlevler uzantısına giriş.
author: cgillum
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: azfuncdf
ms.openlocfilehash: 260811c4ae15b45de6f7bc1b22e3ed6dcea44259
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277914"
---
# <a name="performance-and-scale-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler'de pe ve ölçek (Azure İşlevleri)

Performansı ve ölçeklenebilirliği optimize etmek için [Dayanıklı Fonksiyonların](durable-functions-overview.md)benzersiz ölçekleme özelliklerini anlamak önemlidir.

Ölçek davranışını anlamak için, temel Azure Depolama sağlayıcısının bazı ayrıntılarını anlamanız gerekir.

## <a name="history-table"></a>Geçmiş tablosu

**Geçmiş** tablosu, görev hub'ındaki tüm düzenleme örneklerinin geçmiş olaylarını içeren bir Azure Depolama tablosudur. Bu tablonun adı *TaskHubName*Geçmişi biçimindedir. Örnekler çalıştırıldıkça, bu tabloya yeni satırlar eklenir. Bu tablonun bölüm anahtarı, orkestrasyonun örnek kimliğinden türetilmiştir. Örnek kimlik çoğu durumda rasgeledir ve Azure Depolama'da dahili bölümlerin en iyi şekilde dağıtılmasını sağlar.

Bir düzenleme örneğinin çalışması gerektiğinde, Geçmiş tablosunun uygun satırları belleğe yüklenir. Bu *geçmiş olayları* daha sonra daha önce kontrol edilen durumuna geri almak için orkestratör işlev koduna yeniden çalınır. Bu şekilde durumu yeniden oluşturmak için yürütme geçmişinin kullanımı [Olay Kaynak desen](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing)etkilenir.

## <a name="instances-table"></a>Örnekler tablosu

**Örnekler** tablosu, görev hub'ındaki tüm düzenleme ve varlık örneklerinin durumlarını içeren başka bir Azure Depolama tablosudur. Örnekler oluşturuldukça, bu tabloya yeni satırlar eklenir. Bu tablonun bölüm anahtarı, düzenleme örneği kimliği veya varlık anahtarıdır ve satır tuşu sabit bir sabittir. Orkestrasyon veya varlık örneği başına bir satır vardır.

Bu tablo, (.NET) ve `GetStatusAsync` `getStatus` (JavaScript) API'lerinden örnek sorgu isteklerini ve [http API durum sorgusunu](durable-functions-http-api.md#get-instance-status)karşılamak için kullanılır. Sonunda daha önce bahsedilen **Tarih** tablosunun içeriği ile tutarlı tutulur. Örnek sorgu işlemlerini bu şekilde verimli bir şekilde karşılamak için ayrı bir Azure Depolama tablosunun [kullanılması, Komut ve Sorgu Sorumluluğu Ayrımı (CQRS) deseni](https://docs.microsoft.com/azure/architecture/patterns/cqrs)tarafından etkilenir.

## <a name="internal-queue-triggers"></a>İç sıra tetikleyicileri

Orchestrator işlevleri ve etkinlik işlevleri, işlev uygulamasının görev hub'ındaki iç kuyruklar tarafından tetiklenir. Kuyrukları bu şekilde kullanmak güvenilir "en az bir kez" ileti teslim garantisi sağlar. Dayanıklı İşlevler'de iki tür kuyruk vardır: **denetim sırası** ve iş **öğesi sırası.**

### <a name="the-work-item-queue"></a>İş öğesi sırası

Dayanıklı İşlevler'de görev hub'ı başına bir iş öğesi sırası vardır. Temel bir sıradır ve Azure İşlevler'deki diğer `queueTrigger` kuyruğa benzer şekilde çalışır. Bu sıra, tek bir iletiyi aynı anda sıradan ayırarak durum ifadesüz *etkinlik işlevlerini* tetiklemek için kullanılır. Bu iletilerin her biri, etkinlik işlev girişleri ve yürütülecek işlev gibi ek meta veriler içerir. Dayanıklı İşlevler uygulaması birden çok VM'ye ölçeklendiğinde, bu VM'lerin tümü iş öğesi kuyruğundan iş elde etmek için yarışabilir.

### <a name="control-queues"></a>Denetim sırası(lar)

Dayanıklı *İşlevler'de* görev hub'ı başına birden çok denetim sırası vardır. *Denetim sırası,* daha basit iş öğesi kuyruğundan daha karmaşıktır. Denetim kuyrukları, durumlu orchestrator ve varlık işlevlerini tetiklemek için kullanılır. Orkestratör ve varlık işlevi örnekleri tek tekton olduğundan, VM'ler arasında yük dağıtmak için rakip bir tüketici modeli kullanmak mümkün değildir. Bunun yerine, orchestrator ve varlık iletileri denetim kuyrukları arasında yük dengeli. Bu davranış hakkında daha fazla bilgi sonraki bölümlerde bulunabilir.

Denetim kuyrukları çeşitli orkestrasyon yaşam döngüsü ileti türleri içerir. Örnekler arasında [orchestrator denetim iletileri,](durable-functions-instance-management.md)etkinlik işlevi *yanıt* iletileri ve zamanlayıcı iletileri verilebilir. Tek bir ankette 32'ye kadar ileti denetim kuyruğundan silinecektir. Bu iletiler, hangi orkestrasyon örneğine yönelik olduğu gibi meta verilerin yanı sıra yük verileri de içerir. Birden çok dequeued iletileri aynı düzenleme örneği için tasarlanmıştır, bunlar bir toplu iş olarak işlenir.

### <a name="queue-polling"></a>Sıra yoklama

Dayanıklı görev uzantısı, boşta sıra yoklamanın depolama işlem maliyetleri üzerindeki etkisini azaltmak için rasgele bir üstel geri leme algoritması uygular. İleti bulunduğunda, çalışma zamanı hemen başka bir ileti için denetler; ileti bulunmadığında, yeniden denemeden önce bir süre bekler. Bir sıra iletisi almak için sonraki başarısız denemelerden sonra, bekleme süresi varsayılan olarak 30 saniye olan maksimum bekleme süresine ulaşana kadar artmaya devam eder.

Maksimum yoklama gecikmesi, `maxQueuePollingInterval` [ana bilgisayar.json dosyasındaki](../functions-host-json.md#durabletask)özellik üzerinden yapılandırılabilir. Bu özelliğin daha yüksek bir değere ayarlanması, ileti işleme gecikmelerinin artmasına neden olabilir. Daha yüksek gecikmeler ancak hareketsizlik dönemlerinden sonra beklenebilir. Bu özelliğin daha düşük bir değere ayarlanması, artan depolama işlemleri nedeniyle depolama maliyetlerinin artmasına neden olabilir.

> [!NOTE]
> Azure İşlevler Tüketimi ve Premium planlarında çalışırken, [Azure İşlevler Ölçeği Denetleyicisi](../functions-scale.md#how-the-consumption-and-premium-plans-work) her denetimi ve iş öğesi kuyruğunu her 10 saniyede bir yoklar. Bu ek yoklama, işlev uygulaması örneklerini ne zaman etkinleştireceklerini belirlemek ve ölçek lendirme kararları almak için gereklidir. Yazma sırasında, bu 10 saniyelik aralık sabittir ve yapılandırılamaz.

## <a name="storage-account-selection"></a>Depolama hesabı seçimi

Dayanıklı Işlevler tarafından kullanılan kuyruklar, tablolar ve lekeler yapılandırılmış bir Azure Depolama hesabında oluşturulur. Kullanılacak `durableTask/storageProvider/connectionStringName` **hesap, ana bilgisayar.json** `durableTask/azureStorageConnectionStringName` dosyasındaki Ayarı (veya Dayanıklı Fonksiyonlar 1.x'teki ayar) kullanılarak belirtilebilir.

### <a name="durable-functions-2x"></a>Dayanıklı Fonksiyonlar 2.x

```json
{
  "extensions": {
    "durableTask": {
      "storageProvider": {
        "connectionStringName": "MyStorageAccountAppSetting"
      }
    }
  }
}
```

### <a name="durable-functions-1x"></a>Dayanıklı Fonksiyonlar 1.x

```json
{
  "extensions": {
    "durableTask": {
      "azureStorageConnectionStringName": "MyStorageAccountAppSetting"
    }
  }
}
```

Belirtilmemişse, `AzureWebJobsStorage` varsayılan depolama hesabı kullanılır. Ancak performansa duyarlı iş yükleri için varsayılan olmayan bir depolama hesabının yapılandırılması önerilir. Dayanıklı Fonksiyonlar Azure Depolama'yı yoğun olarak kullanır ve özel bir depolama hesabı kullanmak, Dayanıklı İşlevler depolama kullanımını Azure İşi ana bilgisayarının dahili kullanımından yalıtır.

## <a name="orchestrator-scale-out"></a>Orchestrator ölçeklendirme

Etkinlik işlevleri durumsuzdur ve VM'ler eklenerek otomatik olarak ölçeklendirilir. Orchestrator işlevleri ve varlıkları, diğer taraftan, bir veya daha fazla denetim kuyrukları arasında *bölümlü.* Denetim kuyruklarının sayısı **ana bilgisayar.json** dosyasında tanımlanır. Aşağıdaki örnek host.json snippet `durableTask/storageProvider/partitionCount` özelliği `durableTask/partitionCount` (veya Dayanıklı Fonksiyonlar `3`1.x) ayarlar.

### <a name="durable-functions-2x"></a>Dayanıklı Fonksiyonlar 2.x

```json
{
  "extensions": {
    "durableTask": {
      "storageProvider": {
          "partitionCount": 3
      }
    }
  }
}
```

### <a name="durable-functions-1x"></a>Dayanıklı Fonksiyonlar 1.x

```json
{
  "extensions": {
    "durableTask": {
      "partitionCount": 3
    }
  }
}
```

Görev hub'ı 1 ile 16 bölüm arasında yapılandırılabilir. Belirtilmemişse, varsayılan bölüm **4**sayısı 4'dür.

Birden çok işlev ana bilgisayar örneklerine (genellikle farklı VM'lerde) ölçeklenirken, her örnek denetim sıralarından birinde bir kilit alır. Bu kilitler, blob depolama kiralamaları olarak dahili olarak uygulanır ve bir orkestrasyon örneğinin veya varlığın aynı anda yalnızca tek bir ana bilgisayar örneğinde çalıştığından emin olun. Bir görev hub'ı üç denetim sırası yla yapılandırılırsa, düzenleme örnekleri ve varlıklar üç VM'ye kadar yük dengelenebilir. Etkinlik işlevi yürütme kapasitesini artırmak için ek VM'ler eklenebilir.

Aşağıdaki diyagram, Azure İşlevleri ana bilgisayar ana bilgisayarını ölçeklenmiş bir ortamda depolama varlıklarıyla nasıl etkileşimde bulunduğugöster.

![Ölçek diyagramı](./media/durable-functions-perf-and-scale/scale-diagram.png)

Önceki diyagramda gösterildiği gibi, tüm VM'ler iş öğesi kuyruğundaki iletiler için rekabet eder. Ancak, yalnızca üç VM denetim kuyruklarından iletiler alabilir ve her VM tek bir denetim kuyruğukilitler.

Düzenleme örnekleri ve varlıklar tüm denetim sırası örnekleri arasında dağıtılır. Dağıtım, orkestrasyonun örnek kimliğini veya varlık adı ve anahtar çiftini karma olarak tamamlar. Düzenleme örneği işletmek varsayılan olarak rasgele GUID' lerdir ve örneklerin tüm denetim kuyruklarına eşit olarak dağıtılmalarını sağlar.

Genel olarak konuşursak, orkestratör işlevleri hafif olması amaçlanmıştır ve büyük miktarda bilgi işlem gücü gerektirmemelidir. Bu nedenle, orkestrasyonlar için büyük iş elde etmek için çok sayıda denetim sırası bölümleri oluşturmak gerekli değildir. Ağır işlerin çoğu sonsuz ölçeklenebilir devletsiz etkinlik işlevleri, yapılmalıdır.

## <a name="auto-scale"></a>Otomatik ölçeklendirme

Tüketim ve Elastik Premium planlarında çalışan tüm Azure İşlevlerinde olduğu gibi, Dayanıklı Işlevler [Azure İşlevleri ölçek denetleyicisi](../functions-scale.md#runtime-scaling)aracılığıyla otomatik ölçeklendirmeyi destekler. Ölçek Denetleyicisi, düzenli olarak _peek_ komutları vererek tüm kuyrukların gecikmesini izler. Göze teşrep edilen iletilerin gecikmelerine bağlı olarak, Ölçek Denetleyicisi VM'ler ekleyip eklememeye veya kaldırmaya karar verir.

Ölçek Denetleyicisi denetim sırası iletisi gecikmelerinin çok yüksek olduğunu belirlerse, ileti gecikmesi kabul edilebilir bir düzeye inene veya denetim sırası bölüm sayısına ulaşana kadar VM örnekleri ekler. Benzer şekilde, ölçek denetleyicisi, bölüm sayısına bakılmaksızın iş öğesi sırası gecikmeleri yüksekse, sürekli olarak VM örnekleri ekler.

> [!NOTE]
> Dayanıklı Fonksiyonlar 2.0 ile başlayarak, fonksiyon uygulamaları Elastic Premium planında VNET korumalı hizmet bitiş noktaları içinde çalışacak şekilde yapılandırılabilir. Bu yapılandırmada, Dayanıklı Işlevler Ölçek Denetleyicisi yerine ölçek isteklerini başlatır.

## <a name="thread-usage"></a>İş parçacığı kullanımı

Orchestrator işlevleri, yürütmenin birçok tekrar da belirleyici olmasını sağlamak için tek bir iş parçacığı üzerinde yürütülür. Bu tek iş parçacığı yürütme nedeniyle, orchestrator işlev iş parçacıkları cpu yoğun görevleri gerçekleştirmek değil, G /O yapmak veya herhangi bir nedenle blok önemlidir. G/Ç, engelleme veya birden çok iş parçacığı gerektirebilecek tüm işler etkinlik işlevlerine taşınmalıdır.

Etkinlik işlevleri, sıra nın tetiklediği normal işlevlerle aynı davranışlara sahiptir. G/Ç'yi güvenle yapabilir, CPU yoğun işlemleri yürütebilir ve birden çok iş parçacığı kullanabilirler. Etkinlik tetikleyicileri devletsiz olduğundan, sınırsız sayıda VM'ye serbestçe ölçeklenebilirler.

Varlık işlevleri de tek bir iş parçacığı üzerinde yürütülür ve işlemler tek seferde işlenir. Ancak, varlık işlevleri yürütülebilir kod türü üzerinde herhangi bir kısıtlama yoktur.

## <a name="concurrency-throttles"></a>Eşzamanlılık azaltma

Azure İşlevler, tek bir uygulama örneği içinde aynı anda birden çok işlevyürütmeyi destekler. Bu eşzamanlı yürütme paralelliği artırmaya yardımcı olur ve tipik bir uygulamanın zaman içinde karşılaşacağı "soğuk başlangıç" sayısını en aza indirir. Ancak, yüksek eşzamanlılık, vm başına sistem kaynaklarını bu tür ağ bağlantılarını veya kullanılabilir belleği tüketebilir. İşlev uygulamasının gereksinimlerine bağlı olarak, yüksek yük koşullarında bellek tükenme olasılığını önlemek için her örnek eşpara birimini daraltmak gerekebilir.

Etkinlik, orchestrator ve varlık işlev eşzamanlılık sınırları **ana bilgisayar.json** dosyasında yapılandırılabilir. İlgili ayarlar `durableTask/maxConcurrentActivityFunctions` etkinlik işlevleri `durableTask/maxConcurrentOrchestratorFunctions` ve hem orkestratör hem de varlık işlevleri içindir.

### <a name="functions-20"></a>Fonksiyonlar 2.0

```json
{
  "extensions": {
    "durableTask": {
      "maxConcurrentActivityFunctions": 10,
      "maxConcurrentOrchestratorFunctions": 10
    }
  }
}
```

### <a name="functions-1x"></a>İşlevler 1.x

```json
{
  "durableTask": {
    "maxConcurrentActivityFunctions": 10,
    "maxConcurrentOrchestratorFunctions": 10
  }
}
```

Önceki örnekte, en fazla 10 orchestrator veya varlık işlevi ve 10 etkinlik işlevi aynı anda tek bir VM üzerinde çalıştırılabilir. Belirtilmemişse, eşzamanlı etkinlik ve orchestrator veya entity işlev yürütmelerinin sayısı VM'deki çekirdek sayısının 10 Kat'ına kadar kapatılır.

> [!NOTE]
> Bu ayarlar, bellek ve CPU kullanımını tek bir VM'de yönetmenize yardımcı olmak için yararlıdır. Ancak, birden çok VM arasında ölçeklendirildiğinde, her VM'nin kendi sınırları vardır. Bu ayarlar, eşzamanlılığı genel düzeyde denetlemek için kullanılamaz.

## <a name="extended-sessions"></a>Genişletilmiş oturumlar

Genişletilmiş oturumlar, iletileri işlemeyi bitirdikten sonra bile orkestrasyonları ve varlıkları bellekte tutan bir ayardır. Genişletilmiş oturumları etkinleştirmenin tipik etkisi, Azure Depolama hesabına göre G/Ç'yi azaltır ve genel olarak geliştirilmiş iş ortasıdır.

**Ana bilgisayar.json** `durableTask/extendedSessionsEnabled` `true` dosyasına ayarlayarak genişletilmiş oturumları etkinleştirebilirsiniz. Ayar, `durableTask/extendedSessionIdleTimeoutInSeconds` boşta oturumun bellekte ne kadar süreyle tutulacağını denetlemek için kullanılabilir:

**Fonksiyonlar 2.0**
```json
{
  "extensions": {
    "durableTask": {
      "extendedSessionsEnabled": true,
      "extendedSessionIdleTimeoutInSeconds": 30
    }
  }
}
```

**Fonksiyonlar 1.0**
```json
{
  "durableTask": {
    "extendedSessionsEnabled": true,
    "extendedSessionIdleTimeoutInSeconds": 30
  }
}
```

Bu ayarın farkında olması gereken iki olası dezavantajları vardır:

1. İşlev uygulaması bellek kullanımında genel bir artış var.
2. Çok eşzamanlı, kısa ömürlü orchestrator veya varlık işlevi yürütmeleri varsa, iş açısından genel bir azalma olabilir.

Örnek olarak, `durableTask/extendedSessionIdleTimeoutInSeconds` 30 saniye olarak ayarlanırsa, 1 saniyeden kısa bir süre içinde çalışan kısa ömürlü bir orkestratör veya varlık işlevi bölümü bellekte 30 saniye boyunca kalır. Ayrıca, diğer `durableTask/maxConcurrentOrchestratorFunctions` orkestratör veya varlık işlevlerinin çalışmasını engelleyerek, daha önce bahsedilen kotaya da göre sayılır.

Genişletilmiş oturumların orkestratör ve varlık işlevleri üzerindeki özel etkileri sonraki bölümlerde açıklanmıştır.

### <a name="orchestrator-function-replay"></a>Orchestrator fonksiyonu tekrar

Daha önce de belirtildiği gibi, orchestrator işlevleri **Tarih** tablosunun içeriği kullanılarak yeniden çalınır. Varsayılan olarak, bir yığın ileti denetim kuyruğundan her sırada çözülünde orchestrator işlev kodu yeniden çalınır. Fan-out, fan-in deseni kullanıyor olsanız ve tüm görevlerin tamamlanmasını bekliyor `Task.WhenAll` olsanız `context.df.Task.all` bile (örneğin, .NET'te veya JavaScript'te kullanmak), görev yanıtları toplu işlemleri zaman içinde işlenirken oluşan tekrarlar olacaktır. Genişletilmiş oturumlar etkinleştirildiğinde, orkestratör işlev örnekleri bellekte daha uzun tutulur ve yeni iletiler tam bir geçmiş tekrarı olmadan işlenebilir.

Genişletilmiş oturumların performans artışı en sık aşağıdaki durumlarda gözlenir:

* Aynı anda çalışan sınırlı sayıda düzenleme örnekleri olduğunda.
* Orkestrasyonlar çok sayıda sıralı eyleme (örn. yüzlerce etkinlik işlevi çağrısı) sahipse, bu hızla tamamlar.
* Orkestrasyonlar fan-out ve fan-aynı zamanda etrafında tamamlamak eylemleri çok sayıda.
* Orkestratör işlevleribüyük iletileri işlemek veya herhangi bir CPU yoğun veri işleme yapmak gerektiğinde.

Diğer tüm durumlarda, orkestratör işlevleri için genellikle gözlemlenebilir performans geliştirme yoktur.

> [!NOTE]
> Bu ayarlar yalnızca bir orkestratör işlevi tam olarak geliştirildikten ve test edildikten sonra kullanılmalıdır. Varsayılan agresif yeniden oynatma davranışı, geliştirme sırasında [düzenek işlev kodu kısıtlamalarını](durable-functions-code-constraints.md) algılamak için yararlı olabilir ve bu nedenle varsayılan olarak devre dışı bırakılır.

### <a name="entity-function-unloading"></a>Varlık işlevi boşaltma

Varlık işlevleri tek bir toplu iş halinde en fazla 20 işlemi işler. Bir varlık bir toplu işlemi işlemeyi bitirir bitirmez, durumunu devam eder ve bellekten boşaltır. Genişletilmiş oturumlar ayarını kullanarak varlıkların bellekten boşaltılmasını geciktirebilirsiniz. Varlıklar durum değişikliklerini eskisi gibi sürdürmeye devam ediyor, ancak Azure Depolama'dan gelen yük sayısını azaltmak için yapılandırılan süre boyunca bellekte kalıyor. Azure Depolama'daki yüklerin bu şekilde azaltılması, sık erişilen varlıkların genel iş kısmını artırabilir.

## <a name="performance-targets"></a>Performans hedefleri

Bir üretim uygulaması için Dayanıklı Fonksiyonlar kullanmayı planlarken, planlama sürecinin erken performans gereksinimlerini göz önünde bulundurmak önemlidir. Bu bölümde bazı temel kullanım senaryoları ve beklenen maksimum iş elde numaraları yer almaktadır.

* **Sıralı etkinlik yürütme**: Bu senaryo, bir dizi etkinlik işlevini birbiri ardına çalıştıran bir orkestratör işlevini açıklar. [İşlev Zincirleme](durable-functions-sequence.md) örneğine en çok benzer.
* **Paralel etkinlik yürütme**: Bu senaryo, [Fan-out, Fan-in](durable-functions-cloud-backup.md) desen kullanarak paralel olarak birçok etkinlik işlevleri yürüten bir orchestrator işlevi açıklar.
* **Paralel yanıt işleme**: Bu senaryo [Fan-out, Fan-in](durable-functions-cloud-backup.md) deseni ikinci yarısıdır. Bu fan-in performansı üzerinde duruluyor. Fan-out'un aksine, fan-in'in tek bir orchestrator işlevi örneği tarafından yapıldığını ve bu nedenle yalnızca tek bir VM'de çalışabiliyor.
* **Dış olay işleme**: Bu senaryo, [dış olayları](durable-functions-external-events.md)teker teker bekleyen tek bir orchestrator işlev örneğini temsil eder.
* **Varlık işlem işleme**: Bu senaryo, _tek_ bir [Karşı tüzel kişiliğin](durable-functions-entities.md) sabit bir işlem akışını ne kadar hızlı işleyebileceğini test ediyor.

> [!TIP]
> Fan-out aksine, fan-in işlemleri tek bir VM ile sınırlıdır. Uygulamanız fan-out, fan-in deseni kullanıyorsa ve fan-in performansı konusunda endişeleriniz varsa, etkinlik işlevini fan-out'u birden çok [alt orkestrasyon](durable-functions-sub-orchestrations.md)arasında alt bölmeyi düşünün.

Aşağıdaki tablo, daha önce açıklanan senaryolar için beklenen *maksimum* iş elde numaralarını gösterir. "Örnek", Azure Uygulama Hizmeti'nde tek bir küçük[(A1](../../virtual-machines/sizes-previous-gen.md)) VM üzerinde çalışan bir orkestratör işlevinin tek bir örneğini ifade eder. Her durumda, [genişletilmiş oturumların](#orchestrator-function-replay) etkin olduğu varsayılır. Gerçek sonuçlar, işlev kodu tarafından gerçekleştirilen CPU veya G/Ç çalışmasına bağlı olarak değişebilir.

| Senaryo | Aktarım hızı üst sınırı |
|-|-|
| Sıralı etkinlik yürütme | Saniyede 5 etkinlik, örnek başına |
| Paralel etkinlik yürütme (fan-out) | Saniyede 100 etkinlik, örnek başına |
| Paralel tepki işleme (fan-in) | Saniyede 150 yanıt, örnek başına |
| Dış olay işleme | Saniyede 50 olay, örnek başına |
| Varlık işlemi işleme | Saniyede 64 işlem |

> [!NOTE]
> Bu sayılar Dayanıklı Fonksiyonlar uzantısı v1.4.0 (GA) sürümü itibariyle geçerlidir. Bu sayılar, özellik olgunlaştıkça ve optimizasyonlar yapıldıkça zaman içinde değişebilir.

Beklediğiniz iş çıkış numaralarını görmüyorsanız ve CPU ve bellek kullanımınız sağlıklı görünüyorsa, nedenin [depolama hesabınızın durumuyla](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#troubleshooting-guidance)ilgili olup olmadığını kontrol edin. Dayanıklı Işlevler uzantısı bir Azure Depolama hesabına önemli miktarda yük getirebilir ve yeterince yüksek yükler depolama hesabının daraltlanmasına neden olabilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma ve coğrafi dağıtım hakkında bilgi edinin](durable-functions-disaster-recovery-geo-distribution.md)
