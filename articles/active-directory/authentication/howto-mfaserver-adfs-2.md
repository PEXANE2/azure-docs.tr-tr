---
title: AD FS 2.0 ile Azure MFA Server'ı kullanma - Azure Etkin Dizini
description: Bu, nasıl Azure MFA ve AD FS 2.0 kullanmaya başlayacağınızı açıklayan Azure Multi-Factor Authentication sayfasıdır.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: e71c1d28a90af72890b2399d5da24d08885f3cce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80051205"
---
# <a name="configure-azure-multi-factor-authentication-server-to-work-with-ad-fs-20"></a>Azure Multi-Factor Authentication Sunucusunu AD FS 2.0 ile çalışacak şekilde yapılandırma

Bu makale, Azure Active Directory ile birleştirilen ve kaynakları şirket içinde veya bulutta güvenli hale getirmek isteyen kuruluşlara yöneliktir. Kaynaklarınızı, Azure Multi-Factor Authentication Sunucusu’nu kullanarak ve değeri yüksek uç noktalarda iki aşamalı doğrulamanın tetiklenmesi için AD FS ile çalışacak şekilde yapılandırarak koruyun.

Bu belge AD FS 2.0 ile Multi-Factor Authentication Sunucusu kullanmayı ele alır. AD FS hakkında bilgi için bkz. [Windows Server 2012 R2 AD FS ile Azure Multi-Factor Authentication Sunucusu kullanarak bulut ve şirket içi kaynakları güvenli hale getirme](howto-mfaserver-adfs-2012.md).

> [!IMPORTANT]
> 1 Temmuz 2019 itibariyle Microsoft, yeni dağıtımlar için Artık MFA Server sunmayacak. Kullanıcılarından çok faktörlü kimlik doğrulaması isteyen yeni müşteriler bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmalıdır. 1 Temmuz'dan önce MFA Server'ı etkinleştirmiş olan mevcut müşteriler en son sürümü, gelecekteki güncelleştirmeleri karşıdan yükleyebilecek ve her zamanki gibi etkinleştirme kimlik bilgilerini oluşturabilecek.

## <a name="secure-ad-fs-20-with-a-proxy"></a>AD FS 2.0’ı bir ara sunucu ile güvenli hale getirme

Ara sunucu ile AD FS 2.0’ı güvenli hale getirmek için ADFS ara sunucusuna Azure Multi-Factor Authentication Sunucusu’nu yükleyin.

### <a name="configure-iis-authentication"></a>IIS kimlik doğrulamasını yapılandırma

1. Azure Multi-Factor Authentication Sunucusu’nun soldaki menüsünde **IIS Kimlik Doğrulaması** simgesine tıklayın.
2. **Form Tabanlı** sekmesine tıklayın.
3. **Ekle**’ye tıklayın.

   ![MFA Server IIS Kimlik Doğrulama penceresi](./media/howto-mfaserver-adfs-2/setup1.png)

