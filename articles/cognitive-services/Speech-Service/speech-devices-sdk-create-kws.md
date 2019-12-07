---
title: Özel anahtar sözcük okuma hizmeti oluşturma
titleSuffix: Azure Cognitive Services
description: Cihazınız her zaman bir anahtar sözcüğü (veya tümceciği) dinler. Kullanıcı anahtar sözcüğünü söyyorsa, cihaz sonraki tüm sesleri buluta gönderir ve Kullanıcı konuşmayı durduruncaya kadar. Anahtar keliinizi özelleştirmek, cihazınızı ayırt etmenin ve markanızı güçlendirmenin etkili bir yoludur.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: erhopf
ms.openlocfilehash: 42bcc336bfeb325a08c3d65438d66690c0b35100
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74896475"
---
# <a name="create-a-custom-keyword-by-using-the-speech-service"></a>Konuşma hizmetini kullanarak özel anahtar sözcük oluşturma

Cihazınız her zaman bir anahtar sözcüğü (veya tümceciği) dinler. Örneğin, "Hey Cortana", Cortana yardımcısı için bir anahtar sözcüktür. Kullanıcı anahtar sözcüğünü söyyorsa, cihaz sonraki tüm sesleri buluta gönderir ve Kullanıcı konuşmayı durduruncaya kadar. Anahtar keliinizi özelleştirmek, cihazınızı ayırt etmenin ve markanızı güçlendirmenin etkili bir yoludur.

Bu makalede, cihazınız için özel bir anahtar sözcük oluşturmayı öğreneceksiniz.

## <a name="choose-an-effective-keyword"></a>Etkin bir anahtar sözcük seçin

Bir anahtar sözcük seçerken aşağıdaki yönergeleri göz önünde bulundurun:

* Anahtar kelimelisiniz Ingilizce bir sözcük veya tümcecik olmalıdır. Söyleyin iki saniyeden daha uzun sürer.

* 4 ila 7 hece sözcükleri en iyi çalışır. Örneğin, "Hey, Computer" iyi bir anahtar sözcüktür. Yalnızca "Merhaba" zayıf bir paroladır.

* Anahtar sözcükler, yaygın Ingilizce Söyleniş kurallarını izlemelidir.

* Benzersiz bir veya daha yaygın İngilizce telaffuz kurallarını izleyen made-up sözcük hatalı pozitif sonuçları azaltmak. Örneğin, "computerama" iyi bir anahtar sözcük olabilir.

* Ortak bir sözcük seçmeyin. Örneğin, "yemek" ve "kişiler sık sıradan konuşmada söyleyin sözcükler Git". Bunlar, cihazınız için false Tetikleyiciler olabilir.

* Alternatif söylenişleri olabilecek bir anahtar sözcük kullanmaktan kaçının. Kullanıcılar, cihazlarını yanıt almak için "doğru" telaffuz bilmesi gerekir. Örneğin, "509" "beş sıfır dokuz" bildirilebilir "beş oh dokuz," veya "beş yüz ve dokuz." "R.E.I." "r-e-i" veya "ışın" bildirilebilir "Canlı" "/līv/" veya "/liv/" bildirilebilir.

* Özel karakterler, simgeler veya basamak kullanmayın. Örneğin, "Go #" ve "20 + kediler" iyi anahtar kelimelerdir. Ancak, "NET Git" veya "yirmi kediler artı" işe yarayabilir. Yine de, marka simgeleri kullanın ve uygun telaffuz güçlendirmek için pazarlama ve belgeleri kullanın.

> [!NOTE]
> Anahtar kelime olarak bir trademarked sözcüğü seçerseniz, bu ticari marka sahibi olduğunuzdan veya ticari marka sahibinden, kelimeyi kullanmak için izninizin olduğundan emin olun. Microsoft, sizin tercih ettiğiniz anahtar kelimeden çıkabilecek herhangi bir yasal sorun için tabi değildir.

## <a name="create-your-keyword"></a>Anahtar keliinizi oluşturma

Özel bir anahtar sözcüğü kullanabilmeniz için, [konuşma Studio](https://aka.ms/sdsdk-speechportal)'Daki [özel anahtar](https://aka.ms/sdsdk-wakewordportal) sözcük sayfasını kullanarak bir anahtar sözcük oluşturmanız gerekir. Bir anahtar sözcük girdikten sonra, cihazınıza dağıttığınız bir dosya oluşturur.

1. [Konuşma Studio](https://aka.ms/sdsdk-speechportal) 'ya gidin ve **oturum açın** veya henüz bir konuşma aboneliğiniz yoksa, [**abonelik oluştur**](https://go.microsoft.com/fwlink/?linkid=2086754)' u seçin.

1. [Özel anahtar sözcük](https://aka.ms/sdsdk-wakewordportal) sayfasında **Yeni bir proje**oluşturun. 

1. Bir **ad**, isteğe bağlı bir **Açıklama**girin ve dili seçin. Her dil için bir proje gerekir ve destek şu anda en-US diliyle sınırlı olur.

    ![Anahtar sözcük projenizi açıkla](media/custom-keyword/custom-kws-portal-new-project.png)

1. Listeden projenizi seçin. 

    ![Anahtar sözcük projenizi seçin](media/custom-keyword/custom-kws-portal-project-list.png)

1. Yeni bir anahtar sözcük modeli başlatmak için **modeli eğitme**' ye tıklayın.

1. Anahtar sözcük modeli için bir **ad** ve Isteğe bağlı **Açıklama** girin ve istediğiniz **anahtar sözcüğü** yazın ve **İleri**' ye tıklayın. Etkin bir anahtar sözcük seçmenize yardımcı olmak için bazı [kılavuzlardır](#choose-an-effective-keyword) .

    ![Anahtar keliinizi girin](media/custom-keyword/custom-kws-portal-new-model.png) 

1. Portal şimdi anahtar kelimeden aday söylenişleri oluşturacak. Oynat düğmelerine tıklayarak her bir adayı dinleyin ve yanlış olan herhangi bir söylenin yanındaki denetimleri kaldırın. Yalnızca güzel Söylenişler denetlendikten sonra, anahtar sözcüğünü oluşturmaya başlamak için **eğitme** ' ye tıklayın. 

    ![Anahtar keliinizi gözden geçirin](media/custom-keyword/custom-kws-portal-choose-prons.png) 

1. Modelin oluşturulması on dakika kadar sürebilir. Model tamamlandığında anahtar sözcük listesi **Işlemden** **başarılı** olarak değişecektir. Sonra dosyayı indirebilirsiniz.

    ![Anahtar keliinizi gözden geçirin](media/custom-keyword/custom-kws-portal-download-model.png) 

1. .Zip dosyasını bilgisayarınıza kaydedin. Özel anahtar keliinizi cihazınıza dağıtmak için bu dosyaya ihtiyacınız olacak.

## <a name="next-steps"></a>Sonraki adımlar

[Konuşma CIHAZLARı SDK hızlı başlangıç](https://aka.ms/sdsdk-quickstart)ile özel anahtar keliinizi test edin.
