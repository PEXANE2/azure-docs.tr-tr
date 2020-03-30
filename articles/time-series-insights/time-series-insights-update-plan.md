---
title: Önizleme ortamınızı planlayın - Azure Time Series Öngörüleri | Microsoft Dokümanlar
description: Azure Zaman Serisi Öngörüleri Önizleme ortamınızı yapılandırmak, yönetmek, planlamak ve dağıtmak için en iyi uygulamalar.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 01/15/2020
ms.custom: seodec18
ms.openlocfilehash: b16d78c9670d05fcec8126c5544d1dd97f6a03bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76045713"
---
# <a name="plan-your-azure-time-series-insights-preview-environment"></a>Azure Zaman Serisi Öngörüleri Önizleme ortamınızı planlayın

Bu makalede, Azure Zaman Serisi Öngörüleri Önizlemesini kullanarak planlamak ve hızlı bir şekilde başlamak için en iyi uygulamalar açıklanmaktadır.

> [!NOTE]
> Genel kullanılabilirlik Zaman Serisi Öngörüleri örneğini planlamak için en iyi uygulamalar için [Azure Zaman Serisi Öngörüleri genel kullanılabilirlik ortamınızı planlayın'ı](time-series-insights-environment-planning.md)okuyun.

## <a name="best-practices-for-planning-and-preparation"></a>Planlama ve hazırlık için en iyi uygulamalar

Çevreniziçin planlama ve hazırlama ile ilgili en iyi uygulamalar aşağıdaki makalelerde daha ayrıntılı olarak açıklanmıştır:

* [Bir Zaman Serisi Öngörüleri Önizleme ortamını sağlarken](#the-preview-environment)ne elde esiniz.
* Zaman [Serisi disve Timestamp özellikleriniz nelerdir.](#configure-time-series-ids-and-timestamp-properties)
* Yeni [Zaman Serisi Modeli](#understand-the-time-series-model)nedir ve nasıl kendi oluşturmak için.
* [JSON verimli olaylar göndermek](#shape-your-events)için nasıl .
* Time Serisi Insights [iş felaket kurtarma seçenekleri](#business-disaster-recovery).

Azure Time Series Insights, istediğiniz kadar öde iş modeli kullanır. Ücretler ve kapasite hakkında daha fazla bilgi için [Time Series Insights fiyatlandırmasını](https://azure.microsoft.com/pricing/details/time-series-insights/)okuyun.

## <a name="the-preview-environment"></a>Önizleme ortamı

Bir Time Series Öngörüleri Önizleme ortamı oluşturduğunuzda, iki Azure kaynağı oluşturursunuz:

* Azure Zaman Serisi Öngörüleri Önizleme ortamı
* Azure Depolama genel amaçlı V1 hesabı

Sağlama işleminin bir parçası olarak, sıcak bir depoyu etkinleştirmek isteyip istemediğinizbelirtilir. Sıcak mağaza size katmanlı bir sorgu deneyimi sağlar. Etkinleştirildiğinde, 7 ile 30 gün arasında bir bekletme süresi belirtmeniz gerekir. Sıcak mağaza bekletme süresi içinde yürütülen sorgular genellikle daha hızlı yanıt süreleri sağlar. Bir sorgu sıcak mağaza saklama dönemine yayıldığında, soğuk mağazadan servis edilir.

Sıcak mağazadaki sorgular ücretsizdir, soğuk depodaki sorgular ise maliyete tabidir. Sorgu desenlerinizi anlamak ve sıcak mağaza yapılandırmanızı buna göre planlamak önemlidir. En son verilerdeki etkileşimli analizlerin sıcak mağazanızda ve desen analizlerinizde ve uzun vadeli eğilimlerde soğukta bulunmanızı öneririz.

> [!NOTE]
> Sıcak verilerinizi nasıl sorgulayacağım hakkında daha fazla bilgi için [API Başvurusu'nu](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#uri-parameters)okuyun.

Başlamak için üç ek öğe gerekir:

* [Bir Zaman Serisi Modeli](./time-series-insights-update-tsm.md)
* [Time Series Insights'a bağlı bir etkinlik kaynağı](./time-series-insights-how-to-add-an-event-source-iothub.md)
* [Hem modele](./time-series-insights-send-events.md) eşlenen hem de geçerli JSON biçiminde olan olay kaynağına akan olaylar

## <a name="review-preview-limits"></a>Önizleme sınırlarını gözden geçirme

[!INCLUDE [Review Time Series Insights Preview limits](../../includes/time-series-insights-preview-limits.md)]

## <a name="configure-time-series-ids-and-timestamp-properties"></a>Zaman Serisi dislerini ve Zaman Damgası özelliklerini yapılandırma

Yeni bir Time Series Öngörüleri ortamı oluşturmak için bir Zaman Serisi Kimliği seçin. Bunu yapmak, verileriniz için mantıksal bir bölüm görevi görür. Belirtildiği gibi, Zaman Serisi dislerinizi hazır olduğundan emin olun.

> [!IMPORTANT]
> Zaman Serisi iYe'leri *daha sonra değiştirilemez.* Son seçim den ve ilk kullanımdan önce her birini doğrulayın.

Kaynaklarınızı benzersiz bir şekilde farklılaştırmak için en fazla üç anahtar seçebilirsiniz. Daha fazla bilgi [için, Zaman Serisi Kimliği](./time-series-insights-update-how-to-id.md) ve Depolama [ve giriş](./time-series-insights-update-storage-ingress.md)seçmek için en iyi uygulamaları okuyun.

**Timestamp** özelliği de önemlidir. Olay kaynakları eklediğinizde bu özelliği belirleyebilirsiniz. Her olay kaynağı, zaman içinde olay kaynaklarını izlemek için kullanılan isteğe bağlı bir Timestamp özelliğine sahiptir. Zaman damgası değerleri büyük/küçük harf duyarlıdır ve her olay kaynağının ayrı belirtimine biçimlendirilmelidir.

> [!TIP]
> Etkinlik kaynaklarınız için biçimlendirme ve ayrıştırma gereksinimlerini doğrulayın.

Boş bırakıldığında, olay kaynağının Olay Enqueue Zamanı olay Zaman Damgası olarak kullanılır. Geçmiş verileri veya toplu etkinlikler gönderirseniz, Timestamp özelliğini özelleştirmek varsayılan Olay Enqueue Süresi'nden daha yararlıdır. Daha fazla bilgi için [Azure IoT Hub'ına etkinlik kaynaklarının](./time-series-insights-how-to-add-an-event-source-iothub.md)nasıl ekleyeceğiniz hakkında bilgi edinin.

## <a name="understand-the-time-series-model"></a>Zaman Serisi Modelini Anlayın

Artık Time Series Insights ortamınızın Zaman Serisi Modelini yapılandırabilirsiniz. Yeni model, IoT verilerini bulmayı ve analiz etmeyi kolaylaştırır. Zaman serisi verilerinin kürasyonunu, bakımını ve zenginleşmesini sağlar ve tüketiciye hazır veri kümelerinin hazırlanmasına yardımcı olur. Model, benzersiz kaynağı türler ve hiyerarşiler olarak bilinen değişkenlerle ilişkilendiren bir örneğin eşlenebilen Zaman Serisi kimlikleri kullanır. Yeni Zaman [Serisi Modeli](./time-series-insights-update-tsm.md)hakkında bilgi edinin.

Model dinamiktir, bu nedenle her zaman oluşturulabilir. Hızlı bir şekilde başlamak için, zaman serisi öngörüleri içine veri itme önce oluşturmak ve yükleyin. Modelinizi oluşturmak için [Zaman Serisi Modelini Kullan'ı](./time-series-insights-update-how-to-tsm.md)okuyun.

Birçok müşteri için, Zaman Serisi Modeli zaten mevcut bir varlık modeli veya ERP sistemi yle eşler. Varolan bir modeliniz yoksa, hızlı bir şekilde çalışmaya devam etmek için önceden oluşturulmuş bir kullanıcı deneyimi [sağlanır.](https://github.com/Microsoft/tsiclient) Bir modelin size nasıl yardımcı olabileceğini tasavvur etmek için [örnek demo ortamını](https://insights.timeseries.azure.com/preview/demo)görüntüleyin.

## <a name="shape-your-events"></a>Etkinliklerinizi şekillendirin

Zaman Serisi Öngörüleri'ne olayları nasıl gönderdiğinizi doğrulayabilirsiniz. İdeal olarak, olayları iyi ve verimli bir şekilde denormalize edilir.

Başparmak iyi bir kural:

* Meta verileri Time Series Modelinizde depolayın.
* Zaman Serisi Modu, örnek alanları ve olayların yalnızca Zaman Serisi Kimliği veya Timestamp özelliği gibi gerekli bilgileri içerdiğinden emin olun.

Daha fazla bilgi [için, Şekil olayları](./time-series-insights-send-events.md#supported-json-shapes)okuyun.

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Sonraki adımlar

- İş kurtarma yapılandırma seçeneklerinizi planlamak için [Azure Danışmanı'nı](../advisor/advisor-overview.md) gözden geçirin.
- Zaman Serisi Öngörüleri Önizlemesinde [depolama ve giriş](./time-series-insights-update-storage-ingress.md) hakkında daha fazla bilgi edinin.
- Zaman Serisi Öngörüleri Önizlemesi'nde [veri modelleme](./time-series-insights-update-tsm.md) hakkında bilgi edinin.
