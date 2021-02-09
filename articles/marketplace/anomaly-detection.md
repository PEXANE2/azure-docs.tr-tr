---
title: Ölçülen faturalandırma için anomali algılama | Azure Marketi
description: Tarifeli faturalandırma için otomatik anomali algılamanın, müşterilerinizin ticari Market teklifinizin tarifeli kullanımı için doğru şekilde faturalandırılmaya nasıl yardımcı olduğunu öğrenin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 1/09/2021
author: sayantanroy83
ms.author: sroy
ms.openlocfilehash: d4fb88854359dcd6e383b47d2a8ce4e9c91f867a
ms.sourcegitcommit: 7e117cfec95a7e61f4720db3c36c4fa35021846b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99989528"
---
# <a name="anomaly-detection-for-metered-billing"></a>Ölçülen faturalandırma için anomali algılama

Bu makalede Market ölçüm hizmeti ve ilişkili otomatik anomali algılama özelliği hakkında daha fazla bilgi sağlanır ve müşterileri tarifeli kullanım için doğru bir şekilde faturalıyoruz. Ölçülen faturalandırma seçeneği şu anda [hizmet olarak yazılım](plan-saas-offer.md) (SaaS) teklifleri ve yönetilen bir uygulama planına sahip [Azure uygulamaları](plan-azure-application-offer.md#types-of-plans) için kullanılabilir. Bu seçenek, iş ortaklarının standart olmayan birimlere göre ücretlendirilen ticari Market programında teklif oluşturmalarına olanak tanır.

SaaS ve yönetilen uygulamalar için dağıtılan özel ölçümleri olan iş ortakları, Iş Ortağı Merkezi 'nde belirli _özel_ ölçümlerle _fazla kullanım olayları_ için beklenen kullanım davranışından sapmaları görebilir. Iş Ortağı Merkezi, riski azaltmak için, normal ölçülen faturalandırma davranışını saptamak, ölçülen faturalandırma kullanımını çözümlemek ve en az Kullanıcı müdahalesiyle anormal bulmalara yönelik makine öğrenimi algoritmalarını uygulayan anomali algılama hizmetini kullanır. Ölçülen ödeme kullanım verileri kümelerinde _anomali algılama modellerini_ kullanarak, bildirilen kullanım beklenen kullanımı aştığında yayımcıyı bilgilendirmek için iş ortağı merkezi amaçlar.

## <a name="usability-experience"></a>Kullanılabilirlik deneyimi

Microsoft, müşteriyi Microsoft faturalarını görüntülemeden önce SaaS veya Azure tarafından yönetilen uygulama teklifleri için müşterinin fazla kullanım süresini bildirmek üzere iş ortağına bağımlıdır. Yanlış kullanım raporlandıysanız, müşteri yanlış bir fatura alabilir, Microsoft 'un ve iş ortağının güvenilirlik kazanmasına karşı daha fazla araştırma sağlayabilir.

Bu sorunu azaltmaya yardımcı olmak için, hem SaaS uygulamaları hem de Azure uygulama tarafından yönetilen uygulama planları için bir otomatik anomali algılama özelliği sunulmaktadır. Bu özellik, kullanımı ölçülen faturalandırmaya karşı proaktif olarak izleyen ve beklenen aralıktaki kullanım beklenen değerini tahmin eden bir makine öğrenimi modelidir. Kullanım beklenen aralığın dışındaysa, bir anomali olarak kabul edilir ve Iş Ortağı Merkezi 'nin ticari Market programındaki teklif Genel Bakış sayfasında iş ortağına bir uyarı bildirimi görüntülenir.

Makine öğrenimi modeli analizler kullanım süresi üst. Yayımcı, her teklifin özel ölçüm boyutları için müşterilerinin fazla kullanım ömrü boyunca bildirilen tüm anormallikleri görebilir.

### <a name="view-and-manage-metered-usage-anomalies"></a>Tarifeli kullanım anormalilerini görüntüleyin ve yönetin

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/home)' nde oturum açın.
1. Sol gezinti menüsünde **ticari Market**  >  **Analizi**' ni seçin.
1. **Tarifeli kullanım bozukluileri** sekmesini seçin.

    [![Kullanım sayfasında ölçülen kullanım bozuklulıkları sekmesini gösterir.](./media/anomaly-detection/metered-usage-anomalies.png)](./media/anomaly-detection/metered-usage-anomalies.png#lightbox)
    ***Şekil 1: tarifeli kullanım bozukluklar sekmesi***

1. Tarifeli faturalandırmaya karşı algılanan tüm kullanım bozuklukları için, bir yayımcı olarak anomali 'nin doğru olup olmadığını araştırmanız ve onaylamanız istenir. Tanıyı onaylamak için **anomali olarak işaretle** ' yi seçin.

     [![Anomali olarak Işaretle iletişim kutusunu gösterir.](./media/anomaly-detection/mark-as-anomaly.png)](./media/anomaly-detection/mark-as-anomaly.png#lightbox)
    ***Şekil 2: anomali iletişim kutusu olarak Işaretleme***

1. Algılanan fazla kullanım anomali 'nin orijinal olmadığını düşünüyorsanız, Iş Ortağı Merkezi 'nin belirli bir fazla kullanım kullanımı için **bir anomali olmadığını** seçerek bu geri bildirimi sağlayabilirsiniz.

    [![Neden anomali iletişim kutusu olduğunu gösterir.](./media/anomaly-detection/why-is-it-not-an-anomaly.png)](./media/anomaly-detection/why-is-it-not-an-anomaly.png#lightbox)
    ***Şekil 3: neden bir anomali değil? iletişim kutusu***

1. Bildirilmemiş anormalilerin envanter listesini görmek için sayfayı aşağı kaydırın. Liste, kabul ettiğiniz anormalilerin envanterini sağlar. Iş ortağı merkezinin herhangi birini orijinal veya yanlış olarak işaretlediğini seçebilirsiniz.

   [![Kullanım sayfasında Iş Ortağı Merkezi tarafından onaylanan bildirim listesini gösterir.](./media/anomaly-detection/unacknowledged-anomalies.png)](./media/anomaly-detection/unacknowledged-anomalies.png#lightbox)
    ***Şekil 4: Iş Ortağı Merkezi tarafından kabul edilmemiş anoıes listesi***

1. Ayrıca, fazla kullanım kullanımları üzerinde yaptığınız eylemleri gösteren bir anomali eylem günlüğü görürsünüz. Eylem günlüğünde, hangi fazla kullanım olaylarının orijinal veya yanlış olarak işaretlendiğini görebileceksiniz.

   [ ![ Kullanım sayfasında anomali eylem günlüğünü gösterir.](./media/anomaly-detection/anomaly-action-log.png)](./media/anomaly-detection/anomaly-action-log.png#lightbox) 
    ***Şekil 5: anomali eylem günlüğü***

1. İş ortağı merkezi analizi, dışarı aktarma raporlarında fazla kullanım olaylarının yeniden bildirimini desteklemez. İş Ortağı Merkezi, bir anomali için düzeltilen fazla kullanım kullanımını girmenize olanak sağlar ve Ayrıntılar için Microsoft ekiplerine Ayrıntılar geçirilir. Microsoft, araştırmaya bağlı olarak, fazla ücretlendirilen müşteriye uygun olan kredi para iadelerini de yayımlayacak. Bayrak eklenmiş herhangi bir güvenlik düzeyini seçtiğinizde, kullanım fazla kullanım süresini orijinal olarak işaretlemek için **anomali olarak işaretle** ' yi seçebilirsiniz.

   [ ![ Anomali olarak işaretle iletişim kutusu gösterir.](./media/anomaly-detection/new-reported-usage.png)](./media/anomaly-detection/new-reported-usage.png#lightbox) 
    ***Şekil: 6: anomali olarak işaretleme iletişim kutusu***

Bir fazla kullanım kullanımı Iş Ortağı Merkezi 'nde düzensiz olarak işaretlendiğinde, bu örnekten 30 günlük bir pencere alacaksınız ve bu örnekten, anomali veya yanlış olarak işaretlemek için bu örnekten bir pencere alacaksınız. 30 günlük süre sonra, yayımcı olarak, bozukluklar üzerinde işlem yapamaz.

> [!IMPORTANT]
> Bildirilen fazla kullanım birimi, ifadenin ve mali ayarlama için uygun değildir.

Seçilen hesaplama dönemi için tüm anormallikleri (onaylanan veya başka türlü) görebilirsiniz. Çeşitli hesaplama dönemleri son 30 gün, son 60 gün ve son 90 gündür.

> [!IMPORTANT]
> Iş Ortağı Merkezi 'nde, anomali olarak ilk kez bildirildiğine göre, belirlenen zaman içindeki, belirlenen ve bu süre içinde belirlenen bozukluklar üzerinde işlem yapmanız istenir.

Pencere öğesinde kalıcı olan filtre, tek tek teklifleri ve bireysel özel ölçümleri seçmenize olanak sağlar.

Fazla kullanım kullanımını anomali olarak işaretledikten veya orijinal olarak anomali olarak işaretlenen bir modeli kabul ettikten sonra, seçimi "anomali değil" olarak değiştiremezsiniz.

> [!IMPORTANT]
> Fazla kullanım durumları durumunda fazla kullanım kullanımlarını yeniden gönderebilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.
- [Ticari Market ölçüm hizmeti kullanılarak SaaS için ölçülen faturalandırma](./partner-center-portal/saas-metered-billing.md)
- [Yönetilen uygulama ölçümlü faturalandırma](./partner-center-portal/azure-app-metered-billing.md)
