---
title: Öğretici - Önerilerle Azure maliyetlerini azaltma
description: Bu öğretici, iyileştirme önerilerini kullanarak Azure maliyetlerinizi azaltmanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/24/2019
ms.topic: conceptual
ms.service: cost-management-billing
manager: dougeby
ms.custom: seodec18
ms.openlocfilehash: 04b3802ae17440fe7cb96b424727600ab53aefce
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77168761"
---
# <a name="tutorial-optimize-costs-from-recommendations"></a>Öğretici: Önerilerle maliyetleri iyileştirme

Azure Maliyet Yönetimi, Azure Danışmanı ile birlikte çalışarak maliyet iyileştirme önerileri sunar. Azure Danışmanı boşta olan ve az kullanılan kaynakları belirleyerek verimliliği iyileştirmenize ve geliştirmenize yardımcı olur. Bu öğretici bir örnek üzerinden az kullanılan Azure kaynaklarını belirleme ve maliyetleri azaltmak için gerekli eylemleri gerçekleştirme konusunda size yol gösterecektir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Maliyet iyileştirme önerilerini görüntüleyip olası kullanım verimsizliklerini görme
> * Öneri üzerine bir sanal makineyi yeniden boyutlandırarak maliyet açısından daha verimli bir seçeneğe geçiş yapma
> * Sanal makinenin başarıyla yeniden boyutlandırıldığından emin olmak için eylemi doğrulama

## <a name="prerequisites"></a>Ön koşullar
Farklı kapsamlar ve Azure hesabı türleri için öneriler mevcuttur. Desteklenen hesap türlerinin tam listesini görüntülemek için, bkz. [Maliyet Yönetimi verilerini anlama](understand-cost-mgt-data.md). Maliyet verilerini görüntülemek için aşağıdaki kapsamlardan birine veya daha fazlasına en azından yazma erişiminiz olmalıdır. Kapsamlar hakkında daha fazla bilgi için bkz. [Kapsamları anlama ve birlikte çalışma](understand-work-scopes.md).

- Abonelik
- Kaynak grubu

En az 14 günlük etkinliğe sahip etkin sanal makinelere sahip olmanız gerekir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com/) adresinden Azure portalında oturum açın.

## <a name="view-cost-optimization-recommendations"></a>Maliyet iyileştirme önerilerini görüntüleme

Bir aboneliğe yönelik maliyet iyileştirme önerilerini görüntülemek için istediğiniz kapsamı Azure portalında açın ve **Danışman önerileri**'ni seçin.

Bir yönetim grubuna yönelik önerileri görüntülemek için istediğiniz kapsamı Azure portalında açın ve menüden **Maliyet analizi**'ni seçin. Yönetim grubu gibi farklı bir kapsama geçiş yapmak için **Kapsam** düğmesini kullanın. Menüden **Danışman önerileri**'ni seçin. Kapsamlar hakkında daha fazla bilgi için bkz. [Kapsamları anlama ve birlikte çalışma](understand-work-scopes.md).

![Azure portalında gösterilen Maliyet Yönetimi Danışman önerileri](./media/tutorial-acm-opt-recommendations/advisor-recommendations.png)

Öneriler listesi kullanım verimsizliklerini tanımlar veya tasarruf etmenize yardımcı olabilecek satın alma önerilerini gösterir. Toplam **Olası yıllık tasarruf** rakamı, öneri kurallarına uyan tüm VM'lerinizi kapatmanız veya serbest bırakmanız durumunda tasarruf edebileceğiniz toplam tutarı gösterir. VM'lerinizi kapatmak istemiyorsanız yeniden boyutlandırarak daha düşük maliyetli bir SKU'ya geçirebilirsiniz.

**Olası yıllık tasarruf** rakamına ek olarak **Etki** kategorisi, mümkün olduğunca tasarruf etmenizi sağlayabilecek önerileri tanımlamanıza yardımcı olmak üzere tasarlanmıştır.

