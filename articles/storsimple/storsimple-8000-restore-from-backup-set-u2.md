---
title: StorSimple 8000 serisinde yedeklemeden ses düzeyini geri yükleme | Microsoft Dokümanlar
description: StorSimple birimsini yedekleme kümesinden geri yüklemek için StorSimple Device Manager hizmeti Yedekleme Kataloğu'nun nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/23/2017
ms.author: alkohli
ms.openlocfilehash: 6a2e022697ced90d968075b7a4abe4163be7a539
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60723399"
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Yedekleme kümesinden StorSimple birimini geri yükleme

## <a name="overview"></a>Genel Bakış

Bu öğretici, varolan bir yedekleme kümesini kullanarak StorSimple 8000 serisi bir aygıtta gerçekleştirilen geri yükleme işlemini açıklar. Yerel veya bulut yedeklemesinden bir birim geri yüklemek için **Yedekleme kataloğu** bıçak kullanın. **Yedek katalog** bıçağı, manuel veya otomatik yedekleme alındığında oluşturulan tüm yedekleme kümelerini görüntüler. Yedekleme kümesinden geri yükleme işlemi, veriler arka planda karşıdan yüklenirken sesi hemen çevrimiçi duruma getirir.

Geri yüklemeyi başlatmak için alternatif bir yöntem, **Aygıtlar > [Cihazınız] > Birimleri'ne**gitmektir. **Birimler** bıçak, bir birim seçin, bağlam menüsünü çağırmak için sağ tıklatın ve sonra **Geri yükle'yi**seçin.

## <a name="before-you-restore"></a>Geri yüklemeden önce

Geri yüklemeye başlamadan önce aşağıdaki uyarıları gözden geçirin:

