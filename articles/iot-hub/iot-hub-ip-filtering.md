---
title: Azure IoT Hub IP bağlantısı filtreleri | Microsoft Docs
description: IP filtrelemeyi kullanarak Azure IoT Hub 'ınıza yönelik belirli IP adreslerinden gelen bağlantıları engelleyin. Tek tek IP adreslerinden veya IP adresi aralıklarından gelen bağlantıları engelleyebilirsiniz.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/25/2020
ms.author: robinsh
ms.openlocfilehash: a9a95c0151ed6dd3a2ad4a9d548723cdff0bcfb8
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92147084"
---
# <a name="use-ip-filters"></a>IP filtrelerini kullanma

Güvenlik, Azure IoT Hub tabanlı tüm IoT çözümlerinin önemli bir yönüdür. Bazen güvenlik yapılandırmanız kapsamında cihazların hangi IP adreslerinden bağlanabileceğini açıkça belirtmeniz gerekir. *IP filtresi* özelliği, belirli IPv4 adreslerinden gelen trafiği reddetme veya kabul etme kurallarını yapılandırmanızı sağlar.

## <a name="when-to-use"></a>Kullanılması gereken durumlar

Belirli IP adresleri için IoT Hub uç noktalarını engellemek faydalı olduğunda iki özel kullanım durumu vardır:

* IoT Hub 'ınız yalnızca belirli bir IP adresi aralığından trafik almalıdır ve diğer her şeyi reddedebilir. Örneğin, IoT Hub 'ı ve şirket içi altyapınız arasında özel bağlantılar oluşturmak için IoT Hub 'ınızı [Azure Express Route](../expressroute/expressroute-faqs.md#supported-services) ile birlikte kullanıyorsunuz.

* IoT Hub Yöneticisi tarafından şüpheli olarak tanımlanmış IP adreslerinden gelen trafiği reddetmeniz gerekir.

## <a name="how-filter-rules-are-applied"></a>Filtre kuralları nasıl uygulanır?

