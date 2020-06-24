---
author: mikeparker104
ms.author: miparker
ms.date: 06/02/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: 72065ce602c6d29255a3500e26d8c6f36a19ebed
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85081557"
---
### <a name="register-your-ios-app-for-push-notifications"></a>İOS uygulamanızı anında iletme bildirimleri için kaydedin

Bir iOS uygulamasına anında iletme bildirimleri göndermek için uygulamanızı Apple 'a kaydedin ve anında iletme bildirimleri için kaydolun.  

1. Uygulamanızı henüz kaydetmediyseniz, Apple Geliştirici Merkezi ' nde [IOS sağlama portalı](https://go.microsoft.com/fwlink/p/?LinkId=272456) ' na gidin. Apple KIMLIĞINIZLE portalda oturum açın, **Sertifikalar, tanımlayıcılar & profiller**' e gidin ve **tanımlayıcılar**' ı seçin. **+** Yeni bir uygulama kaydetmek için tıklayın.

    ![iOS Hazırlama Portalı Uygulama Kimlikleri sayfası](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-ios-appids.png)

1. **Yeni bir tanımlayıcı kaydet** ekranında, **uygulama kimlikleri** radyo düğmesini seçin. Daha sonra **Devam** seçeneğini belirleyin.

    ![iOS sağlama portalı kayıt yeni KIMLIK sayfası](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-ios-appids-new.png)

