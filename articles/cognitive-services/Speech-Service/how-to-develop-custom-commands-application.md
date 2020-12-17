---
title: 'Nasıl yapılır: özel komutlar geliştirme uygulamalar-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu nasıl yapılır, özel komutlar uygulamaları geliştirmeyi ve özelleştirmeyi öğrenirsiniz. Özel komutlar, sesli öncelikli etkileşim deneyimleri için iyileştirilmiş zengin sesli komut uygulamaları oluşturmayı kolaylaştırır ve görev tamamlama veya komut ve denetim senaryoları için, özellikle de Nesnelerin İnterneti (IoT) cihazları, çevresel ve gözetimsiz cihazlar için iyi bir şekilde eşleşir.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/15/2020
ms.author: trbye
ms.openlocfilehash: 98c0459e0b67102458169147b1d39e98e2b3e2b1
ms.sourcegitcommit: 8c3a656f82aa6f9c2792a27b02bbaa634786f42d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97633072"
---
# <a name="develop-custom-commands-applications"></a>Özel komut uygulamaları geliştirme

Bu nasıl yapılır, özel komutlar uygulamaları geliştirmeyi ve yapılandırmayı öğrenirsiniz. Özel komutlar, sesli öncelikli etkileşim deneyimleri için iyileştirilmiş zengin sesli komut uygulamaları oluşturmayı kolaylaştırır ve görev tamamlama veya komut ve denetim senaryoları için, özellikle de Nesnelerin İnterneti (IoT) cihazları, çevresel ve gözetimsiz cihazlar için iyi bir şekilde eşleşir.

Bu makalede, bir TV 'yi açıp kapatabilir, sıcaklığın ayarlanması ve bir alarm ayarlayabilmeniz için bir uygulama oluşturacaksınız. Bu temel komutları oluşturduktan sonra, komutları özelleştirmek için aşağıdaki seçenekler ele alınmıştır:

* Komutlara parametreler ekleme
* Komut parametrelerine yapılandırma ekleme
* Etkileşim kuralları oluşturma
* Konuşma yanıtları için dil oluşturma şablonları oluşturma
* Özel ses kullanma 

## <a name="create-application-with-simple-commands"></a>Basit komutlarla uygulama oluşturma

İlk olarak, boş bir özel komutlar uygulaması oluşturarak başlayın. Ayrıntılar için [hızlı başlangıç](quickstart-custom-commands-application.md)bölümüne bakın. Bu kez, bir projeyi içeri aktarmak yerine boş bir proje oluşturursunuz.

1. **Ad** kutusuna proje adını `Smart-Room-Lite` (veya istediğiniz bir şeyi) girin.
1. **Dil** listesinde **İngilizce (Birleşik Devletler)** seçeneğini belirleyin.
1. Seçtiğiniz bir LUO kaynağını seçin veya oluşturun.

   > [!div class="mx-imgBorder"]
   > ![Proje oluşturma](media/custom-commands/create-new-project.png)

### <a name="update-luis-resources-optional"></a>LUSıS kaynaklarını güncelleştirme (isteğe bağlı)

**Yeni proje** penceresinde seçtiğiniz yazma kaynağını güncelleştirebilir ve bir tahmin kaynağı ayarlayabilirsiniz. Tahmin kaynağı, özel komutlar uygulamanız yayımlandığında tanınabilmesi için kullanılır. Geliştirme ve test aşamaları sırasında bir tahmin kaynağına ihtiyacınız yoktur.

### <a name="add-turnon-command"></a>TurnOn komutu Ekle

Az önce oluşturduğunuz boş **Akıllı Oda-Lite** özel komutları uygulamasında, bir utterance işleyen `turn on the tv` ve iletiyle yanıt veren basit bir komut ekleyin `Ok, turning the tv on` .

1. Sol bölmenin en üstünde bulunan **Yeni komut** ' i seçerek yeni bir komut oluşturun. **Yeni komut** penceresi açılır.
1. **Ad** alanı için **turnOn** olarak değer sağlayın.
1. **Oluştur**’u seçin.

Orta bölmede komutunun farklı özellikleri listelenir. Komutun aşağıdaki özelliklerini yapılandırırsınız. Bir komutun tüm yapılandırma özelliklerinin açıklaması için, [Başvurular](./custom-commands-references.md)' a gidin.

| Yapılandırma            | Açıklama                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **Örnek cümleler** | Kullanıcının bu komutu tetikleyebilmesi için söyledikleri örnek                                                                 |
| **Parametreler**       | Komutu tamamlaması için gereken bilgiler                                                                                |
| **Tamamlama kuralları** | Komutu yerine getirmek için gerçekleştirilecek eylemler. Örneğin, kullanıcıya yanıt vermek veya başka bir Web hizmetiyle iletişim kurmak için. |
| **Etkileşim kuralları**   | Daha belirgin veya karmaşık durumları işlemek için ek kurallar                                                              |


> [!div class="mx-imgBorder"]
> ![Komut oluşturma](media/custom-commands/add-new-command.png)

