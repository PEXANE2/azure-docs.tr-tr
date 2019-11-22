---
title: Sanal ağ hizmet uç noktaları - Azure Event Hubs | Microsoft Docs
description: Bu makalede bir sanal ağa Microsoft. EventHub hizmet uç noktası ekleme hakkında bilgi sağlanır.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 03/12/2019
ms.author: shvija
ms.openlocfilehash: 5b02b79980ebe5ea91a1cf16d3ea453ebef3bf08
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74279791"
---
# <a name="use-virtual-network-service-endpoints-with-azure-event-hubs"></a>Azure Event Hubs ile sanal ağ hizmet uç noktaları kullanma

[Sanal ağ (VNet) hizmet uç noktaları][vnet-sep] ile Event Hubs tümleştirmesi, sanal ağlara bağlı sanal makineler gibi iş yüklerinden, her ikisinde de güvenli hale getirilen ağ trafiği yolu ile güvenli erişim sağlar ucundaki.

En az bir sanal ağ alt ağ hizmeti uç noktasına bağlanacak şekilde yapılandırıldıktan sonra, ilgili Event Hubs ad alanı artık sanal ağlardaki her yerden trafiği kabul etmez. Sanal ağ açısından bakıldığında, sanal ağ alt ağından bir yalıtılmış ağ tüneli Mesajlaşma hizmeti için hizmet uç noktası bir Event Hubs ad alanı bağlama yapılandırır. 

Sonuç, alt ağ ve ilgili Event Hubs ad alanı, Mesajlaşma Hizmeti uç noktası bir genel IP aralığında olma gözlemlenebilir ağ adresi artma bağlı iş yükleri arasındaki özel ve yalıtılmış bir ilişkidir. Bu davranış için bir özel durum vardır. Hizmet uç noktasının etkinleştirilmesi, varsayılan olarak, sanal ağla ilişkili IP güvenlik duvarında denyall kuralını etkinleştirir. Olay Hub 'ı genel uç noktasına erişimi etkinleştirmek için IP güvenlik duvarında belirli IP adresleri ekleyebilirsiniz. 


>[!WARNING]
> Sanal ağlar tümleştirmesini uygulamak, diğer Azure hizmetlerinin Event Hubs etkileşimde olmasını engelleyebilir.
>
> Sanal ağlar uygulandığında güvenilen Microsoft Hizmetleri desteklenmez.
>
> Sanal ağlarla çalışmayan yaygın Azure senaryoları ( **listenin ayrıntılı olmadığına** unutmayın)-
> - Azure Stream Analytics
> - Azure Event Grid ile tümleştirme
> - Azure IoT Hub yolları
> - Azure IoT Device Explorer
>
> Aşağıdaki Microsoft hizmetlerinin bir sanal ağda olması gerekir
> - Azure Web Apps
> - Azure İşlevleri

> [!IMPORTANT]
> Sanal ağlar desteklenir **standart** ve **adanmış** Event Hubs'ın katmanları. Temel katmanda desteklenmiyor.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>VNet tümleştirmesi etkin Gelişmiş Güvenlik senaryoları 

Sıkı ve compartmenbir güvenlik gerektiren ve sanal ağ alt ağlarının, compartmençalıştıran hizmetler arasında segmentleme sağladığı çözümler, yine de bu bölmeleri bulunan hizmetler arasında iletişim yollarına gerek duyar.

TCP/IP üzerinden HTTPS taşıyan dahil olmak üzere bölmeler arasında anında herhangi IP yönlendirme, güvenlik açıklarına karşı ağ katmanı kötüye kullanılma riskini taşır üzerinde yukarı. Burada iletileri bile, taraflar arasında geçiş olarak diske yazılır, tamamen yalıtılmış iletişim yolları Mesajlaşma hizmetleri sağlar. İlgili ağ yalıtım sınırı bütünlüğü korunur ancak her ikisi de aynı Event Hubs örneğine bağlı olan iki farklı sanal ağlarda bulunan iş yüklerini verimli bir şekilde ve güvenilir bir şekilde aracılığıyla iletileri, iletişim kurabilir.
 
Bu, bulut çözümleri yalnızca Azure sektör lideri güvenilir ve ölçeklenebilir zaman uyumsuz Mesajlaşma işlevlerini erişmesine, ancak bunlar artık Mesajlaşma iletişim yolları arasında güvenli bir çözüm oluşturmak için kullanabileceğiniz önemli güvenlik compartments anlamına gelir. HTTPS ve diğer TLS Güvenli Yuva protokolleri dahil olmak üzere, tüm eşler arası iletişimi modu ile ulaşılabilir nedir daha doğal olarak daha güvenlidir.

