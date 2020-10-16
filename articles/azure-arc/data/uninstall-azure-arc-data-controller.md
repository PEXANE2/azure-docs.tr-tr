---
title: Azure Arc veri denetleyicisini silme
description: Azure Arc veri denetleyicisini silme
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: a040200c5746defcaee84a951521d5919c0c4d28
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91660685"
---
# <a name="delete-azure-arc-data-controller"></a>Azure Arc veri denetleyicisini silme

Aşağıdaki makalede bir Azure Arc veri denetleyicisinin nasıl silineceği açıklanmaktadır.

Devam etmeden önce, veri denetleyicisinde oluşturulan tüm veri hizmetlerinin şu şekilde kaldırıldığından emin olun:

## <a name="log-in-to-the-data-controller"></a>Veri denetleyicisinde oturum açma

Silmek istediğiniz veri denetleyicisinde oturum açın:

```
azdata login
```

## <a name="list--delete-existing-data-services"></a>Var olan veri hizmetlerini Listele & Sil

Oluşturulmuş herhangi bir SQL yönetilen örneği olup olmadığını denetlemek için aşağıdaki komutu çalıştırın:

```
azdata arc sql mi list
```

Yukarıdaki listeden her bir SQL yönetilen örneği için, DELETE komutunu aşağıdaki gibi çalıştırın:

```
azdata arc sql mi delete -n <name>
# for example: azdata arc sql mi delete -n sqlinstance1
```

Benzer şekilde, PostgreSQL hiper ölçek örneklerini denetlemek için şunu çalıştırın:

```
azdata arc postgres server list
```

Her PostgreSQL hiper ölçek örneği için, DELETE komutunu aşağıdaki gibi çalıştırın:
```
azdata arc postgres server delete -n <name>
# for example: azdata arc postgres server delete -n pg1
```

## <a name="delete-controller"></a>Denetleyiciyi Sil

Tüm SQL yönetilen örnekleri ve PostgreSQL hiper ölçek örnekleri kaldırıldıktan sonra, veri denetleyicisi aşağıdaki gibi silinebilir:

```
azdata arc dc delete -n <name> -ns <namespace>
# for example: azdata arc dc delete -ns arc -n arcdc
```

### <a name="remove-sccs-red-hat-openshift-only"></a>SCCs 'yi Kaldır (yalnızca Red Hat OpenShift)

```console
oc adm policy remove-scc-from-user privileged -z default -n arc
oc adm policy remove-scc-from-user anyuid     -z default -n arc
```

### <a name="delete-cluster-level-objects"></a>Küme düzeyi nesneleri silme

Ad alanı kapsamlı nesnelere ek olarak, CRDs gibi küme düzeyi nesnelerini de silmek isterseniz `clusterroles` `clusterrolebindings` aşağıdaki komutları çalıştırın:

```console
# Cleanup azure arc data service artifacts
#Delete CRDs
kubectl delete crd datacontrollers.arcdata.microsoft.com 
kubectl delete crd sqlmanagedinstances.sql.arcdata.microsoft.com 
kubectl delete crd postgresql-11s.arcdata.microsoft.com 
kubectl delete crd postgresql-12s.arcdata.microsoft.com
#Delete clusterrole
kubectl delete clusterrole <namespace>:cr-arc-metricsdc-reader
#For example: kubectl delete clusterrole arc:cr-arc-metricsdc-reader
#Delete rolebinding
kubectl delete clusterrolebinding <namespace>:crb-arc-metricsdc-reader
#For example: kubectl delete clusterrolebinding arc:crb-arc-metricsdc-reader
```

### <a name="optionally-delete-the-azure-arc-data-controller-namespace"></a>İsteğe bağlı olarak, Azure Arc veri denetleyicisi ad alanını silme


```console
kubectl delete ns <nameSpecifiedDuringCreation>
# for example: kubectl delete ns arc
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Arc etkin veri Hizmetleri nedir?](overview.md)
