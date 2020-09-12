---
title: Azure Application Gateway yapılandırmasına genel bakış
description: Bu makalede, Azure Application Gateway bileşenlerini yapılandırma açıklanmaktadır
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 09/09/2020
ms.author: surmb
ms.openlocfilehash: 0012ac99600c77dce5940387e1da54f29c3f6ab7
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89652005"
---
# <a name="application-gateway-configuration-overview"></a>Application Gateway yapılandırmaya genel bakış

Azure Application Gateway, farklı senaryolar için çeşitli şekillerde yapılandırabileceğiniz çeşitli bileşenlerden oluşur. Bu makalede, her bir bileşenin nasıl yapılandırılacağı gösterilir.

![Application Gateway bileşenleri akış grafiği](./media/configuration-overview/configuration-overview1.png)

Bu görüntüde, üç dinleyici içeren bir uygulama gösterilmektedir. İlk ikisi sırasıyla ve için çok siteli dinleyiclardır `http://acme.com/*` `http://fabrikam.com/*` . Her ikisi de 80 numaralı bağlantı noktasını dinler. Üçüncü, daha önce Güvenli Yuva Katmanı (SSL) sonlandırma olarak bilinen uçtan uca Aktarım Katmanı Güvenliği (TLS) sonlandırmasına sahip temel bir dinleyiciye sahiptir.

## <a name="infrastructure"></a>Altyapı

Application Gateway altyapısı, sanal ağ, alt ağlar, ağ güvenlik grupları ve Kullanıcı tanımlı yolları içerir.

Daha fazla bilgi için bkz. [Application Gateway altyapı yapılandırması](configuration-infrastructure.md).



## <a name="front-end-ip-address"></a>Ön uç IP adresi

Uygulama ağ geçidini genel IP adresi, özel bir IP adresi veya her ikisine de sahip olacak şekilde yapılandırabilirsiniz. İstemcilerin Internet 'e yönelik bir sanal IP (VIP) ile Internet üzerinden erişmesi gereken bir arka ucu barındırdığınızda genel IP gereklidir.

Daha fazla bilgi için bkz. [Application Gateway ön uç IP adresi yapılandırması](configuration-front-end-ip.md).

## <a name="listeners"></a>Dinleyiciler

Dinleyici, bağlantı noktası, protokol, konak ve IP adresini kullanarak gelen bağlantı isteklerini denetleyen mantıksal bir varlıktır. Dinleyiciyi yapılandırdığınızda, bunlar için ağ geçidinde gelen istekteki karşılık gelen değerlerle eşleşen değerler girmeniz gerekir.

Daha fazla bilgi için bkz. [Application Gateway dinleyicisi yapılandırması](configuration-listeners.md).

## <a name="request-routing-rules"></a>Yönlendirme kuralı isteme

Azure portal kullanarak bir uygulama ağ geçidi oluşturduğunuzda, varsayılan bir kural (*rule1*) oluşturursunuz. Bu kural varsayılan dinleyiciyi (*Appgatewayhttplistener*) varsayılan arka uç Havuzu (*appgatewaybackendpool*) ve varsayılan arka uç http ayarları (*Appgatewaybackendhttpsettings*) ile bağlar. Ağ geçidini oluşturduktan sonra, varsayılan kuralın ayarlarını düzenleyebilir veya yeni kurallar oluşturabilirsiniz.

Daha fazla bilgi için bkz. [Application Gateway isteği yönlendirme kuralları](configuration-request-routing-rules.md).

## <a name="http-settings"></a>HTTP ayarları

Application Gateway, burada belirttiğiniz yapılandırmayı kullanarak trafiği arka uç sunucularına yönlendirir. Bir HTTP ayarı oluşturduktan sonra, bir veya daha fazla istek yönlendirme kuralıyla ilişkilendirmeniz gerekir.

Daha fazla bilgi için bkz. [http ayarları yapılandırmasını Application Gateway](configuration-http-settings.md).

## <a name="back-end-pool"></a>Arka uç havuzu

Arka uç havuzunu dört tür arka uç üyesine işaret edebilirsiniz: belirli bir sanal makine, bir sanal makine ölçek kümesi, IP adresi/FQDN veya App Service. 

Bir arka uç havuzu oluşturduktan sonra, bir veya daha fazla istek yönlendirme kuralıyla ilişkilendirmeniz gerekir. Ayrıca, uygulama ağ geçidinizdeki her bir arka uç havuzu için sistem durumu araştırmalarını da yapılandırmanız gerekir. İstek yönlendirme kuralı koşulu karşılandığında, Application Gateway trafiği ilgili arka uç havuzundaki sağlıklı sunuculara (sistem durumu araştırmaları tarafından belirlendiği şekilde) iletir.

## <a name="health-probes"></a>Sistem durumu araştırmaları

Bir Application Gateway, varsayılan olarak arka uçtaki tüm kaynakların sistem durumunu izler. Ancak sistem durumu izleme üzerinde daha fazla denetim sağlamak için her bir arka uç HTTP ayarı için özel bir araştırma oluşturmanız önemle tavsiye ederiz. Özel bir araştırmanın nasıl yapılandırılacağını öğrenmek için bkz. [özel durum araştırma ayarları](application-gateway-probe-overview.md#custom-health-probe-settings).

> [!NOTE]
> Özel bir sistem durumu araştırması oluşturduktan sonra, bir arka uç HTTP ayarıyla ilişkilendirmeniz gerekir. Karşılık gelen HTTP ayarı bir kural kullanan bir dinleyiciyle açıkça ilişkilendirilmediği takdirde, özel bir araştırma arka uç havuzunun sistem durumunu izlemez.

## <a name="next-steps"></a>Sonraki adımlar

Artık Application Gateway bileşenleri hakkında bilgi edineceğimize göre şunları yapabilirsiniz:

- [Azure portal bir uygulama ağ geçidi oluşturun](quick-create-portal.md)
- [PowerShell kullanarak uygulama ağ geçidi oluşturma](quick-create-powershell.md)
- [Azure CLı kullanarak uygulama ağ geçidi oluşturma](quick-create-cli.md)
