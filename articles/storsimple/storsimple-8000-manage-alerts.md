---
title: StorSimple 8000 serisi cihaz için uyarıları görüntüleme ve yönetme
description: StorSimple uyarı koşullarını ve önem derecesini, uyarı bildirimlerini nasıl yapılandırıştırılabildiğini ve uyarıları yönetmek için StorSimple Device Manager hizmetini nasıl kullanacağını açıklar.
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: alkohli
ms.openlocfilehash: ff50836e1438b8d35f26ddfdf165084406f52faf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267826"
---
# <a name="use-the-storsimple-device-manager-service-to-view-and-manage-storsimple-alerts"></a>StorSimple uyarılarını görüntülemek ve yönetmek için StorSimple Device Manager hizmetini kullanın

## <a name="overview"></a>Genel Bakış

StorSimple Device Manager hizmetindeki **Alerts** blade, StorSimple cihazıyla ilgili uyarıları gerçek zamanlı olarak gözden geçirmeniz ve temizlemeniz için bir yol sağlar. Bu bıçaktan, StorSimple aygıtlarınızın sistem durumu sorunlarını ve genel Microsoft Azure StorSimple çözümünü merkezi olarak izleyebilirsiniz.

Bu öğretici, yaygın uyarı koşullarını, uyarı önem düzeyleri ve uyarı bildirimlerinin nasıl yapılandırılabildiğini açıklar. Ayrıca, belirli bir uyarıyı hızlı bir şekilde bulmanızı ve uygun şekilde yanıt vermenizi sağlayan hızlı uyarı başvuru tablolarını içerir.

![Uyarılar sayfası](./media/storsimple-8000-manage-alerts/configure-alerts-email11.png)

## <a name="common-alert-conditions"></a>Sık uyarı koşulları

StorSimple cihazınız çeşitli koşullara yanıt olarak uyarılar oluşturur. En yaygın uyarı koşulları türleri şunlardır:

* **Donanım sorunları** – Bu uyarılar size donanımınızın durumunu anlatır. Firmware yükseltmeleri gerekip gerekmediğini, ağ arabiriminde sorun varsa veya veri sürücülerinizden biriyle ilgili bir sorun varsa size bildirin.
* **Bağlantı sorunları** – Bu uyarılar, veri aktarımında güçlük olduğunda oluşur. Azure depolama hesabına veri aktarımı sırasında veya aygıtlar ile StorSimple Device Manager hizmeti arasında bağlantı olmaması nedeniyle iletişim sorunları oluşabilir. İletişim sorunları, düzeltilmesi en zor sorunlardan bazılarıdır, çünkü çok fazla başarısızlık noktası vardır. Daha gelişmiş sorun gidermeye devam etmeden önce her zaman ağ bağlantısının ve Internet erişiminin kullanılabilir olduğunu doğrulamanız gerekir. Sorun giderme ile ilgili yardım için [Test-Bağlantı cmdlet ile Sorun](storsimple-8000-troubleshoot-deployment.md)Giderme'ye gidin.
* **Performans sorunları** – Bu uyarılar, sisteminiz ağır bir yük altında yken olduğu gibi en iyi şekilde performans sergilemiyorsa neden olur.

Ayrıca, güvenlik, güncelleştirmeler veya iş hatalarıyla ilgili uyarılar da görebilirsiniz.

## <a name="alert-severity-levels"></a>Uyarı önem düzeyleri

Uyarılar, uyarı durumunun yaratacağı etkiye ve uyarıya yanıt verme gereksinimine bağlı olarak farklı önem düzeylerine sahiptir. Önem dereceleri şunlardır:

* **Kritik** – Bu uyarı, sisteminizin başarılı performansını etkileyen bir duruma yanıt olarak verilir. StorSimple hizmetinin kesintiye uğramadığından emin olmak için eylem gereklidir.
* **Uyarı** – Bu durum çözülmezse kritik hale gelebilir. Durumu araştırmalı ve sorunu gidermek için gerekli her türlü önlemi almalısınız.
* **Bilgi** - Bu uyarı, sisteminizi izleme ve yönetmede yararlı olabilecek bilgiler içerir.

## <a name="configure-alert-settings"></a>Uyarı ayarlarını yapılandırma

StorSimple cihazlarınızın her biri için uyarı koşullarının e-posta yoluyla bildirilmesini isteyip istemediğinizi seçebilirsiniz. Ayrıca, diğer uyarı bildirimi alıcılarını, diğer e-posta alıcıları kutusuna, yarı sütunlarla ayrılmış **e-posta alıcıları** kutusuna girerek tanımlayabilirsiniz.

