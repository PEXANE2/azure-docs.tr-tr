---
title: StorSimple Snapshot Manager yedekleme kataloğu | Microsoft Dokümanlar
description: Yedekleme kataloğunu görüntülemek ve yönetmek için StorSimple Snapshot Manager MMC snap-in'in nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: twooley
manager: timlt
editor: ''
ms.assetid: 6abdbfd2-22ce-45a5-aa15-38fae4c8f4ec
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: twooley
ms.openlocfilehash: 38ef7774263e4b28b7c316fd0870ca8f7b89d6b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75931718"
---
# <a name="use-storsimple-snapshot-manager-to-manage-the-backup-catalog"></a>Yedekleme kataloğunu yönetmek için StorSimple Snapshot Manager'ı kullanın

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager birincil işlevi anlık görüntü şeklinde StorSimple birimlerinin uygulama tutarlı yedekleme kopyaları oluşturmak için izin vermektir. Anlık görüntüler daha sonra *yedek katalog*adı verilen bir XML dosyasında listelenir. Yedekleme kataloğu anlık görüntüleri ses grubuna ve ardından yerel anlık görüntüye veya bulut anlık görüntüsüne göre düzenler.

Bu öğretici, aşağıdaki görevleri tamamlamak için **Yedek Katalog** düğümlerini nasıl kullanabileceğinizi açıklar:

* Ses düzeyini geri yükleme
* Bir birim veya hacim grubunu klonlama
* Yedeklemeyi silme
* Dosyayı kurtarma
* Storsimple Snapshot Manager veritabanını geri yükleme

**Kapsam** bölmesinde **Yedek Katalog** düğümünü genişleterek ve sonra birim grubunu genişleterek yedekleme kataloğunu görüntüleyebilirsiniz.

* Birim grup adını tıklattığınızda, **Sonuçlar** bölmesi, birim grubu için kullanılabilen yerel anlık görüntü ve bulut anlık görüntü sayısını gösterir. 
* **Yerel Anlık Görüntü** veya Bulut Anlık **Görüntüsü'ni**tıklattığınızda, **Sonuçlar** bölmesi her yedek anlık görüntü yle ilgili aşağıdaki bilgileri gösterir **(Görünüm** ayarlarınıza bağlı olarak):
  
  * **Ad** – anlık görüntünün çekildiği saat.
  * **Yazın** – bu ister yerel bir anlık görüntü ister bulut anlık görüntüsü olsun.
  * **Sahibi** – içerik sahibi. 
  * **Kullanılabilir** – anlık görüntü nün şu anda kullanılabilir olup olmadığı. **True,** anlık görüntünün kullanılabilir olduğunu ve geri yüklenenebilen bir görüntü olduğunu gösterir; **False,** anlık görüntünün artık kullanılmayacağını gösterir. 
  * **İçe aktarılan** - yedeklemenin içe aktarılıp aktarılmayacağı. **True,** yedeklemenin aygıtın StorSimple Snapshot Manager'da yapılandırıldığı sırada StorSimple Device Manager hizmetinden alındığını gösterir; **False,** içe aktarılmadığını, ancak StorSimple Snapshot Manager tarafından oluşturulduğunu gösterir. (Birim grubunun içe aktarıldığı aygıtı tanımlayan bir sonek eklendiği için, içe aktarılan birim grubunu kolayca tanımlayabilirsiniz.)
    
    ![Yedek katalog](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)
* **Yerel Anlık Görüntü** veya Bulut Anlık **Görüntü'yi**genişletir ve sonra tek bir anlık görüntü adını tıklatırsanız, **Sonuçlar** bölmesi seçtiğiniz anlık görüntü hakkında aşağıdaki bilgileri gösterir:
  
  * **Adı** – sürücü harfi ile tanımlanan hacim. 
  * **Yerel Ad** – sürücünün yerel adı (varsa). 
  * **Aygıt** – ses in bulunduğu aygıtın adı. 
  * **Kullanılabilir** – anlık görüntü nün şu anda kullanılabilir olup olmadığı. **True,** anlık görüntünün kullanılabilir olduğunu ve geri yüklenenebilen bir görüntü olduğunu gösterir; **False,** anlık görüntünün artık kullanılmayacağını gösterir. 