#### <a name="add-example-sentences"></a>Örnek cümleler ekleyin

**Örnek cümleler** bölümüne başlayalım ve kullanıcının söyleme bir örneğini sağlayın.

1. Orta bölmedeki **örnek cümleler** bölümünü seçin.
1. Sağ bölmede örnek ekleyin:

    ```
    turn on the tv
    ```

1.  Bölmenin en üstünde bulunan **Kaydet** ' i seçin.

Şimdilik, parametre yok, bu nedenle **tamamlama kuralları** bölümüne taşıyabiliriz.

#### <a name="add-a-completion-rule"></a>Tamamlama kuralı ekleme

Sonra, komutun bir tamamlama kuralına sahip olması gerekir. Bu kural kullanıcıya bir karşılama eyleminin alındığını söyler. Kurallar ve tamamlama kuralları hakkında daha fazla bilgi edinmek için [Başvurular](./custom-commands-references.md)'a gidin.

1. Varsayılan **tamamlama kuralını seçin** ve bunu aşağıdaki gibi düzenleyin:

    
    | Ayar    | Önerilen değer                          | Açıklama                                        |
    | ---------- | ---------------------------------------- | -------------------------------------------------- |
    | **Ad**       | ConfirmationResponse                  | Kuralın amacını açıklayan bir ad          |
    | **Koşullar** | Yok                                     | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |
    | **Eylemler**    | Ilk varyasyon > basit düzenleyici > konuşma yanıtı gönderin > `Ok, turning the tv on` | Kural koşulu true olduğunda gerçekleştirilecek eylem |

   > [!div class="mx-imgBorder"]
   > ![Konuşma yanıtı oluşturma](media/custom-commands/create-speech-response-action.png)

1. Eylemi kaydetmek için **Kaydet** ' i seçin.
1. **Tamamlama kuralları** bölümüne geri döndüğünüzde tüm değişiklikleri kaydetmek için **Kaydet** ' i seçin. 

    > [!NOTE]
    > Komutuyla birlikte gelen varsayılan tamamlama kuralını kullanmak gerekli değildir. Gerekirse, var olan varsayılan tamamlama kuralını silebilir ve kendi kuralınızı ekleyebilirsiniz.

### <a name="add-settemperature-command"></a>Setsıcaklık Ekle komutu

Şimdi, tek bir utterlik alacak ve iletiyle yanıt verecek bir daha fazla komut **Setsıcaklığı** ekleyin `set the temperature to 40 degrees` `Ok, setting temperature to 40 degrees` .

Örnek cümlesini kullanarak yeni bir komut oluşturmak için **turnOn** komutu için gösterilen adımları izleyin, "**sıcaklığın 40 derece olarak ayarlanması**".

Ardından **, var olan** tamamlama kurallarını aşağıdaki gibi düzenleyin:

| Ayar    | Önerilen değer                          |
| ---------- | ---------------------------------------- |
| Name  | ConfirmationResponse                  |
| Koşullar | Yok                                     |
| Eylemler    | Ilk varyasyon > basit düzenleyici > konuşma yanıtı gönderin > `Ok, setting temperature to 40 degrees` |

Komutta yapılan tüm değişiklikleri kaydetmek için **Kaydet** ' i seçin.

### <a name="add-setalarm-command"></a>SetAlarm komutu Ekle

"**9 har için alarm ayarla**" örnek cümlesini kullanarak yeni bir komut **SetAlarm** oluşturun. Ardından **, var olan** tamamlama kurallarını aşağıdaki gibi düzenleyin:

| Ayar    | Önerilen değer                          |
| ---------- | ---------------------------------------- |
| Kural Adı  | ConfirmationResponse                  |
| Koşullar | Yok                                     |
| Eylemler    | Ilk varyasyon > basit düzenleyici > konuşma yanıtı gönderin >`Ok, setting an alarm for 9 am tomorrow` |

Komutta yapılan tüm değişiklikleri kaydetmek için **Kaydet** ' i seçin.

### <a name="try-it-out"></a>Deneyin

Test sohbeti panelini kullanarak davranışı test edin. Sağ bölmenin en üstünde bulunan **eğitme** simgesini seçin. Eğitim tamamlandığında **Test**' i seçin. Ses veya metin ile aşağıdaki söylenişi örneklerini deneyin:

- Şunu yazın: sıcaklığını 40 derece olarak ayarlayın
- Beklenen yanıt: Tamam, sıcaklık 40 derece olarak ayarlanıyor
- Şunu yazın: TV 'yi açın
- Beklenen yanıt: Tamam, TV 'yi açma
- Şunu yazın: 9 yarın için bir alarm ayarlayın
- Beklenen yanıt: Tamam, 9 yarın için bir alarm ayarlanıyor

> [!div class="mx-imgBorder"]
> ![Web sohbeti ile test etme](media/custom-commands/create-basic-test-chat.png)

