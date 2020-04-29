---
title: Azure Application Gateway ölçümlerini kullanarak AKS 'leri otomatik ölçeklendirme
description: Bu makale, Application Gateway ölçümleri ve Azure Kubernetes ölçüm bağdaştırıcısı kullanarak AKS arka uç yığınlarınızı ölçeklendirmeye yönelik yönergeler sağlar
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 1169ed0e9a2b970ee0e30d73ea20c87001b62786
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80239443"
---
# <a name="autoscale-your-aks-pods-using-application-gateway-metrics-beta"></a>Application Gateway ölçümlerini kullanarak AKS yığınlarınızı otomatik ölçeklendirme (Beta)

Gelen trafik arttıkça, talebe göre uygulamalarınızın ölçeğini ölçeklendirmek çok önemli hale gelir.

Aşağıdaki öğreticide, uygulamanızı ölçeklendirmek için Application Gateway `AvgRequestCountPerHealthyHost` ölçüsünü nasıl kullanabileceğinizi açıkladık. `AvgRequestCountPerHealthyHost`belirli bir arka uç havuzuna ve arka uç HTTP ayarı birleşimine gönderilen ortalama isteği ölçer.

Aşağıdaki iki bileşeni kullanacağız:

* [`Azure Kubernetes Metric Adapter`](https://github.com/Azure/azure-k8s-metrics-adapter)-Ölçüm sunucusu üzerinden Application Gateway ölçümleri göstermek için ölçüm bağdaştırıcısını kullanacağız. Azure Kubernetes ölçüm bağdaştırıcısı, Application Gateway giriş denetleyicisine benzer şekilde Azure 'daki açık kaynaklı bir projem. 
* [`Horizontal Pod Autoscaler`](https://docs.microsoft.com/azure/aks/concepts-scale#horizontal-pod-autoscaler)-Application Gateway ölçümleri kullanmak ve ölçekleme için bir dağıtımı hedeflemek üzere HPA kullanacağız.

## <a name="setting-up-azure-kubernetes-metric-adapter"></a>Azure Kubernetes ölçüm bağdaştırıcısını ayarlama

1. İlk olarak bir Azure AAD hizmet sorumlusu oluşturacak ve Application Gateway kaynak grubuna `Monitoring Reader` erişim atayacağız. 

    ```azurecli
        applicationGatewayGroupName="<application-gateway-group-id>"
        applicationGatewayGroupId=$(az group show -g $applicationGatewayGroupName -o tsv --query "id")
        az ad sp create-for-rbac -n "azure-k8s-metric-adapter-sp" --role "Monitoring Reader" --scopes applicationGatewayGroupId
    ```

1. Şimdi, yukarıda oluşturulan AAD hizmet [`Azure Kubernetes Metric Adapter`](https://github.com/Azure/azure-k8s-metrics-adapter) sorumlusunu kullanarak dağıtırsınız.

    ```bash
    kubectl create namespace custom-metrics
    # use values from service principle created above to create secret
    kubectl create secret generic azure-k8s-metrics-adapter -n custom-metrics \
        --from-literal=azure-tenant-id=<tenantid> \
        --from-literal=azure-client-id=<clientid> \
        --from-literal=azure-client-secret=<secret>
    kubectl apply -f kubectl apply -f https://raw.githubusercontent.com/Azure/azure-k8s-metrics-adapter/master/deploy/adapter.yaml -n custom-metrics
    ```

1. Adında `ExternalMetric` `appgw-request-count-metric`bir kaynak oluşturacağız. Bu kaynak, ölçüm bağdaştırıcısına kaynak grubundaki kaynak `AvgRequestCountPerHealthyHost` `myApplicationGateway` `myResourceGroup` için ölçümü açığa çıkarmak için talimat verecektir. Alanı, `filter` Application Gateway belirli bir arka uç havuzunu ve arka uç http ayarını hedeflemek için kullanabilirsiniz.

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

Ölçüm sunucusu üzerinden kullanıma sunabilebilmemiz `appgw-request-count-metric` için, hedef dağıtımımızın ölçeğini ölçeklendirmek [`Horizontal Pod Autoscaler`](https://docs.microsoft.com/azure/aks/concepts-scale#horizontal-pod-autoscaler) üzere kullanıma hazırız.

Aşağıdaki örnekte, örnek bir dağıtım `aspnet`hedefliyoruz. En fazla sayıda `appgw-request-count-metric` `10` Pod 'a kadar > 200 ' ü yukarı kadar ölçeklendireceğiz.

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