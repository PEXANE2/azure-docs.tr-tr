---
title: İstekleri yönlendirmek için ağ geçidini yapılandırma
description: Service Fabric Mesh'te çalışan uygulamanız için gelen trafiği işleyen ağ geçidini nasıl yapılandıracaklarını öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: ec408403d4baa0f211c6bfe867a15c96513693cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75461959"
---
# <a name="configure-a-gateway-resource-to-route-requests"></a>İstekleri yönlendirmek için ağ geçidi kaynağını yapılandırma

Ağ Geçidi kaynağı, gelen trafiği uygulamanızı barındıran ağa yönlendirmek için kullanılır. İsteklerin belirli hizmetlere veya bitiş noktalarına yönlendirildiği kuralları, isteğin yapısına göre belirtecek şekilde yapılandırın. Mesh'teki ağlar ve ağ geçitleri hakkında daha fazla bilgi için [Service Fabric Mesh'te ağ bağlantısına giriş](service-fabric-mesh-networks-and-gateways.md) ebak. 

Ağ geçidi kaynaklarının dağıtım şablonunuzun (JSON veya yaml) bir parçası olarak bildirilmesi gerekir ve ağ kaynağına bağlıdır. Bu belge, ağ geçidiniz için ayarlanabilecek çeşitli özellikleri özetler ve örnek bir ağ geçidi config'ini kapsar.

## <a name="options-for-configuring-your-gateway-resource"></a>Ağ Geçidi kaynağınızı yapılandırma seçenekleri

Ağ Geçidi kaynağı, uygulamanızın ağı yla temel altyapı ağı (ağ) `open` arasında bir köprü görevi gördüğünüzden. Yalnızca bir tane yapılandırmanız gerekir (Mesh önizlemesinde uygulama başına bir ağ geçidi sınırı vardır). Kaynak bildirimi iki ana bölümden oluşur: kaynak meta verileri ve özellikleri. 

### <a name="gateway-resource-metadata"></a>Ağ geçidi kaynak meta verileri

Bir ağ geçidi aşağıdaki meta verilerle bildirilir:
* `apiVersion`- "2018-09-01-önizleme" (veya daha sonra, gelecekte) olarak ayarlanması gerekir
* `name`- Bu ağ geçidi için bir dize adı
* `type`- "Microsoft.ServiceFabricMesh/ağ geçitleri"
* `location`- Uygulamanızın / ağınızın konumuna ayarlanmalıdır; genellikle dağıtımınızdaki konum parametresine bir başvuru olacaktır
* `dependsOn`- bu ağ için bir giriş noktası olarak hizmet verecek ağ

Azure Kaynak Yöneticisi (JSON) dağıtım şablonunda şu şekilde görünür: 

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

### <a name="gateway-properties"></a>Ağ geçidi özellikleri

Özellikler bölümü, ağ geçidinin bulunduğu ağları ve yönlendirme isteklerikurallarını tanımlamak için kullanılır. 

#### <a name="source-and-destination-network"></a>Kaynak ve hedef ağ 

Her ağ `sourceNetwork` geçidi `destinationNetwork`bir ve . Kaynak ağ, uygulamanızın gelen istekleri alacağı ağ olarak tanımlanır. Ad özelliği her zaman "Aç" olarak ayarlanmalıdır. Hedef ağ, isteklerin hedeflenen ağdır. Bunun ad değeri uygulamanızın yerel ağının kaynak adına ayarlanmalıdır (kaynağa tam başvuru içermelidir). "myNetwork" adlı bir ağda dağıtım için bunun neye benzemesine benzer bir örnek config için aşağıya bakın.

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

Ağ geçidinde, gelen trafiğin nasıl işleneceğini belirten birden çok yönlendirme kuralı olabilir. Yönlendirme kuralı, dinleme bağlantı noktası ile belirli bir uygulamaiçin hedef bitiş noktası arasındaki ilişkiyi tanımlar. TCP yönlendirme kuralları için Bağlantı Noktası:Bitiş Noktası arasında 1:1 eşleme vardır. HTTP yönlendirme kuralları için, isteğin nasıl yönlendirilenecek olacağına karar vermek için isteğe bağlı olarak üstbilgi yolunu inceleyen daha karmaşık yönlendirme kuralları ayarlayabilirsiniz. 

