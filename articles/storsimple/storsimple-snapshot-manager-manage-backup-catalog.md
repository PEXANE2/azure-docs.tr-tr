---
title: StorSimple Snapshot Manager Yedekleme kataloğu | Microsoft Docs
description: Yedekleme kataloğunu görüntülemek ve yönetmek için StorSimple Snapshot Manager MMC ek bileşeninin nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: twooley
manager: timlt
editor: ''
ms.assetid: 6abdbfd2-22ce-45a5-aa15-38fae4c8f4ec
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: twooley
ms.openlocfilehash: 94dccdabfa4cf83077bca3629dee8a796d27182a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85515470"
---
# <a name="use-storsimple-snapshot-manager-to-manage-the-backup-catalog"></a>Yedekleme kataloğunu yönetmek için StorSimple Snapshot Manager kullanma

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager 'ın birincil işlevi, anlık görüntü biçiminde StorSimple birimlerinin uygulamayla tutarlı yedek kopyalarını oluşturmanızı sağlar. Anlık görüntüler daha sonra *Yedekleme kataloğu*ADLı bir XML dosyasında listelenir. Yedekleme kataloğu, anlık görüntüleri birim grubuna ve sonra yerel anlık görüntü veya bulut anlık görüntüsüne göre düzenler.

Bu öğreticide, **Yedekleme kataloğu** düğümünü kullanarak aşağıdaki görevleri nasıl tamamlayabileceğinizi açıklanmaktadır:

* Bir birimi geri yükleme
* Birim veya birim grubunu kopyalama
* Bir yedeklemeyi silme
* Dosyayı kurtarma
* StorSimple Snapshot Manager veritabanını geri yükleme

**Kapsam** bölmesindeki **Yedekleme kataloğu** düğümünü genişleterek ve ardından birim grubunu genişleterek yedekleme kataloğunu görüntüleyebilirsiniz.

* Birim grubu adına tıklarsanız, **sonuçlar** bölmesinde birim grubu için kullanılabilen yerel anlık görüntü ve bulut anlık görüntülerinin sayısı gösterilir. 
* **Yerel anlık görüntü** veya **bulut anlık görüntüsü**' ne tıklarsanız, **sonuçlar** bölmesi her bir yedekleme anlık görüntüsü ( **Görünüm** ayarlarınıza bağlı olarak) hakkında aşağıdaki bilgileri gösterir:
  
  * **Ad** : anlık görüntünün alındığı zaman.
  * **Tür** : bunun yerel bir anlık görüntü veya bir bulut anlık görüntüsü olup olmadığı.
  * **Sahip** – içerik sahibi. 
  * **Kullanılabilir** : anlık görüntünün Şu anda kullanılabilir olup olmadığı. **Doğru** , anlık görüntünün kullanılabildiğini ve geri yüklenebileceğini belirtir; **False** , anlık görüntünün artık kullanılabilir olmadığını gösterir. 
  * **Içeri aktarıldı** : yedeklemenin içeri aktarıldığına bakılmaksızın. **Doğru** , cihazın storsimple Snapshot Manager içinde yapılandırma sırasında storsimple Aygıt Yöneticisi hizmetinden içeri aktarıldığını belirtir; **False** , içe aktarılmadığını ancak storsimple Snapshot Manager tarafından oluşturulduğunu gösterir. (Birim grubunun içeri aktarıldığı cihazı tanımlayan bir sonek eklendiğinden, içeri aktarılan bir birim grubunu kolayca belirleyebilirsiniz.)
    
    ![Yedekleme kataloğu](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)
* **Yerel anlık görüntü** veya **bulut anlık**görüntüsünü genişlettikten sonra tek bir anlık görüntü adına tıkladığınızda, **sonuçlar** bölmesinde seçtiğiniz anlık görüntüyle ilgili aşağıdaki bilgiler görüntülenir:
  
  * **Ad** : sürücü harfi tarafından tanımlanan birim. 
  * **Yerel ad** – sürücünün yerel adı (varsa). 
  * **Device** : birimin bulunduğu cihazın adı. 
  * **Kullanılabilir** : anlık görüntünün Şu anda kullanılabilir olup olmadığı. **Doğru** , anlık görüntünün kullanılabildiğini ve geri yüklenebileceğini belirtir; **False** , anlık görüntünün artık kullanılabilir olmadığını gösterir. 

