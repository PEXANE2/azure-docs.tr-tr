---
title: 'Nasıl yapılır: özel komutlara basit komutlar ekleme uygulama-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel komutlara parametre eklemeyi öğreneceksiniz
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: sausin
ms.openlocfilehash: d2a14a501ebcf0913804ce39019a3fa4018ca141
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85362382"
---
# <a name="add-parameters-to-commands"></a>Komutlara parametre ekleme

Bu makalede, özel komutlara parametre eklemeyi öğreneceksiniz. Parametreler, bir görevi tamamlamaya yönelik komutlar için gereken bilgiler. Karmaşık senaryolarda parametreler, özel eylemleri tetikleyen koşulları tanımlamak için de kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * [Nasıl yapılır: basit komutlarla uygulama oluşturma](./how-to-custom-commands-create-application-with-simple-commands.md)

## <a name="configure-parameters-for-turnon-command"></a>TurnOn komutu için parametreleri yapılandırma

Birden çok cihazı açmak ve kapatmak için mevcut **turnOn** komutunu düzenleyin.

1. Komut şimdi hem on hem de off senaryosunu işleyeceğinden, komutu **Turnonoff**olarak yeniden adlandırın.
   1. Sol bölmede, **turnOn** komutunu seçin ve bölmenin üst kısmındaki **Yeni komut** ' nin yanındaki üç nokta (...) düğmesini seçin.
   
   1. **Yeniden Adlandır**' ı seçin. Komut pencerelerini **Yeniden Adlandır** ' da **adı** **turnonoff**olarak değiştirin.

1. Ardından, bu komuta kullanıcının cihazı açmak mı yoksa kapatmak mı istediğini temsil eden yeni bir parametre eklersiniz.
   1. Orta bölmenin en üstünde bulunan **Ekle** ' yi seçin. Açılan listeden **parametre**' ı seçin.
   1. Sağ bölmedeki **Parametreler** bölümünde, **ad** kutusuna **OnOff**olarak bir değer ekleyin.
   1. **Gerekli**' yi seçin. **Gerekli bir parametre için yanıt Ekle** penceresinde **basit düzenleyici**' yi seçin. **İlk çeşitte**, Ekle
        ```
        On or Off?
        ```
   1. **Güncelleştir**’i seçin.

       > [!div class="mx-imgBorder"]
       > ![Gerekli parametre yanıtı oluştur](media/custom-commands/add-required-on-off-parameter-response.png)
   
   1. Şimdi parametre özelliklerini yapılandıracağız. Bir komutun tüm yapılandırma özelliklerinin açıklaması için, [Başvurular](./custom-commands-references.md)' a gidin. Parametresinin geri kalanını aşağıdaki şekilde yapılandırın:
      

       | Yapılandırma      | Önerilen değer     | Description                                                      |
       | ------------------ | ----------------| ---------------------------------------------------------------------|
       | Name               | `OnOff`           | Parametre için açıklayıcı bir ad                                                                           |
       | Geneldir          | unchecked       | Bu parametre için bir değerin uygulamadaki tüm komutlara genel olarak uygulanıp uygulanmadığı belirten onay kutusu|
       | Gerekli           | checked         | Komutu tamamlamadan önce Bu parametre için bir değer gerekip gerekmediğini belirten onay kutusu |
       | Gerekli parametre için yanıt      |Basit düzenleyici >`On or Off?`      | Bilinmiyorsa bu parametrenin değerini sormak için bir istem |
       | Tür               | Dize          | Parametre türü, örneğin sayı, dize, tarih saat veya Coğrafya   |
       | Yapılandırma      | Önceden tanımlanmış giriş değerlerini iç katalogdan kabul et | Dizeler için bu, girişleri olası bir değerler kümesiyle sınırlandırır |
       | Önceden tanımlanmış giriş değerleri     | `on`, `off`           | Olası değerler ve bunların diğer adları kümesi         |
       
        
   1. Önceden tanımlanmış giriş değerlerini eklemek için, **önceden tanımlanmış bir giriş Ekle** ve **Yeni öğe** penceresinde, yukarıdaki tabloda belirtilen **adı** yazın. Bu durumda, diğer adları kullanmıyoruz, bu nedenle boş bırakabilirsiniz. 
    > [!div class="mx-imgBorder"]
        > ![Parametre oluştur](media/custom-commands/create-on-off-parameter.png)
   1. Parametrenin tüm yapılandırmasını kaydetmek için **Kaydet** ' i seçin.
 
 ### <a name="add-subjectdevice-parameter"></a>SubjectDevice parametresi Ekle 

   1. Sonra, bu komut kullanılarak denetlenebilecek cihazların adını temsil eden ikinci bir parametre eklemek için yeniden **Ekle** ' yi seçin. Aşağıdaki yapılandırmayı kullanın.
   

       | Ayar            | Önerilen değer       |
       | ------------------ | --------------------- |
       | Name               | `SubjectDevice`         |
       | Geneldir          | unchecked             |
       | Gerekli           | checked               |
       | Gerekli parametre için yanıt     | Basit düzenleyici >`Which device do you want to control?`    | 
       | Tür               | Dize                |          |
       | Yapılandırma      | Önceden tanımlanmış giriş değerlerini iç katalogdan kabul et | 
       | Önceden tanımlanmış giriş değerleri | `tv`, `fan`               |
       | Diğer adlar ( `tv` )      | `television`, `telly`     |

   1. **Kaydet**’i seçin

