---
title: Microsoft Translator hizmeti
titlesuffix: Azure Cognitive Services
description: Çeviriciyi, web sitelerinizi, araçlarınızla ve çok dilli kullanıcı deneyimleri sağlamak için diğer çözümlerle tümleştirin.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: overview
ms.date: 09/11/2020
ms.author: swmachan
ms.custom: cog-serv-seo-aug-2020
keywords: Çevirmen, metin çevirisi, makine çevirisi, çeviri hizmeti
ms.openlocfilehash: 32ae7f75c1b953e8af7dfef83c1971c2f78b0b62
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90530344"
---
# <a name="what-is-the-translator-service"></a>Çevirmen hizmeti nedir?

Çevirici, bulut tabanlı bir makine çevirisi hizmetidir ve akıllı uygulamalar oluşturmak için kullanılan bilişsel API 'lerin Azure bilişsel [Hizmetler](https://docs.microsoft.com/azure/?pivot=products&panel=ai) ailesinin bir parçasıdır. Translator, uygulamalarınızda, Web sitelerinde, araçlarınızla ve çözümlerinde tümleştirilebilen kolay bir işlemdir. [70 ' den fazla dilde](languages.md)çok dilli kullanıcı deneyimleri eklemenize olanak tanır ve metin çevirisi için herhangi bir işletim sistemiyle herhangi bir donanım platformunda kullanılabilir.

## <a name="about-microsoft-translator"></a>Microsoft Translator Hakkında

Çevirmen, bir dizi Microsoft ürününü ve hizmetini güçlendirir ve kendi uygulamaları ve iş akışlarında dünya çapındaki binlerce işletme tarafından kullanılır ve bu da içeriğinin küresel bir hedef kitleye ulaşmasını sağlar.

Çevirmen ile desteklenen konuşma çevirisi, [Azure konuşma hizmeti](https://docs.microsoft.com/azure/cognitive-services/speech-service/)aracılığıyla da kullanılabilir. Translator Konuşma Çevirisi API'si ve Özel Konuşma Tanıma hizmetten işlevselliği Birleşik ve tamamen özelleştirilebilir bir hizmette birleştirir. 

## <a name="language-support"></a>Dil desteği

Translator, metin çevirisi, alfabede, dil algılama ve sözlükler için çok dil desteği sağlar. Listenin tamamı için [dil desteği](language-support.md) sayfasını inceleyebilir veya listeye [REST API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages) üzerinden program aracılığıyla erişebilirsiniz.  

## <a name="microsoft-translator-neural-machine-translation"></a>Microsoft Translator Sinirsel Makine Çevirisi

Sinirsel Makine Çevirisi (NMT), yüksek kaliteli yapay zeka destekli makine çevirileri için yeni standarttır. 2010'un ortalarında kalite konusunda duraklama noktasına ulaşan eski İstatistiksel Makine Çevirisi (SMT) teknolojisinin yerini almıştır.

NMT, yalnızca ham çeviri kalitesi puanı değil, daha akıcı olması ve insan diline daha çok benzemesi açısından da daha iyi çeviriler sunar. Bu akıcılığın temel nedeni, NMT'nin sözcükleri çevirmek için cümlenin bağlamından bütünüyle yararlanmasıdır. SMT ise her bir sözcüğün hemen birkaç sözcük öncesindeki ve sonrasındaki anlık bağlamı dikkate alıyordu.

NMT modelleri API'nin temelindedir ve son kullanıcılara görünür değildir. Gözle görülür tek fark, özellikle de Çince, Japonca ve Arapça gibi diller için geliştirilmiş çeviri kalitesidir.

[NMT 'nin nasıl çalıştığı](https://www.microsoft.com/en-us/translator/mt.aspx#nnt)hakkında daha fazla bilgi edinin.

## <a name="improve-translations-with-custom-translator"></a>Özel çevirmenle çevirileri geliştirme

Çevirmen hizmeti uzantısı, özel çevirici, sinir çeviri sistemini özelleştirmenize ve belirli terminoloji ve stiliniz için çeviriyi iyileştirmenize yardımcı olmak üzere Translator ile birlikte kullanılabilir.

Custom Translator sayesinde, kendi işletmenizde veya sektörünüzde kullanılan terminolojiyi işleyebilen çeviri sistemleri oluşturabilirsiniz. Özelleştirilmiş çeviri sisteminiz daha sonra, kategori parametresini kullanarak, düzenli çevirici aracılığıyla, mevcut uygulama, iş akışlarınız ve Web siteleriniz arasında birden çok cihaz türü arasında kolayca tümleşecek.

[Özel çevirici](customization.md)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

- Erişim anahtarı için [kaydolun](translator-text-how-to-signup.md) .
- Translator hizmetini hızlı bir şekilde çağırmak için hızlı [Başlangıç](quickstart-translator.md) Öğreticimizi deneyin.
- [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference) , API 'ler için teknik belgeler sağlar.
- [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
