---
title: Azure Mantık Uygulamaları için web API'leri & REST API'leri oluşturun
description: Azure Logic Apps'taki sistem tümleştirmeleri için API'lerinizi, hizmetlerinizi veya sistemlerinizi aramak için WEB API'leri & REST API'leri oluşturun
services: logic-apps
ms.suite: integration
ms.reviewer: klam, jehollan, logicappspm
ms.topic: article
ms.date: 05/26/2017
ms.openlocfilehash: bb6c99ea12e5b53631d42a04b36b7bfef2337e42
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270543"
---
# <a name="create-custom-apis-you-can-call-from-azure-logic-apps"></a>Azure Logic Apps'tan arayaabileceğiniz özel API'ler oluşturun

Azure Logic Apps, mantık uygulaması iş akışlarında kullanabileceğiniz [yüzlerce bağlayıcı](../connectors/apis-list.md) sunsa da, bağlayıcı olarak kullanılamayan API'leri, sistemleri ve hizmetleri aramak isteyebilirsiniz. Mantık uygulamalarında kullanmak üzere eylemler ve tetikleyiciler sağlayan kendi API'lerinizi oluşturabilirsiniz. Mantık uygulaması iş akışlarından arayabilirsiniz kendi API'lerinizi oluşturmak isteyebileceğiniz diğer nedenler şunlardır:

* Geçerli sistem tümleştirmenizi ve veri tümleştirme iş akışlarınızı genişletin.
* Müşterilerin profesyonel veya kişisel görevleri yönetmek için hizmetinizi kullanmasına yardımcı olun.
* Erişimini, keşfedilebilirliğini ve hizmetiniz için kullanımı genişletin.

Temel olarak, bağlayıcılar takılabilir arayüzler için REST kullanan web API'leri, dokümantasyon için [Swagger meta veri biçimi](https://swagger.io/specification/) ve veri alışverişi biçimi olarak JSON'dur. Bağlayıcılar, HTTP uç noktaları üzerinden iletişim kuran REST API'leri olduğundan, bağlayıcılar oluşturmak için .NET, Java, Python veya Node.js gibi herhangi bir dili kullanabilirsiniz. API barındırma için en iyi, en kolay ve en ölçeklenebilir yollardan birini sunan hizmet olarak platform (PaaS) sunan [Azure Uygulama Hizmeti'nde](../app-service/overview.md)API'lerinizi de barındırabilirsiniz. 

Özel API'ların mantık uygulamalarıyla çalışması için API'niz, mantık uygulaması iş akışlarında belirli görevleri gerçekleştiren [*eylemler*](./logic-apps-overview.md#logic-app-concepts) sağlayabilir. API'niz, yeni veriler veya bir olay belirli bir koşulu karşıladığında bir mantık uygulaması iş akışı başlatan bir [*tetikleyici*](./logic-apps-overview.md#logic-app-concepts) olarak da hareket edebilir. Bu konu, API'nizin sağlamasını istediğiniz davranışa bağlı olarak API'nizde eylemler ve tetikleyiciler oluşturmak için izleyebileceğiniz ortak desenleri açıklar.

API'lerinizi, yüksek ölçeklenebilir ve kolay API barındırma sağlayan hizmet olarak platform (PaaS) sunan [Azure Uygulama Hizmeti'nde](../app-service/overview.md)barındırabilirsiniz.

