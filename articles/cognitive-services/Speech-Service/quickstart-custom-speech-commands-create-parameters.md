---
title: 'Quickstart: Parametreler (Önizleme) ile Özel Komut oluşturma - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel komutlar uygulamasına parametreler eklersiniz.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: donkim
ms.openlocfilehash: 96312bac369cfa5fe3cb8a00fd63ecfbec624918
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80348523"
---
# <a name="quickstart-create-a-custom-command-with-parameters-preview"></a>Quickstart: Parametreleri olan Özel Komut oluşturma (Önizleme)

Önceki [makalede,](./quickstart-custom-speech-commands-create-new.md)parametreleri olmadan komutları yanıtlamak için yeni bir Özel Komutlar proje oluşturdu.

Bu makalede, bu uygulamayı parametrelerle genişleteceğiz, böylece birden çok aygıtı açıp kapatmayı işleyebilir.

## <a name="create-parameters"></a>Parametreler Oluştur

1. [Daha önce oluşturduğumuz](./quickstart-custom-speech-commands-create-new.md) projeyi açın
1. Komut artık açma ve kapama ile ilgileneceği için, Komutu "TurnOnOff" olarak yeniden adlandırın.
   - Komutun adının üzerine titreyin ve adı değiştirmek için edit simgesini seçin
1. Kullanıcının aygıtı açmak veya kapatmak isteyip istemediğini görmek için yeni bir parametre oluşturma
   - Parametreler `+` bölümünün yanındaki simgeyi seçin

   > [!div class="mx-imgBorder"]
   > ![Parametre oluşturma](media/custom-speech-commands/create-on-off-parameter.png)

   | Ayar            | Önerilen değer     | Açıklama                                                                                               |
   | ------------------ | ------------------- | --------------------------------------------------------------------------------------------------------- |
   | Adı               | Onoff               | Parametreniz için açıklayıcı bir ad                                                                     |
   | Küresel mi          | unchecked           | Bu parametre için bir değerin projedeki tüm Komutlara genel olarak uygulanıp uygulanmadığını belirten onay kutusu |
   | Gerekli           | checked             | Komutu tamamlamadan önce bu parametre için bir değer gerekip gerekmediğini belirten onay kutusu          |
   | Yanıt şablonu  | "- Aylak mı, kapalı mı?"      | Bilinmediğinde bu parametrenin değerini sormak için bir istem                                       |
   | Tür               | Dize              | Sayı, Dize veya Tarih Saati gibi parametre türü                                               |
   | Yapılandırma      | String Listesi         | Strings için, String List girişleri olası değerler kümesiyle sınırlandırın                                      |
   | Dize listesi değerleri | açık, kapama             | String List parametresi için, olası değerler kümesi ve bunların eşanlamlıları                                |

   - Ardından, aygıtların `+` adını temsil edecek ikinci bir parametre eklemek için simgeyi yeniden seçin. Bu örnekiçin, bir tv ve bir fan

   | Ayar            | Önerilen değer       | Açıklama                                                                                               |
   | ------------------ | --------------------- | --------------------------------------------------------------------------------------------------------- |
   | Adı               | Konu Cihazı         | Parametreniz için açıklayıcı bir ad                                                                     |
   | Küresel mi          | unchecked             | Bu parametre için bir değerin projedeki tüm Komutlara genel olarak uygulanıp uygulanmadığını belirten onay kutusu |
   | Gerekli           | checked               | Komutu tamamlamadan önce bu parametre için bir değer gerekip gerekmediğini belirten onay kutusu          |
   | Yanıt şablonu  | "- Hangi cihaz?"     | Bilinmediğinde bu parametrenin değerini sormak için bir istem                                       |
   | Tür               | Dize                | Sayı, Dize veya Tarih Saati gibi parametre türü                                               |
   | Yapılandırma      | String Listesi           | Strings için, String List girişleri olası değerler kümesiyle sınırlandırın                                      |
   | Dize listesi değerleri | tv, fan               | String List parametresi için, olası değerler kümesi ve bunların eşanlamlıları                                |
   | Eş anlamlılar (tv)      | televizyon, televizyon     | Bir String List Parametresinin olası her değeri için isteğe bağlı eşanlamlılar                                      |

## <a name="add-sample-sentences"></a>Örnek Cümleler Ekle

Parametrelerle, olası tüm kombinasyonları kapsayan örnek cümleler eklemek yararlıdır. Örnek:

1. Tam parametre bilgileri -`"turn {OnOff} the {SubjectDevice}"`
1. Kısmi parametre bilgileri -`"turn it {OnOff}"`
1. Parametre bilgisi yok -`"turn something"`

Farklı miktarda bilgi içeren örnek cümleler, Özel Komutlar uygulamasının hem tek çekimli çözümleri hem de çok döndürürlü çözümleri kısmi bilgilerle çözmesine olanak sağlar.

Bunu göz önünde bulundurarak, örnek cümleleri aşağıda önerildiği gibi parametreleri kullanmak üzere ediniz.

> [!TIP]
> Örnek Cümleler düzenleyicisinde parametrelerinize başvurmak için kıvırcık ayraçlar kullanın. - `turn {OnOff} the {SubjectDevice}`Daha önce oluşturulmuş parametrelere başvurmak için sekme tamamlamayı kullanın.

> [!div class="mx-imgBorder"]
> ![Parametreleri ile Örnek Cümleler](media/custom-speech-commands/create-parameter-sentences.png)

```
turn {OnOff} the {SubjectDevice}
{SubjectDevice} {OnOff}
turn it {OnOff}
turn something {OnOff}
turn something
```

## <a name="add-parameters-to-completion-rule"></a>Tamamlama kuralına parametreler ekleme

Önceki quickstart'ta oluşturduğunuz Tamamlama [kuralını değiştirin:](./quickstart-custom-speech-commands-create-new.md)

1. Yeni bir Koşul ekleyin ve Gerekli parametreyi seçin. Her `OnOff` ikisini de seçin ve`SubjectDevice`
1. Kullanılacak `OnOff` Konuşma Yanıtı eylemini `SubjectDevice`ve:

   ```
   - Ok, turning {OnOff} the {SubjectDevice}
   ```

## <a name="try-it-out"></a>Deneyin

Test sohbet panelini açın ve birkaç etkileşim deneyin.

- Giriş: tv'yi kapatın
- Çıkış: Tamam, tv kapatma

- Giriş: televizyonu kapatın
- Çıkış: Tamam, tv kapatma

- Giriş: kapatın
- Çıktı: Hangi cihaz?
- Giriş: tv
- Çıkış: Tamam, tv kapatma

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Quickstart: Özel Ses ile Özel Komutları Kullanma (Önizleme)](./quickstart-custom-speech-commands-select-custom-voice.md)
