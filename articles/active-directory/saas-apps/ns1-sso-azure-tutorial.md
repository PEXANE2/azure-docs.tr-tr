---
title: 'Öğretici: Azure için NS1 SSO ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure için Azure Active Directory ve NS1 SSO arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/12/2020
ms.author: jeedes
ms.openlocfilehash: d0fee044506a9a19e09478ef8d70b3719ecc167a
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88554269"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-ns1-sso-for-azure"></a>Öğretici: Azure için NS1 SSO ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Azure için NS1 SSO 'yu Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Azure için NS1 SSO 'yu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Azure için NS1 SSO 'ya erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Azure için NS1 SSO 'da otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin, Azure portal.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Azure çoklu oturum açma (SSO) özellikli abonelik için NS1 SSO.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Azure için NS1 SSO, SP ve ıDP tarafından başlatılan SSO 'yu destekler.
* Azure için NS1 SSO 'yu yapılandırdıktan sonra, oturum denetimini zorunlu kılabilirsiniz. Bu, kuruluşunuzun hassas verilerinin, gerçek zamanlı olarak ayıklanma ve içe taşmayı korur. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).


## <a name="add-ns1-sso-for-azure-from-the-gallery"></a>Galeriden Azure için NS1 SSO 'SU ekleme

Azure için NS1 SSO 'yu Azure AD 'ye tümleştirmeyi yapılandırmak için, Galeriden Azure için NS1 SSO 'SU ile yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Azure için ns1 SSO** yazın.
1. Sonuçlar panelinden **Azure Için ns1 SSO** ' yı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-ns1-sso-for-azure"></a>Azure için NS1 SSO için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure IÇIN ns1 SSO Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için bir Azure AD kullanıcısı ile ilgili Kullanıcı arasında Azure için NS1 SSO 'da bağlantılı bir ilişki kurun.

