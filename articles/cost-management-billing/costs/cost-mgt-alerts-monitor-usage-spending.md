---
title: Maliyet uyarıları ile kullanımı ve harcamaları izleme
description: Bu makalede Azure Maliyet Yönetimi'ndeki maliyet uyarılarının kullanımı ve harcamaları izlemenize nasıl yardımcı olduğu anlatılmaktadır.
author: bandersmsft
ms.author: banders
ms.date: 07/24/2020
ms.topic: how-to
ms.service: cost-management-billing
ms.reviewer: adwise
ms.openlocfilehash: ce63756e19e0991ce7aee0b2b6c9c65712b1680b
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87290804"
---
# <a name="use-cost-alerts-to-monitor-usage-and-spending"></a>Maliyet uyarılarını kullanarak kullanımı ve harcamayı izleme

Bu makale Maliyet Yönetimi uyarılarını kullanarak Azure kullanımınızı ve harcamalarınızı izlemeyi öğrenmenize yardımcı olur. Maliyet uyarıları, kullanılan Azure kaynakları için otomatik olarak oluşturulur. Uyarılar, tüm etkin maliyet yönetimi ve faturalandırma uyarılarını tek bir yerde görüntüler. Tüketiminiz belirli bir eşiğe ulaştığında Maliyet Yönetimi uyarı oluşturur. Üç tür maliyet uyarısı vardır: bütçe uyarıları, kredi uyarıları ve departman harcama kotası uyarıları.

## <a name="budget-alerts"></a>Bütçe uyarıları

Bütçe uyarıları, kullanım veya maliyet temelinde harcama, [bütçenin uyarı koşulunda](tutorial-acm-create-budgets.md) tanımlanan tutara ulaştığında veya onu aştığında size bilgi verir. Maliyet Yönetimi bütçeleri, Azure portalı veya [Azure Tüketim](https://docs.microsoft.com/rest/api/consumption) API'si aracılığıyla oluşturulur.

Azure portalında bütçeler maliyete göre tanımlanır. Azure Tüketim API'sini kullanarak bütçeleri maliyete veya tüketim kullanımına göre tanımlayabilirsiniz. Bütçe uyarıları hem maliyet hem de kullanım tabanlı bütçeleri destekler. Bütçe uyarıları, ilgili koşullar karşılandığında otomatik olarak oluşturulur. Tüm maliyet uyarılarını Azure portalında görüntüleyebilirsiniz. Oluşturulan uyarılar, maliyet uyarılarında gösterilir. Ayrıca bütçenin uyarı alıcıları listesindeki kişilere e-posta da gönderilir.

## <a name="credit-alerts"></a>Kredi uyarıları

Kredi uyarıları, Azure kredisi parasal taahhütleriniz tükendiğinde size bilgi verir. Parasal taahhütler, Kurumsal Anlaşma sahibi olan kuruluşlara yöneliktir. Kredi uyarıları, Azure kredisi bakiyenizin %90 ve %100 seviyesinde otomatik olarak oluşturulur. Uyarı oluşturulduğunda maliyet uyarılarına eklenir ve hesap sahiplerine e-posta gönderilir.

## <a name="department-spending-quota-alerts"></a>Departman harcama kotası uyarıları

Departman harcama kotası uyarıları, departman harcamasının sabit kota eşiğine ulaşması durumunda sizi bilgilendirir. Harcama kotaları EA portalında yapılandırılır. Bir eşik karşılandığında departman sahiplerine e-posta gönderilir ve maliyet uyarılarına da eklenir. Örneğin kotanın %50'si veya %75'i eşik olarak belirlenebilir.

## <a name="supported-alert-features-by-offer-categories"></a>Teklif kategorilerine göre desteklenen uyarı özellikleri

Uyarı türleri için sunulan destek, sahip olduğunuz Azure hesabının (Microsoft teklifinin) türüne göre değişir. Aşağıdaki tabloda farklı Microsoft teklifleri tarafından desteklenen uyarı özellikleri gösterilmektedir. Microsoft tekliflerinin tam listesi için bkz. [Maliyet Yönetimi verilerini anlama](understand-cost-mgt-data.md).

| Uyarı türü | Kurumsal Anlaşma | Microsoft Müşteri Sözleşmesi | Web Direct/Kullandıkça Öde |
|---|---|---|---|
| Bütçe | ✔ | ✔ | ✔ |
| Kredi | ✔ |✘ | ✘ |
| Departman harcama kotası | ✔ | ✘ | ✘ |



## <a name="view-cost-alerts"></a>Maliyet uyarılarını görüntüleme

Maliyet uyarılarını görüntülemek için, Azure portalında istediğiniz kapsamı açın ve menüden **Bütçeler**'i seçin. Farklı bir kapsama geçiş yapmak için **Kapsam** düğmesini kullanın. Menüden **Maliyet uyarıları**'nı seçin. Kapsamlar hakkında daha fazla bilgi için bkz. [Kapsamları anlama ve birlikte çalışma](understand-work-scopes.md).

![Maliyet Yönetimi'nde gösterilen uyarıların örnek görüntüsü](./media/cost-mgt-alerts-monitor-usage-spending/budget-alerts-fullscreen.png)

Maliyet uyarıları sayfasında etkin ve kapatılan uyarıların toplam sayısı görüntülenir.

Tüm uyarılarda uyarı türü görüntülenir. Bütçe uyarısı için oluşturulma nedeni ve geçerli olduğu bütçenin adı gösterilir. Her bir uyarı için oluşturma tarihi, durumu ve geçerli olduğu kapsam (abonelik veya yönetim grubu) sunulur.

Uyarıların durumu **etkin** veya **kapatıldı** olabilir. Etkin durum, uyarının hala geçerli olduğunu gösterir. Kapatıldı durumu, birinin uyarıyı işaretleyerek artık geçerli olmadığını belirttiğini gösterir.

Listedeki uyarılardan birini seçerek ayrıntılarını görüntüleyebilirsiniz. Uyarı ayrıntıları, uyarı hakkında daha fazla bilgi sunar. Bütçe uyarıları, bütçe bağlantısını içerir. Bütçe uyarısıyla ilgili öneriler varsa öneri bağlantısı da gösterilir. Bütçe, kredi ve departman harcama kotası uyarılarındaki maliyet analizi bağlantısını kullanarak uyarı kapsamındaki maliyetleri keşfedebilirsiniz. Aşağıdaki örnekte bir departman harcaması ve uyarı ayrıntıları gösterilmektedir.

![Departman harcamasının ve uyarı ayrıntılarının gösterildiği örnek görüntü](./media/cost-mgt-alerts-monitor-usage-spending/dept-spending-selected-with-credits.png)

Kapatılan bir uyarının ayrıntılarını görüntülediğinizde el ile eylem yapılması gerekiyorsa yeniden etkinleştirebilirsiniz. Aşağıdaki resimde bir örnek gösterilir.

![Kapatma ve yeniden etkinleştirme seçeneklerini gösteren örnek görüntü](./media/cost-mgt-alerts-monitor-usage-spending/Dismiss-reactivate-options.png)

## <a name="see-also"></a>Ayrıca bkz.

- Henüz bir bütçe oluşturmadıysanız veya bütçe için uyarı koşullarını belirlemediyseniz [Bütçe oluşturma ve yönetme](tutorial-acm-create-budgets.md) öğreticisini tamamlayın.
