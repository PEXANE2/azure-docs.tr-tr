---
title: Azure NetApp Dosyalarını Azure Kubernetes Hizmetiyle Tümleştir
description: Azure NetApp Dosyalarını Azure Kubernetes Hizmeti ile nasıl entegre edebilirsiniz öğrenin
services: container-service
author: zr-msft
ms.topic: article
ms.date: 09/26/2019
ms.author: zarhoads
ms.openlocfilehash: 1c4996df66d475c63110e3d2797f55598fd85b8d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273745"
---
# <a name="integrate-azure-netapp-files-with-azure-kubernetes-service"></a>Azure NetApp Dosyalarını Azure Kubernetes Hizmetiyle Tümleştir

[Azure NetApp Files,][anf] Azure'da çalışan kurumsal sınıf, yüksek performanslı, ölçülü bir dosya depolama hizmetidir. Bu makalede, Azure NetApp Dosyalarının Azure Kubernetes Hizmeti (AKS) ile nasıl entegre edilebildiğiniz gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

> [!IMPORTANT]
> AKS kümeniz de [Azure NetApp Dosyalarını destekleyen bir bölgede][anf-regions]olmalıdır.

Ayrıca Azure CLI sürüm 2.0.59 veya daha sonra yüklenmiş ve yapılandırılmış gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

### <a name="limitations"></a>Sınırlamalar

Azure NetApp Dosyalarını kullandığınızda aşağıdaki sınırlamalar geçerlidir:

