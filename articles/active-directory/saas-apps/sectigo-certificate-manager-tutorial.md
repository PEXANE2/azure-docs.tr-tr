---
title: 'Öğretici: Sectigo sertifika yöneticisiyle Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Sectıgo Sertifika Yöneticisi arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: jeedes
ms.openlocfilehash: c589db84b6221aa23868b3b49aea84f33623619f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92673865"
---
# <a name="tutorial-azure-active-directory-integration-with-sectigo-certificate-manager"></a>Öğretici: Sectigo sertifika yöneticisiyle Azure Active Directory tümleştirme

Bu öğreticide, Sectigo sertifika yöneticisini (SCM olarak da bilinir) Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.

Sectigo Sertifika Yöneticisi 'Ni Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'yi, Sectigo Sertifika Yöneticisi 'ne kimlerin erişebileceğini denetlemek için kullanabilirsiniz.
* Kullanıcılar, Azure AD hesaplarıyla (çoklu oturum açma) Sectigo Sertifika Yöneticisi 'nde otomatik olarak oturum açabilirler.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz Azure portal.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi için bkz. [Azure Active Directory uygulamalarda çoklu oturum açma](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini Sectıgo Sertifika Yöneticisi ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Azure AD aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Sectıgo sertifika yöneticisi hesabı.

> [!NOTE]
> Sectıgo, Sectigo Sertifika Yöneticisi 'nin birden çok örneğini çalıştırır. Sectıgo sertifika yöneticisinin ana örneği  **https: \/ /CERT-Manager.com** ve bu URL Bu öğreticide kullanılır.  Hesabınız farklı bir örnekten farklıysa, URL 'Leri uygun şekilde ayarlamanız gerekir.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edin ve Sectigo Sertifika Yöneticisi 'Ni Azure AD ile tümleştirin.

Sectıgo Sertifika Yöneticisi aşağıdaki özellikleri destekler:

* **SP tarafından başlatılan çoklu oturum açma**
* **IDP-başlatılan çoklu oturum açma**

## <a name="add-sectigo-certificate-manager-in-the-azure-portal"></a>Azure portal Sectıgo Sertifika Yöneticisi ekleme

Sectigo sertifika yöneticisini Azure AD ile tümleştirmek için, yönetilen SaaS uygulamaları listenize Sectıgo Sertifika Yöneticisi eklemeniz gerekir.

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Sol menüden **Azure Active Directory**' yi seçin.

    ![Azure Active Directory seçeneği](common/select-azuread.png)

1. **Kurumsal uygulamalar**  >  **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Bir uygulama eklemek için **Yeni uygulama**' yı seçin.

    ![Yeni uygulama seçeneği](common/add-new-app.png)

