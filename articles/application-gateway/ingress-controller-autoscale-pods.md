---
title: Azure Uygulama Ağ Geçidi ölçümlerine sahip AKS bölmelerini otomatik ölçeklendir
description: Bu makalede, Uygulama Ağ Geçidi ölçümleri ve Azure Kubernetes Metrik Bağdaştırıcısı kullanarak AKS arka uç bölmelerinizi nasıl ölçeklendirebilirsiniz hakkında yönergeler
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 1169ed0e9a2b970ee0e30d73ea20c87001b62786
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80239443"
---
# <a name="autoscale-your-aks-pods-using-application-gateway-metrics-beta"></a>Uygulama Ağ Geçidi Ölçümlerini Kullanarak AKS bölmelerinizi otomatik ölçeklendirin (Beta)

Gelen trafik arttıkça, başvurularınızı talebe göre ölçeklendirmek çok önemli hale gelir.

Aşağıdaki öğreticide, uygulamanızı ölçeklendirmek için `AvgRequestCountPerHealthyHost` Application Gateway'in metrik ölçümlerini nasıl kullanabileceğinizi açıklıyoruz. `AvgRequestCountPerHealthyHost`belirli bir arka uç havuzuna gönderilen ortalama istekleri ölçer ve arka uç HTTP ayarı birleşimini ölçer.

Aşağıdaki iki bileşeni kullanacağız:

* [`Azure Kubernetes Metric Adapter`](https://github.com/Azure/azure-k8s-metrics-adapter)- Uygulama Ağ Geçidi ölçümlerini metrik sunucu üzerinden ortaya çıkarmak için metrik bağdaştırıcıyı kullanacağız. Azure Kubernetes Metric Adaptörü, Azure altında, Uygulama Ağ Geçidi Giriş Denetleyicisine benzer bir açık kaynak projesidir. 
* [`Horizontal Pod Autoscaler`](https://docs.microsoft.com/azure/aks/concepts-scale#horizontal-pod-autoscaler)- UYGULAMA Ağ Geçidi ölçümlerini kullanmak ve ölçekleme için bir dağıtım hedeflemek için HPA'yı kullanacağız.

## <a name="setting-up-azure-kubernetes-metric-adapter"></a>Azure Kubernetes Metrik Bağdaştırıcısını Ayarlama

1. Önce bir Azure AAD hizmet ilkesi oluşturacağız ve bu `Monitoring Reader` hizmete Uygulama Ağ Geçidi'nin kaynak grubu üzerinden erişeceğiz. 

    ```azurecli
        applicationGatewayGroupName="<application-gateway-group-id>"
        applicationGatewayGroupId=$(az group show -g $applicationGatewayGroupName -o tsv --query "id")
        az ad sp create-for-rbac -n "azure-k8s-metric-adapter-sp" --role "Monitoring Reader" --scopes applicationGatewayGroupId
    ```

1. Şimdi, yukarıda oluşturulan [`Azure Kubernetes Metric Adapter`](https://github.com/Azure/azure-k8s-metrics-adapter) AAD hizmet ilkesini kullanarak dağıtacağız.

    ```bash
    kubectl create namespace custom-metrics
    # use values from service principle created above to create secret
    kubectl create secret generic azure-k8s-metrics-adapter -n custom-metrics \
        --from-literal=azure-tenant-id=<tenantid> \
        --from-literal=azure-client-id=<clientid> \
        --from-literal=azure-client-secret=<secret>
    kubectl apply -f kubectl apply -f https://raw.githubusercontent.com/Azure/azure-k8s-metrics-adapter/master/deploy/adapter.yaml -n custom-metrics
    ```

1. Biz adı `appgw-request-count-metric` `ExternalMetric` olan bir kaynak oluşturacaktır. Bu kaynak, kaynak grubundaki `AvgRequestCountPerHealthyHost` `myResourceGroup` kaynak `myApplicationGateway` için metrik açığa çıkarmak için metrik bağdaştırıcıya talimat verir. `filter` Alanı, Uygulama Ağ Geçidi'nde belirli bir arka uç havuzunu ve arka uç HTTP ayarını hedeflemek için kullanabilirsiniz.

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

Artık yeni metriğimizin açığa çıkarılap açıklanmamasını görmek için metrik sunucuya bir istekte bulunabilirsiniz:
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

## <a name="using-the-new-metric-to-scale-up-the-deployment"></a>Dağıtımı büyütmek için yeni metriğin kullanılması

Metrik sunucu aracılığıyla `appgw-request-count-metric` ortaya çıkarabildiğimizde, hedef [`Horizontal Pod Autoscaler`](https://docs.microsoft.com/azure/aks/concepts-scale#horizontal-pod-autoscaler) dağıtımımızı ölçeklendirmek için kullanıma hazırız.

Aşağıdaki örnekte, örnek bir `aspnet`dağıtımı hedefleyeceğiz. Pod başına 200 `appgw-request-count-metric` > en fazla `10` Pod'a kadar büyüteceğiz.

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

Apache tezgahı gibi bir yük test aracı kullanarak kurulumunuzu test edin:
```bash
ab -n10000 http://<applicaiton-gateway-ip-address>/
```

## <a name="next-steps"></a>Sonraki adımlar
- [**Sorun Giderme Denetleyicisi sorunları**](ingress-controller-troubleshoot.md): Giriş Denetleyicisi ile ilgili sorunları giderin.