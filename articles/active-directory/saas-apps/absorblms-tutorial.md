---
title: 'Öğretici: artışlarını devralarak LMS ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve artışlarını devralarak LMS arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/05/2021
ms.author: jeedes
ms.openlocfilehash: 5e962592779494e1d60d03e9e8a167d53ac8bda2
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100364179"
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Öğretici: artışlarını devralarak LMS ile tümleştirme Azure Active Directory

Bu öğreticide, artışlarını devralarak LMS 'yi Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Artışlarını devralarak LMS 'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de artışlarını devralarak LMS erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla artışlarını devralarak LMS 'ye otomatik olarak kaydolmalarına imkan tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini artışlarını devralarak LMS ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Artışlarını devralarak LMS çoklu oturum açma etkin aboneliği.

> [!NOTE]
> Bu tümleştirme Ayrıca Azure AD ABD kamu bulut ortamından kullanılabilir. Bu uygulamayı Azure AD ABD kamu bulutu uygulama galerisinde bulabilir ve bunu ortak buluttan yaptığınız şekilde yapılandırabilirsiniz.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Artışlarını devralarak LMS, **IDP** tarafından başlatılan SSO 'yu destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı, tek bir kiracıda yalnızca bir örneğin yapılandırılabilmesini sağlamak için sabit bir dize değeridir.

## <a name="add-absorb-lms-from-the-gallery"></a>Galeriden artışlarını devralarak LMS ekleme

