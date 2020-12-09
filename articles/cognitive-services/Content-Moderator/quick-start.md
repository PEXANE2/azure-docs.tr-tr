---
title: 'Hızlı başlangıç: Web üzerinde Content Moderator deneyin'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, herhangi bir kod yazmak zorunda kalmadan Content Moderator temel işlevselliğini test etmek için çevrimiçi Content Moderator gözden geçirme aracını kullanacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 09/29/2020
ms.author: pafarley
ms.custom: cog-serv-seo-aug-2020
keywords: İçerik Aracı, içerik denetleme
ms.openlocfilehash: c026c42fe3c7a7f3f0d6b80e3123904077c104cf
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96905222"
---
# <a name="quickstart-try-content-moderator-on-the-web"></a>Hızlı başlangıç: Web üzerinde Content Moderator deneyin

Bu hızlı başlangıçta, herhangi bir kod yazmak zorunda kalmadan Content Moderator temel işlevselliğini test etmek için çevrimiçi Content Moderator gözden geçirme aracını kullanacaksınız. Bu hizmeti içerik denetleme uygulamanızla daha hızlı bir şekilde bütünleştirmek isterseniz, [sonraki adımlar](#next-steps) bölümündeki diğer hızlı başlangıçlara bakın.

## <a name="prerequisites"></a>Önkoşullar

- Bir Web tarayıcısı

## <a name="set-up-the-review-tool"></a>İnceleme aracını ayarlama
Content Moderator Inceleme Aracı, insan gözden geçirenlerin kararlar verirken bilişsel hizmete yardımcı olmasını sağlayan Web tabanlı bir araçtır. Bu kılavuzda, Content Moderator hizmetinin nasıl çalıştığını görebilmeniz için İnceleme aracını ayarlamaya yönelik kısa bir işlem öğreneceksiniz. [Content moderator gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) sitesine gidin ve kaydolun.

![Content Moderator giriş sayfası](images/homepage.PNG)

## <a name="create-a-review-team"></a>Bir gözden geçirme ekibi oluşturun

Sonra, bir gözden geçirme ekibi oluşturun. Bu ekip, çalışan bir senaryoda hizmetin denetleme kararlarını el ile gözden geçirenler grubu olacaktır. Bir ekip oluşturmak için bir **bölge** seçmeniz ve bir **Takım adı** ve **Takım Kimliği** sağlamanız gerekir. İş arkadaşlarınızı takıma davet etmek isterseniz, e-posta adreslerini buraya girerek yapabilirsiniz.

> [!NOTE]
> **Ekip adı** , gözden geçirme ekibiniz için kolay bir addır. Bu ad Azure portal görüntülenir. **EKIP kimliği** , gözden geçirme takımınızın program aracılığıyla tanımlamak için kullanılan şeydir.

> [!div class="mx-imgBorder"]
> ![Takım üyesini davet et](images/create-team.png)

Verileri müşteri tarafından yönetilen bir anahtarla (CMK) şifrelemeyi seçerseniz, E0 fiyatlandırma katmanında Content Moderator kaynağınız için **kaynak kimliği** istenir. Sağladığınız kaynağın bu takım için benzersiz olması gerekir. 

> [!div class="mx-imgBorder"]
> ![Takım üyesini CMK ile davet et](images/create-team-cmk.png)

## <a name="upload-sample-content"></a>Örnek içeriği karşıya yükle

Şimdi örnek içerik yüklemeye hazırsınız. **> görüntüsünü dene**' yi seçin, **metni > deneyin** veya **> video kullanmayı deneyin**.

> [!div class="mx-imgBorder"]
> ![Görüntüyü veya metin yönetimini deneyin](images/tryimagesortext.png)

İçeriğinizi denetleme için gönderme. Aşağıdaki örnek metin içeriğini kullanabilirsiniz:

```
Is this a grabage email abcdef@abcd.com, phone: 4255550111, IP: 255.255.255.255, 1234 Main Boulevard, Panapolis WA 96555.
Crap is the profanity here. Is this information PII? phone 4255550111
```

Dahili olarak, gözden geçirme aracı içeriğinizi taramak için denetleme API 'Lerini çağırır. Tarama tamamlandıktan sonra, incelemenizi bekleyen sonuçlar olduğunu bildiren bir ileti görürsünüz.

> [!div class="mx-imgBorder"]
> ![Orta dosyalar](images/submitted.png)

## <a name="review-moderation-tags"></a>Denetleme etiketlerini gözden geçirme

Uygulanan denetleme etiketlerini gözden geçirin. İçeriğinize hangi etiketlerin uygulandığını ve her bir kategoride skor olduğunu görebilirsiniz. Farklı içerik etiketlerinin neleri belirtdikleriyle ilgili daha fazla bilgi edinmek için [görüntü](image-moderation-api.md), [metin](text-moderation-api.md)ve [video](video-moderation-api.md) denetleme makalelerine bakın.

<!-- ![Review results](images/reviewresults_text.png) -->

Bir projede, sizin veya gözden geçirme ekibiniz bu etiketleri değiştirebilir veya gerektiğinde daha fazla etiket ekleyebilir. Bu değişiklikleri **İleri** düğmesiyle gönderebilirsiniz. İş uygulamanız yönetici API 'Lerini çağırdığında, etiketli içerik burada sıraya alınır ve insan gözden geçirme ekipleri tarafından incelenmeye hazırdır. Bu yaklaşımı kullanarak büyük hacimlerde içerik hızla gözden geçirebilirsiniz.

Bu noktada, Content Moderator hizmetinin yapabilecekleri örnekleri görmek için Content Moderator gözden geçirme aracını kullandınız. Daha sonra, gözden geçirme aracı hakkında daha fazla bilgi alabilir ve bunu Inceleme API 'Lerini kullanarak bir yazılım projesiyle tümleştirin ya da bir [sonraki adımlar](#next-steps) bölümüne atlayabilir ve bu da denetim API 'lerini uygulamanızda nasıl kullanacağınızı öğrenebilirsiniz.

## <a name="learn-more-about-the-review-tool"></a>İnceleme aracı hakkında daha fazla bilgi edinin

Content Moderator gözden geçirme aracının nasıl kullanılacağı hakkında daha fazla bilgi edinmek için, [Gözden geçirme aracı](Review-Tool-User-Guide/human-in-the-loop.md) kılavuzuna göz atın ve insan gözden geçirme deneyiminin nasıl ince ayarlamalar yapılacağını öğrenmek için Inceleme aracı API 'lerine bakın:
- [Iş API 'si](try-review-api-job.md) , denetleme API 'lerini kullanarak içeriğinizi tarar ve İnceleme aracında incelemeler oluşturur. 
- [Gözden geçirme API 'si](try-review-api-review.md) , içeriği önce taramadan insan moderatörleri için doğrudan görüntü, metin veya video İncelemeleri oluşturur. 
- [Iş akışı API 'si](try-review-api-workflow.md) , takımınızın oluşturduğu özel iş akışlarıyla ilgili ayrıntıları oluşturur, güncelleştirir ve alır.

Veya kodunuzda denetleme API 'Lerini kullanmaya başlamak için sonraki adımlarla devam edin.

## <a name="next-steps"></a>Sonraki adımlar

Kendi uygulamanızda denetleme API 'Lerinin nasıl kullanılacağını öğrenin.
- Görüntü denetlemeyi uygulayın. Resimleri taramak ve Etiketler, güvenirlik puanları ve diğer ayıklanan bilgileri kullanarak görüntüleri taramak ve olası yetişkin ve akıllı içerikleri algılamak için [API konsolunu](try-image-api.md) veya bir [istemci kitaplığını veya REST API hızlı](client-libraries.md) başlangıcı izleyin.
- Metin denetlemeyi uygulayın. Olası küfür, makine destekli istenmeyen metin sınıflandırması (Önizleme) ve kişisel veriler için metin içeriğini taramak üzere [API konsolunu](try-text-api.md) veya bir [istemci kitaplığını veya REST API hızlı](client-libraries.md) başlangıcı ' nı kullanın.
- Video denetlemeyi uygulayın. Videoları taramak ve olası yetişkinlere ve kcy içeriğini algılamak için [C# için video denetleme nasıl yapılır kılavuzunu](video-moderation-api.md) izleyin. 
