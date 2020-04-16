---
title: Sanal Ağ hizmeti bitiş noktaları - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Bu makalede, sanal ağa Microsoft.EventHub hizmet bitiş noktası nasıl ekleyeceğiniz hakkında bilgi verilmektedir.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 11/26/2019
ms.author: shvija
ms.openlocfilehash: 91b08d6130da640adc28a3b7d85bd33f0e876caf
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81390279"
---
# <a name="use-virtual-network-service-endpoints-with-azure-event-hubs"></a>Azure Etkinlik Hub'ları ile Sanal Ağ hizmeti uç noktalarını kullanma

Olay Hub'larının [Sanal Ağ (VNet) Hizmet Bitiş Noktaları][vnet-sep] ile tümleştirilmesi, sanal ağlara bağlı sanal makineler gibi iş yüklerinden mesajlaşma yeteneklerine güvenli erişim sağlar ve ağ trafik yolu her iki uçta da güvenli hale gelir.

En az bir sanal ağ alt ağ hizmeti bitiş noktasına bağlı olarak yapılandırıldıktan sonra, ilgili Olay Hub'ları ad alanı artık sanal ağlarda her yerden gelen trafiği kabul etmez. Sanal ağ açısından bakıldığında, Olay Hub'larının ad alanını hizmet bitiş noktasına bağlama, sanal ağ alt ağından ileti hizmetine yalıtılmış bir ağ tünelini yapılandırır. 

Sonuç, ileti hizmeti bitiş noktasının genel BIR IP aralığında olmasına rağmen, alt ağa ve ilgili Olay Hub'larına bağlı iş yükleri arasındaki özel ve yalıtılmış bir ilişkidir. Bu davranışın bir istisnası vardır. Varsayılan olarak bir hizmet bitiş noktasıetkinleştirmek, sanal ağile ilişkili [IP güvenlik duvarındaki](event-hubs-ip-filtering.md) `denyall` kuralı etkinleştirir. Olay Hub ortak bitiş noktasına erişimi etkinleştirmek için IP güvenlik duvarına belirli IP adresleri ekleyebilirsiniz. 

>[!WARNING]
> Sanal Ağlar tümleştirmesi uygulamak, diğer Azure hizmetlerinin Etkinlik Hub'larıyla etkileşimkurmasını engelleyebilir.
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


> [!IMPORTANT]
> Sanal ağlar, Etkinlik Hub'larının **standart** ve **özel** katmanlarında desteklenir. **Temel** katmanda desteklenmiyor.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>VNet tümleştirmesi tarafından etkinleştirilen gelişmiş güvenlik senaryoları 

Sıkı ve bölümlere ayrılmış güvenlik gerektiren ve sanal ağ alt ağlarının bölümlere ayrılmış hizmetler arasındaki segmentasyonu sağladığı çözümler, yine de bu bölmelerde bulunan hizmetler arasında iletişim yolları gerektirir.

TCP/IP üzerinden HTTPS taşıyanlar da dahil olmak üzere bölmeler arasındaki herhangi bir anlık IP rotası, ağ katmanındaki güvenlik açıklarından yararlanma riski taşır. Mesajlaşma hizmetleri, iletilerin taraflar arasında geçiş sırasında diske bile yazıldığı yalıtımlı iletişim yolları sağlar. Her ikisi de aynı Olay Hub'ları örneğine bağlı olan iki farklı sanal ağdaki iş yükleri, iletiler aracılığıyla verimli ve güvenilir bir şekilde iletişim kurabilir ve ilgili ağ yalıtım sınır bütünlüğü korunur.
 
Bu, güvenlik duyarlı bulut çözümlerinizin yalnızca Azure endüstri lideri güvenilir ve ölçeklenebilir eşzamanlı mesajlaşma özelliklerine erişmekle birlikte, artık https ve diğer TLS güvenlikli soket protokolleri de dahil olmak üzere eşler arası iletişim modlarında ulaşılabileceklerden daha güvenli güvenli çözüm bölmeleri arasında iletişim yolları oluşturmak için mesajlaşmayı kullanabileceği anlamına gelir.

## <a name="bind-event-hubs-to-virtual-networks"></a>Olay hub'larını sanal ağlara bağlama

**Sanal ağ kuralları,** Azure Etkinlik Hub'larınızın belirli bir sanal ağ alt ağındaki bağlantıları kabul edip etmediğini kontrol eden güvenlik duvarı güvenlik özelliğidir.

