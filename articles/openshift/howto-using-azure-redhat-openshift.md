---
title: Azure Red Hat Openshıft 4,3 kümesi oluşturma | Microsoft Docs
description: Azure Red Hat OpenShift 4,3 ile küme oluşturma
author: lamek
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 03/06/2020
keywords: Aro, OpenShift, az Aro, Red hat, CLI
ms.openlocfilehash: f909c5870be6e394e457ad8f44ea5a253054ffe6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81398887"
---
# <a name="create-access-and-manage-an-azure-red-hat-openshift-43-cluster"></a>Azure Red Hat OpenShift 4,3 kümesi oluşturma, erişme ve yönetme

> [!IMPORTANT]
> Azure Red Hat Openshıft 4,3 'in Şu anda yalnızca Doğu ABD ve Doğu ABD 2 özel önizlemede kullanılabildiğini lütfen unutmayın. Özel Önizleme kabulü yalnızca davet tarafından yapılır. Lütfen bu özelliği etkinleştirmeyi denemeden önce aboneliğinizi kaydettiğinizden emin olun: [Azure Red Hat OpenShift özel önizleme kaydı](https://aka.ms/aro-preview-register)

> [!NOTE]
> Önizleme özellikleri Self-Service ' dir ve olduğu gibi sağlanır ve hizmet düzeyi sözleşmesi (SLA) ve sınırlı garantiden çıkarılır. Bu nedenle, Özellikler üretim kullanımı için tasarlanmamıştır.

## <a name="prerequisites"></a>Ön koşullar

Azure Red Hat OpenShift 4,3 kümesi oluşturmak için şunlar gerekir:

- Azure CLı sürüm 2.0.72 veya üzeri
  
- ' Az Aro ' uzantısı

- Her biri ağ güvenlik grubu iliştirilmemiş iki boş alt ağ içeren bir sanal ağ.  Kümeniz bu alt ağlara dağıtılacak.

- Otomatik olarak bir AAD uygulaması ve hizmet sorumlusu oluşturmak için bir küme AAD uygulaması (istemci KIMLIĞI ve gizli `az aro create` anahtarı) ve hizmet sorumlusu ya da yeterli AAD izinleri.

- RP hizmet sorumlusu ve küme hizmeti sorumlusu, her birinin küme sanal ağı üzerinde katkıda bulunan rolüne sahip olması gerekir.  Sanal ağ üzerinde "Kullanıcı Erişim Yöneticisi" rolüne sahipseniz, `az aro create` rol atamalarını sizin için otomatik olarak ayarlar.

### <a name="install-the-az-aro-extension"></a>' Az Aro ' uzantısını yükler
`az aro` Uzantı, Azure CLI kullanarak doğrudan komut satırından Azure Red Hat OpenShift kümelerini oluşturmanıza, erişimlerinize ve silmesine izin verir.

> [!Note] 
> `az aro` Uzantı, önizlemede CurrentY. Gelecekteki bir sürümde değiştirilmiş veya kaldırılmış olabilir.
> `az aro` Uzantı önizlemesini kabul etmek için `Microsoft.RedHatOpenShift` kaynak sağlayıcısını kaydetmeniz gerekir.
> 
>    ```console
>    az provider register -n Microsoft.RedHatOpenShift --wait
>    ```

1. Azure'da oturum açın.

   ```console
   az login
   ```

2. `az aro` Uzantıyı yüklemek için şu komutu çalıştırın:

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

### <a name="get-a-red-hat-pull-secret-optional"></a>Red hat çekme gizli anahtarı alma (isteğe bağlı)

Red hat çekme gizli anahtarı, kümenizin Red Hat kapsayıcısı kayıt defterlerine ve ek içeriğe erişmesine olanak sağlar. Bir çekme parolasının kullanılması isteğe bağlıdır, ancak önerilir.

Çekme gizli dizisini almak için:

1. https://cloud.redhat.com/openshift/install/azure/aro-provisioned kısmına gidin.
1. Red Hat hesabınızda oturum açın veya iş e-postanızı kullanarak yeni bir Red Hat hesabı oluşturun; hüküm ve koşulları kabul edin.
1. **Çekme gizli anahtarını indir**' i seçin.

*Pull-Secret. txt* dosyasını güvenli bir yere kaydedin; dosyayı her bir küme oluşturduğunuzda kullanacaksınız.

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
  --worker-subnet "$CLUSTER-worker" \
  --cluster-resource-group "aro-$CLUSTER" \
  --domain "$CLUSTER" \
  --pull-secret @pull-secret.txt
```

>[!NOTE]
> Genellikle bir küme oluşturmak yaklaşık 35 dakika sürer.

## <a name="access-the-cluster-console"></a>Küme konsoluna erişin

Küme konsolu URL 'sini (formun `https://console-openshift-console.apps.<random>.<location>.aroapp.io/`) Azure Red Hat openshift 4,3 küme kaynağı altında bulabilirsiniz. Kaynağı görüntülemek için aşağıdaki komutu çalıştırın:

```console
az aro list -o table
```

`kubeadmin` Kullanıcıyı kullanarak kümede oturum açabilirsiniz.  `kubeadmin` Kullanıcı parolasını bulmak için aşağıdaki komutu çalıştırın:

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
