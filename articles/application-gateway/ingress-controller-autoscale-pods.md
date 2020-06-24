---
title: Azure Application Gateway ölçümlerini kullanarak AKS 'leri otomatik ölçeklendirme
description: Bu makale, Application Gateway ölçümleri ve Azure Kubernetes ölçüm bağdaştırıcısı kullanarak AKS arka uç yığınlarınızı ölçeklendirmeye yönelik yönergeler sağlar
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 5e0533a44db269229b2f26fa8d2f2b4f84f4d0b4
ms.sourcegitcommit: 398fecceba133d90aa8f6f1f2af58899f613d1e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2020
ms.locfileid: "85125472"
---
# <a name="autoscale-your-aks-pods-using-application-gateway-metrics-beta"></a>Application Gateway ölçümlerini kullanarak AKS yığınlarınızı otomatik ölçeklendirme (Beta)

Gelen trafik arttıkça, talebe göre uygulamalarınızın ölçeğini ölçeklendirmek çok önemli hale gelir.

Aşağıdaki öğreticide, uygulamanızı ölçeklendirmek için Application Gateway ölçüsünü nasıl kullanabileceğinizi açıkladık `AvgRequestCountPerHealthyHost` . `AvgRequestCountPerHealthyHost`belirli bir arka uç havuzuna ve arka uç HTTP ayarı birleşimine gönderilen ortalama isteği ölçer.

Aşağıdaki iki bileşeni kullanacağız:

* [`Azure Kubernetes Metric Adapter`](https://github.com/Azure/azure-k8s-metrics-adapter)-Ölçüm sunucusu üzerinden Application Gateway ölçümleri göstermek için ölçüm bağdaştırıcısını kullanacağız. Azure Kubernetes ölçüm bağdaştırıcısı, Application Gateway giriş denetleyicisine benzer şekilde Azure 'daki açık kaynaklı bir projem. 
* [`Horizontal Pod Autoscaler`](https://docs.microsoft.com/azure/aks/concepts-scale#horizontal-pod-autoscaler)-Application Gateway ölçümleri kullanmak ve ölçekleme için bir dağıtımı hedeflemek üzere HPA kullanacağız.

## <a name="setting-up-azure-kubernetes-metric-adapter"></a>Azure Kubernetes ölçüm bağdaştırıcısını ayarlama

1. İlk olarak bir Azure AAD hizmet sorumlusu oluşturacak ve `Monitoring Reader` Application Gateway kaynak grubuna erişim atayacağız. 

    ```azurecli
        applicationGatewayGroupName="<application-gateway-group-id>"
        applicationGatewayGroupId=$(az group show -g $applicationGatewayGroupName -o tsv --query "id")
        az ad sp create-for-rbac -n "azure-k8s-metric-adapter-sp" --role "Monitoring Reader" --scopes applicationGatewayGroupId
    ```

1. Şimdi, [`Azure Kubernetes Metric Adapter`](https://github.com/Azure/azure-k8s-metrics-adapter) yukarıda oluşturulan AAD hizmet sorumlusunu kullanarak dağıtırsınız.

    ```bash
    kubectl create namespace custom-metrics
    # use values from service principal created above to create secret
    kubectl create secret generic azure-k8s-metrics-adapter -n custom-metrics \
        --from-literal=azure-tenant-id=<tenantid> \
        --from-literal=azure-client-id=<clientid> \
        --from-literal=azure-client-secret=<secret>
    kubectl apply -f kubectl apply -f https://raw.githubusercontent.com/Azure/azure-k8s-metrics-adapter/master/deploy/adapter.yaml -n custom-metrics
    ```

1. Adında bir kaynak oluşturacağız `ExternalMetric` `appgw-request-count-metric` . Bu kaynak, ölçüm bağdaştırıcısına kaynak `AvgRequestCountPerHealthyHost` grubundaki kaynak için ölçümü açığa çıkarmak için talimat verecektir `myApplicationGateway` `myResourceGroup` . `filter`Alanı, Application Gateway belirli bir arka uç havuzunu ve arka uç http ayarını hedeflemek için kullanabilirsiniz.

    ```yaml
    apiVersion: azure.com/v1alpha2
    kind: ExternalMetric
    metadata:
    name: appgw-request-count-metric
    spec:
        type: azuremonitor
        azure:
            resourceGroup: myResourceGroup # replace with your application gateway's resource group name
            resourceName: myApplicationGateway # replace with your application gateway's name
            resourceProviderNamespace: Microsoft.Network
            resourceType: applicationGateways
        metric:
            metricName: AvgRequestCountPerHealthyHost
            aggregation: Average
            filter: BackendSettingsPool eq '<backend-pool-name>~<backend-http-setting-name>' # optional
    ```

Artık yeni ölçümünüzün sunulduğunu görmek için ölçüm sunucusuna bir istek yapabilirsiniz:
```bash
kubectl get --raw "/apis/external.metrics.k8s.io/v1beta1/namespaces/default/appgw-request-count-metric"
# Sample Output
# {
#   "kind": "ExternalMetricValueList",
#   "apiVersion": "external.metrics.k8s.io/v1beta1",
#   "metadata":
#     {
#       "selfLink": "/apis/external.metrics.k8s.io/v1beta1/namespaces/default/appgw-request-count-metric",
#     },
#   "items":
#     [
#       {
#         "metricName": "appgw-request-count-metric",
#         "metricLabels": null,
#         "timestamp": "2019-11-05T00:18:51Z",
#         "value": "30",
#       },
#     ],
# }
```

## <a name="using-the-new-metric-to-scale-up-the-deployment"></a>Dağıtımı ölçeklendirmek için yeni ölçümü kullanma

Ölçüm sunucusu üzerinden kullanıma sunabilebilmemiz için `appgw-request-count-metric` , [`Horizontal Pod Autoscaler`](https://docs.microsoft.com/azure/aks/concepts-scale#horizontal-pod-autoscaler) hedef dağıtımımızın ölçeğini ölçeklendirmek üzere kullanıma hazırız.

Aşağıdaki örnekte, örnek bir dağıtım hedefliyoruz `aspnet` . `appgw-request-count-metric`En fazla sayıda Pod 'a kadar > 200 ' ü yukarı kadar ölçeklendireceğiz `10` .

Hedef dağıtım adınızı değiştirin ve aşağıdaki otomatik ölçek yapılandırmasını uygulayın:
```yaml
apiVersion: autoscaling/v2beta1
kind: HorizontalPodAutoscaler
metadata:
  name: deployment-scaler
spec:
  scaleTargetRef:
    apiVersion: extensions/v1beta1
    kind: Deployment
    name: aspnet # replace with your deployment's name
  minReplicas: 1
  maxReplicas: 10
  metrics:
  - type: External
    external:
      metricName: appgw-request-count-metric
      targetAverageValue: 200
```

Apache tezgahtır gibi bir yük testi aracı kullanarak kurulumunuzu test edin:
```bash
ab -n10000 http://<applicaiton-gateway-ip-address>/
```

## <a name="next-steps"></a>Sonraki adımlar
- Giriş [**denetleyicisi sorunlarını giderme**](ingress-controller-troubleshoot.md): giriş denetleyicisiyle ilgili sorunları giderin.