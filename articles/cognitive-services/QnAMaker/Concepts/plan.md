---
title: Uygulamanızı planlayın-Soru-Cevap Oluşturma
description: Soru-Cevap Oluşturma uygulamanızı planlamak, Soru-Cevap Oluşturma nasıl çalıştığını ve diğer Azure hizmetleriyle nasıl etkileşime gireceğini ve bazı Bilgi Bankası kavramlarını anlamak için gerekir.
ms.topic: conceptual
ms.date: 07/2/2020
ms.openlocfilehash: d19ec51aec7e71b6f040a03543f72af3aed09556
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85875720"
---
# <a name="plan-your-qna-maker-app"></a>Soru-Cevap Oluşturma Uygulamanızı planlayın

Soru-Cevap Oluşturma uygulamanızı planlamak, Soru-Cevap Oluşturma nasıl çalıştığını ve diğer Azure hizmetleriyle nasıl etkileşime gireceğini ve bazı Bilgi Bankası kavramlarını anlamak için gerekir.

## <a name="azure-resources"></a>Azure kaynakları

Soru-Cevap Oluşturma ile oluşturulan her [Azure kaynağının](azure-resources.md#resource-purposes) belirli bir amacı vardır. Her kaynak kendi amacını, sınırlarını ve [fiyatlandırma katmanını](azure-resources.md#pricing-tier-considerations)içerdiğinden, planlama işleminizin bir parçası olarak bu kaynakların ne yaptığını anlamak önemlidir.

|Kaynak|Amaç|
|--|--|
| [Soru-cevap oluşturma](azure-resources.md#qna-maker-resource) kaynağı|Yazma ve sorgu tahmini|
| [Bilişsel arama](azure-resources.md#cognitive-search-resource) kaynağı|Veri depolama ve arama|
| [Kaynak ve uygulama planı hizmet kaynağı App Service](azure-resources.md#app-service-and-app-service-plan)|Sorgu tahmin uç noktası|
| [Application Insights](azure-resources.md#application-insights) kaynağı|Sorgu tahmin telemetrisi|

### <a name="resource-planning"></a>Kaynak planlama

Yazma ve sorgu tahminini öğrenirken, `F0` her bir kaynağın her biri için ayrı katmanını kullanarak çalışır ve hem yazma hem de sorgu tahmin deneyimi sağlar. Bir üretime, canlı, senaryoya geçtiğinizde, kaynak seçiminizi yeniden değerlendirmeniz durumunda.

#### <a name="qna-maker-resource"></a>Soru-Cevap Oluşturma kaynağı

Tek bir Soru-Cevap Oluşturma kaynağı, birden fazla bilgi tabanını barındırabilir. Bilgi tabanı sayısı, Bilişsel Arama fiyatlandırma katmanının desteklenen dizin miktarına göre belirlenir. [Bilgi temellerine dizinlerin ilişkisi](azure-resources.md#index-usage)hakkında daha fazla bilgi edinin.

#### <a name="knowledge-base-size-and-throughput"></a>Bilgi Bankası boyutu ve aktarım hızı

Gerçek bir uygulama oluşturmayı planlarken, kaynaklarınızı bilgi Bankalarınızın boyutuna ve beklediğinizi sorgu tahmini isteklerine göre planlayın.

Bilgi Bankası boyutu tarafından denetlenir:
* [Bilişsel arama kaynak](../../../search/search-limits-quotas-capacity.md) fiyatlandırma katmanı limitleri
* [Soru-Cevap Oluşturma sınırları](../limits.md)

Bilgi Bankası sorgu tahmini isteği, Web uygulaması planı ve Web uygulaması tarafından denetlenir. Fiyatlandırma katmanınızı planlamak için [önerilen ayarlar](azure-resources.md#recommended-settings) ' a bakın.

### <a name="resource-sharing"></a>Kaynak paylaşımı

Bu kaynaklardan bazıları zaten kullanımda ise, kaynakları paylaşmayı düşünebilirsiniz. Bazı kaynaklar [paylaşılırken](azure-resources.md#share-services-with-qna-maker)bu gelişmiş bir senaryodur.

Aynı Soru-Cevap Oluşturma kaynakta oluşturulan tüm bilgi tabanları aynı **Test** sorgu tahmin uç noktasını paylaşır.

### <a name="understanding-impact-of-resource-selection"></a>Kaynak seçiminin etkisini anlama

Doğru kaynak seçimi, bilgi Bankalarınızın sorgu tahminleri başarıyla yanıtladığı anlamına gelir.

Bilgi tabanınız düzgün çalışmıyorsa, genellikle sorun hatalı kaynak yönetimi olur.

Hatalı kaynak seçimi, hangi [kaynağın değişmesi](azure-resources.md#when-to-change-a-pricing-tier)gerektiğini belirlemede araştırma gerektiriyor.

## <a name="knowledge-bases"></a>Bilgi Bankası tabanları

Bilgi Bankası, Soru-Cevap Oluşturma kaynağına doğrudan bağlanır ve sorgu tahmini isteklerini yanıtlamak için kullanılan soru ve yanıt (QnA) çiftlerini barındırır.

### <a name="language-considerations"></a>Dil konuları

Soru-Cevap Oluşturma kaynağınız üzerinde oluşturulan ilk bilgi tabanı, kaynağın dilini ayarlar. Bir Soru-Cevap Oluşturma kaynağı için yalnızca bir dile sahip olabilirsiniz.

Sorguyu sorgu tahmin uç noktasına göndermeden önce, Soru-Cevap Oluşturma kaynaklarınızı dile göre veya [çeviriciyi](../../translator/translator-info-overview.md) kullanarak başka bir dildeki bir sorguyu bilgi bankasındaki dile göre değiştirin.

### <a name="ingesting-data-sources"></a>Veri kaynaklarını geri ödeme

Bilgi Bankası oluşturmak için kullanılan [veri kaynakları](knowledge-base.md), aşağıdakilerden biri olabilir:

* Genel URL
* Özel SharePoint URL 'SI
* Dosya

Alma işlemi, [desteklenen içerik türlerini](content-types.md) markaşağı dönüştürür. *Yanıtın* daha fazla düzenlemesi markın ile yapılır. Bilgi tabanınızı oluşturduktan sonra, Soru-Cevap Oluşturma portalında [QNA çiftlerini](question-answer-set.md) [zengin metin yazma](../how-to/edit-knowledge-base.md#rich-text-editing-for-answer)ile düzenleyebilirsiniz.

### <a name="data-format-considerations"></a>Veri biçimi konuları

Bir QnA çiftinin son biçimi markın olduğundan [markı desteğini](../reference-markdown-format.md) anlamak önemlidir.

Soru-Cevap Oluşturma, görüntüler dahil olmak üzere içerik için kimlik doğrulaması sağlamadığı için, bağlı görüntülerin, Soru-Cevap Oluşturma portalının test bölmesinde ve tüm istemci uygulamaları içinde görüntülenmesi için genel bir URL 'den erişilebilir olması gerekir.

### <a name="bot-personality"></a>Bot kişiliği

[Kiıt-chat](../how-to/chit-chat-knowledge-base.md)ile bilgi tabanınız için bir bot kişilik ekleyin. Bu kişilik, *profesyonel* ve *kolay*gibi belirli bir konuşma tonlarında verilen yanıtlarla birlikte gelir. Bu CHIT-chat, ekleme, düzenleme ve kaldırma için toplam denetime sahip olduğunuz bir konuşma kümesi olarak sunulmaktadır.

Bot 'niz bilgi tabanınızı bağladığında bir bot kişiliği önerilir. Bilgi tabanınız olan çeşitli hizmetlere bağlanıyorsanız, bilgi tabanınızda CHIT-chat kullanmaya devam etmeyi seçebilirsiniz, ancak bunun sizin için doğru mimari tasarımı olup olmadığını öğrenmek için bot hizmetinin nasıl etkileşime gireceğini gözden geçirmeniz gerekir.

### <a name="conversation-flow-with-a-knowledge-base"></a>Bilgi Bankası ile görüşme akışı

Konuşma akışı genellikle, veya gibi bir kullanıcıdan selamlama ile başlar `Hi` `Hello` . Bilgi tabanınız gibi genel bir Yanıt ile yanıt verebilir `Hi, how can I help you` ve konuşmayı sürdürmek için bir izleme istemleri seçimi de sağlayabilir.

Bir kullanıcının botunuzu nasıl kullanacağınızı bilmesi ve konuşmada bot tarafından terk olmaması için konuşma akışınızı göz önünde bir döngüyle tasarlamanız gerekir. [Takip eden istemler](../how-to/multiturn-conversation.md) , konuşma akışına Izin veren QNA çiftleri arasında bağlama sağlar.

### <a name="authoring-with-collaborators"></a>Ortak çalışanlarla yazma

Ortak çalışanlar, Bilgi Bankası uygulamasının tam geliştirme yığınını paylaşan diğer geliştiriciler olabilir veya yalnızca Bilgi Bankası yazma ile sınırlı olabilir.

Bilgi Bankası yazma, ortak çalışan becerileri kapsamını sınırlandırmak için Azure portal uyguladığınız çeşitli [rol tabanlı erişim izinlerini](../how-to/collaborate-knowledge-base.md) destekler.

## <a name="integration-with-client-applications"></a>İstemci uygulamalarıyla tümleştirme

[İstemci uygulamalarıyla](integration-with-other-applications.md) tümleştirme, tahmin çalışma zamanı uç noktasına bir sorgu gönderme anlamına gelir. Bir sorgu, Soru-Cevap Oluşturma Web uygulaması uç noktasına SDK veya REST tabanlı bir istek ile özel bilgi tabanıza gönderilir.

İstemci isteğini doğru bir şekilde doğrulamak için istemci uygulamasının doğru kimlik bilgilerini ve Bilgi Bankası KIMLIĞINI gönderebilmesi gerekir. Azure bot hizmeti kullanıyorsanız, ayarı Azure portal bot yapılandırmasının bir parçası olarak yapılandırın.

### <a name="conversation-flow-in-a-client-application"></a>İstemci uygulamasında konuşma akışı

Azure bot gibi bir [istemci uygulamasındaki](integration-with-other-applications.md)konuşma akışı, Bilgi Bankası ile etkileşime girmeden önce ve sonra işlevselliği gerektirebilir.

İstemci uygulamanız konuşma akışını destekliyorsa, izleme istemlerini veya CHIT-Chit 'i işlemek için alternatif bir yol sağlamak için, bunları erken tasarlayın ve istemci uygulamasında kullanan sorgunun başka bir hizmet tarafından veya bilgi tabanınıza gönderilip doğru şekilde işlendiğinden emin olun.

### <a name="dispatching-between-qna-maker-and-language-understanding-luis"></a>Soru-Cevap Oluşturma ve Language Understanding arasında gönderme (LUSıS)

Bir istemci uygulaması, yalnızca biri bir Bilgi Bankası tarafından yanıtlanarak çeşitli özellikler sağlayabilir. Diğer özelliklerin yine de konuşma metnini anlaması ve bundan anlamı ayıklamalıdır.

Ortak bir istemci uygulama mimarisi hem Soru-Cevap Oluşturma hem de [Language Understanding (LUIS)](../../LUIS/what-is-luis.md) birlikte kullanılır. LUO, diğer hizmetlere dahil olmak üzere herhangi bir sorgu için metin sınıflandırması ve ayıklama işlemi sağlar, Soru-Cevap Oluşturma bilgi tabanınızdan yanıt sağlar.

Paylaşılan bir [mimaride](../choose-natural-language-processing-service.md), iki hizmet arasında gönderim, bot çerçevesinin [dağıtım](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Dispatch) aracı ile yapılır.

### <a name="active-learning-from-a-client-application"></a>İstemci uygulamasından etkin öğrenme

Soru-Cevap Oluşturma, yanıt için alternatif sorular önererek bilgi bankasını geliştirmek üzere _etkin öğrenimi_ kullanır. Bu [etkin öğrenimi](active-learning-suggestions.md)'nin bir parçası olan istemci uygulaması sorumludur. İstemci uygulaması, konuşma istemleriyle, bilgi bankasından döndürülen yanıtın kullanıcının aradığının yanıtını belirleyemediğini belirleyebilir ve daha iyi yanıtı tespit edebilir. Tahmin kalitesini artırmak için istemci uygulamanın [Bu bilgileri bilgi bankasına geri gönderebilmesi](active-learning-suggestions.md#how-you-give-explicit-feedback-with-the-train-api) gerekir.

### <a name="providing-a-default-answer"></a>Varsayılan bir yanıt sağlama

Bilgi tabanınız bir yanıt bulamazsa, _varsayılan yanıtı_döndürür. Bu yanıt, Soru-Cevap Oluşturma portalından, **Ayarlar** sayfasında veya [API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update#request-body)'lerde yapılandırılabilir.

Bu varsayılan yanıt, Azure bot varsayılan cevabı 'ndan farklıdır. Azure bot varsayılan yanıtı, Azure portal yapılandırma ayarlarının bir parçası olarak ve puan eşiğinin karşılanmadığı durumlarda döndürülür.

## <a name="prediction"></a>Prediction (Tahmin)

Tahmin, bilgi tabanınızdan alınan yanıttır ve yalnızca yanıta göre daha fazla bilgi içerir. Sorgu tahmini yanıtı almak için [Geneateanswer API](query-knowledge-base.md)'sini kullanın.

### <a name="prediction-score-fluctuations"></a>Tahmin puanı dalgalanmaları

Puan, çeşitli faktörlere göre değişebilir:

* Özelliği ile [Generateanswer](query-knowledge-base.md) 'a yanıt olarak istediğiniz yanıt sayısı `top`
* Çeşitli kullanılabilir alternatif sorular
* Meta veriler için filtreleme
* Sorgu `test` veya `production` Bilgi Bankası 'na gönderilen sorgu

[İki aşamalı bir yanıt derecelendirmesi](query-knowledge-base.md#how-qna-maker-processes-a-user-query-to-select-the-best-answer)vardır:
* Bilişsel arama-ilk derece-bir yanıtın bilişsel arama dönmesi için, _izin verilen yanıtların_ sayısı, bilişsel arama tarafından soru-cevap oluşturma derecelendiricisini içine geçebilmeleri için en iyi yanıtların kadar yüksek olması gerekir
* Soru-Cevap Oluşturma saniyelik sıralama-en iyi yanıtı öğrenmek için, özelliği ve makine öğrenimini uygulayın.

### <a name="service-updates"></a>Hizmet güncelleştirmeleri

Hizmet güncelleştirmeleri, [en son çalışma zamanı güncelleştirmeleri](../how-to/set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates)uygulanarak otomatik olarak yönetilir.

### <a name="scaling-throughput-and-resiliency"></a>Ölçeklendirme, verimlilik ve dayanıklılık

Ölçeklendirme, verimlilik ve dayanıklılık, [Azure kaynakları](../how-to/set-up-qnamaker-service-azure.md), fiyatlandırma katmanları ve [Traffic Manager](../how-to/set-up-qnamaker-service-azure.md#business-continuity-with-traffic-manager)gibi herhangi bir çevreleyen mimari tarafından belirlenir.

### <a name="analytics-with-application-insights"></a>Application Insights ile analiz

Bilgi bankalarınıza yönelik tüm sorgular Application Insights depolanır. Ölçümlerinizi anlamak için [en iyi sorguları](../how-to/get-analytics-knowledge-base.md) kullanın.

## <a name="development-lifecycle"></a>Geliştirme yaşam döngüsü

Bilgi bankasındaki [geliştirme yaşam döngüsü](development-lifecycle-knowledge-base.md) devam etmektedir: bilgi tabanınızı Düzenle, test etme ve yayımlama.

### <a name="knowledge-base-development-of-qna-maker-pairs"></a>Soru-Cevap Oluşturma çiftleri hakkında bilgi Bankası geliştirme

[QNA Pairs](question-answer-set.md) , istemci uygulaması kullanımınıza göre tasarlanıp geliştirilebilmelidir.

Her bir çift şunları içerebilir:
* Sorgulama sırasında filtrelenebilir. Bu, QnA çiftlerinizi verilerinize kaynak, içerik, biçim ve amaç hakkında ek bilgilerle etiketetmenize olanak tanır.
* İzleme istemleri-kullanıcının doğru yanıta ulaşması için bilgi tabanınız aracılığıyla bir yol belirleme.
* Diğer sorular-farklı sorular, aramanın çok çeşitli formlardaki yanıtınızı eşleştirmeye izin vermek için önemlidir. [Etkin öğrenme önerileri](active-learning-suggestions.md) , alternatif sorulara sahiptir.

### <a name="devops-development"></a>DevOps geliştirme

DevOps ardışık düzenine eklemek için Bilgi Bankası geliştirme işlemi, bilgi bankasını [toplu test](../quickstarts/batch-testing.md)sırasında yalıtıyor olması gerekir.

Bilgi Bankası, Bilişsel Arama dizinini Soru-Cevap Oluşturma kaynaktaki diğer tüm bilgi bankalarıyla paylaşır. Bilgi Bankası, Bölüm tarafından yalıtılırken, Dizin paylaşımı, yayımlanan bilgi tabanı ile karşılaştırıldığında puanta farklılık oluşmasına neden olabilir.

Ve bilgi bankalarında _aynı puanı_ elde etmek için `test` `production` bir soru-cevap oluşturma kaynağını tek bir Bilgi Bankası ile yalıtın. Bu mimaride, kaynak yalnızca yalıtılmış toplu iş testinin uzunluğuna kadar canlı olmalıdır.

## <a name="next-step"></a>Sonraki adım

* [Azure kaynakları](../how-to/set-up-qnamaker-service-azure.md)
* [Soru ve yanıt çiftleri](question-answer-set.md)