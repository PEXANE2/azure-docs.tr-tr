---
title: Azure Application Insights kullanılabilirlik uyarılarını ayarlama | Microsoft Docs
description: Application Insights’ta web testleri ayarlayın. Web sitesi kullanılamaz duruma gelirse veya yavaş yanıt verirse uyarı alın.
services: application-insights
documentationcenter: ''
author: lgayhardt
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.author: lagayhar
ms.openlocfilehash: 1d7527d6f52235c6b95ad2e336ea9f9ba85d6344
ms.sourcegitcommit: 8e1fb03a9c3ad0fc3fd4d6c111598aa74e0b9bd4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70114388"
---
# <a name="availability-alerts"></a>Kullanılabilirlik uyarıları

[Azure Application Insights](../../azure-monitor/app/app-insights-overview.md), dünyanın her yerindeki noktalarından uygulamanıza düzenli aralıklarla web istekleri gönderir. Uygulamanız yanıt vermiyorsa veya çok yavaş yanıt verirse sizi uyarır.

## <a name="enable-alerts"></a>Uyarıları etkinleştir

Uyarılar artık varsayılan olarak otomatik olarak etkinleştirilir, ancak uyarıyı tamamen yapılandırmak için ilk olarak kullanılabilirlik testinizi oluşturmanız gerekir.

![Deneyim oluştur](./media/availability-alerts/create-test.png)

> [!NOTE]
>  [Yeni Birleşik uyarılarla](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), uyarı kuralının önem derecesi ve [eylem gruplarıyla](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) ilgili bildirim tercihleri, uyarı deneyiminde yapılandırılmalıdır. Aşağıdaki adımlar olmadan yalnızca Portal içi bildirimler alacaksınız.

1. Kullanılabilirlik testini kaydettikten sonra, Ayrıntılar sekmesinde, yeni yaptığınız teste göre üç noktaya tıklayın. "Uyarıyı Düzenle" seçeneğine tıklayın.

   ![Kaydettikten sonra Düzenle](./media/availability-alerts/edit-alert.png)

2. İstenen önem derecesi düzeyi, kural açıklaması ve en önemlisi-bu uyarı kuralı için kullanmak istediğiniz bildirim tercihlerine sahip olan eylem grubunu ayarlayın.

   ![Kaydettikten sonra Düzenle](./media/availability-alerts/set-action-group.png)

### <a name="alert-on-x-out-of-y-locations-reporting-failures"></a>Y/Y konumları raporlama hatalarının X üzerinden uyarı

Yeni bir kullanılabilirlik testi oluşturduğunuzda, [Yeni Birleşik uyarılar deneyiminde](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts)X-Y konumları uyarı kuralı varsayılan olarak etkindir. "Klasik" seçeneğini seçerek veya uyarı kuralını devre dışı bırakmayı tercih edebilirsiniz.

> [!NOTE]
> Yukarıdaki adımları izleyerek uyarı tetiklediği zaman, eylem gruplarını bildirim alacak şekilde yapılandırın. Bu adım olmadan, yalnızca kural tetiklendiğinde Portal içi bildirimler alacaksınız.
>

### <a name="alert-on-availability-metrics"></a>Kullanılabilirlik ölçümleri uyarısı

[Yeni Birleşik uyarıları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts)kullanarak, bölümlenmiş toplam kullanılabilirlik ve test süresi ölçümlerinde de uyarı verebilirsiniz:

1. Ölçüm deneyiminden bir Application Insights kaynağı seçin ve bir kullanılabilirlik ölçümü seçin:

    ![Kullanılabilirlik ölçümleri seçimi](./media/availability-alerts/select-metric.png)

2. Menüden uyarıları Yapılandır seçeneği, sizi uyarı kuralını ayarlamak için belirli testleri veya konumları seçebileceğiniz yeni deneyimle götürür. Bu uyarı kuralının eylem gruplarını burada da yapılandırabilirsiniz.

### <a name="alert-on-custom-analytics-queries"></a>Özel analiz sorgularında uyar

[Yeni Birleşik uyarıları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts)kullanarak [özel günlük sorguları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log)üzerinde uyarı alabilirsiniz. Özel sorgularla, kullanılabilirlik sorunlarının en güvenilir sinyalini almanıza yardımcı olan herhangi bir rastgele koşulla ilgili uyarı alabilirsiniz. Bu, TrackAvailability SDK 'sını kullanarak özel kullanılabilirlik sonuçları gönderiyorsanız de geçerlidir.

> [!Tip]
> Kullanılabilirlik verilerinde ölçümler, TrackAvailability SDK 'sını çağırarak gönderdiğiniz tüm özel kullanılabilirlik sonuçlarını içerir. Özel kullanılabilirlik sonuçlarıyla ilgili uyarı almak için ölçüm desteğiyle ilgili uyarı ' yı kullanabilirsiniz.
>

## <a name="automate-alerts"></a>Uyarıları otomatikleştirin

Azure Resource Manager şablonlarıyla bu işlemi otomatikleştirmek için [Kaynak Yöneticisi şablonuyla ölçüm uyarısı oluşturma](../../azure-monitor/platform/alerts-metric-create-templates.md#template-for-a-availability-test-along-with-availability-test-alert) belgelerine bakın.

## <a name="troubleshooting"></a>Sorun giderme

Adanmış [sorun giderme makalesi](troubleshoot-availability.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Çok adımlı Web testleri](availability-multistep.md)
* [URL ping Web testleri](monitor-web-app-availability.md)
