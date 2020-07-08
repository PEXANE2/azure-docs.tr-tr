---
title: Azure IoT DPS IP bağlantısı filtreleri | Microsoft Docs
description: Belirli IP adreslerinden Azure IoT DPS örneğinize bağlantıları engellemek için IP filtrelemeyi kullanma. Tek tek veya IP adresi aralıklarından bağlantıları engelleyebilirsiniz.
author: wesmc7777
ms.author: wesmc
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: 0b13040b39ed491ec4fee4d6922d41f086edeeb2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79284921"
---
# <a name="use-ip-filters"></a>IP filtrelerini kullanma

Güvenlik, tüm IoT çözümlerinin önemli bir yönüdür. Bazen, güvenlik yapılandırmanızın bir parçası olarak cihazların bağlanabileceği IP adreslerini açıkça belirtmeniz gerekir. Azure IoT Hub cihaz sağlama hizmeti 'nin (DPS) *IP filtresi* özelliği, belirli IPv4 adreslerinden gelen trafiği reddetme veya kabul etme kurallarını yapılandırmanızı sağlar.

## <a name="when-to-use"></a>Kullanılması gereken durumlar

Belirli IP adreslerinden bir DPS uç noktasına bağlantıları engellemek için yararlı olan iki özel kullanım durumu vardır:

