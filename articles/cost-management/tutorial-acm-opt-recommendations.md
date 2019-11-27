---
title: Öğretici - iyileştirme önerilerden yararlanarak Azure maliyetlerini azaltma | Microsoft Docs
description: Bu öğreticide, iyileştirme önerileri hareket olduğunda Azure maliyetleri azaltmanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/24/2019
ms.topic: conceptual
ms.service: cost-management-billing
manager: dougeby
ms.custom: seodec18
ms.openlocfilehash: a9dbb121cab49024aaf0dc65bbac938764d9f8b2
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74229843"
---
# <a name="tutorial-optimize-costs-from-recommendations"></a>Öğretici: önerilerden maliyetleri en iyi duruma getirme

Azure maliyet yönetimi, maliyet iyileştirme önerileri sağlamak için Azure Danışmanı ile çalışır. Azure Danışmanı en iyi duruma getirmek ve boşta ve az kullanılan kaynakları belirleyerek verimliliğini geliştirmenize yardımcı olur. Bu öğretici size burada isteyeceğiniz az kullanılan Azure kaynakları tanımlamak ve maliyetleri azaltmak için eyleme sonra bir örneği açıklanmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Maliyet iyileştirme önerileri olası kullanım verimsizliklerini görüntülemek için görüntüleme
> * Bir öneri daha uygun maliyetli bir seçenek için bir sanal makine yeniden boyutlandırmak için eylem gerçekleştir
> * Sanal makine başarıyla boyutlandırılmış emin olmak için gerekeni doğrulayın

## <a name="prerequisites"></a>Önkoşullar
Çeşitli kapsamlar ve Azure hesap türleri için öneriler mevcuttur. Desteklenen hesap türlerinin tam listesini görüntülemek için bkz. [maliyet yönetimi verilerini anlama](understand-cost-mgt-data.md). Maliyet verilerini görüntülemek için aşağıdaki kapsamlardan birine veya daha fazlasına en azından yazma erişiminiz olmalıdır. Kapsamlar hakkında daha fazla bilgi için bkz. [kapsamları anlama ve bunlarla çalışma](understand-work-scopes.md).

- Abonelik
- Kaynak grubu

Etkinlik için en az 14 gün etkin sanal makinelere sahip olmalıdır.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com/) adresinden Azure portalında oturum açın.

## <a name="view-cost-optimization-recommendations"></a>Maliyet iyileştirme önerileri görüntüleme

Bir aboneliğin maliyet iyileştirme önerilerini görüntülemek için Azure portal istenen kapsamı açın ve **danışman önerileri**' ni seçin.

Bir yönetim grubunun önerilerini görüntülemek için, istenen kapsamı Azure portal açın ve menüden **Maliyet Analizi** ' ni seçin. Yönetim grubu gibi farklı bir kapsama geçiş yapmak için **kapsam** hap ' i kullanın. Menüdeki **danışman önerilerini** seçin. Kapsamlar hakkında daha fazla bilgi için bkz. [kapsamları anlama ve bunlarla çalışma](understand-work-scopes.md).

![Azure portalında gösterilen maliyet Yönetimi Danışmanı önerileri](./media/tutorial-acm-opt-recommendations/advisor-recommendations.png)

Öneriler listesi, kullanım verimsizliklerini tanımlar veya ek paradan tasarruf etmenize yardımcı olabilecek satın alma önerileri gösterilir. Toplanan **olası yıllık tasarruflar** , öneri kurallarını karşılayan tüm VM 'lerinizi kapatırsanız veya serbest bırakırsanız kaydedebilmeniz gereken toplam miktarı gösterir. Bunları kapatmak istemiyorsanız, bunları daha ucuz bir VM SKU için yeniden boyutlandırma düşünmelisiniz.

Olası **yıllık tasarruflarla**birlikte **etki** kategorisi, mümkün olduğunca tasarruf etme potansiyeli olan önerileri belirlemenize yardımcı olmak için tasarlanmıştır.