## <a name="restore-a-volume"></a>Ses düzeyini geri yükleme
Yedeklemeden bir birim geri yüklemek için aşağıdaki yordamı kullanın.

#### <a name="prerequisites"></a>Ön koşullar
Bunu daha önce yapmadıysanız, bir hacim ve hacim grubu oluşturun ve ardından birim hacmi silin. Varsayılan olarak, StorSimple Snapshot Manager silinmesine izin vermeden önce bir birimi yedekler. Bu önlem, birim istemeden silinirse veya verilerin herhangi bir nedenle kurtarılması gerekiyorsa veri kaybını önleyebilir. 

StorSimple Snapshot Manager, ihtiyati yedekleme oluştururken aşağıdaki iletiyi görüntüler.

![Otomatik anlık görüntü iletisi](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

> [!IMPORTANT]
> Bir birim grubunun parçası olan bir birimi silemezsiniz. Silme seçeneği kullanılamıyor. <br>
> 
> 

#### <a name="to-restore-a-volume"></a>Bir sesi geri yüklemek için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın. 
2. **Kapsam** bölmesinde, Yedekleme **Kataloğu** düğümünü genişletin, bir birim grubunu genişletin ve ardından **Yerel Anlık Görüntü** veya Bulut Anlık **Görüntü'lerini**tıklatın. **Sonuçlar** bölmesinde yedek anlık görüntü listesi görüntülenir.
3. Geri yüklemek istediğiniz yedeklemeyi bulun, sağ tıklatın ve sonra **Geri Yükle'yi**tıklatın.
   
    ![Yedekleme kataloğunu geri yükleme](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 
4. Onay sayfasında, ayrıntıları gözden geçirin, **Onayla**yazın ve ardından **Tamam'ı**tıklatın. StorSimple Snapshot Manager, sesi geri yüklemek için yedeklemeyi kullanır.
   
    ![Onay iletisi geri yükleme](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 
5. Geri yükleme eylemini çalışırken izleyebilirsiniz. **Kapsam** **bölmesinde, İşler** düğümünü genişletin ve ardından **Çalıştır'ı**tıklatın. İş ayrıntıları **Sonuçlar** bölmesinde görünür. Geri yükleme işi tamamlandığında, iş ayrıntıları **Son 24 saat** listesine aktarılır.

## <a name="clone-a-volume-or-volume-group"></a>Bir birim veya hacim grubunu klonlama
Bir birim veya birim grubunun yinelenen (klonunu) oluşturmak için aşağıdaki yordamı kullanın.

#### <a name="to-clone-a-volume-or-volume-group"></a>Bir hacim veya hacim grubunu klonlamak için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** bölmesinde, Yedekleme **Kataloğu** düğümünü genişletin, bir birim grubunu genişletin ve ardından **Bulut Anlık Görüntü'ye**tıklayın. **Sonuçlar** bölmesinde yedeklerlistesi görünür.
3. Klonlamak istediğiniz birim veya birim grubunu bulun, ses veya birim grup adını sağ tıklatın ve **Klon'u**tıklatın. **Klon Bulut Anlık Görüntü** iletişim kutusu görüntülenir.
   
    ![Bulut anlık görüntüsünü klonlama](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. Klon **Bulut Anlık** Görüntü iletişim kutusunu aşağıdaki gibi tamamlayın: 
   
   1. **Ad** metin kutusuna, klonlanan birim için bir ad yazın. Bu **ad, Birimler** düğümünde görünür. 
   2. (İsteğe bağlı) **Sürücü'yi**seçin ve ardından açılan listeden bir sürücü harfi seçin.
   3. (İsteğe bağlı) **Klasör (NTFS)** seçeneğini seçin ve bir klasör yolu yazın veya Gözat'ı tıklatın ve klasör için bir konum seçin. 
   4. **Oluştur'u**tıklatın.
5. Klonlama işlemi tamamlandığında, klonlanan birimi başlatmanız gerekir. Sunucu Yöneticisi'ni başlatın ve ardından Disk Yönetimi'ni başlatın. Ayrıntılı talimatlar için [Bkz. Montaj hacimleri.](storsimple-snapshot-manager-manage-volumes.md#mount-volumes) İlkolarak başlatıldıktan sonra, birim **Kapsam** bölmesindeki **Birimler** düğümü altında listelenir. Listelenen birim görmüyorsanız, birim listesini yenileyin **(Birimler** düğümü sağ tıklatın ve sonra **Yenile'yi**tıklatın).

## <a name="delete-a-backup"></a>Yedeklemeyi silme
Yedek katalogdan anlık görüntü silmek için aşağıdaki yordamı kullanın. 

> [!NOTE]
> Anlık görüntü silmek, anlık görüntüyle ilişkili yedeklenen verileri siler. Ancak, buluttan veri temizleme işlemi biraz zaman alabilir.<br>


#### <a name="to-delete-a-backup"></a>Yedeklemeyi silmek için
1. StorSimple Snapshot Manager'ı başlatmak için masaüstü simgesini tıklatın.
2. **Kapsam** bölmesinde, Yedekleme **Kataloğu** düğümünü genişletin, bir birim grubunu genişletin ve ardından **Yerel Anlık Görüntü** veya Bulut Anlık **Görüntü'lerini**tıklatın. **Sonuçlar** bölmesinde anlık görüntü listesi görüntülenir.
3. Silmek istediğiniz anlık görüntüye sağ tıklayın ve ardından **Sil'i**tıklatın.
4. Onay iletisi göründüğünde **Tamam'ı**tıklatın.

## <a name="recover-a-file"></a>Dosyayı kurtarma
Bir dosya yanlışlıkla bir birimden silinirse, silme işleminden önce tarihlenen bir anlık görüntü alarak, anlık görüntüyle ses düzeyinin bir kopyasını oluşturarak ve ardından dosyayı klonlanmış birimden özgün ses birimine kopyalayarak dosyayı kurtarabilirsiniz.

#### <a name="prerequisites"></a>Ön koşullar
Başlamadan önce, ses grubunun geçerli bir yedeklemeolduğundan emin olun. Ardından, o birim grubundaki birimlerden birinde depolanan bir dosyayı silin. Son olarak, silinen dosyayı yedeklemenizden geri yüklemek için aşağıdaki adımları kullanın. 

#### <a name="to-recover-a-deleted-file"></a>Silinen bir dosyayı kurtarmak için
1. Masaüstünüzdeki StorSimple Snapshot Manager simgesine tıklayın. StorSimple Snapshot Manager konsol penceresi görüntülenir. 
2. **Kapsam** bölmesinde, Yedekleme **Kataloğu** düğümünü genişletin ve silinen dosyayı içeren bir anlık görüntüye göz atın. Genellikle, silme işleminden hemen önce oluşturulmuş bir anlık görüntü seçmeniz gerekir.
3. Klonlamak istediğiniz birimi bulun, sağ tıklatın ve **Klon'u**tıklatın. **Klon Bulut Anlık Görüntü** iletişim kutusu görüntülenir.
   
    ![Bulut anlık görüntüsünü klonlama](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. Klon **Bulut Anlık** Görüntü iletişim kutusunu aşağıdaki gibi tamamlayın: 
   
   1. **Ad** metin kutusuna, klonlanan birim için bir ad yazın. Bu **ad, Birimler** düğümünde görünür. 
   2. (İsteğe bağlı) **Sürücü'yi**seçin ve ardından açılan listeden bir sürücü harfi seçin. 
   3. (İsteğe bağlı) **Klasör (NTFS)** seçeneğini seçin ve bir klasör yolu yazın veya **Gözat'ı** tıklatın ve klasör için bir konum seçin. 
   4. **Oluştur'u**tıklatın. 
5. Klonlama işlemi tamamlandığında, klonlanan birimi başlatmanız gerekir. Sunucu Yöneticisi'ni başlatın ve ardından Disk Yönetimi'ni başlatın. Ayrıntılı talimatlar için [Bkz. Montaj hacimleri.](storsimple-snapshot-manager-manage-volumes.md#mount-volumes) İlkolarak başlatıldıktan sonra, birim **Kapsam** bölmesindeki **Birimler** düğümü altında listelenir. 
   
    Listelenen birim görmüyorsanız, birim listesini yenileyin **(Birimler** düğümü sağ tıklatın ve sonra **Yenile'yi**tıklatın).
6. Klonlanmış birim içeren NTFS klasörünü açın, **Birim** düğümünü genişletin ve ardından klonlanan birimi açın. Kurtarmak istediğiniz dosyayı bulun ve birincil birimde kopyalayın.
7. Dosyayı geri yükledikten sonra, klonlanan birimi içeren NTFS klasörünü silebilirsiniz.

## <a name="restore-the-storsimple-snapshot-manager-database"></a>StorSimple Snapshot Manager veritabanını geri yükleme
Ana bilgisayardaki StorSimple Snapshot Manager veritabanını düzenli olarak yedeklemeniz gerekir. Bir felaket oluşursa veya ana bilgisayar herhangi bir nedenle başarısız olursa, daha sonra yedeklemeden geri yükleyebilirsiniz. Veritabanı yedeklemeoluşturma el ile bir işlemdir.

#### <a name="to-back-up-and-restore-the-database"></a>Veritabanını yedeklemek ve geri yüklemek için
1. Microsoft StorSimple Yönetim Hizmetini Durdurun:
   
   1. Sunucu Yöneticisi'ni başlatın.
   2. Server Manager panosunda, **Araçlar** menüsünde **Hizmetler'i**seçin.
   3. **Hizmetler** penceresinde, Microsoft **StorSimple Yönetim Hizmeti'ni**seçin.
   4. Microsoft **StorSimple Management Service**altında, sağ **bölmede, hizmeti durdur'u**tıklatın.
2. Ana bilgisayarda C:\ProgramData\Microsoft\StorSimple\BACatalog'a göz atın. 
   
   > [!NOTE]
   > ProgramData gizli bir klasördür.
   > 
   > 
3. Katalog XML dosyasını bulun, dosyayı kopyalayın ve kopyayı güvenli bir konumda veya bulutta saklayın. Ana bilgisayar başarısız olursa, StorSimple Snapshot Manager'da oluşturduğunuz yedekleme ilkelerini kurtarmanıza yardımcı olmak için bu yedekleme dosyasını kullanabilirsiniz.
   
    ![Azure StorBasit yedekleme kataloğu dosyası](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)
4. Microsoft StorSimple Yönetim Hizmetini yeniden başlatın: 
   
   1. Server Manager panosunda, **Araçlar** menüsünde **Hizmetler'i**seçin.
   2. **Hizmetler** penceresinde, Microsoft **StorSimple Yönetim Hizmeti'ni**seçin.
   3. Microsoft **StorSimple Management Service**altında, sağ bölmede, **hizmeti Yeniden Başlat'ı**tıklatın.
5. Ana bilgisayarda C:\ProgramData\Microsoft\StorSimple\BACatalog'a göz atın. 
6. Katalog XML dosyasını silin ve oluşturduğunuz yedekleme sürümüyle değiştirin. 
7. StorSimple Snapshot Manager'ı başlatmak için masaüstü StorSimple Snapshot Manager simgesini tıklatın. 

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager'ı kullanma](storsimple-snapshot-manager-admin.md)hakkında daha fazla bilgi edinin.
* [StorSimple Snapshot Manager görevleri ve iş akışları](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows)hakkında daha fazla bilgi edinin.

