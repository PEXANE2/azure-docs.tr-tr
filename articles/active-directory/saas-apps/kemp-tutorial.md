---
title: 'Öğretici Azure Active Directory: Kemp LoadMaster Azure AD tümleştirmesi ile çoklu oturum açma (SSO) Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Kemp LoadMaster Azure AD tümleştirmesi arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f42e1731-0477-4955-9571-b69a0f3de0bc
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 07/28/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a0afdecfcab9cc1ab3903429b197fe859d40a7f
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87424884"
---
# <a name="tutorial-azure-active-directory-sso-integration-with-kemp-loadmaster-azure-ad-integration"></a>Öğretici: Kemp LoadMaster Azure AD tümleştirmesiyle SSO tümleştirmesi Azure Active Directory

Bu öğreticide, Kemp LoadMaster Azure AD tümleştirmesini Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Kemp LoadMaster Azure AD tümleştirmesini Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Kemp LoadMaster Azure AD tümleştirmesine erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Kemp LoadMaster Azure AD tümleştirmesine otomatik olarak kaydolmalarına imkan tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Kemp LoadMaster Azure AD tümleştirmesi çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Kemp LoadMaster Azure AD tümleştirmesi **IDP** tarafından başlatılan SSO 'yu destekler
* Kemp LoadMaster Azure AD tümleştirmesini yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve bir şekilde korunmasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-kemp-loadmaster-azure-ad-integration-from-the-gallery"></a>Galeriden Kemp LoadMaster Azure AD tümleştirmesi ekleme