## <a name="bind-event-hubs-to-virtual-networks"></a>Olay hub'ları sanal ağlara bağlama

*Sanal ağ kuralları* Azure Event Hubs ad alanınız belirli bir sanal ağ alt ağından gelen bağlantıları kabul edip etmeyeceğini denetleyen güvenlik duvarı güvenliği özelliğidir.

Bir Event Hubs ad alanı, bir sanal ağa bağlama iki adımlı bir işlemdir. Önce bir sanal ağ alt ağında bir **sanal ağ hizmeti uç noktası** oluşturmanız ve bunu [hizmet uç noktasına genel bakış][vnet-sep]bölümünde açıklandığı gibi "Microsoft. EventHub" için etkinleştirmeniz gerekir. Hizmet uç noktası ekledikten sonra Event Hubs ad alanı ile bağlama bir *sanal ağ kuralı*.

Sanal ağ kuralı, bir sanal ağ alt ağıyla Event Hubs ad alanının bir ilişkidir. Kural bulunduğu sürece bir alt ağa bağlı tüm iş yükleri Event Hubs ad alanına erişimi verilir. Event hubs'ı kendisi asla giden bağlantı kurar, erişim gerekmez ve bu nedenle asla erişimi alt ağınız bu kuralı etkinleştirmek tarafından verilir.

### <a name="create-a-virtual-network-rule-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile bir sanal ağ kuralı oluşturma

Aşağıdaki Resource Manager şablonu var olan bir Event Hubs ad alanı için bir sanal ağ kuralı ekleyerek sağlar.

Şablon parametreleri:

* **namespaceName**: Event Hubs ad alanı.
* **vnetRuleName**: Oluşturulacak sanal ağ kuralı adı.
* **virtualNetworkingSubnetId**: tam Resource Manager yolu için sanal ağ alt ağı; Örneğin, `/subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` sanal ağ varsayılan alt ağ.

> [!NOTE]
> Mümkün olan reddetme kuralları olmadığı sürece, Azure Resource Manager şablonu, bağlantıları kısıtlayameyen **"Izin ver"** olarak ayarlanmış varsayılan eylemi içerir.
> Sanal ağ veya güvenlik duvarları kuralları yaparken, ***"DefaultAction"*** öğesini değiştirmemiz gerekir
> 
> from
> ```json
> "defaultAction": "Allow"
> ```
> -
> ```json
> "defaultAction": "Deny"
> ```
>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "eventhubNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Event Hubs namespace"
        }
      },
      "virtualNetworkName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Virtual Network Rule"
        }
      },
      "subnetName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Virtual Network Sub Net"
        }
      },
      "location": {
        "type": "string",
        "metadata": {
          "description": "Location for Namespace"
        }
      }
    },
    "variables": {
      "namespaceNetworkRuleSetName": "[concat(parameters('eventhubNamespaceName'), concat('/', 'default'))]",
      "subNetId": "[resourceId('Microsoft.Network/virtualNetworks/subnets/', parameters('virtualNetworkName'), parameters('subnetName'))]"
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('eventhubNamespaceName')]",
        "type": "Microsoft.EventHub/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Standard",
          "tier": "Standard"
        },
        "properties": { }
      },
      {
        "apiVersion": "2017-09-01",
        "name": "[parameters('virtualNetworkName')]",
        "location": "[parameters('location')]",
        "type": "Microsoft.Network/virtualNetworks",
        "properties": {
          "addressSpace": {
            "addressPrefixes": [
              "10.0.0.0/23"
            ]
          },
          "subnets": [
            {
              "name": "[parameters('subnetName')]",
              "properties": {
                "addressPrefix": "10.0.0.0/23",
                "serviceEndpoints": [
                  {
                    "service": "Microsoft.EventHub"
                  }
                ]
              }
            }
          ]
        }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.EventHub/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.EventHub/namespaces/', parameters('eventhubNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": 
          [
            {
              "subnet": {
                "id": "[variables('subNetId')]"
              },
              "ignoreMissingVnetServiceEndpoint": false
            }
          ],
          "ipRules":[<YOUR EXISTING IP RULES>],
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

Şablonu dağıtmak için [Azure Resource Manager][lnk-deploy]talimatlarını izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Sanal ağlar hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

- [Azure sanal ağ hizmet uç noktaları][vnet-sep]
- [Azure Event Hubs IP filtreleme][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/resource-group-template-deploy.md
[ip-filtering]: event-hubs-ip-filtering.md
