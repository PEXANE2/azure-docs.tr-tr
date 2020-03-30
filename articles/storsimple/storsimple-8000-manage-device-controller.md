---
title: StorSimple 8000 serisi cihaz denetleyicilerini yönetin | Microsoft Dokümanlar
description: StorSimple aygıt denetleyicilerinizi nasıl durduracağınız, yeniden başlatacağınız, kapatacağınız veya sıfırlamayı öğrenin.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/19/2017
ms.author: alkohli
ms.openlocfilehash: ce49dcaa06288ba9e7a4d232338c727064d59685
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267774"
---
# <a name="manage-your-storsimple-device-controllers"></a>StorSimple cihaz denetleyicilerinizi yönetme

## <a name="overview"></a>Genel Bakış

Bu öğretici, StorSimple aygıt denetleyicilerinizde gerçekleştirilebilecek farklı işlemleri açıklar. StorSimple aygıtınızdaki denetleyiciler, etkin-pasif yapılandırmada yedek (eş) denetleyicilerdir. Belirli bir zamanda, yalnızca bir denetleyici etkindir ve tüm disk ve ağ işlemlerini işler. Diğer denetleyici pasif modda. Etkin denetleyici başarısız olursa, pasif denetleyici otomatik olarak etkin olur.

Bu öğretici, aşağıdakileri kullanarak aygıt denetleyicilerini yönetmek için adım adım yönergeler içerir:

* StorSimple Device Manager hizmetinde cihazınıziçin **denetleyiciler.**
* StorSimple için Windows PowerShell.

Cihaz denetleyicilerini StorSimple Device Manager hizmeti aracılığıyla yönetmeniz. Bir eylem yalnızca StorSimple için Windows PowerShell kullanılarak gerçekleştirilebiliyorsa, öğretici bunu not eder.

Bu öğretici okuduktan sonra, mümkün olacak:

* StorSimple aygıt denetleyicisini yeniden başlatma veya kapatma
* StorSimple aygıtını kapatma
* StorSimple cihazınızı fabrika varsayılanlarına sıfırlama

## <a name="restart-or-shut-down-a-single-controller"></a>Tek bir denetleyiciyi yeniden başlatma veya kapatma
Denetleyici, normal sistem çalışmasının bir parçası olarak yeniden başlatma veya kapatma gerektirmez. Tek bir aygıt denetleyicisi için kapatma işlemleri yalnızca başarısız bir aygıt donanım bileşeninin değiştirilmesi gereken durumlarda yaygındır. Performansın aşırı bellek kullanımından veya arızalı bir denetleyiciden etkilendiği bir durumda denetleyicinin yeniden başlatılması da gerekebilir. Değiştirilen denetleyiciyi etkinleştirmek ve test etmek istiyorsanız, başarılı bir denetleyici değişiminden sonra bir denetleyiciyi yeniden başlatmanız gerekebilir.

Pasif denetleyicinin kullanılabilir olduğu varsayılırsa, cihazın yeniden başlatılması bağlı başlatıcılarda kesintiye neden olmaz. Pasif denetleyici kullanılabilir değilse veya kapatılmışsa, etkin denetleyicinin yeniden başlatılması hizmet kesintisine ve kapalı kalma süresine yol açabilir.

