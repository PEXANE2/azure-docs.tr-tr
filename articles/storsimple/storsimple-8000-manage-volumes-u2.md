---
title: StorSimple birimlerini yönetme (Güncelleştirme 3)
description: StorSimple birimlerinin nasıl ekleyeceğini, değiştirin, izlendiğini ve silineceklerini ve gerekirse nasıl çevrimdışı hale getirilenleri açıklar.
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 12/08/2017
ms.author: alkohli
ms.openlocfilehash: f32f8925bca33d90afa48071d0c0944ba63861cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254774"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-volumes-update-3-or-later"></a>Birimleri yönetmek için StorSimple Device Manager hizmetini kullanın (3 veya sonraki güncelleştirmeyi güncelleştirin)

## <a name="overview"></a>Genel Bakış

Bu öğretici, 3 ve sonraki güncelleştirmeleri çalıştıran StorSimple 8000 serisi cihazlarda hacim oluşturmak ve yönetmek için StorSimple Device Manager hizmetini nasıl kullanacağız açıklanmaktadır.

StorSimple Device Manager hizmeti, Azure portalında, StorSimple çözümünüzü tek bir web arabiriminden yönetmenize olanak tanıyan bir uzantıdır. Tüm aygıtlarınızdaki birimleri yönetmek için Azure portalını kullanın. Ayrıca StorSimple hizmetlerini oluşturabilir ve yönetebilir, aygıtları, yedekleme ilkelerini ve yedekleme kataloğunu yönetebilir ve uyarıları görüntüleyebilirsiniz.

## <a name="volume-types"></a>Birim türleri

StorBasit hacimler:

* **Yerel olarak sabitlenmiş birimler**: Bu birimlerdeki veriler her zaman yerel StorSimple aygıtında kalır.
* **Katmanlı birimler**: Bu birimlerdeki veriler buluta dökülebilir.

Arşiv hacmi katmanlı hacim türüdür. Arşiv hacimleri için kullanılan daha büyük çoğaltma yığın boyutu, aygıtın daha büyük veri segmentlerini buluta aktarmasına olanak tanır.