IP filtresi kuralları IoT Hub hizmet düzeyinde uygulanır. Bu nedenle, IP filtresi kuralları, desteklenen herhangi bir protokolü kullanarak cihazlarından ve arka uç uygulamalardan tüm bağlantılara uygulanır. Ancak, [yerleşik Olay Hub 'ı ile uyumlu uç noktadan](iot-hub-devguide-messages-read-builtin.md) (IoT Hub bağlantı dizesi aracılığıyla değil) doğrudan okuyan ISTEMCILER, IP filtresi kurallarına bağlı değildir. 

IoT Hub 'ınızdaki reddetme IP kuralıyla eşleşen bir IP adresinden gelen bağlantı girişimleri, yetkisiz 401 durum kodu ve açıklaması alır. Yanıt iletisinde IP kuralı belirtilmez. IP adreslerini reddetme, Azure Stream Analytics, Azure sanal makineleri veya Azure portal Device Explorer, IoT Hub ile etkileşime geçmek gibi diğer Azure hizmetlerini engelleyebilir.

> [!NOTE]
> IP filtresi etkinleştirilmiş bir IoT Hub 'ından iletileri okumak için Azure Stream Analytics (ASA) kullanmanız gerekiyorsa, bir [Event Hubs akış girişini](../stream-analytics/stream-analytics-define-inputs.md#stream-data-from-event-hubs) el ile eklemek için IoT Hub 'ınızın Olay Hub 'ı ile uyumlu adını ve uç noktasını kullanın.

## <a name="default-setting"></a>Varsayılan ayar

Varsayılan olarak, bir IoT Hub 'ının portalındaki **IP filtresi** Kılavuzu boştur. Bu varsayılan ayar, hub 'ınızın herhangi bir IP adresinden gelen bağlantıları kabul ettiği anlamına gelir. Bu varsayılan ayar 0.0.0.0/0 IP adres aralığını kabul eden kuralla eşdeğerdir.

IP filtresi ayarları sayfasına ulaşmak için **ağ**, **ortak erişim**' i SEÇIN ve ardından **Seçili IP aralıkları**' nı seçin:

:::image type="content" source="media/iot-hub-ip-filtering/ip-filter-default.png" alt-text="IoT Hub varsayılan IP filtresi ayarları":::

## <a name="add-or-edit-an-ip-filter-rule"></a>IP filtresi kuralını ekleme veya düzenleme

IP filtresi kuralı eklemek için **+ IP Filtresi Kuralı Ekle**'yi seçin.

:::image type="content" source="./media/iot-hub-ip-filtering/ip-filter-add-rule.png" alt-text="IoT Hub varsayılan IP filtresi ayarları":::

**IP Filtresi Kuralı Ekle**'yi seçtikten sonra alanları doldurun.

:::image type="content" source="./media/iot-hub-ip-filtering/ip-filter-after-selecting-add.png" alt-text="IoT Hub varsayılan IP filtresi ayarları":::

* IP Filtresi kuralı için bir **ad** girin. Benzersiz, büyük-küçük harfe duyarlı olmayan alfasayısal bir dize olmalı ve en fazla 128 karakterden oluşmalıdır. Yalnızca ASCII 7-bit alfasayısal karakterler ile `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` karakterleri kabul edilir.

* Tek IPv4 adresi veya CIDR gösteriminde bir IP adresleri bloğu belirtin. Örneğin CIDR gösteriminde 192.168.100.0/22, 192.168.100.0 ile 192.168.103.255 arasındaki 1024 IPv4 adresini temsil eder.

* IP filtresi kuralının **eylemi** olarak **İzin Ver** veya **Engelle**'yi seçin.

Alanları doldurduktan sonra **Kaydet**'i seçerek kuralı kaydedin. Güncelleştirmenin devam ettiğini bildiren bir uyarı görürsünüz.

:::image type="content" source="./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png" alt-text="IoT Hub varsayılan IP filtresi ayarları":::

IP filtresi kurallarının üst sınırı olan 10 kurala ulaştığınızda **Ekle** seçeneği devre dışı bırakılır.

Mevcut kuralı düzenlemek için, değiştirmek istediğiniz verileri seçin, değişikliği yapın ve sonra da **Kaydet**'i seçerek düzenlemenizi kaydedin.

## <a name="delete-an-ip-filter-rule"></a>IP filtresi kuralını silme

IP filtresi kuralını silmek için, söz konusu satırdaki çöp kutusu simgesini seçin ve sonra da **Kaydet**'i seçin. Kural kaldırılır ve değişiklik kaydedilir.

:::image type="content" source="./media/iot-hub-ip-filtering/ip-filter-delete-rule.png" alt-text="IoT Hub varsayılan IP filtresi ayarları":::

## <a name="retrieve-and-update-ip-filters-using-azure-cli"></a>Azure CLı kullanarak IP filtrelerini alma ve güncelleştirme

IoT Hub IP filtreleri, [Azure CLI](/cli/azure/?view=azure-cli-latest)aracılığıyla alınabilir ve güncelleştirilir.

IoT Hub geçerli IP filtrelerini almak için şunu çalıştırın:

```azurecli-interactive
az resource show -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs
```

Bu, mevcut IP filtrelerinizin anahtar altında listelendiği bir JSON nesnesi döndürür `properties.ipFilterRules` :

```json
{
...
    "properties": {
        "ipFilterRules": [
        {
            "action": "Reject",
            "filterName": "MaliciousIP",
            "ipMask": "6.6.6.6/6"
        },
        {
            "action": "Allow",
            "filterName": "GoodIP",
            "ipMask": "131.107.160.200"
        },
        ...
        ],
    },
...
}
```

IoT Hub için yeni bir IP filtresi eklemek için şunu çalıştırın:

```azurecli-interactive
az resource update -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs --add properties.ipFilterRules "{\"action\":\"Reject\",\"filterName\":\"MaliciousIP\",\"ipMask\":\"6.6.6.6/6\"}"
```

IoT Hub var olan bir IP filtresini kaldırmak için şunu çalıştırın:

```azurecli-interactive
az resource update -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs --add properties.ipFilterRules <ipFilterIndexToRemove>
```

`<ipFilterIndexToRemove>`IoT Hub IÇIN IP filtreleri sıralamasına karşılık gelmelidir `properties.ipFilterRules` .

## <a name="retrieve-and-update-ip-filters-using-azure-powershell"></a>Azure PowerShell kullanarak IP filtrelerini alma ve güncelleştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

IoT Hub IP filtreleri alınabilir ve [Azure PowerShell](/powershell/azure/)aracılığıyla ayarlanabilir.

```powershell
# Get your IoT Hub resource using its name and its resource group name
$iothubResource = Get-AzResource -ResourceGroupName <resourceGroupNmae> -ResourceName <iotHubName> -ExpandProperties

# Access existing IP filter rules
$iothubResource.Properties.ipFilterRules |% { Write-host $_ }

# Construct a new IP filter
$filter = @{'filterName'='MaliciousIP'; 'action'='Reject'; 'ipMask'='6.6.6.6/6'}

# Add your new IP filter rule
$iothubResource.Properties.ipFilterRules += $filter

# Remove an existing IP filter rule using its name, e.g., 'GoodIP'
$iothubResource.Properties.ipFilterRules = @($iothubResource.Properties.ipFilterRules | Where 'filterName' -ne 'GoodIP')

# Update your IoT Hub resource with your updated IP filters
$iothubResource | Set-AzResource -Force
```

## <a name="update-ip-filter-rules-using-rest"></a>REST kullanarak IP filtresi kurallarını güncelleştirme

Ayrıca, Azure Kaynak sağlayıcısı 'nın REST uç noktasını kullanarak IoT Hub IP filtresini alabilir ve değiştirebilirsiniz. [createorupdate yöntemi](/rest/api/iothub/iothubresource/createorupdate) altında `properties.ipFilterRules` bölümüne bakın.

## <a name="ip-filter-rule-evaluation"></a>IP filtresi kuralı değerlendirmesi

IP filtresi kuralları sırayla uygulanır ve kabul etme veya reddetme eylemini IP adresiyle eşleşen ilk kural belirler.

Örneğin 192.168.100.0/22 aralığındaki adresleri kabul etmek ve diğer her şeyi reddetmek istiyorsanız, kılavuzdaki ilk kural 192.168.100.0/22 adres aralığını kabul etmelidir. Sonraki kural 0.0.0.0/0 aralığını kullanarak tüm adresleri reddetmelidir.

Kılavuzda satırın başındaki üç dikey noktaya tıklayarak ve sürükleyip bırakmayı kullanarak IP filtresi kurallarınızın sırasını değiştirebilirsiniz.

IP filtresi kurallarınızın yeni sırasını kaydetmek için **Kaydet**'e tıklayın.

:::image type="content" source="media/iot-hub-ip-filtering/ip-filter-rule-order.png" alt-text="IoT Hub varsayılan IP filtresi ayarları":::

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub yeteneklerini daha fazla incelemek için bkz.:

* [IoT Hub ölçümleri](iot-hub-metrics.md)