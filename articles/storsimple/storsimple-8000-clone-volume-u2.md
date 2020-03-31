---
title: StorSimple 8000 serisinde bir hacim klonlamak | Microsoft Dokümanlar
description: Farklı klon türlerini ve kullanımını açıklar ve StorSimple 8000 serisi bir aygıtta tek bir birimi klonlamak için yedekleme kümesini nasıl kullanabileceğinizi açıklar.
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
ms.date: 12/05/2017
ms.author: alkohli
ms.openlocfilehash: 84734aefb72a3330d99c5707b461de2cd5e30484
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255008"
---
# <a name="use-the-storsimple-device-manager-service-in-azure-portal-to-clone-a-volume"></a>Azure portalında StorSimple Cihaz Yöneticisi'ni kullanarak bir birimi kopyalama

## <a name="overview"></a>Genel Bakış

Bu öğretici, **Yedekleme kataloğu** bıçakla tek bir birimi klonlamak için yedekleme kümesini nasıl kullanabileceğinizi açıklar. Ayrıca *geçici* ve *kalıcı* klonlar arasındaki farkı açıklar. Bu öğreticideki kılavuz, Güncelleme 3 veya daha sonra çalıştıran tüm StorSimple 8000 serisi aygıt için geçerlidir.

StorSimple Device Manager hizmeti **Yedek katalog** bıçağı, manuel veya otomatik yedekleme alındığında oluşturulan tüm yedekleme kümelerini görüntüler. Daha sonra klonlamak için yedek kümesinde bir birim seçebilirsiniz.

 ![Yedek ayar listesi](./media/storsimple-8000-clone-volume-u2/bucatalog.png)

## <a name="considerations-for-cloning-a-volume"></a>Bir birimi klonlamak için dikkat edilmesi gerekenler

Bir birimi klonlarken aşağıdaki bilgileri göz önünde bulundurun.

- Bir klon normal bir hacim le aynı şekilde şekilde olur. Bir birim üzerinde mümkün olan herhangi bir işlem klon için kullanılabilir.

- İzleme ve varsayılan yedekleme klonlanmış bir birimde otomatik olarak devre dışı bırakılır. Herhangi bir yedekleme için klonlanmış bir birim yapılandırmanız gerekir.

