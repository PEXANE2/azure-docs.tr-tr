---
title: Azure Service Bus için IP güvenlik duvarı kurallarını yapılandırma
description: Belirli IP adreslerinden Azure Service Bus bağlantılara izin vermek için güvenlik duvarı kuralları kullanma.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 699ece2e78ff0605ff4076b09c023d14e289b1f7
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88064647"
---
# <a name="allow-access-to-azure-service-bus-namespace-from-specific-ip-addresses-or-ranges"></a>Belirli IP adreslerinden veya aralıklardan Azure Service Bus ad alanına erişime izin ver
Varsayılan olarak, istek geçerli kimlik doğrulaması ve yetkilendirmeyle geldiği sürece, Service Bus ad alanlarına internet 'ten erişilebilir. IP güvenlik duvarı ile, [CIDR (sınıfsız etki alanları arası yönlendirme)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösteriminde yalnızca bir dizi IPv4 adresi veya IPv4 adres aralığı ile sınırlayabilirsiniz.

Bu özellik, Azure Service Bus yalnızca belirli bilinen sitelerden erişilebilir olması gereken senaryolarda yararlıdır. Güvenlik duvarı kuralları, belirli IPv4 adreslerinden kaynaklanan trafiği kabul etmek için kuralları yapılandırmanızı sağlar. Örneğin, [Azure Express Route][express-route]ile Service Bus kullanıyorsanız, yalnızca şirket ıçı altyapı IP adreslerinizin veya BIR kurumsal NAT ağ geçidinin adreslerinden gelen trafiğe izin vermek için bir **güvenlik duvarı kuralı** oluşturabilirsiniz. 

> [!IMPORTANT]
> Güvenlik duvarları ve sanal ağlar yalnızca Service Bus **Premium** katmanında desteklenir. **Premier** katmanına yükseltme bir seçenek değilse, paylaşılan erişim IMZASı (SAS) belirtecini güvenli halde tutmanızı ve yalnızca yetkili kullanıcılarla paylaşmanızı öneririz. SAS kimlik doğrulaması hakkında daha fazla bilgi için bkz. [kimlik doğrulama ve yetkilendirme](service-bus-authentication-and-authorization.md#shared-access-signature).

## <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları
IP güvenlik duvarı kuralları Service Bus ad alanı düzeyinde uygulanır. Bu nedenle, kurallar desteklenen herhangi bir protokolü kullanarak istemcilerden gelen tüm bağlantılara uygulanır. Service Bus ad alanındaki izin verilen bir IP kuralıyla eşleşmeyen bir IP adresinden gelen bağlantı girişimleri yetkisiz olarak reddedilir. Yanıt, IP kuralından bahsetmiyor. IP filtresi kuralları sırasıyla uygulanır ve IP adresiyle eşleşen ilk kural kabul etme veya reddetme eylemini belirler.

>[!WARNING]
> Güvenlik duvarı kuralları uygulamak, diğer Azure hizmetlerinin Service Bus etkileşimde olmasını engelleyebilir.
>
> IP filtrelemesi (güvenlik duvarı kuralları) uygulandığında güvenilir Microsoft Hizmetleri desteklenmez ve yakında kullanıma sunulacaktır.
>
> IP filtrelemesi ile çalışmayan yaygın Azure senaryoları ( **listenin ayrıntılı olmadığına** unutmayın)-
> - Azure Event Grid ile tümleştirme
> - Azure IoT Hub yolları
> - Azure IoT Device Explorer
>
> Aşağıdaki Microsoft hizmetlerinin bir sanal ağda olması gerekir
> - Azure App Service
> - Azure İşlevleri
> - Azure Izleyici (Tanılama ayarı)

## <a name="use-azure-portal"></a>Azure portalı kullanma
Bu bölümde, bir Service Bus ad alanı için IP güvenlik duvarı kuralları oluşturmak üzere Azure portal nasıl kullanılacağı gösterilmektedir. 

1. [Azure portal](https://portal.azure.com) **Service Bus ad alanına** gidin.
2. Sol taraftaki menüde, **Ayarlar**altında **ağ** seçeneği ' ni seçin.  

    > [!NOTE]
    > **Ağ** sekmesini yalnızca **Premium** ad alanları için görürsünüz.  
    
    Varsayılan olarak, **Seçili ağlar** seçeneği seçilidir. Bu sayfada en az bir IP güvenlik duvarı kuralı veya bir sanal ağ eklememeniz durumunda, ad alanına genel İnternet üzerinden erişilebilir (erişim anahtarı kullanılarak).

    :::image type="content" source="./media/service-bus-ip-filtering/default-networking-page.png" alt-text="Ağ sayfası-varsayılan" lightbox="./media/service-bus-ip-filtering/default-networking-page.png":::
    
    **Tüm ağlar** seçeneğini belirlerseniz, Service Bus ad alanınız HERHANGI bir IP adresinden gelen bağlantıları kabul eder. Bu varsayılan ayar 0.0.0.0/0 IP adresi aralığını kabul eden bir kuralla eşdeğerdir. 

    ![Güvenlik Duvarı-tüm ağlar seçeneği seçildi](./media/service-bus-ip-filtering/firewall-all-networks-selected.png)
1. Yalnızca belirtilen IP adresinden erişime izin vermek için, henüz seçili değilse **Seçili ağlar** seçeneğini belirleyin. **Güvenlik duvarı** bölümünde şu adımları izleyin:
    1. Geçerli istemci IP 'nize ad alanına erişim sağlamak için **ISTEMCI IP adresi ekle** seçeneğini belirleyin. 
    2. **Adres aralığı**IÇIN, CIDR gösteriminde belirli bir IPv4 adresi veya bir IPv4 adresi aralığı girin. 
    3. **Güvenilen Microsoft hizmetlerinin bu güvenlik duvarını atlamasına izin**vermek isteyip istemediğinizi belirtin. 

        > [!WARNING]
        > **Seçili ağlar** seçeneğini BELIRLEYIP bir IP adresi veya adres aralığı belirtmezseniz, hizmet tüm ağlardan gelen trafiğe izin verir. 

        ![Güvenlik Duvarı-tüm ağlar seçeneği seçildi](./media/service-bus-ip-filtering/firewall-selected-networks-trusted-access-disabled.png)
3. Ayarları kaydetmek için araç çubuğunda **Kaydet** ' i seçin. Onayın Portal bildirimlerinde gösterilmesi için birkaç dakika bekleyin.

    > [!NOTE]
    > Belirli sanal ağlara erişimi kısıtlamak için bkz. [belirli ağlardan erişime Izin ver](service-bus-service-endpoints.md).

## <a name="use-resource-manager-template"></a>Resource Manager şablonu kullanma
Bu bölümde, bir sanal ağ ve bir güvenlik duvarı kuralı oluşturan örnek bir Azure Resource Manager şablonu bulunur.


Aşağıdaki Kaynak Yöneticisi şablonu, var olan bir Service Bus ad alanına bir sanal ağ kuralı eklenmesini sağlar.

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

Şablonu dağıtmak için [Azure Resource Manager][lnk-deploy]talimatlarını izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure sanal ağlarına Service Bus erişimi kısıtlayan için şu bağlantıya bakın:

- [Service Bus için sanal ağ hizmeti uç noktaları][lnk-vnet]

<!-- Links -->

[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[lnk-vnet]: service-bus-service-endpoints.md
[express-route]:  ../expressroute/expressroute-faqs.md#supported-services