---
title: Azure Ön Kapı'daki HTTP üstbilgiiçin protokol desteği | Microsoft Dokümanlar
description: Bu makalede, Ön Kapı'nın desteklediği HTTP üstbilgi iletişim protokolleri açıklanmaktadır.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: bb1de5d51afd01cf0aa519f12aa3665bee804efd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471685"
---
# <a name="protocol-support-for-http-headers-in-azure-front-door"></a>Azure Ön Kapı'da HTTP üstbilgiiçin protokol desteği
Bu makalede, Ön Kapı'nın çağrı yolunun parçalarıyla desteklediği protokol özetlenmiştir (resme bakın). Aşağıdaki bölümler, Front Door tarafından desteklenen HTTP üstbilgileri hakkında daha fazla bilgi sağlar.

![Azure Ön Kapı HTTP üstbilgi protokolü][1]

>[!IMPORTANT]
>Ön Kapı, burada belgelenmemiş herhangi bir HTTP üstbilginin onaylanmıyor.

## <a name="client-to-front-door"></a>Müşteriden Ön Kapıya
Ön Kapı, gelen istekteki üstbilgileri değiştirmeden kabul eder. X-FD-* önekine sahip üstbilgi de dahil olmak üzere, gönderilen bazı ayrılmış üstbilgi gelen istekten kaldırılır.

## <a name="front-door-to-backend"></a>Arka uçiçin Ön Kapı

Ön Kapı, kısıtlamalar nedeniyle kaldırılmadığı sürece gelen bir istekteki üstbilgi içerir. Ön Kapı da aşağıdaki üstbilgi ekler:

| Üst bilgi  | Örnek ve açıklama |
| ------------- | ------------- |
| Via |  Via: 1.1 Azure </br> Front Door, istemcinin HTTP sürümünü ve ardından *Azure'u* Via üstbilgisinin değeri olarak ekler. Bu üstbilgi istemcinin HTTP sürümünü gösterir ve Ön Kapı istemci ve arka uç arasındaki istek için bir ara alıcı olduğunu.  |
| X-Azure-ClientIP | X-Azure-ClientIP: 127.0.0.1 </br> İşlenen istekle ilişkili istemci IP adresini temsil eder. Örneğin, bir proxy'den gelen bir istek, özgün arayanın IP adresini belirtmek için X-Forwarded-For üstbilgisini ekleyebilir. |
| X-Azure-SocketIP |  X-Azure-SocketIP: 127.0.0.1 </br> Geçerli isteğin kaynağı olan TCP bağlantısıyla ilişkili soket IP adresini temsil eder. Bir isteğin istemci IP adresi, kullanıcı tarafından rasgele üzerine yazılabileceğinden, soket IP adresine eşit olmayabilir.|
| X-Azure-Ref |  X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTKANDM3YzgyY2QtMzYWYS00YTU0LTk0YzMtNWZMNNJQNzngn </br> Front Door tarafından sunulan bir isteği tanımlayan benzersiz bir başvuru dizesi. Erişim günlüklerinde arama yapmak için kullanılır ve sorun giderme için kritik öneme yöneliktir.|
| X-Azure İstek Zinciri |  X-Azure-RequestChain: atlama=1 </br> Ön Kapı istek döngüleri algılamak için kullandığı bir üstbilgi ve kullanıcılar ona bir bağımlılık almamalıdır. |
| X-Forwarded-For | X-Forwarded-For: 127.0.0.1 </br> X-Forwarded-For (XFF) HTTP üstbilgi alanı genellikle bir HTTP proxy veya yük dengeleyici sağlayan bir web sunucusuna bağlanan bir istemcinin kaynaklanan IP adresini tanımlar. Varolan bir XFF üstbilgisi varsa, Ön Kapı istemci soketIP'ini ona ekler veya istemci soketi IP'sine XFF üstbilgisini ekler. |
| X-Forwarded-Host | X-Forwarded-Host: contoso.azurefd.net </br> X-Forwarded-Host HTTP üstbilgi alanı, Ana Bilgisayar HTTP istek üstbilgisinde istemci tarafından istenen özgün ana bilgisayarı tanımlamak için kullanılan yaygın bir yöntemdir. Bunun nedeni, Ön Kapı'daki ana bilgisayar adının isteği işleyen arka uç sunucusu için farklılık gösterebilir olmasıdır. |
| X-İlli-Proto | X-Forwarded-Proto: http </br> X-Forwarded-Proto HTTP üstbilgi alanı genellikle bir HTTP isteğinin kaynaklanan protokolünü tanımlamak için kullanılır, çünkü Konfigürasyona dayalı Ön Kapı, HTTPS kullanarak arka uçla iletişim kurabilir. Ters proxy isteği HTTP olsa bile bu doğrudur. |
| X-FD-HealthProbe | X-FD-HealthProbe HTTP başlık alanı Ön Kapı'daki sağlık sondasını tanımlamak için kullanılır. Bu üstbilgi 1 olarak ayarlanmışsa, istek sağlık sondasıdır. X-Forwarded-Host üstbilgi alanına sahip belirli Ön Kapıdan katı erişim istediğinizde kullanabilirsiniz. |

## <a name="front-door-to-client"></a>Müşteriye Ön Kapı

Arka uçtan Ön Kapı'ya gönderilen başlıklar da istemciye iletilir. Aşağıdaki başlıklar Ön Kapı istemcilere gönderilir.

| Üst bilgi  | Örnek |
| ------------- | ------------- |
| X-Azure-Ref |  *X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTKANDM3YzgyY2QtMzYWYS00YTU0LTk0YzMtNWZMNNJQNzngn* </br> Bu, Front Door tarafından sunulan bir isteği tanımlayan benzersiz bir başvuru dizesidir. Bu, erişim günlüklerinde arama yapmak için kullanıldığından sorun giderme için çok önemlidir.|

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturma](quickstart-create-front-door.md)
- [Ön Kapı nasıl çalışır?](front-door-routing-architecture.md)

<!--Image references-->
[1]: ./media/front-door-http-headers-protocol/front-door-protocol-summary.png
