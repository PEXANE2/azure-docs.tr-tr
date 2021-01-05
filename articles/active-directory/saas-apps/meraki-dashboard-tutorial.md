---
title: 'Öğretici: Meraki panosu ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Meraki panosu arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/28/2020
ms.author: jeedes
ms.openlocfilehash: 0c43e085725ceb5db718c9b4c9c9d83787964d63
ms.sourcegitcommit: 1140ff2b0424633e6e10797f6654359947038b8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/30/2020
ms.locfileid: "97813539"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-meraki-dashboard"></a>Öğretici: Meraki panosu ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Meraki panosunu Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Meraki panosunu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Meraki panosuna erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Meraki panosuna otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Meraki panosu çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Meraki panosu **IDP** tarafından başlatılan SSO 'yu destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı, tek bir kiracıda yalnızca bir örneğin yapılandırılabilmesini sağlamak için sabit bir dize değeridir.

## <a name="adding-meraki-dashboard-from-the-gallery"></a>Galeriden Meraki panosu ekleniyor

Meraki panosunun Azure AD ile tümleştirilmesini yapılandırmak için, galerinizden yönetilen SaaS uygulamaları listenize Meraki panosu eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Meraki panosu** yazın.
1. Sonuçlar panelinden **Meraki panosunu** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-meraki-dashboard"></a>Meraki panosu için Azure AD SSO 'Yu yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak Azure AD SSO Ile Meraki panosunu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve Meraki panosundaki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'Yu Meraki panosu ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Meraki Pano SSO 'Yu yapılandırın](#configure-meraki-dashboard-sso)** .
    1. Kullanıcı Azure AD gösterimine bağlı olan Meraki panosunda B. Simon 'ya karşılık gelen bir buna sahip olmak için **[Meraki Panosu test kullanıcısı oluşturun](#create-meraki-dashboard-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **Meraki panosu** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:
     
    **Yanıt URL** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://n27.meraki.com/saml/login/m9ZEgb/< UNIQUE ID >`

    > [!NOTE]
    > Yanıt URL 'SI değeri gerçek değil. Bu değeri, Öğreticinin ilerleyen kısımlarında açıklanan gerçek yanıt URL 'SI değeriyle güncelleştirin.

1. **Kaydet** düğmesine tıklayın.

1. Meraki pano uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/default-attributes.png)

1. Meraki pano uygulaması, yukarılarına ek olarak aşağıda gösterilen SAML yanıtına daha fazla öznitelik geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.
    
    | Ad | Kaynak özniteliği|
    | ---------------| --------- |
    | `https://dashboard.meraki.com/saml/attributes/username` | User. UserPrincipalName |
    | `https://dashboard.meraki.com/saml/attributes/role` | Kullanıcı. atandroles |

    > [!NOTE]
    > Azure AD 'de rolleri nasıl yapılandıracağınızı anlamak için [buraya](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps#app-roles-ui)bakın.

1. **SAML Imzalama sertifikası** bölümünde, **SAML imzalama sertifikası** Iletişim kutusunu açmak için **Düzenle** düğmesine tıklayın.

    ![SAML Imzalama sertifikasını Düzenle](common/edit-certificate.png)

1. **SAML Imzalama sertifikası** bölümünde, **parmak izi değerini** kopyalayın ve bilgisayarınıza kaydedin.

    ![Parmak Izi değerini Kopyala](common/copy-thumbprint.png)

1. **Meraki ayarlama Pano** bölümünde, oturum kapatma URL 'si değerini kopyalayın ve bilgisayarınıza kaydedin.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Meraki panosuna erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Meraki panosu**' nu seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.

    ![kullanıcı rolü](./media/meraki-dashboard-tutorial/user-role.png)

    > [!NOTE]
    > **Rol Seç** seçeneği devre dışı bırakılır ve varsayılan rol seçili kullanıcı için Kullanıcı olur.

1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-meraki-dashboard-sso"></a>Meraki Pano SSO 'Yu yapılandırma

1. Meraki panosundaki yapılandırmayı otomatik hale getirmek için, **uzantıyı yüklemek** üzere **uygulamalar güvenli oturum açma tarayıcı uzantısı** ' nı yüklemeniz gerekir.

    ![Uygulamalarım uzantısı](common/install-myappssecure-extension.png)

2. Tarayıcıya Uzantı eklendikten sonra **Meraki panosunu ayarla** öğesine tıkladığınızda sizi Meraki Pano uygulamasına yönlendirirsiniz. Buradan, Meraki panosunda oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı, uygulamayı sizin için otomatik olarak yapılandırır ve 3-7 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Meraki panosunu el ile ayarlamak istiyorsanız, farklı bir Web tarayıcısı penceresinde, Meraki panosu şirket sitenizde yönetici olarak oturum açın.

1. **Kuruluş**  ->  **ayarları**' na gidin.

    ![Meraki Pano ayarları sekmesi](./media/meraki-dashboard-tutorial/configure-1.png)

1. Kimlik doğrulaması altında **SAML SSO** 'YU **SAML SSO 'yu etkin** olarak değiştirin.

    ![Meraki Pano kimlik doğrulaması](./media/meraki-dashboard-tutorial/configure-2.png)

1. **SAML IDP Ekle**' ye tıklayın.

    ![Meraki panosu SAML IDP ekleme](./media/meraki-dashboard-tutorial/configure-3.png)

1. Azure portal **X. 590 CERT SHA1 parmak izi** metin kutusuna kopyaladığınız **parmak izi** değerini yapıştırın. Daha sonra **Kaydet**'e tıklayın. Kaydettikten sonra tüketici URL 'SI görünür. Tüketici URL 'SI değerini kopyalayın ve Azure portal **temel SAML yapılandırması bölümünde** bunu **yanıt URL 'si** metin kutusuna yapıştırın.

    ![Meraki Pano yapılandırması](./media/meraki-dashboard-tutorial/configure-4.png)

### <a name="create-meraki-dashboard-test-user"></a>Meraki Panosu test kullanıcısı oluştur

1. Farklı bir Web tarayıcısı penceresinde, Meraki panosunda yönetici olarak oturum açın.

1. **Kuruluş**  ->  **yöneticileri**' ne gidin.

    ![Meraki Pano yöneticileri](./media/meraki-dashboard-tutorial/user-1.png)

1. SAML yöneticisi rolleri bölümünde **SAML rolü Ekle** düğmesine tıklayın.

    ![Meraki panosu SAML rolü Ekle düğmesi](./media/meraki-dashboard-tutorial/user-2.png)

1. Rol **meraki_full_admin** girin, **kuruluş erişimini** **dolu** olarak işaretleyin ve **rol oluştur**' a tıklayın. **Meraki_readonly_admin** işlemi yineleyin, bu kez **kuruluş erişimini** **salt okuma** kutusu olarak işaretler.
 
    ![Meraki Panosu kullanıcı oluştur](./media/meraki-dashboard-tutorial/user-3.png)

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz.

* Azure portal bu uygulamayı test et ' e tıklayın ve SSO 'Yu ayarladığınız Meraki panosunda otomatik olarak oturum açmış olmanız gerekir

* Microsoft My Apps ' i kullanabilirsiniz. Uygulamalarım içindeki Meraki pano kutucuğuna tıkladığınızda, SSO 'Yu ayarladığınız Meraki panosunda otomatik olarak oturum açmış olmanız gerekir. Uygulamalarım hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).


## <a name="next-steps"></a>Sonraki adımlar

Meraki panosunu yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).