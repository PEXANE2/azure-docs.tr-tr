---
title: Velero kullanarak Azure Red Hat OpenShift 4 küme uygulama yedeklemesi oluşturma
description: Velero kullanarak Azure Red Hat OpenShift küme uygulamalarınızın nasıl yedeğini oluşturacağınızı öğrenin
ms.service: container-service
ms.topic: article
ms.date: 06/22/2020
author: troy0820
ms.author: b-trconn
keywords: Aro, OpenShift, az Aro, Red hat, CLI
ms.custom: mvc
ms.openlocfilehash: 046cd30c0f93a468287c73573a3d18f4ba66221b
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88690230"
---
# <a name="create-an-azure-red-hat-openshift-4-cluster-application-backup"></a>Azure Red Hat OpenShift 4 küme uygulaması yedeklemesi oluşturma

Bu makalede, ortamınızı bir Azure Red Hat OpenShift 4 küme uygulaması yedeklemesi oluşturacak şekilde hazırlarsınız. Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Önkoşulları kurun ve gerekli araçları kurun
> * Azure Red Hat OpenShift 4 uygulama yedeklemesi oluşturma

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğreticide, Azure CLı sürüm 2.6.0 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="before-you-begin"></a>Başlamadan önce

### <a name="install-velero"></a>Velero 'i yükler

Sisteminize Velero [yüklemek](https://velero.io/docs/main/basic-install/) için işletim sisteminiz için önerilen işlemi izleyin.

### <a name="set-up-azure-storage-account-and-blob-container"></a>Azure depolama hesabı ve BLOB kapsayıcısı ayarlama

Bu adım, ARO kümesinin kaynak grubu dışında bir kaynak grubu oluşturur.  Bu kaynak grubu, yedeklemelerin kalıcı olmasını sağlar ve uygulamaları yeni kümelere geri yükleyebilir.

```bash
AZURE_BACKUP_RESOURCE_GROUP=Velero_Backups
az group create -n $AZURE_BACKUP_RESOURCE_GROUP --location eastus

AZURE_STORAGE_ACCOUNT_ID="velero$(uuidgen | cut -d '-' -f5 | tr '[A-Z]' '[a-z]')"
az storage account create \
    --name $AZURE_STORAGE_ACCOUNT_ID \
    --resource-group $AZURE_BACKUP_RESOURCE_GROUP \
    --sku Standard_GRS \
    --encryption-services blob \
    --https-only true \
    --kind BlobStorage \
    --access-tier Hot

BLOB_CONTAINER=velero
az storage container create -n $BLOB_CONTAINER --public-access off --account-name $AZURE_STORAGE_ACCOUNT_ID
```

## <a name="set-permissions-for-velero"></a>Velero için izinleri ayarlama

### <a name="create-service-principal"></a>Hizmet sorumlusu oluşturma

Velero için yedekleme ve geri yükleme izinleri gerekir. Bir hizmet sorumlusu oluşturduğunuzda, önceki adımda tanımladığınız kaynak grubuna erişmek için Velero izni verirsiniz. Bu adım, kümenin kaynak grubunu alacak:

```bash
export AZURE_RESOURCE_GROUP=aro-$(az aro show --name <name of cluster> --resource-group <name of resource group> | jq -r '.clusterProfile.domain')
```


```bash
AZURE_SUBSCRIPTION_ID=$(az account list --query '[?isDefault].id' -o tsv)

AZURE_TENANT_ID=$(az account list --query '[?isDefault].tenantId' -o tsv)
```

```bash
AZURE_CLIENT_SECRET=$(az ad sp create-for-rbac --name "velero" --role "Contributor" --query 'password' -o tsv \
--scopes  /subscriptions/$AZURE_SUBSCRIPTION_ID)
AZURE_CLIENT_ID=$(az ad sp list --display-name "velero" --query '[0].appId' -o tsv)

```

```bash
cat << EOF  > ./credentials-velero.yaml
AZURE_SUBSCRIPTION_ID=${AZURE_SUBSCRIPTION_ID}
AZURE_TENANT_ID=${AZURE_TENANT_ID}
AZURE_CLIENT_ID=${AZURE_CLIENT_ID}
AZURE_CLIENT_SECRET=${AZURE_CLIENT_SECRET}
AZURE_RESOURCE_GROUP=${AZURE_RESOURCE_GROUP}
AZURE_CLOUD_NAME=AzurePublicCloud
EOF
```

## <a name="install-velero-on-azure-red-hat-openshift-4-cluster"></a>Azure Red Hat OpenShift 4 kümesine Velero 'i yükler

Bu adım, Velero uygulamasını kendi projesine ve yedeklemeleri yapmak için gereken [özel kaynak tanımlarına](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/) ve Velero ile geri yükler. Azure Red Hat OpenShift v4 kümesinde başarıyla oturum açtığınızdan emin olun.


```bash
velero install \
--provider azure \
--plugins velero/velero-plugin-for-microsoft-azure:v1.1.0 \
--bucket $BLOB_CONTAINER \
--secret-file ~/path/to/credentials-velero.yaml \
--backup-location-config resourceGroup=$AZURE_BACKUP_RESOURCE_GROUP,storageAccount=$AZURE_STORAGE_ACCOUNT_ID \
--snapshot-location-config apiTimeout=15m \
--velero-pod-cpu-limit="0" --velero-pod-mem-limit="0" \
--velero-pod-mem-request="0" --velero-pod-cpu-request="0"
```

## <a name="create-a-backup-with-velero"></a>Velero ile yedek oluşturma

Velero ile bir uygulama yedeklemesi oluşturmak için, bu uygulamanın bulunduğu ad alanını dahil etmeniz gerekir.  Bir `nginx-example` ad alanınız varsa ve bu ad alanındaki tüm kaynakları yedeklemeye eklemek istiyorsanız terminalde aşağıdaki komutu çalıştırın:

```bash
velero create backup <name of backup> --include-namespaces=nginx-example
```
Şunu çalıştırarak yedeklemenin durumunu kontrol edebilirsiniz:

```bash
oc get backups -n velero <name of backup> -o yaml
```

Başarılı bir yedekleme çıkış olur `phase:Completed` ve nesneler depolama hesabındaki kapsayıcıda etkin olur.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir Azure Red Hat OpenShift 4 küme uygulaması yedeklendi. Şunları öğrendiniz:

> [!div class="checklist"]
> * Velero kullanarak OpenShift v4 kümesi uygulama yedeklemesi oluşturma


Bir Azure Red Hat OpenShift 4 küme uygulaması geri yüklemesi oluşturmayı öğrenmek için bir sonraki makaleye ilerleyin.

* [Azure Red Hat OpenShift 4 küme uygulaması geri yükleme oluşturma](howto-create-a-restore.md)
