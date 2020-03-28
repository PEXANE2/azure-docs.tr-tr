---
title: 'Öğretici: F5 ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve F5 arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 39382eab-05fe-4dc2-8792-62d742dfb4e1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/19/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a1f06b0b5aa59328d2fe39d501cfdf3ad7524427
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75431465"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-f5"></a>Öğretici: F5 ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, F5'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. F5'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* F5 erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla F5'te otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

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

2. **Kılavuzlu Yapılandırma** sayfasında, sol üst köşedeki **Yükseltme Kılavuzlu Yapılandırma'ya** tıklayın.

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure14.png) 

3. Yükseltme Kılavuzu Yapılandırma pop ekranında, indirilen kullanım örneği paketini yüklemek için **Dosyayı Seç'i** seçin ve **Yükle ve Yükle** düğmesine tıklayın.

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure15.png) 

4. Yükseltme tamamlandığında, **Devam et** düğmesini tıklatın.

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure16.png)

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* F5 **SP ve IDP** SSO başlatılan destekler
* F5 SSO üç farklı şekilde yapılandırılabilir.

- [Kerberos uygulaması için F5 tek oturum açma yı yapılandırma](#configure-f5-single-sign-on-for-kerberos-application)

- [Üstbilgi Tabanlı uygulama için F5 tek oturum açma yı yapılandırma](headerf5-tutorial.md)

- [Gelişmiş Kerberos uygulaması için F5 tek oturum açma yı yapılandırın](advance-kerbf5-tutorial.md)

### <a name="key-authentication-scenarios"></a>Anahtar Kimlik Doğrulama Senaryoları

Open ID Connect, SAML ve WS-Fed gibi modern kimlik doğrulama protokolleri için Azure Active Directory yerel tümleştirme desteğinin yanı sıra F5, Azure AD ile hem dahili hem de harici erişim için eski tabanlı kimlik doğrulama uygulamaları için güvenli erişimi genişleterek bu uygulamalara modern senaryolar (örn. parolasız erişim). Buna şunlar dahildir:

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

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde, **Federation Metadata XML** ve **Sertifikayı (Base64)** bulun ve ardından sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

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

- [Üstbilgi Tabanlı uygulama için F5 tek oturum açma yı yapılandırma](headerf5-tutorial.md)

- [Gelişmiş Kerberos uygulaması için F5 tek oturum açma yı yapılandırın](advance-kerbf5-tutorial.md)

### <a name="configure-f5-single-sign-on-for-kerberos-application"></a>Kerberos uygulaması için F5 tek oturum açma yı yapılandırma

### <a name="guided-configuration"></a>Güdümlü Yapılandırma

1. Yeni bir web tarayıcısı penceresi açın ve F5 (Kerberos) şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Meta veri sertifikasını, kurulum işleminde daha sonra kullanılacak olan F5'e aktarmanız gerekir.

1. Sistem **> Sertifika Yönetimi > Trafik Sertifika Yönetimi > SSL Sertifika Listesi'ne**gidin. Sağ köşeden **Içeri Aktar'ı** seçin. Bir **Sertifika Adı** belirtin (daha sonra config'e başvurulacaktır). Sertifika **Kaynağı'nda,** Yükleme Dosyası'nı seçin, SAML Tek İşaretini yapılandırırken Azure'dan indirilen sertifikayı belirtin. **İçeri Aktar**’a tıklayın.

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure01.png) 

1. Ayrıca, **Uygulama Hostname için SSL Sertifikası gerekir. Sistem > Sertifika Yönetimi > Trafik Sertifika Yönetimi > SSL Sertifika Listesi'ne gidin.** Sağ köşeden **Içeri Aktar'ı** seçin. **İthalat Türü** **PKCS 12(IIS)** olacaktır. Bir **Anahtar Adı** belirtin (config'de daha sonra başvurulacaktır) ve PFX dosyasını belirtin. PFX için **Parola'yı** belirtin. **İçeri Aktar**’a tıklayın.

    >[!NOTE]
    >Örnekte uygulama adımız `Kerbapp.superdemo.live`, Bir Joker Sertifikası kullanıyoruz anahtar adımız`WildCard-SuperDemo.live`

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure02.png) 
 
1. Azure AD Federasyonu ve Uygulama Erişimi'ni kurmak için Rehberli Deneyimi kullanacağız. Git - F5 BIG-IP **Main** ve **Erişim > Güdümlü Yapılandırma > Federasyonu > SAML Servis Sağlayıcısı**seçin. **Yapılandırmayı** başlatmak için **İleri'yi** tıklatın.

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure03.png) 

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure04.png)