Yüksek düzeyde etkiye sahip öneriler şunlardır:
- [Kullandıkça öde maliyetlerinden tasarruf sağlamak için ayrılmış sanal makine örnekleri satın alın](../../advisor/advisor-cost-recommendations.md#buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs)
- [Az kullanılan örnekleri yeniden boyutlandırarak veya kapatarak sanal makineyi harcamalarını iyileştirin](../../advisor/advisor-cost-recommendations.md#optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances)
- [Yönetilen Diskler anlık görüntülerini depolamak için Standart Depolama kullanın](../../advisor/advisor-cost-recommendations.md#use-standard-snapshots-for-managed-disks)

Orta düzeyde etkiye sahip öneriler şunlardır:
- [Başarısız olan Azure Data Factory işlem hatlarını silin](../../advisor/advisor-cost-recommendations.md#delete-azure-data-factory-pipelines-that-are-failing)
- [Sağlanmamış ExpressRoute devrelerini ortadan kaldırarak maliyetleri düşürün](../../advisor/advisor-cost-recommendations.md#reduce-costs-by-eliminating-unprovisioned-expressroute-circuits)
- [Boştaki sanal ağ geçitlerini silerek veya yeniden yapılandırarak maliyetleri azaltın](../../advisor/advisor-cost-recommendations.md#reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways)

## <a name="act-on-a-recommendation"></a>Öneriye göre hareket etme

Azure Danışmanı, sanal makinelerinizin kullanımını yedi gün boyunca izler ve az kullanılan sanal makineleri tespit eder. Dört gün veya daha uzun bir süre boyunca CPU kullanımı yüzde beş veya altı, ağ kullanımı ise yedi MB veya altı olan sanal makineler, az kullanılan sanal makineler olarak kabul edilir.

%5 veya daha az CPU kullanımı varsayılan değerdir ancak ayarları değiştirebilirsiniz. Ayarları değiştirme hakkında daha fazla bilgi için bkz. [Ortalama CPU kullanımı kuralını veya az kullanılan sanal makine önerisini yapılandırma](../../advisor/advisor-get-started.md#configure-low-usage-vm-recommendation).

Belirli senaryolarda tasarım gereği düşük kullanım söz konusu olsa da genellikle sanal makinelerinizin boyutunu daha düşük fiyatlı olanlarla değiştirerek paradan tasarruf edebilirsiniz. Yeniden boyutlandırma işlemi gerçekleştirmeniz durumunda gerçek tasarruf miktarı farklı olabilir. Bir sanal makineyi yeniden boyutlandırma örneğine bakalım.

Öneriler listesinde **Az kullanılan sanal makineleri doğru boyuta getirin veya kapatın** önerisine tıklayın. Sanal makine adayları listesinden yeniden boyutlandırma için bir sanal makine seçin ve ardından sanal makineye tıklayın. Kullanım ölçümlerini doğrulayabilmeniz için sanal makinenin ayrıntıları gösterilir. **Olası yıllık tasarruf** değeri, VM'yi kapatmanız veya kaldırmanız durumunda elde edebileceğiniz tasarrufu gösterir. VM'yi yeniden boyutlandırmak da tasarruf etmenizi sağlar ancak olası yıllık tasarruf değerinin tamamına ulaşmanız mümkün olmayabilir.

![Öneri ayrıntıları örneği](./media/tutorial-acm-opt-recommendations/recommendation-details.png)

VM ayrıntılarında, sanal makinenin uygun bir yeniden boyutlandırma adayı olduğunu doğrulamak için kullanım miktarını kontrol edin.

![Geçmiş kullanımı gösteren VM ayrıntıları örneği](./media/tutorial-acm-opt-recommendations/vm-details.png)

Geçerli sanal makinenin boyutuna dikkat edin. Sanal makinenin yeniden boyutlandırılması gerektiğini doğruladıktan sonra, sanal makinelerin listesini görmek için VM ayrıntılarını kapatın.

Kapatma veya yeniden boyutlandırma adayları listesinde **_&lt;FromVirtualMachineSKU&gt;_ boyutundaki sanal makineyi _&lt;ToVirtualMachineSKU&gt;_ olarak yeniden boyutlandır** ifadesini seçin.
![Sanal makineyi yeniden boyutlandırma seçeneğine sahip örnek öneri](./media/tutorial-acm-opt-recommendations/resize-vm.png)

Sonraki adımda kullanılabilir yeniden boyutlandırma seçeneklerinin bir listesi sunulur. Senaryonuz için en iyi performansı ve maliyet verimliliğini sağlayacak olanı seçin. Aşağıdaki örnekte **Standard_D8s_v3** boyutundaki sanal makine için **Standard_D2s_v3** boyutu seçilmiştir.

![Arasından seçim yapabileceğiniz kullanılabilir VM boyutlarının listesi](./media/tutorial-acm-opt-recommendations/choose-size.png)

Uygun bir boyut seçtikten sonra yeniden boyutlandırma eylemini başlatmak için **Yeniden boyutlandır**'a tıklayın.

Çalışan bir makinenin yeniden boyutlandırılabilmesi için yeniden başlatılması gerekir. Sanal makine bir üretim ortamındaysa yeniden boyutlandırma işlemini çalışma saatlerinden sonra çalıştırmanızı öneririz. Yeniden başlatmanın zamanlanması, sistemin geçici olarak kullanım dışı kalması nedeniyle oluşan kesintileri azaltabilir.

## <a name="verify-the-action"></a>Eylemi doğrulama

VM yeniden boyutlandırma işlemi başarıyla tamamlandığında bir Azure bildirimi gösterilir.

![Başarıyla yeniden boyutlandırılan sanal makine bildirimi](./media/tutorial-acm-opt-recommendations/resized-notification.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Maliyet iyileştirme önerilerini görüntüleyip olası kullanım verimsizliklerini görme
> * Öneri üzerine bir sanal makineyi yeniden boyutlandırarak maliyet açısından daha verimli bir seçeneğe geçiş yapma
> * Sanal makinenin başarıyla yeniden boyutlandırıldığından emin olmak için eylemi doğrulama

Maliyet Yönetimi en iyi deneyimleri makalesini henüz okumadıysanız, maliyetlerin yönetilmesine yardımcı olmak için göz önünde bulundurmanız gereken üst düzey yönergeler ve ilkeler bulabilirsiniz.

> [!div class="nextstepaction"]
> [Maliyet Yönetimi en iyi deneyimleri](cost-mgt-best-practices.md)
