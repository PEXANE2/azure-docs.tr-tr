---
title: Azure Hizmet Veri Servisi için sanal ağ hizmeti bitiş noktalarını yapılandırma
description: Bu makalede, sanal ağa Microsoft.ServiceBus hizmet bitiş noktası nasıl ekleyeceğiniz hakkında bilgi verilmektedir.
services: service-bus
documentationcenter: ''
author: axisc
editor: spelluru
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.date: 12/20/2019
ms.author: aschhab
ms.openlocfilehash: 9dbf65522d5c85e1054ed3f1f6ca9f86180e7f7d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79454990"
---
# <a name="configure-virtual-network-service-endpoints-for-azure-service-bus"></a>Azure Hizmet Veri Servisi için sanal ağ hizmeti bitiş noktalarını yapılandırma

Servis Veri Yolu'nun [Sanal Ağ (VNet) hizmet bitiş noktalarıyla][vnet-sep] tümleştirilmesi, sanal ağlara bağlı sanal makineler gibi iş yüklerinden mesajlaşma yeteneklerine güvenli erişim sağlar ve ağ trafik yolu her iki uçta da güvenli hale gelir.

En az bir sanal ağ altağ hizmeti bitiş noktasına bağlanacak şekilde yapılandırıldıktan sonra, ilgili Servis Veri Aracı ad alanı artık herhangi bir yerden değil, yetkili sanal ağ(lar) trafiğini kabul eder. Sanal ağ açısından bakıldığında, Hizmet Veri Sitesi ad alanını hizmet bitiş noktasına bağlama, sanal ağ alt ağından mesajlaşma hizmetine yalıtılmış bir ağ tünelini yapılandırır.

Sonuç, ileti hizmeti bitiş noktasının genel BIR IP aralığında olmasına rağmen, alt ağa bağlı iş yükleri ile ilgili Service Bus ad alanı arasındaki özel ve yalıtılmış bir ilişkidir.

> [!IMPORTANT]
> Sanal Ağlar yalnızca [Premium katman](service-bus-premium-messaging.md) Hizmet Veri Günü ad alanlarında desteklenir.
> 
> Servis Veri Servisi ile VNet hizmet bitiş noktalarını kullanırken, Standart ve Premium katman Hizmet Veri Mes'ü ad alanlarını karıştıran uygulamalarda bu uç noktaları etkinleştirmemelisiniz. Standart katman VNet'leri desteklemediği için. Bitiş noktası yalnızca Premium katman ad alanlarıyla sınırlıdır.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>VNet tümleştirmesi tarafından etkinleştirilen gelişmiş güvenlik senaryoları 

Sıkı ve bölümlere ayrılmış güvenlik gerektiren ve sanal ağ alt ağlarının bölümlere ayrılmış hizmetler arasındaki segmentasyonu sağladığı çözümler, genellikle bu bölmelerde bulunan hizmetler arasında genellikle iletişim yolları gerektirir.

TCP/IP üzerinden HTTPS taşıyanlar da dahil olmak üzere bölmeler arasındaki herhangi bir anlık IP rotası, ağ katmanındaki güvenlik açıklarından yararlanma riski taşır. Mesajlaşma hizmetleri, iletilerin taraflar arasında geçiş sırasında diske bile yazıldığı tamamen yalıtımlı iletişim yolları sağlar. Her ikisi de aynı Hizmet Veri Yolu örneğine bağlı olan iki farklı sanal ağdaki iş yükleri, iletiler aracılığıyla verimli ve güvenilir bir şekilde iletişim kurabilir ve ilgili ağ yalıtımsınır bütünlüğü korunur.
 
Bu, güvenliğe duyarlı bulut çözümlerinizin yalnızca Azure endüstri lideri güvenilir ve ölçeklenebilir eşzamanlı mesajlaşma özelliklerine erişmedikleri, aynı zamanda artık mesajlaşmayı güvenli çözüm bölmeleri arasında iletişim yolları oluşturmak için kullanabileceği anlamına gelir. HTTPS ve diğer TLS güvenlikli soket protokolleri de dahil olmak üzere eşler arası iletişim moduyla ulaşılabileceklerden daha güvenlidir.

## <a name="binding-service-bus-to-virtual-networks"></a>Hizmet Veri Tos'u Sanal Ağlara Bağlama

*Sanal ağ kuralları,* Azure Hizmet Veri Servisi sunucunuzun belirli bir sanal ağ alt ağından bağlantıları kabul edip etmediğini kontrol eden güvenlik duvarı güvenlik özelliğidir.