Yönlendirme kuralları bağlantı noktası bazında belirtilir. Her giriş bağlantı noktası, ağ geçidi config özellikleri bölümünde kurallar kendi dizi vardır. 

#### <a name="tcp-routing-rules"></a>TCP yönlendirme kuralları 

TCP yönlendirme kuralı aşağıdaki özelliklerden oluşur: 
* `name`- Seçtiğiniz herhangi bir dize olabilir kurala başvuru 
* `port`- gelen istekleri dinlemek için bağlantı noktası 
* `destination`- isteklerin yönlendirilmesi `serviceName`gereken `endpointName`durumlarda , ve , içeren `applicationName`uç nokta belirtimi

Aşağıda bir örnek TCP yönlendirme kuralı verilmiştir:

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

BIR HTTP yönlendirme kuralı aşağıdaki özelliklerden oluşur: 
* `name`- Seçtiğiniz herhangi bir dize olabilir kurala başvuru 
* `port`- gelen istekleri dinlemek için bağlantı noktası 
* `hosts`- Yukarıda belirtilen bağlantı noktasında çeşitli "ana bilgisayarlara" gelen istekler için geçerli olan bir dizi ilke. Ana bilgisayarlar, ağda çalışan ve gelen isteklere, yani bir web uygulamasına hizmet edebilen uygulama ve hizmetler kümesidir. Ana bilgisayar ilkeleri sırayla yorumlanır, bu nedenle azalan özgüllük düzeylerinde aşağıdakileri oluşturmalısınız
    * `name`- Aşağıdaki yönlendirme kurallarının belirtildiği ana bilgisayar ının DNS adı. Burada "*" kullanmak tüm ana bilgisayarlar için yönlendirme kuralları oluşturur.
    * `routes`- Bu belirli ana bilgisayar için bir dizi ilke
        * `match`- Bu kuralın uygulanması için gelen istek yapısının belirtimi, bir dayalı`path`
            * `path`- (gelen `value` URI), `rewrite` (isteğin nasıl iletilmesini istediğiniz) `type` ve (şu anda yalnızca "Önek" olabilir) içerir.
            * `header`- Isteğin üstbilgisinde eşleşen isteğe bağlı üstbilgi değerleri dizisidir, istek yol belirtimiyle eşleşirse (üstte).
              * her giriş `name` içerir (eşleşecek üstbilginin `value` dize adı), (istekteki üstbilginin dize değeri) ve (şu `type` anda yalnızca "Tam" olabilir)
        * `destination`- istek eşleşirse, bu hedefe yönlendirilir, hangi `applicationName`bir `serviceName`kullanılarak belirtilen , ve`endpointName`

Burada, bağlantı noktası 80'den gelen istekler için, bu ağdaki uygulamalar tarafından sunulan tüm ana bilgisayarlariçin geçerli olacak bir örnek HTTP yönlendirme kuralı verilmiştir. İstek URL'sinde yol belirtimiyle eşleşen bir yapı `<IPAddress>:80/pickme/<requestContent>`varsa, yani bitiş noktasına `myListener` yönlendirilir.  

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

## <a name="sample-config-for-a-gateway-resource"></a>Ağ Geçidi kaynağı için örnek config 

Burada tam bir Ağ Geçidi kaynak config gibi görünüyor (bu [Mesh örnekleri repo](https://github.com/Azure-Samples/service-fabric-mesh/blob/2018-09-01-preview/templates/ingress/meshingress.linux.json)mevcut giriş örneğinden uyarlanmıştır):

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

Bu ağ geçidi, bağlantı noktası 80'i dinleyen en az iki hizmetolan "helloWorldService" ve "counterService"den oluşan bir Linux uygulaması olan "meshAppLinux" için yapılandırılmıştır. Gelen isteğin URL yapısına bağlı olarak, isteği bu hizmetlerden birine yönlendirir. 
* "\<IPAddress>:80/helloWorld/\<request\>" bir istek helloWorldService "helloWorldListener" yönlendirilen neden olur. 
* "\<IPAddress>:80/counter/\<request\>" bir istek counterService "counterListener" yönlendirilen neden olur. 

## <a name="next-steps"></a>Sonraki adımlar
* Ağ geçitlerini iş başında görmek için [Giriş örneğini](https://github.com/Azure-Samples/service-fabric-mesh/tree/2018-09-01-preview/templates/ingress) dağıtma