> [!NOTE]
> Cihaz başına en fazla 20 e-posta adresi girebilirsiniz.

Bir aygıt için e-posta bildirimini etkinleştirdikten sonra, bildirim listesinin üyeleri ne zaman kritik bir uyarı oluştuğunda bir e-posta iletisi alır. İletiler *storsimple-alerts-noreply\@mail.windowsazure.com* gönderilecektir ve uyarı durumunu açıklar. Alıcılar kendilerini e-posta bildirim listesinden kaldırmak için **Abonelikten Çık'ı** tıklatabilir.

#### <a name="to-enable-email-notification-of-alerts-for-a-device"></a>Bir aygıt için uyarıların e-posta bildirimini etkinleştirmek için
1. StorSimple Cihaz Yöneticisi hizmetinize gidin. Aygıtlar listesinden, yapılandırmak istediğiniz aygıtı seçin ve tıklatın.
2. Aygıt için **Ayarlar** > **Genel'e** gidin.

   ![Uyarılar bıçak](./media/storsimple-8000-manage-alerts/configure-alerts-email2.png)
   
2. Genel **ayarlar** bıçak, **Uyarı ayarları** gidin ve aşağıdakileri ayarlayın:
   
   1. **E-posta gönder bildirim** alanında **EVET'i**seçin.
   2. **E-posta hizmeti yöneticileri** alanında, hizmet yöneticisinin ve tüm yardımcı yöneticilerin uyarı bildirimlerini alması için **EVET'i** seçin.
   3. Diğer **e-posta alıcıları** alanına, uyarı bildirimlerini alması gereken diğer tüm alıcıların e-posta adreslerini girin. *Birisi\@somewhere.com*biçiminde adları girin. E-posta adreslerini ayırmak için yarım sütunlar kullanın. Aygıt başına en fazla 20 e-posta adresi yapılandırabilirsiniz. 
      
3. Test e-postası bildirimi göndermek **için test e-postası gönder'i**tıklatın. StorSimple Device Manager hizmeti, test bildirimini ileterken durum iletilerini görüntüler.

    ![Uyarı ayarları](./media/storsimple-8000-manage-alerts/configure-alerts-email3.png)

4. Test e-postası gönderildiğinde bir bildirim görürsünüz. 
   
    ![Uyarılar test bildirim e-postası gönderildi](./media/storsimple-8000-manage-alerts/configure-alerts-email4.png)
   
   > [!NOTE]
   > Test bildirimi iletisi gönderilemezse, StorSimple Device Manager hizmeti uygun bir hata iletisi görüntüler. Birkaç dakika bekleyin ve ardından test bildirim iletinizi yeniden göndermeyi deneyin. 

5. Yapılandırmayı tamamladıktan sonra **Kaydet'i**tıklatın. Onayınız istendiğinde **Evet**’e tıklayın.

     ![Uyarılar test bildirim e-postası gönderildi](./media/storsimple-8000-manage-alerts/configure-alerts-email5.png)

## <a name="view-and-track-alerts"></a>Uyarıları görüntüleme ve izleme

StorSimple Device Manager servis özet bıçağı, önem derecesine göre düzenlenmiş cihazlarınızdaki uyarı sayısına hızlı bir bakış sağlar.

![Uyarılar panosu](./media/storsimple-8000-manage-alerts/device-summary4.png)

Önem düzeyine tıkladığınızda **Uyarılar** bıçağı açılır. Sonuçlar yalnızca bu önem düzeyiyle eşleşen uyarıları içerir.

Listedeki bir uyarıyı tıklatmak, uyarının en son ne zaman bildirilmesi, aygıttaki uyarının oluşum sayısı ve uyarıyı çözmek için önerilen eylem de dahil olmak üzere uyarı için ek ayrıntılar sağlar. Bir donanım uyarısı ysa, donanım bileşenini de tanımlar.

![Donanım uyarısı örneği](./media/storsimple-8000-manage-alerts/configure-alerts-email14.png)

