---
title: Azure Olay Hub'ları Güvenlik Duvarı Kuralları | Microsoft Dokümanlar
description: Belirli IP adreslerinden Azure Etkinlik Hub'larına bağlantılara izin vermek için Güvenlik Duvarı Kuralları'nı kullanın.
services: event-hubs
documentationcenter: ''
author: spelluru
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.custom: seodec18
ms.topic: article
ms.date: 12/20/2019
ms.author: spelluru
ms.openlocfilehash: 18212726f0ab921a05a3b640a32754c62958d047
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393134"
---
# <a name="configure-ip-firewall-rules-for-an-azure-event-hubs-namespace"></a>Azure Etkinlik Hub'ları ad alanı için IP güvenlik duvarı kurallarını yapılandırma
Varsayılan olarak, İstek geçerli kimlik doğrulama ve yetkilendirmeyle birlikte geldiği sürece, Olay Hub'ları ad alanlarına internetten erişilebilir. IP güvenlik duvarı ile, [cidr (Classless Etki Alanı Yönlendirmesi)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösteriminde yalnızca bir iPv4 adresi kümesi veya IPv4 adres aralıklarıyla daha da kısıtlayabilirsiniz.

Bu özellik, Azure Etkinlik Hub'larına yalnızca belirli tanınmış sitelerden erişilmesi gereken senaryolarda yararlıdır. Güvenlik duvarı kuralları, belirli IPv4 adreslerinden kaynaklanan trafiği kabul etmek için kuralları yapılandırmanızı sağlar. Örneğin, [Azure Ekspres Rotası][express-route]ile Etkinlik Hub'ları kullanıyorsanız, yalnızca şirket içi altyapı IP adreslerinizden gelen trafiğe izin vermek için bir güvenlik duvarı **kuralı** oluşturabilirsiniz. 

>[!WARNING]
> IP filtrelemesini etkinleştirmek, diğer Azure hizmetlerinin Etkinlik Hub'larıyla etkileşimkurmasını engelleyebilir.
>
> Sanal Ağlar uygulandığında güvenilen Microsoft hizmetleri desteklenmez.
>
> Sanal Ağlarla çalışmayan yaygın Azure senaryoları (listenin **ayrıntılı** olmadığını unutmayın) -
> - Azure Monitör (tanılama ayarı)
> - Azure Stream Analytics
> - Azure Olay Izgarasıyla Tümleştirme
> - Azure IoT Hub Rotaları
> - Azure IoT Aygıt Gezgini
>
> Aşağıdaki Microsoft hizmetlerinin sanal ağda olması gerekir
> - Azure Web Apps
> - Azure İşlevleri


## <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları
IP güvenlik duvarı kuralları Olay Hub'ları ad alanı düzeyinde uygulanır. Bu nedenle, kurallar desteklenen herhangi bir protokol kullanarak istemcilerden gelen tüm bağlantılar için geçerlidir. Olay Hub'ları ad alanında izin verilen bir IP kuralıyla eşleşmeyen bir IP adresinden gelen bağlantı girişimi yetkisiz olarak reddedilir. Yanıtta IP kuralından bahsedilmez. Sırayla IP filtresi kuralları uygulanır ve IP adresiyle eşleşen ilk kural kabul veya reddetme eylemini belirler.

## <a name="use-azure-portal"></a>Azure portalı kullanma
Bu bölümde, Olay Hub'ları ad alanı için IP güvenlik duvarı kuralları oluşturmak için Azure portalının nasıl kullanılacağı gösterilmektedir. 

1. [Azure portalındaki](https://portal.azure.com) **Etkinlik Hub'larınız ad alanına** gidin.
2. Sol menüde **Ağ seçeneği'ni** seçin. **Tüm ağlar** seçeneğini seçerseniz, olay merkezi herhangi bir IP adresinden bağlantıları kabul eder. Bu ayar, 0.0.0.0/0 IP adresi aralığını kabul eden bir kurala eşdeğerdir. 

    ![Güvenlik Duvarı - Tüm ağlar seçeneği seçildi](./media/event-hubs-firewall/firewall-all-networks-selected.png)
1. Belirli ağlara ve IP adreslerine erişimi kısıtlamak için **Seçili Ağlar** seçeneğini belirleyin. Güvenlik **Duvarı** bölümünde aşağıdaki adımları izleyin:
    1. Geçerli istemcinize ad alanına erişim sağlamak için **istemci IP adresinizi ekle** seçeneğini seçin. 
    2. **Adres aralığı**için CIDR gösterimine belirli bir IPv4 adresi veya bir dizi IPv4 adresi girin. 
    3. **Güvenilen Microsoft hizmetlerinin bu güvenlik duvarLarını atlayıp atlamayacağına izin**vermek isteyip istemediğinizi belirtin. 

        ![Güvenlik Duvarı - Tüm ağlar seçeneği seçildi](./media/event-hubs-firewall/firewall-selected-networks-trusted-access-disabled.png)
3. Ayarları kaydetmek için araç çubuğunda **Kaydet'i** seçin. Onayın portal bildirimlerinde gösterilmesi için birkaç dakika bekleyin.


## <a name="use-resource-manager-template"></a>Resource Manager şablonu kullanma

> [!IMPORTANT]
> Güvenlik duvarı kuralları, Olay Hub'larının **standart** ve **özel** katmanlarında desteklenir. Temel katmanda desteklenmiyor.

Aşağıdaki Kaynak Yöneticisi şablonu, varolan bir Olay Hub'ları ad alanına IP filtresi kuralı eklemeyi sağlar.

Şablon parametreleri:

- **ipMask,** CIDR gösteriminde tek bir IPv4 adresi veya IP adresi bloğudur. Örneğin, CIDR gösteriminde 70.37.104.0/24, 70.37.104.0 ile 70.37.104.255 arasında 256 IPv4 adresini temsil eder ve 24 aralık için önemli öneek bitlerinin sayısını gösterir.

> [!NOTE]
> İnkar edilen kurallar mümkün olmasa da, Azure Kaynak Yöneticisi şablonu bağlantıları kısıtlamayan varsayılan eylem **kümesine "İzin Ver"** olarak ayarlanır.
> Sanal Ağ veya Güvenlik Duvarları kurallarını yaparken ***"varsayılan Eylem"i*** değiştirmemiz gerekir
> 
> Kaynak
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

Şablonu dağıtmak için Azure [Kaynak Yöneticisi][lnk-deploy]yönergelerini izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Etkinlik Hub'larına Azure sanal ağlarına erişimi zorlamak için aşağıdaki bağlantıya bakın:

- [Etkinlik Hub'ları için Sanal Ağ Hizmeti Bitiş Noktaları][lnk-vnet]

<!-- Links -->

[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[lnk-vnet]: event-hubs-service-endpoints.md