Kemp LoadMaster Azure AD tümleştirmesinin tümleştirmesini Azure AD ile yapılandırmak için, Galeriden, yönetilen SaaS uygulamaları listenize Kemp LoadMaster Azure AD tümleştirmesi eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Kemp LOADMASTER Azure AD tümleştirmesi** yazın.
1. Sonuçlar panelinden **Kemp LoadMaster Azure AD tümleştirmesi** ' ni seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-kemp-loadmaster-azure-ad-integration"></a>Azure AD SSO 'yu Kemp LoadMaster Azure AD tümleştirmesi için yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu Kemp Loadmaster Azure AD tümleştirmesiyle yapılandırın ve test edin. SSO 'nun çalışması için, Kemp LoadMaster Azure AD tümleştirmesinde bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Kemp LoadMaster Azure AD tümleştirmesiyle yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Kemp LoadMaster Azure AD TÜMLEŞTIRME SSO 'Yu yapılandırın](#configure-kemp-loadmaster-azure-ad-integration-sso)** .

1. **[Web sunucusu yayımlanıyor](#publishing-web-server)**
    1. **[Sanal hizmet oluşturma](#create-a-virtual-service)**
    1. **[Sertifikalar ve güvenlik](#certificates-and-security)**
    1. **[Kemp LoadMaster Azure AD tümleştirmesi SAML profili](#kemp-loadmaster-azure-ad-integration-saml-profile)**
    1. **[Değişiklikleri doğrulama](#verify-the-changes)**
1. **[Kerberos tabanlı kimlik doğrulamasını yapılandırma](#configuring-kerberos-based-authentication)**
    1. **[Kemp LoadMaster Azure AD tümleştirmesi için Kerberos temsili hesabı oluşturma](#create-a-kerberos-delegation-account-for-kemp-loadmaster-azure-ad-integration)**
    1. **[Kemp LoadMaster Azure AD tümleştirme KCD (Kerberos temsili hesapları)](#kemp-loadmaster-azure-ad-integration-kcd-kerberos-delegation-accounts)**
    1. **[Kemp LoadMaster Azure AD tümleştirmesi ESP](#kemp-loadmaster-azure-ad-integration-esp)**
    1. Kemp **[Loadmaster Azure AD tümleştirme testi kullanıcısını](#create-kemp-loadmaster-azure-ad-integration-test-user)** , kullanıcının Azure AD gösterimine bağlı olan Kemp Loadmaster Azure AD tümleştirmesinde B. Simon 'a sahip olmak için oluşturun.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Kemp LOADMASTER Azure AD tümleştirme** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı (VARLıK kimliği)** metin kutusuna bir URL yazın:`https://<KEMP-CUSTOMER-DOMAIN>.com/`

    b. **Yanıt URL** 'si metin kutusuna bir URL yazın:`https://<KEMP-CUSTOMER-DOMAIN>.com/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [Kemp LoadMaster Azure AD tümleştirme istemci destek ekibine](mailto:support@kemp.ax) başvurun. Ayrıca, Azure portal temel SAML yapılandırması bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama Sertifikası** bölümünde **sertifika (base64)** ve **Federasyon meta veri XML**'ı bulun, sertifika ve Federasyon meta veri XML dosyalarını indirmek ve bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](./media/kemp-tutorial/certificate-base-64.png)

1. **Kemp LoadMaster Azure AD tümleştirmesini ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Kemp LoadMaster Azure AD tümleştirmesine erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Kemp LoadMaster Azure AD tümleştirmesi**' ni seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-kemp-loadmaster-azure-ad-integration-sso"></a>Kemp LoadMaster Azure AD tümleştirme SSO 'yu yapılandırma

## <a name="publishing-web-server"></a>Web sunucusu yayımlanıyor 

### <a name="create-a-virtual-service"></a>Sanal hizmet oluşturma 

1. Kemp LoadMaster Azure AD tümleştirme yük ana Web Kullanıcı arabirimine git > sanal hizmetler > Yeni Ekle ' ye gidin.

1. Yeni Ekle ' ye tıklayın.

1. Sanal hizmet için parametreleri belirtin.

    ![Kemp LoadMaster Azure AD tümleştirme Web sunucusu](./media/kemp-tutorial/kemp-1.png)

    a. Sanal adres
    
    b. Bağlantı noktası
    
    c. Hizmet adı (Isteğe bağlı)
    
    d. Protokol 

1. Gerçek sunucular bölümüne gidin.

1. Yeni Ekle ' ye tıklayın.

1. Gerçek sunucu için parametreleri belirtin.
    
    ![Kemp LoadMaster Azure AD tümleştirme Web sunucusu](./media/kemp-tutorial/kemp-2.png)

    a. Uzak adreslere Izin ver ' i seçin
    
    b. Gerçek sunucu adresini yazın
    
    c. Bağlantı noktası
    
    d. İletme yöntemi
    
    e. Ağırlık
    
    f. Bağlantı sınırı
    
    örneğin: Bu gerçek sunucuyu Ekle 'ye tıklayın

## <a name="certificates-and-security"></a>Sertifikalar ve güvenlik
 
### <a name="import-certificate-on-kemp-loadmaster-azure-ad-integration"></a>Kemp LoadMaster Azure AD tümleştirmesinde sertifikayı içeri aktarma 
 
1. Bkz. Azure AD tümleştirme Web portalı > sertifikaları & güvenlik > SSL sertifikaları.

1. Sertifika yapılandırma > sertifikaları Yönet altında.

1. Sertifikayı Içeri Aktar ' a tıklayın.

1. Sertifikayı içeren dosyanın adını belirtin. Dosya aynı zamanda özel anahtarı da barındırabilir. Dosya özel anahtarı içermiyorsa, özel anahtarı içeren dosyanın de belirtilmesi gerekir. Sertifika her ikisi de olabilir. PEK veya. PFX (IIS) biçimi.

1. Sertifika dosyasında Dosya Seç ' e tıklayın.

1. Anahtar dosyasına tıklayın (isteğe bağlı).

1. Kaydet'e tıklayın.

### <a name="ssl-acceleration"></a>SSL hızlandırma
 
1. Sanal hizmetler > sanal hizmetler > Kemp Load ana Web Kullanıcı arabirimi ' ne gidin.

1. Işlem altında Değiştir 'e tıklayın.

1. TCP/x.x.x. için Özellikler: 443 (KIMLIK: 6)-katman 7 ' de çalışıyor, SSL özellikleri ' ne tıklayın.
    
    ![Kemp LoadMaster Azure AD tümleştirme Web sunucusu](./media/kemp-tutorial/kemp-3.png)
    
    a. SSL hızlandırmalı etkin öğesine tıklayın.
    
    b. Kullanılabilir Sertifikalar ' ın altında, içeri aktarılan sertifikayı seçin ve sembol ' e tıklayın `>` .
    
    c. Atanan Sertifikalar ' da istenen SSL sertifikası göründüğünde, **sertifikaları ayarla**' ya tıklayın.

    > [!NOTE]
    > **Sertifikaları ayarla**' ya tıkladığınızdan emin olun.

## <a name="kemp-loadmaster-azure-ad-integration-saml-profile"></a>Kemp LoadMaster Azure AD tümleştirmesi SAML profili
 
### <a name="import-idp-certificate"></a>IDP sertifikasını içeri aktar

Kemp LoadMaster Azure AD tümleştirme web konsoluna git 

1. Sertifikalar ve yetkili altındaki ara Sertifikalar ' a tıklayın.

    ![Kemp LoadMaster Azure AD tümleştirme Web sunucusu](./media/kemp-tutorial/kemp-6.png)

    a. Yeni ara sertifika ekle ' de Dosya Seç ' e tıklayın.
    
    b. Daha önce Azure AD kurumsal uygulamasından indirilen sertifika dosyasına gidin.
    
    c. Aç ' a tıklayın.
    
    d. Sertifika adında bir ad sağlayın.
    
    e. Sertifika Ekle ' ye tıklayın.

### <a name="create-authentication-policy"></a>Kimlik doğrulama Ilkesi oluştur 
 
Sanal hizmetler altında SSO Yönetimi ' ne gidin.

   ![Kemp LoadMaster Azure AD tümleştirme Web sunucusu](./media/kemp-tutorial/kemp-7.png)
   
   a. Bir ad verdikten sonra yeni Istemci tarafı Yapılandırması Ekle altında Ekle ' ye tıklayın.

   b. Kimlik Doğrulama Protokolü altında SAML ' yi seçin.

   c. IDP sağlama altında meta veri dosyasını seçin. 

   d. Dosya Seç ' e tıklayın.

   e. Azure portal 'tan daha önce indirilen XML 'e gidin.

   f. Aç ' a tıklayın ve IDP meta veri dosyasını Içeri Aktar ' a tıklayın.

   örneğin: IDP sertifikasından ara sertifikayı seçin.

   h. Azure portal içinde oluşturulan kimlikle eşleşmesi gereken SP varlık KIMLIĞINI ayarlayın 

   i. SP varlık KIMLIĞINI ayarla ' ya tıklayın.

### <a name="set-authentication"></a>Kimlik doğrulamasını ayarla  
 
Kemp LoadMaster Azure AD tümleştirme Web konsolunda

1. Sanal hizmetler ' e tıklayın.

1. Hizmetleri görüntüle/Değiştir ' e tıklayın.

1. Değiştir ' e tıklayın ve ESP seçenekleri ' ne gidin.
    
    ![Kemp LoadMaster Azure AD tümleştirme Web sunucusu](./media/kemp-tutorial/kemp-8.png)

    a. ESP 'yi etkinleştir ' e tıklayın.
    
    b. Istemci kimlik doğrulaması modunda SAML ' yi seçin.
    
    c. SSO etki alanında daha önce oluşturulan Istemci tarafı kimlik doğrulaması ' nı seçin.
    
    d. Izin verilen sanal konaklara ana bilgisayar adını yazın ve Izin verilen sanal konakları ayarla ' ya tıklayın.
    
    e. Izin verilen sanal dizinlere (erişim gereksinimlerine göre) yazın ve Izin verilen dizinleri ayarla ' ya tıklayın.

### <a name="verify-the-changes"></a>Değişiklikleri doğrulama 
 
Uygulama URL 'sine gidin 

Daha önce kimliği doğrulanmamış erişim yerine kiralandırılmış oturum açma sayfanızı görmeniz gerekir. 

![Kemp LoadMaster Azure AD tümleştirme Web sunucusu](./media/kemp-tutorial/kemp-9.png)

## <a name="configuring-kerberos-based-authentication"></a>Kerberos tabanlı kimlik doğrulamasını yapılandırma 
 
### <a name="create-a-kerberos-delegation-account-for-kemp-loadmaster-azure-ad-integration"></a>Kemp LoadMaster Azure AD tümleştirmesi için Kerberos temsili hesabı oluşturma 

1. Bir kullanıcı hesabı oluşturun (Bu örnekte Apptemsili).
    
    ![Kemp LoadMaster Azure AD tümleştirme Web sunucusu](./media/kemp-tutorial/kemp-10.png)


    a. Öznitelik Düzenleyicisi sekmesini seçin.

    b. ServicePrincipalName öğesine gidin. 

    c. ServicePrincipalName öğesini seçin ve Düzenle ' ye tıklayın.

    d. Eklenecek değer alanına http/kcduser yazın ve Ekle ' ye tıklayın. 

    e. Uygula ve Tamam ' a tıklayın. Yeniden açmadan önce pencerenin kapatılması gerekir (yeni temsili sekmesini görmek için). 

1. Kullanıcı Özellikleri penceresini yeniden açın ve temsili sekmesi kullanılabilir hale gelir. 

1. Temsilci Seçme sekmesini seçin.

    ![Kemp LoadMaster Azure AD tümleştirme Web sunucusu](./media/kemp-tutorial/kemp-11.png)

    a. Bu kullanıcıya yalnızca belirtilen hizmetlere yetki vermek için güven ' i seçin.

    b. Herhangi bir kimlik doğrulama protokolünü kullan'ı seçin.

    c. Gerçek sunucuları ekleyin ve hizmet olarak http 'yi ekleyin. 
    
    d. Genişletilmiş onay kutusunu seçin. 

    e. Tüm sunucuları hem ana bilgisayar adına hem de FQDN 'ye sahip olacak şekilde görebilirsiniz.
    
    f. Tamam ' a tıklayın.

> [!NOTE]
> Uygulama/Web sitesinde SPN 'yi uygun şekilde ayarlayın. Uygulama havuzu kimliği ayarlandığında uygulamaya erişmek için. FQDN adını kullanarak IIS uygulamasına erişmek için, gerçek sunucu komut istemi ' ne gidin ve gerekli parametrelerle SetSpn yazın. Örneğin, Setspn – S HTTP/sescoindc. sunehes. Co., suneshes\kdcuser 

### <a name="kemp-loadmaster-azure-ad-integration-kcd-kerberos-delegation-accounts"></a>Kemp LoadMaster Azure AD tümleştirme KCD (Kerberos temsili hesapları) 

Kemp LoadMaster Azure AD tümleştirme web konsoluna gidin > sanal Hizmetleri > SSO 'yu yönetin.

![Kemp LoadMaster Azure AD tümleştirme Web sunucusu](./media/kemp-tutorial/kemp-12.png)

a. Sunucu tarafı çoklu oturum açma yapılandırması ' na gidin.

b. Yeni sunucu tarafı Yapılandırması Ekle ' de ad yazın ve Ekle ' ye tıklayın.

c. Kimlik doğrulama protokolünde Kerberos kısıtlanmış temsilciyi seçin.

d. Kerberos bölgesine etki alanı adını yazın.

e. Kerberos bölgesine ayarla ' ya tıklayın.

f. Kerberos Anahtar Dağıtım Merkezi etki alanı denetleyicisi IP adresini yazın.

örneğin: Kerberos KDC ayarla ' ya tıklayın.

h. Kerberos Güvenilen Kullanıcı adında KCD Kullanıcı adı yazın.

i. KDC Güvenilen Kullanıcı adını ayarla ' ya tıklayın.

j. Kerberos Güvenilen Kullanıcı parolasına parola yazın.

k. KCD Güvenilen Kullanıcı parolasını ayarla ' ya tıklayın.

### <a name="kemp-loadmaster-azure-ad-integration-esp"></a>Kemp LoadMaster Azure AD tümleştirmesi ESP 

Sanal hizmetler > hizmetleri görüntüle/Değiştir ' e gidin.

![Kemp LoadMaster Azure AD tümleştirme Web sunucusu](./media/kemp-tutorial/kemp-13.png)

a. Sanal hizmetin Nick adı üzerinde Değiştir 'e tıklayın.
    
b. ESP seçenekleri ' ne tıklayın.
    
c. Sunucu kimlik doğrulama modu altında, KCD ' yi seçin.
        
d. Sunucu tarafı yapılandırması altında, önceden oluşturulmuş sunucu tarafı profilini seçin.

### <a name="create-kemp-loadmaster-azure-ad-integration-test-user"></a>Kemp LoadMaster Azure AD tümleştirme test kullanıcısı oluşturma

Bu bölümde, Kemp LoadMaster Azure AD tümleştirmesinde B. Simon adlı bir Kullanıcı oluşturacaksınız. Kemp Loadmaster Azure AD tümleştirme platformunda kullanıcıları eklemek için [Kemp LoadMaster Azure AD tümleştirme istemci destek ekibi](mailto:support@kemp.ax) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Kemp LoadMaster Azure AD tümleştirme kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Kemp LoadMaster Azure AD tümleştirmesinde otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Kemp LoadMaster Azure AD tümleştirmesini deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimlerle Kemp LoadMaster Azure AD tümleştirmesini koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
