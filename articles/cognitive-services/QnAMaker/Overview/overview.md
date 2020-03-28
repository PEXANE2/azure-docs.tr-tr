---
title: QnA Maker hizmeti nedir?
titleSuffix: Azure Cognitive Services
description: QnA Maker, verilerinizin üzerinde kolayca doğal bir konuşma katmanı oluşturan bulut tabanlı bir NLP hizmetidir. Belirli bir doğal dil girişi için en uygun yanıtı bulmak için, özel bilgi tabanınızdan (KB) kullanılabilir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: overview
ms.date: 02/21/2020
ms.author: diberry
ms.openlocfilehash: 2863a086343b0a5d3bc69ae319dbe93d557bcb4a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80052920"
---
# <a name="what-is-the-qna-maker-service"></a>QnA Maker hizmeti nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../../includes/cognitive-services-tls-announcement.md)]

QnA Maker, verilerinizin üzerinde kolayca doğal bir konuşma katmanı oluşturan bulut tabanlı bir Doğal Dil İşleme (NLP) hizmetidir. Belirli bir doğal dil girişi için en uygun yanıtı bulmak için, özel bilgi tabanınızdan (KB) kullanılabilir.

QnA Maker için bir istemci uygulaması bir soruyu yanıtlamak için doğal dilde bir kullanıcı ile iletişim kuranherhangi bir konuşma uygulamasıdır. İstemci uygulamalarına örnek olarak sosyal medya uygulamaları, sohbet botları ve konuşma özellikli masaüstü uygulamaları verilebilir.

## <a name="when-to-use-qna-maker"></a>QnA Maker ne zaman kullanılır

* **Statik bilgilere sahip olduğunuzda** - Yanıtbilgi tabanınızda statik bilgiler varsa QnA Maker'ı kullanın. Bu bilgi tabanı, [PDF'ler ve URL'ler](../concepts/content-types.md)gibi belgelerle oluşturduğunuz ihtiyaçlarınıza özeldir.
* **Bir isteğe, soruya veya komuta aynı yanıtı vermek istediğinizde** - farklı kullanıcılar aynı soruyu gönderdiğinde, aynı yanıt döndürülür.
* **Meta bilgilere dayalı statik bilgilere filtre atmak istediğinizde** , istemci uygulamanızın kullanıcıları ve bilgileriyle ilgili ek filtreleme seçenekleri sağlamak için [meta veri](../how-to/metadata-generateanswer-usage.md) etiketleri ekleyin. Yaygın meta veri [bilgileri, chit-chat,](../how-to/chit-chat-knowledge-base.md)içerik türü veya biçimi, içerik amacı ve içerik tazeliğini içerir.
* **Statik bilgiler içeren bir bot konuşmasını yönetmek istediğinizde** , bilgi tabanınız kullanıcının konuşma metnini veya komutunu alır ve yanıtlar. Cevap önceden belirlenmiş bir konuşma akışının bir parçası ise, [çok dönüşlü bağlam](../how-to/multiturn-conversation.md)ile bilgi tabanında temsil, bot kolayca bu akışı sağlayabilir.

## <a name="use-qna-maker-knowledge-base-in-a-chat-bot"></a>Bir sohbet botQnA Maker bilgi tabanını kullanın

Bir QnA Maker bilgi tabanı yayımlandıktan sonra, istemci uygulaması bilgi tabanı bitiş noktasına bir soru gönderir ve sonuçları JSON yanıtı olarak alır. QnA Maker için ortak bir istemci uygulaması bir sohbet botudur.

![Bir bota soru sorun ve bilgi bankası içeriğinden yanıt alın](../media/qnamaker-overview-learnabout/bot-chat-with-qnamaker.png)

|Adım|Eylem|
|:--|:--|
|1|İstemci uygulaması kullanıcının _sorusunu_ (kendi sözleriyle metin) gönderir), "Bilgi Tabanımı nasıl programsal olarak güncellerim?" bilgi tabanı bitiş noktasına.|
|2|QnA Maker doğru yanıtı ve en iyi yanıtı bulmak için aramayı hassaslaştırmak için kullanılabilecek herhangi bir takip istemlerini sağlamak için eğitilmiş bilgi tabanını kullanır. QnA Maker JSON biçimlendirilmiş bir yanıt döndürür.|
|3|İstemci uygulaması, konuşmaya nasıl devam edeceği yle ilgili kararlar almak için JSON yanıtını kullanır. Bu kararlar, en iyi yanıtı göstermeyi ve aramayı en iyi yanıt için hassaslaştırmak için daha fazla seçenek sunmayı içerebilir. |
|||

## <a name="what-is-a-knowledge-base"></a>Bilgi bankası nedir?

