---
title: 'Nasıl yapılır: özel komuta (Önizleme) yönelik tek adımlı bir düzeltme ekleme-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel komutlarda bir komut için tek adımlı düzeltmelerin nasıl uygulanacağını açıkladık.
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: encorona
ms.openlocfilehash: f43c28d314cb8a0211496664cd20d2c380e4d5b0
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858255"
---
# <a name="how-to-add-a-one-step-correction-to-a-custom-command-preview"></a>Nasıl yapılır: özel komuta tek adımlı bir düzeltme ekleme (Önizleme)

Bu makalede, bir komuta nasıl tek adımlı onay ekleneceğini öğreneceksiniz.

Tek adımlı düzeltme, az önce tamamlanmış olan bir komutu güncelleştirmek için kullanılır. Diğer bir deyişle, yalnızca bir alarm ayarlarsanız, fikrinizi değiştirebilir ve alarmın saatini güncelleştirebilirsiniz. Böyle bir örnek aşağıdaki gibidir.

- Giriş: yarın, öğleden sonra alarmını ayarla
- Çıkış: Tamam, 2020-05-02 12:00:00 için alarm kümesi
- Giriş: Hayır, yarın saat 1pm
- Çıkış: Tamam

Genellikle istemci tarafından komut tamamlanmasının sonucu olarak bir eylem yürüten gerçek bir dünya senaryosu. Bu makalede, bir geliştiricinin arka uç uygulamanızda alarm güncelleştirme mekanizması olduğunu varsaymaktadır.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki makalelerde bulunan adımları tamamlamış olmanız gerekir:
> [!div class="checklist"]

> * [Hızlı başlangıç: özel komut oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-new.md)
> * [Hızlı başlangıç: parametrelerle özel komut oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-parameters.md)
> * [Nasıl yapılır: özel komuta onay ekleme (Önizleme)](./how-to-custom-speech-commands-confirmations.md)


## <a name="add-interaction-rules-for-one-step-correction"></a>Tek adımlı düzeltme için etkileşim kuralları ekleme 

Tek adımlı bir düzeltmeyi göstermek için, [nasıl yapılır: özel komuta onay ekleme (Önizleme)](./how-to-custom-speech-commands-confirmations.md)Içinde oluşturulan **SetAlarm** komutunu genişletelim.
1. Daha önce ayarlanan alarmını güncelleştirmek için bir etkileşim kuralı ekleyin. 

    Bu kural, kullanıcıdan alarm tarihini ve saatini onaylamasını ve bir sonraki sırayla bir onay (Evet/Hayır) bekliyor olduğunu sorar.

   | Ayar               | Önerilen değer                                                  | Açıklama                                        |
   | --------------------- | ---------------------------------------------------------------- | -------------------------------------------------- |
   | Kural Adı             | Önceki alarmı Güncelleştir                                            | Kuralın amacını açıklayan bir ad          |
   | Koşullar            | Önceki komutun gerekli parametre > DateTime & güncelleştirilmesi gerekiyor                | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |   
   | Eylemler               | Konuşma yanıtı gönder-> basit düzenleyici-> önceki alarm {DateTime} öğesine güncelleştiriliyor      | Kural koşulları doğru olduğunda gerçekleştirilecek eylem |
   | Yürütme sonrası durumu | Komut tamamlandı        | Açıldıktan sonra kullanıcının durumu                   |

1. Yeni oluşturduğunuz kuralı etkileşim kurallarının en üstüne taşıyın (panelde kuralı seçin ve ortadaki bölmenin en üstündeki simgenin altında `...` bulunan yukarı oka tıklayın).
   > [!div class="mx-imgBorder"]
   > ![Aralık doğrulaması ekle](media/custom-speech-commands/one-step-correction-rules.png)
    > [!NOTE]
    > Gerçek bir uygulamada, bu kuralın Eylemler bölümünde, bir etkinliği istemciye geri gönderir veya sisteminizdeki alarmını güncelleştirmek için bir HTTP uç noktası çağırabilirsiniz.

## <a name="try-it-out"></a>Deneyin

Seçin `Train`, eğitimin tamamlanmasını bekleyin ve öğesini seçin `Test`.

- Giriş: yarın, öğleden sonra alarmını ayarla
- Çıkış: 2020-05-02 12:00:00 için bir alarm ayarlamak istediğinizden emin misiniz?
- Giriş: Evet
- Çıkış: Tamam, 2020-05-02 12:00:00 için alarm kümesi
- Giriş: Hayır, yarın saat 2PM
- Çıkış: önceki alarm 2020-05-02 14:00:00 ' ye güncelleştiriliyor
