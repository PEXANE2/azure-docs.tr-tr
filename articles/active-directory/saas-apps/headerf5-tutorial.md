---
title: 'Öğretici: F5 ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve F5 arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 59a87abb-1ec1-4438-be07-5b115676115f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/19/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 352f52a2a6d84d352bb46e09f104efde303307f5
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478056"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-f5"></a>Öğretici: F5 ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, F5'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. F5'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* F5 erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla F5'te otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD'de tek oturum açma yla SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek [için Azure Active Directory'deki uygulamalarda tek oturum açma'ya](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.

* F5 tek oturum açma (SSO) özellikli abonelik.

* Ortak çözümün dağıtılması için aşağıdaki lisans lar yer almak için aşağıdaki lisanslar yer almak ta bir

    * F5 BIG-IP® En iyi paket (veya) 

    * F5 BIG-IP Erişim Politikası Yöneticisi™ (APM) bağımsız lisans 

    * F5 BIG-IP Erişim Politikası Yöneticisi™ (APM) mevcut bir BIG-IP F5 BIG-IP® Yerel Trafik Yöneticisi™ (LTM) üzerinde eklenti lisans.

    * Yukarıdaki lisansa ek olarak, F5 sistemi de lisanslı olabilir: 

        * URL kategorisi veritabanını kullanmak için bir URL Filtreleme aboneliği

        * Bilinen saldırganları ve kötü amaçlı trafiği algılamak ve engellemek için bir F5 IP Intelligence aboneliği
     
        * Güçlü kimlik doğrulaması için dijital anahtarları korumak ve yönetmek için bir ağ donanım güvenlik modülü (HSM)

* F5 BIG-IP sistemi APM modülleri ile sağlanır (LTM isteğe bağlıdır)

* İsteğe bağlı olsa da, F5 sistemlerinin etkin bekleme çiftini içeren bir [eşitleme/başarısız cihaz grubunda](https://techdocs.f5.com/content/techdocs/en-us/bigip-14-1-0/big-ip-device-service-clustering-administration-14-1-0.html) (S/F DG) yüksek kullanılabilirlik için kayan bir IP adresiyle (HA) dağıtılması önerilir. Bağlantı Toplama Denetim Protokolü (LACP) kullanılarak daha fazla arabirim artıklığı elde edilebilir. LACP bağlı fiziksel arabirimleri tek bir sanal arabirim (toplam grup) olarak yönetir ve grup içindeki tüm arabirim hatalarını algılar.

* Kerberos uygulamaları için, kısıtlı delegasyon için bir şirket içi AD hizmet hesabı.  BIR AD delegasyonu hesabı oluşturmak için [F5 Belgeleri'ne](https://support.f5.com/csp/article/K43063049) bakın.

## <a name="access-guided-configuration"></a>Access güdümlü yapılandırma

* Access guided configuration' F5 TMOS sürüm 13.1.0.8 ve üzeri sürümde desteklenir. BIG-IP sisteminiz 13.1.0.8'in altındaki bir sürümü çalıştırıyorsa, lütfen **Gelişmiş yapılandırma** bölümüne bakın.

* Access güdümlü yapılandırma tamamen yeni ve kolaylaştırılmış bir kullanıcı deneyimi sunar. Bu iş akışı tabanlı mimari, seçili topolojiye uygun sezgisel, yeniden giren yapılandırma adımları sağlar.

* Yapılandırmaya geçmeden önce, [downloads.f5.com](https://login.f5.com/resource/login.jsp?ctx=719748)en son kullanım örneği paketini indirerek kılavuzlu yapılandırmayı yükseltin. Yükseltmek için aşağıdaki yordamı izleyin.

    >[!NOTE]
    >Aşağıdaki ekran görüntüleri en son yayımlanan sürümü (BIG-IP 15.0 AGC sürümü 5.0 ile) içindir. Aşağıdaki yapılandırma adımları, 13.1.0.8 ile en son BIG-IP sürümü arasında bu kullanım örneği için geçerlidir.

1. F5 BIG-IP Web UI'de **Access >> Guide Configuration'a**tıklayın.

1. **Kılavuzlu Yapılandırma** sayfasında, sol üst köşedeki **Yükseltme Kılavuzlu Yapılandırma'ya** tıklayın.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure14.png) 

1. Yükseltme Kılavuzu Yapılandırma pop ekranında, indirilen kullanım örneği paketini yüklemek için **Dosyayı Seç'i** seçin ve **Yükle ve Yükle** düğmesine tıklayın.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure15.png) 

1. Yükseltme tamamlandığında, **Devam et** düğmesini tıklatın.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure16.png)

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* F5 SSO üç farklı şekilde yapılandırılabilir.

- [Üstbilgi Tabanlı uygulama için F5 tek oturum açma yı yapılandırma](#configure-f5-single-sign-on-for-header-based-application)

- [Kerberos uygulaması için F5 tek oturum açma yı yapılandırma](kerbf5-tutorial.md)

- [Gelişmiş Kerberos uygulaması için F5 tek oturum açma yı yapılandırın](advance-kerbf5-tutorial.md)

### <a name="key-authentication-scenarios"></a>Anahtar Kimlik Doğrulama Senaryoları

* Open ID Connect, SAML ve WS-Fed gibi modern kimlik doğrulama protokolleri için Azure Active Directory yerel tümleştirme desteğinin yanı sıra F5, Azure AD ile hem dahili hem de harici erişim için eski tabanlı kimlik doğrulama uygulamaları için güvenli erişimi genişleterek bu uygulamalara modern senaryolar (örneğin parolasız erişim) sağlar. Buna şunlar dahildir:

* Üstbilgi tabanlı kimlik doğrulama uygulamaları

* Kerberos kimlik doğrulama uygulamaları

* Anonim kimlik doğrulama veya dahili kimlik doğrulama uygulamaları

* NTLM kimlik doğrulama uygulamaları (kullanıcı için çift istemli koruma)

* Form tabanlı Uygulama (kullanıcı için çift istemli koruma)

## <a name="adding-f5-from-the-gallery"></a>Galeriden F5 ekleme

F5'in Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize F5 eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **F5** yazın.
1. Sonuç panelinden **F5'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-f5"></a>F5 için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak F5 ile yapılandırma ve test edin. SSO'nun çalışması için, bir Azure AD kullanıcısı ile F5'teki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu F5 ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[F5 SSO'yu yapılandırın](#configure-f5-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[F5 test kullanıcısını oluşturun](#create-f5-test-user)** - F5'teki B.Simon'ın, kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **F5** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<YourCustomFQDN>.f5.com/`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<YourCustomFQDN>.f5.com/`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<YourCustomFQDN>.f5.com/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [F5 İstemci destek ekibine](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML** ve **Certificate'ı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **F5'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın F5'e erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **F5'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.
1. **Koşullu Erişim'e** tıklayın.
1. Yeni **İlke'ye**tıklayın.
1. Artık F5 Uygulamanızı CA Politikası için bir kaynak olarak görebilir ve Multifactor Auth, Device tabanlı erişim denetimi veya Kimlik Koruma Politikası dahil olmak üzere koşullu erişimi uygulayabilirsiniz.

## <a name="configure-f5-sso"></a>F5 SSO'nun yapılandırılsın

- [Kerberos uygulaması için F5 tek oturum açma yı yapılandırma](kerbf5-tutorial.md)

- [Gelişmiş Kerberos uygulaması için F5 tek oturum açma yı yapılandırın](advance-kerbf5-tutorial.md)

### <a name="configure-f5-single-sign-on-for-header-based-application"></a>Üstbilgi Tabanlı uygulama için F5 tek oturum açma yı yapılandırma

### <a name="guided-configuration"></a>Güdümlü Yapılandırma

1. Yeni bir web tarayıcısı penceresi açın ve F5 (Üstbilgi Tabanlı) şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Sistem **> Sertifika Yönetimi > Trafik Sertifika Yönetimi > SSL Sertifika Listesi'ne**gidin. Sağ köşeden **Içeri Aktar'ı** seçin. Bir **Sertifika Adı** belirtin (daha sonra config'e başvurulacaktır). Sertifika **Kaynağı'nda,** Yükleme Dosyası'nı seçin, SAML Tek İşaretini yapılandırırken Azure'dan indirilen sertifikayı belirtin. **İçeri Aktar**’a tıklayın.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure12.png)
 
1. Ayrıca, **Uygulama Hostname için SSL Sertifikası gerekir. Sistem > Sertifika Yönetimi > Trafik Sertifika Yönetimi > SSL Sertifika Listesi'ne gidin.** Sağ köşeden **Içeri Aktar'ı** seçin. **İthalat Türü** **PKCS 12(IIS)** olacaktır. Bir **Anahtar Adı** belirtin (config'de daha sonra başvurulacaktır) ve PFX dosyasını belirtin. PFX için **Parola'yı** belirtin. **İçeri Aktar**’a tıklayın.

    >[!NOTE]
    >Örnekte uygulama adımız `Headerapp.superdemo.live`, Bir Joker Sertifikası kullanıyoruz `WildCard-SuperDemo.live`anahtar adımız .

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure13.png)

1. Azure AD Federasyonu ve Uygulama Erişimi'ni kurmak için Rehberli Deneyimi kullanacağız. Git - F5 BIG-IP **Main** ve **Erişim > Güdümlü Yapılandırma > Federasyonu > SAML Servis Sağlayıcısı**seçin. **Yapılandırmayı** başlatmak için **İleri'yi** tıklatın.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure01.png)

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure02.png)
 
1. Yapılandırma **Adı**sağlayın. Varlık **Kimliğini** (Azure AD Uygulaması Yapılandırması'nda yapılandırdığınız la aynı) belirtin. Ana **Bilgisayar adını**belirtin. Başvuru için **açıklama** ekleyin. Kalan varsayılan girişleri kabul edin ve seçin ve sonra **Kaydet & Sonraki'ni**tıklatın.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure03.png) 

1. Bu örnekte port 443 ile 192.168.30.20 olarak yeni bir Sanal Sunucu oluşturuyoruz. **Hedef Adreste**Sanal Sunucu IP adresini belirtin. İstemci **SSL Profilini**seçin, yeni oluştur'u seçin. Daha önce yüklenmiş uygulama sertifikasını( bu örnekteki joker kart sertifikasını) ve ilişkili anahtarı belirtin ve ardından **Sonraki& kaydet'i**tıklatın.

    >[!NOTE]
    >Bu örnekte Dahili websunucumuz 888 bağlantı noktasında çalışıyor ve 443 ile yayınlamak istiyoruz.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure04.png) 

1. **IdP bağlayıcınızı yapılandırmak,** Meta verilerinizi belirtmek, Dosya Yı Seç'i tıklatın ve Azure AD'den daha önce indirilen Metadata XML dosyasını yüklemek için Select yöntemi altında. SAML IDP bağlayıcısı için benzersiz bir **Ad** belirtin. Daha önce yüklenen **Meta veri imzalama sertifikasını** seçin. **Sonraki & Kaydet'i**tıklatın.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure05.png)
 
1. **Bir Havuz Seç'in**altında, **Yeni Oluştur'u** belirtin (alternatif olarak zaten var olan bir havuz seçin). Diğer değer varsayılan olsun. Havuz Sunucuları altında IP **Adresi/Düğüm Adı**altında IP Adresi yazın. Bağlantı **Noktasını**belirtin. **Sonraki & Kaydet'i**tıklatın.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure06.png)

1. Tek Oturum Açma Ayarları **ekranında, Tek Oturum Açma'yı etkinleştir'i**seçin. Seçili Tek Oturum Açma Türü altında **HTTP üstbilgi tabanlı**seçin. **session.saml.last.Identity** ile **session.saml.last.attr.name.Identity** under Username Source (Azure AD'deki talep eşlemesini kullanarak ayarlanan bu değişken) değiştirin. SSO Başlıkları altında.

    * **HeaderName : MyAuthorization**

    * **Üstbilgi Değeri : %{session.saml.last.attr.name.Identity}**

    * **Sonraki & Kaydet'i** tıklatın

    Değişkenlerin ve değerlerin tam listesi için Ek'e başvurun. Gerektiğinde daha fazla üstbilgi ekleyebilirsiniz.

    >[!NOTE]
    >Hesap Adı Oluşturulan F5 Temsilcilik Hesabıdır (F5 Belgelerini Kontrol Edin).

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure07.png) 

1. Bu kılavuz amacıyla, uç nokta denetimlerini atlarız.  Ayrıntılar için F5 belgelerine bakın. **Sonraki & Kaydet'i**seçin.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure08.png)

1. Varsayılanları kabul edin ve **Sonraki& kaydet'i**tıklatın. SAML oturum yönetimi ayarlarıyla ilgili ayrıntılar için F5 belgelerine bakın.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure09.png)

1. Özet ekranını gözden geçirin ve BIG-IP'yi yapılandırmak için **Dağıt'ı** seçin. **Finish'e**tıklayın.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure10.png)

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure11.png)

