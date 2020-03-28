---
title: "Quickstart: Web'de İçerik Moderatör deneyin - İçerik Moderatör"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, herhangi bir kod yazmak zorunda kalmadan İçerik Moderatörünün temel işlevselliğini test etmek için çevrimiçi İçerik Moderatör İnceleme aracını kullanacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 03/13/2020
ms.author: pafarley
ms.openlocfilehash: 666b70ba8b632cb2cadf20de384e3e615acb2b3d
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79203599"
---
# <a name="quickstart-try-content-moderator-on-the-web"></a>Quickstart: Web'de İçerik Moderatör deneyin

Bu hızlı başlangıçta, herhangi bir kod yazmak zorunda kalmadan İçerik Moderatörünün temel işlevselliğini test etmek için çevrimiçi İçerik Moderatör İnceleme aracını kullanırsınız. Bu hizmeti uygulamanıza daha hızlı entegre etmek istiyorsanız, [Sonraki adımlar](#next-steps) bölümündeki diğer hızlı başlangıçlara bakın.

## <a name="prerequisites"></a>Ön koşullar

- Bir web tarayıcısı

## <a name="set-up-the-review-tool"></a>İnceleme aracını ayarlama
İçerik Moderatör İnceleme aracı, insan gözden geçirenlerin karar verirken bilişsel hizmete yardımcı olmasını sağlayan web tabanlı bir araçtır. Bu kılavuzda, İçerik Moderatör hizmetinin nasıl çalıştığını görebilmeniz için inceleme aracını ayarlama nın kısa sürecinden geçeceksiniz. [İçerik Moderatör İnceleme araç](https://contentmoderator.cognitive.microsoft.com/) sitesine gidin ve kaydolun.

![İçerik Moderatör Ana Sayfası](images/homepage.PNG)

## <a name="create-a-review-team"></a>İnceleme ekibi oluşturma

Ardından, bir inceleme ekibi oluşturun. Çalışma senaryosunda, bu, hizmetin ılımlılık kararlarını el ile gözden geçiren kişi grubu olacaktır. Bir takım oluşturmak için bir **Bölge**seçmeniz ve **bir Takım Adı** ve **Takım Kimliği**sağlamanız gerekir. İş arkadaşlarınızı ekibe davet etmek isterseniz, bunu e-posta adreslerini buraya girerek yapabilirsiniz.

> [!NOTE]
> **Takım Adı,** inceleme ekibiniz için dostça bir isimdir. Bu, Azure portalında görüntülenen addır. **Ekip Kimliği,** inceleme ekibinizi programlı olarak tanımlamak için kullanılan şeydir.

> [!div class="mx-imgBorder"]
> ![Ekip üyesini davet et](images/create-team.png)

Verileri müşteri tarafından yönetilen bir anahtar (CMK) kullanarak şifrelemeyi seçerseniz, E0 fiyatlandırma katmanındaki İçerik Moderatör kaynağınız için **Kaynak Kimliği** istenir. Sağladığınız kaynak yeni olmalıdır. 

> [!div class="mx-imgBorder"]
> ![CMK ile ekip üyesini davet edin](images/create-team-cmk.png)

İçerik Moderatörü kaynağını yeniden kullanmaya çalışırsanız, şu uyarıyı görürsünüz: 

> [!div class="mx-imgBorder"]
> ![CMK hatası](images/create-team-cmk-fail.png)

## <a name="upload-sample-content"></a>Örnek içerik yükleme

Artık örnek içerik yüklemeye hazırsınız. **> Görüntü'>** seçin, **> Metni deneyin**veya > Video **deneyin.**

![Resim veya Metin Moderasyonunu Deneyin](images/tryimagesortext.png)

İçeriğinizi ılımlılık için gönderin. Dahili olarak, inceleme aracı içeriğinizi taramak için ılımlılık API'lerini çağırır. Tarama tamamlandıktan sonra, incelemenizi bekleyen sonuçlar olduğunu bildiren bir ileti görürsünüz.

![Dosyaları orta dereceli](images/submitted.png)

## <a name="review-moderation-tags"></a>Denetleme denetleme etiketleri

Uygulanan ılımlılık etiketlerini gözden geçirin. İçeriğinize hangi etiketlerin uygulandığını ve her kategoride hangi puanın ne olduğunu görebilirsiniz. Farklı içerik etiketlerinin ne gösterdiği hakkında daha fazla bilgi edinmek için [Resim,](image-moderation-api.md) [Metin](text-moderation-api.md)ve [Video](video-moderation-api.md) Denetleme konularını görün.

![Sonuçları gözden geçirme](images/reviewresults_text.png)

Bir projede, siz veya inceleme ekibiniz bu etiketleri değiştirebilir veya gerektiğinde daha fazla etiket ekleyebilirsiniz. Bu değişiklikleri **İleri** düğmesiyle gönderirsiniz. İş uygulamanız Moderatör API'lerini aradığında, etiketlenen içerik insan inceleme ekipleri tarafından incelenmeye hazır olarak burada sıraya girecektir. Bu yaklaşımı kullanarak büyük içerik hacimlerini hızla inceleyebilirsiniz.

Bu noktada, İçerik Moderatör hizmetinin neler yapabileceğine örnek lerini görmek için İçerik Moderatör İnceleme aracını kullandınız. Ardından, inceleme aracı hakkında daha fazla bilgi edinebilir ve Gözden Geçirme API'lerini kullanarak bir yazılım projesine nasıl entegre edilebilirsiniz ya da Uygulamanızda Moderasyon API'lerini nasıl kullanacağınızı öğrenmek için [Sonraki adımlar](#next-steps) bölümüne atlayabilirsiniz.

## <a name="learn-more-about-the-review-tool"></a>İnceleme aracı hakkında daha fazla bilgi edinin

İçerik Moderatör İnceleme aracını nasıl kullanacağınız hakkında daha fazla bilgi edinmek için Gözden [Geçirme araç](Review-Tool-User-Guide/human-in-the-loop.md) kılavuzuna bir göz atın ve insan inceleme deneyiminde nasıl ince ayar yapılacağını öğrenmek için İnceleme aracı API'lerine bakın:
- [İş](try-review-api-job.md) API'si, ılımlılık API'lerini kullanarak içeriğinizi tarar ve inceleme aracında incelemeler oluşturur. 
- [İnceleme API'si,](try-review-api-review.md) içeriği tarmadan insan moderatörler için doğrudan görüntü, metin veya video incelemeleri oluşturur. 
- [İş Akışı API'si,](try-review-api-workflow.md) ekibinizin oluşturduğu özel iş akışları hakkında ayrıntılar oluşturur, güncelleştirir ve alır.

Veya, kodunuzda Moderasyon API'lerini kullanmaya başlamak için sonraki adımlarla devam edin.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızda Moderasyon API'lerini nasıl kullanacağınızı öğrenin.
- Görüntü ılımlılığı uygulayın. [API konsolunu](try-image-api.md) kullanın veya etiketleri, güven puanlarını ve diğer çıkarılan bilgileri kullanarak görüntüleri tarayıp olası yetişkinlere uygun ve müstehcen içeriği algılamak için [.NET SDK hızlı başlatımını](dotnet-sdk-quickstart.md) izleyin.
- Metin moderasyonu uygulayın. [API](try-text-api.md) konsolu'nu kullanın veya metin içeriğini olası küfür, makine destekli istenmeyen metin sınıflandırması (önizleme) ve kişisel veriler için taramaya [.NET SDK hızlı başlat'ı](dotnet-sdk-quickstart.md) kullanın.
- Video ılımlılığı uygulayın. Videoları tarayıp olası yetişkinlere uygun ve müstehcen içeriği algılamak [için C# için Video Moderasyon nasıl yapılsın kılavuzunu](video-moderation-api.md) izleyin. 
