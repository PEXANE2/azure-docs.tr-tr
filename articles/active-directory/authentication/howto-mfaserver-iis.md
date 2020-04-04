---
title: IIS Kimlik Doğrulama ve Azure MFA Server - Azure Active Directory
description: IIS Kimlik Doğrulaması ve Azure Multi-Factor Authentication Sunucusu dağıtılıyor.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 07/11/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: f6189e2bc6c3c8f28b767902b525b03cb72968bc
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80652907"
---
# <a name="configure-azure-multi-factor-authentication-server-for-iis-web-apps"></a>IIS web uygulamaları için Azure Multi-Factor Authentication Sunucusu

Microsoft IIS web uygulamaları ile IIS kimlik doğrulamasını etkinleştirmek ve yapılandırmak için Azure Multi-Factor Authentication (MFA) Sunucusu’nun IIS Kimlik Doğrulaması bölümünü kullanın. Azure MFA Sunucusu, Azure Multi-Factor Authentication eklemek üzere IIS web sunucusuna yapılan istekleri filtreleyebilen bir eklenti yükler. IIS eklentisi Form Tabanlı Kimlik Doğrulaması ve Tümleşik Windows HTTP Kimlik Doğrulaması için destek sağlar. Güvenilen IP’ler iç IP adreslerini iki öğeli kimlik doğrulamasından muaf tutmak için de kullanılabilir.

> [!IMPORTANT]
> 1 Temmuz 2019 itibariyle Microsoft, yeni dağıtımlar için Artık MFA Server sunmayacak. Kullanıcılarından çok faktörlü kimlik doğrulaması isteyen yeni müşteriler bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmalıdır. 1 Temmuz'dan önce MFA Server'ı etkinleştirmiş olan mevcut müşteriler en son sürümü, gelecekteki güncelleştirmeleri karşıdan yükleyebilecek ve her zamanki gibi etkinleştirme kimlik bilgilerini oluşturabilecek.

