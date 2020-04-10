---
title: Azure Kırmızı Şapka OpenShift 4.3 Kümesi Oluşturma | Microsoft Dokümanlar
description: Azure Red Hat OpenShift 4.3 ile küme oluşturma
author: lamek
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 03/06/2020
keywords: aro, openshift, az aro, kırmızı şapka, cli
ms.openlocfilehash: 9488ef593cf4ec8600dcb42ea4a2cefa4fcb1446
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80998792"
---
# <a name="create-access-and-manage-an-azure-red-hat-openshift-43-cluster"></a>Azure Red Hat OpenShift 4.3 Cluster oluşturma, erişme ve yönetme

> [!IMPORTANT]
> Azure Red Hat OpenShift 4.3'ün şu anda yalnızca Doğu ABD ve Doğu ABD 2'de özel önizlemede kullanılabildiğini lütfen unutmayın. Özel önizleme kabul sadece davetiye ile. Lütfen bu özelliği etkinleştirmeye çalışmadan önce aboneliğinizi kaydettiğinizden emin olun: [Azure Red Hat OpenShift Özel Önizleme Kaydı](https://aka.ms/aro-preview-register)

> [!NOTE]
> Önizleme özellikleri self servistir ve olduğu gibi ve mevcut olduğu şekilde sağlanır ve hizmet düzeyi sözleşmesi (SLA) ve sınırlı garanti dışında dır. Bu nedenle, özellikler üretim kullanımı için değildir.

## <a name="prerequisites"></a>Ön koşullar

Bir Azure Red Hat OpenShift 4.3 kümesi oluşturmak için aşağıdakilere ihtiyacınız olacak:

- Azure CLI sürümü 2.0.72 veya üzeri
  
- 'az aro' uzantısı

- Her biri ağ güvenlik grubu eklenmemiş iki boş alt ağ içeren bir sanal ağ.  Kümeniz bu alt ağlara dağıtılacak.

- Bir küme AAD uygulaması (istemci kimliği ve gizli) ve hizmet `az aro create` sorumlusu veya otomatik olarak sizin için bir AAD uygulaması ve hizmet anadili oluşturmak için yeterli AAD izinleri.

- RP hizmet sorumlusu ve küme hizmet sorumlusunun her biri küme sanal ağında Katılımcı rolüne sahip olmalıdır.  Sanal ağda "Kullanıcı Erişim Yöneticisi" rolü `az aro create` varsa, rol atamaları sizin için otomatik olarak ayarlar.

### <a name="install-the-az-aro-extension"></a>'az aro' uzantısını yükleyin
Uzantı, `az aro` Azure CLI'yi kullanarak Azure Red Hat OpenShift kümelerini doğrudan komut satırından oluşturmanıza, erişmenize ve silmenize olanak tanır.

> [!Note] 
> Uzantı `az aro` önizlemede günceldir. Gelecekteki bir sürümde değiştirilebilir veya kaldırılabilir.
> Uzantı önizlemesini `az aro` kabul etmek için kaynak `Microsoft.RedHatOpenShift` sağlayıcısını kaydetmeniz gerekir.
> 
>    ```console
>    az provider register -n Microsoft.RedHatOpenShift --wait
>    ```

1. Azure'da oturum açın.

   ```console
   az login
   ```

2. Uzantıyı yüklemek için `az aro` aşağıdaki komutu çalıştırın:

   ```console
   az extension add -n aro --index https://az.aroapp.io/preview
   ```

3. ARO uzantısının kayıtlı olduğunu doğrulayın.

   ```console
   az -v
   ...
   Extensions:
   aro                                0.3.0
   ...
   ```
  
### <a name="create-a-virtual-network-containing-two-empty-subnets"></a>İki boş alt ağ içeren bir sanal ağ oluşturma

İki boş alt ağ içeren bir sanal ağ oluşturmak için aşağıdaki adımları izleyin.

1. Aşağıdaki değişkenleri ayarlayın.

   ```console
   LOCATION=eastus        #the location of your cluster
   RESOURCEGROUP="v4-$LOCATION"    #the name of the resource group where you want to create your cluster
   CLUSTER=cluster        #the name of your cluster
   PULL_SECRET="<optional-pull-secret>"
   ```
   >[!NOTE]
   > İsteğe bağlı çekme sırrı, kümenizin ek içerikle birlikte Red Hat kapsayıcı kayıtlarına erişmesini sağlar.
   >
   > Copy Pull Secret'a https://cloud.redhat.com/openshift/install/azure/installer-provisioned gidip tıklatarak çekme sırrınıza *erişin.*
   >
   > Red Hat hesabınızda oturum açmanız veya iş e-postanızla yeni bir Red Hat hesabı oluşturmanız ve şart ve koşulları kabul etmeniz gerekir.
 

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

5. Özel Bağlantı Hizmeti ağ ilkelerini sanal ağınızda ve alt ağlarınızda devre dışı k.s. Bu, ARO hizmetinin kümeye erişmeve yönetmesi için bir gerekliliktir.

   ```console
   az network vnet subnet update \
     -g "$RESOURCEGROUP" \
     --vnet-name vnet \
     -n "$CLUSTER-master" \
     --disable-private-link-service-network-policies true \
     >/dev/null
   ```

## <a name="create-a-cluster"></a>Küme oluşturma

Küme oluşturmak için aşağıdaki komutu çalıştırın.

```console
az aro create \
  -g "$RESOURCEGROUP" \
  -n "$CLUSTER" \
  --vnet vnet \
  --master-subnet "$CLUSTER-master" \
  --worker-subnet "$CLUSTER-worker" \
  --cluster-resource-group "aro-$CLUSTER" \
  --domain "$CLUSTER" \
  --pull-secret "$PULL_SECRET"
```

>[!NOTE]
> Normalde bir küme oluşturmak için yaklaşık 35 dakika sürer.

## <a name="access-the-cluster-console"></a>Küme konsoluna erişin

Azure Red Hat OpenShift 4.3 küme kaynağının altında küme konsolu URL'sini (formun) `https://console-openshift-console.apps.<random>.<location>.aroapp.io/`bulabilirsiniz. Kaynağı görüntülemek için aşağıdaki komutu çalıştırın:

```console
az aro list -o table
```

`kubeadmin` Kullanıcıyı kullanarak kümeye giriş yapabilirsiniz.  Kullanıcının parolasını bulmak için `kubeadmin` aşağıdaki komutu çalıştırın:

```dotnetcli
az aro list-credentials -g "$RESOURCEGROUP" -n "$CLUSTER"
```

## <a name="delete-a-cluster"></a>Küme silme

Bir kümeyi silmek için aşağıdaki komutu çalıştırın.

```console
az aro delete -g "$RESOURCEGROUP" -n "$CLUSTER"

# (optional)
for subnet in "$CLUSTER-master" "$CLUSTER-worker"; do
  az network vnet subnet delete -g "$RESOURCEGROUP" --vnet-name vnet -n "$subnet"
done
```
