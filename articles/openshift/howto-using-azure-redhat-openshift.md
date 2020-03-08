---
title: Azure Red Hat Openshıft 4,3 kümesi oluşturma | Microsoft Docs
description: Azure Red Hat OpenShift 3,11 ile küme oluşturma
author: lamek
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 03/06/2020
keywords: Aro, OpenShift, az Aro, Red hat, CLI
ms.openlocfilehash: 81f8edb42be1f73692062d36440890ef5a1e7c9a
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78899314"
---
# <a name="create-access-and-manage-an-azure-red-hat-openshift-43-cluster"></a>Azure Red Hat OpenShift 4,3 kümesi oluşturma, erişme ve yönetme

> [!IMPORTANT]
> Azure Red Hat OpenShift (ARO) 4,3, önizleme aşamasında sunulmaktadır. Önizleme özellikleri Self-Service ' dir ve olduğu gibi sağlanır ve hizmet düzeyi sözleşmesi (SLA) ve sınırlı garantiden çıkarılır. Bu nedenle, Özellikler üretim kullanımı için tasarlanmamıştır.

## <a name="prerequisites"></a>Önkoşullar

Azure Red Hat OpenShift 4,3 kümesi oluşturmak için şunlar gerekir:

- Azure CLı sürüm 2.0.72 veya üzeri
  
- ' Az Aro ' uzantısı

- Her biri ağ güvenlik grubu iliştirilmemiş iki boş alt ağ içeren bir sanal ağ.  Kümeniz bu alt ağlara dağıtılacak.

- Sizin için otomatik olarak bir AAD uygulaması ve hizmet sorumlusu oluşturmak üzere `az aro create` için bir küme AAD uygulaması (istemci KIMLIĞI ve gizli dizi) ve hizmet sorumlusu ya da yeterli AAD izinleri.

- RP hizmet sorumlusu ve küme hizmeti sorumlusu, her birinin küme sanal ağı üzerinde katkıda bulunan rolüne sahip olması gerekir.  Sanal ağ üzerinde "Kullanıcı Erişim Yöneticisi" rolüne sahipseniz `az aro create`, rol atamalarını sizin için otomatik olarak ayarlar.

### <a name="install-the-az-aro-extension"></a>' Az Aro ' uzantısını yükler
`az aro` uzantısı, Azure CLı kullanarak doğrudan komut satırından Azure Red Hat OpenShift kümelerini oluşturmanıza, erişimlerinize ve silmesine izin verir.

> [!Note] 
> `az aro` uzantısı, önizleme aşamasında CurrentY. Gelecekteki bir sürümde değiştirilmiş veya kaldırılmış olabilir.
> `az aro` uzantısı önizlemesini kabul etmek için `Microsoft.RedHatOpenShift` kaynak sağlayıcısını kaydetmeniz gerekir.
> 
>    ```console
>    az provider register -n Microsoft.RedHatOpenShift --wait
>    ```

1. Azure'da oturum açın.

   ```console
   az login
   ```

2. `az aro` uzantısını yüklemek için şu komutu çalıştırın:

   ```console
   az extension add --source https://arosvc.blob.core.windows.net/az-preview/aro-0.1.0-py2.py3-none-any.whl
   ```

3. ARO uzantısının kayıtlı olduğunu doğrulayın.

   ```console
   az -v
   ...
   Extensions:
   aro                                0.1.0
   ...
   ```
  
### <a name="create-a-virtual-network-containing-two-empty-subnets"></a>İki boş alt ağ içeren bir sanal ağ oluşturun

İki boş alt ağ içeren bir sanal ağ oluşturmak için bu adımları izleyin.

1. Aşağıdaki değişkenleri ayarlayın.

   ```console
   LOCATION=eastus        #the location of your cluster
   RESOURCEGROUP="v4-$LOCATION"    #the name of the resource group where you want to create your cluster
   CLUSTER=cluster        #the name of your cluster
   ```

2. Kümeniz için bir kaynak grubu oluşturun.

   ```console
   az group create -g "$RESOURCEGROUP" -l $LOCATION
   ```

3. Sanal ağı oluşturun.

   ```console
   az network vnet create \
     -g "$RESOURCEGROUP" \
     -n vnet \
     --address-prefixes 10.0.0.0/9 \
     >/dev/null
   ```

4. Sanal ağınıza iki boş alt ağ ekleyin.

   ```console
    for subnet in "$CLUSTER-master" "$CLUSTER-worker"; do
     az network vnet subnet create \
       -g "$RESOURCEGROUP" \
       --vnet-name vnet \
       -n "$subnet" \
       --address-prefixes 10.$((RANDOM & 127)).$((RANDOM & 255)).0/24 \
       --service-endpoints Microsoft.ContainerRegistry \
       >/dev/null
   done
   ```

5. Sanal ağınızda ve alt ağlarınızın özel bağlantı hizmeti için ağ ilkelerini devre dışı bırakın. Bu, ARO hizmetinin kümeye erişmesi ve onu yönetmesi için bir gereksinimdir.

   ```console
   az network vnet subnet update \
     -g "$RESOURCEGROUP" \
     --vnet-name vnet \
     -n "$CLUSTER-master" \
     --disable-private-link-service-network-policies true \
     >/dev/null
   ```

## <a name="create-a-cluster"></a>Küme oluşturma

Bir küme oluşturmak için aşağıdaki komutu çalıştırın.

```console
az aro create \
  -g "$RESOURCEGROUP" \
  -n "$CLUSTER" \
  --vnet vnet \
  --master-subnet "$CLUSTER-master" \
  --worker-subnet "$CLUSTER-worker"
```

>[!NOTE]
> Genellikle bir küme oluşturmak yaklaşık 35 dakika sürer.

## <a name="access-the-cluster-console"></a>Küme konsoluna erişin

Küme konsolu URL 'sini (`https://console-openshift-console.apps.<random>.<location>.aroapp.io/`) Azure Red Hat OpenShift 4,3 küme kaynağı altında bulabilirsiniz. Kaynağı görüntülemek için aşağıdaki komutu çalıştırın:

```console
az aro list -o table
```

`kubeadmin` kullanıcısını kullanarak kümede oturum açabilirsiniz.  `kubeadmin` kullanıcısının parolasını bulmak için aşağıdaki komutu çalıştırın:

```dotnetcli
az aro list-credentials -g "$RESOURCEGROUP" -n "$CLUSTER"
```

## <a name="delete-a-cluster"></a>Küme silme

Kümeyi silmek için aşağıdaki komutu çalıştırın.

```console
az aro delete -g "$RESOURCEGROUP" -n "$CLUSTER"

# (optional)
for subnet in "$CLUSTER-master" "$CLUSTER-worker"; do
  az network vnet subnet delete -g "$RESOURCEGROUP" --vnet-name vnet -n "$subnet"
done
```