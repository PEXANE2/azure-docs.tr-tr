---
title: Azure Ön Kapı - URL Yönlendirme | Microsoft Dokümanlar
description: Bu makale, Azure Ön Kapı'nın yapılandırıldırıldığında rotaları için URL yeniden yönlendirmeyi nasıl desteklediğini anlamanıza yardımcı olur.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/08/2019
ms.author: sharadag
ms.openlocfilehash: 5e3e44c4aee84fe9e2e21174a1d65fdf26b765a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295469"
---
# <a name="url-redirect"></a>URL yeniden yönlendirme
Trafiği yeniden yönlendirmek için Azure Ön Kapı'yı kullanabilirsiniz. Trafiği birden çok düzeyde (iletişim kuralı, ana bilgisayar adı, yol, sorgu dizesi) yeniden yönlendirebilirsiniz ve yeniden yönlendirme yol tabanlı olduğundan tüm işlevler tek tek mikro hizmetler için yapılandırılabilir. Bu, uygulama yapılandırmasını basitleştirir, kaynak kullanımını en iyi duruma getirtir ve genel ve yol tabanlı yeniden yönlendirme de dahil olmak üzere yeni yeniden yönlendirme senaryolarını destekler.
</br>

![Azure Ön Kapı URL Yönlendirme][1]

## <a name="redirection-types"></a>Yeniden yönlendirme türleri
Yeniden yönlendirme türü, istemcilerin yönlendirmenin amacını anlaması için yanıt durum kodunu ayarlar. Aşağıdaki yeniden yönlendirme türleri desteklenir:

- **301 (Kalıcı olarak taşınır)**: Hedef kaynağa yeni bir kalıcı URI atandığını ve bu kaynağa gelecekteki başvuruların ekteki URL'lerden birini kullanması gerektiğini gösterir. HTTP için HTTPS yeniden yönlendirme için 301 durum kodunu kullanın. 
- **302 (Bulundu)**: Hedef kaynağın geçici olarak farklı bir URI altında bulunduğunu gösterir. Yeniden yönlendirme zaman zaman değiştirilebileceğinden, istemci gelecekteki istekler için etkili istek URI'yi kullanmaya devam etmeli.
- **307 (Geçici yönlendirme)**: Hedef kaynağın geçici olarak farklı bir URI altında bulunduğunu ve kullanıcı aracısının, bu URI'ye otomatik yönlendirme gerçekleştirirse istek yöntemini değiştirmemesi gerektiğini gösterir. Yeniden yönlendirme zaman içinde değişebildiği için, istemci gelecekteki istekler için özgün etkili istek URI'yi kullanmaya devam etmeli.
- **308 (Kalıcı yönlendirme)**: Hedef kaynağa yeni bir kalıcı URI atandığını ve bu kaynağa gelecekteki başvuruların ekteki URL'lerden birini kullanması gerektiğini gösterir. Bağlantı düzenleme özelliklerine sahip istemciler, etkili istek URI'ye yapılan başvuruları mümkün olduğunca sunucu tarafından gönderilen yeni başvurulardan birine veya daha fazlasına otomatik olarak yeniden bağlamalı.

## <a name="redirection-protocol"></a>Yeniden yönlendirme protokolü
Yeniden yönlendirme için kullanılacak protokolü ayarlayabilirsiniz. Bu, HTTP'yi HTTPS yeniden yönlendirmesine ayarlamak için en yaygın yeniden yönlendirme özelliği nin en yaygın kullanım örneklerinden birine olanak tanır.

- **Yalnızca HTTPS**: Trafiği HTTP'den HTTPS'ye yönlendirmek istiyorsanız protokolü yalnızca HTTPS olarak ayarlayın. Azure Ön Kapı, yeniden yönlendirmeyi her zaman yalnızca HTTPS'ye ayarlamanızı önerir.
- **Yalnızca HTTP**: Bu, gelen isteği HTTP'ye yönlendirir. Bu değeri yalnızca trafiğinizi şifrelemesiz http tutmak istiyorsanız kullanın.
- **Maç isteği**: Bu seçenek, gelen istek tarafından kullanılan protokolü korur. Bu nedenle, bir HTTP isteği HTTP olarak kalır ve bir HTTPS isteği https sonrası yeniden yönlendirme kalır.

## <a name="destination-host"></a>Hedef ana bilgisayar
Yeniden yönlendirme yapılandırmanın bir parçası olarak, yeniden yönlendirme isteğiiçin ana bilgisayar adını veya etki alanını da değiştirebilirsiniz. Bu alanı, yeniden yönlendirme için URL'deki ana bilgisayar adını değiştirecek veya gelen istekteki ana bilgisayar adını koruyacak şekilde ayarlayabilirsiniz. Bu nedenle, bu alanı kullanarak gönderilen `https://www.contoso.com/*` tüm `https://www.fabrikam.com/*`istekleri yeniden yönlendirebilirsiniz.

## <a name="destination-path"></a>Hedef yol
Yeniden yönlendirmenin bir parçası olarak bir URL'nin yol kesimini değiştirmek istediğiniz durumlar için, bu alanı yeni yol değeriyle ayarlayabilirsiniz. Aksi takdirde, yeniden yönlendirmenin bir parçası olarak yol değerini korumayı seçebilirsiniz. Bu nedenle, bu alanı kullanarak, gönderilen `https://www.contoso.com/\*` tüm `https://www.contoso.com/redirected-site`istekleri ' ye yönlendirebilirsiniz.

## <a name="query-string-parameters"></a>Sorgu dize parametreleri
Yeniden yönlendirilen URL'deki sorgu dize parametrelerini de değiştirebilirsiniz. Gelen istek URL'sinden varolan sorgu dizesini değiştirmek için bu alanı 'Değiştir' olarak ayarlayın ve ardından uygun değeri ayarlayın. Aksi takdirde, alanı 'Koru' olarak ayarlayarak özgün sorgu dizeleri kümesini koruyabilirsiniz. Örnek olarak, bu alanı kullanarak, gönderilen `https://www.contoso.com/foo/bar` tüm trafiği `https://www.contoso.com/foo/bar?&utm_referrer=https%3A%2F%2Fwww.bing.com%2F`. 

## <a name="destination-fragment"></a>Hedef parçası
Hedef parça, URL'nin normalde tarayıcılar tarafından bir sayfadaki belirli bir bölüme inmek için kullanılan '#'dan sonraki bölümüdür. Bu alanı yeniden yönlendirme URL'sine bir parça ekleyecek şekilde ayarlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.

<!--Image references-->
[1]: ./media/front-door-url-redirect/front-door-url-redirect.png