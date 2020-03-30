---
title: Azure Monitör'de Azure Röle ölçümleri | Microsoft Dokümanlar
description: Bu makalede, Azure Röle durumunu izlemek için Azure Monitor'u nasıl kullanabileceğiniz hakkında bilgi verilmektedir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273128"
---
# <a name="azure-relay-metrics-in-azure-monitor"></a>Azure Monitör'de Azure Röle ölçümleri 
Azure Röle ölçümleri, Azure aboneliğinizdeki kaynakların durumunu sağlar. Zengin bir ölçüm veri kümesiyle, Yalnızca ad alanı düzeyinde değil, varlık düzeyinde de Röle kaynaklarınızın genel durumunu değerlendirebilirsiniz. Azure Röle durumunu izlemenize yardımcı oldukları ndan, bu istatistikler önemli olabilir. Ölçümler, Azure desteğine başvurmaya gerek kalmadan kök neden sorunlarının giderilmesine de yardımcı olabilir.

Azure Monitor, çeşitli Azure hizmetlerinde izleme için birleşik kullanıcı arabirimleri sağlar. Daha fazla bilgi için Microsoft [Azure'da İzleme](../monitoring-and-diagnostics/monitoring-overview.md) ve GitHub'da .NET örneği [içeren Azure Monitörünü Al ölçütlerine](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) bakın.

> [!IMPORTANT]
> Bu makale, WCF Relay için değil, yalnızca Azure Röle'nin Karma Bağlantılar özelliği için geçerlidir. 

## <a name="access-metrics"></a>Erişim ölçümleri

Azure Monitor ölçümlere erişmek için birden çok yol sağlar. Azure [portalı](https://portal.azure.com)üzerinden ölçümlere erişebilir veya Azure Monitor API'lerini (REST ve .NET) ve Operasyon Yönetimi Paketi ve Etkinlik Hub'ları gibi çözüm çözümlerini kullanabilirsiniz. Daha fazla bilgi için azure [monitor tarafından toplanan verileri izleme](../azure-monitor/platform/data-platform.md)'ye bakın.

Ölçümler varsayılan olarak etkinleştirilir ve en son 30 günlük verilere erişebilirsiniz. Verileri daha uzun süre saklamanız gerekiyorsa, metrik verileri bir Azure Depolama hesabına arşivleyebilirsiniz. Bu, Azure Monitor'daki [tanılama ayarlarında](../azure-monitor/platform/diagnostic-settings.md) yapılandırılır.

## <a name="access-metrics-in-the-portal"></a>Portaldaki erişim ölçümleri

[Azure portalında](https://portal.azure.com)ölçümleri zaman içinde izleyebilirsiniz. Aşağıdaki örnekte, hesap düzeyinde başarılı isteklerin ve gelen isteklerin nasıl görüntülenenedöneceği gösterilmektedir:

![][1]

Ölçümlere doğrudan ad alanı üzerinden de erişebilirsiniz. Bunu yapmak için, ad alanınızı seçin ve ardından **Ölçüler **'yi tıklatın. 

Boyutları destekleyen ölçümler için, istenen boyut değerine filtre uygulamanız gerekir.

## <a name="billing"></a>Faturalandırma

Azure Monitor'da ölçümleri kullanmak, önizleme sırasında şu anda ücretsizdir. Ancak, ölçümler verilerini sindiren ek çözümler kullanırsanız, bu çözümler tarafından faturalandırılabilir. Örneğin, metrik verileri bir Azure Depolama hesabına arşivlerseniz Azure Depolama tarafından faturalandırılırsınız. Gelişmiş analizler için metrik verileri Azure Monitor günlüklerine aktarıyorsanız, Azure Monitor günlükleri tarafından faturalandırılırsınız.

Aşağıdaki ölçümler, hizmetinizin durumu hakkında genel bir bakış sağlar. 

> [!NOTE]
> Farklı bir ad altında taşındıkları için birkaç ölçümü küçümsüyoruz. Bu, başvurularınızı güncelleştirmenizi gerektirebilir. "Amortismana" anahtar kelimesi ile işaretlenmiş ölçümler ileriye dönük olarak desteklenmeyen.

Tüm ölçüm değerleri her dakika Azure Monitor'a gönderilir. Zaman tanecikleri, ölçüm değerlerinin sunulduğu zaman aralığını tanımlar. Tüm Azure Röle ölçümleri için desteklenen zaman aralığı 1 dakikadır.

## <a name="connection-metrics"></a>Bağlantı ölçümleri

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
| DinleyiciBağlantıları-Başarı  | Belirli bir dönemde Azure Relay'e yapılan başarılı dinleyici bağlantılarının sayısı. <br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|DinleyiciConnections-ClientError |Belirli bir dönemde dinleyici bağlantılarındaki istemci hatalarının sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|DinleyiciConnections-ServerError |Belirli bir dönemde dinleyici bağlantılarındaki sunucu hatalarının sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|GönderenConnections-Başarı |Belirli bir dönemde yapılan başarılı gönderen bağlantı sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|GönderenBağlantılar-İstemci Hatası |Gönderen bağlantılarındaki istemci hatalarının sayısı belirli bir dönemde.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|GönderenConnections-ServerError |Gönderen bağlantılarındaki sunucu hatalarının belirli bir dönemdeki sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|DinleyiciBağlantıları-TotalRequests |Belirli bir dönemdeki dinleyici bağlantılarının toplam sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|GönderenBağlantılar-Toplam İstekler |Gönderenler tarafından belirli bir süre içinde yapılan bağlantı istekleri.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|Aktif Bağlantılar |Etkin bağlantıların sayısı. Bu değer, zaman içinde bir değerdir.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|ActiveListeners |Aktif dinleyici sayısı. Bu değer, zaman içinde bir değerdir.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|Dinleyici Keser |Belirli bir dönemde bağlantısı kesilen dinleyici sayısı.<br/><br/> Birim: Bayt <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|Gönderen Keser |Belirli bir dönemde bağlantısı kesilen gönderenlerin sayısı.<br/><br/> Birim: Bayt <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|

## <a name="memory-usage-metrics"></a>Bellek kullanım ölçümleri

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
|BaytTransfer |Belirli bir süre içinde aktarılan bayt sayısı.<br/><br/> Birim: Bayt <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|

## <a name="metrics-dimensions"></a>Metrik boyutları

Azure Röle, Azure Monitor'daki ölçümler için aşağıdaki boyutları destekler. Ölçümlerinize boyut ekleme isteğe bağlıdır. Boyutlar eklemezseniz, ölçümler ad alanı düzeyinde belirtilir. 

|Boyut adı|Açıklama|
| ------------------- | ----------------- |
|Varlıkadı| Azure Relay, ad alanı altında ileti varlıklarını destekler.|

## <a name="next-steps"></a>Sonraki adımlar

Azure [İzleme genel görünümüne](../monitoring-and-diagnostics/monitoring-overview.md)bakın.

[1]: ./media/relay-metrics-azure-monitor/relay-monitor1.png