### <a name="modify-example-sentences"></a>Örnek cümleleri değiştirme

Parametreleri olan komutlar için, olası tüm birleşimleri kapsayan örnek cümleler eklemek yararlıdır. Örneğin:

* Tam parametre bilgileri-`turn {OnOff} the {SubjectDevice}`
* Kısmi parametre bilgileri-`turn it {OnOff}`
* Parametre bilgisi yok-`turn something`

Farklı bilgi derecesine sahip örnek cümleler, özel komutlar uygulamasının, kısmi bilgilerle hem tek kararlı çözümlerin hem de çok yönlü çözümlerin çözümlenmesine izin verir.

Bu şekilde, parametreleri aşağıda önerilen şekilde kullanmak için örnek cümleleri düzenleyin:

```
turn {OnOff} the {SubjectDevice}
{SubjectDevice} {OnOff}
turn it {OnOff}
turn something {OnOff}
turn something
```

**Kaydet**’i seçin.

> [!TIP]
> Örnek cümleler düzenleyicisinde parametrelerinizi belirtmek için küme ayraçları kullanın. - `turn {OnOff} the {SubjectDevice}`Daha önce oluşturulan parametreler tarafından desteklenen otomatik tamamlama sekmesini kullanın.

### <a name="modify-completion-rules-to-include-parameters"></a>Tamamlama kurallarını parametreleri içerecek şekilde değiştirme

Mevcut tamamlama kuralını **ConfirmationResponse**değiştirin.

1. **Koşullar** bölümünde **Koşul Ekle**' yi seçin.
1. **Yeni koşul** penceresinde, **tür** listesinde, **gerekli parametreler**' i seçin. Aşağıdaki denetim listesinde, hem **OnOff** hem de **subjectdevice**' ı işaretleyin.
1. **Oluştur**'u seçin.
1. **Eylemler** bölümünde, mevcut **konuşma yanıtı gönder** eylemini, eylemin üzerine gelerek ve Düzenle düğmesini seçerek düzenleyin. Bu kez, yeni oluşturulan **OnOff** ve **subjectdevice** parametrelerini kullanın

    ```
    Ok, turning the {SubjectDevice} {OnOff}
    ```
1. **Kaydet**’i seçin.

### <a name="try-it-out"></a>Deneyin
1. Sağ bölmenin en üstünde bulunan **eğitme** simgesini seçin.

1. Eğitim tamamlandığında **Test**' i seçin. Uygulama pencerenizin bir **testi** görüntülenir.
 Birkaç etkileşimi deneyin.

    - Giriş: TV 'yi kapatma
    - Çıkış: Tamam, TV 'yi kapatma
    - Giriş: televizyon kapatma
    - Çıkış: Tamam, TV 'yi kapatma
    - Giriş: kapat
    - Çıkış: hangi cihazı denetlemek istiyorsunuz?
    - Giriş: TV
    - Çıkış: Tamam, TV 'yi kapatma

