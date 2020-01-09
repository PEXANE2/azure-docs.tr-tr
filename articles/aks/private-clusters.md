---
title: Özel Azure Kubernetes hizmet kümesi
description: Özel bir Azure Kubernetes hizmeti (AKS) kümesi oluşturmayı öğrenin
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 12/10/2019
ms.author: mlearned
ms.openlocfilehash: 8af0f998df2a92e51078a2e23806cca07ff08ca3
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75480092"
---
# <a name="public-preview---private-azure-kubernetes-service-cluster"></a>Genel önizleme-özel Azure Kubernetes hizmet kümesi

Özel kümede, denetim düzlemi/API sunucusu [RFC1918](https://tools.ietf.org/html/rfc1918)içinde TANıMLı iç IP adreslerine sahip olur.  Özel bir küme kullanarak, API sunucunuz ve düğüm havuzlarınız arasındaki ağ trafiğinin yalnızca özel ağ üzerinde kalmasını sağlayabilirsiniz.

AKS tarafından yönetilen bir Azure aboneliğinde bulunan denetim düzlemi/API sunucusu ve bir müşteri aboneliğinde bulunan müşteriler kümesi/düğüm havuzu arasındaki iletişim, API sunucusu VNET 'teki [özel bağlantı hizmeti][private-link-service] ve müşteri aks kümesinin alt ağında kullanıma sunulan özel bir uç nokta aracılığıyla birbirleriyle iletişim kurabilir.

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis kabul etme sürecindedir. Önizlemeler, "olduğu gibi" ve "kullanılabilir olarak" verilmiştir ve hizmet düzeyi sözleşmelerinden ve sınırlı garantiden çıkarılır. AKS önizlemeleri, müşteri desteğinin en iyi çaba temelinde kısmen ele alınmıştır. Bu nedenle, bu özellikler üretim kullanımı için tasarlanmamıştır. Ek bilgi için lütfen aşağıdaki destek makalelerine bakın:
>
> * [AKS destek Ilkeleri](support-policies.md)
> * [Azure desteği SSS](faq.md)

## <a name="before-you-begin"></a>Başlamadan önce

* Azure CLı sürüm 2.0.77 veya üzeri ve aks-Preview 0.4.18 uzantısı gerekir

## <a name="current-supported-regions"></a>Desteklenen geçerli bölgeler
* Batı ABD
* Batı ABD 2
* Doğu ABD 2
* Kanada Orta
* Kuzey Avrupa
* Batı Avrupa
* Doğu Avustralya

## <a name="install-latest-aks-cli-preview-extension"></a>En son AKS CLı önizleme uzantısını yükler

Özel kümeleri kullanmak için, *aks-Preview* CLI uzantısının sürüm 0.4.18 veya üzeri olması gerekir. [Az Extension Add][az-extension-add] komutunu kullanarak *aks-Preview* Azure CLI uzantısını yükledikten sonra [az Extension Update][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin::

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```
> [!CAUTION]
> Bir abonelik üzerinde bir özelliği kaydettiğinizde, o özelliği şu anda kaydedemezsiniz. Bazı Önizleme özelliklerini etkinleştirdikten sonra, daha sonra abonelikte oluşturulan tüm AKS kümeleri için varsayılanlar kullanılabilir. Üretim aboneliklerinde Önizleme özelliklerini etkinleştirmeyin. Önizleme özelliklerini test etmek ve geri bildirim toplamak için ayrı bir abonelik kullanın.

```azurecli-interactive
az feature register --name AKSPrivateLinkPreview --namespace Microsoft.ContainerService
```

Durumun *kaydı*gösterilmesi birkaç dakika sürebilir. [Az Feature List][az-feature-list] komutunu kullanarak kayıt durumunu denetleyebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSPrivateLinkPreview')].{Name:name,State:properties.state}"
```

Durum kaydedildiğinde, [az Provider Register][az-provider-register] komutunu kullanarak *Microsoft. Containerservice* kaynak sağlayıcısı kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
az provider register --namespace Microsoft.Network
```
## <a name="create-a-private-aks-cluster"></a>Özel AKS kümesi oluşturma

#### <a name="default-basic-networking"></a>Varsayılan temel ağ 

```azurecli-interactive
az aks create -n <private-cluster-name> -g <private-cluster-resource-group> --load-balancer-sku standard --enable-private-cluster  
```
Burada--Enable-Private-Cluster, özel bir küme için zorunlu bir bayrak 

#### <a name="advanced-networking"></a>Gelişmiş ağ  

```azurecli-interactive
az aks create \ 
    --resource-group <private-cluster-resource-group>\ 
    --name <private-cluster-name> \ 
    --load-balancer-sku standard
    --enable-private-cluster 
    --network-plugin azure \ 
    --vnet-subnet-id <subnet-id> \ 
    --docker-bridge-address 172.17.0.1/16 \ 
    --dns-service-ip 10.2.0.10 \ 
    --service-cidr 10.2.0.0/24 \ 
```
Burada--Enable-Private-Cluster, özel bir küme için zorunlu bir bayrak 

## <a name="steps-to-connect-to-the-private-cluster"></a>Özel kümeye bağlanma adımları
API sunucusu bitiş noktasının genel IP adresi yok. Sonuç olarak, kullanıcıların bir sanal ağda Azure sanal makinesi oluşturması ve API sunucusuna bağlanması gerekir. İçindeki adımlar

* Kümeye bağlanmak için kimlik bilgilerini alın

   ```azurecli-interactive
   az aks get-credentials --name MyManagedCluster --resource-group MyResourceGroup
   ```
* AKS kümesiyle aynı VNET 'te bir VM oluşturun veya farklı bir VNET 'te VM oluşturun ve bu VNET 'i AKS kümesi VNET ile eşler
* Farklı bir VNET 'te VM oluşturursanız, bu VNET ve Özel DNS bölgesi arasında bir bağlantı ayarlamanız gerekir
    * Portalda MC_ * kaynak grubuna gidin 
    * Özel DNS bölgesine tıklayın 
    * Sol bölmedeki sanal ağ bağlantısını seçin
    * VM 'nin VNET 'i Özel DNS bölgesine eklemek için yeni bir bağlantı oluşturun *(DNS bölgesi bağlantısının kullanılabilir olması birkaç dakika sürer)*
* VM 'ye SSH
* Kubectl aracını yükleyip Kubectl komutlarını çalıştırın

## <a name="dependencies"></a>Bağımlılıklar  
* Yalnızca standart LB-temel yük dengeleyici desteği yok  

## <a name="limitations"></a>Sınırlamalar 
* Aynı [Azure özel bağlantı hizmeti sınırlamaları][private-link-service] özel kümeler için geçerlidir, Azure özel uç noktaları ve sanal ağ hizmeti uç noktaları şu anda aynı VNET 'te desteklenmemektedir
* Özel bir kümede özel bir Azure VNET 'te özel acı örneklerini döndürme için sanal düğüm desteği yok
* Özel kümeler ile kutudan Azure DevOps tümleştirmesi desteği yok
* Müşterilerin ACR 'nin özel AKS ile çalışmasına ihtiyacı varsa, ACR 'nin VNET 'in aracı küme VNET 'i ile eşlenmiş olması gerekir
* Azure Dev Spaces için geçerli destek yok
* Mevcut AKS kümelerini özel kümelere dönüştürme desteği yok  
* Müşteri alt ağındaki özel uç noktasını silmek veya değiştirmek kümenin çalışmayı durdurmasına neden olur 
* Kapsayıcılar için Azure Izleyici canlı veriler şu anda desteklenmiyor
* Kendi DNS 'nizi getir şu anda desteklenmiyor


<!-- LINKS - internal -->
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[private-link-service]: https://docs.microsoft.com/azure/private-link/private-link-service-overview
