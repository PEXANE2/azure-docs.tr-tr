---
title: İzleme nasıl yapılandırılır - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins için izleme ve günlüğe kaydetme seçeneklerini nasıl yapılandırılabildiğini öğrenin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/21/2020
ms.custom: seodec18
ms.openlocfilehash: e35e18be20af3bd9f6fdc9541f9abfe857a6b87c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76511867"
---
# <a name="how-to-configure-monitoring-in-azure-digital-twins"></a>Azure Digital Twins'te izleme nasıl yapılandırılır?

Azure Digital Twins sağlam günlüğe kaydetme, izleme ve analizleri destekler. Çözüm geliştiricileri, bir IoT uygulamasının karmaşık izleme gereksinimlerini desteklemek için Azure Monitor günlüklerini, tanılama günlüklerini, etkinlik günlüğünü ve diğer hizmetleri kullanabilir. Günlüğe kaydetme seçenekleri, çeşitli hizmetlerdeki kayıtları sorgulamak veya görüntülemek ve birçok hizmet için parçalı günlüğe kaydetme kapsamı sağlamak için birleştirilebilir.

Bu makalede, günlüğe kaydetme ve izleme seçenekleri ve bunları Azure Digital Twins'e özgü şekillerde nasıl birleştirilen özetlenmektedir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="review-activity-logs"></a>Etkinlik günlüklerini gözden geçirme

Azure [etkinlik günlükleri,](../azure-monitor/platform/platform-logs-overview.md) her Azure hizmet örneği için abonelik düzeyinde olay ve çalışma geçmişleri hakkında hızlı bilgiler sağlar.

Abonelik düzeyindeki olaylar şunlardır:

* Kaynak oluşturma
* Kaynak kaldırma
* Uygulama sırları oluşturma
* Diğer hizmetlerle tümleştirme

Azure Digital Twins için etkinlik günlüğü varsayılan olarak etkinleştirilir ve Azure portalı nda şu şekilde bulunabilir:

