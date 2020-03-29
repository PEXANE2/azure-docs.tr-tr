---
title: StorSimple Virtual Array yedeklemesini klonlayın | Microsoft Dokümanlar
description: Bir yedeklemeyi nasıl klonlayacağını ve StorSimple Virtual Array' inizden bir dosyayi kurtarmayı öğrenin.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: af6e979c-55e3-477c-b53e-a76a697f80c9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: feffbb634af62d70a840febcf2a04afb7bdeeddd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60580937"
---
# <a name="clone-from-a-backup-of-your-storsimple-virtual-array"></a>StorSimple Virtual Array'inizin bir yedeğinden klonlama

## <a name="overview"></a>Genel Bakış

Bu makalede, Microsoft Azure StorBasit Sanal Dizinizde paylaşımlarınızın veya birimlerinizin yedek kümesinin nasıl klonlanması adım adım açıklanmaktadır. Klonlanmış yedekleme, silinmiş veya kaybolan bir dosyayı kurtarmak için kullanılır. Makale ayrıca, dosya sunucusu olarak yapılandırılan StorSimple Virtual Array'inizde öğe düzeyinde kurtarma gerçekleştirmek için ayrıntılı adımlar da içerir.

## <a name="clone-shares-from-a-backup-set"></a>Yedekleme kümesinde kopya paylaşımları

