---
title: İstekleri yönlendirmek için bir ağ geçidi yapılandırma
description: Service Fabric ağ üzerinde çalışan uygulamanız için gelen trafiği işleyen ağ geçidini nasıl yapılandıracağınızı öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: ec408403d4baa0f211c6bfe867a15c96513693cb
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75461959"
---
# <a name="configure-a-gateway-resource-to-route-requests"></a>İstekleri yönlendirmek için bir ağ geçidi kaynağı yapılandırma

Bir ağ geçidi kaynağı, gelen trafiği uygulamanızın barındırıldığı ağa yönlendirmek için kullanılır. İsteğin yapısına bağlı olarak, isteklerin belirli hizmetlere veya uç noktalara yönlendirildiği kuralları belirtmek için yapılandırın. Ağ ve ağ geçitleri hakkında daha fazla bilgi için bkz. Service Fabric ağdayken [ağa giriş](service-fabric-mesh-networks-and-gateways.md) . 

Ağ Geçidi kaynaklarının, dağıtım şablonunuzun (JSON veya YAML) bir parçası olarak bildirilmesini ve bir ağ kaynağına bağlı olması gerekir. Bu belge, ağ geçidiniz için ayarlanabileceğini ve örnek bir ağ geçidi yapılandırmasını ele alan çeşitli özellikleri özetler.

## <a name="options-for-configuring-your-gateway-resource"></a>Ağ Geçidi kaynağınızı yapılandırma seçenekleri

Ağ Geçidi kaynağı, uygulamanızın ağı ile temeldeki altyapının ağı (`open` ağı) arasında bir köprü görevi görür. Yalnızca bir tane yapılandırmanız gerekir (ağ önizlemesinde, uygulama başına bir ağ geçidi sınırı vardır). Kaynak bildirimi iki ana bölümden oluşur: kaynak meta verileri ve özellikleri. 

### <a name="gateway-resource-metadata"></a>Ağ Geçidi kaynak meta verileri

Bir ağ geçidi aşağıdaki meta verilerle bildiriliyor:
* `apiVersion`-gelecekte "2018-09-01-Preview" (veya sonraki bir sürümü) olarak ayarlanması gerekiyor
* `name`-bu ağ geçidi için bir dize adı
* `type`-"Microsoft. Servicefabrickafes/Gateway"
* `location`-uygulamanızın/ağınızın konumuna ayarlanmalıdır; genellikle dağıtımınızdaki konum parametresine bir başvuru olur
* `dependsOn`-bu ağ geçidi için bir giriş noktası olarak kullanılacak ağ

Azure Resource Manager (JSON) dağıtım şablonunda şöyle görünür: 

```json
{
  "apiVersion": "2018-09-01-preview",
  "name": "myGateway",
  "type": "Microsoft.ServiceFabricMesh/gateways",
  "location": "[parameters('location')]",
  "dependsOn": [
    "Microsoft.ServiceFabricMesh/networks/myNetwork"
  ],
  "properties": {
    [...]
  }
}
```

### <a name="gateway-properties"></a>Ağ Geçidi özellikleri

Özellikler bölümü, ağ geçidinin bulunduğu ağları ve yönlendirme isteklerinin kurallarını tanımlamak için kullanılır. 

#### <a name="source-and-destination-network"></a>Kaynak ve hedef ağ 

Her ağ geçidi için bir `sourceNetwork` ve `destinationNetwork`gerekir. Kaynak ağ, uygulamanızın gelen istekleri alacağı ağ olarak tanımlanır. Name özelliği her zaman "Open" olarak ayarlanmalıdır. Hedef ağ, isteklerin hedeflediği bir ağ. Bunun için ad değeri, uygulamanızın yerel ağının kaynak adı olarak ayarlanmalıdır (kaynağa tam başvuru dahil edilmelidir). "MyNetwork" adlı bir ağda bir dağıtım için bunun nasıl görüneceğine ilişkin örnek bir yapılandırma için aşağıya bakın.

