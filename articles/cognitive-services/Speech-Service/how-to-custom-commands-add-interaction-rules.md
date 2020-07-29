---
title: 'Nasıl yapılır: özel komuta onay ekleme'
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel komutlarda bir komut için onayların nasıl uygulanacağını öğreneceksiniz.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: sausin
ms.openlocfilehash: 7d6c0928196c9e8e1abf6aa7f724a58753ce3d2a
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87289045"
---
# <a name="add-interaction-rules"></a>Etkileşim kuralları ekleme

Bu makalede, **etkileşim kuralları**hakkında bilgi edineceksiniz. Bunlar, daha belirgin veya karmaşık durumları işlemek için ek kurallardır. Kendi özel etkileşim kurallarınızı yazmak için ücretsiz olduğunuzda, bu makalede aşağıdaki hedeflenen senaryolar için etkileşim kurallarından yararlanabilirsiniz:

* Onaylama komutları
* Komutlara tek adımlı bir düzeltme ekleme

Etkileşim kuralları hakkında daha fazla bilgi edinmek için [Başvurular](./custom-commands-references.md) bölümüne gidin.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki makalelerde bulunan adımları tamamlamalısınız:
> [!div class="checklist"]
> * [Nasıl yapılır: basit komutlarla uygulama oluşturma](./how-to-custom-commands-create-application-with-simple-commands.md)
> * [Nasıl yapılır: komutlara parametreler ekleme](./how-to-custom-commands-add-parameters-to-commands.md)

## <a name="add-confirmations-to-a-command"></a>Komuta onaylar ekleme

Bir onay eklemek için **Setsıcaklık** komutunu kullanın. Onay elde etmek için aşağıdaki adımları kullanarak etkileşim kuralları oluşturursunuz.

1. Sol bölmedeki **Setsıcaklık** komutunu seçin.
1. Orta bölmede **Ekle** ' ye tıklayarak etkileşim kuralları ekleyin. Ardından **etkileşim kuralları**  >  **Onayla komutunu**seçin.

    Bu eylem, kullanıcıdan alarm tarih ve saatini onaylamasını ve bir sonraki sırayla bir onay (Evet/Hayır) beklediğini isteyecektir.

    1. Aşağıdaki yapılandırmaya göre **Onayla komut** etkileşim kuralını değiştirin:
        1. **Sıcaklığı onaylamak**için **adı** yeniden adlandırın.
        1. **Gerekli parametre**sıcaklığı olarak yeni bir koşul ekleyin  >  **Temperature**.
        1. Yeni bir eylem ekleyin **tür**  >  **konuşma yanıtı gönder**  >  **sıcaklığı {sıcaklık} derece olarak ayarlamak istediğinizden emin misiniz?**
        1. **Beklentiler** bölümünde **kullanıcıdan onay bekleyen** varsayılan değeri bırakın.
      
         > [!div class="mx-imgBorder"]
         > ![Gerekli parametre yanıtı oluştur](media/custom-speech-commands/add-validation-set-temperature.png)
    

    1. **Onaylama** başarılı etkileşim kuralını, başarılı bir onaylamayı işleyecek şekilde değiştirin (Kullanıcı Evet olarak kabul edilir).
      
          1. **Adı** **onay sıcaklığı olarak değiştirme başarılı oldu**.
          1. Zaten var olan **onayın başarılı olduğunu** bırakın.
          1. **Type**  >  **Gerekli parametre**  >  **sıcaklığını**tür olarak yeni bir koşul ekleyin.
          1. **Yürütme sonrası durumunun** varsayılan değerini **yürütme kuralları yürütme**olarak bırakın.

    1. **Onay reddedildi etkileşim kuralını** , onay reddedildiğinde senaryoları işleyecek şekilde değiştirin (Kullanıcı Hayır olarak kabul edilir).

          1. **Adı** **onay sıcaklığı**olarak değiştirme reddedildi.
          1. Zaten mevcut **onay reddedildi** koşulunu bırakın.
          1. **Type**  >  **Gerekli parametre**  >  **sıcaklığını**tür olarak yeni bir koşul ekleyin.
          1. Yeni bir eylem ekleyin **Type**  >  **konuşma yanıtı gönder**  >  **sorun yok. Ne daha sonra sıcaklık?**
          1. **Yürütme sonrası durumunun** varsayılan değerini **kullanıcının girişini bekle**olarak bırakın.

> [!IMPORTANT]
> Bu makalede, yerleşik onay özelliğini kullandınız. Etkileşim kurallarını bir tane de el ile ekleyebilirsiniz.
   

### <a name="try-out-the-changes"></a>Değişiklikleri deneyin

Eğitim **' i seçin,** eğitimin bitmesini bekleyin ve **Test**' i seçin.

- **Giriş**: sıcaklığı 80 derece olarak ayarlayın
- **Çıkış**: sıcaklığı 80 derece olarak ayarlamak istediğinizden emin misiniz?
- **Giriş**: Hayır
- **Çıkış**: sorun değil. Ne daha sonra sıcaklık?
- **Giriş**: 72 derece
- **Çıkış**: sıcaklığı 72 derece olarak ayarlamak istediğinizden emin misiniz?
- **Giriş**: Evet
- **Çıkış**: Tamam, sıcaklık 83 derece olarak ayarlanıyor


