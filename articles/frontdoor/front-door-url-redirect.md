---
title: Azure ön kapısı-URL yeniden yönlendirme | Microsoft Docs
description: Bu makale, Azure ön kapısının yönlendirme kuralları için URL yeniden yönlendirmeyi nasıl desteklediğini anlamanıza yardımcı olur.
services: front-door
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/28/2020
ms.author: duau
ms.openlocfilehash: 61077c7900530fd4c5be64054bedd9c5d087fe77
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91442053"
---
# <a name="url-redirect"></a>URL yeniden yönlendirme
Azure ön kapısı şu düzeylerin her birinde trafiği yeniden yönlendirebilir: protokol, konak adı, yol, sorgu dizesi. Bu işlevler, yeniden yönlendirme yol tabanlı olduğundan bağımsız mikro hizmetler için yapılandırılabilir. Bu, kaynak kullanımını iyileştirerek uygulama yapılandırmasını kolaylaştırabilir ve genel ve yol tabanlı yeniden yönlendirme dahil yeni yeniden yönlendirme senaryolarını destekler.
</br>

:::image type="content" source="./media/front-door-url-redirect/front-door-url-redirect.png" alt-text="Azure ön kapı URL 'SI yönlendirmesi":::

## <a name="redirection-types"></a>Yeniden yönlendirme türleri
Yeniden yönlendirme türü, istemcilerin yeniden yönlendirmenin amacını anlaması için yanıt durum kodunu ayarlar. Aşağıdaki yeniden yönlendirme türleri desteklenir:

- **301 (kalıcı olarak taşındı)**: hedef kaynağa yeni BIR kalıcı URI atandığını gösterir. Bu kaynağa gelecek tüm başvurular, eklenen URI 'lerden birini kullanacaktır. HTTP-HTTPS yönlendirmesi için 301 durum kodu kullanın. 
- **302 (bulunan)**: hedef kaynağın geçici olarak farklı bir URI altında olduğunu gösterir. Yeniden yönlendirme bazen değişebileceğinizden, istemci gelecekteki istekler için geçerli istek URI 'sini kullanmaya devam etmelidir.
- **307 (geçici yeniden yönlendirme)**: hedef kaynağın geçici olarak farklı bir URI altında olduğunu gösterir. Kullanıcı Aracısı, bu URI 'ye otomatik yeniden yönlendirme yaparsanız istek yöntemini değiştirmemelidir. Yeniden yönlendirme zaman içinde değişebileceğinizden, istemci, gelecekteki istekler için özgün etkin istek URI 'sini kullanmaya devam etmeyi uygun şekilde gösterir.
- **308 (kalıcı yeniden yönlendirme)**: hedef kaynağa yeni BIR kalıcı URI atandığını gösterir. Bu kaynağa gelecek tüm başvurular, iliştirilmiş URI 'lerden birini kullanmalıdır.

## <a name="redirection-protocol"></a>Yeniden yönlendirme protokolü
Yönlendirme için kullanılacak protokolü ayarlayabilirsiniz. Yeniden yönlendirme özelliğinin en yaygın kullanım durumları HTTP olarak HTTPS yönlendirmesi olarak ayarlanmıştır.

- **Yalnızca https**: trafiği http 'den https 'ye yönlendirmek istiyorsanız, protokolü yalnızca https olarak ayarlayın. Azure ön kapısının her zaman yeniden yönlendirmeyi yalnızca HTTPS olarak ayarlamanız önerilir.
- **Yalnızca http**: gelen isteği http 'ye yönlendirir. Bu değeri yalnızca, trafik HTTP 'nizi, şifreli olmayan HTTP 'yi tutmak istiyorsanız kullanın.
- **Eşleştirme isteği**: Bu seçenek, gelen istek tarafından kullanılan protokolü korur. Bu nedenle, HTTP isteği HTTP olarak kalır ve HTTPS isteği HTTPS Post yeniden yönlendirmesi kalır.

## <a name="destination-host"></a>Hedef konak
Yeniden yönlendirme yönlendirmesi yapılandırmanın bir parçası olarak, yeniden yönlendirme isteği için ana bilgisayar adı veya etki alanını da değiştirebilirsiniz. Bu alanı, yeniden yönlendirmenin URL 'sindeki ana bilgisayar adını değiştirmek ya da başka bir şekilde gelen istekten ana bilgisayar adını korumak için ayarlayabilirsiniz. Bu nedenle, bu alanı kullanarak üzerinde gönderilen tüm istekleri öğesine yeniden `https://www.contoso.com/*` yönlendirebilirsiniz `https://www.fabrikam.com/*` .

## <a name="destination-path"></a>Hedef yol
Yeniden yönlendirmenin bir parçası olarak bir URL 'nin yol segmentini değiştirmek istediğiniz durumlarda, bu alanı yeni yol değeri ile ayarlayabilirsiniz. Aksi takdirde, yol değerini yeniden yönlendirmenin bir parçası olarak korumayı seçebilirsiniz. Bu nedenle, bu alanı kullanarak öğesine gönderilen tüm istekleri öğesine yeniden yönlendirebilirsiniz `https://www.contoso.com/\*`  `https://www.contoso.com/redirected-site` .

## <a name="query-string-parameters"></a>Sorgu dizesi parametreleri
Ayrıca, yeniden yönlendirilen URL 'deki sorgu dizesi parametrelerini de değiştirebilirsiniz. Gelen istek URL 'sindeki mevcut Sorgu dizelerini değiştirmek için, bu alanı ' replace ' olarak ayarlayın ve uygun değeri ayarlayın. Aksi halde, alanı ' preserve ' olarak ayarlayarak özgün sorgu dizeleri kümesini tutabilirsiniz. Örnek olarak, bu alanı kullanarak öğesine gönderilen tüm trafiği öğesine yeniden yönlendirebilirsiniz `https://www.contoso.com/foo/bar` `https://www.contoso.com/foo/bar?&utm_referrer=https%3A%2F%2Fwww.bing.com%2F` . 

## <a name="destination-fragment"></a>Hedef parça
Hedef parça, Web sayfasının belirli bir bölümüne gitmek için tarayıcı tarafından kullanılan ' # ' öğesinden sonra URL 'nin bölümüdür. Bu alanı yeniden yönlendirme URL 'sine bir parça eklemek için ayarlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
