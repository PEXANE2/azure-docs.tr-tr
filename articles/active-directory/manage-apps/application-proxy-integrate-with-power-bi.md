---
title: Azure AD Application Proxy ile Power BI'ye uzaktan erişimi etkinleştirme
description: Şirket içi Power BI'yi Azure AD Application Proxy ile nasıl tümleştirileştirilene ilişkin temel bilgileri kapsar.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: bc96c94152b39cc70cfc4553690faaa5b9cb8d20
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77111581"
---
# <a name="enable-remote-access-to-power-bi-mobile-with-azure-ad-application-proxy"></a>Azure AD Uygulama Ara Sunucusu ile Power BI Mobil’e uzaktan erişimi etkinleştirme

Bu makalede, Power BI mobil uygulamasının Power BI Report Server (PBIRS) ve SQL Server Reporting Services (SSRS) 2016 ve sonrası hizmetlerine bağlanmasını sağlamak için Azure AD Application Proxy'nin nasıl kullanılacağı açıklanmaktadır. Bu tümleştirme sayesinde, şirket ağından uzakta olan kullanıcılar Power BI mobil uygulamasından Power BI raporlarına erişebilir ve Azure AD kimlik doğrulaması ile korunabilir. Bu koruma, Koşullu Erişim ve çok faktörlü kimlik doğrulama gibi [güvenlik avantajlarını](application-proxy-security.md#security-benefits) içerir.  

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, Rapor Hizmetlerini zaten dağıttığınızı ve [Uygulama Proxy'yi etkinleştirdiğinizi](application-proxy-add-on-premises-application.md)varsayar.

- Uygulama Proxy'sini etkinleştirmek için bir Windows sunucusuna bağlayıcı yüklemeve bağlayıcının Azure AD hizmetleriyle iletişim kurabilmesi için [ön koşulların](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment) tamamlanması gerekiyor.  
- Power BI'yi yayınlarken, aynı dahili ve harici etki alanlarını kullanmanızı tavsiye ettik. Özel etki alanları hakkında daha fazla bilgi edinmek için Bkz. [Uygulama Proxy'sinde özel etki alanlarıyla çalışma.](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-custom-domain)
- Bu entegrasyon Power **BI Mobile iOS ve Android** uygulaması için kullanılabilir.

## <a name="step-1-configure-kerberos-constrained-delegation-kcd"></a>Adım 1: Kerberos Kısıtlı Delegasyonu (KCD) Yapılandırın

Windows kimlik doğrulaması kullanan şirket içi uygulamalarda, Kerberos kimlik doğrulama protokolü ve Kerberos kısıtlı delegasyonu (KCD) adı verilen bir özellik ile tek oturum açma (SSO) elde edebilirsiniz. KCD, yapılandırıldığınızda, kullanıcı doğrudan Windows'da oturum açmış olsa bile, Uygulama Proxy bağlayıcısının kullanıcı için bir Windows belirteci almasına olanak tanır. KCD hakkında daha fazla bilgi edinmek için Uygulama Proxy ile uygulamalarınızda tek oturum açma için [Kerberos Kısıtlı Delegasyon Genel Bakışı](https://technet.microsoft.com/library/jj553400.aspx) ve [Kerberos Kısıtlı Delegasyonu'na](application-proxy-configure-single-sign-on-with-kcd.md)bakın.

Reporting Services tarafında yapılandırılacak çok fazla ayar yoktur. Uygun Kerberos kimlik doğrulamasını etkinleştirmek için geçerli bir Hizmet Asıl Adı (SPN) olduğundan emin olun. Ayrıca, Raporlama Hizmetleri sunucusunun Kimlik Belirleme kimlik belirlemesi için etkinleştirildiğinden emin olun.

Raporlama hizmetleri için KCD'yi ayarlamak için aşağıdaki adımları ile devam edin.

### <a name="configure-the-service-principal-name-spn"></a>Hizmet Ana Adı (SPN) yapılandırma

