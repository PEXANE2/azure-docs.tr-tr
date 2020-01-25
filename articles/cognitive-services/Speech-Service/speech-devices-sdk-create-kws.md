---
title: Özel anahtar sözcükler oluşturma-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Cihazınız her zaman bir anahtar sözcüğü (veya tümceciği) dinler. Kullanıcı anahtar sözcüğünü söyyorsa, cihaz sonraki tüm sesleri buluta gönderir ve Kullanıcı konuşmayı durduruncaya kadar. Anahtar keliinizi özelleştirmek, cihazınızı ayırt etmenin ve markanızı güçlendirmenin etkili bir yoludur.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/11/2019
ms.author: dapine
ms.openlocfilehash: 46e9f8e660c3fd62807d630481e6b3057d2351a5
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76717011"
---
# <a name="create-a-custom-keyword-using-speech-studio"></a>Konuşma Studio kullanarak özel anahtar sözcük oluşturma

Cihazınız her zaman bir anahtar sözcüğü (veya tümceciği) dinler. Örneğin, "Hey Cortana", Cortana yardımcısı için bir anahtar sözcüktür. Kullanıcı anahtar sözcüğünü söyyorsa, cihaz sonraki tüm sesleri buluta gönderir ve Kullanıcı konuşmayı durduruncaya kadar. Anahtar keliinizi özelleştirmek, cihazınızı ayırt etmenin ve markanızı güçlendirmenin etkili bir yoludur.

Bu makalede, cihazınız için özel bir anahtar sözcük oluşturmayı öğreneceksiniz.

## <a name="create-your-keyword"></a>Anahtar keliinizi oluşturma

Özel bir anahtar sözcüğü kullanabilmeniz için, [konuşma Studio](https://aka.ms/sdsdk-speechportal)'Daki [özel anahtar](https://aka.ms/sdsdk-wakewordportal) sözcük sayfasını kullanarak bir anahtar sözcük oluşturmanız gerekir. Bir anahtar sözcük girdikten sonra, cihazınıza dağıttığınız bir dosya oluşturur.

1. [Konuşma Studio](https://aka.ms/sdsdk-speechportal) 'ya gidin ve **oturum açın** veya henüz bir konuşma aboneliğiniz yoksa, [**abonelik oluştur**](https://go.microsoft.com/fwlink/?linkid=2086754)' u seçin.

1. [Özel anahtar sözcük](https://aka.ms/sdsdk-wakewordportal) sayfasında **Yeni bir proje**oluşturun. 

1. Bir **ad**, isteğe bağlı bir **Açıklama**girin ve dili seçin. Her dil için bir proje gerekir ve destek şu anda en-US diliyle sınırlı olur.

    ![Anahtar sözcük projenizi açıkla](media/custom-keyword/custom-kws-portal-new-project.png)

1. Listeden projenizi seçin. 

    ![Anahtar sözcük projenizi seçin](media/custom-keyword/custom-kws-portal-project-list.png)

1. Yeni bir anahtar sözcük modeli başlatmak için **modeli eğitme**' ye tıklayın.

1. Anahtar sözcük modeli için bir **ad** ve Isteğe bağlı **Açıklama** girin ve istediğiniz **anahtar sözcüğü** yazın ve **İleri**' ye tıklayın. Etkin bir anahtar sözcük seçmenize yardımcı olmak için bazı [kılavuzlardır](speech-devices-sdk-kws-guidelines.md#choose-an-effective-keyword) .

    ![Anahtar keliinizi girin](media/custom-keyword/custom-kws-portal-new-model.png)

1. Portal şimdi anahtar kelimeden aday söylenişleri oluşturacak. Oynat düğmelerine tıklayarak her bir adayı dinleyin ve yanlış olan herhangi bir söylenin yanındaki denetimleri kaldırın. Yalnızca güzel Söylenişler denetlendikten sonra, anahtar sözcüğünü oluşturmaya başlamak için **eğitme** ' ye tıklayın. 

    ![Anahtar keliinizi gözden geçirin](media/custom-keyword/custom-kws-portal-choose-prons.png)

1. Modelin oluşturulması otuz dakika kadar sürebilir. Model tamamlandığında anahtar sözcük listesi **Işlemden** **başarılı** olarak değişecektir. Sonra dosyayı indirebilirsiniz.

    ![Anahtar keliinizi gözden geçirin](media/custom-keyword/custom-kws-portal-download-model.png)

1. .Zip dosyasını bilgisayarınıza kaydedin. Özel anahtar keliinizi cihazınıza dağıtmak için bu dosyaya ihtiyacınız olacak.

## <a name="next-steps"></a>Sonraki adımlar

[Konuşma CIHAZLARı SDK hızlı başlangıç](https://aka.ms/sdsdk-quickstart)ile özel anahtar keliinizi test edin.