> [!TIP]
> Test panelinde, bu ses/metin girişinin nasıl işlendiği konusunda bilgi için **ayrıntıları etkinleştir** ' i seçebilirsiniz.

## <a name="add-parameters-to-commands"></a>Komutlara parametre ekleme

Bu bölümde, komutlarınıza nasıl parametre ekleneceğini öğrenirsiniz. Parametreler, bir görevi tamamlamaya yönelik komutlar için gereken bilgiler. Karmaşık senaryolarda parametreler, özel eylemleri tetikleyen koşulları tanımlamak için de kullanılabilir.

### <a name="configure-parameters-for-turnon-command"></a>TurnOn komutu için parametreleri yapılandırma

Birden çok cihazı açmak ve kapatmak için mevcut **turnOn** komutunu düzenleyerek başlayın.

1. Artık komut hem açık hem de kapalı senaryolarla işlem işleymeyecektir, komutu **Turnonoff** olarak yeniden adlandırın.
   1. Sol bölmede, **turnOn** komutunu seçin ve bölmenin üst kısmındaki **Yeni komut** ' nin yanındaki üç nokta (...) düğmesini seçin.
   
   1. **Yeniden Adlandır**' ı seçin. Komut pencerelerini **Yeniden Adlandır** ' da **adı** **turnonoff** olarak değiştirin.

1. Ardından, bu komuta kullanıcının cihazı açmak mı yoksa kapatmak mı istediğini temsil eden yeni bir parametre eklersiniz.
   1. Orta bölmenin en üstünde bulunan  **Ekle** ' yi seçin. Açılan listeden **parametre**' ı seçin.
   1. Sağ bölmedeki **Parametreler** bölümünde, **ad** kutusuna **OnOff** olarak bir değer ekleyin.
   1. **Gerekli**' yi seçin. **Gerekli bir parametre için yanıt Ekle** penceresinde **basit düzenleyici**' yi seçin. **İlk çeşitte**, Ekle
        ```
        On or Off?
        ```
   1. **Güncelleştir**’i seçin.

       > [!div class="mx-imgBorder"]
       > ![Gerekli parametre yanıtı oluştur](media/custom-commands/add-required-on-off-parameter-response.png)
   
   1. Şimdi parametre özelliklerini yapılandıracağız. Bir komutun tüm yapılandırma özelliklerinin açıklaması için, [Başvurular](./custom-commands-references.md)' a gidin. Parametresinin özelliklerini şu şekilde yapılandırın:
      

       | Yapılandırma      | Önerilen değer     | Açıklama                                                      |
       | ------------------ | ----------------| ---------------------------------------------------------------------|
       | Ad               | `OnOff`           | Parametre için açıklayıcı bir ad                                                                           |
       | Geneldir          | unchecked       | Bu parametre için bir değerin uygulamadaki tüm komutlara genel olarak uygulanıp uygulanmadığı belirten onay kutusu|
       | Gerekli           | checked         | Komutu tamamlamadan önce Bu parametre için bir değer gerekip gerekmediğini belirten onay kutusu |
       | Gerekli parametre için yanıt      |Basit düzenleyici > `On or Off?`      | Bilinmiyorsa bu parametrenin değerini sormak için bir istem |
       | Tür               | Dize          | Parametre türü, örneğin sayı, dize, tarih saat veya Coğrafya   |
       | Yapılandırma      | Önceden tanımlanmış giriş değerlerini iç katalogdan kabul et | Dizeler için bu, girişleri olası bir değerler kümesiyle sınırlandırır |
       | Önceden tanımlanmış giriş değerleri     | `on`, `off`           | Olası değerler ve bunların diğer adları kümesi         |
       
        
   1. Önceden tanımlanmış giriş değerlerini eklemek için, **önceden tanımlanmış bir giriş Ekle** ve **Yeni öğe**  penceresinde, yukarıdaki tabloda belirtilen **adı** yazın. Bu durumda, diğer adları kullanmıyoruz, bu nedenle boş bırakabilirsiniz.
   
      > [!div class="mx-imgBorder"]
      > ![Parametre oluştur](media/custom-commands/create-on-off-parameter.png)

   1. Parametrenin tüm yapılandırmasını kaydetmek için **Kaydet** ' i seçin.
 
#### <a name="add-subjectdevice-parameter"></a>SubjectDevice parametresi Ekle 

1. Sonra, bu komut kullanılarak denetlenebilecek cihazların adını temsil eden ikinci bir parametre eklemek için yeniden **Ekle** ' yi seçin. Aşağıdaki yapılandırmayı kullanın.


    | Ayar            | Önerilen değer       |
    | ------------------ | --------------------- |
    | Name               | `SubjectDevice`         |
    | Geneldir          | unchecked             |
    | Gerekli           | checked               |
    | Gerekli parametre için yanıt     | Basit düzenleyici > `Which device do you want to control?`    | 
    | Tür               | Dize                |          |
    | Yapılandırma      | Önceden tanımlanmış giriş değerlerini iç katalogdan kabul et | 
    | Önceden tanımlanmış giriş değerleri | `tv`, `fan`               |
    | Diğer adlar ( `tv` )      | `television`, `telly`     |

