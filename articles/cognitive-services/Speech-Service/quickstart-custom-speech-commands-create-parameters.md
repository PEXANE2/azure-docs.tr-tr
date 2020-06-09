---
title: 'Hızlı başlangıç: parametreler ile özel komutlar önizleme uygulaması oluşturma-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede, bir özel komutlar uygulamasına parametreler ekleyerek birden çok cihazı açıp kapatabilirsiniz.
services: cognitive-services
author: nitinme
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: nitinme
ms.openlocfilehash: 940b3604487fbef7736c0d8f4f9299563b0b86d5
ms.sourcegitcommit: 5504d5a88896c692303b9c676a7d2860f36394c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84509313"
---
# <a name="quickstart-create-a-custom-commands-preview-application-with-parameters"></a>Hızlı başlangıç: parametrelerle özel komutlar önizleme uygulaması oluşturma

[Önceki makalede](./quickstart-custom-speech-commands-create-new.md), parametresiz bir basit özel komutlar uygulaması oluşturdunuz.

Bu makalede, birden çok cihazı açıp kapatmak için bu uygulamayı parametrelerle genişleteceksiniz.

## <a name="create-parameters"></a>Parametreler oluşturma

1. [Önceki makalede](./quickstart-custom-speech-commands-create-new.md)oluşturduğunuz projeyi açın.

   Mevcut komutu, birden çok cihazı açmak ve kapatmak için kullanılacak şekilde düzenliyoruz.
1. Komut şimdi hem açık hem de kapalı olacağı için, onu **Turnonoff**olarak yeniden adlandırın.
   1. Sol bölmede, **turnOn** komutunu seçin ve bölmenin üst kısmındaki **Yeni komut** ' nin yanındaki üç nokta (**...**) düğmesini seçin.
   
   1. **Yeniden Adlandır**' ı seçin. **Yeniden Adlandır komut** penceresinde, **adı** **turonoff**olarak değiştirin. **Kaydet**'i seçin.

1. Kullanıcının cihazı açmak mı yoksa kapatmak mı istediğini göstermek için bir parametre oluşturun.
   1. Orta bölmenin üst kısmında **Ekle** ' yi seçin. Açılan listede **parametre**' ı seçin.
   1. Sağ bölmedeki **Parametreler** bölümünde, **ad** kutusuna bir değer ekleyin.
   1. **Gerekli**' yi seçin. **Gerekli bir parametre için yanıt Ekle** penceresinde **basit düzenleyici**' yi seçin. **İlk değişim** kutusunda şu metni girin:
        ```
        On or Off?
        ```
   1. **Güncelleştir**’i seçin.

       > [!div class="mx-imgBorder"]
       > ![Gerekli parametre yanıtı oluştur](media/custom-speech-commands/add-required-on-off-parameter-response.png)
   
1. Parametresinin geri kalanını aşağıdaki şekilde yapılandırın:
       

    | Yapılandırma      | Önerilen değer     | Description                                                      |
    | ------------------ | ----------------| ---------------------------------------------------------------------|
    | **Adı**               | **Mikrofon**           | Parametre için açıklayıcı bir ad.                                                                  |
    | **Geneldir**          | Temizlenen       | Parametrenin bir değerinin uygulamadaki tüm komutlara Global olarak uygulanıp uygulanmadığını belirten bir onay kutusu.|
    | **Gerekli**           | Seçildi         | Parametre için bir değer gerekip gerekmediğini belirten bir onay kutusu.  |
    | **Gerekli parametre için yanıt**      |**Basit düzenleyici-> veya kapalı?**      | Bilinmiyorsa parametrenin değerini sormak için bir istem. |
    | **Tür**               | **Dize**          | Parametre türü. Örneğin, sayı, dize, tarih saat, coğrafya.   |
    | **Yapılandırma**      | **Önceden tanımlanmış giriş değerlerini iç katalogdan kabul et** | Dizeler için bu ayar girişleri olası bir değerler kümesiyle sınırlandırır. |
    | **Önceden tanımlanmış giriş değerleri**     | **Açık**, **kapalı**             | Olası değerler ve bunların diğer adları kümesi.         |
       


    > [!div class="mx-imgBorder"]
    > ![Parametre oluştur](media/custom-speech-commands/create-on-off-parameter.png)

