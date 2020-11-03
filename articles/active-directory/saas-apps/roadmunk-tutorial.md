---
title: 'Öğretici: Roadmunk ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Roadmunk arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/28/2020
ms.author: jeedes
ms.openlocfilehash: 2ee794237c644cd6ff9652f4bc2e47a8ff590161
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243943"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-roadmunk"></a>Öğretici: Roadmunk ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Roadmunk 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Roadmunk 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Roadmunk 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Roadmunk otomatik olarak oturum açmalarına olanak tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Roadmunk çoklu oturum açma (SSO) etkin abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Roadmunk **SP ve ıDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-roadmunk-from-the-gallery"></a>Galeriden Roadmunk ekleme

Roadmunk tümleştirmesini Azure AD 'ye göre yapılandırmak için, Galeriden Roadmunk yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** ' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama** ' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Roadmunk** yazın.
1. Sonuçlar panelinden **Roadmunk** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-sso-for-roadmunk"></a>Roadmunk için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu Roadmunk ile yapılandırın ve test edin. SSO 'nun çalışması için, Roadmunk içinde bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Roadmunk ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[ROADMUNK SSO 'Yu yapılandırın](#configure-roadmunk-sso)** .
    1. Kullanıcının Azure AD gösterimine bağlı olan Roadmunk 'de B. Simon 'ya karşılık gelen bir **[Roadmunk test kullanıcısı oluşturun](#create-roadmunk-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **Roadmunk** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma** ' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML** ' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **hizmet sağlayıcısı meta verileri dosyanız** varsa ve **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları gerçekleştirin:

    a. **Meta veri dosyasını karşıya yükle** ' ye tıklayın.

    ![Meta veri dosyasını karşıya yükle](common/upload-metadata.png)

    b. **ROADMUNK SSO 'Yu Yapılandır** bölümünde 4. adımda indirdiğiniz meta veri dosyasını seçmek için **klasör logosu** ' na tıklayın ve **karşıya yükle** ' ye tıklayın.

    ![meta veri dosyası seçin](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra, **tanımlayıcı** ve **yanıt URL** değerleri temel SAML yapılandırması bölümünde otomatik olarak doldurulur.

    ![image1](common/idp-intiated.png)

    > [!Note]
    > **Tanımlayıcı** ve **yanıt URL 'si** değerleri otomatik olarak alamazsanız, değerleri gereksinimlerinize göre el ile girin.

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![image2](common/metadata-upload-additional-signon.png)

    **Oturum açma URL** 'si metin kutusuna URL 'yi yazın:`https://login.roadmunk.com`

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Roadmunk ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)
### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory** ' i seçin, **Kullanıcılar** ' ı seçin ve ardından **tüm kullanıcılar** ' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur** 'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Roadmunk 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar** ' ı seçin ve ardından **tüm uygulamalar** ' ı seçin.
1. Uygulamalar listesinde **Roadmunk** ' yi seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcı Ekle** ' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-roadmunk-sso"></a>Roadmunk SSO 'yu yapılandırma

1. Roadmunk Web sitesinde yönetici olarak oturum açın.

1. Sayfanın altındaki **Kullanıcı simgesine** tıklayın ve **Hesap ayarları** ' nı seçin.

    ![Hesap ayarları](./media/roadmunk-tutorial/account.png)

1. **Şirket > kimlik doğrulama ayarları** ' na gidin.

1. Kimlik doğrulama ayarları ' nda aşağıdaki adımları gerçekleştirin.

    ![Kimlik Doğrulama Ayarları](./media/roadmunk-tutorial/saml-sso.png)

    a. **SAML çoklu oturum açma (SSO)** özelliğini etkinleştirin.

    b. Adım 1 ' de, **meta VERI XML** dosyasını karşıya yükleyebilir veya URL 'ye verebilirsiniz.

    c. 2. adımda, **Roadmunk meta veri** dosyasını indirin ve bilgisayarınıza kaydedin.

    d. SSO ile oturum açmak istiyorsanız, yalnızca 3. adımda **SAML Sign-In zorla** ' yı etkinleştirin.

    e. **Kaydet** 'e tıklayın.


### <a name="create-roadmunk-test-user"></a>Roadmunk test kullanıcısı oluştur

1. Roadmunk Web sitesinde yönetici olarak oturum açın.

1. Sayfanın altındaki **Kullanıcı simgesine** tıklayın ve **Hesap ayarları** ' nı seçin.

    ![Hesap ayarları test kullanıcısı](./media/roadmunk-tutorial/account.png)

1. **Kullanıcılar** sekmesine gidin ve **kullanıcıyı davet et** ' e tıklayın.

    ![Kullanıcıyı davet et](./media/roadmunk-tutorial/create-user.png)

1. Formda gerekli alanları girin ve **davet et** ' e tıklayın.


## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Roadmunk kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Roadmunk için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="next-steps"></a>Sonraki Adımlar

Roadmunk yapılandırıldıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).


