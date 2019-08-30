---
title: Azure geçişi 'nde bağımlılık görselleştirme | Microsoft Docs
description: Azure geçişi 'nde sunucu değerlendirmesi hizmeti 'nde değerlendirme hesaplamalarına genel bakış sağlar
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: hamusa
ms.openlocfilehash: e7b69e57d4e01c292a2c6d032ba995d1936b88dc
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70142898"
---
# <a name="dependency-visualization"></a>Bağımlılık görselleştirmesi

Azure Geçişi: Sunucu değerlendirmesi, Azure 'a geçiş için şirket içi makinelerin değerlendirir gruplarıdır. Grupları oluşturmak için sunucu değerlendirmesi ' nde bağımlılık görselleştirme işlevini kullanabilirsiniz. Bu makale, bu özellik hakkında bilgi sağlar.

> [!NOTE]
> Bağımlılık görselleştirme işlevselliği Azure Kamu 'da kullanılamaz.

## <a name="overview"></a>Genel Bakış

Sunucu değerlendirmesindeki bağımlılık görselleştirme, geçiş değerlendirmeleri için yüksek güvenilirlikli gruplar oluşturmanızı sağlar. Bağımlılık görselleştirmesini kullanarak, makinelerin ağ bağımlılıklarını görüntüleyebilir ve Azure 'a birlikte geçirilmesi gereken ilgili makineleri belirleyebilirsiniz. Bu işlevsellik, uygulamanızı oluşturan ve Azure 'a birlikte geçirilmesi gereken makinelere tamamen haberdar olduğunuz senaryolarda yararlıdır.

## <a name="before-you-start"></a>Başlamadan önce

- Bir Azure geçişi projesi [oluşturduğunuzdan](how-to-add-tool-first-time.md) emin olun.
- Zaten bir proje oluşturduysanız, Azure geçişini eklediğinizden emin olun: [](how-to-assess.md) Sunucu değerlendirmesi aracı.
- Azure geçişi 'nde makinelerinizi keşfetdiğinizden emin olun; Bunu, [VMware](how-to-set-up-appliance-vmware.md) veya [Hyper-V](how-to-set-up-appliance-hyper-v.md)için bir Azure geçiş gereci ayarlayarak yapabilirsiniz. Gereç, şirket içi makineleri bulur ve Azure geçişi 'ne meta veri ve performans verileri gönderir: etmenize yardımcı olur. [Daha fazla bilgi edinin](migrate-appliance.md).

## <a name="how-does-it-work"></a>Nasıl çalışır?

Azure geçişi, bağımlılık görselleştirmesi için [Azure izleyici günlüklerinde](../log-analytics/log-analytics-overview.md) [hizmet eşlemesi](../operations-management-suite/operations-management-suite-service-map.md) çözümünü kullanır.
- Bağımlılık görselleştirmesini yükseltmek için, yeni veya mevcut bir Log Analytics çalışma alanını Azure geçişi projesi ile ilişkilendirmeniz gerekir.
- Yalnızca Azure geçişi projesinin oluşturulduğu abonelikte bir çalışma alanı oluşturabilir veya ekleyebilirsiniz.
- Bir projeye Log Analytics çalışma alanı iliştirmek için:
    1. **Sunucular** sekmesinde, **Azure geçişi: Sunucu değerlendirmesi** kutucuğu, **Genel Bakış ' a**tıklayın.
    2. **Genel bakış**bölümünde, **temelleri**genişletmek için aşağı oka tıklayın.
    3. **OMS çalışma alanında** **yapılandırma gerekiyor**' a tıklayın.
    4. **Çalışma alanını Yapılandır**' da, yeni bir çalışma alanı oluşturmak isteyip istemediğinizi belirtin veya var olanı kullanın:
    
    ![Çalışma alanı ekle](./media/how-to-create-group-machine-dependencies/workspace.png)

