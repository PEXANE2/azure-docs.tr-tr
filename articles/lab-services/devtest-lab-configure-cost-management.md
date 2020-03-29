---
title: Azure DevTest Labs'da aylık tahmini laboratuvar maliyet trendini görüntüleyin
description: Bu makalede, Azure DevTest Labs'daki laboratuvarınızın maliyetinin (aylık tahmini maliyet eğilim grafiği) nasıl izleyebilirsiniz hakkında bilgi verilmektedir.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 2e4fe55fac5edf73e16df05bd38cc2712a94377d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721736"
---
# <a name="track-costs-associated-with-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki bir laboratuvarla ilişkili maliyetleri izleme
Bu makalede, laboratuvarınızın maliyetini izleme hakkında bilgi verilmektedir. Laboratuvar için geçerli takvim ayı için tahmini maliyet trent'i nasıl görüntüleyebilirsiniz gösterir. Makalede ayrıca, laboratuvarda kaynak başına aylık maliyetin nasıl görüntülenebilir olduğu da gösterilmektedir.

## <a name="view-the-monthly-estimated-lab-cost-trend"></a>Aylık tahmini laboratuvar maliyet eğilimini görüntüleme 
Bu bölümde, geçerli takvim ayının tahmini maliyet-to-date ve geçerli takvim ayı için öngörülen ay sonu maliyetini görüntülemek için **Aylık Tahmini Maliyet Eğilim** grafiğini nasıl kullanacağınızı öğrenirsiniz. Ayrıca, ulaşıldığında sonuçları size bildirmesi için DevTest Labs'ı tetikleyen harcama hedefleri ve eşikleri ayarlayarak laboratuvar maliyetlerini nasıl yönettiğinizi de öğrenirsiniz.

