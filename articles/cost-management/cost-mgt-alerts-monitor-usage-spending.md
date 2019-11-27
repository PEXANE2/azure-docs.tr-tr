---
title: Kullanım ve maliyet uyarıları ile harcama izleme | Microsoft Docs
description: Bu makalede, maliyet uyarılarının Azure maliyet yönetimi 'nde kullanımı ve harcamalarınızı izlemenize nasıl yardımcı olduğu açıklanır.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: cost-management-billing
manager: alavital
ms.custom: ''
ms.openlocfilehash: 4be484cdff2014f11c872da9a246ef8406447712
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230109"
---
# <a name="use-cost-alerts-to-monitor-usage-and-spending"></a>Maliyet uyarılarını kullanarak kullanımı ve harcamayı izleme

Bu makale, Azure kullanımınızı ve harcamalarınızı izlemek için maliyet yönetimi uyarılarını anlamanıza ve kullanmanıza yardımcı olur. Maliyet uyarıları, Azure kaynakları tüketildiği zaman otomatik olarak oluşturulur. Uyarılar tüm etkin maliyet yönetimi ve faturalandırma uyarılarını tek bir yerde gösterir. Tüketiminize verilen bir eşiğe ulaştığında, uyarılar maliyet yönetimi tarafından oluşturulur. Üç tür maliyet uyarısı vardır: bütçe uyarıları, kredi uyarıları ve departman harcama kotası uyarıları.

## <a name="budget-alerts"></a>Bütçe uyarıları

Bütçe uyarıları, kullanım veya maliyete dayanarak, [bütçenin uyarı koşulunda](tutorial-acm-create-budgets.md)tanımlanan miktara ulaştığında veya bu miktarı aştığında size bildirir. Maliyet yönetimi bütçeleri Azure portal veya [Azure tüketim](https://docs.microsoft.com/rest/api/consumption) API 'si kullanılarak oluşturulur.

Azure portal, bütçeler maliyete göre tanımlanır. Azure tüketim API 'sini kullanarak, bütçeler maliyet veya tüketim kullanımına göre tanımlanır. Bütçe uyarıları hem maliyet tabanlı hem de kullanım tabanlı bütçeleri destekler. Bütçe uyarıları, bütçe uyarı koşulları her karşılandığında otomatik olarak oluşturulur. Tüm maliyet uyarılarını Azure portal görüntüleyebilirsiniz. Bir uyarı oluşturulduğunda, maliyet uyarılarında gösterilir. Ayrıca, bütçenin uyarı alıcıları listesindeki kişilere bir uyarı e-postası gönderilir.

## <a name="credit-alerts"></a>Kredi Uyarıları

Kredi Uyarıları, Azure kredi parasal taahhütlerine harcanan zaman sizi bilgilendirir. Parasal Taahhütler, kurumsal sözleşmeleri olan kuruluşlara yöneliktir. Kredi Uyarıları %90 ve Azure kredi bakiyeniz %100 ' de otomatik olarak oluşturulur. Bir uyarı oluşturulduğunda, maliyet uyarılarında ve hesap sahiplerine gönderilen e-postada yansıtılır.

## <a name="department-spending-quota-alerts"></a>Departman harcama kota uyarıları

Departman harcama kotası uyarıları, departman harcamanın, kotanın sabit eşiğine ulaştığında sizi uyarır. Harcama kotaları EA portalında yapılandırılır. Her bir eşik karşılandığında, departman sahiplerine bir e-posta oluşturur ve maliyet uyarılarında gösterilir. Örneğin, kotanın %50 veya %75 ' i.

## <a name="supported-alert-features-by-offer-categories"></a>Teklif kategorilerine göre desteklenen uyarı özellikleri

Uyarı türleri desteği, sahip olduğunuz Azure hesabının türüne bağlıdır (Microsoft teklifi). Aşağıdaki tabloda, çeşitli Microsoft teklifleri tarafından desteklenen uyarı özellikleri gösterilmektedir. [Maliyet yönetimi verilerini anlamak](understand-cost-mgt-data.md)için Microsoft tekliflerinin tam listesini görüntüleyebilirsiniz.

| Uyarı türü | Kurumsal Anlaşma | Microsoft Müşteri Sözleşmesi | Web doğrudan/Kullandıkça öde |
|---|---|---|---|
| Bütçe | ✔ | ✔ | ✔ |
| Kredisi | ✔ |✘ | ✘ |
| Departman harcama kotası | ✔ | ✘ | ✘ |



## <a name="view-cost-alerts"></a>Maliyet uyarılarını görüntüle

Maliyet uyarılarını görüntülemek için Azure portal istenen kapsamı açın ve menüdeki **bütçeler** ' ı seçin. Farklı bir kapsama geçiş yapmak için **kapsam** hap ' i kullanın. Menüdeki **Maliyet uyarılarını** seçin. Kapsamlar hakkında daha fazla bilgi için bkz. [kapsamları anlama ve bunlarla çalışma](understand-work-scopes.md).

![Maliyet yönetimi 'nde gösterilen uyarıların örnek görüntüsü](./media/cost-mgt-alerts-monitor-usage-spending/budget-alerts-fullscreen.png)

Etkin ve kapatılan uyarıların toplam sayısı, maliyet uyarıları sayfasında görüntülenir.

Tüm uyarılar uyarı türünü gösterir. Bir bütçe uyarısı, neden oluşturulma nedeninin ve geçerli olduğu bütçenin adını gösterir. Her uyarı oluşturulduğu tarihi, durumunu ve uyarının uygulandığı kapsamı (abonelik veya yönetim grubu) gösterir.

Olası durum **etkin** ve **kapatıldı**içeriyor. Etkin durum, uyarının hala ilgili olduğunu gösterir. Kapatılan durum, birinin uyarıyı artık uygun değil olarak ayarlayacağını gösterir.

Ayrıntılarını görüntülemek için listeden bir uyarı seçin. Uyarı ayrıntıları uyarı hakkında daha fazla bilgi gösterir. Bütçe uyarıları, bütçeye bir bağlantı içerir. Bir bütçe uyarısı için öneri varsa, öneriye bir bağlantı de gösterilir. Bütçe, kredi ve departman harcama kotası uyarıları, uyarının kapsamı için maliyetleri keşfedebileceğiniz maliyet analizinde analiz yapmak için bir bağlantı sağlar. Aşağıdaki örnekte, uyarı ayrıntıları olan bir departman için harcama gösterilmektedir.

![Uyarı ayrıntıları olan bir departman için harcama gösteren örnek resim](./media/cost-mgt-alerts-monitor-usage-spending/dept-spending-selected-with-credits.png)

Kapatılan bir uyarının ayrıntılarını görüntülediğinizde, el ile gerçekleştirilen eylem gerekliyse yeniden etkinleştirebilirsiniz. Aşağıdaki resimde bir örnek gösterilir.

![Kapat ve yeniden etkinleştir seçeneklerini gösteren örnek resim](./media/cost-mgt-alerts-monitor-usage-spending/Dismiss-reactivate-options.png)

## <a name="see-also"></a>Ayrıca bkz.

- Henüz bir bütçe oluşturmadıysanız veya bir bütçe için uyarı koşulları ayarlamadıysanız, [bütçeleri oluştur ve Yönet](tutorial-acm-create-budgets.md) öğreticisini doldurun.
