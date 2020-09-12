---
title: Azure ön kapısı-URL yeniden yönlendirme | Microsoft Docs
description: Bu makale, yapılandırıldıysa, Azure ön kapısının rotalar için URL yeniden yönlendirmeyi nasıl desteklediğini anlamanıza yardımcı olur.
services: front-door
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/08/2019
ms.author: duau
ms.openlocfilehash: 41cb2343cb86d2ec756bb0a2fb690b7df886024f
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89399047"
---
# <a name="url-redirect"></a>URL yeniden yönlendirme
Trafiği yeniden yönlendirmek için Azure ön kapısına yararlanabilirsiniz. Trafiği birden çok düzeyde (protokol, ana bilgisayar adı, yol, sorgu dizesi) yeniden yönlendirebilir ve yeniden yönlendirme yol tabanlı olduğundan bağımsız mikro hizmetler için tüm işlevleri yapılandırabilirsiniz. Bu, uygulama yapılandırmasını basitleştirir, kaynak kullanımını iyileştirir ve genel ve yol tabanlı yeniden yönlendirme dahil yeni yeniden yönlendirme senaryolarını destekler.
</br>

![Azure ön kapı URL 'SI yönlendirmesi][1]

## <a name="redirection-types"></a>Yeniden yönlendirme türleri
Yeniden yönlendirme türü, istemcilerin yeniden yönlendirmenin amacını anlaması için yanıt durum kodunu ayarlar. Aşağıdaki yeniden yönlendirme türleri desteklenir:

- **301 (kalıcı olarak taşındı)**: hedef kaynağa yeni BIR kalıcı URI atandığını ve bu kaynağa gelecek tüm başvuruları, iliştirilmiş URI 'lerden birini kullanmak üzere atandığını gösterir. HTTP-HTTPS yönlendirmesi için 301 durum kodu kullanın. 
- **302 (bulunan)**: hedef kaynağın geçici olarak farklı bir URI altında bulunduğunu gösterir. Yeniden yönlendirme, zaman içinde değiştirilediğinden, istemci gelecekteki istekler için geçerli istek URI 'sini kullanmaya devam etmeyi bir kez daha ertir.
- **307 (geçici yeniden yönlendirme)**: hedef kaynağın geçici olarak farklı bir URI altında yer aldığını ve bu URI 'ye otomatik yeniden yönlendirme gerçekleştiriyorsa Kullanıcı aracısının istek YÖNTEMINI değiştirmemelidir. Yeniden yönlendirme zaman içinde değişebileceğinizden, istemci, gelecekteki istekler için özgün etkin istek URI 'sini kullanmaya devam etmeyi uygun şekilde gösterir.
- **308 (kalıcı yeniden yönlendirme)**: hedef kaynağa yeni BIR kalıcı URI atandığını ve bu kaynağa gelecek tüm başvuruları, iliştirilmiş URI 'lerden birini kullanmak üzere atandığını gösterir. Bağlantı düzenlemesi özelliklerine sahip istemciler, etkin istek URI 'sine yapılan başvuruları, sunucu tarafından gönderilen yeni başvuruların bir veya daha fazlasına otomatik olarak yeniden bağlamayı (mümkün olduğunda) sağlar.

## <a name="redirection-protocol"></a>Yeniden yönlendirme protokolü
Yönlendirme için kullanılacak protokolü ayarlayabilirsiniz. Bu, yeniden yönlendirme özelliğinin en yaygın kullanım çalışmalarından birine izin verir, yani HTTP olarak HTTPS yönlendirmesi ayarlanır.

- **Yalnızca https**: trafiği http 'den https 'ye yönlendirmek istiyorsanız, protokolü yalnızca https olarak ayarlayın. Azure ön kapısının her zaman yeniden yönlendirmeyi yalnızca HTTPS olarak ayarlamanız önerilir.
- **Yalnızca http**: Bu, gelen isteği http 'ye yönlendirir. Bu değeri yalnızca, trafik HTTP 'nizi, şifreli olmayan HTTP 'yi tutmak istiyorsanız kullanın.
- **Eşleştirme isteği**: Bu seçenek, gelen istek tarafından kullanılan protokolü korur. Bu nedenle, HTTP isteği HTTP olarak kalır ve HTTPS isteği HTTPS Post yeniden yönlendirmesi kalır.

## <a name="destination-host"></a>Hedef konak
Yeniden yönlendirme yönlendirmesi yapılandırmanın bir parçası olarak, yeniden yönlendirme isteği için ana bilgisayar adı veya etki alanını da değiştirebilirsiniz. Bu alanı, yeniden yönlendirmenin URL 'sindeki ana bilgisayar adını değiştirmek ya da başka bir şekilde gelen istekten ana bilgisayar adını korumak için ayarlayabilirsiniz. Bu nedenle, bu alanı kullanarak üzerinde gönderilen tüm istekleri öğesine yeniden `https://www.contoso.com/*` yönlendirebilirsiniz `https://www.fabrikam.com/*` .

## <a name="destination-path"></a>Hedef yol
Yeniden yönlendirmenin bir parçası olarak bir URL 'nin yol segmentini değiştirmek istediğiniz durumlarda, bu alanı yeni yol değeri ile ayarlayabilirsiniz. Aksi takdirde, yol değerini yeniden yönlendirmenin bir parçası olarak korumayı seçebilirsiniz. Bu nedenle, bu alanı kullanarak öğesine gönderilen tüm istekleri öğesine yeniden yönlendirebilirsiniz `https://www.contoso.com/\*`  `https://www.contoso.com/redirected-site` .

## <a name="query-string-parameters"></a>Sorgu dizesi parametreleri
Ayrıca, yeniden yönlendirilen URL 'deki sorgu dizesi parametrelerini de değiştirebilirsiniz. Gelen istek URL 'sindeki mevcut Sorgu dizelerini değiştirmek için, bu alanı ' replace ' olarak ayarlayın ve uygun değeri ayarlayın. Aksi halde, alanı ' koru ' olarak ayarlayarak özgün sorgu dizeleri kümesini koruyabilirsiniz. Örnek olarak, bu alanı kullanarak öğesine gönderilen tüm trafiği öğesine yeniden yönlendirebilirsiniz `https://www.contoso.com/foo/bar` `https://www.contoso.com/foo/bar?&utm_referrer=https%3A%2F%2Fwww.bing.com%2F` . 

## <a name="destination-fragment"></a>Hedef parça
Hedef parça, genellikle tarayıcılar tarafından bir sayfadaki belirli bir bölüme kara olarak kullanılan ' # ' öğesinden sonra URL 'nin bölümüdür. Bu alanı yeniden yönlendirme URL 'sine bir parça eklemek için ayarlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.

<!--Image references-->
[1]: ./media/front-door-url-redirect/front-door-url-redirect.png