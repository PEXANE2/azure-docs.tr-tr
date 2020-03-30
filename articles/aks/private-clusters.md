---
title: Özel bir Azure Kubernetes Hizmet kümesi oluşturma
description: Özel bir Azure Kubernetes Hizmeti (AKS) kümesini nasıl oluşturabilirsiniz öğrenin
services: container-service
ms.topic: article
ms.date: 2/21/2020
ms.openlocfilehash: cdefcfe460a97f647afa05947e92fae0c4d07001
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79499294"
---
# <a name="create-a-private-azure-kubernetes-service-cluster"></a>Özel bir Azure Kubernetes Hizmet kümesi oluşturma

Özel bir kümede, denetim düzlemi veya API sunucusu, [RFC1918 - Özel Internetler için Adres Ayırma](https://tools.ietf.org/html/rfc1918) belgesinde tanımlanan dahili IP adreslerine sahiptir. Özel bir küme kullanarak, API sunucunuz la düğüm havuzlarınız arasındaki ağ trafiğinin yalnızca özel ağda kalmasını sağlayabilirsiniz.

Denetim düzlemi veya API sunucusu, Azure Kubernetes Hizmeti (AKS) tarafından yönetilen bir Azure aboneliğindedir. Müşterinin küme veya düğüm havuzu müşterinin aboneliğindedir. Sunucu ve küme veya düğüm havuzu, API sunucusu sanal ağındaki [Azure Özel Bağlantı hizmeti][private-link-service] ve müşterinin AKS kümesinin alt ağında açıkta kalan özel bir bitiş noktası aracılığıyla birbirleriyle iletişim kurabilir.

## <a name="prerequisites"></a>Ön koşullar

* Azure CLI sürüm 2.2.0 veya sonrası

## <a name="create-a-private-aks-cluster"></a>Özel BIR AKS kümesi oluşturma

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir kaynak grubu oluşturun veya AKS kümeniz için varolan bir kaynak grubu kullanın.

```azurecli-interactive
az group create -l westus -n MyResourceGroup
```

### <a name="default-basic-networking"></a>Varsayılan temel ağ 

```azurecli-interactive
az aks create -n <private-cluster-name> -g <private-cluster-resource-group> --load-balancer-sku standard --enable-private-cluster  
```
Nerede *--enable-private-cluster* özel bir küme için zorunlu bir bayraktır. 

### <a name="advanced-networking"></a>Gelişmiş ağ  

```azurecli-interactive
az aks create \
    --resource-group <private-cluster-resource-group> \
    --name <private-cluster-name> \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id <subnet-id> \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 
```
Nerede *--enable-private-cluster* özel bir küme için zorunlu bir bayraktır. 

> [!NOTE]
> Docker köprüsü adresi CIDR (172.17.0.1/16) alt ağı CIDR ile çakışıyorsa, Docker köprü adresini uygun şekilde değiştirin.

## <a name="options-for-connecting-to-the-private-cluster"></a>Özel kümeye bağlanma seçenekleri

API sunucu bitiş noktasının ortak IP adresi yoktur. API sunucusunu yönetmek için AKS kümesinin Azure Sanal Ağı'na (VNet) erişimi olan bir VM kullanmanız gerekir. Özel kümeye ağ bağlantısı oluşturmak için çeşitli seçenekler vardır.

* AKS kümesiyle aynı Azure Sanal Ağ'da (VNet) bir VM oluşturun.
* Ayrı bir ağda VM kullanın ve [Sanal ağ eşlemi][virtual-network-peering]ayarlayın.  Bu seçenek hakkında daha fazla bilgi için aşağıdaki bölüme bakın.
* Ekspres [Rota veya VPN][express-route-or-VPN] bağlantısı kullanın.

AKS kümesiyle aynı VNET'te VM oluşturmak en kolay seçenektir.  Express Route ve VPN'ler maliyet ekler ve ek ağ karmaşıklığı gerektirir.  Sanal ağ eşleme, çakışan aralıklar olmadığından emin olmak için ağ CIDR aralıklarınızı planlamanızı gerektirir.

## <a name="virtual-network-peering"></a>Sanal ağ eşleme

Belirtildiği gibi, VNet eşleme özel kümenize erişmek için bir yoldur. VNet eşlemi kullanmak için sanal ağ ve özel DNS bölgesi arasında bir bağlantı kurmanız gerekir.
    
1. Azure portalındaki MC_* kaynak grubuna gidin.  
2. Özel DNS bölgesini seçin.   
3. Sol bölmede **Sanal ağ** bağlantısını seçin.  
4. Özel DNS bölgesine VM'nin sanal ağını eklemek için yeni bir bağlantı oluşturun. DNS bölge bağlantısının kullanılabilir olması birkaç dakika sürer.  
5. Azure portalındaki MC_* kaynak grubuna geri dön.  
6. Sağ bölmede sanal ağı seçin. Sanal ağ adı *aks-vnet şeklindedir.\**  
7. Sol **bölmede, Eşler'i**seçin.  
8. **Ekle,** VM'nin sanal ağını ekleyin ve ardından eşlemi oluşturun'u seçin.  
9. VM'nin bulunduğu sanal ağa gidin, **Peerings'i**seçin, AKS sanal ağını seçin ve ardından eşlemeoluşturun. Adres AKS sanal ağında ve VM'nin sanal ağ çakışmasında aralıkları varsa, bakan başarısız olur. Daha fazla bilgi için [Sanal ağ eşleme'ye][virtual-network-peering]bakın.