QnA [Maker, içeriğinizi](../concepts/knowledge-base.md) soru ve yanıt kümelerinden oluşan bir bilgi tabanına içeri solur. Alma işlemi, soru ve yanıt kümeleri arasındaki ilişkileri ima etmek için yapılandırılmış ve yarı yapılandırılmış içeriğinizin bölümleri arasındaki ilişki hakkında bilgi ayıklar. Bu soru ve yanıt kümelerini veya yeni kümeler ekleyebilirsiniz.

Soru ve yanıt kümesinin içeriği şunları içerir:
* Sorunun tüm alternatif formları
* Arama sırasında yanıt seçeneklerini filtrelemek için kullanılan meta veri etiketleri
* Arama iyileştirmesine devam etmek için izleme istemleri

![Meta verilerle örnek soru ve cevap](../media/qnamaker-overview-learnabout/example-question-and-answer-with-metadata.png)

Bilgi tabanınızı yayımladıktan sonra, bir istemci uygulaması bir kullanıcının sorusunu bitiş noktanıza gönderir. QnA Maker hizmetiniz soruyu işler ve en iyi yanıtla yanıt verir.

## <a name="create-manage-and-publish-to-a-bot-without-code"></a>Kod olmadan bir bot oluşturma, yönetme ve yayımlama

QnA Maker portalı tam bilgi tabanı yazma deneyimi sağlar. Belgeleri, mevcut haliyle bilgi tabanınıza aktarabilirsiniz. Bu belgeler (SSS, ürün kılavuzu, elektronik tablo veya web sayfası gibi) soru ve yanıt kümelerine dönüştürülür. Her küme, takip istemleri için analiz edilir ve diğer kümelere bağlanır. Son _işaretleme_ biçimi, görüntüler ve bağlantılar da dahil olmak üzere zengin sunuyu destekler.

Bilgi tabanınız düzenlendikten sonra, bilgi tabanını herhangi bir kod yazmadan çalışan bir [Azure Web App botuna](https://azure.microsoft.com/services/bot-service/) yayınlayın. Robotunuzu Azure [portalında](https://portal.azure.com) test edin veya indirip geliştirmeye devam edin.

## <a name="search-quality-and-ranking-provides-the-best-possible-answer"></a>Arama kalitesi ve sıralama mümkün olan en iyi cevabı sağlar

QnA Maker sistemi katmanlı bir sıralama yaklaşımıdır. Veriler, ilk sıralama katmanı olarak da hizmet veren Azure aramasında depolanır. Azure aramasından elde edilen en iyi sonuçlar, nihai sonuçları ve güven puanını oluşturmak için QnA Maker'ın NLP yeniden sıralama modelinden geçirilir.

## <a name="qna-maker-improves-the-conversation-process"></a>QnA Maker konuşma sürecini geliştirir

QnA Maker, temel soru ve yanıt kümelerinizi geliştirmenize yardımcı olmak için çok yönlü istemler ve etkin öğrenme sağlar.

**Çok dönüşlü istemler,** soru ve yanıt çiftlerini bağlama fırsatı verir. Bu bağlantı istemci uygulamasının en iyi yanıtı sağlamasına olanak tanır ve son yanıt için aramayı hassaslaştırmak için daha fazla soru sağlar.

Bilgi tabanı, yayınlanan bitiş noktasında kullanıcılardan sorular aldıktan sonra, QnA Maker bu gerçek dünyadaki sorulara, kaliteyi artırmak için bilgi tabanınızda değişiklik önermek için **aktif öğrenme** uygular.

## <a name="development-lifecycle"></a>Geliştirme yaşam döngüsü

QnA Maker, tam gelişim yaşam döngüsüne entegre etmek için işbirliği izinleri ile birlikte yazma, eğitim ve yayıncılık sağlar.

> [!div class="mx-imgBorder"]
> ![Gelişim döngüsünün kavramsal görüntüsü](../media/qnamaker-overview-learnabout/development-cycle.png)


## <a name="how-do-i-start"></a>Nereden başlayabilirim?

**Adım 1**: [Azure portalında](https://portal.azure.com)bir QnA Maker kaynağı oluşturun.

**Adım 2**: [QnA Maker](https://www.qnamaker.ai) portalında bir bilgi tabanı oluşturun. Bilgi tabanını oluşturmak için [dosya ve URL](../concepts/content-types.md) ekleyin.

**Adım 3**: Bilgi tabanınızı yayınlayın ve [cURL veya Postacı'yı](../Quickstarts/get-answer-from-knowledge-base-using-url-tool.md)kullanarak özel bitiş noktanızdan test edin.

**Adım 4**: İstemci uygulamanızdan, programlı olarak bilgi tabanınızın bitiş noktasını arayın. İstemci uygulaması, kullanıcıya en iyi yanıtı göstermek için JSON yanıtını işler.

## <a name="next-steps"></a>Sonraki adımlar
QnA Maker, özel bilgi tabanınızı oluşturmak, yönetmek ve dağıtmak için ihtiyacınız olan her şeyi sağlar.

> [!div class="nextstepaction"]
> [En son değişiklikleri gözden geçirme](../whats-new.md)