1. Arama kutusuna **Sectıgo Sertifika Yöneticisi** girin. Arama sonuçlarında, **Sectigo Sertifika Yöneticisi**' ni seçin ve ardından **Ekle**' yi seçin.

    ![Sonuçlar listesinde sectıgo Sertifika Yöneticisi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon** adlı bir test kullanıcısına bağlı olarak Sectigo Sertifika Yöneticisi Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz. Çoklu oturum açma 'nın çalışması için, bir Azure AD kullanıcısı ve Sectıgo Sertifika Yöneticisi 'nde ilgili Kullanıcı arasında bağlantılı bir ilişki kurmanız gerekir.

Azure AD çoklu oturum açma 'yı Sectigo Sertifika Yöneticisi ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamalısınız:

| Görev | Açıklama |
| --- | --- |
| **[Azure AD çoklu oturum açmayı yapılandırma](#configure-azure-ad-single-sign-on)** | Kullanıcılarınızın bu özelliği kullanmasına olanak sağlar. |
| **[Sectigo Sertifika Yöneticisi çoklu oturum açmayı yapılandırma](#configure-sectigo-certificate-manager-single-sign-on)** | Uygulamadaki çoklu oturum açma ayarlarını yapılandırır. |
| **[Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)** | Britta Simon adlı Kullanıcı için Azure AD çoklu oturum açmayı sınar. |
| **[Azure AD test kullanıcısını atama](#assign-the-azure-ad-test-user)** | Azure AD çoklu oturum açma kullanımı için Britta Simon 'U sağlar. |
| **[Sectıgo Sertifika Yöneticisi test kullanıcısı oluşturma](#create-a-sectigo-certificate-manager-test-user)** | Kullanıcının Azure AD gösterimine bağlı olan Sectigo Sertifika Yöneticisi 'nde Britta Simon 'ın bir karşılığı oluşturur. |
| **[Çoklu oturum açma testi](#test-single-sign-on)** | Yapılandırmanın çalıştığını doğrular. |

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde Azure AD çoklu oturum açma 'yı Azure portal Sectıgo Sertifika Yöneticisi ile yapılandırırsınız.

1. [Azure Portal](https://portal.azure.com/), **Sectıgo Sertifika Yöneticisi** uygulama tümleştirme bölmesinde **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma seçeneğini yapılandırma](common/select-sso.png)

1. Çoklu oturum **açma yöntemi seçin** bölmesinde, çoklu oturum açmayı etkinleştirmek için **SAML** veya **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

1. **SAML Ile tek Sign-On ayarla** bölmesinde, **temel SAML yapılandırması** bölmesini açmak için **Düzenle** ' yi (kurşun kalem simgesi) seçin.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları izleyin:

    1. **Tanımlayıcı (VARLıK kimliği)** kutusunda, ana Sectıgo Sertifika Yöneticisi örneği için **https: \/ /CERT-Manager.com/Shibboleth** girin.

    1. **Yanıt URL 'si** kutusunda, ana Sectıgo Sertifika Yöneticisi örneği için **https: \/ /CERT-Manager.com/Shibboleth.SSO/SAML2/Post** girin.
        
    > [!NOTE]
    > Genel olarak, **oturum açma URL 'Si** *SP tarafından başlatılan mod* için zorunludur, bu da Sectigo Sertifika Yöneticisi ' nden oturum açması gerekmez.        

1. İsteğe bağlı olarak, *IDP tarafından başlatılan modu* yapılandırmak ve **testin** çalışmasına ızın vermek için **temel SAML yapılandırma** bölümünde aşağıdaki adımları uygulayın:

    1. **Ek URL 'Ler ayarla**' yı seçin.

    1. **Geçiş durumu** kutusunda, Sectigo Sertifika Yöneticisi MÜŞTERIYE özgü URL 'nizi girin. Ana Sectıgo Sertifika Yöneticisi örneği için **https: \/ /CERT-Manager.com/customer/ \<customerURI\> /IDP** girin.

    ![Sectıgo sertifika yöneticisi etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-relay.png)

1. **Kullanıcı öznitelikleri & talepler** bölümünde aşağıdaki adımları uygulayın:

    1. Tüm **ek talepleri** silin.
    
    1. **Yeni talep Ekle** ' yi seçin ve aşağıdaki dört talebi ekleyin:
    
        | Name | Ad Alanı | Kaynak | Kaynak özniteliği | Description |
        | --- | --- | --- | --- | --- |
        | eduPersonPrincipalName | empty | Öznitelik | User. UserPrincipalName | Yöneticiler için Sectıgo Sertifika Yöneticisi içindeki **IDP kışı kimliği** alanıyla eşleşmelidir. |
        | posta | empty | Öznitelik | Kullanıcı. Mail | Gerekli |
        | givenName | empty | Öznitelik | Kullanıcı. | İsteğe Bağlı |
        | sn | empty | Öznitelik | User. soyadı | İsteğe Bağlı |

       ![Sectıgo Sertifika Yöneticisi-dört yeni talep ekleyin](media/sectigo-certificate-manager-tutorial/additional-claims.png)

1. **SAML Imzalama sertifikası** bölümünde, **Federasyon meta verileri XML**' nün yanındaki **İndir** ' i seçin. XML dosyasını bilgisayarınıza kaydedin.

    ![Federasyon meta verileri XML indirme seçeneği](common/metadataxml.png)

### <a name="configure-sectigo-certificate-manager-single-sign-on"></a>Sectigo Sertifika Yöneticisi çoklu oturum açmayı yapılandırma

Sectıgo Sertifika Yöneticisi tarafında çoklu oturum açmayı yapılandırmak için, indirilen Federasyon meta verileri XML dosyasını [Sectıgo Sertifika Yöneticisi destek ekibine](https://sectigo.com/support)gönderin. Sectıgo Sertifika Yöneticisi destek ekibi, SAML çoklu oturum açma bağlantısının her iki tarafında da doğru şekilde ayarlandığından emin olmak için, bunları göndereceğiniz bilgileri kullanır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümde, Azure portal Britta Simon adlı bir test kullanıcısı oluşturacaksınız.

1. Azure Portal, kullanıcılar **Azure Active Directory**  >    >  **tüm kullanıcılar**' ı seçin.

    ![Kullanıcılar ve tüm kullanıcılar seçenekleri](common/users.png)

1. **Yeni Kullanıcı**' yı seçin.

    ![Yeni Kullanıcı seçeneği](common/new-user.png)

1. **Kullanıcı** bölmesinde, aşağıdaki adımları izleyin:

    1. **Ad** kutusuna **Brittasıon** yazın.
  
    1. **Kullanıcı adı** kutusuna **brittasıon yazın \@ \<your-company-domain> . \<extension\>** Örneğin, **brittasıon \@ contoso.com**.

    1. **Parolayı göster** onay kutusunu seçin. **Parola** kutusunda görüntülenen değeri kaydedin.

    1. **Oluştur**’u seçin.

    ![Kullanıcı bölmesi](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, kullanıcının Azure çoklu oturum açmayı kullanabilmesi için Sectıgo Sertifika Yöneticisi 'ne giderek Britta Simon erişimi verirsiniz.

1. Azure Portal **Kurumsal uygulamalar**  >  **tüm uygulamalar**  >  **sectıgo Sertifika Yöneticisi**' ni seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Uygulamalar listesinde, **Sectigo Sertifika Yöneticisi**' ni seçin.

    ![Uygulamalar listesinde sectıgo Sertifika Yöneticisi](common/all-applications.png)

1. Menüsünde **Kullanıcılar ve gruplar**' ı seçin.

    ![Kullanıcılar ve gruplar seçeneği](common/users-groups-blade.png)

1. **Kullanıcı ekle**'yi seçin. Sonra **atama Ekle** bölmesinde **Kullanıcılar ve gruplar**' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** bölmesinde, Kullanıcı listesinde **Britta Simon** ' u seçin. **Seç**’i seçin.

1. SAML assertion 'da bir rol değeri bekliyorsanız, **Rol Seç** bölmesinde, kullanıcının listeden ilgili rolünü seçin. **Seç**’i seçin.

1. **Atama Ekle** bölmesinde **ata**' yı seçin.

### <a name="create-a-sectigo-certificate-manager-test-user"></a>Sectıgo Sertifika Yöneticisi test kullanıcısı oluşturma

Bu bölümde, Sectigo Sertifika Yöneticisi 'nde Britta Simon adlı bir Kullanıcı oluşturacaksınız. Kullanıcıları Sectigo Sertifika Yöneticisi platformuna eklemek için [Sectigo Sertifika Yöneticisi destek](https://sectigo.com/support) ekibiyle birlikte çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

#### <a name="test-from-sectigo-certificate-manager-sp-initiated-single-sign-on"></a>Sectigo Sertifika Yöneticisi 'nden test (SP tarafından başlatılan çoklu oturum açma)

Müşterinizin özel URL 'sine (ana Sectıgo Sertifika Yöneticisi örneği, https: \/ /CERT-Manager.com/Customer//) gidin \<customerURI\> ve aşağıdaki düğmeyi seçin **ya da ile oturum açın**.  Doğru yapılandırılmışsa, Sectigo Sertifika Yöneticisi 'nde otomatik olarak oturumunuz açılır.

#### <a name="test-from-azure-single-sign-on-configuration-idp-initiated-single-sign-on"></a>Azure çoklu oturum açma yapılandırmasından test etme (ıDP tarafından başlatılan çoklu oturum açma)

**Sectıgo Sertifika Yöneticisi** uygulama tümleştirme bölmesinde, **Çoklu oturum açma** ' yı seçin ve **Test** düğmesini seçin.  Doğru yapılandırılmışsa, Sectigo Sertifika Yöneticisi 'nde otomatik olarak oturumunuz açılır.

#### <a name="test-by-using-the-my-apps-portal-idp-initiated-single-sign-on"></a>My Apps portalını kullanarak test etme (ıDP-başlatılan çoklu oturum açma)

Uygulamalarım portalındaki **Sectıgo Sertifika Yöneticisi** ' ni seçin.  Doğru yapılandırılmışsa, Sectigo Sertifika Yöneticisi 'nde otomatik olarak oturumunuz açılır. Uygulamalarım portalı hakkında daha fazla bilgi için bkz. [Apps Portalındaki uygulamalara erişme ve bunları kullanma](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinmek için şu makaleleri gözden geçirin:

- [SaaS uygulamalarını Azure Active Directory tümleştirme öğreticilerinin listesi](./tutorial-list.md)
- [Azure Active Directory uygulamalarda çoklu oturum açma](../manage-apps/what-is-single-sign-on.md)
- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)