## <a name="dependencies"></a>Bağımlılıklar  

* Özel Bağlantı hizmeti yalnızca Standart Azure Yük Dengeleyicisi'nde desteklenir. Temel Azure Yük Dengeleyicisi desteklenmez.  
* Özel bir DNS sunucusu kullanmak için Azure DNS IP 168.63.129.16'yı özel DNS sunucusundaki yukarı Akım DNS sunucusu olarak ekleyin.

## <a name="limitations"></a>Sınırlamalar 
* IP yetkili aralıkları özel api sunucu bitiş noktasına uygulanamaz, bunlar yalnızca genel API sunucusuna uygulanır
* Kullanılabilirlik Bölgeleri şu anda belirli bölgeler için desteklenir, bu belgenin başlangıcına bakın 
* [Azure Özel Bağlantı hizmeti sınırlamaları,][private-link-service] şu anda aynı sanal ağda desteklenmeyen özel kümeler, Azure özel uç noktaları ve sanal ağ hizmeti bitiş noktaları için geçerlidir.
* Özel bir Azure sanal ağında özel Azure Kapsayıcı Örnekleri (ACI) döndürmek için özel kümedeki sanal düğümdesteği yok
* Azure DevOps tümleştirmesi için özel kümelerle kutudan destek yok
* Azure Kapsayıcı Kayıt Defteri'nin özel AKS ile çalışmasını sağlaması gereken müşteriler için, Konteyner Kayıt Defteri sanal ağının aracı kümesi sanal ağıyla birlikte bakılması gerekir.
* Azure Dev Spaces için geçerli destek yok
* Varolan AKS kümelerini özel kümelere dönüştürmek için destek yok
* Müşteri alt netindeki özel bitiş noktasının silmesi veya değiştirilmesi kümenin çalışmasını durdurur. 
* Kapsayıcılar için Azure Monitor Canlı Veriler şu anda desteklenmez.


<!-- LINKS - internal -->
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[private-link-service]: /azure/private-link/private-link-service-overview
[virtual-network-peering]: ../virtual-network/virtual-network-peering-overview.md
[azure-bastion]: ../bastion/bastion-create-host-portal.md
[express-route-or-vpn]: ../expressroute/expressroute-about-virtual-network-gateways.md