* DPS, yalnızca belirli bir IP adresi aralığından trafik almalıdır ve diğer her şeyi reddedebilir. Örneğin, DPS ile cihazlarınız arasında özel bağlantılar oluşturmak için [Azure Express Route](https://azure.microsoft.com/documentation/articles/expressroute-faqs/#supported-services) ile DPS 'yı kullanıyorsunuz.

* DPS Yöneticisi tarafından şüpheli olarak tanımlanmış IP adreslerinden gelen trafiği reddetmeniz gerekir.

## <a name="how-filter-rules-are-applied"></a>Filtre kuralları nasıl uygulanır

IP filtresi kuralları, DPS örnek düzeyinde uygulanır. Bu nedenle, IP filtresi kuralları, desteklenen herhangi bir protokolü kullanarak cihazlarından ve arka uç uygulamalardan tüm bağlantılara uygulanır.

DPS örneğiniz için bir IP adresinden reddetme IP kuralıyla eşleşen herhangi bir bağlantı girişimi, yetkisiz 401 durum kodu ve açıklaması alır. Yanıt iletisi, IP kuralından bahsetmiyor.

## <a name="default-setting"></a>Varsayılan ayar

Varsayılan olarak, DPS portalındaki **IP filtresi** Kılavuzu boştur. Bu varsayılan ayar, DPS 'in herhangi bir IP adresinden gelen bağlantıları kabul ettiği anlamına gelir. Bu varsayılan ayar 0.0.0.0/0 IP adresi aralığını kabul eden bir kuralla eşdeğerdir.

![IoT DPS varsayılan IP filtresi ayarları](./media/iot-dps-ip-filtering/ip-filter-default.png)

## <a name="add-or-edit-an-ip-filter-rule"></a>IP filtresi kuralı ekleme veya düzenleme

Bir IP filtre kuralı eklemek için **+ IP filtre kuralı ekle**' yi seçin.

![IoT DPS 'e bir IP filtre kuralı ekleme](./media/iot-dps-ip-filtering/ip-filter-add-rule.png)

**IP filtre kuralı ekle**' yi seçtikten sonra alanları girin.

![IP filtre kuralı ekle seçeneğini belirledikten sonra](./media/iot-dps-ip-filtering/ip-filter-after-selecting-add.png)

* IP filtresi kuralı için bir **ad** girin. Bu, 128 karakter uzunluğunda benzersiz, büyük/küçük harfe duyarsız, alfasayısal bir dize olmalıdır. Yalnızca ASCII 7 bit alfasayısal karakterler ve kabul edilir `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` .

* CıDR gösteriminde tek bir IPv4 adresi veya IP adresi bloğu sağlayın. Örneğin, CıDR gösteriminde 192.168.100.0/22, 192.168.100.0 'den 192.168.103.255 'e 1024 IPv4 adresini temsil eder.

* IP filtresi kuralı için **eylem** olarak **Izin ver** veya **Engelle** ' yi seçin.

Alanları doldurduktan sonra, kuralı kaydetmek için **Kaydet** ' i seçin. Güncelleştirmenin devam ettiğini bildiren bir uyarı görürsünüz.

![IP filtresi kuralını kaydetme bildirimi](./media/iot-dps-ip-filtering/ip-filter-save-new-rule.png)

En fazla 10 IP filtresi kuralına ulaştığınızda **Ekle** seçeneği devre dışıdır.

Mevcut bir kuralı düzenlemek için, değiştirmek istediğiniz verileri seçin, değişikliği yapın ve ardından **Kaydet** ' i seçerek düzenleme bilgilerinizi kaydedin.

> [!NOTE]
> IP adreslerini reddetme, diğer Azure hizmetlerinin DPS örneğiyle etkileşimini engelleyebilir.

## <a name="delete-an-ip-filter-rule"></a>IP filtre kuralını Sil

Bir IP filtre kuralını silmek için söz konusu satırdaki çöp kutusu simgesini seçin ve ardından **Kaydet**' i seçin. Kural kaldırılır ve değişiklik kaydedilir.

![IoT DPS IP filtre kuralını silme](./media/iot-dps-ip-filtering/ip-filter-delete-rule.png)


## <a name="update-ip-filter-rules-in-code"></a>Koddaki IP filtresi kurallarını Güncelleştir

Azure Kaynak sağlayıcısı 'nın REST uç noktasını kullanarak DPS IP filtrenizi alabilir ve değiştirebilirsiniz. Bkz `properties.ipFilterRules` . [CreateOrUpdate yöntemi](https://docs.microsoft.com/rest/api/iot-dps/iotdpsresource/createorupdate).

DPS IP filtresi kurallarının güncelleştirilmesi Şu anda Azure CLı veya Azure PowerShell desteklenmez, ancak Azure Resource Manager şablonlarıyla gerçekleştirilebilir. Kaynak Yöneticisi şablonlarının kullanımı hakkında rehberlik için bkz. [Azure Resource Manager şablonları](../azure-resource-manager/templates/overview.md) . Aşağıdaki şablon örnekleri, DPS IP filtresi kuralları oluşturma, düzenleme ve silme işlemlerinin nasıl yapılacağını göstermektedir.

### <a name="add-an-ip-filter-rule"></a>IP filtresi kuralı ekle

Aşağıdaki şablon örneği, tüm trafiği kabul eden "AllowAll" adlı yeni bir IP filtre kuralı oluşturur.

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

Şablonun IP filtresi kuralı özniteliklerini gereksinimlerinize göre güncelleştirin.

| Öznitelik                | Açıklama |
| ------------------------ | ----------- |
| **Süzgeç**           | IP filtresi kuralı için bir ad girin. Bu, 128 karakter uzunluğunda benzersiz, büyük/küçük harfe duyarsız, alfasayısal bir dize olmalıdır. Yalnızca ASCII 7 bit alfasayısal karakterleri ve {'-', ': ', '/', ' \' , '. ', ' + ', '% ', ' _ ', ' # ', ' * ', '? ', '! ', ' (', ') ', ', ', ' = ', ' @ ', '; ', ' ' '} kabul edilir. |
| **Eylem**               | Kabul edilen değerler **Accept**,    **Reject**   IP filtre kuralı için eylem olarak kabul veya Red. |
| **ipMask**               | CıDR gösteriminde tek bir IPv4 adresi veya IP adresi bloğu sağlayın. Örneğin, CıDR gösteriminde 192.168.100.0/22, 192.168.100.0 'den 192.168.103.255 'e 1024 IPv4 adresini temsil eder. |


### <a name="update-an-ip-filter-rule"></a>IP filtre kuralını güncelleştirme

Aşağıdaki şablon örneği, daha önce gösterilen "AllowAll" adlı IP filtre kuralını tüm trafiği reddedecek şekilde güncelleştirir.

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

### <a name="delete-an-ip-filter-rule"></a>IP filtre kuralını Sil

Aşağıdaki şablon örneği, DPS örneği için tüm IP filtresi kurallarını siler.

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

IP filtresi kuralları sırasıyla uygulanır ve IP adresiyle eşleşen ilk kural kabul etme veya reddetme eylemini belirler.

Örneğin, 192.168.100.0/22 aralığındaki adresleri kabul etmek ve diğer her şeyi reddetmek istiyorsanız kılavuzdaki ilk kural 192.168.100.0/22 adres aralığını kabul etmelidir. Sonraki kural 0.0.0.0/0 aralığını kullanarak tüm adresleri reddetmelidir.

Kılavuzdaki IP filtre kurallarınızın sırasını, bir satırın başlangıcında bulunan üç dikey noktaya tıklayıp sürükle ve bırak seçeneğini kullanarak değiştirebilirsiniz.

Yeni IP filtresi kuralı siparişinizi kaydetmek için **Kaydet**' e tıklayın.

![DPS IP filtresi kurallarınızın sırasını değiştirme](./media/iot-dps-ip-filtering/ip-filter-rule-order.png)

## <a name="next-steps"></a>Sonraki adımlar

DPS yönetimi hakkında daha fazla bilgi için bkz.:

* [IoT DPS IP adreslerini anlama](iot-dps-understand-ip-address.md)
* [Azure CLı kullanarak DPS 'yi yapılandırma](how-to-manage-dps-with-cli.md)
* [DPS 'e erişimi denetleme](how-to-control-access.md)
