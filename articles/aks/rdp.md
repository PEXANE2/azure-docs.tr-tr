---
title: RDP içine Azure Kubernetes Service (AKS) küme Windows Server düğümleri
description: Sorun giderme ve bakım görevleri için Azure Kubernetes Service (AKS) kümesi Windows Server düğümleri ile nasıl RDP bağlantısı oluşturabilirsiniz öğrenin.
services: container-service
ms.topic: article
ms.date: 06/04/2019
ms.openlocfilehash: 897504aa9902d0feaf4245c719d3a4a3c6fd2241
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77594490"
---
# <a name="connect-with-rdp-to-azure-kubernetes-service-aks-cluster-windows-server-nodes-for-maintenance-or-troubleshooting"></a>RdP ile Azure Kubernetes Hizmetine (AKS) windows server düğümlerini bakım veya sorun giderme için bağlama

Azure Kubernetes Service (AKS) kümenizin yaşam döngüsü boyunca bir AKS Windows Server düğümüne erişmeniz gerekebilir. Bu erişim bakım, günlük toplama veya diğer sorun giderme işlemleri için olabilir. RDP kullanarak AKS Windows Server düğümlerine erişebilirsiniz. Alternatif olarak, AKS Windows Server düğümlerine erişmek için SSH'yi kullanmak istiyorsanız ve küme oluşturma sırasında kullanılan aynı anahtar çiftine erişebiliyorsanız, SSH'deki adımları [Azure Kubernetes Service (AKS) küme düğümlerine][ssh-steps]kadar takip edebilirsiniz. Güvenlik amacıyla AKS düğümleri internete maruz kalmamış.

Windows Server düğüm desteği şu anda AKS önizleme de.

Bu makalede, kendi özel IP adreslerini kullanarak bir AKS düğümü ile rdp bağlantısı oluşturmak için nasıl gösterir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Windows Server düğümü olan varolan bir AKS kümeniz olduğunu varsayar. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi kullanarak Windows kapsayıcısı olan bir AKS kümesi oluşturma makalesine][aks-windows-cli]bakın. Sorun gidermek istediğiniz Windows Server düğümü için Windows yöneticisi kullanıcı adı ve parolasına ihtiyacınız vardır. Ayrıca [Microsoft Uzak Masaüstü][rdp-mac]gibi bir RDP istemcisi gerekir.

Ayrıca Azure CLI sürümü 2.0.61 veya daha sonra yüklenmiş ve yapılandırılmış gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

## <a name="deploy-a-virtual-machine-to-the-same-subnet-as-your-cluster"></a>Sanal bir makineyi kümenizle aynı alt ağa dağıtma

AKS kümenizin Windows Server düğümlerinde harici olarak erişilebilen IP adresleri yoktur. RDP bağlantısı yapmak için, herkese açık bir IP adresine sahip sanal bir makineyi Windows Server düğümlerinizle aynı alt ağa dağıtabilirsiniz.

Aşağıdaki örnek, *myResourceGroup* kaynak grubunda *myVM* adında bir sanal makine oluşturur.

İlk olarak, Windows Server düğüm havuzunuz tarafından kullanılan alt ağı alın. Alt net kimliğini almak için alt netin adını almanız gerekir. Alt netin adını almak için vnet'in adını almanız gerekir. Kümenizi ağ listesi için sorgulayarak vnet adını alın. Kümeyi sorgulamak için onun adına ihtiyacınız var. Bunların tümlerini Azure Bulut Kabuğu'nda aşağıdakileri çalıştırarak elde edebilirsiniz:

```azurecli-interactive
CLUSTER_RG=$(az aks show -g myResourceGroup -n myAKSCluster --query nodeResourceGroup -o tsv)
VNET_NAME=$(az network vnet list -g $CLUSTER_RG --query [0].name -o tsv)
SUBNET_NAME=$(az network vnet subnet list -g $CLUSTER_RG --vnet-name $VNET_NAME --query [0].name -o tsv)
SUBNET_ID=$(az network vnet subnet show -g $CLUSTER_RG --vnet-name $VNET_NAME --name $SUBNET_NAME --query id -o tsv)
```

Artık SUBNET_ID sahip olduğunuza göre, VM'yi oluşturmak için aynı Azure Bulut Bulutu penceresinde aşağıdaki komutu çalıştırın:

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image win2019datacenter \
    --admin-username azureuser \
    --admin-password myP@ssw0rd12 \
    --subnet $SUBNET_ID \
    --query publicIpAddress -o tsv
