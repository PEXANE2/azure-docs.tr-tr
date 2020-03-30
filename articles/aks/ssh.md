---
title: Azure Kubernetes Service (AKS) kümesi düğümlerine SSH ile bağlanma
description: Sorun giderme ve bakım görevleri için Azure Kubernetes Hizmeti (AKS) küme düğümleriyle nasıl SSH bağlantısı oluşturabilirsiniz öğrenin.
services: container-service
ms.topic: article
ms.date: 07/31/2019
ms.openlocfilehash: dfdcda40a24142f85bbeb360aacf0971d72d181f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77593640"
---
# <a name="connect-with-ssh-to-azure-kubernetes-service-aks-cluster-nodes-for-maintenance-or-troubleshooting"></a>Bakım veya sorun giderme amacıyla SSH ile Azure Kubernetes Service (AKS) kümesi düğümlerine bağlanma

Azure Kubernetes Service (AKS) kümenizin yaşam döngüsü boyunca bir AKS düğümüne erişmeniz gerekebilir. Bu erişim bakım, günlük toplama veya diğer sorun giderme işlemleri için olabilir. Windows Server düğümleri de dahil olmak üzere SSH kullanarak AKS düğümlerine erişebilirsiniz (şu anda AKS'de önizlemede). [Ayrıca uzak masaüstü protokolü (RDP) bağlantılarını kullanarak Windows Server düğümlerine de bağlanabilirsiniz.][aks-windows-rdp] Güvenlik amacıyla, AKS düğümleri internete maruz değildir. AKS düğümlerine SSH için özel IP adresini kullanırsınız.

Bu makalede, kendi özel IP adreslerini kullanarak bir AKS düğümü ile SSH bağlantısı oluşturmak için nasıl gösterir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

Varsayılan olarak, BIR AKS kümesi oluşturduğunuzda ssh anahtarları elde edilir veya oluşturulur, sonra düğümlere eklenir. Bu makalede, AKS kümenizi oluşturduğunuzda kullanılan SSH anahtarlarından farklı SSH tuşlarının nasıl belirtilen bir şekilde belirtilen. Makalede ayrıca düğümünüzün özel IP adresini nasıl belirleyip SSH kullanarak ona bağlanabileceğinizi de gösterir. Farklı bir SSH anahtarı belirtmeniz gerekmiyorsa, düğüme SSH ortak anahtarıeklemek için adımı atlayabilirsiniz.

Bu makalede, bir SSH anahtarı nız olduğunu da varsayar. [macOS, Linux veya][ssh-nix] [Windows][ssh-windows]kullanarak bir SSH tuşu oluşturabilirsiniz. Anahtar çiftini oluşturmak için PuTTY Gen kullanıyorsanız, anahtar çiftini varsayılan PuTTy özel anahtar biçimi (.ppk dosyası) yerine OpenSSH biçiminde kaydedin.

Ayrıca Azure CLI sürümü 2.0.64 veya daha sonra yüklenmiş ve yapılandırılmış gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

## <a name="configure-virtual-machine-scale-set-based-aks-clusters-for-ssh-access"></a>SSH erişimi için sanal makine ölçeği kümesi tabanlı AKS kümelerini yapılandırma

SSH erişimi için sanal makine ölçeği küme tabanlı yapılandırmak için, kümenizin sanal makine ölçek kümesinin adını bulun ve SSH ortak anahtarınızı bu ölçek kümesine ekleyin.

AKS kümenizin kaynak grup adını almak için [az aks göster][az-aks-show] komutunu, ardından ölçek kümenizin adını almak için [az vmss liste][az-vmss-list] komutunu kullanın.

```azurecli-interactive
CLUSTER_RESOURCE_GROUP=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
SCALE_SET_NAME=$(az vmss list --resource-group $CLUSTER_RESOURCE_GROUP --query [0].name -o tsv)
```

Yukarıdaki örnek, *myResourceGroup'taki* *myAKSCluster* için küme kaynak grubunun adını *CLUSTER_RESOURCE_GROUP.* Örnek daha sonra ölçek kümesi adını listelemek ve *SCALE_SET_NAME*atamak için *CLUSTER_RESOURCE_GROUP* kullanır.

> [!IMPORTANT]
> Şu anda, Azure CLI'yi kullanarak sanal makine ölçeği kümesi tabanlı AKS kümeleriniz için SSH tuşlarınızı güncelleştirmeniz gerekir.
> 
> Linux düğümleri için SSH tuşları şu anda yalnızca Azure CLI kullanılarak eklenebilir. SSH kullanarak bir Windows Server düğümüne bağlanmak istiyorsanız, AKS kümesini oluşturduğunuzda sağlanan SSH tuşlarını kullanın ve SSH ortak anahtarınızı eklemek için bir sonraki komut kümesini atlayın. Yine de bu bölümün son komutu gösterilir sorun gidermek istediğiniz düğümün IP adresi gerekir. Alternatif olarak, SSH kullanmak yerine [uzak masaüstü protokolü (RDP) bağlantılarını kullanarak Windows Server düğümlerine bağlanabilirsiniz.][aks-windows-rdp]

Sanal makine ölçeği kümesindeki düğümlere SSH tuşlarınızı eklemek için [az vmss uzantı kümesini][az-vmss-extension-set] ve [az vmss update instances][az-vmss-update-instances] komutlarını kullanın.

```azurecli-interactive
az vmss extension set  \
    --resource-group $CLUSTER_RESOURCE_GROUP \
    --vmss-name $SCALE_SET_NAME \
    --name VMAccessForLinux \
    --publisher Microsoft.OSTCExtensions \
    --version 1.4 \
    --protected-settings "{\"username\":\"azureuser\", \"ssh_key\":\"$(cat ~/.ssh/id_rsa.pub)\"}"

az vmss update-instances --instance-ids '*' \
    --resource-group $CLUSTER_RESOURCE_GROUP \
    --name $SCALE_SET_NAME
```

Yukarıdaki örnekte *önceki* komutlardan CLUSTER_RESOURCE_GROUP ve *SCALE_SET_NAME* değişkenler kullanır. Yukarıdaki örnekte, SSH ortak anahtarınızın konumu olarak *~/.ssh/id_rsa.pub* da kullanır.

> [!NOTE]
> Varsayılan olarak, AKS düğümleri için kullanıcı adı *azureuser'dır.*

SSH ortak anahtarınızı ölçek kümesine ekledikten sonra, IP adresini kullanarak bu ölçek kümesindeki bir düğüm sanal makinesine SSH yapabilirsiniz. [Kubectl get komutunu][kubectl-get]kullanarak AKS küme düğümlerinin özel IP adreslerini görüntüleyin.

```console
kubectl get nodes -o wide
```

Aşağıdaki örnek çıktı, bir Windows Server düğümü de dahil olmak üzere kümedeki tüm düğümlerin iç IP adreslerini gösterir.

```console
$ kubectl get nodes -o wide

NAME                                STATUS   ROLES   AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                    KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-42485177-vmss000000   Ready    agent   18h   v1.12.7   10.240.0.4    <none>        Ubuntu 16.04.6 LTS          4.15.0-1040-azure   docker://3.0.4
aksnpwin000000                      Ready    agent   13h   v1.12.7   10.240.0.67   <none>        Windows Server Datacenter   10.0.17763.437
```

Sorun gidermek istediğiniz düğümün iç IP adresini kaydedin.

SSH kullanarak düğümünüze erişmek için [SSH bağlantısını oluştur'daki](#create-the-ssh-connection)adımları izleyin.

## <a name="configure-virtual-machine-availability-set-based-aks-clusters-for-ssh-access"></a>SSH erişimi için sanal makine kullanılabilirliği kümesi tabanlı AKS kümelerini yapılandırma

Sanal makine kullanılabilirliği küme tabanlı AKS kümenizi SSH erişimi için yapılandırmak için kümenizin Linux düğümünün adını bulun ve ssh ortak anahtarınızı bu düğüme ekleyin.

AKS kümenizin kaynak grup adını almak için [az aks göster][az-aks-show] komutunu, ardından kümenizin Linux düğümünün sanal makine adını listelemek için [az vm liste][az-vm-list] komutunu kullanın.

```azurecli-interactive
CLUSTER_RESOURCE_GROUP=$(az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv)
az vm list --resource-group $CLUSTER_RESOURCE_GROUP -o table
```

Yukarıdaki örnek, *myResourceGroup'taki* *myAKSCluster* için küme kaynak grubunun adını *CLUSTER_RESOURCE_GROUP.* Örnek daha sonra sanal makine adını listelemek için *CLUSTER_RESOURCE_GROUP* kullanır. Örnek çıktı sanal makinenin adını gösterir:

```
Name                      ResourceGroup                                  Location
------------------------  ---------------------------------------------  ----------
aks-nodepool1-79590246-0  MC_myResourceGroupAKS_myAKSClusterRBAC_eastus  eastus
```

Düğüme SSH tuşlarınızı eklemek için [az vm kullanıcı güncelleştirme][az-vm-user-update] komutunu kullanın.

```azurecli-interactive
az vm user update \
    --resource-group $CLUSTER_RESOURCE_GROUP \
    --name aks-nodepool1-79590246-0 \
    --username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

Yukarıdaki örnekte *CLUSTER_RESOURCE_GROUP* değişkeni ve önceki komutlardan düğüm sanal makine adı kullanır. Yukarıdaki örnekte, SSH ortak anahtarınızın konumu olarak *~/.ssh/id_rsa.pub* da kullanır. Bir yol belirtmek yerine SSH ortak anahtarınızın içeriğini de kullanabilirsiniz.

> [!NOTE]
> Varsayılan olarak, AKS düğümleri için kullanıcı adı *azureuser'dır.*

SSH ortak anahtarınızı düğüm sanal makinesine ekledikten sonra, IP adresini kullanarak sanal makineye SSH kullanabilirsiniz. [az vm list-ip-adresler][az-vm-list-ip-addresses] komutunu kullanarak AKS küme düğümünün özel IP adresini görüntüleyin.

```azurecli-interactive
az vm list-ip-addresses --resource-group $CLUSTER_RESOURCE_GROUP -o table
```

Yukarıdaki örnekte, önceki komutlarda *CLUSTER_RESOURCE_GROUP* değişken kümesi kullanır. Aşağıdaki örnek çıktı AKS düğümlerinin özel IP adreslerini gösterir:

```
VirtualMachine            PrivateIPAddresses
------------------------  --------------------
aks-nodepool1-79590246-0  10.240.0.4
```

## <a name="create-the-ssh-connection"></a>SSH bağlantısını oluşturma

AKS düğümüne SSH bağlantısı oluşturmak için AKS kümenizde bir yardımcı bölme çalıştırın. Bu yardımcı bölme, kümeye SSH erişimi ve ardından ek SSH düğümü erişimi sağlar. Bu yardımcı bölmeyi oluşturmak ve kullanmak için aşağıdaki adımları tamamlayın:

1. Bir `debian` kapsayıcı görüntüsü çalıştırın ve ona bir terminal oturumu takın. Bu kapsayıcı AKS kümesinde herhangi bir düğüm ile bir SSH oturumu oluşturmak için kullanılabilir:

    ```console
    kubectl run --generator=run-pod/v1 -it --rm aks-ssh --image=debian
    ```

    > [!TIP]
    > Windows Server düğümleri kullanıyorsanız (şu anda AKS'de önizlemede), Debian kapsayıcısını Linux düğümünde zamanlamak için komuta bir düğüm seçici ekleyin:
    >
    > `kubectl run -it --rm aks-ssh --image=debian --overrides='{"apiVersion":"apps/v1","spec":{"template":{"spec":{"nodeSelector":{"beta.kubernetes.io/os":"linux"}}}}}'`

1. Terminal oturumu kapsayıcıya bağlandıktan sonra aşağıdakileri kullanarak `apt-get`bir SSH istemcisi kurun:

    ```console
    apt-get update && apt-get install openssh-client -y
    ```

1. Kapsayıcınıza bağlı olmayan yeni bir terminal penceresi açın, özel SSH anahtarınızı yardımcı bölmeye kopyalayın. Bu özel anahtar AKS düğümüne SSH oluşturmak için kullanılır. 

   Gerekirse , *~/.ssh/id_rsa'yi* özel SSH anahtarınızın konumuna değiştirin:

    ```console
    kubectl cp ~/.ssh/id_rsa $(kubectl get pod -l run=aks-ssh -o jsonpath='{.items[0].metadata.name}'):/id_rsa
    ```

1. Terminal oturumuna konteynerinize dönün, kopyalanan `id_rsa` özel SSH anahtarındaki izinleri güncelleştirin, böylece yalnızca kullanıcı tarafından okunacak şekilde:

    ```console
    chmod 0600 id_rsa
    ```

1. AKS düğümünüze bir SSH bağlantısı oluşturun. Yine, AKS düğümleri için varsayılan kullanıcı adı *azureuser'dır.* SSH anahtarına ilk güvenilirken bağlantıyla devam etme istemini kabul edin. Daha sonra AKS düğümünüzün bash istemi ile sağlanır:

    ```console
    $ ssh -i id_rsa azureuser@10.240.0.4

    ECDSA key fingerprint is SHA256:A6rnRkfpG21TaZ8XmQCCgdi9G/MYIMc+gFAuY9RUY70.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added '10.240.0.4' (ECDSA) to the list of known hosts.

    Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 4.15.0-1018-azure x86_64)

     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage

      Get cloud support with Ubuntu Advantage Cloud Guest:
        https://www.ubuntu.com/business/services/cloud

    [...]

    azureuser@aks-nodepool1-79590246-0:~$
    ```

## <a name="remove-ssh-access"></a>SSH erişimini kaldırma

Yapıldığında, `exit` SSH oturumu ve `exit` daha sonra etkileşimli kapsayıcı oturumu. Bu kapsayıcı oturumu kapandığında, AKS kümesinden SSH erişimi için kullanılan bölme silinir.

## <a name="next-steps"></a>Sonraki adımlar

Ek sorun giderme verilerine ihtiyacınız varsa, [kubelet günlüklerini görüntüleyebilir][view-kubelet-logs] veya [Kubernetes ana düğüm günlüklerini görüntüleyebilirsiniz.][view-master-logs]

<!-- EXTERNAL LINKS -->
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- INTERNAL LINKS -->
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-vm-list]: /cli/azure/vm#az-vm-list
[az-vm-user-update]: /cli/azure/vm/user#az-vm-user-update
[az-vm-list-ip-addresses]: /cli/azure/vm#az-vm-list-ip-addresses
[view-kubelet-logs]: kubelet-logs.md
[view-master-logs]: view-master-logs.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[aks-windows-rdp]: rdp.md
[ssh-nix]: ../virtual-machines/linux/mac-create-ssh-keys.md
[ssh-windows]: ../virtual-machines/linux/ssh-from-windows.md
[az-vmss-list]: /cli/azure/vmss#az-vmss-list
[az-vmss-extension-set]: /cli/azure/vmss/extension#az-vmss-extension-set
[az-vmss-update-instances]: /cli/azure/vmss#az-vmss-update-instances