1. Yeni uygulamanız için aşağıdaki üç değeri güncelleştirin ve ardından **devam**' ı seçin:

   * **Açıklama**: uygulamanız için açıklayıcı bir ad yazın.

   * **Paket kimliği**: **com. <organization_identifier>. <Product_name>** [uygulama dağıtım KıLAVUZUNDA](https://help.apple.com/xcode/mac/current/#/dev91fe7130a)belirtildiği gibi bir paket kimliği girin. Aşağıdaki ekran görüntüsünde, `mobcat` değer bir kuruluş tanımlayıcısı olarak ve **pushdemo** değeri de ürün adı olarak kullanılır.

      ![iOS sağlama portalı kayıt uygulama KIMLIĞI sayfası](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-new-appid-bundle.png)

   * **Anında Iletme bildirimleri**: **yetenekler** bölümündeki **anında iletme bildirimleri** seçeneğini işaretleyin.

      ![Yeni Uygulama Kimliği kaydetme formu](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-new-appid-push.png)

      Bu eylem, uygulama KIMLIĞINIZI ve bilgileri onaylamak için isteklerinizi oluşturur. **Devam**' ı seçin ve ardından yenı uygulama kimliğini onaylamak için **Kaydet** ' i seçin.

      ![Yeni uygulama KIMLIĞINI Onayla](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-new-appid-register.png)

      **Kaydet**' i seçtikten sonra, yenı uygulama kimliğini **sertifikalar, tanımlayıcılar & profiller** sayfasında bir satır öğesi olarak görürsünüz.

1. **Sertifikalar, tanımlayıcılar & profiller** sayfasında, **tanımlayıcılar**altında, oluşturduğunuz uygulama kimliği satır öğesini bulun. Ardından, kendi satırını seçerek **uygulama kimliği yapılandırmasını düzenleme** ekranınızı görüntüleyin.

#### <a name="creating-a-certificate-for-notification-hubs"></a>Notification Hubs için sertifika oluşturma

Bildirim Hub 'ının **Apple Push bildirim hizmetleri (APNs)** ile çalışmasını sağlamak için bir sertifika gerekir ve iki şekilde bulunabilir:

1. Doğrudan Bildirim Hub 'ına (*özgün yaklaşım*) [yüklenebilen bir P12 Push sertifikası oluşturma](#option-1-creating-a-p12-push-certificate-that-can-be-uploaded-directly-to-notification-hub)

1. [Belirteç tabanlı kimlik doğrulaması için kullanılabilecek bir P8 sertifikası oluşturma](#option-2-creating-a-p8-certificate-that-can-be-used-for-token-based-authentication) (*daha yeni ve önerilen yaklaşım*)

Yeni yaklaşım, [APNs Için belirteç tabanlı (http/2) kimlik doğrulamasında](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-http2-token-authentification)açıklanmak üzere çeşitli avantajlar içerir. Daha az adım gereklidir, ancak belirli senaryolar için de uygulanan. Ancak, Bu öğreticinin amaçları doğrultusunda çalışdıklarından her iki yaklaşım için de adımlar sunulmaktadır.

##### <a name="option-1-creating-a-p12-push-certificate-that-can-be-uploaded-directly-to-notification-hub"></a>SEÇENEK 1: doğrudan Bildirim Hub 'ına yüklenebilen bir P12 Push sertifikası oluşturma

1. Mac’inizde Anahtar Zinciri Erişimi aracını çalıştırın. Bu, **yardımcı programlar** klasöründen veya başlatma panelindeki **diğer** klasörden açılabilir.

1. **Anahtarlık erişimi**seçin, **sertifika Yardımcısı**' nı genişletin ve bir **sertifika yetkilisinden sertifika iste**' yi seçin.

    ![Anahtarlık Erişimi kullanarak yeni sertifika isteğinde bulunma](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

   > [!NOTE]
   > Varsayılan olarak, Anahtarlık erişimi listedeki ilk öğeyi seçer. Bu, **Sertifikalar** **kategorisindeyseniz ve Apple Dünya genelinde geliştirici Ilişkileri sertifika yetkilisiyseniz** listedeki ilk öğe değilse bir sorun olabilir. CSR 'yi (sertifika Imzalama Isteği) oluşturmadan önce, anahtar olmayan bir öğeye sahip olduğunuzdan veya **Apple Dünya genelinde geliştirici Ilişkileri sertifika yetkilisi** anahtarının seçildiğinden emin olun.

1. **Kullanıcı e-posta adresinizi**seçin, **ortak ad** değerini girin, **diske kaydedilmiş**' ı belirttiğinizden emin olun ve ardından **devam**' ı seçin. **CA e-posta adresini** gerekli olmadığından boş bırakın.

    ![Beklenen sertifika bilgileri](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-csr-info.png)

1. **Farklı kaydet**' de **sertifika IMZALAMA isteği (CSR) dosyası** için bir ad girin, **konum ' u**seçin ve ardından **Kaydet**' i seçin.

    ![Sertifika için bir dosya adı seçin](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Bu eylem, **CSR dosyasını** seçilen konuma kaydeder. Varsayılan konum **Desktop**' dır. Bu dosya için seçilen konumu unutmayın.

1. [IOS sağlama portalındaki](https://go.microsoft.com/fwlink/p/?LinkId=272456) **sertifikalar, tanımlayıcılar & profiller** sayfasına dönün, denetlenen **anında iletme bildirimleri** seçeneğine kaydırın ve ardından sertifikayı oluşturmak için **Yapılandır** ' ı seçin.

    ![Uygulama Kimliğini Düzenle sayfası](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-edit-appid.png)

1. **Apple Anında Iletilen bildirim HIZMETI TLS/SSL sertifikaları** penceresi görüntülenir. **GELIŞTIRME TLS/SSL sertifikası** bölümünün altında **sertifika oluştur** düğmesini seçin.

    ![Uygulama Kimliği için sertifika oluştur düğmesi](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    **Yeni sertifika oluştur** ekranı görüntülenir.

    > [!NOTE]
    > Bu öğretici geliştirme sertifikası kullanır. Aynı işlem üretim sertifika kaydedildiğinde de kullanılır. Bildirimleri gönderirken aynı sertifika türünü kullandığınızdan kesinlikle emin olun.

1. **Dosya Seç**' i seçin, **CSR dosyasını**kaydettiğiniz konuma gidin ve ardından bu sertifikayı yüklemek için sertifika adına çift tıklayın. Daha sonra **Devam** seçeneğini belirleyin.

1. Portal sertifikayı oluşturduktan sonra **İndir** düğmesini seçin. Sertifikayı kaydedin ve kaydedildiği konumu unutmayın.

    ![Oluşturulan sertifika indirme sayfası](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    Sertifika indirilir ve **indirmeler** klasörünüzde bilgisayarınıza kaydedilir.

    ![İndirilenler klasöründe sertifika dosyasını bulma](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [!NOTE]
    > Varsayılan olarak, indirilen geliştirme sertifikası **aps_development. cer**olarak adlandırılır.

1. İndirilen **aps_development.cer** bildirim sertifikasına çift tıklayın. Bu eylem yeni sertifikayı Anahtar Zinciri’ne aşağıdaki resimde gösterildiği gibi yüklenir:

    ![Yeni sertifikanın gösterildiği anahtarlık erişim sertifikaları listesi](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [!NOTE]
    > Sertifikanıza ilişkin ad farklı olabilir, ancak bu ada **Apple Development IOS anında Iletme Hizmetleri** ön eki alınır ve ilgili paket tanımlayıcısı onunla ilişkilendirilmiş olur.

1. Anahtarlık **erişimi ' nde,**  +  **Sertifikalar** kategorisinde oluşturduğunuz yeni gönderim sertifikasına**tıklayın** . **Dışarı aktar**' ı seçin, dosyayı adlandırın, **P12** biçimini seçin ve ardından **Kaydet**' i seçin.

    ![Sertifikayı p12 biçiminde dışarı aktarma](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)

    Sertifikayı parolayla korumayı seçebilirsiniz, ancak parola isteğe bağlıdır. Parola oluşturmayı atlamak istiyorsanız **Tamam** ' ı tıklatın. Aktarılmış P12 sertifikasının dosya adını ve konumunu bir yere unutmayın. Bunlar, APNs ile kimlik doğrulamayı etkinleştirmek için kullanılır.

    > [!NOTE]
    > P12 dosyanızın adı ve konumunuz, bu öğreticide görüntülenenden farklı olabilir.

##### <a name="option-2-creating-a-p8-certificate-that-can-be-used-for-token-based-authentication"></a>Seçenek 2: belirteç tabanlı kimlik doğrulaması için kullanılabilecek bir P8 sertifikası oluşturma

1. Aşağıdaki ayrıntıları unutmayın:

    * **Uygulama kimliği öneki** (**Takım Kimliği**)
    * **Paket Kimliği**

1. **Sertifikalar, tanımlayıcılar & profillere**geri dönüp **anahtarlar**' a tıklayın.

   > [!NOTE]
   > **APNs**için yapılandırılmış bir anahtarınız zaten varsa, oluşturulduktan sonra doğrudan indirdiğiniz P8 sertifikasını yeniden kullanabilirsiniz. Bu durumda **3** ile **5**arasındaki adımları yoksayabilirsiniz.

1. **+** Yeni bir anahtar oluşturmak için düğmeye tıklayın (veya **anahtar oluştur** düğmesi).
1. Uygun bir **anahtar adı** değeri sağlayın, ardından **Apple Anında iletilen BILDIRIM hizmeti (APNs)** seçeneğini işaretleyin ve ardından **devam**' a ve ardından bir sonraki ekrana **Kaydol** ' a tıklayın.
1. **İndir** ' e tıklayın ve ardından **p8** dosyasını ( *AuthKey_* ön eki) güvenli bir yerel dizine taşıyın ve **bitti**' ye tıklayın.

   > [!NOTE]
   > P8 dosyanızı güvenli bir yerde sakladığınızdan emin olun (ve bir yedek kaydedin). Anahtarınızı indirdikten sonra sunucu kopyası kaldırıldığından yeniden karşıdan yüklenemez.
  
1. **Anahtarlar**' da oluşturduğunuz anahtara (veya bunun yerine kullanmayı seçtiyseniz var olan bir anahtara) tıklayın.
1. **Anahtar kimliği** değerini bir yere getirin.
1. P8 sertifikanızı [**Visual Studio Code**](https://code.visualstudio.com)gibi tercih ettiğiniz uygun bir uygulamada açın. Anahtar değerini ( **-----Başlangıç özel anahtarı-----** ve **-----son özel anahtar-----**) unutmayın.

    -----ÖZEL ANAHTARA-----BAŞLA  
    <key_value>  
    -----SON ÖZEL ANAHTAR-----

    > [!NOTE]
    > Bu, daha sonra **Bildirim Hub 'ı**yapılandırmak için kullanılacak **belirteç değeridir** .

Bu adımların sonunda, [Bildirim Hub 'ıNıZı APNs bilgileriyle yapılandırma](#configure-your-notification-hub-with-apns-information)bölümünde daha sonra kullanmak için aşağıdaki bilgilere sahip olmanız gerekir:

* **Takım Kimliği** (bkz. 1. adım)
* **Paket kimliği** (bkz. 1. adım)
* **Anahtar kimliği** (bkz. Adım 7)
* **Belirteç değeri** (P8 anahtar değeri, 5. adımda elde edilir)

### <a name="create-a-provisioning-profile-for-the-app"></a>Uygulama için bir sağlama profili oluşturun

1. [IOS sağlama portalı](https://go.microsoft.com/fwlink/p/?LinkId=272456)' na dönün, **sertifikalar, tanımlayıcılar & profiller**' i seçin, sol menüden **profiller** ' i seçin ve ardından **+** Yeni bir profil oluşturmak için öğesini seçin. **Yeni bir sağlama profili kaydet** ekranı görüntülenir.

1. Sağlama profili türü olarak **geliştirme** bölümünde **iOS uygulama geliştirmeyi** seçin ve ardından **devam**' ı seçin.

    ![Sağlama profili listesi](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

1. Ardından, **uygulama kimliği** açılır listesinden oluşturduğunuz uygulama kimliğini seçin ve **devam**' ı seçin.

    ![Uygulama Kimliği'ni seçin](./media/notification-hubs-common-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)

1. **Sertifika seç** penceresinde, kod imzalama için kullandığınız geliştirme sertifikasını seçin ve **devam**' ı seçin.

    > [!NOTE]
    > Bu sertifika, [önceki adımda](#creating-a-certificate-for-notification-hubs)oluşturduğunuz gönderim sertifikası değil. Bu, geliştirme sertifikasıdır. Bir tane yoksa, bu öğretici için bir [Önkoşul](#prerequisites) olduğundan, oluşturmanız gerekir. Geliştirici sertifikaları, [Xcode](https://developer.apple.com/library/archive/documentation/ToolsLanguages/Conceptual/YourFirstAppStoreSubmission/ProvisionYourDevicesforDevelopment/ProvisionYourDevicesforDevelopment.html) veya [Visual Studio](https://docs.microsoft.com/xamarin/ios/get-started/installation/device-provisioning/)aracılığıyla [Apple Geliştirici Portalında](https://developer.apple.com)oluşturulabilir.

1. **Sertifikalar, tanımlayıcılar & profiller** sayfasına dönün, sol menüden **profiller** ' i seçin ve ardından **+** Yeni bir profil oluşturmak için öğesini seçin. **Yeni bir sağlama profili kaydet** ekranı görüntülenir.

1. **Sertifika seç** penceresinde, oluşturduğunuz geliştirme sertifikasını seçin. Daha sonra **Devam** seçeneğini belirleyin.

1. Ardından, test için kullanılacak cihazları seçin ve **devam**' ı seçin.

1. Son olarak, **sağlama profili adı**bölümünde profil için bir ad seçin ve **Oluştur**' u seçin.

    ![Sağlama profili adını seçin](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)

1. Yeni sağlama profili oluşturulduğunda **İndir**' i seçin. Kaydedildiği konumu unutmayın.

1. Sağlama profilinin konumuna gidin ve bunu geliştirme makinenize yüklemek için çift tıklayın.
