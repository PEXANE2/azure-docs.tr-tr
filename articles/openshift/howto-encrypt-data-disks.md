---
title: Azure Red Hat OpenShift (ARO) üzerinde müşteri tarafından yönetilen bir anahtarla (CMK) kalıcı birim taleplerini şifreleyin
description: Kendi anahtarını getir (BYOK)/müşteri tarafından yönetilen anahtar (CMK) Azure Red Hat OpenShift için dağıtım yönergeleri
ms.topic: article
ms.date: 02/24/2021
author: stuartatmicrosoft
ms.author: stkirk
ms.service: azure-redhat-openshift
keywords: şifreleme, bYok, Aro, OpenShift, Red Hat
ms.openlocfilehash: 09e1f92a967c7b77d3bb8e27769f4cafd4fd4f53
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102055005"
---
# <a name="encrypt-persistent-volume-claims-with-a-customer-managed-key-cmk-on-azure-red-hat-openshift-aro-preview"></a>Azure Red Hat OpenShift (ARO) ile kalıcı birim taleplerini, müşteri tarafından yönetilen anahtar (CMK) ile şifreleme (Önizleme)

Azure depolama, bekleyen bir depolama hesabındaki tüm verileri şifreler. Varsayılan olarak, veriler işletim sistemi ve veri diskleri içeren Microsoft Platformu tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde daha fazla denetim için, Azure Red Hat OpenShift kümelerinizdeki verileri şifrelemek üzere müşteri tarafından yönetilen anahtarlar sağlayabilirsiniz.

> [!NOTE]
> Bu aşamada, destek yalnızca, müşteri tarafından yönetilen anahtarlarla ARO kalıcı birimlerini şifrelemek için mevcuttur. Bu özellik şu anda işletim sistemi diskleri için kullanılamaz.

> [!IMPORTANT]
> ARO Önizleme özellikleri self servis, kabul etme esasına göre sunulmaktadır. Önizlemeler "olduğu gibi" ve "kullanılabilir olarak" verilmiştir ve hizmet düzeyi anlaşmalarından ve sınırlı garantiden çıkarılır. ARO önizlemeleri, müşteri desteği tarafından kısmen bir en iyi performans kapsamında ele alınmıştır. Bu nedenle, bu özellikler üretim kullanımı için tasarlanmamıştır.

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede şu şekilde varsayılmaktadır:

* OpenShift sürüm 4,4 (veya üzeri) üzerinde önceden var olan bir ARO kümeniz var.

