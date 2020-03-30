---
title: Azure IoT Hub IP bağlantı filtreleri | Microsoft Dokümanlar
description: Azure IoT hub'ınıza belirli IP adreslerinden bağlantıları engellemek için IP filtreleme nasıl kullanılır? Bağlantıları tek tek veya IP adresleri aralıklarından engelleyebilirsiniz.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/22/2017
ms.author: robinsh
ms.openlocfilehash: a6bd8a766f3205358a65ef2fd0816643e4261cab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68414336"
---
# <a name="use-ip-filters"></a>IP filtrelerini kullanma

Güvenlik, Azure IoT Hub'ına dayalı herhangi bir IoT çözümünün önemli bir yönüdür. Bazen, güvenlik yapılandırmanızın bir parçası olarak hangi aygıtların bağlanabileceği IP adreslerini açıkça belirtmeniz gerekir. *IP filtresi* özelliği, belirli IPv4 adreslerinden gelen trafiği reddetmek veya kabul etmek için kuralları yapılandırmanızı sağlar.

## <a name="when-to-use"></a>Kullanılması gereken durumlar

Belirli IP adresleri için IoT Hub uç noktalarını engellemeye yararlı olduğunda iki özel kullanım noktası vardır:

* IoT hub'ınız yalnızca belirli bir IP adresi aralığından trafik almalı ve diğer her şeyi reddetmelidir. Örneğin, Bir IoT hub'ı ile şirket içi altyapınız arasında özel bağlantılar oluşturmak için [Azure Express Route](https://azure.microsoft.com/documentation/articles/expressroute-faqs/#supported-services) ile IoT hub'ınızı kullanıyorsunuz.

* IoT hub yöneticisi tarafından şüpheli olarak tanımlanan IP adreslerinden gelen trafiği reddetmeniz gerekir.

## <a name="how-filter-rules-are-applied"></a>Filtre kuralları nasıl uygulanır?

IP filtre kuralları IoT Hub hizmet düzeyinde uygulanır. Bu nedenle IP filtresi kuralları, desteklenen protokolleri kullanarak aygıtlardan ve arka uç uygulamalardan gelen tüm bağlantılar için geçerlidir.

IoT hub'ınızdaki reddeden IP kuralıyla eşleşen bir IP adresinden gelen herhangi bir bağlantı girişimi yetkisiz bir 401 durum kodu ve açıklaması alır. Yanıt iletisi IP kuralından bahsetmez.

## <a name="default-setting"></a>Varsayılan ayar

Varsayılan olarak, bir IoT hub'ı için portaldaki **IP Filtresi** ızgarası boştur. Bu varsayılan ayar, hub'ınızın herhangi bir IP adresinden bağlantıları kabul ettiği anlamına gelir. Bu varsayılan ayar, 0.0.0.0/0 IP adresi aralığını kabul eden bir kurala eşdeğerdir.

![IoT Hub varsayılan IP filtresi ayarları](./media/iot-hub-ip-filtering/ip-filter-default.png)

## <a name="add-or-edit-an-ip-filter-rule"></a>IP filtresi kuralı ekleme veya yönetme

IP filtre kuralı eklemek için **+ IP Filtresi Kuralı Ekle'yi**seçin.

![IoT hub'ına IP filtresi kuralı ekleme](./media/iot-hub-ip-filtering/ip-filter-add-rule.png)

**IP Filtre Kuralı Ekle'yi**seçtikten sonra alanları doldurun.

![IP Filtresi ekle kuralını seçtikten sonra](./media/iot-hub-ip-filtering/ip-filter-after-selecting-add.png)

* IP Filtresi kuralı için bir **ad** sağlayın. Bu, 128 karakter uzunluğunda, benzersiz, büyük/küçük harf duyarlı, alfasayısal bir dize olmalıdır. Yalnızca ASCII 7-bit alfasayısal karakterler `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` artı kabul edilir.

* CIDR gösteriminde tek bir IPv4 adresi veya IP adresi bloğu sağlayın. Örneğin, CIDR notasyonu 192.168.100.0/22 192.168.100.0 192.168.103.255 1024 IPv4 adreslerini temsil eder.

* IP filtresi kuralıiçin **eylem** olarak **İzin Ver** veya **Engelle'yi** seçin.

Alanları doldurduktan sonra kuralı kaydetmek için **Kaydet'i** seçin. Güncelleştirmenin devam ettiğini bildiren bir uyarı görürsünüz.

![IP filtresi kuralını kaydetme hakkında bildirim](./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png)

En fazla 10 IP filtre kuralına ulaştığınızda **Ekle** seçeneği devre dışı bırakılır.

Varolan bir kuralı değiştirmek için, değiştirmek istediğiniz verileri seçin, değişikliği yapın ve ardından da değişikliği kaydetmek için **Kaydet'i** seçin.

> [!NOTE]
> IP adreslerini reddetmek, diğer Azure Hizmetlerinin (Azure Akış Analizi, Azure Sanal Makineleri veya portaldaki Aygıt Gezgini gibi) IoT hub'ı ile etkileşimkurmasını engelleyebilir.

> [!WARNING]
> IP filtresi etkin leştirilmiş bir IoT hub'ından gelen iletileri okumak için Azure Akış Analizi'ni (ASA) kullanıyorsanız, ASA bağlantı dizesinde IoT Hub'ınızın Olay Hub'ı uyumlu adını ve bitiş noktasını kullanın.

## <a name="delete-an-ip-filter-rule"></a>IP filtresi kuralını silme

IP filtresi kuralını silmek için, o satırdaki çöp kutusu simgesini seçin ve ardından **Kaydet'i**seçin. Kural kaldırılır ve değişiklik kaydedilir.

![IoT Hub IP filtre kuralını silme](./media/iot-hub-ip-filtering/ip-filter-delete-rule.png)

## <a name="retrieve-and-update-ip-filters-using-azure-cli"></a>Azure CLI kullanarak IP filtrelerini alın ve güncelleyin

IoT Hub'ınızın IP filtreleri [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)üzerinden alınabilir ve güncellenebilir.

IoT Hub'ınızın geçerli IP filtrelerini almak için çalıştırın:

```azurecli-interactive
az resource show -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs
```

Bu, varolan IP filtrelerinizin anahtarın altında `properties.ipFilterRules` listelendiği bir JSON nesnesini döndürür:

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

IoT Hub'ınız için yeni bir IP filtresi eklemek için çalıştırın:

```azurecli-interactive
az resource update -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs --add properties.ipFilterRules "{\"action\":\"Reject\",\"filterName\":\"MaliciousIP\",\"ipMask\":\"6.6.6.6/6\"}"
```

IoT Hub'ınızdaki varolan bir IP filtresini kaldırmak için çalıştırın:

```azurecli-interactive
az resource update -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs --add properties.ipFilterRules <ipFilterIndexToRemove>
```

IoT Hub'ınızdaki IP filtrelerinin sıralanmasına karşılık gelen ler `<ipFilterIndexToRemove>` olduğunu `properties.ipFilterRules`unutmayın.

## <a name="retrieve-and-update-ip-filters-using-azure-powershell"></a>Azure PowerShell kullanarak IP filtrelerini alın ve güncelleyin

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

IoT Hub'ınızın IP filtreleri [Azure PowerShell](/powershell/azure/overview)üzerinden alınabilir ve ayarlanabilir.

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

## <a name="update-ip-filter-rules-using-rest"></a>REST kullanarak IP filtre kurallarını güncelleştirin

Ayrıca, Azure kaynak sağlayıcısının REST bitiş noktasını kullanarak IoT Hub'ınızın IP filtresini alabilir ve değiştirebilirsiniz. `properties.ipFilterRules` [Createorupdate yöntemine](https://docs.microsoft.com/rest/api/iothub/iothubresource/createorupdate)bakın.

## <a name="ip-filter-rule-evaluation"></a>IP filtresi kuralı değerlendirmesi

Sırayla IP filtresi kuralları uygulanır ve IP adresiyle eşleşen ilk kural kabul veya reddetme eylemini belirler.

Örneğin, 192.168.100.0/22 aralığındaki adresleri kabul etmek ve diğer her şeyi reddetmek istiyorsanız, kılavuzdaki ilk kural 192.168.100.0/22 adres aralığını kabul etmelidir. Sonraki kural 0.0.0.0/0 aralığını kullanarak tüm adresleri reddetmelidir.

Bir satırın başındaki üç dikey noktayı tıklatarak ve sürükle ve bırak'ı kullanarak ızgaradaki IP filtre kurallarınızın sırasını değiştirebilirsiniz.

Yeni IP filtresi kural sıranızı kaydetmek için **Kaydet'i**tıklatın.

![IoT Hub IP filtre kurallarınızın sırasını değiştirme](./media/iot-hub-ip-filtering/ip-filter-rule-order.png)

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [İşlemleri izleme](iot-hub-operations-monitoring.md)
* [IoT Hub ölçümleri](iot-hub-metrics.md)