```

Aşağıdaki örnek çıktı, VM'nin başarıyla oluşturulduğunu gösterir ve sanal makinenin genel IP adresini görüntüler.

```console
13.62.204.18
```

Sanal makinenin genel IP adresini kaydedin. Bu adresi daha sonraki bir adımda kullanırsınız.

## <a name="allow-access-to-the-virtual-machine"></a>Sanal makineye erişime izin verme

AKS düğüm havuzu alt ağları varsayılan olarak NSG'ler (Ağ Güvenlik Grupları) ile korunur. Sanal makineye erişmek için NSG'ye erişimi etkinleştirmeniz gerekir.

> [!NOTE]
> NSG'ler AKS hizmeti tarafından kontrol edilir. NSG'de yapacağınız herhangi bir değişiklik, kontrol uçağı tarafından istediğiniz zaman üzerine yazılacaktır.
>

İlk olarak, kural eklemek için kaynak grubu ve nsg nsg adını almak:

```azurecli-interactive
CLUSTER_RG=$(az aks show -g myResourceGroup -n myAKSCluster --query nodeResourceGroup -o tsv)
NSG_NAME=$(az network nsg list -g $CLUSTER_RG --query [].name -o tsv)
```

Ardından, NSG kuralını oluşturun:

```azurecli-interactive
az network nsg rule create --name tempRDPAccess --resource-group $CLUSTER_RG --nsg-name $NSG_NAME --priority 100 --destination-port-range 3389 --protocol Tcp --description "Temporary RDP access to Windows nodes"
```

## <a name="get-the-node-address"></a>Düğüm adresini alın

Bir Kubernetes kümesini yönetmek için [kubectl][kubectl], Kubernetes komut satırı istemcisi kullanırsınız. Azure Bulut Su Şur'u kullanıyorsanız, `kubectl` zaten yüklenmiş. Yerel `kubectl` olarak yüklemek için [az aks install-cli][az-aks-install-cli] komutunu kullanın:
    
```azurecli-interactive
az aks install-cli
```

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Bu komut kimlik bilgilerini karşıdan yükler ve Kubernetes CLI'yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

[Kubectl get][kubectl-get] komutunu kullanarak Windows Server düğümlerinin dahili IP adresini listeleyin:

```console
kubectl get nodes -o wide
```

Aşağıdaki örnek çıktı, Windows Server düğümleri de dahil olmak üzere kümedeki tüm düğümlerin iç IP adreslerini gösterir.

```console
$ kubectl get nodes -o wide
NAME                                STATUS   ROLES   AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                    KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-42485177-vmss000000   Ready    agent   18h   v1.12.7   10.240.0.4    <none>        Ubuntu 16.04.6 LTS          4.15.0-1040-azure   docker://3.0.4
aksnpwin000000                      Ready    agent   13h   v1.12.7   10.240.0.67   <none>        Windows Server Datacenter   10.0.17763.437
```

Sorun gidermek istediğiniz Windows Server düğümünün dahili IP adresini kaydedin. Bu adresi daha sonraki bir adımda kullanırsınız.

## <a name="connect-to-the-virtual-machine-and-node"></a>Sanal makineye ve düğüme bağlanın

[Microsoft Remote Desktop][rdp-mac]gibi bir RDP istemcisini kullanarak daha önce oluşturduğunuz sanal makinenin genel IP adresine bağlanın.

![RDP istemcisi kullanarak sanal makineye bağlanma görüntüsü](media/rdp/vm-rdp.png)

Sanal makinenize bağlandıktan sonra, sanal makinenizin içinden bir RDP istemcisi kullanarak sorun gidermek istediğiniz Windows Server düğümünün *dahili IP adresine* bağlanın.

![BIR RDP istemcisi kullanarak Windows Server düğümüne bağlanma görüntüsü](media/rdp/node-rdp.png)

Artık Windows Server düğümünüze bağlısınız.

![Windows Server düğümündeki cmd pencere görüntüsü](media/rdp/node-session.png)

Artık *cmd* penceresinde herhangi bir sorun giderme komutları çalıştırabilirsiniz. Windows Server düğümleri Windows Server Core'u kullandığından, RDP üzerinden bir Windows Server düğümüne bağlandığınızda tam bir GUI veya diğer GUI araçları yoktur.

## <a name="remove-rdp-access"></a>RDP erişimini kaldırma

Bittiğinde, Windows Server düğümüne RDP bağlantısından çıkın ve RDP oturumundan sanal makineye çıkın. Her iki RDP oturumunu da çıktıktan sonra [az vm silme][az-vm-delete] komutuyla sanal makineyi silin:

```azurecli-interactive
az vm delete --resource-group myResourceGroup --name myVM
```

Ve NSG kuralı:

```azurecli-interactive
CLUSTER_RG=$(az aks show -g myResourceGroup -n myAKSCluster --query nodeResourceGroup -o tsv)
NSG_NAME=$(az network nsg list -g $CLUSTER_RG --query [].name -o tsv)
```

```azurecli-interactive
az network nsg rule delete --resource-group $CLUSTER_RG --nsg-name $NSG_NAME --name tempRDPAccess
```

## <a name="next-steps"></a>Sonraki adımlar

Ek sorun giderme verilerine ihtiyacınız varsa, [Kubernetes ana düğüm günlüklerini][view-master-logs] veya [Azure Monitörünü][azure-monitor-containers]görüntüleyebilirsiniz.

<!-- EXTERNAL LINKS -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[rdp-mac]: https://aka.ms/rdmac

<!-- INTERNAL LINKS -->
[aks-windows-cli]: windows-container-cli.md
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-vm-delete]: /cli/azure/vm#az-vm-delete
[azure-monitor-containers]: ../azure-monitor/insights/container-insights-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[ssh-steps]: ssh.md
[view-master-logs]: view-master-logs.md
