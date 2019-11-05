---
title: Azure Kubernetes hizmetinde (AKS) API sunucusu yetkilendirilmiş IP aralıkları
description: Azure Kubernetes Service (AKS) ' de API sunucusuna erişim için bir IP adresi aralığı kullanarak kümenizin güvenliğini nasıl sağlayacağınızı öğrenin
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: mlearned
ms.openlocfilehash: 8418499cc3e094162ac7483aaa6c71e74db95ae1
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73472970"
---
# <a name="secure-access-to-the-api-server-using-authorized-ip-address-ranges-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) içindeki yetkili IP adresi aralıklarını kullanarak API sunucusuna güvenli erişim

Kubernetes 'de, API sunucusu, küme içinde kaynak oluşturmak veya düğüm sayısını ölçeklendirmek gibi eylemleri gerçekleştirmek için istekleri alır. API sunucusu, bir küme ile etkileşimde bulunmak ve yönetmek için merkezi bir yoldur. Küme güvenliğini artırmak ve saldırıları en aza indirmek için, API sunucusuna yalnızca sınırlı bir IP adresi aralığı kümesinden erişilebilir olması gerekir.

Bu makalede, API sunucusu yetkilendirilmiş IP adresi aralıklarının hangi IP adreslerinin ve Cıdrs 'nin denetim düzlemine erişebileceğini sınırlamak için nasıl kullanılacağı gösterilmektedir.

> [!IMPORTANT]
> Yeni kümelerde, API sunucusu yetkilendirilmiş IP adresi aralıkları yalnızca *Standart* SKU yük dengeleyicide desteklenir. *Temel* SKU yük dengeleyiciye ve yapılandırılmış API sunucusu yetkilendirilmiş IP adresi aralıklarına sahip mevcut kümeler, olduğu gibi çalışmaya devam edecektir. Mevcut clusers da yükseltilebilir ve bunlar çalışmaya devam edecektir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, [Kubernetes kullanan][kubenet]kullanan kümelerle çalıştığınızdan varsayılmaktadır.  [Azure Container Networking Interface (CNı)][cni-networking] tabanlı kümeler sayesinde, erişimi güvenli hale getirmek için gerekli yol tablosuna sahip değilsiniz.  Yol tablosunu el ile oluşturmanız gerekir.  Daha fazla bilgi için bkz. [Rota tablolarını yönetme](https://docs.microsoft.com/azure/virtual-network/manage-route-table) .

API sunucusu yetkilendirilmiş IP aralıkları yalnızca sizin oluşturduğunuz yeni AKS kümelerinde çalışır. Bu makalede, Azure CLı kullanarak bir AKS kümesinin nasıl oluşturulacağı gösterilmektedir.

Azure CLı sürüm 2.0.76 veya sonraki bir sürümün yüklü ve yapılandırılmış olması gerekir. Sürümü bulmak için `az --version` çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek][install-azure-cli].

## <a name="limitations"></a>Sınırlamalar

API sunucusu yetkilendirilmiş IP aralıklarını yapılandırdığınızda aşağıdaki sınırlamalar geçerlidir:

* API sunucusuyla iletişim de engellendiğinden Azure Dev Spaces Şu anda kullanamazsınız.

## <a name="overview-of-api-server-authorized-ip-ranges"></a>API sunucusu yetkilendirilmiş IP aralıklarına genel bakış

Kubernetes API sunucusu, temeldeki Kubernetes API 'Lerinin nasıl açığa çıkmasıdır. Bu bileşen, `kubectl` veya Kubernetes panosu gibi yönetim araçları için etkileşim sağlar. AKS, adanmış bir API sunucusuyla tek kiracılı bir Küme Yöneticisi sağlar. Varsayılan olarak, API sunucusuna bir genel IP adresi atanır ve rol tabanlı erişim denetimleri (RBAC) kullanarak erişimi kontrol etmelisiniz.

Genel olarak erişilebilen AKS denetim düzlemi/API sunucusuna erişimi güvenli hale getirmek için, yetkilendirilmiş IP aralıklarını etkinleştirebilir ve kullanabilirsiniz. Bu yetkili IP aralıkları yalnızca tanımlı IP adresi aralıklarının API sunucusuyla iletişim kurmasına izin verir. Bu yetkili IP aralıklarının parçası olmayan bir IP adresinden API sunucusuna yapılan istek engellenir. Daha sonra kullanıcıları ve istediği eylemleri yetkilendirmek için RBAC kullanmaya devam etmelisiniz.

API sunucusu ve diğer küme bileşenleri hakkında daha fazla bilgi için bkz. [AKS Için Kubernetes temel kavramları][concepts-clusters-workloads].

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

