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
ms.openlocfilehash: 86a12bd1dccc2b6ac15010546d7e990b768ebc02
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75456459"
---
# <a name="how-to-add-a-one-step-correction-to-a-custom-command-preview"></a>Nasıl yapılır: özel komuta tek adımlı bir düzeltme ekleme (Önizleme)

Bu makalede, bir komuta nasıl tek adımlı onay ekleneceğini öğreneceksiniz.

Tek adımlı düzeltme, az önce tamamlanmış olan bir komutu güncelleştirmek için kullanılır.

Örneğin, yalnızca bir alarm ayarlarsanız, fikrinizi değiştirebilir ve alarmın saatini güncelleştirebilirsiniz.

- Giriş: yarın, öğleden sonra alarmını ayarla
- Çıkış: "Tamam, alarm kümesi 12/06/2019 12:00:00"
- Giriş: Hayır, yarın saat 1pm
- Çıkış: "Tamam

Bunun, bir geliştiricinin arka uç uygulamanızda alarmın güncelleştirilmesi için bir mekanizmaya sahip olduğunu aklınızda bulundurun.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki makalelerde bulunan adımları tamamlamış olmanız gerekir:

- [Hızlı başlangıç: özel komut oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-new.md)
- [Hızlı başlangıç: parametrelerle özel komut oluşturma (Önizleme)](./quickstart-custom-speech-commands-create-parameters.md)
- [Nasıl yapılır: özel komuta onay ekleme (Önizleme)](./how-to-custom-speech-commands-confirmations.md)

## <a name="add-the-advanced-rules-for-one-step-correction"></a>Tek adımlı düzeltme için gelişmiş kuralları ekleme 

Tek adımlı bir düzeltmeyi göstermek için, [Onaylarda](./how-to-custom-speech-commands-confirmations.md)oluşturulan **SetAlarm** komutunu bu şekilde genişletelim.
 
1. Önceki uyarıyı güncelleştirmek için gelişmiş bir kural ekleyin. 

    Bu kural, kullanıcıdan alarm tarihini ve saatini onaylamasını ve bir sonraki sırayla bir onay (Evet/Hayır) bekliyor olduğunu sorar.

   | Ayar               | Önerilen değer                                                  | Açıklama                                        |
   | --------------------- | ---------------------------------------------------------------- | -------------------------------------------------- |
   | Kural Adı             | Önceki alarmı Güncelleştir                                            | Kuralın amacını açıklayan bir ad          |
   | Koşullar            | UpdateLastCommand & gerekli parametre-DateTime                | Kuralın ne zaman çalıştırılabilmesine belirleme koşulları    |   
   | Eylemler               | SpeechResponse-"-önceki alarm {DateTime} ile güncelleştiriliyor"       | Kural koşulu true olduğunda gerçekleştirilecek eylem |
   | Yürütmeden sonra durum | Komutu Tamam                                                 | Açıldıktan sonra kullanıcının durumu                   |

1. Yeni oluşturduğunuz kuralı gelişmiş kuralların üst kısmına taşıyın (panelde kural üzerinde ilerleyin ve yukarı oka tıklayın).
   > [!div class="mx-imgBorder"]
   > ![Aralık doğrulaması ekleme](media/custom-speech-commands/one-step-correction-rules.png)

> [!NOTE]
> Gerçek bir uygulamada, bu kuralın Eylemler bölümünde, bir etkinliği istemciye geri gönderir veya sisteminizdeki alarmını güncelleştirmek için bir HTTP uç noktası çağırabilirsiniz.

## <a name="try-it-out"></a>Deneyin

Test panelini seçin ve birkaç etkileşimi deneyin.

- Giriş: yarın, öğleden sonra alarmını ayarla
- Çıkış: "12/07/2019 12:00:00 için bir alarm ayarlamak istediğinizden emin misiniz?"
- Giriş: Evet
- Çıkış: "Tamam, alarm kümesi 12/07/2019 12:00:00"
- Giriş: Hayır, yarın saat 1pm
- Çıkış: "önceki alarm 12/07/2019 13:00:00 ' ye güncelleştiriliyor"
