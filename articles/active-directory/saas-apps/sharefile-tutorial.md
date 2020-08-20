---
title: 'Öğretici: Citrix ShareFile ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Citrix ShareFile arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/18/2020
ms.author: jeedes
ms.openlocfilehash: cfbb704799a1884c689bd0de547526a33f1ba7ce
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88651930"
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Öğretici: Citrix ShareFile ile tümleştirme Azure Active Directory

Bu öğreticide Citrix ShareFile 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Citrix ShareFile 'ı Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Citrix ShareFile erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Citrix ShareFile (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini Citrix ShareFile ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Citrix ShareFile çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Citrix ShareFile **SP** tarafından başlatılan SSO 'yu destekler
* Citrix ShareFile yapılandırıldıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-citrix-sharefile-from-the-gallery"></a>Galeriden Citrix ShareFile ekleme

Citrix ShareFile 'ın Azure AD 'ye tümleştirilmesini yapılandırmak için Galeriden Citrix ShareFile öğesini yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Citrix ShareFile** yazın.
1. Sonuçlar panelinden **Citrix ShareFile** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre Citrix ShareFile ile yapılandırıp test edersiniz.
Çoklu oturum açma 'nın çalışması için, bir Azure AD kullanıcısı ve Citrix ShareFile içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açmayı Citrix ShareFile ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurmanız gerekir:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
2. **[Citrix ShareFile SSO 'Yu yapılandırma](#configure-citrix-sharefile-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    * Citrix **[ShareFile test kullanıcısı oluşturun](#create-citrix-sharefile-test-user)** ; bu, kullanıcının Azure AD gösterimine bağlı olan Citrix ShareFile 'Da Britta Simon 'a sahip olmalıdır.
3. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Citrix ShareFile** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<tenant-name>.sharefile.com/saml/login`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:

    - `https://<tenant-name>.sharefile.com`
    - `https://<tenant-name>.sharefile.com/saml/info`
    - `https://<tenant-name>.sharefile1.com/saml/info`
    - `https://<tenant-name>.sharefile1.eu/saml/info`
    - `https://<tenant-name>.sharefile.eu/saml/info`

    c. **Yanıt URL** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:
    
    - `https://<tenant-name>.sharefile.com/saml/acs`
    - `https://<tenant-name>.sharefile.eu/saml/<URL path>`
    - `https://<tenant-name>.sharefile.com/saml/<URL path>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek oturum açma URL 'SI, tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [Citrix ShareFile istemci destek ekibine](https://www.citrix.co.in/products/citrix-content-collaboration/support.html) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

4. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Citrix ShareFile ayarla** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

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

Bu bölümde, Citrix ShareFile erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Citrix ShareFile**öğesini seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-citrix-sharefile-sso"></a>Citrix ShareFile SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, **Citrix ShareFile** şirket sitenizde yönetici olarak oturum açın.

1. Sol taraftaki menüde, **Ayarlar simgesi**  ->  **güvenlik**  ->  **oturumu & güvenlik ilkesi**' ne tıklayın.
   
    ![Hesap yönetimi](./media/sharefile-tutorial/settings-security.png "Hesap yönetimi")

1. **Çoklu oturum açma/SAML 2,0 yapılandırma** Iletişim kutusunda **temel ayarlar**altında aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma](./media/sharefile-tutorial/saml-configuration.png "Çoklu oturum açma")
   
    a. **SAML etkinleştir**' de **Evet** ' i seçin.

    b. **ShareFile Issuer/ENTITY ID** değerini kopyalayın ve Azure Portal **temel SAML yapılandırması** iletişim kutusundaki **tanımlayıcı URL** kutusuna yapıştırın.
    
    c. **IDP veren/VARLıK kimliği** metin kutusunda, Azure Portal kopyaladığınız **Azure AD tanımlayıcısının** değerini yapıştırın.

    d. **X. 509.440 sertifikası** alanının yanındaki **Değiştir** ' e tıklayın ve ardından Azure Portal indirdiğiniz sertifikayı karşıya yükleyin.
    
    e. **Oturum açma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.
    
    f. **Logout URL** metin kutusuna, Azure Portal kopyaladığınız **Logout URL 'si** değerini yapıştırın.

5. Citrix ShareFile yönetim portalında **Kaydet** ' e tıklayın.

## <a name="create-citrix-sharefile-test-user"></a>Citrix ShareFile test kullanıcısı oluştur

1. **Citrix ShareFile** kiracınızda oturum açın.

2. **Kişiler**  ->  **Kullanıcıları Yönet ana**  ->  **Yeni Kullanıcı oluştur**  ->  **çalışanlar oluştur**' a tıklayın.
   
    ![Çalışan oluştur](./media/sharefile-tutorial/create-user.png "Çalışan oluştur")

3. **Temel bilgiler** bölümünde aşağıdaki adımları uygulayın:
   
    ![Temel bilgiler](./media/sharefile-tutorial/user-form.png "Temel bilgiler")
   
    a. **Ad** metin kutusuna **Ilk Kullanıcı adını** **Britta**olarak yazın.
   
    b.  **Soyadı** metin kutusunda, Kullanıcı **adını** **Simon**olarak yazın.
   
    c. **E-posta adresi** metin kutusuna Britta Simon 'un e-posta adresini **brittasıon \@ contoso.com**olarak yazın.

4. **Kullanıcı Ekle**'ye tıklayın.
  
    >[!NOTE]
    >Azure AD hesap sahibi bir e-posta alır ve etkin hale gelmeden önce hesaplarını onaylamak için bir bağlantıyı izler. Azure AD Kullanıcı hesapları sağlamak için Citrix ShareFile tarafından sunulan diğer Citrix ShareFile Kullanıcı hesabı oluşturma araçlarını veya API 'Leri kullanabilirsiniz.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Citrix ShareFile kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Citrix ShareFile dosyasında otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

