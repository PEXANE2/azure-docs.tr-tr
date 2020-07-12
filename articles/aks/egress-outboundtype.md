---
title: Azure Kubernetes Service (AKS) içinde Kullanıcı tanımlı yolları (UDR) özelleştirme
description: Azure Kubernetes Service (AKS) içinde özel çıkış yolu tanımlama hakkında bilgi edinin
services: container-service
ms.topic: article
ms.author: juluk
ms.date: 06/29/2020
author: jluk
ms.openlocfilehash: 4c5d6bf83d9aa9c3717b0f8e08785b0fc897577d
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86244455"
---
# <a name="customize-cluster-egress-with-a-user-defined-route"></a>Küme çıkış listesini Kullanıcı tanımlı bir yol ile özelleştirme

AKS kümesinden çıkış, belirli senaryolara uyacak şekilde özelleştirilebilir. Varsayılan olarak AKS, çıkış için ayarlanacak ve kullanılacak standart bir SKU Load Balancer sağlar. Ancak, genel IP 'Ler izin verilmedikçe veya çıkış için ek atlamalar gerekliyse, varsayılan kurulum tüm senaryoların gereksinimlerini karşılamayabilir.

Bu makalede, genel IP 'Lere izin vermeyen ve kümenin bir ağ sanal gereci (NVA) arkasına oturmasının gerekli olduğu gibi özel ağ senaryolarını desteklemek için bir kümenin çıkış yolunun nasıl özelleştirileceği gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar
* Azure CLı sürüm 2.0.81 veya üzeri
* `2020-01-01`Veya daha büyük API sürümü


## <a name="limitations"></a>Sınırlamalar
* OutboundType, yalnızca küme oluşturma zamanında tanımlanabilir ve daha sonra güncellenemiyor.
* Ayar `outboundType` , ve için AKS kümeleri `vm-set-type` gerektirir `VirtualMachineScaleSets` `load-balancer-sku` `Standard` .
* `outboundType`Bir değere ayarlandığında `UDR` , küme için geçerli giden bağlantıya sahip kullanıcı tanımlı bir yol gerekir.
* `outboundType`Bir değere ayarlandığında `UDR` , yük dengeleyicisine yönlendirilen giriş kaynak IP 'si kümenin giden çıkış hedefi adresiyle **eşleşmeyebilir** .

## <a name="overview-of-outbound-types-in-aks"></a>AKS 'deki giden türlere genel bakış

AKS kümesi, `outboundType` yük dengeleyici veya Kullanıcı tanımlı yönlendirme türünde benzersiz bir şekilde özelleştirilebilir.

> [!IMPORTANT]
> Giden türü yalnızca kümenizin çıkış trafiğini etkiler. Daha fazla bilgi için bkz. giriş [denetleyicilerini ayarlama](ingress-basic.md).

> [!NOTE]
> UDR ve Kubernetes kullanan Networking ile kendi [yol tablonuzu][byo-route-table] kullanabilirsiniz. Küme kimliğinin (hizmet sorumlusu veya yönetilen kimlik) özel yol tablosuna katkıda bulunan izinlerine sahip olduğundan emin olun.

### <a name="outbound-type-of-loadbalancer"></a>Yük dengeleyici giden türü

`loadBalancer`Ayarlanırsa, AKS aşağıdaki yapılandırmayı otomatik olarak tamamlar. Yük dengeleyici, bir AKS atanmış genel IP üzerinden çıkış için kullanılır. Giden türü `loadBalancer` `loadBalancer` , aks kaynak sağlayıcısı tarafından oluşturulan yük dengeleyiciden çıkış bekleyen, türü Kubernetes hizmetlerini destekler.

Aşağıdaki yapılandırma AKS tarafından yapılır.
   * Küme çıkışı için genel bir IP adresi sağlandı.
   * Genel IP adresi, yük dengeleyici kaynağına atanır.
   * Yük Dengeleyici için arka uç havuzları kümedeki aracı düğümleri için ayarlanır.

Varsayılan olarak, AKS kümelerinde dağıtılan ve ' ı kullanan bir ağ topolojisi vardır `outboundType` `loadBalancer` .

![OutboundType-lb](media/egress-outboundtype/outboundtype-lb.png)

### <a name="outbound-type-of-userdefinedrouting"></a>UserDefinedRouting 'in giden türü

> [!NOTE]
> Giden türü kullanmak gelişmiş bir ağ senaryosudur ve uygun ağ yapılandırması gerektirir.

`userDefinedRouting`Ayarlanırsa, AKS çıkış yollarını otomatik olarak yapılandırmaz. Çıkış kurulumu sizin tarafınızdan yapılmalıdır.

AKS kümesi, standart yük dengeleyici (SLB) mimarisi kullanılırken açık çıkış oluşturmanız gerektiğinden, daha önce yapılandırılmış bir alt ağa sahip mevcut bir sanal ağa dağıtılmalıdır. Bu şekilde, Bu mimaride bir güvenlik duvarı, ağ geçidi, ara sunucu veya ağ adresi çevirisi (NAT) 'nin standart yük dengeleyiciye veya gerecine atanan bir genel IP tarafından yapılmasına izin vermek için bu mimarinin çıkış trafiğini açıkça göndermesi gerekir.

AKS kaynak sağlayıcısı standart yük dengeleyici (SLB) dağıtır. Yük dengeleyici hiçbir kurala göre yapılandırılmaz ve [bir kural yerleştirilene kadar ücretlendirilmez](https://azure.microsoft.com/pricing/details/load-balancer/). AKS, SLB ön ucu için otomatik olarak genel bir IP adresi sağlamaz veya yük dengeleyici arka uç havuzunu **otomatik olarak yapılandırır** .

## <a name="deploy-a-cluster-with-outbound-type-of-udr-and-azure-firewall"></a>Bir kümeyi UDR ve Azure Güvenlik duvarının giden türüyle dağıtma

Kullanıcı tanımlı bir yol kullanarak giden türü olan bir kümenin uygulamasını göstermek için bir küme, kendi alt ağında Azure Güvenlik duvarı olan bir sanal ağ üzerinde yapılandırılabilir. [Azure Güvenlik Duvarı ile çıkış trafiğini kısıtla örneğinde](limit-egress-traffic.md#restrict-egress-traffic-using-azure-firewall)bu örneğe bakın.

> [!IMPORTANT]
> UDR 'nin giden türü, yol tablosundaki NVA 'nın (ağ sanal gereç) 0.0.0.0/0 ve sonraki atlama hedefi için bir yol olmasını gerektirir.
> Yol tablosunun varsayılan bir 0.0.0.0/0 olması zaten Internet 'e ait bir genel IP 'ye sahip olmadığı için, bu yolun yalnızca bu yolu eklemeniz durumunda çıkış sağlayamayacak. AKS, Internet 'e işaret eden bir 0.0.0.0/0 yolu oluşturmayacağını doğrular, bunun yerine NVA veya Gateway, vb. olur.


## <a name="next-steps"></a>Sonraki adımlar

Bkz. [Azure ağ UDR 'ye genel bakış](../virtual-network/virtual-networks-udr-overview.md).

Bkz. [yol tablosu oluşturma, değiştirme veya silme](../virtual-network/manage-route-table.md).

<!-- LINKS - internal -->
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[byo-route-table]: configure-kubenet.md#bring-your-own-subnet-and-route-table-with-kubenet
