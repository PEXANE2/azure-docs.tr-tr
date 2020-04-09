---
title: Azure Hizmet Veri Servisi veri hizmetleri için IP güvenlik duvarı kurallarını yapılandırma
description: Belirli IP adreslerinden Azure Hizmet Veri Servisi'ne bağlantılara izin vermek için Güvenlik Duvarı Kuralları nasıl kullanılır?
services: service-bus
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus
ms.devlang: na
ms.topic: article
ms.date: 12/20/2019
ms.author: aschhab
ms.openlocfilehash: 24591c20ed707d9541eece0698ecd6e6b5ddee35
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878196"
---
# <a name="configure-ip-firewall-rules-for-azure-service-bus"></a>Azure Hizmet Veri Servisi veri hizmetleri için IP güvenlik duvarı kurallarını yapılandırma
Varsayılan olarak, Hizmet Veri Servisi ad alanlarına, istek geçerli kimlik doğrulama ve yetkilendirmeyle birlikte geldiği sürece internetten erişilebilir. IP güvenlik duvarı ile, [cidr (Classless Etki Alanı Yönlendirmesi)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösteriminde yalnızca bir iPv4 adresi kümesi veya IPv4 adres aralıklarıyla daha da kısıtlayabilirsiniz.

Bu özellik, Azure Servis Veri Tos'unyalnızca belirli tanınmış sitelerden erişilmesi gereken senaryolarda yararlıdır. Güvenlik duvarı kuralları, belirli IPv4 adreslerinden kaynaklanan trafiği kabul etmek için kuralları yapılandırmanızı sağlar. Örneğin, [Azure Ekspres Rotası][express-route]ile Hizmet Veri Yolu kullanıyorsanız, yalnızca şirket içi altyapı IP adreslerinizden veya kurumsal bir NAT ağ geçidinin adreslerinden gelen trafiğe izin vermek için bir güvenlik duvarı **kuralı** oluşturabilirsiniz. 

> [!IMPORTANT]
> Güvenlik duvarları ve Sanal Ağlar yalnızca Servis Veri Veri Tos'un **premium** katmanında desteklenir. **En üst** düzeye yükseltme bir seçenek değilse, Paylaşılan Erişim İmzası (SAS) belirteci güvenli tutmanızı ve yalnızca yetkili kullanıcılarla paylaşmanızı öneririz. SAS kimlik doğrulaması hakkında daha fazla bilgi için kimlik [doğrulama ve yetkilendirme](service-bus-authentication-and-authorization.md#shared-access-signature)ye bakın.

## <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları
IP güvenlik duvarı kuralları Hizmet Veri Servisi ad alanı düzeyinde uygulanır. Bu nedenle, kurallar desteklenen herhangi bir protokol kullanarak istemcilerden gelen tüm bağlantılar için geçerlidir. Hizmet Veri Gönderisi ad alanında izin verilen bir IP kuralıyla eşleşmeyen bir IP adresinden gelen bağlantı girişimi yetkisiz olarak reddedilir. Yanıtta IP kuralından bahsedilmez. Sırayla IP filtresi kuralları uygulanır ve IP adresiyle eşleşen ilk kural kabul veya reddetme eylemini belirler.

## <a name="use-azure-portal"></a>Azure portalı kullanma
Bu bölümde, Hizmet Veri Aracı ad alanı için IP güvenlik duvarı kuralları oluşturmak için Azure portalının nasıl kullanılacağı gösterilmektedir. 

1. [Azure portalındaki](https://portal.azure.com) **Hizmet Veri Servisi ad alanınıza** gidin.
2. Sol menüde **Ağ seçeneği'ni** seçin. Varsayılan olarak, **Tüm ağlar** seçeneği seçilir. Servis Veri Gönderi ad alanınız herhangi bir IP adresinden bağlantıları kabul eder. Bu varsayılan ayar, 0.0.0.0/0 IP adresi aralığını kabul eden bir kurala eşdeğerdir. 

    ![Güvenlik Duvarı - Tüm ağlar seçeneği seçildi](./media/service-bus-ip-filtering/firewall-all-networks-selected.png)
1. Sayfanın üst kısmında **Seçili Ağlar** seçeneğini seçin. Güvenlik **Duvarı** bölümünde aşağıdaki adımları izleyin:
    1. Geçerli istemcinize ad alanına erişim sağlamak için **istemci IP adresinizi ekle** seçeneğini seçin. 
    2. **Adres aralığı**için CIDR gösterimine belirli bir IPv4 adresi veya bir dizi IPv4 adresi girin. 
    3. **Güvenilen Microsoft hizmetlerinin bu güvenlik duvarLarını atlayıp atlamayacağına izin**vermek isteyip istemediğinizi belirtin. 

        ![Güvenlik Duvarı - Tüm ağlar seçeneği seçildi](./media/service-bus-ip-filtering/firewall-selected-networks-trusted-access-disabled.png)
3. Ayarları kaydetmek için araç çubuğunda **Kaydet'i** seçin. Onayın portal bildirimlerinde gösterilmesi için birkaç dakika bekleyin.

## <a name="use-resource-manager-template"></a>Resource Manager şablonu kullanma
Bu bölümde, sanal ağ ve güvenlik duvarı kuralı oluşturan örnek bir Azure Kaynak Yöneticisi şablonu vardır.


Aşağıdaki Kaynak Yöneticisi şablonu, varolan bir Hizmet Veri Sitesi ad alanına sanal ağ kuralı eklemeyi sağlar.

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
      "servicebusNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Service Bus namespace"
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
      "namespaceNetworkRuleSetName": "[concat(parameters('servicebusNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('servicebusNamespaceName')]",
        "type": "Microsoft.ServiceBus/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Premium",
          "tier": "Premium"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.ServiceBus/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.ServiceBus/namespaces/', parameters('servicebusNamespaceName'))]"
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

Hizmet Veri Servisi'nden Azure sanal ağlarına erişimi zorlamak için aşağıdaki bağlantıya bakın:

- [Servis Veri Servisi için Sanal Ağ Hizmeti Bitiş Noktaları][lnk-vnet]

<!-- Links -->

[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[lnk-vnet]: service-bus-service-endpoints.md
[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