Yüksek etki önerileri şunlardır:
- [Kullandıkça Öde maliyetlerinden tasarruf etmek için ayrılmış sanal makine örnekleri satın alın](../advisor/advisor-cost-recommendations.md#buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs)
- [Az kullanılan örnekleri yeniden boyutlandırarak veya kapatarak sanal makineyi harcamayı iyileştirin](../advisor/advisor-cost-recommendations.md#optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances)
- [Yönetilen disk anlık görüntülerini depolamak için standart depolama kullanma](../advisor/advisor-cost-recommendations.md#use-standard-snapshots-for-managed-disks)

Orta etki önerileri şunlardır:
- [Başarısız olan Azure Data Factory işlem hatlarını silme](../advisor/advisor-cost-recommendations.md#delete-azure-data-factory-pipelines-that-are-failing)
- [Sağlanmamış ExpressRoute devreleri ortadan kaldırarak maliyetleri düşürün](../advisor/advisor-cost-recommendations.md#reduce-costs-by-eliminating-unprovisioned-expressroute-circuits)
- [Boştaki Sanal ağ geçitlerini silerek veya yeniden yapılandırarak maliyetleri azaltın](../advisor/advisor-cost-recommendations.md#reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways)

## <a name="act-on-a-recommendation"></a>Bir öneriye hareket

Azure Danışmanı, sanal makine kullanımınızı yedi gün boyunca izler ve daha sonra az kullanılan sanal makineleri tanımlar. Yedi MB, CPU kullanımı yüzde veya daha az beştir ve ağ kullanımı sanal makineleri olması veya kullanımı düşük sanal makineleri için dört daha az veya daha fazla gün olarak kabul edilir.

%5 veya daha az CPU kullanımı varsayılan ayardır, ancak ayarları ayarlayabilirsiniz. Ayarı ayarlama hakkında daha fazla bilgi için, [Ortalama CPU kullanımı kuralını yapılandırma veya düşük kullanım sanal makine önerisi](../advisor/advisor-get-started.md#configure-low-usage-vm-recommendation)bölümüne bakın.

Bazı senaryolar tasarım gereği düşük kullanımı sonuçlanabilir olsa da, genellikle daha ucuz boyutları için sanal makinelerinizin boyutunu değiştirerek tasarruf sağlayabilirsiniz. Bir yeniden boyutlandırma eylemi seçerseniz tasarruf ettiğiniz gerçek miktarlar değişiklik gösterebilir. Bir sanal makine yeniden boyutlandırma, bir örneği atalım.

Öneriler listesinde, **doğru boyut ' a tıklayın veya kapalı sanal makineler** önerisi ' ne tıklayın. Sanal makine adayları listesinde, yeniden boyutlandırma ve ardından sanal makineyi bir sanal makine seçin. Kullanım ölçümlerini doğrulayabilir sanal makinenin ayrıntıları gösterilir. **Olası yıllık tasarruf** değeri, sanal makineyi kapatırsanız veya kaldırırsanız kaydedebilecekleri şeydir. Bir VM'yi yeniden boyutlandırmadan büyük olasılıkla, maliyet tasarrufu sağlayacak, ancak tam olası yıllık tasarruf miktarı kaydetmez.

![Öneri ayrıntılarını örneği](./media/tutorial-acm-opt-recommendations/recommendation-details.png)

VM ayrıntılarında uygun boyutlandırma aday olduğundan emin olmak için sanal makine kullanımını denetleyin.

![Geçmiş kullanımını gösteren örnek sanal makine ayrıntıları](./media/tutorial-acm-opt-recommendations/vm-details.png)

Geçerli sanal makinenin boyutu unutmayın. Sanal makine yeniden boyutlandırıldı olduğunu doğruladıktan sonra sanal makinelerin listesini görebilmesi için sanal makine ayrıntıları kapatın.

Kapatılacak veya yeniden boyutlandırılacak aday listesinde, * * yeniden boyutlandır *&lt;Fromvirtualdeninesku&gt;* "&lt;Tovirtual, inesku&gt;* * * olarak belirleyin.
sanal makineyi yeniden boyutlandırma seçeneğiyle ![örnek önerisi](./media/tutorial-acm-opt-recommendations/resize-vm.png)

Ardından, kullanılabilir boyutlandırma seçeneklerin bir listesi ile sunulur. Senaryonuz için en iyi performans ve uygun maliyet sağlayacak bir tane seçin. Aşağıdaki örnekte, seçilen seçenek **Standard_D8s_v3** **Standard_D2s_v3**olarak yeniden boyutlandırır.

![Bir boyut seçebileceğiniz kullanılabilir VM boyutlarını örnek listesi](./media/tutorial-acm-opt-recommendations/choose-size.png)

Uygun bir boyut seçtikten sonra yeniden boyutlandır eylemini başlatmak için yeniden **Boyutlandır** ' ı tıklatın.

Yeniden boyutlandırma, yeniden başlatmak için etkin olarak çalışan bir sanal makineyi gerektirir. Bir üretim ortamında sanal makine ise iş saat sonra yeniden boyutlandırma işlemi çalıştırmanızı öneririz. Yeniden başlatma zamanlama tarafından kısa bir süre içinde kullanılabilir olmaması nedeniyle kesintileri azaltabilir.

## <a name="verify-the-action"></a>Eylemi doğrula

VM yeniden boyutlandırma işlemi başarıyla tamamlandığında, Azure bir bildirim gösterilir.

![Sanal makine yeniden boyutlandırıldı başarılı bildirimi](./media/tutorial-acm-opt-recommendations/resized-notification.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Maliyet iyileştirme önerileri olası kullanım verimsizliklerini görüntülemek için görüntüleme
> * Bir öneri daha uygun maliyetli bir seçenek için bir sanal makine yeniden boyutlandırmak için eylem gerçekleştir
> * Sanal makine başarıyla boyutlandırılmış emin olmak için gerekeni doğrulayın

Maliyet yönetimi en iyi yöntemler makalesi okumadıysanız, maliyetleri yönetmenize yardımcı olmak için dikkate alınması gereken üst düzey yönergeler ve sağlar.

> [!div class="nextstepaction"]
> [Maliyet yönetimi en iyi uygulamaları](cost-mgt-best-practices.md)
