---
title: Özel anahtar kelimeler oluşturma - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Cihazınız her zaman bir anahtar kelime (veya tümcecik) için dinliyor. Kullanıcı anahtar kelimeyi söylediğinde, kullanıcı konuşmayı durdurana kadar aygıt sonraki tüm sesleri buluta gönderir. Anahtar kelimenizi özelleştirmek, cihazınızı farklılaştırmanın ve markanızı güçlendirmenin etkili bir yoludur.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/11/2019
ms.author: dapine
ms.openlocfilehash: 46e9f8e660c3fd62807d630481e6b3057d2351a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76717011"
---
# <a name="create-a-custom-keyword-using-speech-studio"></a>Speech Studio'u kullanarak özel bir anahtar kelime oluşturma

Cihazınız her zaman bir anahtar kelime (veya tümcecik) için dinliyor. Örneğin, "Hey Cortana" Cortana yardımcısı için bir anahtar kelimedir. Kullanıcı anahtar kelimeyi söylediğinde, kullanıcı konuşmayı durdurana kadar aygıt sonraki tüm sesleri buluta gönderir. Anahtar kelimenizi özelleştirmek, cihazınızı farklılaştırmanın ve markanızı güçlendirmenin etkili bir yoludur.

Bu makalede, cihazınız için özel bir anahtar kelime oluşturmayı öğrenirsiniz.

## <a name="create-your-keyword"></a>Anahtar kelimenizi oluşturma

Özel bir anahtar kelime kullanabilmeniz için, [Konuşma Stüdyosu'ndaki](https://aka.ms/sdsdk-speechportal) [Özel Anahtar Kelime](https://aka.ms/sdsdk-wakewordportal) sayfasını kullanarak bir anahtar kelime oluşturmanız gerekir. Bir anahtar kelime sağladıktan sonra, cihazınıza dağıttığınız bir dosya üretir.

1. [Konuşma Stüdyosu'na](https://aka.ms/sdsdk-speechportal) gidin ve **oturum açın** veya henüz bir konuşma aboneliğiniz yoksa [**abonelik oluştur'u**](https://go.microsoft.com/fwlink/?linkid=2086754)seçin.

1. Özel [Anahtar Kelime](https://aka.ms/sdsdk-wakewordportal) sayfasında, **yeni**bir proje oluşturun. 

1. Bir **Ad**, isteğe bağlı **Bir Açıklama**girin ve dili seçin. Dil başına bir projeye ihtiyacınız olacak ve destek şu anda en-ABD diliyle sınırlıdır.

    ![Anahtar kelime projenizi açıklayın](media/custom-keyword/custom-kws-portal-new-project.png)

1. Projenizi listeden seçin. 

    ![Anahtar kelime projenizi seçin](media/custom-keyword/custom-kws-portal-project-list.png)

1. Yeni bir anahtar kelime modeli başlatmak için **Train modelini**tıklatın.

1. Anahtar kelime modeli için bir **Ad** girin ve isteğe bağlı **Açıklama** ve seçtiğiniz **Anahtar Kelime** yazın ve **İleri'yi**tıklatın. Etkili bir anahtar kelime seçmeye yardımcı olacak bazı [yönergelerimiz](speech-devices-sdk-kws-guidelines.md#choose-an-effective-keyword) vardır.

    ![Anahtar kelimenizi girin](media/custom-keyword/custom-kws-portal-new-model.png)

1. Portal artık anahtar kelimeniz için aday telaffuzları oluşturacaktır. Oynat düğmelerini tıklatarak her adayı dinleyin ve yanlış telaffuzların yanındaki çekleri kaldırın. Yalnızca iyi telaffuzlar kontrol edildikten sonra, anahtar kelimeoluşturmaya başlamak için **Tren'i** tıklatın. 

    ![Anahtar kelimenizi inceleyin](media/custom-keyword/custom-kws-portal-choose-prons.png)

1. Modelin oluşturulması otuz dakika kadar sürebilir. Anahtar kelime listesi, model tamamlandığında **İşleme'den** **Başarılı'ya** değişir. Daha sonra dosyayı indirebilirsiniz.

    ![Anahtar kelimenizi inceleyin](media/custom-keyword/custom-kws-portal-download-model.png)

1. .zip dosyasını bilgisayarınıza kaydedin. Özel anahtar kelimenizi cihazınıza dağıtmak için bu dosyaya ihtiyacınız olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

[Konuşma Aygıtları SDK Quickstart](https://aka.ms/sdsdk-quickstart)ile özel anahtar kelimenizi test edin.
