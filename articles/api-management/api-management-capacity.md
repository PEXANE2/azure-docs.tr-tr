---
title: Azure API Yönetimi örneğinin kapasitesi | Microsoft Dokümanlar
description: Bu makalede, kapasite ölçümünün ne olduğu ve bir Azure API Yönetimi örneğini ölçeklendirilip ölçeklendirilip ölçeklendirilmeyeceği konusunda bilinçli kararların nasıl verileceği açıklanmaktadır.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 06/18/2018
ms.author: apimpm
ms.custom: fasttrack-edit
ms.openlocfilehash: b6d949b50be348e72cedfc3710383308b04de106
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336000"
---
# <a name="capacity-of-an-azure-api-management-instance"></a>Azure API Management örneği kapasitesi

**Kapasite,** daha fazla yük barındıracak şekilde bir API Yönetimi örneğini ölçeklendirilip ölçeklendirmemeye karar vermek için bilinçli kararlar almak için en önemli [Azure Monitor ölçüsüdür.](api-management-howto-use-azure-monitor.md#view-metrics-of-your-apis) Yapısı karmaşıktır ve bazı davranışlar dayatmaz.

Bu makalede, **kapasitenin** ne olduğu ve nasıl nasıl olduğu açıklanmaktadır. Azure portalında **kapasite** ölçümlerine nasıl erişileceğinizi gösterir ve API Yönetimi örneğini ölçekleme veya yükseltmeyi ne zaman düşünmeniz gerektiğini önerir.

> [!IMPORTANT]
> Bu makalede, Azure API Yönetimi örneğini kapasite ölçümüne göre nasıl izleyip ölçeklendirebileceğiniz açıklanmaktadır. Ancak, tek bir API Yönetimi örneği gerçekten kapasitesine *ulaştığında* ne olduğunu anlamak da aynı derecede önemlidir. Azure API Yönetimi, örneklerin fiziksel aşırı yüklenmesini önlemek için hizmet düzeyinde azaltma uygulamaz. Bir örnek fiziksel kapasitesine ulaştığında, gelen istekleri işleyemeyen aşırı yüklü web sunucusuna benzer şekilde çalışacaktır: gecikme süresi artacak, bağlantılar kesilecek, zaman ekme hataları, vb. Bu, API istemcilerinin diğer dış hizmetlerdeki gibi bu olasılıkla başa çıkmak için hazırlıklı olması gerektiği anlamına gelir (örn. yeniden deneme ilkeleri uygulayarak).

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları izlemek için şunları yapmış olmalısınız:

+ Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Bir APIM örneği. Daha fazla bilgi için [bkz.](get-started-create-service-instance.md)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="what-is-capacity"></a>Kapasite nedir

![Kapasite ölçümü](./media/api-management-capacity/capacity-ingredients.png)

**Kapasite,** API Yönetimi örneğindeki yükün bir göstergesidir. Kaynak kullanımını (CPU, bellek) ve ağ sıra uzunluklarını yansıtır. CPU ve bellek kullanımı kaynak tüketimini şu şekilde ortaya çıkarır:

+ İstekleri iletme veya bir ilke çalıştırmayı içerebilen istek işleme gibi API Yönetimi veri düzlemi hizmetleri.
+ Azure Portalı veya ARM üzerinden uygulanan yönetim eylemleri veya [geliştirici portalından](api-management-howto-developer-portal.md)gelen yük gibi API Yönetimi yönetimi düzlem hizmetleri.
+ Yeni bağlantılarda TLS el sıkışma maliyetini içeren işlemler de dahil olmak üzere seçili işletim sistemi işlemleri.

Toplam **kapasite,** bir API Yönetimi örneğinin her biriminden kendi değerlerinin ortalamasıdır.

Kapasite **ölçümü** API Yönetimi örneğinizle ilgili sorunları yüzeye çıkarmak için tasarlanmış olsa da, sorunların **kapasite ölçümündeki**değişikliklere yansıtılmayacak durumlar vardır.

## <a name="capacity-metric-behavior"></a>Kapasite metrik davranışı

Yapısı nedeniyle, gerçek hayatta **kapasite** birçok değişkenden etkilenebilir, örneğin:

+ bağlantı desenleri (bir istekteki yeni bağlantı vs varolan bağlantıyı yeniden kullanma)
+ bir istek ve yanıt boyutu
+ her API'da veya istek gönderen istemci sayısında yapılandırılan ilkeler.

İstekler üzerinde ne kadar karmaşık işlemler yapılırsa, **kapasite** tüketimi de o kadar yüksek olur. Örneğin, karmaşık dönüşüm ilkeleri, basit bir istek iletmesinden çok daha fazla CPU tüketir. Yavaş arka uç hizmet yanıtları da artacaktır.

> [!IMPORTANT]
> **Kapasite,** işlenen istek sayısının doğrudan bir ölçüsü değildir.

![Kapasite metrik ani artışları](./media/api-management-capacity/capacity-spikes.png)

**Kapasite,** işlenmemiş istek ler olmasa bile aralıklı olarak yükselebilir veya sıfırdan büyük olabilir. Sistem veya platforma özgü eylemler nedeniyle gerçekleşir ve bir örneği ölçeklendirilip ölçeklendirmemeye karar verirken dikkate alınmamalıdır.

Düşük **kapasiteli metrik,** API Yönetimi örneğinizin herhangi bir sorun yaşamadığı anlamına gelmez.
  
## <a name="use-the-azure-portal-to-examine-capacity"></a>Kapasiteyi incelemek için Azure Portalı'nı kullanın
  
![Kapasite ölçümü](./media/api-management-capacity/capacity-metric.png)  

1. [Azure portalındaki](https://portal.azure.com/)APIM örneğinize gidin.
2. **Ölçümler**’i seçin.
3. Mor bölümden, kullanılabilir ölçümlerden **Kapasite** ölçümünü seçin ve varsayılan **Avg** toplamayı bırakın.

    > [!TIP]
    > Yanlış yorumlardan kaçınmak için her zaman konum başına **kapasite** metrik dökümüne bakmalısınız.

4. Yeşil bölümden, metrik boyutuna göre bölmek için **Konum'u** seçin.
5. Bölümün üst çubuğundan istediğiniz zaman dilimini seçin.

    Beklenmeyen bir şey olduğunda size bildirmek için bir metrik uyarı ayarlayabilirsiniz. Örneğin, APIM örneğiniz 20 dakikadan uzun süredir beklenen en yüksek kapasiteyi aştığında bildirimler alın.

    >[!TIP]
    > Hizmetinizin kapasitesi nin ne zaman azaldığını bildirmek için uyarıları yapılandırabilir veya otomatik olarak bir Azure API Yönetimi birimi eklemek için Azure Monitor otomatik ölçekleme işlevini kullanabilirsiniz. Ölçekleme işlemi yaklaşık 30 dakika sürebilir, bu nedenle kurallarınızı buna göre planlamanız gerekir.  
    > Yalnızca ana konumu ölçekleme izin verilir.

## <a name="use-capacity-for-scaling-decisions"></a>Kararları ölçekleme için kapasite kullanma

**Kapasite,** daha fazla yük barındıracak bir API Yönetimi örneğini ölçeklendirilip ölçeklendirmemeye karar vermek için alınan ölçüttür. Aşağıdakileri dikkate alın:

+ Uzun vadeli bir eğilim ve ortalama bakıyor.
+ Büyük olasılıkla yükteki herhangi bir artışla ilgili olmayan ani ani ani artışları göz ardı etmek (açıklama için "Kapasite metrik davranışı" bölümüne bakın).
+ **Kapasitenin**değeri daha uzun bir süre için %60 veya %70'i aştığında (örneğin 30 dakika) örneğin, örneğin imalinizi yükseltme veya ölçekleme. Hizmetiniz veya senaryonuz için farklı değerler daha iyi çalışabilir.

>[!TIP]  
> Trafiğinizi önceden tahmin edebildiyseniz, APIM örneğini beklediğiniz iş yüklerine göre test edin. Kiracınızdaki istek yükünü kademeli olarak artırabilir ve kapasite ölçümünün hangi değerinin en yüksek yükünüze karşılık olduğunu izleyebilirsiniz. Belirli bir anda ne kadar kapasite kullanıldığını anlamak için Azure portalını kullanmak için önceki bölümdeki adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure API Yönetimi hizmeti örneğini ölçeklendirme veya yükseltme](upgrade-and-scale.md)