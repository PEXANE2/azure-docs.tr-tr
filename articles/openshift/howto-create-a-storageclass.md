---
title: Azure Red Hat OpenShift 4 üzerinde bir Azure Dosyalar StorageClass oluşturma
description: Azure Red Hat OpenShift üzerinde bir Azure dosyaları StorageClass oluşturma hakkında bilgi edinin
ms.service: container-service
ms.topic: article
ms.date: 10/16/2020
author: grantomation
ms.author: b-grodel
keywords: Aro, OpenShift, az Aro, Red hat, CLI, Azure dosyası
ms.custom: mvc
ms.openlocfilehash: a7415a481b133c2f528ba4636c0297ce5cfa23a7
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92747883"
---
# <a name="create-an-azure-files-storageclass-on-azure-red-hat-openshift-4"></a>Azure Red Hat OpenShift 4 üzerinde bir Azure Dosyalar StorageClass oluşturma

Bu makalede, Azure Red Hat OpenShift 4 için Azure dosyalarını kullanarak ReadWriteMany (RWX) depolama alanını dinamik olarak sağlayan bir StorageClass oluşturacaksınız. Şunları öğreneceksiniz:

> [!div class="checklist"]
> * Önkoşulları kurun ve gerekli araçları kurun
> * Azure dosya hazırlayıcısı ile Azure Red Hat OpenShift 4 StorageClass oluşturma

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğreticide, Azure CLı sürüm 2.6.0 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="before-you-begin"></a>Başlamadan önce

Aboneliğinize bir Azure Red Hat OpenShift 4 kümesi dağıtın, bkz. [Azure Red Hat OpenShift 4 kümesi oluşturma](tutorial-create-cluster.md)


### <a name="set-up-azure-storage-account"></a>Azure depolama hesabı ayarlama

Bu adım, Azure Red Hat OpenShift (ARO) kümesinin kaynak grubu dışında bir kaynak grubu oluşturur. Bu kaynak grubu, Azure Red Hat OpenShift 'in dinamik hazırlayıcı tarafından oluşturulan Azure dosya paylaşımlarını içerecektir.

```bash
AZURE_FILES_RESOURCE_GROUP=aro_azure_files
LOCATION=eastus

az group create -l $LOCATION -n $AZURE_FILES_RESOURCE_GROUP

AZURE_STORAGE_ACCOUNT_NAME=aroazurefilessa

az storage account create \
    --name $AZURE_STORAGE_ACCOUNT_NAME \
    --resource-group $AZURE_FILES_RESOURCE_GROUP \
    --kind StorageV2 \
    --sku Standard_LRS
```

## <a name="set-permissions"></a>İzinleri ayarlama
### <a name="set-resource-group-permissions"></a>Kaynak grubu izinlerini ayarla

ARO hizmeti sorumlusu, yeni Azure depolama hesabı kaynak grubunda ' listKeys ' iznine sahip olmalıdır. Bunu başarmak için ' katkıda bulunan ' rolünü atayın. 

```bash
ARO_RESOURCE_GROUP=aro-rg
CLUSTER=cluster
ARO_SERVICE_PRINCIPAL_ID=$(az aro show -g $ARO_RESOURCE_GROUP -n $CLUSTER –-query servicePrincipalProfile.clientId -o tsv)

az role assignment create –-role Contributor -–assignee $ARO_SERVICE_PRINCIPAL_ID -g $AZURE_FILES_RESOURCE_GROUP
```

### <a name="set-aro-cluster-permissions"></a>ARO küme izinlerini ayarlama

OpenShift kalıcı birim bağlayıcı hizmeti hesabı gizli dizileri okuyabilme olanağı gerektirir. Bunu başarmak için bir OpenShift kümesi rolü oluşturun ve atayın.
```bash
ARO_API_SERVER=$(az aro list --query "[?contains(name,'$CLUSTER')].[apiserverProfile.url]" -o tsv)

oc login -u kubeadmin -p $(az aro list-credentials -g $ARO_RESOURCE_GROUP -n $CLUSTER --query=kubeadminPassword -o tsv) $APISERVER

oc create clusterrole azure-secret-reader \
    --verb=create,get \
    --resource=secrets

oc adm policy add-cluster-role-to-user azure-secret-reader system:serviceaccount:kube-system:persistent-volume-binder
```

## <a name="create-storageclass-with-azure-files-provisioner"></a>Azure dosyaları hazırlayıcı ile StorageClass oluşturma

Bu adım, bir Azure dosyaları hazırlayıcısı ile bir StorageClass oluşturur. StorageClass bildiriminde, bir depolama hesabının ayrıntıları, ARO kümesinin geçerli kaynak grubunun dışında bir depolama hesabına bakmasını bilmesi için gereklidir.

```bash
cat << EOF >> azure-storageclass-azure-file.yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azure-file
provisioner: kubernetes.io/azure-file
parameters:
  location: $LOCATION
  skuName: Standard_LRS 
  storageAccount: $AZURE_STORAGE_ACCOUNT_NAME
  resourceGroup: $AZURE_FILES_RESOURCE_GROUP
reclaimPolicy: Delete
volumeBindingMode: Immediate
EOF

oc create -f azure-storageclass-azure-file.yaml
```

## <a name="change-the-default-storageclass-optional"></a>Varsayılan StorageClass 'ı değiştirme (isteğe bağlı)

ARO üzerindeki varsayılan StorageClass, Managed-Premium olarak adlandırılır ve Azure-disk provisioner kullanır. Bunu, StorageClass bildirimlerinde düzeltme eki komutları vererek değiştirin.

```bash
oc patch storageclass managed-premium -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"false"}}}'

oc patch storageclass azure-file -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

## <a name="verify-azure-file-storageclass-optional"></a>Azure dosyası StorageClass 'ı doğrula (isteğe bağlı)

Yeni bir uygulama oluşturun ve buna depolama alanı atayın.

```bash
oc new-project azfiletest
oc new-app –template httpd-example

#Wait for the pod to become Ready
curl $(oc get route httpd-example -n azfiletest -o jsonpath={.spec.host})

oc set volume dc/httpd-example --add --name=v1 -t pvc --claim-size=1G -m /data

#Wait for the new deployment to rollout
export POD=$(oc get pods --field-selector=status.phase==Running -o jsonpath={.items[].metadata.name})
oc exec $POD -- bash -c "mkdir ./data"
oc exec $POD -- bash -c "echo 'azure file storage' >> /data/test.txt"

oc exec $POD -- bash -c "cat /data/test.txt"
azure file storage
```
test.txt dosya Ayrıca Azure portal Depolama Gezgini aracılığıyla da görünür. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Microsoft Azure dosyalarını ve Azure Red Hat OpenShift 4 ' ü kullanarak dinamik kalıcı depolama oluşturdunuz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Depolama Hesabı oluşturma
> * Azure Red Hat OpenShift 4 kümesinde Azure dosyaları hazırlayıcısı 'nı kullanarak bir StorageClass yapılandırma

Azure Red Hat OpenShift 4 desteklenen kaynakları hakkında bilgi edinmek için sonraki makaleye ilerleyin.

* [Azure Red Hat OpenShift destek ilkesi](support-policies-v4.md)
