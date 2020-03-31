---
title: Azure Uygulama Öngörüleri ile kullanılabilirlik uyarıları ayarlama | Microsoft Dokümanlar
description: Application Insights’ta web testleri ayarlayın. Web sitesi kullanılamaz duruma gelirse veya yavaş yanıt verirse uyarı alın.
ms.topic: conceptual
author: lgayhardt
ms.author: lagayhar
ms.date: 06/19/2019
ms.reviewer: sdash
ms.openlocfilehash: e0c1a93ef663762bec199abc5aa7eabbc821168d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77654962"
---
# <a name="availability-alerts"></a>Kullanılabilirlik uyarıları

[Azure Application Insights](../../azure-monitor/app/app-insights-overview.md), dünyanın her yerindeki noktalarından uygulamanıza düzenli aralıklarla web istekleri gönderir. Uygulamanız yanıt vermiyorsa veya çok yavaş yanıt veriyorsa sizi uyarabilir.

## <a name="enable-alerts"></a>Uyarıları etkinleştirme

Uyarılar artık varsayılan olarak otomatik olarak etkinleştirilir, ancak uyarıyı tam olarak yapılandırmak için ilk olarak kullanılabilirlik testinizi oluşturmanız gerekir.

![Deneyim oluşturun](./media/availability-alerts/create-test.png)

> [!NOTE]
>  Yeni [birleştirilmiş uyarılarda,](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts)uyarı kuralı nın önem derecesi ve [eylem gruplarıyla](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) bildirim tercihleri uyarılar deneyiminde yapılandırılmalıdır. **must be** Aşağıdaki adımlar olmadan yalnızca portal içi bildirimler alırsınız.

1. Kullanılabilirlik testini kurtardıktan sonra, ayrıntılar sekmesinde az önce yaptığınız testin elipsine tıklayın. "Uyarıyı edit" seçeneğini tıklayın.

   ![Kaydedin'den sonra edin](./media/availability-alerts/edit-alert.png)

2. İstenilen önem düzeyini, kural açıklamasını ve en önemlisi - bu uyarı kuralı için kullanmak istediğiniz bildirim tercihlerine sahip eylem grubu.

   ![Kaydedin'den sonra edin](./media/availability-alerts/set-action-group.png)

### <a name="alert-on-x-out-of-y-locations-reporting-failures"></a>Hataları bildiren Y konumlarının X'i hakkında uyarı

Yeni bir kullanılabilirlik testi oluşturduğunuzda, Y konumlarının X dışında uyarı [kuralı, yeni birleştirilmiş uyarılar deneyiminde](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts)varsayılan olarak etkinleştirilir. "Klasik" seçeneğini seçerek veya uyarı kuralını devre dışı bırakmak için seçebilirsiniz.

> [!NOTE]
> Yukarıdaki adımları izleyerek uyarı tetiklendiğinde eylem gruplarını bildirim alacak şekilde yapılandırın. Bu adım olmadan, yalnızca kural tetiklendiğinde portal içi bildirimler alırsınız.
>

### <a name="alert-on-availability-metrics"></a>Kullanılabilirlik ölçümleri hakkında uyarı

Yeni [birleştirilmiş uyarıları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts)kullanarak, parçalı toplu kullanılabilirlik ve test süresi ölçümleri konusunda da uyarıda bulunabilirsiniz:

1. Ölçümler deneyiminde bir Uygulama Öngörüleri kaynağı seçin ve kullanılabilirlik ölçüsünü seçin:

    ![Kullanılabilirlik ölçümleri seçimi](./media/availability-alerts/select-metric.png)

2. Menüden uyarıları yapılandır seçeneği sizi, uyarı kuralını ayarlamak için belirli testleri veya konumları seçebileceğiniz yeni deneyime götürür. Bu uyarı kuralı için eylem gruplarını da burada yapılandırabilirsiniz.

### <a name="alert-on-custom-analytics-queries"></a>Özel analitik sorgularında uyarı

Yeni [birleştirilmiş uyarıları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts)kullanarak, [özel günlük sorgularında](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log)uyarıda bulunabilirsiniz. Özel sorgularla, kullanılabilirlik sorunlarının en güvenilir sinyalini almanıza yardımcı olacak herhangi bir rasgele koşul hakkında uyarıda bulunabilirsiniz. TrackAvailability SDK'yı kullanarak özel kullanılabilirlik sonuçları gönderiyorsanız, bu da geçerlidir.

> [!Tip]
> Kullanılabilirlik verilerindeki ölçümler, TrackAvailability SDK'mızı arayarak gönderebileceğiniz özel kullanılabilirlik sonuçlarını içerir. Özel kullanılabilirlik sonuçları hakkında uyarıda bulunması için ölçüm desteğinde uyarıyı kullanabilirsiniz.
>

## <a name="automate-alerts"></a>Uyarıları otomatikleştir

Bu işlemi Azure Kaynak Yöneticisi şablonlarıyla otomatikleştirmek için, [Kaynak Yöneticisi şablon belgeleriyle metrik uyarı oluştur'a](../../azure-monitor/platform/alerts-metric-create-templates.md#template-for-an-availability-test-along-with-a-metric-alert) bakın.

## <a name="troubleshooting"></a>Sorun giderme

Özel [sorun giderme makalesi](troubleshoot-availability.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Çok adımlı web testleri](availability-multistep.md)
* [Url ping web testleri](monitor-web-app-availability.md)
