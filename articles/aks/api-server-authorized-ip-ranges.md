---
title: Azure Kubernetes hizmetinde (AKS) API sunucusu yetkilendirilmiş IP aralıkları
description: Azure Kubernetes Service (AKS) ' de API sunucusuna erişim için bir IP adresi aralığı kullanarak kümenizin güvenliğini nasıl sağlayacağınızı öğrenin
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: mlearned
ms.openlocfilehash: 487940bfb5d6e7c5eebf99f804f57c3e17709377
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70276494"
---
# <a name="preview---secure-access-to-the-api-server-using-authorized-ip-address-ranges-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) ' de yetkili IP adresi aralıklarını kullanarak API sunucusuna önizleme-güvenli erişim

Kubernetes 'de, API sunucusu, küme içinde kaynak oluşturmak veya düğüm sayısını ölçeklendirmek gibi eylemleri gerçekleştirmek için istekleri alır. API sunucusu, bir küme ile etkileşimde bulunmak ve yönetmek için merkezi bir yoldur. Küme güvenliğini artırmak ve saldırıları en aza indirmek için, API sunucusuna yalnızca sınırlı bir IP adresi aralığı kümesinden erişilebilir olması gerekir.

Bu makalede, API sunucusu tarafından yetkilendirilmiş IP adresi aralıklarının, düzlemi denetlemek için istekleri sınırlamak üzere nasıl kullanılacağı gösterilir. Bu özellik şu anda önizleme sürümündedir.

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis kabul etme sürecindedir. Önizlemeler, "olduğu gibi" ve "kullanılabilir olarak" verilmiştir ve hizmet düzeyi sözleşmelerinden ve sınırlı garantiden çıkarılır. AKS önizlemeleri, müşteri desteğinin en iyi çaba temelinde kısmen ele alınmıştır. Bu nedenle, bu özellikler üretim kullanımı için tasarlanmamıştır. Ek bilgi için lütfen aşağıdaki destek makalelerine bakın:
>
> * [AKS destek Ilkeleri][aks-support-policies]
> * [Azure desteği SSS][aks-faq]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makale, [kubenet] [kubenet] kullanan kümelerle çalışıyorassumess.  [Azure Container Networking Interface (CNı)] [CNI-Networking] tabanlı kümeler ile, erişimi güvenli hale getirmek için gerekli yol tablosuna sahip değilsiniz.  Yol tablosunu el ile oluşturmanız gerekir.  Daha fazla bilgi için bkz. [Rota tablolarını yönetme](https://docs.microsoft.com/azure/virtual-network/manage-route-table) .

API sunucusu yetkilendirilmiş IP aralıkları yalnızca sizin oluşturduğunuz yeni AKS kümelerinde çalışır. Bu makalede, Azure CLı kullanarak bir AKS kümesinin nasıl oluşturulacağı gösterilmektedir.

Azure CLı sürüm 2.0.61 veya sonraki bir sürümün yüklü ve yapılandırılmış olması gerekir. Sürümü `az --version` bulmak için ' i çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Aks-Preview CLı uzantısını yükler

API sunucusu yetkilendirilmiş IP aralıklarını yapılandırmak için, *aks-Preview* CLI uzantısının sürüm 0.4.1 veya daha yüksek olması gerekir. [Az Extension Add][az-extension-add] komutunu kullanarak *aks-Preview* Azure CLI uzantısını yükledikten sonra [az Extension Update][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-feature-flag-for-your-subscription"></a>Aboneliğiniz için özellik bayrağını Kaydet

API sunucusu yetkilendirilmiş IP aralıklarını kullanmak için, öncelikle aboneliğinizde bir özellik bayrağını etkinleştirin. *Apıversecuritypreview* Özellik bayrağını kaydetmek için, aşağıdaki örnekte gösterildiği gibi [az Feature Register][az-feature-register] komutunu kullanın:

> [!CAUTION]
> Bir abonelik üzerinde bir özelliği kaydettiğinizde, o özelliği şu anda kaydedemezsiniz. Bazı Önizleme özelliklerini etkinleştirdikten sonra, daha sonra abonelikte oluşturulan tüm AKS kümeleri için varsayılanlar kullanılabilir. Üretim aboneliklerinde Önizleme özelliklerini etkinleştirmeyin. Önizleme özelliklerini test etmek ve geri bildirim toplamak için ayrı bir abonelik kullanın.

```azurecli-interactive
az feature register --name APIServerSecurityPreview --namespace Microsoft.ContainerService
```

Durumun *kayıtlı*gösterilmesi birkaç dakika sürer. [Az Feature List][az-feature-list] komutunu kullanarak kayıt durumunu denetleyebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/APIServerSecurityPreview')].{Name:name,State:properties.state}"
```

Hazırlandığınızda, [az Provider Register][az-provider-register] komutunu kullanarak *Microsoft. Containerservice* kaynak sağlayıcısı kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

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
    --generate-ssh-keys

# Get credentials to access the cluster
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="create-outbound-gateway-for-firewall-rules"></a>Güvenlik duvarı kuralları için giden ağ geçidi oluşturma

Bir sonraki bölümde yetkili IP aralıklarını etkinleştirdiğinizde bir kümedeki düğümlerin, API sunucusuyla güvenilir bir şekilde iletişim kurabildiğinden emin olmak için, giden ağ geçidi olarak kullanılmak üzere bir Azure Güvenlik duvarı oluşturun. Daha sonra Azure Güvenlik duvarının IP adresi, sonraki bölümde yetkili API sunucusu IP adresleri listesine eklenir.

> [!WARNING]
> Azure Güvenlik duvarı kullanımı, aylık bir fatura döngüsüne göre önemli maliyetler oluşturabilir. Azure Güvenlik Duvarı 'nı kullanma gereksinimi yalnızca bu ilk önizleme döneminde gerekli olmalıdır. Daha fazla bilgi ve maliyet planlaması için bkz. [Azure Güvenlik Duvarı fiyatlandırması][azure-firewall-costs].
>
> Alternatif olarak, kümeniz [Standart SKU yük dengeleyiciyi][standard-sku-lb]kullanıyorsa, Azure Güvenlik duvarını giden ağ geçidi olarak yapılandırmanız gerekmez. [Az Network public-ip List][az-network-public-ip-list] kullanın ve genellikle *Mc_* ile başlayan aks kümenizin kaynak grubunu belirtin. Bu, kümeniz için ortak IP 'yi görüntüler; bu, beyaz listeye eklenebilir. Örneğin:
>
> ```azurecli-interactive
> RG=$(az aks show --resource-group myResourceGroup --name myAKSClusterSLB --query nodeResourceGroup -o tsv)
> SLB_PublicIP=$(az network public-ip list --resource-group $RG --query [].ipAddress -o tsv)
> az aks update --api-server-authorized-ip-ranges $SLB_PublicIP --resource-group myResourceGroup --name myAKSClusterSLB
> ```

İlk olarak, AKS kümesi ve sanal ağ için *Mc_* kaynak grubu adını alın. Ardından, [az Network VNET subnet Create][az-network-vnet-subnet-create] komutunu kullanarak bir alt ağ oluşturun. Aşağıdaki örnek, *10.200.0.0/16*CIDR aralığı ile *AzureFirewallSubnet* adlı bir alt ağ oluşturur:

```azurecli-interactive
# Get the name of the MC_ cluster resource group
MC_RESOURCE_GROUP=$(az aks show \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --query nodeResourceGroup -o tsv)

# Get the name of the virtual network used by the cluster
VNET_NAME=$(az network vnet list \
    --resource-group $MC_RESOURCE_GROUP \
    --query [0].name -o tsv)

# Create a subnet in the virtual network for Azure Firewall
az network vnet subnet create \
    --resource-group $MC_RESOURCE_GROUP \
    --vnet-name $VNET_NAME \
    --name AzureFirewallSubnet \
    --address-prefixes 10.200.0.0/16
```

Azure Güvenlik duvarı oluşturmak için [az Extension Add][az-extension-add] komutunu kullanarak *Azure-Firewall* CLI uzantısını yüklemelisiniz. Ardından, [az Network Firewall Create][az-network-firewall-create] komutunu kullanarak bir güvenlik duvarı oluşturun. Aşağıdaki örnek, *Myazurefirewall*adlı bir Azure Güvenlik duvarı oluşturur:

```azurecli-interactive
# Install the CLI extension for Azure Firewall
az extension add --name azure-firewall

# Create an Azure firewall
az network firewall create \
    --resource-group $MC_RESOURCE_GROUP\
    --name myAzureFirewall
```

Bir Azure Güvenlik Duvarı trafiği akışa çıkış yapan bir genel IP adresine atanır. [Az Network public-ip Create][az-network-public-ip-create] komutunu kullanarak genel bir adres oluşturun ve ardından genel IP 'yi uygulayan [az Network Firewall IP-Config Create][az-network-firewall-ip-config-create] komutunu kullanarak güvenlik duvarında bir IP yapılandırması oluşturun:

```azurecli-interactive
# Create a public IP address for the firewall
FIREWALL_PUBLIC_IP=$(az network public-ip create \
    --resource-group $MC_RESOURCE_GROUP \
    --name myAzureFirewallPublicIP \
    --sku Standard \
    --query publicIp.ipAddress -o tsv)

# Associated the firewall with virtual network
az network firewall ip-config create \
    --resource-group $MC_RESOURCE_GROUP \
    --name myAzureFirewallIPConfig \
    --vnet-name $VNET_NAME \
    --firewall-name myAzureFirewall \
    --public-ip-address myAzureFirewallPublicIP
```

Şimdi [az Network Firewall Network-Rule Create][az-network-firewall-network-rule-create] komutunu kullanarak tüm *TCP* trafiğine *izin* vermek için Azure Güvenlik Duvarı ağ kuralını oluşturun. Aşağıdaki örnek, herhangi bir kaynak veya hedef adresi olan trafik için *Allowtcpoutbağlanacak* adlı bir ağ kuralı oluşturur:

```azurecli-interactive
az network firewall network-rule create \
    --resource-group $MC_RESOURCE_GROUP \
    --firewall-name myAzureFirewall \
    --name AllowTCPOutbound \
    --collection-name myAzureFirewallCollection \
    --priority 200 \
    --action Allow \
    --protocols TCP \
    --source-addresses '*' \
    --destination-addresses '*' \
    --destination-ports '*'
```

Azure Güvenlik duvarını ağ rotası ile ilişkilendirmek için, mevcut yol tablosu bilgilerini, Azure Güvenlik duvarının iç IP adresini ve ardından API sunucusunun IP adresini alın. Bu IP adresleri, trafiğin küme iletişimi için nasıl yönlendirildiğini denetlemek için sonraki bölümde belirtilmiştir.

```azurecli-interactive
# Get the AKS cluster route table
ROUTE_TABLE=$(az network route-table list \
    --resource-group $MC_RESOURCE_GROUP \
    --query "[?contains(name,'agentpool')].name" -o tsv)

# Get internal IP address of the firewall
FIREWALL_INTERNAL_IP=$(az network firewall show \
    --resource-group $MC_RESOURCE_GROUP \
    --name myAzureFirewall \
    --query ipConfigurations[0].privateIpAddress -o tsv)

# Get the IP address of API server endpoint
K8S_ENDPOINT_IP=$(kubectl get endpoints -o=jsonpath='{.items[?(@.metadata.name == "kubernetes")].subsets[].addresses[].ip}')
```

Son olarak, mevcut AKS ağ yolu tablosunda, API sunucusu iletişimi için Azure Güvenlik Duvarı gerecini kullanmasına izin veren [az Network Route-Table Route Create][az-network-route-table-route-create] komutunu kullanarak bir yol oluşturun.

```azurecli-interactive
az network route-table route create \
    --resource-group $MC_RESOURCE_GROUP \
    --route-table-name $ROUTE_TABLE \
    --name AzureFirewallAPIServer \
    --address-prefix $K8S_ENDPOINT_IP/32 \
    --next-hop-ip-address $FIREWALL_INTERNAL_IP \
    --next-hop-type VirtualAppliance

echo "Public IP address for the Azure Firewall instance that should be added to the list of API server authorized addresses is:" $FIREWALL_PUBLIC_IP
```

Azure Güvenlik Duvarı gerecinizin genel IP adresini bir yere getirin. Bu adres, sonraki bölümde API sunucusu yetkilendirilmiş IP aralıkları listesine eklenir.

## <a name="enable-authorized-ip-ranges"></a>Yetkilendirilmiş IP aralıklarını etkinleştir

API sunucusu yetkilendirilmiş IP aralıklarını etkinleştirmek için, yetkilendirilmiş IP adresi aralıklarının bir listesini sağlarsınız. Bir CıDR aralığı belirttiğinizde, aralıktaki ilk IP adresi ile başlayın. Örneğin, *137.117.106.90/29* geçerli bir aralıktır, ancak ARALıKTAKI ilk IP adresini *137.117.106.88/29*gibi belirttiğinizden emin olun.

[Az aks Update][az-aks-update] komutunu kullanın ve izin vermek için *--api-Server-yetkilendirilmiş-IP aralıklarını* belirtin. Bu IP adresi aralıkları genellikle şirket içi ağlarınız tarafından kullanılan adres aralıklarından oluşur. Önceki adımda elde edilen kendi Azure Güvenlik duvarınızın genel IP adresini ekleyin (örneğin, *20.42.25.196/32*).

Aşağıdaki örnek, *Myresourcegroup*adlı kaynak grubunda *Myakscluster* adlı kümede bulunan API sunucusu yetkili IP aralıklarını mümkün bir şekilde sunar. Yetkilendirmede kullanılacak IP adresi aralıkları *20.42.25.196/32* ' dir (Azure Güvenlik DUVARı genel IP adresi), ardından *172.0.0.0/16* ve *168.10.0.0/18*:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges 20.42.25.196/32,172.0.0.0/16,168.10.0.0/18
```

## <a name="update-or-disable-authorized-ip-ranges"></a>Yetkili IP aralıklarını Güncelleştir veya devre dışı bırak

Yetkili IP aralıklarını güncelleştirmek veya devre dışı bırakmak için [az aks Update][az-aks-update] komutunu yeniden kullanırsınız. Aşağıdaki örnekte gösterildiği gibi, izin vermek istediğiniz güncelleştirilmiş CıDR aralığını belirtin veya API sunucusu yetkilendirilmiş IP aralıklarını devre dışı bırakmak için boş bir Aralık belirtin:

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
[azure-firewall-costs]: https://azure.microsoft.com/pricing/details/azure-firewall/

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[concepts-clusters-workloads]: concepts-clusters-workloads.md
[concepts-security]: concepts-security.md
[operator-best-practices-cluster-security]: operator-best-practices-cluster-security.md
[create-aks-sp]: kubernetes-service-principal.md#manually-create-a-service-principal
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-network-vnet-subnet-create]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-create
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-network-firewall-create]: /cli/azure/ext/azure-firewall/network/firewall#ext-azure-firewall-az-network-firewall-create
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[az-network-firewall-ip-config-create]: /cli/azure/ext/azure-firewall/network/firewall/ip-config#ext-azure-firewall-az-network-firewall-ip-config-create
[az-network-firewall-network-rule-create]: /cli/azure/ext/azure-firewall/network/firewall/network-rule#ext-azure-firewall-az-network-firewall-network-rule-create
[az-network-route-table-route-create]: /cli/azure/network/route-table/route#az-network-route-table-route-create
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[standard-sku-lb]: load-balancer-standard.md
