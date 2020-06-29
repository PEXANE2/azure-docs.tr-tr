---
title: StorSimple 8000 serisinde bir birimi kopyalayın | Microsoft Docs
description: Farklı kopya türlerini ve kullanımını açıklar ve bir StorSimple 8000 serisi cihazında tek bir birimi kopyalamak için bir yedekleme kümesini nasıl kullanabileceğinizi açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/05/2017
ms.author: alkohli
ms.openlocfilehash: 1ddc7d9e359fbb43680ae093c4e6cd2b9fb1ab2d
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85514975"
---
# <a name="use-the-storsimple-device-manager-service-in-azure-portal-to-clone-a-volume"></a>Azure portalında StorSimple Cihaz Yöneticisi'ni kullanarak bir birimi kopyalama

## <a name="overview"></a>Genel Bakış

Bu öğreticide, yedekleme **kataloğu** dikey penceresi aracılığıyla tek bir birimi kopyalamak için bir yedekleme kümesini nasıl kullanabileceğiniz açıklanmaktadır. Ayrıca *geçici* ve *kalıcı* kopyalar arasındaki farkı açıklar. Bu öğreticideki kılavuz, güncelleştirme 3 veya sonraki sürümleri çalıştıran tüm StorSimple 8000 serisi cihazlar için geçerlidir.

StorSimple Aygıt Yöneticisi hizmeti **Yedekleme kataloğu** dikey penceresi, el ile veya otomatik yedeklemeler çekilirken oluşturulan tüm yedekleme kümelerini görüntüler. Ardından, kopyalamak üzere bir yedekleme kümesindeki bir birimi seçebilirsiniz.

 ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/bucatalog.png)

## <a name="considerations-for-cloning-a-volume"></a>Birim kopyalama konuları

Bir birim kopyalanırken aşağıdaki bilgileri göz önünde bulundurun.

- Bir kopya, normal birimle aynı şekilde davranır. Bir birimde mümkün olan herhangi bir işlem, kopya için kullanılabilir.

- Kopyalanmış bir birimde izleme ve varsayılan yedekleme otomatik olarak devre dışıdır. Tüm yedeklemeler için kopyalanmış bir birim yapılandırmanız gerekir.

- Yerel olarak sabitlenmiş bir birim katmanlı birim olarak klonlanır. Kopyalanmış birimin yerel olarak sabitlenmiş olması gerekiyorsa, kopyalama işlemi başarılı bir şekilde tamamlandıktan sonra kopyayı yerel olarak sabitlenmiş bir birime dönüştürebilirsiniz. Katmanlı bir birimi yerel olarak sabitlenmiş bir birime dönüştürme hakkında daha fazla bilgi için, [birim türünü değiştirme](storsimple-8000-manage-volumes-u2.md#change-the-volume-type)bölümüne gidin.

- Klonlanan bir birimi, kopyalamadan hemen sonra yerel olarak sabitlenmiş bir şekilde dönüştürmeye çalışırsanız (hala geçici bir kopya olduğunda), dönüştürme aşağıdaki hata iletisiyle başarısız olur:

    `Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.`

    Bu hata yalnızca farklı bir cihaza kopyalama yapıyorsanız alınır. Önce geçici kopyayı kalıcı bir kopyaya dönüştürürseniz, birimi yerel olarak sabitlenmiş olarak dönüştürebilirsiniz. Kalıcı bir kopyaya dönüştürmek için geçici kopyanın bir bulut anlık görüntüsünü alın.

## <a name="create-a-clone-of-a-volume"></a>Birimin kopyasını oluşturma

Aynı cihazda, başka bir cihazda, hatta yerel veya bulut anlık görüntüsü kullanarak bir bulut gerecinde bir kopya oluşturabilirsiniz.

Aşağıdaki yordamda, yedekleme kataloğundan bir kopyanın nasıl oluşturulacağı açıklanmaktadır.  Kopyalama işlemini başlatmak için alternatif bir yöntem **birimler**'e gidip bir birim seçin, sonra bağlam menüsünü çağırmak ve **Kopyala**' yı seçmek için sağ tıklayın.

Yedekleme kataloğundan biriminizin bir kopyasını oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-clone-a-volume"></a>Bir birimi kopyalamak için

1. StorSimple Aygıt Yöneticisi hizmetinize gidin ve ardından **Yedekleme kataloğu**' na tıklayın.

2. Aşağıdaki şekilde bir yedekleme kümesi seçin:
   
   1. Uygun aygıtı seçin.
   2. Açılan listede, seçmek istediğiniz yedekleme için birim veya yedekleme ilkesini seçin.
   3. Zaman aralığını belirtin.
   4. Bu sorguyu yürütmek için **Uygula** ' ya tıklayın.

      Seçilen birim veya yedekleme ilkesiyle ilişkili yedeklemeler, yedekleme kümeleri listesinde görünmelidir.
   
      ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/bucatalog.png)
     
3. İlişkili birimi görüntülemek için yedekleme kümesini genişletin ve bir yedekleme kümesindeki bir birimi seçin. Sağ tıklayıp bağlam menüsünden **Kopyala**' yı seçin.

   ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/clonevol3b.png) 

