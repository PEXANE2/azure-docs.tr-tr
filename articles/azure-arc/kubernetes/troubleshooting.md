---
title: Yaygın Azure Arc etkin Kubernetes sorunlarını giderme (Önizleme)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Arc etkin Kubernetes kümeleriyle ilgili yaygın sorunları giderme.
keywords: Kubernetes, yay, Azure, kapsayıcılar
ms.openlocfilehash: 1527f8d4ca06c2deaf4ce18b73bfdb515dcadc63
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83725593"
---
# <a name="azure-arc-enabled-kubernetes-troubleshooting-preview"></a>Azure Arc etkin Kubernetes sorunlarını giderme (Önizleme)

Bu belgede bağlantı, izinler ve aracılar ile ilgili bazı genel sorun giderme senaryoları sağlanmaktadır.

## <a name="general-troubleshooting"></a>Genel sorun giderme

### <a name="azure-cli-set-up"></a>Azure CLı kurulumu
Az connectedk8s veya az k8sconfiguration CLı komutlarını kullanmadan önce, az önce doğru Azure aboneliğine göre çalışacak şekilde ayarlandığından emin olmanız gerekir.

```console
az account set --subscription 'subscriptionId'
az account show
```

### <a name="azure-arc-agents"></a>Azure-yay aracıları
Azure Arc etkin Kubernetes için tüm aracılar ad alanında Pod olarak dağıtılır `azure-arc` . Normal işlemler altında tüm yığınların çalışıyor olması ve sistem durumu denetimlerinin geçirilmesi gerekir.

İlk olarak, Azure Arc helb sürümünü doğrulayın:

```console
$ helm --namespace default status azure-arc
NAME: azure-arc
LAST DEPLOYED: Fri Apr  3 11:13:10 2020
NAMESPACE: default
STATUS: deployed
REVISION: 5
TEST SUITE: None
```

Held sürümü bulunamazsa veya eksikse kümeyi yeniden eklemeyi deneyin.

Helb sürümü varsa ve `STATUS: deployed` aracı durumunu şunu kullanarak tespit ederseniz `kubectl` :

```console
$ kubectl -n azure-arc get deployments,pods
NAME                                        READY   UP-TO-DATE AVAILABLE AGE
deployment.apps/cluster-metadata-operator   1/1     1           1        16h
deployment.apps/clusteridentityoperator     1/1     1           1        16h
deployment.apps/config-agent                1/1     1           1        16h
deployment.apps/controller-manager          1/1     1           1        16h
deployment.apps/flux-logs-agent             1/1     1           1        16h
deployment.apps/metrics-agent               1/1     1           1        16h
deployment.apps/resource-sync-agent         1/1     1           1        16h

NAME                                            READY   STATUS   RESTART AGE
pod/cluster-metadata-operator-7fb54d9986-g785b  2/2     Running  0       16h
pod/clusteridentityoperator-6d6678ffd4-tx8hr    3/3     Running  0       16h
pod/config-agent-544c4669f9-4th92               3/3     Running  0       16h
pod/controller-manager-fddf5c766-ftd96          3/3     Running  0       16h
pod/flux-logs-agent-7c489f57f4-mwqqv            2/2     Running  0       16h
pod/metrics-agent-58b765c8db-n5l7k              2/2     Running  0       16h
pod/resource-sync-agent-5cf85976c7-522p5        3/3     Running  0       16h
```

Tüm pods 'Ler `STATUS` olarak göstermelidir `Running` ve ya `READY` da olmalıdır `3/3` `2/2` . Günlükleri getirin ve döndürülen veya döndüren Pod 'leri `Error` tanıtın `CrashLoopBackOff` .

## <a name="unable-to-connect-my-kubernetes-cluster-to-azure"></a>Kubernetes kümeme Azure 'a bağlanamıyor

Kümelerin Azure 'a bağlanması için hem Azure aboneliğine hem de `cluster-admin` hedef kümeye erişime sahip olmanız gerekir. Kümeye ulaşılamadığından veya izinleri yetersizse, ekleme başarısız olur.

