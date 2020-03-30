---
title: Azure'da gerçek zamanlı ve akışlı işlem çözümü seçin
description: Uygulamanızı Azure'da oluşturmak için doğru gerçek zamanlı analiz ve akış işleme teknolojisini nasıl seçeceğinizi öğrenin.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/15/2019
ms.openlocfilehash: 2146b1bd782aba5d98729a2d37d956744e469ba1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75860257"
---
# <a name="choose-a-real-time-analytics-and-streaming-processing-technology-on-azure"></a>Azure'da gerçek zamanlı analiz ve akış işleme teknolojisi seçin

Azure'da gerçek zamanlı analiz ler ve akış işlemleri için çeşitli hizmetler mevcuttur. Bu makalede, hangi teknolojinin uygulamanız için en uygun olduğuna karar vermek için gereken bilgiler sağlandığınızı sağlar.

## <a name="when-to-use-azure-stream-analytics"></a>Azure Akış Analizi ne zaman kullanılır?

Azure Akış Analizi, Azure'daki akış analitiği için önerilen hizmettir. Aşağıdakileri içeren ancak bunlarla sınırlı olmayan çok çeşitli senaryolar içindir:

* Veri görselleştirme için panolar
* Zamansal ve mekansal desenler veya anomalilerden gerçek zamanlı [uyarılar](stream-analytics-set-up-alerts.md)
* Ayıklama, Dönüştürme, Yükleme (ETL)
* [Olay Kaynak deseni](/azure/architecture/patterns/event-sourcing)
* [IoT Edge](stream-analytics-edge.md)

Uygulamanıza bir Azure Akışı Analizi işi eklemek, zaten bildiğiniz SQL dilini kullanarak Akış analitiğini Azure'da çalışır hale getirmek için en hızlı yoldur. Azure Akış Analizi bir iş hizmetidir, bu nedenle kümeleri yönetmek için zaman harcamak zorunda kalmamanız ve iş düzeyinde %99,9'luk bir SLA ile kapalı kalma süreniz konusunda endişelenmenize gerek yoktur. Faturalandırma da başlangıç maliyetlerini düşük (bir Akış Birimi) düşük, ancak ölçeklenebilir (en fazla 192 Akış Birimleri) yapma iş düzeyinde yapılır. Birkaç Stream Analytics işini çalıştırmak, bir kümeyi çalıştırmak ve sürdürmekten çok daha uygun maliyetlidir.

Azure Akış Analizi, zengin bir kutudan çıkma deneyimine sahiptir. Herhangi bir ek kurulum olmadan aşağıdaki özelliklerden hemen yararlanabilirsiniz:

* [Pencereli agregalar,](stream-analytics-window-functions.md)zamansal birleştirmeler ve zamansal analitik işlevler gibi yerleşik zamansal işleçler.
* Yerel Azure [giriş](stream-analytics-add-inputs.md) ve [çıktı](stream-analytics-define-outputs.md) bağdaştırıcıları
* Yavaş değişen [başvuru verileri](stream-analytics-use-reference-data.md) (arama tabloları olarak da bilinir), geofencing için jeouzamsal başvuru verileriyle birleştirme de dahil olmak üzere destek.
* [Anomali Algılama](stream-analytics-machine-learning-anomaly-detection.md) gibi entegre çözümler
* Aynı sorguda birden çok zaman penceresi
* Rasgele diziler halinde birden çok zamansal işleç oluşturma yeteneği.
* Etkinlik Hub'larına gelen girişten, sürekli yüksek verimde Olay Hub'larından ve Olay Hub'larına ağ gecikmesi de dahil olmak üzere Event Hub'larına çıkış inişine kadar 100 ms'lik uçtan uca gecikme

## <a name="when-to-use-other-technologies"></a>Diğer teknolojilerin ne zaman kullanılacağı

### <a name="you-want-to-write-udfs-udas-and-custom-deserializers-in-a-language-other-than-javascript-or-c"></a>JavaScript veya C dışındaki bir dilde UDF'ler, UD'ler ve özel deserializers yazmak istiyorsunuz #

Azure Akış Analizi, bulut işleri için JavaScript'te kullanıcı tanımlı işlevleri (UDF) veya kullanıcı tanımlı toplamaları (UDA) ve IoT Edge işleri için C# bilgilerini destekler. C# kullanıcı tanımlı deserializers da desteklenir. Java veya Python gibi diğer dillerde bir deserializer, UDF veya UDA uygulamak istiyorsanız, Spark Structured Streaming'i kullanabilirsiniz. Ayrıca rasgele akış işleme yapmak için kendi sanal makineleri üzerinde Olay Hub'ları **EventProcessorHost** çalıştırabilirsiniz.

### <a name="your-solution-is-in-a-multi-cloud-or-on-premises-environment"></a>Çözümünüz çok bulutlu veya şirket içi bir ortamda

Azure Akış Analizi, Microsoft'un tescilli teknolojisidir ve yalnızca Azure'da kullanılabilir. Çözümünüzün Bulutlar'da veya şirket içinde taşınabilir olması gerekiyorsa, Kıvılcım Yapılandırılmış Akış veya Fırtına gibi açık kaynaklı teknolojileri göz önünde bulundurun.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portalını kullanarak Bir Akış Analizi işi oluşturun](stream-analytics-quick-create-portal.md)
* [Azure PowerShell'i kullanarak Bir Akış Analizi işi oluşturun](stream-analytics-quick-create-powershell.md)
* [Visual Studio'u kullanarak Bir Stream Analytics işi oluşturun](stream-analytics-quick-create-vs.md)
* [Visual Studio Code'u kullanarak Bir Akış Analizi işi oluşturun](quick-create-vs-code.md)
