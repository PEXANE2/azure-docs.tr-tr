---
title: Azure API Yönetimi örneğini yükseltme ve ölçeklendirme | Microsoft Dokümanlar
description: Bu konu, Bir Azure API Yönetimi örneğinin nasıl yükseltileceği ve ölçeklendirileceği açıklanmaktadır.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/18/2018
ms.author: apimpm
ms.openlocfilehash: 64649c86dbd3c3469247308bfc4dd0ed12e06949
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70018230"
---
# <a name="upgrade-and-scale-an-azure-api-management-instance"></a>Azure API Yönetimi örneğini yükseltme ve ölçeklendirme  

Müşteriler, birimler ekleyerek ve kaldırarak bir Azure API Yönetimi (APIM) örneğini ölçeklendirebilir. Bir **birim** özel Azure kaynaklarından oluşur ve aylık bir dizi API çağrısı olarak ifade edilen belirli bir yük taşıma kapasitesine sahiptir. Bu sayı bir çağrı sınırını değil, kaba kapasite planlamasına izin verecek maksimum bir verim değerini temsil eder. Gerçek iş ortası ve gecikme sayının sayısı ve eşzamanlı bağlantı hızı, yapılandırılan ilkelerin türü ve sayısı, istek ve yanıt boyutları ve arka uç gecikmesi gibi etkenlere bağlı olarak büyük ölçüde değişir.

Her birimin kapasitesi ve fiyatı, birimin bulunduğu **katmana** bağlıdır. Dört katman arasında seçim yapabilirsiniz: **Geliştirici,** **Temel,** **Standart,** **Premium**. Bir katmandaki bir hizmetin kapasitesini artırmanız gerekiyorsa, bir birim eklemeniz gerekir. APIM örneğinde şu anda seçilen katman daha fazla birim eklenmesine izin vermiyorsa, daha üst düzey bir katmana yükseltmeniz gerekir.

Her birimin fiyatı ve kullanılabilir özellikler (örneğin, çok bölgeli dağıtım) APIM örneğiniz için seçtiğiniz katmana bağlıdır. [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) makalesi, birim başına fiyatı ve her katmanda elde ettiğiniz özellikleri açıklar. 

>[!NOTE]
>[Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) makalesi, her katmandaki yaklaşık birim kapasite sayısını gösterir. Daha doğru sayılar elde etmek için API'leriniz için gerçekçi bir senaryoya bakmanız gerekir. Azure [API Yönetimi örnek makalesinin Kapasitesin'e](api-management-capacity.md) bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları izlemek için şunları

+ Etkin bir Azure aboneliğine sahip olun.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ APIM örneği var. Daha fazla bilgi için [bkz.](get-started-create-service-instance.md)

+ [Azure API Yönetimi örneğinin Kapasite](api-management-capacity.md)kavramını anlayın.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="upgrade-and-scale"></a>Yükseltme ve ölçeklendirme  

Dört katman arasında seçim yapabilirsiniz: **Geliştirici,** **Temel,** **Standart** ve **Premium.** **Geliştirici** katmanı hizmeti değerlendirmek için kullanılmalıdır; üretim için kullanılmamalıdır. **Geliştirici** katmanında SLA yoktur ve bu katmanı ölçeklendiremezsiniz (birimleri ekleme/kaldırma). 

**Temel,** **Standart** ve **Premium,** SLA'ya sahip ve ölçeklendirilebilen üretim katmanlarıdır. **Temel** katman, SLA'ya sahip en ucuz katmandır ve 2 adede kadar ölçeklendirilebilir, **Standart** katman dört adede kadar ölçeklendirilebilir. **Premium** katmanına istediğiniz sayıda birim ekleyebilirsiniz.

**Premium** katman, tek bir Azure API Yönetimi örneğini istediğiniz sayıda Azure bölgesinde dağıtmanıza olanak tanır. Başlangıçta bir Azure API Yönetimi hizmeti oluşturduğunuzda, örnek yalnızca bir birim içerir ve tek bir Azure bölgesinde bulunur. İlk bölge **birincil** bölge olarak belirlenmiştir. Ek bölgeler kolayca eklenebilir. Bir bölge eklerken, ayırmak istediğiniz birim sayısını belirtirsiniz. Örneğin, **birincil** bölgede bir birim ve başka bir bölgede beş birim olabilir. Birim sayısını her bölgedeki trafiğe göre uyarlayabilirsiniz. Daha fazla bilgi için, [birden çok Azure bölgesine Azure API Yönetimi hizmeti örneğini nasıl dağıtılayabilirsiniz'](api-management-howto-deploy-multi-region.md)e bakın.

Herhangi bir katmana yükseltme ve düşürme yapabilirsiniz. Yükseltme veya düşürmenin Premium katmandan Standard veya Basic'e düşürüldüğünde VNET'ler veya çok bölgeli dağıtım gibi bazı özellikleri kaldırabileceğini unutmayın.

> [!NOTE]
> Yükseltme veya ölçek lendirme işleminin uygulanması 15 ila 45 dakika sürebilir. Bittiğinde size bildirilir.

> [!NOTE]
> **Tüketim** katmanındaki API Yönetimi hizmeti, trafiğe göre otomatik olarak ölçeklendirilir.

## <a name="use-the-azure-portal-to-upgrade-and-scale"></a>Yükseltme ve ölçeklendirme için Azure portalını kullanın

![Azure portalında APIM'yi ölçeklendirin](./media/upgrade-and-scale/portal-scale.png)

1. [Azure portalındaki](https://portal.azure.com/)APIM örneğinize gidin.
2. Menüden **Ölçek ve fiyatlandırma'yı** seçin.
3. İstediğini seçin.
4. Eklemek istediğiniz **birim** sayısını belirtin. Kaydırıcıyı kullanabilir veya birim sayısını yazabilirsiniz.  
    **Premium** katmanı seçerseniz, öncelikle bir bölge seçmeniz gerekir.
5. **Kaydet**’e basın.

## <a name="downtime-during-scaling-up-and-down"></a>Yukarı ve aşağı ölçekleme sırasında çalışmama süresi
Geliştirici katmanından veya Geliştirici katmanından ölçekleme yapıyorsunuz, kapalı kalma süresi olacaktır. Aksi takdirde, hiçbir kesinti yoktur. 


## <a name="next-steps"></a>Sonraki adımlar

- [Bir Azure API Management hizmeti örneğini birden fazla Azure bölgesine dağıtma](api-management-howto-deploy-multi-region.md)
- [Azure API Yönetimi hizmeti örneğini otomatik olarak ölçeklendirme](api-management-howto-autoscale.md)