1. Azure Dijital İkizler örneğini seçme.
1. Ekran panelini açmak için **Etkinlik günlüğünü** seçme:

    [![Etkinlik günlüğü](media/how-to-configure-monitoring/activity-log.png)](media/how-to-configure-monitoring/activity-log.png#lightbox)

Gelişmiş etkinlik günlüğü için:

1. **Etkinlik Günlüğü Analizine Genel Bakışı**görüntülemek için **Günlükler** seçeneğini seçin:

    [![Seçim](media/how-to-configure-monitoring/activity-log-select.png)](media/how-to-configure-monitoring/activity-log-select.png#lightbox)

1. **Etkinlik Günlüğü Analizine Genel Bakış,** temel etkinlik günlüğü verilerini özetler:

    [![Etkinlik günlüğü analizine genel bakış]( media/how-to-configure-monitoring/log-analytics-overview.png)]( media/how-to-configure-monitoring/log-analytics-overview.png#lightbox)

>[!TIP]
>Abonelik düzeyindeki olaylarla ilgili hızlı öngörüler için **etkinlik günlüklerini** kullanın.

## <a name="enable-customer-diagnostic-logs"></a>Müşteri tanılama günlüklerini etkinleştirme

Azure [tanı ayarları,](../azure-monitor/platform/platform-logs-overview.md) etkinlik günlüğe kaydetmeyi tamamlamak için her Azure örneği için ayarlanabilir. Etkinlik günlükleri abonelik düzeyindeki olaylarla ilgili olsa da, tanılama günlüğü kaynakların kendi operasyonel geçmişine ilişkin öngörüler sağlar.

Tanısal günlüğe kaydetmeye örnek olarak şunlar verilebilir:

* Kullanıcı tanımlı bir işlevin yürütme süresi
* Başarılı bir API isteğinin yanıt durum kodu
* Uygulama anahtarını kasadan alma

Bir örnek için tanılama günlüklerini etkinleştirmek için:

1. Kaynağı Azure portalında getirin.
1. **Tanılama ayarlarını**seçin:

    [![Tanılama ayarları bir](media/how-to-configure-monitoring/diagnostic-settings-one.png)](media/how-to-configure-monitoring/diagnostic-settings-one.png#lightbox)

1. Veri toplamak için **tanılamayı Aç'ı** seçin (daha önce etkinleştirilmediyse).
1. İstenen alanları doldurun ve verilerin nasıl ve nerede kaydedileceğini seçin:

    [![Tanılama ayarları iki](media/how-to-configure-monitoring/diagnostic-settings-two.png)](media/how-to-configure-monitoring/diagnostic-settings-two.png#lightbox)

    Tanılama günlükleri genellikle [Azure Dosya Depolama](../storage/files/storage-files-deployment-guide.md) kullanılarak kaydedilir ve Azure Monitor [günlükleriyle](../azure-monitor/log-query/get-started-portal.md)paylaşılır. Her iki seçenek de seçilebilir.

>[!TIP]
>Kaynak işlemleriyle ilgili öngörüler için **tanılama günlüklerini** kullanın.

## <a name="azure-monitor-and-log-analytics"></a>Azure monitör ve günlük analitiği

IoT uygulamaları birbirinden farklı kaynakları, aygıtları, konumları ve verileri tek bir arada birleştirir. Ayrıntılı günlük, genel uygulama mimarisinin her belirli parçası, hizmeti veya bileşeni hakkında ayrıntılı bilgi sağlar, ancak genellikle bakım ve hata ayıklama için birleşik bir genel bakış gereklidir.

Azure Monitor, günlük kaynaklarının tek bir konumda görüntülenmesini ve analiz edilmesini sağlayan güçlü günlük analizi hizmetini içerir. Bu nedenle Azure Monitor, gelişmiş IoT uygulamalarındaki günlükleri analiz etmek için son derece yararlıdır.

Kullanım örnekleri şunlardır:

* Birden çok tanılama günlüğü geçmişini sorgulama
* Kullanıcı tanımlı birkaç işlev için günlükleri görme
* Belirli bir zaman çerçevesi içinde iki veya daha fazla hizmetin günlüklerini görüntüleme

Tam günlük [sorgulama, Azure Monitor günlükleri](../azure-monitor/log-query/log-query-overview.md)aracılığıyla sağlanır. Bu güçlü özellikleri ayarlamak için:

1. Azure portalında **Günlük Analitiği'ni** arayın.
1. Kullanılabilir **Log Analytics çalışma alanı** örnekleriniz görüntülenir. Birini seçin ve sorgu için **Günlükler'i** seçin:

    [![Günlük analitiği](media/how-to-configure-monitoring/log-analytics.png)](media/how-to-configure-monitoring/log-analytics.png#lightbox)

1. Zaten bir **Log Analytics çalışma alanı** örneğiniz yoksa, **Ekle** düğmesini seçerek bir çalışma alanı oluşturabilirsiniz:

    [![OMS oluşturma](media/how-to-configure-monitoring/log-analytics-oms.png)](media/how-to-configure-monitoring/log-analytics-oms.png#lightbox)

Log **Analytics çalışma alanı** örneğiniz sağlandıktan sonra, **Log Management'ı**kullanarak katlar günlüklerinde girişleri bulmak veya belirli kriterleri kullanarak arama yapmak için güçlü sorgular kullanabilirsiniz:

   [![Günlük yönetimi](media/how-to-configure-monitoring/log-analytics-management.png)](media/how-to-configure-monitoring/log-analytics-management.png#lightbox)

Güçlü sorgu işlemleri hakkında daha fazla bilgi için [sorgularla başlarken](../azure-monitor/log-query/get-started-queries.md)okuyun.

> [!NOTE]
> **Olayları Log Analytics çalışma alanına** ilk kez gönderirken 5 dakikalık bir gecikme yaşayabilirsiniz.

Azure Monitor günlükleri ayrıca **Tanıla ve sorunları çözerek**görüntülenebilen güçlü hata ve uyarı bildirim hizmetleri de sağlar:

   [![Uyarı ve hata bildirimleri](media/how-to-configure-monitoring/log-analytics-notifications.png)](media/how-to-configure-monitoring/log-analytics-notifications.png#lightbox)

>[!TIP]
>Birden çok uygulama işlevleri, abonelikler veya hizmetler için günlük geçmişlerini sorgulamak için **Günlük Analizi çalışma alanını** kullanın.

## <a name="other-options"></a>Diğer seçenekler

Azure Digital Twins, uygulamaya özel günlük ve güvenlik denetimini de destekler. Azure Dijital İkizler örneğiniz için kullanılabilen tüm Azure günlük seçeneklerine ayrıntılı bir genel bakış için [Azure günlük denetim](../security/fundamentals/log-audit.md) makalesini okuyun.

## <a name="next-steps"></a>Sonraki adımlar

- Azure etkinlik [günlükleri](../azure-monitor/platform/platform-logs-overview.md)hakkında daha fazla bilgi edinin.

- [Tanılama günlüklerine genel bir bakış](../azure-monitor/platform/platform-logs-overview.md)okuyarak Azure tanı lama ayarlarına daha derinlemesine dalın.

- [Azure Monitor günlükleri](../azure-monitor/log-query/get-started-portal.md)hakkında daha fazla bilgi edinin.
