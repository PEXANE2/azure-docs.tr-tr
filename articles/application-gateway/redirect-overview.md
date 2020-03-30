---
title: Azure Uygulama Ağ Geçidi'ne genel bakışı yeniden yönlendirme
description: Bir dinleyiciden alınan trafiği başka bir dinleyiciye veya harici bir siteye yönlendirmek için Azure Application Gateway'deki yönlendirme özelliği hakkında bilgi edinin.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 11/16/2019
ms.author: amsriva
ms.openlocfilehash: 5943d8aad4d5dd0d981fae9b2325dd3fc75b31e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74129876"
---
# <a name="application-gateway-redirect-overview"></a>Uygulama Ağ Geçidi yönlendirme genel bakış

Trafiği yeniden yönlendirmek için uygulama ağ geçidini kullanabilirsiniz.  Bir dinleyiciden alınan trafiği başka bir dinleyiciye veya bir dış siteye yeniden yönlendirmeyi sağlayan genel bir yeniden yönlendirme mekanizmasına sahiptir. Bu, uygulama yapılandırmasını basitleştirir, kaynak kullanımını en iyi duruma getirtir ve genel ve yol tabanlı yeniden yönlendirme de dahil olmak üzere yeni yeniden yönlendirme senaryolarını destekler.

Birçok web uygulaması için ortak bir yeniden yönlendirme senaryosu, uygulama ve kullanıcıları arasındaki tüm iletişimin şifreli bir yol üzerinden gerçekleşmesini sağlamak için otomatik HTTP'den HTTPS'ye yeniden yönlendirmeyi desteklemektir. Geçmişte müşteriler, tek amacı HTTP'den aldığı istekleri HTTPS'ye yönlendirmek olan özel bir arka uç havuzu oluşturma gibi teknikler kullanmıştır. Uygulama Ağ Geçidi'ndeki yeniden yönlendirme desteğiyle, bunu yalnızca yönlendirme kuralına yeni bir yönlendirme yapılandırması ekleyerek ve hedef dinleyici olarak HTTPS protokolü olan başka bir dinleyici belirterek gerçekleştirebilirsiniz.

Aşağıdaki yeniden yönlendirme türleri desteklenir:

- 301 Kalıcı Yönlendirme
- 302 Bulundu
- 303 Bkz. Diğer
- 307 Geçici Yönlendirme

Application Gateway yeniden yönlendirme desteği aşağıdaki özellikleri sunar:

-  **Küresel yeniden yönlendirme**

   Bir dinleyiciden ağ geçidindeki başka bir dinleyiciye yönlendirir. Bu özellik, bir sitede HTTP’den HTTPS’ye yeniden yönlendirmeyi sağlar.
- **Yol tabanlı yeniden yönlendirme**

   Bu tür yeniden yönlendirme, HTTP'nin HTTPS'ye yeniden yönlendirmesini yalnızca belirli bir site alanında (örneğin/sepet/* olarak gösterilen bir alışveriş sepeti alanı) etkinleştirir.
- **Harici siteye yönlendirme**

![Yönlendirme](./media/redirect-overview/redirect.png)

Bu değişiklikle, müşterilerin hedef dinleyiciyi veya yeniden yönlendirmenin istendiği harici siteyi belirten yeni bir yeniden yönlendirme yapılandırma nesnesi oluşturmaları gerekir. Yapılandırma öğesi, URI yolunu ve sorgu dizesini yeniden yönlendirilen URL'ye eklemeyi etkinleştirmek için seçenekleri de destekler. Yeniden yönlendirme türünü de seçebilirsiniz. Oluşturulduktan sonra, bu yönlendirme yapılandırması yeni bir kural la kaynak dinleyiciye eklenir. Temel bir kural kullanırken, yeniden yönlendirme yapılandırması bir kaynak dinleyiciyle ilişkilidir ve genel bir yönlendirmedir. Yol tabanlı bir kural kullanıldığında, yeniden yönlendirme yapılandırması URL yolu haritasında tanımlanır. Bu nedenle yalnızca bir sitenin belirli yol alanı için geçerlidir.

### <a name="next-steps"></a>Sonraki adımlar

[Url yeniden yönlendirmeyi bir uygulama ağ geçidinde yapılandırma](tutorial-url-redirect-powershell.md)