1. Yapılandırma **Adı**sağlayın. Varlık **Kimliğini** (Azure AD Uygulaması Yapılandırması'nda yapılandırdığınız la aynı) belirtin. Ana **Bilgisayar adını**belirtin. Başvuru için **açıklama** ekleyin. Kalan varsayılan girişleri kabul edin ve seçin ve sonra **Kaydet & Sonraki'ni**tıklatın.

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure05.png) 

1. Bu örnekte port 443 ile 192.168.30.200 olarak yeni bir Sanal Sunucu oluşturuyoruz. **Hedef Adreste**Sanal Sunucu IP adresini belirtin. İstemci **SSL Profilini**seçin, yeni oluştur'u seçin. Daha önce yüklenmiş uygulama sertifikasını( bu örnekteki joker kart sertifikasını) ve ilişkili anahtarı belirtin ve ardından **Sonraki& kaydet'i**tıklatın.

    >[!NOTE]
    >Bu örnekte Dahili websunucumuz 80 nolu bağlantı noktasında çalışıyor ve 443 ile yayınlamak istiyoruz.

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure06.png)

1. **IdP bağlayıcınızı yapılandırmak,** Meta verilerinizi belirtmek, Dosya Yı Seç'i tıklatın ve Azure AD'den daha önce indirilen Metadata XML dosyasını yüklemek için Select yöntemi altında. SAML IDP bağlayıcısı için benzersiz bir **Ad** belirtin. Daha önce yüklenen **Meta veri imzalama sertifikasını** seçin. **Sonraki & Kaydet'i**tıklatın.

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure07.png)  

1. **Bir Havuz Seç'in**altında, **Yeni Oluştur'u** belirtin (alternatif olarak zaten var olan bir havuz seçin). Diğer değer varsayılan olsun. Havuz Sunucuları altında IP **Adresi/Düğüm Adı**altında IP Adresi yazın. Bağlantı **Noktasını**belirtin. **Sonraki & Kaydet'i**tıklatın.
 
    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure08.png)

1. Tek Oturum Açma Ayarları **ekranında, Tek Oturum Açma'yı etkinleştir'i**seçin. **Seçilen Tek İşaret-On Türü** altında **Kerberos**seçin. **session.saml.last.Identity** ile **session.saml.last.attr.name.Identity** under **Username Source** (Azure AD'deki talep eşlemesini kullanarak ayarlanan bu değişken) değiştirin. **Gelişmiş Ayarı Göster'i**seçin. **Kerberos Realm** altında Alan Adı yazın. **Hesap Adı/ Hesap Şifresi** altında APM Temsilcilik Hesabı ve Şifresini belirtin. **KDC** Alanında Etki Alanı Denetleyicisi IP'sini belirtin. **Sonraki & Kaydet'i**tıklatın.

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure09.png)   

1. Bu kılavuz amacıyla, uç nokta denetimlerini atlarız.  Ayrıntılar için F5 belgelerine bakın.  Ekranda **Kaydet & İleri'yi**seçin.

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure10.png) 

1. Varsayılanları kabul edin ve **Sonraki& kaydet'i**tıklatın. SAML oturum yönetimi ayarlarıyla ilgili ayrıntılar için F5 belgelerine başvurun.


    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure11.png) 
 
