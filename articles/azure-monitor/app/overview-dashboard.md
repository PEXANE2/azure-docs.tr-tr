---
title: Azure Uygulama Öngörüleri Genel Bakış Panosu | Microsoft Dokümanlar
description: Azure Uygulama Öngörüleri ve Genel Bakış Panosu işleviyle uygulamaları izleyin.
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: e5188972d9058b85a9765c7d33f6209b37245d7e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77669905"
---
# <a name="application-insights-overview-dashboard"></a>Uygulama Öngörüleri Genel Bakış panosu

Uygulama Öngörüleri, uygulamanızın sağlık durumunun ve performansının hızlı ve bir bakışta değerlendirilmesine olanak sağlamak için her zaman özet genel bakış bölmesi sağlamıştır. Yeni genel bakış panosu daha hızlı ve esnek bir deneyim sağlar.

## <a name="how-do-i-test-out-the-new-experience"></a>Yeni deneyimi nasıl test edebilirim?

Yeni genel bakış panosu artık varsayılan olarak başlatılır:

![Genel Bakış Önizleme Bölmesi](./media/overview-dashboard/overview.png)

## <a name="better-performance"></a>Daha iyi performans

Zaman aralığı seçimi basit bir tek tıklama arabirimine basitleştirilmiştir.

![Zaman aralığı](./media/overview-dashboard/app-insights-overview-dashboard-03.png)

Genel performans büyük ölçüde artırıldı. **Arama** ve **Analitik**gibi popüler özelliklere tek tıklamayla erişebilirsiniz. KPI döşemesi dinamik olarak güncelleyen her varsayılan, ilgili Uygulama Öngörüleri özelliklerine ilişkin bilgiler sağlar. Başarısız **istekler** hakkında daha fazla bilgi edinmek **için, Araştırma** üstbilgisinin altındaki Hataları seçin:

![Hatalar](./media/overview-dashboard/app-insights-overview-dashboard-04.png)

## <a name="application-dashboard"></a>Uygulama panosu

Uygulama panosu, uygulama nızın durumu ve performansıhakkında tamamen özelleştirilebilir tek bölme görünümü sağlamak için Azure'daki mevcut pano teknolojisinden yararlanır.

Varsayılan panoya erişmek için sol üst köşedeki _Uygulama Panosu'nu_ seçin.

![Pano görünümü](./media/overview-dashboard/app-insights-overview-dashboard-05.png)

Panoya ilk kez bu kez erişiyorsanız, varsayılan bir görünüm başlatacaktır:

![Pano görünümü](./media/overview-dashboard/0001-dashboard.png)

İsterseniz varsayılan görünümü tutabilirsiniz. Ya da ekibinizin ihtiyaçlarına en iyi şekilde uyacak şekilde panodan ekleyebilir ve silebilirsiniz.

> [!NOTE]
> Application Insights kaynağına erişimi olan tüm kullanıcılar aynı Uygulama panosu deneyimini paylaşır. Bir kullanıcı tarafından yapılan değişiklikler tüm kullanıcılar için görünümü değiştirir.

Genel bakış deneyimine geri dönmek için şunları seçmememiz gerekenleri seçin:

![Genel Bakış Düğmesi](./media/overview-dashboard/app-insights-overview-dashboard-07.png)

## <a name="troubleshooting"></a>Sorun giderme

**Yapılandırılan döşeme ayarlarını** seçerseniz ve 31 günü aşan özel bir zaman aralığı ayarlarsanız, panonuz 90 günlük varsayılan veri saklama yla bile 31 günlük veri yi geçmez. Şu anda bu davranış için geçici çözüm yok.

## <a name="next-steps"></a>Sonraki adımlar

- [Huniler](../../azure-monitor/app/usage-funnels.md)
- [Bekletme](../../azure-monitor/app/usage-retention.md)
- [Kullanıcı Akışları](../../azure-monitor/app/usage-flows.md)
