---
title: Azure API Management ile gelişmiş istek azaltma
description: Azure API Yönetimi ile esnek kota ve fiyat sınırlayıcı ilkeler oluşturmayı ve nasıl uygulayacağınızı öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: fc813a65-7793-4c17-8bb9-e387838193ae
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2018
ms.author: apimpm
ms.openlocfilehash: 467d9cee74567fc0d19031773415675ae7c51818
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71066762"
---
# <a name="advanced-request-throttling-with-azure-api-management"></a>Azure API Management ile gelişmiş istek azaltma
Gelen istekleri azaltabilmek Azure API Yönetimi'nin önemli bir rolüdür. API Yönetimi, isteklerin oranını veya aktarılan toplam istek/verileri denetleyerek API sağlayıcılarının API'lerini kötüye kullanımdan korumalarına ve farklı API ürün katmanları için değer yaratmalarına olanak tanır.

## <a name="product-based-throttling"></a>Ürün bazlı azaltma
Bugüne kadar, fiyat azaltma özellikleri Azure portalında tanımlanan belirli bir Ürün aboneliğiyle sınırlık. Bu, API sağlayıcısının API'lerini kullanmak için kaydolan geliştiricilere sınırlamalar uygulaması için yararlıdır, ancak örneğin API'nin tek tek son kullanıcılarını azaltmada yardımcı olmaz. Geliştiricinin uygulamasının tek bir kullanıcısının tüm kotayı tüketmesi ve ardından geliştiricinin diğer müşterilerinin uygulamayı kullanabilmelerini engellemesi mümkündür. Ayrıca, yüksek miktarda istek oluşturabilecek birkaç müşteri, ara sıra kullanıcılara erişimi sınırlayabilir.

## <a name="custom-key-based-throttling"></a>Özel anahtar tabanlı azaltma

> [!NOTE]
> Ve `rate-limit-by-key` `quota-by-key` ilkeler, Azure API Yönetimi'nin Tüketim katmanında olduğunda kullanılamaz. 

Yeni [oran limit-by-key](/azure/api-management/api-management-access-restriction-policies#LimitCallRateByKey) ve [kota-by-key](/azure/api-management/api-management-access-restriction-policies#SetUsageQuotaByKey) ilkeleri trafik kontrolü için daha esnek bir çözüm sağlar. Bu yeni ilkeler, trafik kullanımını izlemek için kullanılan anahtarları tanımlamak için ifadeleri tanımlamanıza olanak sağlar. Bu çalışma şekli en kolay bir örnekle gösterilmiştir. 

## <a name="ip-address-throttling"></a>IP Adresi azaltma
Aşağıdaki ilkeler, ayda toplam 1.000.000 arama ve 10.000 kilobayt bant genişliğiyle tek bir istemci IP adresini dakikada yalnızca 10 aramayla sınırlandırAbilir. 

```xml
<rate-limit-by-key  calls="10"
          renewal-period="60"
          counter-key="@(context.Request.IpAddress)" />

<quota-by-key calls="1000000"
          bandwidth="10000"
          renewal-period="2629800"
          counter-key="@(context.Request.IpAddress)" />
```

Internet'teki tüm istemciler benzersiz bir IP adresi kullanmışsa, bu kullanıcı tarafından kullanımı sınırlamanın etkili bir yolu olabilir. Ancak, birden çok kullanıcı, Bir NAT aygıtı üzerinden Internet'e erişmelerinden dolayı tek bir genel IP adresini paylaşıyor olabilir. Buna rağmen, kimlik doğrulaması olmayan erişime `IpAddress` izin veren API'ler için en iyi seçenek olabilir.

## <a name="user-identity-throttling"></a>Kullanıcı kimliği azaltma
Son kullanıcının kimliği doğrulanırsa, o kullanıcıyı benzersiz olarak tanımlayan bilgilere dayalı olarak bir azaltma anahtarı oluşturulabilir.

```xml
<rate-limit-by-key calls="10"
    renewal-period="60"
    counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
```

Bu örnek, Yetkilendirme üstbilgisinin nasıl ayıklanır, nesneye `JWT` dönüştürülür ve kullanıcıyı tanımlamak ve bunu oran sınırlayıcı anahtar olarak kullanmak için belirteci konusunu nasıl kullanacağız gösterir. Kullanıcı kimliği diğer taleplerden `JWT` biri olarak depolanırsa, bu değer onun yerine kullanılabilir.

## <a name="combined-policies"></a>Birleşik ilkeler
Yeni azaltma ilkeleri varolan azaltma ilkelerinden daha fazla denetim sağlasa da, her iki özelliği de birleştiren değer vardır. Ürün abonelik anahtarına göre daraltma[(Abonelikle arama oranını sınırla](/azure/api-management/api-management-access-restriction-policies#LimitCallRate) ve [kullanım kotasını abonelikle ayarla)](/azure/api-management/api-management-access-restriction-policies#SetUsageQuota)kullanım düzeylerine göre şarj ederek BIR API'den para kazanmayı etkinleştirmek için harika bir yoldur. Kullanıcı tarafından gazlaşabilme nin ince taneli denetimi tamamlayıcıdır ve bir kullanıcının davranışının diğerinin deneyimini alçaltmasını önler. 

## <a name="client-driven-throttling"></a>İstemci tahrikli azaltma
Azaltma anahtarı bir [ilke ifadesi](/azure/api-management/api-management-policy-expressions)kullanılarak tanımlandığında, azaltmanın kapsamını seçen API sağlayıcısıdır. Ancak, bir geliştirici kendi müşterilerini nasıl sınırlandıracaklarını denetlemek isteyebilir. Bu, geliştiricinin istemci uygulamasının anahtarı API'ye iletmesine izin verecek özel bir üstbilgi tanıtarak API sağlayıcısı tarafından etkinleştirilebilir.

```xml
<rate-limit-by-key calls="100"
          renewal-period="60"
          counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>
```

Bu, geliştiricinin istemci uygulamasının oranı sınırlayan anahtarı nasıl oluşturmak istediklerini seçmesini sağlar. İstemci geliştiriciler, kullanıcılara anahtar kümeleri ayırarak ve anahtar kullanımını döndürerek kendi fiyat katmanları oluşturabilir.

## <a name="summary"></a>Özet
Azure API Yönetimi, API hizmetinizi korumak ve değer katmak için fiyat ve teklif azaltma sağlar. Özel kapsam landırma kurallarına sahip yeni azaltma ilkeleri, müşterilerinizin daha iyi uygulamalar oluşturmasına olanak sağlamak için bu ilkeler üzerinde daha ince gelişmiş denetim yapmanızı sağlar. Bu makaledeki örnekler, istemci IP adresleri, kullanıcı kimliği ve istemci oluşturulan değerleri ile oran sınırlayıcı anahtarlar üreterek bu yeni ilkelerin kullanımını göstermektedir. Ancak, kullanıcı aracısı, URL yol parçaları, ileti boyutu gibi kullanılabilecek iletinin birçok başka bölümü vardır.

## <a name="next-steps"></a>Sonraki adımlar
Lütfen bize bu konu için bir GitHub sorunu olarak geribildirim verin. Senaryolarınızda mantıklı bir seçim olan diğer olası anahtar değerleri duymak harika olacaktır.

