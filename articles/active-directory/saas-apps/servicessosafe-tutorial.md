---
title: 'Öğretici: | ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory Services@SoSafe Microsoft Docs'
description: Azure Active Directory ve arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin Services@SoSafe .
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/23/2020
ms.author: jeedes
ms.openlocfilehash: 3fe42dd37bda3bef549570c32018179b88d06957
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93365004"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-servicessosafe"></a>Öğretici: ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory Services@SoSafe

Bu öğreticide, Services@SoSafe Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Services@SoSafeAzure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de erişimi olan denetim Services@SoSafe .
* Kullanıcılarınızın Azure AD hesaplarıyla otomatik olarak oturum açmalarına olanak sağlayın Services@SoSafe .
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Services@SoSafe Çoklu oturum açma (SSO) etkin abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Services@SoSafe**SP ve ıDP** tarafından başlatılan SSO 'yu destekler
* Services@SoSafe**tam zamanında** Kullanıcı sağlamayı destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı, tek bir kiracıda yalnızca bir örneğin yapılandırılabilmesini sağlamak için sabit bir dize değeridir.


## <a name="adding-servicessosafe-from-the-gallery"></a>Services@SoSafeGaleriden ekleme

Services@SoSafe' I Azure AD ile tümleştirmeyi yapılandırmak için, Services@SoSafe Galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** ' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama** ' yı seçin.
1. **Galeriden Ekle** bölümünde, Services@SoSafe Arama kutusuna yazın.
1. Services@SoSafeSonuçlar panelinden seçim yapın ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-sso-for-servicessosafe"></a>İçin Azure AD SSO 'yu yapılandırın ve test edin Services@SoSafe

Services@SoSafe **B. Simon** adlı bir test KULLANıCıSı kullanarak Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve içindeki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir Services@SoSafe .

Azure AD SSO 'yu ile yapılandırmak ve test etmek için Services@SoSafe aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[ServicesSoSafe SSO 'Yu yapılandırma](#configure-servicessosafe-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. **[ServicesSoSafe test kullanıcısı oluşturun](#create-servicessosafe-test-user)** Services@SoSafe ; Bu, kullanıcının Azure AD gösterimine bağlı olan, ' de B. Simon 'a karşılık gelen bir karşılığı.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, Services@SoSafe uygulama tümleştirme sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma** ' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML** ' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, uygulama zaten Azure ile önceden tümleştirildiği için kullanıcının herhangi bir adım yapması gerekmez.

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://api.sosafe.de/v1/auth/saml/login/<TENANT_ID>`

    > [!NOTE]
    > Oturum açma URL 'SI değeri gerçek değil. Değeri gerçek oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için [ Services@SoSafe istemci destek ekibine](mailto:support@sosafe.de) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Ayarla Services@SoSafe bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

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

Bu bölümde, uygulamasına erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz Services@SoSafe .

1. Azure portal **Kurumsal uygulamalar** ' ı seçin ve ardından **tüm uygulamalar** ' ı seçin.
1. Uygulamalar listesinde, öğesini seçin Services@SoSafe .
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcı Ekle** ' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-servicessosafe-sso"></a>ServicesSoSafe SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde Services@SoSafe Web sitesinde yönetici olarak oturum açın.

1. **Genişletilmiş veriler** ' e tıklayın ve aşağıdaki sayfada aşağıdaki adımları gerçekleştirin.

    ![Genişletilmiş veri SAML sayfası](./media/servicessosafe-tutorial/saml-settings.png)

    a. Azure portal 'den **Azure KIRACı kimliği** metin kutusuna Kiracı kimliği değerinin değerini yapıştırın.

    b. İndirilen sertifika **(base64)** Azure Portal Not defteri ' nden açın ve içeriği **sertifika** metin kutusuna yapıştırın.

    c. **Oturum açma URL 'si** kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    d. **Azure AD tanımlayıcı** kutusunda, Azure Portal KOPYALADıĞıNıZ **varlık kimliği** değerini yapıştırın.

    e. **Logout URL 'si** kutusunda, Azure Portal KOPYALADıĞıNıZ **Logout URL** değerini yapıştırın.

    f. **Kaydet** 'e tıklayın

### <a name="create-servicessosafe-test-user"></a>ServicesSoSafe test kullanıcısı oluşturma

Bu bölümde, ' de Britta Simon adlı bir Kullanıcı oluşturulur Services@SoSafe . Services@SoSafe , varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir kullanıcı içinde zaten mevcut değilse Services@SoSafe , kimlik doğrulamasından sonra yeni bir tane oluşturulur.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

#### <a name="sp-initiated"></a>SP başlatıldı:

1. Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu Services@SoSafe , oturum açma akışını BAŞLATABILECEĞINIZ URL 'yi oturum açmaya yönlendirir.  

1. Services@SoSafeDoğrudan oturum açma URL 'sine gidin ve oturum açma akışını buradan başlatın.

#### <a name="idp-initiated"></a>IDP başlatıldı:

* Azure portal **Bu uygulamayı test et** ' e tıklayın ve SSO 'yu ayarladığınız ' de otomatik olarak oturum açmış olmanız gerekir Services@SoSafe 

Uygulamayı dilediğiniz modda test etmek için Microsoft Access panel ' i de kullanabilirsiniz. Services@SoSafeErişim panelinde Kutucuğa tıkladığınızda, SP modunda yapılandırıldıysa, oturum açma akışını başlatmak için uygulama oturum açma sayfasına yönlendirilirsiniz ve ıDP modunda yapılandırıldıysa, SSO 'yu ayarladığınız ' de otomatik olarak oturum açmış olmanız gerekir Services@SoSafe . Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="next-steps"></a>Sonraki adımlar

Yapılandırdıktan sonra Services@SoSafe , kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).