Bilgileri Microsoft Destek'e göndermeniz gerekiyorsa, uyarı ayrıntılarını bir metin dosyasına kopyalayabilirsiniz. Öneriyi takip ettikten ve uyarı koşulunu şirket içinde çözdükten sonra, **Uyarılar** bıçağındaki uyarıyı seçip **Temizle'yi**tıklatarak uyarıyı cihazdan temizlemeniz gerekir. Birden çok uyarıyı temizlemek için her uyarıyı seçin, **Uyarı** sütunu dışında herhangi bir sütunu tıklatın ve ardından temizlenecek tüm uyarıları seçtikten sonra **Temizle'yi** tıklatın. Sorun çözüldüğünde veya sistem uyarıyı yeni bilgilerle güncellediğinde bazı uyarıların otomatik olarak temizlendiğini unutmayın.

**Temizle'yi**tıklattığınızda, uyarı ve sorunu gidermek için attığınız adımlar hakkında yorum sağlama fırsatına sahip olursunuz. Başka bir olay yeni bilgilerle tetiklenirse, bazı olaylar sistem tarafından temizlenir. Bu durumda, aşağıdaki iletiyi görürsünüz.

![Uyarı iletisi temizle](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Uyarıları sıralama ve inceleme

Raporları gruplar halinde gözden geçirip temizlemek için raporları uyarılarda çalıştırmayı daha verimli bulabilirsiniz. Ayrıca, **Alerts** blade en fazla 250 uyarıları görüntüleyebilir. Bu uyarı sayısını aştıysanız, tüm uyarılar varsayılan görünümde görüntülenmez. Hangi uyarıların görüntülendiğini özelleştirmek için aşağıdaki alanları birleştirebilirsiniz:

* **Durum** – **Etkin** veya **Temizlenmiş** uyarıları görüntüleyebilirsiniz. Sisteminizde etkin uyarılar hala tetiklenirken, temizlenen uyarılar bir yönetici tarafından el ile temizlenir veya sistem uyarı koşulunu yeni bilgilerle güncelleştirdiği için programlanabilir şekilde temizlenir.
* **Önem derecesi** – Tüm önem düzeyleri (kritik, uyarı, bilgi) veya yalnızca kritik uyarılar gibi belirli bir önem derecesine ilişkin uyarıları görüntüleyebilirsiniz.
* **Kaynak** – Tüm kaynaklardan gelen uyarıları görüntüleyebilir veya uyarıları hizmetten veya aygıtlardan biri veya tümünden gelen lerle sınırlandırabilirsiniz.
* **Zaman aralığı** – **Kimden** ve **To** tarihleri ve zaman damgalarını belirterek, ilgilendiğiniz zaman dilimindeki uyarılara bakabilirsiniz.

![Uyarılar listesi](./media/storsimple-8000-manage-alerts/configure-alerts-email11.png)

## <a name="alerts-quick-reference"></a>Uyarıları hızlı başvuru

Aşağıdaki tablolarda karşılaşabileceğiniz Microsoft Azure StorSimple uyarılarının yanı sıra mevcut yerlerde ek bilgi ve öneriler listelenmektedir. StorSimple aygıt uyarıları aşağıdaki kategorilerden birine girer:

* [Bulut bağlantısı uyarıları](#cloud-connectivity-alerts)
* [Küme uyarıları](#cluster-alerts)
* [Olağanüstü durum kurtarma uyarıları](#disaster-recovery-alerts)
* [Donanım uyarıları](#hardware-alerts)
* [İş hatası uyarıları](#job-failure-alerts)
* [Yerel olarak sabitlenmiş ses uyarıları](#locally-pinned-volume-alerts)
* [Ağ uyarıları](#networking-alerts)
* [Performans uyarıları](#performance-alerts)
* [Güvenlik uyarıları](#security-alerts)
* [Destek paketi uyarıları](#support-package-alerts)

### <a name="cloud-connectivity-alerts"></a>Bulut bağlantısı uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemler |
|:--- |:--- |:--- |
| bulut kimlik *>* <bağlantı kurulamaz. |Depolama hesabına bağlanamıyor. |Aygıtınızda bir bağlantı sorunu olabilir gibi görünüyor. Sorunu tanımlamak `Test-HcsmConnection` ve gidermek için lütfen cihazınızda StorSimple için Windows PowerShell Arabiriminden cmdlet çalıştırın. Ayarlar doğruysa, sorun uyarının yükseltildiği depolama hesabının kimlik bilgileriyle birlikte olabilir. Bu durumda, çözebileceğiniz sorunlar olup olmadığını belirlemek için `Test-HcsStorageAccountCredential` cmdlet'i kullanın.<ul><li>Ağ ayarlarınızı kontrol edin.</li><li>Depolama hesabı kimlik bilgilerinizi kontrol edin.</li></ul> |
| Cihazınızdan son <*sayısı*> dakika boyunca sinyal alamadık. |Aygıta bağlanamıyor. |Cihazınızda bir bağlantı sorunu var gibi görünüyor. Sorunu tanımlamak `Test-HcsmConnection` ve gidermek veya ağ yöneticinize başvurmak için lütfen cihazınızdaki StorSimple için Windows PowerShell Arabirimindeki cmdlet'i kullanın. |

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>Bulut bağlantısı başarısız olduğunda StorBasit davranış

Üretimde çalışan StorSimple aygıtımiçin bulut bağlantısı başarısız olursa ne olur?

StorSimple üretim cihazınızda bulut bağlantısı başarısız olursa, cihazınızın durumuna bağlı olarak aşağıdakiler oluşabilir:

* **Cihazınızdaki yerel veriler için**: Bir süre için herhangi bir kesinti olmayacak ve okumalar sunulmaya devam edecektir. Ancak, bekleyen IOs sayısı arttıkça ve bir sınırı aştıkça, okumalar başarısız olmaya başlayabilir.

    Cihazınızdaki veri miktarına bağlı olarak, bulut bağlantısındaki bozulmadan sonraki ilk birkaç saat boyunca yazmalar da gerçekleşmeye devam edecektir. Daha sonra yazmayavaşlar ve bulut bağlantısı birkaç saat boyunca bozulursa başarısız olmaya başlar. (Buluta itilmesi gereken veriler için aygıtta geçici depolama alanı vardır. Veriler gönderildiğinde bu alan temizlenir. Bağlantı başarısız olursa, bu depolama alanındaki veriler buluta itilmez ve IO başarısız olur.)
* **Buluttaki veriler için**: Bulut bağlantısı hatalarının çoğunda bir hata döndürülür. Bağlantı geri yüklendikten sonra, kullanıcı nın sesi çevrimiçi duruma getirmesine gerek kalmadan IOs'a devam edilir. Nadir durumlarda, Azure portalından sesi çevrimiçi olarak geri getirmek için kullanıcı müdahalesi gerekebilir.
* **Devam eden bulut anlık görüntüleri**için : İşlem 4-5 saat içinde birkaç kez yeniden denendi ve bağlantı geri yüklenmezse, bulut anlık görüntüleri başarısız olur.

### <a name="cluster-alerts"></a>Küme uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemler |
|:--- |:--- |:--- |
| Aygıt <*aygıtın adını*> için başarısız oldu. |Cihaz bakım modunda. |Cihaz, bakım moduna girilmesi veya dışarı çıkması nedeniyle arızalı kaldı. Bu normaldir ve herhangi bir eylem gereklidir. Bu uyarıyı kabul ettikten sonra, uyarılar sayfasından temizleyin. |
| Aygıt <*aygıtın adını*> için başarısız oldu. |Aygıt firmware veya yazılım sadece güncellendi. |Bir güncelleştirme nedeniyle bir küme başarısız oldu. Bu normaldir ve herhangi bir eylem gereklidir. Bu uyarıyı kabul ettikten sonra, uyarılar sayfasından temizleyin. |
| Aygıt <*aygıtın adını*> için başarısız oldu. |Denetleyici kapatıldı veya yeniden başlatıldı. |Etkin denetleyici kapatıldığından veya bir yönetici tarafından yeniden başlatıldığından aygıt üzerinde başarısız oldu. Hiçbir eylem gerekli değildir. Bu uyarıyı kabul ettikten sonra, uyarılar sayfasından temizleyin. |
| Aygıt <*aygıtın adını*> için başarısız oldu. |Planlı başarısız. |Bunun planlanmış bir hata olduğunu doğrulayın. Uygun eylemi yaptıktan sonra, bu uyarıyı uyarılar sayfasından temizleyin. |
| Aygıt <*aygıtın adını*> için başarısız oldu. |Planlanmamış bir başarısızlık. |StorSimple, planlanmamış arızalardan otomatik olarak kurtulmak için oluşturulmuştır. Bu uyarıların çok sayıda görürse, Microsoft Destek'e başvurun. |
| Aygıt <*aygıtın adını*> için başarısız oldu. |Diğer/bilinmeyen neden. |Bu uyarıların çok sayıda görürse, Microsoft Destek'e başvurun. Sorun çözüldükten sonra, bu uyarıyı uyarılar sayfasından temizleyin. |
| Kritik bir aygıt hizmeti durumu başarısız olarak bildirir. |Datapath hizmet hatası. |Yardım için Microsoft Destek'e başvurun. |
| Ağ arabirimi için sanal IP adresi <*DATA #*> başarısız olarak durum bildirir. |Diğer/bilinmeyen neden. |Bazen geçici koşullar bu uyarılara neden olabilir. Bu durumda, bu uyarı bir süre sonra otomatik olarak temizlenir. Sorun devam ederse, Microsoft Destek'e başvurun. |
| Ağ arabirimi için sanal IP adresi <*DATA #*> başarısız olarak durum bildirir. |Arayüz adı: <*DATA* `<IP address>` #> IP adresi, ağda yinelenen bir IP adresi algılandığı için çevrimiçi olarak getirilemez. |Yinelenen IP adresinin ağdan kaldırıldığından emin olun veya arabirimi farklı bir IP adresiyle yeniden yapılandırın. |

### <a name="disaster-recovery-alerts"></a>Olağanüstü durum kurtarma uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemler |
|:--- |:--- |:--- |
| Kurtarma işlemleri bu hizmetin tüm ayarlarını geri yükleyemedi. Aygıt yapılandırma verileri bazı aygıtlar için tutarsız durumdadır. |Olağanüstü durum kurtarma dan sonra algılanan veri tutarsızlığı. |Hizmetteki şifrelenmiş veriler aygıttakiyle eşitlenmez. Senkronizasyon işlemini başlatmak için Aygıt <*aygıt adını* StorSimple Device Manager'dan> yetkilendirin. Güvenlik profilini geri yüklemek için bu `Restore-HcsmEncryptedServiceData` cmdlet'e giriş olarak eski parolayı sağlayarak, aygıt <*aygıt adını*> cmdlet'i çalıştırmak için StorSimple için Windows PowerShell Arabirimini kullanın. Ardından hizmet `Invoke-HcsmServiceDataEncryptionKeyChange` veri şifreleme anahtarını güncelleştirmek için cmdlet'i çalıştırın. Uygun eylemi yaptıktan sonra, bu uyarıyı uyarılar sayfasından temizleyin. |

### <a name="hardware-alerts"></a>Donanım uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemler |
|:--- |:--- |:--- |
| Donanım bileşeni <*bileşen kimliği*> *durum*> <olarak raporeder. | |Bazen geçici koşullar bu uyarılara neden olabilir. Bu nedenle, bu uyarı bir süre sonra otomatik olarak temizlenir. Sorun devam ederse, Microsoft Destek'e başvurun. |
| Pasif kontrol cihazı arızalı. |Pasif (ikincil) denetleyici çalışmıyor. |Cihazınız çalışıyor, ancak kontrolörlerinizden biri arızalı. Denetleyiciyi yeniden başlatmayı dene. Sorun çözülmezse, Microsoft Destek'e başvurun. |

### <a name="job-failure-alerts"></a>İş hatası uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemler |
|:--- |:--- |:--- |
| <*kaynak birim grup kimliğinin* yedek> başarısız oldu. |Yedekleme işi başarısız oldu. |Bağlantı sorunları yedekleme işleminin başarıyla tamamlanmasını engelliyor olabilir. Bağlantı sorunları yoksa, en fazla yedekleme sayısına ulaşmış olabilirsiniz. Artık gerekmeyen yedeklemeleri silin ve işlemi yeniden deneyin. Uygun eylemi yaptıktan sonra, bu uyarıyı uyarılar sayfasından temizleyin. |
| Hedef birim seri numaralarını <için> <*kaynak yedekleme öğesi nin* grubu *>.* |Klon işi başarısız oldu. |Yedeklemenin hala geçerli olduğunu doğrulamak için yedekleme listesini yenileyin. Yedekleme geçerliyse, bulut bağlantısı sorunlarının klon işleminin başarıyla tamamlanmasını engelleyebilir. Bağlantı sorunları yoksa, depolama sınırına ulaşmış olabilirsiniz. Artık gerekmeyen yedeklemeleri silin ve işlemi yeniden deneyin. Sorunu gidermek için uygun eylemi yaptıktan sonra, bu uyarıyı uyarılar sayfasından temizleyin. |
| Kaynak *yedekleme öğesi <* geri yükleme> başarısız oldu. |Geri yükleme işi başarısız oldu. |Yedeklemenin hala geçerli olduğunu doğrulamak için yedekleme listesini yenileyin. Yedekleme geçerliyse, bulut bağlantısı sorunlarının geri yükleme işleminin başarıyla tamamlanmasını engelleyebilir. Bağlantı sorunları yoksa, depolama sınırına ulaşmış olabilirsiniz. Artık gerekmeyen yedeklemeleri silin ve işlemi yeniden deneyin. Sorunu gidermek için uygun eylemi yaptıktan sonra, bu uyarıyı uyarılar sayfasından temizleyin. |

### <a name="locally-pinned-volume-alerts"></a>Yerel olarak sabitlenmiş ses uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemler |
|:--- |:--- |:--- |
| Yerel birim <*birim adı* oluşturulması> başarısız oldu. |Birim oluşturma işi başarısız oldu. <*Başarısız hata koduna karşılık gelen hata iletisi*>. |Bağlantı sorunları, alan oluşturma işleminin başarıyla tamamlanmasını engelliyor olabilir. Yerel olarak sabitlenmiş hacimler kalın bir şekilde karşılanır ve alan oluşturma işlemi katmanlı birimlerin buluta dökülmesini içerir. Bağlantı sorunları yoksa, aygıttaki yerel alanı tüketmiş olabilirsiniz. Bu işlemi yeniden denemeden önce aygıtta alan olup olmadığını belirleyin. |
| Yerel birim <*birim adı*> genişletme başarısız oldu. |Birim değiştirme işi,> *başarısız hata koduna karşılık gelen* <hata iletisi nedeniyle başarısız oldu. |Bağlantı sorunları, birim genişletme işleminin başarıyla tamamlanmasını engelliyor olabilir. Yerel olarak sabitlenmiş birimler kalın bir şekilde karşılanır ve varolan alanı genişletme işlemi katmanlı birimlerin buluta dökülmesini içerir. Bağlantı sorunları yoksa, aygıttaki yerel alanı tüketmiş olabilirsiniz. Bu işlemi yeniden denemeden önce aygıtta alan olup olmadığını belirleyin. |
| Birim <*birim adının* dönüştürülmesi> başarısız oldu. |Birim türünü yerel olarak sabitlenmiş katmanlı ya dönüştürmek için birim dönüştürme işi başarısız oldu. |Birimin yerel olarak sabitlenmiş türden katmanlı olarak dönüştürülmesi tamamlanamadı. İşlemin başarıyla tamamlanmasını engelleyen bağlantı sorunları olmadığından emin olun. Sorun giderme bağlantısı sorunları için [Test-HcsmConnection cmdlet ile Sorun Giderme](storsimple-8000-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet)gidin.<br>Yerel olarak sabitlenmiş orijinal birim, dönüşüm sırasında yerel olarak sabitlenmiş birimden elde edilen bazı veriler buluta döküldüğünden katmanlı bir birim olarak işaretlenmiştir. Ortaya çıkan katmanlı hacim, gelecekteki yerel birimler için geri alınamayan aygıtta yerel alanı işgal ediyor.<br>Tüm verilerin yerel olarak yeniden kullanılabilir hale getirilmesini sağlamak için bağlantı sorunlarını giderin, uyarıyı temizleyin ve bu birimi orijinal yerel olarak sabitlenmiş birim türüne dönüştürün. |
| Birim <*birim adının* dönüştürülmesi> başarısız oldu. |Birim türünü katmanlıdan yerel olarak sabitlenmiş olarak dönüştürmek için birim dönüştürme işi başarısız oldu. |Birimin katmanlı türden yerel olarak sabitlenmiş türünden dönüştürülmesi tamamlanamadı. İşlemin başarıyla tamamlanmasını engelleyen bağlantı sorunları olmadığından emin olun. Sorun giderme bağlantısı sorunları için [Test-HcsmConnection cmdlet ile Sorun Giderme](storsimple-8000-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet)gidin.<br>Dönüştürme işleminin bir parçası olarak yerel olarak sabitlenmiş bir birim olarak işaretlenmiş özgün katmanlı birim bulutta bulunan verilere sahip olmaya devam ederken, bu birim için aygıtta kalın olarak sağlanan alan artık gelecekteki yerel birimler için geri alınamaz.<br>Bağlantı sorunlarını giderin, uyarıyı temizleyin ve aygıtta kalın olarak sağlanan yerel alanın geri alınabilmesini sağlamak için bu birimi orijinal katmanlı birim türüne geri dönüştürün. |
| *<toplu grup adının* yerel anlık görüntüleri için yerel alan tüketimine yaklaşma> |Yedekleme ilkesinin yerel anlık görüntüleri yakında yer tükenebilir ve ana bilgisayar yazma hatalarını önlemek için geçersiz kılınabilir. |Bu yedekleme ilkesi grubuyla ilişkili birimlerdeki yüksek veri karmaşasının yanında sık sık yerel anlık görüntüler, aygıttaki yerel alanın hızla tüketilmesine neden oluyor. Artık gerekolmayan tüm yerel anlık görüntüleri silin. Ayrıca, daha az sıklıkta yerel anlık görüntü almak ve bulut anlık görüntülerini düzenli olarak alındığından emin olmak için bu yedekleme ilkesi için yerel anlık görüntü zamanlamalarınızı güncelleştirin. Bu eylemler alınmazsa, bu anlık görüntüler için yerel alan yakında tükenebilir ve sistem, ana bilgisayar yazmalarının başarıyla işlemeye devam etmesini sağlamak için bunları otomatik olarak siler. |
| *<toplu grup adı*> yerel anlık görüntüler geçersiz kılındı. |<toplu grup *adının* yerel anlık görüntüleri> geçersiz kılındı ve aygıttaki yerel alanı aştıkları için silindi. |Bunun gelecekte yinelenmemesini sağlamak için, bu yedekleme ilkesinin yerel anlık görüntü zamanlamalarını gözden geçirin ve artık gerekmeyen yerel anlık görüntüleri silin. Bu yedekleme ilkesi grubuyla ilişkili birimlerdeki yüksek veri karmaşasının yanında sık sık yerel anlık görüntüler, aygıttaki yerel alanın hızla tüketilmesine neden olabilir. |
| Kaynak *yedekleme öğesi <* geri yükleme> başarısız oldu. |Geri yükleme işi başarısız oldu. |Bu yedekleme ilkesinde yerel olarak sabitlenmiş veya yerel olarak sabitlenmiş ve katmanlı birimlerin bir karışımı varsa, yedeklemenin hala geçerli olduğunu doğrulamak için yedekleme listesini yenileyin. Yedekleme geçerliyse, bulut bağlantısı sorunlarının geri yükleme işleminin başarıyla tamamlanmasını engelleyebilir. Bu anlık görüntü grubunun bir parçası olarak geri yüklenen yerel olarak sabitlenmiş birimler, tüm verilerini aygıta karşı yüklenmez ve bu anlık görüntü grubunda katmanlı ve yerel olarak sabitlenmiş birimlerin bir karışımı varsa, bunlar birbirleriyle eşitlenmez. Geri yükleme işlemini başarıyla tamamlamak için, bu gruptaki birimleri ana bilgisayarda çevrimdışı olarak alın ve geri yükleme işlemini yeniden deneyin. Geri yükleme işlemi sırasında gerçekleştirilen birim verilerinde yapılan değişikliklerin kaybolacağını unutmayın. |

### <a name="networking-alerts"></a>Ağ uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemler |
|:--- |:--- |:--- |
| StorSimple hizmeti(ler) başlatılamamadı. |Datapath hatası |Sorun devam ederse, Microsoft Destek'e başvurun. |
| 'Data0' için yinelenen IP adresi algılandı. | |Sistem IP adresi '10.0.0.1' için bir çakışma algılamıştır. Aygıt aygıtındaki ağ kaynağı 'Data0' * \<aygıt1>* çevrimdışıdır. Bu IP adresinin bu ağdaki başka bir varlık tarafından kullanılmadığından emin olun. Ağ sorunlarını gidermek için [Get-NetAdapter cmdlet ile Sorun](storsimple-8000-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet)Giderme'ye gidin. Bu sorunu çözmeye yardımcı olmak için ağ yöneticinize başvurun. Sorun devam ederse, Microsoft Destek'e başvurun. |
| 'Data0' için IPv4 (veya IPv6) adresi çevrimdışıdır. | |IP adresi '10.0.0.1' olan ağ kaynağı 'Data0'. ve aygıt * \<aygıtındaki* '22' önek uzunluğu1>çevrimdışıdır. Bu arabirimin bağlı olduğu anahtar bağlantı noktalarının çalışır durumda olduğundan emin olun. Ağ sorunlarını gidermek için [Get-NetAdapter cmdlet ile Sorun](storsimple-8000-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet)Giderme'ye gidin. |
| Kimlik doğrulama hizmetine bağlanamadı. |Datapath hatası |Kimlik doğrulaması için kullanılan URL'lere erişilemez. Güvenlik duvarı kurallarınızın StorSimple aygıtı için belirtilen URL desenlerini içerdiğinden emin olun. Azure portalındaki URL desenleri hakkında daha\/fazla bilgi için https: /aka.ms/ss-8000-network-reqs adresine gidin. Azure Kamu Bulutu kullanıyorsanız, https:\//aka.ms/ss8000-gov-network-reqs'deki URL desenlerine gidin.|

### <a name="performance-alerts"></a>Performans uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemler | |
|:--- |:--- |:--- | --- |
| Aygıt yükü> <*eşiği* aştı. |Beklenenden daha yavaş yanıt süreleri. |Cihazınız, yoğun bir giriş/çıkış yükü altında kullanımı raporlar. Bu, cihazınızın olması gerektiği kadar iyi çalışmamasını sağlayabilir. Aygıta iliştirdiğiniz iş yüklerini gözden geçirin ve başka bir aygıta taşınabilecek veya artık gerekli olmayan iş yüklerinin olup olmadığını belirleyin.|
| StorSimple hizmeti(ler) başlatılamamadı. |Datapath hatası |Sorun devam ederse, Microsoft Destek'e başvurun. |

### <a name="security-alerts"></a>Güvenlik uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemler |
|:--- |:--- |:--- |
| Microsoft Destek oturumu başladı. |Üçüncü taraf erişilen destek oturumu. |Lütfen bu erişimin yetkili olduğunu onaylayın. Uygun eylemi yaptıktan sonra, bu uyarıyı uyarılar sayfasından temizleyin. |
| <*öğe*> için parola> <*süre* içinde sona erer. |Parola nın sona ermesi yaklaşıyor. |Süresi dolmadan önce parolanızı değiştirin. |
| <*öğesi kimliği* için güvenlik yapılandırma bilgileri eksik>. | |Bu birim kapsayıcıile ilişkili birimler StorSimple yapılandırmanızı çoğaltmak için kullanılamaz. Verilerinizin güvenli bir şekilde depolandığından emin olmak için, birim kapsayıcısını ve birim kapsayıcısıyla ilişkili birimleri silmenizi öneririz. Uygun eylemi yaptıktan sonra, bu uyarıyı uyarılar sayfasından temizleyin. |
| <*numarası*> giriş girişimleri <*öğe kimliği*> için başarısız oldu. |Birden çok başarısız oturum açma denemesi. |Cihazınız saldırı altında olabilir veya yetkili bir kullanıcı yanlış bir parolayla bağlanmaya çalışıyor olabilir.<ul><li>Yetkili kullanıcılarınızla iletişim kurun ve bu girişimlerin yasal bir kaynaktan geldiğini doğrulayın. Çok sayıda başarısız oturum açma denemesi görmeye devam ederseniz, uzaktan yönetimi devre dışı bırakmayı ve ağ yöneticinizle iletişim kurmayı düşünün. Uygun eylemi yaptıktan sonra, bu uyarıyı uyarılar sayfasından temizleyin.</li><li>Anlık Görüntü Yöneticisi örneklerinizin doğru parolayla yapılandırıldığından denetleyin. Uygun eylemi yaptıktan sonra, bu uyarıyı uyarılar sayfasından temizleyin.</li></ul>Daha fazla bilgi [için, süresi dolmuş aygıt parolasını değiştir'e](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password)gidin. |
| Hizmet veri şifreleme anahtarını değiştirirken bir veya daha fazla hata oluştu. | |Hizmet veri şifreleme anahtarını değiştirirken karşılaşılan hatalar vardı. Hata koşullarını ele aldıktan sonra, `Invoke-HcsmServiceDataEncryptionKeyChange` hizmeti güncelleştirmek için cihazınızdaki StorSimple için Windows PowerShell Arabiriminden cmdlet çalıştırın. Bu sorun devam ederse, Microsoft desteğine başvurun. Sorunu çözdükten sonra, bu uyarıyı uyarılar sayfasından temizleyin. |

### <a name="support-package-alerts"></a>Destek paketi uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemler |
|:--- |:--- |:--- |
| Destek paketinin oluşturulması başarısız oldu. |StorSimple paketi oluşturamadı. |Bu operasyonu yeniden deneyin. Sorun devam ederse, Microsoft Destek'e başvurun. Sorunu çözdükten sonra, bu uyarıyı uyarılar sayfasından temizleyin. |

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple hataları ve sorun giderme aygıt dağıtım sorunları](storsimple-8000-troubleshoot-deployment.md)hakkında daha fazla bilgi edinin.

