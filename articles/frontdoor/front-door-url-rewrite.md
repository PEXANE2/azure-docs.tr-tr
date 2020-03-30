---
title: Azure Ön Kapı - URL Yeniden Yazma | Microsoft Dokümanlar
description: Bu makale, yapılandırıldığınızda Azure Ön Kapı'nın rotalarınız için URL'yi nasıl yeniden yazdığını anlamanıza yardımcı olur.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 1e5bd565be7a1cabf08ddf33c65eb12b5294249f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471481"
---
# <a name="url-rewrite-custom-forwarding-path"></a>URL yeniden yazma (özel yönlendirme yolu)
Azure Ön Kapı, isteği arka uca iletmek için yaparken kullanmak üzere isteğe bağlı **Özel Yönlendirme Yolunu** yapılandırmanıza izin vererek URL'nin yeniden yazmasını destekler. Özel yönlendirme yolu sağlanmazsa Front Door varsayılan olarak yönlendirilen istekte kullanılan URL alanına gelen URL yolunu kopyalar. Yönlendirilen istekte kullanılan Konak üst bilgisi seçilen arka uç için yapılandırılmış olan değerdir. Ne yaptığını ve nasıl yapılandırabileceğinizi öğrenmek için [Arka Uç Ana Bilgisayar Üstbilgisini](front-door-backend-pool.md#hostheader) okuyun.

URL'nin özel iletme yolunu kullanarak yeniden yazmanın güçlü bir bölümü, gelen yolun iletilen yola joker karakter yoluna eşleşen herhangi bir bölümünü kopyalayacak olmasıdır (bu yol segmentleri aşağıdaki örnekte **yeşil** segmentlerdir):
</br>
![Azure Ön Kapı URL'si Yeniden Yaz][1]

## <a name="url-rewrite-example"></a>URL yeniden yazma örneği
Aşağıdaki ön uç ana bilgisayarları ve yapılandırılan yollar ile bir yönlendirme kuralı düşünün:

| Ana bilgisayarlar      | Yollar       |
|------------|-------------|
| www\.contoso.com | /\*         |
|            | /foo        |
|            | /foo/\*     |
|            | /foo/bar/\* |

Aşağıdaki tablonun ilk sütununda gelen istek örnekleri, ikinci sütunda ise "en özel" eşleşen rota 'Yol' ne olacağı gösterilmektedir.  Tablonun ilk satırının üçüncü ve sonraki sütunları, bu satırdaki istekle eşleşirse iletilen istek yolunun ne olacağını gösteren örnekleri temsil eden bu sütunlarda kalan satırlarla yapılandırılan **Özel İletme Yolları'na**örnektir.

Örneğin, ikinci satırboyunca okursak, gelen istek `www.contoso.com/sub`için , özel iletme yolu ise, `/`iletilen yolun . `/sub` Özel yönlendirme yolu ise, `/fwd/`o zaman iletilen `/fwd/sub`yol olacaktır. Ve saire, kalan sütunlar için. Aşağıdaki yolların **vurgulanan** bölümleri joker karakter eşleşmesinin bir parçası olan bölümleri temsil eder.


| Gelen istek       | En özel maç yolu | /          | /fwd/          | /foo/          | /foo/bar/          |
|------------------------|--------------------------|------------|----------------|----------------|--------------------|
| www\.contoso.com/            | /\*                      | /          | /fwd/          | /foo/          | /foo/bar/          |
| www\.contoso.com/**alt**     | /\*                      | /**Alt**   | /fwd/**alt**   | /foo/**alt**   | /foo/bar/**alt**   |
| www\.contoso.com/**a/b/c**   | /\*                      | /**a/b/c** | /fwd/**a/b/c** | /foo/**a/b/c** | /foo/bar/**a/b/c** |
| www\.contoso.com/foo         | /foo                     | /          | /fwd/          | /foo/          | /foo/bar/          |
| www\.contoso.com/foo/        | /foo/\*                  | /          | /fwd/          | /foo/          | /foo/bar/          |
| www\.contoso.com/foo/**bar** | /foo/\*                  | /**Bar**   | /fwd/**bar**   | /foo/**bar**   | /foo/bar/**bar**   |


## <a name="optional-settings"></a>İsteğe bağlı ayarlar
Verilen yönlendirme kuralı ayarları için de belirtebileceğiniz ek isteğe bağlı ayarlar vardır:

* **Önbellek Yapılandırması** - Devre dışı bırakılmış veya belirtilmemişse, bu yönlendirme kuralıyla eşleşen istekler önbelleğe alınmış içeriği kullanmaya çalışmaz ve bunun yerine her zaman arka uçtan getirir. [Ön Kapı ile Önbelleğe Alma](front-door-caching.md)hakkında daha fazla bilgi edinin.



## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.

<!--Image references-->
[1]: ./media/front-door-url-rewrite/front-door-url-rewrite-example.jpg