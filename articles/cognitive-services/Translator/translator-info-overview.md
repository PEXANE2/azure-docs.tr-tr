---
title: Microsoft Translator hizmeti
titlesuffix: Azure Cognitive Services
description: Çeviriciyi, web sitelerinizi, araçlarınızla ve çok dilli kullanıcı deneyimleri sağlamak için diğer çözümlerle tümleştirin.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.topic: overview
ms.subservice: translator-text
ms.date: 03/15/2021
ms.author: lajanuar
ms.custom: cog-serv-seo-aug-2020
keywords: Çevirmen, metin çevirisi, makine çevirisi, çeviri hizmeti
ms.openlocfilehash: ec76aa7554110b7440eb825f2d5e86ae2da6baa2
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104657731"
---
# <a name="what-is-the-translator-service"></a>Çevirmen hizmeti nedir?

Çevirici, bulut tabanlı bir makine çevirisi hizmetidir ve akıllı uygulamalar oluşturmak için kullanılan bilişsel API 'lerin Azure bilişsel [Hizmetler](../../index.yml?panel=ai&pivot=products) ailesinin bir parçasıdır. Translator, uygulamalarınızda, Web sitelerinde, araçlarınızla ve çözümlerinde tümleştirilebilen kolay bir işlemdir. [90 dilde ve diatadaki](./language-support.md) çok dilli kullanıcı deneyimleri eklemenize olanak tanır ve herhangi bir işletim sistemiyle metin çevirisi için kullanılabilir.

Bu belge aşağıdaki makale türlerini içerir:  

* [**Hızlı**](quickstart-translator.md) başlangıçlarda, hizmette istek yapma konusunda size kılavuzluk eden başlangıç yönergeleri bulunur.  
* [**Nasıl yapılır kılavuzlarında**](translator-how-to-signup.md) , hizmetin daha belirli veya özelleştirilmiş yollarla kullanılmasına ilişkin yönergeler bulunur.  
* [**Kavramlar**](character-counts.md) , hizmet işlevselliği ve özelliklerinin ayrıntılı açıklamalarını sağlar.  
* [**Öğreticiler**](tutorial-wpf-translation-csharp.md) daha fazla iş çözümlerinde hizmeti bir bileşen olarak nasıl kullanacağınızı gösteren kılavuzlardır.  


## <a name="about-microsoft-translator"></a>Microsoft Translator Hakkında

Çevirmen birçok Microsoft ürününü ve hizmetini güçlendirir ve kendi uygulamalarında ve iş akışlarında dünya çapındaki binlerce işletme tarafından kullanılır.

Çevirmen ile desteklenen konuşma çevirisi, [Azure konuşma hizmeti](../speech-service/index.yml)aracılığıyla da kullanılabilir. Translator Konuşma Çevirisi API'si ve Özel Konuşma Tanıma hizmetten işlevselliği Birleşik ve tamamen özelleştirilebilir bir hizmette birleştirir. 

## <a name="language-support"></a>Dil desteği

Translator, metin çevirisi, alfabede, dil algılama ve sözlükler için çok dil desteği sağlar. Listenin tamamı için [dil desteği](language-support.md) sayfasını inceleyebilir veya listeye [REST API](./reference/v3-0-languages.md) üzerinden program aracılığıyla erişebilirsiniz.  

## <a name="microsoft-translator-neural-machine-translation"></a>Microsoft Translator Sinirsel Makine Çevirisi

Sinirsel Makine Çevirisi (NMT), yüksek kaliteli yapay zeka destekli makine çevirileri için yeni standarttır. 2010'un ortalarında kalite konusunda duraklama noktasına ulaşan eski İstatistiksel Makine Çevirisi (SMT) teknolojisinin yerini almıştır.

NMT, yalnızca ham çeviri kalitesi puanı değil, daha akıcı olması ve insan diline daha çok benzemesi açısından da daha iyi çeviriler sunar. Bu akıcılığın temel nedeni, NMT'nin sözcükleri çevirmek için cümlenin bağlamından bütünüyle yararlanmasıdır. SMT ise her bir sözcüğün hemen birkaç sözcük öncesindeki ve sonrasındaki anlık bağlamı dikkate alıyordu.

NMT modelleri API'nin temelindedir ve son kullanıcılara görünür değildir. Gözle görülür tek fark, özellikle de Çince, Japonca ve Arapça gibi diller için geliştirilmiş çeviri kalitesidir.

[NMT 'nin nasıl çalıştığı](https://www.microsoft.com/en-us/translator/mt.aspx#nnt)hakkında daha fazla bilgi edinin.

## <a name="improve-translations-with-custom-translator"></a>Özel çevirmenle çevirileri geliştirme

 Translator hizmetinin bir uzantısı olan [özel çevirici](customization.md), sinir çeviri sistemini özelleştirmek ve belirli terminoloji ve stiliniz için çeviriyi geliştirmek üzere kullanılabilir.

Özel çevirmenle, kendi işletmenizde veya sektörde kullanılan terminolojiyi işlemek için çeviri sistemleri oluşturabilirsiniz. Özelleştirilmiş çeviri sisteminiz, kategori parametresini kullanarak, normal çevirici aracılığıyla mevcut uygulamalarınız, iş akışlarınız, Web siteleriniz ve cihazlarınızla kolayca tümleştirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

- Erişim Anahtarlarınızı ve uç noktanızı almak için [bir Translator hizmeti oluşturun](./translator-how-to-signup.md) .
- Translator hizmetini hızlı bir şekilde çağırmak için hızlı [Başlangıç](quickstart-translator.md) Öğreticimizi deneyin.
- [API başvurusu](./reference/v3-0-reference.md) , API 'ler için teknik belgeler sağlar.
- [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
