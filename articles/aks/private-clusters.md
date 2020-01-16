---
title: Özel bir Azure Kubernetes hizmet kümesi oluşturma
description: Özel bir Azure Kubernetes hizmeti (AKS) kümesi oluşturmayı öğrenin
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 12/10/2019
ms.author: mlearned
ms.openlocfilehash: e01579272878a2436abca2ee50f0f6ea6cf78cbf
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76045600"
---
# <a name="create-a-private-azure-kubernetes-service-cluster-preview"></a>Özel bir Azure Kubernetes hizmet kümesi oluşturma (Önizleme)

Özel bir kümede, denetim düzlemi veya API sunucusu, [Özel Internetler belgesi Için RFC1918-Address ayırması](https://tools.ietf.org/html/rfc1918) içinde tanımlanan iç IP adreslerine sahiptir. Özel bir küme kullanarak, API sunucunuz ve düğüm havuzlarınız arasındaki ağ trafiğinin yalnızca özel ağ üzerinde kalmasını sağlayabilirsiniz.

Denetim düzlemi veya API sunucusu, Azure Kubernetes hizmeti (AKS) tarafından yönetilen bir Azure aboneliğinde bulunur. Müşterinin kümesi veya düğüm havuzu müşterinin aboneliğine ait. Sunucu ve küme veya düğüm havuzu, API sunucusu sanal ağındaki [Azure özel bağlantı hizmeti][private-link-service] ve müşterinin aks kümesinin alt ağında kullanıma sunulan özel bir uç nokta aracılığıyla birbirleriyle iletişim kurabilir.

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis ' dir ve bir katılım temelinde sunulur. Önizlemeler *,* ve *kullanılabilir* olarak sağlanır ve hızmet düzeyi sözleşmesi (SLA) ve sınırlı garantiden çıkarılır. AKS önizlemeleri, müşteri desteğinin *en iyi çaba* temelinde kısmen ele alınmıştır. Bu nedenle, Özellikler üretim kullanımı için tasarlanmamıştır. Daha fazla bilgi için aşağıdaki destek makalelerine bakın:
>
> * [AKS destek Ilkeleri](support-policies.md)
> * [Azure desteği SSS](faq.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure CLı sürüm 2.0.77 veya üzeri ve Azure CLı AKS önizleme uzantısı sürüm 0.4.18

## <a name="currently-supported-regions"></a>Şu anda desteklenen bölgeler
* Batı ABD
* Batı ABD 2
* Doğu ABD 2
* Kanada Orta
* Kuzey Avrupa
* Batı Avrupa
* Doğu Avustralya

## <a name="install-the-latest-azure-cli-aks-preview-extension"></a>En son Azure CLı AKS önizleme uzantısını yükler

Özel kümeleri kullanmak için Azure CLı AKS önizleme uzantısı sürüm 0.4.18 veya sonraki bir sürümü gerekir. [Az Extension Add][az-extension-add] komutunu kullanarak Azure CLI aks önizleme uzantısını yükledikten sonra aşağıdaki [az Extension Update][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```
> [!CAUTION]
> Bir abonelik üzerinde bir özelliği kaydettiğinizde, o özelliği şu anda kaydedemezsiniz. Bazı Önizleme özelliklerini etkinleştirdikten sonra, abonelikte oluşturulan tüm AKS kümeleri için varsayılan ayarları kullanabilirsiniz. Üretim aboneliklerinde Önizleme özelliklerini etkinleştirmeyin. Önizleme özelliklerini test etmek ve geri bildirim toplamak için ayrı bir abonelik kullanın.

```azurecli-interactive
az feature register --name AKSPrivateLinkPreview --namespace Microsoft.ContainerService
```

Kayıt durumunun *kayıtlı*olarak gösterilmesi birkaç dakika sürebilir. Aşağıdaki [az Feature List][az-feature-list] komutunu kullanarak durumu kontrol edebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSPrivateLinkPreview')].{Name:name,State:properties.state}"
```

Durum kaydedildiğinde, aşağıdaki [az Provider Register][az-provider-register] komutunu kullanarak *Microsoft. Containerservice* kaynak sağlayıcısı kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
az provider register --namespace Microsoft.Network
```
## <a name="create-a-private-aks-cluster"></a>Özel AKS kümesi oluşturma

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

## <a name="connect-to-the-private-cluster"></a>Özel kümeye Bağlan
API sunucusu uç noktasının genel IP adresi yok. Sonuç olarak, bir sanal ağda bir Azure sanal makinesi (VM) oluşturmanız ve API sunucusuna bağlanmanız gerekir. Bunu yapmak için aşağıdakileri yapın:

1. Kümeye bağlanmak için kimlik bilgilerini alın.

   ```azurecli-interactive
   az aks get-credentials --name MyManagedCluster --resource-group MyResourceGroup
   ```

1. Aşağıdakilerden birini yapın:
   * AKS kümesiyle aynı sanal ağda bir VM oluşturun.  
   * Farklı bir sanal ağda VM oluşturun ve bu sanal ağı AKS kümesi sanal ağıyla eşler.

     Farklı bir sanal ağda VM oluşturursanız, bu sanal ağ ile özel DNS bölgesi arasında bir bağlantı kurun. Bunu yapmak için:
    
     a. Azure portal MC_ * kaynak grubuna gidin.  
     b. Özel DNS bölgesini seçin.   
     c. Sol bölmede **sanal ağ** bağlantısını seçin.  
     d. VM 'nin sanal ağını özel DNS bölgesine eklemek için yeni bir bağlantı oluşturun. DNS bölgesi bağlantısının kullanılabilir olması birkaç dakika sürer.  
     e. Azure portal MC_ * kaynak grubuna geri dönün.  
     f. Sağ bölmede sanal ağı seçin. Sanal ağ adı *aks-VNET-\** biçimindedir.  
     g. Sol bölmede, eşlemeler ' i **seçin.**  
     h. **Ekle**' yi SEÇIN, VM 'nin sanal ağını ekleyin ve ardından eşlemeyi oluşturun.  
     i. VM 'nin bulunduğu sanal ağa gidin, **eşlemeler ' i seçin,** aks sanal ağını seçin ve ardından eşlemeyi oluşturun. AKS sanal ağındaki adres aralıkları ve VM 'nin sanal ağ çakışması, eşleme başarısız olur. Daha fazla bilgi için bkz. [sanal ağ eşlemesi][virtual-network-peering].

1. VM 'ye Secure Shell (SSH) aracılığıyla erişin.
1. Kubectl aracını yükleyip Kubectl komutlarını çalıştırın.


## <a name="dependencies"></a>Bağımlılıklar  
* Özel bağlantı hizmeti yalnızca standart Azure Load Balancer desteklenir. Temel Azure Load Balancer desteklenmez.  

## <a name="limitations"></a>Sınırlamalar 
* [Azure özel bağlantı hizmeti sınırlamaları][private-link-service] , özel kümeler, Azure özel uç noktaları ve sanal ağ hizmeti uç noktaları için geçerlidir; bu, şu anda aynı sanal ağda desteklenmemektedir.
* Özel bir kümedeki sanal düğümler için özel bir Azure sanal ağındaki özel Cisco uygulama merkezli altyapı (Cisco acı) örneklerini döndürme desteği yoktur.
* Özel kümeler ile kutudan Azure DevOps tümleştirmesi desteklenmez.
* Azure Container Registry özel AKS ile çalışmak üzere etkinleştirmesi gereken müşteriler için, Container Registry sanal ağı aracı kümesi sanal ağıyla eşlenmelidir.
* Azure Dev Spaces için geçerli destek yok.
* Mevcut AKS kümelerini özel kümelere dönüştürmeye yönelik destek yoktur.  
* Müşteri alt ağındaki özel uç noktasını silmek veya değiştirmek kümenin çalışmayı durdurmasına neden olur. 
* Kapsayıcılar için Azure Izleyici canlı veriler şu anda desteklenmiyor.
* *Kendı DNS 'Nizi getir* Şu anda desteklenmiyor.


<!-- LINKS - internal -->
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[private-link-service]: https://docs.microsoft.com/azure/private-link/private-link-service-overview
[virtual-network-peering]: ../virtual-network/virtual-network-peering-overview.md