Olay Hub'larının ad alanını sanal ağa bağlamaiki adımlı bir işlemdir. Öncelikle bir sanal ağın alt ağında sanal bir **Ağ hizmeti bitiş noktası** oluşturmanız ve hizmet bitiş noktasına genel [bakış][vnet-sep] makalesinde açıklandığı gibi **Microsoft.EventHub** için etkinleştirmeniz gerekir. Hizmet bitiş noktasını ekledikten sonra, Olay Hub'ları ad alanını **sanal ağ kuralıyla**ona bağlarsınız.

Sanal ağ kuralı, Olay Hub'larının ad alanının sanal ağ alt ağıyla ilişkisidir. Kural mevcut olsa da, alt ağa bağlı tüm iş yüklerine Olay Hub'ları ad alanına erişim hakkı verilir. Olay Hub'larının kendisi hiçbir zaman giden bağlantılar kurmaz, erişim sağlaması gerekmez ve bu nedenle bu kuralı etkinleştirerek alt ağınıza erişim izni verilmez.

## <a name="use-azure-portal"></a>Azure portalı kullanma
Bu bölümde, sanal ağ hizmeti bitiş noktası eklemek için Azure portalını nasıl kullanacağınızı gösterir. Erişimi sınırlamak için, bu Olay Hub'ları ad alanı için sanal ağ hizmeti bitiş noktasını tümleştirmeniz gerekir.

1. [Azure portalındaki](https://portal.azure.com) **Etkinlik Hub'larınız ad alanına** gidin.
2. Sol menüde **Ağ seçeneği'ni** seçin. **Tüm ağlar** seçeneğini seçerseniz, olay merkezi herhangi bir IP adresinden bağlantıları kabul eder. Bu ayar, 0.0.0.0/0 IP adresi aralığını kabul eden bir kurala eşdeğerdir. 

    ![Güvenlik Duvarı - Tüm ağlar seçeneği seçildi](./media/event-hubs-firewall/firewall-all-networks-selected.png)
1. Belirli ağlara erişimi geri almak için sayfanın üst kısmındaki **Seçili Ağlar** seçeneğini seçin.
2. Sayfanın **Sanal Ağ** bölümünde **+Varolan sanal ağı ekle***'yi seçin. Yeni bir VNet oluşturmak istiyorsanız **+ Yeni sanal ağ** oluşturun' seçeneğini belirleyin. 

    ![varolan sanal ağ ekleme](./media/event-hubs-tutorial-vnet-and-firewalls/add-vnet-menu.png)
3. Sanal ağlar listesinden sanal ağı seçin ve ardından **alt ağı**seçin. Listeye sanal ağı eklemeden önce hizmet bitiş noktasını etkinleştirmeniz gerekir. Hizmet bitiş noktası etkin değilse, portal bunu etkinleştirmenizi ister.
   
   ![alt ağı seçin](./media/event-hubs-tutorial-vnet-and-firewalls/select-subnet.png)

4. **Microsoft.EventHub**için alt ağ için hizmet bitiş noktası etkinleştirildikten sonra aşağıdaki başarılı iletiyi görmeniz gerekir. Ağı eklemek için sayfanın en altında **Ekle'yi** seçin. 

    ![alt ağı seçin ve bitiş noktasını etkinleştirin](./media/event-hubs-tutorial-vnet-and-firewalls/subnet-service-endpoint-enabled.png)

    > [!NOTE]
    > Hizmet bitiş noktasını etkinleştiremiyorsanız, Kaynak Yöneticisi şablonu kullanarak eksik sanal ağ hizmeti bitiş noktasını yok sayabilirsiniz. Bu işlevsellik portalda kullanılamaz.
6. Ayarları kaydetmek için araç çubuğunda **Kaydet'i** seçin. Onayın portal bildirimlerinde gösterilmesi için birkaç dakika bekleyin.

    ![Ağı kaydet](./media/event-hubs-tutorial-vnet-and-firewalls/save-vnet.png)


## <a name="use-resource-manager-template"></a>Resource Manager şablonu kullanma

Aşağıdaki Kaynak Yöneticisi şablonu, varolan bir Olay Hub'ları ad alanına sanal ağ kuralı eklemeyi sağlar.

Şablon parametreleri:

* **namespaceName**: Olay Hub'ları ad alanı.
* **vnetRuleName**: Oluşturulacak Sanal Ağ kuralının adı.
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
- [Azure Etkinlik Hub'ları IP filtreleme][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[ip-filtering]: event-hubs-ip-filtering.md