**Paylaşımları klonlamaya çalışmadan önce, bu işlemi tamamlamak için aygıtta yeterli alana sahip olduğundan emin olun.** [Azure portalında](https://portal.azure.com/)bir yedeklemeden klonlamak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-clone-a-share"></a>Bir payı klonlamak için

1. **Cihazlar** a.ş.'ye göz atın. Cihazınızı seçin ve tıklatın ve sonra **Paylaşımlar'ı**tıklatın. Klonlamak istediğiniz paylaşımı seçin, bağlam menüsünü çağırmak için paylaşıma sağ tıklayın. **Klon'u**seçin.
   
   ![Yedekleme yi klonlama](./media/storsimple-virtual-array-clone/cloneshare1.png)
2. **Klon** **bıçağında, Yedekleme > Seçin'i** tıklatın ve ardından aşağıdakileri yapın: 
   
   a.    Bu aygıttaki bir yedeklemeyi zaman aralığına göre filtreleyin. Son 7 **gün, Son** **30 gün**ve Geçmiş **yıl**arasından seçim yapabilirsiniz.
   
   b.    Görüntülenen filtre uygulanmış yedeklemeler listesinde, klonlamak için bir yedekleme seçin.
   
   c.    **Tamam**'a tıklayın.
   
   ![Yedekleme yi klonlama](./media/storsimple-virtual-array-clone/cloneshare3.png)
3. **Klon** bıçağında Hedef **ayarları'nı** tıklatın ve ardından aşağıdakileri yapın:
   
   a.    Bir paylaşım adı sağlayın. Paylaşım adı 3-127 karakter içermelidir.
   
   b.    İsteğe bağlı olarak klonlanan pay için bir açıklama sağlayın.
   
   c.    Geri aldığınız payın türünü değiştiremezsiniz. Katmanlı bir pay katmanlı olarak klonlanır ve yerel olarak sabitlenmiş bir pay yerel olarak sabitlenir.
   
   d.    Kapasite, klonlamakta olduğunuz payın boyutuna eşit olarak ayarlanır.
   
   e.    Bu paylaşım için yöneticileri atayın. Klon tamamlandıktan sonra Dosya Gezgini aracılığıyla paylaşım özelliklerini değiştirebilirsiniz.
   
   f.    **Tamam**'a tıklayın.
   
   ![Yedekleme yi klonlama](./media/storsimple-virtual-array-clone/cloneshare6.png)

4. Klon işi başlatmak için **Clone'u** tıklatın. İş tamamlandıktan sonra klon işlemi başlar ve size bildirilir. Klonlamanın ilerlemesini izlemek için **İşler** bıçağına gidin ve iş ayrıntılarını görüntülemek için işi tıklatın.
5. Klon başarıyla oluşturulduktan sonra cihazınızdaki **Paylaşımlar** bıçağına geri gidin.
6. Artık cihazınızdaki paylaşımlar listesinde ki yeni klonlanmış payı görüntüleyebilirsiniz. Katmanlı bir pay katmanlı olarak klonlanır ve yerel olarak sabitlenmiş bir pay yerel olarak sabitlenmiş bir pay olarak.
   
   ![Yedekleme yi klonlama](./media/storsimple-virtual-array-clone/cloneshare10.png)

## <a name="clone-volumes-from-a-backup-set"></a>Yedekleme kümesinden kopya birimleri

Azure portalında bir yedeklemeden klonlamak için, bir paylaşımı klonlarken kine benzer adımlar gerçekleştirmeniz gerekir. Klon işlemi, yedeklemeyi aynı sanal aygıtta yeni bir ses düzeyine klonlar; farklı bir aygıta klonlayamazsınız.

#### <a name="to-clone-a-volume"></a>Bir birimi klonlamak için

1. **Cihazlar** a.ş.'ye göz atın. Cihazınızı seçin ve tıklatın ve sonra **Birimler'i**tıklatın. Klonlamak istediğiniz birimi seçin, bağlam menüsünü çağırmak için birimi sağ tıklatın. **Klon'u**seçin.
   
   ![Bir birimi kopyalama](./media/storsimple-virtual-array-clone/clonevolume1.png)
2. **Klon** bıçağında **Yedekleme'yi** tıklatın ve ardından aşağıdakileri yapın: 
   
   a.    Bu aygıttaki bir yedeklemeyi zaman aralığına göre filtreleyin. Son 7 **gün, Son** **30 gün**ve Geçmiş **yıl**arasından seçim yapabilirsiniz. 
   
   b.    Görüntülenen filtre uygulanmış yedeklemeler listesinde, klonlamak için bir yedekleme seçin.
   
   c.    **Tamam**'a tıklayın.
   
   ![Yedekleme yi klonlama](./media/storsimple-virtual-array-clone/clonevolume3.png)
3. **Klon** bıçağında, **Hedef ses ayarlarını** tıklatın ve ardından aşağıdakileri yapın:
   
   a. Aygıt adı otomatik olarak doldurulur.
   
   b. **Klonlanan birim**için bir birim adı sağlayın. Birim adı 3 ila 127 karakter içermelidir.
   
   c. Birim türü otomatik olarak özgün ses birimine ayarlanır. Katmanlı bir birim katmanlı olarak klonlanır ve yerel olarak sabitlenmiş bir birim yerel olarak sabitlenir.
   
   d. Bağlı **ana bilgisayarlar**için **Seç'i**tıklatın.
   
   ![Yedekleme yi klonlama](./media/storsimple-virtual-array-clone/clonevolume4.png)
4. Bağlı **ana bilgisayar** bıçaklarında, varolan bir ACR'den seçim seçin veya yeni bir ACR ekleyin. Yeni bir ACR eklemek için bir ACR adı ve ana bilgisayar IQN sağlamanız gerekir. **Seç'i**tıklatın.
   
   ![Yedekleme yi klonlama](./media/storsimple-virtual-array-clone/clonevolume5.png)
5. Klon işi başlatmak için **Clone'u** tıklatın.
   
   ![Yedekleme yi klonlama](./media/storsimple-virtual-array-clone/clonevolume6.png)  
6. Klon işi oluşturulduktan sonra klonlama başlar. Klon oluşturulduktan sonra cihazınızdaki Volumes bıçağında görüntülenir. Katmanlı bir birimin katmanlı olarak klonlanmış olduğunu ve yerel olarak sabitlenmiş bir birimin yerel olarak sabitlenmiş bir birim olarak klonlanmış olduğunu unutmayın.
   
   ![Yedekleme yi klonlama](./media/storsimple-virtual-array-clone/clonevolume8.png)
7. Birim, birimler listesinde çevrimiçi olarak göründüğünde, birim kullanılabilir. iSCSI başlatıcısı ana bilgisayarda, iSCSI başlatıcı özellikleri penceresindeki hedeflerin listesini yenileyin. Klonlanmış birim adını içeren yeni bir hedef, durum sütununun altında 'etkin olmayan' olarak görünmelidir.
8. Hedefi seçin ve **Bağlan'ı**tıklatın. Başlatıcı hedefe bağlandıktan sonra durum **Bağlı**olarak değiştirilmelidir.
9. Disk **Yönetimi** penceresinde, monte edilen birimler aşağıdaki resimde gösterildiği gibi görünür. Bulunan birime sağ tıklayın (disk adına tıklayın) ve ardından **Çevrimiçi**’ne tıklayın.

> [!IMPORTANT]
> Bir yedekleme kümesinden bir birim veya paylaşım klonlamaya çalışırken, klon işi başarısız olursa, portalda bir hedef birim veya paylaşım oluşturulabilir. Bu öğeden kaynaklanan gelecekteki sorunları en aza indirmek için bu hedef hacmi ni silmeniz veya portaldaki paylaşımınız önemlidir.
> 
> 

## <a name="item-level-recovery-ilr"></a>Öğe düzeyinde kurtarma (ILR)

Bu sürüm, dosya sunucusu olarak yapılandırılan StorSimple Virtual Array'de madde düzeyinde kurtarma (ILR) sunar. Bu özellik, StorSimple aygıtındaki tüm paylaşımların bulut yedeklemesinden dosya ve klasörlerin parçalı kurtarmasını yapmanızı sağlar. Self servis bir model kullanarak son yedeklemelerden silinen dosyaları alabilirsiniz.

Her paylaşımda en son yedeklemeleri içeren bir *.backups* klasörü vardır. İstediğiniz yedeklemeye gidebilir, ilgili dosya ve klasörleri yedeklemeden kopyalayabilir ve geri yükleyebilirsiniz. Bu özellik, yöneticilerin dosyaları yedeklemelerden geri alma çağrılarını ortadan kaldırır.

1. ILR gerçekleştirirken, yedekleri Dosya Gezgini aracılığıyla görüntüleyebilirsiniz. Yedeklemeye bakmak istediğiniz belirli paylaşımı tıklatın. Tüm yedeklemeleri depolayan paylaşımın altında oluşturulan bir *.backups* klasörü görürsünüz. Yedekleri görüntülemek için *.backups* klasörünü genişletin. Klasör, tüm yedekleme hiyerarşisinin bdeki görünümünü gösterir. Bu görünüm isteğe bağlı olarak oluşturulur ve genellikle oluşturmak yalnızca birkaç saniye sürer.
   
   Son beş yedekleme bu şekilde görüntülenir ve madde düzeyinde bir kurtarma gerçekleştirmek için kullanılabilir. Son beş yedekleme, hem varsayılan zamanlanmış hem de el ile yedekleri içerir.
   
   * Aygıt adı &lt;&gt;DailySchedule-YYYYMMDD-HHMMSS-UTC olarak adlandırılan **zamanlanmış yedeklemeler.**
   * Ad-hoc-YYYYMMDD-HHMMSS-UTC olarak adlandırılan **manuel yedeklemeler.**
     
     ![](./media/storsimple-virtual-array-clone/image14.png)

2. Silinen dosyanın en son sürümünü içeren yedeklemeyi tanımlayın. Klasör adı, önceki servis taleplerinin her birinde bir UTC zaman damgası içerse de, klasörün oluşturulduğu saat yedeklemenin başlatıldığu gerçek aygıt zamanıdır. Yedekleri bulmak ve tanımlamak için klasör zaman damgasını kullanın.

3. Önceki adımda tanımladığınız yedeklemede geri yüklemek istediğiniz klasörü veya dosyayı bulun. Not, yalnızca izinli olduğunuz dosyaları veya klasörleri görüntüleyebilirsiniz. Belirli dosya veya klasörlere erişemiyorsanız, paylaşım yöneticisine başvurun. Yönetici, paylaşım izinlerini erişmek ve belirli bir dosya veya klasöre erişim sağlamak için Dosya Gezgini'ni kullanabilir. Paylaşım yöneticisinin tek bir kullanıcı yerine bir kullanıcı grubu olması önerilen en iyi uygulamadır.

4. Dosyayı veya klasörü StorSimple dosya sunucunuzdaki uygun paylaşıma kopyalayın.

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Sanal Dizinizi yerel web Kullanıcı Birliğini kullanarak nasıl yöneteceğimiz](storsimple-ova-web-ui-admin.md)hakkında daha fazla bilgi edinin.

