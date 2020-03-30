---
title: Azure Hizmet Kumaş Kafesi'nde çalışan bir uygulamayı otomatik ölçeklendirme
description: Service Fabric Mesh uygulamasının hizmetleri için otomatik ölçekilkelerini nasıl yapılandırabileceğinizi öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: fb72806dd7ba838ba7170bda409715bc074e1d99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75461983"
---
# <a name="create-autoscale-policies-for-a-service-fabric-mesh-application"></a>Service Fabric Mesh uygulaması için otomatik ölçeklendirme ilkeleri oluşturma
Uygulamaları Service Fabric Mesh'e dağıtmanın en önemli avantajlarından biri, hizmetlerinizi kolayca ölçeklendirebilme veya dışarı edebilme yeteneğidir. Bu, hizmetlerinizdeki çeşitli miktarda yükü işlemek veya kullanılabilirliği artırmak için kullanılmalıdır. Hizmetlerinizi el ile ölçeklendirebilir veya otomatik ölçeklendirme ilkeleri ayarlayabilirsiniz.

[Otomatik ölçeklendirme,](service-fabric-mesh-scalability.md#autoscaling-service-instances) hizmet örneklerinizin (yatay ölçeklendirme) sayısını dinamik olarak ölçeklendirmenize olanak tanır. Otomatik ölçekleme büyük esneklik sağlar ve CPU veya bellek kullanımına dayalı servis örneklerinin sağlanmasını veya kaldırılmasını sağlar.

## <a name="options-for-creating-an-auto-scaling-policy-trigger-and-mechanism"></a>Otomatik ölçekleme ilkesi, tetikleyici ve mekanizma oluşturma seçenekleri
Ölçeklendirmek istediğiniz her hizmet için otomatik ölçeklendirme ilkesi tanımlanır. İlke YAML hizmet kaynak dosyasında veya JSON dağıtım şablonunda tanımlanır. Her ölçekleme ilkesi iki bölümden oluşur: bir tetikleyici ve ölçekleme mekanizması.

Tetikleyici, otomatik ölçekleme ilkesinin ne zaman çağrıldırıldıcı olduğunu tanımlar.  Tetikleyici türünü (ortalama yük) ve izlenecek ölçümü (CPU veya bellek) belirtin.  Yüzde olarak belirtilen üst ve alt yük eşikleri. Ölçek aralığı, belirtilen kullanımın (ortalama CPU yükü gibi) şu anda dağıtılan tüm hizmet örnekleri arasında ne sıklıkta denetlenileceği (saniye cinsinden) tanımlar.  İzlenen metrik alt eşiğin altına düştüğünde veya üst eşiğin üzerine çıktığında mekanizma tetiklenir.  

Ölçekleme mekanizması, ilke tetiklendiğinde ölçekleme işleminin nasıl gerçekleştirilini tanımlar.  Mekanizma türünü (yineleme ekleme/kaldırma), en küçük ve maksimum yineleme sayılarını (tümseler olarak) belirtin.  Hizmet yinelemelerinin sayısı hiçbir zaman minimum sayımaltında veya en yüksek sayımın üzerinde ölçeklendirilmez.  Ayrıca ölçeklendirme işleminde eklenecek veya kaldırılacak yineleme sayısı olan ölçek artışını tamsayı olarak belirtin.  

## <a name="define-an-auto-scaling-policy-in-a-json-template"></a>JSON şablonunda otomatik ölçekleme ilkesi tanımlama

Aşağıdaki örnekte, JSON dağıtım şablonunda otomatik ölçekleme ilkesi gösterilmektedir.  Otomatik ölçeklendirme ilkesi, ölçeklendirilecek hizmetin özelliğinde beyan edilir.  Bu örnekte, bir CPU ortalama yük tetikleyicisi tanımlanır.  Dağıtılan tüm örneklerin ortalama CPU yükü 0,2'nin (%20) altına düşerse mekanizma tetiklenir. veya 0.8 (%80) üzerinde gider.  CPU yükü her 60 saniyede bir kontrol edilir.  İlke tetiklenirse örnekleri eklemek veya kaldırmak için ölçekleme mekanizması tanımlanır.  Hizmet örnekleri, bir tanesinin artışlarla eklenir veya kaldırılır.  Bir ve en fazla 40 örnek sayısı nın en az örnek sayısı da tanımlanır.

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

## <a name="define-an-autoscale-policy-in-a-serviceyaml-resource-file"></a>Service.yaml kaynak dosyasında otomatik ölçeklendirme ilkesi tanımlama
Aşağıdaki örnekte, bir hizmet kaynağı (YAML) dosyasında otomatik ölçekleme ilkesi gösterilmektedir.  Otomatik ölçeklendirme ilkesi ölçeklendirilecek hizmetin bir özelliği olarak bildirilir.  Bu örnekte, bir CPU ortalama yük tetikleyicisi tanımlanır.  Dağıtılan tüm örneklerin ortalama CPU yükü 0,2'nin (%20) altına düşerse mekanizma tetiklenir. veya 0.8 (%80) üzerinde gider.  CPU yükü her 60 saniyede bir kontrol edilir.  İlke tetiklenirse örnekleri eklemek veya kaldırmak için ölçekleme mekanizması tanımlanır.  Hizmet örnekleri, bir tanesinin artışlarla eklenir veya kaldırılır.  Bir ve en fazla 40 örnek sayısı nın en az örnek sayısı da tanımlanır.

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
Bir hizmeti el ile nasıl [ölçeklendireceklerini](service-fabric-mesh-tutorial-template-scale-services.md) öğrenin