Aylık Tahmini Maliyet Eğilimi grafiğini görüntülemek için aşağıdaki adımları izleyin: 

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.
3. Laboratuvarlar listesinden, laboratuvarınızı seçin.  
4. Sol menüde **Yapılandırma ve ilkeler'i** seçin.  
4. Sol menüdeki **Maliyet izleme** bölümünde **Maliyet eğilimini** seçin. Aşağıdaki ekran görüntüsü, bir maliyet grafiği örneği gösterir. 
   
    ![Maliyet grafiği](./media/devtest-lab-configure-cost-management/graph.png)

    **Tahmini maliyet** değeri, geçerli takvim ayının tahmini maliyet-to-date değeridir. **Öngörülen maliyet,** önceki beş günün laboratuvar maliyeti kullanılarak hesaplanan, geçerli takvim ayının tamamı için hesaplanan tahmini maliyettir.

    Maliyet tutarları bir sonraki tam sayıya yuvarlanır. Örnek: 

   * 5.01 6'ya kadar yuvarlar 
   * 5.50'den 6'ya yuvarlar
   * 5.99'dan 6'ya yuvarlar

     Grafiğin üzerinde belirtildiği gibi, grafikte varsayılan olarak gördüğünüz maliyetler, [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) teklif oranlarını kullanarak *tahmini* maliyetlerdir. Ayrıca, [laboratuvarınızın maliyet hedeflerini yöneterek](#managing-cost-targets-for-your-lab) grafiklerde görüntülenen kendi harcama hedeflerinizi de ayarlayabilirsiniz.

     Aşağıdaki maliyetler maliyet hesaplaması dahil *edilmez:*

   * Azure DevTest Labs, CSP veya Dreamspark aboneliklerini desteklemeyen laboratuvar maliyetini hesaplamak için [Azure faturalandırma API'lerini](../cost-management-billing/manage/usage-rate-card-overview.md) kullandığından, CSP ve Dreamspark abonelikleri şu anda desteklenmemektedir.
   * Teklif oranlarınız. Şu anda, Microsoft veya Microsoft iş ortaklarıile anlaşma yaptığınız teklif oranlarını (aboneliğiniz altında gösterilir) kullanamazsınız. Yalnızca Kullandıkça Öde fiyatları kullanılır.
   * Vergileriniz
   * İndirimleriniz
   * Faturalandırma para biriminiz. Şu anda, laboratuvar maliyeti yalnızca USD cinsinden görüntülenir.

### <a name="managing-cost-targets-for-your-lab"></a>Laboratuvarınız için maliyet hedeflerini yönetme
DevTest Labs, aylık Tahmini Maliyet Trendi grafiğinde görüntüleyebileceğiniz bir harcama hedefi belirleyerek laboratuvarınızdaki maliyetleri daha iyi yönetmenize olanak tanır. DevTest Labs, belirtilen hedef harcamalara veya eşiğe ulaşıldığında size bir bildirim de gönderebilir. 

1. Maliyet **eğilim** sayfasında Hedefi **Yönet'i**seçin.

    ![Hedef düğmesini yönetme](./media/devtest-lab-configure-cost-management/cost-trend-manage-target.png)
2. Hedef **Yönet** sayfasında bir harcama hedefi ve eşikleri belirtin. Ayrıca, seçilen her eşiğin maliyet eğilim grafiğinde mi yoksa bir webhook bildirimi aracılığıyla mı bildirildiğini de ayarlayabilirsiniz.

    ![Hedef bölmeyi yönetme](./media/devtest-lab-configure-cost-management/cost-trend-manage-target-pane.png)

   - Maliyet hedeflerinin izlenmesini istediğiniz bir zaman dilimi seçin.
      - **Aylık**: maliyet hedefleri aylık olarak izlenir.
      - **Sabit**: maliyet hedefleri, başlangıç ve bitiş tarihlerinde belirttiğiniz tarih aralığı için izlenir. Genellikle, bu değerler projenizin çalışma zamanlanma süresini gösterir.
   - Hedef **maliyet**belirtin. Örneğin, tanımladığınız zaman diliminde bu laboratuvariçin ne kadar harcamayı planladığınızı.
   - Belirtilen **Hedef maliyetinizin**%125'ine kadar olan %25'lik artışlarla raporlanmasını istediğiniz eşiği etkinleştirmek veya devre dışı bırakmayı seçin.
      - **Bildir**: Bu eşik karşılandığında, belirttiğiniz bir webhook URL'si tarafından bildirilirsiniz.
      - **Grafikte Çizim**: Bu eşik karşılandığında, sonuçlar Aylık Tahmini Maliyet Eğilim grafiğini görüntülemede açıklandığı gibi görüntülediğiniz maliyet eğilim grafiğinde çizilir.
   - Eşik karşılandığında **Bildir'i** seçmeyi seçerseniz, bir webhook URL belirtmeniz gerekir. Maliyet tümleştirmeleri alanında, **tümleştirme eklemek için buraya tıklayın'ı**seçin. Yapılandırma bildirim bölmesine bir **Webhook URL** girin ve ardından **Tamam'ı**seçin.

       ![Bildirim bölmesini yapılandırma](./media/devtest-lab-configure-cost-management/configure-notification.png)

     - **Bildirimde**bulunmayı belirtirseniz, bir webhook URL'si tanımlamanız gerekir.
     - Aynı şekilde, bir webhook URL'si tanımlıyorsanız, Maliyet eşik bölmesinde **Bildirim'i** **Açık** olarak ayarlamanız gerekir.
     - Buraya girmeden önce bir webhook oluşturmanız gerekir.  

       Web hooks hakkında daha fazla bilgi için [bkz.](../azure-functions/functions-create-a-web-hook-or-api-function.md) 

## <a name="view-cost-by-resource"></a>Kaynağa göre maliyeti görüntüleme 
Laboratuarlarda aylık maliyet eğilim özelliği, geçerli takvim ayında ne kadar harcadığınızı görmenizi sağlar. Ayrıca, son yedi gün içinde harcamalarınıza bağlı olarak, ay sonuna kadar harcamaların tahminini de gösterir. Laboratuvardaki harcamaların neden eşikleri erken karşıladığını anlamanıza yardımcı olmak için, bir tabloda **kaynak başına** aylık maliyeti gösteren kaynak özelliğine göre **maliyet** kullanabilirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.
3. Laboratuvarlar listesinden istediğiniz laboratuarı seçin.  
4. Sol menüde **Yapılandırma ve ilkeler'i** seçin.
5. Sol menüdeki Maliyet izleme bölümünde **kaynağa göre** **Maliyet'i** seçin. Bir laboratuvarla ilişkili her kaynakla ilişkili maliyetleri görürsünüz. 

    ![Kaynağa göre maliyet](./media/devtest-lab-configure-cost-management/cost-by-resource.png)

Bu özellik, laboratuvar harcamalarını azaltmak için işlem yapabilmeniz için en pahalı kaynakları kolayca belirlemenize yardımcı olur. Örneğin, bir VM'nin maliyeti VM'nin boyutuna bağlıdır. VM boyutu ne kadar büyükse, maliyet daha fazladır. VM'nin boyutunu ve sahibini kolayca bulabilirsiniz, böylece vm sahibiyle konuşup bu tür VM boyutunun neden gerekli olduğunu ve boyutu düşürme şansı nın olup olmadığını anlayabilirsiniz.

[Otomatik kapatma ilkesi,](devtest-lab-set-lab-policy.md?#set-auto-shutdown-policy) günün belirli bir saatinde laboratuvar VM'lerini kapatarak maliyeti azaltmanıza yardımcı olur. Ancak, bir laboratuvar kullanıcısı VM'yi çalıştırma maliyetini artıran kapatma ilkesini devre dışı layabilir. Otomatik kapatma ilkesinden çıkarılıp devre dışı bırakılmış olup olmadığını görmek için tabloda bir VM seçebilirsiniz. Bu durumda, VM'nin neden poliçeden çıkarıldığını öğrenmek için VM sahibiyle konuşabilirsiniz.
 
## <a name="next-steps"></a>Sonraki adımlar
Bir sonraki denemede deneyecek bazı şeyler şunlardır:

* [Laboratuvar ilkelerini tanımlayın](devtest-lab-set-lab-policy.md) - Laboratuvarınızın ve VM'lerinin nasıl kullanıldığını yönetmek için kullanılan çeşitli ilkeleri nasıl ayarlayınızı öğrenin. 
* [Özel görüntü oluşturma](devtest-lab-create-template.md) - VM oluşturduğunuzda, özel bir görüntü veya Market görüntüsü olabilecek bir taban belirtirsiniz. Bu makalede, bir VHD dosyasından özel bir görüntü oluşturmak için nasıl gösteriş.
* [Market görüntülerini yapılandır -](devtest-lab-configure-marketplace-images.md) DevTest Labs, Azure Marketi görüntülerine dayalı VM oluşturmayı destekler. Bu makalede, bir laboratuvarda VM oluştururken hangi Azure Marketi görüntülerinin kullanılabilir, varsa nasıl kullanılacağı belirtilecektir.
* [Laboratuvarda VM oluşturma](devtest-lab-add-vm.md) - Temel görüntüden (özel veya Pazar Yeri) VM'nin nasıl oluşturulabildiğini ve VM'nizdeki yapılarla nasıl çalışacağınızı gösterir.