```json 
"properties": {
  "description": "Service Fabric Mesh Gateway",
  "sourceNetwork": {
    "name": "Open"
  },
  "destinationNetwork": {
    "name": "[resourceId('Microsoft.ServiceFabricMesh/networks', 'myNetwork')]"
  },
  [...]
}
```

#### <a name="rules"></a>Kurallar 

Bir ağ geçidi, gelen trafiğin nasıl işleneceğini belirten birden çok yönlendirme kuralına sahip olabilir. Bir yönlendirme kuralı, belirli bir uygulamanın dinleme bağlantı noktası ve hedef uç noktası arasındaki ilişkiyi tanımlar. TCP yönlendirme kuralları için, bağlantı noktası: uç noktası arasında 1:1 eşlemesi vardır. HTTP yönlendirme kuralları için isteğin yolunu inceleyecek daha karmaşık yönlendirme kuralları ve isteğe bağlı olarak, isteğin nasıl yönlendirilileceğine karar verebilirsiniz. 

Yönlendirme kuralları her bağlantı noktası temelinde belirtilir. Her giriş bağlantı noktasının, ağ geçidi yapılandırmasının Özellikler bölümünde kendi kural dizisi vardır. 

#### <a name="tcp-routing-rules"></a>TCP yönlendirme kuralları 

TCP yönlendirme kuralı aşağıdaki özelliklerden oluşur: 
* `name`, istediğiniz herhangi bir dize olabilecek kurala başvuru 
* `port`-gelen istekler için dinlemek için bağlantı noktası 
* isteklerin yönlendirilmek zorunda olduğu konum için `applicationName`, `serviceName`ve `endpointName`içeren `destination` uç nokta belirtimi

Örnek bir TCP yönlendirme kuralı aşağıda verilmiştir:

```json
"properties": {
  [...]
  "tcp": [
    {
      "name": "web",
      "port": 80,
      "destination": {
        "applicationName": "myApp",
        "serviceName": "myService",
        "endpointName": "myListener"
      }
    }
  ]
}
```


#### <a name="http-routing-rules"></a>HTTP yönlendirme kuralları 

Bir HTTP yönlendirme kuralı aşağıdaki özelliklerden oluşur: 
* `name`, istediğiniz herhangi bir dize olabilecek kurala başvuru 
* `port`-gelen istekler için dinlemek için bağlantı noktası 
* `hosts`-yukarıda belirtilen bağlantı noktasında çeşitli "konaklara" gelen isteklere uygulanan bir ilke dizisi. Konaklar, ağda çalışan uygulamalar ve hizmetler kümesidir ve gelen isteklere (örn. bir Web uygulaması) hizmet verebilir. Ana bilgisayar ilkeleri sırayla yorumlanıyor, bu nedenle aşağıdakileri azalan düzeyde ayrıntısıyla olarak oluşturmanız gerekir
    * `name`-aşağıdaki yönlendirme kuralları belirtilen ana bilgisayarın DNS adı. Burada "*" kullanılması, tüm konaklar için yönlendirme kuralları oluşturur.
    * `routes`-bu konak için bir ilke dizisi
        * Bu kural için, bir `path` göre uygulanacak gelen istek yapısının belirtimi `match`
            * `path`-bir `value` (gelen URI), `rewrite` (isteğin iletilmesini istediğiniz) ve bir `type` (Şu anda yalnızca "önek" olabilir) içerir
            * `header`, isteğin üst bilgisinde, istek yol belirtimiyle (yukarıda) eşleşiyorsa, eşleştirilecek bir üst bilgi değerleri dizisidir.
              * her giriş `name` (eşleştirilecek üstbilginin dize adı), `value` (istekteki üstbilginin dize değeri) ve bir `type` (Şu anda yalnızca "tam" olabilir) içerir
        * `destination`-istek eşleşiyorsa, `applicationName`, `serviceName`ve kullanarak belirtilen bu hedefe yönlendirilir `endpointName`

