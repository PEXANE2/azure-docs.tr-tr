---
title: Microsoft Authenticator uygulamasına parolaları içeri aktarma-Azure AD
description: Popüler parola yöneticilerinden parolaları Microsoft kimlik doğrulama uygulamasına aktarma.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 01/28/2021
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: ecc6580148dfba92077336a26ff9160fbe88eb2c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "99806164"
---
# <a name="import-passwords-into-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasına parolaları içeri aktarma

Microsoft Authenticator Google Chrome, Firefox, LastPass, Bitwarden ve Roboford 'dan parolaların içeri aktarılmasını destekler. Microsoft şu anda mevcut parola yöneticinize destek vermezse, [şablon CSV 'imize oturum açma kimlik bilgilerini el ile girebilirsiniz](https://go.microsoft.com/fwlink/?linkid=2134938). Mevcut parolalarınızı içeri aktarıp kimlik doğrulayıcı uygulamasında yönetmek için, parolalarınızı mevcut parola yöneticinizin virgülle ayrılmış değerler (CSV) biçimine aktarmanız yeterlidir. Daha sonra, dışarı aktarılan CSV 'yi Chrome tarayıcı uzantımızda veya doğrudan Doğrulayıcı uygulamasına (Android ve iOS) kimlik doğrulayıcısına aktarın.

## <a name="import-from-google-chrome-or-android-smart-lock"></a>Google Chrome veya Android Akıllı Kilit içeri aktar

Google Chrome veya Android Akıllı Kilit 'teki parolalarınızı, akıllı telefoninizdeki veya masaüstü bilgisayarınızdaki kimlik doğrulayıcısına aktarabilirsiniz. Seçenekleriniz şunlardır:

- [Android ve iOS 'da Chrome 'dan içeri aktarma](#import-from-chrome-on-android-and-ios)
- [Chrome masaüstü tarayıcısından içeri aktar](#import-from-chrome-desktop-browser)

### <a name="import-from-chrome-on-android-and-ios"></a>Android ve iOS 'da Chrome 'dan içeri aktarma

Android ve Apple telefonlarında Google Chrome kullanıcıları, birkaç basit adımla parolalarını doğrudan telefonlarından içeri aktarabilir.

1. Kimlik doğrulayıcı uygulamasını telefonunuza yükleyip **parolalar** sekmesini açın.

1. Telefonunuzda Google Chrome 'da oturum açın.

1. ![ ](./media/user-help-authenticator-app-import-passwords/ellipsis-chrome.png) Android telefonlar için sağ üst köşedeki Google Chrome üç nokta menüsüne veya iOS cihazları için sağ alt kısmına dokunun ve ardından Ayarlar ' a dokunun **.**

   Platform | Bağlantı
   ---------- | --------
   Android | ![Google Chrome ayarları menü konumu](./media/user-help-authenticator-app-import-passwords/android-settings-menu.png)
   iOS | ![Google Chrome ayarları menü simgesi](./media/user-help-authenticator-app-import-passwords/apple-settings-menu.png)

1. **Ayarlar**' da, **parolalar**' ı açın.

   Platform | Bağlantı
   ---------- | --------
   Android | ![Andoıd Chrome parolaları komut konumu](./media/user-help-authenticator-app-import-passwords/android-passwords-location.png)
   iOS | ![Apple Chrome parolaları komut konumu](./media/user-help-authenticator-app-import-passwords/apple-passwords-location.png)

1. Android cihazlarda, ![ ](./media/user-help-authenticator-app-import-passwords/ellipsis-chrome.png) Android telefonlar için sağ üst köşedeki Google Chrome üç nokta menüsüne veya iOS cihazları için sağ alt kısmına dokunun ve ardından **Parolaları dışa aktar**' a dokunun.

   Platform | Bağlantı
   ---------- | --------
   Android | ![Android Chrome dışa aktarma parolaları konumu](./media/user-help-authenticator-app-import-passwords/android-export-passwords-location.png)
   iOS | ![Apple Chrome dışa aktarma parolaları konumu](./media/user-help-authenticator-app-import-passwords/apple-export-passwords-location.png)

   PIN, parmak izi veya yüz tanıma sağlamanız gerekir. Dışarı aktarmaya başlamak için kimliğinizi doğrulayın ve **Parolaları yeniden ver** ' e dokunun.

1. Parolalar verildikten sonra Chrome, içine aktardığınız uygulamayı seçmenizi ister. Parolaları içeri aktarmaya başlamak için **kimlik doğrulayıcı** ' yı seçin. Tamamlandığında içeri aktarma durumu hakkında bilgilenirsiniz.

   Platform | Bağlantı
   ---------- | --------
   Android | ![Android Chrome içeri aktarma parolaları konumu](./media/user-help-authenticator-app-import-passwords/android-chrome-import.png)
   iOS | ![Apple Chrome içeri aktarma parolaları konumu](./media/user-help-authenticator-app-import-passwords/apple-chrome-import.png)

### <a name="import-from-chrome-desktop-browser"></a>Chrome masaüstü tarayıcısından içeri aktar

Başlamadan önce, Chrome tarayıcınızda [Microsoft otomatik doldurma uzantısını](https://chrome.google.com/webstore/detail/microsoft-autofill/fiedbfgcleddlbcmgdigjgdfcggjcion) yüklemeli ve oturum açmalısınız.

1. [Google Password Manager](https://passwords.google.com) 'ı herhangi bir tarayıcıda açın. Henüz yapmadıysanız, Google hesabınızda oturum açın.

1. Dişli simgesini seçin ![Masaüstü parolası Yöneticisi dişli simgesi](./media/user-help-authenticator-app-import-passwords/desktop-password-manager-gear.png) Parola ayarları sayfasını açmak için.

1. **Dışarı aktar**' ı seçin, ardından bir sonraki sayfada parolalarınızı dışarı aktarmaya başlamak Için **dışarı aktar** ' ı seçin. Kimliğinizi onaylamanız istendiğinde Google parolanızı sağlayın. Tamamlandığında içeri aktarma durumu hakkında bilgilenirsiniz.

   ![Masaüstü Chrome tarayıcı parolaları dışa aktarma komut konumu](./media/user-help-authenticator-app-import-passwords/desktop-chrome-export-passwords-location.png)

1. Otomatik doldurma Chrome uzantısını açın ve **Ayarlar**' ı seçin.

   ![Masaüstü Chrome tarayıcısı uzantı ayarları konumunu otomatik doldur](./media/user-help-authenticator-app-import-passwords/desktop-chrome-autofill-settings.png)

1. Bir iletişim kutusu açmak için **verileri Içeri aktar** ' ı seçin. Ardından, CSV dosyasını bulmak ve içeri aktarmak için **Dosya Seç** ' i seçin.

   ![Masaüstü Chrome tarayıcısı veri CSV konumunu Içeri aktarma](./media/user-help-authenticator-app-import-passwords/desktop-chrome-import-csv.png)

## <a name="import-from-firefox"></a>Firefox 'dan içeri aktar

Firefox, parolaların yalnızca masaüstü tarayıcısından verilmesini sağlar, bu nedenle Firefox 'tan parolaları içeri aktarmadan önce Firefox Masaüstü tarayıcısına erişiminiz olduğundan emin olun.

1. Masaüstünüzdeki Firefox 'un en son sürümünde oturum açın ve şunu seçin ![Firefox "hamburger" menüsü](./media/user-help-authenticator-app-import-passwords/desktop-firefox-ellipsis-icon.png) ekranın sağ üst köşesindeki menü.

1. **Oturum açmaları ve parolalar ' ı** seçin.

   ![Masaüstü Firefox tarayıcı oturum açma ve parola konumu](./media/user-help-authenticator-app-import-passwords/desktop-firefox-passwords-location.png)

1. Firefox Lockwise sayfasında, ![ Firefox üç nokta menü ](./media/user-help-authenticator-app-import-passwords/desktop-firefox-ellipsis-icon.png) menüsünü seçin, **oturum açma işlemlerini** seçin ve ardından **dışarı aktar**' ı seçerek amacınızı onaylayın. PIN 'inizi, cihaz parolanızı girerek veya parmak izlerinizi tarayarak kendiniz belirlemeniz istenir. Firefox başarıyla tanımlandıktan sonra, parolalarınızı CSV biçiminde seçilen konuma dışarı aktarır.

   ![Masaüstü Firefox tarayıcı parola konumunu dışarı aktarma](./media/user-help-authenticator-app-import-passwords/desktop-firefox-export-passwords-location.png)

1. Parolalarınızı bir masaüstü tarayıcısından veya iOS veya Android telefonlarından Authenticator halinde içeri aktarabilirsiniz. Telefonunuzdaki kimlik doğrulayıcı uygulamasına aktarmak için:

      1. Tercih edilen ve güvenli bir yöntem kullanarak Android veya iOS telefonunuzdaki içe aktarılmış CSV dosyasını aktarın ve ardından indirin. Sonra, içeri aktarmayı başlatmak için CSV dosyasını Authenticator uygulamasıyla paylaşabilirsiniz.

         Platform | Bağlantı
         ---------- | --------
         Android | ![Android Chrome içeri aktarma parolaları konumu](./media/user-help-authenticator-app-import-passwords/android-chrome-import.png)
         iOS | ![Apple Chrome içeri aktarma parolaları konumu](./media/user-help-authenticator-app-import-passwords/apple-chrome-import.png)

      1. Parolanızı kimlik doğrulayıcısına başarıyla aldıktan sonra, masaüstünüzdeki veya cep telefonunuzdaki CSV dosyasını silin.

## <a name="import-from-lastpass"></a>LastPass 'dan içeri aktar

LastPass yalnızca bir masaüstü tarayıcısından parolayı dışarı aktarmayı destekler, bu nedenle parolaları içeri aktarmaya başlamadan önce bir masaüstü tarayıcısına erişiminiz olduğundan emin olun.

1. [LastPass Web sitesinde](https://lastpass.com) oturum açın ve **Gelişmiş Seçenekler**' i seçin ve ardından **dışarı aktar**' ı seçin.

   ![Masaüstü LastPass Parola konumunu dışarı aktarma](./media/user-help-authenticator-app-import-passwords/desktop-lastpass-export-passwords-location.png)

1. Ana parolanızı belirterek kendinize kendiniz tanıtın. Bundan sonra, Web sayfasında, verdiğiniz parolaları görürsünüz.

1. Web sayfasının içeriğini kopyalayın.

1. Not defteri 'Ni (veya sık kullandığınız metin düzenleyiciyi) açın ve kopyalanmış içeriği yapıştırın.

1. **Dosya** &gt; **farklı kaydet**' i seçerek bu not defteri dosyasını kaydedin. Masaüstünüzdeki güvenli bir konuma ". csv" (LastPass.csv gibi) ile biten bir ad sağlayın.

   ![Masaüstü LastPass CSV dosyasını Kaydet](./media/user-help-authenticator-app-import-passwords/desktop-lastpass-save-import-file.png)

1. Parolalarınızı bir masaüstü tarayıcısında veya iOS veya Android telefonlarında kimlik doğrulayıcısına aktarabilirsiniz. Telefonunuzdaki kimlik doğrulayıcı uygulamasına aktarmak için:

      1. Bir tercih edilen ve güvenli şekilde, dışarıya aktarılmış CSV dosyasını akıllı telefonunuzda aktarın ve indirin. Ardından içeri aktarmayı başlatmak için CSV dosyasını Authenticator uygulamasıyla paylaşabilirsiniz.

         Platform | Bağlantı
         ---------- | --------
         Android | ![Android LastPass parolaları içeri aktarma konumu](./media/user-help-authenticator-app-import-passwords/android-chrome-import.png)
         iOS | ![Apple LastPass parolalarını içeri aktarma konumu](./media/user-help-authenticator-app-import-passwords/apple-chrome-import.png)

      1. Parolanızı kimlik doğrulayıcısına başarıyla aldıktan sonra, masaüstünüzdeki veya cep telefonunuzdaki CSV dosyasını silin.

## <a name="import-from-bitwarden"></a>Bitwarden içeri aktar

Bitwarden parolaları yalnızca bir masaüstü tarayıcısından dışarı aktarmayı destekler, bu nedenle parolaları içeri aktarmaya başlamadan önce bir masaüstü tarayıcısına erişiminiz olduğundan emin olun.

1. ' Da oturum açın https://vault.bitwarden.com/ ve **araçları** &gt; **dışarı aktar**' ı seçin. CSV olarak dosya biçimini seçin, ana parolanızı girin ve dışarı aktarmaya başlamak için **kasayı dışarı aktar** ' ı seçin.

   ![Bit Warden dışa aktarma kasa konumu](./media/user-help-authenticator-app-import-passwords/desktop-bitwarden-export-command-location.png)

1. Parolalarınızı bir masaüstü tarayıcısında veya iOS veya Android telefonlarında kimlik doğrulayıcısına aktarabilirsiniz. Telefonunuzdaki kimlik doğrulayıcı uygulamasına aktarmak için:

      1. Bir tercih edilen ve güvenli şekilde, dışarıya aktarılmış CSV dosyasını akıllı telefonunuzda aktarın ve indirin. Ardından içeri aktarmayı başlatmak için CSV dosyasını Authenticator uygulamasıyla paylaşabilirsiniz.

         Platform | Bağlantı
         ---------- | --------
         Android | ![Android Bitwarden Import parola konumu](./media/user-help-authenticator-app-import-passwords/android-chrome-import.png)
         iOS | ![Apple Bitwarden Import parola konumu](./media/user-help-authenticator-app-import-passwords/apple-chrome-import.png)

      1. Parolanızı kimlik doğrulayıcısına başarıyla aldıktan sonra, masaüstünüzdeki veya cep telefonunuzdaki CSV dosyasını silin.

## <a name="import-from-roboform"></a>Roboford 'den içeri aktar

Roboford, parolaların yalnızca masaüstü uygulamasından dışarı aktarılmasını sağlar, bu yüzden içeri aktarmaya başlamadan önce bir masaüstündeki Robofore uygulamasına erişiminizin olduğundan emin olun.

1. Masaüstü istemcinizden Roboford 'yi başlatın ve hesabınızda oturum açın.

1. **Roboford** menüsündeki **seçenekleri** belirleyin.

   ![Desktop Roboford Seçenekler menüsü](./media/user-help-authenticator-app-import-passwords/desktop-roboform-options.png)

1. **Hesap & verileri** &gt; **dışarı aktarma**' yı seçin.

   ![Desktop Roboford dışarı aktarma komut konumu](./media/user-help-authenticator-app-import-passwords/desktop-roboform-accounts-data.png)

1. Verdiğiniz dosyayı kaydetmek için güvenli bir konum seçin. **Veri** türü olarak **Oturumlar** ' ı seçin ve CSV dosyasını biçim olarak seçin ve ardından **dışarı aktar**' ı seçin.

   ![Desktop Roboford dışarı aktarma iletişim kutusu](./media/user-help-authenticator-app-import-passwords/desktop-roboform-export-dialog.png)

1. Amacınızı onaylayın ve CSV dosyası daha sonra seçili konuma aktarıldığından emin olun.

   ![Desktop Roboford dışarı aktarma onayı iletişim kutusu](./media/user-help-authenticator-app-import-passwords/desktop-roboform-confirmation.png)

1. Parolalarınızı bir masaüstü tarayıcısında veya iOS veya Android telefonlarında kimlik doğrulayıcısına aktarabilirsiniz. Telefonunuzdaki kimlik doğrulayıcı uygulamasına aktarmak için:

      1. Bir tercih edilen ve güvenli şekilde, dışarıya aktarılmış CSV dosyasını akıllı telefonunuzda aktarın ve indirin. Ardından içeri aktarmayı başlatmak için CSV dosyasını Authenticator uygulamasıyla paylaşabilirsiniz.

         Platform | Bağlantı
         ---------- | --------
         Android | ![Android Roboford parola konumunu al](./media/user-help-authenticator-app-import-passwords/android-chrome-import.png)
         iOS | ![Apple Roboford parola konumunu al](./media/user-help-authenticator-app-import-passwords/apple-chrome-import.png)

      1. Parolanızı kimlik doğrulayıcısına başarıyla aldıktan sonra, masaüstünüzdeki veya cep telefonunuzdaki CSV dosyasını silin.

## <a name="import-by-creating-a-csv"></a>CSV oluşturarak içeri aktarma

Parola yöneticinizin parolalarını içeri aktarma adımları Bu makalede listelenmiyorsa, parolalarınızı doğrulayıcıya aktarmak için kullanabileceğiniz bir CSV oluşturabilirsiniz. Microsoft, biçimlendirme kolaylığı için bu adımları bir masaüstünde izlemenizi önerir.

1. Masaüstünüzde [içeri aktarma şablonumuzu indirip açın](https://go.microsoft.com/fwlink/?linkid=2134938). Apple iPhone, Safari ve anahtarlık Kullanıcı kullanıyorsanız, şimdi 4. adıma geçebilirsiniz.

1. Parolalarınızı şifreli olmayan bir CSV dosyasında mevcut parola yöneticisinden dışarı aktarın.

1. İçe aktarılmış CSV 'nizden ilgili sütunları şablon CSV 'ye kopyalayın ve kaydedin.

1. İçe aktarılmış bir CSV 'niz yoksa, var olan parola yöneticinizin her bir oturumunu şablon CSV 'ye kopyalayabilirsiniz. Üst bilgi satırını kaldırmayın veya değiştirmeyin. Bitirdiğinizde, bir sonraki adıma başlamadan önce verilerinizin bütünlüğünü doğrulayın.

1. Parolalarınızı bir masaüstü tarayıcısında veya iOS veya Android telefonlarında kimlik doğrulayıcısına aktarabilirsiniz. Telefonunuzdaki kimlik doğrulayıcı uygulamasına aktarmak için:

      1. Bir tercih edilen ve güvenli şekilde, dışarıya aktarılmış CSV dosyasını akıllı telefonunuzda aktarın ve indirin. Ardından içeri aktarmayı başlatmak için CSV dosyasını Authenticator uygulamasıyla paylaşabilirsiniz.

         Platform | Bağlantı
         ---------- | --------
         Android | ![Android CSV içeri aktarma parolaları konumu](./media/user-help-authenticator-app-import-passwords/android-chrome-import.png)
         iOS | ![Apple CSV içeri aktarma parolaları konumu](./media/user-help-authenticator-app-import-passwords/apple-chrome-import.png)

      1. Parolanızı kimlik doğrulayıcısına başarıyla aldıktan sonra, masaüstünüzdeki veya cep telefonunuzdaki CSV dosyasını silin.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

Başarısız içeri aktarmaların en yaygın nedeni, CSV dosyasında yanlış biçimlendirmedir. Sorunu gidermek için aşağıdaki adımları deneyebilirsiniz.

- Geçerli parola yöneticinizin parolalarını içeri aktarmayı zaten desteklediğimiz hakkında bilgi için bu makaleye bakın. Bunu yapabiliriz, ilgili sağlayıcınız için bahsedilen adımları izleyerek içeri aktarma işlemini yeniden denemek isteyebilirsiniz.

- Şu anda parola yöneticinizin biçimini içeri aktarmayı desteklemiyorsa, [CSV dosyanızı el ile oluşturarak](#import-by-creating-a-csv)yeniden deneyebilirsiniz.

- Aşağıdaki önerilerden CSV verilerinin bütünlüğünü doğrulayabilirsiniz:

  - İlk satır üç sütunlu bir üst bilgi içermelidir: **URL**, **Kullanıcı adı** ve **parola**.

  - Her satır, **URL** ve **parola** sütunları altında bir değer içermelidir.

- İçeriğinizi [CSV şablon dosyasına](https://go.microsoft.com/fwlink/?linkid=2134938)yapıştırarak CSV 'yi yeniden oluşturabilirsiniz.

- Başka bir şey çalışmadıysanız, lütfen kimlik doğrulayıcı uygulama ayarlarından **geri bildirim gönder** bağlantısını kullanarak sorununuzu bildirin.
