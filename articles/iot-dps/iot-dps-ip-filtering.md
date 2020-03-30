---
title: Azure IoT DPS IP bağlantı filtreleri | Microsoft Dokümanlar
description: Belirli IP adreslerinden Azure IoT DPS örneğinize kadar bağlantıları engellemek için IP filtreleme nasıl kullanılır? Bağlantıları tek tek veya IP adresleri aralıklarından engelleyebilirsiniz.
author: wesmc7777
ms.author: wesmc
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: 0b13040b39ed491ec4fee4d6922d41f086edeeb2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284921"
---
# <a name="use-ip-filters"></a>IP filtrelerini kullanma

Güvenlik herhangi bir IoT çözümünün önemli bir yönüdür. Bazen, güvenlik yapılandırmanızın bir parçası olarak hangi aygıtların bağlanabileceği IP adreslerini açıkça belirtmeniz gerekir. Azure IoT Hub Aygıt Sağlama Hizmeti 'nin (DPS) *IP filtresi* özelliği, belirli IPv4 adreslerinden gelen trafiği reddetme veya kabul etme kurallarını yapılandırmanıza olanak tanır.

## <a name="when-to-use"></a>Kullanılması gereken durumlar

Belirli IP adreslerinden DPS bitiş noktasına bağlantılarını engellemenin yararlı olduğu iki özel kullanım noktası vardır:

