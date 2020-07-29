---
title: 'Öğretici: Cloud akademik My-SSO ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ile bulut akademik My-SSO arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 560c86fb-e25c-4428-a1dd-a5711cfece5c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 07/16/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb118e4d2fa811bf88ff13db84848ebb230ed209
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87294901"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cloud-academy---sso"></a>Öğretici: Cloud akademik My-SSO ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide Cloud akademik My-SSO 'yu Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Cloud akademik My-SSO 'yu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de bulut akademik My-SSO 'ya erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla bulut akademik My-SSO ' da otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Cloud akademik My-SSO çoklu oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Bulut akademik My-SSO, **SP** tarafından başlatılan SSO 'yu destekler

* Cloud akademik My-SSO 'yu yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve bu verileri korumayı koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-cloud-academy---sso-from-the-gallery"></a>Galeriden bulut akademik My-SSO 'SU ekleniyor

Cloud akademik My-SSO ile Azure AD arasındaki tümleştirmeyi yapılandırmak için, Galeriden bulut akademik My-SSO ' u yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Cloud AKADEMIK My-SSO** yazın.
1. Sonuçlar panelinden **Cloud akademik My-SSO** ' yı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-sso-for-cloud-academy---sso"></a>Cloud akademik My-SSO için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu Cloud akademik My-SSO ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve Cloud akademik My-SSO içindeki ilgili Kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO 'yu Cloud akademik My-SSO ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Cloud akademik My-SSO SSO 'Yu yapılandırın](#configure-cloud-academy-sso-sso)** .
    1. Cloud akademik **[My-SSO test kullanıcısı oluşturma](#create-cloud-academy-sso-test-user)** -Kullanıcı Azure AD gösterimine bağlı olan Cloud akademik My-SSO 'da B. Simon 'un bir karşılığı olacak.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Cloud AKADEMIK My-SSO** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    **Oturum açma URL** 'si metin kutusuna URL 'yi yazın:`https://cloudacademy.com/login/enterprise/`

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'sini** kopyalamak ve bilgisayarınıza kaydetmek için Kopyala düğmesine tıklayın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

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

Bu bölümde, Cloud akademik My-SSO 'ya erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Cloud akademik My-SSO**' yı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-cloud-academy-sso-sso"></a>Cloud akademik My-SSO SSO 'yu yapılandırma

1. Farklı bir tarayıcı penceresinde, bulut akademik My-SSO şirket sitesinde yönetici olarak oturum açın.

1. Şirket adına tıklayın ve menüden **ayarlar & tümleştirmeler** ' i seçin.

    ![Yapılandırma ](./media/cloud-academy-sso-tutorial/config-1.PNG)

1. **Ayarlar & tümleştirmeler** sayfasında, **tümleştirmeler** sekmesine gidin ve **SSO** kartı ' na tıklayın.

    ![Yapılandırma ](./media/cloud-academy-sso-tutorial/config-2.PNG)

1. Aşağıdaki sayfada aşağıdaki adımları gerçekleştirin:

    ![Yapılandırma ](./media/cloud-academy-sso-tutorial/config-3.PNG)

    a. **VARLıK kimliği URL 'si** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **varlık kimliği** değerini yapıştırın.

    b. **SSO URL** metin kutusuna, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    c. İndirilen **sertifikayı (base64)** Azure Portal Not defteri ' nden açın ve içeriği **sertifika** metin kutusuna yapıştırın.

    d. **Ad kimliği biçimi** metin kutusunda, varsayılan değer,`urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`

1. **Kaydet** düğmesine tıklayın.

    > [!NOTE]
    > Cloud akademik My-SSO yapılandırma hakkında daha fazla bilgi için lütfen [Destek makalesine](https://support.cloudacademy.com/hc/articles/360043908452-Setting-Up-Single-Sign-On)başvurun.

### <a name="create-cloud-academy-sso-test-user"></a>Bulut akademik My-SSO test kullanıcısı oluştur

1. **Cloud akademik My-SSO** ' da oturum açın.

1. Şirket adına tıklayın ve menüden **Üyeler** ' i seçin.

    ![ Test kullanıcısı oluştur ](./media/cloud-academy-sso-tutorial/create-user.PNG)

1. **Üyeleri davet et** ' e tıklayın ve **tek üye davet et**' i seçin.

    ![ Test kullanıcısı oluştur ](./media/cloud-academy-sso-tutorial/create-user-1.PNG)

1. Gerekli alanları girin ve **davet et**' e tıklayın.

    ![ Test kullanıcısı oluştur ](./media/cloud-academy-sso-tutorial/create-user-2.PNG)

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Cloud akademik My-SSO kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Cloud akademik My-SSO ' da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Cloud akademik My-SSO 'yu deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimlerle bulut akademik My-SSO 'SU nasıl korunur](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)