## <a name="advanced-configuration"></a>Gelişmiş Yapılandırma

Bu bölüm, Kılavuzlu yapılandırmayı kullanamıyorsanız veya ek Parametreler eklemek/değiştirmek istiyorsanız kullanılmak üzere tasarlanmıştır. Uygulama Hostname için bir TLS/SSL sertifikasına ihtiyacınız olacaktır.

1. Sistem **> Sertifika Yönetimi > Trafik Sertifika Yönetimi > SSL Sertifika Listesi'ne**gidin. Sağ köşeden **Içeri Aktar'ı** seçin. **İthalat Türü** **PKCS 12(IIS)** olacaktır. Bir **Anahtar Adı** belirtin (config'de daha sonra başvurulacaktır) ve PFX dosyasını belirtin. PFX için **Parola'yı** belirtin. **İçeri Aktar**’a tıklayın.

    >[!NOTE]
    >Örnekte uygulama adımız `Headerapp.superdemo.live`, Bir Joker Sertifikası kullanıyoruz `WildCard-SuperDemo.live`anahtar adımız .
  
    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure17.png)

### <a name="adding-a-new-web-server-to-bigip-f5"></a>BigIP-F5'e yeni bir Web Sunucusu Ekleme

1. Ana **> IApps > Uygulama Hizmetleri > Uygulama > Oluştur'a**tıklayın.