* ' OC ' OpenShift komut satırı aracınız, Base64 (temel yardımcı programlar 'ın parçası) ve ' az ' Azure CLı yüklü.

* Genel küme Yönetici kullanıcısı (kubeadmin) olarak *OC* kullanarak Aro kümenizde oturum açarsınız.

* Azure CLı 'da, \ ' i kullanarak, ARO kümesiyle aynı abonelikte " *katkıda bulunan"* erişimine izin vermek için yetkilendirilmiş bir hesapla oturum açarsınız.

## <a name="limitations"></a>Sınırlamalar

* Müşteri tarafından yönetilen anahtar şifrelemesi için kullanılabilirlik bölgeye özeldir. Belirli bir Azure bölgesinin durumunu görmek için [Azure bölgelerini][supported-regions]inceleyin.
* Ultra diskler kullanıyorsanız, başlamadan önce aboneliğinizde Ultra diskleri etkinleştirin.

## <a name="create-an-azure-key-vault-instance"></a>Azure Key Vault örneği oluşturma
Anahtarlarınızı depolamak için bir Azure Key Vault örneği kullanılmalıdır. Temizleme koruması ve geçici silme etkinken yeni bir Key Vault oluşturun. Ardından, kendi özel anahtarınızı depolamak için kasa içinde yeni bir anahtar oluşturun:

```azurecli-interactive
# Create an Azure Key Vault resource in a supported Azure region
az keyvault create -n $vaultName -g $buildRG --enable-purge-protection true -o table
# Create the actual key within the Azure Key Vault
az keyvault key create --vault-name $vaultName --name $vaultKeyName --protection software -o jsonc
```

## <a name="create-an-azure-disk-encryption-set"></a>Azure disk şifreleme kümesi oluşturma

Azure disk şifrelemesi kümesi, ARO 'daki diskler için başvuru noktası olarak kullanılır. Bu, önceki adımda oluşturduğumuz Azure Key Vault bağlanır ve müşteri tarafından yönetilen anahtarları bu konumdan çeker.

```azurecli-interactive
# Retrieve the Key Vault Id and store it in a variable
keyVaultId="$(az keyvault show --name $vaultName --query [id] -o tsv)"

# Retrieve the Key Vault key URL and store it in a variable
keyVaultKeyUrl="$(az keyvault key show --vault-name $vaultName --name $vaultKeyName  --query [key.kid] -o tsv)"

# Create an Azure disk encryption set
az disk-encryption-set create -n $desName -g $myRG --source-vault $keyVaultId --key-url $keyVaultKeyUrl -o table
```

## <a name="grant-the-disk-encryption-set-access-to-key-vault"></a>Disk şifrelemeyi Key Vault için erişim izni ver
Önceki adımlarda oluşturduğumuz disk şifreleme kümesini kullanın ve disk şifrelemeyi Azure Key Vault için erişim izni verin:

```azurecli-interactive
# First, find the disk encryption set's AppId value.
desIdentity="$(az disk-encryption-set show -n $desName -g $myRG --query [identity.principalId] -o tsv)"

# Next, update the Key Vault security policy settings to allow access to the disk encryption set.
az keyvault set-policy -n $vaultName -g $myRG --object-id $desIdentity --key-permissions wrapkey unwrapkey get -o table

# Now, ensure the disk encryption set can read the contents of the Azure Key Vault.
az role assignment create --assignee $desIdentity --role Reader --scope $keyVaultId -o jsonc
```

### <a name="obtain-other-ids-required-for-role-assignments"></a>Rol atamaları için gereken diğer kimlikleri alma
ARO kümesinin, ARO kümesindeki kalıcı birim taleplerini (PVC) şifrelemek için disk şifreleme kümesini kullanmasına izin vermemiz gerekiyor. Bunu yapmak için yeni bir Yönetilen Hizmet Kimliği (MSI) oluşturacağız. Ayrıca, ARO MSI ve disk şifreleme kümesi için de diğer izinleri ayarlayacağız.
```
# First, get the application ID of the service principal used in the ARO cluster.
aroSPAppId="$(oc get secret azure-credentials -n kube-system -o jsonpath='{.data.azure_client_id}' | base64 --decode)"

# Next, get the object ID of the service principal used in the ARO cluster.
aroSPObjId="$(az ad sp show --id $aroSPAppId -o tsv --query [objectId])"

# Set the name of the ARO Managed Service Identity. 
msiName="$aroCluster-msi"

# Create the Managed Service Identity (MSI) required for disk encryption.
az identity create -g $myRG -n $msiName -o jsonc

# Get the ARO Managed Service Identity application ID.
aroMSIAppId="$(az identity show -n $msiName -g $myRG -o tsv --query [clientId])"

# Get the resource ID for the disk encryption set and the Key Vault resource group.
myRGResourceId="$(az group show -n $myRG -o tsv --query [id])"
```

### <a name="implement-other-role-assignments-required-for-byokcmk-encryption"></a>BYOK/CMK şifrelemesi için gereken diğer rol atamalarını Uygula
Önceki adımda elde edilen değişkenleri kullanarak gerekli rol atamalarını uygulayın:

```azurecli-interactive
# Ensure the Azure Disk Encryption Set can read the contents of the Azure Key Vault.
az role assignment create --assignee $desIdentity --role Reader --scope $keyVaultId -o jsonc

# Assign the MSI AppID 'Reader' permission over the disk encryption set & Key Vault resource group.
az role assignment create --assignee $aroMSIAppId --role Reader --scope $myRGResourceId -o jsonc

# Assign the ARO Service Principal 'Contributor' permission over the disk encryption set & Key Vault Resource Group.
az role assignment create --assignee $aroSPObjId --role Contributor --scope $myRGResourceId -o jsonc
```

## <a name="create-a-k8s-storage-class-for-encrypted-premium--ultra-disks-optional"></a>Şifrelenmiş Premium & Ultra diskler için k8s depolama sınıfı oluşturma (isteğe bağlı)
Premium_LRS ve UltraSSD_LRS diskleri için BYOK/CMK için kullanılacak depolama sınıflarını oluşturun:
```
# Premium Disks
cat > managed-premium-encrypted-byok.yaml<< EOF
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: managed-premium-encrypted-byok
provisioner: kubernetes.io/azure-disk
parameters:
  skuname: Premium_LRS
  kind: Managed
  diskEncryptionSetID: "/subscriptions/subId/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/desName"
  resourceGroup: myRG
reclaimPolicy: Delete
allowVolumeExpansion: true
volumeBindingMode: WaitForFirstConsumer
EOF

# Ultra Disks
cat > managed-ultra-encrypted-byok.yaml<< EOF
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: managed-ultra-encrypted-byok
provisioner: kubernetes.io/azure-disk
parameters:
  skuname: UltraSSD_LRS
  kind: Managed
  diskEncryptionSetID: "/subscriptions/subId/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/desName"
  resourceGroup: myRG
  cachingmode: None
  diskIopsReadWrite: "2000"  # minimum value: 2 IOPS/GiB
  diskMbpsReadWrite: "320"   # minimum value: 0.032/GiB
reclaimPolicy: Delete
allowVolumeExpansion: true
volumeBindingMode: WaitForFirstConsumer
EOF
```
### <a name="set-up-your-storage-class-configuration"></a>Depolama sınıfı yapılandırmanızı ayarlama
ARO kümeniz için benzersiz olan değişkenleri iki depolama sınıfı yapılandırma dosyasına değiştirin:
```
# Insert your current active subscription ID into the configuration
sed -i "s/subId/$subId/g" managed-premium-encrypted-byok.yaml
sed -i "s/subId/$subId/g" managed-ultra-encrypted-byok.yaml

# Replace the name of the Resource Group which contains the disk encryption set and Key Vault
sed -i "s/myRG/$myRG/g" managed-premium-encrypted-byok.yaml
sed -i "s/myRG/$myRG/g" managed-ultra-encrypted-byok.yaml

# Replace the name of the disk encryption set
sed -i "s/desName/$desName/g" managed-premium-encrypted-byok.yaml
sed -i "s/desName/$desName/g" managed-ultra-encrypted-byok.yaml
```
Daha sonra, depolama sınıfı yapılandırmasını uygulamak için bu dağıtımı ARO kümenizde çalıştırın:
```
# Update cluster with the new storage classes
oc apply -f managed-premium-encrypted-byok.yaml
oc apply -f managed-ultra-encrypted-byok.yaml
```
## <a name="test-encryption-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla şifrelemeyi test etme
Kümenizin, PVC şifrelemesi için müşteri tarafından yönetilen bir anahtar kullanıp kullanmediğini denetlemek için uygun depolama sınıfını kullanarak kalıcı bir birim talebi oluşturacağız. Aşağıdaki kod parçacığı bir pod oluşturur ve Standart diskler kullanılarak kalıcı bir birim talebi bağlar
```
# Create a pod which uses a persistent volume claim referencing the new storage class
cat > test-pvc.yaml<< EOF
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mypod-with-byok-encryption-pvc
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: managed-premium-encrypted-byok
  resources:
    requests:
      storage: 1Gi
---
kind: Pod
apiVersion: v1
metadata:
  name: mypod-with-byok-encryption
spec:
  containers:
  - name: mypod-with-byok-encryption
    image: nginx:1.15.5
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 250m
        memory: 256Mi
    volumeMounts:
    - mountPath: "/mnt/azure"
      name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: mypod-with-byok-encryption-pvc
EOF
```
### <a name="apply-the-test-pod-configuration-file"></a>Test Pod yapılandırma dosyasını Uygula
Test Pod yapılandırmasını uygulamak ve yeni kalıcı birim talebinin UID 'sini döndürmek için aşağıdaki komutları yürütün. UID, diskin BYOK/CMK kullanılarak şifrelendiğini doğrulamak için kullanılacaktır.
```
# Apply the test pod configuration file and set the PVC UID as a variable to query in Azure later.
pvcUid="$(oc apply -f test-pvc.yaml -o json | jq -r '.items[0].metadata.uid')"

# Determine the full Azure Disk name.
pvName="$(oc get pv pvc-$pvcUid -o json |jq -r '.spec.azureDisk.diskName')"
```
### <a name="verify-pvc-disk-is-configured-with-encryptionatrestwithcustomerkey"></a>PVC diskinin "EncryptionAtRestWithCustomerKey" ile yapılandırıldığını doğrulama 
Pod, BYOK/CMK depolama sınıfına başvuran kalıcı bir birim talebi oluşturmamalıdır. Aşağıdaki komutun çalıştırılması, PVC 'nin beklendiği gibi dağıtıldığını doğrular:
```azurecli-interactive
# Describe the OpenShift cluster-wide persistent volume claims
oc describe pvc

# Verify with Azure that the disk is encrypted with a customer-managed key
az disk show -n $pvName -g $myRG -o json --query [encryption]
```

## <a name="next-steps"></a>Sonraki adımlar

<!-- LINKS - external -->

<!-- LINKS - internal -->
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[best-practices-security]: /azure/aks/operator-best-practices-cluster-security
[byok-azure-portal]: /azure/storage/common/storage-encryption-keys-portal
[customer-managed-keys]: /azure/virtual-machines/windows/disk-encryption#customer-managed-keys
[key-vault-generate]: /azure/key-vault/key-vault-manage-with-cli2
[supported-regions]: /azure/virtual-machines/windows/disk-encryption#supported-regions

