---
title: Özel bir Azure Kubernetes hizmet kümesi oluşturma
description: Özel bir Azure Kubernetes hizmeti (AKS) kümesi oluşturmayı öğrenin
services: container-service
ms.topic: article
ms.date: 2/21/2020
ms.openlocfilehash: b8b4f8062d9f60648e22ab4eb0be78eb47159834
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79205180"
---
# <a name="create-a-private-azure-kubernetes-service-cluster"></a>Özel bir Azure Kubernetes hizmet kümesi oluşturma

Özel bir kümede, denetim düzlemi veya API sunucusu, [Özel Internetler belgesi Için RFC1918-Address ayırması](https://tools.ietf.org/html/rfc1918) içinde tanımlanan iç IP adreslerine sahiptir. Özel bir küme kullanarak, API sunucunuz ve düğüm havuzlarınız arasındaki ağ trafiğinin yalnızca özel ağ üzerinde kalmasını sağlayabilirsiniz.

Denetim düzlemi veya API sunucusu, Azure Kubernetes hizmeti (AKS) tarafından yönetilen bir Azure aboneliğinde bulunur. Müşterinin kümesi veya düğüm havuzu müşterinin aboneliğine ait. Sunucu ve küme veya düğüm havuzu, API sunucusu sanal ağındaki [Azure özel bağlantı hizmeti][private-link-service] ve müşterinin aks kümesinin alt ağında kullanıma sunulan özel bir uç nokta aracılığıyla birbirleriyle iletişim kurabilir.

## <a name="prerequisites"></a>Önkoşullar

* Azure CLı sürüm 2.2.0 veya üzeri

## <a name="create-a-private-aks-cluster"></a>Özel AKS kümesi oluşturma

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

AKS kümeniz için bir kaynak grubu oluşturun veya var olan bir kaynak grubunu kullanın.

```azurecli-interactive
az group create -l westus -n MyResourceGroup
```

### <a name="default-basic-networking"></a>Varsayılan temel ağ 

```azurecli-interactive
az aks create -n <private-cluster-name> -g <private-cluster-resource-group> --load-balancer-sku standard --enable-private-cluster  
```
Burada *--Enable-Private-Cluster* , özel bir küme için zorunlu bir bayrak. 

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
Burada *--Enable-Private-Cluster* , özel bir küme için zorunlu bir bayrak. 

> [!NOTE]
> Docker Köprüsü CıDR (172.17.0.1/16) alt ağ CıDR ile çakışıyor, Docker köprü adresini uygun şekilde değiştirin.

## <a name="options-for-connecting-to-the-private-cluster"></a>Özel kümeye bağlanma seçenekleri

API sunucusu uç noktasının genel IP adresi yok. API sunucusunu yönetmek için, AKS kümesinin Azure sanal ağına (VNet) erişimi olan bir VM kullanmanız gerekir. Özel kümeye Ağ bağlantısı kurmak için çeşitli seçenekler vardır.

* AKS kümesiyle aynı Azure sanal ağında (VNet) bir VM oluşturun.
* Ayrı bir ağda bir VM kullanın ve [sanal ağ eşlemesi][virtual-network-peering]ayarlayın.  Bu seçenek hakkında daha fazla bilgi için aşağıdaki bölüme bakın.
* Bir [Express Route veya VPN][express-route-or-VPN] bağlantısı kullanın.

AKS kümesiyle aynı VNET 'te VM oluşturma en kolay seçenektir.  Express Route ve VPN 'Ler maliyet ekler ve ek ağ karmaşıklığı gerektirir.  Sanal ağ eşlemesi, çakışan aralıklar bulunmadığından emin olmak için ağ CıDR aralıklarını planlamanız gerekir.

## <a name="virtual-network-peering"></a>Sanal ağ eşleme

Belirtildiği gibi, VNet eşlemesi özel kümenize erişmenin bir yoludur. VNet eşlemesini kullanmak için sanal ağ ile özel DNS bölgesi arasında bir bağlantı ayarlamanız gerekir.
    
1. Azure portal MC_ * kaynak grubuna gidin.  
2. Özel DNS bölgesini seçin.   
3. Sol bölmede **sanal ağ** bağlantısını seçin.  
4. VM 'nin sanal ağını özel DNS bölgesine eklemek için yeni bir bağlantı oluşturun. DNS bölgesi bağlantısının kullanılabilir olması birkaç dakika sürer.  
5. Azure portal MC_ * kaynak grubuna geri dönün.  
6. Sağ bölmede sanal ağı seçin. Sanal ağ adı *aks-VNET-\** biçimindedir.  
7. Sol bölmede, eşlemeler ' i **seçin.**  
8. **Ekle**' yi SEÇIN, VM 'nin sanal ağını ekleyin ve ardından eşlemeyi oluşturun.  
9. VM 'nin bulunduğu sanal ağa gidin, **eşlemeler ' i seçin,** aks sanal ağını seçin ve ardından eşlemeyi oluşturun. AKS sanal ağındaki adres aralıkları ve VM 'nin sanal ağ çakışması, eşleme başarısız olur. Daha fazla bilgi için bkz. [sanal ağ eşlemesi][virtual-network-peering].

## <a name="dependencies"></a>Bağımlılıklar  

* Özel bağlantı hizmeti yalnızca standart Azure Load Balancer desteklenir. Temel Azure Load Balancer desteklenmez.  
* Özel bir DNS sunucusu kullanmak için, bu IP 168.63.129.16 iletmek üzere DNS ile bir AD sunucusu dağıtın

## <a name="limitations"></a>Sınırlamalar 
* IP yetkili aralıkları özel API sunucusu uç noktasına uygulanamıyor, yalnızca ortak API sunucusu için geçerlidir
* Kullanılabilirlik Alanları Şu anda belirli bölgelerde destekleniyor, bu belgenin başlangıcına bakın 
* [Azure özel bağlantı hizmeti sınırlamaları][private-link-service] , özel kümeler, Azure özel uç noktaları ve sanal ağ hizmeti uç noktaları için geçerlidir; bu, şu anda aynı sanal ağda desteklenmemektedir.
* Özel bir kümede özel bir Azure sanal ağında özel Azure Container Instances (ACI) döndürme için sanal düğümler desteklenmez
* Özel kümeler ile kutudan Azure DevOps tümleştirmesi desteği yok
* Azure Container Registry özel AKS ile çalışmak üzere etkinleştirmesi gereken müşteriler için, Container Registry sanal ağı aracı kümesi sanal ağıyla eşlenmelidir.
* Azure Dev Spaces için geçerli destek yok
* Mevcut AKS kümelerini özel kümelere dönüştürme desteği yok
* Müşteri alt ağındaki özel uç noktasını silmek veya değiştirmek kümenin çalışmayı durdurmasına neden olur. 
* Kapsayıcılar için Azure Izleyici canlı veriler şu anda desteklenmiyor.


<!-- LINKS - internal -->
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[private-link-service]: /private-link/private-link-service-overview
[virtual-network-peering]: ../virtual-network/virtual-network-peering-overview.md
[azure-bastion]: ../bastion/bastion-create-host-portal.md
[express-route-or-vpn]: ../expressroute/expressroute-about-virtual-network-gateways.md

