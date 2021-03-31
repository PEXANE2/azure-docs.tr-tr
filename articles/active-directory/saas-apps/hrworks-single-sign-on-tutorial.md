---
title: 'Öğretici: Hrçalışmatek Sign-On ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Hrçalışmalerde çoklu oturum açma arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/02/2020
ms.author: jeedes
ms.openlocfilehash: d6b23213d9d986c62a227b3e182a22896d128222
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "92442550"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-hrworks-single-sign-on"></a>Öğretici: hr, tek Sign-On ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Hrçalışmaları tek Sign-On Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Hrçalışmatek Sign-On Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Hrçalışmalar çoklu oturum açma 'ya erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla HR, tek Sign-On için otomatik olarak oturum açmalarına izin verir.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* HR, tek Sign-On çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Hrçalışmatek Sign-On **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-hrworks-single-sign-on-from-the-gallery"></a>Galeriden Hrçalışmatek Sign-On ekleme

HR, tek Sign-On 'ın Azure AD 'ye tümleştirilmesini yapılandırmak için, galerideki HR, tek Sign-On öğesini yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **hrçalışmaçoklu oturum açma** yazın.
1. Sonuçlar panelinden **Hrçalışmaçoklu oturum açma** 'yı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-hrworks-single-sign-on"></a>Azure AD çoklu oturum açmayı, hr, tek Sign-On için yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak Hrçalışmatek Sign-On Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve Hrwork çoklu oturum açma içindeki ilgili Kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO 'yu Hrçalışmalar çoklu oturum açma özelliğiyle yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Hrçalışmatek Sign-On SSO yapılandırma](#configure-hrworks-single-sign-on-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    * **[HR, tek Sign-On test kullanıcısı oluşturma](#create-hrworks-single-sign-on-test-user)** -kullanıcının Azure AD gösterimine bağlı tek Sign-On HRM 'de B. Simon 'a sahip olmak için.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **hrçalışmaçoklu oturum açma** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://login.hrworks.de/?companyId=<companyId>&directssologin=true`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Sign-On URL 'siyle güncelleştirin. Değeri almak için [Hrçalışmatek Sign-On istemci desteği ekibine](mailto:nadja.sommerfeld@hrworks.de) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Hrçalışmalarınızla çoklu oturum açma ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, Hruse çoklu oturum açma 'ya erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Hrçalışmaçoklu oturum açma**' yı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-hrworks-single-sign-on-sso"></a>Hrçalışmatek Sign-On SSO yapılandırma

1. Hrmın çoklu oturum açma içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yüklemeniz**' ne tıklayarak **uygulamalarımın güvenli oturum açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalarım uzantısı](common/install-myappssecure-extension.png)

1. Tarayıcıya Uzantı eklendikten sonra, **HR, tek oturum açma ayarla** ' ya tıklayın. Bu, sizi hrup tek Sign-On uygulamasına yönlendirir. Buradan, Hrçalışmalar çoklu oturum açma 'da oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı, uygulamayı sizin için otomatik olarak yapılandırır ve 3-4 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. Hrçalışmatek Sign-On el ile kurmak isterseniz, yeni bir Web tarayıcısı penceresi açın ve HR, tek Sign-On şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1.   >    >    >  Menü çubuğunun sol tarafındaki yönetici temelleri güvenliğine **Çoklu oturum açma** ' ya tıklayın ve aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırma](./media/hrworks-single-sign-on-tutorial/configure01.png)

    a. **Çoklu oturum açma kullan** kutusunu işaretleyin.

    b. **Meta veri giriş yöntemi** olarak **XML meta** verilerini seçin.

    c. **NameID Için** **bağımsız bir NameID tanımlayıcı** değeri seçin.

    d. Not defteri 'nde, Azure portal indirdiğiniz meta veri XML dosyasını açın, içeriğini kopyalayın ve sonra **meta veri** metin kutusuna yapıştırın.

    e. **Kaydet**’e tıklayın.

### <a name="create-hrworks-single-sign-on-test-user"></a>Hrçalışmatek Sign-On test kullanıcısı oluşturma

Azure AD kullanıcılarını etkinleştirmek için, hr, çoklu oturum açma 'da oturum açın, hr, çoklu oturum açma ' ya sağlanması gerekir. Hrçalışmalerde çoklu oturum açma bölümünde, sağlama el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. HR, tek Sign-On yönetici olarak oturum açın.

1. **Yönetici**  >  **kişileri**  >    >  menü çubuğunun sol tarafından **Yeni kişiyi** kişiler ' e tıklayın.

     ![Ekran görüntüsü, kişiler ve yeni kişi seçiliyken H R çalışma sayfasını gösterir.](./media/hrworks-single-sign-on-tutorial/configure02.png)

1. Açılır pencerede **İleri**' ye tıklayın.

    ![Ekran görüntüsü, kişi için seçim yapabileceğiniz ülkelerin listesini gösterir.](./media/hrworks-single-sign-on-tutorial/configure03.png)

1. **Yasal koşullar için ülke ile yeni kişi oluştur** açılır penceresinde, **ad**, **Soyadı** gibi ilgili ayrıntıları girin ve **Oluştur**' a tıklayın.

    ![Ekran görüntüsü, kişi için ilk ve son adları girebileceğiniz metin kutularını gösterir.](./media/hrworks-single-sign-on-tutorial/configure04.png)

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Hrçalışmatek Sign-On kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız HR, tek Sign-On otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir? ](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory'de koşullu erişim nedir?](../conditional-access/overview.md)

- [Azure AD ile Hrçalışmatek Sign-On deneyin](https://aad.portal.azure.com/)