1. **Kaydet**’i seçin

#### <a name="modify-example-sentences"></a>Örnek cümleleri değiştirme

Parametreleri olan komutlar için, olası tüm birleşimleri kapsayan örnek cümleler eklemek yararlıdır. Örnek:

* Tam parametre bilgileri- `turn {OnOff} the {SubjectDevice}`
* Kısmi parametre bilgileri- `turn it {OnOff}`
* Parametre bilgisi yok- `turn something`

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
> Örnek cümleler düzenleyicisinde parametrelerinizi belirtmek için küme ayraçları kullanın. - `turn {OnOff} the {SubjectDevice}` Daha önce oluşturulan parametreler tarafından desteklenen otomatik tamamlama sekmesini kullanın.

#### <a name="modify-completion-rules-to-include-parameters"></a>Tamamlama kurallarını parametreleri içerecek şekilde değiştirme

Mevcut tamamlama kuralını **ConfirmationResponse** değiştirin.

1. **Koşullar** bölümünde **Koşul Ekle**' yi seçin.
1. **Yeni koşul** penceresinde, **tür** listesinde, **gerekli parametreler**' i seçin. Aşağıdaki denetim listesinde, hem **OnOff** hem de **subjectdevice**' ı işaretleyin.
1. **Iglobali** işaretlenmemiş olarak bırakın.
1. **Oluştur**’u seçin.
1. **Eylemler** bölümünde, mevcut **konuşma yanıtı gönder** eylemini, eylemin üzerine gelerek ve Düzenle düğmesini seçerek düzenleyin. Bu kez, yeni oluşturulan **OnOff** ve **subjectdevice** parametrelerini kullanın

    ```
    Ok, turning the {SubjectDevice} {OnOff}
    ```
1. **Kaydet**’i seçin.

Sağ bölmenin üst kısmındaki **eğitme** simgesini seçerek değişiklikleri deneyin. Eğitim tamamlandığında **Test**' i seçin. Uygulama pencerenizin bir **testi** görüntülenir. Aşağıdaki etkileşimleri deneyin.

- Giriş: TV 'yi kapatma
- Çıkış: Tamam, TV 'yi kapatma
- Giriş: televizyon kapatma
- Çıkış: Tamam, TV 'yi kapatma
- Giriş: kapat
- Çıkış: hangi cihazı denetlemek istiyorsunuz?
- Giriş: TV
- Çıkış: Tamam, TV 'yi kapatma

### <a name="configure-parameters-for-settemperature-command"></a>Setsıcaklık komutu için parametreleri yapılandırma

Kullanıcı tarafından yönlendirilerek sıcaklığın ayarlanmış olduğunu etkinleştirmek için **setsıcaklık** komutunu değiştirin.

Aşağıdaki yapılandırmayla yeni parametre **sıcaklığı** ekleyin

| Yapılandırma      | Önerilen değer     |
| ------------------ | ----------------|
| Name               | `Temperature`           |
| Gerekli           | checked         |
| Gerekli parametre için yanıt      | Basit düzenleyici > `What temperature would you like?`
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
| Eylemler           | Konuşma yanıtı gönder > `Ok, setting temperature to {Temperature} degrees` |

### <a name="configure-parameters-to-the-setalarm-command"></a>Parametreleri SetAlarm komutuna göre yapılandırma

Aşağıdaki yapılandırmaya sahip **DateTime** adlı bir parametre ekleyin.

   | Ayar                           | Önerilen değer                     | 
   | --------------------------------- | ----------------------------------------|
   | Name                              | `DateTime`                               |
   | Gerekli                          | checked                                 |
   | Gerekli parametre için yanıt   | Basit düzenleyici > `For what time?`            | 
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
   | Eylemler    | Konuşma yanıtı gönder- `Ok, alarm set for {DateTime}`  |

Farklı komutlarla ilgili olan söyleylerini kullanarak üç komutu birlikte test edin. Farklı komutlar arasında geçiş gerçekleştirebileceğinizi unutmayın.

- Giriş: bir alarm ayarlama
- Çıkış: hangi süre?
- Giriş: TV 'yi açın
- Çıkış: Tamam, TV 'yi açma
- Giriş: bir alarm ayarlama
- Çıkış: hangi süre?
- Giriş: 17:00
- Çıkış: Tamam, 2020-05-01 17:00:00 için alarm kümesi

## <a name="add-configurations-to-commands-parameters"></a>Komut parametrelerine yapılandırma ekleme

Bu bölümde, aşağıdakiler dahil olmak üzere gelişmiş parametre yapılandırması hakkında daha fazla bilgi edinebilirsiniz:

 - Parametre değerleri dışarıdan özel komutlar uygulamasına tanımlanmış bir kümesine ait olabilir
 - Parametrelerin değerine doğrulama yan tümceleri ekleme

