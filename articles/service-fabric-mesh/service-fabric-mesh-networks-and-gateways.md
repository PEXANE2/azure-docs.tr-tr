---
title: Azure Hizmet Kumaşı ağına giriş
description: Service Fabric Mesh'te ağlar, ağ geçitleri ve akıllı trafik yönlendirmesi hakkında bilgi edinin.
author: dkkapur
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: dc793e2991783cc9b7b46d92fcc8e0267feb529b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75459142"
---
# <a name="introduction-to-networking-in-service-fabric-mesh-applications"></a>Hizmet Kumaş Örgü uygulamalarında ağ girişi
Bu makalede, farklı yük dengeleyicileri türleri, ağ ağlarını uygulamalarınız ile diğer ağlara nasıl bağlandığı ve uygulamalarınızdaki hizmetler arasında trafiğin nasıl yönlendirildiğini açıklanır.

## <a name="layer-4-vs-layer-7-load-balancers"></a>Katman 4 vs katman 7 yük dengeleyicileri
Yük dengeleme, genellikle katman 4 (L4) ve katman 7 (L7) olarak ağ için [OSI modelinde](https://en.wikipedia.org/wiki/OSI_model) farklı katmanlarda gerçekleştirilebilir.  Tipik olarak, iki tür yük dengeleyicisi vardır:

- Bir L4 yük dengeleyicisi, paketlerin içeriğine bakılmaksızın paketlerin teslimi ile ilgilenen ağ aktarım katmanında çalışır. Yalnızca paket üstbilgileri yük dengeleyicitarafından denetlenir, bu nedenle dengeleme ölçütleri IP adresleri ve bağlantı noktalarıyla sınırlıdır. Örneğin, istemci yük bakiyesine TCP bağlantısı yapar. Yük dengeleyicisi bağlantıyı sonlandırır (doğrudan SYN'ye yanıt vererek), bir arka uç seçer ve arka uca yeni bir TCP bağlantısı yapar (yeni bir SYN gönderir). Bir L4 yük dengeleyicisi genellikle yalnızca L4 TCP/UDP bağlantısı veya oturumu düzeyinde çalışır. Böylece yük dengesi baytları yönlendirir ve aynı seanstan gelen baytların aynı arka uçta olmasını sağlar. L4 yük dengeleyicisi, hareket ettiği baytların uygulama ayrıntılarından habersizdir. Baytlar herhangi bir uygulama protokolü olabilir.

- Bir L7 yük dengeleyicisi, her paketin içeriğiyle ilgilenen uygulama katmanında çalışır. HTTP, HTTPS veya WebSockets gibi protokolleri anladığı için paket içeriğini inceler. Bu, yük dengeleyicisine gelişmiş yönlendirme yapma olanağı sağlar. Örneğin, bir istemci yük dengeleyicisine tek bir HTTP/2 TCP bağlantısı yapar. Yük dengeleyicisi daha sonra iki arka uç bağlantısı yapmaya devam eder. İstemci yük bakiyesine iki HTTP/2 akışı gönderdiğinde, akış bir arka uç için gönderilir ve ikinci akış iki arka uç için gönderilir. Böylece, çok farklı istek yükleri olan çoklama istemcileri bile arka uçlar arasında verimli bir şekilde dengelenir. 

## <a name="networks-and-gateways"></a>Ağlar ve ağ geçitleri
Hizmet [Kumaş Kaynak](service-fabric-mesh-service-fabric-resources.md)Modeli'nde, Ağ kaynağı, bağımlılık olarak adlandırılabilecek bir Uygulama veya Hizmet kaynağından bağımsız olarak, ayrı ayrı dağıtılabilir bir kaynaktır. Bu internete açık uygulamalar için bir ağ oluşturmak için kullanılır. Farklı uygulamalardan gelen birden çok hizmet aynı ağın bir parçası olabilir. Bu özel ağ Service Fabric tarafından oluşturulur ve yönetilir ve bir Azure sanal ağı (VNET) değildir. Uygulamalar, VNET bağlantısını etkinleştirmek ve devre dışı kaldırmak için dinamik olarak eklenebilir ve ağ kaynağından kaldırılabilir. 

Ağ geçidi, iki ağ arasında köprü yapmak için kullanılır. Ağ Geçidi kaynağı, gelişmiş HTTP(S) uygulama yönlendirmesi için herhangi bir protokol ve L7 yönlendirmesi için L4 yönlendirmesi sağlayan bir [Elçi proxy'si](https://www.envoyproxy.io/) dağıtır. Ağ geçidi, trafiği harici bir ağdan ağınıza yönlendirir ve trafiği hangi hizmete yönlendireceklerini belirler.  Harici ağ, diğer Azure uygulamalarınızla ve kaynaklarınızla bağlantı kurmanıza olanak tanıyan açık bir ağ (aslında ortak internet) veya Azure sanal ağı olabilir. 

![Ağ ve ağ geçidi][Image1]

Ağ kaynağı `ingressConfig`ile oluşturulduğunda, ortak bir IP ağ kaynağına atanır. Genel IP, ağ kaynağının ömrüne bağlı olacaktır.

Bir Mesh uygulaması oluşturulduğunda, varolan bir ağ kaynağına başvurması gerekir. Yeni ortak bağlantı noktaları eklenebilir veya varolan bağlantı noktaları giriş yapılandırmasından kaldırılabilir. Bir uygulama kaynağı başvuruyorsa, ağ kaynağı için bir silme başarısız olur. Uygulama silindiğinde, ağ kaynağı kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar 
Service Fabric Mesh hakkında daha fazla bilgi edinmek için genel bakışı okuyun:
- [Hizmet Kumaş Örgü genel bakış](service-fabric-mesh-overview.md)

[Image1]: media/service-fabric-mesh-networks-and-gateways/NetworkAndGateway.png