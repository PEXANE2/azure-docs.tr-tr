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
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75988506"
---
# <a name="use-cost-alerts-to-monitor-usage-and-spending"></a>Maliyet uyarılarını kullanarak kullanımı ve harcamayı izleme

Bu makale, Azure kullanımınızı ve harcamalarınızı izlemek için maliyet yönetimi uyarılarını anlamanıza ve kullanmanıza yardımcı olur. Maliyet uyarıları, Azure kaynakları tüketildiği zaman otomatik olarak oluşturulur. Uyarılar tüm etkin maliyet yönetimi ve faturalandırma uyarılarını tek bir yerde gösterir. Tüketiminize verilen bir eşiğe ulaştığında, uyarılar maliyet yönetimi tarafından oluşturulur. Üç tür maliyet uyarısı vardır: bütçe uyarıları, kredi uyarıları ve departman harcama kotası uyarıları.

## <a name="budget-alerts"></a>Bütçe uyarıları

Bütçe uyarıları, kullanım veya maliyete dayanarak, [bütçenin uyarı koşulunda](tutorial-acm-create-budgets.md)tanımlanan miktara ulaştığında veya bu miktarı aştığında size bildirir. Maliyet yönetimi bütçeleri Azure portal veya [Azure tüketim](https://docs.microsoft.com/rest/api/consumption) API 'si kullanılarak oluşturulur.

Azure portalında bütçeler maliyete göre tanımlanır. Azure Tüketim API'si kullanıldığında bütçeler maliyete veya tüketim kullanımına göre tanımlanır. Bütçe uyarıları hem maliyet tabanlı hem de kullanım tabanlı bütçeleri destekler. Bütçe uyarı koşulları her gerçekleştiğinde bütçe uyarıları otomatik olarak oluşturulur. Azure portalında tüm maliyet uyarılarını görüntüleyebilirsiniz. Her uyarı oluşturulduğunda, maliyet uyarılarında gösterilir. Bütçenin uyarı alıcıları listesindeki kişilere de bir uyarı e-postası gönderilir.

## <a name="credit-alerts"></a>Kredi uyarıları

Kredi uyarıları size Azure kredisi parasal taahhütlerinizin tüketildiğini bildirir. Parasal taahhütler Kurumsal Anlaşmaları olan kuruluşlara yöneliktir. Kredi uyarıları, Azure kredisi bakiyenizin %90'ına ve %100'üne ulaşıldığında otomatik olarak oluşturulur. Her uyarı oluşturulduğunda, bu uyarı maliyet uyarılarında ve hesap sahiplerine gönderilen e-postada yansıtılır.

## <a name="department-spending-quota-alerts"></a>Departman harcama kotası uyarıları

Departman harcama kotası uyarıları size departman harcamalarının kotanın sabit bir eşiğine ulaştığını bildirir. Harcama kotaları EA portalında yapılandırılır. Her eşiğe ulaşıldığında departman sahiplerine bir e-posta oluşturulur ve maliyet uyarılarında gösterilir. Örneğin kotanın %50'si veya %75'i olabilir.

## <a name="supported-alert-features-by-offer-categories"></a>Teklif kategorilerine göre desteklenen uyarı özellikleri

Uyarı türleri desteği sahip olduğunuz Azure hesabının türüne (Microsoft teklifi) bağlıdır. Aşağıdaki tabloda, çeşitli Microsoft tekliflerinin desteklediği uyarı özellikleri gösterilir. [Maliyet yönetimi verilerini anlamak](understand-cost-mgt-data.md)için Microsoft tekliflerinin tam listesini görüntüleyebilirsiniz.

| Uyarı türü | Kurumsal Sözleşme | Microsoft Müşteri Sözleşmesi | Web doğrudan/Kullandıkça öde |
|---|---|---|---|
| Bütçe | ✔ | ✔ | ✔ |
| Kredi | ✔ |✘ | ✘ |
| Departman harcama kotası | ✔ | ✘ | ✘ |



## <a name="view-cost-alerts"></a>Maliyet uyarılarını görüntüleme

Maliyet uyarılarını görüntülemek için Azure portalında istenen kapsamı açın ve menüde **Bütçeler**'i seçin. Farklı bir kapsama geçiş yapmak için **Kapsam** hapını kullanın. Menüde **Maliyet uyarıları**'nı seçin. Kapsamlar hakkında daha fazla bilgi için bkz. [kapsamları anlama ve bunlarla çalışma](understand-work-scopes.md).

![Maliyet Yönetimi'nde gösteriler uyarıların örnek resmi](./media/cost-mgt-alerts-monitor-usage-spending/budget-alerts-fullscreen.png)

Etkin ve kapatılmış uyarıların toplam sayısı maliyet uyarıları sayfasında görüntülenir.

Tüm uyarılarda uyarı türü gösterilir. Bütçe uyarısında uyarının oluşturulma nedeni ve uygulandığı bütçenin adı gösterilir. Her uyarı oluşturulduğu tarihi, durumunu ve uyarının uygulandığı kapsamı (abonelik veya yönetim grubu) gösterir.

Olası durumlar **etkin** ve **kapatıldı** durumlarıdır. Etkin durumu uyarının hala geçerli olduğunu belirtir. Kapatıldı durumu birinin uyarıyı artık geçerli değil olarak işaretlediğini belirtir.

Ayrıntılarını görüntülemek için listeden bir uyarı seçin. Uyarı ayrıntılarında uyarı hakkında daha fazla bilgi yer alır. Bütçe uyarıları bütçenin bağlantısını içerir. Bütçe uyarısı için bir öneri sağlanmışsa, önerinin bağlantısı da gösterilir. Bütçe, kredi ve departman harcama kotası uyarılarında, uyarının kapsamı için maliyetleri inceleyebileceğiniz maliyet analizinin bağlantısını yer alır. Aşağıdaki örnekte, uyarı ayrıntıları olan bir departman için harcama gösterilmektedir.

![Uyarı ayrıntıları olan bir departman için harcama gösteren örnek resim](./media/cost-mgt-alerts-monitor-usage-spending/dept-spending-selected-with-credits.png)

Kapatılan bir uyarının ayrıntılarını görüntülediğinizde, el ile gerçekleştirilen eylem gerekliyse yeniden etkinleştirebilirsiniz. Aşağıdaki resimde bir örnek gösterilir.

![Kapat ve yeniden etkinleştir seçeneklerini gösteren örnek resim](./media/cost-mgt-alerts-monitor-usage-spending/Dismiss-reactivate-options.png)

## <a name="see-also"></a>Ayrıca bkz.

- Henüz bir bütçe oluşturmadıysanız veya bir bütçe için uyarı koşulları ayarlamadıysanız, [bütçeleri oluştur ve Yönet](tutorial-acm-create-budgets.md) öğreticisini doldurun.
