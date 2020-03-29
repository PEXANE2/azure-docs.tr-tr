---
title: Sık sorulan sorular (SSS) - Bing Resim Arama API
titleSuffix: Azure Cognitive Services
description: Bing Resim Arama API'si ile ilgili kavramlar, kod ve senaryolarla ilgili sık sorulan soruların yanıtlarını bulun.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: 6841e573446103466e2719797da9e4161b70b5a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68881693"
---
# <a name="frequently-asked-questions-faq-about-the-bing-image-search-api"></a>Bing Resim Arama API'si hakkında sık sorulan sorular (SSS)

Azure'da Microsoft Bilişsel Hizmetler için Bing Resim Arama API'si ile ilgili kavramlar, kod lar ve senaryolarla ilgili sık sorulan soruların yanıtlarını bulun.

## <a name="response-headers-in-javascript"></a>JavaScript'teki yanıt üstbilgi

Bing Resim Arama API'sindeki yanıtlarda aşağıdaki üstbilgi oluşabilir.

| `Attribute`         | `Description` |
| ------------------- | ------------- |
| `X-MSEdge-ClientID` |Bing'in kullanıcıya atadığı benzersiz kimlik |
| `BingAPIs-Market`   |Talebi yerine getirmek için kullanılan pazar |
| `BingAPIs-TraceId`  |Bu istek için Bing API sunucusundaki günlük girişi (destek için) |

İstemci kimliğini sürdürmek ve sonraki isteklerle iade etmek özellikle önemlidir. Bunu yaptığınızda, arama, sıralama arama sonuçlarında geçmiş bağlamı kullanır ve tutarlı bir kullanıcı deneyimi sağlar.

Ancak, JavaScript'ten Bing Resim Arama API'sini aradığınızda, tarayıcınızın yerleşik güvenlik özellikleri (CORS) bu üstbilgi değerlerine erişmenizi engelleyebilir.

Üstbilgiye erişmek için, Bing Resim Arama API isteğini BIR CORS proxy'si aracılığıyla yapabilirsiniz. Böyle bir ara sunucudan gelen yanıtta, yanıt üst bilgilerini beyaz listeye alan ve JavaScript’in kullanımına sunan `Access-Control-Expose-Headers` üst bilgisi bulunur.

[Öğretici uygulamamızın](tutorial-bing-image-search-single-page-app.md) isteğe bağlı istemci üstbilgilerine erişmesine izin vermek için BIR CORS proxy'si yüklemek kolaydır. İlk olarak, henüz yüklemediyseniz [Node.js'yi yükleyin](https://nodejs.org/en/download/). Ardından komut istemiyle aşağıdaki komutu girin.

    npm install -g cors-proxy-server

Ardından, HTML dosyasındaki Bing Resim Arama API bitiş noktasını şu şekilde değiştirin:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Son olarak, aşağıdaki komutla CORS ara sunucusunu başlatın:

    cors-proxy-server

Öğretici uygulamasını kullanırken komut penceresini açık bırakın; pencere kapatılırsa ara sunucu durdurulur. Arama sonuçlarının altındaki genişletilebilir HTTP Üst Bilgileri bölümünde artık `X-MSEdge-ClientID` üst bilgisini (diğerleriyle birlikte) görebilir ve bunun her istekte aynı olduğunu doğrulayabilirsiniz.

## <a name="response-headers-in-production"></a>Üretimdeki yanıt başlıkları

Önceki yanıtta açıklanan CORS proxy yaklaşımı geliştirme, test ve öğrenme için uygundur.

Ancak, üretim ortamında, Bing Web Arama API'sını kullanan Web sayfasıyla aynı etki alanında sunucu tarafında bir komut dosyası barındırmanız gerekir. Bu komut dosyası aslında Web sayfası JavaScript istek üzerine API çağrıları yapmak ve üstbilgi de dahil olmak üzere tüm sonuçları, istemciye geri geçmek gerekir. İki kaynak (sayfa ve komut dosyası) bir kaynağı paylaştığından, CORS devreye girmez ve özel üstbilgiler Web sayfasındaki JavaScript'e erişebilir.

Bu yaklaşım, API anahtarınızı yalnızca sunucu tarafındaki komut dosyasının buna ihtiyacı olduğundan, herkese açık hale gelmekten de korur. Komut dosyası, isteğin yetkilendirildiğinden emin olmak için başka bir yöntem (HTTP referrer gibi) kullanabilir.

## <a name="next-steps"></a>Sonraki adımlar

Sorunuz eksik bir özellik veya işlevsellik hakkında mı? [Kullanıcı Sesi web sitemizde](https://cognitive.uservoice.com/forums/555907-bing-search)talep etmeyi veya oy vermeyi düşünün.

## <a name="see-also"></a>Ayrıca bkz.

 [Yığın Taşma: Bilişsel Hizmetler](https://stackoverflow.com/questions/tagged/bing-api)
