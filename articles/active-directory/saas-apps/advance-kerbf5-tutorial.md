---
title: 'Öğretici: F5 ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve F5 arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9c5fb47a-1c5d-437a-b4c1-dbf739eaf5e3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 984fd0c7946a50922315269c87e08b1c35b74348
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74074751"
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

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* F5 **SP ve IDP** SSO başlatılan destekler
* F5 SSO üç farklı şekilde yapılandırılabilir.

- [Gelişmiş Kerberos uygulaması için F5 tek oturum açma yı yapılandırın](#configure-f5-single-sign-on-for-advanced-kerberos-application)

- [Üstbilgi Tabanlı uygulama için F5 tek oturum açma yı yapılandırma](headerf5-tutorial.md)

- [Kerberos uygulaması için F5 tek oturum açma yı yapılandırma](kerbf5-tutorial.md)

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
1. **[F5-SSO'yu yapılandırın](#configure-f5-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
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

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

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

## <a name="configure-f5-sso"></a>F5 SSO'nun yapılandırılsın

- [Üstbilgi Tabanlı uygulama için F5 tek oturum açma yı yapılandırma](headerf5-tutorial.md)

- [Kerberos uygulaması için F5 tek oturum açma yı yapılandırma](kerbf5-tutorial.md)

### <a name="configure-f5-single-sign-on-for-advanced-kerberos-application"></a>Gelişmiş Kerberos uygulaması için F5 tek oturum açma yı yapılandırın

1. Yeni bir web tarayıcısı penceresi açın ve F5 (Advanced Kerberos) şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Meta veri sertifikasını, kurulum işleminde daha sonra kullanılacak olan F5'e (Gelişmiş Kerberos) aktarmanız gerekir. Sistem **> Sertifika Yönetimi > Trafik Sertifika Yönetimi >> SSL Sertifika Listesine**gidin. Sağ **Import** köşenin İthalat'ına tıklayın.

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure01.png)
 
1. SAML IDP'yi kurmak için **Access > Federation > SAML Servis Sağlayıcısı'na gidin > Meta verilerden > oluşturun.**

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure02.png)

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure03.png)
 
    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure04.png)

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure05.png)
 
1. Görev 3'ten yüklenen Sertifikayı belirtin

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure06.png)

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure07.png)

 1. SAML SP'yi kurmak için **Access > Federation > SAML Service Federation > Yerel SP Hizmetleri > Oluşturun'a**gidin.

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure08.png)
 
1. **Tamam**'a tıklayın.

1. SP Yapılandırmasını seçin ve **Bind/UnBind IdP Bağlayıcılarını**tıklatın.

     ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure09.png)
 
 
1. Yeni **Satır Ekle'yi** tıklatın ve önceki adımda oluşturulan **Harici IdP konektörünü** seçin.

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure10.png)
 
1. Kerberos SSO'nun yapılandırılması **için, Tek Oturum açma > > Kerberos'a Erişin**

    >[!Note]
    >Oluşturulacak ve belirtilecek Kerberos Delegasyon Hesabı'na ihtiyacınız olacaktır. Bkz. KCD Bölümü ( Değişken Referanslar Için Ek)

    • Kullanıcı Adı Kaynağı`session.saml.last.attr.name.http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`

    • Kullanıcı Alemi Kaynak`session.logon.last.domain`

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure11.png)

1. Access Profilini yapılandırmak için, **Access > Profili/İlkeleri > Erişim Profili (oturum ilkeleri başına)**.

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure12.png)

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure13.png)

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure14.png)

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure15.png)

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure16.png)
 
    * session.logon.last.usernameUPN expr {[mcget {session.saml.last.identity}]}

    * session.ad.lastactualdomain TEXT superdemo.live

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure17.png)

    * (userPrincipalName=%{session.logon.last.usernameUPN})

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure18.png)

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure19.png)

    * session.logon.last.username expr { "[mcget {session.ad.last.attr.sAMAccountName}]]" }

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure20.png)

    * mcget {session.logon.last.username}
    * mcget {session.logon.last.password}

1. Yeni düğüm eklemek için **Yerel Trafik > Düğümleri > Düğüm Listesi > +** gidin.

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure21.png)
 
1. Yeni bir Havuz oluşturmak için **Yerel Trafik > Havuzları'na**gidin > Havuz Listesi > Oluşturun.

     ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure22.png)

 1. Yeni bir sanal sunucu oluşturmak **için, Sanal Sunucular > Sanal Sunucular > + > Yerel Trafik > gidin.**

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure23.png)

1. Önceki Adımda Oluşturulan Erişim Profilini Belirtin.

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure24.png) 

### <a name="setting-up-kerberos-delegation"></a>Kerberos Delegasyonunun Kurulması 

