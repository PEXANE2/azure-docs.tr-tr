---
title: Azaltma sorunlarını veya '429 - Çok fazla istek' hatalarını işleme
description: Azure Mantık Uygulamaları'nda azaltma sorunları veya 'HTTP 429 Çok fazla istek' hatası yla ilgili sorunlar nasıl çözüme bulanması gerekir?
services: logic-apps
ms.suite: integration
ms.reviewer: deli, logicappspm
ms.topic: conceptual
ms.date: 04/13/2020
ms.openlocfilehash: fbfd52065bc0522668488492de2181f252f86a4e
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81272687"
---
# <a name="handle-throttling-problems-429---too-many-requests-errors-in-azure-logic-apps"></a>Azure Mantık Uygulamalarında azaltma sorunlarını (429 - "Çok fazla istek" hatası) işleme

[Azure Logic Apps'ta,](../logic-apps/logic-apps-overview.md)mantıksal uygulamanız azaltma sırasında ["HTTP 429 Çok fazla istek" hatası](https://developer.mozilla.org/docs/Web/HTTP/Status/429) döndürür ve bu hata, istek sayısı hedefin belirli bir süre içinde işleyebilir oranını aştığında gerçekleşir. Azaltma, gecikmiş veri işleme, azaltılmış performans hızı ve belirtilen yeniden deneme ilkesini aşma gibi hatalar gibi sorunlara neden olabilir.

![SQL Server konektörde azaltma](./media/handle-throttling-problems-429-errors/example-429-too-many-requests-error.png)

Mantık uygulamanızın karşılaşabileceği bazı yaygın azaltma türleri şunlardır:

* [Mantık uygulaması](#logic-app-throttling)
* [Bağlayıcı](#connector-throttling)
* [Hedef servis veya sistem](#destination-throttling)

<a name="logic-app-throttling"></a>

## <a name="logic-app-throttling"></a>Mantık uygulaması azaltma

Azure Logic Apps hizmetinin kendi [iş oluşturma limitleri](../logic-apps/logic-apps-limits-and-config.md#throughput-limits)vardır. Bu nedenle, mantık uygulamanız bu sınırları aşarsa, mantık uygulama kaynağınız sadece belirli bir örnek veya çalıştırma değil, daraltılır.

Azaltma olaylarını bu düzeyde bulmak için, mantıksal uygulamanızın Azure portalında **Ölçümler** bölmesini kontrol edin.

1. Azure [portalında](https://portal.azure.com)mantık uygulamanızı Logic App Designer'da açın.

1. Mantık uygulaması menüsünde, **İzleme**altında, **Ölçümler'i**seçin.

1. **Grafik Başlığı'nın**altında, varolana başka bir metrik eklemek için **Metrik Ekle'yi** seçin.

1. İlk metrik çubukta, **METRIC** listesinden **Eylem Daraltılmış Olaylar'ı**seçin. İkinci metrik çubukta, **METRIC** listesinden, **Tetikleme Azaltılmış Olayları**seçin.

Bu düzeyde azaltma işlemek için şu seçeneklere sahipsiniz:

* Aynı anda çalıştırılabilen mantık uygulaması örneklerinin sayısını sınırlayın.

  Varsayılan olarak, mantık uygulamanızın tetikleyici durumu aynı anda birden fazla kez karşılanırsa, mantık uygulamanız için birden çok tetikleyici örneği aynı anda veya *paralel olarak*çalışır. Bu davranış, önceki iş akışı örneği çalışma bitmeden önce her tetikleyici örneği yangınları anlamına gelir.

  Aynı anda çalıştırılabilen tetikleyici örneklerinin varsayılan sayısı [sınırsız](../logic-apps/logic-apps-limits-and-config.md#concurrency-looping-and-debatching-limits)olsa da, [tetikleyicinin eşzamanlılık ayarını açarak](../logic-apps/logic-apps-workflow-actions-triggers.md#change-trigger-concurrency)bu sayıyı sınırlayabilirsiniz ve gerekirse varsayılan değerdışında bir sınır seçebilirsiniz.

* Yüksek iş elde modunu etkinleştirin.

  Bir mantık uygulaması, [5 dakikalık bir yuvarlama aralığı üzerinde çalıştırılabilen eylem sayısı için varsayılan bir sınıra](../logic-apps/logic-apps-limits-and-config.md#throughput-limits)sahiptir. Bu sınırı maksimum eylem sayısına yükseltmek için mantık uygulamanızda [yüksek iş verimi modunu](../logic-apps/logic-apps-workflow-actions-triggers.md#run-high-throughput-mode) açın.

* Tetikleyicilerde dizi debatching ("split on") davranışını devre dışı bırakın.

  Bir tetikleyici, kalan iş akışı eylemlerinin işlemesi için bir dizi döndürürse, tetikleyicinin [ **Split On** ayarını ayarlaması](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch) dizi öğelerini böler ve her dizi öğesi için bir iş akışı örneği başlatır ve birden çok eşzamanlı çalıştırmayı Split [ **On** sınırına](../logic-apps/logic-apps-limits-and-config.md#concurrency-looping-and-debatching-limits)kadar etkin bir şekilde tetikler. Azaltmayı denetlemek **için, Split On** davranışını kapatın ve mantık uygulamanızın çağrı başına tek bir öğeyi işlemek yerine tüm diziyi tek bir çağrıyla işlemesini sorun.

* Eylemleri daha küçük mantık uygulamalarına dönüştürün.

  Daha önce de belirtildiği gibi, bir mantık uygulaması [5 dakikalık bir süre boyunca çalıştırılabilen varsayılan eylem sayısıyla](../logic-apps/logic-apps-limits-and-config.md#throughput-limits)sınırlıdır. [Yüksek iş elde modunu](../logic-apps/logic-apps-workflow-actions-triggers.md#run-high-throughput-mode)etkinleştirerek bu sınırı artırabilmenize rağmen, her bir mantık uygulamasında çalışan eylem sayısının sınırın altında kalması için mantık uygulamanızın eylemlerini daha küçük mantık uygulamalarına ayırmak isteyip istemediğiniz de düşünebilirsiniz. Bu şekilde, tek bir mantık uygulaması kaynağıüzerindeki yükü azaltır ve yükü birden çok mantık uygulamasına dağıtırsınız. Bu çözüm, büyük veri kümelerini işleyen veya aynı anda çalışan eylemleri, döngü yinelemelerini veya her döngü yinelemesi içindeki eylemleri eylem yürütme sınırını aşan eylemler için daha iyi çalışır.

  Örneğin, bu mantık uygulaması, bir SQL Server veritabanından tablo almak için tüm işi yapar ve her tablodan satırları alır. Her döngü **için** aynı anda her tablo da aynı anda, böylece **Satır lar al** eylem her tablo için satırları döndürür. Bu tablolardaki veri miktarlarına bağlı olarak, bu eylemler eylem yürütmeleri sınırını aşabilir.

  ![Mantık uygulaması "önce" refactoring](./media/handle-throttling-problems-429-errors/refactor-logic-app-before-version.png)

  Yeniden düzenlemeden sonra, mantık uygulaması artık bir ebeveyn ve alt mantık uygulamasıdır. Üst katmanlar SQL Server'dan alır ve satırları almak için her tablo için bir alt mantık uygulaması çağırır:

  ![Tek bir eylem için mantık uygulaması oluşturma](./media/handle-throttling-problems-429-errors/refactor-logic-app-single-connection-1.png)

  Her tablo için satırları almak için ana mantık uygulaması tarafından çağrılan alt mantık uygulaması aşağıda veda edebilirsiniz:

  ![İkinci bir eylem için başka bir mantık uygulaması oluşturma](./media/handle-throttling-problems-429-errors/refactor-logic-app-single-connection-2.png)

<a name="connector-throttling"></a>

## <a name="connector-throttling"></a>Konektör azaltma

Her bağlayıcının, bağlayıcının teknik başvuru sayfasında bulabileceğiniz kendi azaltma sınırları vardır. Örneğin, [Azure Hizmet Veri Çarası bağlayıcısı](https://docs.microsoft.com/connectors/servicebus/) dakikada 6.000 çağrıya izin veren bir azaltma sınırına sahipken, SQL Server bağlayıcısının işlem [türüne göre değişen azaltma limitleri](https://docs.microsoft.com/connectors/sql/)vardır.

HTTP gibi bazı tetikleyiciler ve eylemler, özel durum işleme uygulamak için [yeniden deneme ilkesi sınırlarına](../logic-apps/logic-apps-limits-and-config.md#retry-policy-limits) göre özelleştirebileceğiniz bir ["yeniden deneme ilkesi"](../logic-apps/logic-apps-exception-handling.md#retry-policies) vardır. Bu ilke, bir tetikleyici nin veya eylemin, özgün istek başarısız olduğunda veya zaman açıklandığında isteği yeniden denip etmediğini ve 408, 429 veya 5xx yanıtıyla sonuçlanıp sonuçlanmadığını belirtir. Bu nedenle, azaltma 429 hatasını başlatıp döndürdüğünde, Logic Apps desteklendiği yerde yeniden deneme ilkesini izler.

Tetikleyici veya eylemin yeniden deneme ilkelerini destekleyip desteklemediğini öğrenmek için tetikleyiciveya eylemin ayarlarını kontrol edin. Bir tetikleyicinin veya eylemin yeniden deneme denemelerini görüntülemek için, mantık uygulamanızın çalışan geçmişine gidin, gözden geçirmek istediğiniz çalıştırmayı seçin ve girişler, çıktılar ve yeniden denemeler hakkındaki ayrıntıları görüntülemek için bu tetikleyiciyi veya eylemi genişletin:

![Eylemin çalışma geçmişini, yeniden denemelerini, girişlerini ve çıktılarını görüntüleme](./media/handle-throttling-problems-429-errors/example-429-too-many-requests-retries.png)

Yeniden deneme geçmişi hata bilgileri sağlasa da, bağlayıcı azaltma ile hedef [azaltma](#destination-throttling)arasında ayrım yapmada sorun olabilir. Bu durumda, kaynağı tanımlamak için yanıtın ayrıntılarını gözden geçirmeniz veya bazı azaltma aralığı hesaplamaları gerçekleştirmeniz gerekebilir.

Küresel, çok kiracılı Azure Logic Apps hizmetindeki mantıksal uygulamalarda, azaltma *bağlantı* düzeyinde gerçekleşir. Örneğin, bir [tümleştirme hizmeti ortamında (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)çalışan mantık uygulamaları için, küresel, çok kiracılı Logic Apps hizmetinde çalıştıkları için BK dışı bağlantılariçin azaltma yine de olur. Ancak, İmKB konektörleri tarafından oluşturulan Ise bağlantıları, İmKB'nizde çalıştıkları için daraltılmış değildir.

Bu düzeyde azaltma işlemek için şu seçeneklere sahipsiniz:

* Mantık uygulamasının verileri işlemek için bölümlere ayırması için tek bir eylem için birden çok bağlantı ayarlayın.

  Bu seçenek için, bir eylemin isteklerini aynı kimlik bilgilerini kullanarak aynı hedefe birden çok bağlantıya bölerek iş yükünü dağıtıp dağıtamayacağınızı düşünün.

  Örneğin, mantık uygulamanızın bir SQL Server veritabanından tabloları aldığını ve sonra her tablodan satırları aldığını varsayalım. İşlemeniz gereken satır sayısına bağlı olarak, toplam satır sayısını işlemek üzere daha küçük kümelere bölmek için her döngü **için** birden çok bağlantı ve birden çok döngü kullanabilirsiniz. Bu senaryo, toplam satır sayısını ikiye bölmek **için her** döngü için iki döngü kullanır. Her döngü **için** ilk ilk yarısında alır bir ifade kullanır. Diğer **her** döngü için ikinci yarısında alır farklı bir ifade kullanır, örneğin:<p>

    * İfade 1: `take()` İşlev bir koleksiyonun ön kısmını alır. Daha fazla bilgi için [ **`take()`** işleve](workflow-definition-language-functions-reference.md#take)bakın.

      `@take(collection-or-array-name, div(length(collection-or-array-name), 2))`

    * İfade 2: `skip()` İşlev koleksiyonun ön kısmını kaldırır ve diğer tüm öğeleri döndürür. Daha fazla bilgi için [ **`skip()`** işleve](workflow-definition-language-functions-reference.md#skip)bakın.

      `@skip(collection-or-array-name, div(length(collection-or-array-name), 2))`

    Aşağıda, bu ifadeleri nasıl kullanabileceğinizi gösteren görsel bir örnek verilmiştir:

    ![Tek bir eylem için birden çok bağlantı oluşturma ve kullanma](./media/handle-throttling-problems-429-errors/create-multiple-connections-per-action.png)

* Her eylem için farklı bir bağlantı kurun.

  Bu seçenek için, eylemler aynı hizmete veya sisteme bağlanıp aynı kimlik bilgilerini kullansa bile, her eylemin isteklerini kendi bağlantıları üzerinden yayarak iş yükünü dağıtıp dağıtamayacağınızı düşünün.

  Örneğin, mantık uygulamanızın tabloları bir SQL Server veritabanından aldığını ve her tablodaki her satırı aldığını varsayalım. Tablolar alma nın bir bağlantı kullanması için ayrı bağlantılar kullanabilirsiniz, her satırı almak ise başka bir bağlantı kullanır.

  ![Her eylem için farklı bağlantılar oluşturma ve kullanma](./media/handle-throttling-problems-429-errors/create-connection-per-action.png)

* ["Her biri için" döngüsünde](../logic-apps/logic-apps-control-flow-loops.md#foreach-loop)eşzamanlılık veya paralellik değiştirin.

  Varsayılan olarak, "Her" döngü yinelemeleri [eşzamanlılık sınırına](../logic-apps/logic-apps-limits-and-config.md#looping-debatching-limits)kadar aynı anda çalışır. "Her biri için" döngü içinde daraltılmış bir bağlayıcınız varsa, paralel çalışan döngü yinelemelerinin sayısını azaltabilirsiniz. Daha fazla bilgi için şu konulara bakın:
  
  * ["Her" döngüiçin - eşzamanlılığı değiştirin veya sırayla çalıştırın](../logic-apps/logic-apps-control-flow-loops.md#sequential-foreach-loop)

  * [İş Akışı Tanımı Dil şeması - Her döngü için](../logic-apps/logic-apps-workflow-actions-triggers.md#foreach-action)

  * [İş Akışı Tanımı Dil şeması - "Her biri için" döngü eşzamanlılık değiştirin](../logic-apps/logic-apps-workflow-actions-triggers.md#change-for-each-concurrency)

  * [İş Akışı Tanımı Dil şeması - "Her biri için" döngüleri sırayla çalıştırın](../logic-apps/logic-apps-workflow-actions-triggers.md#sequential-for-each)

<a name="destination-throttling"></a>

## <a name="destination-service-or-system-throttling"></a>Hedef hizmeti veya sistem azaltma

Bir bağlayıcının kendi azaltma sınırları olsa da, bağlayıcı tarafından çağrılan hedef hizmet veya sistem de azaltma sınırları olabilir. Örneğin, Microsoft Exchange Server'daki bazı API'lerin Office 365 Outlook bağlayıcısı daha sıkı azaltma sınırlamaları vardır.

Varsayılan olarak, bir mantık uygulamasıörnekleri ve bu örneklerin içindeki döngüler veya dallar *paralel olarak*çalıştırın. Bu davranış, birden çok örneğin aynı anda aynı bitiş noktasını çağırabileceği anlamına gelir. Her örnek diğerinin varlığından anlama geldiğinden, başarısız eylemleri yeniden deneme girişimleri birden çok çağrının aynı anda çalıştırmaya çalıştığı [yarış koşulları](https://en.wikipedia.org/wiki/Race_condition) oluşturabilir, ancak başarılı olmak için, bu çağrıların azaltma gerçekleşmeden önce hedef servise veya sisteme ulaşması gerekir.

Örneğin, 100 öğeiçeren bir diziniz olduğunu varsayalım. Dizide yinelemek için "her biri için" döngüsü kullanırsınız ve paralel yineleme sayısını 20 veya [geçerli varsayılan sınırla](../logic-apps/logic-apps-limits-and-config.md#concurrency-looping-and-debatching-limits)sınırlandırabilmek için döngünün eşzamanlılık denetimini açarsınız. Bu döngüiçinde, bir eylem diziden bir öğeyi saniyede yalnızca 15 çağrıya izin veren bir SQL Server veritabanına ekler. Bu senaryo, bir birikim biriktirme listesi birikmesi ve hiçbir zaman çalıştırılamadığı için azaltma sorunuyla sonuçlanır.

Bu tablo, eylemin yeniden deneme aralığı 1 saniye olduğunda döngüde ne olacağının zaman çizelgesini açıklar:

| Zaman içinde nokta | Çalışan eylem sayısı | Başarısız olan eylem sayısı | Bekleyen yeniden deneme sayısı |
|---------------|----------------------------|-----------------------------|---------------------------|
| T + 0 saniye | 20 uç | 5 başarısız, SQL sınırı nedeniyle | 5 yeniden deneme |
| T + 0.5 saniye | 15 ekler, önceki 5 yeniden denemeler bekliyor nedeniyle | Önceki SQL limiti nedeniyle 15 başarısız | 20 yeniden deneme <br>(önceki 5 + 15 yeni) |
| T + 1 saniye | 20 uç | 5 başarısız artı önceki 20 yeniden deneme, SQL sınırı nedeniyle | 25 yeniden deneme (önceki 20 + 5 yeni)
|||||

Bu düzeyde azaltma işlemek için şu seçeneklere sahipsiniz:

* Her biri tek bir işlemi işleyebilsin diye mantık uygulamaları oluşturun.

  * Bu bölümdeki örnek SQL Server senaryosuyla devam ederek, dizi öğelerini kuyruğa koyan bir mantıksal uygulama oluşturabilirsiniz, örneğin [Azure Hizmet Veri Servisi kuyruğu.](../connectors/connectors-create-api-servicebus.md) Daha sonra, bu kuyruktaki her öğe için yalnızca ekleme işlemini gerçekleştiren başka bir mantık uygulaması oluşturursunuz. Bu şekilde, yalnızca bir mantık uygulaması örneği herhangi bir zamanda çalışır, bu da ekleme işlemini tamamlar ve kuyruktaki bir sonraki öğeye taşınır veya örnek 429 hata alır, ancak verimsiz yeniden denemegirişiminde bulunmaz.

  * Her eylem için bir alt öğeyi veya iç içe geçen mantık uygulamasını çağıran bir üst mantık uygulaması oluşturun. Üst öğenin, üst öğenin sonucuna göre farklı alt uygulamaları araması gerekiyorsa, hangi alt uygulamayı arayacağını belirleyen bir koşul eylemi veya geçiş eylemi kullanabilirsiniz. Bu desen, arama veya işlem sayısını azaltmanıza yardımcı olabilir.

    Örneğin, her biri e-posta hesabınızı "Başarı" veya "Hata" gibi belirli bir konu için her dakika kontrol eden bir yoklama tetikleyicisi olan iki mantık uygulamanız olduğunu varsayalım. Bu kurulum saatte 120 arama yla sonuçlanır. Bunun yerine, her dakika anket yapan ancak öznenin "Başarı" veya "Hata" olmasına göre çalışan bir alt mantık uygulaması çağıran tek bir ana mantık uygulaması oluşturursanız, bu durumda yoklama denetimlerinin sayısını yarıya veya 60'a düşürür.

* Toplu işişlemeyi ayarlayın.

  Hedef servis toplu işoperasyonlarını destekliyorsa, maddeleri tek tek değil, gruplar halinde veya toplu işlerde işleyerek azaltmayı giderebilirsiniz. Toplu işleme çözümlerini uygulamak için bir "toplu alıcı" mantık uygulaması ve bir "toplu gönderen" mantık uygulaması oluşturursunuz. Toplu işlem gönderen, belirtilen ölçütleriniz karşılanana kadar iletileri veya öğeleri toplar ve bu iletileri veya öğeleri tek bir grupta gönderir. Toplu işlem alıcısı bu grubu kabul eder ve bu iletileri veya öğeleri işler. Daha fazla bilgi için gruplar [halinde toplu işlem iletileri](../logic-apps/logic-apps-batch-process-send-receive-messages.md)bakın.

* Webhook sürümlerini yoklama sürümleri yerine tetikleyiciler ve eylemler için kullanın.

  Neden? Bir yoklama tetikleyicisi hedef hizmeti veya sistemi belirli aralıklarla denetlemeye devam eder. Her saniye gibi çok sık bir aralık, azaltma sorunları oluşturabilir. Ancak, [http Webhook](../connectors/connectors-native-webhook.md)gibi bir webhook tetikleyicisi veya eylemi, hedef hizmete veya sisteme yalnızca tek bir çağrı oluşturur, bu çağrı abonelik zamanında gerçekleşir ve hedefin yalnızca bir olay olduğunda tetikleyiciyi veya eylemi iletmesini ister. Bu şekilde, tetikleyici veya eylem sürekli hedef kontrol etmek zorunda değildir.
  
  Bu nedenle, hedef hizmet veya sistem webhooks'u destekliyorsa veya webhook sürümüne sahip bir bağlayıcı sağlıyorsa, bu seçenek yoklama sürümünü kullanmaktan daha iyidir. Webhook tetikleyicilerini ve eylemlerini tanımlamak için, `ApiConnectionWebhook` bu türe sahip olduklarını veya bir yineleme belirtmenizi gerektirmediklerini doğrulayın. Daha fazla bilgi için [APIConnectionWebhook tetikleyicisi](../logic-apps/logic-apps-workflow-actions-triggers.md#apiconnectionwebhook-trigger) ve [APIConnectionWebhook eylemine](../logic-apps/logic-apps-workflow-actions-triggers.md#apiconnectionwebhook-action)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Logic Apps sınırları ve yapılandırması](../logic-apps/logic-apps-limits-and-config.md) hakkında daha fazla bilgi edinin