## <a name="configure-parameters-for-settemperature-command"></a>Setsıcaklık komutu için parametreleri yapılandırma

Kullanıcı tarafından yönlendirilerek sıcaklığın ayarlanmış olduğunu etkinleştirmek için **setsıcaklık** komutunu değiştirin.

Aşağıdaki yapılandırmayla yeni parametre **sıcaklığı** ekleyin

| Yapılandırma      | Önerilen değer     |
| ------------------ | ----------------|
| Name               | `Temperature`           |
| Gerekli           | checked         |
| Gerekli parametre için yanıt      | Basit düzenleyici >`What temperature would you like?`
| Tür               | Sayı          |


Örneği aşağıdaki değerlere göre düzenleyin.

```
set the temperature to {Temperature} degrees
change the temperature to {Temperature}
set the temperature
change the temperature
```

Mevcut tamamlama kurallarını aşağıdaki yapılandırmaya göre düzenleyin.

| Yapılandırma      | Önerilen değer     |
| ------------------ | ----------------|
| Koşullar         | Gerekli parametre > sıcaklık           |
| Eylemler           | Konuşma yanıtı gönder >`Ok, setting temperature to {Temperature} degrees` |

### <a name="try-it-out"></a>Deneyin

Değişiklikleri birkaç etkileşimle **eğitme** ve **Test** etme.

- Giriş: sıcaklık ayarla
- Çıkış: hangi sıcaklık istiyorsunuz?
- Giriş: 50 derece
- Çıkış: Tamam, sıcaklık 50 derece olarak ayarlanıyor

## <a name="configure-parameters-to-the-setalarm-command"></a>Parametreleri SetAlarm komutuna göre yapılandırma

Aşağıdaki yapılandırmaya sahip **DateTime** adlı bir parametre ekleyin.

   | Ayar                           | Önerilen değer                     | 
   | --------------------------------- | ----------------------------------------|
   | Name                              | `DateTime`                               |
   | Gerekli                          | checked                                 |
   | Gerekli parametre için yanıt   | Basit düzenleyici >`For what time?`            | 
   | Tür                              | DateTime                                |
   | Tarih Varsayılanları                     | Tarih eksik ise bugün kullan            |
   | Zaman Varsayılanları                     | Eğer zaman içinde kullanım günü kullanım günü     |


> [!NOTE]
> Bu makalede dize, sayı ve tarih saat parametre türlerinden ağırlıklı kullanıyoruz. Desteklenen tüm parametre türlerinin ve özelliklerinin listesi için, [Başvurular](./custom-commands-references.md)' a gidin.


Aşağıdaki değerlere yapılan örnekleri düzenleyin.

```
set an alarm for {DateTime}
set alarm {DateTime}
alarm for {DateTime}
```

Mevcut tamamlama kurallarını aşağıdaki yapılandırmaya göre düzenleyin.

   | Ayar    | Önerilen değer                               |
   | ---------- | ------------------------------------------------------- |
   | Eylemler    | Konuşma yanıtı gönder-`Ok, alarm set for {DateTime}`  |


### <a name="try-it-out"></a>Deneyin

Değişiklikleri **eğitme** ve **Test** etme.
- Giriş: yarın, öğleden sonra alarmını ayarla
- Çıkış: Tamam, 2020-05-02 12:00:00 için alarm kümesi
- Giriş: bir alarm ayarlama
- Çıkış: ne zaman?
- Giriş: 17:00
- Çıkış: Tamam, 2020-05-01 17:00:00 için alarm kümesi


## <a name="try-out-all-the-commands"></a>Tüm komutları deneyin

Farklı komutlarla ilgili olan söyleylerini kullanarak üç komutu birlikte test edin. Farklı komutlar arasında geçiş gerçekleştirebileceğinizi unutmayın.

- Giriş: bir alarm ayarlama
- Çıkış: hangi süre?
- Giriş: TV 'yi açın
- Çıkış: Tamam, TV 'yi açma
- Giriş: bir alarm ayarlama
- Çıkış: hangi süre?
- Giriş: 17:00
- Çıkış: Tamam, 2020-05-01 17:00:00 için alarm kümesi

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Nasıl yapılır: komut parametrelerine yapılandırma ekleme](./how-to-custom-commands-add-parameter-configuration.md)