- Yerel olarak sabitlenmiş bir birim katmanlı bir birim olarak klonlanır. Klonlanan birimin yerel olarak sabitlenebilmek için gereken, klon işlemi başarıyla tamamlandıktan sonra klonu yerel olarak sabitlenmiş bir ses e dönüştürebilirsiniz. Katmanlı bir birimi yerel olarak sabitlenmiş bir birime dönüştürme hakkında bilgi [için, birim türünü değiştir'e](storsimple-8000-manage-volumes-u2.md#change-the-volume-type)gidin.

- Klonlandırılmış bir birimi klonlamadan hemen sonra (geçici bir klon olduğunda) yerel olarak sabitlenmiş katmanlı ya da sabitlenmiş bir birim dönüştürmeye çalışırsanız, dönüştürme aşağıdaki hata iletisiyle başarısız olur:

    `Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.`

    Bu hata yalnızca farklı bir aygıtta klonlama yaptığınızda alınır. Geçici klonu önce kalıcı bir klona dönüştürürseniz, birimi yerel olarak sabitlenmiş olarak başarıyla dönüştürebilirsiniz. Kalıcı bir klona dönüştürmek için geçici klonun bulut anlık görüntüsünü alın.

## <a name="create-a-clone-of-a-volume"></a>Bir birim klon oluşturma

Yerel veya bulut anlık görüntüsünü kullanarak aynı aygıtta, başka bir aygıtta ve hatta bir bulut aygıtında bir klon oluşturabilirsiniz.

Aşağıdaki yordam, yedekleme kataloğundan nasıl bir klon oluşturulacak açıklanmaktadır.  Klon başlatmak için alternatif bir yöntem **Hacimler**gitmektir, bir birim seçin, sonra bağlam menüsünü çağırmak için sağ tıklatın ve **Klon**seçin.

Yedekleme kataloğundan biriminizin bir klonu oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-clone-a-volume"></a>Bir birimi klonlamak için

1. StorSimple Device Manager hizmetinize gidin ve ardından **Yedekleme kataloğunu**tıklatın.

2. Aşağıdaki gibi bir yedekleme kümesi seçin:
   
   1. Uygun aygıtı seçin.
   2. Açılan listede, seçmek istediğiniz yedeklemenin ses düzeyini veya yedekleme ilkesini seçin.
   3. Zaman aralığını belirtin.
   4. Bu sorguyu yürütmek için **Uygula'yı** tıklatın.

      Seçili birim veya yedekleme ilkesiyle ilişkili yedeklemeler yedekleme kümeleri listesinde görünmelidir.
   
      ![Yedek ayar listesi](./media/storsimple-8000-clone-volume-u2/bucatalog.png)
     
3. İlişkili birimi görüntülemek için yedekleme kümesini genişletin ve yedek kümede bir birim seçin. Sağ tıklatın ve ardından bağlam menüsünden **Klon'u**seçin.

   ![Yedek ayar listesi](./media/storsimple-8000-clone-volume-u2/clonevol3b.png) 

3. **Klon** bıçağında aşağıdaki adımları yapın:
   
   1. Bir hedef aygıtı tanımlayın. Burası, klonun oluşturulacağı yerdir. Aynı aygıtı seçebilir veya başka bir aygıt belirtebilirsiniz.

      > [!NOTE]
      > Klon için gereken kapasitenin hedef aygıttaki kapasiteden daha düşük olduğundan emin olun.
       
   2. Klonunuzun benzersiz bir birim adı belirtin. Ad 3 ile 127 karakter arasında olmalıdır.
      
       > [!NOTE]
       > Yerel olarak sabitlenmiş bir birimi klonlasanız **bile, Klon Hacmi Olarak** alanı **Katmanlı** olacaktır. Bu ayarı değiştiremezsiniz; ancak, klonlanmış birimin yerel olarak sabitlenebilmeniz gerekiyorsa, klonu başarıyla oluşturduktan sonra yerel olarak sabitlenmiş bir birime dönüştürebilirsiniz. Katmanlı bir birimi yerel olarak sabitlenmiş bir birime dönüştürme hakkında bilgi [için, birim türünü değiştir'e](storsimple-8000-manage-volumes-u2.md#change-the-volume-type)gidin.
          
   3. **Bağlı ana bilgisayarlar**altında, klon için bir erişim denetim kaydı (ACR) belirtin. Yeni bir ACR ekleyebilir veya varolan listeden seçim yapabilirsiniz. ACR, bu klona hangi ana bilgisayarların erişebileceğini belirler.
      
       ![Yedek ayar listesi](./media/storsimple-8000-clone-volume-u2/clonevol3a.png) 

   4. İşlemi tamamlamak için **Clone'u** tıklatın.

4. Bir klon işi başlatılır ve klon başarıyla oluşturulduğunda size bildirilir. İş bildirimini tıklatın veya klon işini izlemek için **İşler** bıçağına gidin.

    ![Yedek ayar listesi](./media/storsimple-8000-clone-volume-u2/clonevol5.png)

7. Klonlama işi tamamlandıktan sonra cihazınıza gidin ve **Ardından Birimler'i**tıklatın. Birimler listesinde, kaynak hacmine sahip aynı birim kapsayıcısında oluşturulan klonu görmeniz gerekir.

    ![Yedek ayar listesi](./media/storsimple-8000-clone-volume-u2/clonevol6.png)

Bu şekilde oluşturulan bir klon geçici bir klondur. Klon türleri hakkında daha fazla bilgi için [Geçici ve kalıcı klonlara](#transient-vs-permanent-clones)bakın.


## <a name="transient-vs-permanent-clones"></a>Geçici vs kalıcı klonlar
Geçici klonlar yalnızca başka bir aygıta klonladiğinizde oluşturulur. Belirli bir birimi yedekleme kümesinden StorSimple Device Manager tarafından yönetilen farklı bir aygıta kopyalayabilirsiniz. Geçici klon, özgün birimdeki verilere başvurur ve bu verileri hedef aygıtta yerel olarak okumak ve yazmak için kullanır.

Geçici bir klonun bulut görüntüsünü aldıktan sonra, ortaya çıkan klon *kalıcı* bir klondur. Bu işlem sırasında, verilerin bir kopyası bulutta oluşturulur ve bu verileri kopyalama zamanı, verilerin boyutuna ve Azure gecikmelerine göre belirlenir (bu bir Azure'dan Azure'a kopyadır). Bu işlem günler ile haftalar arasında sürebilir. Geçici klon kalıcı bir klon haline gelir ve klonlanmış olduğu orijinal birim verilerine herhangi bir başvuru yoktur.

## <a name="scenarios-for-transient-and-permanent-clones"></a>Geçici ve kalıcı klonlar için senaryolar
Aşağıdaki bölümlerde geçici ve kalıcı klonların kullanılabildiği örnek durumlar açıklanacaktır.

### <a name="item-level-recovery-with-a-transient-clone"></a>Geçici bir klonla madde düzeyinde kurtarma
Bir yıllık Microsoft PowerPoint sunu dosyasını kurtarmanız gerekir. BT yöneticiniz o zamana ait belirli yedeklemeyi tanımlar ve sonra ses düzeyini filtreler. Yönetici daha sonra ses düzeyini klonlar, aradığınız dosyayı bulur ve size sağlar. Bu senaryoda, geçici bir klon kullanılır.

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Üretim ortamında kalıcı bir klonla test
Üretim ortamındabir test hatasını doğrulamanız gerekir. Üretim ortamında birimin bir klonu oluşturur sunuz ve ardından bağımsız bir klonlanmış hacim oluşturmak için bu klonun bulut anlık görüntüsünü alırsınız. Bu senaryoda, kalıcı bir klon kullanılır.

## <a name="next-steps"></a>Sonraki adımlar
* [Bir yedekleme kümesinden StorSimple birimini](storsimple-8000-restore-from-backup-set-u2.md)nasıl geri yükleyin öğrenin.
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini](storsimple-8000-manager-service-administration.md)nasıl kullanacağınızı öğrenin.

