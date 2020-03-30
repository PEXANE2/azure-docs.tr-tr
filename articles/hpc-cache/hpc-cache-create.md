---
title: Azure HPC Önbelleği Oluşturma
description: Azure HPC Önbellek örneği oluşturma
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 11/11/2019
ms.author: rohogue
ms.openlocfilehash: c6090d19ce530829b79dca69636c2123e2519961
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80129562"
---
# <a name="create-an-azure-hpc-cache"></a>Azure HPC Önbelleği Oluşturma

Önbelleğinizi oluşturmak için Azure portalını kullanın.

![alt kısmında oluştur düğmesi ile Azure portalında önbelleğe genel bakış ekran görüntüsü](media/hpc-cache-home-page.png)

## <a name="define-basic-details"></a>Temel ayrıntıları tanımlama

![Azure portalında proje ayrıntıları sayfasının ekran görüntüsü](media/hpc-cache-create-basics.png)

**Project Details'da**önbelleği barındıracak abonelik ve kaynak grubunu seçin. Aboneliğin [erişim](hpc-cache-prereqs.md#azure-subscription) listesinde olduğundan emin olun.

**Hizmet Ayrıntıları'nda**önbellek adını ve diğer öznitelikleri ayarlayın:

* Konum - Desteklenen [bölgelerden](hpc-cache-overview.md#region-availability)birini seçin.
* Sanal ağ - Varolan bir ağ seçebilir veya yeni bir sanal ağ oluşturabilirsiniz.
* Alt ağ - Yalnızca bu Azure HPC Önbelleği örneği için kullanılacak en az 64 IP adresine (/24) sahip bir alt ağ seçin veya oluşturun.

## <a name="set-cache-capacity"></a>Önbellek kapasitesini ayarlama
<!-- referenced from GUI - update aka.ms link if you change this header text -->

**Önbellek** sayfasında önbelleğinizin kapasitesini ayarlamanız gerekir. Burada ayarlanan değerler, önbelleğinizin ne kadar veri tutabileceğini ve istemci isteklerine ne kadar hızlı hizmet verebileceğini belirler.

Kapasite önbelleğin maliyetini de etkiler.

Bu iki değeri ayarlayarak kapasiteyi seçin:

* ÖNbellek (iş verimi) için GB/saniye'de maksimum veri aktarım hızı
* Önbelleğe alınan veriler için ayrılan depolama miktarı, TB'de

Kullanılabilir iş yapma değerlerinden ve önbellek depolama boyutlarından birini seçin.

Gerçek veri aktarım hızının iş yüküne, ağ hızlarına ve depolama hedeflerinin türüne bağlı olduğunu unutmayın. Seçtiğiniz değerler, tüm önbellek sistemi için maksimum iş kısmını ayarlar, ancak bunların bir kısmı genel ek görev için kullanılır. Örneğin, bir istemci önbellekte zaten depolanmış olmayan bir dosya isterse veya dosya eski olarak işaretlenmişse, önbelleğiniz arka uç depolamaalanından almak için bazı iş bilgisini kullanır.

Azure HPC Önbelleği, önbellek isabet oranlarını en üst düzeye çıkarmak için hangi dosyaların önbelleğe alındığını ve önceden yüklendiğini yönetir. Önbellek içeriği sürekli olarak değerlendirilir ve dosyalara daha az sıklıkta erişildiğinde uzun süreli depolama alanına taşınır. Meta veriler ve diğer ek bilgiler için ek alan içeren etkin çalışma dosyaları kümesini rahatça tutabilen bir önbellek depolama boyutu seçin.

![önbellek boyutlandırma sayfasının ekran görüntüsü](media/hpc-cache-create-capacity.png)

## <a name="add-resource-tags-optional"></a>Kaynak etiketleri ekleme (isteğe bağlı)

**Etiketler** sayfası, Azure HPC Önbellek örneğinize [kaynak etiketleri](https://go.microsoft.com/fwlink/?linkid=873112) eklemenize olanak tanır.

## <a name="finish-creating-the-cache"></a>Önbelleği oluşturmayı bitirme

Yeni önbelleği yapılandırıldıktan sonra **Gözden Geçir + oluştur** sekmesini tıklatın. Portal seçimlerinizi doğrular ve seçimlerinizi gözden geçirmenizi sağlar. Her şey doğruysa, **Oluştur'u**tıklatın.

Önbellek oluşturma yaklaşık 10 dakika sürer. Azure portalının bildirimler panelinde ilerlemeyi izleyebilirsiniz.

![portalda önbellek oluşturma "dağıtım devam ediyor" ve "bildirimler" sayfalarının ekran görüntüsü](media/hpc-cache-deploy-status.png)

Oluşturma sona erdiğinde, yeni Azure HPC Önbelleği örneğine bağlantı yla bir bildirim görüntülenir ve önbellek aboneliğinizin **Kaynaklar** listesinde görünür.
<!-- double check on notification -->

![Azure portalındaki Azure HPC Önbelleği örneğinin ekran görüntüsü](media/hpc-cache-new-overview.png)

## <a name="next-steps"></a>Sonraki adımlar

Önbelleğiniz **Kaynaklar** listesinde göründükten sonra, önbelleğinizin veri kaynaklarına erişimini sağlamak için depolama hedeflerini tanımlayın.

* [Depolama hedefleri ekleme](hpc-cache-add-storage.md)