> [!TIP] 
> API'lerinizi web uygulamaları olarak dağıtabiliyor olsanız da, API'lerinizi API uygulamaları olarak dağıtmayı düşünün, bu da bulutta ve şirket içinde API'ler oluşturduğunuzda, barındırırken ve tükettiğinizde işinizi kolaylaştırabilir. API'lerinizdeki hiçbir kodu değiştirmeniz gerekmesin , kodunuzu bir API uygulamasına dağıtmanız gerekir. Örneğin, bu dillerle oluşturulan API uygulamalarını nasıl oluşturarak nasıl oluşturabildiğini öğrenin: 
> 
> * [ASP.NET](../app-service/app-service-web-get-started-dotnet.md). 
> * [Java](../app-service/app-service-web-get-started-java.md)
> * [Node.js](../app-service/app-service-web-get-started-nodejs.md)
> * [PHP](../app-service/app-service-web-get-started-php.md)
> * [Python](../app-service/containers/quickstart-python.md)
> * [Ruby](../app-service/containers/quickstart-ruby.md)
>
> Mantık uygulamaları için oluşturulmuş API Uygulaması örnekleri için [Azure Logic Apps GitHub deposunu](https://github.com/logicappsio) veya [blogunu](https://aka.ms/logicappsblog)ziyaret edin.

## <a name="how-do-custom-apis-differ-from-custom-connectors"></a>Özel API'lerin özel konektörlerden farkı nedir?

Özel API'ler ve [özel bağlayıcılar,](../logic-apps/custom-connector-overview.md) takılabilir arabirimler için REST kullanan web API'leri, dokümantasyon için [Swagger meta veri biçimini](https://swagger.io/specification/) ve veri alışverişi biçimi olarak JSON'dur. Bu API'ler ve bağlayıcılar HTTP uç noktaları üzerinden iletişim kuran REST API'leri olduğundan, özel API'ler ve bağlayıcılar oluşturmak için .NET, Java, Python veya Node.js gibi herhangi bir dili kullanabilirsiniz.

Özel API'ler, bağlayıcı olmayan API'leri aramanıza ve HTTP + Swagger, Azure API Yönetimi veya Uygulama Hizmetleri ile arayaabileceğiniz uç noktaları sağlamanıza izin tanır. Özel bağlayıcılar özel API'ler gibi çalışır, ancak aynı zamanda şu özniteliklere sahiptir:

* Azure'da Logic Apps Bağlayıcısı kaynakları olarak kaydedilir.
* Logic Apps Tasarımcısı'nda Microsoft tarafından yönetilen konektörlerin yanında simgelerle görünün.
* Yalnızca, mantıksal uygulamaların dağıtıldığı bölgede aynı Azure Active Directory kiracısına ve Azure aboneliğine sahip bağlayıcıların yazarları ve mantıksal uygulama kullanıcıları tarafından kullanılabilir.

Microsoft sertifikası için kayıtlı bağlayıcıları da aday gösterebilirsiniz. Bu işlem, kayıtlı bağlayıcıların genel kullanım ölçütlerini karşıladığını doğrular ve bu bağlayıcıları Power Automate ve Microsoft Power Apps'taki kullanıcılar için kullanılabilir hale getirir.

Özel bağlayıcılar hakkında daha fazla bilgi için bkz. 

* [Özel bağlayıcılara genel bakış](../logic-apps/custom-connector-overview.md)
* [Web API'larından özel bağlayıcılar oluşturma](../logic-apps/custom-connector-build-web-api-app-tutorial.md)
* [Azure Mantık Uygulamaları'na özel bağlayıcılar kaydedin](../logic-apps/logic-apps-custom-connector-register.md)

## <a name="helpful-tools"></a>Yararlı araçlar

ÖZEL API, API'nin işlemlerini ve parametrelerini açıklayan bir [Swagger belgesine](https://swagger.io/specification/) sahipse, mantık uygulamalarıyla en iyi şekilde çalışır.
[Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)gibi birçok kitaplık, otomatik olarak sizin için Swagger dosyaoluşturabilirsiniz. Ekran adları, özellik türleri ve benzeri için Swagger dosyasına açıklama yapmak için, Swagger dosyanızın mantık uygulamalarıyla iyi çalışması için [TRex'i](https://github.com/nihaue/TRex) de kullanabilirsiniz.

<a name="actions"></a>

## <a name="action-patterns"></a>Eylem desenleri

Mantık uygulamalarının görevleri gerçekleştirmesi için özel API'nizin [*eylemler*](./logic-apps-overview.md#logic-app-concepts)sağlaması gerekir. API'nizdeki her işlem bir eylemi eşler. Temel eylem, HTTP isteklerini kabul eden ve HTTP yanıtlarını döndüren bir denetleyicidir. Örneğin, bir mantık uygulaması web uygulamanıza veya API uygulamanıza bir HTTP isteği gönderir. Uygulamanız daha sonra, mantık uygulamasının işleyebilir içeriğiile birlikte bir HTTP yanıtı döndürür.

Standart bir eylem için, API'nize bir HTTP istek yöntemi yazabilir ve bu yöntemi bir Swagger dosyasında açıklayabilirsiniz. Daha sonra API'nizi doğrudan bir [HTTP eylemi](../connectors/connectors-native-http.md) veya http [+ Swagger](../connectors/connectors-native-http-swagger.md) eylemiyle arayabilirsiniz. Varsayılan olarak, yanıtlar istek [zaman sınırı](./logic-apps-limits-and-config.md)içinde döndürülmelidir. 

![Standart eylem deseni](./media/logic-apps-create-api-app/standard-action.png)

<a name="pattern-overview"></a>Bir mantık uygulamasını, API'niz daha uzun süren görevleri bitirirken beklemek için, API'niz bu konuda açıklanan [eşzamanlı yoklama deseni](#async-pattern) veya eşzamanlı [webhook deseni](#webhook-actions) izleyebilir. Bu desenlerin farklı davranışlarını görselleştirmenize yardımcı olan bir benzetme için, bir pastaneden özel pasta sipariş etme işlemini hayal edin. Yoklama deseni, pastanın hazır olup olmadığını kontrol etmek için her 20 dakikada bir fırını aradığınız davranışı yansıtıyor. Webhook deseni, pasta hazır olduğunda sizi arayabilmeleri için fırının sizden telefon numaranızı istediği davranışı yansıtıyor.

Örnekler için [Logic Apps GitHub deposunu](https://github.com/logicappsio)ziyaret edin. Ayrıca, eylemler [için kullanım ölçümü](logic-apps-pricing.md)hakkında daha fazla bilgi edinin.

<a name="async-pattern"></a>

### <a name="perform-long-running-tasks-with-the-polling-action-pattern"></a>Yoklama eylem deseniyle uzun süren görevleri gerçekleştirin

API'nizin [istek zaman aşımı sınırından](./logic-apps-limits-and-config.md)daha uzun süre çalıştırılabilen görevleri gerçekleştirmesi için, eşzamanlı yoklama deseni kullanabilirsiniz. Bu desen, API'nizin ayrı bir iş parçacığında çalışmasını sağlar, ancak Logic Apps altyapısına etkin bir bağlantı tutun. Bu şekilde, mantıksal uygulama, API'niz çalışmayı tamamlamadan önce iş akışındaki bir sonraki adıma zaman vermez veya devam etmez.

İşte genel desen:

1. Motorun, API'nizin isteği kabul ettiğini ve çalışmaya başladığını bildiğinden emin olun.
2. Motor sonraki iş durumu isteklerini yaptığında, API'niz görevi bitirdiğinde motora bildirin.
3. Mantık uygulaması iş akışının devam edilebilmeleri için ilgili verileri motora döndürün.

<a name="bakery-polling-action"></a>Şimdi yoklama desenine önceki fırın benzetmesini uygulayın ve bir fırını çağırıp teslimat için özel bir pasta sipariş ettiğinizi düşünün. Pasta yapma işlemi zaman alır ve fırın pasta üzerinde çalışırken telefonda beklemek istemiyorum. Fırın siparişinizi onaylar ve pastanın durumu için her 20 dakikada bir aramanızı sağlar. 20 dakika geçtikten sonra fırını ararsın ama pastanızın bitmediğini ve 20 dakika içinde aramanız gerektiğini söylerler. Bu ileri geri görüşme işlemi siz arayana kadar devam eder ve fırın size siparişinizin hazır olduğunu söyler ve pastanızı teslim eder. 

Bu anket düzenini tekrar haritalayalım. Pasta, özel API'nizi temsil ederken, siz, pasta müşterisi, Logic Apps motorunu temsil esiniz. Motor API'nizi bir istekle aradığında, API'niz isteği onaylar ve motorun iş durumunu kontrol edebileceği zaman aralığıyla yanıt verir. Ekinleriniz işin tamamlandığı yanıtını verene ve verileri mantık uygulamanıza döndürene ve ardından iş akışına devam edene kadar motor iş durumunu denetlemeye devam eder. 

![Yoklama eylem deseni](./media/logic-apps-create-api-app/custom-api-async-action-pattern.png)

API'nizin izleyeceği özel adımlar, API'nin bakış açısından açıklanmıştır:

1. API'niz çalışmaya başlamak için bir HTTP isteği `202 ACCEPTED` aldığında, `location` bu adımda daha sonra açıklanan üstbilgiyle hemen bir HTTP yanıtı döndürün. Bu yanıt, Logic Apps motorunun API'nizin isteği aldığını, istek yükünü (veri girişi) kabul ettiğini ve şu anda işlenmekte olduğunu bilmesini sağlar. 
   
   Yanıt `202 ACCEPTED` şu üstbilgiiçermelidir:
   
   * *Gerekli*: `location` Logic Apps altyapısının API'nizin iş durumunu kontrol edebileceği bir URL'ye giden mutlak yolu belirten üstbilgi

   * *İsteğe* `retry-after` Bağlı : İş durumu için `location` URL'yi kontrol etmeden önce motorun beklemesi gereken saniye sayısını belirten üstbilgi. 

     Varsayılan olarak, motor her 20 saniyede bir kontrol eder. Farklı bir aralık belirtmek `retry-after` için üstbilgi ve bir sonraki ankete kadar saniye sayısını ekleyin.

2. Belirtilen süre geçtikten sonra, Logic `location` Apps altyapısı iş durumunu denetlemek için URL'yi yoklar. API'niz bu denetimleri gerçekleştirmeli ve şu yanıtları döndürmelidir:
   
   * İş yapılırsa, yanıt `200 OK` yüküyle birlikte bir HTTP yanıtı döndürün (bir sonraki adım için giriş).

   * İş hala işliyorsa, başka `202 ACCEPTED` bir HTTP yanıtı döndürün, ancak özgün yanıtla aynı üstbilgiyle.

API'niz bu deseni izlediğinde, iş durumunu denetlemeye devam etmek için mantık uygulaması iş akışı tanımında hiçbir şey yapmanız gerekir. Motor bir HTTP `202 ACCEPTED` yanıtı ve `location` geçerli bir üstbilgi aldığında, motor asenkron `location` desenine saygı duyar ve API'niz 202 olmayan bir yanıtı döndürene kadar üstbilgikontrol eder.

> [!TIP]
> Örneğin bir eşzamanlı desen için, [GitHub](https://github.com/logicappsio/LogicAppsAsyncResponseSample)bu eşzamanlı denetleyici yanıt örneği gözden geçirin.

<a name="webhook-actions"></a>

### <a name="perform-long-running-tasks-with-the-webhook-action-pattern"></a>Webhook eylem deseniyle uzun süren görevleri gerçekleştirin

Alternatif olarak, uzun süren görevler ve eşzamanlı işleme için webhook deseni kullanabilirsiniz. Bu desen mantık uygulaması duraklatma ve devam iş akışı önce işleme bitirmek için API bir "geri arama" bekleyin. Bu geri arama, bir olay olduğunda URL'ye ileti gönderen bir HTTP POST'dur. 

<a name="bakery-webhook-action"></a>Şimdi webhook desen önceki fırın benzetmesi uygulayın ve bir fırın aramak ve teslimat için özel bir pasta sipariş düşünün. Pasta yapma işlemi zaman alır ve fırın pasta üzerinde çalışırken telefonda beklemek istemiyorum. Fırın siparişinizi onaylıyor, ama bu sefer onlara telefon numaranızı verin ki pasta bittiğinde sizi arayabilsinler. Bu kez, fırın sipariş hazır olduğunda söyler ve pasta teslim eder.

Bu webhook desenini geri eşlediğimizde, fırın özel API'nizi temsil ederken, siz, pasta müşterisi, Logic Apps motorunu temsil esiniz. Motor API'nizi bir istekle çağırır ve bir "geri arama" URL'si içerir.
İş bittiğinde, API'niz motoru bilgilendirmek ve verileri mantık uygulamanıza döndürmek için URL'yi kullanır ve bu da iş akışına devam edin. 

Bu desen için, denetleyicinizde iki uç `subscribe` nokta ayarlayın:`unsubscribe`

*  `subscribe`bitiş noktası: Yürütme, API'nizin iş akışındaki eylemine ulaştığında, Logic Apps altyapısı `subscribe` bitiş noktasını çağırır. Bu adım, mantık uygulamasının API'nizin depoladığını bir geri arama URL'si oluşturmasına ve çalışma tamamlandığında API'nizden geri çağrıyı beklemesine neden olur. API'niz daha sonra URL'ye bir HTTP POST ile geri çağırır ve döndürülen tüm içeriği ve üstbilgileri mantık uygulamasına giriş olarak geçirir.

* `unsubscribe`bitiş noktası: Mantık uygulaması çalışması iptal edilirse, Logic `unsubscribe` Apps motoru bitiş noktasını çağırır. Ekin'iniz daha sonra geri arama URL'sinin kaydını açabilir ve gerekli işlemleri durdurabilir.

![Webhook eylem deseni](./media/logic-apps-create-api-app/custom-api-webhook-action-pattern.png)

> [!NOTE]
> Şu anda, Logic App Designer Swagger üzerinden webhook uç noktaları keşfetmek desteklemiyor. Bu desen için, bir [ **Webhook** eylemi](../connectors/connectors-native-webhook.md) eklemeniz ve isteğiniz için URL'yi, üstbilgi ve gövdeyi belirtmeniz gerekir. [Ayrıca](logic-apps-workflow-actions-triggers.md#apiconnection-webhook-action)bkz. Geri arama URL'sini geçirmek için, `@listCallbackUrl()` önceki alanlardan herhangi birinde iş akışı işlevini gerektiği gibi kullanabilirsiniz.

> [!TIP]
> Örnek bir webhook deseni için, GitHub bu [webhook tetikleme örneği](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs)gözden geçirin.

<a name="triggers"></a>

## <a name="trigger-patterns"></a>Tetik leme desenleri

Özel API'niz, yeni veriler veya bir olay belirli bir koşulu karşıladığında bir mantık uygulaması başlatan bir [*tetikleyici*](./logic-apps-overview.md#logic-app-concepts) görevi görebilir. Bu tetikleyici, hizmet bitiş noktanızda yeni veriler veya olaylar için düzenli olarak denetleyebilir veya bekleyip dinleyebilir. Yeni veriler veya bir olay belirtilen koşulu karşılıyorsa, tetikleyici tetiklenir ve tetikleyiciyi dinleyen mantık uygulamasını başlatır. Mantık uygulamalarını bu şekilde başlatmak için API'niz [*yoklama tetikleyicisini*](#polling-triggers) veya [*webhook tetikleyici*](#webhook-triggers) deseni izleyebilir. Bu desenler [yoklama eylemleri](#async-pattern) ve [webhook eylemleri](#webhook-actions)için muadillerine benzer. Ayrıca, [tetikleyiciler için kullanım ölçümü](logic-apps-pricing.md)hakkında daha fazla bilgi edinin.

<a name="polling-triggers"></a>

### <a name="check-for-new-data-or-events-regularly-with-the-polling-trigger-pattern"></a>Yoklama tetikleyici deseniyle düzenli olarak yeni verileri veya olayları denetleme

Bir *yoklama* tetikleyicisi, daha önce bu konuda açıklanan [yoklama eylemine](#async-pattern) çok benzer hareket eder. Logic Apps altyapısı, yeni veriler veya olaylar için tetikleyici bitiş noktasını düzenli aralıklarla arar ve denetler. Motor yeni veriler veya belirtilen koşulunkarşıladığı bir olay bulursa, tetikleyici devreye sayılsa. Daha sonra, motor verileri giriş olarak işleyen bir mantık uygulaması örneği oluşturur. 

![Yoklama tetikleyici deseni](./media/logic-apps-create-api-app/custom-api-polling-trigger-pattern.png)

> [!NOTE]
> Her yoklama isteği, hiçbir mantık uygulaması örneği oluşturulmasa bile bir eylem yürütmesi olarak sayılır. Aynı verilerin birden çok kez işlenmesini önlemek için tetikleyiciniz, zaten okunmuş ve mantık uygulamasına aktarılan verileri temizlemelidir.

API'nin bakış açısından açıklanan bir yoklama tetikleyicisi için belirli adımlar şunlardır:

| Yeni veri veya olay mı buldu?  | API yanıtı | 
| ------------------------- | ------------ |
| Bulundu | Yanıt yüküyle bir HTTP `200 OK` durumu döndür (sonraki adım için giriş). <br/>Bu yanıt bir mantık uygulaması örneği oluşturur ve iş akışını başlatır. | 
| Bulunamadı | Üstbilgi `202 ACCEPTED` ve `retry-after` üstbilgi yle bir HTTP durumunu döndürün. `location` <br/>Tetikleyiciler için `location` üstbilgi, genellikle `triggerState` "zaman damgası" olan bir sorgu parametresi de içermelidir. API'niz, mantık uygulamasının en son ne zaman tetiklediğini izlemek için bu tanımlayıcıyı kullanabilir. | 
||| 

Örneğin, hizmetinizi yeni dosyalar için düzenli aralıklarla denetlemek için, şu davranışlara sahip bir yoklama tetikleyicisi oluşturabilirsiniz:

| İstek `triggerState`içerir? | API yanıtı | 
| -------------------------------- | -------------| 
| Hayır | Bir HTTP `202 ACCEPTED` durumunun `location` yanı `triggerState` sıra geçerli saate `retry-after` ve aralıkta 15 saniyeye ayarlanmış bir üstbilgi döndürün. | 
| Evet | Için'den sonra eklenen `DateTime` dosyalar `triggerState`için servisinizi kontrol edin. | 
||| 

| Bulunan dosya sayısı | API yanıtı | 
| --------------------- | -------------| 
| Tek dosya | Bir HTTP `200 OK` durumunu ve içerik yükünü `triggerState` döndürün, döndürülen dosyaiçin `DateTime` güncelleştirin ve aralığı 15 saniyeye ayarlayın. `retry-after` | 
| Birden çok dosya | Bir defada bir dosya `200 OK` döndürün `triggerState`ve bir `retry-after` HTTP durumunu güncelleştirin ve aralığı 0 saniyeye ayarlayın. </br>Bu adımlar, motora daha fazla veri kullanılabildiğini ve motorun üstbilgideki `location` URL'den verileri hemen istemesi gerektiğini bildirin. | 
| Dosya yok | Bir HTTP `202 ACCEPTED` durumunu döndürün, `triggerState`değiştirmeyin `retry-after` ve aralığı 15 saniyeolarak ayarlayın. | 
||| 

> [!TIP]
> Örneğin yoklama tetikleyici deseni için, [GitHub'daki](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/PollTriggerController.cs)bu anket tetikleyici denetleyici örneğini gözden geçirin.

<a name="webhook-triggers"></a>

### <a name="wait-and-listen-for-new-data-or-events-with-the-webhook-trigger-pattern"></a>Webhook tetikleme deseni yle yeni verileri veya olayları bekleyin ve dinleyin

Webhook tetikleyicisi, hizmetinizdeki son noktada yeni verileri veya olayları bekleyen ve dinleyen bir *itme* tetikleyicisidir. Yeni veri veya olay belirtilen koşulu karşılıyorsa, tetikleyici yangınları ve daha sonra giriş olarak veri işleyen bir mantık uygulaması örneği oluşturur.
Webhook tetikleyicileri daha önce bu konuda açıklanan [webhook eylemleri](#webhook-actions) gibi `subscribe` hareket `unsubscribe` ve ile ayarlanır ve uç noktalar. 

* `subscribe`bitiş noktası: Mantık uygulamanızda bir webhook tetikleyicisi ekleyip kaydettiğinizde, Logic Apps motoru `subscribe` bitiş noktasını çağırır. Bu adım, mantık uygulamasının API'nizin depoladığını bir geri arama URL'si oluşturmasına neden olur. Yeni veriler veya belirtilen koşulu karşılayan bir olay olduğunda, API'niz URL'ye bir HTTP POST ile geri çağrır. İçerik yükü ve üstbilgi, mantık uygulamasına giriş olarak geçer.

* `unsubscribe`bitiş noktası: Webhook tetikleyicisi veya tüm mantık uygulaması silinirse, `unsubscribe` Logic Apps motoru bitiş noktasını çağırır. Ekin'iniz daha sonra geri arama URL'sinin kaydını açabilir ve gerekli işlemleri durdurabilir.

![Webhook tetikleme deseni](./media/logic-apps-create-api-app/custom-api-webhook-trigger-pattern.png)

> [!NOTE]
> Şu anda, Logic App Designer Swagger üzerinden webhook uç noktaları keşfetmek desteklemiyor. Bu desen için, bir [ **Webhook** tetikleyici](../connectors/connectors-native-webhook.md) si eklemeniz ve isteğiniz için URL'yi, üstbilgi ve gövdeyi belirtmeniz gerekir. Ayrıca bakınız [HTTPWebhook tetikleyici](logic-apps-workflow-actions-triggers.md#httpwebhook-trigger). Geri arama URL'sini geçirmek için, `@listCallbackUrl()` önceki alanlardan herhangi birinde iş akışı işlevini gerektiği gibi kullanabilirsiniz.
>
> Aynı verilerin birden çok kez işlenmesini önlemek için tetikleyiciniz, zaten okunmuş ve mantık uygulamasına aktarılan verileri temizlemelidir.

> [!TIP]
> Örnek bir webhook deseni için, GitHub bu [webhook tetik denetleyicisi örnek](https://github.com/logicappsio/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs)gözden geçirin.

## <a name="improve-security-for-calls-to-your-apis-from-logic-apps"></a>Mantık uygulamalarından API'lerinize yapılan aramalar için güvenliği artırın

Özel API'lerinizi oluşturduktan sonra, API'leriniz için kimlik doğrulamasını ayarlayın, böylece bunları mantık uygulamalarından güvenli bir şekilde arayabilirsiniz. [Mantık uygulamalarından özel API'lere yapılan aramalariçin güvenliği nasıl artırılamayı](../logic-apps/logic-apps-custom-api-authentication.md)öğrenin.

## <a name="deploy-and-call-your-apis"></a>API'lerinizi dağıtın ve arayın

Kimlik doğrulaması ayarladıktan sonra API'leriniz için dağıtım ayarlayın. [Mantık uygulamalarından özel API'leri nasıl dağıtıp çağırılamayı](../logic-apps/logic-apps-custom-api-host-deploy-call.md)öğrenin.

## <a name="publish-custom-apis-to-azure"></a>Azure'da özel API'ler yayımlama

Azure'daki diğer Logic Apps kullanıcıları için özel API'lerinizi kullanılabilir hale getirmek için güvenlik eklemeniz ve bunları Logic App bağlayıcıları olarak kaydetmeniz gerekir. Daha fazla bilgi için bkz. [Özel bağlayıcılara genel bakış](../logic-apps/custom-connector-overview.md). 

Özel API'lerinizi Logic Apps, Power Automate ve Microsoft Power Apps'taki tüm kullanıcılar için kullanılabilir hale getirmek için güvenlik eklemeniz, API'lerinizi Logic App bağlayıcıları olarak kaydetmeniz ve bağlayıcılarınızı [Microsoft Azure Sertifikalı programı](https://azure.microsoft.com/marketplace/programs/certified/logic-apps/)için aday göstermeniz gerekir. 

## <a name="get-support"></a>Destek alın

* Özel API'lerle ilgili [customapishelp@microsoft.com](mailto:customapishelp@microsoft.com)özel yardım için, '.

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

* Logic Apps’in geliştirilmesine yardımcı olmak için, [Logic Apps kullanıcı geri bildirim sitesinde](https://aka.ms/logicapps-wish) oy kullanın veya fikirlerinizi paylaşın. 

## <a name="next-steps"></a>Sonraki adımlar

* [Hataları ve özel durumları işleme](../logic-apps/logic-apps-exception-handling.md)
* [HTTP uç noktalarıyla mantık uygulamalarını arama, tetikleme veya yuvalama](../logic-apps/logic-apps-http-endpoint.md)
* [Eylemler ve tetikleyiciler için kullanım ölçümü](../logic-apps/logic-apps-pricing.md)