### <a name="configure-parameter-as-external-catalog-entity"></a>Parametreyi dış Katalog varlığı olarak yapılandırma

Özel komutlar, bir Web uç noktası üzerinden barındırılan dış kataloglara başvurmak için dize türü parametrelerini yapılandırmanıza olanak tanır. Bu, özel komutlar uygulamasında düzenleme yapılmadan dış kataloğu bağımsız olarak güncelleştirmenizi sağlar. Bu yaklaşım, Katalog girişlerinin numara içinde büyük olabilecek durumlarda faydalıdır.

**Türolmayan** komuttan **subjectdevice** parametresini yeniden kullanın. Bu parametrenin geçerli yapılandırması, **iç katalogdan önceden tanımlanmış girdileri kabul** ediyor. Bu, parametre yapılandırmasında tanımlanan statik cihaz listesini ifade eder. Bu içeriği bağımsız olarak güncelleştirilebilen bir dış veri kaynağına taşımak istiyoruz.

Bunu yapmak için yeni bir Web uç noktası ekleyerek başlayın. Sol bölmedeki **Web uç noktaları** bölümüne gidin ve aşağıdaki yapılandırmaya sahip yeni bir Web uç noktası ekleyin.

| Ayar | Önerilen değer |
|----|----|
| Name | `getDevices` |
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

**Eğitme** ' i seçerek deneyin ve eğitim tamamlanmasını bekleyin. Eğitim tamamlandığında, **Test** ' i seçin ve birkaç etkileşimi deneyin.

* Giriş: aç
* Çıkış: hangi cihazı denetlemek istiyorsunuz?
* Giriş: ışıklar
* Çıkış: Tamam, ışıkları açma

> [!NOTE]
> Web uç noktasında barındırılan tüm cihazları şimdi nasıl denetleyebildiğinize dikkat edin. Yine de uygulamayı yeni değişiklikleri test etmek ve uygulamayı yeniden yayımlamak için eğitmeniz gerekir.

### <a name="add-validation-to-parameters"></a>Parametrelere doğrulama ekleme

**Doğrulamalar** , parametre değerindeki kısıtlamaları yapılandırmanıza olanak tanıyan belirli parametre türleri için geçerli olan yapılardır ve değerler kısıtlamaların içine düşmediğinde düzeltme isteminde bulunur. Doğrulama yapısını genişleten parametre türlerinin tam listesi için, [Başvurular](./custom-commands-references.md) ' a gidin.

**Setsıcaklık** komutunu kullanarak doğrulamaları test edin. **Sıcaklık** parametresi için bir doğrulama eklemek için aşağıdaki adımları kullanın.

1. Sol bölmedeki **Setsıcaklık** komutunu seçin.
1. Orta bölmedeki  **sıcaklık** ' ı seçin.
1. Sağ bölmede **Doğrulama Ekle** ' yi seçin.
1. **Yeni doğrulama** penceresinde doğrulamayı aşağıdaki şekilde yapılandırın ve **Oluştur**' u seçin.


    | Parametre yapılandırması | Önerilen değer | Açıklama |
    | ---- | ---- | ---- |
    | Minimum Değer | `60` | Sayı parametreleri için bu parametrenin varsaydığı en küçük değer |
    | Maksimum Değer | `80` | Sayı parametreleri için bu parametrenin varsaydığı en büyük değer |
    | Hata yanıtı |  Ilk varyasyon > basit düzenleyici > `Sorry, I can only set temperature between 60 and 80 degrees. What temperature do you want?` | Doğrulama başarısız olursa yeni bir değer sormak isteyip istemediğinizi sor |

    > [!div class="mx-imgBorder"]
    > ![Aralık doğrulaması ekle](media/custom-commands/add-validations-temperature.png)

Sağ bölmenin en üstünde bulunan **eğitme** simgesini seçerek deneyin. Eğitim tamamlandığında, **Test** ' i seçin ve birkaç etkileşimi deneyin:

- Giriş: sıcaklığı 72 derece olarak ayarlayın
- Çıkış: Tamam, sıcaklık 72 derece olarak ayarlanıyor
- Giriş: sıcaklığı 45 derece olarak ayarlayın
- Çıkış: Üzgünüz, yalnızca 60 ve 80 derece arasında sıcaklık ayarlayabiliyorum
- Giriş: bunun yerine 72 derece yapın
- Çıkış: Tamam, sıcaklık 72 derece olarak ayarlanıyor

## <a name="add-interaction-rules"></a>Etkileşim kuralları ekleme

Etkileşim kuralları, belirli veya karmaşık durumları işlemek için *ek kurallardır* . Kendi özel etkileşim kurallarınızı yazmak için ücretsiz olduğunuzda, bu örnekte aşağıdaki hedeflenen senaryolar için etkileşim kurallarını kullanabilirsiniz:

* Onaylama komutları
* Komutlara tek adımlı bir düzeltme ekleme

Etkileşim kuralları hakkında daha fazla bilgi edinmek için [Başvurular](./custom-commands-references.md) bölümüne gidin.