API sunucusu yetkilendirilmiş IP aralıkları yalnızca yeni AKS kümelerinde çalışır. Küme oluşturma işleminin parçası olarak yetkili IP aralıklarını etkinleştiremezsiniz. Küme oluşturma işleminin parçası olarak yetkili IP aralıklarını etkinleştirmeye çalışırsanız, çıkış IP adresi bu noktada tanımlanmadığı için küme düğümleri dağıtım sırasında API sunucusuna erişemez.

İlk olarak, [az aks Create][az-aks-create] komutunu kullanarak bir küme oluşturun. Aşağıdaki örnek, *Myresourcegroup*adlı kaynak grubunda *Myakscluster* adlı tek düğümlü bir küme oluşturur.

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location eastus

# Create an AKS cluster
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --vm-set-type VirtualMachineScaleSets \
    --load-balancer-sku standard \
    --generate-ssh-keys

# Get credentials to access the cluster
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="update-cluster-with-authorized-ip-ranges"></a>Kümeyi yetkilendirilmiş IP aralıklarıyla Güncelleştir

Varsayılan olarak, kümeniz, giden ağ geçidini yapılandırmak için kullanabileceğiniz [Standart SKU yük dengeleyiciyi][standard-sku-lb]kullanır. [Az Network public-ip List][az-network-public-ip-list] kullanın ve genellikle *Mc_* ile başlayan aks kümenizin kaynak grubunu belirtin. Bu, kümenizin genel IP 'sini görüntüler ve bu, izin verebilir. [Az aks Update][az-aks-update] komutunu kullanın ve kümenin IP 'sine izin vermek için *--api-Server-yetkilendirilmiş-IP aralıkları* parametresini belirtin. Örneğin:

```azurecli-interactive
RG=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
SLB_PublicIP=$(az network public-ip list --resource-group $RG --query [].ipAddress -o tsv)
az aks update --api-server-authorized-ip-ranges $SLB_PublicIP --resource-group myResourceGroup --name myAKSCluster
```

API sunucusu yetkilendirilmiş IP aralıklarını etkinleştirmek için [az aks Update][az-aks-update] komutunu kullanın ve yetkili IP adresi aralıklarının listesini sağlamak için *--API-Server-yetkilendirilmiş-IP aralıkları* parametresini belirtin. Bu IP adresi aralıkları genellikle şirket içi ağlarınızda veya genel IP 'Lerde kullanılan adres aralıklarından oluşur. Bir CıDR aralığı belirttiğinizde, aralıktaki ilk IP adresi ile başlayın. Örneğin, *137.117.106.90/29* geçerli bir aralıktır, ancak ARALıKTAKI ilk IP adresini *137.117.106.88/29*gibi belirttiğinizden emin olun.

Aşağıdaki örnek, *Myresourcegroup*adlı kaynak grubunda *Myakscluster* adlı kümede bulunan API sunucusu yetkili IP aralıklarını mümkün bir şekilde sunar. Yetkilendirmede kullanılacak IP adresi aralıkları *172.0.0.0/16* (pod/Nodes adres aralığı) ve *168.10.0.0/18* (ServiceCidr):

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges 172.0.0.0/16,168.10.0.0/18
```

> [!NOTE]
> Bu aralıkları bir izin verilenler listesine eklemeniz gerekir:
> - Güvenlik Duvarı genel IP adresi
> - Hizmet CıDR
> - Alt ağların, düğümler ve pod ile adres aralığı
> - Kümeyi yönettiğiniz ağları temsil eden herhangi bir Aralık

## <a name="disable-authorized-ip-ranges"></a>Yetkili IP aralıklarını devre dışı bırak

Yetkili IP aralıklarını devre dışı bırakmak için [az aks Update][az-aks-update] kullanın ve API sunucusu yetkilendirilmiş IP aralıklarını devre dışı bırakmak için boş bir Aralık belirtin. Örneğin:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges ""
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, API sunucusu yetkilendirilmiş IP aralıklarını etkinleştirdiniz. Bu yaklaşım, güvenli bir AKS kümesini nasıl çalıştıracağınızı gösteren bir parçasıdır.

Daha fazla bilgi için bkz. [aks 'teki uygulamalar ve kümeler Için güvenlik kavramları][concepts-security] ve [aks 'de küme güvenliği ve yükseltmeleri için en iyi yöntemler][operator-best-practices-cluster-security].

<!-- LINKS - external -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/#kubenet

<!-- LINKS - internal -->
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[concepts-clusters-workloads]: concepts-clusters-workloads.md
[concepts-security]: concepts-security.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-cluster-security]: operator-best-practices-cluster-security.md
[standard-sku-lb]: load-balancer-standard.md
