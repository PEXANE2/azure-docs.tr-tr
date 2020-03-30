---
title: Azure Ön Kapı - Yönlendirme kuralı eşleştirme izleme | Microsoft Dokümanlar
description: Bu makale, Azure Ön Kapı'nın gelen bir istek için kullanılacak yönlendirme kuralıyla nasıl eşleştiğini anlamanıza yardımcı olur
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
ms.openlocfilehash: 605974e76c3ca878784129f7c9827a78d0642da6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471600"
---
# <a name="how-front-door-matches-requests-to-a-routing-rule"></a>Ön Kapı istekleri yönlendirme kuralıyla nasıl eşleşir?

Bir bağlantı kurduktan ve Bir SSL el sıkışması yaptıktan sonra, bir istek Ön Kapı ortamına indiğinde, Ön Kapı'nın yaptığı ilk şeylerden biri, isteği eşleştirmek için hangi özel yönlendirme kuralını belirleyen ve daha sonra tanımlanan eylem. Aşağıdaki belge, Ön Kapı'nın bir HTTP isteğini işlerken hangi Rota yapılandırmasını kullanacağını nasıl belirlediğini açıklar.

## <a name="structure-of-a-front-door-route-configuration"></a>Ön Kapı rota yapılandırmasının yapısı
Ön Kapı yönlendirme kuralı yapılandırması iki ana bölümden oluşur: "sol taraf" ve "sağ taraf". Gelen isteği rotanın sol tarafına eşleştirirken, sağ taraf isteği nasıl işlediğimizi tanımlar.

### <a name="incoming-match-left-hand-side"></a>Gelen maç (sol taraf)
Aşağıdaki özellikler, gelen isteğin yönlendirme kuralıyla (veya sol tarafla) eşleşip eşleşmediğini belirler:

* **HTTP Protokolleri** (HTTP/HTTPS)
* **Ana bilgisayarlar** (örneğin, www\.foo.com, \*.bar.com)
* **Yollar** (örneğin,\*/ ,\*/users/ , /file.gif)

Bu özellikler, Protokol/Ana Bilgisayar/Yol'un her birleşimi nin olası bir eşleşme kümesi olması için dahili olarak genişletilir.

### <a name="route-data-right-hand-side"></a>Rota verileri (sağ taraf)
İsteğin nasıl işlenip işlenmeyeceğine ilişkin karar, önbelleğe almanın belirli bir rota için etkin olup olmadığına bağlıdır. Bu nedenle, istek için önbelleğe alınmış bir yanıt yoksa, isteği yapılandırılmış arka uç havuzunda uygun arka uca ileteceğiz.

## <a name="route-matching"></a>Rota eşleştirme
Bu bölümde, belirli bir Ön Kapı yönlendirme kuralına nasıl uyacağız üzerinde durulacaktır. Temel kavram, her zaman **en özel maç ilk** sadece "sol tarafı" bakarak maç olmasıdır.  Biz ilk HTTP protokolü dayalı maç, sonra Frontend ev sahibi, sonra Yol.

### <a name="frontend-host-matching"></a>Frontend ana bilgisayar eşleştirme
Frontend ana bilgisayarlarını eşleştirirken, mantığı aşağıdaki gibi kullanırız:

1. Ev sahibi üzerinde tam bir eşleşme ile herhangi bir yönlendirme arayın.
2. Tam bir ön uç ev sahibi eşleşme yoksa, isteği reddedin ve 400 Kötü İstek hatası gönderin.

Bu işlemi daha fazla açıklamak için, Ön Kapı rotalarının örnek bir yapılandırmasına bakalım (yalnızca sol taraf):

| Yönlendirme kuralı | Frontend ev sahipleri | Yol |
|-------|--------------------|-------|
| A | foo.contoso.com | /\* |
| B | foo.contoso.com | /kullanıcılar/\* |
| C | www\.fabrikam.com, foo.adventure-works.com  | /\*, /images/\* |

Aşağıdaki gelen istekler Ön Kapı'ya gönderilirse, yukarıdaki aşağıdaki yönlendirme kurallarına göre eşleşirler:

