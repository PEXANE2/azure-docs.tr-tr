---
title: 'Öğretici: Zscaler Internet erişimi yöneticisiyle Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Zscaler Internet erişimi Yöneticisi arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/25/2021
ms.author: jeedes
ms.openlocfilehash: 70afa0a02f4e303105aec1884b966796854c6f49
ms.sourcegitcommit: f6193c2c6ce3b4db379c3f474fdbb40c6585553b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102449332"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-internet-access-administrator"></a>Öğretici: Zscaler Internet erişim yöneticisiyle Azure Active Directory tümleştirme

Bu öğreticide, Zscaler Internet erişim yöneticisini Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Zscaler Internet erişimi yöneticisini Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Zscaler Internet erişim yöneticisine erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Zscaler Internet erişim yöneticisine otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Zscaler Internet Access Administrator çoklu oturum açma (SSO) aboneliği etkin.

> [!NOTE]
> Bu tümleştirme Ayrıca Azure AD ABD kamu bulut ortamından kullanılabilir. Bu uygulamayı Azure AD ABD kamu bulutu uygulama galerisinde bulabilir ve bunu ortak buluttan yaptığınız şekilde yapılandırabilirsiniz.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Zscaler Internet Access Administrator, **IDP** tarafından başlatılan SSO 'yu destekler.

## <a name="add-zscaler-internet-access-administrator-from-the-gallery"></a>Galeriden Zscaler Internet erişimi Yöneticisi ekleme

