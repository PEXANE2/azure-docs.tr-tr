---
title: Azure Application Gateway nedir?
description: Uygulamanıza web trafiğini yönetmek için bir Azure uygulama ağ geçidini nasıl kullanabileceğinizi öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.custom: mvc
ms.date: 03/04/2020
ms.author: victorh
ms.openlocfilehash: 4a4395801218409fe77d1081689ba80b495fcfad
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78302585"
---
# <a name="what-is-azure-application-gateway"></a>Azure Application Gateway nedir?

Azure Application Gateway, web uygulamalarınıza trafiği yönetmenizi sağlayan bir web trafiği yük dengeleyicisidir. Geleneksel yük dengeleyiciler aktarım katmanında (OSI katman 4 - TCP ve UDP) çalışır ve trafiği kaynak IP adresi ve bağlantı noktasına göre hedef bir IP adresi ve bağlantı noktasına yönlendirir.

Uygulama Ağ Geçidi, URI yolu veya ana bilgisayar üstleri gibi bir HTTP isteğinin ek özniteliklerine dayalı yönlendirme kararları verebilir. Örneğin, gelen URL’yi temel alarak trafiği yönlendirebilirsiniz. Yani `/images` gelen URL’deyse, trafiği görüntüler için yapılandırılmış belirli bir sunucu kümesine (havuz olarak da bilinir) yönlendirebilirsiniz. URL'deyse, `/video` bu trafik videolar için en iyi duruma getirilmiş başka bir havuza yönlendirilir.

![imageURLroute](./media/application-gateway-url-route-overview/figure1-720.png)

Bu yönlendirme türü, uygulama katmanı (OSI katman 7) yük dengelemesi olarak bilinir. Azure Application Gateway, URL tabanlı yönlendirmeyi ve daha fazlasını yapabilir.

>[!NOTE]
> Azure, senaryolarınız için tam olarak yönetilen yük dengeleme çözümleri sunar. Yüksek performanslı, düşük gecikmeli, Katman-4 yük dengelemesi gerekiyorsa, [bkz.](../load-balancer/load-balancer-overview.md) Global DNS yük dengeleme arıyorsanız, [bkz.](../traffic-manager/traffic-manager-overview.md) Uçtan uca senaryolarınız bu çözümleri birleştirerek yararlanabilir.
>
> Azure yük dengeleme seçenekleri karşılaştırması için [Azure'da yük dengeleme seçeneklerine genel bakış'a](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview)bakın.

## <a name="features"></a>Özellikler

Uygulama Ağ Geçidi özellikleri hakkında bilgi edinmek için [Azure Uygulama Ağ Geçidi özelliklerine](features.md)bakın.

## <a name="pricing-and-sla"></a>Fiyatlandırma ve SLA

Uygulama Ağ Geçidi fiyatlandırma bilgileri için Uygulama [Ağ Geçidi fiyatlandırması'na](https://azure.microsoft.com/pricing/details/application-gateway/)bakın.

Uygulama Ağ Geçidi SLA bilgileri için Bkz. [Uygulama Ağ Geçidi SLA.](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_2/)

## <a name="next-steps"></a>Sonraki adımlar

Gereksinimlerinize ve ortamınıza bağlı olarak, Azure portalı, Azure PowerShell veya Azure CLI'yi kullanarak bir test Uygulama Ağ Geçidi oluşturabilirsiniz.

- [Hızlı başlangıç: Azure Application Gateway ile doğrudan web trafiği - Azure portalı](quick-create-portal.md)
- [Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure PowerShell](quick-create-powershell.md)
- [Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure CLI](quick-create-cli.md)