| Gelen ön uç ana bilgisayar | Eşleşen yönlendirme kuralı(lar) |
|---------------------|---------------|
| foo.contoso.com | A, B |
| www\.fabrikam.com | C |
| images.fabrikam.com | Hata 400: Kötü İstek |
| foo.adventure-works.com | C |
| contoso.com | Hata 400: Kötü İstek |
| www\.adventure-works.com | Hata 400: Kötü İstek |
| www\.northwindtraders.com | Hata 400: Kötü İstek |

### <a name="path-matching"></a>Yol eşleştirme
Belirli ön uç ana bilgisayarını belirledikten ve olası yönlendirme kurallarını sadece ön uç ana bilgisayarıyla olan rotalara filtreledikten sonra, Ön Kapı sonra yönlendirme kurallarını istek yoluna göre filtreler. Ön uç ana bilgisayarları olarak benzer bir mantık kullanırız:

1. Yol'da tam eşleşme içeren herhangi bir yönlendirme kuralıarayın
2. Yollarla tam eşleşme yoksa, eşleşen bir joker Yol ile yönlendirme kurallarını arayın
3. Eşleşen bir Yol'da yönlendirme kuralları bulunmazsa, isteği reddedin ve 400: Hata İstek hatası HTTP yanıtını döndürün.

>[!NOTE]
> Joker kartı olmayan tüm Yollar, Yollar'a tam olarak eşleşiyor olarak kabul edilir. Yol bir kesikle bitse bile, yine de tam eşleşme olarak kabul edilir.

Daha fazla açıklamak için, başka bir örnek kümesine bakalım:

| Yönlendirme kuralı | Ön uç ana bilgisayar    | Yol     |
|-------|---------|----------|
| A     | www\.contoso.com | /        |
| B     | www\.contoso.com | /\*      |
| C     | www\.contoso.com | /ab      |
| D     | www\.contoso.com | /abc     |
| E     | www\.contoso.com | /abc/    |
| F     | www\.contoso.com | /abc/\*  |
| G     | www\.contoso.com | /abc/def |
| H     | www\.contoso.com | /yol/   |

Bu yapılandırma göz önüne alındığında, aşağıdaki örnek eşleşen tablo neden olur:

| Gelen İstek    | Eşleşen Rota |
|---------------------|---------------|
| www\.contoso.com/            | A             |
| www\.contoso.com/a           | B             |
| www\.contoso.com/ab          | C             |
| www\.contoso.com/abc         | D             |
| www\.contoso.com/abzzz       | B             |
| www\.contoso.com/abc/        | E             |
| www\.contoso.com/abc/d       | F             |
| www\.contoso.com/abc/def     | G             |
| www\.contoso.com/abc/defzzz  | F             |
| www\.contoso.com/abc/def/ghi | F             |
| www\.contoso.com/path        | B             |
| www\.contoso.com/path/       | H             |
| www\.contoso.com/path/zzz    | B             |

>[!WARNING]
> </br> Catch-all yolu Yolu (), ile tam maç önuç ana bilgisayar`/*`için yönlendirme kuralları yoksa, o zaman herhangi bir yönlendirme kuralı bir eşleşme olmayacaktır.
>
> Örnek yapılandırma:
>
> | Yol | Host             | Yol    |
> |-------|------------------|---------|
> | A     | profile.contoso.com | /api/\* |
>
> Eşleşen tablo:
>
> | Gelen istek       | Eşleşen Rota |
> |------------------------|---------------|
> | profile.domain.com/other | Yok. Hata 400: Kötü İstek |

### <a name="routing-decision"></a>Yönlendirme kararı
Tek bir Ön Kapı yönlendirme kuralına eşleştikten sonra, isteği nasıl işleyeceğiz seçmeliyiz. Eşleşen yönlendirme kuralı için, Ön Kapı önbelleğe alınmış bir yanıt varsa, aynı istemciye geri servis alır. Aksi takdirde, değerlendirilen bir sonraki şey, eşleşen yönlendirme kuralı için [URL Yeniden Yazma (özel yönlendirme yolu)](front-door-url-rewrite.md) yapılandırılmış olup olmadığıdır. Tanımlanmış özel bir iletme yolu yoksa, istek yapılandırılan arka uç havuzunda uygun arka uça iletilir. Aksi takdirde, istek yolu tanımlanan [özel iletme yoluna](front-door-url-rewrite.md) göre güncelleştirilir ve sonra arka uca iletilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