## <a name="implement-corrections-in-a-command"></a>Bir komutta düzeltmeleri uygulama

Bu bölümde, tek adımlı bir düzeltme yapılandırırsınız ve bu işlem, tamamlama eylemi daha önce yürütüldükten sonra kullanılır. Ayrıca, komutun henüz karşılanmamış olması durumunda bir düzeltmenin varsayılan olarak nasıl etkin olduğunu gösteren bir örnek görürsünüz. Komut tamamlandığında bir düzeltme eklemek için, yeni **Alarmtone**parametresini ekleyin.

Sol bölmeden **SetAlarm** komutunu seçin ve yeni **alarmtone**parametresini ekleyin.
        
- **Ad**  >  **Alarmton**
- **Tür**  >  **Dize**
- **Varsayılan değer**  >  **Oymes**
- **Yapılandırma**  >  **Önceden tanımlanmış giriş değerlerini iç katalogdan kabul etme**
- **Önceden tanımlanmış giriş değerleri**  >  Tek tek önceden tanımlanmış girişler olarak **oyırmes**, **jne**ve **yankı**


Sonra, **DateTime** parametresinin yanıtını, **alarmtone} olarak sesle ayarlamaya başlamaya hazırlanın. Ne zaman?** Sonra tamamlama kuralını aşağıdaki gibi değiştirin:

1. Mevcut tamamlama kuralını **ConfirmationResponse**seçin.
1. Sağ bölmede, var olan eylemin üzerine gelin ve **Düzenle**' yi seçin.
1. Konuşma yanıtını **Tamam, uyarı kümesi {DateTime} için güncelleştirin. Uyarı tonu {AlarmTone}.**

### <a name="try-out-the-changes"></a>Değişiklikleri deneyin

Eğitim **' i seçin,** eğitimin bitmesini bekleyin ve **Test**' i seçin.
Aşağıdaki detersliği deneyin:

- **Giriş**: bir alarm ayarlayın.
- **Çıkış**: oyda oylarla alarm ayarlamaya hazırlanıyor. Ne zaman?
- **Giriş**: 9 yarın için jsıas ile bir alarm ayarlayın.
- **Çıkış**: tamam, 2020-05-30 09:00:00 için alarm kümesi. Uyarı tonu, Jbir.

> [!IMPORTANT]
> Uyarı tonu devam eden bir komutta herhangi bir açık yapılandırma olmadan değiştirilebilir, örneğin, komut henüz tamamlanmamıştır. *Komut için bir düzeltme varsayılan olarak, komutun yerine getirilme numarası ne olursa olsun, tüm komut parametreleri için etkindir.*

### <a name="correction-when-command-is-completed"></a>Komut tamamlandığında Düzeltme

Özel komutlar platformu, komut tamamlandığında bile tek adımlı bir düzeltme için özelliği de sağlar. Bu özellik varsayılan olarak etkin değildir. Açıkça yapılandırılması gerekir. Tek adımlı bir düzeltme yapılandırmak için aşağıdaki adımları kullanın.

1. **SetAlarm** komutunda, daha önce set alarmını güncelleştirmek Için **Öncekini Güncelleştir komutunu** yazın ' ın bir etkileşim kuralını ekleyin. **Önceki alarmı güncelleştirmek**için etkileşim kuralının varsayılan **adını** yeniden adlandırın.
1. Önceki komutun olduğu gibi **güncelleştirilmesi için gereken** varsayılan koşulu bırakın.
1. **Type**  >  **Gerekli parametre DateTime parametresi**olarak yeni bir koşul ekleyin  >  **DateTime**.
1. Türe yeni bir eylem ekleyin **Type**  >  **konuşma yanıtı gönder**  >  **basit düzenleyici**  >  **önceki Alarm süresini {DateTime} olarak güncelleştiriyor.**
1. **Yürütme sonrası durumunun** varsayılan değerini **komut tamamlandı**olarak bırakın.

### <a name="try-out-the-changes"></a>Değişiklikleri deneyin

Eğitim **' i seçin,** eğitimin bitmesini bekleyin ve **Test**' i seçin.

- **Giriş**: bir alarm ayarlayın.
- **Çıkış**: oyda oylarla alarm ayarlamaya hazırlanıyor. Ne zaman?
- **Giriş**: 9 yarın için jsıas ile bir alarm ayarlayın.
- **Çıkış**: tamam, 2020-05-21 09:00:00 için alarm kümesi. Uyarı tonu, Jbir.
- **Giriş**: Hayır, 8 har.
- **Çıkış**: önceki alarm süresini 2020-05-29 08:00 olarak güncelleştirme.

> [!NOTE]
> Gerçek bir uygulamada, bu düzeltme kuralının **Eylemler** bölümünde, bir etkinliği istemciye geri göndermeniz veya sisteminizdeki alarm saatini güncelleştirmek IÇIN bir HTTP uç noktası çağırmanız gerekir. Bu eylem yalnızca Alarm süresini güncelleştirmekten ve komutun başka bir özniteliğini içermemelidir. Bu durumda, uyarı tonu olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Nasıl yapılır: konuşma yanıtları için dil oluşturma şablonları ekleme](./how-to-custom-commands-add-language-generation-templates.md)
