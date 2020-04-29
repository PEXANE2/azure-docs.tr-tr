---
title: Sanal ağ hizmeti uç noktaları-Azure Event Hubs | Microsoft Docs
description: Bu makalede bir sanal ağa Microsoft. EventHub hizmet uç noktası ekleme hakkında bilgi sağlanır.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81390279"
---
# <a name="use-virtual-network-service-endpoints-with-azure-event-hubs"></a>Azure Event Hubs ile sanal ağ hizmet uç noktalarını kullanma

[Sanal ağ (VNet) hizmet uç noktaları][vnet-sep] ile Event Hubs tümleştirmesi, sanal ağlara bağlı sanal makineler gibi iş yüklerinden, her iki uçta da güvenli hale getirilen ağ trafiği yolu ile güvenli erişim sağlar.

En az bir sanal ağ alt ağ hizmeti uç noktasına bağlanacak şekilde yapılandırıldıktan sonra, ilgili Event Hubs ad alanı artık sanal ağlardaki her yerden trafiği kabul etmez. Sanal ağ perspektifinden bir Event Hubs ad alanını bir hizmet uç noktasına bağlamak, sanal ağ alt ağından mesajlaşma hizmetine yalıtılmış bir ağ tüneli yapılandırır. 

Sonuç olarak, alt ağa ve ilgili Event Hubs ad alanıyla ilişkili olan iş yükleri arasında özel ve yalıtılmış bir ilişki vardır. Bu, bir genel IP aralığında yer alan mesajlaşma hizmeti uç noktasının observable ağ adresi artma. Bu davranış için bir özel durum vardır. Hizmet uç noktasının etkinleştirilmesi, varsayılan olarak sanal ağla ilişkili `denyall` [IP güvenlik duvarında](event-hubs-ip-filtering.md) kuralı etkinleştirir. Olay Hub 'ı genel uç noktasına erişimi etkinleştirmek için IP güvenlik duvarında belirli IP adresleri ekleyebilirsiniz. 

>[!WARNING]
> Sanal ağlar tümleştirmesini uygulamak, diğer Azure hizmetlerinin Event Hubs etkileşimde olmasını engelleyebilir.
>
> Sanal ağlar uygulandığında güvenilen Microsoft Hizmetleri desteklenmez.
>
> Sanal ağlarla çalışmayan yaygın Azure senaryoları ( **listenin ayrıntılı olmadığına** unutmayın)-
> - Azure Izleyici (Tanılama ayarı)
> - Azure Stream Analytics
> - Azure Event Grid ile tümleştirme
> - Azure IoT Hub yolları
> - Azure IoT Device Explorer
>
> Aşağıdaki Microsoft hizmetlerinin bir sanal ağda olması gerekir
> - Azure Web Apps
> - Azure İşlevleri


> [!IMPORTANT]
> Sanal ağlar, Event Hubs **Standart** ve **adanmış** katmanlarında desteklenir. **Temel** katmanda desteklenmez.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>VNet tümleştirmesi tarafından etkinleştirilen gelişmiş güvenlik senaryoları 

Sıkı ve compartmenbir güvenlik gerektiren ve sanal ağ alt ağlarının, compartmençalıştıran hizmetler arasında segmentleme sağladığı çözümler, yine de bu bölmeleri bulunan hizmetler arasında iletişim yollarına gerek duyar.

TCP/IP üzerinden HTTPS 'yi yürüten bu dahil olmak üzere, bölmeleri arasındaki tüm anında IP rotası, üzerindeki ağ katmanından güvenlik açıklarına karşı yararlanma riskini taşır. Mesajlaşma Hizmetleri, iletilerin taraflar arasında geçiş yaparken bile diske yazıldığı, yalıtılmış iletişim yolları sağlar. Aynı Event Hubs örneğine bağlanan iki ayrı sanal ağdaki iş yükleri iletiler aracılığıyla verimli ve güvenilir bir şekilde iletişim kurabilir, ancak ilgili ağ yalıtımı sınır bütünlüğü korunur.
 
Bu, güvenlik duyarlı bulut çözümlerinizin yalnızca Azure sektör lideri güvenilir ve ölçeklenebilir zaman uyumsuz mesajlaşma özelliklerine erişim elde edemeyeceği anlamına gelir, ancak artık HTTPS ve diğer TLS güvenlikli yuva protokolleri de dahil olmak üzere herhangi bir eşler arası iletişim moduyla ulaşılabilir 'dan daha güvenli olan güvenli çözüm bölmeleri arasında iletişim yolları oluşturmak için mesajlaşma kullanabilir.

## <a name="bind-event-hubs-to-virtual-networks"></a>Olay Hub 'larını sanal ağlara bağlama

**Sanal ağ kuralları** , Azure Event Hubs ad uzayının belirli bir sanal ağ alt ağından gelen bağlantıları kabul edip etmediğini denetleyen güvenlik duvarı güvenlik özelliğidir.

