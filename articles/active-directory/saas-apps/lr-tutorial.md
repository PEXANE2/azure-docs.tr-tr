---
title: 'Öğretici: LoginRadius ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ile LoginRadius arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.openlocfilehash: 1376dcb76c22bcd70937f533d337ee9679e9dc59
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96455837"
---
# <a name="tutorial-azure-active-directory-integration-with-loginradius"></a>Öğretici: LoginRadius ile Azure Active Directory tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile LoginRadius tümleştirmeyi öğreneceksiniz.

LoginRadius 'yi Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de, LoginRadius erişimi olan ' i denetleyebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla otomatik olarak LoginRadius (çoklu oturum açma) oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini LoginRadius ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Bir LoginRadius çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* LoginRadius **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-loginradius-from-the-gallery"></a>Galeriden LoginRadius ekleme

Azure AD 'de LoginRadius tümleştirmesini yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize LoginRadius eklemeniz gerekir.

**Galeriden LoginRadius eklemek için:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesini seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve ardından **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için **Yeni uygulama** düğmesini seçin:

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **loginradius** yazın, sonuç panelinde **loginradius** ' ı seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini seçin.

    ![Sonuç listesinde LoginRadius](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon** adlı bir test kullanıcısına göre loginradius ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve LoginRadius ile ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

Azure AD çoklu oturum açma 'yı LoginRadius ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında tek Sign-On ayarlarını yapılandırmak için **[LoginRadius çoklu oturum açmayı yapılandırın](#configure-loginradius-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcının Azure AD gösterimine bağlı olan LoginRadius 'de Britta Simon 'ın bir karşılığı olacak şekilde **[loginradius test kullanıcısı oluşturun](#create-loginradius-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı LoginRadius ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **loginradius** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seçin** bölmesinde, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** bölmesini açmak için **Düzenle** simgesini seçin.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde:

   ![LoginRadius etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier.png)

   1. **Oturum açma URL 'si** metin kutusunda URL 'yi girin`https://secure.loginradius.com/login`

   1. **Tanımlayıcı (VARLıK kimliği)** metin kutusuna URL 'yi girin`https://lr.hub.loginradius.com/`

   1. **Yanıt URL 'si (onaylama tüketici hizmeti URL 'si)** metin kutusunda, LOGINRADIUS ACS URL 'sini girin`https://lr.hub.loginradius.com/saml/serviceprovider/AdfsACS.aspx` 

5. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **LoginRadius ayarla** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

   ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

   - Oturum Açma URL’si

   - Azure AD tanımlayıcısı

   - Oturum kapatma URL 'SI

## <a name="configure-loginradius-single-sign-on"></a>LoginRadius tek Sign-On yapılandırma

Bu bölümde, LoginRadius yönetici konsolunda Azure AD çoklu oturum açma 'yı etkinleştirirsiniz.

