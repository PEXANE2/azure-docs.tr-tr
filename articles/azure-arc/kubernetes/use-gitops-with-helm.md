---
title: Yay etkin Kubernetes kümesinde Gilar kullanarak Held grafikleri dağıtma (Önizleme)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Azure Arc etkin küme yapılandırması (Önizleme) için Held ile Gilar kullanma
keywords: Gilar, Kubernetes, K8s, Azure, Held, Arc, AKS, Azure Kubernetes hizmeti, kapsayıcılar
ms.openlocfilehash: eea81d458ac6631c4a023134b3198e4cdb04526e
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91541620"
---
# <a name="deploy-helm-charts-using-gitops-on-arc-enabled-kubernetes-cluster-preview"></a>Yay etkin Kubernetes kümesinde Gilar kullanarak Held grafikleri dağıtma (Önizleme)

Helk, Kubernetes uygulamalarının yaşam döngüsünü yüklemenize ve yönetmenize yardımcı olan bir açık kaynaklı paketleme aracıdır. APT ve yum gibi Linux paket yöneticilerine benzer şekilde, Helm, önceden yapılandırılmış Kubernetes kaynakları paketleri olan Kubernetes grafiklerini yönetmek için kullanılır.

Bu makalede, Azure Arc etkinleştirilmiş Kubernetes ile Held 'yi yapılandırma ve kullanma işlemlerinin nasıl yapılacağı gösterilir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir Azure Arc etkin Kubernetes bağlı kümesine sahip olduğunuzu varsaymaktadır. Bağlı bir kümeye ihtiyacınız varsa, [küme bağlama hızlı başlangıç](./connect-cluster.md)bölümüne bakın.

## <a name="overview-of-using-gitops-and-helm-with-azure-arc-enabled-kubernetes"></a>Azure Arc etkin Kubernetes ile Gira ve Held kullanımına genel bakış

 Hele işleci, Held grafik sürümlerini otomatikleştiren bir Flox uzantısı sağlar. Grafik sürümü, HelmRelease adlı bir Kubernetes özel kaynağı aracılığıyla açıklanır. Flox bu kaynakları git 'ten kümeye eşitler ve Held operatörü kaynaklarda belirtilen şekilde Helu grafiklerinin serbest bırakıldığını sağlar.

 Bu belgede kullanılan [örnek depo](https://github.com/Azure/arc-helm-demo) aşağıdaki şekilde yapılandırılmıştır:

```console
├── charts
│   └── azure-arc-sample
│       ├── Chart.yaml
│       ├── templates
│       │   ├── NOTES.txt
│       │   ├── deployment.yaml
│       │   └── service.yaml
│       └── values.yaml
└── releases
    └── app.yaml
```

Git deposunda, biri helk grafiği ve biri de yayınlar yapılandırmasını içeren iki diziniz var. Dizininde, `releases` `app.yaml` aşağıda gösterilen HelmRelease yapılandırmasını içerir:

```yaml
apiVersion: helm.fluxcd.io/v1
kind: HelmRelease
metadata:
  name: azure-arc-sample
  namespace: arc-k8s-demo
spec:
  releaseName: arc-k8s-demo
  chart:
    git: https://github.com/Azure/arc-helm-demo
    ref: master
    path: charts/azure-arc-sample
  values:
    serviceName: arc-k8s-demo
```

HELI yayın yapılandırması aşağıdaki alanları içerir:

- `metadata.name` zorunludur ve Kubernetes adlandırma kurallarını izlemesi gerekiyor
- `metadata.namespace` isteğe bağlıdır ve yayın oluşturulduğunu belirler
- `spec.releaseName` isteğe bağlıdır ve sağlanmazsa, sürüm adı $namespace $name
- `spec.chart.path` , depo köküne göre belirtilen grafiği içeren dizindir
- `spec.values` Varsayılan parametre değerlerinin grafik kendisinden olan Kullanıcı özelleştirmelerdir

HelmRelease spec. Values içinde belirtilen seçenekler, grafik kaynağından. YAML değerleri içinde belirtilen seçenekleri geçersiz kılar.

Resmi [Heli operatörü belgelerinde](https://docs.fluxcd.io/projects/helm-operator/en/stable/) HelmRelease hakkında daha fazla bilgi edinebilirsiniz

## <a name="create-a-configuration"></a>Yapılandırma oluşturma

İçin Azure CLı uzantısını kullanarak `k8sconfiguration` , bağlantılı kümemizi örnek git deposuna bağlayalim. Bu yapılandırmaya bir ad vereceğiz `azure-arc-sample` ve Flox işlecini `arc-k8s-demo` ad alanına dağıtacağız.

```console
az k8sconfiguration create --name azure-arc-sample --cluster-name AzureArcTest1 --resource-group AzureArcTest --operator-instance-name flux --operator-namespace arc-k8s-demo --operator-params='--git-readonly --git-path=releases' --enable-helm-operator --helm-operator-version='0.6.0' --helm-operator-params='--set helm.versions=v3' --repository-url https://github.com/Azure/arc-helm-demo.git --scope namespace --cluster-type connectedClusters
```

### <a name="configuration-parameters"></a>Yapılandırma parametreleri

Yapılandırma oluşturmayı özelleştirmek için [kullanabileceğiniz ek parametreler hakkında bilgi edinin](./use-gitops-connected-cluster.md#additional-parameters).

## <a name="validate-the-configuration"></a>Yapılandırmayı doğrulama

Azure CLı 'yı kullanarak `sourceControlConfiguration` başarıyla oluşturulduğunu doğrulayın.

```console
az k8sconfiguration show --name azure-arc-sample --cluster-name AzureArcTest1 --resource-group AzureArcTest --cluster-type connectedClusters
```

`sourceControlConfiguration`Kaynak, uyumluluk durumu, iletiler ve hata ayıklama bilgileriyle güncelleştirilir.

**Çıktıların**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Installed",
    "lastConfigApplied": "2019-12-05T05:34:41.481000",
    "message": "{\"OperatorMessage\":null,\"ClusterState\":null}",
    "messageLevel": "3"
  },
  "enableHelmOperator": "True",
  "helmOperatorProperties": {
    "chartValues": "--set helm.versions=v3",
    "chartVersion": "0.6.0"
  },
  "id": "/subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/azure-arc-sample",
  "name": "azure-arc-sample",
  "operatorInstanceName": "flux",
  "operatorNamespace": "arc-k8s-demo",
  "operatorParams": "--git-readonly --git-path=releases",
  "operatorScope": "namespace",
  "operatorType": "Flux",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": "",
  "repositoryUrl": "https://github.com/Azure/arc-helm-demo.git",
  "resourceGroup": "AzureArcTest",
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
}
```

## <a name="validate-application"></a>Uygulamayı doğrula

Aşağıdaki komutu çalıştırın ve `localhost:8080` uygulamanın çalıştığını doğrulamak için tarayıcınızda öğesine gidin.

```console
kubectl port-forward -n arc-k8s-demo svc/arc-k8s-demo 8080:8080
```

## <a name="next-steps"></a>Sonraki adımlar

- [Küme yapılandırmasını yönetmek için Azure Ilkesini kullanma](./use-azure-policy.md)
