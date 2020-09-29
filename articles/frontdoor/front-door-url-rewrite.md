---
title: Azure ön kapısı-URL yeniden yazma | Microsoft Docs
description: Bu makale, yapılandırıldıysa Azure ön kapısının, yollarınız için URL yeniden yazma şeklini anlamanıza yardımcı olur.
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
ms.openlocfilehash: eb5b4ab8a23a374aec54d65dd5390ab3fec3e905
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91445483"
---
# <a name="url-rewrite-custom-forwarding-path"></a>URL yeniden yazma (özel iletme yolu)
Azure ön kapısı, arka uca iletme isteği oluşturulurken kullanılacak isteğe bağlı bir **özel Iletme yolu** yapılandırarak URL yeniden yazmayı destekler. Varsayılan olarak, özel bir iletme yolu sağlanmazsa, ön kapı, gelen URL yolunu iletilen istekte kullanılan URL 'ye kopyalar. Yönlendirilen istekte kullanılan Konak üst bilgisi seçilen arka uç için yapılandırılmış olan değerdir. Ne yaptığını ve nasıl yapılandırabileceğinizi öğrenmek için [arka uç ana bilgisayar üst bilgisini](front-door-backend-pool.md#hostheader) okuyun.

URL yeniden yazma işleminin güçlü bölümü, özel iletme yolunun, gelen yolun bir bölümünü iletilen yola bir joker karakterle (Bu yol kesimleri aşağıdaki örnekteki **yeşil** kesimlerdir) kopyalayacaktır:
</br>

:::image type="content" source="./media/front-door-url-rewrite/front-door-url-rewrite-example.jpg" alt-text="Azure ön kapı URL 'SI yeniden yazma":::

## <a name="url-rewrite-example"></a>URL yeniden yazma örneği
Aşağıdaki ön uç konakları ve yapılandırılmış yolların birleşimini içeren bir yönlendirme kuralı düşünün:

| Ana bilgisayarlar      | Yollar       |
|------------|-------------|
| www \. contoso.com | /\*   |
|            | /foo        |
|            | foo\*     |
|            | /ç/Bar/\* |

Aşağıdaki tablonun ilk sütununda gelen istek örnekleri gösterilmektedir ve ikinci sütun "en özel" eşleşen yol ' Path ' olarak gösterilir.  Tablonun üçüncü ve diğer sütunları yapılandırılmış **özel Iletme yollarının**örnekleridir.

Örneğin, ikinci satırda okuduk, gelen istek için `www.contoso.com/sub` özel iletme yolu ise, iletilen yolun ne olduğunu söyledi `/` `/sub` . Özel iletme yolu `/fwd/` olsaydı, iletilen yol olur `/fwd/sub` . Daha sonra, kalan sütunlar için. Aşağıdaki yolların **vurgulanmış** kısımları, joker karakter eşleşmesi kapsamında olan bölümleri temsil eder.

| Gelen istek       | En özel eşleşme yolu | /          | /FWD/          | foo          | /ç/Bar/          |
|------------------------|--------------------------|------------|----------------|----------------|--------------------|
| www \. contoso.com/            | /\*                      | /          | /FWD/          | foo          | /ç/Bar/          |
| www \. contoso.com/**Sub**     | /\*                      | /**alt**   | /FWD/**Sub**   | **/alt/alt**   | /alt/bar/**alt**   |
| www \. contoso.com/**a/b/c**   | /\*                      | /**a/b/c** | /FWD/**a/b/c** | /k/**a/b/c** | /ç/Bar/**a/b/c** |
| www \. contoso.com/foo         | /foo                     | /          | /FWD/          | foo          | /ç/Bar/          |
| www \. contoso.com/foo/        | foo\*                  | /          | /FWD/          | foo          | /ç/Bar/          |
| www \. contoso.com/foo/**çubuğu** | foo\*                  | /**çubuktaki**   | /FWD/**Bar**   | /Altbilgi/**çubuk**   | /ç/Bar/**çubuk**   |

## <a name="optional-settings"></a>İsteğe bağlı ayarlar
Belirli bir yönlendirme kuralı ayarları için de belirtebileceğiniz ek isteğe bağlı ayarlar vardır:

* **Önbellek yapılandırması** -devre dışı bırakılırsa veya belirtilmemişse, bu yönlendirme kuralıyla eşleşen istekler önbelleğe alınmış içeriği kullanmaya çalışmaz ve bunun yerine arka uca her zaman getirir. [Ön kapılı önbelleğe alma](front-door-caching.md)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
