---
title: StorSimple Snapshot Manager kullanıcı arayüzü | Microsoft Dokümanlar
description: StorSimple Snapshot Manager kullanıcı arabirimini açıklar ve yedekleme işlerini ve yedekleme kataloğunu yönetmek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: twooley
manager: timlt
editor: ''
ms.assetid: c7d91892-2881-41a2-a7a2-908dc3646493
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: twooley
ms.custom: ''
ms.openlocfilehash: 56771d2e62289485017f34c6a9ab26e1d53610ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75933968"
---
# <a name="use-storsimple-snapshot-manager-user-interface-to-manage-backup-jobs-and-backup-catalog"></a>Yedekleme işlerini ve yedekleme kataloğunu yönetmek için StorSimple Snapshot Manager kullanıcı arabirimini kullanın

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager, yedeklemeleri almak ve yönetmek için kullanabileceğiniz sezgisel bir kullanıcı arabirimine sahiptir. Bu öğretici, kullanıcı arabirimine giriş sağlar ve bileşenlerin her birinin nasıl kullanılacağını açıklar. StorSimple Snapshot Manager'ın ayrıntılı açıklaması için [Bkz. StorSimple Snapshot Manager nedir?](storsimple-what-is-snapshot-manager.md)

### <a name="console-description"></a>Konsol açıklaması
Kullanıcı arabirimini görüntülemek için masaüstünüzdeki StorSimple Snapshot Manager simgesini tıklatın. Aşağıdaki resimde gösterildiği gibi konsol penceresi görüntülenir.

![StorSimple Snapshot Manager bölmeleri](./media/storsimple-use-snapshot-manager/HCS_SSM_gui_panes.png)

Konsol penceresi beş ana öğeye sahiptir. Her öğenin tam açıklaması için uygun bağlantıyı tıklatın.

