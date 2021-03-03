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
ms.date: 02/15/2021
ms.author: lajanuar
ms.custom: cog-serv-seo-aug-2020
keywords: Çevirmen, metin çevirisi, makine çevirisi, çeviri hizmeti
ms.openlocfilehash: 72df700d38cad3ee31ef438d7df1c890acde9e6b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101727983"
---
# <a name="what-is-the-translator-service"></a>Çevirmen hizmeti nedir?

Çevirici, bulut tabanlı bir makine çevirisi hizmetidir ve akıllı uygulamalar oluşturmak için kullanılan bilişsel API 'lerin Azure bilişsel [Hizmetler](../../index.yml?panel=ai&pivot=products) ailesinin bir parçasıdır. Translator, uygulamalarınızda, Web sitelerinde, araçlarınızla ve çözümlerinde tümleştirilebilen kolay bir işlemdir. [90 dilde ve diatadaki](./language-support.md)çok dilli kullanıcı deneyimleri eklemenize olanak tanır. Ve bu, herhangi bir donanım platformunda metin çevirisi için herhangi bir işletim sistemiyle kullanılabilir.

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

 Translator hizmetinin bir uzantısı olan özel çevirici, sinir çeviri sistemini özelleştirmek ve belirli terminoloji ve stiliniz için çeviriyi geliştirmek üzere Translator ile birlikte kullanılabilir.

Özel çevirmenle, kendi işletmenizde veya sektörde kullanılan terminolojiyi işlemek için çeviri sistemleri oluşturabilirsiniz. Özelleştirilmiş çeviri sisteminiz, kategori parametresini kullanarak, normal çevirici aracılığıyla mevcut uygulamalarınız, iş akışlarınız, Web siteleriniz ve cihazlarınızla kolayca tümleştirilebilir.

[Özel Çeviri](customization.md) hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

- Erişim Anahtarlarınızı ve uç noktanızı almak için [bir Translator hizmeti oluşturun](./translator-how-to-signup.md) .
- Translator hizmetini hızlı bir şekilde çağırmak için hızlı [Başlangıç](quickstart-translator.md) Öğreticimizi deneyin.
- [API başvurusu](./reference/v3-0-reference.md) , API 'ler için teknik belgeler sağlar.
- [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
