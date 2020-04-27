---
title: StorSimple 8000 serisine bir birimi yedekten geri yükleme | Microsoft Docs
description: Bir StorSimple birimini bir yedekleme kümesinden geri yüklemek için StorSimple Aygıt Yöneticisi hizmeti yedekleme kataloğunun nasıl kullanılacağını açıklar.
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
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "60723399"
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Yedekleme kümesinden StorSimple birimini geri yükleme

## <a name="overview"></a>Genel Bakış

Bu öğreticide, mevcut bir yedekleme kümesi kullanılarak StorSimple 8000 serisi cihazında gerçekleştirilen geri yükleme işlemi açıklanmaktadır. Bir birimi yerel veya bulut yedeğinden geri yüklemek için **Yedekleme kataloğu** dikey penceresini kullanın. **Yedekleme kataloğu** dikey penceresi, el ile veya otomatik yedeklemeler çekilirken oluşturulan tüm yedekleme kümelerini görüntüler. Bir yedekleme kümesinden geri yükleme işlemi, veriler arka planda indirilirken birimi çevrimiçi duruma getirir.

Geri yüklemeyi başlatmak için alternatif bir yöntem, **cihazlara > [cihazınız] > birimlere**gitmenize olanak sağlar. **Birimler** dikey penceresinde bir birim seçin, bağlam menüsünü çağırmak için sağ tıklayın ve ardından **geri yükle**' yi seçin.

## <a name="before-you-restore"></a>Geri yüklemeden önce

Bir geri yükleme işlemine başlamadan önce, aşağıdaki uyarıları gözden geçirin:

* **Birimi çevrimdışına** almalısınız; geri yükleme işlemini gerçekleştirmeden önce, birimi konakta ve cihazda çevrimdışı duruma getirin. Geri yükleme işlemi otomatik olarak birimi cihaza çevrimiçi duruma getirse de, cihazı konakta çevrimiçi olarak el ile taşımanız gerekir. Birim cihazda çevrimiçi olduğunda birimi konakta çevrimiçi duruma getirebilirsiniz. (Geri yükleme işlemi bitinceye kadar beklemeniz gerekmez.) Yordamlar için [bir birimi çevrimdışına alma](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline)bölümüne gidin.

