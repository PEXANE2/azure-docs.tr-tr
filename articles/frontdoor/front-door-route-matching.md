---
title: Azure ön kapı-yönlendirme kuralı eşleştirme izleme | Microsoft Docs
description: Bu makale, Azure ön kapısının gelen bir istek için hangi yönlendirme kuralına karşılık kullanılacağını anlamanıza yardımcı olur
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
ms.openlocfilehash: 67940db973f494cd4a12c2f16db528e0b113d656
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "91449212"
---
# <a name="how-requests-are-matched-to-a-routing-rule"></a>İsteklerin bir yönlendirme kuralıyla nasıl eşleştiği

Bir bağlantı kurulduktan ve bir TLS anlaşmasını tamamladıktan sonra, istek bir ön kapı ortamında olduğunda, ön kapıdaki ilk kapıların kendisi, istekle eşleşen belirli bir yönlendirme kuralını tespit etmek ve ardından yapılandırmada tanımlanan eylemi gerçekleştirmek için gerekli olan belirli bir yönlendirme kuralını belirlemektir. Aşağıdaki belge, bir HTTP isteğini işlerken ön kapısının hangi yol yapılandırmasını kullandığını belirler.

## <a name="structure-of-a-front-door-route-configuration"></a>Ön kapı yolu yapılandırması yapısı
Ön kapı yönlendirme kuralı yapılandırması iki ana bölümden oluşur: "sol taraf" ve "sağ taraf". Sağ taraftaki istek, isteği nasıl işleyeceğini tanımladığımızda, gelen istek, yolun sol tarafından eşleşir.

### <a name="incoming-match-left-hand-side"></a>Gelen eşleşme (sol taraftaki)
Aşağıdaki özellikler, gelen isteğin yönlendirme kuralıyla (veya Sol tarafla) eşleşip eşleşmediğini belirtir:

* **HTTP protokolleri** (http/https)
* **Konaklar** (örneğin, www \. foo.com, \* . Bar.com)
* **Yollar** (örneğin,/ \* ,/users/ \* ,/file.gif)

Bu özellikler dahili olarak genişletilir, böylece her protokol/ana bilgisayar/yol birleşimi potansiyel bir eşleşme kümesi olur.

### <a name="route-data-right-hand-side"></a>Verileri yönlendirme (sağ taraftaki)
İsteğin nasıl işleyebileceğine karar vermek, önbelleğe almanın, belirli bir yol için etkin olup olmamasına bağlıdır. Bu nedenle, istek için önbelleğe alınmış bir yanıt yoksa, isteği yapılandırılan arka uç havuzundaki uygun arka uca ileteceğiz.

## <a name="route-matching"></a>Rota eşleştirme
Bu bölüm, belirli bir ön kapı yönlendirme kuralıyla nasıl eşleştiğimiz üzerine odaklanacaktır. Temel kavram, her zaman yalnızca "sol tarafta" görünen **en özel eşleşme** ile eşleştiğimiz bir kavramdır.  Önce HTTP protokolüne, sonra ön uç konağına ve ardından yola göre eşleştik.

### <a name="frontend-host-matching"></a>Ön uç ana bilgisayar eşleştirme
Ön uç Konakları eşleştirilirken, aşağıda tanımlanan mantığı kullanıyoruz:

1. Konakta tam eşleşme olan herhangi bir yönlendirmeyi arayın.
2. Kesin ön uç Konakları eşleşmezse, isteği reddedin ve 400 Hatalı Istek hatası gönderin.

Bu işlemi daha ayrıntılı olarak açıklamak için ön kapı yollarının örnek yapılandırmasına bakalım (yalnızca sol taraftaki):

| Yönlendirme kuralı | Ön uç Konakları | Yol |
|-------|--------------------|-------|
| A | foo.contoso.com | /\* |
| B | foo.contoso.com | /Users/\* |
| C | www \. fabrikam.com, foo.Adventure-Works.com  | /\*,/images/\* |

Aşağıdaki gelen istekler ön kapıya gönderilmişse, yukarıdaki yönlendirme kurallarına göre eşleşmesi gerekir:

| Gelen ön uç Konağı | Eşleşen yönlendirme kuralları |
|---------------------|---------------|
| foo.contoso.com | A, B |
| www \. fabrikam.com | C |
| images.fabrikam.com | Hata 400: Hatalı Istek |
| foo.adventure-works.com | C |
| contoso.com | Hata 400: Hatalı Istek |
| www \. Adventure-Works.com | Hata 400: Hatalı Istek |
| www \. northwindtraders.com | Hata 400: Hatalı Istek |

### <a name="path-matching"></a>Yol eşleştirme
Belirli ön uç konağını belirledikten ve olası yönlendirme kurallarını yalnızca o ön uç konağına sahip yollara filtreledikten sonra, ön kapı, daha sonra yönlendirme kurallarını istek yoluna göre filtreler. Ön uç konakları olarak benzer bir mantık kullanıyoruz:

1. Yolda tam eşleşme olan herhangi bir yönlendirme kuralını arayın
2. Tam eşleşme yolu yoksa, eşleşen bir joker karakter yoluyla yönlendirme kuralları arayın
3. Eşleşen bir yol ile hiçbir yönlendirme kuralı bulunmazsa, isteği reddedin ve 400: Hatalı Istek hatası HTTP yanıtı döndürür.

>[!NOTE]
> Joker karakteri olmayan tüm yollar tam eşleşme yolları olarak kabul edilir. Yol, eğik çizgiyle bitse de tam eşleşme kabul edilir.

Daha ayrıntılı bir açıklama için diğer örnek kümesine göz atalım:

| Yönlendirme kuralı | Ön uç Konağı    | Yol     |
|-------|---------|----------|
| A     | www \. contoso.com | /        |
| B     | www \. contoso.com | /\*      |
| C     | www \. contoso.com | /AB      |
| D     | www \. contoso.com | /ABC     |
| E     | www \. contoso.com | ABC    |
| F     | www \. contoso.com | ABC\*  |
| G     | www \. contoso.com | /abc/def |
| H     | www \. contoso.com | /Path   |

Bu yapılandırma verildiğinde, aşağıdaki örnek eşleşen tablo neden olur:

| Gelen Istek    | Eşleşen yol |
|---------------------|---------------|
| www \. contoso.com/            | A             |
| www \. contoso.com/a           | B             |
| www \. contoso.com/AB          | C             |
| www \. contoso.com/ABC         | D             |
| www \. contoso.com/abzzz       | B             |
| www \. contoso.com/ABC/        | E             |
| www \. contoso.com/ABC/d       | F             |
| www \. contoso.com/abc/def     | G             |
| www \. contoso.com/ABC/defzzz  | F             |
| www \. contoso.com/abc/def/Ghi | F             |
| www \. contoso.com/path        | B             |
| www \. contoso.com/path/       | H             |
| www \. contoso.com/path/zzz    | B             |

>[!WARNING]
> </br> Tam eşleşen bir ön uç ana bilgisayar için bir catch-all yol yoluna () sahip bir yönlendirme kuralı yoksa `/*` , herhangi bir yönlendirme kuralıyla eşleşme olmayacaktır.
>
> Örnek yapılandırma:
>
> | Yol | Ana bilgisayar             | Yol    |
> |-------|------------------|---------|
> | A     | profile.contoso.com | /api\* |
>
> Eşleşen tablo:
>
> | Gelen istek       | Eşleşen yol |
> |------------------------|---------------|
> | profile.domain.com/other | Yok. Hata 400: Hatalı Istek |

### <a name="routing-decision"></a>Yönlendirme kararı
Tek bir ön kapı yönlendirme kuralıyla eşleştirdikten sonra, isteği nasıl işleyeceğini seçmemiz gerekir. Eşleşen yönlendirme kuralı için ön kapıda önbelleğe alınmış bir yanıt varsa, aynı şekilde istemciye geri sunulur. Aksi takdirde, değerlendirilen bir sonraki şey, eşleşen yönlendirme kuralı için [URL yeniden yazma (özel iletme yolu)](front-door-url-rewrite.md) yapılandırılıp yapılandırılmadığını belirtir. Tanımlı özel bir iletme yolu yoksa, istek yapılandırılmış arka uç havuzundaki uygun arka uca iletilir. Diğer bir deyişle, istek yolu, tanımlanan [özel iletme yoluna](front-door-url-rewrite.md) göre güncelleştirilir ve sonra arka uca iletir.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