1. Özet ekranını gözden geçirin ve BIG-IP'yi yapılandırmak için **Dağıt'ı** seçin.
 
    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure12.png)

1. Uygulama yapılandırıldıktan sonra **Finish'e**tıklayın.

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure13.png)

## <a name="advanced-configuration"></a>Gelişmiş Yapılandırma

>[!NOTE]
>Referans için [tıklayınız](https://techdocs.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-single-sign-on-11-5-0/2.html)

### <a name="configuring-an-active-directory-aaa-server"></a>Active Directory AAA sunucusuyapılandırma

Access İlke Yöneticisi'nde (APM) bir Active Directory AAA sunucusu, APM'nin kullanıcıların kimliğini doğrulamak için kullanması için etki alanı denetleyicilerini ve kimlik bilgilerini belirtmek için yapılandırırsınız.

1.  Ana sekmede, **Etkin Dizin > AAA Sunucuları > Erişim İlkesi'ni**tıklatın. Active Directory Servers listesi ekranı açılır.

2.  **Oluştur'u**tıklatın. Yeni Sunucu özellikleri ekranı açılır.

3.  **Ad** alanında, kimlik doğrulama sunucusu için benzersiz bir ad yazın.

4.  Etki **Alanı Adı** alanında, Windows etki alanının adını yazın.

5.  Sunucu **Bağlantısı** ayarı için aşağıdaki seçeneklerden birini seçin:

    * AAA sunucusu için yüksek kullanılabilirlik ayarlamak için **Havuzu Kullan'ı** seçin.

    * Bağımsız işlevsellik için AAA sunucusunu kurmak için **Doğrudan'ı** seçin.

6.  **Direct'i**seçtiyseniz, Etki **Alanı Denetleyicisi** alanına bir ad yazın.

7.  **Havuzu**Kullan'ı seçtiyseniz, havuzu yapılandırın:

    * **Etki Alanı Denetleyicisi Havuz Adı** alanına bir ad yazın.

    * Her biri için IP adresini ve ana bilgisayar adını yazarak ve **Ekle** düğmesini tıklatarak havuzdaki **Etki Alanı Denetleyicilerini** belirtin.

    * AAA sunucusunun durumunu izlemek için bir sistem durumu monitörü seçme seçeneğiniz var: bu durumda yalnızca **gateway_icmp** monitör uygundur; **Sunucu Havuzu İzleme** listesinden seçebilirsiniz.

8.  Yönetici **Adı** alanında, Active Directory yönetim izinleri olan bir yönetici için büyük/küçük harf duyarlı bir ad yazın. APM, AD Sorgusu için **Yönetici Adı** ve **Yönetici Parolası** alanlarındaki bilgileri kullanır. Etkin Dizin anonim sorgular için yapılandırılırsa, yönetici adı sağlamanız gerekmez. Aksi takdirde, APM'nin Active Directory sunucusuna bağlamak, kullanıcı grubu bilgilerini almak ve parolayla ilgili işlevselliği desteklemek için Active Directory parola ilkelerini getirmek için yeterli ayrıcalığa sahip bir hesaba ihtiyacı vardır. (Örneğin, AD Sorgusu eyleminde son kullanma tarihinden önce parolayı değiştirmek için İstem kullanıcısını seçerseniz, APM parola ilkeleri getirmelidir.) Bu yapılandırmada Yönetici hesap bilgilerini sağlamazsanız, APM bilgileri almak için kullanıcı hesabını kullanır. Kullanıcı hesabı yeterli ayrıcalığa sahipse bu çalışır.

9.  Yönetici **Parolası** alanına, Etki Alanı Adı ile ilişkili yönetici parolasını yazın.

10. Yönetici **Parolasını Doğrula** alanında, Etki Alanı **Adı** ayarı ile ilişkili yönetici parolasını yeniden yazın.

11. Grup **Önbellek Ömür Boyu** alanında gün sayısını yazın. Varsayılan ömür 30 gündür.

12. Parola **Güvenlik Nesnesi Önbelleği Yaşam Boyu** alanına gün sayısını yazın. Varsayılan ömür 30 gündür.

13. **Kerberos Kimlik Doğrulama Şifreleme Türü** listesinden bir şifreleme türü seçin. Varsayılan **yok.** Bir şifreleme türü belirtirseniz, BIG-IP sistemi ilk kimlik doğrulama hizmeti isteği (AS-REQ) paketinde Kerberos kimlik doğrulama verilerini içerir.

14. Zaman **aralığı** alanında, AAA sunucusu için bir zaman aralığı (saniye cinsinden) yazın. (Bu ayar isteğe bağlıdır.)

15. **Bitti'yi**tıklatın. Yeni sunucu listede görüntülenir. Bu, yeni Active Directory sunucusunu Active Directory Servers listesine ekler.

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure17.png)

