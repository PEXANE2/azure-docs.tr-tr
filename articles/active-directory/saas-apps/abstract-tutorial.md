---
title: 'Öğretici: Azure Active Directory Tümleştirmesi ile soyut | Microsoft Docs'
description: Azure Active Directory hem de Abstract arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2eb167ab-d769-4661-a8cb-ae371cb63d2a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b8038784c9da30a42541688536169576cd9a92e9
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68235014"
---
# <a name="tutorial-integrate-abstract-with-azure-active-directory"></a>Öğretici: Soyut Azure Active Directory ile tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile soyut tümleştirme öğreneceksiniz. Soyut Azure AD ile tümleştirdiğinizde, şunları yapabilirsiniz:

* Soyut erişimi, Azure AD'de denetler.
* Otomatik olarak soyut için kendi Azure AD hesapları ile oturum açmış olmasını sağlayın.
* Bir merkezi konumda - Azure portalı hesaplarınızı yönetin.

Azure AD SaaS uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gerekir:

* Azure AD aboneliğiniz. Bir aboneliğiniz yoksa, alabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).
* Soyut çoklu oturum açma (SSO) abonelik etkin.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, yapılandırma ve Azure AD SSO bir test ortamında test edin.

* Soyut destekler **SP ve IDP** tarafından başlatılan

## <a name="adding-abstract-from-the-gallery"></a>Soyut galeri ekleme

Azure AD'de soyut tümleştirmesini yapılandırmak için soyut Galeriden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmeti.
1. Gidin **kurumsal uygulamalar** seçip **tüm uygulamaları**.
1. Yeni bir uygulama eklemek için seçin **yeni uygulama**.
1. İçinde **Galeriden Ekle** bölümüne şunu yazın **soyut** arama kutusuna.
1. Seçin **soyut** gelen sonuçlar panelinde ve uygulama ekleyin. Uygulama, kiracınıza eklendiği sırada birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Yapılandırma ve Azure AD SSO ile soyut olarak adlandırılan bir test kullanıcısı kullanarak test etme **B.Simon**. Çalışmak SSO için soyut bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişki oluşturmanız gerekir.

Yapılandırma ve Azure AD SSO ile soyut sınamak için aşağıdaki yapı taşlarını tamamlayın:

1. **[Azure AD SSO'yu yapılandırma](#configure-azure-ad-sso)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[Soyut SSO'yu yapılandırarak](#configure-abstract-sso)**  - uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Soyut bir test kullanıcısı oluşturma](#create-abstract-test-user)**  - kullanıcı Azure AD gösterimini bağlı soyut bir karşılığı Britta simon'un sağlamak için.
6. **[Test SSO](#test-sso)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO'yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek üzere aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com/), **soyut** uygulama tümleştirme sayfası, bulma **Yönet** bölümünde ve seçin **çoklu oturum açma**.
1. Üzerinde **tek bir oturum açma yönteminizi seçmeniz** sayfasında **SAML**.
1. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında, düzenleme/kalem simgesine tıklayıp **temel SAML yapılandırma** ayarlarını düzenlemek için.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Üzerinde **temel SAML yapılandırma** uygulama, önceden yapılandırılmış bölümü **IDP** başlatılan modu ve gerekli URL'ler zaten önceden doldurulur Azure ile. Tıklayarak yapılandırmayı kaydetmek kullanıcının erişmesi **Kaydet** düğmesi.

1. Tıklayın **ek URL'lerini ayarlayın** ve uygulamada yapılandırmak istiyorsanız, aşağıdaki adımı uygulayın **SP** başlatılan modu:

    İçinde **oturum açma URL'si** metin kutusuna bir URL yazın:  `https://app.abstract.com/signin`

4. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlayın** sayfasında **SAML imzalama sertifikası** bölümünde, kopyalamak için Kopyala düğmesine **uygulama Federasyon meta verileri URL'sini** ve üzerinde kaydedin, bilgisayar.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-abstract-sso"></a>Soyut SSO yapılandırma

Aldığınızdan emin olun, `App Federation Metadata Url` ve `Azure AD Identifier` Azure portalından gereksinim duyacağınız Özet üzerinde SSO yapılandırmak için bu.

Bu bilgiler üzerinde bulabilirsiniz **yukarı çoklu oturum açma SAML ile ayarlayın** sayfası:

* `App Federation Metadata Url` Bulunan **SAML imzalama sertifikası** bölümü.
* `Azure AD Identifier` Bulunan **soyut kümesi** bölümü.


Artık üzerinde soyut SSO yapılandırmaya hazırsınız:

>[!Note]
>Bir kuruluş soyut SSO ayarlarını erişmek için yönetici hesabı ile kimlik doğrulaması yapmanız gerekir.

1. Açık [soyut web uygulaması](https://app.abstract.com/).
2. Git **izinleri** sayfasında sol kenar çubuğu.
3. İçinde **yapılandırma SSO** bölümünde, girin, **meta veri URL'si** ve **varlık kimliği**.
4. Sahip olabileceğiniz durumların el ile girin. E-postaları el ile özel durumları bölümünde listelenen SSO atlamak ve e-posta ve parolanızla oturum açmaya devam edebilir. 
5. Tıklayın **değişiklikleri kaydetmek**.

>[!Note] 
>Birincil e-posta adreslerini el ile özel durumlar listesinde kullanmanız gerekir. İkincil bir kullanıcının e-posta, liste e-posta olup olmadığını SSO etkinleştirme başarısız olur. Bu durumda, başarısız olan hesabı için birincil e-posta ile bir hata iletisi görürsünüz. Kullanıcı bildiğiniz doğruladıktan sonra birincil e-posta el ile özel durumlar listesine ekleyin.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, bir test kullanıcısı B.Simon adlı Azure portalında oluşturacaksınız.

1. Azure Portalı'ndaki sol bölmeden seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. İçinde **kullanıcı** özellikleri, aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. İçinde **kullanıcı adı** alanına username@companydomain.extension. Örneğin: `B.Simon@contoso.com`.
   1. Seçin **Show parola** onay kutusunu işaretleyin ve ardından görüntülenen değeri yazın **parola** kutusu.
   1.           **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma kullanmak için soyut erişim vererek B.Simon tıklatmalarını sağlarsınız.

1. Azure portalında **kurumsal uygulamalar**ve ardından **tüm uygulamaları**.
1. Uygulamalar listesinde **soyut**.
1. Uygulamanın genel bakış sayfasında bulma **Yönet** seçin ve bölüm **kullanıcılar ve gruplar**.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. Seçin **Kullanıcı Ekle**, ardından **kullanıcılar ve gruplar** içinde **atama Ekle** iletişim.

    ![Kullanıcı ekleme bağlantısı](common/add-assign-user.png)

1. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **B.Simon** kullanıcılar listesinden ardından **seçin** ekranın alt kısmındaki düğmesi.
1. SAML onaylama işlemi herhangi bir rolü değer de beklediğiniz varsa **rolü Seç** iletişim kutusunda, listeden bir kullanıcı için uygun rolü seçin ve ardından **seçin** ekranın alt kısmındaki düğmesi.
1. İçinde **atama Ekle** iletişim kutusunda, tıklayın **atama** düğmesi.

### <a name="create-abstract-test-user"></a>Soyut bir test kullanıcısı oluşturma

SSO soyut üzerinde test etmek için:

1. Açık [soyut web uygulaması](https://app.abstract.com/).
2. Git **izinleri** sayfasında sol kenar çubuğu.
3. Tıklayın **Hesabımı ile Test**. Test başarısız, lütfen olursa [destek ekibimize başvurun](https://www.abstract.com/help/contact/).

>[!Note]
>Bir kuruluş soyut SSO ayarlarını erişmek için yönetici hesabı ile kimlik doğrulaması yapmanız gerekir.
Bu kuruluş yöneticisi hesabı, Azure portalında soyut olmayana atanması gerekir.

### <a name="test-sso"></a>Test SSO 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Soyut kutucuğa tıkladığınızda, erişim Paneli'nde, otomatik olarak için SSO'yu ayarladığınız soyut oturum. Erişim paneli hakkında daha fazla bilgi için bkz: [erişim Paneli'ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [ SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