Bu ağdaki uygulamalar tarafından sunulan tüm konaklara 80 numaralı bağlantı noktası üzerinden gelen istekler için uygulanacak örnek bir HTTP yönlendirme kuralı aşağıda verilmiştir. İstek URL 'SI yol belirtimiyle eşleşen bir yapıya sahipse, yani, `<IPAddress>:80/pickme/<requestContent>``myListener` uç noktasına yönlendirilir.  

```json
"properties": {
  [...]
  "http": [
    {
      "name": "web",
      "port": 80,
      "hosts": [
        {
          "name": "*",
          "routes": [
            {
              "match": {
                "path": {
                  "value": "/pickme",
                  "rewrite": "/",
                  "type": "Prefix"
                }
              },
              "destination": {
                "applicationName": "meshApp",
                "serviceName": "myService",
                "endpointName": "myListener"
              }
            }
          ]
        }
      ]
    }
  ]
}
```

## <a name="sample-config-for-a-gateway-resource"></a>Bir ağ geçidi kaynağı için örnek yapılandırma 

Tam ağ geçidi kaynak yapılandırması şöyle görünür (Bu durum, [kafes örnekleri](https://github.com/Azure-Samples/service-fabric-mesh/blob/2018-09-01-preview/templates/ingress/meshingress.linux.json)deposunda bulunan giriş örneğinden uyarlanmıştır):

```json
{
  "apiVersion": "2018-09-01-preview",
  "name": "ingressGatewayLinux",
  "type": "Microsoft.ServiceFabricMesh/gateways",
  "location": "[parameters('location')]",
  "dependsOn": [
    "Microsoft.ServiceFabricMesh/networks/meshNetworkLinux"
  ],
  "properties": {
    "description": "Service Fabric Mesh Gateway for Linux mesh samples.",
    "sourceNetwork": {
      "name": "Open"
    },
    "destinationNetwork": {
      "name": "[resourceId('Microsoft.ServiceFabricMesh/networks', 'meshNetworkLinux')]"
    },
    "http": [
      {
        "name": "web",
        "port": 80,
        "hosts": [
          {
            "name": "*",
            "routes": [
              {
                "match": {
                  "path": {
                    "value": "/hello",
                    "rewrite": "/",
                    "type": "Prefix"
                  }
                },
                "destination": {
                  "applicationName": "meshAppLinux",
                  "serviceName": "helloWorldService",
                  "endpointName": "helloWorldListener"
                }
              },
              {
                "match": {
                  "path": {
                    "value": "/counter",
                    "rewrite": "/",
                    "type": "Prefix"
                  }
                },
                "destination": {
                  "applicationName": "meshAppLinux",
                  "serviceName": "counterService",
                  "endpointName": "counterServiceListener"
                }
              }
            ]
          }
        ]
      }
    ]
  }
}
```

Bu ağ geçidi, 80 bağlantı noktasını dinleyen "Merhaba Dünya hizmeti" ve "counterService" olmak üzere en az iki hizmetten oluşan "Mesetlinux" bir Linux uygulaması için yapılandırılmıştır. Gelen isteğin URL yapısına bağlı olarak, isteği bu hizmetlerden birine yönlendirir. 
* "\<IPAddress >: 80/helloWorld/\<isteği\>", bir isteğin helloWorldService 'teki "helloWorldListener" öğesine yönlendirilme oluşmasına neden olur. 
* "\<IPAddress >: 80/Counter/\<isteği\>" bir isteğin, counterService 'teki "counterListener" öğesine yönlendirilme oluşmasına neden olur. 

## <a name="next-steps"></a>Sonraki adımlar
* Ağ geçitlerini eylemde görmek için giriş [örneğini](https://github.com/Azure-Samples/service-fabric-mesh/tree/2018-09-01-preview/templates/ingress) dağıtın
