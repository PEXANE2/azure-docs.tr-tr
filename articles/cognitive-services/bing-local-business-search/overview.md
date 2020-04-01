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
ms.openlocfilehash: 0006761126eb1d561da7eeff97e8a9928d62ddb0
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478943"
---
# <a name="what-is-bing-local-business-search"></a>Bing Yerel İş Araması nedir?
Bing Yerel İş Arama API' si, uygulamalarınızın arama sorgularına dayalı olarak yerel işletmeler hakkında bilgi bulmasını sağlayan yeni bir hizmettir. Örneğin, `q=<business-name> in Redmond, Washington`, `q=Italian restaurants near me`veya . 

## <a name="features"></a>Özellikler
| Özellik | Açıklama |  
| -- | -- | 
| [Yerel işletmeleri ve konumları bulun](quickstarts/local-quickstart.md) | Bing Yerel İşletme Arama API'si, bir sorgudan yerelleştirilmiş sonuçlar alır. Sonuçlar, işletmenin web sitesi için bir URL içerir ve aşağıdakiler dahil olmak üzere metin, telefon numarası ve coğrafi konumu görüntüler: GPS koordinatları, şehir, sokak adresi |  
| [Yerel sonuçları coğrafi sınırlarla filtreleme](specify-geographic-search.md) | Sonuçları dairesel bir alan veya kare sınırlayıcı kutu tarafından belirtilen belirli bir coğrafi alanla sınırlamak için arama parametreleri olarak koordinatlar ekleyin. | 
| [Yerel işletme sonuçlarını kategoriye göre filtreleme](local-categories.md) | Yerel işletme sonuçlarını kategoriye göre arayın. Bu seçenek, çeşitli iş kategorilerinde yerelleştirilmiş sonuçları döndürmek için arayanın ters IP konumunu veya GPS koordinatlarını kullanır.|

## <a name="workflow"></a>İş akışı
BING Yerel İş Arama API'sini HTTP isteklerini gerçekleştirebilen ve JSON yanıtlarını ayrışdırabilen herhangi bir programlama dilinden arayın. Bu hizmete REST API kullanılarak erişilebilir.
 
1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz [yoksa, ücretsiz bir hesap oluşturabilirsiniz.](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api)   
2. URL `q=""` sorgu parametresi için arama terimlerinizi kodlayın. Örneğin `q=nearby+restaurant` veya `q=nearby%20restaurant` olabilir. Gerekirse pagasyon da ayarlayın. 
3. Bing [Yerel İş Arama API'sine istek](quickstarts/local-quickstart.md) gönderme 
4. Ayrışdırış JSON yanıtı 

> [!NOTE]
> Şu anda, Yerel İş Arama: 
> * Sadece piyasayı `en-US` destekler. 
> * Bing Autosuggest'ı desteklemez. 

## <a name="next-steps"></a>Sonraki adımlar
- [Sorgu ve yanıt](local-search-query-response.md)
- [Yerel İş Arama sıcağınızda](quickstarts/local-quickstart.md)
- [Yerel İşletme Arama API'si başvurusu](local-search-reference.md)
- [Kullanım ve görüntüleme gereksinimleri](use-display-requirements.md)
