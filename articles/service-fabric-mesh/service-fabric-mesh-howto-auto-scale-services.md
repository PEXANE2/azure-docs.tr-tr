---
title: Azure Service Fabric ağı 'nda çalışan bir uygulamayı otomatik ölçeklendirme
description: Service Fabric bir kafes uygulamasının Hizmetleri için otomatik ölçek ilkelerini nasıl yapılandıracağınızı öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: fb72806dd7ba838ba7170bda409715bc074e1d99
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75461983"
---
# <a name="create-autoscale-policies-for-a-service-fabric-mesh-application"></a>Service Fabric kafes uygulaması için otomatik ölçeklendirme ilkeleri oluşturma
Service Fabric kafese uygulama dağıtmanın başlıca avantajlarından biri, hizmetlerinizi kolayca ölçeklendirebilme olanağı sağlar. Bu, hizmetinizdeki farklı yük miktarını işlemek veya kullanılabilirliği iyileştirmek için kullanılmalıdır. Hizmetlerinizi el ile ölçeklendirebilir veya otomatik ölçeklendirme ilkeleri ayarlayabilirsiniz.

[Otomatik ölçeklendirme](service-fabric-mesh-scalability.md#autoscaling-service-instances) , hizmet örneklerinizin sayısını (yatay ölçeklendirme) dinamik olarak ölçeklendirmenize olanak tanır. Otomatik ölçeklendirme, büyük ölçüde esneklik sağlar ve CPU veya bellek kullanımına göre hizmet örneklerinin sağlanması veya kaldırılmasına olanak sağlar.

## <a name="options-for-creating-an-auto-scaling-policy-trigger-and-mechanism"></a>Otomatik ölçeklendirme ilkesi, tetikleyici ve mekanizması oluşturma seçenekleri
Otomatik ölçeklendirme ilkesi, ölçeklendirmek istediğiniz her hizmet için tanımlanır. İlke, YAML hizmeti kaynak dosyasında ya da JSON dağıtım şablonunda tanımlanır. Her ölçeklendirme ilkesi iki bölümden oluşur: bir tetikleyici ve ölçekleme mekanizması.

Tetikleyici, bir otomatik ölçeklendirme ilkesi çağrıldığında tanımlar.  Tetikleyici türünü (Ortalama Yük) ve izlenecek ölçüyü (CPU veya bellek) belirtin.  Yüzde olarak belirtilen üst ve alt yükleme eşikleri. Ölçek aralığı, şu anda dağıtılmış olan tüm hizmet örneklerinde belirtilen kullanımı (saniye cinsinden) (ortalama CPU yükü gibi) ne sıklıkta denetleyebileceğini tanımlar.  Bu mekanizma, izlenen ölçüm alt eşiğin altına düştüğünde veya üst eşiğin üstünde arttığında tetiklenir.  

Ölçeklendirme mekanizması, ilke tetiklendiğinde ölçeklendirme işleminin nasıl gerçekleştirileceğini tanımlar.  En düşük ve en fazla çoğaltma sayısını (tamsayı olarak) (örneğin, çoğaltma Ekle/Kaldır) belirtin.  Hizmet çoğaltmaları sayısı, hiçbir şekilde en düşük sayının altına veya en yüksek sayının altına ölçeklenmez.  Ayrıca ölçek artışını bir tamsayı olarak belirtin, bu da bir ölçeklendirme işleminde eklenecek veya kaldırılacak çoğaltmaların sayısıdır.  

## <a name="define-an-auto-scaling-policy-in-a-json-template"></a>JSON şablonunda otomatik ölçeklendirme ilkesi tanımlama

Aşağıdaki örnek, bir JSON dağıtım şablonundaki bir otomatik ölçeklendirme ilkesini gösterir.  Otomatik ölçeklendirme ilkesi, bir hizmetin ölçeklenebileceği bir özelliği içinde bildirilmiştir.  Bu örnekte, bir CPU Ortalama Yük tetikleyicisi tanımlanmıştır.  Dağıtılan tüm örneklerin ortalama CPU yükü 0,2 altına düşerse mekanizma tetiklenir (%20) veya 0,8 (80%) üzerine gider.  CPU yükü her 60 saniyede bir denetlenir.  Ölçek mekanizması, ilke tetikleniyorsa örnek eklemek veya kaldırmak için tanımlanır.  Hizmet örnekleri, bir tane artışlarına eklenecek veya kaldırılacak.  Minimum örnek sayısı bir ve 40 en büyük örnek sayısı da tanımlanmıştır.

```json
{
"apiVersion": "2018-09-01-preview",
"name": "WorkerApp",
"type": "Microsoft.ServiceFabricMesh/applications",
"location": "[parameters('location')]",
"dependsOn": [
"Microsoft.ServiceFabricMesh/networks/worker-app-network"
],
"properties": {
"services": [   
    { ... },       
    {
    "name": "WorkerService",
    "properties": {
        "description": "Worker Service",
        "osType": "linux",
        "codePackages": [
        {  ...              }
        ],
        "replicaCount": 1,
        "autoScalingPolicies": [
        {
            "name": "AutoScaleWorkerRule",
            "trigger": {
                "kind": "AverageLoad",
                "metric": {
                    "kind": "Resource",
                    "name": "cpu"
                },
                "lowerLoadThreshold": "0.2",
                "upperLoadThreshold": "0.8",
                "scaleIntervalInSeconds": "60"
            },
            "mechanism": {
                "kind": "AddRemoveReplica",
                "minCount": "1",
                "maxCount": "40",
                "scaleIncrement": "1"
            }
        }
        ],        
        ...
    }
    }
]
}
}
```

## <a name="define-an-autoscale-policy-in-a-serviceyaml-resource-file"></a>Bir Service. YAML kaynak dosyasında otomatik ölçeklendirme ilkesi tanımlama
Aşağıdaki örnekte, bir hizmet kaynağı (YAML) dosyasındaki bir otomatik ölçeklendirme ilkesi gösterilmektedir.  Otomatik ölçeklendirme ilkesi, ölçeklendirilebilecek hizmetin bir özelliği olarak bildirilmiştir.  Bu örnekte, bir CPU Ortalama Yük tetikleyicisi tanımlanmıştır.  Dağıtılan tüm örneklerin ortalama CPU yükü 0,2 altına düşerse mekanizma tetiklenir (%20) veya 0,8 (80%) üzerine gider.  CPU yükü her 60 saniyede bir denetlenir.  Ölçek mekanizması, ilke tetikleniyorsa örnek eklemek veya kaldırmak için tanımlanır.  Hizmet örnekleri, bir tane artışlarına eklenecek veya kaldırılacak.  Minimum örnek sayısı bir ve 40 en büyük örnek sayısı da tanımlanmıştır.

```yaml
## Service definition ##
application:
  schemaVersion: 1.0.0-preview2
  name: WorkerApp
  properties:
    services:
      - name: WorkerService
        properties:
          description: WorkerService description.
          osType: Linux
          codePackages:
            ...
          replicaCount: 1
          autoScalingPolicies:
            - name: AutoScaleWorkerRule
              trigger:
                kind: AverageLoad
                metric:
                  kind: Resource
                  name: cpu
                lowerLoadThreshold: 0.2
                upperLoadThreshold: 0.8
                scaleIntervalInSeconds: 60
              mechanism:
                kind: AddRemoveReplica
                minCount: 1
                maxCount: 40
                scaleIncrement: 1
          ...
```

## <a name="next-steps"></a>Sonraki adımlar
[Bir hizmeti el ile ölçeklendirmeyi](service-fabric-mesh-tutorial-template-scale-services.md) öğrenin