* **Geri yükleme sonrasında birim türü** – silinen birimler anlık görüntüdeki türe göre geri yüklenir; diğer bir deyişle, yerel olarak sabitlenmiş birimler yerel olarak sabitlenmiş birimler ve katmanlı birim olarak geri yüklenmiş birimler olarak geri yüklenir.

    Mevcut birimlerde, birimin geçerli kullanım türü, anlık görüntüde depolanan türü geçersiz kılar. Örneğin, birim türü katmanlı olduğunda ve birim türü artık yerel olarak sabitlendiğinde (gerçekleştirilen bir dönüştürme işlemi nedeniyle) bir birimi bir anlık görüntüden geri yüklerseniz, birim yerel olarak sabitlenmiş bir birim olarak geri yüklenir. Benzer şekilde, mevcut bir yerel olarak sabitlenmiş bir birim genişletilmişse ve daha sonra birim daha küçük olduğunda alınmış daha eski bir anlık görüntüden geri yüklenirse, geri yüklenen birim geçerli genişletilmiş boyutu korur.

    Birim geri yüklenirken katmanlı bir birimden yerel olarak sabitlenmiş bir birime veya yerel olarak sabitlenmiş bir birimden bir birimi bir katmanlı birime dönüştüremezsiniz. Geri yükleme işlemi tamamlanana kadar bekleyin ve ardından birimi başka bir türe dönüştürebilirsiniz. Bir birimi dönüştürme hakkında daha fazla bilgi için [birim türünü değiştirme](storsimple-8000-manage-volumes-u2.md#change-the-volume-type)bölümüne gidin. 

* **Birim boyutu geri yüklenen birime yansıtılır** . Bu, silinen yerel olarak sabitlenmiş bir birimi geri yüklüyorsanız (yerel olarak sabitlenmiş birimler tam olarak sağlandığından) önemli bir konudur. Daha önce silinmiş olan yerel olarak sabitlenmiş bir birimi geri yüklemeyi denemeden önce yeterli alana sahip olduğunuzdan emin olun.

* **Bir birimi geri yüklenirken genişletemezsiniz** ; birimi genişletmeyi denemeden önce geri yükleme işlemi tamamlanana kadar bekleyin. Bir birimi genişletme hakkında daha fazla bilgi için [bir birimi değiştirme](storsimple-8000-manage-volumes-u2.md#modify-a-volume)bölümüne gidin.

* **Yerel bir birimi geri yüklerken yedekleme gerçekleştirebilirsiniz** : yordamlar için [, yedekleme Ilkelerini yönetmek üzere StorSimple Aygıt Yöneticisi hizmetini kullanma](storsimple-8000-manage-backup-policies-u2.md)bölümüne gidin.

* **Geri yükleme işlemini iptal edebilirsiniz** . geri yükleme işini iptal ederseniz, geri yükleme işlemini başlatmadan önce birim bulunduğu duruma geri döndürülür. Yordamlar için [bir Işi Iptal etme](storsimple-8000-manage-jobs-u2.md#cancel-a-job)bölümüne gidin.

## <a name="how-does-restore-work"></a>Geri yükleme nasıl çalışır?

Güncelleştirme 4 veya üstünü çalıştıran cihazlarda heatmap tabanlı bir geri yükleme uygulanır. Verilere erişmek için ana bilgisayar istekleri cihaza ulaştığında, bu istekler izlenir ve bir ısı haritasını oluşturulur. Yüksek istek hızı, daha yüksek bir ısı ile veri öbeklerinin sonucunu azaltır ve daha düşük bir istek hızı daha düşük olan parçalara çevrilir. Verilere en az iki kez erişmeniz _gerekir._ Değiştirilen bir dosya aynı zamanda _sık_erişimli olarak işaretlenir. Geri yüklemeyi başlattığınızda, verilerin öngörülü olarak kullanılması heatmap 'e göre oluşur. Güncelleştirme 4 ' ten önceki sürümler için, veriler yalnızca erişim temelinde geri yükleme sırasında indirilir.

Aşağıdaki uyarılar, heatmap tabanlı geri yüklemeler için geçerlidir:

* Heatmap izlemesi yalnızca katmanlı birimlerde etkindir ve yerel olarak sabitlenmiş birimler desteklenmez.

* Bir birim başka bir cihaza kopyalanırken heatmap tabanlı geri yükleme desteklenmez. 

* Bir yerinde geri yükleme varsa ve birimin geri yüklenmesi için bir yerel anlık görüntü varsa, bu durumda yeniden doldurma işlemi gerçekleşmez (veriler yerel olarak zaten kullanılabilir). 

* Varsayılan olarak, geri doldurma işleri, verileri heatmap 'e göre proaktif olarak yeniden sarmalayarak başlatılır. 

Güncelleştirme 4 ' te, Windows PowerShell cmdlet 'leri çalışan yeniden doldurma işlerini sorgulamak, bir yeniden doldurma işini iptal etmek ve yeniden doldurma işinin durumunu almak için kullanılabilir.

* `Get-HcsRehydrationJob`-Bu cmdlet, yeniden doldurma işinin durumunu alır. Tek bir birim için tek bir yeniden doldurma işi tetiklenir.

* `Set-HcsRehydrationJob`-Bu cmdlet, yeniden doldurma işlemi devam ederken, yeniden doldurma işini duraklatmanızı, durdurmanızı, sürdürmenizi sağlar.

Yeniden doldurma cmdlet 'leri hakkında daha fazla bilgi için, [StorSimple Için Windows PowerShell cmdlet başvurusuna](https://technet.microsoft.com/library/dn688168.aspx)bakın.

Otomatik yeniden doldurma sayesinde genellikle daha yüksek geçici okuma performansı beklenmektedir. Geliştirmelerin gerçek büyüklüğü, erişim deseninin, veri dalgalanması ve veri türü gibi çeşitli faktörlere bağlıdır. 

Bir yeniden doldurma işini iptal etmek için PowerShell cmdlet 'ini kullanabilirsiniz. Sonraki geri yüklemeler için yeniden doldurma işlerini kalıcı olarak devre dışı bırakmak istiyorsanız [Microsoft desteği başvurun](storsimple-8000-contact-microsoft-support.md).

## <a name="how-to-use-the-backup-catalog"></a>Yedekleme kataloğunu kullanma

**Yedekleme kataloğu** dikey penceresi, yedekleme kümesi seçiminizi daraltmanıza yardımcı olan bir sorgu sağlar. Aşağıdaki parametrelere göre alınan yedekleme kümelerini filtreleyebilirsiniz:

* **Zaman aralığı** : yedekleme kümesinin oluşturulduğu tarih ve saat aralığı.
* **Cihaz** : yedekleme kümesinin oluşturulduğu cihaz.
* Yedekleme **ilkesi** veya **birimi** : Bu yedekleme kümesiyle ilişkili yedekleme ilkesi veya birim.

Filtrelenmiş yedekleme kümeleri aşağıdaki özniteliklere göre tablo olarak ayarlanır:

* **Name** : yedekleme ilkesi veya yedekleme kümesiyle ilişkili birimin adı.
* **Tür** – yedekleme kümeleri yerel anlık görüntüler veya bulut anlık görüntüleri olabilir. Yerel anlık görüntü, cihazda yerel olarak depolanan tüm birim verilerinizin yedeğine karşın bir bulut anlık görüntüsü, bulutta bulunan birim verilerinin yedeklemesini ifade eder. Yerel anlık görüntüler, veri dayanıklılığı için bulut anlık görüntüleri seçildiği halde daha hızlı erişim sağlar.
* **Size** : yedekleme kümesinin gerçek boyutu.
* **Oluşturulma** tarihi: yedeklemelerin oluşturulduğu tarih ve saat. 
* **Birimler** -yedekleme kümesiyle ilişkili birim sayısı.
* **Başlatıldı** – yedeklemeler bir zamanlamaya göre otomatik olarak veya bir kullanıcı tarafından el ile başlatılabilir. (Yedeklemeleri zamanlamak için bir yedekleme İlkesi kullanabilirsiniz. Alternatif olarak, etkileşimli veya isteğe bağlı bir yedekleme gerçekleştirmek için **yedeği al** seçeneğini kullanabilirsiniz.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>StorSimple biriminizi bir yedekten geri yükleme

StorSimple biriminizi belirli bir yedekten geri yüklemek için **Yedekleme kataloğu** dikey penceresini kullanabilirsiniz. Ancak, bir birimi geri yüklemenin, birimi yedeklemenin alındığı duruma geri döndürdüğünü aklınızda bulundurun. Yedekleme işleminden sonra eklenen tüm veriler kaybedilir.

> [!WARNING]
> Yedekten geri yükleme, mevcut birimlerin yedekten yerini alır. Bu, yedekleme alındıktan sonra yazılan verilerin kaybedilmesine neden olabilir.


### <a name="to-restore-your-volume"></a>Biriminiz geri yüklemek için
1. StorSimple Aygıt Yöneticisi hizmetinize gidin ve ardından **Yedekleme kataloğu**' na tıklayın.

2. Aşağıdaki şekilde bir yedekleme kümesi seçin:
   
   1. Zaman aralığını belirtin.
   2. Uygun aygıtı seçin.
   3. Açılan listede, seçmek istediğiniz yedekleme için birim veya yedekleme ilkesini seçin.
   4. Bu sorguyu yürütmek için **Uygula** ' ya tıklayın.

      Seçilen birim veya yedekleme ilkesiyle ilişkili yedeklemeler, yedekleme kümeleri listesinde görünmelidir.
   
      ![Yedekleme kümesi listesi](./media/storsimple-8000-restore-from-backup-set-u2/bucatalog.png)     
     
3. İlişkili birimleri görüntülemek için yedekleme kümesini genişletin. Bu birimlerin geri yüklemeden önce konakta ve cihazda çevrimdışına alınması gerekir. Cihazınızın **birimler** dikey penceresinde birimlere erişin ve sonra çevrimdışına almak için [birimi çevrimdışına alma](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) ' daki adımları izleyin.
   
   > [!IMPORTANT]
   > Birimleri cihazda çevrimdışı duruma almadan önce, önce konakta bulunan birimleri çevrimdışına aldığınızdan emin olun. Birimleri konakta çevrimdışına alıp verilerin bozulmasına yol açabilir.
   
4. **Yedekleme kataloğu** sekmesine geri gidin ve bir yedekleme kümesi seçin. Sağ tıklayıp bağlam menüsünden **geri yükle**' yi seçin.

    ![Yedekleme kümesi listesi](./media/storsimple-8000-restore-from-backup-set-u2/restorebu1.png)

5. Onayınız istenir. Geri yükleme bilgilerini gözden geçirin ve ardından onay onay kutusunu seçin.
   
    ![Onay sayfası](./media/storsimple-8000-restore-from-backup-set-u2/restorebu2.png)

7. **Geri yükle**' ye tıklayın. Bu, **işler** sayfasına erişerek görüntüleyebilmeniz için bir geri yükleme işi başlatır.

   ![Onay sayfası](./media/storsimple-8000-restore-from-backup-set-u2/restorebu5.png)

8. Geri yükleme tamamlandıktan sonra, birimlerinizin içeriğinin yedekteki birimlerle değiştirildiğini doğrulayın.


## <a name="if-the-restore-fails"></a>Geri yükleme başarısız olursa

Geri yükleme işlemi herhangi bir nedenle başarısız olursa bir uyarı alırsınız. Bu durumda, yedeklemenin hala geçerli olduğunu doğrulamak için yedekleme listesini yenileyin. Yedekleme geçerliyse ve buluttan geri yüklüyorsanız, bağlantı sorunları soruna neden olabilir.

Geri yükleme işlemini gerçekleştirmek için, birimi konakta çevrimdışına alın ve geri yükleme işlemini yeniden deneyin. Geri yükleme işlemi sırasında gerçekleştirilen birim verilerinde yapılan tüm değişikliklerin kaybolacağını unutmayın.

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple birimlerini yönetmeyi](storsimple-8000-manage-volumes-u2.md)öğrenin.
* StorSimple [cihazınızı yönetmek Için storsimple Aygıt Yöneticisi hizmetini nasıl kullanacağınızı](storsimple-8000-manager-service-administration.md)öğrenin.

