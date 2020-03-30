---
title: StorSimple 8000 serisi için Destek bileti veya kılıfı oluşturma
description: Destek isteğini nasıl günlüğe kaydedin ve StorSimple 8000 serisi cihazınızda bir destek oturumu başlatın.
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: 99de3a6fbbbb1c4324df1712a5e24fd334ca4977
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254995"
---
# <a name="contact-microsoft-support"></a>Microsoft Desteği’ne başvurma

StorSimple Device Manager, hizmet özeti bıçak içinde **yeni bir destek isteği günlüğe kaydetme** olanağı sağlar. StorSimple çözümünüzle ilgili herhangi bir sorunla karşılaşırsanız, teknik destek için bir hizmet isteği oluşturabilirsiniz. Destek mühendisinizle çevrimiçi bir oturumda, StorSimple cihazınızda bir destek oturumu başlatmanız da gerekebilir. Bu makale size yol vereb:

* Destek isteği oluşturma.
* Portal içinden bir destek isteği yaşam döngüsünü nasıl yönetebilirsiniz.
* StorSimple cihazınızın Windows PowerShell arabiriminde destek oturumu başlatma.

Bir Destek isteği oluşturmadan önce [StorSimple 8000 Serisi Destek SLA'larını ve bilgilerini](https://msdn.microsoft.com/library/mt433077.aspx) inceleyin.

## <a name="create-a-support-request"></a>Destek isteği oluşturma

[Destek planınıza](https://azure.microsoft.com/support/plans/)bağlı olarak, StorSimple cihazınızdaki bir sorun için destek biletleri oluşturabilirsiniz. Destek isteği oluşturmak için aşağıdaki adımları gerçekleştirin:

#### <a name="to-create-a-support-request"></a>Destek isteği oluşturmak için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. Hizmet özeti bıçak **ayarlarında, DESTEK + SORUN GIDERME** bölümüne gidin ve ardından Yeni destek **isteğini**tıklatın.
     
    ![Yeni portal aracılığıyla MS Desteği ile iletişimkurun](./media/storsimple-8000-contact-microsoft-support/contactsupport1.png)
   
2. Yeni **destek isteği** bıçağında **Temeller'i**seçin. **Basics** bıçak, aşağıdaki adımları yapmak:
   1. Sorun **türü** açılır listesinden **Teknik'** i seçin.
   2. Geçerli **Abonelik,** **Hizmet** türü ve **Kaynak** (StorSimple Device Manager hizmeti) otomatik olarak seçilir. 
   3. Aboneliğinizle ilişkili birden çok planınız varsa, açılan listeden bir **Destek planı** seçin. Teknik Destek'i etkinleştirmek için ücretli bir destek planına ihtiyacınız var.
   4. **İleri**'ye tıklayın.

       ![Yeni portal aracılığıyla MS Desteği ile iletişimkurun](./media/storsimple-8000-contact-microsoft-support/contactsupport2.png)

3. Yeni **destek isteği** bıçağında **Adım 2 Sorunu'nu**seçin. **Problem** bıçağında aşağıdaki adımları yapın:
    
    1. Önem **Derecesini**seçin.
    2. Sorunun cihazla mı yoksa StorSimple Device Manager hizmetiyle mi ilgili olduğunu belirtin.
    3. Bu sorun için bir **Kategori** seçin ve sorun hakkında daha fazla **ayrıntı** sağlayın.
    4. Sorunun başlangıç tarihini ve saatini sağlayın.
    5. Dosya **yüklemesinde,** destek paketinize göz atmak için klasör simgesini tıklatın.
    6. **Tanı bilgilerini paylaş'ı**denetleyin.
    7. **İleri**'ye tıklayın.

       ![Yeni portal aracılığıyla MS Desteği ile iletişimkurun](./media/storsimple-8000-contact-microsoft-support/contactsupport3.png) 

4. Yeni **destek isteği** bıçağında, **Adım 3 İletişim bilgilerini**tıklatın. İletişim **bilgileri** bıçak, aşağıdaki adımları yapın:

   1. İletişim **seçeneklerinde,** tercih ettiğiniz iletişim yöntemini (telefon veya e-posta) ve dili sağlayın. Yanıt süresi, abonelik planınıza göre otomatik olarak seçilir.
   2. İletişim bilgileri, adınızı, e-posta, isteğe bağlı kişi, ülke / bölge sağlayın. Gelecekteki destek istekleri onay kutusu **için kişi değişikliklerini kaydet'i** seçin.
   3. **Oluştur'u**tıklatın.
   
       ![Yeni portal aracılığıyla MS Desteği ile iletişimkurun](./media/storsimple-8000-contact-microsoft-support/contactsupport5.png)   

      Microsoft Destek, daha fazla bilgi, tanı ve çözüm için size ulaşmak için bu bilgileri kullanır.
      Talebinizi gönderdikten sonra, bir Destek mühendisi isteğinize devam etmek için en kısa sürede sizinle irtibata geçecektir.

## <a name="manage-a-support-request"></a>Destek isteğini yönetme

Bir destek bileti oluşturduktan sonra portal üzerinden bu biletin yaşam döngüsünü yönetebilirsiniz.

#### <a name="to-manage-your-support-requests"></a>Destek isteklerinizi yönetmek için

1. Yardım ve destek sayfasına ulaşmak için **Gözat > Yardım + desteğe**gidin.

    ![Destek isteklerini yönetme](./media/storsimple-8000-contact-microsoft-support/managesupport1.png)

2. Tüm destek isteklerinin bir tabular listesi **Yardım + destek** bıçağında görüntülenir.

    ![Destek isteklerini yönetme](./media/storsimple-8000-contact-microsoft-support/managesupport2.png)

3. Bir destek isteğini seçin ve tıklatın. Bu isteğin durumunu ve ayrıntılarını görüntüleyebilirsiniz. Bu isteği takip etmek istiyorsanız **+ Yeni iletiyi** tıklatın.

    ![Destek isteklerini yönetme](./media/storsimple-8000-contact-microsoft-support/managesupport3.png)

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell'de destek oturumu başlatma

StorSimple aygıtıyla karşılaşabileceğiniz sorunları gidermek için Microsoft Destek ekibiyle etkileşimde bulunmanız gerekir. Microsoft Destek cihazınızda oturum açmak için bir destek oturumu kullanması gerekebilir.

Destek oturumu başlatmak için aşağıdaki adımları gerçekleştirin:

#### <a name="to-start-a-support-session"></a>Destek oturumu başlatmak için

1. Seri konsolu kullanarak veya uzak bir bilgisayardan telnet oturumu aracılığıyla doğrudan aygıta erişin. Bunu yapmak için, [aygıt seri konsoluna bağlanmak için PuTTY'yi kullan'daki](storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console)adımları izleyin.
2. Açılan oturumda, komut istemi almak için **Enter** tuşuna basın.
3. Seri konsol menüsünde, seçenek 1'i seçin, **tam erişimle giriş yapın.**
4. İsteyerek, aşağıdaki parolayı yazın:
   
    `Password1`
5. İsteyerek, aşağıdaki komutu yazın:
   
    `Enable-HcsSupportAccess`
6. Şifreli bir dize size sunulacaktır. Bu dizeyi Not Defteri gibi bir metin düzenleyicisine kopyalayın.
7. Bu dizeyi kaydedin ve Microsoft Destek'e bir e-posta iletisi gönderin.

> [!IMPORTANT]
> Çalıştırarak destek erişimini devre `Disable-HcsSupportAccess`dışı kullanabilirsiniz. StorSimple aygıtı, oturum başlatıldıktan 8 saat sonra destek erişimini devre dışı kaldırmaya da çalışır. Bir destek oturumu başlattıktan sonra StorSimple aygıt kimlik bilgilerinizi değiştirmek en iyi yöntemdir.


## <a name="next-steps"></a>Sonraki adımlar

[StorSimple 8000 serisi cihazınızla ilgili sorunları nasıl tanılayıp çözeceğinizi](storsimple-8000-troubleshoot-deployment.md) öğrenin
