---
title: 'Öğretici: Yakınlaştırma ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve yakınlaştırma arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0ebdab6c-83a8-4737-a86a-974f37269c31
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/08/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8e36d1bb91e70e21ee1940e189bfedaebafa4412
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68975936"
---
# <a name="tutorial-integrate-zoom-with-azure-active-directory"></a>Öğretici: Yakınlaştırmayı Azure Active Directory tümleştirin

Bu öğreticide, yakınlaştırmayı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Yakınlaştırmayı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de yakınlaştırma erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla yakınlaştırmak için otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Çoklu oturum açma (SSO) özellikli aboneliği yakınlaştır.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Yakınlaştırma, **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-zoom-from-the-gallery"></a>Galeriden yakınlaştırma ekleme

Yakınlaştırma tümleştirmesini Azure AD 'ye göre yapılandırmak için Galeri 'den yönetilen SaaS uygulamaları listenize yakınlaştırma eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Yakınlaştır** yazın.
1. Sonuçlar panelinden **Yakınlaştır** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-zoom"></a>Azure AD çoklu oturum açmayı yakınlaştırma için yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu yakınlaştırarak yapılandırın ve test edin. SSO 'nun çalışması için, yakınlaştırılmış bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO 'yu yakınlaştırma ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
2. **[YAKıNLAŞTıR SSO 'Yu yapılandırma](#configure-zoom-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. Yakınlaştırma **[testi kullanıcısı oluşturma](#create-zoom-test-user)** -kullanıcının Azure AD gösterimine bağlı olarak yakınlaştırılmış B. Simon 'a sahip olmak için.
3. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), Uygulama tümleştirmesini **Yakınlaştır** sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<companyname>.zoom.us`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`<companyname>.zoom.us`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için [Yakınlaştırma istemci desteği ekibine](https://support.zoom.us/hc/en-us) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. Yakınlaştırma uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekliyor. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.  **Kullanıcı öznitelikleri** iletişim kutusunu açmak için **Düzenle**simgesine tıklayın.

    ![image](common/edit-attribute.png)

6. Yukarıdaki uygulamanın yanı sıra, yakınlaştırma uygulaması daha fazla özniteliğin SAML yanıtına geri geçirilmesini bekler.   **Kullanıcı öznitelikleri** **** iletişimkutusundakiKullanıcıtalepleribölümündeaşağıdakitablodagösterildiğigibiSAMLbelirteciözniteliğieklemekiçinaşağıdakiadımları gerçekleştirin: 

    | Ad | Ad Alanı  |  Kaynak özniteliği|
    | ---------------| --------------- | --------- |
    | E-posta adresi  | Kullanıcı. Mail  | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/mail` |
    | Ad  | Kullanıcı.  | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` |
    | Soyadı  | User. soyadı  | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` |
    | Telefon numarası  | Kullanıcı. telephoneNumber  | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/phone` |
    | Bölüm  | User. Departmanı  | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/department` |
    | role |    Kullanıcı. atanan |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/role` |

    > [!NOTE]
    > Azure AD 'de rolün nasıl yapılandırılacağını öğrenmek için lütfen [buraya](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management) tıklayın

    a. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' ye tıklayın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını yazın.

    c. **Öznitelik**olarak kaynak seçin.

    d. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini yazın.

    e. Tıklayın **Tamam**

    f. **Kaydet**’e tıklayın.

    > [!NOTE]
    > Yakınlaştırma, SAML yükünde grup talebini bekleyebilir, bu nedenle herhangi bir grup oluşturduysanız, bu grup bilgilerini onların sonunda de yapılandırabilmeleri için lütfen grup bilgilerini kullanarak [istemci desteği ekibine yakınlaştırın](https://support.zoom.us/hc/en-us) . Ayrıca, [istemci destek ekibine yakınlaştırmak](https://support.zoom.us/hc/en-us) IÇIN nesne kimliğini sağlamanız gerekir; böylece, kendi sonunda yapılandırılabilirler. Nesne KIMLIĞINI almak için lütfen [belgeyi](https://support.zoom.us/hc/en-us/articles/115005887566) izleyin.

4. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Yakınlaştırmayı ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
    1. **Ad** alanına `B.Simon` girin.  
    1. **Kullanıcı adı** alanına, username@companydomain.extensiongirin. Örneğin: `B.Simon@contoso.com`.
    1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
    1. **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, yakınlaştırma erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Yakınlaştır**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-zoom-sso"></a>Yakınlaştırma SSO 'yu Yapılandır

1. Farklı bir Web tarayıcısı penceresinde, yakınlaştırma şirket sitenizde yönetici olarak oturum açın.

2. **Çoklu oturum açma** sekmesine tıklayın.

    ![Çoklu oturum açma sekmesi](./media/zoom-tutorial/ic784700.png "Çoklu oturum açma")

3. **Güvenlik denetimi** sekmesine tıklayın ve ardından **Çoklu oturum açma** ayarlarına gidin.

4. Çoklu oturum açma bölümünde aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açma bölümü](./media/zoom-tutorial/ic784701.png "Çoklu oturum açma")

    a. **Oturum açma sayfası URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    b. **Oturum kapatma sayfası URL 'si** değeri için Azure Portal gitmeniz ve sol tarafta bulunan **Azure Active Directory** tıklamanıza ve sonra **uygulama kayıtları**' ne gitmeniz gerekir.

    ![Azure Active Directory düğmesi](./media/zoom-tutorial/appreg.png)

    c. **Uç noktalara** tıklayın

    ![Uç nokta düğmesi](./media/zoom-tutorial/endpoint.png)

    d. **SAML-P oturum kapatma uç noktasını** kopyalayın ve **oturum kapatma sayfası URL 'si** metin kutusuna yapıştırın.

    ![Son noktayı Kopyala düğmesi](./media/zoom-tutorial/endpoint1.png)

    e. Base-64 kodlu sertifikanızı Not defteri 'nde açın, içeriğini panonuza kopyalayın ve ardından **kimlik sağlayıcısı sertifikası** metin kutusuna yapıştırın.

    f. **Veren** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcısının** değerini yapıştırın. 

    g. **Kaydet**’e tıklayın.

    > [!NOTE]
    > Daha fazla bilgi için yakınlaştırma belgelerini ziyaret edin[https://zoomus.zendesk.com/hc/articles/115005887566](https://zoomus.zendesk.com/hc/articles/115005887566)

### <a name="create-zoom-test-user"></a>Yakınlaştırma testi kullanıcısı oluştur

Azure AD kullanıcılarının yakınlaştırmak için oturum açmasını etkinleştirmek üzere, yakınlaştırmaları gerekir. Yakınlaştırma durumunda sağlama, el ile gerçekleştirilen bir görevdir.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. **Yakınlaştırma** şirket sitenizde yönetici olarak oturum açın.

2. **Hesap yönetimi** sekmesine tıklayın ve ardından **Kullanıcı yönetimi**' ne tıklayın.

3. Kullanıcı Yönetimi bölümünde, Kullanıcı **Ekle**' ye tıklayın.

    ![Kullanıcı yönetimi](./media/zoom-tutorial/ic784703.png "Kullanıcı yönetimi")

4. **Kullanıcı Ekle** sayfasında, aşağıdaki adımları uygulayın:

    ![Kullanıcı Ekle](./media/zoom-tutorial/ic784704.png "Kullanıcı Ekle")

    a. **Kullanıcı türü**olarak **temel**' yı seçin.

    b. **E-postalar** metin kutusuna, sağlamak istediğiniz geçerli BIR Azure AD hesabının e-posta adresini yazın.

    c. **Ekle**'yi tıklatın.

> [!NOTE]
> Azure Active Directory Kullanıcı hesapları sağlamak için yakınlaştırma tarafından sunulan diğer tüm Kullanıcı hesabı oluşturma araçlarını veya API 'Leri kullanabilirsiniz.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde yakınlaştırma kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız yakınlaştırmadan otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

