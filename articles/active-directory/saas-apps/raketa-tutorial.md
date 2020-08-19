---
title: 'Öğretici: Çketa ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory Microsoft Docs'
description: Azure Active Directory ile Çketa arasında çoklu oturum açmayı yapılandırmayı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/28/2020
ms.author: jeedes
ms.openlocfilehash: f5a01d74d22d6ea010d80ef1ef8ce1a53fbd4422
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88548822"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-raketa"></a>Öğretici: Çketa ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile bir Üketa tümleştirme hakkında bilgi edineceksiniz. Azure AD ile bir Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de, Çketa erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla birlikte otomatik olarak oturum açmalarına olanak tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Tek oturum açma (SSO) özellikli bir abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Rampa, **SP** tarafından başlatılan SSO 'yu destekler.
* Kketa 'yı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve geri alımını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-raketa-from-the-gallery"></a>Galeriden Kketa ekleme

Korvaa 'nın tümleştirmesini Azure AD 'ye göre yapılandırmak için galerideki bir gketa 'yı yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti [1] seçin.

    ![rkt_1](./media/raketa-tutorial/azure-active-directory.png)

1. [2] **kurumsal uygulamalarına** gidin ve **tüm uygulamalar** [3] ' i seçin.

1. Yeni uygulama eklemek için yeni [4] **uygulamasını** seçin. 

    ![rkt_2](./media/raketa-tutorial/new-app.png)

1. **Galeriden Ekle** [5] bölümünde, [6] arama kutusuna **bir bketa** yazın.

1. [7] sonuçlar panelinden **bir Arketa** seçin ve ardından **Ekle** düğmesine [8] tıklayın. 

    ![rkt_3](./media/raketa-tutorial/add-btn.png)


## <a name="configure-and-test-azure-ad-single-sign-on-for-raketa"></a>Hketa için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak, jketa Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ile ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu, Korketa ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için, **[Aketa SSO 'Yu yapılandırın](#configure-raketa-sso)** .
    1. User 'ın Azure AD gösterimine bağlı olan bir Bketa **[test kullanıcısı oluşturma](#create-raketa-test-user)** -bu, bir.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **aketa** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma** [9] seçeneğini belirleyin.

    ![rkt_4](./media/raketa-tutorial/manage-sso.png)

1. **Çoklu oturum açma yöntemi seçin** sayfasında [9], **SAML** [10] öğesini seçin.

    ![rkt_5](./media/raketa-tutorial/saml.png)

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** [11] için Düzenle/kalem simgesine tıklayın.

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    1. **Tanımlayıcıda (VARLıK kimliği)** [12] ve **oturum açma URL 'si** [14] metin kutularında URL 'yi yazın: `https://raketa.travel/` .

    1. **Yanıt URL 'si** metin kutusunda [13], şu stili kullanarak bir URL yazın: `https://raketa.travel/sso/acs?clientId=<CLIENT_ID>` .  

    ![rkt_6](./media/raketa-tutorial/enter-urls.png)

    > [!NOTE]
    > Yanıt URL 'SI değeri gerçek değil. Değeri gerçek yanıt URL 'siyle güncelleştirin. Değeri almak için [bir istemci destek ekibine](mailto:help@raketa.travel) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için [15] **İndir** seçeneğini belirleyin.

1. **En kornu ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    1. Oturum açma URL 'SI [16] – kullanıcıları kimlik doğrulama sistemine yönlendirmek için kullanılan, yetkilendirme Web sayfası URL 'SI.

    1. Azure AD tanımlayıcısı [17] – Azure AD tanımlayıcısı.

    1. Logout URL 'SI [18] – oturum kapatıldıktan sonra kullanıcıları yeniden yönlendirmek için kullanılan Web sayfası URL 'SI.

    ![rkt_7](./media/raketa-tutorial/copy-urls.png)


### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory** [1] öğesini seçin, **Kullanıcılar** [19] öğesini seçin ve ardından **tüm kullanıcılar** [20] ' ı seçin.

1. Ekranın üst kısmında **Yeni Kullanıcı** [21] seçeneğini belirleyin.

    ![rkt_8](./media/raketa-tutorial/new-user.png)

1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:

   1. [22] **Kullanıcı adı** alanına öğesini girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.

   1. **Ad** alanına [23] yazın `B.Simon` .

   1. **Parolayı göster** onay kutusunu (25) seçin ve ardından [24] **parola** kutusunda görüntülenen değeri yazın.

   1. **Oluştur** [26] öğesine tıklayın. 

    ![rkt_9](./media/raketa-tutorial/create-user.png)


### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, jketa 'ya erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar** [2] ' ı seçin ve ardından **tüm uygulamalar** [3] ' i seçin.

1. Uygulamalar listesinde, **Korketa** [27] öğesini seçin.  

    ![rkt_10](./media/raketa-tutorial/add-raketa.png)

1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar** [28] ' ı seçin. 

    ![rkt_11](./media/raketa-tutorial/users-groups.png)

1. **Kullanıcı Ekle** [29] seçeneğini belirleyin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** [30] öğesini seçin.

    ![rkt_12](./media/raketa-tutorial/add-user-raketa.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** [31] öğesini seçin ve ardından ekranın alt kısmındaki **Seç** [32] düğmesine tıklayın.

1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine [33] tıklayın. 

    ![rkt_13](./media/raketa-tutorial/assign-user.png)

## <a name="configure-raketa-sso"></a>Üketa SSO 'yu yapılandırma

**Bir** yandan tek oturum açma 'yı yapılandırmak için, indirilen **sertifikayı (Base64)** ve Azure Portal ' den uygun kopyalanmış URL 'leri, [bir destek ekibine](mailto:help@raketa.travel)göndermeniz gerekir. Bu ayar, SAML SSO bağlantısının her iki tarafında da düzgün bir şekilde ayarlanmasını sağlamak üzere ayarlanmıştır.

### <a name="create-raketa-test-user"></a>Bir test kullanıcısı oluşturma

Bu bölümde, Çketa 'da B. Simon adlı bir Kullanıcı oluşturacaksınız. Rat 'leri bir platformdaki kullanıcıları eklemek için [bir destek ekibi](mailto:help@raketa.travel) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde bir Kketa kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Kketa 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile bir Çketa deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimlerle Kketa koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)