---
title: 'Öğretici: Azure FXT Kenar Filer kümesine düğüm ekleme'
description: Azure FXT Edge Filer depolama önbelleğine düğüm ekleme
author: ekpgh
ms.author: rohogue
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.openlocfilehash: 6251fe8f88b7db25e3c09898540e07754d72fb0d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75551956"
---
# <a name="tutorial-add-cluster-nodes-to-an-azure-fxt-edge-filer-cluster"></a>Öğretici: Azure FXT Edge Filer kümesine küme düğümleri ekleme

Yeni bir Azure FXT Edge Filer kümesi yalnızca bir düğümle oluşturulur. Diğer yapılandırmayı yapmadan önce en az iki düğüm daha eklemeli ve yüksek kullanılabilirlik sağlamalısınız. 

Bu öğretici, küme düğümlerinin nasıl ekleyeceğini ve Yüksek Kullanılabilirlik (HA) özelliğini nasıl etkinleştireceklerini açıklar. 

Bu öğreticide şunları öğreneceksiniz: 

> [!div class="checklist"]
> * FXT kümesine düğüm ekleme
> * HA nasıl etkinleştirilir

Bu öğreticideki adımların tamamlanması yaklaşık 45 dakika sürer.

Bu öğreticiyi başlatmadan önce, eklemek istediğiniz düğümler üzerinde güç ve [ilk parolalarını ayarlayın.](fxt-node-password.md) 

## <a name="1-load-the-cluster-nodes-page"></a>1. Küme Düğümleri sayfasını yükleyin

