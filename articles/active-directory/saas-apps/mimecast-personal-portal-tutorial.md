---
title: 'Öğretici: Mimecast kişisel portalı ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Mimecast kişisel portalı arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/15/2021
ms.author: jeedes
ms.openlocfilehash: cc12afb9d4a889d53689421a98b522a8e3264da6
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101644621"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-mimecast-personal-portal"></a>Öğretici: Mimecast kişisel portalı ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Mimecast kişisel portalını Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Mimecast kişisel portalını Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Mimecast kişisel portalına erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla, Mimecast kişisel Portalı ' na otomatik olarak kaydolmalarına imkan tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Mimecast kişisel portalı çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Mimecast kişisel portalı **SP ve ıDP** tarafından başlatılan SSO 'yu destekler
 
## <a name="add-mimecast-personal-portal-from-the-gallery"></a>Galeriden Mimecast kişisel portalını ekleyin

Mimecast kişisel portalının tümleştirmesini Azure AD 'ye yapılandırmak için, Galeriden Mimecast kişisel portalı 'nı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Mimecast kişisel portalı** yazın.
1. Sonuçlar panelinden **Mimecast kişisel portalı** ' nı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-mimecast-personal-portal"></a>Mimecast kişisel portalı için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak, Mimecast kişisel portalı Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, Mimecast kişisel portalındaki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Mimecast kişisel portalı ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Mimecast Personal Portal SSO 'Yu yapılandırma](#configure-mimecast-personal-portal-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. Mimecast kişisel **[portalı test kullanıcısı oluşturma](#create-mimecast-personal-portal-test-user)** -kullanıcının Azure AD gösterimine bağlı olan Mimecast kişisel portalı 'nda B. Simon 'a sahip olmak için.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **Mimecast kişisel portalı** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, uygulamayı IDP tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    a. **Tanımlayıcı** metin kutusuna aşağıdaki kalıbı kullanarak URL 'yi yazın:

    | Region  |  Değer | 
    | --------------- | --------------- |
    | Avrupa          | `https://eu-api.mimecast.com/sso/<accountcode>`|
    | Birleşik Devletler   | `https://us-api.mimecast.com/sso/<accountcode>`|
    | Güney Afrika    | `https://za-api.mimecast.com/sso/<accountcode>`|
    | Avustralya       | `https://au-api.mimecast.com/sso/<accountcode>`|
    | Yurtdışında        | `https://jer-api.mimecast.com/sso/<accountcode>`|

    > [!NOTE]
    > Bu `accountcode` değeri, **Hesap**  >  **ayarları**  >  **Hesap kodu** altında Mimecast kişisel portalında bulabilirsiniz. `accountcode`Tanımlayıcısını tanımlayıcıya ekleyin.

    b. **Yanıt URL** 'si metin kutusuna URL 'yi yazın:

    | Region  |  Değer |
    | --------------- | --------------- |
    | Avrupa          | `https://eu-api.mimecast.com/login/saml`|
    | Birleşik Devletler   | `https://us-api.mimecast.com/login/saml`|
    | Güney Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Avustralya       | `https://au-api.mimecast.com/login/saml`|
    | Yurtdışında        | `https://jer-api.mimecast.com/login/saml`|

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak istiyorsanız:

    **Oturum açma URL 'si** metin kutusuna URL 'yi yazın:

    | Region  |  Değer |
    | --------------- | --------------- |
    | Avrupa          | `https://eu-api.mimecast.com/login/saml`|
    | Birleşik Devletler   | `https://us-api.mimecast.com/login/saml`|
    | Güney Afrika    | `https://za-api.mimecast.com/login/saml`|
    | Avustralya       | `https://au-api.mimecast.com/login/saml`|
    | Yurtdışında        | `https://jer-api.mimecast.com/login/saml`|

1. **Kaydet**’e tıklayın.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'sini** kopyalamak ve bilgisayarınıza kaydetmek için Kopyala düğmesine tıklayın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

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

Bu bölümde, Mimecast kişisel portalına erişim izni vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Mimecast kişisel portalı**' nı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-mimecast-personal-portal-sso"></a>Mimecast kişisel portalı SSO 'SU yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, Mimecast yönetim konsolunda oturum açın.

1. **Yönetim**  >  **Hizmetleri**  >  **uygulamalarına** gidin.

    ![Ekran görüntüsü, seçili uygulamaların bulunduğu Mimecast penceresini gösterir.](./media/mimecast-personal-portal-tutorial/services.png)

1. **Kimlik doğrulama profilleri** sekmesi ' ne tıklayın.
    
    ![Ekran görüntüsü, kimlik doğrulama profillerinin seçili olduğu uygulama sekmesini gösterir.](./media/mimecast-personal-portal-tutorial/authentication-profiles.png)

1. **Yeni kimlik doğrulama profili** sekmesi ' ne tıklayın.

    ![Ekran görüntüsü yeni kimlik doğrulama profili seçili olduğunu gösterir.](./media/mimecast-personal-portal-tutorial/new-authenticatio-profile.png)

1. **Açıklama** metin kutusuna geçerli bir açıklama girin ve **Mimecast kIşIsel portalı Için SAML kimlik doğrulamasını zorla** onay kutusunu seçin.

    ![Ekran görüntüsü yeni kimlik doğrulama profili seçili olduğunu gösterir.](./media/mimecast-personal-portal-tutorial/selecting-personal-portal.png)

1. **Mimecast kişisel portalı Için SAML yapılandırması** sayfasında, aşağıdaki adımları uygulayın:

    ![Ekran görüntüsünde, Yönetim Konsolu için SAML kimlik doğrulamasının zorla ' nın nasıl oluşturulacağı gösterilir.](./media/mimecast-personal-portal-tutorial/sso-settings.png)

    a. **Sağlayıcı** Için, açılan listeden **Azure Active Directory** seçin.

    b. **Meta veri URL 'si** metin kutusunda, Azure Portal kopyaladığınız **uygulama Federasyon meta veri URL 'si** değerini yapıştırın.

    c. **İçeri Aktar**’a tıklayın. Meta veri URL 'sini içeri aktardıktan sonra alanlar otomatik olarak doldurulur ve bu alanlar üzerinde herhangi bir işlem gerçekleştirmeye gerek kalmaz.

    d. **Parola korumalı bağlam kullan** ' ın işaretini kaldırın ve **Tümleşik kimlik doğrulaması bağlam** onay kutularını kullanın.

    e. **Kaydet**’e tıklayın.

### <a name="create-mimecast-personal-portal-test-user"></a>Mimecast kişisel portalı test kullanıcısı oluştur

1. Farklı bir Web tarayıcısı penceresinde, Mimecast yönetim konsolunda oturum açın.

1. **Yönetim**  >  **dizinleri**  >  **iç dizinleri**' ne gidin.

    ![Ekran görüntüsünde, Mimecast kişisel portalının SAML yapılandırması gösterilir ve burada açıklanan değerleri girebilirsiniz.](./media/mimecast-personal-portal-tutorial/internal-directories.png)

1. Etki alanı aşağıda bahsedildiğinde, etki alanınızı seçin, aksi takdirde lütfen **yeni etki alanına** tıklayarak yeni bir etki alanı oluşturun.

    ![Ekran görüntüsü, Iç dizinlerin seçili olduğu Mimecast penceresini gösterir.](./media/mimecast-personal-portal-tutorial/domain-name.png)

1. **Yeni adres** sekmesi ' ne tıklayın.

    ![Ekran görüntüsü seçili etki alanını gösterir.](./media/mimecast-personal-portal-tutorial/new-address.png)

1. Aşağıdaki sayfada gerekli Kullanıcı bilgilerini sağlayın:

    ![Ekran görüntüsü, açıklanan değerleri girebileceğiniz sayfayı gösterir.](./media/mimecast-personal-portal-tutorial/user-information.png)

    a. **E-posta adresi** metin kutusuna, gibi kullanıcının e-posta adresini girin `B.Simon@yourdomainname.com` .

    b. **Genel ad** metin kutusuna kullanıcının **tam adını** girin.

    c. **Parola** ve **Parolayı Onayla** metin kutularına kullanıcının parolasını girin.

    d. **Oturum açmada zorla Değiştir** onay kutusunu seçin.

    e. **Kaydet**’e tıklayın.

    f. Kullanıcılara roller atamak için **rol Düzenle** ' ye tıklayın ve gerekli rolü, kuruluşunuzun gereksinimlerine göre kullanıcıya atayın.

    ![Ekran görüntüsü, rol düzenleme seçebileceğiniz adres ayarlarını gösterir.](./media/mimecast-personal-portal-tutorial/assign-role.png)


## <a name="test-sso"></a>Test SSO 'SU 
Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

#### <a name="sp-initiated"></a>SP başlatıldı:

* Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu, oturum açma akışını başlatabileceğiniz Mimecast kişisel portalı oturum açma URL 'sine yeniden yönlendirilir.  

* Mimecast kişisel portalı oturum açma URL 'sine doğrudan gidin ve oturum akışını buradan başlatın.

#### <a name="idp-initiated"></a>IDP başlatıldı:

* Azure portal **Bu uygulamayı test et** ' e tıklayın ve SSO 'Yu ayarladığınız Mimecast kişisel portalında otomatik olarak oturum açmış olmanız gerekir 

Uygulamayı dilediğiniz modda test etmek için Microsoft My Apps ' i de kullanabilirsiniz. Uygulamalarım içindeki Mimecast kişisel portalı kutucuğuna tıkladığınızda, SP modunda yapılandırılmışsa, oturum açma akışını başlatmak için uygulama oturum açma sayfasına yönlendirilirsiniz ve ıDP modunda yapılandırıldıysa, SSO 'yu ayarladığınız Mimecast kişisel portalında otomatik olarak oturum açmış olmanız gerekir. Uygulamalarım hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

Mimecast kişisel portalını yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app)