Zscaler Internet Access Administrator 'ın Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden Zscaler Internet Erişim Yöneticisi ' ni, Galeri 'den yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Zscaler Internet Erişim Yöneticisi** yazın.
1. Sonuçlar panelinden **Zscaler Internet erişimi Yöneticisi** ' ni seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-zscaler-internet-access-administrator"></a>Zscaler Internet erişimi Yöneticisi için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak Zscaler Internet erişim YÖNETICISIYLE Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, Zscaler Internet Erişim Yöneticisi ' nde bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Zscaler Internet erişimi yöneticisiyle yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
2. **[Zscaler Internet erişim YÖNETICISI SSO 'Yu yapılandırma](#configure-zscaler-internet-access-administrator-sso)** -uygulama tarafında tek Sign-On ayarlarını yapılandırmak için.
    1. **[Zscaler Internet Access Administrator test kullanıcısı oluşturun](#create-zscaler-internet-access-administrator-test-user)** -Zscaler Internet Erişim Yöneticisi 'Nde kullanıcının Azure AD gösterimine bağlı Britta Simon 'a sahip olmak için.
3. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **Zscaler Internet Access Administrator** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML Ile tek Sign-On ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusunda, gereksiniminize göre aşağıdaki URL 'lerden birini yazın:

    | Tanımlayıcı |
    |------------|
    | `https://admin.zscaler.net` |
    | `https://admin.zscalerone.net` |
    | `https://admin.zscalertwo.net` |
    | `https://admin.zscalerthree.net` |
    | `https://admin.zscloud.net` |
    | `https://admin.zscalerbeta.net` |

    b. **Yanıt URL 'si** metin kutusuna, gereksiniminize göre aşağıdaki URL 'lerden birini yazın:

    | Yanıt URL'si |
    |-----------|
    | `https://admin.zscaler.net/adminsso.do` |
    | `https://admin.zscalerone.net/adminsso.do` |
    | `https://admin.zscalertwo.net/adminsso.do` |
    | `https://admin.zscalerthree.net/adminsso.do` |
    | `https://admin.zscloud.net/adminsso.do` |
    | `https://admin.zscalerbeta.net/adminsso.do` |

5. Zscaler Internet erişimi yönetici uygulaması, belirli bir biçimde SAML onayları bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini, uygulama tümleştirme sayfasındaki **Kullanıcı öznitelikleri & talepler** bölümünde yönetebilirsiniz. **SAML Ile tek Sign-On ayarlama sayfasında**, **Kullanıcı öznitelikleri & talepler** Iletişim kutusunu açmak için **Düzenle** düğmesine tıklayın.

    ![Öznitelik bağlantısı](./media/zscaler-internet-access-administrator-tutorial/attributes.png)

6. **Kullanıcı öznitelikleri** Iletişim kutusundaki **Kullanıcı talepleri** bölümünde, YUKARıDAKI görüntüde gösterildiği gibi SAML belirteci özniteliğini yapılandırın ve aşağıdaki adımları gerçekleştirin:

    | Name  | Kaynak özniteliği  |
    | ---------| ------------ |
    | Rol | Kullanıcı. atandroles |

    a. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' ye tıklayın.

    b. **Kaynak özniteliği** listesinden öznitelik değerini seçin.

    c. **Tamam**’a tıklayın.

    d. **Kaydet**’e tıklayın.

    > [!NOTE]
    > Azure AD 'de rolün nasıl yapılandırılacağını öğrenmek için lütfen [buraya](../develop/howto-add-app-roles-in-azure-ad-apps.md#app-roles-ui--preview) tıklayın.

7. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

8. **Zscaler Internet erişimi yöneticisini ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

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

Bu bölümde, Zscaler Internet erişim yöneticisine erişim vererek, B. Simon 'u Azure çoklu oturum açma özelliğini kullanacak şekilde etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Zscaler Internet Erişim Yöneticisi**' ni seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Rolleri yukarıda açıklanan şekilde ayarlarsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-zscaler-internet-access-administrator-sso"></a>Zscaler Internet erişimi yönetici SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, Zscaler Internet Access Yönetici Kullanıcı arabiriminize oturum açın.

2. **Yönetim > Yönetici Yönetimi** ' ne gidin ve aşağıdaki adımları gerçekleştirin ve Kaydet ' e tıklayın:

    ![Ekran görüntüsü, SAML kimlik doğrulamasını etkinleştirme, S S L sertifikasını karşıya yükleme ve bir veren belirleme seçenekleriyle yönetici yönetimini gösterir.](./media/zscaler-internet-access-administrator-tutorial/management.png "Yönetim")

    a. **SAML kimlik doğrulamasını etkinleştir**' i işaretleyin.

    b. **Ortak SSL sertifikasındaki** Azure Portal adresinden INDIRDIĞINIZ Azure SAML imzalama sertifikasını karşıya yüklemek Için **karşıya yükle**' ye tıklayın.

    c. İsteğe bağlı olarak, ek güvenlik için, SAML yanıtının vereni doğrulamak üzere **veren** ayrıntılarını ekleyin.

3. Yönetici Kullanıcı arabiriminde aşağıdaki adımları gerçekleştirin:

    ![Ekran görüntüsü, adımları gerçekleştirebileceğiniz Yönetici U 'yi gösterir.](./media/zscaler-internet-access-administrator-tutorial/activation.png)

    a. Sol alt kısımdaki **etkinleştirme** menüsünün üzerine gelin.

    b. **Etkinleştir**' e tıklayın.

### <a name="create-zscaler-internet-access-administrator-test-user"></a>Zscaler Internet erişimi yönetici test kullanıcısı oluşturma

Bu bölümün amacı, Zscaler Internet Access Administrator 'da Britta Simon adlı bir Kullanıcı oluşturmaktır. Zscaler Internet erişimi, yönetici SSO 'SU için tam zamanında sağlamayı desteklemez. Yönetici hesabını el ile oluşturmanız gerekir.
Yönetici hesabı oluşturma adımları için, Zscaler belgelerine bakın:

https://help.zscaler.com/zia/adding-admins

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz.

* Azure portal bu uygulamayı test et ' e tıklayın ve SSO 'yu ayarladığınız Zscaler Internet Erişim Yöneticisi 'nde otomatik olarak oturum açmış olmanız gerekir

* Microsoft My Apps ' i kullanabilirsiniz. Uygulamalarım içindeki Zscaler Internet erişimi Yöneticisi kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Zscaler Internet Erişim Yöneticisi 'nde otomatik olarak oturum açmış olmanız gerekir. Uygulamalarım hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

Zscaler Internet erişimi Yöneticisi 'ni yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).
