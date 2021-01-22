---
title: Maliyet uyarıları ile kullanımı ve harcamaları izleme
description: Bu makalede Azure Maliyet Yönetimi'ndeki maliyet uyarılarının kullanımı ve harcamaları izlemenize nasıl yardımcı olduğu anlatılmaktadır.
author: bandersmsft
ms.author: banders
ms.date: 09/03/2020
ms.topic: how-to
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: adwise
ms.openlocfilehash: d1546ef92d923b9d72c4aca0ddf61d2b93646656
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98602351"
---
# <a name="use-cost-alerts-to-monitor-usage-and-spending"></a>Maliyet uyarılarını kullanarak kullanımı ve harcamayı izleme

Bu makale Maliyet Yönetimi uyarılarını kullanarak Azure kullanımınızı ve harcamalarınızı izlemeyi öğrenmenize yardımcı olur. Maliyet uyarıları, kullanılan Azure kaynakları için otomatik olarak oluşturulur. Uyarılar, tüm etkin maliyet yönetimi ve faturalandırma uyarılarını tek bir yerde görüntüler. Tüketiminiz belirli bir eşiğe ulaştığında Maliyet Yönetimi uyarı oluşturur. Üç tür maliyet uyarısı vardır: bütçe uyarıları, kredi uyarıları ve departman harcama kotası uyarıları.

## <a name="budget-alerts"></a>Bütçe uyarıları

Bütçe uyarıları, kullanım veya maliyet temelinde harcama, [bütçenin uyarı koşulunda](tutorial-acm-create-budgets.md) tanımlanan tutara ulaştığında veya onu aştığında size bilgi verir. Maliyet Yönetimi bütçeleri, Azure portalı veya [Azure Tüketim](/rest/api/consumption) API'si aracılığıyla oluşturulur.

Azure portalında bütçeler maliyete göre tanımlanır. Azure Tüketim API'sini kullanarak bütçeleri maliyete veya tüketim kullanımına göre tanımlayabilirsiniz. Bütçe uyarıları hem maliyet hem de kullanım tabanlı bütçeleri destekler. Bütçe uyarıları, ilgili koşullar karşılandığında otomatik olarak oluşturulur. Tüm maliyet uyarılarını Azure portalında görüntüleyebilirsiniz. Oluşturulan uyarılar, maliyet uyarılarında gösterilir. Ayrıca bütçenin uyarı alıcıları listesindeki kişilere e-posta da gönderilir.

Bütçe API’sini kullanarak farklı bir dilde e-posta uyarısı gönderebilirsiniz. Daha fazla bilgi için bkz. [Bütçe uyarısı e-postaları için desteklenen yerel ayarlar](manage-automation.md#supported-locales-for-budget-alert-emails).

## <a name="credit-alerts"></a>Kredi uyarıları

Kredi uyarıları Azure Ön Ödemeniz (eski adıyla parasal taahhüt) tükendiğinde size bunu bildirir. Azure Ön Ödemesi, Kurumsal Anlaşma sahibi olan kuruluşlara yöneliktir. Kredi uyarıları, Azure Ön Ödeme kredisi bakiyenizin %90 ve %100 düzeyinde otomatik olarak oluşturulur. Her uyarı oluşturulduğunda, bu uyarı maliyet uyarılarında ve hesap sahiplerine gönderilen e-postada yansıtılır.

## <a name="department-spending-quota-alerts"></a>Departman harcama kotası uyarıları

Departman harcama kotası uyarıları size departman harcamalarının kotanın sabit bir eşiğine ulaştığını bildirir. Harcama kotaları EA portalında yapılandırılır. Her eşiğe ulaşıldığında departman sahiplerine bir e-posta oluşturulur ve maliyet uyarılarında gösterilir. Örneğin kotanın %50'si veya %75'i olabilir.

## <a name="supported-alert-features-by-offer-categories"></a>Teklif kategorilerine göre desteklenen uyarı özellikleri

Uyarı türleri desteği sahip olduğunuz Azure hesabının türüne (Microsoft teklifi) bağlıdır. Aşağıdaki tabloda, çeşitli Microsoft tekliflerinin desteklediği uyarı özellikleri gösterilir. Microsoft tekliflerinin tam listesi için bkz. [Maliyet Yönetimi verilerini anlama](understand-cost-mgt-data.md).

| Uyarı türü | Kurumsal Anlaşma | Microsoft Müşteri Sözleşmesi | Web Direct/Kullandıkça Öde |
|---|---|---|---|
| Bütçe | ✔ | ✔ | ✔ |
| Kredi | ✔ |✘ | ✘ |
| Departman harcama kotası | ✔ | ✘ | ✘ |



## <a name="view-cost-alerts"></a>Maliyet uyarılarını görüntüleme

Maliyet uyarılarını görüntülemek için Azure portalında istenen kapsamı açın ve menüde **Bütçeler**'i seçin. Farklı bir kapsama geçiş yapmak için **Kapsam** hapını kullanın. Menüde **Maliyet uyarıları**’nı seçin. Kapsamlar hakkında daha fazla bilgi için bkz. [Kapsamları anlama ve birlikte çalışma](understand-work-scopes.md).

![Maliyet Yönetimi'nde gösterilen uyarıların örnek görüntüsü](./media/cost-mgt-alerts-monitor-usage-spending/budget-alerts-fullscreen.png)

Etkin ve kapatılmış uyarıların toplam sayısı maliyet uyarıları sayfasında görüntülenir.

Tüm uyarılarda uyarı türü gösterilir. Bütçe uyarısında uyarının oluşturulma nedeni ve uygulandığı bütçenin adı gösterilir. Her bir uyarı için oluşturma tarihi, durumu ve geçerli olduğu kapsam (abonelik veya yönetim grubu) sunulur.

Olası durumlar **etkin** ve **kapatıldı** durumlarıdır. Etkin durumu uyarının hala geçerli olduğunu belirtir. Kapatıldı durumu birinin uyarıyı artık geçerli değil olarak işaretlediğini belirtir.

Ayrıntılarını görüntülemek için listeden bir uyarı seçin. Uyarı ayrıntılarında uyarı hakkında daha fazla bilgi yer alır. Bütçe uyarıları bütçenin bağlantısını içerir. Bütçe uyarısı için bir öneri sağlanmışsa, önerinin bağlantısı da gösterilir. Bütçe, kredi ve departman harcama kotası uyarılarında, uyarının kapsamı için maliyetleri inceleyebileceğiniz maliyet analizinin bağlantısını yer alır. Aşağıdaki örnekte bir departman harcaması ve uyarı ayrıntıları gösterilmektedir.

![Departman harcamasının ve uyarı ayrıntılarının gösterildiği örnek görüntü](./media/cost-mgt-alerts-monitor-usage-spending/dept-spending-selected-with-credits.png)

Kapatılan bir uyarının ayrıntılarını görüntülediğinizde el ile eylem yapılması gerekiyorsa yeniden etkinleştirebilirsiniz. Aşağıdaki resimde bir örnek gösterilir.

![Kapatma ve yeniden etkinleştirme seçeneklerini gösteren örnek görüntü](./media/cost-mgt-alerts-monitor-usage-spending/Dismiss-reactivate-options.png)

## <a name="see-also"></a>Ayrıca bkz.

- Henüz bir bütçe oluşturmadıysanız veya bütçe için uyarı koşullarını belirlemediyseniz [Bütçe oluşturma ve yönetme](tutorial-acm-create-budgets.md) öğreticisini tamamlayın.