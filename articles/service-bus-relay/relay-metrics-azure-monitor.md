---
title: Azure Izleyici 'de ölçümleri Azure Relay | Microsoft Docs
description: Bu makalede, Azure Relay durumunu izlemek için Azure Izleyici 'yi nasıl kullanabileceğiniz hakkında bilgi verilmektedir.
services: service-bus-relay
documentationcenter: .NET
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: 159249e2c997e4c414127992b08a83b488281e46
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78273128"
---
# <a name="azure-relay-metrics-in-azure-monitor"></a>Azure Izleyici 'de Azure Relay ölçümleri 
Azure Relay ölçümler, Azure aboneliğinizdeki kaynakların durumunu sağlar. Zengin bir ölçüm verileri kümesiyle, geçiş kaynaklarınızın genel durumunu yalnızca ad alanı düzeyinde değil, aynı zamanda varlık düzeyinde değerlendirebilirsiniz. Bu istatistikler Azure Relay durumunu izlemenize yardımcı olduklarından önemli olabilir. Ölçümler, Azure desteğine başvurun gerek kalmadan kök neden sorunlarını da yardımcı olabilir.

Azure İzleyici, çeşitli Azure Hizmetleri genelinde izleme için birleştirilmiş bir kullanıcı arabirimi sağlar. Daha fazla bilgi için bkz. [Microsoft Azure 'de izleme](../monitoring-and-diagnostics/monitoring-overview.md) ve GitHub 'da [.net örneği ile Azure izleyici ölçümlerini alma](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) .

> [!IMPORTANT]
> Bu makale, WCF Geçişi değil, yalnızca Azure Relay Karma Bağlantılar özelliği için geçerlidir. 

## <a name="access-metrics"></a>Erişim ölçümleri

Azure İzleyici ölçümlerine erişim birden çok yol sağlar. Ölçümlere [Azure Portal](https://portal.azure.com)aracılığıyla erişebilir veya Azure Izleyici API 'LERINI (REST ve .net) ve Işlem yönetimi paketi ve Event Hubs gibi analiz çözümlerini kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure izleyici tarafından toplanan verileri izleme](../azure-monitor/platform/data-platform.md).

Ölçümler, varsayılan olarak etkindir ve en son 30 Günün verilerini erişebilir. Uzun bir süre saklamak istiyorsanız ölçüm verileri bir Azure depolama hesabına arşivleyebilir. Bu, Azure Izleyici 'de [Tanılama ayarları](../azure-monitor/platform/diagnostic-settings.md) 'nda yapılandırılır.

## <a name="access-metrics-in-the-portal"></a>Portalda erişim ölçümlerini

[Azure Portal](https://portal.azure.com)ölçümleri zaman içinde izleyebilirsiniz. Aşağıdaki örnek, başarılı istekleri ve hesap düzeyinde gelen istekleri görüntülemek gösterilmektedir:

![][1]

Ad alanı aracılığıyla doğrudan ölçümleri de erişebilirsiniz. Bunu yapmak için, ad alanınızı seçin ve ardından * * ölçümler * * öğesine tıklayın. 

Boyutları destekleyen ölçümler için, istenen boyut değeri ile filtrelemeniz gerekir.

## <a name="billing"></a>Faturalama

Ölçümleri kullanarak Azure İzleyici'de önizleme şu an ücretsizdir. Ölçüm verilerini alma, ek çözümleri kullanırsanız, ancak bu çözümler tarafından faturalandırılırsınız. Örneğin, ölçüm verileri bir Azure depolama hesabına arşivleme, Azure Depolama tarafından faturalandırılır. Ayrıca, ölçüm verilerini gelişmiş analizler için Azure Izleyici günlüklerine kadıysanız Azure Izleyici günlükleri tarafından da faturalandırılırsınız.

Aşağıdaki ölçümler size sistem hizmetinizin genel bakış sunar. 

> [!NOTE]
> Farklı bir adla geçildiği size çeşitli ölçümleri kullanımdan. Bu, başvurularını güncelleştirmek gerektirebilir. "Kullanım dışı" anahtar sözcüğüyle işaretli ölçümleri, bundan sonra desteklenmeyecek.

Azure İzleyici, tüm ölçüm değerleri dakikada gönderilir. Zaman ayrıntı düzeyi ölçüm değerleri sunulduğu zaman aralığını tanımlar. Tüm Azure Relay ölçümleri için desteklenen zaman aralığı 1 dakikadır.

## <a name="connection-metrics"></a>Bağlantı ölçümü

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| ListenerConnections-başarılı  | Belirli bir süre içinde Azure Relay yapılan başarılı dinleyici bağlantısı sayısı. <br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|ListenerConnections-ClientError |Belirli bir süre boyunca dinleyici bağlantılarında istemci hatalarının sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|ListenerConnections-Sunucuhatası |Belirli bir süre boyunca dinleyici bağlantılarında sunucu hatalarının sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|SenderConnections-başarılı |Belirli bir süre içinde yapılan başarılı gönderici bağlantısı sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|SenderConnections-ClientError |Belirli bir süre boyunca gönderici bağlantılarında istemci hatalarının sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|SenderConnections-Sunucuhatası |Belirli bir süre boyunca gönderici bağlantılarında sunucu hatalarının sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|ListenerConnections-TotalRequests |Belirli bir süre içindeki dinleyici bağlantılarının toplam sayısı.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|SenderConnections-TotalRequests |Belirli bir süre içinde göndericiler tarafından yapılan bağlantı istekleri.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|ActiveConnection sayısı |Etkin bağlantı sayısı. Bu değer bir zaman noktası değeridir.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|ActiveListeners |Etkin dinleyicilerinin sayısı. Bu değer bir zaman noktası değeridir.<br/><br/> Birim: sayısı <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Listenerdisconnect |Belirtilen bir süre içinde bağlantısı kesilen dinleyicilerinin sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|
|Gönderenin bağlantısı kesiliyor |Belirli bir süre içinde bağlantısı kesilen gönderici sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="memory-usage-metrics"></a>Bellek kullanım ölçümleri

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|BytesTransferred |Belirtilen bir süre içinde aktarılan bayt sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: toplam <br/> Boyut: EntityName|

## <a name="metrics-dimensions"></a>Ölçümleri boyutları

Azure Relay, Azure Izleyici 'de ölçümler için aşağıdaki boyutları destekler. Boyutları için ölçümlerinizi eklemek isteğe bağlıdır. Ölçümleri, boyutları eklemezseniz, ad alanı düzeyinde belirtilir. 

|Boyut adı|Açıklama|
| ------------------- | ----------------- |
|EntityName| Azure Relay ad alanı altındaki mesajlaşma varlıklarını destekler.|

## <a name="next-steps"></a>Sonraki adımlar

Bkz. [Azure izlemeye genel bakış](../monitoring-and-diagnostics/monitoring-overview.md).

[1]: ./media/relay-metrics-azure-monitor/relay-monitor1.png