1. LoginRadius [Yönetici Konsolu](https://adminconsole.loginradius.com/login) hesabınızda oturum açın.

2. [Loginradius yönetim konsolundaki](https://secure.loginradius.com/account/team) **Takım Yönetimi** bölümüne gidin.

3. **Çoklu oturum açma** sekmesini seçin ve ardından **Azure AD**' yi seçin:

   ![LoginRadius ekip yönetimi konsolundaki çoklu oturum açma menüsünü gösteren ekran görüntüsü](./media/loginradius-tutorial/azure-ad.png)
4. Azure AD Kurulumu sayfasında, aşağıdaki adımları izleyin:

   ![LoginRadius Ekip Yönetimi konsolunda Azure Active Directory yapılandırmayı gösteren ekran görüntüsü](./media/loginradius-tutorial/single-sign-on.png)

    1. **Kimlik sağlayıcısı konumunda**, Azure AD hesabınızdan aldığınız oturum açma uç noktasını girin.

    1. **Kimlik sağlayıcısı oturum kapatma URL 'si**' nde, Azure AD HESABıNıZDAN aldığınız oturum kapatma uç noktasını girin.
 
    1. **Kimlik sağlayıcısı sertifikası**' nda Azure AD hesabınızı ALDıĞıNıZ Azure AD sertifikasını girin. Üst bilgi ve alt bilgiyle sertifika değeri girin. Örnek: `-----BEGIN CERTIFICATE-----<certifciate value>-----END CERTIFICATE-----`

    1. **Hizmet sağlayıcı sertifikası** ve **sunucu sağlayıcısı sertifika anahtarı**' nda, sertifikanızı ve anahtarınızı girin. 

       Komut satırında (Linux/Mac) aşağıdaki komutları çalıştırarak kendinden imzalı bir sertifika oluşturabilirsiniz:

       - SP için sertifika anahtarını almak için komutu: `openssl genrsa -out lr.hub.loginradius.com.key 2048`

       - SP için sertifikayı almak için komutu: `openssl req -new -x509 -key lr.hub.loginradius.com.key -out lr.hub.loginradius.com.cert -days 3650 -subj /CN=lr.hub.loginradius.com`
     
       > [!NOTE]
       > Üst bilgi ve alt bilgiyle sertifika ve sertifika anahtarı değerlerini girdiğinizden emin olun:
       > - Sertifika değeri örnek biçimi: `-----BEGIN CERTIFICATE-----<certifciate value>-----END CERTIFICATE-----`
       > - Sertifika anahtar değeri örnek biçimi: `-----BEGIN RSA PRIVATE KEY-----<certifciate key value>-----END RSA PRIVATE KEY-----`

5. **Veri eşleme** bölümünde alanlar ' ı (SP alanları) seçin ve karşılık gelen Azure ad alanlarını (IDP alanları) girin.

    Aşağıda, Azure AD 'de listelenen bazı alan adları verilmiştir.

    | Alanlar    | Profil anahtarı                                                          |
    | --------- | -------------------------------------------------------------------- |
    | E-posta     | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` |
    | FirstName | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`    |
    | LastName  | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`      |

    > [!NOTE]
    > **E-posta** alanı eşlemesi gereklidir. **FirstName** ve **LastName** alan eşlemeleri isteğe bağlıdır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. **Kullanıcı** özellikleri ' nde aşağıdaki adımları gerçekleştirin.

   ![Kullanıcı iletişim kutusu](common/user-properties.png)

   1. **Ad** alanına **Brittasıon** yazın.
  
   1. **Kullanıcı adı** alanına, girin `brittasimon@yourcompanydomain.extension` . Örneğin, BrittaSimon@contoso.com.

   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.

   1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Azure çoklu oturum açma özelliğini kullanarak LoginRadius 'e erişim izni vererek Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **loginradius**' i seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Loginradius**' ı seçin.

    ![Uygulamalar listesindeki LoginRadius bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesini seçin, sonra **atama Ekle** bölmesinde **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** bölmesinde, **Kullanıcılar** listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesini seçin.

6. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** bölmesinde, Kullanıcı için listeden uygun rolü seçin. Ardından, ekranın alt kısmındaki **Seç** düğmesini seçin.

7. **Atama Ekle** bölmesinde **ata** düğmesini seçin.

### <a name="create-loginradius-test-user"></a>LoginRadius test kullanıcısı oluştur

1. LoginRadius [Yönetici Konsolu](https://adminconsole.loginradius.com/login) hesabınızda oturum açın.

2. LoginRadius yönetim konsolundaki takım yönetimi bölümüne gidin.

   ![LoginRadius Yönetici Konsolu 'Nu gösteren ekran görüntüsü](./media/loginradius-tutorial/team-management.png)
3. Formu açmak için yan menüdeki **Takım üyesi ekle** ' yi seçin. 

4. **Takım üyesi ekle** formunda, kullanıcının ayrıntılarını sağlayarak ve kullanıcının sahip olmasını istediğiniz izinleri atayarak, loginradius sitenizde Britta Simon adlı bir Kullanıcı oluşturursunuz. Rollere dayalı izinler hakkında daha fazla bilgi edinmek için, LoginRadius [Yönetim üyeleri](https://www.loginradius.com/docs/api/v2/admin-console/team-management/manage-team-members#roleaccesspermissions0) belgesinin [rol erişim izinleri](https://www.loginradius.com/docs/api/v2/admin-console/team-management/manage-team-members#roleaccesspermissions0) bölümüne bakın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

1. Bir tarayıcıda öğesine gidin https://accounts.loginradius.com/auth.aspx ve **beslenir oturum açma '** yı seçin.
2. LoginRadius uygulamanızın adını girin ve ardından **oturum aç**' ı seçin.
3. Azure AD hesabınızda oturum açmanızı isteyen bir açılır pencere açması gerekir.
4. Kimlik doğrulamasından sonra, açılır pencere kapatılacak ve LoginRadius yönetici konsolunda oturum açılır.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)
