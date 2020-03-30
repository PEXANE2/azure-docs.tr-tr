---
title: StorSimple Snapshot Manager ve birimleri | Microsoft Dokümanlar
description: Birimleri görüntülemek ve yönetmek ve yedeklemeleri yapılandırmak için StorSimple Snapshot Manager MMC snap-in'in nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: twooley
manager: carmonm
editor: ''
ms.assetid: 78896323-e57c-431e-bbe2-0cbde1cf43a2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: twooley
ms.openlocfilehash: f09d4dd46a50f1794e51342a939b8919c5c523ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254657"
---
# <a name="use-storsimple-snapshot-manager-to-view-and-manage-volumes"></a>Birimleri görüntülemek ve yönetmek için StorSimple Snapshot Manager'ı kullanın
## <a name="overview"></a>Genel Bakış
Birimleri seçmek ve bunlarla ilgili bilgileri görüntülemek için StorSimple Snapshot Manager **Birimleri** düğümünü **(Kapsam** bölmesindeki) kullanabilirsiniz. Birimler, ana bilgisayar tarafından monte edilen birimlere karşılık gelen sürücüler olarak sunulur. **Birim** düğümü, iSCSI ve aygıtın kullanımı yla keşfedilen birimler de dahil olmak üzere StorSimple tarafından desteklenen yerel birimleri ve birim türlerini gösterir. 

