---
title: Azure Kubernetes Hizmeti'nde (AKS) birden çok bölme için statik bir birim oluşturma
description: Azure Kubernetes Hizmetinde (AKS) birden çok eşzamanlı bölmeyle kullanılmak üzere Azure Dosyaları ile nasıl el ile bir birim oluşturabilirsiniz öğrenin
services: container-service
ms.topic: article
ms.date: 03/01/2019
ms.openlocfilehash: 084ab5cd6736c9148bcab1faf048d3d9081855d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77596411"
---
# <a name="manually-create-and-use-a-volume-with-azure-files-share-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) Azure Dosyaları paylaşımı yla el ile bir birim oluşturun ve kullanın

Kapsayıcı tabanlı uygulamaların genellikle verilere harici bir veri hacminde erişmeleri ve verileri devam etmesi gerekir. Birden çok bölmenin aynı depolama birimine eşzamanlı olarak erişmeye ihtiyacı varsa, [Sunucu İleti Bloğu (SMB) protokolünü][smb-overview]kullanarak bağlanmak için Azure Dosyaları'nı kullanabilirsiniz. Bu makalede, bir Azure Files paylaşımını el ile nasıl oluşturup AKS'deki bir bölmeye nasıl ekleyip ekleyip ekleyip takabileceğiniz gösterilmektedir.

Kubernetes ciltleri hakkında daha fazla bilgi için [AKS'deki uygulamalar için Depolama seçeneklerine][concepts-storage]bakın.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

Ayrıca Azure CLI sürüm 2.0.59 veya daha sonra yüklenmiş ve yapılandırılmış gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma

Azure Dosyalarını Kubernetes birimi olarak kullanamadan önce bir Azure Depolama hesabı ve dosya paylaşımı oluşturmanız gerekir. Aşağıdaki komutlar *myAKSShare*adlı bir kaynak grubu oluşturmak , bir depolama hesabı ve *aksshare*adlı bir Dosya payı:

```azurecli-interactive
# Change these four parameters as needed for your own environment
AKS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
AKS_PERS_RESOURCE_GROUP=myAKSShare
AKS_PERS_LOCATION=eastus
AKS_PERS_SHARE_NAME=aksshare

# Create a resource group
az group create --name $AKS_PERS_RESOURCE_GROUP --location $AKS_PERS_LOCATION

# Create a storage account
az storage account create -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -l $AKS_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -o tsv)

# Create the file share
az storage share create -n $AKS_PERS_SHARE_NAME --connection-string $AZURE_STORAGE_CONNECTION_STRING

# Get storage account key
STORAGE_KEY=$(az storage account keys list --resource-group $AKS_PERS_RESOURCE_GROUP --account-name $AKS_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" -o tsv)

# Echo storage account name and key
echo Storage account name: $AKS_PERS_STORAGE_ACCOUNT_NAME
echo Storage account key: $STORAGE_KEY
```

Komut dosyası çıktısının sonunda gösterilen depolama hesabı adını ve anahtarını not alın. Kubernetes birimini aşağıdaki adımlardan birinde oluşturduğunuzda bu değerler gereklidir.

## <a name="create-a-kubernetes-secret"></a>Bir Kubernetes sırrı oluşturun

Kubernetes'in önceki adımda oluşturulan dosya paylaşımına erişmek için kimlik bilgilerine ihtiyacı vardır. Bu kimlik bilgileri, bir Kubernetes bölmesi oluşturduğunuzda başvurulan bir [Kubernetes gizli][kubernetes-secret]depolanır.

Sırrı `kubectl create secret` oluşturmak için komutu kullanın. Aşağıdaki örnekte, *azure-secret* adlı paylaşılan bir oluşturma ve önceki adımdan *azurestorageaccountname* ve *azurestorageaccountkey* doldurur. Varolan bir Azure depolama hesabı nı kullanmak için hesap adını ve anahtarını sağlayın.

```console
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=$AKS_PERS_STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=$STORAGE_KEY
```

## <a name="mount-the-file-share-as-a-volume"></a>Dosya payını birim olarak monte edin

Azure Dosyaları paylaşımını bölmenize monte etmek için, birim hacmi kapsayıcı spektrumuna yapılandırın. Aşağıdaki içeriklerle birlikte yeni `azure-files-pod.yaml` bir dosya oluşturun. Dosyalar paylaşımının veya gizli adının adını değiştirdiyseniz, *shareName* ve *secretName'yi*güncelleyin. İstenirse, `mountPath`Dosyalar paylaşımının bölmeye monte edildiği yolu güncelleştirin. Windows Server kapsayıcıları için (şu anda AKS'de önizlemede), *'D:'* gibi Windows yolu kuralını kullanan bir *mountPath* belirtin.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - image: nginx:1.15.5
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
      - name: azure
        mountPath: /mnt/azure
  volumes:
  - name: azure
    azureFile:
      secretName: azure-secret
      shareName: aksshare
      readOnly: false