### <a name="add-confirmations-to-a-command"></a>Komuta onaylar ekleme

Bir onay eklemek için **Setsıcaklık** komutunu kullanın. Onay elde etmek için aşağıdaki adımları kullanarak etkileşim kuralları oluşturursunuz.

1. Sol bölmedeki **Setsıcaklık** komutunu seçin.
1. Orta bölmede **Ekle** ' ye tıklayarak etkileşim kuralları ekleyin. Ardından **etkileşim kuralları**  >  **Onayla komutunu** seçin.

    Bu eylem, kullanıcıdan alarm tarih ve saatini onaylamasını ve bir sonraki sırayla bir onay (Evet/Hayır) beklediğini isteyecektir.

    1. Aşağıdaki yapılandırmaya göre **Onayla komut** etkileşim kuralını değiştirin:
        1. **Sıcaklığı onaylamak** için **adı** yeniden adlandırın.
        1. **Gerekli parametre** sıcaklığı olarak yeni bir koşul ekleyin  >  .
        1. Yeni bir eylem ekleyin **tür**  >  **konuşma yanıtı gönder**  >  **sıcaklığı {sıcaklık} derece olarak ayarlamak istediğinizden emin misiniz?**
        1. **Beklentiler** bölümünde **kullanıcıdan onay bekleyen** varsayılan değeri bırakın.
      
         > [!div class="mx-imgBorder"]
         > ![Gerekli parametre yanıtı oluştur](media/custom-speech-commands/add-validation-set-temperature.png)
    

    1. **Onaylama** başarılı etkileşim kuralını, başarılı bir onaylamayı işleyecek şekilde değiştirin (Kullanıcı Evet olarak kabul edilir).
      
          1. **Adı** **onay sıcaklığı olarak değiştirme başarılı oldu**.
          1. Zaten var olan **onayın başarılı olduğunu** bırakın.
          1.   >  **Gerekli parametre**  >  **sıcaklığını** tür olarak yeni bir koşul ekleyin.
          1. **Yürütme sonrası durumunun** varsayılan değerini **yürütme kuralları yürütme** olarak bırakın.

    1. **Onay reddedildi etkileşim kuralını** , onay reddedildiğinde senaryoları işleyecek şekilde değiştirin (Kullanıcı Hayır olarak kabul edilir).

          1. **Adı** **onay sıcaklığı** olarak değiştirme reddedildi.
          1. Zaten mevcut **onay reddedildi** koşulunu bırakın.
          1.   >  **Gerekli parametre**  >  **sıcaklığını** tür olarak yeni bir koşul ekleyin.
          1. Yeni bir eylem ekleyin   >  **konuşma yanıtı gönder**  >  **sorun yok. Ne daha sonra sıcaklık?**
          1. **Yürütme sonrası durumunun** varsayılan değerini **kullanıcının girişini bekle** olarak bırakın.

> [!IMPORTANT]
> Bu makalede, yerleşik onay özelliğini kullandınız. Etkileşim kurallarını bir tane de el ile ekleyebilirsiniz.
   
**Eğitme**' i seçerek değişiklikleri deneyin, eğitimin bitmesini bekleyin ve **Test**' i seçin.

- **Giriş**: sıcaklığı 80 derece olarak ayarlayın
- **Çıkış**: sıcaklığı 80 derece olarak ayarlamak istediğinizden emin misiniz?
- **Giriş**: Hayır
- **Çıkış**: sorun değil. Ne daha sonra sıcaklık?
- **Giriş**: 72 derece
- **Çıkış**: sıcaklığı 72 derece olarak ayarlamak istediğinizden emin misiniz?
- **Giriş**: Evet
- **Çıkış**: Tamam, sıcaklık 83 derece olarak ayarlanıyor

### <a name="implement-corrections-in-a-command"></a>Bir komutta düzeltmeleri uygulama

Bu bölümde, tek adımlı bir düzeltme yapılandırırsınız ve bu işlem, tamamlama eylemi daha önce yürütüldükten sonra kullanılır. Ayrıca, komutun henüz karşılanmamış olması durumunda bir düzeltmenin varsayılan olarak nasıl etkin olduğunu gösteren bir örnek görürsünüz. Komut tamamlandığında bir düzeltme eklemek için, yeni **Alarmtone** parametresini ekleyin.

Sol bölmeden **SetAlarm** komutunu seçin ve yeni **alarmtone** parametresini ekleyin.
        
- **Ad**  >  **Alarmton**
- **Tür**  >  **Dize**
- **Varsayılan değer**  >  **Oymes**
- **Yapılandırma**  >  **Önceden tanımlanmış giriş değerlerini iç katalogdan kabul etme**
- **Önceden tanımlanmış giriş değerleri**  >  Tek tek önceden tanımlanmış girişler olarak **oyırmes**, **jne** ve **yankı**


Sonra, **DateTime** parametresinin yanıtını, **alarmtone} olarak sesle ayarlamaya başlamaya hazırlanın. Ne zaman?** Sonra tamamlama kuralını aşağıdaki gibi değiştirin:

1. Mevcut tamamlama kuralını **ConfirmationResponse** seçin.
1. Sağ bölmede, var olan eylemin üzerine gelin ve **Düzenle**' yi seçin.
1. Konuşma yanıtını **Tamam, uyarı kümesi {DateTime} için güncelleştirin. Uyarı tonu {AlarmTone}.**

> [!IMPORTANT]
> Uyarı tonu devam eden bir komutta herhangi bir açık yapılandırma olmadan değiştirilebilir, örneğin, komut henüz tamamlanmamıştır. *Komut için bir düzeltme varsayılan olarak, komutun yerine getirilme numarası ne olursa olsun, tüm komut parametreleri için etkindir.*

#### <a name="correction-when-command-is-completed"></a>Komut tamamlandığında Düzeltme

Özel komutlar platformu, komut tamamlandığında bile tek adımlı bir düzeltme için özelliği de sağlar. Bu özellik varsayılan olarak etkin değildir. Açıkça yapılandırılması gerekir. Tek adımlı bir düzeltme yapılandırmak için aşağıdaki adımları kullanın.

1. **SetAlarm** komutunda, daha önce set alarmını güncelleştirmek Için **Öncekini Güncelleştir komutunu** yazın ' ın bir etkileşim kuralını ekleyin. **Önceki alarmı güncelleştirmek** için etkileşim kuralının varsayılan **adını** yeniden adlandırın.
1. Önceki komutun olduğu gibi **güncelleştirilmesi için gereken** varsayılan koşulu bırakın.
1.   >  **Gerekli parametre DateTime parametresi** olarak yeni bir koşul ekleyin  >  .
1. Türe yeni bir eylem ekleyin   >  **konuşma yanıtı gönder**  >  **basit düzenleyici**  >  **önceki Alarm süresini {DateTime} olarak güncelleştiriyor.**
1. **Yürütme sonrası durumunun** varsayılan değerini **komut tamamlandı** olarak bırakın.

**Eğitme**' i seçerek değişiklikleri deneyin, eğitimin bitmesini bekleyin ve **Test**' i seçin.

- **Giriş**: bir alarm ayarlayın.
- **Çıkış**: oyda oylarla alarm ayarlamaya hazırlanıyor. Ne zaman?
- **Giriş**: 9 yarın için jsıas ile bir alarm ayarlayın.
- **Çıkış**: tamam, 2020-05-21 09:00:00 için alarm kümesi. Uyarı tonu, Jbir.
- **Giriş**: Hayır, 8 har.
- **Çıkış**: önceki alarm süresini 2020-05-29 08:00 olarak güncelleştirme.

> [!NOTE]
> Gerçek bir uygulamada, bu düzeltme kuralının **Eylemler** bölümünde, bir etkinliği istemciye geri göndermeniz veya sisteminizdeki alarm saatini güncelleştirmek IÇIN bir HTTP uç noktası çağırmanız gerekir. Bu eylem yalnızca Alarm süresini güncelleştirmekten ve komutun başka bir özniteliğini içermemelidir. Bu durumda, uyarı tonu olacaktır.

## <a name="add-language-generation-templates-for-speech-responses"></a>Konuşma yanıtları için dil oluşturma şablonları ekleme

Dil oluşturma şablonları, istemciye gönderilen yanıtları özelleştirmenize olanak tanır ve yanıtlara varyansı ortaya çıkarabilir. Dil oluşturma özelleştirmesi şu şekilde sağlanabilir:

* Dil oluşturma şablonlarının kullanımı
* Uyarlamalı ifadelerin kullanımı

Özel komutlar şablonları, BotFramework 'in [LG şablonlarını](/azure/bot-service/file-format/bot-builder-lg-file-format#templates)temel alır. Özel komutlar gerektiğinde yeni bir LG şablonu oluşturduğundan (yani, parametrelerde veya eylemlerdeki konuşma yanıtları için) LG şablonunun adını belirtmeniz gerekmez. Bu nedenle, şablonunuzu şöyle tanımlamak yerine şunları yapın:

 ```
    # CompletionAction
    - Ok, turning {OnOff} the {SubjectDevice}
    - Done, turning {OnOff} the {SubjectDevice}
    - Proceeding to turn {OnOff} {SubjectDevice}
 ```

Yalnızca adı olmayan şablonun gövdesini aşağıdaki gibi tanımlamanız gerekir.

> [!div class="mx-imgBorder"]
> ![Şablon Düzenleyicisi örneği](./media/custom-commands/template-editor-example.png)


Bu değişiklik, istemciye gönderilen konuşma yanıtlarının çeşitlemesini tanıtır. Bu nedenle, aynı utterlik için ilgili konuşma yanıtı, belirtilen seçeneklerin dışında rastgele kullanıma sunulur.

