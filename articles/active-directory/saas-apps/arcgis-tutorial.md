---
title: 'Öğretici: argıs Online ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve ArcGIS arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/08/2021
ms.author: jeedes
ms.openlocfilehash: 7a445eefa31e741562105e89fa105d404ccc0c7e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101646359"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Öğretici: ArcGIS Online ile tümleştirme Azure Active Directory

Bu öğreticide, Azure Active Directory (Azure AD) ile argıs 'yi çevrimiçi olarak tümleştirmeyi öğreneceksiniz. ArcGIS Online 'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de, argıs online 'a erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla çevrimiçi olarak internet 'e kaydolmalarına olanak tanır.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* ArcGIS Online çoklu oturum açma (SSO) aboneliği etkin.

> [!NOTE]
> Bu tümleştirme Ayrıca Azure AD ABD kamu bulut ortamından kullanılabilir. Bu uygulamayı Azure AD ABD kamu bulutu uygulama galerisinde bulabilir ve bunu ortak buluttan yaptığınız şekilde yapılandırabilirsiniz.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* ArcGIS Online, **SP** tarafından başlatılan SSO 'yu destekler.

## <a name="add-arcgis-online-from-the-gallery"></a>Galeriden çevrimiçi olarak ArcGIS ekleme

ArcGIS Online 'ın Azure AD ile tümleştirilmesini yapılandırmak için, galerinizden yönetilen SaaS uygulamaları listenize çevrimiçi olarak ArcGIS eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **argıs online** yazın.
1. Sonuçlar panelinden ArcGIS **Online** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-arcgis-online"></a>Argıs Online için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak, ArcGIS Online Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, çevrimiçi bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu ArcGIS Online ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Argıs ONLINE SSO 'Yu yapılandırma](#configure-arcgis-online-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. ArcGIS **[online test kullanıcısı oluşturun](#create-arcgis-online-test-user)** ; bu, kullanıcının Azure AD gösterimine bağlı olan ArcGIS Online 'da B. Simon 'ın bir karşılığı olacak.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, ArcGIS **çevrimiçi** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<companyname>.maps.arcgis.com`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`<companyname>.maps.arcgis.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için [çevrimiçi istemci desteği ekibine](https://support.esri.com/en/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. ArcGIS **Online** içindeki yapılandırmayı otomatik hale getirmek için, **uzantıyı yüklemek** üzere **uygulamalarımı güvenli oturum açma tarayıcı uzantısı** ' nı yüklemeniz gerekir.

    ![image](./media/arcgis-tutorial/install-extension.png)

7. Tarayıcıya Uzantı eklendikten sonra, **çevrimiçi** olarak bulunan ArcGIS Online uygulamasına tıklayın. Buradan, çevrimiçi ortamda oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı, uygulamayı sizin için otomatik olarak yapılandırır ve daha sonra **çevrimiçi çoklu oturum açmayı yapılandırma** bölümündeki adımları otomatik hale getirir.

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

Bu bölümde, argıs online 'a internet erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, ArcGIS **Online**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-arcgis-online-sso"></a>Argıs online SSO 'yu yapılandırma

1. Argıs 'yi el ile çevrimiçi olarak kurmak istiyorsanız yeni bir Web tarayıcı penceresi açın ve argıs şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

2. **Ayarları Düzenle**' ye tıklayın.

    ![Ayarları Düzenle](./media/arcgis-tutorial/settings.png "Ayarları Düzenle")

3. **Güvenlik**' e tıklayın.

    ![Güvenlik](./media/arcgis-tutorial/secure.png "Güvenlik")

4. **Kurumsal oturum açmalar** altında **kimlik sağlayıcısını ayarla**' ya tıklayın.

    ![Kurumsal oturum açmalar](./media/arcgis-tutorial/enterprise.png "Kurumsal oturum açmalar")

5. **Kimlik sağlayıcısı yapılandırmasını ayarla** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![Kimlik sağlayıcısını ayarla](./media/arcgis-tutorial/identity-provider.png "Kimlik sağlayıcısını ayarla")

    a. **Ad** metin kutusuna kuruluşunuzun adını yazın.

    b. **Kurumsal kimlik sağlayıcısı Için meta veriler kullanılarak sağlanacak** **bir dosya** seçin.

    c. İndirilen meta veri dosyanızı karşıya yüklemek için **Dosya Seç**' e tıklayın.

    d. **KIMLIK sağlayıcısını ayarla**' ya tıklayın.

### <a name="create-arcgis-online-test-user"></a>ArcGIS Online test kullanıcısı oluşturma

Azure AD kullanıcılarının çevrimiçi olarak ArcGIS 'de oturum açmasını sağlamak için, bu kullanıcıların çevrimiçi olarak ArcGIS 'de sağlanması gerekir.  
ArcGIS çevrimiçi olduğunda, sağlama el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **Argıs** kiracınızda oturum açın.

2. **Üyeleri davet et**' e tıklayın.

    ![Üyeleri davet et](./media/arcgis-tutorial/invite.png "Üyeleri davet et")

3. **E-posta göndermeden otomatik olarak üye Ekle**' yi seçin ve ardından **İleri**' ye tıklayın.

    ![Üyeleri otomatik olarak ekle](./media/arcgis-tutorial/members.png "Üyeleri otomatik olarak ekle")

4. **Üyeler** iletişim sayfasında, aşağıdaki adımları uygulayın:

    ![Ekle ve gözden geçir](./media/arcgis-tutorial/review.png "Ekle ve gözden geçir")

     a. Sağlamak istediğiniz geçerli bir Azure AD hesabının **e-posta**, **ad** ve **Soyadı adını** girin.

     b. **Ekle ve gözden geçir**' e tıklayın.
5. Girdiğiniz verileri gözden geçirin ve ardından **üye Ekle**' ye tıklayın.

    ![Üye Ekle](./media/arcgis-tutorial/add.png "Üye Ekle")

    > [!NOTE]
    > Azure Active Directory hesap sahibi bir e-posta alır ve etkin hale gelmeden önce hesaplarını onaylamak için bir bağlantıyı izler.

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

* Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu, oturum açma akışını başlatabileceğiniz EGIS çevrimiçi oturum açma URL 'sine yeniden yönlendirilir. 

* Doğrudan çevrimiçi oturum açma URL 'sine gidin ve oturum açma akışını buradan başlatın.

* Microsoft My Apps ' i kullanabilirsiniz. Uygulamalarım çevrimiçi kutucuğuna tıkladığınızda, bu, çevrimiçi oturum açma URL 'sine yönlendirilir. Uygulamalarım hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

ArcGIS Online 'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).