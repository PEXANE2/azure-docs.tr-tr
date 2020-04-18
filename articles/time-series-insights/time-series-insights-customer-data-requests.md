---
title: Müşteri veri istek özellikleri - Azure Time Series Öngörüleri | Microsoft Dokümanlar
description: Azure Time Series Öngörüleri'nde müşteri veri isteği özellikleri hakkında bilgi edinin.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.date: 04/17/2020
ms.topic: conceptual
ms.service: time-series-insights
services: time-series-insights
ms.custom: seodec18
ms.openlocfilehash: 3578710bf066e7745215d8efacafd2cf6c005eac
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81640490"
---
# <a name="summary-of-customer-data-request-features"></a>Müşteri veri seçimi özelliklerinin özeti

Azure Time Series Öngörüleri, milyarlarca olayı aynı anda yutmayı, depolamayı, keşfetmeyi ve analiz etmeyi kolaylaştıran depolama, analiz ve görselleştirme bileşenlerine sahip yönetilen bir bulut hizmetidir.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Bir veri konusu isteğine tabi olabilecek kişisel verileri görüntülemek, dışa aktarmak ve silmek için Azure Time Series Insights kiracı yöneticisi Azure portalını veya REST API'lerini kullanabilir. Veri konu isteklerini hizmet etebetmek için Azure portalının kullanılması, çoğu kullanıcının tercih ettiği bu işlemleri gerçekleştirmek için daha az karmaşık bir yöntem sağlar.

## <a name="identifying-customer-data"></a>Müşteri verilerinin tanımlanması

Azure Time Series Öngörüleri, kişisel verileri Time Series Insights yöneticileri ve kullanıcılarıyla ilişkili veriler olarak kabul eder. Time Series Insights, ortama erişimi olan kullanıcıların Azure Active Directory nesne kimliğini depolar. Azure portalı kullanıcı e-posta adreslerini görüntüler, ancak bu e-posta adresleri Zaman Serisi Öngörüleri içinde depolanmaz, Azure Active Directory'deki Azure Active Directory nesne kimliği kullanılarak dinamik olarak aranır.

## <a name="deleting-customer-data"></a>Müşteri verilerini silme

Kiracı yönetici, Azure portalını kullanarak müşteri verilerini silebilir.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

Ancak, portal üzerinden müşteri verilerini silmeden önce, kullanıcının erişim ilkelerini Azure portalındaki Zaman Serisi Öngörüleri ortamından kaldırmanız gerekir. Daha fazla bilgi için [Azure portalLarını kullanarak Time Series Insights ortamına veri erişimi ver'i](time-series-insights-data-access.md)okuyun.

Ayrıca REST API'yi kullanarak erişim ilkelerinde silme işlemleri de gerçekleştirebilirsiniz. Daha fazla bilgi için [Erişim İlkeleri - Sil '](https://docs.microsoft.com/rest/api/time-series-insights/management/accesspolicies/delete)i okuyun.

Zaman Serisi Öngörüleri, Azure portalındaki İlke bıçağıyla entegre edilmiştir. Hem Zaman Serisi Öngörüleri hem de İlke bıçağı, hizmet içinde depolanan kullanıcı verilerini görüntülemenizi, dışa aktarmanızı ve silmenizi sağlar. Azure portalının İlke kılıcı içinde gerçekleştirilen herhangi bir silme işlemi, Zaman Serisi Öngörüleri içinde kullanıcı verilerinin silinmesine neden olabilir. Örneğin, bir kullanıcının kaydedilmiş bir kişisel sorgusu varsa, bu sorgu Zaman Serisi Öngörüleri gezgininden kalıcı olarak silinir. Kullanıcının kaydedilmiş paylaşılan bir sorgusu varsa, sorgu devam eder, ancak kullanıcı bilgileri kalıcı olarak silinir. Aşağıdaki not, bu görevlerin nasıl yerine getirilenene ilişkin yönergeleri içerir.

## <a name="exporting-customer-data"></a>Müşteri verilerini dışa aktarma

Verileri silmeye benzer şekilde, kiracı yönetici Azure portalındaki İlke bıçağından Time Series Öngörüleri'nde depolanan verileri görüntüleyebilir ve dışa aktarabilir.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

Kiracı yöneticiyseniz, Azure portalında Zaman Serisi Öngörüleri ortamında veri erişim ilkelerini görüntüleyebilirsiniz. Daha fazla bilgi için [Azure portalLarını kullanarak Time Series Insights ortamına veri erişimi ver'i](time-series-insights-data-access.md)okuyun.

Sağlanan REST API'deki "çevreye göre liste" işlemini kullanarak erişim ilkelerinde dışa aktarma işlemleri gerçekleştirmek de mümkündür. Daha fazla bilgi için [Erişim İlkeleri - Çevreye Göre Listele'yi](https://docs.microsoft.com/rest/api/time-series-insights/management/accesspolicies/listbyenvironment)okuyun.

## <a name="to-delete-data-stored-within-time-series-insights"></a>Zaman Serisi Öngörüleri içinde depolanan verileri silmek için

Kişisel veriler, kullanıcı ve yönetici verilerinden farklı bir senaryo olan Time Series Insights depolamasına giriş yapabilir. Time Series Insights'ta depolanan verileri kişisel veri olarak görürseniz, aşağıdaki adımları kullanarak bu verileri dışa aktarabilir ve silebilirsiniz:

**Verileri görüntüleme ve dışa aktarma**

Zaman Serisi Öngörüleri içinde depolanan verileri görüntülemek ve dışa aktarmak için bu verileri aramanız gerekir. Verileri görüntülemek ve dışa aktarmak için Zaman Serisi Öngörüler gezgini veya Time Series Insights sorgu API'lerini kullanabilirsiniz. Zaman Serisi Öngörüler gezginini kullanarak verileri görüntülemek ve dışa aktarmak için, önce söz konusu kullanıcı verilerini bulmak için arama yapın. Arama yaptıktan sonra grafiğe sağ tıklayın ve **Olayları Keşfet'i**seçin. Olaylar ızgarası görünür ve verileri CSV ve JSON olarak dışa aktarma seçenekleri sunar.

Daha fazla bilgi için [Azure Time Series Insights explorer'ı](time-series-insights-explorer.md)okuyun.

**Verileri silme**

Şu anda, Time Series Insights verilerin parçalı silinmesini desteklemez. Ancak, Time Series Insights, bekletme ilkelerini yapılandırarak Time Series Öngörüleri içinde depolanan müşteri verilerini kaldırma olanağı sağlar. Silme gereksinimlerinizi desteklemek için tüm Time Series Öngörüleri ortamının bekletme süresini istediğiniz gün sayısına ayarlayabilirsiniz.

Daha fazla bilgi [için, Zaman Serisi Öngörüleri'nde Yapılandırma bekletme'yi](time-series-insights-how-to-configure-retention.md)okuyun.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Time Series Öngörüleri ortamınıza veri erişimi verme](./time-series-insights-data-access.md)hakkında daha fazla bilgi edinin.

* Azure [Zaman Serisi Öngörüleri kaşifini](time-series-insights-explorer.md)görüntüleyin.

* Zaman [Serisi Öngörülerinde bekletme yapılandırma](time-series-insights-how-to-configure-retention.md)hakkında bilgi edinin.