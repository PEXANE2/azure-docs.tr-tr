---
title: 'Nasıl yapılır: Özel komuta tek adımlık düzeltme ekleme (Önizleme) - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede, Özel Komutlar'daki bir komut için tek adımlı düzeltmelerin nasıl uygulanacağını açıklıyoruz.
services: cognitive-services
author: encorona-ms
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: encorona
ms.openlocfilehash: 86a12bd1dccc2b6ac15010546d7e990b768ebc02
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75456459"
---
# <a name="how-to-add-a-one-step-correction-to-a-custom-command-preview"></a>Nasıl Yapılır: Özel Komut'a tek adımlık düzeltme ekleme (Önizleme)

Bu makalede, bir komuta tek adımlı onay eklemeyi öğreneceksiniz.

Tek adımlı düzeltme, yeni tamamlanmış bir komutu güncelleştirmek için kullanılır.

Yani sadece bir alarm kurmak, fikrinizi değiştirmek ve alarm saatini güncelleyebilirsiniz.

- Giriş: Yarın öğlen alarmı ayarlayın
- Çıktı: "Tamam, alarm 12.06.2019 12:00:00 için ayarlanmıştır"
- Giriş: Hayır, yarın saat 13:00'te
- Çıktı: "Tamam

Bunun, bir geliştirici olarak arka uç uygulamanızdaki alarmı güncelleştirecek bir mekanizmanız olduğu anlamına geldiğini unutmayın.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki makalelerdeki adımları tamamlamış olmalısınız:

- [Quickstart: Özel Komut Oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-new.md)
- [Quickstart: Parametreler (Önizleme) ile Özel Komut oluşturma](./quickstart-custom-speech-commands-create-parameters.md)
- [Nasıl Yapılır: Özel Komuta onay ekleme (Önizleme)](./how-to-custom-speech-commands-confirmations.md)

## <a name="add-the-advanced-rules-for-one-step-correction"></a>Tek adımlı düzeltme için gelişmiş kuralları ekleyin 

Tek adımlı düzeltmeyi göstermek [için, Onaylar Nasıl](./how-to-custom-speech-commands-confirmations.md)Yapılılır'da oluşturulan **SetAlarm** komutunu genişletelim.
 
1. Önceki alarmı güncelleştirmek için gelişmiş bir kural ekleyin. 

    Bu kural, kullanıcıdan alarmın tarih ve saatini onaylamasını ister ve bir sonraki dönüş için bir onay (evet/hayır) bekleyecektir.

   | Ayar               | Önerilen değer                                                  | Açıklama                                        |
   | --------------------- | ---------------------------------------------------------------- | -------------------------------------------------- |
   | Kural Adı             | Önceki alarmı güncelleştirin                                            | Kuralın amacını açıklayan bir ad          |
   | Koşullar            | UpdateLastCommand & Gerekli Parametre - DateTime                | Kuralın ne zaman çalıştırılacakolabileceğini belirleyen koşullar    |   
   | Eylemler               | SpeechResponse - "- Önceki alarmı {DateTime} olarak güncelleştirmek"       | Kural koşulu doğru olduğunda yapılacak eylem |
   | Yürütme den sonra durum | Tam komut                                                 | Dönüşten sonra kullanıcının durumu                   |

1. Oluşturduğunuz kuralı gelişmiş kuralların en üstüne taşıyın (paneldeki kuralın üzerine kaydırın ve YUKARı ok'u tıklatın).
   > [!div class="mx-imgBorder"]
   > ![Aralık doğrulama ekleme](media/custom-speech-commands/one-step-correction-rules.png)

> [!NOTE]
> Gerçek bir uygulamada, bu kuralın Eylemler bölümünde de istemciye bir etkinlik gönderir veya sisteminizdeki alarmı güncelleştirmek için bir HTTP bitiş noktası çağırırsınız.

## <a name="try-it-out"></a>Deneyin

Test panelini seçin ve birkaç etkileşim deneyin.

- Giriş: Yarın öğlen alarmı ayarlayın
- Çıktı: "12/07/2019 12:00:00 için alarm kurmak istediğinizden emin misiniz?"
- Giriş: Evet
- Çıktı: "Tamam, alarm 12.07.2019 12:00:00 için ayarlanmıştır"
- Giriş: Hayır, yarın saat 13:00'te
- Çıktı: "Önceki alarmın 12.07.2019 13:00:00'a güncellenmesi"
