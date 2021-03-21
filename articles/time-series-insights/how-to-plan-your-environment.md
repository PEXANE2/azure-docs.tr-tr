---
title: Gen2 ortamınızı planlayın-Azure Time Series Insights | Microsoft Docs
description: Azure Time Series Insights Gen2 ortamınızı yapılandırmak, yönetmek, planlamak ve dağıtmak için en iyi uygulamalar.
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 09/30/2020
ms.custom: seodec18
ms.openlocfilehash: c8d96d7b5952c020493af278ee1ea8ad5ff46716
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "95016794"
---
# <a name="plan-your-azure-time-series-insights-gen2-environment"></a>Azure Time Series Insights Gen2 ortamınızı planlayın

Bu makalede Azure Time Series Insights Gen2 kullanarak hızlı bir şekilde planlamak ve kullanmaya başlamak için en iyi yöntemler açıklanmaktadır.

## <a name="best-practices-for-planning-and-preparation"></a>Planlama ve hazırlık için en iyi uygulamalar

Ortamınızı planlamaya ve hazırlamaya yönelik en iyi uygulamalar aşağıdaki makalelerde daha ayrıntılı olarak açıklanmıştır:

* [Azure Time Series Insights Gen2 ortamı sağladığınızda](#the-gen2-environment)alacağınız Özellikler.
* [Zaman serisi kimlikleriniz ve zaman damgası özellikleridir](#configure-time-series-ids-and-timestamp-properties).
* Yeni [zaman serisi modelinin ne olduğu](#understand-the-time-series-model)ve kendinizinkini nasıl oluşturabileceğiniz.
* [JSON 'da olayları verimli](#shape-your-events)bir şekilde gönderme.
* [İş olağanüstü durum kurtarma seçeneklerini](#business-disaster-recovery)Azure Time Series Insights.

Azure Time Series Insights Kullandıkça Öde iş modelini kullanır. Ücretler ve kapasite hakkında daha fazla bilgi için [Azure Time Series Insights fiyatlandırmayı](https://azure.microsoft.com/pricing/details/time-series-insights/)okuyun.

## <a name="the-gen2-environment"></a>Gen2 ortamı

Bir Azure Time Series Insights Gen2 ortamı sağladığınızda iki Azure kaynağı oluşturursunuz:

* Bir Azure Time Series Insights Gen2 ortamı
* Bir Azure depolama hesabı

Sağlama sürecinin bir parçası olarak, bir ısınma deposunu etkinleştirmek isteyip istemediğinizi belirlersiniz. Sıcak mağaza size katmanlı bir sorgu deneyimi sağlar. Etkinleştirildiğinde, 7 ila 30 gün arasında bir saklama süresi belirtmeniz gerekir. Sıcak mağaza Bekletme dönemi içinde yürütülen sorgular genellikle daha hızlı yanıt süreleri sağlar. Bir sorgu, yarı zamanlı saklama süresi üzerinden yayıldığında, bu, soğuk depodan sunulur.

Isınma mağazasındaki sorgular ücretsizdir, ancak soğuk depodaki sorgular maliyetlendirilir. Sorgu desenlerinizi anlamanız ve ısınma Mağazası yapılandırmanızı buna göre planlamanız önemlidir. En son veriler üzerinde etkileşimli analizler 'in, ısınma mağazalarınızın yanı sıra, uzun süreli eğilimler soğuk bir şekilde bulunmasını öneririz.

> [!NOTE]
> Isınma verilerinizi sorgulama hakkında daha fazla bilgi edinmek için [API başvurusunu](/rest/api/time-series-insights/dataaccessgen2/query/execute#uri-parameters)okuyun.

Başlamak için üç ek öğe gereklidir:

* [Zaman serisi modeli](./concepts-model-overview.md)
* [Time Series Insights bağlı bir olay kaynağı](./concepts-streaming-ingestion-event-sources.md)
* Hem modele eşlenmiş hem de geçerli JSON biçiminde olan [olay kaynağına akan olaylar](./time-series-insights-send-events.md)

## <a name="review-azure-time-series-insights-gen2-limits"></a>Azure Time Series Insights Gen2 sınırlarını gözden geçirin

[!INCLUDE [Review Azure Time Series Insights Gen2 limits](../../includes/time-series-insights-preview-limits.md)]

## <a name="configure-time-series-ids-and-timestamp-properties"></a>Zaman serisi kimliklerini ve zaman damgası özelliklerini yapılandırma

Yeni bir Azure Time Series Insights ortamı oluşturmak için bir zaman serisi KIMLIĞI seçin. Bunun yapılması verileriniz için bir mantıksal bölüm işlevi görür. Belirtildiği gibi, zaman serisi kimliklerinizi hazırlayın.

> [!IMPORTANT]
> Zaman serisi kimlikleri *daha sonra değiştirilemez*. Son seçim ve ilk kullanmadan önce her birini doğrulayın.

Kaynaklarınızı benzersiz şekilde ayırt etmek için en fazla üç anahtar seçebilirsiniz. Daha fazla bilgi için, [zaman SERISI kimliği ve alım kuralları seçmek üzere en iyi uygulamaları](./how-to-select-tsid.md) okuyun. [](concepts-json-flattening-escaping-rules.md)

**Timestamp** özelliği de önemlidir. Olay kaynaklarını eklediğinizde bu özelliği belirleyebilirsiniz. Her olay kaynağında, olay kaynaklarını zaman içinde izlemek için kullanılan isteğe bağlı bir zaman damgası özelliği vardır. Zaman damgası değerleri büyük/küçük harfe duyarlıdır ve her bir olay kaynağının tek bir belirtimine biçimlendirilmelidir.

Boş bırakıldığında, olayın IoT Hub sırada sıraya alındığı veya Olay Hub 'ı olay zaman damgası olarak kullanıldığı zaman. Genel olarak, kullanıcılar zaman damgası özelliğini özelleştirmeyi ve algılayıcı ya da etiketin Merkez sıraya alma süresi yerine okumayı oluşturduğu zamanı kullanması gerekir. Daha fazla bilgi edinmek ve zaman dilimi uzaklıklarını okuma [olay kaynağı zaman damgasını](./concepts-streaming-ingestion-event-sources.md#event-source-timestamp)okuyun.

## <a name="understand-the-time-series-model"></a>Zaman serisi modelini anlama

Artık Azure Time Series Insights ortamınızın zaman serisi modelini yapılandırabilirsiniz. Yeni model IoT verilerini bulmayı ve çözümlemeyi kolaylaştırır. Zaman serisi verilerinin listesini alma, bakım ve zenginleştirmeye olanak sağlar ve tüketiciye yönelik veri kümelerini hazırlamaya yardımcı olur. Model, benzersiz kaynağı türler ve hiyerarşiler olarak bilinen değişkenlerle ilişkilendiren bir örnekle eşlenen zaman serisi kimliklerini kullanır. Daha fazla bilgi edinmek için [zaman serisi modeline](./concepts-model-overview.md) genel bakış konusunu okuyun.

Model dinamiktir, bu nedenle herhangi bir zamanda derlenebilir. Hızlı bir başlangıç yapmak için, Azure Time Series Insights verileri dağıtmadan önce derleyin ve karşıya yükleyin. Modelinizi derlemek için [zaman serisi modelini kullanın](./concepts-model-overview.md)' ı okuyun.

Birçok müşteri için, zaman serisi modeli mevcut bir varlık modeliyle veya ERP sistemiyle eşlenir. Mevcut bir modeliniz yoksa, hızlı bir şekilde çalışmaya başlaması için önceden oluşturulmuş bir kullanıcı deneyimi [sağlanır](https://github.com/Microsoft/tsiclient) . Bir modelin size nasıl yardımcı olabileceğini öğrenmek için [örnek Tanıtım ortamını](https://insights.timeseries.azure.com/preview/demo)görüntüleyin.

## <a name="shape-your-events"></a>Olaylarınızı şekillendirin

Azure Time Series Insights olayları gönderme yolunu doğrulayabilirsiniz. İdeal olarak, olaylarınızın iyi ve verimli bir şekilde etkili olması gerekir.

Parmak için iyi bir kural:

* Meta verileri zaman serisi modelinize depolayın.
* Zaman serisi modunun, örnek alanlarının ve olayların yalnızca gerekli bilgileri (örneğin, zaman serisi KIMLIĞI veya zaman damgası özelliği) içerdiğinden emin olun.

Daha fazla bilgi edinmek ve olayların nasıl düzleştirilmesi ve depolanacağını anlamak için, [JSON düzleştirme ve kaçış kurallarını](./concepts-json-flattening-escaping-rules.md)okuyun.

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Advisor](../advisor/advisor-overview.md) 'ı inceleyerek iş kurtarma yapılandırması seçeneklerinizi planlayın.
* [Azure Advisor](../advisor/advisor-overview.md) 'ı inceleyerek iş kurtarma yapılandırması seçeneklerinizi planlayın.
* Azure Time Series Insights Gen2 ' deki [veri](./concepts-ingestion-overview.md) alımı hakkında daha fazla bilgi edinin.
* Azure Time Series Insights Gen2 'de [veri depolama](./concepts-storage.md) hakkındaki makaleyi gözden geçirin.
* Azure Time Series Insights Gen2 'de [veri modelleme](./concepts-model-overview.md) hakkında bilgi edinin.