Artışlarını devralarak LMS 'nin tümleştirmesini Azure AD 'ye göre yapılandırmak için galerideki artışlarını devralarak LMS 'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **artışlarını devralarak LMS** yazın.
1. Sonuçlar panelinden **ARTıŞLARıNı DEVRALARAK LMS** ' yi seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-absorb-lms"></a>Artışlarını devralarak LMS için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak Azure AD SSO 'YU artışlarını devralarak LMS ile yapılandırın ve test edin. SSO 'nun çalışması için, artışlarını devralarak LMS 'de bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu artışlarını devralarak LMS ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[ARTıŞLARıNı DEVRALARAK LMS SSO 'Yu yapılandırın](#configure-absorb-lms-sso)** .
    1. User 'ın Azure AD gösterimine bağlı olan artışlarını devralarak LMS 'de B. Simon 'ya karşılık gelen bir **[ARTıŞLARıNı DEVRALARAK LMS test kullanıcısı oluşturun](#create-absorb-lms-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **ARTıŞLARıNı DEVRALARAK LMS** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **SAML Ile tek Sign-On ayarla** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** düğmesine tıklayın.

    **Artışlarını devralarak 5 kullanıyorsanız-Kullanıcı arabirimi** aşağıdaki yapılandırmayı kullanın:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın::`https://<SUBDOMAIN>.myabsorb.com/account/saml`

    b. **Yanıt URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın::`https://<SUBDOMAIN>.myabsorb.com/account/saml`

    **Artışlarını devralarak 5 kullanıyorsanız-yeni Learner deneyimi** aşağıdaki yapılandırmayı kullanın:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<SUBDOMAIN>.myabsorb.com/api/rest/v2/authentication/saml`

    b. **Yanıt URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın::`https://<SUBDOMAIN>.myabsorb.com/api/rest/v2/authentication/saml`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [ARTıŞLARıNı DEVRALARAK LMS istemci desteği ekibine](https://support.absorblms.com/hc/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. Aşağıdaki ekran görüntüsünde, **NameIdentifier** 'ın **User. UserPrincipalName** ile eşlendiği varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/edit-attribute.png)

6. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **ARTıŞLARıNı DEVRALARAK LMS 'Yi ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

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

Bu bölümde, artışlarını devralarak LMS 'ye erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **ARTıŞLARıNı DEVRALARAK LMS**' yi seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-absorb-lms-sso"></a>Artışlarını devralarak LMS SSO 'yu yapılandırma

1. Yeni bir Web tarayıcısı penceresinde, artışlarını devralarak LMS şirket sitenizde yönetici olarak oturum açın.

2. Sağ üst taraftaki **Hesap** düğmesini seçin.

    ![Hesap düğmesi](./media/absorblms-tutorial/account.png)

3. Hesap bölmesinde **Portal Ayarları**' nı seçin.

    ![Portal ayarları bağlantısı](./media/absorblms-tutorial/portal.png)

4. **SSO ayarlarını yönet** sekmesini seçin.

    ![Kullanıcılar sekmesi](./media/absorblms-tutorial/sso.png)

5. **Tek Sign-On ayarlarını yönet** sayfasında, aşağıdakileri yapın:

    ![Çoklu oturum açma yapılandırma sayfası](./media/absorblms-tutorial/settings.png)

    a. **Ad** metin kutusuna Azure AD marketi SSO 'su gibi bir ad girin.

    b. **Yöntem** olarak **SAML** ' yi seçin.

    c. Not defteri 'nde, Azure portal indirdiğiniz sertifikayı açın. **---Başlangıç sertifikası---** ve **---son sertifika---** etiketlerini kaldırın. Ardından, **anahtar** kutusunda kalan içeriği yapıştırın.

    d. **Mod** kutusunda **kimlik sağlayıcısı başlatıldı**' ı seçin.

    e. **Kimlik özellik** kutusunda, Azure AD 'de Kullanıcı tanımlayıcısı olarak yapılandırdığınız özniteliği seçin. Örneğin, Azure AD 'de *NameIdentifier* seçilirse **Kullanıcı adı**' nı seçin.

    f. **Imza türü** olarak **SHA256** öğesini seçin.

    örneğin: **Oturum açma URL** 'si kutusunda, Azure Portal uygulamanın **Özellikler** sayfasından **Kullanıcı erişim URL** 'sini yapıştırın.

    h. Oturum **kapatma URL**'si içinde, Azure Portal oturum açma **URL 'si** değerini, ' nin oturum açma **yapılandırma** penceresinden yapıştırın.

    i. Öğesini **otomatik olarak** **Açık** olarak değiştirin.

6. Kaydet ' i seçin **.**

    ![Yalnızca SSO oturumu açma geçişine Izin ver](./media/absorblms-tutorial/save.png)

### <a name="create-absorb-lms-test-user"></a>Artışlarını devralarak LMS test kullanıcısı oluşturma

Azure AD kullanıcılarının artışlarını devralarak LMS 'de oturum açması için, artışlarını devralarak LMS 'de ayarlanması gerekir. Artışlarını devralarak LMS söz konusu olduğunda, sağlama el ile gerçekleştirilen bir görevdir.

**Kullanıcı sağlamayı yapılandırmak için aşağıdaki adımları uygulayın:**

1. Artışlarını devralarak LMS şirket sitenizde yönetici olarak oturum açın.

2. **Kullanıcılar** bölmesinde, **Kullanıcılar**' ı seçin.

    ![Kullanıcılar bağlantısı](./media/absorblms-tutorial/users.png)

3. **Kullanıcı** sekmesini seçin.

    ![Yeni açılan liste ekle](./media/absorblms-tutorial/add.png)

4. **Kullanıcı Ekle** sayfasında, aşağıdakileri yapın:

    ![Kullanıcı Ekle sayfası](./media/absorblms-tutorial/user.png)

    a. **Ad** kutusuna ilk adı yazın (örneğin, **Britta**).

    b. **Son ad** kutusunda, **Simon** gibi soyadı yazın.

    c. **Kullanıcı adı** kutusuna, **Britta Simon** gibi bir tam ad yazın.

    d. **Parola** kutusuna kullanıcı parolası yazın.

    e. **Parolayı Onayla** kutusuna parolayı yeniden yazın.

    f. **Etkin** **' e geçiş etkin '** i ayarlayın.

5. Kaydet ' i seçin **.**

    ![Yalnızca SSO oturumu açma geçişine Izin ver](./media/absorblms-tutorial/save.png)

    > [!NOTE]
    > Varsayılan olarak, Kullanıcı sağlama SSO 'da etkinleştirilmemiştir. Müşteri bu özelliği etkinleştirmek isterse, [Bu](https://support.absorblms.com/hc/en-us/articles/360014083294-Incoming-SAML-2-0-SSO-Account-Provisioning) belgede belirtilen şekilde ayarlanmaları gerekir. Ayrıca, kullanıcının artışlarını devralarak 5 ' te kullanılabilir olduğunu, ACS URL 'SI ile **Yeni Learner deneyimini** de unutmayın.`https://company.myabsorb.com/api/rest/v2/authentication/saml`

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz.

* Azure portal bu uygulamayı test et ' e tıklayın ve SSO 'yu ayarladığınız artışlarını devralarak LMS ' de otomatik olarak oturum açmış olmanız gerekir.

* Microsoft My Apps ' i kullanabilirsiniz. Uygulamalarım içindeki artışlarını devralarak LMS kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız artışlarını devralarak LMS 'de otomatik olarak oturum açmış olmanız gerekir. Uygulamalarım hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="next-steps"></a>Sonraki adımlar

Artışlarını devralarak LMS 'yi yapılandırdıktan sonra, kuruluşunuzun gizli verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).
