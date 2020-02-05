---
title: 'Öğretici: Wdesk ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ile Wdesk arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 06900a91-a326-4663-8ba6-69ae741a536e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.openlocfilehash: a4cfcf20fc7a6a3532f65c3e797da6c876844d2c
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76985576"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-wdesk"></a>Öğretici: Wdesk ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Wdesk 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Wdesk 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Wdesk 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Wdesk 'e otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Wdesk çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Wdesk **SP** ve **IDP** tarafından başlatılan SSO 'yu destekler
* Wdesk 'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve çıkartılmasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-wdesk-from-the-gallery"></a>Galeriden Wdesk ekleme

Wdesk 'in tümleştirmesini Azure AD ile yapılandırmak için, Galeriden Wdesk 'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **wdesk** yazın.
1. Sonuçlar panelinden **Wdesk** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre wdesk ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Wdesk 'teki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD SSO 'yu Wdesk ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Wdesk SSO 'Yu yapılandırma](#configure-wdesk-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. **[Wdesk test kullanıcısı oluşturun](#create-wdesk-test-user)** -bu kullanıcının Azure AD gösterimine bağlı olan wdesk 'e ait B. Simon 'a karşılık gelen bir karşılığı olmalıdır.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Wdesk ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **wdesk** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    ![Wdesk etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın: `https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`

    b. **Yanıt URL 'si** metin kutusuna şu kalıbı kullanarak bir URL yazın: `https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![Wdesk etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın: `https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Bu değerleri, SSO 'yu yapılandırırken WDesk portalından alırsınız.

4. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Wdesk 'ı ayarlama** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına brittasimon@yourcompanydomain.extensionyazın. Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Wdesk 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **wdesk**' i seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Wdesk**' i seçin.

    ![Uygulamalar listesindeki Wdesk bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-wdesk-sso"></a>Wdesk SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, Wdesk 'de güvenlik yöneticisi olarak oturum açın.

2. Sol alt tarafta **yönetici** ' ye tıklayın ve **Hesap Yöneticisi**' ni seçin:
 
     ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

3. Wdesk admin ' de, **güvenlik** **' e gidin ve SAML > ** **SAML ayarları**:

    ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

4. **Genel ayarlar**altında **SAML çoklu oturum açmayı etkinleştir**' i işaretleyin:

    ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

5. **Hizmet sağlayıcı ayrıntıları**' nın altında, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

      a. **Oturum açma URL** 'sini kopyalayın ve Azure Portal oturum açma **URL 'si** metin kutusuna yapıştırın.
   
      b. **Meta veri URL 'sini** kopyalayıp Azure Portal **tanımlayıcı** metin kutusuna yapıştırın.
       
      c. **Tüketici URL 'sini** kopyalayıp Azure Portal **Yanıtla URL** metin kutusuna yapıştırın.
   
      d. Değişiklikleri kaydetmek için Azure portal **Kaydet** ' e tıklayın.      

6. IDP ayarlarını **Düzenle** iletişim kutusunu açmak Için **IDP ayarlarını yapılandır** öğesine tıklayın. Azure portal kaydettiğiniz **meta veri. xml** dosyasını bulmak Için **Dosya Seç** ' e tıklayın, ardından karşıya yükleyin.
    
    ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
7. Tıklayın **değişiklikleri kaydetmek**.

    ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

### <a name="create-wdesk-test-user"></a>Wdesk test kullanıcısı oluşturma

Azure AD kullanıcılarının Wdesk 'de oturum açmasını sağlamak için, Wdesk 'e sağlanması gerekir. Wdesk 'de sağlama, el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Wdesk 'de güvenlik yöneticisi olarak oturum açın.

2. **Yönetici** > **Hesap Yöneticisi**' ne gidin.

     ![Çoklu oturum açmayı yapılandırın](./media/wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

3. **Kişiler**altında **Üyeler** ' e tıklayın.

4. Şimdi **üye Ekle iletişim kutusunu** açmak Için **üye Ekle** ' ye tıklayın. 
   
    ![Bir Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/createuser1.png)  

5. **Kullanıcı** metin kutusunda, brittasimon@contoso.com gibi kullanıcının Kullanıcı adını girin ve **devam** düğmesine tıklayın.

    ![Bir Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/createuser3.png)

6.  Ayrıntıları aşağıda gösterildiği gibi girin:
  
    ![Bir Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/createuser4.png)
 
    a. **E-posta** metin kutusuna brittasimon@contoso.comgibi kullanıcının e-postasını girin.

    b. **Ad** metin kutusuna, ilk Kullanıcı adını **Britta**gibi girin.

    c. **Soyadı** metin kutusuna, **Simon**gibi kullanıcı adının soyadını girin.

7. **Üye kaydet** düğmesine tıklayın.  

    ![Bir Azure AD test kullanıcısı oluşturma](./media/wdesk-tutorial/createuser5.png)

### <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Wdesk kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Wdesk 'e otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)