---
title: Azure Event Hubs güvenlik duvarı kuralları | Microsoft Docs
description: Belirli IP adreslerinden Azure Event Hubs bağlantılara izin vermek için güvenlik duvarı kurallarını kullanın.
ms.topic: article
ms.date: 07/16/2020
ms.openlocfilehash: 2b886aaaf40e5c82d9c7ac3ce5abeda8f54cad3b
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87288050"
---
# <a name="configure-ip-firewall-rules-for-an-azure-event-hubs-namespace"></a>Azure Event Hubs ad alanı için IP güvenlik duvarı kurallarını yapılandırma
Varsayılan olarak, istek geçerli kimlik doğrulaması ve yetkilendirmeyle geldiği sürece, Event Hubs ad alanlarına internet 'ten erişilebilir. IP güvenlik duvarı ile, [CIDR (sınıfsız etki alanları arası yönlendirme)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösteriminde yalnızca bir dizi IPv4 adresi veya IPv4 adres aralığı ile sınırlayabilirsiniz.

Bu özellik, Azure Event Hubs yalnızca belirli bilinen sitelerden erişilebilir olması gereken senaryolarda yardımcı olur. Güvenlik duvarı kuralları, belirli IPv4 adreslerinden kaynaklanan trafiği kabul etmek için kuralları yapılandırmanızı sağlar. Örneğin, [Azure Express Route][express-route]ile Event Hubs kullanıyorsanız, yalnızca şirket ıçı altyapı IP adreslerinden gelen trafiğe izin vermek için bir **güvenlik duvarı kuralı** oluşturabilirsiniz. 

>[!WARNING]
> IP filtrelemeyi etkinleştirmek, diğer Azure hizmetlerinin Event Hubs etkileşimde olmasını engelleyebilir.
>
> Sanal ağlar uygulandığında güvenilen Microsoft Hizmetleri desteklenmez.
>
> Sanal ağlarla çalışmayan yaygın Azure senaryoları ( **listenin ayrıntılı olmadığına** unutmayın)-
> - Azure Stream Analytics
> - Azure IoT Hub yolları
> - Azure IoT Device Explorer
>
> Aşağıdaki Microsoft hizmetlerinin bir sanal ağda olması gerekir
> - Azure Web Apps
> - Azure İşlevleri


## <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları
IP güvenlik duvarı kuralları Event Hubs ad alanı düzeyinde uygulanır. Bu nedenle, kurallar desteklenen herhangi bir protokolü kullanarak istemcilerden gelen tüm bağlantılara uygulanır. Event Hubs ad alanındaki izin verilen bir IP kuralıyla eşleşmeyen bir IP adresinden gelen bağlantı girişimleri yetkisiz olarak reddedilir. Yanıt, IP kuralından bahsetmiyor. IP filtresi kuralları sırasıyla uygulanır ve IP adresiyle eşleşen ilk kural kabul etme veya reddetme eylemini belirler.

## <a name="use-azure-portal"></a>Azure portalı kullanma
Bu bölümde, bir Event Hubs ad alanı için IP güvenlik duvarı kuralları oluşturmak üzere Azure portal nasıl kullanılacağı gösterilmektedir. 

1. [Azure portal](https://portal.azure.com) **Event Hubs ad alanına** gidin.
2. Sol taraftaki menüden **ağ** seçeneği ' ni seçin. **Tüm ağlar** seçeneğini belirlerseniz, Olay Hub 'ı HERHANGI bir IP adresinden gelen bağlantıları kabul eder. Bu ayar 0.0.0.0/0 IP adresi aralığını kabul eden bir kuralla eşdeğerdir. 

    ![Güvenlik Duvarı-tüm ağlar seçeneği seçildi](./media/event-hubs-firewall/firewall-all-networks-selected.png)
1. Belirli ağlara ve IP adreslerine erişimi kısıtlamak için **Seçili ağlar** seçeneğini belirleyin. **Güvenlik duvarı** bölümünde şu adımları izleyin:
    1. Geçerli istemci IP 'nize ad alanına erişim sağlamak için **ISTEMCI IP adresi ekle** seçeneğini belirleyin. 
    2. **Adres aralığı**IÇIN, CIDR gösteriminde belirli bir IPv4 adresi veya bir IPv4 adresi aralığı girin. 
    3. **Güvenilen Microsoft hizmetlerinin bu güvenlik duvarını atlamasına izin**vermek isteyip istemediğinizi belirtin. 

        > [!WARNING]
        > **Seçili ağlar** seçeneğini BELIRLEYIP bir IP adresi veya adres aralığı belirtmezseniz, hizmet tüm ağlardan gelen trafiğe izin verir. 

        ![Güvenlik Duvarı-tüm ağlar seçeneği seçildi](./media/event-hubs-firewall/firewall-selected-networks-trusted-access-disabled.png)
3. Ayarları kaydetmek için araç çubuğunda **Kaydet** ' i seçin. Onayın Portal bildirimlerinde gösterilmesi için birkaç dakika bekleyin.


## <a name="use-resource-manager-template"></a>Resource Manager şablonu kullanma

> [!IMPORTANT]
> Güvenlik duvarı kuralları, Event Hubs **Standart** ve **adanmış** katmanlarında desteklenir. Temel katmanda desteklenmez.

Aşağıdaki Kaynak Yöneticisi şablonu, var olan bir Event Hubs ad alanına bir IP filtre kuralı eklenmesine olanak sağlar.

Şablon parametreleri:

- **IPMask** , CIDR gösteriminde tek bir IPv4 ADRESIDIR veya IP adresi bloğudur. Örneğin, CıDR gösteriminde 70.37.104.0/24, Aralık için önemli olan önek bit sayısını belirten, 70.37.104.0 ile 70.37.104.255 arasındaki 256 IPv4 adresini temsil eder.

> [!NOTE]
> Mümkün olan reddetme kuralları olmadığı sürece, Azure Resource Manager şablonu, bağlantıları kısıtlayameyen **"Izin ver"** olarak ayarlanmış varsayılan eylemi içerir.
> Sanal ağ veya güvenlik duvarları kuralları yaparken, ***"DefaultAction"*** öğesini değiştirmemiz gerekir
> 
> Kaynak
> ```json
> "defaultAction": "Allow"
> ```
> şöyle değiştirin:
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
      "location": {
        "type": "string",
        "metadata": {
          "description": "Location for Namespace"
        }
      }
    },
    "variables": {
      "namespaceNetworkRuleSetName": "[concat(parameters('eventhubNamespaceName'), concat('/', 'default'))]",
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
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.EventHub/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.EventHub/namespaces/', parameters('eventhubNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": [<YOUR EXISTING VIRTUAL NETWORK RULES>],
          "ipRules": 
          [
            {
                "ipMask":"10.1.1.1",
                "action":"Allow"
            },
            {
                "ipMask":"11.0.0.0/24",
                "action":"Allow"
            }
          ],
          "trustedServiceAccessEnabled": false,
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

Şablonu dağıtmak için [Azure Resource Manager][lnk-deploy]talimatlarını izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure sanal ağlarına Event Hubs erişimi kısıtlayan için şu bağlantıya bakın:

- [Event Hubs için sanal ağ hizmeti uç noktaları][lnk-vnet]

<!-- Links -->

[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[lnk-vnet]: event-hubs-service-endpoints.md
