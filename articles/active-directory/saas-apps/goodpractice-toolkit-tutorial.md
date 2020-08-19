---
title: 'Öğretici: fikir araçları araç seti ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve fikir araçları araç seti arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/12/2020
ms.author: jeedes
ms.openlocfilehash: 10773a2a379291fa861dbb2adfdda2ba3f63117f
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88550648"
---
# <a name="tutorial-azure-active-directory-integration-with-mind-tools-toolkit"></a>Öğretici: fikir araçları araç seti ile Azure Active Directory tümleştirme

Bu öğreticide, fikir araçları araç takımını Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.

Bu tümleştirmeyle şunları yapabilirsiniz:

* Azure AD 'de, fikir araçları araç seti 'ne erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla fikir araçları araç seti (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlama.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portal.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini, fikir araçları araç seti ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Çoklu oturum açma (SSO) etkin olan bir fikir araçları araç seti aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Fikir araçları araç seti SP tarafından başlatılan SSO 'yu destekler.
* Fikir araç seti, tam zamanında Kullanıcı sağlamayı destekler.
* Fikir araçları araç takımını yapılandırdıktan sonra, oturum denetimini zorunlu kılabilirsiniz. Bu denetim, kuruluşunuzun hassas verilerinin, gerçek zamanlı olarak ayıklanma ve savunma korumasını korur. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="add-mind-tools-toolkit-from-the-gallery"></a>Galeriden fikir araçları araç seti ekle

Fikir araçları araç takımını Azure AD 'ye tümleştirmeyi yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize fikir araçları araç seti eklemeniz gerekir.

1. Bir iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
1. En soldaki gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **fikir araçları araç seti** yazın.
1. Arama sonuçlarından **fikir araçları araç seti** ' ni seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **B. Simon**adlı bir test kullanıcısı kullanarak, fikir araçları araç seti Ile Azure AD çoklu oturum açma 'yı yapılandırıp test edersiniz. Çoklu oturum açma için, bir Azure AD kullanıcısı ile ilgili Kullanıcı ve göz önünde Araçlar araç seti arasında bağlı bir ilişki kurmanız gerekir.

Azure AD çoklu oturum açma 'yı fikir araçları araç seti ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** .
    1. B. Simon ile Azure AD çoklu oturum açma sınamasını test etmek için **[bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** .
    1. Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek üzere **[Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user)** .
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[fikir araçları araç SETI SSO 'Yu yapılandırın](#configure-mind-tools-toolkit-sso)** .
    1. Göz önünde bulundurularak bir araç **[seti test kullanıcısı oluşturun](#create-a-mind-tools-toolkit-test-user)** . Bu karşılık gelen bu, kullanıcının Azure AD gösterimine bağlıdır.
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[test SSO 'su](#test-sso)** .

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Bu bölümde, aşağıdaki adımları izleyerek Azure AD çoklu oturum açma 'yı fikir araçları araç seti ile yapılandırırsınız:

1. [Azure Portal](https://portal.azure.com/), **fikir araçları araç seti** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Tek oturum açma vurgulanmış şekilde Yönet bölümü](common/select-sso.png)

1. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-besi** seçin.

    ![SAML vurgulanmış olarak çoklu oturum açma yöntemi seç iletişim kutusu](common/select-saml-option.png)

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesini seçin.

    ![Temel SAML yapılandırması için kalem simgesi vurgulanmış şekilde SAML ile çoklu oturum açmayı ayarla sayfası](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **oturum açma URL 'si** kutusuna, modele sahip bir URL girin `https://app.goodpractice.net/#/<subscriptionUrl>/s/<locationId>` .

    > [!NOTE]
    > **Oturum açma URL 'si** değeri gerçek değil. Değeri, gerçek oturum açma URL 'SI ile güncelleştirin. Değeri almak için, [fikir araçları araç seti istemci destek ekibine](mailto:support@goodpractice.com) başvurun.

1. **SAML Ile çoklu oturum açmayı ayarla** sayfasında, **SAML imzalama sertifikası** bölümüne gidin. **Federasyon meta VERI XML**' nin SAĞıNDA, XML metnini indirmek ve bilgisayarınıza kaydetmek için **İndir** ' i seçin. XML içeriği, belirlediğiniz seçeneklere bağlıdır.

    ![SAML Imzalama sertifikası bölümü, Federasyon meta veri XML ' nün yanında vurgulanmış şekilde](common/metadataxml.png)

1. **Fikir ayarlama araçları araç seti** bölümünde, gereken URL 'lerden hangisinin istediğinizi kopyalayın.

    * **Oturum açma URL 'SI**

    * **Azure AD tanımlayıcısı**

    * **Oturum kapatma URL 'SI**

    ![Yapılandırma URL 'Leri vurgulanmış şekilde ayarlanan fikir araçları araç seti bölümü](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portal, B. Simon adlı bir test kullanıcısı oluşturacaksınız:

1. Azure Portal en sol tarafında, kullanıcılar **Azure Active Directory**  >  **Users**  >  **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmında **Yeni Kullanıcı**' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına **B. Simon**girin.  
   1. **Kullanıcı adı** alanına **B. Simon@**_şirketetkialanı_yazın **.** _uzantı_. Örneğin, B.Simon@contoso.com.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda gösterilen değeri yazın.
   1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, fikir araçları araç seti 'ne erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirin.

1. Azure Portal **Kurumsal uygulamalar**  >  **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **fikir araçları araç seti**' ni seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümüne gidin ve **Kullanıcılar ve gruplar**' ı seçin.

   ![Kullanıcılar ve gruplar vurgulanmış şekilde Yönet bölümü](common/users-groups-blade.png)

1. **Kullanıcı ekle**'yi seçin. **Atama Ekle** Iletişim kutusunda **Kullanıcılar ve gruplar**' ı seçin.

   ![Kullanıcı Ekle vurgulanmış olan kullanıcılar ve gruplar penceresi](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** öğesini seçin. Ardından ekranın alt kısmındaki **Seç** düğmesini seçin.
1. SAML onaylama 'da herhangi bir rol değeri bekleliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin. Ardından ekranın alt kısmındaki **Seç** düğmesini seçin.
1. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

## <a name="configure-mind-tools-toolkit-sso"></a>Fikir yapılandırma araç seti SSO

**Fikir araçları araç seti** tarafında çoklu oturum açmayı yapılandırmak için, Indirilen **Federasyon meta verileri XML** metnini ve daha önce kopyalanmış URL 'Leri [fikir araçları araç seti destek ekibine](mailto:support@goodpractice.com)gönderin. Bu ayar, SAML SSO bağlantısının her iki tarafında da düzgün bir şekilde ayarlanmasını sağlamak için bu ayarı yapılandırır.

### <a name="create-a-mind-tools-toolkit-test-user"></a>Fikir araçları araç seti test kullanıcısı oluşturma

Bu bölümde, fikir araçları araç seti ' nde B. Simon adlı bir Kullanıcı oluşturacaksınız.

Fikir araçları araç seti, varsayılan olarak etkinleştirilen tam zamanında sağlamayı destekler. Bu bölümde gerçekleştirmeniz gereken bir işlem yok. Bir Kullanıcı, fikir araçları araç setinde zaten mevcut değilse, fikir araçları araç seti 'ne erişmeye çalıştığınızda yeni bir tane oluşturulur.

### <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı My Apps portalını kullanarak test edersiniz.

Uygulamalarım portalındaki fikir araçları araç seti kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız fikir araçları araç setinde otomatik olarak oturumunuz açılır. Uygulamalarım portalı hakkında daha fazla bilgi için bkz. [My Apps portalına giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirmek için öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile fikir araçları araç seti 'ni deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimlerle fikir araçları araç setini koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
