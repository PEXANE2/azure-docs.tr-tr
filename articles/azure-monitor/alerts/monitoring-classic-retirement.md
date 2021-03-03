---
title: Azure Izleyici 'de izleme & klasik uyarı güncelleştirmesi
description: Daha önce uyarılar altında Azure portal gösterildiği gibi, klasik izleme hizmetleri ve işlevlerinin kullanımdan kaldırılması açıklaması (klasik).
author: yanivlavi
services: azure-monitor
ms.topic: conceptual
ms.date: 02/14/2021
ms.author: yalavi
ms.subservice: alerts
ms.openlocfilehash: a862e54b5e1eddce5fcabf050eba1cfa1f103e30
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101734766"
---
# <a name="unified-alerting--monitoring-in-azure-monitor-replaces-classic-alerting--monitoring"></a>Azure Izleyici 'de izleme & birleştirilmiş uyarı, klasik uyarı & izlemeyi değiştirir

Azure Izleyici, Azure kaynakları arasında ' tek ölçüm ' ve ' bir uyarı ' desteği sunan Birleşik bir izleme yığınıdır. Bu [blog gönderisine](https://azure.microsoft.com/blog/new-full-stack-monitoring-capabilities-in-azure-monitor/)daha fazla bilgi görüntüleyin. Yeni Azure izleme ve uyarı platformları daha hızlı, daha akıllı ve genişletilebilir olacak şekilde geliştirilmiştir; bulut bilgi işlem ve Microsoft Akıllı bulut felseonuna göre daha fazla bilgi almak üzere tasarlanmıştır.

Yeni Azure izleme ve uyarı platformu sayesinde, Azure Izleyici 'deki klasik uyarılar, genel bulut kullanıcıları için devre dışı bırakılır, ancak yine de **31 mayıs 2021** tarihine kadar sınırlı kullanımda olabilir. Azure Kamu bulutu ve Azure Çin 21Vianet için klasik uyarılar, **29 şubat 2024** tarihinde devre dışı bırakılacak.

 ![Azure portal 'de klasik uyarı](media/monitoring-classic-retirement/monitor-alert-screen2.png) 

Başlamanızı ve yeni platformda uyarılarınızı yeniden oluşturmayı öneririz.

> [!IMPORTANT]
> Etkinlik günlüğünde oluşturulan klasik uyarı kuralları kullanım dışı olmayacaktır veya geçirilmez. Etkinlik günlüğünde oluşturulan tüm klasik uyarı kuralları, yeni Azure Izleyici-uyarılarından olduğu gibi erişilebilir ve kullanılabilir. Daha fazla bilgi için bkz. [Azure izleyici kullanarak etkinlik günlüğü uyarılarını oluşturma, görüntüleme ve yönetme](./alerts-activity-log.md). Benzer şekilde, hizmet durumundaki uyarılara erişilebilir ve yeni hizmet durumu bölümünde olduğu gibi kullanılabilirler. Ayrıntılar için bkz. [hizmet durumu bildirimleri uyarıları](../../service-health/alerts-activity-log-service-notifications-portal.md).

## <a name="unified-metrics-and-alerts-for-azure-resources"></a>Azure kaynakları için birleştirilmiş ölçümler ve uyarılar

Mart 2018 ' de, Azure kaynakları için yeni nesil uyarı yayımladık. Daha yeni ölçüm platformu ve uyarı daha hızlıdır ve boyutlar kullanılarak daha ayrıntı düzeyi sağlar. Boyutlar, belirli bir değer birleşimine, koşula veya işleme dilimlemenize ve filtrelemenize izin verir. Daha yeni ölçüm uyarıları, daha fazla bildirim ve otomasyon eylemine izin veren eylem gruplarını kullanır. [Azure izleyici kullanarak ölçüm uyarılarını yönetme](./alerts-metric.md)hakkında daha fazla bilgi için bkz..

Azure kaynakları için yeni ölçümler şu şekilde kullanılabilir:

- **Azure Izleyici standart platform ölçümleri** : çeşitli Azure hizmetlerinden ve ürünlerinden daha önceden doldurulan popüler ölçümler sağlar. Daha fazla bilgi için [Azure izleyici 'de desteklenen ölçümler](./alerts-metric-near-real-time.md#metrics-and-dimensions-supported) ve [Azure izleyici 'de ölçüm uyarılarını destekleme](./alerts-metric-overview.md#supported-resource-types-for-metric-alerts)hakkında bu makaleye bakın.
- **Azure Izleyici özel ölçümleri** : Azure tanılama Aracısı da dahil olmak üzere Kullanıcı tarafından yönetilen kaynaklardan ölçümler sağlar. Daha fazla bilgi için bkz. [Azure izleyici 'de özel ölçümler](../essentials/metrics-custom-overview.md)hakkında bu makaleye bakın. Özel ölçümleri kullanarak, [Windows Azure tanılama Aracısı](../essentials/collect-custom-metrics-guestos-resource-manager-vm.md) ve [etkileyen telegraf Aracısı](../essentials/collect-custom-metrics-linux-telegraf.md)tarafından toplanan ölçümleri de yayımlayabilirsiniz.

## <a name="retirement-of-classic-monitoring-and-alerting-platform"></a>Klasik izleme ve uyarı platformunun kullanımdan kaldırılması

Daha önce belirtildiği gibi, eski klasik izleme ve uyarı genel bulut kullanıcıları için devre dışı bırakıldı. Bu, ilgili API 'lerin, Azure portal arabiriminin ve hizmetlerin kapatılmasını içerir, ancak yine de **31 mayıs 2021** tarihine kadar sınırlı kullanımda olur. Azure Kamu bulutu ve Azure Çin 21Vianet için klasik uyarılar, **29 şubat 2024** tarihinde devre dışı bırakılacak.

Özellikle, devre dışı bırakma kapsamı, Azure portal [Uyarılar (klasik) bölümünde](./alerts-classic.overview.md) bulunan ve [Microsoft. Insights/alertrules](/rest/api/monitor/alertrules) kaynakları olarak erişilebilen klasik ölçümlere yöneliktir.

Diğer bir deyişle:

- Klasik izleme ve uyarılar hizmeti kullanımdan kaldırılacak ve yeni uyarı kuralları oluşturmak için artık kullanılamayacak.
- Uyarılarda (klasik) devam eden herhangi bir uyarı kuralı, bildirimler yürütülmeye ve tetiklenmesine devam edecektir.
- En klasik uyarı kuralları geçirilecek. İşlem herhangi bir kesinti olmadan sorunsuz olacaktır ve müşterilerin izleme kapsamında hiçbir kaybı olmayacaktır.
- Tetiklenen bildirimler, yeni yük yapısını içerir. Hedefin Yeni yapıyla çalışması için uyarlanmasını gerekecektir.
- [Otomatik olarak geçirilemeyen bazı klasik uyarı kuralları](alerts-understand-migration.md#manually-migrating-classic-alerts-to-newer-alerts) ve kullanıcılardan çalışmaya devam etmesi için el ile gerçekleştirilen eylem gerekir.

> [!IMPORTANT]
> Azure Izleyici, klasik uyarı kurallarını yeni platforma gönüllü [olarak geçirmek için bir araç](alerts-using-migration-tool.md) aldı. Hizmet devre dışı bırakıldıktan sonra, kalan kurallar otomatik olarak geçirilir. Müşterilerin klasik uyarı kuralları geçişten sonra, klasik uyarı kuralı yükünün otomatikleştirilmesini, [diğer Azure kaynaklarına yönelik Birleşik ölçümler ve uyarılardan](#unified-metrics-and-alerts-for-azure-resources)yeni yükü işlemek için uyarlanmasını sağlamak gerekecektir. Daha fazla bilgi için bkz. [Klasik uyarı kuralı geçişine hazırlanma](alerts-prepare-migration.md).

## <a name="pricing-for-migrated-alert-rules"></a>Geçirilen uyarı kuralları fiyatlandırması

Azure Monitor [Klasik uyarılarınızı](./alerts-classic.overview.md) yeni uyarılar deneyimine geçirmenize yardımcı olmak için bir geçiş aracı kullanıma sunuyoruz. Geçirilen uyarı kuralları ve karşılık gelen geçirilmiş eylem grupları (e-posta, Web kancası veya LogicApp) ücretsiz olarak kalır. Eşiği, toplama türünü ve toplama ayrıntı düzeyini düzenleme özelliği de dahil olmak üzere klasik uyarılarla sahip olduğunuz işlevsellik, geçirilen uyarı kuralınız ile ücretsiz olarak kullanılabilir olmaya devam edecektir. Ancak, yeni uyarı platformu özelliklerinden herhangi birini, bildirimleri veya eylem türlerini kullanmak için geçirilmiş uyarı kuralını düzenlerseniz, buna karşılık gelen bir ücret uygulanır. Uyarı kuralları ve bildirimlerin fiyatlandırmasıyla ilgili daha fazla bilgi için bkz. [Azure Monitor fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/).

Aşağıda, uyarı kuralınız için ücret ödemeniz gereken durumların örnekleri verilmiştir:

- Yeni Azure Izleyici platformunda ücretsiz birimlerin ötesinde oluşturulan yeni (geçirilmeyen) uyarı kuralı
- Azure Izleyici tarafından dahil edilen ve ücretsiz birimlerin ötesinde saklanan tüm veriler
- Application Insights tarafından yürütülen tüm çoklu test Web testleri
- Azure Izleyici 'de yer alan ücretsiz birimlerin ötesinde depolanan özel ölçümler
- Sıklık, birden çok kaynak/boyut, [dinamik eşikler](../alerts/alerts-dynamic-thresholds.md), kaynak/sinyal değiştirme vb. gibi daha yeni ölçüm uyarısı özelliklerini kullanmak için düzenlenen tüm geçirilmiş uyarı kuralları.
- Daha yeni bildirimleri veya SMS, sesli çağrı ve/veya ıTSM tümleştirmesi gibi eylem türlerini kullanmak için düzenlenen geçirilmiş eylem grupları.

## <a name="next-steps"></a>Sonraki adımlar

* [Yeni Birleşik Azure İzleyicisi](../overview.md)hakkında bilgi edinin.
* Yeni [Azure uyarıları](./alerts-overview.md)hakkında daha fazla bilgi edinin.