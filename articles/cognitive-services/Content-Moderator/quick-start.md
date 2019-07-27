---
title: "Hızlı Başlangıç: Web 'de Content Moderator deneyin Content Moderator"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, herhangi bir kod yazmak zorunda kalmadan Content Moderator temel işlevselliğini test etmek için çevrimiçi Content Moderator gözden geçirme aracını kullanacaksınız.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: sajagtap
ms.openlocfilehash: e5b20bb45c08330c59fc72712b9e65b488c8c68b
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68564454"
---
# <a name="quickstart-try-content-moderator-on-the-web"></a>Hızlı Başlangıç: Web üzerinde Content Moderator deneyin

Bu hızlı başlangıçta, herhangi bir kod yazmak zorunda kalmadan Content Moderator temel işlevselliğini test etmek için çevrimiçi Content Moderator gözden geçirme aracını kullanacaksınız. Bu hizmeti uygulamanıza daha hızlı bir şekilde bütünleştirmek istiyorsanız [sonraki adımlar](#next-steps) bölümündeki diğer hızlı başlangıçlara bakın.

## <a name="prerequisites"></a>Önkoşullar

- Bir Web tarayıcısı

## <a name="set-up-the-review-tool"></a>İnceleme aracını ayarlama
Content Moderator Inceleme Aracı, insan gözden geçirenlerin kararlar verirken bilişsel hizmete yardımcı olmasını sağlayan Web tabanlı bir araçtır. Bu kılavuzda, Content Moderator hizmetinin nasıl çalıştığını görebilmeniz için, gözden geçirme aracını ayarlama işleminin kısa bir sürecine gidecaksınız. [Content moderator gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) sitesine gidin ve kaydolun.

![Content Moderator giriş sayfası](images/homepage.PNG)

## <a name="create-a-review-team"></a>Bir gözden geçirme ekibi oluşturun

Sonra, bir gözden geçirme ekibi oluşturun. Çalışan bir senaryoda bu, hizmetin denetleme kararlarını el ile gözden geçirebilen kişi grubu olacaktır. Şimdilik yalnızca bir takım adı oluşturmanız gerekir. İş arkadaşlarınızı takıma davet etmek isterseniz, e-posta adreslerini buraya girerek yapabilirsiniz.

![Takım üyesini davet et](images/QuickStart-2-small.png)

## <a name="upload-sample-content"></a>Örnek içeriği karşıya yükle

Şimdi örnek içerik yüklemeye hazırsınız. **> görüntüsünü dene**' yi seçin, **metni > deneyin**veya **> video kullanmayı deneyin**.

![Görüntüyü veya metin yönetimini deneyin](images/tryimagesortext.png)

İçeriğinizi denetleme için gönderme. Dahili olarak, gözden geçirme aracı içeriğinizi taramak için denetleme API 'Lerini çağırır. Tarama tamamlandıktan sonra, incelemenizi bekleyen sonuçlar olduğunu bildiren bir ileti görürsünüz.

![Orta dosyalar](images/submitted.png)

## <a name="review-moderation-tags"></a>Denetleme etiketlerini gözden geçirme

Uygulanan denetleme etiketlerini gözden geçirin. İçeriğinize hangi etiketlerin uygulandığını ve her bir kategoride skor olduğunu görebilirsiniz. Farklı içerik etiketlerinin neleri belirtdikleriniz hakkında daha fazla bilgi edinmek için [görüntü](image-moderation-api.md), [metin](text-moderation-api.md)ve [video](video-moderation-api.md) Denetleme konularına bakın.

![Sonuçları gözden geçirin](images/reviewresults_text.png)

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
- Görüntü denetlemeyi uygulayın. Resimleri taramak ve Etiketler, güvenirlik puanları ve diğer ayıklanan bilgileri kullanarak olası yetişkinlere yönelik içeriği algılamak için [API konsolunu](try-image-api.md) veya [ C# hızlı](image-moderation-quickstart-dotnet.md) başlangıcı kullanın.
- Metin denetlemeyi uygulayın. [API konsolunu](try-text-api.md) kullanın veya olası küfür, makine destekli istenmeyen metin sınıflandırması (Önizleme) ve kişisel veriler için metin içeriğini taramak üzere [ C# hızlı](text-moderation-quickstart-dotnet.md) başlangıcı kullanın.
- Video denetlemeyi uygulayın. Videoları taramak ve olası yetişkinlere ve kcy içeriğini algılamak için [video denetleme nasıl yapılır Kılavuzu ' nu C# ](video-moderation-api.md) izleyin. 
