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
ms.openlocfilehash: 9a5b5de71ee290b39603968cf4309171689e22e4
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74184840"
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

1. [Özel anahtar sözcük](https://aka.ms/sdsdk-wakewordportal) sayfasında, istediğiniz anahtar sözcüğü yazın ve **anahtar sözcük ekle**' ye tıklayın. Etkin bir anahtar sözcük seçmenize yardımcı olmak için bazı [kılavuzlardır](#choose-an-effective-keyword) . Destek şu anda en-US diliyle sınırlıdır.

    ![Anahtar keliinizi girin](media/speech-devices-sdk/custom-kws-portal-enter-keyword.png)

1. Portal şimdi anahtar kelimeden aday söylenişleri oluşturacak. Oynat düğmelerine tıklayarak her bir adayı dinleyin ve yanlış olan herhangi bir söylenin yanındaki denetimleri kaldırın. Yalnızca güzel Söylenişler denetlendikten sonra, anahtar sözcüğünü oluşturmaya başlamak için **Gönder** ' i seçin. Anahtar sözcüğünü değiştirmek istiyorsanız, ilk olarak var olanı, üzerine gelindiğinde satırın sağ tarafında görüntülenen Sil düğmesine tıklayarak kaldırın.

    ![Anahtar keliinizi gözden geçirin](media/speech-devices-sdk/custom-kws-portal-review-keyword.png)

1. Modelin oluşturulması bir dakika kadar sürebilir. Bundan sonra dosyayı indirmeniz istenir.

    ![Anahtar keliinizi indirin](media/speech-devices-sdk/custom-kws-portal-download-keyword.png)

1. .Zip dosyasını bilgisayarınıza kaydedin. Özel anahtar keliinizi cihazınıza dağıtmak için bu dosyaya ihtiyacınız olacak.

## <a name="next-steps"></a>Sonraki adımlar

[Konuşma CIHAZLARı SDK hızlı başlangıç](https://aka.ms/sdsdk-quickstart)ile özel anahtar keliinizi test edin.
