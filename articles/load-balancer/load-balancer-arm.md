---
title: Load Balancer için Azure Resource Manager desteği
description: Bu makalede Azure Load Balancer ile Azure PowerShell ve şablonlar kullanın
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/19/2019
ms.author: allensu
ms.openlocfilehash: 57a60a65dee995695224aa1b451e848ea8768ab1
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74215398"
---
# <a name="azure-resource-manager-support-with-azure-load-balancer"></a>Azure Load Balancer desteği Azure Resource Manager



Azure Resource Manager, Azure 'daki hizmetler için tercih edilen yönetim çerçevesidir. Azure Load Balancer, Azure Resource Manager tabanlı API 'Ler ve araçlar kullanılarak yönetilebilir.

## <a name="concepts"></a>Kavramlar

Kaynak Yöneticisi, Azure Load Balancer aşağıdaki alt kaynakları içerir:

* Ön uç IP yapılandırması – bir yük dengeleyici, sanal IP 'Ler (VIP) olarak da bilinen bir veya daha fazla ön uç IP adresi içerebilir. Bu IP adresleri trafik için bir giriş işlevi görür.
* Arka uç adres havuzu – bu havuz, yükün dağıtıldığı sanal makine ağ arabirimi kartı (NIC) ile ilişkili IP adreslerinin bir koleksiyonudur.
* Yük Dengeleme kuralları – bir kural özelliği verilen bir ön uç IP ve bağlantı noktası bileşimini bir arka uç IP adresleri ve bağlantı noktası bileşimi ile eşleştirir. Tek bir yük dengeleyici birden fazla yük dengeleme kuralına sahip olabilir. Her kural, ön uç IP ve bağlantı noktasının ve VM 'lerle ilişkili bağlantı noktasının bir birleşimidir.
* Yoklamalar – yoklamalar, VM örneklerinin sistem durumunu izlemenize olanak tanır. Bir sistem durumu araştırması başarısız olursa, VM örneği otomatik olarak dönüşten alınır.
* Gelen NAT kuralları – ön uç IP 'si üzerinden akan ve arka uç IP 'ye dağıtılan gelen trafiği tanımlayan NAT kuralları.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Hızlı başlangıç şablonları

Azure Resource Manager bildirim temelli bir şablon kullanarak uygulamalarınıza kaynak sağlamanıza olanak tanır. Tek bir şablonda birden çok hizmeti bağımlılıklarıyla birlikte dağıtabilirsiniz. Uygulama yaşam döngüsünün her aşamasında uygulamanızı tekrar tekrar dağıtmak için aynı şablonu kullanırsınız.

Şablonlar şunlar için tanımlar içerebilir:
* **Sanal makineler**
* **Sanal ağlar**
* **Kullanılabilirlik kümeleri**
* **Ağ arabirimleri (NIC)**
* **Depolama hesapları**
* **Yük dengeleyiciler**
* **Ağ güvenlik grupları**
* **Genel IP 'Ler.** 

Şablonlar sayesinde, karmaşık bir uygulama için ihtiyacınız olan her şeyi oluşturabilirsiniz. Şablon dosyası, sürüm denetimi ve işbirliği için içerik yönetim sistemine denetlenebilir.

[Şablonlar hakkında daha fazla bilgi edinin](../azure-resource-manager/resource-manager-template-walkthrough.md)

[Ağ kaynakları hakkında daha fazla bilgi](../networking/networking-overview.md)

Azure Load Balancer kullanan hızlı başlangıç şablonları için, topluluk tarafından oluşturulan şablonlar kümesini barındıran [GitHub deposuna](https://github.com/Azure/azure-quickstart-templates) bakın.

Şablon örnekleri:

* [bir Load Balancer ve yük dengeleme kurallarında 2 VM](https://go.microsoft.com/fwlink/?LinkId=544799)
* [Iç Load Balancer ve yük dengeleyici kurallarına sahip bir VNET 'te 2 sanal makine](https://go.microsoft.com/fwlink/?LinkId=544800)
* [Yük dengeleyicide 2 VM ve LB üzerinde NAT kuralları yapılandırma](https://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>PowerShell veya CLı ile Azure Load Balancer ayarlama

Azure Resource Manager cmdlet 'leri, komut satırı araçları ve REST API 'Leri ile çalışmaya başlama

* [Azure ağ cmdlet 'leri](https://docs.microsoft.com/powershell/module/az.network#networking) , bir Load Balancer oluşturmak için kullanılabilir.
* [Azure Resource Manager kullanarak yük dengeleyici oluşturma](load-balancer-get-started-ilb-arm-ps.md)
* [Azure CLı 'yi Azure Kaynak yönetimi ile kullanma](../xplat-cli-azure-resource-manager.md)
* [REST API 'Leri Load Balancer](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a>Sonraki adımlar

[Internet 'e yönelik yük dengeleyici oluşturmaya](load-balancer-get-started-internet-arm-ps.md) başlayın ve belirli ağ trafiği davranışı için [dağıtım modu](load-balancer-distribution-mode.md) türünü yapılandırın.

[Yük Dengeleyici için BOŞTAKI TCP zaman aşımı ayarlarını](load-balancer-tcp-idle-timeout.md)yönetmeyi öğrenin. Bu ayarlar, uygulamanızın bir yük dengeleyicinin arkasındaki sunucular için bağlantıları canlı tutmaya ihtiyaç duyduğunda önemlidir.