![MFA Server'da IIS Kimlik Doğrulaması](./media/howto-mfaserver-iis/iis.png)

## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu ile Form Tabanlı IIS Kimlik Doğrulaması kullanma

Form tabanlı kimlik doğrulaması kullanan bir IIS web uygulamasını güvenli hale getirmek için, IIS web sunucusuna Azure Multi-Factor Authentication Sunucusu’nu yükleyin ve Sunucu’yu aşağıdaki yordama göre yapılandırın:

1. Azure Multi-Factor Authentication Sunucusu’nun soldaki menüsünde IIS Kimlik Doğrulaması simgesine tıklayın.
2. **Form Tabanlı** sekmesine tıklayın.
3. **Ekle**’ye tıklayın.
4. Kullanıcı adı, parola ve etki alanı değişkenlerini otomatik olarak `https://localhost/contoso/auth/login.aspx`algılamak için, Otomatik Yapılandırma Formu Tabanlı Web Sitesi iletişim kutusuna Giriş URL'sini (beğen) girin ve **Tamam'ı**tıklatın.
5. Tüm kullanıcılar Sunucu'ya aktarıldıysa veya alınıp alınmayacaksa ve çok faktörlü kimlik doğrulamasına tabi yse, **Çok Faktörlü Kimlik Doğrulamayı İste** kullanıcı eşlem kutusunu denetleyin. Çok sayıda kullanıcı Sunucu’ya henüz aktarılmadı ve/veya multi-factor authentication’da muaf tutulacaksa, kutunun işaretini kaldırın.
6. Sayfa değişkenleri otomatik olarak algılanamıyorsa Otomatik Yapılandırma Form Tabanlı Web Sitesi iletişim kutusunda **Elle Belirt**’e tıklayın.
7. Otomatik Yapılandırma Form Tabanlı Web Sitesi iletişim kutusunda, URL Gönder alanına oturum açma sayfası URL’sini girin ve Uygulama adı girin (isteğe bağlı). Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir.
8. Doğru İstek biçimini seçin. Bu post **veya çoğu** web uygulamaları için GET ayarlanır.
9. Kullanıcı adı değişkenini, Parola değişkenini ve Etki alanı değişkenini (oturum açma sayfasında görünüyorsa) girin. Girdi kutularının adlarını bulmak için bir web tarayıcısında oturum açma sayfasına gidin, sayfaya sağ tıklayın ve **Kaynağı Görüntüle**’yi seçin.
10. Tüm kullanıcılar Sunucu'ya aktarıldıysa veya alınıp alınmayacaksa ve çok faktörlü kimlik doğrulamasına tabi yse Azure **Çok Faktörlü Kimlik Doğrulama kullanıcı eşlem** kutusunu denetleyin. Çok sayıda kullanıcı Sunucu’ya henüz aktarılmadı ve/veya multi-factor authentication’da muaf tutulacaksa, kutunun işaretini kaldırın.
11. Aşağıdaki gibi gelişmiş ayarları gözden geçirmek için **Gelişmiş**’e tıklayın:

    - Özel bir reddetme sayfa dosyası seçme
    - Tanımlama bilgilerini kullanarak web sitesinde başarılı kimlik doğrulamalarını belirli bir dönem boyunca önbelleğe alma
    - Birincil kimlik bilgilerinin bir Windows Etki Alanı, LDAP dizinine göre mi yoksa RADIUS sunucusuna göre mi doğrulanacağını belirtin.

12. Form Tabanlı Web Sitesi Ekle iletişim kutusuna dönmek için **Tamam**’a tıklayın.
13. **Tamam**'a tıklayın.
14. URL ve sayfa değişkenleri algılandığında veya girildiğinde, web sitesi verileri Form Tabanlı panelde görüntülenir.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu ile Tümleşik Windows Kimlik Doğrulaması kullanma

Tümleşik Windows HTTP kimlik doğrulaması kullanan bir IIS web uygulamasını güvenli hale getirmek için, IIS web sunucusuna Azure MFA Sunucusu’nu yükleyin ve aşağıdaki adımları izleyerek Sunucu’yu yapılandırın:

1. Azure Multi-Factor Authentication Sunucusu’nun soldaki menüsünde IIS Kimlik Doğrulaması simgesine tıklayın.
2. **HTTP** sekmesine tıklayın.
3. **Ekle**’ye tıklayın.
4. Temel URL ekle iletişim kutusuna, HTTP kimlik doğrulamasının gerçekleştirildiği <http://localhost/owa>web sitesinin URL'sini girin (beğen) ve bir Uygulama adı (isteğe bağlı) girin. Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir.
5. Varsayılan yeterli değilse, Boşta kalma zaman aşımı ve Maksimum oturum sürelerini ayarlayın.
6. Tüm kullanıcılar Sunucu'ya aktarıldıysa veya alınıp alınmayacaksa ve çok faktörlü kimlik doğrulamasına tabi yse, **Çok Faktörlü Kimlik Doğrulamayı İste** kullanıcı eşlem kutusunu denetleyin. Çok sayıda kullanıcı Sunucu’ya henüz aktarılmadı ve/veya multi-factor authentication’da muaf tutulacaksa, kutunun işaretini kaldırın.
7. İstenirseniz **Çerez önbellek** kutusunu işaretleyin.
8. **Tamam**'a tıklayın.

## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication Sunucusu için IIS Eklentilerini Etkinleştirme

Form Tabanlı ya da HTTP kimlik doğrulaması URL’lerini ve ayarlarını yapılandırdıktan sonra, Azure Multi-Factor Authentication IIS eklentilerinin IIS’de yüklenmesi ve etkinleştirilmesi gereken konumları seçin. Aşağıdaki yordamı kullanın:

1. IIS 6'da çalışıyorsanız, **ISAPI** sekmesini tıklatın. Web uygulamasının altında çalıştırmakta olduğu web sitesini seçin (örneğin Varsayılan Web Sitesi) bu site için Azure Çok Faktörlü Kimlik Doğrulama ISAPI filtresi eklentisini etkinleştirin.
2. IIS 7 veya daha yüksek te çalışıyorsanız, **Yerel Modül** sekmesini tıklatın. IIS eklentisini istenilen düzeyde etkinleştirmek için sunucuyu, web sitelerini veya uygulamaları seçin.
3. Ekranın üst kısmındaki **IIS kimlik doğrulamasını etkinleştir** kutusunu tıklatın. Azure Multi-Factor Authentication artık seçilen IIS uygulamasını güvenli hale getirir. Kullanıcıların sunucuya aktarıldığından emin olun.

## <a name="trusted-ips"></a>Güvenilen IP'ler

Güvenilen IP'ler kullanıcıların belirli IP adresleri veya alt ağlardan kaynaklanan web sitesi istekleri için Azure Multi-Factor Authentication’ı atlamasına olanak tanır. Örneğin, ofisten oturum açarken kullanıcıların Azure Multi-Factor Authentication’dan muaf tutulmasını isteyebilirsiniz. Bunun için ofis alt ağını Güvenilen IP’ler girişi olarak belirtebilirsiniz. Güvenilen IP’leri yapılandırmak için aşağıdaki yordamı kullanın:

1. IIS Kimlik Doğrulaması bölümünde **Güvenilen IP'ler** sekmesine tıklayın.
2. **Ekle**’ye tıklayın.
3. Güvenilir IP Ekle iletişim kutusu göründüğünde, Tek **IP,** **IP aralığı**veya **Subnet** radyo düğmesini seçin.
4. İzin verilmesi gereken IP adresini, IP adresi aralığını veya alt ağı girin. Bir alt ağ giriyorsanız uygun Ağ maskesini seçip **Tamam**’a tıklayın.