4. Kullanıcı adı, parola ve etki alanı değişkenlerini otomatik olarak algılamak için, Otomatik Yapılandırma Formu Tabanlı Web Sitesi iletişim kutusuna giriş URL'sini (beğen) `https://sso.contoso.com/adfs/ls`girin ve **Tamam'ı**tıklatın.
5. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve iki aşamalı doğrulamaya tabi olacaksa **Azure Multi-Factor Authentication kullanıcılarının eşleşmesini gerektir** kutusunu işaretleyin. Sunucu’ya henüz aktarılmamış ve/veya iki aşamalı doğrulamadan muaf tutulacak çok sayıda kullanıcı varsa kutunun işaretini kaldırın.
6. Sayfa değişkenleri otomatik olarak algılanamıyorsa, **El Ile Belirt'i tıklatın...** Web Sitesi iletişim kutusunda Elle Belirt düğmesine tıklayın.
7. Form Tabanlı Web Sitesi Ekle iletişim kutusuna, URL Gönder alanındaki AD FS giriş `https://sso.contoso.com/adfs/ls`sayfasına URL girin (beğen) ve bir Uygulama adı girin (isteğe bağlı). Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir.
8. İstek biçimini **POST veya GET olarak**ayarlayın.
9. Kullanıcı adı değişkeni (ctl00$ContentPlaceHolder1$UsernameTextBox) ve Parola değişkenini (ctl00$ContentPlaceHolder1$PasswordTextBox) girin. Form tabanlı oturum açma sayfanız bir etki alanı metin kutusu görüntülerse, Etki alanı değişkenini de girin. Oturum açma sayfasında girdi kutularının adlarını bulmak için, bir web tarayıcısında oturum açma sayfasına gidin, sayfaya sağ tıklayın ve **Kaynağı Görüntüle**’yi seçin.
10. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve iki aşamalı doğrulamaya tabi olacaksa **Azure Multi-Factor Authentication kullanıcılarının eşleşmesini gerektir** kutusunu işaretleyin. Sunucu’ya henüz aktarılmamış ve/veya iki aşamalı doğrulamadan muaf tutulacak çok sayıda kullanıcı varsa kutunun işaretini kaldırın.

    ![MFA Server'a form tabanlı web sitesi ekleme](./media/howto-mfaserver-adfs-2/manual.png)

11. **Gelişmiş...** öğesine tıklayarak gelişmiş ayarları gözden geçirin. Yapılandırabileceğiniz ayarlar şunlardır:

    - Özel bir reddetme sayfa dosyası seçme
    - Tanımlama bilgilerini kullanarak web sitesinde başarılı kimlik doğrulamalarını önbelleğe alma
    - Birincil kimlik bilgilerini doğrulamanın nasıl gerçekleştirileceğini seçme

12. AD FS ara sunucusunun etki alanına katılması pek olası olmadığından, kullanıcı içeri aktarma ve ön kimlik doğrulama için etki alanı denetleyicisine bağlanmak amacıyla LDAP’yi kullanabilirsiniz. Gelişmiş Form Tabanlı Web Sitesi iletişim kutusunda, **Birincil Kimlik Doğrulama** sekmesini tıklatın ve Ön kimlik doğrulama kimlik doğrulama türü için **LDAP Bind'i** seçin.
13. Tamamlandığında Form Tabanlı Web Sitesi Ekle iletişim kutusuna dönmek için **Tamam**’a tıklayın.
14. **Tamam**’a tıklayarak iletişim kutusunu kapatın.
15. URL ve sayfa değişkenleri algılandığında veya girildiğinde, web sitesi verileri Form Tabanlı panelde görüntülenir.
16. IIS eklentisini istediğiniz düzeyde etkinleştirmek için **Yerel Modül** sekmesine tıklayın ve sunucuyu, AD FS ara sunucusunun altında çalıştığı web sitesini (“Varsayılan Web Sitesi” gibi) veya AD FS ara sunucu uygulamasını (“adfs” altındaki “ls” gibi) seçin.
17. Ekranın üst kısmındaki **IIS kimlik doğrulamasını etkinleştir** kutusunu tıklatın.

IIS kimlik doğrulaması şimdi etkinleştirildi.

### <a name="configure-directory-integration"></a>Dizin tümleştirmesini yapılandırma

IIS kimlik doğrulamasını etkinleştirdiniz, ancak LDAP aracılığıyla Active Directory’de (AD) ön kimlik doğrulama yapabilmek için etki alanı denetleyicisi için LDAP bağlantısı yapılandırmanız gerekir.

1. **Dizin Tümleştirme** simgesine tıklayın.
2. Ayarlar sekmesinde **Özel LDAP yapılandırması kullan** radyo düğmesini seçin.

   ![LdAP ayarlarını belirli LDAP ayarları için yapılandırın](./media/howto-mfaserver-adfs-2/ldap1.png)

3. **Düzenle**’ye tıklayın.
4. LDAP Yapılandırmasını Düzenle iletişim kutusunda, AD etki alanı denetleyicisine bağlanmak için gerekli bilgilerle alanları doldurun. Bu alanların açıklamaları Azure Multi-Factor Authentication Sunucusu yardım dosyasında da bulunmaktadır.
5. **Test** düğmesine tıklayarak LDAP bağlantısını test edin.

   ![MFA Server'da LDAP Yapılandırmasına Test Edin](./media/howto-mfaserver-adfs-2/ldap2.png)

6. LDAP bağlantı testi başarılı olduysa **Tamam**’a tıklayın.

### <a name="configure-company-settings"></a>Şirket ayarlarını yapılandırma

1. Ardından, **Şirket Ayarları** simgesini tıklatın ve Kullanıcı Adı **Çözüm sekmesini** seçin.
2. Kullanıcı adları radyo düğmesini **eşleştirmek için LDAP benzersiz tanımlayıcı özniteliğini kullanın.**
3. Kullanıcılar, kullanıcı adlarını “etki alanı\kullanıcı adı” biçiminde girerse Sunucunun, LDAP sorgusunu oluşturduğunda etki alanını kullanıcı adından çıkarabilmesi gerekir. Bu, bir kayıt defteri ayarı aracılığıyla yapılabilir.
4. Kayıt defteri düzenleyicisini açın ve 64 bit sunucuda HKEY_LOCAL_MACHINE/SOFTWARE/Wow6432Node/Positive Networks/PhoneFactor öğesine gidin. 32 bit sunucuda, “Wow6432Node” öğesini yoldan çıkarın. “UsernameCxz_stripPrefixDomain” adlı bir DWORD kayıt defteri anahtarı oluşturun ve değeri 1 olarak ayarlayın. Azure Multi-Factor Authentication artık AD FS ara sunucusunu güvenli hale getirir.

Kullanıcıların Active Directory’den Sunucuya aktarıldığından emin olun. Bu konumlardan web sitesinde oturum açken iki aşamalı doğrulama gerektirmemesi için dahili IP adreslerine izin vermek istiyorsanız [Güvenilir IP'ler bölümüne](#trusted-ips) bakın.

![Şirket ayarlarını yapılandırmak için kayıt defteri düzenleyicisi](./media/howto-mfaserver-adfs-2/reg.png)

## <a name="ad-fs-20-direct-without-a-proxy"></a>Ara sunucu olmadan AD FS 2.0 Direct

AD FS ara sunucusu kullanılmadığında AD FS’yi güvenli hale getirebilirsiniz. AD FS sunucusunda Azure Multi-Factor Authentication Sunucusu’nu yükleyin ve aşağıdaki adımları kullanarak Sunucu’yu yapılandırın:

1. Azure Multi-Factor Authentication Sunucusu’nun soldaki menüsünde **IIS Kimlik Doğrulaması** simgesine tıklayın.
2. **HTTP** sekmesine tıklayın.
3. **Ekle**’ye tıklayın.
4. Temel URL Ekle iletişim kutusuna, HTTP kimlik doğrulamasının gerçekleştirildiği AD FS web sitesinin URL'sini Temel URL alanına girin (örneğin). `https://sso.domain.com/adfs/ls/auth/integrated` Ardından bir uygulama adı (isteğe bağlı) girin. Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir.
5. İsterseniz, Boşta kalma zaman aşımı ve Maksimum oturum sürelerini ayarlayın.
6. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve iki aşamalı doğrulamaya tabi olacaksa **Azure Multi-Factor Authentication kullanıcılarının eşleşmesini gerektir** kutusunu işaretleyin. Sunucu’ya henüz aktarılmamış ve/veya iki aşamalı doğrulamadan muaf tutulacak çok sayıda kullanıcı varsa kutunun işaretini kaldırın.
7. İsterseniz tanımlama bilgisi önbellek kutusunu işaretleyin.

   ![Ara sunucu olmadan AD FS 2.0 Direct](./media/howto-mfaserver-adfs-2/noproxy.png)

8. **Tamam**'a tıklayın.
9. IIS eklentisini istediğiniz düzeyde etkinleştirmek için **Yerel Modül** sekmesine tıklayın ve sunucuyu, web sitesini (“Varsayılan Web Sitesi” gibi) veya AD FS uygulamasını (“adfs” altındaki “ls” gibi) seçin.
10. Ekranın üst kısmındaki **IIS kimlik doğrulamasını etkinleştir** kutusunu tıklatın.

Azure Multi-Factor Authentication artık AD FS’yi güvenli hale getirir.

Kullanıcıların Active Directory’den Sunucuya aktarıldığından emin olun. Bu konumlardan web sitesinde oturum açken iki aşamalı doğrulama gerektirmemesi için dahili IP adreslerine izin vermek istiyorsanız Güvenilir IP'ler bölümüne bakın.

## <a name="trusted-ips"></a>Güvenilen IP'ler

Güvenilen IP'ler, kullanıcıların belirli IP adresleri veya alt ağlardan kaynaklanan web sitesi istekleri için Azure Multi-Factor Authentication’ı atlamasına olanak tanır. Örneğin, kullanıcıları ofiste oturum açtıklarında iki aşamalı doğrulamadan muaf tutabilirsiniz. Bunun için ofis alt ağını Güvenilen IP’ler girişi olarak belirtebilirsiniz.

### <a name="to-configure-trusted-ips"></a>Güvenilen IP'leri yapılandırmak için

1. IIS Kimlik Doğrulaması bölümünde **Güvenilen IP'ler** sekmesine tıklayın.
2. **Ekle'yi tıklatın...** tıklayın.
3. Güvenilen IP Ekle iletişim kutusu göründüğünde **Tek bir IP**, **IP aralığı** ve **Alt ağ** radyo düğmelerinden birini seçin.
4. İzin verilmesi gereken IP adresini, IP adresi aralığını veya alt ağı girin. Bir alt ağ giriyorsanız uygun Ağ maskesini seçip **Tamam** düğmesine tıklayın.

![Güvenilen IP'leri MFA Server'a yapılandırma](./media/howto-mfaserver-adfs-2/trusted.png)
