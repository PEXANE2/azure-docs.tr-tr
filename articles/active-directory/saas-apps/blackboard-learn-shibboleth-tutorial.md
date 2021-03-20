---
title: 'Öğretici: kara Pano öğrenimi ile Azure Active Directory tümleştirme-Shibbotath | Microsoft Docs'
description: Azure Active Directory ve BlackICE-Shibboteth arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/19/2021
ms.author: jeedes
ms.openlocfilehash: b1b3f265d0e1fcad2953292c5227c2630c6df229
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "101649922"
---
# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn---shibboleth"></a>Öğretici: kara tahta öğrenimi Azure Active Directory tümleştirme-Shibbotath

Bu öğreticide, Azure Active Directory (Azure AD) ile BlackICE 'ın öğrenimlerini nasıl tümleştirileceğini öğreneceksiniz. BlackICE 'ın öğrenimlerini Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de kara Pano öğrenimlerini içeren denetim-Shibboyeth.
* Kullanıcılarınızın Azure AD hesaplarıyla BlackICE-Shibboda 'da otomatik olarak oturum açabilmesi için kullanıcılarınızı etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:
 
* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* BlackICE-Shibboyeth çoklu oturum açma (SSO) özellikli abonelik öğrenimi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* BlackICE 'ın öğrenme-Shibboyeth, **SP** tarafından başlatılan SSO 'yu destekler

## <a name="add-blackboard-learn---shibboleth-from-the-gallery"></a>BlackICE 'ın öğrenimlerini ekleme-Shibboya Galerisi

BlackICE 'ın öğrenilmesi hakkında bilgi edinin-Shibbotath 'ın Azure AD 'ye tümleştirilmesi için, Galeriden yönetilen SaaS uygulamaları listenize BlackICE 'ın öğrenimi öğrenimi ' ni eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriye Ekle** bölümünde, arama kutusuna **BlackICE-Shibboleth** yazın.
1. Sonuçlar panelinden **BlackICE 'ın öğrenimlerini** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-blackboard-learn---shibboleth"></a>Azure AD SSO 'yu BlackICE için yapılandırma ve test etme öğrenme-Shibbotath

**B. Simon** adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu BlackICE-Shibbotath ile birlikte yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ile ilgili Kullanıcı ve BlackICE-Shibboteth arasında bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu BlackICE-Shibbotath ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[BlackICE 'ı yapılandırma-Shibbotath SSO](#configure-blackboard-learn---shibboleth-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. **[Kara Pano Oluştur-shibbotath test kullanıcısı](#create-blackboard-learn---shibboleth-test-user)** -B. Simon BlackICE 'ın, kullanıcının Azure AD gösterimine bağlı öğrendiği bilgi edinin.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı BlackICE-Shibboteth öğrenimi ile yapılandırmak için aşağıdaki adımları uygulayın:

1. Azure portal, **BlackICE-Shibboyeth** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

3. **SAML Ile tek Sign-On ayarla** sayfasında, **temel SAML yapılandırması** iletişim kutusunu açmak için kalem simgesi ' ne tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<yourblackoardlearnserver>.blackboardlearn.com/Shibboleth.sso/Login`

    b. **Tanımlayıcı** kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<yourblackoardlearnserver>.blackboardlearn.com/shibboleth-sp`

    c. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<yourblackoardlearnserver>.blackboardlearn.com/Shibboleth.sso/SAML2/POST`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Sign-On URL 'SI, tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [kara Pano öğrenimlerini-Shibbotath istemci destek ekibine](https://www.blackboard.com/forms/contact-us_form.aspx) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **BlackICE panosunu ayarla-Shibboyeth** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

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

Bu bölümde, BlackICE 'ın öğrenimi öğrenimi-Shibboyeth aracılığıyla Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **BlackICE-Shibboyeth**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="configure-blackboard-learn---shibboleth-sso"></a>BlackICE 'ın öğrenimlerini yapılandırma-Shibboteth SSO

**BlackICE-Shibboleth** tarafında çoklu oturum açmayı yapılandırmak Için Indirilen **Federasyon meta veri XML** 'sini ve Azure Portal ' den uygun kopyalanmış URL 'Leri [BlackICE-Shibboleth destek ekibine](https://www.blackboard.com/forms/contact-us_form.aspx)göndermeniz gerekir. Bu ayar, SAML SSO bağlantısının her iki tarafında da düzgün bir şekilde ayarlanmasını sağlamak üzere ayarlanmıştır.

### <a name="create-blackboard-learn---shibboleth-test-user"></a>Kara pano oluşturma öğrenme-Shibbotath test kullanıcısı

Bu bölümde, BlackICE-Shibboteth 'da Britta Simon adlı bir Kullanıcı oluşturacaksınız. BlackICE- [shibbotath destek ekibi](https://www.blackboard.com/forms/contact-us_form.aspx) sayesinde kullanıcıları kara bilgi edinin-shibboseth platformunda ekleme. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

* Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu, oturum açma akışını başlatabileceğiniz BlackICE-Shibboyeth oturum açma URL 'sine yeniden yönlendirilir. 

* Kara Pano öğrenimi-Shibboyeth oturum açma URL 'sine doğrudan gidin ve oturum akışını buradan başlatın.

* Microsoft My Apps ' i kullanabilirsiniz. Uygulamalarım içindeki BlackICE-Shibboteth kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız BlackICE-Shibbotath 'da otomatik olarak oturum açmış olmanız gerekir. Uygulamalarım hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

BlackICE 'ın öğrenimlerini yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).