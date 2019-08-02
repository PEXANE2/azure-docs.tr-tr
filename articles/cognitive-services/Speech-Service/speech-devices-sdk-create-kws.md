---
title: Özel bir uyandırma sözcük okuma hizmeti oluşturma
titleSuffix: Azure Cognitive Services
description: Cihazınız, her zaman bir Uyanma sözcük (veya tümceciği) dinliyor. Uyandırma word kullanıcı diyor, kullanıcının dikte durdurur kadar cihaz buluta tüm sonraki ses gönderir. Uyandırma Word'ün özelleştirme, Cihazınızı ayırt ve marka bilgilerinizi güçlendirmek için etkili bir yoludur.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 2bc1a6cbbf1e0d790326849a41b0788e332daa31
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68553101"
---
# <a name="create-a-custom-wake-word-by-using-the-speech-service"></a>Konuşma hizmeti kullanarak bir özel Uyandırma sözcük oluşturma

Cihazınız, her zaman bir Uyanma sözcük (veya tümceciği) dinliyor. Örneğin, "Hey Cortana" Cortana Yardımcısı için bir Uyanma sözcüktür. Uyandırma word kullanıcı diyor, kullanıcının dikte durdurur kadar cihaz buluta tüm sonraki ses gönderir. Uyandırma Word'ün özelleştirme, Cihazınızı ayırt ve marka bilgilerinizi güçlendirmek için etkili bir yoludur.

Bu makalede, özel Uyandırma word cihazınız için nasıl oluşturulduğunu öğrenin.

## <a name="choose-an-effective-wake-word"></a>Etkili Uyandırma word seçin

Uyandırma word seçtiğinizde aşağıdaki yönergeleri göz önünde bulundurun:

* Uyandırma Word'ün İngilizce bir sözcük veya ifade olmalıdır. Söyleyin iki saniyeden daha uzun sürer.

* 4 ila 7 hece sözcükleri en iyi çalışır. Örneğin, "Hey, bilgisayar" bir iyi Uyandırma sözcüktür. Yalnızca "Merhaba" zayıf bir paroladır.

* Uyandırma sözcükleri ortak İngilizce telaffuz kurallarını izlemelidir.

* Benzersiz bir veya daha yaygın İngilizce telaffuz kurallarını izleyen made-up sözcük hatalı pozitif sonuçları azaltmak. Örneğin, "computerama" iyi Uyandırma word olabilir.

* Ortak bir sözcük seçmeyin. Örneğin, "yemek" ve "kişiler sık sıradan konuşmada söyleyin sözcükler Git". Bunlar, cihazınız için false Tetikleyiciler olabilir.

* Söyleyiş olabilir bir Uyanma sözcük kullanmaktan kaçının. Kullanıcılar, cihazlarını yanıt almak için "doğru" telaffuz bilmesi gerekir. Örneğin, "509" "beş sıfır dokuz" bildirilebilir "beş oh dokuz," veya "beş yüz ve dokuz." "R.E.I." "r-e-i" veya "ışın" bildirilebilir "Canlı" "/līv/" veya "/liv/" bildirilebilir.

* Özel karakterler, simgeler veya basamak kullanmayın. Örneğin, "Git #" ve "20 + kediler" iyi Uyandırma sözcükleri olmaz. Ancak, "NET Git" veya "yirmi kediler artı" işe yarayabilir. Yine de, marka simgeleri kullanın ve uygun telaffuz güçlendirmek için pazarlama ve belgeleri kullanın.

> [!NOTE]
> Bir ticari marka olarak kaydettirilmiş sözcük Uyandırma word'olarak seçerseniz, bu ticari marka olduğunuz veya ticari marka sahibinin Word'ü kullanma iznine sahip emin olun. Microsoft, Uyandırma word'ün seçiminize göre ortaya yasal sorunları sorumlu değildir.

## <a name="create-your-wake-word"></a>Uyandırma Word'ün oluşturma

Aygıtınızla özel bir uyandırma sözcüğü kullanabilmeniz için, Microsoft özel uyandırma sözcük oluşturma hizmeti ile bir uyandırma sözcüğü oluşturmanız gerekir. Bir uyandırma sözcüğü sağlamaktan sonra, hizmet cihazınızda uyandırma sözcüğünü etkinleştirmek için geliştirme setinizi dağıttığınız bir dosya oluşturur.

1. [Özel konuşma tanıma hizmet portalına](https://aka.ms/sdsdk-speechportal) gidin ve **oturum açın** ya da bir konuşma aboneliğiniz yoksa [**abonelik oluştur**](https://go.microsoft.com/fwlink/?linkid=2086754) ' a tıklayın.

    ![Özel Konuşma Tanıma hizmet portalı](media/speech-devices-sdk/wake-word-4.png)

1. [Özel uyandırma sözcüğü](https://aka.ms/sdsdk-wakewordportal) sayfasında, tercih ettiğiniz en uyandırma sözcüğünü yazın ve **uyandırma sözcüğü ekle**' ye tıklayın. Etkin bir anahtar [](#choose-an-effective-wake-word) sözcük seçmenize yardımcı olmak için bazı kılavuzlardır. Şu anda yalnızca en-US dilini destekliyoruz.

    ![Uyandırma sözcük girin](media/speech-devices-sdk/wake-word-5.png)

1. Uyandırma kelimesinin üç farklı söylenişleri oluşturulacaktır. Dilediğiniz söylenişleri seçebilirsiniz. Ardından, uyandırma sözcüğünü oluşturmak için **Gönder** ' i seçin. Uyandırma sözcüğünü değiştirmek istiyorsanız, lütfen önce var olanı kaldırın, telaffuz çizgisi üzerine geldiğinizde Sil simgesi görüntülenir.

    ![Uyandırma kelimeyi gözden geçirin](media/speech-devices-sdk/wake-word-6.png)

1. Modelin oluşturulması bir dakika kadar sürebilir. Dosyayı indirmeniz istenecektir.

    ![Uyandırma kelimeyi indirin](media/speech-devices-sdk/wake-word-7.png)

1. .Zip dosyasını bilgisayarınıza kaydedin. Özel uyandırma kelimeyi geliştirme paketine dağıtmak için bu dosyaya ihtiyacınız olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Özel uyandırma Word 'ü [konuşma hızlı başlangıç](https://aka.ms/sdsdk-quickstart)ile test edin.