1. Ayarları kaydetmek için **Kaydet**’i seçin.

 1. İkinci bir parametre eklemek için yeniden **Ekle** ' yi seçin. Bu parametre, cihazın adını temsil eder. Şu ayarları kullanın:
   

       | Ayar            | Önerilen değer       | Description                                                                                               |
       | ------------------ | --------------------- | --------------------------------------------------------------------------------------------------------- |
       | **Adı**               | **SubjectDevice**         | Parametre için açıklayıcı bir ad.                                                                     |
       | **Geneldir**          | Temizlenen             | Parametrenin bir değerinin uygulamadaki tüm komutlara Global olarak uygulanıp uygulanmadığını belirten bir onay kutusu. |
       | **Gerekli**           | Seçildi               | Parametre için bir değer gerekip gerekmediğini belirten bir onay kutusu.          |
       | **Basit düzenleyici**      | **Hangi cihaz?**    | Bilinmiyorsa parametrenin değerini sormak için bir istem.                                       |
       | **Tür**               | **Dize**                | Parametre türü. Örneğin, sayı, dize, tarih saat, coğrafya.                                                |
       | **Yapılandırma**      | **Önceden tanımlanmış giriş değerlerini iç katalogdan kabul et** | Dizeler için bu ayar girişleri olası bir değerler kümesiyle sınırlandırır.       |
       | **Önceden tanımlanmış giriş değerleri** | **TV**, **fan**               | Olası değerler ve bunların diğer adları kümesi.                               |
       | **Diğer adlar** (TV)      | **televizyon**, **Telefon**     | Önceden tanımlanmış giriş değerlerinin her biri için isteğe bağlı diğer adlar.                                 |

## <a name="add-example-sentences"></a>Örnek cümleler ekleyin

Parametrelere sahip komutlarda, olası tüm birleşimleri kapsayan örnek cümleler eklemek yararlıdır. Örneğin:

- Parametre bilgilerini doldurun:`turn {OnOff} the {SubjectDevice}`
- Kısmi parametre bilgileri:`turn it {OnOff}`
- Parametre bilgisi yok:`turn something`

Farklı miktarda bilgilere sahip örnek cümleler, özel komutlar uygulamasının, kısmi bilgiler içeren tek kararlı çözümlerin ve çoklu çift çözümlerin çözümlenme yapmasına izin verir.

Göz önünde bulundurularak, parametreleri burada önerilen şekilde kullanmak için örnek cümleleri düzenleyin:

```
turn {OnOff} the {SubjectDevice}
{SubjectDevice} {OnOff}
turn it {OnOff}
turn something {OnOff}
turn something
```
> [!TIP]
> Örnek cümleler düzenleyicisinde, parametrelerinizi belirtmek için küme ayraçları kullanın: `turn {OnOff} the {SubjectDevice}` .
>
> Daha önce oluşturulmuş parametreler tarafından tanımlanan otomatik tamamlama sekmesini kullanın.

## <a name="add-parameters-to-completion-rules"></a>Tamamlama kurallarına parametreler ekleme

[Önceki hızlı](./quickstart-custom-speech-commands-create-new.md)başlangıçta oluşturduğunuz tamamlama kuralını değiştirin.

1. **Koşullar** bölümünde **Koşul Ekle**' yi seçin.
1. **Yeni koşul** penceresinde, **tür** listesinde, **gerekli parametreler**' i seçin. Listede hem **OnOff** hem de **subjectdevice**' ı seçin.
1. **Oluştur**'u seçin.
1. **Eylemler** bölümünde, mevcut **konuşma yanıtı gönder** eylemini, eylemin üzerine gelerek ve Düzenle düğmesini seçerek düzenleyin. Bu kez, yeni `OnOff` ve parametrelerini kullanın `SubjectDevice` :

    ```
    Ok, turning {OnOff} the {SubjectDevice}
    ```

## <a name="try-it-out"></a>Deneyin
1. Sağ bölmenin en üstünde **eğitme** ' yi seçin.

1. Eğitim bittiğinde **Test**' i seçin.
    
    Uygulama pencerenizin bir **testi** görüntülenir.

1. Birkaç etkileşimi deneyin.

        - Input: turn off the tv
        - Output: Ok, turning off the tv        
        - Input: turn off the television
        - Output: Ok, turning off the tv
        - Input: turn it off
        - Output: Which device?
        - Input: the tv
        - Output: Ok, turning off the tv

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Hızlı başlangıç: özel sesle özel komutlar kullanma (Önizleme)](./quickstart-custom-speech-commands-select-custom-voice.md)