Desteklenen birimler hakkında daha fazla bilgi [için birden çok birim türü için Destek'e](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types)gidin.

![Sonuçlar bölmesinde birim listesi](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

**Ciltler** düğümü, StorSimple Snapshot Manager bunları bulduktan sonra birimleri yeniden aramanızı veya silmenizi de sağlar. 

Bu öğretici, hacimleri nasıl monte edebileceğinizi, başlatabileceğinizi ve biçimlendirebileceğinizi ve storSimple Snapshot Manager'ı aşağıdakiler için nasıl kullanabileceğinizi açıklar:

* Birimler hakkındaki bilgileri görüntüleme 
* Birimleri silme
* Yeniden tazyik hacimleri 
* Temel bir birimi yapılandırma ve yedekleme
* Dinamik bir aynalı ses düzeyini yapılandırın ve yedekle

> [!NOTE]
> **Tüm Birim** düğüm eylemleri **Eylemler** bölmesinde de kullanılabilir.
> 
> 

## <a name="mount-volumes"></a>Montaj hacimleri
StorSimple birimlerini monte etmek, başlatmak ve biçimlendirmek için aşağıdaki yordamı kullanın. Bu yordam, sabit diskleri ve ilgili birimleri veya bölümleri yönetmek için bir sistem yardımcı programı olan Disk Yönetimi'ni kullanır. Disk Yönetimi hakkında daha fazla bilgi için Microsoft TechNet web sitesinden [Disk Yönetimi'ne](https://technet.microsoft.com/library/cc770943.aspx) gidin.

#### <a name="to-mount-volumes"></a>Hacimleri monte etmek için
1. Ana bilgisayarda Microsoft iSCSI başlatıcısını başlatın.
2. Hedef portal veya bulma IP adresi olarak arabirim IP adreslerinden birini tedarik edin ve aygıta bağlanın. Aygıt bağlandıktan sonra, birimler Windows sisteminize erişebilir. Microsoft iSCSI başlatıcısının kullanımı hakkında daha fazla bilgi için, Microsoft iSCSI [Başlatıcısını Yükleme ve Yapılandırma][1]bölümünde "iSCSI hedef aygıtına bağlanma" bölümüne gidin.
3. Disk Yönetimini başlatmak için aşağıdaki seçeneklerden birini kullanın:
   
   * **Run** kutusuna Diskmgmt.msc yazın.
   * Sunucu Yöneticisi'ni başlatın, **Depolama** düğümünü genişletin ve ardından **Disk Yönetimi'ni**seçin.
   * **Yönetim Araçları**başlatın, **Bilgisayar Yönetimi** düğümünü genişletin ve ardından **Disk Yönetimi'ni**seçin. 
     
     > [!NOTE]
     > Disk Yönetimi'ni çalıştırmak için yönetici ayrıcalıklarını kullanmanız gerekir.
     > 
     > 
4. Ses(ler) çevrimiçi alın:
   
   1. Disk Yönetimi'nde, **Çevrimdışı**olarak işaretlenmiş herhangi bir birimde sağ tıklatın.
   2. **Diski Yeniden Etkinleştir'i**tıklatın. Disk yeniden etkinleştirildikten sonra disk **Çevrimiçi** olarak işaretlenmelidir.
5. Birim(ler) başlatma:
   
   1. Keşfedilen birimleri sağ tıklatın.
   2. **Menüde, Diski Başlatma'yı**seçin.
   3. **Diski Başlatma** iletişim kutusunda, başlatılmasını istediğiniz diskleri seçin ve ardından **Tamam'ı**tıklatın.
6. Basit birimleri biçimlendir:
   
   1. Biçimlendirmek istediğiniz bir birimi sağ tıklatın.
   2. Menüde Yeni **Basit Birim'i**seçin.
   3. Birimi biçimlendirmek için Yeni Basit Birim sihirbazını kullanın:
      
      * Birim boyutunu belirtin.
      * Bir sürücü mektubu tedarik edin.
      * NTFS dosya sistemini seçin.
      * 64 KB ayırma birimi boyutu belirtin.
      * Hızlı biçimlendirme gerçekleştirin.
7. Çok bölümlü birimleri biçimlendirin. Talimatlar için, [Disk Yönetimi'nin uygulanmasında](https://msdn.microsoft.com/library/dd163556.aspx)"Bölümler ve Birimler" bölümüne gidin.

## <a name="view-information-about-your-volumes"></a>Birimlerinizle ilgili bilgileri görüntüleme
Yerel ve Azure StorSimple birimleri hakkındaki bilgileri görüntülemek için aşağıdaki yordamı kullanın.

#### <a name="to-view-volume-information"></a>Birim bilgilerini görüntülemek için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın. 
2. **Kapsam** bölmesinde **Birimler** düğümünü tıklatın. **Sonuçlar** bölmesinde, tüm Azure StorSimple birimleri de dahil olmak üzere yerel ve monte edilmiş birimlerin listesi görünür. **Sonuçlar** bölmesindeki sütunlar yapılandırılabilir. (Birimler düğümüne **Volumes** sağ tıklayın, **Görünüm'ü**seçin ve **ardından Sütun Ekle/Kaldır'ı**seçin .)
   
    ![Sütunları yapılandırma](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)
   
   | Sonuçlar sütunu | Açıklama |
   |:--- |:--- |
   |  Adı |**Ad** sütunu, keşfedilen her birimiçin atanan sürücü harfini içerir. |
   |  Cihaz |**Aygıt** sütunu, ana bilgisayara bağlı aygıtın IP adresini içerir. |
   |  Cihaz Ses Adı |**Aygıt Birim Adı** sütunu, seçili birimin ait olduğu aygıt biriminin adını içerir. Bu, belirli bir birim için Azure portalında tanımlanan birim adıdır. |
   |  Erişim Yolları |**Erişim Yolları** sütunu ses birimine erişim yolunu görüntüler. Bu, ses in ana bilgisayarda erişilebildiği sürücü harfi veya montaj noktasıdır. |

## <a name="delete-a-volume"></a>Bir birim silme
StorSimple Snapshot Manager'dan bir birimi silmek için aşağıdaki yordamı kullanın.

> [!NOTE]
> Herhangi bir birim grubunun parçasıysa, bir birimi silemezsiniz. (Silme seçeneği, bir birim grubunun üyesi olan birimler için kullanılamaz.) Hacmi silmek için tüm ses grubunu silmeniz gerekir.

#### <a name="to-delete-a-volume"></a>Bir birimi silmek için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** bölmesinde **Birimler** düğümünü tıklatın. 
3. **Sonuçlar** bölmesinde, silmek istediğiniz birime sağ tıklayın.
4. Menüde **Sil'i**tıklatın. 
   
    ![Bir birim silme](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 
5. **Sesi Sil** iletişim kutusu görüntülenir. Metin kutusuna **Onayla** yazın ve ardından **Tamam'ı**tıklatın.
6. Varsayılan olarak, StorSimple Snapshot Manager silmeden önce bir birim yedekler. Bu önlem, silme kasıtsız ise veri kaybına karşı sizi koruyabilir. StorSimple Snapshot Manager, sesi yedeklerken **Otomatik Anlık Görüntü** iletisi görüntüler. 
   
    ![Otomatik anlık görüntü iletisi](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Yeniden tazyik hacimleri
StorSimple Snapshot Manager'a bağlı birimleri yeniden tarayabilmek için aşağıdaki yordamı kullanın.

#### <a name="to-rescan-the-volumes"></a>Birimleri yeniden tsketmek için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** bölmesinde, **Birimler'i**sağ tıklatın ve ardından **Rescan birimlerini**tıklatın.
   
    ![Yeniden tazyik hacimleri](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
   
    Bu yordam, cilt listesini StorSimple Snapshot Manager ile eşitler. Yeni birimler veya silinen birimler gibi tüm değişiklikler sonuçlara yansıtılır.

## <a name="configure-and-back-up-a-basic-volume"></a>Temel bir birimi yapılandırma ve yedekleme
Temel birimin yedeğini yapılandırmak için aşağıdaki yordamı kullanın ve ardından hemen bir yedekleme başlatın veya zamanlanmış yedeklemeler için bir ilke oluşturun.

### <a name="prerequisites"></a>Ön koşullar
Başlamadan önce:

* StorSimple aygıtının ve ana bilgisayar bilgisayarının doğru şekilde yapılandırıldığından emin olun. Daha fazla bilgi için [şirket içi StorSimple aygıtınızı dağıtın.](storsimple-deployment-walkthrough-u2.md)
* StorSimple Snapshot Manager'ı yükleyin ve yapılandırır. Daha fazla bilgi [için, StorSimple Snapshot Manager'ı dağıt'a](storsimple-snapshot-manager-deployment.md)gidin.

#### <a name="to-configure-backup-of-a-basic-volume"></a>Temel bir birimin yedeklemesini yapılandırmak için
1. StorSimple aygıtında temel bir birim oluşturun.
2. Montaj, baş harfe dönüştürme ve ses düzeyini [Montaj ses birimlerinde](#mount-volumes)açıklandığı şekilde biçimlendirin. 
3. Masaüstünüzdeki StorSimple Snapshot Manager simgesine tıklayın. StorSimple Snapshot Manager penceresi görüntülenir. 
4. **Kapsam** bölmesinde, **Birimler** düğümüne sağ tıklayın ve ardından **Rescan birimlerini**seçin. Tarama tamamlandığında, **Sonuçlar** bölmesinde bir birim listesi görünmelidir. 
5. **Sonuçlar** bölmesinde, birime sağ tıklayın ve ardından **Birim Grubu Oluştur'u**seçin. 
   
    ![Birim grubu oluşturma](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 
6. Birim **Grubu Oluştur** iletişim kutusunda, birim grubu için bir ad yazın, ona hacimatayın ve ardından **Tamam'ı**tıklatın.
7. **Kapsam** bölmesinde, Birim **Grupları** düğümünü genişletin. Yeni birim **grubu, Birim Grupları** düğümünün altında görünmelidir. 
8. Birim grup adını sağ tıklatın.
   
   * Etkileşimli (isteğe bağlı) yedekleme işi başlatmak için **Yedeklemeyi Al'ı**tıklatın. 
   * Otomatik yedekleme zamanlamak için **Yedekleme İlkesi Oluştur'u**tıklatın. **Genel** sayfada, listeden bir birim grubu seçin. **Zamanlama** sayfasında zamanlama ayrıntılarını girin. İşiniz bittiğinde **Tamam**'a tıklayın. 
9. Yedekleme işinin başladığını doğrulamak için **Kapsam** bölmesindeki **İşler** düğümünü genişletin ve ardından **Çalışan** düğümünü tıklatın. Şu anda çalışan işlerin listesi **Sonuçlar** bölmesinde görünür. 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Dinamik bir yansıtılmış ses düzeyini yapılandırma ve yedekleme
Dinamik bir yansıtılmış birimin yedekleme yapılandırmak için aşağıdaki adımları tamamlayın:

* Adım 1: Dinamik bir yansıtılmış birim oluşturmak için Disk Yönetimi'ni kullanın. 
* Adım 2: Yedeklemeyapılandırmak için StorSimple Snapshot Manager'ı kullanın.

### <a name="prerequisites"></a>Ön koşullar
Başlamadan önce:

* StorSimple aygıtının ve ana bilgisayar bilgisayarının doğru şekilde yapılandırıldığından emin olun. Daha fazla bilgi için [şirket içi StorSimple aygıtınızı dağıtın.](storsimple-8000-deployment-walkthrough-u2.md)
* StorSimple Snapshot Manager'ı yükleyin ve yapılandırır. Daha fazla bilgi [için, StorSimple Snapshot Manager'ı dağıt'a](storsimple-snapshot-manager-deployment.md)gidin.
* StorSimple aygıtında iki birim yapılandırın. (Örneklerde, kullanılabilir birimler **Disk 1** ve Disk 2'dir.) **Disk 2** 

### <a name="step-1-use-disk-management-to-create-a-dynamic-mirrored-volume"></a>Adım 1: Dinamik bir aynalı birim oluşturmak için Disk Yönetimini kullanın
Disk Yönetimi, sabit diskleri ve içerdikleri birimleri veya bölümleri yönetmek için bir sistem yardımcı programıdır. Disk Yönetimi hakkında daha fazla bilgi için Microsoft TechNet web sitesinden [Disk Yönetimi'ne](https://technet.microsoft.com/library/cc770943.aspx) gidin.

#### <a name="to-create-a-dynamic-mirrored-volume"></a>Dinamik bir aynalı hacim oluşturmak için
1. Disk Yönetimini başlatmak için aşağıdaki seçeneklerden birini kullanın: 
   
   * **Çalıştır** kutusunu açın, **Diskmgmt.msc**yazın ve Enter tuşuna basın.
   * Sunucu Yöneticisi'ni başlatın, **Depolama** düğümünü genişletin ve ardından **Disk Yönetimi'ni**seçin. 
   * **Yönetim Araçları**başlatın, **Bilgisayar Yönetimi** düğümünü genişletin ve ardından **Disk Yönetimi'ni**seçin. 
2. StorSimple aygıtında iki birim olduğundan emin olun. (Örnekte, kullanılabilir birimler **Disk 1** ve Disk 2'dir.) **Disk 2** 
3. Disk Yönetimi penceresinde, alt bölmenin sağ sütununda Disk **1'i** sağ tıklatın ve **Yeni Yansıtılmış Birim'i**seçin. 
   
    ![Yeni Aynalı Hacim](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 
4. Yeni **Yansıtılan Birim** sihirbazı sayfasında **İleri'yi**tıklatın.
5. **Diskleri Seç** sayfasında, **Seçili** bölmede **Disk 2'yi** seçin, **Ekle'yi**tıklatın ve sonra **İleri'yi**tıklatın. 
6. Sürücü **Mektubu veya Yol Ata,** varsayılanları kabul edin ve sonra **İleri'yi**tıklatın. 
7. Birim Boyutunu **Biçimlendir** sayfasında, **Ayırma Birimi Boyutu** kutusunda **64K'yı**seçin. Hızlı **biçim** onay kutusunu yap'ı seçin ve sonra **İleri'yi**tıklatın. 
8. Yeni **Yansıtılan Birim'i Tamamlama** sayfasında ayarlarınızı gözden geçirin ve **ardından Bitir'i**tıklatın. 
9. Bir ileti, temel diskin dinamik bir diske dönüştürüleceğini gösterir. **Evet'i**tıklatın.
   
    ![Dinamik disk dönüştürme iletisi](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 
10. Disk Yönetimi'nde, Disk 1 ve Disk 2'nin dinamik yansıtılmış birimler olarak gösterildiğini doğrulayın. (**Dinamik** durum sütununda görünmelidir ve kapasite çubuğu rengi yansıtılmış bir birim gösteren kırmızıya değiştirilmelidir.) 
    
    ![Disk Yönetimi aynalı dinamik diskler](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 

### <a name="step-2-use-storsimple-snapshot-manager-to-configure-backup"></a>Adım 2: Yedeklemeyapılandırmak için StorSimple Snapshot Manager'ı kullanın
Dinamik bir yansıtılmış birim yapılandırmak için aşağıdaki yordamı kullanın ve sonra hemen bir yedekleme başlatmak veya zamanlanmış yedeklemeler için bir ilke oluşturun.

#### <a name="to-configure-backup-of-a-dynamic-mirrored-volume"></a>Dinamik aynalı birimin yedeklemesini yapılandırmak için
1. Masaüstünüzdeki StorSimple Snapshot Manager simgesine tıklayın. StorSimple Snapshot Manager penceresi görüntülenir. 
2. **Kapsam** bölmesinde, **Birimler** düğümüne sağ tıklayın ve **Rescan birimlerini**seçin. Tarama tamamlandığında, **Sonuçlar** bölmesinde bir birim listesi görünmelidir. Dinamik aynalı birim tek bir birim olarak listelenir. 
3. **Sonuçlar** bölmesinde, dinamik aynalı birim sağ tıklatın ve sonra **Birim Grubu Oluştur'u**tıklatın. 
4. Birim **Grubu Oluştur** iletişim kutusunda, birim grubu için bir ad yazın, dinamik aynalı birimi bu gruba atayın ve ardından **Tamam'ı**tıklatın. 
5. **Kapsam** bölmesinde, Birim **Grupları** düğümünü genişletin. Yeni birim **grubu, Birim Grupları** düğümünün altında görünmelidir. 
6. Birim grup adını sağ tıklatın. 
   
   * Etkileşimli (isteğe bağlı) yedekleme işi başlatmak için **Yedeklemeyi Al'ı**tıklatın. 
   * Otomatik yedekleme zamanlamak için **Yedekleme İlkesi Oluştur'u**tıklatın. **Genel** sayfada, listeden birim grubunu seçin. **Zamanlama** sayfasında zamanlama ayrıntılarını girin. İşiniz bittiğinde **Tamam**'a tıklayın. 
7. Yedekleme işini çalışırken izleyebilirsiniz. **Kapsam** bölmesinde, **İşler** düğümünü genişletin ve ardından **Çalıştır'ı**tıklatın, İş ayrıntıları **Sonuçlar** bölmesinde görünür. Yedekleme işi tamamlandığında, ayrıntılar **Son 24** saat iş listesine aktarılır. 

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager'ı](storsimple-snapshot-manager-admin.md)nasıl kullanacağınızı öğrenin.
* [Birim grupları oluşturmak ve yönetmek için StorSimple Snapshot Manager'ı](storsimple-snapshot-manager-manage-volume-groups.md)nasıl kullanacağınızı öğrenin.

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
