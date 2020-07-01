---
title: Bing Yerel İşletme Arama API'si nedir?
titleSuffix: Azure Cognitive Services
description: Bing Yerel İşletme Arama API'si, uygulamalarınızın arama sorguları temelinde yerel konumlar ve işletmeler hakkında bilgi bulmasına olanak tanıyan bir RESTful hizmetidir.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: overview
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: 685ee0c616234563981e55f14213e424daae32f5
ms.sourcegitcommit: 32592ba24c93aa9249f9bd1193ff157235f66d7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85611280"
---
# <a name="what-is-bing-local-business-search"></a>Bing yerel Iş arama nedir?
Bing yerel Iş Arama API 'SI, uygulamalarınızın arama sorguları temelinde yerel işletmeler hakkında bilgi bulmasına olanak tanıyan bir yeniden hizmet hizmetidir. Örneğin, `q=<business-name> in Redmond, Washington` veya `q=Italian restaurants near me` . 

## <a name="features"></a>Özellikler
| Özellik | Description |  
| -- | -- | 
| [Yerel işletmeleri ve konumları bulma](quickstarts/local-quickstart.md) | Bing yerel Iş Arama API 'SI, bir sorgudan yerelleştirilmiş sonuçlar alır. Sonuç olarak, iş Web sitesinin bir URL 'si ve görüntü metni, telefon numarası ve coğrafi konum gibi bir URL bulunur: GPS koordinatları, şehir, sokak adresi |  
| [Yerel sonuçları coğrafi sınırlara göre filtrele](specify-geographic-search.md) | Sonuçları, dairesel bir alan veya kare sınırlayıcı kutusuyla belirtilen belirli bir coğrafi alanla sınırlamak için arama parametresi olarak koordinatları ekleyin. | 
| [Yerel iş sonuçlarını kategoriye göre filtrele](local-categories.md) | Kategoriye göre yerel iş sonuçlarını arayın. Bu seçenek, çeşitli iş kategorilerindeki yerelleştirilmiş sonuçları döndürmek için çağıranın ters IP konumunu veya GPS koordinatlarını kullanır.|

## <a name="workflow"></a>İş akışı
HTTP istekleri yapıp JSON yanıtlarını ayrıştırabilen herhangi bir programlama dilinden Bing yerel Iş Arama API 'sini çağırın. Bu hizmete REST API kullanılarak erişilebilir.
 
1. Bing Arama API'leri erişimi olan bilişsel [HIZMETLER API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/cognitive-services/).   
2. URL, sorgu parametresi için arama terimlerinizi kodlayın `q=""` . Örneğin `q=nearby+restaurant` veya `q=nearby%20restaurant` olabilir. Gerekirse, sayfalandırma da ayarlayın. 
3. [Bing yerel Iş Arama API 'sine istek](quickstarts/local-quickstart.md) gönderme 
4. JSON yanıtını Ayrıştır 

> [!NOTE]
> Şu anda yerel Iş araması: 
> * Yalnızca `en-US` pazar payını destekler. 
> * Bing Otomatik Öneri desteklemez. 

## <a name="next-steps"></a>Sonraki adımlar
- [Sorgu ve yanıt](local-search-query-response.md)
- [Yerel Iş araması hızlı başlangıç](quickstarts/local-quickstart.md)
- [Yerel İşletme Arama API'si başvurusu](local-search-reference.md)
- [Kullanım ve görüntüleme gereksinimleri](use-display-requirements.md)
