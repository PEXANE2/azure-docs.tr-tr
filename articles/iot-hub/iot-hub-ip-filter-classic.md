---
title: Azure IoT Hub klasik IP Filtresi (kullanım dışı) | Microsoft Docs
description: Klasik IP filtresinden yükseltme ve avantajlar nelerdir?
author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/16/2020
ms.author: jlian
ms.openlocfilehash: 6f326bafb311acedc48c5a349c78f1cd6bcebc87
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101661163"
---
# <a name="iot-hub-classic-ip-filter-and-how-to-upgrade"></a>Klasik IP filtresi IoT Hub ve yükseltme 

IoT Hub için yükseltilen IP filtresi, yerleşik uç noktayı korur ve varsayılan olarak güvenlidir. Hiçbir şekilde hiçbir şekilde hiçbir değişiklik yapmayın, yükseltilen IP filtresinin gelişmiş güvenlik modeli klasik IP filtresiyle uyumsuzdur, bu nedenle kullanımdan kaldırma duyuruyoruz. Yükseltilen yeni IP filtresi hakkında daha fazla bilgi edinmek için bkz. [Yenilikler ve](#whats-new) [IoT Hub IP filtreleri](iot-hub-ip-filtering.md).

Hizmet kesintisi yaşamamak için, geçiş son tarihinden önce Kılavuzlu yükseltmeyi gerçekleştirmeniz gerekir, bu noktada yükseltme otomatik olarak gerçekleştirilir. Geçiş zaman çizelgesi hakkında daha fazla bilgi için bkz. [Azure Update](https://aka.ms/ipfilterv2azupdate).

## <a name="how-to-upgrade"></a>Yükseltme

1.  Azure portal ziyaret edin
2.  IoT Hub'ınıza gidin.
3.  Sol taraftaki menüden **ağ** ' ı seçin.
4.  IP filtrenizi yeni modele yükseltmenizi isteyen bir başlık görmeniz gerekir. Devam etmek için **Evet**’i seçin.
    :::image type="content" source="media/iot-hub-ip-filter-classic/ip-filter-upgrade-banner.png" alt-text="Klasik IP filtresinden yükseltmek için başlık istemi gösteren resim":::
5.  Yeni IP filtresi tüm IP 'yi varsayılan olarak engellediği için, yükseltme, bireysel reddetme kurallarınızı kaldırır, ancak kaydetmeden önce bunları gözden geçibileceğiniz bir şansınız sağlar. Kuralların sizin için çalışmadıklarından emin olmak için kuralları dikkatle gözden geçirin.
6.  Yükseltmeyi bitirmeden istemleri izleyin.

## <a name="whats-new"></a>Yenilikler

### <a name="secure-by-default"></a>Varsayılan olarak güvenli

Klasik IP filtresi, tüm IP adreslerinin varsayılan olarak IoT Hub bağlanmasına izin verir ve bu, en yaygın ağ güvenliği senaryolarıyla iyi hizalanmaz. Genellikle, yalnızca güvenilir IP adreslerinin IoT Hub 'ınıza bağlanabilmesini ve diğer her şeyi reddedebilmesini istersiniz. Bu hedefe klasik IP filtresiyle ulaşmak için çok adımlı bir işlemdir. Örneğin, yalnızca öğesinden gelen trafiği kabul etmek istiyorsanız, şunları yapmanız `192.168.100.0/22` gerekir

1. İçin tek bir *izin verme* kuralı yapılandırın `192.168.100.0/22` .
1. İçin farklı bir *engelleme* kuralı yapılandırın `0.0.0.0/0` ("tümünü engelle" kuralı)
1. Kuralların, blok kuralının üzerinde sıralanan izin verme kuralına göre doğru şekilde sıralanmış olduğundan emin olun.

Uygulamada, bu çok adımlı işlem karışıklıklara neden olur. Kullanıcılar "tümünü engelle" kuralını yapılandırmadı veya kuralları doğru şekilde sıraladı; bu, istenmeyen bir açığa neden oldu. 

Yeni IP filtresi, varsayılan olarak tüm IP adreslerini engeller. Yalnızca açıkça eklediğiniz IP aralıklarının IoT Hub bağlanmasına izin verilir. Yukarıdaki örnekte adım 2 ve 3 ' te artık gerek yoktur. Bu yeni davranış, [Varsayılan olarak güvenli ilkeye](https://wikipedia.org/wiki/Secure_by_default)göre yapılandırmayı ve güvenliği basitleştirir.

### <a name="protect-the-built-in-event-hub-compatible-endpoint"></a>Yerleşik Olay Hub 'ı ile uyumlu uç noktasını koruma

Klasik IP filtresi yerleşik uç noktaya uygulanamıyor. Bu sınırlama, bir blok All kuralı (blok) yapılandırılmış bir olayın `0.0.0.0/0` , yerleşik uç noktasının herhangi BIR IP adresinden erişilebilir olduğu anlamına gelir.

Yeni IP filtresi, yerleşik uç noktaya kuralları uygulama seçeneği sunar ve bu da ağ güvenlik tehditlerine maruz kalma olasılığını azaltır.

:::image type="content" source="media/iot-hub-ip-filter-classic/ip-filter-built-in-endpoint.png" alt-text="Yerleşik uç noktaya uygulanacak veya olmayan geçiş noktasını gösteren resim":::

> [!NOTE]
> Bu seçenek ücretsiz (F1) IoT Hub 'ları için kullanılamaz. Yerleşik uç noktaya IP filtresi kuralları uygulamak için ücretli bir IoT Hub 'ı kullanın.

### <a name="api-impact"></a>API etkisi

Yükseltilen IP filtresi, IoT Hub kaynak API 'sinde `2020-08-31` ( `2020-08-31-preview` ve ile) ve sonraki sürümlerde kullanılabilir. Klasik IP filtresi, tüm API sürümlerinde hala kullanılabilir, ancak geçiş son tarihinin yakınında gelecek bir API sürümünde kaldırılacaktır. Geçiş zaman çizelgesi hakkında daha fazla bilgi için bkz. [Azure Update](https://aka.ms/ipfilterv2azupdate).

## <a name="tip-try-the-changes-before-they-apply"></a>İpucu: değişiklikleri uygulamadan önce deneyin

Yeni IP filtresi tüm IP adreslerini varsayılan olarak engellediği için, bireysel blok kuralları artık uyumlu değildir. Bu nedenle, Kılavuzlu yükseltme işlemi bu bireysel blok kurallarını kaldırır. 

Klasik IP filtresiyle içindeki değişikliği denemek için:

1. IoT Hub 'ınızdaki **ağ** sekmesini ziyaret edin
1. Geri almak istemeniz durumunda mevcut IP Filtresi (klasik) yapılandırmanızı aklınızda edin
1. **Blok** içeren kuralların yanında, kaldırmak için çöp kutusu simgesini seçin
1. İle en alta başka bir kural ekleyin `0.0.0.0/0` ve **Engelle** ' yi seçin.
1. **Kaydet**’i seçin

Bu yapılandırma, klasik yükseltmeden sonra yeni IP filtresinin nasıl davranacağını taklit eder. Tek istisna, klasik IP filtresini kullanmayı denemek mümkün olmayan yerleşik uç nokta korumasından biridir. Ancak, bu özellik isteğe bağlıdır, bu nedenle, bir şeyi kesintiye uğrayacağını düşünüyorsanız, kullanmak zorunda kalmazsınız.

## <a name="tip-check-diagnostic-logs-for-all-ip-connections-to-your-iot-hub"></a>İpucu: IoT Hub 'ınıza tüm IP bağlantıları için tanılama günlüklerine bakın

Kesintisiz bir geçiş sağlamak için, bağlantılar kategorisi altındaki tanılama günlüklerinizi kontrol edin. `maskedIpAddress`Aralıkların beklediği gibi olup olmadığını görmek için özelliğini arayın. Unutmayın: yeni IP filtresi, açıkça eklenmemiş olan tüm IP adreslerini engeller.

## <a name="iot-hub-classic-ip-filter-documentation-retired"></a>Klasik IP filtresi belgelerini IoT Hub (kullanımdan kaldırıldı)

> [!IMPORTANT]
> Aşağıda, klasik IP filtresine yönelik özgün belgeler, devre dışı bırakılıyor.

Güvenlik, Azure IoT Hub tabanlı tüm IoT çözümlerinin önemli bir yönüdür. Bazen güvenlik yapılandırmanız kapsamında cihazların hangi IP adreslerinden bağlanabileceğini açıkça belirtmeniz gerekir. *IP filtresi* özelliği, belirli IPv4 adreslerinden gelen trafiği reddetme veya kabul etme kurallarını yapılandırmanızı sağlar.

### <a name="when-to-use"></a>Kullanılması gereken durumlar

Belirli IP adresleri için IoT Hub uç noktalarını engellemek faydalı olduğunda iki özel kullanım durumu vardır:

* IoT Hub 'ınız yalnızca belirli bir IP adresi aralığından trafik almalıdır ve diğer her şeyi reddedebilir. Örneğin, IoT Hub 'ı ve şirket içi altyapınız arasında özel bağlantılar oluşturmak için IoT Hub 'ınızı [Azure Express Route](../expressroute/expressroute-faqs.md#supported-services) ile birlikte kullanıyorsunuz.

* IoT Hub Yöneticisi tarafından şüpheli olarak tanımlanmış IP adreslerinden gelen trafiği reddetmeniz gerekir.

### <a name="how-filter-rules-are-applied"></a>Filtre kuralları nasıl uygulanır?

IP filtresi kuralları IoT Hub hizmet düzeyinde uygulanır. Bu nedenle, IP filtresi kuralları, desteklenen herhangi bir protokolü kullanarak cihazlarından ve arka uç uygulamalardan tüm bağlantılara uygulanır. Ancak, [yerleşik Olay Hub 'ı ile uyumlu uç noktadan](iot-hub-devguide-messages-read-builtin.md) (IoT Hub bağlantı dizesi aracılığıyla değil) doğrudan okuyan ISTEMCILER, IP filtresi kurallarına bağlı değildir. 

IoT Hub 'ınızdaki reddetme IP kuralıyla eşleşen bir IP adresinden gelen bağlantı girişimleri, yetkisiz 401 durum kodu ve açıklaması alır. Yanıt iletisinde IP kuralı belirtilmez. IP adreslerini reddetme, Azure Stream Analytics, Azure sanal makineleri veya Azure portal Device Explorer, IoT Hub ile etkileşime geçmek gibi diğer Azure hizmetlerini engelleyebilir.

> [!NOTE]
> IP filtresi etkinleştirilmiş bir IoT Hub 'ından iletileri okumak için Azure Stream Analytics (ASA) kullanmanız gerekiyorsa, bir [Event Hubs akış girişini](../stream-analytics/stream-analytics-define-inputs.md#stream-data-from-event-hubs) el ile eklemek için IoT Hub 'ınızın Olay Hub 'ı ile uyumlu adını ve uç noktasını kullanın.

### <a name="default-setting"></a>Varsayılan ayar

Varsayılan olarak, bir IoT Hub 'ının portalındaki **IP filtresi** Kılavuzu boştur. Bu varsayılan ayar, hub 'ınızın herhangi bir IP adresinden gelen bağlantıları kabul ettiği anlamına gelir. Bu varsayılan ayar 0.0.0.0/0 IP adres aralığını kabul eden kuralla eşdeğerdir.

IP filtresi ayarları sayfasına ulaşmak için **ağ**, **ortak erişim**' i SEÇIN ve ardından **Seçili IP aralıkları**' nı seçin:

:::image type="content" source="media/iot-hub-ip-filter-classic/ip-filter-default.png" alt-text="IoT Hub varsayılan IP filtresi ayarları":::

### <a name="add-or-edit-an-ip-filter-rule"></a>IP filtresi kuralını ekleme veya düzenleme

IP filtresi kuralı eklemek için **+ IP Filtresi Kuralı Ekle**'yi seçin.

:::image type="content" source="./media/iot-hub-ip-filter-classic/ip-filter-add-rule.png" alt-text="IoT Hub 'ına bir IP filtre kuralı ekleme":::

**IP Filtresi Kuralı Ekle**'yi seçtikten sonra alanları doldurun.

:::image type="content" source="./media/iot-hub-ip-filter-classic/ip-filter-after-selecting-add.png" alt-text="IP Filtresi Kuralı Ekle'yi seçtikten sonra":::

* IP Filtresi kuralı için bir **ad** girin. Benzersiz, büyük-küçük harfe duyarlı olmayan alfasayısal bir dize olmalı ve en fazla 128 karakterden oluşmalıdır. Yalnızca ASCII 7-bit alfasayısal karakterler ile `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` karakterleri kabul edilir.

* Tek IPv4 adresi veya CIDR gösteriminde bir IP adresleri bloğu belirtin. Örneğin CIDR gösteriminde 192.168.100.0/22, 192.168.100.0 ile 192.168.103.255 arasındaki 1024 IPv4 adresini temsil eder.

* IP filtresi kuralının **eylemi** olarak **İzin Ver** veya **Engelle**'yi seçin.

Alanları doldurduktan sonra **Kaydet**'i seçerek kuralı kaydedin. Güncelleştirmenin devam ettiğini bildiren bir uyarı görürsünüz.

:::image type="content" source="./media/iot-hub-ip-filter-classic/ip-filter-save-new-rule.png" alt-text="IP filtresi kuralını kaydetme bildirimi":::

IP filtresi kurallarının üst sınırı olan 10 kurala ulaştığınızda **Ekle** seçeneği devre dışı bırakılır.

Mevcut kuralı düzenlemek için, değiştirmek istediğiniz verileri seçin, değişikliği yapın ve sonra da **Kaydet**'i seçerek düzenlemenizi kaydedin.

### <a name="delete-an-ip-filter-rule"></a>IP filtresi kuralını silme

IP filtresi kuralını silmek için, söz konusu satırdaki çöp kutusu simgesini seçin ve sonra da **Kaydet**'i seçin. Kural kaldırılır ve değişiklik kaydedilir.

:::image type="content" source="./media/iot-hub-ip-filter-classic/ip-filter-delete-rule.png" alt-text="IoT Hub IP filtresi kuralını silme":::

### <a name="retrieve-and-update-ip-filters-using-azure-cli"></a>Azure CLı kullanarak IP filtrelerini alma ve güncelleştirme

IoT Hub IP filtreleri, [Azure CLI](/cli/azure/)aracılığıyla alınabilir ve güncelleştirilir.

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

### <a name="retrieve-and-update-ip-filters-using-azure-powershell"></a>Azure PowerShell kullanarak IP filtrelerini alma ve güncelleştirme

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

### <a name="update-ip-filter-rules-using-rest"></a>REST kullanarak IP filtresi kurallarını güncelleştirme

Ayrıca, Azure Kaynak sağlayıcısı 'nın REST uç noktasını kullanarak IoT Hub IP filtresini alabilir ve değiştirebilirsiniz. [createorupdate yöntemi](/rest/api/iothub/iothubresource/createorupdate) altında `properties.ipFilterRules` bölümüne bakın.

### <a name="ip-filter-rule-evaluation"></a>IP filtresi kuralı değerlendirmesi

IP filtresi kuralları sırayla uygulanır ve kabul etme veya reddetme eylemini IP adresiyle eşleşen ilk kural belirler.

Örneğin 192.168.100.0/22 aralığındaki adresleri kabul etmek ve diğer her şeyi reddetmek istiyorsanız, kılavuzdaki ilk kural 192.168.100.0/22 adres aralığını kabul etmelidir. Sonraki kural 0.0.0.0/0 aralığını kullanarak tüm adresleri reddetmelidir.

Kılavuzda satırın başındaki üç dikey noktaya tıklayarak ve sürükleyip bırakmayı kullanarak IP filtresi kurallarınızın sırasını değiştirebilirsiniz.

IP filtresi kurallarınızın yeni sırasını kaydetmek için **Kaydet**'e tıklayın.

:::image type="content" source="media/iot-hub-ip-filter-classic/ip-filter-rule-order.png" alt-text="IoT Hub IP filtresi kurallarınızın sırasını değiştirme":::

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub yeteneklerini daha fazla incelemek için bkz.:

* [IP filtrelerini kullanma](iot-hub-ip-filtering.md)