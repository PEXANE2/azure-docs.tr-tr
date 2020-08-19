---
title: Çevirmen nedir? -Translator
titlesuffix: Azure Cognitive Services
description: Çeviriciyi, Web siteleriniz, araçları ve diğer çözümlerinizi, çok dilli kullanıcı deneyimleri sağlamak için tümleştirin.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: overview
ms.date: 05/26/2020
ms.author: swmachan
ms.custom: seodec18
ms.openlocfilehash: 5da9e5b38b88a5a148bd3d85664daa523153086b
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88589461"
---
# <a name="what-is-the-translator"></a>Çevirmen nedir?

Çevirmen, uygulamalarınızda, Web sitelerinde, araçlarınızla ve çözümlerinde tümleştirilecek. [70 ' den fazla dilde](languages.md)çok dilli kullanıcı deneyimleri eklemenize olanak tanır ve metinden metne dil çevirisi için herhangi bir işletim sistemiyle herhangi bir donanım platformunda kullanılabilir.

Çevirmen, bulutta makine öğrenimi ve AI algoritmalarının Azure bilişsel [Hizmetler](https://docs.microsoft.com/azure/?pivot=products&panel=ai) koleksiyonunun bir parçasıdır ve geliştirme projelerinizde kolayca tüketilebilir.

## <a name="about-microsoft-translator"></a>Microsoft Translator Hakkında

Çevirmen, bulut tabanlı bir makine çevirisi hizmetidir. Çekirdek hizmet, bir dizi Microsoft ürününü ve hizmeti güçlendirir ve kendi uygulamaları ve iş akışlarında dünya çapındaki binlerce işletme tarafından kullanılan ve içeriklerinin küresel bir hedef kitleye ulaşmasını sağlayan bir çevirmendir.

Çevirmen tarafından desteklenen konuşma çevirisi, [Microsoft konuşma hizmeti](https://docs.microsoft.com/azure/cognitive-services/speech-service/)aracılığıyla da kullanılabilir. Translator Konuşma Çevirisi API'si ve Özel Konuşma Tanıma hizmetten işlevselliği Birleşik ve tamamen özelleştirilebilir bir hizmette birleştirir.Konuşma Tanıma API'si, 15 Ekim 2019 tarihinde kullanımdan kaldırılacak Translator Konuşma Çevirisi API'sinin yerini alacaktır.

## <a name="language-support"></a>Dil desteği

Microsoft Translator çeviri, harf çevirisi, dil algılama ve sözlükler için çoklu dil desteği sunmaktadır. Listenin tamamı için [dil desteği](language-support.md) sayfasını inceleyebilir veya listeye [REST API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages) üzerinden program aracılığıyla erişebilirsiniz.  

## <a name="microsoft-translator-neural-machine-translation"></a>Microsoft Translator Sinirsel Makine Çevirisi

Sinirsel Makine Çevirisi (NMT), yüksek kaliteli yapay zeka destekli makine çevirileri için yeni standarttır. 2010'un ortalarında kalite konusunda duraklama noktasına ulaşan eski İstatistiksel Makine Çevirisi (SMT) teknolojisinin yerini almıştır.

NMT, yalnızca ham çeviri kalitesi puanı değil, daha akıcı olması ve insan diline daha çok benzemesi açısından da daha iyi çeviriler sunar. Bu akıcılığın temel nedeni, NMT'nin sözcükleri çevirmek için cümlenin bağlamından bütünüyle yararlanmasıdır. SMT ise her bir sözcüğün hemen birkaç sözcük öncesindeki ve sonrasındaki anlık bağlamı dikkate alıyordu.

NMT modelleri API'nin temelindedir ve son kullanıcılara görünür değildir. Gözle görülür tek fark, özellikle de Çince, Japonca ve Arapça gibi diller için geliştirilmiş çeviri kalitesidir.

[NMT'nin nasıl çalıştığı](https://www.microsoft.com/en-us/translator/mt.aspx#nnt) hakkında daha fazla bilgi edinin

## <a name="language-customization"></a>Dil özelleştirmesi

Temel Microsoft Translator hizmeti uzantısı, özel çevirici, sinir çeviri sistemini özelleştirmenize ve belirli terminoloji ve stiliniz için çeviriyi iyileştirmenize yardımcı olan çevirmenle birlikte kullanılabilir.

Custom Translator sayesinde, kendi işletmenizde veya sektörünüzde kullanılan terminolojiyi işleyebilen çeviri sistemleri oluşturabilirsiniz. Özelleştirilmiş çeviri sisteminiz daha sonra, kategori parametresini kullanarak, düzenli çevirici aracılığıyla, mevcut uygulama, iş akışlarınız ve Web siteleriniz arasında birden çok cihaz türü arasında kolayca tümleşecek.

[Dil özelleştirmesi](customization.md) hakkında daha fazla bilgi edinin

## <a name="next-steps"></a>Sonraki adımlar

- Erişim anahtarı için [kaydolun](translator-text-how-to-signup.md) .
- [API başvurusu](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference) , API 'ler için teknik belgeler sağlar.
- [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