## <a name="restore-a-volume"></a>Bir birimi geri yükleme
Bir birimi yedekten geri yüklemek için aşağıdaki yordamı kullanın.

#### <a name="prerequisites"></a>Ön koşullar
Daha önce yapmadıysanız, bir birim ve birim grubu oluşturun ve ardından birimi silin. Varsayılan olarak, StorSimple Snapshot Manager, silinmesine izin vermeden önce bir birimi yedekler. Bu önlem, birim istenmeden silinirse veya verilerin herhangi bir nedenle kurtarılması gerekiyorsa veri kaybını engelleyebilir. 

StorSimple Snapshot Manager, önlem amaçlı yedeklemesini oluştururken aşağıdaki iletiyi görüntüler.

![Otomatik anlık görüntü iletisi](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

> [!IMPORTANT]
> Birim grubunun parçası olan bir birimi silemezsiniz. Silme seçeneği kullanılamaz. <br>
> 
> 

#### <a name="to-restore-a-volume"></a>Bir birimi geri yüklemek için
1. StorSimple Snapshot Manager başlatmak için masaüstü simgesine tıklayın. 
2. **Kapsam** bölmesinde, **Yedekleme kataloğu** düğümünü genişletin, bir birim grubunu genişletin ve ardından **Yerel anlık görüntüler** veya **bulut anlık görüntüleri**' ne tıklayın. Yedekleme anlık görüntülerinin listesi, **sonuçlar** bölmesinde görüntülenir.
3. Geri yüklemek istediğiniz yedeği bulun, sağ tıklayın ve ardından **geri yükle**' ye tıklayın.
   
    ![Yedekleme kataloğunu geri yükleme](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 
4. Onay sayfasında, ayrıntıları gözden geçirin, **Onayla**yazın ve ardından **Tamam**' a tıklayın. StorSimple Snapshot Manager, birimi geri yüklemek için yedeklemeyi kullanır.
   
    ![Geri yükleme onayı iletisi](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 
5. Geri yükleme eylemini çalışırken izleyebilirsiniz. **Kapsam** bölmesinde, **işler** düğümünü genişletin ve ardından **çalışıyor**' a tıklayın. İş ayrıntıları **sonuçlar** bölmesinde görünür. Geri yükleme işi tamamlandığında, iş ayrıntıları **son 24 saat** listesine aktarılır.

## <a name="clone-a-volume-or-volume-group"></a>Birim veya birim grubunu kopyalama
Birim veya birim grubunun bir çoğaltmasını (klonlamak) oluşturmak için aşağıdaki yordamı kullanın.

#### <a name="to-clone-a-volume-or-volume-group"></a>Birim veya birim grubunu kopyalamak için
1. StorSimple Snapshot Manager başlatmak için masaüstü simgesine tıklayın.
2. **Kapsam** bölmesinde, **Yedekleme kataloğu** düğümünü genişletin, bir birim grubunu genişletin ve ardından **bulut anlık görüntüleri**' ne tıklayın. Yedeklemeler listesi **sonuçlar** bölmesinde görünür.
3. Kopyalamak istediğiniz birim veya birim grubunu bulun, birim veya birim grubu adına sağ tıklayın ve **Kopyala**' ya tıklayın. **Bulut anlık görüntüsünü Kopyala** iletişim kutusu görüntülenir.
   
    ![Bulut anlık görüntüsünü kopyalama](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. **Bulut anlık görüntüsünü Kopyala** iletişim kutusunu aşağıda gösterildiği gibi doldurun: 
   
   1. **Ad** metin kutusuna kopyalanmış birim için bir ad yazın. Bu ad **birimler** düğümünde görünür. 
   2. (İsteğe bağlı) **sürücü**' yi seçin ve ardından açılır listeden bir sürücü harfi seçin.
   3. (İsteğe bağlı) **klasör (NTFS)** öğesini seçin ve bir klasör yolu yazın veya bul ' a tıklayın ve klasör için bir konum seçin. 
   4. **Oluştur**'a tıklayın.
5. Kopyalama işlemi tamamlandığında klonlanan birimi başlatmalısınız. Sunucu Yöneticisi başlatın ve ardından disk yönetimi 'ni başlatın. Ayrıntılı yönergeler için bkz. [bağlama birimleri](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Bu işlem başlatıldıktan sonra birim, **kapsam** bölmesindeki **birimler** düğümünün altında listelenir. Listelenen birimi görmüyorsanız, birim listesini yenileyin ( **birimler** düğümüne sağ tıklayın ve ardından **Yenile**' ye tıklayın).

## <a name="delete-a-backup"></a>Bir yedeklemeyi silme
Yedekleme kataloğundan bir anlık görüntüyü silmek için aşağıdaki yordamı kullanın. 

> [!NOTE]
> Anlık görüntü silindiğinde, anlık görüntüyle ilişkili yedeklenen veriler silinir. Ancak, buluttan verileri temizleme işlemi biraz zaman alabilir.<br>


#### <a name="to-delete-a-backup"></a>Bir yedeklemeyi silmek için
1. StorSimple Snapshot Manager başlatmak için masaüstü simgesine tıklayın.
2. **Kapsam** bölmesinde, **Yedekleme kataloğu** düğümünü genişletin, bir birim grubunu genişletin ve ardından **Yerel anlık görüntüler** veya **bulut anlık görüntüleri**' ne tıklayın. **Sonuçlar** bölmesinde anlık görüntülerin bir listesi görüntülenir.
3. Silmek istediğiniz anlık görüntüye sağ tıklayın ve ardından **Sil**' e tıklayın.
4. Onay iletisi göründüğünde **Tamam**' a tıklayın.

## <a name="recover-a-file"></a>Dosyayı kurtarma
Bir dosya yanlışlıkla bir birimden silinirse, silme işlemini önceden izleyen bir anlık görüntüyü alarak, birimin bir kopyasını oluşturmak için anlık görüntüyü kullanarak ve sonra dosyayı klonlanmış birimden orijinal birime kopyalayarak dosyayı kurtarabilirsiniz.

#### <a name="prerequisites"></a>Ön koşullar
Başlamadan önce, birim grubunun güncel yedeğine sahip olduğunuzdan emin olun. Daha sonra, o birim grubundaki birimlerden birinde depolanan bir dosyayı silin. Son olarak, Silinen dosyayı yedeklemeinizden geri yüklemek için aşağıdaki adımları kullanın. 

#### <a name="to-recover-a-deleted-file"></a>Silinen bir dosyayı kurtarmak için
1. Masaüstünüzdeki StorSimple Snapshot Manager simgesine tıklayın. StorSimple Snapshot Manager Konsolu penceresi görüntülenir. 
2. **Kapsam** bölmesinde, **Yedekleme kataloğu** düğümünü genişletin ve Silinen dosyayı içeren bir anlık görüntüye gidin. Genellikle, silme işleminden hemen önce oluşturulmuş bir anlık görüntü seçmelisiniz.
3. Kopyalamak istediğiniz birimi bulun, sağ tıklayın ve **Kopyala**' ya tıklayın. **Bulut anlık görüntüsünü Kopyala** iletişim kutusu görüntülenir.
   
    ![Bulut anlık görüntüsünü kopyalama](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. **Bulut anlık görüntüsünü Kopyala** iletişim kutusunu aşağıda gösterildiği gibi doldurun: 
   
   1. **Ad** metin kutusuna kopyalanmış birim için bir ad yazın. Bu ad **birimler** düğümünde görünür. 
   2. Seçim **Sürücü**' yi seçin ve ardından açılır listeden bir sürücü harfi seçin. 
   3. Seçim **Klasör (NTFS)** öğesini seçin ve bir klasör yolu yazın ya da **görüntüle** ' ye tıklayın ve klasör için bir konum seçin. 
   4. **Oluştur**'a tıklayın. 
5. Kopyalama işlemi tamamlandığında klonlanan birimi başlatmalısınız. Sunucu Yöneticisi başlatın ve ardından disk yönetimi 'ni başlatın. Ayrıntılı yönergeler için bkz. [bağlama birimleri](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Bu işlem başlatıldıktan sonra birim, **kapsam** bölmesindeki **birimler** düğümünün altında listelenir. 
   
    Listelenen birimi görmüyorsanız, birim listesini yenileyin ( **birimler** düğümüne sağ tıklayın ve ardından **Yenile**' ye tıklayın).
6. Kopyalanmış birimi içeren NTFS klasörünü açın, **birimler** düğümünü genişletin ve kopyalanan birimi açın. Kurtarmak istediğiniz dosyayı bulun ve birincil birime kopyalayın.
7. Dosyayı geri yükledikten sonra klonlanan birimi içeren NTFS klasörünü silebilirsiniz.

## <a name="restore-the-storsimple-snapshot-manager-database"></a>StorSimple Snapshot Manager veritabanını geri yükleme
Ana bilgisayarda StorSimple Snapshot Manager veritabanını düzenli olarak yedeklemeniz gerekir. Bir olağanüstü durum oluşursa veya ana bilgisayar herhangi bir nedenle başarısız olursa, bunu yedekten geri yükleyebilirsiniz. Veritabanı yedeklemesini oluşturmak manuel bir işlemdir.

#### <a name="to-back-up-and-restore-the-database"></a>Veritabanını yedeklemek ve geri yüklemek için
1. Microsoft StorSimple Yönetim hizmetini durdurun:
   
   1. Sunucu Yöneticisi'ni başlatın.
   2. Sunucu Yöneticisi panosunda, **Araçlar** menüsünde **Hizmetler**' i seçin.
   3. **Hizmetler** penceresinde **Microsoft StorSimple yönetim hizmeti**' ni seçin.
   4. Sağ bölmedeki **Microsoft StorSimple yönetim hizmeti**altında **hizmeti Durdur**' a tıklayın.
2. Ana bilgisayarda C:\programdata\microsoft\storsimple\bacatalogdizinine göz atın. 
   
   > [!NOTE]
   > ProgramData gizli bir klasördür.
   > 
   > 
3. Katalog XML dosyasını bulun, dosyayı kopyalayın ve kopyayı güvenli bir konuma veya buluta depolayın. Ana bilgisayar başarısız olursa, StorSimple Snapshot Manager içinde oluşturduğunuz yedekleme ilkelerinin kurtarılmasını sağlamak için bu yedekleme dosyasını kullanabilirsiniz.
   
    ![Azure StorSimple yedekleme kataloğu dosyası](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)
4. Microsoft StorSimple Yönetim hizmetini yeniden başlatın: 
   
   1. Sunucu Yöneticisi panosunda, **Araçlar** menüsünde **Hizmetler**' i seçin.
   2. **Hizmetler** penceresinde **Microsoft StorSimple yönetim hizmeti**' ni seçin.
   3. Sağ bölmedeki **Microsoft StorSimple yönetim hizmeti**altında **hizmeti yeniden Başlat**' a tıklayın.
5. Ana bilgisayarda C:\programdata\microsoft\storsimple\bacatalogdizinine göz atın. 
6. Katalog XML dosyasını silin ve bunu oluşturduğunuz yedek sürümle değiştirin. 
7. StorSimple Snapshot Manager başlatmak için masaüstü StorSimple Snapshot Manager simgesine tıklayın. 

## <a name="next-steps"></a>Sonraki adımlar
* StorSimple [çözümünüzü yönetmek Için storsimple Snapshot Manager kullanma](storsimple-snapshot-manager-admin.md)hakkında daha fazla bilgi edinin.
* [StorSimple Snapshot Manager görevleri ve iş akışları](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows)hakkında daha fazla bilgi edinin.