1. **Adı** sağlayın ve **Şablon** altında **f5.http**seçin .
 
    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure18.png)

1. Bu durumda HeaderApp2'mizi https olarak harici olarak yayınlayacağız, **BIG-IP sistemi SSL Trafiğini nasıl ele almalıdır?** **SSL'yi Istemciden Sonlandırma, sunuculara Düz metin (SSL Boşaltma)** belirtiriz. Sertifikanızı ve Anahtarınızı **hangi SSL sertifikasının** altında kullanmak istediğinizi ve **hangi SSL özel anahtarını kullanmak istediğinizi belirtin?** Sanal Sunucu IÇIN **hangi IP Adresini kullanmak istiyorsunuz?** 

    * **Diğer ayrıntıları belirtin**

        * FQDN  

        * Uygulama havuzundan çıkmalarını belirtin veya yeni bir tane oluşturun.

        * Yeni bir App Server oluşturuyorsanız **dahili IP Adresi** ve **bağlantı noktası numarası**belirtin.

        ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure19.png) 

1. **Bitti'yi**tıklatın.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure20.png) 

1. Uygulama Özelliklerinin değiştirilebilmesini sağlayın. **Ana > IApps > Uygulama Hizmetleri: Uygulamalar >> HeaderApp2'ye**tıklayın. Sıkı **Güncelleştirmelerin** İşaretini Kaldırın (GUI dışındaki bazı ayarları değiştireceğiz). **Güncelleştir** düğmesini tıklatın.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure21.png) 

