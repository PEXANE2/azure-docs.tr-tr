---
title: Azure API Yönetimi'ndeki Abonelikler | Microsoft Dokümanlar
description: Azure API Yönetimi'nde abonelik kavramı hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: 9144af131e1427d0b3226655c871921ac1d91665
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70073415"
---
# <a name="subscriptions-in-azure-api-management"></a>Azure API Management'ta abonelikler

Abonelikler Azure API Yönetimi'nde önemli bir kavramdır. Bunlar, API tüketicilerinin API Yönetimi örneği aracılığıyla yayınlanan API'lere erişebilmek için en yaygın yoldur. Bu makalede, kavramın genel bir bakış sağlar.

## <a name="what-are-subscriptions"></a>Abonelikler nedir?

API Yönetimi aracılığıyla API'leri yayımladığınızda, abonelik anahtarlarını kullanarak bu API'lere erişimi güvence altına almak kolay ve yaygındır. Yayımlanmış API'leri tüketmesi gereken geliştiriciler, bu API'lere çağrı yaparken HTTP isteklerine geçerli bir abonelik anahtarı eklemelidir. Aksi takdirde, aramalar API Yönetimi ağ geçidi tarafından hemen reddedilir. Arka uç servislerine iletilirler.

API'lere erişmek için abonelik anahtarı almak için abonelik gerekir. Abonelik aslında bir çift abonelik anahtarı için adlandırılmış bir kapsayıcıdır. Yayımlanmış API'leri tüketen geliştiriciler abonelik alabilir. Ve API yayıncılarının onayına ihtiyaçları yok. API yayıncıları doğrudan API tüketicileri için abonelikler oluşturabilir.

> [!TIP]
> API Yönetimi, aşağıdaki örnekler de dahil olmak üzere API'lere erişimi güvence altına almak için diğer mekanizmaları da destekler:
> - [OAuth2.0](api-management-howto-protect-backend-with-aad.md)
> - [İstemci sertifikaları](api-management-howto-mutual-certificates-for-clients.md)
> - [IP beyaz liste](https://docs.microsoft.com/azure/api-management/api-management-access-restriction-policies#RestrictCallerIPs)

## <a name="scope-of-subscriptions"></a>Aboneliklerin kapsamı

Abonelikler çeşitli kapsamlarla ilişkilendirilebilir: ürün, tüm API'ler veya tek bir API.

### <a name="subscriptions-for-a-product"></a>Bir ürünün abonelikleri

Geleneksel olarak, API Yönetimi'ndeki abonelikler her zaman tek bir [API ürün](api-management-terminology.md) kapsamıyla ilişkilendirildi. Geliştiriciler geliştirici portalında ürün listesini buldular. Daha sonra kullanmak istedikleri ürünler için abonelik istekleri gönderirlerdi. Bir abonelik isteği otomatik olarak veya API yayımcısı tarafından onaylandıktan sonra, geliştirici üründeki tüm API'lere erişmek için bu istekteki anahtarları kullanabilir. Şu anda geliştirici portalı yalnızca kullanıcı profili bölümü altında ürün kapsamı aboneliklerini gösterir. 

![Ürün abonelikleri](./media/api-management-subscriptions/product-subscription.png)

> [!TIP]
> Belirli senaryolarda, API yayımcıları abonelik gereksinimi olmadan bir API ürününün kamuya yayımlanmasını isteyebilir. Azure portalındaki ürünün **Ayarlar** **sayfasındaabonelik İste** seçeneğini de seçebilirler. Sonuç olarak, ürünün altındaki tüm API'lara API anahtarı olmadan erişilebilir.

### <a name="subscriptions-for-all-apis-or-an-individual-api"></a>Tüm API'ler veya tek bir API için abonelikler

API Yönetimi'nin [Tüketim](https://aka.ms/apimconsumptionblog) katmanını tanıttığımızda, anahtar yönetimini kolaylaştırmak için birkaç değişiklik yaptık:
- İlk olarak, iki abonelik kapsamı daha ekledik: tüm API'ler ve tek bir API. Aboneliklerin kapsamı artık bir API ürünüyle sınırlı değildir. Artık bir ÜRÜN oluşturmave api eklemeye gerek kalmadan bir API'ye veya API Yönetimi örneğindeki tüm API'lere erişim sağlayan anahtarlar oluşturmak mümkündür. Ayrıca, her API Yönetimi örneği artık değişmez, tüm API aboneliğiyle birlikte gelir. Bu abonelik, test konsolu içindeki API'leri test etmeyi ve hata ayıklamayı daha kolay ve daha kolay hale getirir.

- İkinci olarak, API Yönetimi artık **bağımsız** aboneliklere izin veriyor. Aboneliklerin artık bir geliştirici hesabıyla ilişkilendirilmesi gerekmez. Bu özellik, birkaç geliştirici veya takımın aboneliği paylaşması gibi senaryolarda yararlıdır.

- Son olarak, API yayıncıları artık doğrudan Azure portalında [abonelikler oluşturabilir:](api-management-howto-create-subscriptions.md)

    ![Esnek abonelikler](./media/api-management-subscriptions/flexible-subscription.png)

## <a name="next-steps"></a>Sonraki adımlar
API Yönetimi hakkında daha fazla bilgi alın:

+ API Yönetimi'nde diğer [kavramları](api-management-terminology.md) öğrenin.
+ API Yönetimi hakkında daha fazla bilgi edinmek için [eğitimlerimizi](import-and-publish.md) takip edin.
+ Sık sorulan sorular için [SSS sayfamızı](api-management-faq.md) kontrol edin.