* [Menü çubuğu](#menu-bar) 
* [Araç çubuğu](#tool-bar) 
* [Kapsam bölmesi](#scope-pane) 
* [Sonuç bölmesi](#results-pane) 
* [Eylemler bölmesi](#actions-pane) 

Ayrıca, StorSimple Snapshot Manager [klavye gezintisini ve bir dizi kısayolları](#keyboard-navigation-and-shortcuts)destekler.

### <a name="console-accessibility"></a>Konsol erişilebilirliği
StorSimple Snapshot Manager kullanıcı arabirimi, Windows işletim sistemi ve Microsoft Yönetim Konsolu (MMC) tarafından sağlanan erişilebilirlik özelliklerinin yanı sıra StorSimple Snapshot Manager'a özgü klavye kısayollarını destekler. 

* Windows erişilebilirlik özelliklerinin açıklaması [için Windows için Klavye kısayollarına](https://support.microsoft.com/kb/126449)gidin. 
* MMC erişilebilirlik özelliklerinin açıklaması [için MMC 3.0 için Erişilebilirlik](https://technet.microsoft.com/library/cc766075.aspx)
* StorSimple Snapshot Manager erişilebilirlik özelliklerinin açıklaması için [Klavye gezintisine ve kısayollara](#keyboard-navigation-and-shortcuts)gidin.

## <a name="menu-bar"></a>Menü çubuğu
Konsol penceresinin üst kısmındaki menü [çubuğunda Dosya](#file-menu), [Eylem](#action-menu), [Görünüm](#view-menu), [Sık Kullanılanlar](#favorites-menu), [Pencere](#window-menu)ve [Yardım](#help-menu) menüleri bulunmaktadır.

Menü deki kullanılabilir komutların listesini görmek için menü çubuğundaki herhangi bir öğeyi tıklatın. Aşağıdaki örnekte menü çubuğunda seçilen **Görünüm** menüsü gösterilmektedir.

![Seçili menüyü görüntüleme](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png)

### <a name="file-menu"></a>Dosya menüsü
**Dosya** menüsü standart Microsoft Yönetim Konsolu (MMC) komutlarını içerir.

#### <a name="menu-access"></a>Menü erişimi
**Dosya** menüsünü görüntülemek için menü çubuğunda **Dosya'yı** tıklatın. Aşağıdaki menü görüntülenir.

![StorSimple Snapshot Manager Dosya menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_FileMenu.png) 

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tabloda **Dosya** menüsünde görünen öğeler açıklanmaktadır.

| Menü öğesi | Açıklama |
|:--- |:--- |
| Yeni |StorSimple Snapshot Manager'ı temel alan yeni bir konsol oluşturmak için **Yeni'yi** tıklatın. |
| Open |Varolan bir konsolu açmak için **Aç'ı** tıklatın. |
| Kaydet |Geçerli konsolu kaydetmek için **Kaydet'i** tıklatın. |
| Farklı kaydet |Geçerli konsolun yeni ve yeniden adlandırılmış bir örneğini oluşturmak için **Kaydet'i** tıklatın. Görünümü özelleştirmek ve daha sonra almaya kaydetmek için **Farklı Kaydet** seçeneğini kullanın. Örneğin, belirli sunucuları işaret eden StorSimple Snapshot Manager snap-ins oluşturabilirsiniz. |
| Ekle/Kaldır |Snap-in'leri eklemek veya kaldırmak ve **Kapsam** bölmesinde düğümleri düzenlemek için **Ekle/Kaldır'ı** tıklatın. Daha fazla bilgi için [MMC 3.0'da Snap-in'leri ve Uzantıları Ekle, Kaldır ve Düzenle'ye](https://technet.microsoft.com/library/cc722035.aspx)gidin. |
| Seçenekler |Konsol simgesini değiştirmek, kullanıcı erişim modlarını ve izinlerini belirtmek veya kullanılabilir disk alanını artırmak için konsol dosyalarını silmek için **Seçenekler'i** tıklatın. |
| Dosya yolları listesi |Son açtığınız bir dosyayı yeniden açmak için numaralandırılmış listedeki bir yolu tıklatın. |
| Çık |**Dosya** menüsünü kapatmak için **Çık'ı** tıklatın. |

### <a name="action-menu"></a>Eylem menüsü
Kullanılabilir eylemlerden seçim yapmak için **Eylem** menüsünü kullanın. Kullanabileceğiniz öğeler **Kapsam** bölmesinde veya **Sonuçlar** bölmesinde yaptığınız seçime bağlıdır.

#### <a name="menu-access"></a>Menü erişimi
**Eylem** menüsünü görüntülemek için aşağıdakilerden birini yapın:

* **Kapsam** bölmesinde veya **Sonuçlar** bölmesinde bir öğeyi sağ tıklatın.
* **Kapsam** bölmesi veya **Sonuçlar** bölmesinde bir öğe seçin ve ardından menü çubuğunda **Eylem'i** tıklatın. 

Örneğin, **Kapsam** bölmesinde üst düğümü seçerseniz ve menü çubuğunda **Eylem'i** sağ tıklatın veya tıklatırsanız, aşağıdaki menü görüntülenir.

![StorSimple Snapshot Manager Eylem menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_Action_menu.png)

**Eylemler** bölmesi (konsolun sağında) **Eylem** menüsüyle aynı eylem listesini içerir. Ayrıca, **Eylemler** bölmesi, **Sonuçlar** bölmesinin özel bir görünümünü oluşturmanıza olanak tanıyan **Görünüm** menüsü seçeneklerini içerir.

![Görünüm menüsü açıkken eylemler bölmesi](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tablo, StorSimple Snapshot Manager eylemlerinin alfabetik bir listesini içerir. 

* **Eylem** sütunu düğümler ve sonuçlar üzerinde gerçekleştirebileceğiniz eylemleri listeler. 
* **Gezinti** sütunu, eylemi seçebilmeniz için uygun **Eylem** menüsünün nasıl görüntülenebildiğini açıklar. Bazı eylemler birden çok **Eylem** menüsünde görünür. Bu eylemler için, madde işaretli listeden bir **Gezinti** seçeneği seçin. 
* **Açıklama** sütunu, Eylem menüsünde veya **Eylemler** bölmesindeki her eylemin nasıl kullanılacağını açıklar ve ne yaptığını açıklar.

> [!NOTE]
> **Eylemler** bölmesi ve **Eylem** menüleri, Buradan **Görünüm,** **Yeni Pencere**, **Yenile**, **Dışa Aktarma Listesi**ve **Yardım**gibi ek seçenekler içerir. Bu seçenekler MMC'nin bir parçası olarak kullanılabilir ve StorSimple Snapshot Manager'a özgü değildir. Tablo, bu seçeneklerin açıklamalarını içerir.
> 
> 

| Eylem | Gezinti | Açıklama |
|:--- |:--- |:--- |
| Kimlik doğrulaması |**Aygıtlar** düğüm'üne tıklayın ve **Sonuçlar** bölmesinde bir aygıtı sağ tıklatın. |Aygıt için yapılandırdığınız parolayı girmek için **Kimlik Doğrulaması'yı** tıklatın. |
| Kopyalama |**Yedekleme Kataloğu'nu**genişletin, **Bulut Anlık Görüntülerini**genişletin, tarihli bir yedeklemeyi tıklatın ve ardından **Sonuçlar** bölmesinde bir birim seçin. |Bulut anlık görüntüsünün bir kopyasını oluşturmak ve belirlediğiniz bir konumda depolamak için **Clone'u** tıklatın. |
| Aygıtı Yapılandırma |**Aygıtlar** düğümüne sağ tıklayın. |Windows ana bilgisayara bağlanmak için tek bir aygıtı veya birden çok aygıtı yapılandırmak için **Aygıtı Yapılandır'ı** tıklatın. |
| Yedekleme İlkesi Oluştur |Aşağıdakilerden birini yapın:<ul><li>Sağ tıklatma **Yedekleme İlkeleri.**</li><li>**Birim Grupları**tıklatın veya genişletin ve sonra bir birim grubunu sağ tıklatın.</li><li>**Yedekleme Kataloğu'nu**tıklatın veya genişletin ve ardından bir birim grubunu sağ tıklatın.</li></ul> |Bir birim grubu için zamanlanmış bir yedekleme yapılandırmak için **Yedek İlke Oluştur'u** tıklatın. |
| Birim Grubu Oluştur |Aşağıdakilerden birini yapın:<ul><li>**Birimler** düğüm'üne tıklayın ve ardından **Sonuçlar** bölmesinde bir birimi sağ tıklatın.</li><li>**Birim Grupları** düğümüne sağ tıklayın.</li></ul> |Birim grubuna hacim atamak için **Birim Grubu Oluştur'u** tıklatın. |
| Sil |Bir düğüm veya sonucu tıklatın (Bu öğe birçok **Eylem** menüsünde ve **Eylemler** bölmelerinde görünür.) |Seçtiğiniz düğümü veya sonucu silmek için **Sil'i** tıklatın. Onay iletişim kutusu göründüğünde, silme işlemini onaylayın veya iptal edin. |
| Ayrıntılar |**Aygıtlar** düğüm'üne tıklayın ve **ardından Sonuçlar** bölmesinde bir aygıtı sağ tıklatın. |Aygıtın yapılandırma ayrıntılarını görmek için **Ayrıntılar'ı** tıklatın. |
| Düzenle |**Yedekleme İlkeleri'ni**tıklatın ve ardından **Sonuçlar** bölmesinde bir ilkeyi sağ tıklatın. |Bir birim grubunun yedekleme zamanlamasını değiştirmek için **Düzenley'i** tıklatın. |
| İhracat Listesi |Herhangi bir düğümü veya sonucu tıklatın (Bu öğe tüm **Eylem** menülerinde ve **Eylemler** bölmelerinde görünür.) |Bir listeyi virgülle ayrılmış değer (CSV) dosyasına kaydetmek için **Dışa Aktarma Listesi'ni** tıklatın. Daha sonra bu dosyayı çözümleme için bir elektronik tablo uygulamasına aktarabilirsiniz. |
| Yardım |Herhangi bir düğümü veya sonucu tıklatın. (Bu öğe tüm **Eylem** menülerinde ve **Eylemler** bölmelerinde görünür.) |Ayrı bir tarayıcı penceresinde çevrimiçi Yardım'ı açmak için **Yardım'ı** tıklatın. |
| Buradan Yeni Pencere |Herhangi bir düğümü veya sonucu tıklatın (Bu öğe tüm **Eylem** menülerinde ve **Eylemler** bölmelerinde görünür.) |Yeni bir StorSimple Snapshot Manager penceresini açmak için **Buradan Yeni Pencere'yi** tıklatın. |
| Yenile |Herhangi bir düğümü veya sonucu tıklatın (Bu öğe tüm **Eylem** menülerinde ve **Eylemler** bölmelerinde görünür.) |Şu anda görüntülenen StorSimple Snapshot Manager penceresini güncelleştirmek için **Yenile'yi** tıklatın. |
| Cihazı Yenile |**Aygıtlar** düğüm'üne tıklayın ve **Sonuçlar** bölmesinde bir aygıtı sağ tıklatın. |Bağlı belirli bir aygıtı StorSimple Snapshot Manager ile eşitlemek için **Cihazı Yenile'yi** tıklatın. |
| Cihazları Yenile |**Aygıtlar** düğümüne sağ tıklayın. |Bağlı aygıtlistenizi StorSimple Snapshot Manager ile senkronize etmek için **Cihazları Yenile'yi** tıklatın. |
| Yeniden tazyik hacimleri |**Birimler** düğümüne sağ tıklayın. |**Sonuçlar** bölmesinde görünen birimlerin listesini güncelleştirmek için **Rescan birimlerini** tıklatın. |
| Geri Yükleme |**Yedekleme Kataloğu'nu**genişletin, birim grubunu genişletin, Yerel **Anlık Görüntüler'i** veya Bulut **Anlık Görüntülerini**genişletin ve ardından bir yedeklemeyi sağ tıklatın. |Geçerli birim grubu verilerini seçili yedeklemedeki verilerle değiştirmek için **Geri Yükle'yi** tıklatın. |
| Yedekleme Yi Ele |Aşağıdakilerden birini yapın:<ul><li>**Birim Gruplarını**genişletin ve ardından bir birim grubunu sağ tıklatın.</li><li>**Yedekleme Kataloğu'nu**genişletin ve ardından bir birim grubunu sağ tıklatın.</li></ul> |Yedekleme işini hemen başlatmak için **Yedeklemeyi Al'ı** tıklatın. |
| Geçiş İthalat ı Ekranı |**Kapsam** bölmesindeki üst düğüme (örneklerdeki **StorSimple Snapshot Manager** düğümü) sağ tıklatın. |StorSimple Device Manager hizmet panosundan alınan birim gruplarını ve ilişkili yedeklemeleri göstermek veya gizlemek için **Geçiş İçerimi Ekranı'nı** tıklatın. |

### <a name="view-menu"></a>Görünüm menüsü
**Sonuçlar** bölmesi içeriğinin özel bir görünümünü oluşturmak için **Görünüm** menüsünü kullanın. **Görünüm** menüsü **Sütun Ekle/Kaldır** ve **Özelleştir** seçeneklerini içerir.

#### <a name="menu-access"></a>Menü erişimi
Menü çubuğunda veya **Eylemler** bölmesinde **Görünüm** menüsüne erişebilirsiniz.

![StorSimple Snapshot Manager Görünüm menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png) 

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tabloda **Görünüm** menüsünde görünen öğeler açıklanmaktadır.

| Menü öğesi | Açıklama |
|:--- |:--- |
| Sütun Ekleme/Kaldırma |**Sonuçlar** bölmesine sütun eklemek veya kaldırmak için **Sütun Ekle/Kaldır'ı** tıklatın. |
| Özelleştirme |Öğeleri StorSimple Snapshot Manager konsol penceresinde göstermek veya gizlemek için **Özelleştir'i** tıklatın. |

### <a name="favorites-menu"></a>Sık Kullanılanlar menüsü
Sık kullandığınız sayfa görünümlerini ve görevleri eklemek, kaldırmak ve düzenlemek için **Sık Kullanılanlar** menüsünü kullanın. 

#### <a name="menu-access"></a>Menü erişimi
Menü çubuğundan **Sık Kullanılanlar** menüsüne erişebilirsiniz.

![StorSimple Snapshot Manager Sık Kullanılanlar menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_FavoritesMenu.png)

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tabloda **Sık Kullanılanlar** menüsünde görünen öğeler açıklanmaktadır.

| Menü öğesi | Açıklama |
|:--- |:--- |
| Sık kullanılanlara ekleme |Geçerli görünümü sık kullanılanlar listenize eklemek için **Sık Kullanılanlara Ekle'yi** tıklatın. |
| Sık Kullanılanları Düzenle |Sık Kullanılanlar klasörünüzün içeriğini düzenlemek için **Sık Kullanılanları** Düzenle'yi tıklatın. |

### <a name="window-menu"></a>Pencere menüsü
StorSimple Snapshot Manager konsol pencerelerini eklemek ve yeniden düzenlemek için **Pencere** menüsünü kullanın.

#### <a name="menu-access"></a>Menü erişimi
Menü çubuğundaki **Pencere** menüsüne erişebilirsiniz.

![StorSimple Snapshot Manager Pencere menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_WindowMenu.png)

Menünün altındaki numaralı liste, şu anda açık olan pencereleri gösterir. Pencereyi ön plana çıkarmak için bu listedeki herhangi bir pencereyi tıklatın. 

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tabloda Pencere menüsünde görünen öğeler açıklanmaktadır.

| Menü öğesi | Açıklama |
|:--- |:--- |
| Yeni Pencere |Yeni bir konsol penceresi açmak için **Yeni Pencere'yi** tıklatın (varolan pencereye ek olarak). |
| Cascade |Açık konsol pencerelerini basamaklı bir tarzda görüntülemek için **Basamaklı'yı** tıklatın. |
| Yatay Döşeme |Açık konsol pencerelerini döşeme (veya ızgara) biçiminde görüntülemek için **Yatay Olarak Döşeme'yi** tıklatın. |
| Simgeleri Düzenle |Masaüstünüze açık ve dağılmış birden çok konsol pencereniz varsa, bunları en aza indirin ve ardından ekranınızın alt kısmında yatay bir satırda düzenlemek için **Simgeleri** Düzenle'yi tıklatın. |

### <a name="help-menu"></a>Yardım menüsü
StorSimple Snapshot Manager ve MMC için kullanılabilir çevrimiçi yardımı görüntülemek için **Yardım** menüsünü kullanın. Ayrıca, şu anda sisteminizde yüklü olan MMC ve StorSimple Snapshot Manager yazılım sürümleri hakkındaki bilgileri de görüntüleyebilirsiniz. 

Menü çubuğundaki **Yardım** menüsüne erişebilirsiniz. Ayrıca, **Eylemler** bölmesinden StorSimple Snapshot Manager yardım konularına da erişebilirsiniz.

![StorSimple Snapshot Manager Yardım menüsü](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpMenu.png)

#### <a name="menu-description"></a>Menü açıklaması
Aşağıdaki tabloda Yardım menüsünde görünen öğeler açıklanmaktadır.

| Menü öğesi | Açıklama |
|:--- |:--- |
| StorSimple Snapshot Manager'a Yardım |**StorSimple Snapshot Manager yardımını** ayrı bir pencerede açmak için StorSimple Snapshot Manager'a Yardım'ı tıklatın. |
| Yardım Konuları |MMC çevrimiçi yardımını ayrı bir pencerede açmak için **Yardım Konuları'nı** tıklatın. |
| TechCenter Web Sitesi |Microsoft TechNet Tech Center giriş sayfasını ayrı bir pencerede açmak için **TechCenter Web Sitesini** tıklatın. |
| Microsoft Management Console Hakkında |Sisteminizde Microsoft Yönetim Konsolu'nun hangi sürümünün yüklü olduğunu görmek için **Microsoft Yönetim Konsolu Hakkında'yı** tıklatın. |
| StorSimple Snapshot Manager Hakkında |Sisteminizde snap-in'in hangi sürümünün yüklü olduğunu görmek için **StorSimple Snapshot Manager hakkında'yı** tıklatın. |

## <a name="tool-bar"></a>Araç çubuğu
Menü çubuğunun altında yer alan araç çubuğunda gezinti ve görev simgeleri bulunur. Her simge, belirli bir göreviçin bir kısayoldur.

### <a name="icon-descriptions"></a>Simge açıklamaları
Aşağıdaki tabloda araç çubuğunda görünen simgeler açıklanmaktadır. 

| Simge | Açıklama |
|:--- |:--- |
| ![Sol ok](./media/storsimple-use-snapshot-manager/HCS_SSM_LeftArrow.png) |Önceki sayfaya dönmek için sol ok simgesini tıklatın. |
| ![Sağ ok](./media/storsimple-use-snapshot-manager/HCS_SSM_RightArrow.png) |Bir sonraki sayfaya gitmek için sağ oku tıklatın (ok griyse, eylem kullanılamaz). |
| ![Yukarı simgesi](./media/storsimple-use-snapshot-manager/HCS_SSM_Up.png) |Konsol ağacında **(Kapsam** bölmesi) bir düzeye çıkmak için yukarı simgesini tıklatın. |
| ![Konsol ağacını göster/gizle](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowConsoleTree.png) |**Kapsam** bölmesini göstermek veya gizlemek için konsol ağacı simgesini göster/gizle'yi tıklatın. |
| ![Dışa aktarma listesi](./media/storsimple-use-snapshot-manager/HCS_SSM_ExportListIcon.png) |Bir listeyi belirttiğiniz bir CSV dosyasına aktarmak için dışa aktarma listesi simgesini tıklatın. |
| ![Yardım simgesi](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpIcon.png) |Çevrimiçi Bir MMC yardım konusunu açmak için yardım simgesini tıklatın. |
| ![Eylemler bölmesini göster/gizle](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowAction.png) |**Eylemler** bölmesini göstermek veya gizlemek için **Eylemler** bölmesini göster/gizle simgesini tıklatın. |

## <a name="scope-pane"></a>Kapsam bölmesi
**Kapsam** bölmesi, StorSimple Snapshot Manager Kullanıcı UI'deki en soldaki bölmedir. Konsol (veya düğüm) ağacını içerir ve StorSimple Snapshot Manager için birincil gezinti mekanizmasıdır. 

### <a name="scope-pane-structure"></a>Kapsam bölmesi yapısı
**Kapsam** bölmesi, ağaç yapısında düzenlenmiş bir dizi tıklanabilir nesne (düğüm) içerir. 

![Kapsam bölmesi](./media/storsimple-use-snapshot-manager/HCS_SSM_Scope_pane.png) 

* Düğümü genişletmek veya daraltmak için düğüm adının yanındaki ok simgesini tıklatın.
* Düğümün durumunu veya içeriğini görüntülemek için düğüm adını tıklatın. Bilgiler **Sonuçlar** bölmesinde görünür. 

**Kapsam** bölmesi aşağıdaki düğümleri içerir: 

* [Aygıt düğümü](#devices-node) 
* [Birim düğümü](#volumes-node) 
* [Birim Grupları düğümü](#volume-groups-node) 
* [Yedekleme İlkeleri düğümü](#backup-policies-node) 
* [Yedek Katalog düğümü](#backup-catalog-node) 
* [İş düğümü](#jobs-node) 

### <a name="scope-pane-tasks"></a>Kapsam bölme görevleri
Belirli bir düğüm üzerinde bir eylemi tamamlamak için **Kapsam** bölmesini kullanabilirsiniz. Bir görev seçmek için aşağıdakilerden birini yapın:

* Düğüme sağ tıklayın ve ardından görünen menüden görevi seçin.
* Düğümü tıklatın ve sonra menü çubuğunda **Eylem'i** tıklatın. Görünen menüden görevi seçin.
* Düğümü tıklatın ve sonra **Eylemler** bölmesinde eylemi seçin.

Bir düğüm seçtiğinizde ve görev listesini görmek için bu yöntemlerden herhangi birini kullandığınızda, yalnızca bu düğümüzerinde gerçekleştirilebilecek eylemler gösterilir.

### <a name="devices-node"></a>Aygıt düğümü
**Aygıt** düğümü, StorSimple Anlık Görüntü Yöneticisi'ne bağlı StorSimple aygıtlarını ve StorSimple sanal aygıtlarını temsil eder. Bir aygıtı bağlamak ve yapılandırmak için bu düğümü seçin ve ilişkili birimlerini, birim gruplarını ve varolan yedekleme kopyalarını aktarın. Birden çok aygıt tek bir ana bilgisayara bağlanabilir.

* Düğümü genişletmek için **Aygıtlar'ın**yanındaki ok simgesini tıklatın.
* Kullanılabilir eylemlerden oluşan bir menü görmek için **Aygıtdüğümü** sağ tıklatın veya genişletilmiş görünümde görünen düğümlerden herhangi birini sağ tıklatın.
* Yapılandırılan aygıtların listesini görmek için **Kapsam** bölmesinde **Aygıtlar'ı** tıklatın. Aygıtların listesi, her aygıtla ilgili bilgilerle birlikte **Sonuçlar** bölmesinde görünür.

### <a name="volumes-node"></a>Birim düğümü
**Birim** düğümü, iSCSI aracılığıyla keşfedilenler ve bir aygıt aracılığıyla keşfedilenler de dahil olmak üzere ana bilgisayar tarafından monte edilen birimlere karşılık gelen sürücüleri temsil eder. Kullanılabilir birimlerin listesini görüntülemek ve tek tek birimleri birim gruplarına atamak için bu düğümü kullanın.

* Düğümü genişletmek **için, Birimler'in**yanındaki ok simgesini tıklatın.
* Kullanılabilir eylemlerden oluşan bir menü görmek **için, Birimler** düğümüne sağ tıklayın veya genişletilmiş görünümde görünen düğümlerden herhangi birini sağ tıklatın.
* Birimlerin listesini görmek için **Kapsam** bölmesinde **Birimler'i** tıklatın. Birim listesi, her birim hakkındaki bilgilerle birlikte **Sonuçlar** bölmesinde görünür.

### <a name="volume-groups-node"></a>Birim Grupları düğümü
Birim grupları tutarlılık grupları olarak da bilinir. Her birim grubu, yedekleme işlemleri sırasında uygulama tutarlılığı sağlamaya yardımcı olan uygulamayla ilgili birimler denoluşan bir havuzdur. Bu grupları yapılandırmak ve etkileşimli yedeklemeler almak veya yedekleme zamanlamaları oluşturmak için **Birim Grupları** düğümlerini kullanın. 

* Düğümü genişletmek **için, Birim Grupları'nın**yanındaki ok simgesini tıklatın.
* Kullanılabilir eylemlerden oluşan bir menü görmek **için, Birim Grupları** düğümüne sağ tıklayın veya genişletilmiş görünümde görünen düğümlerden herhangi birini sağ tıklatın.
* Birim gruplarının listesini görmek için **Kapsam** bölmesinde **Birim Grupları'nı** tıklatın. Birim gruplarının listesi, her birim grubu hakkındaki bilgilerle birlikte **Sonuçlar** bölmesinde görünür.

### <a name="backup-policies-node"></a>Yedekleme İlkeleri düğümü
Yedekleme ilkeleri, yerel ve bulut anlık görüntüleri için iş zamanlamalarıdır. Yedeklemenin ne sıklıkta oluşturulduğunu ve yedeklemenin ne kadar süreyle saklanıp tutulması gerektiğini belirtmek için **Yedekleme İlkeleri** düğümlerini kullanın. 

* Düğümü genişletmek için **Yedekleme İlkeleri'nin**yanındaki ok simgesini tıklatın.
* Kullanılabilir eylemlerden oluşan bir menü görmek için, Genişletilmiş Görünüm'de görünen düğümlerden herhangi birini **Sağ tıklatın.**
* Yedekleme ilkelerinin listesini görmek için **Kapsam** bölmesinde **Yedekleme İlkeleri'ni** tıklatın. Yedekleme ilkelerinin listesi, her ilke hakkındaki bilgilerle birlikte **Sonuçlar** bölmesinde görünür.

> [!NOTE]
> En fazla 64 yedekleme yi koruyabilirsiniz.


### <a name="backup-catalog-node"></a>Yedek Katalog düğümü
**Yedekleme Kataloğu** düğümü, Azure StorSimple birimlerinin yerinde ve şirket dışı yedeklemelerinin listelerini içerir. Bu düğüm birim grubuna göre düzenlenir ve her birim grubu kapsayıcısı yerel anlık görüntüler **(Yerel Anlık Görüntü**düğümü) ve bulut anlık görüntüleri **(Bulut Anlık** Görüntü düğümü) için ayrı yapılar içerir. Genişletildiğinde, her birim grubu kapsayıcısı etkileşimli olarak veya yapılandırılmış bir ilke tarafından alınan tüm başarılı yedeklemeleri listeler.

* Düğümü genişletmek için **Yedek Katalog'un**yanındaki ok simgesini tıklatın.
* Kullanılabilir eylemlerden oluşan bir menü görmek için **Yedek Katalog** düğümüne sağ tıklayın veya genişletilmiş görünümde görünen düğümlerden herhangi birini sağ tıklatın.
* Yedek anlık görüntü listesini görmek için **Kapsam** bölmesinde **Yedek Katalog'u** tıklatın. Anlık görüntü listesi, her anlık görüntü yle ilgili bilgilerle birlikte **Sonuçlar** bölmesinde görünür.

### <a name="local-snapshots-node"></a>Yerel Anlık Görüntü düğümü
**Yerel Anlık Görüntü** düğümü, belirli bir birim grubun yerel anlık görüntülerini listeler. Düğüm, **Kapsam** bölmesindeki **Yedek Katalog** düğümünün altında yer alır. Yerel anlık görüntüler, Azure StorSimple aygıtında depolanan birim verilerinin zamanlı olarak kopyalarıdır. Genellikle, bu tür yedekleme oluşturulabilir ve hızlı bir şekilde geri yüklenebilir. Yerel bir yedekleme kopyası gibi yerel bir anlık görüntü kullanabilirsiniz.

* Düğümü genişletmek için Yerel **Anlık Görüntüler'in**yanındaki ok simgesini tıklatın.
* Kullanılabilir eylemlerden oluşan bir menü görmek için, **Yerel Anlık Görüntü** düğümüne sağ tıklayın veya genişletilmiş görünümde görünen düğümlerden herhangi birini sağ tıklatın.
* Yerel anlık görüntülerin listesini görmek için **Kapsam** bölmesinde **Yerel Anlık Görüntüler'i** tıklatın. Anlık görüntü listesi, her anlık görüntü yle ilgili bilgilerle birlikte **Sonuçlar** bölmesinde görünür.

### <a name="cloud-snapshots-node"></a>Bulut Anlık Görüntü düğümü
**Bulut Anlık Görüntü** düğümü, belirli bir birim grubu için bulut anlık görüntülerini listeler. Düğüm, **Kapsam** bölmesindeki **Yedek Katalog** düğümünün altında yer alır. Bulut anlık görüntüleri, bulutta depolanan birim verilerinin zaman içinde nokta kopyalarıdır. Bulut anlık görüntüsü, farklı, site dışı depolama sisteminde çoğaltılan anlık görüntüye eşdeğerdir. Bulut anlık görüntüleri özellikle olağanüstü durum kurtarma senaryolarında yararlıdır.

* Düğümü genişletmek için **Bulut Anlık Görüntü'nün**yanındaki ok simgesini tıklatın.
* Kullanılabilir eylemlerden oluşan bir menü görmek için **Bulut Anlık Görüntü** düğümüne sağ tıklayın veya genişletilmiş görünümde görünen düğümlerden herhangi birini sağ tıklatın.
* Bulut anlık görüntü listesini görmek için **Kapsam** bölmesinde **Bulut Anlık Görüntü'nün** ekini tıklatın. Anlık görüntü listesi, her anlık görüntü yle ilgili bilgilerle birlikte **Sonuçlar** bölmesinde görünür.

### <a name="jobs-node"></a>İş düğümü
**İşler** düğümü zamanlanan, çalışan ve yakın zamanda tamamlanan yedekleme işleri hakkında bilgi içerir. 

* Düğümü genişletmek için **İşler'in**yanındaki ok simgesini tıklatın.
* Kullanılabilir eylemlerden oluşan bir menü görmek **için, İş düğümüne** sağ tıklayın veya genişletilmiş görünümde görünen düğümlerden herhangi birini sağ tıklatın.
* Zamanlanan işlerin listesini görmek için **İşler** düğümini genişletin ve ardından **Zamanlanan'ı**tıklatın. Daha önce yapılandırılmış işlerin listesi ve her iş hakkında bilgi **Sonuçlar** bölmesinde görünür. 
* Son tamamlanan işlerin listesini görmek için **İşler** düğümini genişletin ve ardından **Son 24 Saat'i**tıklatın. Son 24 saat içinde tamamlanan işlerin listesi **Sonuçlar** bölmesinde görünür. **Sonuçlar** bölmesi, tamamlanan her iş hakkında da bilgi içerir.
* Şu anda çalışmakta olan işlerin listesini görmek için **İşler** düğümlerini genişletin ve ardından **Çalıştır'ı**tıklatın. Şu anda çalışan işlerin listesi ve her iş hakkında bilgi **Sonuçlar** bölmesinde görünür.

## <a name="results-pane"></a>Sonuç bölmesi
**Sonuçlar** bölmesi, StorSimple Snapshot Manager Kullanıcı UI'deki orta bölmedir. **Kapsam** bölmesinde seçtiğiniz düğüm için listeler ve ayrıntılı durum bilgileri içerir.

### <a name="example"></a>Örnek
Aşağıdaki örneği görmek için **Kapsam** bölmesinde **Birim Grupları** düğümünü tıklatın. **Sonuçlar** bölmesi, her grup la ilgili ayrıntıları içeren birim gruplarının listesini görüntüler.

![Sonuç bölmesi](./media/storsimple-use-snapshot-manager/HCS_SSM_Results_pane.png) 

**Sonuçlar** bölmesinde gösterilen ayrıntıları yapılandırabilirsiniz: **Kapsam** bölmesinde bir düğümü sağ tıklatın, **Görünüm'ü**tıklatın ve ardından **Sütun Ekle/Kaldır'ı**tıklatın.

## <a name="actions-pane"></a>Eylemler bölmesi
**Eylemler** bölmesi, StorSimple Snapshot Manager Kullanıcı UI'deki sağ bölmedir. **Kapsam** bölmesinde veya **Sonuçlar** bölmesinde seçtiğiniz düğüm, görünüm veya veriler üzerinde gerçekleştirebileceğiniz bir işlem menüsü içerir. **Eylemler** bölmesi, **Kapsam** bölmesi ve **Sonuçlar** bölmesindeki öğeler için kullanılabilen **Eylem** menüleri ile aynı komutları içerir. Her eylemin açıklaması için **Eylem** menüsü bölümündeki tabloya bakın.

### <a name="examples"></a>Örnekler
Aşağıdaki örneği görmek için **Kapsam** bölmesinde **İşler** düğümünü genişletin ve **Zamanlanmış'ı**tıklatın. **Eylemler** bölmesi, **Zamanlanan** düğüm için kullanılabilir eylemleri görüntüler.

![Eylemler bölmesi zamanlanmış işler örneği](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane.png) 

**Kapsam** bölmesinde daha fazla seçenek görmek için **İşler** düğümünü genişletin, **Zamanlanmış'ı**tıklatın ve ardından **Sonuçlar** bölmesinde zamanlanmış bir işi tıklatın. **Eylemler** bölmesi, aşağıdaki örnekte gösterildiği gibi, zamanlanan iş için kullanılabilir eylemleri görüntüler.

![Eylemler bölme iş eylemleri örneği](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

## <a name="keyboard-navigation-and-shortcuts"></a>Klavye gezintisi ve kısayollar
StorSimple Snapshot Manager, Windows işletim sisteminin ve Microsoft Yönetim Konsolu'nun (MMC) erişilebilirlik özelliklerini sağlar. Ayrıca, aşağıdaki bölümlerde açıklandığı gibi StorSimple Snapshot Manager'a özgü bazı klavye gezinti özellikleri ve kısayolları da içerir.

* [Klavye navigasyon tuşları](#keyboard-navigation-keys) 
* [Menü çubuğu kısayol tuşları](#menu-bar-shortcut-keys) 
* [Kapsam bölmesi kısayol tuşları](#scope-pane-shortcut-keys) 

### <a name="keyboard-navigation-keys"></a>Klavye navigasyon tuşları
Aşağıdaki tabloda StorSimple Snapshot Manager kullanıcı arabiriminde gezinmek için kullanabileceğiniz anahtarlar açıklanmaktadır. 

| Navigasyon tuşu | Eylem |
|:--- |:--- |
| Aşağı ok tuşu |Menü veya bölmedeki bir sonraki öğeye dikey olarak taşımak için aşağı ok tuşunu kullanın. |
| Enter |Bir eylemi tamamlamak için Enter tuşuna basın ve ardından bir sonraki adıma geçin. Örneğin, **İleri**, **Tamam**, veya **Oluştur**'u seçmek için Enter tuşuna basabilir ve ardından sihirbazda bir sonraki adıma gidebilirsiniz. |
| Esc |Menüyü kapatmak veya bir sayfayı iptal edip kapatmak için Esc tuşuna basın. |
| F1 |Şu anda etkin olan pencereiçin bir yardım konusunu görüntülemek için F1 tuşuna basın. |
| F5 |Düğümü yenilemek için F5 tuşuna basın. |
| F6 |**Kapsam** bölmesinden **Sonuçlar** bölmesine geçmek için F6 tuşuna basın. |
| F10 |Menü çubuğuna gitmek için F10 tuşuna basın. |
| Sol ok tuşu |Menü çubuğu seçeneğinden önceki seçeneğe yatay olarak taşımak için sol ok tuşunu kullanın. Menü çubuğundaki önceki öğeye geçdiğinizde, önceki öğenin eylem (veya bağlam) menüsü görüntülenir. |
| Sağ ok tuşu |Bir menü çubuğu seçeneğinden diğerine yatay olarak taşımak için sağ ok tuşunu kullanın. Menü çubuğundaki bir sonraki öğeye geçdiğinizde, yeni öğenin eylem (veya bağlam) menüsü görüntülenir. |
| Sekme tuşu |Konsoldaki bir sonraki bölmeye veya bir sayfadaki bir sonraki seçime veya metin kutusuna taşımak için Sekme tuşunu kullanın. |
| Yukarı ok tuşu |Menü veya bölmedeki önceki öğeye dikey olarak taşımak için yukarı ok tuşunu kullanın. |

### <a name="menu-bar-shortcut-keys"></a>Menü çubuğu kısayol tuşları
Aşağıdaki tabloda menü çubuğunun kısayol tuşu birleşimleri açıklanmaktadır. Kısayol tuşlarına bastıktan ve menü açıldıktan sonra menü kısayol tuşlarını (menüdeki altı çizili tuşlar) kullanabilirsiniz. Menü çubuğu hakkında daha fazla bilgi için [Menü çubuğuna](#menu-bar)gidin.

| Kısayol | Sonuç | Menü Kısayol Tuşu | Sonuç |
|:--- |:--- |:--- |:--- |
| ALT+F |**Dosya** menüsünü açar. |N |Yeni bir konsol örneği açar. |
|  |O |İdari **Araçlar** sayfasını açar. | |
|  |S |StorSimple Snapshot Manager konsolu kaydeder. | |
|  |A |Olarak **Kaydet** sayfasını açar. | |
|  |M |**Ekle/Kaldır'ın Tutturma** sayfasını açar. | |
|  |P |**Seçenekler** sayfasını açar. | |
|  |H |Çevrimiçi Yardım'ı açar. | |
| ALT+A |**Eylem** menüsünü açar. |I |Alma ekranı seçeneğini açar ve kapatır. |
|  |W |Yeni bir StorSimple Snapshot Manager konsolu açar. | |
|  |F |StorSimple Snapshot Manager konsolu güncelleştirir. | |
|  |L |**Dışa Aktarma Listesi** sayfasını açar. | |
|  |H |Çevrimiçi Yardım'ı açar. | |
| ALT+V |**Görünüm** menüsünü açar. |A |Sütun **Ekle/Kaldır** sayfasını açar. |
|  |U |Görünümü **Özelleştir** sayfasını açar. | |
| ALT+O |Sık **Kullanılanlar** menüsünü açar. |A |Sık **Kullanılanlara Ekle** sayfasını açar. |
|  |O |Sık **Kullanılanları Düzenle** sayfasını açar. | |
| ALT+W |**Pencere** menüsünü açar. |N |Başka bir StorSimple Snapshot Manager penceresi açar. |
|  |C |Tüm açık konsol pencerelerini basamaklı bir tarzda görüntüler. | |
|  |T |Tüm açık konsol pencerelerini ızgara deseni yle görüntüler. | |
|  |I |Simgeleri ekranınızın altındaki yatay bir satırda düzenler. | |
| ALT+H |**Yardım** menüsünü açar. |H |Çevrimiçi Yardım'ı açar. |
|  |T |Microsoft TechNet Tech Center web sayfasını açar. | |
|  |A |Microsoft **Yönetim Konsolu Hakkında** sayfasını açar. | |

### <a name="scope-pane-shortcut-keys"></a>Kapsam bölmesi kısayol tuşları
Aşağıdaki **tablolar, Kapsam** bölmesindeki her düğüm için kısayol anahtar birleşimlerini gösterir. 

* [Aygıt düğüm kısayol tuşları](#devices-node-shortcut-keys)
* [Birim düğüm kısayol tuşları](#volumes-node-shortcut-keys)
* [Birim Grupları düğüm kısayol tuşları](#volume-groups-node-shortcut-keys)
* [Yedekleme İlkeleri düğüm kısayol tuşları](#backup-policies-node-shortcut-keys)
* [Yedek Katalog düğümü kısayol tuşları](#backup-catalog-node-shortcut-keys)
* [İş düğümü kısayol tuşları](#jobs-node-shortcut-keys)

#### <a name="devices-node-shortcut-keys"></a>Aygıt düğüm kısayol tuşları
| Menü Kısayolu | Sonuç |
|:--- |:--- |
| C |Aygıt **Yapıla** sayfasını açar. |
| D |Aygıt ların ve aygıt ayrıntılarının listesini yeniler. |
| V |**Görünüm** menüsünü açar. |
| W |**Ayrıntılar** düğümüne odaklanmış yeni bir StorSimple Snapshot Manager konsolu açar. |
| F |StorSimple Snapshot Manager konsolu güncelleştirir. |
| L |**Dışa Aktarma Listesi** sayfasını açar. |
| H |Çevrimiçi Yardım'ı açar. |

#### <a name="volumes-node-shortcut-keys"></a>Birim düğüm kısayol tuşları
| Menü Kısayolu | Sonuç |
|:--- |:--- |
| V |Birim listesini güncelleştirir. |
| V (iki kez basın) |**Görünüm** menüsünü açar. |
| W |**Birimler** düğümüne odaklanmış yeni bir StorSimple Snapshot Manager konsolu açar. |
| F |StorSimple Snapshot Manager konsolu güncelleştirir. |
| L |**Dışa Aktarma Listesi** sayfasını açar. |
| H |Çevrimiçi Yardım'ı açar. |

#### <a name="volume-groups-node-shortcut-keys"></a>Birim Grupları düğüm kısayol tuşları
| Menü Kısayolu | Sonuç |
|:--- |:--- |
| G |Birim **Grubu Oluştur** sayfasını açar. |
| V |**Görünüm** menüsünü açar. |
| W |**Birim Grupları** düğümüne odaklanmış yeni bir StorSimple Snapshot Manager konsolu açar. |
| F |StorSimple Snapshot Manager konsolu güncelleştirir. |
| L |**Dışa Aktarma Listesi** sayfasını açar. |
| H |Çevrimiçi Yardım'ı açar. |

#### <a name="backup-policies-node-shortcut-keys"></a>Yedekleme İlkeleri düğüm kısayol tuşları
| Menü Kısayolu | Sonuç |
|:--- |:--- |
| B |**İlke Oluştur** sayfasını açar. |
| V |**Görünüm** menüsünü açar. |
| W |**Birim Grupları** düğümüne odaklanmış yeni bir StorSimple Snapshot Manager konsolu açar. |
| F |StorSimple Snapshot Manager konsolu güncelleştirir. |
| L |**Dışa Aktarma Listesi **sayfasını açar. |
| H |Çevrimiçi Yardım'ı açar. |

#### <a name="backup-catalog-node-shortcut-keys"></a>Yedek Katalog düğümü kısayol tuşları
| Menü Kısayolu | Sonuç |
|:--- |:--- |
| W |**Birim Grupları** düğümüne odaklanmış yeni bir StorSimple Snapshot Manager konsolu açar. |
| F |StorSimple Snapshot Manager konsolu güncelleştirir. |
| H |Çevrimiçi Yardım'ı açar. |

#### <a name="jobs-node-shortcut-keys"></a>İş düğümü kısayol tuşları
| Menü Kısayolu | Sonuç |
|:--- |:--- |
| V |**Görünüm** menüsünü açar. |
| W |**İşler** düğümüne odaklanmış yeni bir StorSimple Snapshot Manager konsolu açar. |
| F |StorSimple Snapshot Manager konsolu güncelleştirir. |
| L |**Dışa Aktarma Listesi** sayfasını açar. |
| H |Çevrimiçi Yardım'ı açar |

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager'ı](storsimple-snapshot-manager-admin.md)nasıl kullanacağınızı öğrenin.
* [Aygıtları bağlamak ve yönetmek için StorSimple Snapshot Manager'ı](storsimple-snapshot-manager-manage-devices.md)nasıl kullanacağınızı öğrenin.

