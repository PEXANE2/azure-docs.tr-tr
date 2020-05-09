---
title: 'Hızlı başlangıç: parametrelerle özel komut oluşturma (Önizleme)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel komutlar uygulamasına parametreler ekleyeceksiniz.
services: cognitive-services
author: don-d-kim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: donkim
ms.openlocfilehash: bf77616123f9311f7384fea515f250e47b354c8c
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82853576"
---
# <a name="quickstart-create-a-custom-commands-application-with-parameters-preview"></a>Hızlı başlangıç: parametrelerle özel komutlar uygulaması oluşturma (Önizleme)

[Önceki makalede](./quickstart-custom-speech-commands-create-new.md), parametresiz bir basit özel komutlar uygulaması oluşturdunuz.

Bu makalede, parametreleri kullanarak birden çok cihazı açmayı ve kapatmayı işleyebilmesi için bu uygulamayı genişletirsiniz.

## <a name="create-parameters"></a>Parametre oluştur

1. [Daha önce oluşturduğunuz](./quickstart-custom-speech-commands-create-new.md) projeyi açın
1. Birden çok cihazı açmak ve kapatmak için mevcut komutu düzenleyelim.
1. Komut şimdi üzerinde işlem yaptığından ve kapatıcağından, komutunu olarak `TurnOnOff`yeniden adlandırın.
   - Sol bölmede, `TurnOn` komutu seçin ve ardından bölmenin üst `...` `+ New command` kısmındaki simgesinin yanındaki simgesine tıklayın.
   
   - Simge `Rename` seçin. **Yeniden Adlandır komut** açılır penceresinde **adı** olarak `TurOnOff`değiştirin. Sonra **Kaydet**' i seçin.

1. Ardından, kullanıcının cihazı açmak mı yoksa kapatmak mı istediğini göstermek için yeni bir parametre oluşturun.
   - Orta `+ Add` bölmenin en üstünde bulunan simgesi seçin. Açılan listeden **parametre**' ı seçin.
   - En sağdaki bölmede **Parametreler** yapılandırma bölümünü görebilirsiniz.
   - **Ad**için değer ekleyin.
   - **Gerekli** onay kutusunu işaretleyin. **Gerekli bir parametre için yanıt Ekle** penceresinde **basit düzenleyici** ve **ilk çeşitlemeyi**seçin, Ekle
        ```
        On or Off?
        ```
   - **Güncelleştir**’i seçin.

       > [!div class="mx-imgBorder"]
       > ![Gerekli parametre yanıtı oluştur](media/custom-speech-commands/add-required-on-off-parameter-response.png)
   
   - Daha sonra, parametresinin geri kalanını aşağıdaki gibi yapılandıralım ve yapılandırma tüm yapılandırmalarını parametreye kaydetmeyi `Save` seçin.
       

       | Yapılandırma      | Önerilen değer     | Açıklama                                                      |
       | ------------------ | ----------------| ---------------------------------------------------------------------|
       | Name               | Mikrofon           | Parametre için açıklayıcı bir ad                                                                           |
       | Geneldir          | unchecked       | Bu parametre için bir değerin uygulamadaki tüm komutlara genel olarak uygulanıp uygulanmadığı belirten onay kutusu|
       | Gerekli           | checked         | Komutu tamamlamadan önce Bu parametre için bir değer gerekip gerekmediğini belirten onay kutusu |
       | Gerekli parametre için yanıt      |Basit düzenleyici-> veya kapalı?      | Bilinmiyorsa bu parametrenin değerini sormak için bir istem |
       | Tür               | Dize          | Parametre türü, örneğin sayı, dize, tarih saat veya Coğrafya   |
       | Yapılandırma      | Önceden tanımlanmış giriş değerlerini iç katalogdan kabul et | Dizeler için bu, girişleri olası bir değerler kümesiyle sınırlandırır |
       | Önceden tanımlanmış giriş değerleri     | açık, kapalı             | Olası değerler ve bunların diğer adları kümesi         |
       
        > [!div class="mx-imgBorder"]
        > ![Parametre oluştur](media/custom-speech-commands/create-on-off-parameter.png)

   - Ardından, aşağıdaki yapılandırmaya `+ Add` sahip cihazların adını temsil edecek ikinci bir parametre eklemek için simgeyi yeniden seçin.
   

       | Ayar            | Önerilen değer       | Açıklama                                                                                               |
       | ------------------ | --------------------- | --------------------------------------------------------------------------------------------------------- |
       | Name               | SubjectDevice         | Parametre için açıklayıcı bir ad                                                                     |
       | Geneldir          | unchecked             | Bu parametre için bir değerin uygulamadaki tüm komutlara genel olarak uygulanıp uygulanmadığı belirten onay kutusu |
       | Gerekli           | checked               | Komutu tamamlamadan önce Bu parametre için bir değer gerekip gerekmediğini belirten onay kutusu          |
       | Basit düzenleyici      | Hangi cihaz?    | Bilinmiyorsa bu parametrenin değerini sormak için bir istem                                       |
       | Tür               | Dize                | Parametre türü, örneğin sayı, dize, tarih saat veya Coğrafya                                                |
       | Yapılandırma      | Önceden tanımlanmış giriş değerlerini iç katalogdan kabul et | Dizeler için, bir dize listesi girişleri olası değerler kümesiyle sınırlandırır       |
       | Önceden tanımlanmış giriş değerleri | TV, fan               | Olası değerler ve bunların diğer adları kümesi                               |
       | Diğer adlar (TV)      | televizyon, telefon     | Önceden tanımlanmış giriş değerlerinin olası her değeri için isteğe bağlı diğer adlar                                 |