Gerekirse, birim türünü yerelden katmanlıya veya katmanlıdan yerele değiştirebilirsiniz. Daha fazla bilgi için [ses düzeyini değiştir'e](#change-the-volume-type)gidin.

### <a name="locally-pinned-volumes"></a>Yerel olarak sabitlenmiş birimler

Yerel olarak sabitlenmiş birimler, verileri buluta katmanlamayan ve böylece bulut bağlantısından bağımsız birincil veriler için yerel garantiler sağlayan tam olarak sağlanan birimlerdir. Yerel olarak sabitlenmiş birimlere ait veriler çoğaltılıp sıkıştırılmadı; ancak, yerel olarak sabitlenmiş birimlerin anlık görüntüleri deduplicated. 

Yerel olarak sabitlenmiş hacimler tam olarak karşılanır; bu nedenle, bunları oluştururken cihazınızda yeterli alana sahip olmalısınız. StorSimple 8100 aygıtında en fazla 8 TB boyutuna ve 8600 aygıtına 20 TB'ye kadar yerel olarak sabitlenmiş hacimler sağlayabilirsiniz. StorSimple anlık görüntüler, meta veriler ve veri işleme için aygıtta kalan yerel alanı ayırır. Yerel olarak sabitlenmiş bir birimin boyutunu kullanılabilir maksimum alana yükseltebilirsiniz, ancak oluşturulduktan sonra bir birimin boyutunu azaltamazsınız.

Yerel olarak sabitlenmiş bir birim oluşturduğunuzda, katmanlı birimlerin oluşturulması için kullanılabilir alan azalır. Bunun tersi de doğrudur: varolan katmanlı birimlerinüz varsa, yerel olarak sabitlenmiş birimler oluşturmak için kullanılabilen alan yukarıda belirtilen maksimum sınırlardan daha düşük olacaktır. Yerel birimler hakkında daha fazla bilgi için, [yerel olarak sabitlenmiş ciltler hakkında sık sorulan sorulara](storsimple-8000-local-volume-faq.md)bakın.

### <a name="tiered-volumes"></a>Katmanlı birimler

Katmanlı birimler, sık erişilen verilerin aygıtta yerel kaldığı ve daha az kullanılan verilerin otomatik olarak buluta katmanlandığı ince olarak sağlanan birimlerdir. İnce sağlama, kullanılabilir depolamanın fiziksel kaynakları aştığı bir sanallaştırma teknolojisidir. StorSimple, yeterli depolama alanını önceden ayırmak yerine, geçerli gereksinimleri karşılamak için yeterli alan ayırmak için ince sağlama kullanır. StorSimple değişen talepleri karşılamak için bulut depolamayı artırabilir veya azaltabilir, çünkü bulut depolamanın esnek yapısı bu yaklaşımı kolaylaştırır.

Arşiv verileri için katmanlı birim kullanıyorsanız, hacminiziçin çoğaltma yığın boyutunu 512 KB olarak değiştirmek **için daha az sıklıkta erişilen arşiv veri** onay kutusunu kullanmak için bu birimi kullanın'ı seçin. Bu seçeneği seçmezseniz, karşılık gelen katmanlı birim 64 KB'lik bir yığın boyutu kullanır. Daha büyük bir yinelenenleri kaldırma öbek boyutu cihazın büyük arşiv verilerinin buluta aktarımını hızlandırmasını sağlar.


### <a name="provisioned-capacity"></a>Sağlanan kapasite

Her aygıt ve ses türü için maksimum sağlanan kapasite için aşağıdaki tabloya bakın. (Yerel olarak sabitlenmiş birimlerin sanal bir aygıtta kullanılamadığını unutmayın.)

|  | Maksimum katmanlı hacim boyutu | Yerel olarak sabitlenmiş maksimum birim boyutu |
| --- | --- | --- |
| **Fiziksel cihazlar** | | |
| 8100 |64 TB |8 TB |
| 8600 |64 TB |20 TB |
| **Sanal cihazlar** | | |
| 8010 |30 TB |Yok |
| 8020 |64 TB |Yok |

## <a name="the-volumes-blade"></a>Hacimler bıçak

**Birimler** bıçak başlatıcıları (sunucular) için Microsoft Azure StorSimple aygıtında sağlanan depolama birimleri yönetmenize olanak sağlar. Hizmetinize bağlı StorSimple aygıtlarında birimlerin listesini görüntüler.

 ![Birimler sayfası](./media/storsimple-8000-manage-volumes-u2/volumeslist.png)

Bir birim bir dizi özniteliklerden oluşur:

* **Birim Adı** – Benzersiz olması gereken ve birimin tanımlanmasına yardımcı olan açıklayıcı bir ad. Bu ad, belirli bir birimde filtre lediğinizde raporları izlemede de kullanılır. Birim oluşturulduktan sonra, yeniden adlandırılamaz.
* **Durum** – Çevrimiçi veya çevrimdışı olabilir. Bir birim çevrimdışıysa, birimde erişime izin verilen başlatıcılar (sunucular) tarafından görülemez.
* **Kapasite** – başlatıcı (sunucu) tarafından depolanabilecek toplam veri miktarını belirtir. Yerel olarak sabitlenmiş hacimler tamamen karşılanır ve StorSimple aygıtında yer alır. Katmanlı birimler ince olarak karşılanır ve veriler çoğaltılır. İnce olarak sağlanan birimlersayesinde cihazınız, fiziksel depolama kapasitesini yapılandırılmış hacim kapasitesine göre dahili olarak veya bulut üzerinde önceden tahsis etmez. Hacim kapasitesi isteğe bağlı olarak tahsis edilir ve tüketilir.
* **Tür** – Birimin **Katmanlı** (varsayılan) veya **Yerel olarak sabitlenip sabitlenmediğini**gösterir.

Aşağıdaki görevleri gerçekleştirmek için bu öğreticideki yönergeleri kullanın:

* Ses düzeyi ekleme 
* Birim değiştirme 
* Birim türünü değiştirme
* Bir birim silme 
* Bir birimi çevrimdışı alma 
* Bir sesi izleme 

## <a name="add-a-volume"></a>Ses düzeyi ekleme

StorSimple 8000 serisi cihazınızın dağıtımı sırasında [bir birim oluşturdunuz.](storsimple-8000-deployment-walkthrough-u2.md#step-6-create-a-volume) Birim eklemek de benzer bir yordamdır.

#### <a name="to-add-a-volume"></a>Birim eklemek için

1. **Cihazlar** dikey penceresindeki tablosal cihaz listesinden cihazınızı seçin. **+ Birim ekle**’ye tıklayın.

    ![Yeni birim ekleme](./media/storsimple-8000-manage-volumes-u2/step5createvol1.png)

2. **Birim ekle** dikey penceresinde:
   
    1. **Cihazı seçin** alanı otomatik olarak geçerli cihazınızla doldurulur.

    2. Açılan listeden, birim eklemeniz gereken birim kapsayıcısını seçin.

    3.  Biriminiz için bir **Ad** yazın. Birim oluşturulduktan sonra, birimi yeniden adlandıramazsınız.

    4. Açılan listede biriminizin **Tür**’ünü seçin. Yerel garantilerin, düşük gecikme sürelerinin ve yüksek performansın gerektiği iş yükleri için **Yerel olarak sabitlenmiş** bir birim seçin. Diğer tüm veriler için **Katmanlı** bir birim seçin. Bu birimi arşiv verileri için kullanıyorsanız **Bu birimi daha az sıklıkta erişilen arşiv verileri için kullanın**’ı işaretleyin.
      
       Katmanlı birim ölçülü sayıda kaynak kullanarak sağlanır ve hızlı oluşturulabilir. Arşiv verileri için hedeflenen katmanlı birime yönelik olarak **Bu birimi daha az sıklıkta erişilen arşiv verileri için kullan**’ın seçilmesi, biriminizle ilgili yinelenenleri kaldırma öbek boyutunu 512 KB olarak değiştirir. Bu alan işaretli değilse, ilgili katmanlı birim 64 KB öbek boyutu kullanır. Daha büyük bir yinelenenleri kaldırma öbek boyutu cihazın büyük arşiv verilerinin buluta aktarımını hızlandırmasını sağlar.
       
       Yerel olarak sabitlenmiş bir birim sıkı hazırlanmıştır ve birimdeki birincil verilerin cihazda yerel kalmasını ve buluta dağıtılmamasını sağlar.  Yerel olarak sabitlenmiş bir birim oluşturursanız cihaz, istenen boyutta birimi sağlamak için yerel katmanlardaki kullanılabilir alanı denetler. Yerel olarak sabitlenmiş birim oluşturma işlemi var olan verileri cihazdan buluta dağılmasını kapsayabilir ve birim oluşturmak için geçen süre uzun olabilir. Toplam süre hazırlanan birimin boyutuna, kullanılabilir ağ bant genişliğine ve cihazınızdaki verilere bağlıdır.

    5. Biriminiz için **Sağlanan Kapasite**’yi belirtin. Seçili birim türünü temel alan kullanılabilir kapasiteyi not edin. Belirtilen birim boyutu kullanılabilir alanı aşmamalıdır.
      
       8100 cihazında 8,5 TB'a kadar yerel olarak sabitlenmiş birim ya da 200 TB'a kadar katmanlı birim sağlayabilirsiniz. Daha büyük olan 8600 cihazında 22,5 TB'a kadar yerel olarak sabitlenmiş birim ya da 500 TB'a kadar katmanlı birim sağlayabilirsiniz. Katmanlı birimlerin çalışma kümesini barındırmak için cihazda yerel alan gerektiğinden, yerel olarak sabitlenmiş birimlerin oluşturulması, katmanlı birimlerin sağlanması için kullanılabilecek alanı etkiler. Bu nedenle, yerel olarak sabitlenmiş birim oluşturursanız, katmanlı birimlerin oluşturulması için gereken boş alan azalır. Benzer şekilde, katmanlı birim oluşturulduysa, yerel olarak sabitlenmiş birimlerin oluşturulması için kullanılabilir alan azalır.
      
       8100 cihazınızda 8,5 TB boyutunda (izin verilen en yüksek boyut) yerel olarak sabitlenmiş bir birim sağlarsanız, cihazdaki kullanılabilir yerel alanın tümünü kullanmış olursunuz. Katmanlı birimin çalışan kümesinin barındıracak cihazda yerel alan olmadığından, bu noktadan sonra herhangi bir katmanlı birim oluşturamazsınız. Var olan katmanlı birimler kullanılabilir alanı de etkiler. Örneğin, zaten 106 TB boyutunda katmanlı birimlerin bulunduğu bir 8100 cihazınız varsa, yerel olarak sabitlenmiş birimlerin kullanabileceği yalnızca 4 TB’lık alan kalır.

    6. **Bağlı konaklar** alanında oka tıklayın. Bağlı **ana bilgisayar** bıçaklarında, varolan bir ACR seçin veya yeni bir ACR ekleyin. Yeni bir ACR seçerseniz, ACR'niz için bir **Ad** sağlayın, Windows ana bilgisayarınızın **iSCSI Kalifiye Adı'nı** (IQN) sağlayın. IQN’niz yoksa Windows Server konağının IQN’sini al’a gidin. **Oluştur'u**tıklatın. Belirtilen ayarlarla bir birim oluşturulur.

        ![Oluştur’a tıklayın](./media/storsimple-8000-manage-volumes-u2/step5createvol3.png)

Yeni cildin artık kullanıma hazır.

> [!NOTE]
> Yerel olarak sabitlenmiş bir birim oluşturur ve hemen ardından yerel olarak sabitlenmiş başka bir birim oluşturursanız, birim oluşturma işleri sırayla çalışır. İlk birim oluşturma işi, bir sonraki birim oluşturma işi başlamadan önce bitirmelidir.

## <a name="modify-a-volume"></a>Birim değiştirme

Bir birimi genişletmeniz gerektiğinde veya ses birimine erişen ana bilgisayarları değiştirmeniz gerektiğinde değiştirin.

> [!IMPORTANT]
> * Aygıttaki ses boyutunu değiştirirseniz, ses boyutunun ana bilgisayarda da değiştirilmesi gerekir.
> * Burada açıklanan ana bilgisayar adımları Windows Server 2012 (2012R2) içindir. Linux veya diğer ana bilgisayar işletim sistemleri için prosedürler farklı olacaktır. Başka bir işletim sistemi çalıştıran bir ana bilgisayardaki ses düzeyini değiştirirken ana bilgisayar işletim sistemi yönergelerinize bakın.

#### <a name="to-modify-a-volume"></a>Bir birimi değiştirmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Aygıtların tabular listesinden, değiştirmek istediğiniz ses düzeyine sahip aygıtı seçin. **Ayarlar > Birimleri'ni**tıklatın.

    ![Volumes blade'e git](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

2. Birimlerin tabular listesinden, bağlam menüsünü çağırmak için birimi ve sağ tıklatmayı seçin. Çevrimdışı değiştireceğiniz birimi almak için **çevrimdışı al'ı** seçin.

    ![Ses düzeyini seçme ve çevrimdışına alma](./media/storsimple-8000-manage-volumes-u2/modifyvol4.png)

3. Çevrimdışı **Al** bıçağında, sesi çevrimdışı almanın etkisini gözden geçirin ve ilgili onay kutusunu seçin. Önce ana bilgisayardaki ilgili birimin çevrimdışı olduğundan emin olun. StorSimple'a bağlı ana bilgisayar sunucunuzda bir birimin çevrimdışı nasıl alınacağına ilişkin bilgi için işletim sistemine özgü yönergelere bakın. **Çevrimdışı Kaldır'ı**tıklatın.

    ![Sesi çevrimdışı namına almanın etkisini gözden geçirme](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)

4. Birim çevrimdışı olduktan sonra (birim durumuna göre gösterildiği gibi), birim ve bağlam menüsünü çağırmak için sağ tıklatın seçin. **Birimi Değiştir'i**seçin.

    ![Birimi değiştir'i seçin](./media/storsimple-8000-manage-volumes-u2/modifyvol9.png)


5. Ses **düzeyini değiştir** bıçağında aşağıdaki değişiklikleri yapabilirsiniz:
   
   1. Birim **Adı** düzenlenemez.
   2. **Türü** yerel olarak sabitlenmiş ten katmanlı ya da katmanlı dan yerel olarak sabitlenmiş olarak dönüştürün (daha fazla bilgi için [ses türünü değiştir'e](#change-the-volume-type) bakınız).
   3. Sağlanan **Kapasiteyi Artırın.** **Sağlanan Kapasite** yalnızca artırılabilir. Bir birim oluşturulduktan sonra küçültemezsiniz.
   4. **Bağlı ana bilgisayarlar**altında, ACR'yi değiştirebilirsiniz. Bir ACR'yi değiştirmek için ses düzeyi çevrimdışı olmalıdır.

       ![Sesi çevrimdışı namına almanın etkisini gözden geçirme](./media/storsimple-8000-manage-volumes-u2/modifyvol11.png)

5. Değişikliklerinizi kaydetmek için **Kaydet'i** tıklatın. Onayınız istendiğinde **Evet**’e tıklayın. Azure portalı bir güncelleştirme hacmi iletisi görüntüler. Birim başarıyla güncelleştirildiğinde bir başarı iletisi görüntüler.

    ![Sesi çevrimdışı namına almanın etkisini gözden geçirme](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)

7. Bir birim genişletiyorsanız, Windows ana bilgisayarınızdaki aşağıdaki adımları tamamlayın:
   
   1. Bilgisayar **Yönetimi** ->**Disk Yönetimi'ne**gidin.
   2. Sağ tıklayın **Disk Yönetimi** ve **Rescan Diskler**seçin.
   3. Diskler listesinde, güncellediğiniz birimi, sağ tıklatma nızı ve ardından **Birimi Genişlet'i**seçin. Ses Düzeyini Genişlet sihirbazı başlar. **İleri**'ye tıklayın.
   4. Varsayılan değerleri kabul eden sihirbazı tamamlayın. Sihirbaz tamamlandıktan sonra, ses düzeyi artan boyutu göstermelidir.
      
      > [!NOTE]
      > Yerel olarak sabitlenmiş bir birimi genişletir ve hemen ardından yerel olarak sabitlenmiş başka bir birimi genişletirseniz, birim genişletme işleri sırayla çalışır. Bir sonraki birim genişletme işi başlamadan önce ilk birim genişletme işi bitirilmelidir.
      

## <a name="change-the-volume-type"></a>Birim türünü değiştirme

Birim türünü katmanlıdan yerel olarak sabitlenmiş veya yerel olarak sabitlenmiş ten katmanlı ya da katmanlı olarak değiştirebilirsiniz. Ancak, bu dönüşüm sık karşılaşılan bir durum olmamalıdır.

### <a name="tiered-to-local-volume-conversion-considerations"></a>Yerel hacim dönüştürme hususlarına katmanlı

Bir birimi katmanlıdan yerel olarak sabitlenmiş olarak dönüştürmenin bazı nedenleri şunlardır:

* Veri kullanılabilirliği ve performansı ile ilgili yerel garantiler
* Bulut gecikmelerinin ve bulut bağlantısı sorunlarının ortadan kaldırılması.

Genellikle, bunlar sık erişmek istediğiniz küçük varolan birimlerdir. Yerel olarak sabitlenmiş bir birim oluşturulduğunda tam olarak karşılanır. 

Katmanlı bir birimi yerel olarak sabitlenmiş bir birime dönüştürüyorsanız, StorSimple dönüştürmeye başlamadan önce cihazınızda yeterli alana sahip olduğunuzu doğrular. Yeterli alanınız yoksa, bir hata alırsınız ve işlem iptal edilir. 

> [!NOTE]
> Katmanlıdan yerel olarak sabitlenmiş bir dönüşüme başlamadan önce, diğer iş yüklerinizin alan gereksinimlerini göz önünde bulundurun. 

Katmanlı bir birimden yerel olarak sabitlenmiş bir ses e dönüştürme, aygıt performansını olumsuz etkileyebilir. Ayrıca, aşağıdaki etkenler dönüşümü tamamlamak için gereken süreyi artırabilir:

* Yetersiz bant genişliği vardır.
* Geçerli yedekleme yok.

Bu faktörlerin sahip olabileceği etkileri en aza indirmek için:

* Bant genişliği azaltma ilkelerinizi gözden geçirin ve özel bir 40 Mbps bant genişliğinin kullanılabilir olduğundan emin olun.
* Dönüşüm'ün yoğun olmayan saatler için zamanlanın.
* Dönüştürmeye başlamadan önce bir bulut anlık görüntüsü alın.

Birden çok birimi dönüştürüyorsanız (farklı iş yüklerini destekliyorsanız), önce daha yüksek öncelik birimlerinin dönüştürülmesi için birim dönüştürmeye öncelik vermelisiniz. Örneğin, dosya paylaşımı iş yükleriyle birimleri dönüştürmeden önce sanal makineleri (VM'ler) veya SQL iş yüklerine sahip birimleri barındıran birimleri dönüştürmeniz gerekir.

### <a name="local-to-tiered-volume-conversion-considerations"></a>Yerelden katmanlı hacim dönüştürme hususları

Diğer birimleri sağlamak için ek alana ihtiyacınız varsa, yerel olarak sabitlenmiş bir birimi katmanlı bir birim olarak değiştirmek isteyebilirsiniz. Yerel olarak sabitlenmiş birimi katmanlı ya dönüştürdüğünüzde, aygıttaki kullanılabilir kapasite serbest bırakılan kapasitenin boyutuna göre artar. Bağlantı sorunları, bir birimin yerel türden katmanlı türe dönüştürülmesini engelliyorsa, yerel birim dönüştürme tamamlanana kadar katmanlı bir birimin özelliklerini sergiler. Bunun nedeni, bazı verilerin buluta dökülmüş olmasıdır. Bu dökülen veriler, işlem yeniden başlatılAna ve tamamlanana kadar serbest bırakılamayan aygıtta yerel alanı işgal etmeye devam ediyor.

> [!NOTE]
> Bir birimi dönüştürmebiraz zaman alabilir ve başladıktan sonra bir dönüşümü iptal edemezsiniz. Birim dönüşüm sırasında çevrimiçi kalır ve yedekleme alabilirsiniz, ancak dönüşüm gerçekleşirken birimi genişletemezsiniz veya geri yükleyemezsiniz.


#### <a name="to-change-the-volume-type"></a>Birim türünü değiştirmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Aygıtların tabular listesinden, değiştirmek istediğiniz ses düzeyine sahip aygıtı seçin. **Ayarlar > Birimleri'ni**tıklatın.

    ![Volumes blade'e git](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

3. Birimlerin tabular listesinden, bağlam menüsünü çağırmak için birimi ve sağ tıklatmayı seçin. **Değiştir'i**seçin.

    ![Bağlam menüsünden değiştir'i seçin](./media/storsimple-8000-manage-volumes-u2/changevoltype2.png)

4. Ses **düzeyini** Değiştir'de, **Tür** açılır listesinden yeni türü seçerek ses türünü değiştirin.
   
   * Türü **Yerel olarak sabitlenmiş**olarak değiştiriyorsanız, StorSimple yeterli kapasite olup olmadığını kontrol edecektir.
   * Türü **Katmanlı** olarak değiştiriyorsanız ve bu birim arşiv verileri için kullanılacaksa, **daha az sıklıkta erişilen arşiv veri** onay kutusunu kullanmak için bu birimi kullanın'ı seçin.
   * Yerel olarak sabitlenmiş bir birimi katmanlı veya _tam tersi_olarak yapılandırıyorsanız, aşağıdaki ileti görüntülenir.
   
     ![Birim türü iletisini değiştirme](./media/storsimple-8000-manage-volumes-u2/changevoltype3.png)

7. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın. Onay istendiğinde, dönüşüm işlemini başlatmak için **Evet'i** tıklatın. 

    ![Kaydet ve onayla](./media/storsimple-8000-manage-volumes-u2/modifyvol11.png)

8. Azure portalı, iş oluşturma birimi için sesi güncelleştirecek bir bildirim görüntüler. Birim dönüştürme işinin durumunu izlemek için bildirime tıklayın.

    ![Birim dönüştürme işi](./media/storsimple-8000-manage-volumes-u2/changevoltype5.png)

## <a name="take-a-volume-offline"></a>Bir birimi çevrimdışı alma

Birimi değiştirmeyi veya silmeyi planlarken bir birimi çevrimdışı almanız gerekebilir. Bir birim çevrimdışı olduğunda, okuma yazma erişimi için kullanılamaz. Ana bilgisayar ve aygıttaki ses düzeyini çevrimdışına almanız gerekir.

#### <a name="to-take-a-volume-offline"></a>Bir birimi çevrimdışı almak için

1. Çevrimdışı olmadan önce söz konusu birimin kullanılmadığından emin olun.
2. Önce ana bilgisayardaki ses düzeyini çevrimdışına alın. Bu, birimdeki veri bozulması riskini ortadan kaldırır. Belirli adımlar için, ana bilgisayar işletim sisteminizin yönergelerine bakın.
3. Ana bilgisayar çevrimdışı olduktan sonra, aşağıdaki adımları gerçekleştirerek aygıttaki ses düzeyini çevrimdışı na alın:
   
    1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Aygıtların tabular listesinden, değiştirmek istediğiniz ses düzeyine sahip aygıtı seçin. **Ayarlar > Birimleri'ni**tıklatın.

        ![Volumes blade'e git](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

    2. Birimlerin tabular listesinden, bağlam menüsünü çağırmak için birimi ve sağ tıklatmayı seçin. Çevrimdışı değiştireceğiniz birimi almak için **çevrimdışı al'ı** seçin.

        ![Ses düzeyini seçme ve çevrimdışına alma](./media/storsimple-8000-manage-volumes-u2/modifyvol4.png)

3. Çevrimdışı **Al** bıçağında, sesi çevrimdışı almanın etkisini gözden geçirin ve ilgili onay kutusunu seçin. **Çevrimdışı Kaldır'ı**tıklatın. 

    ![Sesi çevrimdışı namına almanın etkisini gözden geçirme](./media/storsimple-8000-manage-volumes-u2/modifyvol5.png)
      
      Birim çevrimdışı olduğunda size bildirilir. Birim durumu çevrimdışı için de güncellenir.
      
4. Bir birim çevrimdışı olduktan sonra, ses düzeyini ve sağ tıklatma seçeneğini belirlerseniz, **Çevrimiçi Getir** seçeneği bağlam menüsünde kullanılabilir hale gelir.

> [!NOTE]
> **Çevrimdışı Kaldır** komutu, ses düzeyini çevrimdışına almak için aygıta bir istek gönderir. Ana bilgisayarlar hala ses düzeyini kullanıyorsa, bu kırık bağlantılara neden olur, ancak birimi çevrimdışı almak başarısız olmaz.

## <a name="delete-a-volume"></a>Bir birim silme

> [!IMPORTANT]
> Bir birimi yalnızca çevrimdışıysa silebilirsiniz.

Bir birimi silmek için aşağıdaki adımları tamamlayın.

#### <a name="to-delete-a-volume"></a>Bir birimi silmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Aygıtların tabular listesinden, değiştirmek istediğiniz ses düzeyine sahip aygıtı seçin. **Ayarlar > Birimleri'ni**tıklatın.

    ![Volumes blade'e git](./media/storsimple-8000-manage-volumes-u2/modifyvol2.png)

3. Silmek istediğiniz birimin durumunu denetleyin. Silmek istediğiniz birim çevrimdışı değilse, önce çevrimdışı alın. Birim çevrimdışı [olarak ses düzeyini al'daki](#take-a-volume-offline)adımları izleyin.
4. Birim çevrimdışı olduktan sonra, birim seçin, bağlam menüsünü çağırmak için sağ tıklatın ve sonra **Sil'i**seçin.

    ![Bağlam menüsünden sil'i seçin](./media/storsimple-8000-manage-volumes-u2/deletevol1.png)

5. **Sil** bıçağında, bir birimi silmenin etkisine karşı onay kutusunu gözden geçirin ve seçin. Bir birimi sildiğinizde, birimde bulunan tüm veriler kaybolur. 

    ![Değişiklikleri kaydetme ve onaylama](./media/storsimple-8000-manage-volumes-u2/deletevol2.png)

6. Birim silindikten sonra, silme işlemini belirtmek için birim güncelleştirmelerinin tabular listesi.

    ![Güncelleştirilmiş ses listesi](./media/storsimple-8000-manage-volumes-u2/deletevol3.png)
   
   > [!NOTE]
   > Yerel olarak sabitlenmiş bir birimi silerseniz, yeni birimler için kullanılabilir alan hemen güncelleştirilemeyebilir. StorSimple Aygıt Yöneticisi Hizmeti, düzenli olarak kullanılabilen yerel alanı güncelleştirir. Yeni birim oluşturmaya çalışmadan önce birkaç dakika beklemenizi öneririz.
   >
   > Ayrıca, yerel olarak sabitlenmiş bir birimi siler seniz ve hemen ardından yerel olarak sabitlenmiş başka bir birimi silerseniz, birim silme işleri sırayla çalışır. Bir sonraki birim silme işi başlamadan önce ilk birim silme işi bitmelidir.

## <a name="monitor-a-volume"></a>Bir sesi izleme

Birim izleme, bir birim için G/Ç ile ilgili istatistikleri toplamanızı sağlar. İzleme, oluşturduğunuz ilk 32 birim için varsayılan olarak etkinleştirilir. Ek birimlerin izlenmesi varsayılan olarak devre dışı bırakılır. 

> [!NOTE]
> Klonlanan birimlerin izlenmesi varsayılan olarak devre dışı bırakılır.


Bir birim için izlemeyi etkinleştirmek veya devre dışı katmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-or-disable-volume-monitoring"></a>Birim izlemeyi etkinleştirmek veya devre dışı kılabilir

1. StorSimple Cihaz Yöneticisi hizmetinize gidin ve **Cihazlar**’a tıklayın. Aygıtların tabular listesinden, değiştirmek istediğiniz ses düzeyine sahip aygıtı seçin. **Ayarlar > Birimleri'ni**tıklatın.
2. Birimlerin tabular listesinden, bağlam menüsünü çağırmak için birimi ve sağ tıklatmayı seçin. **Değiştir'i**seçin.
3. Ses **düzeyini Değiştir** bıçağında, **izlemeyi etkinleştirmek** veya devre dışı kalmak için **Etkinleştir'i** veya devre **dışı kılmasını** seçin.

    ![İzlemeyi devre dışı bırakma](./media/storsimple-8000-manage-volumes-u2/monitorvol1.png) 

4. **Kaydet'i** tıklatın ve onay istendiğinde **Evet'i**tıklatın. Azure portalı, birim başarıyla güncelleştirildikten sonra sesi ve ardından başarı iletisini güncelleştirmek için bir bildirim görüntüler.

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple birimini nasıl klonlamayı](storsimple-8000-clone-volume-u2.md)öğrenin.
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini](storsimple-8000-manager-service-administration.md)nasıl kullanacağınızı öğrenin.