Hizmet Veri Sitesi ad alanını sanal ağa bağlamaiki aşamalı bir işlemdir. Öncelikle bir Sanal **Ağ** alt ağında bir Sanal Ağ hizmet bitiş noktası oluşturmanız ve hizmet bitiş noktasına genel [bakışta][vnet-sep]açıklandığı gibi **Microsoft.ServiceBus** için etkinleştirmeniz gerekir. Hizmet bitiş noktasını ekledikten sonra, Servis Veri Sitesi ad alanını **sanal ağ kuralıyla**ona bağlarsınız.

Sanal ağ kuralı, Hizmet Veri Mesle'nin ad alanının sanal ağ alt ağıyla ilişkisidir. Kural olsa da, alt ağa bağlı tüm iş yüklerine Hizmet Veri Yolundan ad alanına erişim izni verilir. Service Bus'un kendisi hiçbir zaman giden bağlantılar kurmaz, erişim sağlaması gerekmez ve bu nedenle bu kuralı etkinleştirerek alt ağınıza erişim izni verilmez.

## <a name="use-azure-portal"></a>Azure portalı kullanma
Bu bölümde, sanal ağ hizmeti bitiş noktası eklemek için Azure portalını nasıl kullanacağınızı gösterir. Erişimi sınırlamak için, bu Olay Hub'ları ad alanı için sanal ağ hizmeti bitiş noktasını tümleştirmeniz gerekir.

1. [Azure portalındaki](https://portal.azure.com) **Hizmet Veri Servisi ad alanınıza** gidin.
2. Sol menüde **Ağ seçeneği'ni** seçin. Varsayılan olarak, **Tüm ağlar** seçeneği seçilir. Ad alanınız herhangi bir IP adresinden bağlantıları kabul eder. Bu varsayılan ayar, 0.0.0.0/0 IP adresi aralığını kabul eden bir kurala eşdeğerdir. 

    ![Güvenlik Duvarı - Tüm ağlar seçeneği seçildi](./media/service-endpoints/firewall-all-networks-selected.png)
1. Sayfanın üst kısmında **seçili ağlar** seçeneğini seçin.
2. Sayfanın **Sanal Ağ** bölümünde **+Ekle'yi**seçin. 

    ![varolan sanal ağ ekleme](./media/service-endpoints/add-vnet-menu.png)
3. Sanal ağlar listesinden sanal ağı seçin ve ardından **alt ağı**seçin. Listeye sanal ağı eklemeden önce hizmet bitiş noktasını etkinleştirmeniz gerekir. Hizmet bitiş noktası etkin değilse, portal bunu etkinleştirmenizi ister.
   
   ![alt ağı seçin](./media/service-endpoints/select-subnet.png)

4. **Microsoft.ServiceBus**için alt ağ için hizmet bitiş noktası etkinleştirildikten sonra aşağıdaki başarılı iletiyi görmeniz gerekir. Ağı eklemek için sayfanın en altında **Ekle'yi** seçin. 

    ![alt ağı seçin ve bitiş noktasını etkinleştirin](./media/service-endpoints/subnet-service-endpoint-enabled.png)

    > [!NOTE]
    > Hizmet bitiş noktasını etkinleştiremiyorsanız, Kaynak Yöneticisi şablonu kullanarak eksik sanal ağ hizmeti bitiş noktasını yok sayabilirsiniz. Bu işlevsellik portalda kullanılamaz.
6. Ayarları kaydetmek için araç çubuğunda **Kaydet'i** seçin. Onayın portal bildirimlerinde gösterilmesi için birkaç dakika bekleyin. **Kaydet** düğmesi devre dışı bırakılmalı. 

    ![Ağı kaydet](./media/service-endpoints/save-vnet.png)

## <a name="use-resource-manager-template"></a>Resource Manager şablonu kullanma
Aşağıdaki Kaynak Yöneticisi şablonu, varolan bir Hizmet Veri Sitesi ad alanına sanal ağ kuralı eklemeyi sağlar.

Template parameters:

* **namespaceName**: Service Bus namespace.
* **virtualNetworkingSubnetId**: Sanal ağ alt ağı için tam nitelikli Kaynak Yöneticisi yolu; örneğin, `/subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` sanal bir ağın varsayılan alt ağı için.

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

Şablon:

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
      "namespaceNetworkRuleSetName": "[concat(parameters('servicebusNamespaceName'), concat('/', 'default'))]",
      "subNetId": "[resourceId('Microsoft.Network/virtualNetworks/subnets/', parameters('virtualNetworkName'), parameters('subnetName'))]"
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
                    "service": "Microsoft.ServiceBus"
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
        "type": "Microsoft.ServiceBus/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.ServiceBus/namespaces/', parameters('servicebusNamespaceName'))]"
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

Sanal ağlar hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

- [Azure sanal ağ hizmeti bitiş noktaları][vnet-sep]
- [Azure Servis Veri Servisi IP filtreleme][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[ip-filtering]: service-bus-ip-filtering.md
