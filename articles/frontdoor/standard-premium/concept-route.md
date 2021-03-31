---
title: Azure ön kapısının Standart/Premium rotası nedir?
description: Bu makale, Azure ön kapısının Standart/Premium 'un gelen bir istek için hangi yönlendirme kuralına karşılık kullanılacağını anlamanıza yardımcı olur.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/18/2021
ms.author: duau
ms.openlocfilehash: db026c4903aa30a0a4c8154af8ad6eeb4b72b706
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101100519"
---
# <a name="what-is-azure-front-door-standardpremium-preview-route"></a>Azure ön kapısının Standart/Premium (Önizleme) rotası nedir?

> [!Note]
> Bu belge, Azure ön kapısının Standart/Premium (Önizleme) içindir. Azure ön kapısı hakkında bilgi mi arıyorsunuz? [Burada](../front-door-overview.md)görüntüleyin.

Azure ön kapısının Standart/Premium yolu, gelen istek Azure ön kapısına geldiğinde trafiğin nasıl işleneceğini tanımlar. Yol ayarları aracılığıyla bir etki alanı ile arka uç kaynak grubu arasında bir ilişki tanımlanmıştır. Mak gibi gelişmiş özellikleri etkinleştirerek, kural kümesi, trafik üzerinde daha ayrıntılı denetim ulaşılabilir.

Ön kapı Standart/Premium Yönlendirme yapılandırması iki önemli bölümden oluşur: "sol taraftaki" ve "sağ taraf". Gelen istek, yolun sol tarafından ve sağ tarafta isteği nasıl işleyeceğini tanımlıyor.

> [!IMPORTANT]
> Azure ön kapı Standart/Premium (Önizleme) Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

### <a name="incoming-match-left-hand-side"></a>Gelen eşleşme (sol taraftaki)

Aşağıdaki özellikler, gelen isteğin yönlendirme kuralıyla (veya Sol tarafla) eşleşip eşleşmediğini belirtir:

* **HTTP protokolleri** (http/https)
* **Konaklar** (örneğin, www \. foo.com, \* . Bar.com)
* **Yollar** (örneğin,/ \* ,/users/ \* ,/file.gif)

Bu özellikler dahili olarak genişletilir, böylece her protokol/ana bilgisayar/yol birleşimi potansiyel bir eşleşme kümesi olur.

### <a name="route-data-right-hand-side"></a>Verileri yönlendirme (sağ taraftaki)

İsteğin nasıl işleyebileceğine karar vermek, yönlendirmenin etkin olup olmamasına bağlıdır. Önbelleğe alınmış bir yanıt yoksa, istek uygun arka uca iletilir.

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

Azure ön kapısının Standart/Premium, belirli ön uç konağını belirler ve yalnızca o ön uç ana bilgisayarıyla olan yollara yönelik olası yönlendirme kurallarını filtreledikten sonra. Ön kapı daha sonra, istek yoluna göre yönlendirme kurallarını filtreler. Ön uç konakları olarak benzer bir mantık kullanıyoruz:

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

Azure ön kapısının Standart/Premium tek bir yönlendirme kuralıyla eşleştirdikten sonra, isteğin nasıl işleyeceğini seçmesi gerekir. Azure ön kapısının Standart/Premium için, eşleşen yönlendirme kuralı için kullanılabilir önbelleğe alınmış bir yanıt varsa istek istemciye geri sunulur. Azure ön kapısı Standart/Premium, eşleşen yönlendirme kuralı için ayarlanmış bir kural olup olmadığı konusunda bir sonraki şeydir. Tanımlı bir kural kümesi yoksa, istek arka uç havuzuna olduğu gibi iletilir. Aksi takdirde, kural kümesi yapılandırıldıklarında sırayla yürütülür.

## <a name="next-steps"></a>Sonraki adımlar

[Ön kapı Standart/Premium oluşturmayı](create-front-door-portal.md)öğrenin.