> [!IMPORTANT]
> * **Çalışan bir denetleyici, fazlalık kaybına ve daha fazla kesinti riskine neden olacağından fiziksel olarak asla kaldırılmaz.**
> * Aşağıdaki yordam yalnızca StorSimple fiziksel aygıtı için geçerlidir. StorSimple Cloud Appliance'ı nasıl başlatacak, durdurup yeniden başlatacağız hakkında bilgi için bulut [cihazıyla Çalışma 'ya](storsimple-8000-cloud-appliance-u2.md#work-with-the-storsimple-cloud-appliance)bakın.

StorSimple Aygıt Yöneticisi hizmetinin Azure portalı veya StorSimple için Windows PowerShell üzerinden tek bir aygıt denetleyicisini yeniden başlatabilir veya kapatabilirsiniz.

Aygıt denetleyicilerinizi Azure portalından yönetmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-restart-or-shut-down-a-controller-in-azure-portal"></a>Azure portalında bir denetleyiciyi yeniden başlatmak veya kapatmak için
1. StorSimple Aygıt Yöneticisi hizmetinizde **Cihazlar'a**gidin. Aygıtlar listesinden cihazınızı seçin. 

    ![Cihaz seçin](./media/storsimple-8000-manage-device-controller/manage-controller1.png)

2. Ayarlar **> Denetleyicileri'ne**gidin.
   
    ![StorSimple aygıt denetleyicilerinin sağlıklı olduğunu doğrulayın](./media/storsimple-8000-manage-device-controller/manage-controller2.png)
3. **Denetleyiciler** bıçağında, cihazınızdaki her iki denetleyicinin durumunun **Sağlıklı**olduğunu doğrulayın. Bir denetleyici seçin, sağ tıklatın ve sonra **Yeniden Başlat** veya **Kapat'ı**seçin.

    ![StorSimple aygıt denetleyicilerini yeniden başlat'ı seçin veya kapatın](./media/storsimple-8000-manage-device-controller/manage-controller3.png)

4. Denetleyiciyi yeniden başlatmak veya kapatmak için bir iş oluşturulur ve varsa geçerli uyarılar sunulur. Yeniden başlatma veya kapatmayı izlemek için **Hizmet > Etkinlik günlüklerine** gidin ve ardından hizmetinize özgü parametrelere göre filtre uygulayın. Bir denetleyici kapatıldıysa, açmak için kumandayı açmak için güç düğmesine basmanız gerekir.

#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell'deki bir denetleyiciyi yeniden başlatmak veya kapatmak için
StorSimple için Windows PowerShell'den StorSimple aygıtınızdaki tek bir denetleyiciyi kapatmak veya yeniden başlatmak için aşağıdaki adımları gerçekleştirin.

1. Aygıta seri konsol veya uzak bir bilgisayardan telnet oturumu üzerinden erişin. Denetleyici 0 veya Controller 1'e bağlanmak [için, aygıt seri konsoluna bağlanmak için PuTTY'yi kullan'daki](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console)adımları izleyin.
2. Seri konsol menüsünde, seçenek 1 seçin, **tam erişim ile giriş yapın.**
3. Banner iletisinde, bağlı olduğunuz denetleyiciye (Denetleyici 0 veya Denetleyici 1) ve etkin veya pasif (bekleme) denetleyici olup olmadığını not alın.
   
   * İstemde tek bir denetleyiciyi kapatmak için yazın:
     
       `Stop-HcsController`
     
       Bu, bağlı olduğunuz denetleyiciyi kapatır. Etkin denetleyiciyi durdurursanız, aygıt pasif denetleyiciye geçemezse.

   * Bir denetleyiciyi yeniden başlatmak için, istek te, yazın:
     
       `Restart-HcsController`
     
       Bu işlem bağlandığınız denetleyiciyi yeniden başlatır. Etkin denetleyiciyi yeniden başlatırsanız, yeniden başlatmadan önce pasif denetleyiciye geçemezse.

## <a name="shut-down-a-storsimple-device"></a>StorSimple aygıtını kapatma

Bu bölümde, çalışan veya başarısız bir StorSimple aygıtının uzak bir bilgisayardan nasıl kapatılabildiğini açıklar. Her iki aygıt denetleyicisi kapatıldıktan sonra bir aygıt kapatılır. Aygıt kapatma işlemi, aygıt fiziksel olarak taşındığında veya hizmet dışı kaldığında yapılır.

> [!IMPORTANT]
> Aygıtı kapatmadan önce aygıt bileşenlerinin sistem durumunu kontrol edin. Cihazınıza gidin ve ardından **Ayarlar > Donanım sağlık'ı**tıklatın. Durum **ve donanım sağlık** bıçağında, tüm bileşenlerin LED durumunun yeşil olduğunu doğrulayın. Yalnızca sağlıklı bir aygıt yeşil bir duruma sahiptir. Aygıtınız arızalı bir bileşeni değiştirmek için kapatılıyorsa, ilgili bileşen(ler) için başarısız (kırmızı) veya bozulmuş (sarı) bir durum görürsünüz.


#### <a name="to-shut-down-a-storsimple-device"></a>StorSimple aygıtını kapatmak için

1. Cihazınızdaki pasif denetleyiciyi tanımlamak ve kapatmak için yeniden başlat'ı kullanın veya bir denetleyici yordamını [kapatın.](#restart-or-shut-down-a-single-controller) Bu işlemi Azure portalında veya StorSimple için Windows PowerShell'de gerçekleştirebilirsiniz.
2. Etkin denetleyiciyi kapatmak için yukarıdaki adımı tekrarlayın.
3. Şimdi cihazın arka düzlemine bakmalısınız. İki denetleyici tamamen kapatıldıktan sonra, her iki denetleyicideki durum LED'leri kırmızı yanıp sönmelidir. Şu anda cihazı tamamen kapatmanız gerekiyorsa, hem Güç hem de Soğutma Modülleri'ndeki (PCM) güç anahtarlarını KAPALI konuma çevirin. Bu, aygıtı kapatmalıdır.

## <a name="reset-the-device-to-factory-default-settings"></a>Cihazı varsayılan fabrika ayarlarına sıfırlama

> [!IMPORTANT]
> Cihazınızı fabrika varsayılan ayarlarına sıfırlamanız gerekiyorsa, Microsoft Destek'e başvurun. Aşağıda açıklanan yordam yalnızca Microsoft Desteği ile birlikte kullanılmalıdır.

Bu yordam, Microsoft Azure StorSimple aygıtınızı StorSimple için Windows PowerShell kullanarak varsayılan ayarlara sıfırlamayı açıklar.
Aygıtı sıfırlamak varsayılan olarak tüm kümeden tüm verileri ve ayarları kaldırır.

Microsoft Azure StorSimple aygıtınızı varsayılan ayarlara sıfırlamak için aşağıdaki adımları gerçekleştirin:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell'de aygıtı varsayılan ayarlara sıfırlamak için
1. Aygıta seri konsolu üzerinden erişin. **Etkin** denetleyiciye bağlı olduğundan emin olmak için banner iletisini denetleyin.
2. Seri konsol menüsünde, seçenek 1 seçin, **tam erişim ile giriş yapın.**
3. İstemde, tüm verileri, meta verileri ve denetleyici ayarlarını kaldırarak tüm kümeyi sıfırlamak için aşağıdaki komutu yazın:
   
    `Reset-HcsFactoryDefault`
   
    Bunun yerine tek bir denetleyiciyi sıfırlamak için parametreli [Sıfırlama-HcsFactoryDefault](https://technet.microsoft.com/library/dn688132.aspx) cmdlet'ini `-scope` kullanın.)
   
    Sistem birden çok kez yeniden başlatılır. Sıfırlama başarıyla tamamlandığında size bildirilir. Sistem modeline bağlı olarak, bir 8100 cihaz için 45-60 dakika ve 8600 bu işlemi bitirmek için 60-90 dakika sürebilir.
   
## <a name="questions-and-answers-about-managing-device-controllers"></a>Cihaz denetleyicilerini yönetme ile ilgili sorular ve yanıtlar
Bu bölümde, StorSimple aygıt denetleyicilerinin yönetimi yle ilgili sık sorulan sorulardan bazılarını özetledik.

**S.** Aygıtımdaki her iki kumanda da sağlıklıysa ve açıksa ve etkin denetleyiciyi yeniden başlatıp kapatırsam ne olur?

**A.** Cihazınızdaki her iki denetleyici de sağlıklıysa ve açıksa, onay için istenirsiniz. Şunları seçebilirsiniz:

* **Etkin denetleyiciyi yeniden başlatın** - Etkin bir denetleyiciyi yeniden başlatmanın aygıtın pasif denetleyicide başarısız olması nedeniyle olduğu bildirilir. Denetleyici yeniden başlatılır.
* **Etkin bir denetleyiciyi kapatın** – Etkin bir denetleyicinin kapatılmasının kapalı kalma süresine neden olduğu bildirilir. Ayrıca kumandayı açmak için cihazdaki güç düğmesine basmanız gerekir.

**S.** Aygıtımdaki pasif denetleyici kullanılamıyorsa veya kapatılırsa ve etkin denetleyiciyi yeniden başlatıp kapatırsam ne olur?

**A.** Cihazınızdaki pasif denetleyici kullanılamıyorsa veya kapalıysa ve aşağıdakileri yapmayı tercih eyseniz:

* **Etkin denetleyiciyi yeniden başlatın** - İşleme devam ın hizmetin geçici olarak kesintiye uğramasına neden olacağı bildirilir ve onay için istenirsiniz.
* **Etkin bir denetleyiciyi kapatın** – İşleme devam ın kapalı kalma süresiyle sonuçlanması durumunda size bildirilir. Ayrıca aygıtı açmak için bir veya her iki kumandanın güç düğmesine basmanız gerekir. Onay için istenir.

**S.** Denetleyici yeniden başlatma veya kapatma ne zaman ilerleme kaydedilemez?

**A.** Aşağıdaki nedenler halinde bir denetleyiciyi yeniden başlatma veya kapatma başarısız olabilir:

* Aygıt güncelleştirmesi devam ediyor.
* Denetleyici yeniden başlatma zaten devam ediyor.
* Denetleyici kapatma işlemi devam ediyor.

**S.** Bir denetleyicinin yeniden başlatılıp başlatılmadı veya kapatıldığını nasıl anlayabilirsiniz?

**A.** Denetleyici bıçağındaki denetleyici durumunu kontrol edebilirsiniz. Denetleyici durumu, denetleyicinin yeniden başlatma veya kapatma işleminde olup olmadığını gösterir. Ayrıca, denetleyici yeniden başlatılırsa veya kapatılırsa **Uyarılar** bıçağı bir bilgilendirme uyarısı içerir. Denetleyici yeniden başlatma ve kapatma işlemleri de etkinlik günlüklerine kaydedilir. Etkinlik günlükleri hakkında daha fazla bilgi için [etkinlik günlüklerini görüntüleyin.](storsimple-8000-service-dashboard.md#view-the-activity-logs)

**S.** Denetleyicinin başarısız olması sonucu G/Ç'ye herhangi bir etkisi var mı?

**A.** Başlatıcılar ve etkin denetleyici arasındaki TCP bağlantıları, denetleyicinin başarısız olması sonucu sıfırlanır, ancak pasif denetleyici işlemi üstlendiğinde yeniden kurulacaktır. Bu işlem sırasında başlatıcılar ve aygıt arasında G/Ç etkinliğinde geçici (30 saniyeden az) bir duraklama olabilir.

**S.** Denetleyicim kapatıldıktan ve kaldırıldıktan sonra hizmete nasıl geri dönerim?

**A.** Bir denetleyiciyi servise döndürmek için, [StorSimple cihazınızdaki bir denetleyici](storsimple-8000-controller-replacement.md)modüldeğiştir'de açıklandığı gibi kasaya takmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* Bu öğreticide listelenen yordamları kullanarak çözemeyeceğiniz StorSimple aygıt denetleyicileri ile ilgili sorunlarla karşılaşırsanız, [Microsoft Destek'e başvurun.](storsimple-8000-contact-microsoft-support.md)
* StorSimple Aygıt Yöneticisi hizmetini kullanma hakkında daha fazla bilgi edinmek için [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanın.](storsimple-8000-manager-service-administration.md)