1. Bu noktada sanal Sunucu göz atmak gerekir.

### <a name="configuring-f5-as-sp-and-azure-as-idp"></a>F5'i SP ve Azure olarak IDP olarak yapılandırma

1.  **Yerel SP Hizmeti > > Federasyon> Yerel SP Hizmetine Eriş'i tıklatın > oluştur veya + işareti'ni tıklatın.**

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure22.png)

1. Servis Sağlayıcı Hizmeti için Ayrıntıları Belirtin. F5 SP Yapılandırması'nı temsil eden **Adı** belirtin. **Varlık Kimliği** belirtin (genellikle uygulama URL'si ile aynı).

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure23.png)

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure24.png)

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure25.png)

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure26.png)

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure27.png)

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure28.png)

### <a name="create-idp-connector"></a>Idp Konektörü Oluşturma

1. **Bind/Unbind IdP Bağlayıcıları** düğmesini tıklatın, **Yeni IdP Bağlayıcısı Oluştur'u** seçin ve **Metadata** seçeneğini seçin ve ardından aşağıdaki adımları gerçekleştirin:
 
    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure29.png)

    a. Azure AD'den indirilen metadata.xml dosyasına göz atın ve bir **Kimlik Sağlayıcı Adı**belirtin.

    b. **Tamam'ı**tıklatın.

    c. Bağlayıcı oluşturulur ve sertifika metadata xml dosyasından otomatik olarak hazırdır.
    
    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure30.png)

    d. Tüm isteği Azure AD'ye göndermek için F5BIG-IP'yi yapılandırın.

    e. **Yeni Satır Ekle'yi**tıklatın, **AzureIDP'yi** seçin (önceki adımlarda oluşturulduğu gibi, 

    f. **Eşleşen Kaynak = %{session.server.landinguri}** 

    g. **Eşleşen Değer = /***

    h. **Güncelleme'yi** tıklatın

    i. **Tamam'ı** tıklatın

    j. **SAML IDP kurulumu tamamlandı**
    
    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure31.png)

### <a name="configure-f5-policy-to-redirect-users-to-azure-saml-idp"></a>Kullanıcıları Azure SAML IDP'ye yönlendirmek için F5 İlkesini yapılandırın

1. Kullanıcıları Azure SAML IDP'ye yönlendirmek için F5 İlkesi'ni yapılandırmak için aşağıdaki adımları gerçekleştirin:

    a. **Ana > Access > Profili/İlkeleri > Erişim Profilleri'ni**tıklatın.

    b. **Oluştur** düğmesine tıklayın.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure32.png)
 
    c. **Ad** belirtin (Örnekte BaşlıkAppAzureSAMLPolicy).

    d. Diğer ayarları özelleştirebilirsiniz lütfen F5 Belgeleri'ne bakın.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure33.png)

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure34.png) 

    e. **Bitti'yi**tıklatın.

    f. İlke oluşturma tamamlandıktan sonra İlke'yi tıklatın ve **Erişim İlkesi** Sekmesine gidin.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure35.png)
 
    g. **Visual Policy düzenleyicisi,** **Profil için Erişim İlkesi'ni** edin.

    h. Görsel Politika düzenleyicisinde + Oturum Aç'a tıklayın ve **SAML Auth'u**seçin.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure36.png)

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure37.png)
 
    i. **Öğe Ekle'yi**tıklatın.

    j. **Özellikler** **altında Ad** belirtin ve **AAA Server** altında önceden yapılandırılmış SP seçin, **KAYDET'i**tıklatın.
 
    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure38.png)

    k. Temel İlke, ek kaynaklar/öznitelik depoları dahil etmek için ilkeyi özelleştirebilirsiniz hazırdır.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure39.png)
 
    l. Üstteki Access **İlkesi Uygula** bağlantısını tıklattığınızdan emin olun.

### <a name="apply-access-profile-to-the-virtual-server"></a>Sanal Sunucuya Erişim Profili Uygula

1. F5 BIG-IP APM'nin profil ayarlarını gelen trafiğe uygulayabilmesi ve önceden tanımlanmış erişim ilkesini çalıştırabilmesi için erişim profilini Sanal Sunucu'ya atayın.

    a. **Ana** > **Yerel Trafik** > **Sanal Sunucular'ı**tıklatın.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure40.png)
 
    b. Sanal sunucuya tıklayın, **Access** **Profili** bölümüne gidin ve oluşturulan SAML İlkesi'ni seçin (örnek BaşlıkAppAzureSAMLPolicy'de)

    c. **Güncelleme'yi** tıklatın
 
    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure41.png)

    d. gelen iddiadan özel SAML özniteliklerini ayıklamak ve arka uç test uygulamasına HTTP üstbilgi olarak geçirmek için bir F5 BIG-IP iRule oluşturun®. **IRule Listesi > Ana > Yerel Trafik > > oluştur'u tıklatın**

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure42.png)
 
    e. Aşağıdaki F5 BIG-IP iRule metnini Tanım penceresine yapıştırın.

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure43.png)
 
    RULE_INIT { ayar statik::debug 0 } ACCESS_ACL_ALLOWED {

    set AZUREAD_USERNAME [ACCESS::session datahttp://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameget "session.saml.last.attr.name. "] if { $static::debug } { log local0. "AZUREAD_USERNAME = $AZUREAD_USERNAME" } if { !( [HTTP::başlık var "AZUREAD_USERNAME"]) } { HTTP::başlık ekleme "AZUREAD_USERNAME" $AZUREAD_USERNAME }

    set AZUREAD_DISPLAYNAME [ACCESS::session datahttp://schemas.microsoft.com/identity/claims/displaynameget "session.saml.last.attr.name. "] if { $static::debug } { log local0. "AZUREAD_DISPLAYNAME = $AZUREAD_DISPLAYNAME" } if { !( [HTTP:::üstbilgi "AZUREAD_DISPLAYNAME" var]) } { HTTP::başlık ekleme "AZUREAD_DISPLAYNAME" $AZUREAD_DISPLAYNAME }

    kümesi AZUREAD_EMAILADDRESS [ACCESS::session datahttp://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddressget "session.saml.last.attr.name. "] if { $static::debug } { log local0. "AZUREAD_EMAILADDRESS = $AZUREAD_EMAILADDRESS" } if { !( [HTTP::başlık var "AZUREAD_EMAILADDRESS"]) } { HTTP:::üstbilgi ekleme "AZUREAD_EMAILADDRESS" $AZUREAD_EMAILADDRESS }}

    **Aşağıdaki örnek çıktı**

    ![F5 (Üstbilgi Tabanlı) yapılandırması](./media/headerf5-tutorial/configure44.png)
 
### <a name="create-f5-test-user"></a>F5 test kullanıcısı oluşturma

Bu bölümde, F5'te B.Simon adında bir kullanıcı oluşturursunuz. Kullanıcıları F5 platformuna eklemek için [F5 İstemci destek ekibiyle](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45) birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir. 

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki F5 döşemesini tıklattığınızda, SSO'yu kurduğunuz F5'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile F5'i deneyin](https://aad.portal.azure.com/)

- [Kerberos uygulaması için F5 tek oturum açma yı yapılandırma](kerbf5-tutorial.md)

- [Gelişmiş Kerberos uygulaması için F5 tek oturum açma yı yapılandırın](advance-kerbf5-tutorial.md)