Bir Event Hubs ad alanını bir sanal ağa bağlamak iki adımlı bir işlemdir. Önce bir sanal ağın alt ağında bir **sanal ağ hizmeti uç noktası** oluşturmanız ve bunu [hizmet uç noktası genel bakış][vnet-sep] makalesinde açıklandığı gibi **Microsoft. EventHub** için etkinleştirmeniz gerekir. Hizmet uç noktasını ekledikten sonra, Event Hubs ad alanını bir **sanal ağ kuralıyla**bağlayın.

Sanal ağ kuralı, bir sanal ağ alt ağıyla Event Hubs ad alanının bir ilişkidir. Kural var olsa da, alt ağa erişen tüm iş yükleri Event Hubs ad alanına erişim izni verilir. Event Hubs kendisi hiçbir şekilde giden bağlantı oluşturmaz, erişim elde etmek zorunda kalmaz ve bu nedenle bu kuralı etkinleştirerek alt ağınız için hiçbir şekilde erişim izni verilmemiş olur.

## <a name="use-azure-portal"></a>Azure portalı kullanma
Bu bölümde, bir sanal ağ hizmeti uç noktası eklemek için Azure portal nasıl kullanılacağı gösterilmektedir. Erişimi sınırlandırmak için, bu Event Hubs ad alanı için sanal ağ hizmet uç noktasını tümleştirmeniz gerekir.

1. [Azure portal](https://portal.azure.com) **Event Hubs ad alanına** gidin.
2. Sol taraftaki menüden **ağ** seçeneği ' ni seçin. **Tüm ağlar** seçeneğini belirlerseniz, Olay Hub 'ı HERHANGI bir IP adresinden gelen bağlantıları kabul eder. Bu ayar 0.0.0.0/0 IP adresi aralığını kabul eden bir kuralla eşdeğerdir. 

    ![Güvenlik Duvarı-tüm ağlar seçeneği seçildi](./media/event-hubs-firewall/firewall-all-networks-selected.png)
1. Belirli ağlara erişimi yeniden bağlamak için sayfanın üst kısmındaki **Seçili ağlar** seçeneğini belirleyin.
2. Sayfanın **sanal ağ** bölümünde, * * + var olan sanal ağı ekle * * * seçeneğini belirleyin. Yeni bir VNet oluşturmak istiyorsanız **+ Yeni sanal ağ oluştur** ' u seçin. 

    ![var olan sanal ağı ekle](./media/event-hubs-tutorial-vnet-and-firewalls/add-vnet-menu.png)
3. Sanal ağlar listesinden sanal ağı seçin ve ardından **alt ağı**seçin. Sanal ağı listeye eklemeden önce hizmet uç noktasını etkinleştirmeniz gerekir. Hizmet uç noktası etkinleştirilmemişse, Portal bunu etkinleştirmenizi ister.
   
   ![alt ağ seçin](./media/event-hubs-tutorial-vnet-and-firewalls/select-subnet.png)

4. Alt ağ için hizmet uç noktası **Microsoft. EventHub**için etkinleştirildikten sonra aşağıdaki başarılı iletiyi görmeniz gerekir. Ağı eklemek için sayfanın alt kısmındaki **Ekle** ' yi seçin. 

    ![alt ağ seçin ve uç noktayı etkinleştirin](./media/event-hubs-tutorial-vnet-and-firewalls/subnet-service-endpoint-enabled.png)

    > [!NOTE]
    > Hizmet uç noktasını etkinleştiremeyebilirsiniz, Kaynak Yöneticisi şablonunu kullanarak eksik sanal ağ hizmeti uç noktasını yoksayabilirsiniz. Portalda bu işlev kullanılamaz.
6. Ayarları kaydetmek için araç çubuğunda **Kaydet** ' i seçin. Onayın Portal bildirimlerinde gösterilmesi için birkaç dakika bekleyin.

    ![Ağı kaydet](./media/event-hubs-tutorial-vnet-and-firewalls/save-vnet.png)


## <a name="use-resource-manager-template"></a>Resource Manager şablonu kullanma

Aşağıdaki Kaynak Yöneticisi şablonu, var olan bir Event Hubs ad alanına bir sanal ağ kuralı eklenmesini sağlar.

Şablon parametreleri:

* **NamespaceName**: Event Hubs ad alanı.
* **Vnetrutaname**: oluşturulacak sanal ağ kuralının adı.
* **Virtualnetworkingsubnetıd**: sanal ağ alt ağı için tam olarak nitelenmiş Kaynak Yöneticisi yolu; Örneğin, `/subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` bir sanal ağın varsayılan alt ağı için.

> [!NOTE]
> Mümkün olan reddetme kuralları olmadığı sürece, Azure Resource Manager şablonu, bağlantıları kısıtlayameyen **"Izin ver"** olarak ayarlanmış varsayılan eylemi içerir.
> Sanal ağ veya güvenlik duvarları kuralları yaparken, ***"DefaultAction"*** öğesini değiştirmemiz gerekir
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

Şablonu dağıtmak için [Azure Resource Manager][lnk-deploy]talimatlarını izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Sanal ağlar hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

- [Azure sanal ağ hizmet uç noktaları][vnet-sep]
- [Azure Event Hubs IP filtreleme][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[ip-filtering]: event-hubs-ip-filtering.md