Kümenin Denetim Panelini bir web tarayıcısında açın ve yönetici olarak oturum açın. (Ayrıntılı [talimatlar, Ayarlar sayfalarını aç](fxt-cluster-create.md#open-the-settings-pages)altında genel bakış makalesinde yer almaktadır.)

Denetim Masası açıldığında **Pano** sekmesini gösterir. 

![Denetim Masası Panosu (ilk sekme)](media/fxt-cluster-config/dashboard-1-node.png)

Bu resim, tek bir düğümle yeni oluşturulan bir kümenin panosunu gösterir.

## <a name="2-locate-the-node-to-add"></a>2. Eklemek için düğümü bulun

Düğüm eklemek için **Ayarlar** sekmesini tıklatın ve **Küme** bölümündeki **FXT Düğümleri** sayfasını seçin.

![Cluster > FXT Düğümleri yüklü denetim paneli Ayarları sekmesi (ikinci sekme)](media/fxt-cluster-config/settings-fxt-nodes.png)

**FXT Düğümleri - Birebir olmayan** lar listesi, tüm atanmamış FXT düğümlerini gösterir (çoğu veri merkezinin yalnızca birkaç ı vardır. Kümeye eklemek istediğiniz FXT düğümlerini bulun.

> [!Tip] 
> **Birleştirilmiş listede** istediğiniz düğümü bulamazsanız, aşağıdaki gereksinimleri karşılayıp karşılamadığını denetleyin:
> 
> * Bu güç ve bir [kök parola kümesi](fxt-node-password.md)oldu .
> * Erişebileceğiniz bir ağa bağlıdır. VLAN'lar kullanıyorsanız, kümeyle aynı VLAN üzerinde olmalıdır.
> * Bonjour protokolü ile tespit edilebilir. 
>
>   Bazı güvenlik duvarı ayarları, Bonjour tarafından kullanılan TCP/UDP bağlantı noktalarını engeller ve bu da FXT işletim sisteminin düğümleri otomatik olarak algılamasını engeller.
> 
> Eklemek istediğiniz düğüm listede yoksa aşağıdaki çözümleri deneyin: 
> 
> * IP adresine göre bulmak için **El Ile Keşfet** düğmesini tıklatın.
> 
> * Geçici IP adreslerini el ile atayın. Bu nadirdir, ancak etiketli VLAN'ları kullanırsanız ve düğümler doğru ağda değilse veya ağınız kendi kendine atanan IP adreslerine izin vermiyorsa gerekebilir. [Statik bir IP adresini el ile ayarlamak](https://azure.github.io/Avere/legacy/create_cluster/4_8/html/static_ip.html)için bu belgenin eski sürümündeki yönergeleri izleyin.

Düğüm adı, IP adresi, yazılım sürümü ve uygunluk durumu listede görüntülenir. Genellikle Durum **sütununda** "Katılmak istiyor" yazan veya düğümü kümeye katılmak için uygun olmayan bir sistem veya donanım sorununu açıklar.

**Eylemler** sütununda düğümün kümeye eklenmesini veya yazılımını güncelleştirmenize izin veren düğmeler vardır. Güncelleştirme düğmesi, kümedeki düğümlerle eşleşen yazılım sürümünü otomatik olarak yükler.

Kümedeki tüm düğümlerin işletim sistemi aynı sürümünü kullanması gerekir, ancak düğüm eklemeden önce yazılımı güncelleştirmeniz gerekmez. Katılmaya İzin **Ver** düğmesini tıklattıktan sonra, küme birleştirme işlemi kümedeki sürümle eşleşen işletim sistemi yazılımını otomatik olarak denetler ve yükler.

Bu sayfadaki seçenekler hakkında daha fazla bilgi edinmek için Cluster Configuration Guide'daki [ **Cluster** > FXT Düğümlerini](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_fxt_nodes.html) okuyun.

## <a name="3-click-the-allow-to-join-button"></a>3. "Katılmak için izin ver" düğmesine tıklayın 

Eklemek istediğiniz düğüm için **Eylemler** sütunundaki **Join To Join*** düğmesini tıklatın.

Düğmeyi tıklattıktan sonra, yazılımı kümeye eklemeye hazırlık olarak güncelleştirildikçe düğümün durumu değişebilir. 

Aşağıdaki resimde kümeye katılma sürecinde olan bir düğüm gösterilmektedir (büyük olasılıkla, eklenmeden önce bir işletim sistemi güncelleştirmesi alıyor). Kümeye eklenme sürecinde olan düğümler için **Eylemler** sütununda düğme görünmüyor.

![düğüm tablosunun bir satırı, düğümün adını, IP adresini, yazılım sürümünü, "Katılmaya izin verilen" iletisini ve boş bir son sütunu gösterir](media/fxt-cluster-config/node-join-in-process.png)

Birkaç dakika sonra, yeni düğüm **FXT Düğümleri** ayarları sayfasının üst kısmında küme düğümleri listesinde görünmelidir. 

Kümenize diğer düğümleri eklemek için bu işlemi yineleyin. Başka bir başlatmadan önce kümeye katılmayı bitirmek için bir düğümün beklemeniz gerekmez.

## <a name="enable-high-availability"></a>Yüksek Kullanılabilirlik Etkinleştirme

Kümenize ikinci bir düğüm ekledikten sonra, denetim panosu panosunda yüksek kullanılabilirlik özelliğinin yapılandırılmadığını belirten bir uyarı iletisi görebilirsiniz. 

Yüksek kullanılabilirlik (HA), küme düğümlerinin aşağı inerlerse birbirlerini dengelemesini sağlar. HA varsayılan olarak etkinleştirildi.

!["Kümede birden fazla düğüm vardır, ancak HA etkin değildir..." iletisi ile pano sekmesi Koşullar tablosunda](media/fxt-cluster-config/no-ha-2-nodes.png)

> [!Note] 
> Kümede en az üç düğüm olana kadar HA'yı etkinleştirme.

HA'yı açmak için bu yordamı izleyin: 

1. **Ayarlar** sekmesinin **Küme** bölümüne **Yüksek Kullanılabilirlik** sayfasını yükleyin.

   ![HA yapılandırma sayfası (Küme > Yüksek Kullanılabilirlik). "Enable HA" onay kutusu en üstte, gönder düğmesi ise en alttadır.](media/fxt-cluster-config/enable-ha.png)

2. **HA'yı etkinleştir** etiketli kutuyu tıklatın ve **Gönder** düğmesini tıklatın. 

HA'nın etkin olduğunu doğrulamak için **Pano'da** bir uyarı görüntülenir.

!["HA artık tamamen yapılandırılmıştır" iletisini gösteren pano tablosu](media/fxt-cluster-config/ha-configured-alert.png)


## <a name="next-steps"></a>Sonraki adımlar

Kümenizdeki tüm düğümleri ekledikten sonra, kümenizin uzun vadeli depolama alanını yapılandırarak kuruluma devam edin.

> [!div class="nextstepaction"]
> [Arka uç depolama ekleme ve sanal ad alanını ayarlama](fxt-add-storage.md)