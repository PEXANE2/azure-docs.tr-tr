---
title: include dosyası
description: include dosyası
services: notification-hubs
author: sethmanheim
ms.service: notification-hubs
ms.topic: include
ms.date: 02/10/2020
ms.author: sethm
ms.custom: include file
ms.openlocfilehash: a9e8574ea2d7222871c7f065383e6c0c62057dd3
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "81007844"
---
## <a name="generate-the-certificate-signing-request-file"></a>Sertifika imzalama isteği dosyasını oluşturma

Apple Anında Bildirim Hizmeti (APNs), anında iletme bildirimlerinizi doğrulamak için sertifikalar kullanır. Bildirim gönderip almak için gereken bildirim sertifikasını oluşturacak bu talimatları uygulayın. Bu kavramlar hakkında daha fazla bilgi için resmi [Apple Push Bildirim Hizmeti](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html) belgelerine bakın.

Apple'ın imzalı bir itme sertifikası oluşturmak için kullandığı Sertifika İmzalama İsteği (CSR) dosyasını oluşturun.

1. Mac’inizde Anahtar Zinciri Erişimi aracını çalıştırın. **Yardımcı Programlar** klasöründen veya Launchpad'deki **Diğer** klasörden açılabilir.

1. **Anahtar Zinciri Erişimi'ni**seçin, **Sertifika Yardımcısı'nı genişletin**ve ardından **Sertifika Yetkilisi'nden Sertifika İste'yi**seçin.

    ![Anahtarlık Erişimi kullanarak yeni sertifika isteğinde bulunma](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

   > [!NOTE]
   > Varsayılan olarak, Anahtar Zinciri Erişimi listedeki ilk öğeyi seçer. **Sertifikalar** kategorisindeyseniz ve **Apple Worldwide Developer Relations Certification Authority** listedeki ilk öğe değilse bu sorun olabilir. CSR (Sertifika İmzalama İsteği) oluşturmadan önce anahtar olmayan bir öğeye sahip olduğundan veya **Apple Worldwide Developer Relations Certification Authority** anahtarının seçildiğinden emin olun.

1. Kullanıcı **E-posta Adresinizi**seçin, **Ortak Ad** değerinizi girin, **diske Kaydedilmiş'yi**belirttiğinizden emin olun ve sonra **Devam et'i**seçin. **Ca E-posta Adresini** gerekli olmadığı için boş bırakın.

    ![Gerekli sertifika bilgileri](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)

1. **Kaydet'te**CSR dosyası için bir ad girin , **Where'deki**konumu seçin ve sonra **Kaydet'i**seçin.

    ![Sertifika için bir dosya adı seçme](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Bu eylem, CSR dosyasını seçili konuma kaydeder. Varsayılan konum **Masaüstü'dür.** Bu dosya için seçilen konumu unutmayın.

Ardından, uygulamanızı Apple'a kaydedin, anında iletme bildirimlerini etkinleştirin ve bir itme sertifikası oluşturmak için dışa aktarılan CSR'yi yükleyin.

## <a name="register-your-app-for-push-notifications"></a>Anında iletme bildirimleri için uygulamanızı kaydetme

Bir iOS uygulamasına anında iletme bildirimleri göndermek için uygulamanızı Apple'a kaydedin ve anında iletme bildirimleri için kaydolun.  

1. Uygulamanızı daha önce kaydetmediyseniz, Apple Geliştirici Merkezi'ndeki [iOS Sağlama Portalı'na](https://go.microsoft.com/fwlink/p/?LinkId=272456) göz atın. Apple Kimliğinizle portalda oturum açın ve **Tanımlayıcıları**seçin. Ardından **+** yeni bir uygulamayı kaydetmek için seçin.

    ![iOS Hazırlama Portalı Uygulama Kimlikleri sayfası](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. Yeni **Bir Tanımlayıcı Kaydol** ekranında, **Uygulama Kimlikleri** radyo düğmesini seçin. Daha sonra **Devam** seçeneğini belirleyin.

    ![iOS Sağlama Portalı yeni kimlik sayfası kaydetti](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids-new.png)

3. Yeni uygulamanız için aşağıdaki üç değeri güncelleyin ve ardından **Devam et'i**seçin:

   * **Açıklama**: Uygulamanız için açıklayıcı bir ad yazın.

   * **Paket Kimliği**: [Uygulama Dağıtım Kılavuzu'nda](https://help.apple.com/xcode/mac/current/#/dev91fe7130a)belirtildiği gibi **Kuruluş Tanımlayıcı.Ürün Adı** formunun paket kimliğini girin. *Kuruluş Tanımlayıcısı* ve *Ürün Adı* değerleri, Xcode projenizi oluştururken kullandığınız kuruluş tanımlayıcısı ve ürün adı ile eşleşmelidir. Aşağıdaki ekran **görüntüsünde, Bildirim Hub'ları** değeri bir kuruluş tanımlayıcısı olarak kullanılır ve **GetStarted** değeri ürün adı olarak kullanılır. Xcode'un doğru yayımlama profilini kullanması için **Paket Tanımlayıcı** değerinin Xcode projenizdeki değerle eşleştiğinden emin olun.

      ![iOS Sağlama Portalı kayıt uygulaması kimlik sayfası](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-bundle.png)

   * **Anında Iletme Bildirimleri**: **Özellikler** bölümündeki **Push Bildirimleri** seçeneğini işaretleyin.

      ![Yeni Uygulama Kimliği kaydetme formu](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-push.png)

      Bu eylem, Uygulama Kimliğinizi oluşturur ve bilgileri onaylamanızı ister. **Devam et'i**seçin, ardından yeni Uygulama Kimliğini onaylamak için **Kaydol'u** seçin.

      ![Yeni Uygulama Kimliğini onaylama](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-register.png)

      **Kaydol'u**seçtikten sonra, yeni Uygulama Kimliğini **Sertifikalar, Tanımlayıcılar & Profiller** sayfasında bir satır öğesi olarak görürsünüz.

4. **Sertifikalar'da, Tanımlayıcılar & Profiller** sayfasında, **Tanımlayıcılar**altında, yeni oluşturduğunuz App ID satır öğesini bulun ve **Uygulama Kimliği Yapılandırma ekranınızı düzenle** ekranını görüntülemek için satırını seçin.

## <a name="creating-a-certificate-for-notification-hubs"></a>Bildirim Hub'ları için Sertifika Oluşturma
Bildirim merkezinin **APNS**ile çalışmasını sağlamak için bir sertifika gereklidir. Bu iki şekilde yapılabilir:

1. Doğrudan Bildirim Hub'ına yüklenebilen bir **.p12** oluşturun.  
2. [Belirteç tabanlı kimlik doğrulaması](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-http2-token-authentification) *(yeni yaklaşım)* için kullanılabilecek bir **.p8** oluşturun.

Yeni [yaklaşımın, APNS için Belirteç tabanlı (HTTP/2) kimlik doğrulamasında](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-http2-token-authentification)belgelenen bir dizi avantajı (sertifika kullanmayla karşılaştırıldığında) vardır. Ancak, her iki yaklaşım için de adımlar sağlanmıştır. 

### <a name="option-1-creating-a-p12-push-certificate-that-can-be-uploaded-directly-to-notification-hub"></a>SEÇENEK 1: Doğrudan Bildirim Hub'ına yüklenebilen bir .p12 push sertifikası oluşturma

1. Denetlenen **Push Bildirimleri** seçeneğine gidin ve ardından sertifikayı oluşturmak için **Yapılandır'ı** seçin.

    ![Uygulama Kimliğini Düzenle sayfası](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)

2. **Apple Push Notification hizmeti SSL Sertifikaları** penceresi görüntülenir. **Geliştirme SSL Sertifikası** bölümünün altındaki **Sertifika Oluştur** düğmesini seçin.

    ![Uygulama Kimliği için sertifika oluştur düğmesi](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    **Yeni Sertifika Oluştur** ekranı görüntülenir.

    > [!NOTE]
    > Bu öğretici geliştirme sertifikası kullanır. Aynı işlem üretim sertifika kaydedildiğinde de kullanılır. Bildirimleri gönderirken aynı sertifika türünü kullandığınızdan kesinlikle emin olun.

3. **Dosya yı seçin,** CSR dosyasını ilk görevden kaydettiğiniz konuma göz atın ve ardından yüklemek için sertifika adını çift tıklatın. Daha sonra **Devam** seçeneğini belirleyin.

4. Portal sertifikayı oluşturduktan sonra **İndir** düğmesini seçin. Sertifikayı kaydedin ve kaydedildiği yeri hatırlayın.

    ![Oluşturulan sertifika indirme sayfası](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    Sertifika indirilir ve **İndirilenler** klasörünüzde bilgisayarınıza kaydedilir.

    ![İndirilenler klasöründe sertifika dosyasını bulma](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [!NOTE]
    > Varsayılan olarak, indirilen geliştirme sertifikası **aps_development.cer**olarak adlandırılır.

5. İndirilen **aps_development.cer** bildirim sertifikasına çift tıklayın. Bu eylem yeni sertifikayı Anahtar Zinciri’ne aşağıdaki resimde gösterildiği gibi yüklenir:

    ![Yeni sertifikanın gösterildiği anahtarlık erişim sertifikaları listesi](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [!NOTE]
    > Sertifikanızdaki ad farklı olsa da, ad **Apple Development iOS Push Services**ile önceden belirlenmiş olacaktır.

6. Anahtar Zinciri Erişimi’nde **Sertifikalar** kategorisinde oluşturduğunuz yeni bildirim sertifikasına sağ tıklayın. **Dışa Aktar'ı**seçin, dosyayı adlandırın, **.p12** biçimini seçin ve sonra **Kaydet'i**seçin.

    ![Sertifikayı p12 biçiminde dışarı aktarma](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)

    Sertifikayı parolayla korumayı seçebilirsiniz, ancak bu isteğe bağlıdır. Parola oluşturmayı atlamak istiyorsanız **Tamam'ı** tıklatın. Dışarı aktarılan .p12 sertifikanın dosya adını ve konumunu not edin. Bunlar, APN'lerle kimlik doğrulamayı etkinleştirmek için kullanılır.

    > [!NOTE]
    > .p12 dosya adınız ve konumunuz bu öğreticide görünenden farklı olabilir.

### <a name="option-2-creating-a-p8-certificate-that-can-be-used-for-token-based-authentication"></a>SEÇENEK 2: Belirteç tabanlı kimlik doğrulama için kullanılabilecek bir .p8 sertifikası oluşturma

1. Aşağıdaki ayrıntılara dikkat edin:

    - **Uygulama Kimliği Öneki** (bu bir **Takım Kimliğidir)**
    - **Paket Kimliği**
    
2. Geri **Sertifikalar, Tanımlayıcılar & Profiller**, **Tuşları**tıklatın .

   > [!NOTE]
   > **APNS**için yapılandırılan bir anahtarınız varsa, oluşturulduktan hemen sonra indirdiğiniz .p8 sertifikasını yeniden kullanabilirsiniz. Eğer öyleyse, **3** ile **5**arasında adımları yoksayabilirsiniz.

3. Yeni **+** bir tuş oluşturmak için düğmeye (veya **bir tuş oluştur** düğmesini) tıklatın.
4. Uygun bir **Anahtar Adı** değeri sağlayın, ardından Apple Push **Bildirimleri hizmetini (APN)** seçeneğini denetleyin ve ardından **Devam**et'i ve ardından bir sonraki ekranda **Kaydol'u** tıklatın.
5. **Karşıdan Yükleme'yi** tıklatın ve sonra **.p8** dosyasını *(AuthKey_* önceden belirlenmiş) güvenli bir yerel dizine taşıyın, ardından **Bitti'yi**tıklatın.

   > [!NOTE] 
   > .p8 dosyanızı güvenli bir yerde sakladığınızdan (ve yedekleme kaydettiğinizden) emin olun. Anahtarınızı indirdikten sonra, sunucu kopyası kaldırıldığıiçin yeniden indirilemez.
  
6. **Tuşlar'da,** yeni oluşturduğunuz tuşa (veya bunun yerine kullanmayı seçtiyseniz varolan bir tuşa) tıklayın.
7. **Anahtar Kimliği** değerine dikkat edin.
8. .p8 sertifikanızı [**Visual Studio Code**](https://code.visualstudio.com) gibi seçtiğiniz uygun bir uygulamada açın ve ardından anahtar değerine dikkat edin. Bu **-----BEGIN PRIVATE KEY-----** ve **-----END PRIVATE KEY-----** arasındaki değerdir.

    ```
    -----BEGIN PRIVATE KEY-----
    <key_value>
    -----END PRIVATE KEY-----
    ```

    > [!NOTE]
    > Bu, **Bildirim Hub'ını**yapılandırmak için daha sonra kullanılacak **belirteç değeridir.** 

Bu adımların sonunda, [bildirim hub'ınızı APN'ler bilgileriyle yapılandırın'](#configure-your-notification-hub-with-apns-information)da daha sonra kullanılmak üzere aşağıdaki bilgilere sahip olmalısınız:

- **Takım Kimliği** (bkz. adım 1)
- **Paket kimliği** (bkz. adım 1)
- **Anahtar Kimliği** (bkz. adım 7)
- **Belirteç değeri** yani .p8 tuşu değeri (bkz. adım 8)

## <a name="create-a-provisioning-profile-for-the-app"></a>Uygulama için bir sağlama profili oluşturun

1. [iOS Sağlama Portalına](https://go.microsoft.com/fwlink/p/?LinkId=272456)dönün, **Sertifikalar, Tanımlayıcılar & Profiller'i**seçin, sol menüden **Profiller'i** seçin ve ardından yeni bir profil oluşturmak için seçin. **+** **Yeni Bir Sağlama Profili Kaydol** ekranı görüntülenir.

1. Sağlama profili türü olarak **Geliştirme** altında **iOS Uygulama Geliştirme'yi** seçin ve ardından Devam **et'i**seçin.

    ![Sağlama profili listesi](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

1. Ardından, **Uygulama Kimliği** açılır listesinden oluşturduğunuz uygulama kimliğini seçin ve **Devam et'i**seçin.

    ![Uygulama Kimliği'ni seçin](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)

1. **Sertifikaları Seç** penceresinde, kod imzalama için kullandığınız geliştirme sertifikasını seçin ve **Devam et'i**seçin. Bu sertifika oluşturduğunuz itme sertifikası değildir. Biri yoksa, onu oluşturmanız gerekir. Sertifika varsa, bir sonraki adıma geçin. Yoksa geliştirme sertifikası oluşturmak için:

    1. **Sertifika bulunmadığını**görürseniz, Sertifika **Oluştur'u**seçin.
    2. **Yazılım** bölümünde Apple **Geliştirme'yi**seçin. Daha sonra **Devam** seçeneğini belirleyin.
    3. Yeni **Sertifika Oluştur** ekranında **Dosya'yı seçin'** i seçin.
    4. Daha önce oluşturduğunuz **Sertifika İmzalama İsteği** sertifikasına göz atın, seçin ve sonra **Aç'ı**seçin.
    5. **Devam**'ı seçin.
    6. Geliştirme sertifikasını indirin ve kaydedildiği konumu hatırlayın.

1. **Sertifikalar, Tanımlayıcılar & Profiller** sayfasına dönün, sol menüden **Profiller'i** **+** seçin ve ardından yeni bir profil oluşturmak için seçin. **Yeni Bir Sağlama Profili Kaydol** ekranı görüntülenir.

1. Sertifikaları **Seç** penceresinde, yeni oluşturduğunuz geliştirme sertifikasını seçin. Daha sonra **Devam** seçeneğini belirleyin.

1. Ardından, sınama için kullanılacak aygıtları seçin ve **Devam et'i**seçin.

1. Son olarak, Profil Adı **Sağlama'daki**profil için bir ad seçin ve **Oluştur'u**seçin.

    ![Sağlama profili adını seçin](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)

1. Yeni sağlama profili oluşturulduğunda **İndir'i**seçin. Kaydedildiği yeri hatırlayın.

1. Sağlama profilinin konumuna göz atın ve ardından Xcode geliştirme makinenize yüklemek için iki kez tıklatın.

## <a name="create-a-notification-hub"></a>Bildirim hub’ı oluşturma

Bu bölümde, bir bildirim hub'ı oluşturur ve .p12 push sertifikası veya belirteç tabanlı kimlik doğrulamasını kullanarak APN'lerle kimlik doğrulamayı yapılandırabilirsiniz. Önceden oluşturduğunuz bir bildirim hub'ı kullanmak istiyorsanız, adım 5'e atlayabilirsiniz.

[!INCLUDE [notification-hubs-portal-create-new-hub](notification-hubs-portal-create-new-hub.md)]

## <a name="configure-your-notification-hub-with-apns-information"></a>Bildirim merkezinizi APN bilgileriyle yapılandırma

**Bildirim Hizmetleri**altında, **Apple 'ı (APNS)** seçin ve bildirim [hub'ları için sertifika oluşturma](#creating-a-certificate-for-notification-hubs) bölümünde daha önce seçtiğiniz yaklaşımı temel alan uygun adımları izleyin.  

> [!NOTE]
> Uygulama Için **Üretim** **Modunu** yalnızca uygulamanızı mağazadan satın alan kullanıcılara anında iletme bildirimleri göndermek istiyorsanız kullanın.

### <a name="option-1-using-a-p12-push-certificate"></a>SEÇENEK 1: .p12 itme sertifikası kullanma

1. **Sertifika**’yı seçin.

1. Dosya simgesini seçin.

1. Daha önce dışa aktardığınız .p12 dosyasını seçin ve sonra **Aç'ı**seçin.

1. Gerekirse, doğru parolayı belirtin.

1. **Korumalı alan** modunu seçin.

    ![Azure portal'da APNs sertifikasını yapılandırma](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-apple-config-cert.png)

1. **Kaydet'i**seçin.

### <a name="option-2-using-token-based-authentication"></a>OPTION 2: Belirteç tabanlı kimlik doğrulamayı kullanma

1. **Token'ı**seçin.
1. Daha önce edindiğiniz aşağıdaki değerleri girin:

    - **Anahtar Kimliği**
    - **Paket Kimliği**
    - **Takım Kimliği**
    - **Belirte -ci** 

1. **Sandbox'ı** seçin
1. **Kaydet'i**seçin. 

Bildirim hub'ınızı artık APN'lerle yapılandırıldınız. Ayrıca, uygulamanızı kaydetmek ve anında iletme bildirimleri göndermek için bağlantı dizelerine de sahipsiniz.