Azure için NS1 SSO ile Azure AD SSO 'yu yapılandırmak ve test etmek için genel adımlar şunlardır:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** .

    a. B. Simon ile Azure AD çoklu oturum açma sınamasını test etmek için **[bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** .

    b. Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek üzere **[Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user)** .
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Azure SSO IÇIN ns1 SSO 'Yu yapılandırın](#configure-ns1-sso-for-azure-sso)** .

    a. Azure **[test kullanıcısı için BIR ns1 SSO oluşturun](#create-an-ns1-sso-for-azure-test-user)** ve Azure IÇIN ns1 SSO 'da B. Simon 'a karşılık gelen bir. Bu karşılık gelen bu, kullanıcının Azure AD gösterimine bağlıdır.
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[test SSO 'su](#test-sso)** .

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Azure için ns1 SSO** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun. **Çoklu oturum açma**seçeneğini belirleyin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesini seçin.

   ![Kalem simgesi vurgulanmış şekilde, SAML ile çoklu oturum açmayı ayarlama ekran görüntüsü](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusuna aşağıdaki URL 'yi yazın:`https://api.nsone.net/saml/metadata`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanan bir URL yazın:`https://api.nsone.net/saml/sso/<ssoid>`

1. **Ek URL 'Ler ayarla**' yı seçin ve uygulamayı **SP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusuna aşağıdaki URL 'yi yazın:`https://my.nsone.net/#/login/sso`

    > [!NOTE]
    > Yanıt URL 'SI değeri gerçek değil. Yanıt URL 'SI değerini gerçek yanıt URL 'siyle güncelleştirin. Değeri almak için [Azure istemci desteği ekibine yönelik ns1 SSO](mailto:techops@nsone.net) ile iletişime geçin. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. Azure uygulaması için NS1 SSO, belirli bir biçimde SAML onayları bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini, uygulama tümleştirme sayfasındaki **Kullanıcı öznitelikleri & talepler** bölümünde yönetebilirsiniz. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **Kullanıcı öznitelikleri** iletişim kutusunu açmak için kurşun kalem simgesini seçin.

    ![Kalem simgesi vurgulanmış şekilde kullanıcı özniteliklerinin & talepler bölümünün ekran görüntüsü](./media/ns1-sso-for-azure-tutorial/attribute-edit-option.png)

1. Talebi düzenlemek için öznitelik adını seçin.

    ![Öznitelik adı vurgulanmış şekilde kullanıcı özniteliklerinin & talepler bölümünün ekran görüntüsü](./media/ns1-sso-for-azure-tutorial/attribute-claim-edit.png)

1. **Dönüşüm**' i seçin.

    ![Talep yönetme bölümünün, dönüşümle vurgulanan ekran görüntüsü](./media/ns1-sso-for-azure-tutorial/prefix-edit.png)

1. **Dönüştürmeyi Yönet** bölümünde aşağıdaki adımları uygulayın:

    ![Farklı alanlarla vurgulanmış şekilde dönüşümü yönetme bölümünün ekran görüntüsü](./media/ns1-sso-for-azure-tutorial/prefix-added.png)

    1. **Dönüşüm**olarak **ExactMailPrefix ()** seçeneğini belirleyin.

    1. **Parameter 1**olarak **User. UserPrincipalName** öğesini seçin.

    1. **Ekle**’yi seçin.

    1. **Kaydet**’i seçin.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde Kopyala düğmesini seçin. Bu, **uygulama Federasyon meta veri URL 'sini** kopyalar ve bilgisayarınıza kaydeder.

    ![SAML Imzalama sertifikasının, Kopyala düğmesi vurgulanmış şekilde ekran görüntüsü](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure Portal sol bölmeden, kullanıcılar **Azure Active Directory**  >  **Users**  >  **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:

   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** alanında gösterilen değeri yazın.
   1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Azure için NS1 SSO 'ya erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirin.

1. Azure Portal **Kurumsal uygulamalar**  >  **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Azure Için ns1 SSO**' yı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   ![Kullanıcılar ve gruplar vurgulanmış şekilde Yönet bölümünün ekran görüntüsü](common/users-groups-blade.png)

1. **Kullanıcı ekle**'yi seçin. **Atama Ekle** Iletişim kutusunda **Kullanıcılar ve gruplar**' ı seçin.

    ![Kullanıcılar ve gruplar sayfasının ekran görüntüsü, Kullanıcı Ekle vurgulanmış olarak](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** öğesini seçin. Ardından ekranın alt kısmındaki **Seç** düğmesini seçin.
1. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin. Ardından ekranın alt kısmındaki **Seç** düğmesini seçin.
1. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

## <a name="configure-ns1-sso-for-azure-sso"></a>Azure SSO için NS1 SSO 'yu yapılandırma

Azure için NS1 SSO 'da çoklu oturum açmayı yapılandırmak için, uygulama Federasyon meta veri URL 'sini [Azure destek ekibi Için ns1 SSO](mailto:techops@nsone.net)'ya göndermeniz gerekir. Bu ayar, SAML SSO bağlantısının her iki tarafında da düzgün bir şekilde ayarlanmasını sağlamak için bu ayarı yapılandırır.

### <a name="create-an-ns1-sso-for-azure-test-user"></a>Azure test kullanıcısı için bir NS1 SSO oluşturma

Bu bölümde, Azure için NS1 SSO 'da B. Simon adlı bir Kullanıcı oluşturacaksınız. Azure platformu için NS1 SSO 'daki kullanıcıları eklemek üzere Azure destek ekibi için NS1 SSO ile çalışın. Kullanıcıları oluşturup etkinleştirene kadar çoklu oturum açma kullanamazsınız.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim paneli ' ni kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Access panel 'de Azure için NS1 SSO kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız Azure için NS1 SSO 'da otomatik olarak oturum açmış olmanız gerekir. Daha fazla bilgi için bkz. [erişim paneli 'Ne giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Azure için NS1 SSO 'yu deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)