### <a name="saml-configuration"></a>SAML Yapılandırması

1. Meta veri sertifikasını, kurulum işleminde daha sonra kullanılacak olan F5'e aktarmanız gerekir. Sistem **> Sertifika Yönetimi > Trafik Sertifika Yönetimi > SSL Sertifika Listesi'ne**gidin. Sağ köşeden **Içeri Aktar'ı** seçin.

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure18.png)

2. SAML IDP'yi kurmak **için, Access > Federation > SAML: Service Provider > External Idp Connectors'a gidin**ve **Meta verilerden > oluştur'u**tıklatın.

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure19.png)

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure20.png)

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure21.png)

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure22.png)

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure23.png)

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure24.png)

1. SAML SP'yi ayarlamak için **Access > Federation > SAML Servis Sağlayıcısı'na gidin > Yerel SP Hizmetleri'ni** tıklatın ve **Oluştur'u**tıklatın. Aşağıdaki bilgileri tamamlayın ve **Tamam'ı**tıklatın.

    * Tür Adı: KerbApp200SAML
    * Varlık Kimliği*:https://kerbapp200.superdemo.live
    * SP Ad Ayarları
    * Şema: https
    * Ev sahibi: kerbapp200.superdemo.live
    * Açıklama: kerbapp200.superdemo.live

     ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure25.png)

     b. SP Configuration, KerbApp200SAML'i seçin ve **Bind/UnBind IdP Bağlayıcılarını**tıklatın.

     ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure26.png)

     ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure27.png)

     c. Yeni **Satır Ekle'ye** tıklayın ve önceki adımda oluşturulan **Harici IdP bağlayıcısını** seçin, **Güncelleştir'i**tıklatın ve ardından **Tamam'ı**tıklatın.

     ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure28.png)

1. Kerberos SSO yapılandırma için, **Tek Oturum açma > Kerberos > Erişim**gidin, tam bilgi ve **Bitmiş**tıklayın .

    >[!Note]
    > Oluşturulacak ve belirtilecek Kerberos Delegasyon Hesabı'na ihtiyacınız olacaktır. Bkz. KCD Bölümü (Değişken Referanslar Için Ek)

    * **Kullanıcı Adı Kaynak**: session.saml.last.attr.name.http:\//schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname

    * **Kullanıcı Diyarı Kaynak**: session.logon.last.domain

        ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure29.png)

1. Access Profilini yapılandırmak için **Access > Profili/İlkeler> Erişim Profiline**gidin , **Oluştur'u**tıklatın, aşağıdaki bilgileri tamamlayın ve **Bitti'yi**tıklatın.

    * Adı: KerbApp200
    * Profil Türü: Tümü
    * Profil Kapsamı: Profil
    * Diller: İngilizce

        ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure30.png)

1. Adı, KerbApp200 tıklayın, aşağıdaki bilgileri tamamlamak ve **Güncelleme**tıklayın.

    * Etki Alanı Çerez: superdemo.live
    * SSO Yapılandırması: KerAppSSO_sso

        ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure31.png)

