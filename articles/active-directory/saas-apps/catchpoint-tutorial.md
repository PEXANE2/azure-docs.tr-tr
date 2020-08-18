---
title: 'Öğretici: yakalama noktasıyla çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory'
description: Azure Active Directory ve catch noktası arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/27/2020
ms.author: jeedes
ms.openlocfilehash: 649396b81402e9229eb9ea2c627b60f249f8c601
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88530316"
---
# <a name="tutorial-azure-active-directory-single-sign-on-integration-with-catchpoint"></a>Öğretici: Catch noktasıyla çoklu oturum açma tümleştirmesi Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile catch noktasını tümleştirmeyi öğreneceksiniz. Catch noktasını Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'den bir catch noktasına Kullanıcı erişimini denetleyin.
* Azure AD hesapları olan kullanıcılar için otomatik catch noktası oturum açma özelliğini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Çoklu oturum açma (SSO) etkin olan bir catch noktası aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Catch noktası SP tarafından başlatılan ve ıDP tarafından başlatılan SSO 'yu destekler.
* Catch noktası tam zamanında (JıT) Kullanıcı sağlamayı destekler.
* Catch noktasını yapılandırdıktan sonra, oturum denetimini zorunlu kılabilirsiniz. Bu önlem, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanma ve savunma sürecinde koruma sağlar. Oturum denetimi, Koşullu erişimin bir uzantısıdır. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="add-catchpoint-from-the-gallery"></a>Galeriden catch noktası ekleme

Catch noktasının tümleştirmesini Azure AD 'ye göre yapılandırmak için, yönetilen SaaS uygulamaları listenize catch noktası ekleyin.