```

Bölmeyi `kubectl` oluşturmak için komutu kullanın.

```console
kubectl apply -f azure-files-pod.yaml
```

Artık */mnt/azure'a*monte edilmiş bir Azure Dosyaları paylaşımı içeren çalışan bir bölmeniz var. Paylaşımın `kubectl describe pod mypod` başarıyla monte edilebiyi doğrulamak için kullanabilirsiniz. Aşağıdaki yoğunlaştırılmış örnek çıktı, kapsayıcıya monte edilen hacmi gösterir:

```
Containers:
  mypod:
    Container ID:   docker://86d244cfc7c4822401e88f55fd75217d213aa9c3c6a3df169e76e8e25ed28166
    Image:          nginx:1.15.5
    Image ID:       docker-pullable://nginx@sha256:9ad0746d8f2ea6df3a17ba89eca40b48c47066dfab55a75e08e2b70fc80d929e
    State:          Running
      Started:      Sat, 02 Mar 2019 00:05:47 +0000
    Ready:          True
    Mounts:
      /mnt/azure from azure (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-z5sd7 (ro)
[...]
Volumes:
  azure:
    Type:        AzureFile (an Azure File Service mount on the host and bind mount to the pod)
    SecretName:  azure-secret
    ShareName:   aksshare
    ReadOnly:    false
  default-token-z5sd7:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-z5sd7
[...]
```

## <a name="mount-options"></a>Bağlama seçenekleri

*fileMode* ve *dirMode* için varsayılan değer Kubernetes sürüm 1.9.1 ve üzeri için *0755'tir.* Kuberetes sürüm 1.8.5 veya daha büyük ve statik olarak kalıcı hacim nesnesi oluşturan bir küme *kullanıyorsanız, kalıcı Hacim* nesnesi üzerinde montaj seçeneklerinin belirtilmesi gerekir. Aşağıdaki örnek *0777*ayarlar:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: azurefile
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  storageClassName: azurefile
  azureFile:
    secretName: azure-secret
    shareName: aksshare
    readOnly: false
  mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
  - mfsymlinks
  - nobrl
```

Sürüm 1.8.0 - 1.8.4 kümesi kullanıyorsanız, 0 olarak ayarlanmış *runAsUser* *0*değeri ile bir güvenlik bağlamı belirtilebilir. Pod güvenlik bağlamı hakkında daha fazla bilgi için [bkz.][kubernetes-security-context]

Montaj seçeneklerinizi güncelleştirmek için, *PersistentVolume*içeren *azurefile montaj seçenekleri-pv.yaml* dosyası oluşturun. Örnek:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: azurefile
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  storageClassName: azurefile
  azureFile:
    secretName: azure-secret
    shareName: aksshare
    readOnly: false
  mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
  - mfsymlinks
  - nobrl
```

*PersistentVolume'i*kullanan *persistentVolumeClaim* ile *azurefile-mount-options-pvc.yaml* dosyası oluşturun. Örnek:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi
```

PersistentVolume `kubectl` ve *PersistentVolumeClaim* oluşturmak *PersistentVolumeClaim*için komutları kullanın.

```console
kubectl apply -f azurefile-mount-options-pv.yaml
kubectl apply -f azurefile-mount-options-pvc.yaml
```

*PersistentVolumeClaim'inizin* oluşturulduğunu ve *PersistentVolume'e*bağlı olduğunu doğrulayın.

```console
$ kubectl get pvc azurefile

NAME        STATUS   VOLUME      CAPACITY   ACCESS MODES   STORAGECLASS   AGE
azurefile   Bound    azurefile   5Gi        RWX            azurefile      5s
```

*PersistentVolumeClaim'inize* başvurmak için konteyner spektrumunuzu güncelleyin ve bölmenizi güncelleyin. Örnek:

```yaml
...
  volumes:
  - name: azure
    persistentVolumeClaim:
      claimName: azurefile
```

## <a name="next-steps"></a>Sonraki adımlar

İlişkili en iyi uygulamalar [için, AKS'deki depolama ve yedekleme ler için en iyi uygulamalara][operator-best-practices-storage]bakın.

AKS kümeleri Azure Dosyaları ile etkileşim de dahil olmak üzere daha fazla bilgi için [Azure Dosyaları için Kubernetes][kubernetes-files]eklentisi'ne bakın.

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[smb-overview]: /windows/desktop/FileIO/microsoft-smb-protocol-and-cifs-protocol-overview
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-storage-create]: /cli/azure/storage/account#az-storage-account-create
[az-storage-key-list]: /cli/azure/storage/account/keys#az-storage-account-keys-list
[az-storage-share-create]: /cli/azure/storage/share#az-storage-share-create
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[operator-best-practices-storage]: operator-best-practices-storage.md
[concepts-storage]: concepts-storage.md
