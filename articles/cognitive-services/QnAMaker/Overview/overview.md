---
title: Soru-Cevap Oluşturma Hizmeti nedir?
description: Soru-Cevap Oluşturma, verileriniz üzerinde kolayca doğal bir konuşma katmanı oluşturan bulut tabanlı bir NLP hizmetidir. Özel bilgi tabanınızdan (KB), belirli bir doğal dil girişi için en uygun yanıtı bulmak üzere kullanılabilir.
ms.topic: overview
ms.date: 05/26/2020
ms.openlocfilehash: d2ff2d789f2ea1ae6018d95ef1d880da87b4ff74
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83994876"
---
# <a name="what-is-the-qna-maker-service"></a>Soru-Cevap Oluşturma Hizmeti nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../../includes/cognitive-services-tls-announcement.md)]

Soru-Cevap Oluşturma, verileriniz üzerinde kolayca doğal bir konuşma katmanı oluşturan bulut tabanlı doğal dil Işleme (NLP) hizmetidir. Özel bilgi tabanınızdan (KB), belirli bir doğal dil girişi için en uygun yanıtı bulmak üzere kullanılabilir.

Soru-Cevap Oluşturma istemci uygulaması, bir soruyu yanıtlamak için doğal dilde bir kullanıcıyla iletişim kuran herhangi bir konuşma uygulamasıdır. İstemci uygulamalarına örnek olarak sosyal medya uygulamaları, sohbet botları ve konuşma özellikli masaüstü uygulamaları verilebilir.

## <a name="when-to-use-qna-maker"></a>Ne zaman kullanılacağı Soru-Cevap Oluşturma

* **Statik bilgileriniz varsa** -bilgi yanıtlarınızın bilgi bankasında statik bilgileriniz varsa soru-cevap oluşturma kullanın. Bu Bilgi Bankası, [PDF 'ler ve URL 'ler](../concepts/content-types.md)gibi belgelerle oluşturduğunuz gereksinimlerinize göre özel olarak tasarlanmıştır.
* **Bir istek, soru veya komuta aynı yanıtı sağlamak istediğinizde** , farklı kullanıcılar aynı soruyu gönderdiğinde aynı yanıt döndürülür.
* **Meta bilgilere göre statik bilgileri filtrelemek istediğinizde,** istemci uygulamanızın kullanıcılarına ve bilgilerine uygun ek filtreleme seçenekleri sağlamak için [meta veri](../how-to/metadata-generateanswer-usage.md) etiketleri ekleyin. Ortak meta veri bilgileri, [CHIT-chat](../how-to/chit-chat-knowledge-base.md), içerik türü veya biçimi, içerik amacı ve içerik yeniliği içerir.
* **Statik bilgiler içeren bir bot iletişimini yönetmek istediğinizde,** bilgi tabanınız bir kullanıcının konuşma metin veya komutunu alır ve yanıt verir. Yanıt, bilgi bankasında [Çoklu oturum açma](../how-to/multiturn-conversation.md)ile temsil edilen önceden belirlenmiş bir konuşma akışının parçasıysa, bu akışı kolayca sağlayabilir.

## <a name="use-qna-maker-knowledge-base-in-a-chat-bot"></a>Sohbet bot 'ta Soru-Cevap Oluşturma Bilgi Bankası 'nı kullanma

Bir Soru-Cevap Oluşturma Bilgi Bankası yayımlandığında, istemci uygulaması Bilgi Bankası uç noktanıza soru gönderir ve sonuçları JSON yanıtı olarak alır. Soru-Cevap Oluşturma için ortak bir istemci uygulaması, sohbet bot ' dır.

![Bilgi Bankası içeriğisinden bir yardım alın ve yanıt alın](../media/qnamaker-overview-learnabout/bot-chat-with-qnamaker.png)

|Adım|Eylem|
|:--|:--|
|1|İstemci uygulaması, kullanıcının _sorusunu_ (metni kendi kelimelerimi) gönderir, "nasıl yaparım? program aracılığıyla bilgi bankamı Güncelleştir" Bilgi Bankası uç noktanıza.|
|2|Soru-Cevap Oluşturma, doğru yanıtı ve en iyi yanıtı aramayı iyileştirmek için kullanılabilecek herhangi bir izleme istemini sağlamak için eğitilen Bilgi Bankası 'nı kullanır. Soru-Cevap Oluşturma JSON biçimli bir yanıt döndürür.|
|3|İstemci uygulaması, konuşmayı sürdürmek üzere kararlar almak için JSON yanıtını kullanır. Bu kararlar en iyi yanıtı göstermek ve en iyi yanıtı aramayı iyileştirmek için daha fazla seçenek sunmak içerebilir. |
|||

## <a name="what-is-a-knowledge-base"></a>Bilgi Bankası nedir?

Soru-Cevap Oluşturma [içeriğinizi](../concepts/knowledge-base.md) bir soru-cevap çifti bilgi tabanına aktarır. İçeri aktarma işlemi, soru ve yanıt çiftleri arasındaki ilişkileri öğrenmek için yapılandırılmış ve yarı yapılandırılmış içeriğinizin kısımları arasındaki ilişki hakkındaki bilgileri ayıklar. Bu soruyu ve yanıt çiftlerini düzenleyebilir veya yeni çiftler ekleyebilirsiniz.

Soru ve yanıt çiftinin içeriği şunları içerir:
* Sorunun tüm alternatif formları
* Arama sırasında yanıt seçimlerini filtrelemek için kullanılan meta veri etiketleri
* Arama iyileştirmesi ile devam etmek için izleme istemleri

![Meta veriler ile örnek soru ve yanıt](../media/qnamaker-overview-learnabout/example-question-and-answer-with-metadata.png)

Bilgi tabanınızı yayımladıktan sonra bir istemci uygulaması, bir kullanıcının sorusunu uç noktanıza gönderir. Soru-Cevap Oluşturma hizmetiniz soruyu işler ve en iyi Yanıtla yanıt verir.

## <a name="create-manage-and-publish-to-a-bot-without-code"></a>Kod olmadan bir bot oluşturun, yönetin ve yayımlayın

Soru-Cevap Oluşturma Portal, tüm Bilgi Bankası yazma deneyimini sağlar. Belge bilgilerini geçerli biçiminde bilgi tabanınızı içeri aktarabilirsiniz. Bu belgeler (SSS, ürün el kitabı, elektronik tablo veya Web sayfası gibi), soru ve yanıt çiftlerine dönüştürülür. Her bir çift izleme istemleri için çözümlenir ve diğer çiftlerine bağlanır. Son _markaşağı_ biçimi, görüntüler ve bağlantılar dahil zengin sunumu destekler.

Bilgi tabanınız düzenlendikten sonra, Bilgi Bankası 'nı herhangi bir kod yazmadan çalışan bir [Azure Web uygulaması bot](https://azure.microsoft.com/services/bot-service/) 'a yayımlayın. [Azure Portal](https://portal.azure.com) , botunuzu test edin veya geliştirmeye devam edin.

## <a name="search-quality-and-ranking-provides-the-best-possible-answer"></a>Arama kalitesi ve derecelendirmesi olası en iyi yanıtı sağlar

Soru-Cevap Oluşturma sistemi katmanlı bir derecelendirme yaklaşımıdır. Veriler, Azure Search 'te depolanır ve bu da ilk derecelendirme katmanı olarak da kullanılır. Azure Search 'un en iyi sonuçları, son sonuçları ve güvenirlik Puanını üretmek için Soru-Cevap Oluşturma NLP yeniden derecelendirme modelinden geçirilir.

## <a name="qna-maker-improves-the-conversation-process"></a>Soru-Cevap Oluşturma konuşma sürecini geliştirir

Soru-Cevap Oluşturma, basit soru ve yanıt eşlerinizi iyileştirebilmeniz için çok yönlü istemler ve etkin öğrenme sağlar.

**Çoklu açma istemleri** , soru ve yanıt çiftlerini bağlama fırsatı sağlar. Bu bağlantı, istemci uygulamasının en iyi yanıtı sağlamasına ve nihai yanıt aramasını iyileştirmek için daha fazla soru sunmasına olanak tanır.

Bilgi Bankası, yayımlanan bitiş noktasındaki kullanıcılardan gelen soruları aldıktan sonra Soru-Cevap Oluşturma, bu gerçek dünyadaki sorulara, kaliteyi artırmak için bilgi bankasında değişiklik önermek üzere **etkin öğrenme** uygular.

## <a name="development-lifecycle"></a>Geliştirme yaşam döngüsü

Soru-Cevap Oluşturma, tam geliştirme yaşam döngüsü ile tümleştirme için birlikte çalışma izinleriyle birlikte yazma, eğitim ve yayımlama olanağı sağlar.

> [!div class="mx-imgBorder"]
> ![Geliştirme döngüsünün kavramsal görüntüsü](../media/qnamaker-overview-learnabout/development-cycle.png)


## <a name="how-do-i-start"></a>Nereden başlayabilirim?

**1. adım**: [Azure Portal](https://portal.azure.com)soru-cevap oluşturma kaynak oluşturma.

**2. adım**: [soru-cevap oluşturma](https://www.qnamaker.ai) portalında Bilgi Bankası oluşturma. Bilgi Bankası oluşturmak için [dosya ve URL 'ler](../concepts/content-types.md) ekleyin.

**Adım 3**: [kıvrımlı veya Postman](../Quickstarts/get-answer-from-knowledge-base-using-url-tool.md)kullanarak özel uç noktanıza bilgi bankasını ve testinizi yayımlayın.

**4. adım**: istemci uygulamanızdan, program aracılığıyla bilgi tabanınızın uç noktasını çağırın. İstemci uygulaması, kullanıcıya en iyi yanıtı göstermek için JSON yanıtını işler.

## <a name="next-steps"></a>Sonraki adımlar
Soru-Cevap Oluşturma, özel bilgi tabanınızı oluşturmak, yönetmek ve dağıtmak için ihtiyacınız olan her şeyi sağlar.

> [!div class="nextstepaction"]
> [En son değişiklikleri gözden geçirin](../whats-new.md)
