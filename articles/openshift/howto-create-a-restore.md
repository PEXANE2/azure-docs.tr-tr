---
title: Velero kullanarak Azure Red Hat OpenShift 4 küme uygulaması geri yükleme oluşturma
description: Velero kullanarak Azure Red Hat OpenShift küme uygulamalarınızı geri yükleme işlemini nasıl oluşturacağınızı öğrenin
ms.service: container-service
ms.topic: article
ms.date: 06/22/2020
author: troy0820
ms.author: b-trconn
keywords: Aro, OpenShift, az Aro, Red hat, CLI
ms.custom: mvc
ms.openlocfilehash: 9eac34d643ba0df4be79a064858c580c884de727
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92078570"
---
# <a name="create-an-azure-red-hat-openshift-4-cluster-application-restore"></a>Azure Red Hat OpenShift 4 küme uygulaması geri yükleme oluşturma

Bu makalede, ortamınızı bir Azure Red Hat OpenShift 4 küme uygulaması geri yüklemesi oluşturacak şekilde hazırlarsınız. Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Önkoşulları kurun ve gerekli araçları kurun
> * Azure Red Hat OpenShift 4 uygulaması geri yükleme oluşturma

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğreticide, Azure CLı sürüm 2.6.0 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="before-you-begin"></a>Başlamadan önce

### <a name="create-an-azure-red-hat-openshift-4-application-backup"></a>Azure Red Hat OpenShift 4 uygulama yedeklemesi oluşturma

Azure Red Hat OpenShift 4 uygulama yedeklemesi oluşturmak için bkz. [Azure Red Hat OpenShift 4 yedeklemesi oluşturma](./howto-create-a-backup.md)

## <a name="restore-an-azure-red-hat-openshift-4-application"></a>Azure Red Hat OpenShift 4 uygulamasını geri yükleme

Bu adımlar, daha önce Velero ile yedeklenen bir uygulamayı geri yüklemenize olanak tanır.
Geri yükleme için hangi yedeklemelerin kullanılabilir olduğunu görmek için, küme tarafından şu anda tanınan yedeklemelerin listesini kontrol edebilirsiniz.  Bu adımı gerçekleştirmek için aşağıdaki komutu yürütmeniz gerekir:

_(Bu adım, Velero ' i "Velero" adlı bir projeye yüklediğinizi varsayar)_

```bash
oc get backups -n velero
```

Geri yüklemek istediğiniz yedeklemeye sahip olduktan sonra aşağıdaki komutla geri yükleme işlemini gerçekleştirmeniz gerekir:

```bash
velero restore create <name of restore> --from-backup <name of backup from above output list>
```

Bu adım, bir yedekleme oluştururken önceki adımdan yedeklenen Kubernetes nesnelerini oluşturur.

Geri yüklemenin durumunu görmek için aşağıdaki adımları yürütün:

```bash
oc get restore -n velero <name of restore created previously> -o yaml
```
Aşamada `Completed` , Azure Red Hat 4 uygulamanız geri yüklenmelidir.

## <a name="restore-an-azure-red-hat-openshift-4-application-with-included-snapshots"></a>Azure Red Hat OpenShift 4 uygulamasını eklenen anlık görüntülerle geri yükleme


Velero kullanarak kalıcı birimlerde bir Azure Red Hat OpenShift 4 uygulamasının geri yüklemesini oluşturmak için aşağıdaki komutla geri yüklemeyi gerçekleştirmeniz gerekir:

```bash
velero restore create <name of the restore> --from-backup <name of backup from above output list> --exclude-resources="nodes,events,events.events.k8s.io,backups.ark.heptio.com,backups.velero.io,restores.ark.heptio.com,restores.velero.io"
```
Bu adım, bir yedekleme oluştururken önceki adımdan yedeklenen Kubernetes nesnelerini oluşturur.

Geri yüklemenin durumunu görmek için aşağıdaki adımları yürütün:

```bash
oc get restore -n velero <name of restore created previously> -o yaml
```
Aşamada `Completed` , Azure Red Hat 4 uygulamanız geri yüklenmelidir.

Yedekleme oluşturma ve Velero kullanarak geri yükleme hakkında daha fazla bilgi için bkz [. Backup OpenShift Resources in Native Way](https://www.openshift.com/blog/backup-openshift-resources-the-native-way)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir Azure Red Hat OpenShift 4 küme uygulaması geri yüklendi. Şunları öğrendiniz:

> [!div class="checklist"]
> * Velero kullanarak OpenShift v4 küme uygulaması geri yüklemesi oluşturma
> * Velero kullanarak anlık görüntülerle OpenShift v4 küme uygulaması geri yüklemesi oluşturma


Azure Red Hat OpenShift 4 desteklenen kaynakları hakkında bilgi edinmek için sonraki makaleye ilerleyin.

* [Azure Red Hat OpenShift v4 desteklenen kaynaklar](supported-resources.md)