1. **Access Politikası'nı** tıklatın ve ardından Profil "KerbApp200" için **Erişim İlkesi'ni Edit'i** tıklatın.

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure32.png)

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure33.png)

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure34.png)

    * **session.logon.last.usernameUPN expr {[mcget {session.saml.last.identity}]}**

    * **session.ad.lastactualdomain TEXT superdemo.live**

        ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure35.png)

    * **(userPrincipalName=%{session.logon.last.usernameUPN})**

        ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure36.png)

        ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure37.png)

    * **session.logon.last.username expr { "[mcget {session.ad.last.attr.sAMAccountName}]]" }**

        ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure38.png)

    * **mcget {session.logon.last.username}**
    * **mcget {session.logon.last.password**

1. Yeni Düğüm eklemek için **Yerel Trafik > Düğümleri > Düğüm Listesi'ne gidin, Oluştur'u tıklatın,** aşağıdaki bilgileri tamamlayın ve **ardından Bitti'yi**tıklatın.

    * Adı: KerbApp200
    * Açıklama: KerbApp200
    * Adres: 192.168.20.200

        ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure39.png)

1. Yeni bir Havuz oluşturmak için **Yerel Trafik > Havuzları > Havuz Listesi'ne gidin, Oluştur'u tıklatın,** aşağıdaki bilgileri tamamlayın ve **Bitti'yi**tıklatın.

    * Adı: KerbApp200-Havuz
    * Açıklama: KerbApp200-Havuz
    * Sağlık Monitörleri: http
    * Adres: 192.168.20.200
    * Servis Noktası: 81

        ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure40.png)

1. Sanal Sunucu oluşturmak **için, Yerel Trafik > Sanal Sunucular > Sanal Sunucu Listesi > +** gidin , aşağıdaki bilgileri doldurun ve **Bitti**tıklayın .

    * Adı: KerbApp200
    * Hedef Adres/Maske: Host 192.168.30.200
    * Servis Bağlantı Noktası: Bağlantı Noktası 443 HTTPS
    * Erişim Profili: KerbApp200
    * Önceki Adımda Oluşturulan Erişim Profilini Belirt

        ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure41.png)

        ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure42.png)

### <a name="setting-up-kerberos-delegation"></a>Kerberos Delegasyonunun Kurulması 

>[!NOTE]
>Referans için [tıklayınız](https://www.f5.com/pdf/deployment-guides/kerberos-constrained-delegation-dg.pdf)

*  **Adım 1:** Temsilci Hesabı Oluşturma

    **Örnek:**
    * Alan Adı: **superdemo.live**

    * Sam Hesap Adı: **büyük ipuser**

    * Yeni ADUser -Name "APM Delegation Account" host/big-ipuser.superdemo.live@superdemo.live -UserPrincipalName -SamAccountName "big-ipuser" -PasswordNeverExpires $true -Enabled $true -AccountPassword (Read-Host -AsSecureString "Password!1234")

* **Adım 2:** SPN'yi ayarlayın (APM Temsilci Hesabı'nda)

    **Örnek:**
    * setspn –Bir **host/big-ipuser.superdemo.live** big-ipuser

* **Adım 3:** SPN Delegasyonu (Uygulama Hizmeti Hesabı için) F5 Temsilciliği Hesabı için uygun Temsilciliği kur.
    Aşağıdaki örnekte, APM Delegasyonu hesabı FRP-App1.superdemo için KCD için yapılandırıldı. canlı uygulama.

    ![F5 (Kerberos) konfigürasyonu](./media/kerbf5-tutorial/configure43.png)

* Yukarıdaki referans belgede belirtilen ayrıntıları [bu](https://techdocs.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-single-sign-on-11-5-0/2.html)altında sağlayın.

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

- [Üstbilgi Tabanlı uygulama için F5 tek oturum açma yı yapılandırma](headerf5-tutorial.md)

- [Gelişmiş Kerberos uygulaması için F5 tek oturum açma yı yapılandırın](advance-kerbf5-tutorial.md)