## <a name="add-example-sentences"></a>Örnek cümleler ekleyin

Parametreleri olan komutlarla, olası tüm birleşimleri kapsayan örnek cümleler eklemek yararlıdır. Örneğin:

1. Tam parametre bilgileri-`turn {OnOff} the {SubjectDevice}`
1. Kısmi parametre bilgileri-`turn it {OnOff}`
1. Parametre bilgisi yok-`turn something`

Farklı bilgi derecesine sahip örnek cümleler, özel komutlar uygulamasının, kısmi bilgilerle hem tek kararlı çözümlerin hem de çok yönlü çözümlerin çözümlenmesine izin verir.

Bu şekilde, parametreleri aşağıda önerilen şekilde kullanmak için örnek cümleleri düzenleyin.

```
turn {OnOff} the {SubjectDevice}
{SubjectDevice} {OnOff}
turn it {OnOff}
turn something {OnOff}
turn something
```
> [!TIP]
> Örnek cümleler düzenleyicisinde parametrelerinizi belirtmek için küme ayraçları kullanın. - `turn {OnOff} the {SubjectDevice}`Daha önce oluşturulan parametreler tarafından desteklenen otomatik tamamlama sekmesini kullanın.

## <a name="add-parameters-to-completion-rules"></a>Tamamlama kurallarına parametreler ekleme

[Önceki hızlı](./quickstart-custom-speech-commands-create-new.md)başlangıçta oluşturduğumuz tamamlama kuralını değiştirin.

1. **Koşullar** bölümünde **+ Koşul Ekle** ' yi seçerek yeni bir koşul ekleyin
1. Yeni açılan **Yeni koşulda**, `Required parameters` **tür** açılır listesinden seçim yapın. Aşağıdaki denetim listesinde, `OnOff` ve `SubjectDevice`' i işaretleyin.
1. **Oluştur**'a tıklayın.
1. **Eylemler** bölümünde, mevcut konuşma yanıtı gönder eylemini, eylemin üzerine gelip düzenleme simgesine tıklayarak düzenleyin. Bu kez, yeni oluşturulan `OnOff` ve `SubjectDevice` parametreleri kullanalım

    ```
    Ok, turning {OnOff} the {SubjectDevice}
    ```

## <a name="try-it-out"></a>Deneyin
1. Sağ `Train` bölmenin en üstünde bulunan simgesi seçin.

1. Eğitim tamamlandığında, öğesini seçin `Test`.
    - Yeni bir **test, uygulama** pencereniz görüntülenir.
    - Birkaç etkileşimi deneyin.

        - Giriş: TV 'yi kapatma
        - Çıkış: Tamam, TV 'yi kapatma        
        - Giriş: televizyon kapatma
        - Çıkış: Tamam, TV 'yi kapatma
        - Giriş: kapat
        - Çıkış: hangi cihaz?
        - Giriş: TV
        - Çıkış: Tamam, TV 'yi kapatma

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Hızlı başlangıç: özel sesle özel komutlar kullanma (Önizleme)](./quickstart-custom-speech-commands-select-custom-voice.md)