* Azure NetApp Dosyaları yalnızca [belirli Azure bölgelerinde][anf-regions]kullanılabilir.
* Azure NetApp Dosyalarını kullanabilmek için önce Azure NetApp Dosyaları hizmetine erişebilirsiniz. Erişim başvurusunda bulunmak için [Azure NetApp Dosyaları bekleme listesi gönderme formunu][anf-waitlist]kullanabilirsiniz. Azure NetApp Files ekibinden resmi onay e-postasını alana kadar Azure NetApp Files hizmetine erişemezsiniz.
* Azure NetApp Files hizmetiniz AKS kümenizle aynı sanal ağda oluşturulmalıdır.
* Bir AKS kümesinin ilk dağıtımından sonra, Azure NetApp Dosyaları için yalnızca statik sağlama desteklenir.
* Azure NetApp Files ile dinamik sağlama yı kullanmak için [NetApp Trident](https://netapp-trident.readthedocs.io/) sürümünü 19.07 veya sonraki sürümde yükleyin ve yapılandırdı.

## <a name="configure-azure-netapp-files"></a>Azure NetApp Dosyalarını Yapılandırma

> [!IMPORTANT]
> *Microsoft.NetApp* kaynak sağlayıcısını kaydettirebilmeniz için aboneliğiniz için [Azure NetApp Dosyaları bekleme listesi gönderme formunu][anf-waitlist] doldurmanız gerekir. Azure NetApp Files ekibinden resmi onay e-postasını alana kadar kaynak sağlama yı kaydedemezsiniz.

*Microsoft.NetApp* kaynak sağlayıcısına kaydolun:

```azurecli
az provider register --namespace Microsoft.NetApp --wait
```

> [!NOTE]
> Bu tamamlamak için biraz zaman alabilir.

AKS ile kullanılmak üzere bir Azure NetApp hesabı oluşturduğunuzda, düğüm **kaynak** grubunda hesap oluşturmanız gerekir. İlk olarak, [az aks show][az-aks-show] komutu ile `--query nodeResourceGroup` kaynak grup adını alın ve sorgu parametresi ekleyin. Aşağıdaki örnek, *myResourceGroup*kaynak grubu adında *myAKSCluster* adlı AKS kümesi için düğüm kaynak grubunu alır:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv
```

```output
MC_myResourceGroup_myAKSCluster_eastus
```

[Az netappfiles hesabını][az-netappfiles-account-create]kullanarak **düğüm** kaynak grubunda ve AKS kümenizle aynı bölgede bir Azure NetApp Files hesabı oluşturun. Aşağıdaki örnek, *MC_myResourceGroup_myAKSCluster_eastus* kaynak grubunda ve *eastus* bölgesinde *myaccount1* adlı bir hesap oluşturur:

```azurecli
az netappfiles account create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --location eastus \
    --account-name myaccount1
```

[Az netappfiles havuzu oluşturarak][az-netappfiles-pool-create]yeni bir kapasite havuzu oluşturun. Aşağıdaki örnek, 4 TB boyutu ve *Premium* hizmet düzeyi ile *mypool1* adlı yeni bir kapasite havuzu oluşturur:

```azurecli
az netappfiles pool create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --location eastus \
    --account-name myaccount1 \
    --pool-name mypool1 \
    --size 4 \
    --service-level Premium
```

[Az network vnet subnet oluşturarak][az-network-vnet-subnet-create] [Azure NetApp Dosyalarına devretmek için][anf-delegate-subnet] bir alt ağ oluşturun. *Bu alt ağ, AKS kümenizle aynı sanal ağda olmalıdır.*

```azurecli
RESOURCE_GROUP=MC_myResourceGroup_myAKSCluster_eastus
VNET_NAME=$(az network vnet list --resource-group $RESOURCE_GROUP --query [].name -o tsv)
VNET_ID=$(az network vnet show --resource-group $RESOURCE_GROUP --name $VNET_NAME --query "id" -o tsv)
SUBNET_NAME=MyNetAppSubnet
az network vnet subnet create \
    --resource-group $RESOURCE_GROUP \
    --vnet-name $VNET_NAME \
    --name $SUBNET_NAME \
    --delegations "Microsoft.NetApp/volumes" \
    --address-prefixes 10.0.0.0/28
```

[Az netappfiles birim oluştur][az-netappfiles-volume-create]kullanarak bir birim oluşturun.

```azurecli
RESOURCE_GROUP=MC_myResourceGroup_myAKSCluster_eastus
LOCATION=eastus
ANF_ACCOUNT_NAME=myaccount1
POOL_NAME=mypool1
SERVICE_LEVEL=Premium
VNET_NAME=$(az network vnet list --resource-group $RESOURCE_GROUP --query [].name -o tsv)
VNET_ID=$(az network vnet show --resource-group $RESOURCE_GROUP --name $VNET_NAME --query "id" -o tsv)
SUBNET_NAME=MyNetAppSubnet
SUBNET_ID=$(az network vnet subnet show --resource-group $RESOURCE_GROUP --vnet-name $VNET_NAME --name $SUBNET_NAME --query "id" -o tsv)
VOLUME_SIZE_GiB=100 # 100 GiB
UNIQUE_FILE_PATH="myfilepath2" # Please note that creation token needs to be unique within all ANF Accounts

az netappfiles volume create \
    --resource-group $RESOURCE_GROUP \
    --location $LOCATION \
    --account-name $ANF_ACCOUNT_NAME \
    --pool-name $POOL_NAME \
    --name "myvol1" \
    --service-level $SERVICE_LEVEL \
    --vnet $VNET_ID \
    --subnet $SUBNET_ID \
    --usage-threshold $VOLUME_SIZE_GiB \
    --creation-token $UNIQUE_FILE_PATH \
    --protocol-types "NFSv3"
```

## <a name="create-the-persistentvolume"></a>Kalıcı Hacim oluşturma

[Az netappfiles ses gösterisini][az-netappfiles-volume-show] kullanarak hacminizin ayrıntılarını listeleyin

```azurecli
az netappfiles volume show --resource-group $RESOURCE_GROUP --account-name $ANF_ACCOUNT_NAME --pool-name $POOL_NAME --volume-name "myvol1"
```

```output
{
  ...
  "creationToken": "myfilepath2",
  ...
  "mountTargets": [
    {
      ...
      "ipAddress": "10.0.0.4",
      ...
    }
  ],
  ...
}
```

Kalıcı `pv-nfs.yaml` Birim tanımlayan bir sayı oluşturun. *OluşturmaToken* ve `server` önceki komuttan *ipAddress* ile değiştirin. `path` Örnek:

```yaml
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-nfs
spec:
  capacity:
    storage: 100Gi
  accessModes:
    - ReadWriteMany
  nfs:
    server: 10.0.0.4
    path: /myfilepath2
```

*Sunucuyu* ve *yolu* önceki adımda oluşturduğunuz NFS (Ağ Dosya Sistemi) biriminizin değerlerine güncelleştirin. [Kubectl uygula][kubectl-apply] komutu ile PersistentVolume oluşturun:

```console
kubectl apply -f pv-nfs.yaml
```

Kalıcı Birimin *Durumunu* doğrulayın [kubectl açıklama][kubectl-describe] komutunu kullanarak *kullanılabilir:*

```console
kubectl describe pv pv-nfs
```

## <a name="create-the-persistentvolumeclaim"></a>PersistentVolumeClaim oluşturma

Kalıcı `pvc-nfs.yaml` Birim tanımlayan bir sayı oluşturun. Örnek:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-nfs
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: ""
  resources:
    requests:
      storage: 1Gi
```

[Kubectl uygula][kubectl-apply] komutu ile PersistentVolumeClaim oluşturun:

```console
kubectl apply -f pvc-nfs.yaml
```

Kalıcı *Status* VolumeClaim durumunu doğrulayın [kubectl açıklama][kubectl-describe] komutunu kullanarak *Bound:*

```console
kubectl describe pvc pvc-nfs
```

## <a name="mount-with-a-pod"></a>Pod ile montaj

PersistentVolumeClaim'i kullanan tanımlayıcı bir `nginx-nfs.yaml` bölme oluşturun. Örnek:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: nginx-nfs
spec:
  containers:
  - image: nginx
    name: nginx-nfs
    command:
    - "/bin/sh"
    - "-c"
    - while true; do echo $(date) >> /mnt/azure/outfile; sleep 1; done
    volumeMounts:
    - name: disk01
      mountPath: /mnt/azure
  volumes:
  - name: disk01
    persistentVolumeClaim:
      claimName: pvc-nfs
```

[Kubectl uygula][kubectl-apply] komutu ile pod oluşturun:

```console
kubectl apply -f nginx-nfs.yaml
```

[Kubectl açıklama][kubectl-describe] komutunu kullanarak bölmenin *çalıştığını* doğrulayın:

```console
kubectl describe pod nginx-nfs
```

Bölmeye bağlanmak için [kubectl exec][kubectl-exec] kullanarak ses biriminizin bölmeye monte edildiğini doğrulayın ve `df -h` ses inin monte edilip edilemediğini kontrol edin.

```console
$ kubectl exec -it nginx-nfs -- bash
```

```output
root@nginx-nfs:/# df -h
Filesystem             Size  Used Avail Use% Mounted on
...
10.0.0.4:/myfilepath2  100T  384K  100T   1% /mnt/azure
...
```

## <a name="next-steps"></a>Sonraki adımlar

Azure NetApp Dosyaları hakkında daha fazla bilgi için [Azure NetApp Dosyaları nedir'e][anf]bakın. AKS ile NFS kullanma hakkında daha fazla bilgi için, [Azure Kubernetes Hizmeti (AKS) ile Bir NFS (Ağ Dosya Sistemi) Linux Server hacmini El ile oluşturun ve kullanın.][aks-nfs]


[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[aks-nfs]: azure-nfs-volume.md
[anf]: ../azure-netapp-files/azure-netapp-files-introduction.md
[anf-delegate-subnet]: ../azure-netapp-files/azure-netapp-files-delegate-subnet.md
[anf-quickstart]: ../azure-netapp-files/
[anf-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=netapp&regions=all
[anf-waitlist]: https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR8cq17Xv9yVBtRCSlcD_gdVUNUpUWEpLNERIM1NOVzA5MzczQ0dQR1ZTSS4u
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-netappfiles-account-create]: /cli/azure/netappfiles/account?view=azure-cli-latest#az-netappfiles-account-create
[az-netappfiles-pool-create]: /cli/azure/netappfiles/pool?view=azure-cli-latest#az-netappfiles-pool-create
[az-netappfiles-volume-create]: /cli/azure/netappfiles/volume?view=azure-cli-latest#az-netappfiles-volume-create
[az-netappfiles-volume-show]: /cli/azure/netappfiles/volume?view=azure-cli-latest#az-netappfiles-volume-show
[az-network-vnet-subnet-create]: /cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-create
[install-azure-cli]: /cli/azure/install-azure-cli
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-exec]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec
