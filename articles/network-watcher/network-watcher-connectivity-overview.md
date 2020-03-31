---
title: Azure Ağ İzleyicisi Bağlantı Sorun Giderme Sorununa Giriş | Microsoft Dokümanlar
description: Bu sayfa, Ağ İzleyicisi bağlantı sorun giderme özelliğine genel bir bakış sağlar
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: damendo
ms.openlocfilehash: cae3072a3468b232e95d7c1949948b71059695ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283283"
---
# <a name="introduction-to-connection-troubleshoot-in-azure-network-watcher"></a>Azure Ağ İzleyicisi'nde bağlantı sorun gidermeye giriş

Network Watcher'ın bağlantı sorunu giderme özelliği, sanal bir makineden sanal makineye (VM), tam nitelikli etki alanı adı (FQDN), URI veya IPv4 adresine doğrudan TCP bağlantısını denetleme olanağı sağlar. Ağ senaryoları karmaşıktır, ağ güvenlik grupları, güvenlik duvarları, kullanıcı tanımlı rotalar ve Azure tarafından sağlanan kaynaklar kullanılarak uygulanır. Karmaşık yapılandırmalar, sorun giderme bağlantı sorunlarını zora sokar. Ağ İzleyicisi, bağlantı sorunlarını bulmak ve algılamak için gereken süreyi azaltmaya yardımcı olur. Döndürülen sonuçlar, bir bağlantı sorununun bir platformdan mı yoksa kullanıcı yapılandırma sorunundan mı kaynaklandığına ilişkin öngörüler sağlayabilir. Bağlantı [PowerShell,](network-watcher-connectivity-powershell.md) [Azure CLI](network-watcher-connectivity-cli.md)ve REST [API](network-watcher-connectivity-rest.md)ile kontrol edilebilir.

> [!IMPORTANT]
> Bağlantı sorun giderme, sorun giderdiğiniz VM'nin `AzureNetworkWatcherExtension` VM uzantısıyüklü olması gerekir. Uzantıyı Windows VM'de yüklemek [için Windows için Azure Network Watcher Agent sanal makine uzantısını](../virtual-machines/windows/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ziyaret edin ve Linux VM için Linux için Azure Network [Watcher Agent sanal makine uzantısını](../virtual-machines/linux/extensions-nwa.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json)ziyaret edin. Uzantı, hedef bitiş noktasında gerekli değildir.

## <a name="response"></a>Yanıt

Aşağıdaki tablo, bağlantı sorunu giderme işi tamamlandığında döndürülen özellikleri gösterir.

|Özellik  |Açıklama  |
|---------|---------|
|Bağlantı Durumu     | Bağlantı denetiminin durumu. Olası sonuçlar **Ulaşılabilir** ve **Ulaşılamıyor.**        |
|AvgLatencyInMs     | Milisaniye cinsinden bağlantı denetimi sırasında ortalama gecikme süresi. (Yalnızca çek durumuna ulaşılabilirse gösterilir)        |
|MinLatencyInMs     | Milisaniye cinsinden bağlantı denetimi sırasında minimum gecikme süresi. (Yalnızca çek durumuna ulaşılabilirse gösterilir)        |
|MaxLatencyInMs     | Milisaniye cinsinden bağlantı denetimi sırasında maksimum gecikme süresi. (Yalnızca çek durumuna ulaşılabilirse gösterilir)        |
|ProbesSent     | Kontrol sırasında gönderilen sonda sayısı. Maksimum değer 100'dür.        |
|ProbesFailed     | Denetim sırasında başarısız olan sonda sayısı. Maksimum değer 100'dür.        |
|Atlamalar     | Kaynaktan hedefe atlama yolu ile hop.        |
|Hops[]. Türü     | Kaynak türü. Olası değerler **Kaynak**, **VirtualAppliance**, **VnetLocal**ve **Internet**vardır.        |
|Hops[]. Kimliği | Hop benzersiz tanımlayıcısı.|
|Hops[]. Adres | Atlamanın IP adresi.|
|Hops[]. Resourceıd | Atlama bir Azure kaynağıysa atlamanın Kaynak Kimliği. Bir internet kaynağı ise, ResourceID **Internet'tir.** |
|Hops[]. NextHopIds | Bir sonraki atlamanın benzersiz tanımlayıcısı alınır.|
|Hops[]. Sorun | O atlamada kontrol sırasında karşılaşılan sorunlar dan oluşan bir koleksiyon. Sorun yoksa, değer boştur.|
|Hops[]. Sorunlar[]. Kökenli | Sorunun oluştuğu geçerli atlamada. Olası değerler şunlardır:<br/> **Gelen** - Sorun geçerli atlama önceki atlama bağlantıda<br/>**Giden** - Sorun geçerli atlamadan sonraki hop bağlantıda<br/>**Yerel** - Sorun geçerli atlamada.|
|Hops[]. Sorunlar[]. Önem | Sorunun önem derecesi algılandı. Olası değerler **Hata** ve **Uyarı'dır.** |
|Hops[]. Sorunlar[]. Türü |Bulunan sorunun türü. Olası değerler şunlardır: <br/>**CPU**<br/>**Bellek**<br/>**GuestFirewall**<br/>**DnsÇözünürlük**<br/>**AğGüvenliği Kuralı**<br/>**UserDefinedRoute** |
|Hops[]. Sorunlar[]. Bağlam |Konuyla ilgili ayrıntılar bulundu.|
|Hops[]. Sorunlar[]. Bağlam[].key |Anahtar değer çiftinin anahtarı döndürülür.|
|Hops[]. Sorunlar[]. Bağlam[].value |Döndürülen anahtar değer çiftinin değeri.|

Aşağıda, atlamada bulunan bir soruna bir örnek verilmiştir.

```json
"Issues": [
    {
        "Origin": "Outbound",
        "Severity": "Error",
        "Type": "NetworkSecurityRule",
        "Context": [
            {
                "key": "RuleName",
                "value": "UserRule_Port80"
            }
        ]
    }
]
```
## <a name="fault-types"></a>Hata tipleri

Bağlantı sorun giderme, bağlantıyla ilgili hata türlerini döndürür. Aşağıdaki tablo, döndürülen geçerli hata türlerinin bir listesini sağlar.

|Tür  |Açıklama  |
|---------|---------|
|CPU     | Yüksek CPU kullanımı.       |
|Bellek     | Yüksek Bellek kullanımı.       |
|GuestFirewall     | Sanal makine güvenlik duvarı yapılandırması nedeniyle trafik engellendi.        |
|DNSResolution     | DNS çözümü hedef adres için başarısız oldu.        |
|AğGüvenliği Kuralı    | Trafik Bir NSG Kuralı tarafından engellenir (Kural döndürülür)        |
|UserDefinedRoute|Trafik, kullanıcı tanımlı veya sistem rotası nedeniyle bırakılır. |

### <a name="next-steps"></a>Sonraki adımlar

[Azure portalı](network-watcher-connectivity-portal.md), [PowerShell](network-watcher-connectivity-powershell.md), [Azure CLI](network-watcher-connectivity-cli.md)veya [REST API'yi](network-watcher-connectivity-rest.md)kullanarak bağlantıları nasıl gidereceklerini öğrenin.