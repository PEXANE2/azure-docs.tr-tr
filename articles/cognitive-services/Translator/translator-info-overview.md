---
title: Translator nedir? -Translator
titlesuffix: Azure Cognitive Services
description: Çeviriciyi, web sitelerinizi, araçlarınızla ve çok dilli kullanıcı deneyimleri sağlamak için diğer çözümlerle tümleştirin.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: overview
ms.date: 05/26/2020
ms.author: swmachan
ms.custom: seodec18
ms.openlocfilehash: 206e1bc86404fa41b84647c50482dbe63301c0af
ms.sourcegitcommit: ac5cbef0706d9910a76e4c0841fdac3ef8ed2e82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89425968"
---
# <a name="what-is-translator"></a>Translator nedir?

Azure bilişsel hizmetler çevirmeni, bulut tabanlı bir makine çevirisi hizmetidir ve akıllı uygulamalar oluşturmaya yönelik bilişsel API 'lerin Azure bilişsel [Hizmetler](https://docs.microsoft.com/azure/?pivot=products&panel=ai) ailesinin bir parçasıdır. Translator, uygulamalarınızda, Web sitelerinde, araçlarınızla ve çözümlerinde tümleştirilebilen kolay bir işlemdir. [70 ' den fazla dilde](languages.md)çok dilli kullanıcı deneyimleri eklemenize olanak tanır ve metinden metne dil çevirisi için herhangi bir işletim sistemiyle herhangi bir donanım platformunda kullanılabilir.

## <a name="about-microsoft-translator"></a>Microsoft Translator Hakkında

Azure bilişsel hizmetler çevirmeni, bulut tabanlı bir makine çevirisi hizmetidir. Çevirmen, bir dizi Microsoft ürününü ve hizmetini güçlendirir ve kendi uygulamaları ve iş akışlarında dünya çapındaki binlerce işletme tarafından kullanılır ve bu da içeriğinin küresel bir hedef kitleye ulaşmasını sağlar.

Çevirmen ile desteklenen konuşma çevirisi, [Microsoft konuşma hizmeti](https://docs.microsoft.com/azure/cognitive-services/speech-service/)aracılığıyla da kullanılabilir. Translator Konuşma Çevirisi API'si ve Özel Konuşma Tanıma hizmetten işlevselliği Birleşik ve tamamen özelleştirilebilir bir hizmette birleştirir. 

## <a name="language-support"></a>Dil desteği

Microsoft Translator çeviri, harf çevirisi, dil algılama ve sözlükler için çoklu dil desteği sunmaktadır. Listenin tamamı için [dil desteği](language-support.md) sayfasını inceleyebilir veya listeye [REST API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages) üzerinden program aracılığıyla erişebilirsiniz.  

## <a name="microsoft-translator-neural-machine-translation"></a>Microsoft Translator Sinirsel Makine Çevirisi

Sinirsel Makine Çevirisi (NMT), yüksek kaliteli yapay zeka destekli makine çevirileri için yeni standarttır. 2010'un ortalarında kalite konusunda duraklama noktasına ulaşan eski İstatistiksel Makine Çevirisi (SMT) teknolojisinin yerini almıştır.

NMT, yalnızca ham çeviri kalitesi puanı değil, daha akıcı olması ve insan diline daha çok benzemesi açısından da daha iyi çeviriler sunar. Bu akıcılığın temel nedeni, NMT'nin sözcükleri çevirmek için cümlenin bağlamından bütünüyle yararlanmasıdır. SMT ise her bir sözcüğün hemen birkaç sözcük öncesindeki ve sonrasındaki anlık bağlamı dikkate alıyordu.

NMT modelleri API'nin temelindedir ve son kullanıcılara görünür değildir. Gözle görülür tek fark, özellikle de Çince, Japonca ve Arapça gibi diller için geliştirilmiş çeviri kalitesidir.

[NMT'nin nasıl çalıştığı](https://www.microsoft.com/en-us/translator/mt.aspx#nnt) hakkında daha fazla bilgi edinin

## <a name="language-customization"></a>Dil özelleştirmesi

Çevirmen hizmeti uzantısı, özel çevirici, sinir çeviri sistemini özelleştirmenize ve belirli terminoloji ve stiliniz için çeviriyi iyileştirmenize yardımcı olmak üzere Translator ile birlikte kullanılabilir.

Custom Translator sayesinde, kendi işletmenizde veya sektörünüzde kullanılan terminolojiyi işleyebilen çeviri sistemleri oluşturabilirsiniz. Özelleştirilmiş çeviri sisteminiz daha sonra, kategori parametresini kullanarak, düzenli çevirici aracılığıyla, mevcut uygulama, iş akışlarınız ve Web siteleriniz arasında birden çok cihaz türü arasında kolayca tümleşecek.

[Dil özelleştirmesi](customization.md) hakkında daha fazla bilgi edinin

## <a name="next-steps"></a>Sonraki adımlar

- Erişim anahtarı için [kaydolun](translator-text-how-to-signup.md) .
- [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference) , API 'ler için teknik belgeler sağlar.
- [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