1. İş, okul veya kişisel Microsoft hesabı [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol bölmede **Azure Active Directory** hizmeti seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **catch Point** yazın.
1. Sonuçlar panelinden **catch noktası** ' nı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-catchpoint"></a>Catch noktası için Azure AD çoklu oturum açmayı yapılandırma ve test etme

SSO 'nun çalışması için, catch noktasındaki bir kullanıcıyla bir Azure AD kullanıcısını bağlamanız gerekir. Bu öğreticide, **B. Simon**adlı bir test kullanıcısı yapılandıracağız. 

Aşağıdaki bölümleri doldurun:

1. Kullanıcılarınıza yönelik bu özelliği etkinleştirmek için [Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso).
    * B. Simon ile Azure AD çoklu oturum açma 'yı test etmek için [bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user).
    * Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek üzere [Azure AD test kullanıcısını atayın](#assign-the-azure-ad-test-user).
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için [catch noktası SSO 'Yu yapılandırın](#configure-catchpoint-sso).
    * B. Simon Azure AD sınama hesabının, catch noktasındaki benzer bir kullanıcı hesabına bağlanmasına izin vermek için [catch noktası test kullanıcısı oluşturun](#create-a-catchpoint-test-user).
1. Yapılandırmanın çalıştığını doğrulamak için [test SSO 'su](#test-sso).

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure AD SSO 'yu etkinleştirmek için Azure portal aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. **Catch noktası** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırma** ayarlarını düzenlemek için kalem simgesini seçin.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. Yakalama noktası için başlatma modunu yapılandırın:
   - **IDP**tarafından başlatılan mod için aşağıdaki alanlar için değerleri girin:
     - **Tanımlayıcı**için:`https://portal.catchpoint.com/SAML2`
     - **Yanıt URL 'si**için:`https://portal.catchpoint.com/ui/Entry/SingleSignOn.aspx`
   - **SP**tarafından başlatılan mod Için **ek URL 'ler ayarla** ' yı seçin ve aşağıdaki değeri girin:
     - **Oturum açma URL 'si**için:`https://portal.catchpoint.com/ui/Entry/SingleSignOn.aspx`

1. Catch noktası uygulaması, SAML onaylamalarını belirli bir biçimde bekler. SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri ekleyin. Aşağıdaki tabloda varsayılan özniteliklerin listesi yer almaktadır:

    | Adı | Kaynak özniteliği|
    | ------------ | --------- |
    | GivenName | User. givenneame |
    | Soyadı | User. soyadı |
    | EmailAddress | Kullanıcı. Mail |
    | Adı | User. UserPrincipalName |
    | Benzersiz kullanıcı tanımlayıcısı | User. UserPrincipalName |

    ![Kullanıcı öznitelikleri & talep listesi ekran görüntüsü](common/default-attributes.png)

1. Ayrıca, catch noktası uygulaması başka bir özniteliğin SAML yanıtına geçirilmesini bekler. Aşağıdaki tabloya bakın. Bu öznitelik de önceden doldurulur, ancak gereksinimlerinize uyacak şekilde gözden geçirebilir ve güncelleştirebilirsiniz.

    | Adı | Kaynak özniteliği|
    | ------------ | --------- |
    | ad alanı | Kullanıcı. atanan |

    > [!NOTE]
    > `namespace`Talebin hesap adıyla eşlenmesi gerekir. Bu hesap adı, bir Azure AD 'de SAML yanıtına geri geçirilecek bir rolle ayarlanmalıdır. Azure AD 'deki roller hakkında daha fazla bilgi için bkz. [Kurumsal uygulamalar IÇIN SAML belirtecinde verilen rol talebini yapılandırma](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management).

1. **SAML Ile çoklu oturum açmayı ayarlama** sayfasına gidin. **SAML Imzalama sertifikası** bölümünde, **sertifika (base64)** bulun. Sertifikayı bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Catch noktasını ayarla** bölümünde, daha sonraki bir adımda Ihtiyacınız olan URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı bir Azure AD test kullanıcısı oluşturmak için Azure portal kullanırsınız.

1. Azure Portal sol bölmeden, kullanıcılar **Azure Active Directory**  >  **Users**  >  **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com` girin.
   1. **Parolayı göster** onay kutusunu seçin. Görünen parola değerini aklınızda edin.
   1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, catch noktasına erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirirsiniz.

1. Azure Portal **Kurumsal uygulamalar**  >  **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **catch noktası**' nı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin ve sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    !["Kullanıcı Ekle" bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, Kullanıcı listesinden **B. Simon** öğesini seçin. Ekranın alt kısmındaki **Seç** ' e tıklayın.
1. SAML onaylama 'da bir rol değeri bekleliyorsanız, **Rol Seç** iletişim kutusuna bakın ve listeden kullanıcının rolünü seçin. Ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

## <a name="configure-catchpoint-sso"></a>Catch noktası SSO 'SU yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, catch noktası uygulamasında Yönetici olarak oturum açın.

1. **Ayarlar** simgesini ve ardından **SSO kimlik sağlayıcısını**seçin.

    ![SSO kimlik sağlayıcısı seçiliyken catch noktası ayarları ekran görüntüsü](./media/catchpoint-tutorial/configuration1.png)

1. **Çoklu oturum açma** sayfasında, aşağıdaki alanları girin:

   ![Catch noktası çoklu oturum açma sayfası ekran görüntüsü](./media/catchpoint-tutorial/configuration2.png)

   Alan | Değer
   ----- | ----- 
   **Ad Alanı** | Geçerli bir ad alanı değeri.
   **Kimlik sağlayıcısı veren** | `Azure AD Identifier`Azure Portal değeri.
   **Çoklu oturum açma URL 'Si** | `Login URL`Azure Portal değeri.
   **Sertifika** | `Certificate (Base64)`Azure Portal indirilen dosyanın içeriği. Görüntülemek ve kopyalamak için Not defteri 'ni kullanın.

   Ayrıca, **meta verileri karşıya yükle** seçeneğini belirleyerek **Federasyon meta veri XML** 'sini de karşıya yükleyebilirsiniz.

1. **Kaydet**’i seçin.

### <a name="create-a-catchpoint-test-user"></a>Catch noktası test kullanıcısı oluşturma

Yakalama noktası, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde eylem öğesi yok. B. Simon, catch noktasındaki bir kullanıcı olarak zaten mevcut değilse, kimlik doğrulamasından sonra oluşturulur.

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı My Apps portalını kullanarak test edersiniz.

Uygulamalarım portalındaki catch noktası kutucuğunu seçtiğinizde, otomatik olarak, catch noktası uygulamasında SSO yapılandırılmış olarak oturum açmış olmanız gerekir. Uygulamalarım portalı hakkında daha fazla bilgi için bkz. [uygulamalarım portalından oturum açma ve uygulamaları başlatma](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access).

> [!NOTE]
> Catch noktası uygulamasında oturum açma sayfasından oturum açtığınızda, **catch noktası kimlik bilgilerini**sağladıktan sonra geçerli **ad alanı** değerini **Şirket kimlik bilgileri (SSO)** alanına girin ve **oturum aç**' ı seçin.
> 
> ![Catch noktası yapılandırması](./media/catchpoint-tutorial/loginimage.png)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile catch noktasını deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