* DPS'niz yalnızca belirli bir IP adresi aralığından trafik almalı ve diğer her şeyi reddetmelidir. Örneğin, BIR DPS ile aygıtlarınız arasında özel bağlantılar oluşturmak için DPS'nizi [Azure Ekspres Rotası](https://azure.microsoft.com/documentation/articles/expressroute-faqs/#supported-services) ile kullanıyorsunuz.

* DPS yöneticisi tarafından şüpheli olarak tanımlanan IP adreslerinden gelen trafiği reddetmeniz gerekir.

## <a name="how-filter-rules-are-applied"></a>Filtre kuralları nasıl uygulanır?

IP filtresi kuralları DPS örnek düzeyinde uygulanır. Bu nedenle IP filtresi kuralları, desteklenen protokolleri kullanarak aygıtlardan ve arka uç uygulamalardan gelen tüm bağlantılar için geçerlidir.

DPS örneğinizdeki reddeden IP kuralıyla eşleşen bir IP adresinden gelen herhangi bir bağlantı girişimi yetkisiz bir 401 durum kodu ve açıklaması alır. Yanıt iletisi IP kuralından bahsetmez.

## <a name="default-setting"></a>Varsayılan ayar

Varsayılan olarak, DPS portalındaki **IP Filtresi** ızgarası boştur. Bu varsayılan ayar, DPS'nizin herhangi bir IP adresinden bağlantıları kabul ettiği anlamına gelir. Bu varsayılan ayar, 0.0.0.0/0 IP adresi aralığını kabul eden bir kurala eşdeğerdir.

![IoT DPS varsayılan IP filtre ayarları](./media/iot-dps-ip-filtering/ip-filter-default.png)

## <a name="add-or-edit-an-ip-filter-rule"></a>IP filtresi kuralı ekleme veya yönetme

IP filtre kuralı eklemek için **+ IP Filtresi Kuralı Ekle'yi**seçin.

![IoT DPS'ye IP filtresi kuralı ekleme](./media/iot-dps-ip-filtering/ip-filter-add-rule.png)

**IP Filtre Kuralı Ekle'yi**seçtikten sonra alanları doldurun.

![IP Filtresi ekle kuralını seçtikten sonra](./media/iot-dps-ip-filtering/ip-filter-after-selecting-add.png)

* IP Filtresi kuralı için bir **ad** sağlayın. Bu, 128 karakter uzunluğunda, benzersiz, büyük/küçük harf duyarlı, alfasayısal bir dize olmalıdır. Yalnızca ASCII 7-bit alfasayısal karakterler `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` artı kabul edilir.

* CIDR gösteriminde tek bir IPv4 adresi veya IP adresi bloğu sağlayın. Örneğin, CIDR notasyonu 192.168.100.0/22 192.168.100.0 192.168.103.255 1024 IPv4 adreslerini temsil eder.

* IP filtresi kuralıiçin **eylem** olarak **İzin Ver** veya **Engelle'yi** seçin.

Alanları doldurduktan sonra kuralı kaydetmek için **Kaydet'i** seçin. Güncelleştirmenin devam ettiğini bildiren bir uyarı görürsünüz.

![IP filtresi kuralını kaydetme hakkında bildirim](./media/iot-dps-ip-filtering/ip-filter-save-new-rule.png)

En fazla 10 IP filtre kuralına ulaştığınızda **Ekle** seçeneği devre dışı bırakılır.

Varolan bir kuralı değiştirmek için, değiştirmek istediğiniz verileri seçin, değişikliği yapın ve ardından da değişikliği kaydetmek için **Kaydet'i** seçin.

> [!NOTE]
> IP adreslerini reddetmek, diğer Azure Hizmetlerinin DPS örneğiyle etkileşimkurmasını engelleyebilir.

## <a name="delete-an-ip-filter-rule"></a>IP filtresi kuralını silme

IP filtresi kuralını silmek için, o satırdaki çöp kutusu simgesini seçin ve ardından **Kaydet'i**seçin. Kural kaldırılır ve değişiklik kaydedilir.

![IoT DPS IP filtre kuralını silme](./media/iot-dps-ip-filtering/ip-filter-delete-rule.png)


## <a name="update-ip-filter-rules-in-code"></a>IP filtresi kurallarını kodda güncelleştirme

Azure kaynak sağlayıcısının REST bitiş noktasını kullanarak DPS IP filtrenizi alabilir ve değiştirebilirsiniz. `properties.ipFilterRules` [Createorupdate yöntemine](https://docs.microsoft.com/rest/api/iot-dps/iotdpsresource/createorupdate)bakın.

DPS IP filtre kurallarının güncelleştirilmesi şu anda Azure CLI veya Azure PowerShell ile desteklenmez, ancak Azure Kaynak Yöneticisi şablonlarıyla gerçekleştirilebilir. Bkz. [Azure Kaynak Yöneticisi şablonları](../azure-resource-manager/templates/overview.md) Kaynak Yöneticisi şablonlarını kullanma konusunda kılavuz için şablonlar. İzleyen şablon örnekleri DPS IP filtre kurallarının nasıl oluşturulup düzenlenip silineceklerini gösterir.

### <a name="add-an-ip-filter-rule"></a>IP filtresi kuralı ekleme

Aşağıdaki şablon örneği, tüm trafiği kabul eden "AllowAll" adlı yeni bir IP filtresi kuralı oluşturur.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", 
    "contentVersion": "1.0.0.0", 
    "parameters": {
        "iotDpsName": {
            "type": "string",
            "defaultValue": "[resourceGroup().name]",
            "minLength": 3,
            "metadata": {
                "description": "Specifies the name of the IoT DPS service."
            }
        }, 
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "Location for Iot DPS resource."
            }
        }        
    }, 
    "variables": {
        "iotDpsApiVersion": "2020-01-01"
    }, 
    "resources": [
        {
            "type": "Microsoft.Devices/provisioningServices",
            "apiVersion": "[variables('iotDpsApiVersion')]",
            "name": "[parameters('iotDpsName')]",
            "location": "[parameters('location')]",
            "sku": {
                "name": "S1",
                "tier": "Standard",
                "capacity": 1
            },
            "properties": {
                "IpFilterRules": [
                    {
                        "FilterName": "AllowAll",
                        "Action": "Accept",
                        "ipMask": "0.0.0.0/0"
                    }
                ]
            }            
        }
    ]
}
```

Gereksinimlerinize göre şablonun IP filtresi kural özniteliklerini güncelleştirin.

| Öznitelik                | Açıklama |
| ------------------------ | ----------- |
| **Filtername**           | IP Filtresi kuralı için bir ad sağlayın. Bu, 128 karakter uzunluğunda, benzersiz, büyük/küçük harf duyarlı, alfasayısal bir dize olmalıdır. Sadece ASCII 7-bit alfasayısal karakterler artı {'-', '', '',', ' '\''',,', '',',,',',',',',',',',',',',',',',',',',',',',',',',',',',',',''''}.',',''''''''''''''''}.',.',','''''''','''',','''''''','','','''''''''''''''','',','',',',',','.','.',',',',',',','','','',','',''''','','',',',',''','',',''''''},'','''''''','',''''''' |
| **Eylem**               | Kabul edilen değerler, IP filtresi kuralının eylemi olarak **Kabul veya**  **Reddet'tir.**  |
| **ipMask**               | CIDR gösteriminde tek bir IPv4 adresi veya IP adresi bloğu sağlayın. Örneğin, CIDR notasyonu 192.168.100.0/22 192.168.100.0 192.168.103.255 1024 IPv4 adreslerini temsil eder. |


### <a name="update-an-ip-filter-rule"></a>IP filtresi kuralını güncelleştirme

Aşağıdaki şablon örneği, tüm trafiği reddetmek için daha önce gösterilen "AllowAll" adlı IP filtresi kuralını güncelleştirir.

```json
{ 
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",  
    "contentVersion": "1.0.0.0",  
    "parameters": { 
        "iotDpsName": { 
            "type": "string", 
            "defaultValue": "[resourceGroup().name]", 
            "minLength": 3, 
            "metadata": { 
                "description": "Specifies the name of the IoT DPS service." 
            } 
        },  
        "location": { 
            "type": "string", 
            "defaultValue": "[resourceGroup().location]", 
            "metadata": { 
                "description": "Location for Iot DPS resource." 
            } 
        }        
    },  
    "variables": { 
        "iotDpsApiVersion": "2020-01-01" 
    },  
    "resources": [ 
        { 
            "type": "Microsoft.Devices/provisioningServices", 
            "apiVersion": "[variables('iotDpsApiVersion')]", 
            "name": "[parameters('iotDpsName')]", 
            "location": "[parameters('location')]", 
            "sku": { 
                "name": "S1", 
                "tier": "Standard", 
                "capacity": 1 
            }, 
            "properties": { 
                "IpFilterRules": [ 
                    { 
                        "FilterName": "AllowAll", 
                        "Action": "Reject", 
                        "ipMask": "0.0.0.0/0" 
                    } 
                ] 
            }             
        } 
    ] 
}
```

### <a name="delete-an-ip-filter-rule"></a>IP filtresi kuralını silme

Aşağıdaki şablon örneği DPS örneği için tüm IP filtresi kurallarını siler.

```json
{ 
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",  
    "contentVersion": "1.0.0.0",  
    "parameters": { 
        "iotDpsName": { 
            "type": "string", 
            "defaultValue": "[resourceGroup().name]", 
            "minLength": 3, 
            "metadata": { 
                "description": "Specifies the name of the IoT DPS service." 
            } 
        },  
        "location": { 
            "type": "string", 
            "defaultValue": "[resourceGroup().location]", 
            "metadata": { 
                "description": "Location for Iot DPS resource." 
            } 
        }        
    },  
    "variables": { 
        "iotDpsApiVersion": "2020-01-01" 
    },  
    "resources": [ 
        { 
            "type": "Microsoft.Devices/provisioningServices", 
            "apiVersion": "[variables('iotDpsApiVersion')]", 
            "name": "[parameters('iotDpsName')]", 
            "location": "[parameters('location')]", 
            "sku": { 
                "name": "S1", 
                "tier": "Standard", 
                "capacity": 1 
            }, 
            "properties": { 
            }             
        } 
    ] 
}
```



## <a name="ip-filter-rule-evaluation"></a>IP filtresi kuralı değerlendirmesi

Sırayla IP filtresi kuralları uygulanır ve IP adresiyle eşleşen ilk kural kabul veya reddetme eylemini belirler.

Örneğin, 192.168.100.0/22 aralığındaki adresleri kabul etmek ve diğer her şeyi reddetmek istiyorsanız, kılavuzdaki ilk kural 192.168.100.0/22 adres aralığını kabul etmelidir. Sonraki kural 0.0.0.0/0 aralığını kullanarak tüm adresleri reddetmelidir.

Bir satırın başındaki üç dikey noktayı tıklatarak ve sürükle ve bırak'ı kullanarak ızgaradaki IP filtre kurallarınızın sırasını değiştirebilirsiniz.

Yeni IP filtresi kural sıranızı kaydetmek için **Kaydet'i**tıklatın.

![DPS IP filtre kurallarınızın sırasını değiştirme](./media/iot-dps-ip-filtering/ip-filter-rule-order.png)

## <a name="next-steps"></a>Sonraki adımlar

DpS'yi yönetmek için bkz:

* [IoT DPS IP adreslerini anlama](iot-dps-understand-ip-address.md)
* [Azure CLI'yi kullanarak DPS'yi yapılandırma](how-to-manage-dps-with-cli.md)
* [DPS'ye erişimi kontrol etme](how-to-control-access.md)