* **Ses düzeyini çevrimdışına almanız gerekir** – Geri yükleme işlemini başlatmadan önce hem ana bilgisayarda hem de aygıtta ses düzeyini çevrimdışına alın. Geri yükleme işlemi aygıtın sesini otomatik olarak çevrimiçi duruma getirse de, aygıtı ana bilgisayarda el ile çevrimiçi duruma getirmeniz gerekir. Ses aygıtında çevrimiçi olur olmaz, ses düzeyini ana bilgisayarda çevrimiçi duruma getirebilirsiniz. (Geri yükleme işlemi tamamlanana kadar beklemeniz gerekmez.) Yordamlar [için, bir birimi çevrimdışı olarak alın'](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline)a gidin.

* **Geri yüklemeden sonra ses türü** – Silinen birimler anlık görüntüdeki türe bağlı olarak geri yüklenir; diğer bir diğer adıyla, yerel olarak sabitlenen birimler, yerel olarak sabitlenmiş birimler olarak geri yüklenir ve katmanlı birimler olarak geri yüklenir.

    Varolan birimler için, birimin geçerli kullanım türü anlık görüntüde depolanan türü geçersiz kılar. Örneğin, birim türü katmanlı olduğunda alınan anlık görüntüden bir birim geri yüklenirseniz ve bu birim türü artık yerel olarak sabitlenirse (gerçekleştirilen bir dönüşüm işlemi nedeniyle), birim yerel olarak sabitlenmiş bir birim olarak geri yüklenir. Benzer şekilde, varolan yerel olarak sabitlenmiş bir birim genişletildiyse ve daha sonra ses daha küçük olduğunda çekilen eski bir anlık görüntüden geri yüklenirse, geri yüklenen birim geçerli genişletilmiş boyutu korur.

    Bir birim, katmanlı bir birimden yerel olarak sabitlenmiş bir birime veya yerel olarak sabitlenmiş bir birimden, birim geri yüklenirken katmanlı bir birime dönüştüremezsiniz. Geri yükleme işlemi tamamlanana kadar bekleyin ve sonra ses düzeyini başka bir türe dönüştürebilirsiniz. Birim dönüştürme hakkında bilgi için [birim türünü değiştir'e](storsimple-8000-manage-volumes-u2.md#change-the-volume-type)gidin. 

* **Ses düzeyi geri yüklenen birimde yansıtılır** – Silinmiş yerel olarak sabitlenmiş bir birimi geri yükleniyorsanız (yerel olarak sabitlenmiş birimler tam olarak sağlandığı için) bu önemli bir husustur. Daha önce silinmiş yerel olarak sabitlenmiş bir birimi geri yüklemeye çalışmadan önce yeterli alana sahip olduğundan emin olun.

* **Geri yüklenirken bir birimi genişletemezsiniz** – Sesi genişletmeye çalışmadan önce geri yükleme işlemi bitene kadar bekleyin. Bir birimi genişletme hakkında bilgi [için, birimi değiştir'e](storsimple-8000-manage-volumes-u2.md#modify-a-volume)gidin.

* **Yerel bir birimi geri yüklerken yedekleme gerçekleştirebilirsiniz** – Yordamlar için yedekleme ilkelerini yönetmek için [StorSimple Device Manager hizmetini kullanın.](storsimple-8000-manage-backup-policies-u2.md)

* **Geri yükleme işlemini iptal edebilirsiniz** – Geri yükleme işini iptal ederseniz, ses düzeyi geri yükleme işlemini başlatmadan önce olduğu duruma geri alınır. Yordamlar [için, bir işi iptal](storsimple-8000-manage-jobs-u2.md#cancel-a-job)etme'ye gidin.

## <a name="how-does-restore-work"></a>Geri yükleme nasıl çalışır?

Güncelleştirme 4 veya daha sonra çalıştıran aygıtlar için ısı haritası tabanlı bir geri yükleme uygulanır. Ana bilgisayar, verilere erişim istekleri aygıta eriştikçe, bu istekler izlenir ve bir ısı haritası oluşturulur. Yüksek istek oranı, daha yüksek ısıya sahip veri yığınlarıyla sonuçlanırken, daha düşük istek oranı daha düşük ısılı parçalara çevirir. _Sıcak_olarak işaretlenecek verilere en az iki kez erişmelisiniz. Değiştirilen bir dosya da _sıcak_olarak işaretlenir. Geri yüklemeyi başlattıktan sonra, ısı haritasına dayalı olarak verilerin proaktif olarak hidrasyonu gerçekleşir. Güncelleştirme 4'ten önceki sürümler için veriler yalnızca erişime dayalı olarak geri yükleme sırasında indirilir.

Aşağıdaki uyarılar ısı haritası tabanlı geri yüklemeler için geçerlidir:

* Isı haritası izleme yalnızca katmanlı birimler için etkinleştirilir ve yerel olarak sabitlenmiş birimler desteklenmez.

* Isı haritası tabanlı geri yükleme, bir ses düzeyini başka bir aygıta klonlarken desteklenmez. 

* Aygıtta bir yerinde geri yükleme varsa ve geri yüklenecek birim için yerel bir anlık görüntü varsa, o zaman yeniden su landırmayız (veriler yerel olarak zaten mevcut olduğundan). 

* Varsayılan olarak, geri yüklediğinizde, rehidrasyon işleri ısı haritasına dayalı verileri proaktif olarak yeniden sulandırın. 

Güncelleme 4'te, Windows PowerShell cmdlets rehidrasyon işleri çalışan sorgulamak için kullanılabilir, bir rehidrasyon iş iptal, ve rehidrasyon iş durumunu almak.

* `Get-HcsRehydrationJob`- Bu cmdlet rehidrasyon iş durumunu alır. Tek bir rehidrasyon işi tek bir cilt için tetiklenir.

* `Set-HcsRehydrationJob`- Bu cmdlet, rehidrasyon devam ederken rehidrasyon işi, duraklatmak, durdurmak, rehidrasyon iş devam etmenizi sağlar.

Rehidrasyon cmdlets hakkında daha fazla bilgi için, [StorSimple için Windows PowerShell cmdlet referans](https://technet.microsoft.com/library/dn688168.aspx)gidin.

Otomatik rehidrasyon ile, genellikle daha yüksek geçici okuma performansı beklenmektedir. İyileştirmelerin gerçek büyüklüğü erişim deseni, veri karmaşası ve veri türü gibi çeşitli etkenlere bağlıdır. 

Rehidrasyon işini iptal etmek için PowerShell cmdlet'i kullanabilirsiniz. Gelecekteki tüm geri yüklemeler için rehidrasyon işlerini kalıcı olarak devre dışı kılmış olmak istiyorsanız, [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)

## <a name="how-to-use-the-backup-catalog"></a>Yedekleme kataloğu nasıl kullanılır?

**Yedek Katalog** bıçağı, yedek set seçiminizi daraltmanıza yardımcı olan bir sorgu sağlar. Aşağıdaki parametrelere göre alınan yedekleme kümelerine filtre uygulayabilirsiniz:

* **Zaman aralığı** – Yedekleme kümesinin oluşturulduğu tarih ve saat aralığı.
* **Aygıt** – Yedekleme kümesinin oluşturulduğu aygıt.
* **Yedekleme ilkesi** veya **Birim** - Bu yedekleme kümesiyle ilişkili yedekleme ilkesi veya birim.

Filtre uygulanmış yedekleme kümeleri aşağıdaki özniteliklere göre tablolanır:

* **Ad** – Yedekleme kümesiyle ilişkili yedekleme ilkesinin veya biriminin adı.
* **Tür** - Yedekleme kümeleri yerel anlık görüntüler veya bulut anlık görüntüleri olabilir. Yerel anlık görüntü, aygıtta yerel olarak depolanan tüm birim verilerinizin yedeği olurken, bulut anlık görüntüsü bulutta bulunan birim verilerinin yedeğini ifade eder. Yerel anlık görüntüler daha hızlı erişim sağlarken, veri esnekliği için bulut anlık görüntüleri seçilir.
* **Boyut** – Yedekleme kümesinin gerçek boyutu.
* **Oluşturuldu** – Yedeklemelerin oluşturulduğu tarih ve saat. 
* **Birimler** - Yedekleme kümesiyle ilişkili birim sayısı.
* **Başlatılmış** – Yedeklemeler bir programa göre veya kullanıcı tarafından el ile otomatik olarak başlatılabilir. (Yedeklemeleri zamanlamak için bir yedekleme ilkesi kullanabilirsiniz. Alternatif olarak, etkileşimli veya isteğe bağlı yedekleme almak için **Yedeklemeyi Al** seçeneğini kullanabilirsiniz.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>StorSimple ses düzeyinibir yedeklemeden geri yükleme

StorSimple ses inizi belirli bir yedeklemeden geri yüklemek için **Yedek Katalog** bıçağını kullanabilirsiniz. Ancak, bir birim geri yedekleme alındığında olduğu duruma ses geri olacağını unutmayın. Yedekleme işleminden sonra eklenen tüm veriler kaybolur.

> [!WARNING]
> Yedeklemeden geri geri alma, yedeklemeden varolan birimleri değiştirir. Bu, yedekleme alındıktan sonra yazılan verilerin kaybolmasına neden olabilir.


### <a name="to-restore-your-volume"></a>Ses inizi geri yüklemek için
1. StorSimple Device Manager hizmetinize gidin ve ardından **Yedekleme kataloğunu**tıklatın.

2. Aşağıdaki gibi bir yedekleme kümesi seçin:
   
   1. Zaman aralığını belirtin.
   2. Uygun aygıtı seçin.
   3. Açılan listede, seçmek istediğiniz yedeklemenin ses düzeyini veya yedekleme ilkesini seçin.
   4. Bu sorguyu yürütmek için **Uygula'yı** tıklatın.

      Seçili birim veya yedekleme ilkesiyle ilişkili yedeklemeler yedekleme kümeleri listesinde görünmelidir.
   
      ![Yedek ayar listesi](./media/storsimple-8000-restore-from-backup-set-u2/bucatalog.png)     
     
3. İlişkili birimleri görüntülemek için yedekleme kümesini genişletin. Bu birimleri geri yüklemeden önce ana bilgisayar ve aygıtta çevrimdışı olarak alınmalıdır. Cihazınızın **Birimler** bıçak larındaki birimlere erişin ve bunları çevrimdışı hale getirmek için [bir birim çevrimdışı alın'daki](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) adımları izleyin.
   
   > [!IMPORTANT]
   > Aygıttaki birimleri çevrimdışına almadan önce, önce ana bilgisayardaki birimleri çevrimdışına aldığınızdan emin olun. Ana bilgisayardaki birimleri çevrimdışına almazsanız, bu işlem veri bozulmasına neden olabilir.
   
4. **Yedek Katalog** sekmesine geri gidin ve bir yedekleme kümesi seçin. Sağ tıklatın ve ardından bağlam menüsünden **Geri Yükle'yi**seçin.

    ![Yedek ayar listesi](./media/storsimple-8000-restore-from-backup-set-u2/restorebu1.png)

5. Onayınız istenir. Geri yükleme bilgilerini gözden geçirin ve onay onay kutusunu seçin.
   
    ![Onay sayfası](./media/storsimple-8000-restore-from-backup-set-u2/restorebu2.png)

7. **Geri Yükle'yi**tıklatın. Bu, **İşler** sayfasına erişerek görüntülediğiniz bir geri yükleme işini başlatır.

   ![Onay sayfası](./media/storsimple-8000-restore-from-backup-set-u2/restorebu5.png)

8. Geri yükleme tamamlandıktan sonra, birimlerinizin içeriğinin yedekteki birimlerle değiştirilmelerini doğrulayın.


## <a name="if-the-restore-fails"></a>Geri yükleme başarısız olursa

Geri yükleme işlemi herhangi bir nedenle başarısız olursa bir uyarı alırsınız. Bu durumda, yedeklemenin hala geçerli olduğunu doğrulamak için yedekleme listesini yenileyin. Yedekleme geçerliyse ve buluttan geri alıyorsanız, bağlantı sorunları soruna neden olabilir.

Geri yükleme işlemini tamamlamak için, ses düzeyini ana bilgisayarda çevrimdışı na alın ve geri yükleme işlemini yeniden deneyin. Geri yükleme işlemi sırasında gerçekleştirilen birim verilerinde yapılan değişikliklerin kaybolacağını unutmayın.

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple birimlerini nasıl yöneteceklerini](storsimple-8000-manage-volumes-u2.md)öğrenin.
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini](storsimple-8000-manager-service-administration.md)nasıl kullanacağınızı öğrenin.