- Bir çalışma alanı ilişkilendirirken, yeni bir çalışma alanı oluşturma veya var olanı iliştirme seçeneğini alacaksınız:
  - Yeni bir çalışma alanı oluşturduğunuzda, çalışma alanı için bir ad belirtmeniz gerekir. Çalışma alanı daha sonra geçiş projesiyle aynı [Azure Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/) içindeki bir bölgede oluşturulur.
  - Mevcut bir çalışma alanını eklediğinizde, geçiş projesiyle aynı abonelikte bulunan tüm kullanılabilir çalışma alanlarını seçebilirsiniz. Yalnızca [hizmet eşlemesi desteklendiği](../azure-monitor/insights/vminsights-enable-overview.md#prerequisites)bir bölgede oluşturulan çalışma alanlarının listelendiğini unutmayın. Bir çalışma alanı iliştirebilmek için, çalışma alanına ' okuyucu ' erişiminizin olduğundan emin olun.

  > [!NOTE]
  > Bir projeye çalışma alanı iliştirdikten sonra, daha sonra değiştiremezsiniz.

- İlişkili çalışma alanı, anahtar **geçiş projesiyle**etiketlenebilir ve Azure Portal arama yapmak için kullanabileceğiniz değer **proje adıdır**.
- Projeyle ilişkili çalışma alanına gitmek için projeye **genel bakış** sayfasının **Essentials** bölümüne gidebilir ve çalışma alanına erişebilirsiniz

    ![Log Analytics çalışma alanında gezin](./media/concepts-dependency-visualization/oms-workspace.png)

Bağımlılık görselleştirmesini kullanmak için, çözümlemek istediğiniz her şirket içi makineye aracıları indirip yüklemeniz gerekir.  

- [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) her makineye yüklenmelidir. MMA aracısının nasıl yükleneceği hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies#install-the-mma) .
- [Bağımlılık aracısının](../azure-monitor/platform/agents-overview.md#dependency-agent) her makinede yüklü olması gerekir. Bağımlılık aracısının nasıl yükleneceği hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies#install-the-dependency-agent) .
- Ayrıca, İnternet bağlantısı olmayan makineleriniz varsa, bu makinelere Log Analytics ağ geçidini indirip yüklemeniz gerekir.

Bağımlılık görselleştirmesi kullanmadığınız takdirde değerlendirmek istediğiniz makinelerde bu aracılara ihtiyacınız yoktur.

## <a name="do-i-need-to-pay-for-it"></a>Bunun için ödeme yapmam gerekiyor mu?

Bağımlılık görselleştirme özelliği ek bir ücret ödemeden kullanılabilir. Sunucu değerlendirmesi içindeki bağımlılık görselleştirme özelliğinin kullanılması Hizmet Eşlemesi gerektirir ve Azure geçişi projesi ile yeni veya var olan bir Log Analytics çalışma alanını ilişkilendirmeniz gerekir. Sunucu değerlendirmesindeki bağımlılık görselleştirme işlevselliği ilk 180 gün boyunca ücretsizdir.

1. Bu Log Analytics çalışma alanındaki Hizmet Eşlemesi dışındaki çözümlerin kullanımı [standart Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) ücretlendirmeye tabi olacaktır.
2. Geçiş senaryolarını ek bir ücret ödemeden desteklemek için Hizmet Eşlemesi çözümü, Log Analytics çalışma alanını Azure geçişi projesi ile ilişkilendirme gününden itibaren ilk 180 gün boyunca ücret ödemez. 180 gün sonra standart Log Analytics ücretleri uygulanır.

Aracıları çalışma alanına kaydettiğinizde, aracı adımları yüklensin sayfasında proje tarafından verilen KIMLIĞI ve anahtarı kullanın.

Azure geçişi projesi silindiğinde, çalışma alanı onunla birlikte silinmez. Proje silme işlemini postala, Hizmet Eşlemesi kullanımı ücretsizdir ve her düğüm, Log Analytics çalışma alanının ücretli katmanına göre ücretlendirilecektir.

> [!NOTE]
> Bağımlılık görselleştirme özelliği bir Log Analytics çalışma alanı aracılığıyla Hizmet Eşlemesi kullanır. 28 Şubat 2018 ' den itibaren, Azure 'un genel kullanıma sunulmasından dolayı, özellik artık ek ücret ödemeden kullanılabilir. Ücretsiz kullanım çalışma alanını kullanmak için yeni bir proje oluşturmanız gerekecektir. Genel kullanılabilirlik öncesinde mevcut çalışma alanları ücrete tabi olmaya devam ediyor, bu nedenle yeni bir projeye geçiş yapmanızı öneririz.

Azure Geçişi fiyatlandırması hakkında daha fazla bilgiyi [burada](https://azure.microsoft.com/pricing/details/azure-migrate/) bulabilirsiniz.

## <a name="how-do-i-manage-the-workspace"></a>Çalışma alanını yönetmek Nasıl yaparım? mı?

Log Analytics çalışma alanını Azure geçişi dışında kullanabilirsiniz. Oluşturulduğu Azure geçişi projesini silerseniz silinmez. Artık çalışma alanına ihtiyacınız yoksa el ile [silin](../azure-monitor/platform/manage-access.md) .

Azure geçişi projesini silmediğiniz takdirde Azure geçişi tarafından oluşturulan çalışma alanını silmeyin. Bunu yaparsanız, bağımlılık görselleştirme işlevselliği beklendiği gibi çalışmaz.

## <a name="next-steps"></a>Sonraki adımlar
- [Makine bağımlılıklarını kullanan makineleri gruplandırın](how-to-create-group-machine-dependencies.md)
- Bağımlılık görselleştirmesinde SSS hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/resources-faq#what-is-dependency-visualization) .
