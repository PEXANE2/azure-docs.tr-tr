---
title: Video Dizinleyici'de Dil modelini özelleştirme - Azure
titleSuffix: Azure Media Services
description: Bu makalede, Video Indexer'da Dil modelinin ne olduğu ve nasıl özelleştirilene ilgili genel bir bakış yer almaktadır.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: anzaman
ms.openlocfilehash: b096b9352be65033f2fb782b118e815dc16b43b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73838312"
---
# <a name="customize-a-language-model-with-video-indexer"></a>Video Dizinleyici ile Dil modelini özelleştirme

Video Indexer, Microsoft Özel Konuşma [Hizmeti](https://azure.microsoft.com/services/cognitive-services/custom-speech-service/)ile tümleştirme yoluyla otomatik konuşma tanımayı destekler. Uyarlama metnini, yani sözcük dağarcığının dishaline uyum sağlamasını istediğiniz etki alanından metin yükleyerek Dil modelini özelleştirebilirsiniz. Modelinizi eğittikten sonra, uyarlama metninde görünen yeni sözcükler varsayılan telaffuzunu varsayarak tanınacak ve Dil modeli yeni olası sözcük dizilerini öğrenir. Özel Dil modelleri İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Çince (Basitleştirilmiş), Japonca, Rusça, Brezilya Portekizcesi, Hintçe ve Korece için desteklenir. 

Örnek olarak "Kubernetes" (Azure Kubernetes hizmeti bağlamında) gibi son derece özel bir kelimeyi ele alalım. Kelime Video Indexer için yeni olduğundan, "topluluklar" olarak kabul edilmektedir. Modeli "Kubernetes" olarak tanıması için eğitmelisin. Diğer durumlarda, sözcükler var, ancak Dil modeli bunların belirli bir bağlamda görünmesini beklemiyor. Örneğin, "kapsayıcı hizmeti" özel olmayan bir Dil modelinin belirli bir sözcük kümesi olarak tanıyacağı 2 sözcüklü bir dizi değildir.

Metin dosyasındaki bir listede bağlam olmadan sözcükler yükleme seçeneğiniz vardır. Bu kısmi adaptasyon olarak kabul edilir. Alternatif olarak, daha iyi uyarlama için içeriğiniz ile ilgili belgelerin veya cümlelerin metin dosyasını(lar) yükleyebilirsiniz.

Video Dizinleyici API'lerini veya web sitesini, bu konunun [Sonraki adımlar](#next-steps) bölümünde açıklandığı gibi özel Dil modelleri oluşturmak ve bunları yeniden oluşturmak için kullanabilirsiniz.

## <a name="best-practices-for-custom-language-models"></a>Özel Dil modelleri için en iyi uygulamalar

Video Indexer sözcük kombinasyonları olasılıklarına göre öğrenir, böylece en iyi öğrenmek için:

* Konuşulan cümlelerin yeterince gerçek örneklerini verin.
* Satır başına sadece bir cümle koy, daha fazla değil. Aksi takdirde sistem cümleler arasında olasılıkları öğrenir.
* Bu başkalarına karşı kelime artırmak için bir cümle olarak bir kelime koymak tamam, ama sistem tam cümleler en iyi öğrenir.
* Yeni sözcükler veya kısaltmalar tanıtırken, mümkünse, sisteme mümkün olduğunca çok bağlam vermek için tam bir cümlede birçok kullanım örneği verin.
* Birkaç adaptasyon seçeneği koymayı deneyin ve sizin için nasıl çalıştıklarını görün.
* Aynı cümlenin birden çok kez tekrarını önlemek. Bu girdinin geri kalanına karşı önyargı oluşturabilir.
* Onlar atılır alırsınız gibi nadir olmayan semboller (~, # @ &) dahil kaçının. Göründükleri cümleler de atılır.
* Yüz binlerce cümle gibi çok büyük girdiler koymaktan kaçının, çünkü bunu yapmak artırmanın etkisini sulandıracaktır.

## <a name="next-steps"></a>Sonraki adımlar

[API'leri kullanarak Dil modelini özelleştir](customize-language-model-with-api.md)

[Web sitesini kullanarak Dil modelini özelleştir](customize-language-model-with-website.md)