>[!Note]
>Daha fazla bilgi için [buraya](https://www.f5.com/pdf/deployment-guides/kerberos-constrained-delegation-dg.pdf) bakın

* **Adım 1: Temsilci Hesabı Oluşturma**

    * Örnek
    ```
    Domain Name : superdemo.live
    Sam Account Name : big-ipuser

    New-ADUser -Name "APM Delegation Account" -UserPrincipalName host/big-ipuser.superdemo.live@superdemo.live -SamAccountName "big-ipuser" -PasswordNeverExpires $true -Enabled $true -AccountPassword (Read-Host -AsSecureString "Password!1234")
    ```

* **Adım 2: SPN'yi ayarlama (APM Temsilcilik Hesabında)**

    *  Örnek
    ```
    setspn –A host/big-ipuser.superdemo.live big-ipuser
    ```

* **Adım 3: SPN Delegasyonu (Uygulama Hizmeti Hesabı için)**

    * F5 Temsilciliği Hesabı için uygun Temsilciliği hazırla.
    * Aşağıdaki örnekte, APM Delegasyonu hesabı FRP-App1.superdemo.live uygulaması için KCD için yapılandırıldı.

        ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure25.png)

1. Yukarıdaki referans belgede belirtilen ayrıntıları [bu](https://techdocs.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-single-sign-on-11-5-0/2.html) belge kapsamında sağlayın

1. Ek- SAML – F5 BIG-IP Değişken haritalamaları aşağıda gösterilmiştir:

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure26.png)

    ![F5 (Gelişmiş Kerberos) yapılandırması](./media/advance-kerbf5-tutorial/configure27.png) 

1. Aşağıda varsayılan SAML Öznitelikleri tüm listesidir. GivenName aşağıdaki dize kullanılarak temsil edilir.
`session.saml.last.attr.name.http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`

| | |
| -- | -- |
| eb46b6b6.session.saml.last.assertionID | `<TENANT ID>` |
| eb46b6b6.session.saml.last.assertionIssueInstant  | `<ID>` |
| eb46b6b6.session.saml.last.assertionIssuer | `https://sts.windows.net/<TENANT ID>`/ |
| eb46b6b6.session.saml.last.attr.name.http:\//schemas.microsoft.com/claims/authnmethodsreferences | `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` |
| eb46b6b6.session.saml.last.attr.name.http:\//schemas.microsoft.com/identity/claims/displayname | kullanıcı0 |
| eb46b6b6.session.saml.last.attr.name.http:\//schemas.microsoft.com/identity/claims/identityprovider | `https://sts.windows.net/<TENANT ID>/` |
| eb46b6b6.session.saml.last.attr.name.http:\//schemas.microsoft.com/identity/claims/objectidentifier | `<TENANT ID>` |
| eb46b6b6.session.saml.last.attr.name.http:\//schemas.microsoft.com/identity/claims/tenantid | `<TENANT ID>` |
| eb46b6b6.session.saml.last.attr.name.http:\//schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress | `user0@superdemo.live` |
| eb46b6b6.session.saml.last.attr.name.http:\//schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname | kullanıcı0 |
| eb46b6b6.session.saml.last.attr.name.http:\//schemas.xmlsoap.org/ws/2005/05/identity/claims/name | `user0@superdemo.live` |
| eb46b6b6.session.saml.last.attr.name.http:\//schemas.xmlsoap.org/ws/2005/05/identity/claims/surname | 0 |
| eb46b6b6.session.saml.last.audience | `https://kerbapp.superdemo.live` |
| eb46b6b6.session.saml.last.authNContextClassRef | vazo:vaha:isimler:tc:SAML:2.0:ac:sınıflar:Şifre |
| eb46b6b6.session.saml.last.authNInstant | `<ID>` |
| eb46b6b6.session.saml.last.identity | `user0@superdemo.live` |
| eb46b6b6.session.saml.last.inResponseTo | `<TENANT ID>` |
| eb46b6b6.session.saml.last.nameIDValue | `user0@superdemo.live` |
| eb46b6b6.session.saml.last.nameIdFormat | urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress |
| eb46b6b6.session.saml.last.responseDestination | `https://kerbapp.superdemo.live/saml/sp/profile/post/acs` |
| eb46b6b6.session.saml.last.responseId | `<TENANT ID>` |
| eb46b6b6.session.saml.last.responseIssueInstant | `<ID>` |
| eb46b6b6.session.saml.last.responseIssuer | `https://sts.windows.net/<TENANT ID>/` |
| eb46b6b6.session.saml.last.result | 1 |
| eb46b6b6.session.saml.last.samlVersion | 2,0 |
| eb46b6b6.session.saml.last.sessionIndex | `<TENANT ID>` |
| eb46b6b6.session.saml.last.statusValue | vazo:vaha:isimler:tc:SAML:2.0:status:Başarı |
| eb46b6b6.session.saml.last.subjectConfirmDataNotOnOrOrAfter | `<ID>` |
| eb46b6b6.session.saml.last.subjectConfirmDataRecipient | `https://kerbapp.superdemo.live/saml/sp/profile/post/acs` |
| eb46b6b6.session.saml.last.subjectConfirmMethod | vazo:vaha:isimler:tc:SAML:2.0:cm:bearer |
| eb46b6b6.session.saml.last.validityNotBefore | `<ID>` |
| eb46b6b6.session.saml.last.validityNotOnOrAfter | `<ID>` |

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

- [Kerberos uygulaması için F5 tek oturum açma yı yapılandırma](kerbf5-tutorial.md)