LG şablonlarından yararlanmak, uyarlamalı ifadeler kullanan komutlar için karmaşık konuşma yanıtları tanımlamanızı da sağlar. Daha fazla ayrıntı için [LG şablonları biçimine](/azure/bot-service/file-format/bot-builder-lg-file-format#templates) bakabilirsiniz. Varsayılan olarak özel komutlar, aşağıdaki küçük farklılıklar ile tüm özellikleri destekler:

* LG şablonları varlıklarda $ {entityName} olarak temsil edilir. Özel komutlarda varlıkları kullanmıyoruz, ancak parametreler $ {parameterName} veya {parameterName} gösterimlerden biriyle değişken olarak kullanılabilir
* Şablon kompozisyonu ve genişletmesi özel komutlarda desteklenmez. Bunun nedeni, dosyayı doğrudan düzenlememelisiniz `.lg` , ancak yalnızca otomatik olarak oluşturulan şablonların yanıtları.
* LG tarafından eklenen özel işlevler özel komutlarda desteklenmez. Önceden tanımlanmış işlevler hala desteklenmektedir.
* Seçenekler (Strict, yeniden Değiştir & lineBreakStyle) özel komutlarda desteklenmez.

### <a name="add-template-responses-to-turnonoff-command"></a>TurnOnOff komutuna şablon yanıtları ekleme

Şu yapılandırmayla yeni bir parametre eklemek için **Turnonoff** komutunu değiştirin:

| Ayar            | Önerilen değer       | 
| ------------------ | --------------------- | 
| Name               | `SubjectContext`         | 
| Geneldir          | unchecked             | 
| Gerekli           | unchecked               | 
| Tür               | Dize                |
| Varsayılan değer      | `all` |
| Yapılandırma      | Önceden tanımlanmış giriş değerlerini iç katalogdan kabul et | 
| Önceden tanımlanmış giriş değerleri | `room`, `bathroom`, `all`|

#### <a name="modify-completion-rule"></a>Tamamlanma kuralını Değiştir

Mevcut tamamlama kuralı **ConfirmationResponse**' nın **Eylemler** bölümünü düzenleyin. **Eylem düzenleme** açılır penceresinde, **Şablon Düzenleyicisi** ' ne geçin ve metni aşağıdaki örnekle değiştirin.

```
- IF: @{SubjectContext == "all" && SubjectDevice == "lights"}
    - Ok, turning all the lights {OnOff}
- ELSEIF: @{SubjectDevice == "lights"}
    - Ok, turning {OnOff} the {SubjectContext} {SubjectDevice}
- ELSE:
    - Ok, turning the {SubjectDevice} {OnOff}
    - Done, turning {OnOff} the {SubjectDevice}
```

Uygulamanızı aşağıdaki şekilde **eğitme** ve **Test** etme. Şablon değerinin birden çok alternatifine ve ayrıca Uyarlamalı ifadelerin kullanımına bağlı olarak, yanıtların çeşitlemesine dikkat edin.

* Giriş: TV 'yi açın
* Çıkış: Tamam, TV 'yi açma
* Giriş: TV 'yi açın
* Çıkış: bitti, TV açık
* Giriş: ışıkları kapatma
* Çıkış: Tamam, tüm ışıkları kapatma
* Giriş: Oda ışıklarını kapatma
* Çıkış: Tamam, Oda ışıklarını kapatma

## <a name="use-custom-voice"></a>Özel Ses kullanma

Özel komutlar yanıtlarını özelleştirmenin başka bir yolu da özel bir çıkış sesi seçkullanmaktır. Varsayılan sesi özel bir sese geçirmek için aşağıdaki adımları kullanın.

1. Özel komutlar uygulamanızda, sol bölmeden **Ayarlar** ' ı seçin.
1. Orta bölmeden **özel ses** ' i seçin.
1. Tablodan istenen özel veya genel sesi seçin.
1. **Kaydet**’i seçin.

> [!div class="mx-imgBorder"]
> ![Parametrelere sahip örnek cümleler](media/custom-commands/select-custom-voice.png)

> [!NOTE]
> - **Genel sesler** için **sinir türleri** yalnızca belirli bölgelerde kullanılabilir. Kullanılabilirliği denetlemek için [bölgeye/uç noktaya göre standart ve sinir seslikleri](./regions.md#standard-and-neural-voices)bölümüne bakın.
> - **Özel sesler** Için özel ses projesi sayfasından oluşturulabilirler. Bkz. [özel sesle çalışmaya başlama](./how-to-custom-voice.md).

Artık uygulama, varsayılan ses yerine seçili sesle yanıt verir.

## <a name="next-steps"></a>Sonraki adımlar

* Konuşma SDK 'sını kullanarak [özel komutlar uygulamanızı](how-to-custom-commands-setup-speech-sdk.md) bir istemci uygulamasıyla tümleştirmeyi öğrenin.
* Azure DevOps ile özel komutlar uygulamanız için [sürekli dağıtım ayarlayın](how-to-custom-commands-deploy-cicd.md) . 
                      