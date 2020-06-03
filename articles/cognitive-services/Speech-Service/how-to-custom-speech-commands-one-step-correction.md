---
title: Özel komutlar önizleme-konuşma hizmeti 'nde tek adımlı düzeltme ekleme
titleSuffix: Azure Cognitive Services
description: Özel komutlar önizleme uygulamasında bir komut için tek adımlı bir düzeltme eklemeyi öğrenin.
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: encorona
ms.openlocfilehash: 05f63ba4e70f649df33905f1e92fb1fab866a86c
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310436"
---
# <a name="add-a-one-step-correction-to-a-custom-command-in-a-custom-commands-preview-application"></a>Özel bir komut önizleme uygulamasında özel komuta tek adımlı bir düzeltme ekleme

Bu makalede, özel komutlar önizleme uygulamasında bir komuta tek adımlı onay eklemeyi öğreneceksiniz.

Tek adımlı düzeltme, az önce tamamlanmış olan bir komutu güncelleştirmek için kullanılır. Bir alarm komutuna tek adımlı bir düzeltme eklediğinizde fikrinizi değiştirebilir ve alarmın saatini güncelleştirebilirsiniz. Örneğin:

- Giriş: yarın, öğleden sonra alarmını ayarla
- Çıkış: Tamam, 2020-05-02 12:00:00 için alarm kümesi
- Giriş: Hayır, yarın saat 1pm
- Çıkış: Tamam

> [!NOTE]
> Gerçek dünyada bir senaryoda istemci, komut tamamlama işleminin sonucu olarak bir eylem yürütür. Bu makalede, arka uç uygulamanızda alarmını güncelleştirmek için bir mekanizmanın olduğunu varsaymaktadır.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki makalelerdeki adımları uygulayın:
> [!div class="checklist"]

> * [Hızlı başlangıç: özel komutlar önizleme uygulaması oluşturma](./quickstart-custom-speech-commands-create-new.md)
> * [Hızlı başlangıç: parametrelerle özel bir komut önizleme uygulaması oluşturma](./quickstart-custom-speech-commands-create-parameters.md)
> * [Nasıl yapılır: özel komutlar önizleme uygulamasında bir komuta onaylar ekleme](./how-to-custom-speech-commands-confirmations.md)

## <a name="add-interaction-rules-for-one-step-correction"></a>Tek adımlı düzeltme için etkileşim kuralları ekleme

Tek adımlı bir düzeltmeyi göstermek için, [nasıl yapılır: özel komutlar önizlemede bir komuta onay ekleme](./how-to-custom-speech-commands-confirmations.md)bölümünde oluşturduğunuz **SetAlarm** komutunu genişletin.

1. **SetAlarm** komutunuz güncelleştirmek için bir etkileşim kuralı ekleyin.

    Bu kural, kullanıcıdan alarm tarihini ve saatini onaylamasını ve bir sonraki sırayla bir onay (Evet/Hayır) beklediğini ister.

   | Ayar               | Önerilen değer                                                  | Description                                        |
   | --------------------- | ---------------------------------------------------------------- | -------------------------------------------------- |
   | **Kural Adı**             | **Önceki alarmı Güncelleştir**                                            | Kuralın amacını açıklayan bir ad          |
   | **Koşullar**            | **Önceki komutun gerekli parametre > DateTime & güncelleştirilmesi gerekiyor**                | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |   
   | **Eylemler**               | **Konuşma yanıtı gönder-> basit düzenleyici-> önceki alarm {DateTime} öğesine güncelleştiriliyor**      | Kural koşulları doğru olduğunda gerçekleştirilecek eylem |
   | **Yürütme sonrası durumu** | **Komut tamamlandı**        | Açıldıktan sonra kullanıcının durumu                   |

1. Bölmesinde, az önce oluşturduğunuz etkileşim kuralını seçin. Bölmenin sol üst köşesindeki üç nokta (**...**) düğmesini seçin. Ardından, kuralı **etkileşim kuralları** listesinin en üstüne taşımak Için **Yukarı taşı** okunu kullanın.
   > [!div class="mx-imgBorder"]
   > ![Aralık doğrulaması ekle](media/custom-speech-commands/one-step-correction-rules.png)

    > [!NOTE]
    > Gerçek dünyada bir uygulamada, bir etkinliği istemciye geri göndermek veya sisteminizdeki alarmını güncelleştirmek için bir HTTP uç noktası çağırmak için **Eylemler** bölümünü kullanın.

## <a name="try-it-out"></a>Deneyin

1. **Eğitim**' i seçin.

1. Eğitim yapıldıktan sonra **Test**' i seçin ve ardından bu etkileşimleri deneyin:

   - Giriş: yarın, öğleden sonra alarmını ayarla
   - Çıkış: 2020-05-02 12:00:00 için bir alarm ayarlamak istediğinizden emin misiniz?
   - Giriş: Evet
   - Çıkış: Tamam, 2020-05-02 12:00:00 için alarm kümesi
   - Giriş: Hayır, yarın saat 2PM
   - Çıkış: önceki alarm 2020-05-02 14:00:00 ' ye güncelleştiriliyor