### <a name="insufficient-cluster-permissions"></a>Küme izinleri yetersiz

Belirtilen kubeconfig dosyası, Azure Arc aracılarını yüklemek için yeterli izinlere sahip değilse, Azure CLı komutu Kubernetes API 'sini çağırmaya çalışırken bir hata döndürür.

```console
$ az connectedk8s connect --resource-group AzureArc --name AzureArcCluster
Command group 'connectedk8s' is in preview. It may be changed/removed in a future release.
Ensure that you have the latest helm version installed before proceeding to avoid unexpected errors.
This operation might take a while...

Error: list: failed to list: secrets is forbidden: User "myuser" cannot list resource "secrets" in API group "" at the cluster scope
```

Küme sahibi, Küme Yöneticisi izinlerine sahip bir Kubernetes kullanıcısı kullanmalıdır.

### <a name="installation-timeouts"></a>Yükleme zaman aşımları

Azure Arc aracı yüklemesi için hedef kümede bir kapsayıcı kümesi çalışıyor olmalıdır. Küme yavaş bir internet bağlantısı üzerinden çalışıyorsa kapsayıcı görüntüsü çekme işlemi Azure CLı zaman aşımlarından daha uzun sürebilir.

```console
$ az connectedk8s connect --resource-group AzureArc --name AzureArcCluster
Command group 'connectedk8s' is in preview. It may be changed/removed in a future release.
Ensure that you have the latest helm version installed before proceeding to avoid unexpected errors.
This operation might take a while...

There was a problem with connect-agent deployment. Please run 'kubectl -n azure-arc logs -l app.kubernetes.io/component=connect-agent -c connect-agent' to debug the error.
```

## <a name="configuration-management"></a>Yapılandırma yönetimi

### <a name="general"></a>Genel
Kaynak denetimi yapılandırmasıyla ilgili sorunları gidermeye yardımcı olmak için,--hata ayıklama anahtarıyla az Commands komutunu çalıştırın.

```console
az provider show -n Microsoft.KubernetesConfiguration --debug
az k8sconfiguration create <parameters> --debug
```

### <a name="create-source-control-configuration"></a>Kaynak denetimi yapılandırması oluştur
Microsoft. Kubernetes/connectedCluster kaynağında katkıda bulunan rolü, Microsoft. KubernetesConfiguration/sourceControlConfiguration kaynağı oluşturmak için gereklidir ve yeterlidir.

### <a name="configuration-remains-pending"></a>Yapılandırma kalır`Pending`

```console
kubectl -n azure-arc logs -l app.kubernetes.io/component=config-agent -c config-agent
$ k -n pending get gitconfigs.clusterconfig.azure.com  -o yaml
apiVersion: v1
items:
- apiVersion: clusterconfig.azure.com/v1beta1
  kind: GitConfig
  metadata:
    creationTimestamp: "2020-04-13T20:37:25Z"
    generation: 1
    name: pending
    namespace: pending
    resourceVersion: "10088301"
    selfLink: /apis/clusterconfig.azure.com/v1beta1/namespaces/pending/gitconfigs/pending
    uid: d9452407-ff53-4c02-9b5a-51d55e62f704
  spec:
    correlationId: ""
    deleteOperator: false
    enableHelmOperator: false
    giturl: git@github.com:slack/cluster-config.git
    helmOperatorProperties: null
    operatorClientLocation: azurearcfork8s.azurecr.io/arc-preview/fluxctl:0.1.3
    operatorInstanceName: pending
    operatorParams: '"--disable-registry-scanning"'
    operatorScope: cluster
    operatorType: flux
  status:
    configAppliedTime: "2020-04-13T20:38:43.081Z"
    isSyncedWithAzure: true
    lastPolledStatusTime: ""
    message: 'Error: {exit status 1} occurred while doing the operation : {Installing
      the operator} on the config'
    operatorPropertiesHashed: ""
    publicKey: ""
    retryCountPublicKey: 0
    status: Installing the operator
kind: List
metadata:
  resourceVersion: ""
  selfLink: ""
```
