---
title: 'Nasıl yapılır: parametreyi dış varlık varlığı olarak yapılandırma'
titleSuffix: Azure Cognitive Services
description: Bu makalede, bir Web uç noktası üzerinden sunulan kataloğa başvurmak için bir dize parametresinin nasıl yapılandırılacağı açıklanmaktadır.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: sausin
ms.openlocfilehash: 45dba3b7f46ec558c46b8505da26fd3ef4de4cbc
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87284192"
---
# <a name="add-configurations-to-commands-parameters"></a>Komut parametrelerine yapılandırma ekleme

Bu makalede, aşağıdakiler dahil olmak üzere gelişmiş parametre yapılandırması hakkında daha fazla bilgi edinebilirsiniz:

 - Parametre değerleri dışarıdan özel komutlar uygulamasına tanımlanmış bir kümesine ait olabilir
 - Parametrelerin değerine doğrulama yan tümceleri ekleme

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki makalelerde bulunan adımları tamamlamış olmanız gerekir:

> [!div class="checklist"]
> * [Nasıl yapılır: basit komutlarla uygulama oluşturma](./how-to-custom-commands-create-application-with-simple-commands.md)
> * [Nasıl yapılır: komutlara parametreler ekleme](./how-to-custom-commands-add-parameters-to-commands.md)


## <a name="configure-parameter-as-external-catalog-entity"></a>Parametreyi dış Katalog varlığı olarak yapılandırma

Bu bölümde, bir Web uç noktası üzerinden barındırılan dış kataloglara başvuracak dize türü parametreleri yapılandırırsınız. Bu, özel komutlar uygulamasında düzenleme yapılmadan dış kataloğu bağımsız olarak güncelleştirmenizi sağlar. Bu yaklaşım, Katalog girişlerinin numara içinde büyük olabilecek durumlarda faydalıdır.

**Türolmayan** komuttan **subjectdevice** parametresini yeniden kullanın. Bu parametrenin geçerli yapılandırması, **iç katalogdan önceden tanımlanmış girdileri kabul**ediyor. Bu, parametre yapılandırmasında tanımlanan statik cihaz listesini ifade eder. Bu içeriği bağımsız olarak güncelleştirilebilen bir dış veri kaynağına taşımak istiyoruz.

Bunu yapmak için yeni bir Web uç noktası ekleyerek başlayın. Sol bölmedeki **Web uç noktaları** bölümüne gidin ve aşağıdaki yapılandırmaya sahip yeni bir Web uç noktası ekleyin.

| Ayar | Önerilen değer |
|----|----|
| Ad | `getDevices` |
| URL | `https://aka.ms/speech/cc-sampledevices` |
| Yöntem | GET |


URL için önerilen değer sizin için işe yaramazsa, denetlenebilecek cihazların listesini içeren bir JSON döndüren basit bir Web uç noktası yapılandırmanız ve barındırması gerekir. Web uç noktası aşağıdaki şekilde biçimlendirilen bir JSON döndürmelidir:
    
```json
{
    "fan" : [],
    "refrigerator" : [
        "fridge"
    ],
    "lights" : [
        "bulb",
        "bulbs",
        "light"
        "light bulb"
    ],
    "tv" : [
        "telly",
        "television"
        ]
}

```


Ardından, **Subjectdevice** parametre ayarları sayfasına gidin ve özellikleri aşağıdaki şekilde değiştirin.

| Ayar | Önerilen değer |
| ----| ---- |
| Yapılandırma | Dış katalogdan önceden tanımlanmış girişleri kabul et |                               
| Katalog uç noktası | getDevices |
| Yöntem | GET |

Sonra **Kaydet**' i seçin.

> [!IMPORTANT]
> Sol bölmedeki **Web uç** noktası bölümünde ayarlanmış Web uç noktası yoksa bir parametreyi dış katalogdan kabul etmek için yapılandırma seçeneği görmezsiniz.

### <a name="try-it-out"></a>Deneyin

**Tren** ' ı seçin ve eğitim tamamlanmasını bekleyin. Eğitim tamamlandığında, **Test** ' i seçin ve birkaç etkileşimi deneyin.

* Giriş: aç
* Çıkış: hangi cihazı denetlemek istiyorsunuz?
* Giriş: ışıklar
* Çıkış: Tamam, ışıkları açma

> [!NOTE]
> Web uç noktasında barındırılan tüm cihazları şimdi nasıl denetleyebildiğinize dikkat edin. Yine de uygulamayı yeni değişiklikleri test etmek ve uygulamayı yeniden yayımlamak için eğitmeniz gerekir.

## <a name="add-validation-to-parameters"></a>Parametrelere doğrulama ekleme

**Doğrulamalar** , parametre değerindeki kısıtlamaları yapılandırmanıza olanak tanıyan belirli parametre türleri için geçerli olan yapılardır ve değerler kısıtlamaların içine düşmediğinde düzeltme isteminde bulunur. Doğrulama yapısını genişleten parametre türlerinin tam listesi için, [Başvurular](./custom-commands-references.md) ' a gidin.

**Setsıcaklık** komutunu kullanarak doğrulamaları test edin. **Sıcaklık** parametresi için bir doğrulama eklemek için aşağıdaki adımları kullanın.

1. Sol bölmedeki **Setsıcaklık** komutunu seçin.
1. Orta bölmedeki **sıcaklık** ' ı seçin.
1. Sağ bölmede **Doğrulama Ekle** ' yi seçin.
1. **Yeni doğrulama** penceresinde doğrulamayı aşağıdaki şekilde yapılandırın ve **Oluştur**' u seçin.


    | Parametre yapılandırması | Önerilen değer | Açıklama |
    | ---- | ---- | ---- |
    | Minimum Değer | `60` | Sayı parametreleri için bu parametrenin varsaydığı en küçük değer |
    | Maksimum Değer | `80` | Sayı parametreleri için bu parametrenin varsaydığı en büyük değer |
    | Hata yanıtı |  Ilk varyasyon > basit düzenleyici >`Sorry, I can only set temperature between 60 and 80 degrees. What temperature do you want?` | Doğrulama başarısız olursa yeni bir değer sormak isteyip istemediğinizi sor |

    > [!div class="mx-imgBorder"]
    > ![Aralık doğrulaması ekle](media/custom-commands/add-validations-temperature.png)

### <a name="try-it-out"></a>Deneyin

1. Sağ bölmenin en üstünde bulunan **eğitme** simgesini seçin.

1. Eğitim tamamlandığında, **Test** ' i seçin ve birkaç etkileşimi deneyin:

    - Giriş: sıcaklığı 72 derece olarak ayarlayın
    - Çıkış: Tamam, sıcaklık 72 derece olarak ayarlanıyor
    - Giriş: sıcaklığı 45 derece olarak ayarlayın
    - Çıkış: Üzgünüz, yalnızca 60 ve 80 derece arasında sıcaklık ayarlayabiliyorum
    - Giriş: bunun yerine 72 derece yapın
    - Çıkış: Tamam, sıcaklık 72 derece olarak ayarlanıyor

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Nasıl yapılır: etkileşim kuralları ekleme](./how-to-custom-commands-add-interaction-rules.md)
