---
title: 'Öğretici: Veracode ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ile Veracode arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 4fe78050-cb6d-4db9-96ec-58cc0779167f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/05/2019
ms.author: jeedes
ms.openlocfilehash: 67b4d4842bfa330e51244cee0d9ad8ad0ab6a24f
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68825968"
---
# <a name="tutorial-azure-active-directory-integration-with-veracode"></a>Öğretici: Veracode ile tümleştirme Azure Active Directory

Bu öğreticide, Veracode 'u Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Veracode 'u Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de, Veracode erişimi olan denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Veracode (çoklu oturum açma) için otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini Veracode ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Veracode çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Veracode, **IDP** tarafından başlatılan SSO 'yu destekler

* Veracode **, tam zamanında** Kullanıcı sağlamayı destekler

## <a name="adding-veracode-from-the-gallery"></a>Galeriden Veracode ekleme

Veracode 'un Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize Veracode eklemeniz gerekir.

**Galeriden Veracode eklemek için aşağıdaki adımları uygulayın:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Veracode**yazın, sonuç panelinden **Veracode** ' u seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

     ![Sonuçlar listesinde Veracode](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre Veracode ile yapılandırıp test edersiniz.
Çoklu oturum açma 'nın çalışması için, bir Azure AD kullanıcısı ile ilgili Kullanıcı arasındaki bir bağlantı ilişkisinin oluşturulması gerekir.

Azure AD çoklu oturum açma 'yı Veracode ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Veracode çoklu oturum açmayı yapılandırın](#configure-veracode-single-sign-on)** .
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. Kullanıcının Azure AD gösterimine bağlı bir Veracode 'da Britta Simon 'a sahip olmak için **[Veracode test kullanıcısı oluşturun](#create-veracode-test-user)** .
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Veracode ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Veracode** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulama zaten Azure ile önceden tümleştirildiği için kullanıcının herhangi bir adım yapması gerekmez.

    ![Veracode etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/preintegrated.png)

5. Veracode uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir. **Kullanıcı öznitelikleri** iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![image](common/edit-attribute.png)

6. Veracode uygulaması, yukarıdakine ek olarak SAML yanıtına daha fazla özniteliğin geri geçirilmesini bekler. **Kullanıcı öznitelikleri** Iletişim kutusundaki **Kullanıcı talepleri** bölümünde AŞAĞıDAKI tabloda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin:

    | Ad | Kaynak özniteliği|
    | ---------------| --------------- |
    | FirstName |Kullanıcı. |
    | Soyadı |User. soyadı |
    | email |Kullanıcı. Mail |

    a. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' ye tıklayın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını yazın.

    c. **Ad alanını** boş bırakın.

    d. **Öznitelik**olarak kaynak seçin.

    e. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

7. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

8. Ayarlama **Veracode** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-veracode-single-sign-on"></a>Veracode çoklu oturum açmayı yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, bir yönetici olarak Veracode şirket sitenizde oturum açın.

2. Üstteki menüde **Ayarlar**' a ve ardından **yönetici**' ye tıklayın.
   
    ![Yönetim](./media/veracode-tutorial/ic802911.png "Yönetim")

3. **SAML** sekmesine tıklayın.

4. **Kuruluş SAML ayarları** bölümünde aşağıdaki adımları uygulayın:
   
    ![Yönetim](./media/veracode-tutorial/ic802912.png "Yönetim")
   
    a.  **Veren** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcısının** değerini yapıştırın.
    
    b. İndirilen sertifikanızı Azure portal yüklemek için **Dosya Seç**' e tıklayın.
   
    c. **Kendini kaydolmayı etkinleştir**' i seçin.

1. **Kendi kendine kayıt ayarları** bölümünde aşağıdaki adımları gerçekleştirin ve ardından **Kaydet**' e tıklayın:
   
    ![Yönetim](./media/veracode-tutorial/ic802913.png "Yönetim")
   
    a. **Yeni Kullanıcı etkinleştirmesi**olarak **etkinleştirme gerekli değil**' i seçin.
   
    b. **Kullanıcı veri güncelleştirmeleri**olarak **tercih Veracode Kullanıcı verileri**' ni seçin.
   
    c. **SAML öznitelik ayrıntıları**için aşağıdakileri seçin:
      * **Kullanıcı rolleri**
      * **İlke Yöneticisi**
      * **Geçirenlerin**
      * **Güvenlik lideri**
      * **İdari**
      * **Gönderenin**
      * **Oluşturucu**
      * **Tüm tarama türleri**
      * **Takım üyelikleri**
      * **Varsayılan takım**

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına yazın brittasimon@yourcompanydomain.extension. Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d.           **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Veracode 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **Veracode**' u seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Veracode**' u seçin.

    ![Uygulamalar listesindeki Veracode bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-veracode-test-user"></a>Veracode test kullanıcısı oluşturma

Azure AD kullanıcılarının Veracode 'da oturum açmasını sağlamak için bunların Veracode içinde sağlanması gerekir. Veracode durumunda, sağlama otomatik bir görevdir. Sizin için herhangi bir eylem öğesi yok. İlk çoklu oturum açma denemesi sırasında gerekirse kullanıcılar otomatik olarak oluşturulur.

> [!NOTE]
> Azure AD Kullanıcı hesapları sağlamak için Veracode tarafından sunulan diğer herhangi bir diğer Veracode Kullanıcı hesabı oluşturma aracını veya API 'Leri kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Veracode kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Veracode 'da otomatik olarak oturum açmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