SPN, Kerberos kimlik doğrulaması kullanan bir hizmet için benzersiz bir tanımlayıcıdır. Rapor sunucunuz için uygun bir HTTP SPN'niz olduğundan emin olmanız gerekir. Rapor sunucunuz için doğru Hizmet Asıl Adı'nı (SPN) yapılandırma hakkında bilgi almak isterseniz bkz. [Register a Service Principal Name (SPN) for a Report Server (Rapor Sunucusu için bir Hizmet Asıl Adı (SPN) kaydı yapma)](https://msdn.microsoft.com/library/cc281382.aspx).
-L seçeneği ile Setspn komutunu çalıştırarak SPN eklenmiştir doğrulayabilirsiniz. Bu komut hakkında daha fazla bilgi edinmek için [Setspn'e](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spn-setspn-syntax.aspx)bakın.

### <a name="enable-negotiate-authentication"></a>Kimlik Doğrulamayı Etkinleştir

Bir rapor sunucusunun Kerberos kimlik doğrulamasını kullanmasını etkinleştirmek için, rapor sunucusunun Kimlik Doğrulama Türünü RSWindowsNegotiate olarak yapılandırın. Rsreportserver.config dosyasını kullanarak bu ayarı yapılandırın.

```xml
<AuthenticationTypes>
    <RSWindowsNegotiate />
    <RSWindowsKerberos />
    <RSWindowsNTLM />
</AuthenticationTypes>
```

Daha fazla bilgi için bkz. [Modify a Reporting Services Configuration File (Bir Reporting Services Yapılandırma Dosyasını değiştirme)](https://msdn.microsoft.com/library/bb630448.aspx) ve [Configure Windows Authentication on a Report Server (Bir Rapor Sunucusunda Windows Kimlik Doğrulamasını yapılandırma)](https://msdn.microsoft.com/library/cc281253.aspx).

### <a name="ensure-the-connector-is-trusted-for-delegation-to-the-spn-added-to-the-reporting-services-application-pool-account"></a>Raporlama Hizmetleri uygulama havuzu hesabına eklenen SPN'ye temsilcilik için Bağlayıcının güvenilir olduğundan emin olun
Azure AD Application Proxy hizmetinin kullanıcı kimliklerini Raporlama Hizmetleri uygulama havuzu hesabına devredilebilmeleri için KCD'yi yapılandırın. Azure AD'de kimlik doğrulaması yapılan kullanıcılarınız için Kerberos biletlerini almak için Uygulama Proxy bağlayıcısının etkinleştirerek KCD'yi yapılandırın. Daha sonra bu sunucu bağlamı hedef uygulamaya veya bu durumda Raporlama Hizmetleri'ne geçirir.

KCD'yi yapılandırmak için her konektör makinesi için aşağıdaki adımları yineleyin:

1. Etki alanı yöneticisi olarak etki alanı denetleyicisinde oturum açın ve ardından **Etkin Dizin Kullanıcıları ve Bilgisayarlarını**açın.
2. Konektörün çalıştırdığı bilgisayarı bulun.  
3. Bilgisayarı çift tıklatın ve ardından **Temsilci** sekmesini seçin.
4. Delegasyon ayarlarını **yalnızca belirtilen hizmetlere delegasyon için bu bilgisayarı Güven'e**ayarlayın. Ardından, **kimlik doğrulama iletişim kuralını kullan'ı**seçin.
5. **Ekle'yi**seçin ve ardından **Kullanıcılar veya Bilgisayarlar'ı**seçin.
6. Raporlama Hizmetleri için kullandığınız hizmet hesabını girin. Bu hesap, Reporting Services yapılandırmasında SPN'yi eklediğiniz hesaptır.
7. **Tamam**'a tıklayın. Değişiklikleri kaydetmek için Yeniden **Tamam'ı** tıklatın.

Daha fazla bilgi için Uygulama Proxy ile uygulamalarınızda tek oturum açma için [Kerberos Kısıtlı Temsilciliği'ne](application-proxy-configure-single-sign-on-with-kcd.md)bakın.

## <a name="step-2-publish-report-services-through-azure-ad-application-proxy"></a>Adım 2: Azure AD Uygulama Proxy'si aracılığıyla Rapor Hizmetlerini Yayımlama

Artık Azure AD Uygulama Proxy'sini yapılandırmaya hazırsınız.

1. Rapor Hizmetlerini Aşağıdaki ayarları içeren Uygulama Proxy'si aracılığıyla yayımlayın. Uygulama Proxy aracılığıyla uygulamanın nasıl yayımlanacağına ilişkin adım adım talimatlar için Azure [AD Application Proxy kullanarak uygulamaları yayımlama'ya](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad)bakın.
   - **Dahili URL**: Bağlayıcının şirket ağında ulaşabileceği Rapor Sunucusu'na URL'yi girin. Bu URL'ye bağlayıcının yüklü olduğu sunucudan erişilebilen olduğundan emin olun. En iyi yöntem, Application Proxy aracılığıyla `https://servername/` yayınlanan alt yollarla ilgili sorunları önlemek gibi üst düzey bir etki alanı kullanmaktır. Örneğin, kullanın `https://servername/` ve `https://servername/reports/` `https://servername/reportserver/`değil ya .
     > [!NOTE]
     > Report Server için güvenli bir HTTPS bağlantısı kullanmanızı öneririz. Bkz. [SSL bağlantılarını yerel mod rapor sunucusunda yapılandırın](https://docs.microsoft.com/sql/reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server?view=sql-server-2017) ve nasıl bilgi verilebilirsiniz.
   - **Harici URL**: Power BI mobil uygulamasının bağlanacak genel URL'ye girin. Örneğin, özel bir `https://reports.contoso.com` etki alanı kullanılıyorsa gibi görünebilir. Özel bir etki alanı kullanmak için etki alanı için bir sertifika yükleyin ve bir DNS kaydını uygulamanız için varsayılan msappproxy.net etki alanına işaret edin. Ayrıntılı adımlar için Azure [AD Application Proxy'deki özel etki alanlarıyla çalışma'ya](application-proxy-configure-custom-domain.md)bakın.

   - **Ön kimlik doğrulama yöntemi**: Azure Active Directory

2. Uygulamanız yayınlandıktan sonra, tek oturum açma ayarlarını aşağıdaki adımlarla yapılandırın:

   a. Portaldaki uygulama sayfasında Tek **oturum açma'yı**seçin.

   b. **Tek Oturum Açma Modu** **için, Tümleşik Windows Kimlik Doğrulaması'nı**seçin.

   c. **İç Uygulama SPN'sini** daha önce ayarladığınız değere ayarlayın.  

   d. Bağlayıcının kullanıcılarınız adına kullanması için **Devredilen Giriş Kimliğini** seçin. Daha fazla bilgi için bkz: [Farklı şirket içi ve bulut kimlikleri ile çalışma.](application-proxy-configure-single-sign-on-with-kcd.md#working-with-different-on-premises-and-cloud-identities)

   e. Değişikliklerinizi kaydetmek için **Kaydet'i** tıklatın.

Uygulamanızı ayarlamayı bitirmek için **Kullanıcılar ve gruplar bölümüne** gidin ve kullanıcıları bu uygulamaya erişmeleri için atayın.

## <a name="step-3-modify-the-reply-uris-for-the-application"></a>Adım 3: Uygulama için Yanıt URI's değiştirin

Power BI mobil uygulamasının Rapor Hizmetlerine bağlanıp erişebilmeleri için, 2. 

1. Azure Etkin Dizin **Genel Bakış** sayfasında Uygulama **kayıtlarını**seçin.
2. Adım 2'de oluşturduğunuz uygulama için **Tüm uygulamalar** sekmesinde arama.
3. Uygulamayı seçin, ardından **Kimlik Doğrulama'yı**seçin.
4. Kullandığınız platforma bağlı olarak aşağıdaki Yönlendirme URL'lerini ekleyin.

   Power BI Mobile **iOS**için uygulamayı yapılandırırken, Kamu İstemci (Mobile & Desktop) türündeki Yönlendirme URL'lerini ekleyin:
   - `msauth://code/mspbi-adal%3a%2f%2fcom.microsoft.powerbimobile`
   - `msauth://code/mspbi-adalms%3a%2f%2fcom.microsoft.powerbimobilems`
   - `mspbi-adal://com.microsoft.powerbimobile`
   - `mspbi-adalms://com.microsoft.powerbimobilems`
   
   Power BI Mobile **Android**için uygulamayı yapılandırırken, Aşağıdaki Yönlendirme URI'lerini ekleyin: Public Client (Mobile & Desktop):
   - `urn:ietf:wg:oauth:2.0:oob`
   - `mspbi-adal://com.microsoft.powerbimobile`
   - `msauth://com.microsoft.powerbim/g79ekQEgXBL5foHfTlO2TPawrbI%3D` 
   - `msauth://com.microsoft.powerbim/izba1HXNWrSmQ7ZvMXgqeZPtNEU%3D`

   > [!IMPORTANT]
   > Uygulamanın doğru çalışması için Yönlendirme ÜR'leri eklenmelidir. Uygulamayı hem Power BI Mobile iOS hem de Android için yapılandırıyorsanız, iOS için yapılandırılan Yönlendirme URI'leri listesine aşağıdaki `urn:ietf:wg:oauth:2.0:oob`Tür Kamu İstemcisi (Mobil & Masaüstü) URI'yi yeniden yönlendirmeyi ekleyin: .

## <a name="step-4-connect-from-the-power-bi-mobile-app"></a>Adım 4: Power BI Mobil Uygulamasından bağlanın

1. Power BI mobil uygulamasında, Raporlama Hizmetleri örneğinize bağlanın. Bunu yapmak için, Application Proxy aracılığıyla yayınladığınız uygulamaiçin **Dış URL'yi** girin.

   ![Harici URL ile Power BI mobil uygulaması](media/application-proxy-integrate-with-power-bi/app-proxy-power-bi-mobile-app.png)

2. **Bağlan**’ı seçin. Azure Etkin Dizin oturum açımına yönlendirilirsiniz.

3. Kullanıcınız için geçerli kimlik bilgilerini girin ve **Oturum Aç'ı**seçin. Raporlama Hizmetleri sunucunuzdaki öğeleri görürsünüz.

## <a name="step-5-configure-intune-policy-for-managed-devices-optional"></a>Adım 5: Yönetilen aygıtlar için Intune ilkesini yapılandırın (isteğe bağlı)

Şirketinizin iş gücünün kullandığı istemci uygulamalarını yönetmek için Microsoft Intune'u kullanabilirsiniz. Intune, veri şifreleme ve ek erişim gereksinimleri gibi özellikleri kullanmanıza olanak tanır. Intune aracılığıyla uygulama yönetimi hakkında daha fazla bilgi edinmek için Bkz. Intune App Management. Power BI mobil uygulamasının Intune ilkesiyle çalışmasını sağlamak için aşağıdaki adımları kullanın.

1. Azure **Etkin Dizini'ne** gidin ve ardından **Uygulama Kayıtları'** na gidin.
2. Yerel istemci uygulamanızı kaydederken Adım 3'te yapılandırılan uygulamayı seçin.
3. Uygulamanın sayfasında **API İzinleri'ni**seçin.
4. **İzin Ekle'yi**tıklatın. 
5. **Kuruluşum kullandığı API'ler**altında "Microsoft Mobile Application Management" araması yapın ve seçin.
6. **DeviceManagementManagedApps.ReadWrite** iznini uygulamaya ekleyin
7. Uygulamaya izin izni vermek için **Yönetici Onayı'nı** tıklatın.
8. [Uygulama koruma ilkelerini nasıl oluşturup atayabileceğinize](https://docs.microsoft.com/intune/app-protection-policies)atıfta bulunarak istediğiniz Intune ilkesini yapılandırın.

## <a name="troubleshooting"></a>Sorun giderme

Uygulama, bir raporu birkaç dakikadan fazla yüklemeye çalıştıktan sonra bir hata sayfası döndürürse, zaman ayarı değiştirmeniz gerekebilir. Varsayılan olarak, Uygulama Proxy'si bir isteğe yanıt vermesi 85 saniye kadar sürecek uygulamaları destekler. Bu ayarı 180 saniyeye uzatmak için uygulamanın Uygulama Proxy ayarları sayfasında **Uzun'a** arka uç zaman ekini seçin. Hızlı ve güvenilir raporların nasıl oluşturulacazı ile ilgili ipuçları için [Power BI Reports Best Practices'e](https://docs.microsoft.com/power-bi/power-bi-reports-performance)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Yerel istemci uygulamalarının proxy uygulamalarıyla etkileşime girmesini etkinleştirme](application-proxy-configure-native-client-application.md)
- [Power BI mobil uygulamalarında şirket içi rapor sunucusu raporlarını ve KPI'lerini görüntüleme](https://docs.microsoft.com/power-bi/consumer/mobile/mobile-app-ssrs-kpis-mobile-on-premises-reports)
