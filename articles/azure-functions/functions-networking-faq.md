---
title: Azure İşlevleri'nde ağ la ilgili sık sorulan sorular
description: Azure İşlevleri ile ağ oluşturmak için en sık karşılaşılan sorulardan ve senaryolardan bazılarına yanıtlar.
author: alexkarcher-msft
ms.topic: troubleshooting
ms.date: 4/11/2019
ms.author: alkarche
ms.reviewer: glenga
ms.openlocfilehash: acb1e942c1f342ce6fee7d8aeacafcc1d7b6fd91
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75409539"
---
# <a name="frequently-asked-questions-about-networking-in-azure-functions"></a>Azure İşlevleri'nde ağ la ilgili sık sorulan sorular

Bu makalede, Azure İşlevleri'nde ağ oluşturma yla ilgili sık sorulan sorular listelanmaktadır. Daha kapsamlı bir genel bakış [için, İşlevler ağ seçeneklerine](functions-networking-options.md)bakın.

## <a name="how-do-i-set-a-static-ip-in-functions"></a>Fonksiyonlar statik IP'yi nasıl ayarlıyorum?

Bir App Service Environment'da bir işlev dağıtmak, şu anda işleviniz için statik gelen ve giden BIR IP'ye sahip olmak için tek yoldur. Uygulama Hizmet Ortamı nı kullanma yla ilgili ayrıntılar için, Uygulama Hizmet Ortamı ile [bir dahili yük dengeleyicisi oluştur ve kullanın](../app-service/environment/create-ilb-ase.md)makalesiyle başlayın.

## <a name="how-do-i-restrict-internet-access-to-my-function"></a>İşlevime internet erişimini nasıl kısıtlarım?

Internet erişimini birkaç şekilde kısıtlayabilirsiniz:

* [IP kısıtlamaları](../app-service/app-service-ip-restrictions.md): Ip aralığına göre işlev uygulamanıza gelen trafiği kısıtlayın.
    * IP kısıtlamaları altında, İşlevinizi yalnızca belirli bir sanal ağdan gelen trafiği kabul etmek üzere kısıtlayan [Hizmet Bitiş Noktalarını](../virtual-network/virtual-network-service-endpoints-overview.md)da yapılandırabilirsiniz.
* Tüm HTTP tetikleyicilerinin kaldırılması. Bazı uygulamalar için, sadece HTTP tetikleyicileri önlemek ve işlevinizi tetiklemek için başka bir olay kaynağı kullanmak yeterlidir.

Azure portal düzenleyicisinin çalışan işlevinize doğrudan erişim gerektirdiğini unutmayın. Azure portalındaki herhangi bir kod değişikliği, kullandığınız aygıtın ip'sinin beyaz listeye alınması için portala göz atmasını gerektirir. Ancak, ağ kısıtlamaları içeren platform özellikleri sekmesi altında her şeyi kullanmaya devam edebilirsiniz.

## <a name="how-do-i-restrict-my-function-app-to-a-virtual-network"></a>İşlev uygulamamı sanal ağla nasıl sınırlandırım?

[Hizmet Bitiş Noktalarını](./functions-networking-options.md#private-site-access)kullanarak bir işlev uygulaması nın **gelen** trafiğini sanal ağla sınırlandırabilirsiniz. Bu yapılandırma yine de işlev uygulamasının internete giden aramalar yapmasına olanak tanır.

Tüm trafiğin sanal ağ üzerinden akması gibi bir işlevi tamamen kısıtlamanın tek yolu, dahili olarak yük dengeli bir Uygulama Hizmeti Ortamı kullanmaktır. Bu seçenek, sitenizi sanal ağ içinde özel bir altyapıya dağıtır ve tüm tetikleyicileri ve trafiği sanal ağ üzerinden gönderir. 

Uygulama Hizmet Ortamı nı kullanma yla ilgili ayrıntılar için, Uygulama Hizmet Ortamı ile [bir dahili yük dengeleyicisi oluştur ve kullanın](../app-service/environment/create-ilb-ase.md)makalesiyle başlayın.

## <a name="how-can-i-access-resources-in-a-virtual-network-from-a-function-app"></a>Bir işlev uygulamasından sanal ağdaki kaynaklara nasıl erişebilirim?

Sanal ağ tümleştirmesi kullanarak çalışan bir işlevden sanal ağdaki kaynaklara erişebilirsiniz. Daha fazla bilgi için [Sanal ağ tümleştirmesi'ne](functions-networking-options.md#virtual-network-integration)bakın.

## <a name="how-do-i-access-resources-protected-by-service-endpoints"></a>Hizmet bitiş noktalarıtarafından korunan kaynaklara nasıl erişebilirim?

Sanal ağ tümleştirmesi kullanarak çalışan bir işlevden hizmet sonu açısından güvenli kaynaklara erişebilirsiniz. Daha fazla bilgi için [sanal ağ tümleştirmesi'ne](functions-networking-options.md#virtual-network-integration)bakın.

## <a name="how-can-i-trigger-a-function-from-a-resource-in-a-virtual-network"></a>Sanal ağdaki bir kaynaktan bir işlevi nasıl tetikleyebilirim?

[Hizmet Bitiş Noktaları'nı](./functions-networking-options.md#private-site-access)kullanarak HTTP tetikleyicilerinin sanal bir ağdan çağrılmasını izin verebilirsiniz. 

Ayrıca, işlev uygulamanızı Premium plana, Uygulama Hizmeti planına veya Uygulama Hizmeti Ortamına dağıtarak sanal ağdaki diğer tüm kaynaklardan bir işlevi tetikleyebilirsiniz. Daha fazla bilgi için [HTTP dışı sanal ağ tetikleyicilerine](./functions-networking-options.md#virtual-network-triggers-non-http) bakın

## <a name="how-can-i-deploy-my-function-app-in-a-virtual-network"></a>İşlev uygulamamı sanal ağda nasıl dağıtabilirim?

Bir Uygulama Hizmet Ortamına dağıtmak, tamamen sanal bir ağ içinde olan bir işlev uygulaması oluşturmanın tek yoludur. Uygulama Hizmet Ortamı ile bir dahili yük dengeleyicisi kullanma hakkında ayrıntılı bilgi için, Uygulama Hizmet Ortamı ile bir [dahili yük dengeleyicisi oluştur ve kullanın](https://docs.microsoft.com/azure/app-service/environment/create-ilb-ase)makalesiyle başlayın.

Sanal ağ kaynaklarına yalnızca tek yönlü erişime veya daha az kapsamlı ağ yalıtımına ihtiyaç duyduğunuz senaryolar [için, İşlevler ağ genel bakışına](functions-networking-options.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Ağ ve işlevler hakkında daha fazla bilgi edinmek için: 

* [Sanal ağ entegrasyonu ile başlamak hakkında öğretici izleyin](./functions-create-vnet.md)
* [Azure İşlevleri'ndeki ağ seçenekleri hakkında daha fazla bilgi edinin](./functions-networking-options.md)
* [Uygulama Hizmeti ve İşlevleri ile sanal ağ entegrasyonu hakkında daha fazla bilgi edinin](../app-service/web-sites-integrate-with-vnet.md)
* [Azure'da sanal ağlar hakkında daha fazla bilgi edinin](../virtual-network/virtual-networks-overview.md)
* [App Service Ortamları ile daha fazla ağ özelliği ve denetim etkinleştirin](../app-service/environment/intro.md)