3. **Kopyala** dikey penceresinde aşağıdaki adımları uygulayın:
   
   1. Bir hedef cihaz belirler. Bu, kopyanın oluşturulacağı konumdur. Aynı cihazı seçebilir veya başka bir cihaz belirtebilirsiniz.

      > [!NOTE]
      > Kopya için gereken kapasitenin hedef cihazda bulunan kapasiteden düşük olduğundan emin olun.
       
   2. Klonlarınız için benzersiz bir birim adı belirtin. Ad 3 ila 127 karakter içermelidir.
      
       > [!NOTE]
       > Yerel olarak sabitlenmiş bir birim klonsanız bile, **birimi farklı Kopyala** alanı **katmanlı** olur. Bu ayarı değiştiremezsiniz; Ancak kopyalanmış birimin yerel olarak sabitlenmiş olması gerekiyorsa, kopyayı başarıyla oluşturduktan sonra kopyayı yerel olarak sabitlenmiş bir birime dönüştürebilirsiniz. Katmanlı bir birimi yerel olarak sabitlenmiş bir birime dönüştürme hakkında daha fazla bilgi için, [birim türünü değiştirme](storsimple-8000-manage-volumes-u2.md#change-the-volume-type)bölümüne gidin.
          
   3. **Bağlı konaklar**' ın altında, kopya için bir erişim denetimi kaydı (ACR) belirtin. Yeni bir ACR ekleyebilir veya mevcut listeden seçim yapabilirsiniz. ACR, hangi konağın bu kopyaya erişebileceğini tespit eder.
      
       ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/clonevol3a.png) 

   4. İşlemi gerçekleştirmek için **Kopyala** ' ya tıklayın.

4. Bir kopyalama işi başlatılır ve kopyalama başarıyla oluşturulduğunda size bildirilir. Kopyalama işini izlemek için iş bildirimine tıklayın veya **işler** dikey penceresine gidin.

    ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/clonevol5.png)

7. Kopyalama işi tamamlandıktan sonra cihazınıza gidin ve ardından **birimler**' e tıklayın. Birim listesinde, kaynak birime sahip aynı birim kapsayıcısında yeni oluşturulan kopyayı görmeniz gerekir.

    ![Yedekleme kümesi listesi](./media/storsimple-8000-clone-volume-u2/clonevol6.png)

Bu şekilde oluşturulan bir kopya, geçici bir kopyadır. Kopya türleri hakkında daha fazla bilgi için bkz. [geçici ve kalıcı kopyalar](#transient-vs-permanent-clones).


## <a name="transient-vs-permanent-clones"></a>Geçici ve kalıcı kopyalar
Geçici kopyalar yalnızca başka bir cihaza kopyaladığınızda oluşturulur. Belirli bir birimi bir yedekleme kümesinden, StorSimple Aygıt Yöneticisi tarafından yönetilen farklı bir cihaza kopyalayabilirsiniz. Geçici kopya, orijinal birimdeki verilere başvuru içerir ve bu verileri hedef cihazda yerel olarak okumak ve yazmak için kullanır.

Geçici bir kopyanın bulut anlık görüntüsünü aldıktan sonra, sonuçta elde edilen kopya *kalıcı* bir kopyadır. Bu işlem sırasında, verilerin bir kopyası bulutta oluşturulur ve bu verileri kopyalama süresi verilerin boyutuna ve Azure gecikmeleri 'na göre belirlenir (Bu bir Azure-Azure kopyasıdır). Bu işlem, haftalara kadar gün sürebilir. Geçici kopya kalıcı bir kopya haline gelir ve kopyalandığı orijinal birim verilerine başvuru içermez.

## <a name="scenarios-for-transient-and-permanent-clones"></a>Geçici ve kalıcı kopyalar için senaryolar
Aşağıdaki bölümlerde, geçici ve kalıcı klonların kullanılabileceği örnek durumlar açıklanır.

### <a name="item-level-recovery-with-a-transient-clone"></a>Geçici bir kopya ile öğe düzeyinde kurtarma
Bir yıllık eski Microsoft PowerPoint sunu dosyasını kurtarmanız gerekir. BT yöneticiniz bu zamandan belirli bir yedeklemeyi tanımlar ve ardından birimi filtreler. Yönetici daha sonra birimi klonlar, aradığınız dosyayı konumlandırır ve size sağlar. Bu senaryoda, geçici bir kopya kullanılır.

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Kalıcı bir kopya ile üretim ortamında test etme
Üretim ortamında bir test hatasını doğrulamanız gerekir. Üretim ortamında birimin bir kopyasını oluşturun ve sonra bağımsız kopyalanmış bir birim oluşturmak için bu kopyanın bir bulut anlık görüntüsünü alın. Bu senaryoda kalıcı bir kopya kullanılır.

## <a name="next-steps"></a>Sonraki adımlar
* Bir [StorSimple birimini bir yedekleme kümesinden geri yüklemeyi](storsimple-8000-restore-from-backup-set-u2.md)öğrenin.
* StorSimple [cihazınızı yönetmek Için storsimple Aygıt Yöneticisi hizmetini nasıl kullanacağınızı](storsimple-8000-manager-service-administration.md)öğrenin.

