---
title: 'Öğretici: Work.com ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Work.com arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e7a6dc16eef1bb36a5bd6cbf0502a83481230bc0
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67087089"
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a>Öğretici: Work.com ile tümleştirme Azure Active Directory

Bu öğreticide, Work.com 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Work.com Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Work.com 'e erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Work.com (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Work.com ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Work.com çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Work.com **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-workcom-from-the-gallery"></a>Galeriden Work.com ekleme

Work.com tümleştirmesini Azure AD 'ye göre yapılandırmak için, Galeriden Work.com yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Work.com eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Work.com**yazın, sonuç panelinden **Work.com** ' yi seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

    ![Sonuç listesinde Work.com](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre Work.com ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Work.com 'deki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açmayı Work.com ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Work.com çoklu oturum açmayı yapılandırın](#configure-workcom-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcının Azure AD gösterimine bağlı olan Work.com 'de Britta Simon 'ın bir karşılığı olacak şekilde **[Work.com test kullanıcısı oluşturun](#create-workcom-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

>[!NOTE]
>Çoklu oturum açmayı yapılandırmak için, henüz özel bir Work.com etki alanı adı belirlemeniz gerekir. En az bir etki alanı adı tanımlamanız, etki alanı adınızı test etmeniz ve tüm kuruluşunuza dağıtmanız gerekir.

Azure AD çoklu oturum açmayı Work.com ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Work.com** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Work.com etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-signonurl.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`http://<companyname>.my.salesforce.com`

    > [!NOTE]
    > Değer gerçek değil. Değeri, gerçek oturum açma URL 'SI ile güncelleştirin. Değeri almak için [Work.com istemci destek ekibine](https://help.salesforce.com/articleView?id=000159855&type=3) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Work.com ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-workcom-single-sign-on"></a>Work.com çoklu oturum açmayı yapılandırma

1. Work.com kiracınızda yönetici olarak oturum açın.

2. **Kurulum**'a gidin.
   
    ![Kurulum](./media/work-com-tutorial/ic794108.png "Kurulum")

3. Sol gezinti bölmesindeki **Yönet** bölümünde, **etki alanı yönetimi** ' ne tıklayarak ilgili bölümü genişletin ve **sonra etki alanım ' a** tıklayarak etki **alanım** sayfasını açın. 
   
    ![Etki alanım](./media/work-com-tutorial/ic767825.png "Etki alanım")

4. Etki alanınızı doğru şekilde ayarlandığını doğrulamak için, bu, "**kullanıcılara dağıtılan 4. adım**" içinde olduğundan emin olun ve "**etki alanı**ayarlarımı" gözden geçirin.
   
    ![Kullanıcıya dağıtılan etki alanı](./media/work-com-tutorial/ic784377.png "Kullanıcıya dağıtılan etki alanı")

5. Work.com kiracınızda oturum açın.

6. **Kurulum**'a gidin.
    
    ![Kurulum](./media/work-com-tutorial/ic794108.png "Kurulum")

7. **Güvenlik denetimleri** menüsünü genişletin ve **Çoklu oturum açma ayarları**' na tıklayın.
    
    ![Çoklu oturum açma ayarları](./media/work-com-tutorial/ic794113.png "Çoklu oturum açma ayarları")

8. **Çoklu oturum açma ayarları** iletişim sayfasında, aşağıdaki adımları uygulayın:
    
    ![SAML etkin](./media/work-com-tutorial/ic781026.png "SAML etkin")
    
    a. **SAML etkin**' i seçin.
    
    b. **Yeni**' ye tıklayın.

9. **SAML çoklu oturum açma ayarları** bölümünde aşağıdaki adımları uygulayın:
    
    ![SAML çoklu oturum açma ayarı](./media/work-com-tutorial/ic794114.png "SAML çoklu oturum açma ayarı")
    
    a. **Ad** metin kutusuna yapılandırmanız için bir ad yazın.  
       
    > [!NOTE]
    > **Ad** için bir değer sağlanması, **API adının** metin kutusunu otomatik olarak doldurur.
    
    b. **Veren** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcısının** değerini yapıştırın.
    
    c. İndirilen sertifikayı Azure portal yüklemek için, **Araştır**' a tıklayın.
    
    d. **Varlık kimliği** metin kutusuna yazın `https://salesforce-work.com`.
    
    e. **SAML kimlik türü**olarak, onaylama ' yı seçtiğinizde **Kullanıcı nesnesinden federasyon kimliği bulunur**.
    
    f. **SAML kimlik konumu**olarak, Select **Identity öğesi Subject ifadesinin NameIdentfier öğesidir**.
    
    g. **Kimlik sağlayıcısı oturum açma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    h. **Kimlik sağlayıcısı oturum kapatma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **Logout URL 'si** değerini yapıştırın.
    
    i. **Hizmet sağlayıcısı Istek bağlamayı başlattığı**Için **http post**' ı seçin.
    
    j. **Kaydet**’e tıklayın.

10. Work.com Klasik portalda, sol gezinti bölmesinde, **etki alanı yönetimi** ' ne tıklayarak ilgili bölümü genişletin **ve etki alanım ' a** tıklayarak etki **alanım** sayfasını açın. 
    
    ![Etki alanım](./media/work-com-tutorial/ic794115.png "Etki alanım")

11. **Etki alanım** sayfasında, **oturum açma sayfası markalama** bölümünde **Düzenle**' ye tıklayın.
    
    ![Oturum açma sayfası markası](./media/work-com-tutorial/ic767826.png "Oturum açma sayfası markası")

12. **Oturum açma sayfası markalama** sayfasında, **kimlik doğrulama HIZMETI** bölümünde **SAML SSO ayarlarınızın** adı görüntülenir. Seçin ve ardından **Kaydet**' e tıklayın.
    
    ![Oturum açma sayfası markası](./media/work-com-tutorial/ic784366.png "Oturum açma sayfası markası")

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına yazın `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Work.com 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon özelliğini etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **Work.com**' yi seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Work.com**' yi seçin.

    ![Uygulamalar listesindeki Work.com bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-workcom-test-user"></a>Work.com test kullanıcısı oluştur

Azure Active Directory kullanıcıların oturum açabilmeleri için, Work.com ' a sağlanması gerekir. Work.com durumunda sağlama, el ile gerçekleştirilen bir görevdir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları uygulayın:

1. Work.com Şirket sitenizde yönetici olarak oturum açın.

2. **Kurulum**'a gidin.
   
    ![Kurulum](./media/work-com-tutorial/IC794108.png "Kurulum")

3. **Kullanıcıları \> Yönet**' e gidin.
   
    ![Kullanıcıları yönetme](./media/work-com-tutorial/IC784369.png "Kullanıcıları Yönet")

4. **Yeni Kullanıcı**' ya tıklayın.
   
    ![Tüm Kullanıcılar](./media/work-com-tutorial/IC794117.png "Tüm Kullanıcılar")

5. Kullanıcı düzenleme bölümünde, ilgili metin kutularına sağlamak istediğiniz geçerli bir Azure AD hesabının özniteliklerinde aşağıdaki adımları gerçekleştirin:
   
    ![Kullanıcı düzenleme](./media/work-com-tutorial/ic794118.png "Kullanıcı düzenleme")
   
    a. **Ilk ad** metin kutusuna **, kullanıcının** **ilk adını** yazın.
    
    b. **Soyadı** metin kutusuna kullanıcı **Simon**' un **soyadını** yazın.
    
    c. **Diğer ad** metin **kutusuna kullanıcının** **adını** yazın.
    
    d. **E-posta** metin kutusuna kullanıcının Brittasimon@contoso.com **e-posta adresini** yazın.
    
    e. **Kullanıcı adı** metin kutusuna, gibi Brittasimon@contoso.combir Kullanıcı adı yazın.
    
    f. **Nick adı** metin kutusunda, Kullanıcı **Simon**için bir **Nick adı** yazın.
    
    g. **Rol**, **Kullanıcı Lisansı**ve **profil**' i seçin.
    
    h. **Kaydet**’e tıklayın.  
      
    > [!NOTE]
    > Azure AD hesap tutucusu, hesabı etkin olmadan önce onaylamaya yönelik bir bağlantı içeren bir e-posta alır.
    > 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Work.com kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Work.com için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

