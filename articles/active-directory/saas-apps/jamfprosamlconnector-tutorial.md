---
title: 'Öğretici: Jamf Pro ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Jamf Pro arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/11/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1d83dbe756e8e6acdb58861ac359801bc13a63c4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77373227"
---
# <a name="tutorial-azure-active-directory-sso-integration-with-jamf-pro"></a>Öğretici: Jamf Pro ile Azure Active Directory SSO entegrasyonu

Bu eğitimde, Jamf Pro'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edilebildiğini öğreneceksiniz. Jamf Pro'yı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Jamf Pro'ya kimlerin erişebilenleri denetlemek için Azure AD'yi kullanın.
* Kullanıcılarınızı Azure AD hesaplarıyla Jamf Pro'da otomatik olarak oturum açın.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için [Azure Active Directory ile tek oturum açma'ya](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Tek oturum açma (SSO) etkin bir Jamf Pro aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. 

* Jamf Pro, **SP tarafından başlatılan** ve **IdP tarafından başlatılan** SSO'yı destekler.
* Jamf Pro'u yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum Denetimi'ni uygulayabilirsiniz. Oturum Denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="add-jamf-pro-from-the-gallery"></a>Galeriden Jamf Pro ekle

Jamf Pro'nun Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Jamf Pro'yu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabını veya kişisel Microsoft hesabınızı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol bölmede **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle bölümüne,** arama kutusuna *Jamf Pro* girin.
1. Sonuçlar panelinden **Jamf Pro'yu** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-sso-in-azure-ad-for-jamf-pro"></a>Jamf Pro için Azure AD'de SSO'u yapılandırma ve test edin

Azure AD SSO'nu B.Simon adlı bir test kullanıcısı kullanarak Jamf Pro ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Jamf Pro'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Bu bölümde, Azure AD SSO'su Jamf Pro ile yapılandırın ve test esiniz.

1. Kullanıcılarınızın bu özelliği kullanabilmesi için [SSO'yı Azure AD'da yapılandırın.](#configure-sso-in-azure-ad)
    1. Azure AD SSO'yu B.Simon hesabıyla sınamak için [bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)
    1. B.Simon'ın Azure AD'de SSO'yu kullanabilmesi için [Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)
1. Uygulama tarafındaki SSO ayarlarını yapılandırmak için [Jamf Pro'da SSO'yu yapılandırın.](#configure-sso-in-jamf-pro)
    1. Jamf Pro'da B.Simon'ın, kullanıcının Azure AD gösterimine bağlı bir muadili olması için [bir Jamf Pro test kullanıcısı oluşturun.](#create-a-jamf-pro-test-user)
1. Yapılandırmanın çalıştığını doğrulamak için [SSO yapılandırmasını sınama.](#test-the-sso-configuration)

## <a name="configure-sso-in-azure-ad"></a>Azure AD'de SSO'ya yapı

Bu bölümde, Azure portalında Azure AD SSO'yu etkinleştirin.

1. Azure [portalında,](https://portal.azure.com/) **Jamf Pro** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve Tek **Oturum Aç'ı**seçin.
1. Tek **Oturum Açma Yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla Tek Oturum** Açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için kalem simgesini seçin.

   ![Temel SAML Yapılandırma sayfasını düzenleme.](common/edit-urls.png)

1. Temel **SAML Yapılandırma** bölümünde, uygulamayı **IdP tarafından başlatılan** modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki formülü kullanan bir URL girin:`https://<subdomain>.jamfcloud.com/saml/metadata`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki formülü kullanan bir URL girin:`https://<subdomain>.jamfcloud.com/saml/SSO`

1. **Ek URL'ler Ayarla'yı**seçin. Uygulamayı **SP tarafından başlatılan** modda, **Oturum Aç URL** metin kutusuna yapılandırmak istiyorsanız, aşağıdaki formülü kullanan bir URL girin:`https://<subdomain>.jamfcloud.com`

    > [!NOTE]
    > Bu değerler gerçek değil. Bu değerleri gerçek tanımlayıcı, yanıt URL'si ve oturum açma URL'si ile güncelleştirin. Gerçek tanımlayıcı değerini, daha sonra öğreticide açıklanan Jamf Pro portalındaki **Tek Oturum Açma** bölümünden alırsınız. Gerçek alt etki alanı değerini tanımlayıcı değerinden ayıklayabilir ve bu alt etki alanı bilgilerini oturum açma URL'niz ve yanıt URL'niz olarak kullanabilirsiniz. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen formüllere de bakabilirsiniz.

1. **SAML** ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümüne gidin, **App Federation Metadata URL'sini**kopyalamak için **kopyalama** düğmesini seçin ve ardından bilgisayarınıza kaydedin.

    ![SAML İmza Sertifikası indirme bağlantısı](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalının sol bölmesinde **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.
   1. Kullanıcı **adı** alanına [ad]@[companydomain] girin. [uzantısı]. Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'a Jamf Pro'ya erişim izni ver.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Jamf Pro'yu**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   ![Kullanıcı ve gruplar'ı seçin](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle düğmesini seçin](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini seçin.
1. SAML iddiasında, **Rolü Seç** iletişim kutusunda herhangi bir rol değeri bekliyorsanız, kullanıcı için uygun rolü seçin. Ardından, ekranın altındaki **Seç** düğmesini seçin.
1. Atama **Ekle** iletişim kutusunda Atama **düğmesini** seçin.

## <a name="configure-sso-in-jamf-pro"></a>Jamf Pro'da SSO'u yapılandırın

1. Jamf Pro içindeki yapılandırmayı otomatikleştirmek için, uzantıyı yükleyin'i seçerek **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** **yükleyin.**

    ![Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısı sayfası](common/install-myappssecure-extension.png)

2. Uzantıyı tarayıcıya ekledikten sonra **Jamf Pro'yu**ayarla'yı seçin. Jamf Pro uygulaması açıldığında, oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı otomatik olarak yapılandıracak ve 3'ten 7'ye kadar olan adımları otomatikleştirecektir.

    ![Jamf Pro'da kurulum yapılandırma sayfası](common/setup-sso.png)

3. Jamf Pro'u el ile kurmak için yeni bir web tarayıcısı penceresi açın ve Jamf Pro şirket sitenizde yönetici olarak oturum açın. Ardından, aşağıdaki adımları atın.

4. Sayfanın sağ üst köşesinden **Ayarlar simgesini** seçin.

    ![Jamf Pro'da ayarlar simgesini seçin](./media/jamfprosamlconnector-tutorial/configure1.png)

5. **Tek Oturum Açma'yı**seçin.

    ![Jamf Pro'da Tek Oturum Açma'yı seçin](./media/jamfprosamlconnector-tutorial/configure2.png)

6. Tek **Oturum Açma** sayfasında aşağıdaki adımları izleyin.

    ![Jamf Pro'da Tek Oturum Açma sayfası](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. **Edit'i**seçin.

    b. Tek **Oturum Açma Kimlik Doğrulamasını Etkinleştir** onay kutusunu seçin.

  c. **Kimlik Sağlayıcısı** açılır menüsünden seçenek olarak **Azure'u** seçin.

  d. ENTITY **ID** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Tanımlayıcı (Entity ID)** alanına yapıştırın.

> [!NOTE]
> Azure portalındaki `<SUBDOMAIN>` **Temel SAML Yapılandırması** bölümünde oturum açma URL'sini ve yanıt URL'sini tamamlamak için alandaki değeri kullanın.

  e. Kimlik Sağlayıcısı Meta **veri kaynağı** açılır menüsünden **Meta veri URL'sini** seçin. Görünen alana, Azure portalından kopyaladığınız **Uygulama Federasyonu Metaveri Url** değerini yapıştırın.

  f. (İsteğe bağlı) Belirteç son kullanma değerini edin veya "SAML belirteci sona ermesini devre dışı" seçeneğini belirleyin.

7. Aynı sayfada, **Kullanıcı Eşleme** bölümüne gidin. Ardından, aşağıdaki adımları atın.

    ![Jamf Pro'daki Tek Oturum Açma sayfasının Kullanıcı Eşleme bölümü.](./media/jamfprosamlconnector-tutorial/tutorial-jamfprosamlconnector-single.png)

    a. Kimlik Sağlayıcı Kullanıcı **Eşlemi**için **NameID** seçeneğini seçin. Varsayılan olarak, bu seçenek **NameID**olarak ayarlanır, ancak özel bir öznitelik tanımlayabilirsiniz.

    b. **Jamf Pro Kullanıcı Eşleme**için **E-posta** seçin. Jamf Pro, IdP tarafından önce kullanıcılar, sonra gruplar tarafından gönderilen SAML özniteliklerini eşler. Bir kullanıcı Jamf Pro'ya erişmeye çalıştığında, Jamf Pro kimlik sağlayıcıdan kullanıcı hakkında bilgi alır ve tüm Jamf Pro kullanıcı hesaplarıyla eşleşir. Gelen kullanıcı hesabı bulunamazsa, Jamf Pro bu hesabı grup adına göre eşleştirmeye çalışır.

    c. `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` **KİmLİk SAGLAYICI GRUP ÖZDEYİş Ad** alanına değeri yapıştırın.

    d. Aynı sayfada, **Güvenlik** bölümüne gidin ve **kullanıcıların Tek Oturum Açma kimlik doğrulamasını atlayabilmeleri için izin**ver'i seçin. Sonuç olarak, kullanıcılar kimlik doğrulaması için Kimlik Sağlayıcısı oturum açma sayfasına yönlendirilmez ve bunun yerine doğrudan Jamf Pro'da oturum açabilir. Bir kullanıcı Jamf Pro'ya Kimlik Sağlayıcısı üzerinden erişmeye çalıştığında, IdP tarafından başlatılan SSO kimlik doğrulaması ve yetkilendirmesi gerçekleşir.

    e. **Kaydet'i**seçin.

### <a name="create-a-jamf-pro-test-user"></a>Jamf Pro test kullanıcısı oluşturma

Azure AD kullanıcılarının Jamf Pro'da oturum açabilmesi için, bu kullanıcıların Jamf Pro'da oturum açmaları gerekir. Jamf Pro'da sağlama el ile bir görevdir.

Bir kullanıcı hesabı sağlamak için aşağıdaki adımları izleyin:

1. Jamf Pro şirket sitenizde yönetici olarak oturum açın.

2. Sayfanın sağ üst köşesindeki **Ayarlar** simgesini seçin.

    ![Jamf Pro'daki ayarlar simgesi](./media/jamfprosamlconnector-tutorial/configure1.png)

3. **Jamf Pro Kullanıcı Hesapları & Grupları'nı**seçin.

    ![Jamf Pro Kullanıcı Hesapları & Gruplar simgesi Jamf Pro ayarlarında](./media/jamfprosamlconnector-tutorial/user1.png)

4. **Yeni'yi**seçin.

    ![Jamf Pro Kullanıcı Hesapları & Gruplar sistem ayarları sayfası](./media/jamfprosamlconnector-tutorial/user2.png)

5. **Standart Hesap Oluştur'u**seçin.

    ![Jamf Pro Kullanıcı Hesapları & Gruplar sayfasında Standart Hesap Oluştur seçeneği](./media/jamfprosamlconnector-tutorial/user3.png)

6. Yeni **Hesap** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Jamf Pro sistem ayarlarında yeni hesap kurulum seçenekleri](./media/jamfprosamlconnector-tutorial/user4.png)

    a. **USERNAME** alanına, test `Britta Simon`kullanıcısının tam adını girin.

    b. Kuruluşunuza uygun **ACCESS LEVEL,** **PRIVILEGE SET**ve ACCESS **STATUS** seçeneklerini seçin.

    c. TAM **İsİm** alanına `Britta Simon`girin.

    d. **E-POSTA ADRESİ** alanına Britta Simon'ın hesabının e-posta adresini girin.

    e. **PAROLA** alanına kullanıcının parolasını girin.

    f. **PAROLAYI DOĞRULA** alanına, kullanıcının parolasını yeniden girin.

    g. **Kaydet'i**seçin.

## <a name="test-the-sso-configuration"></a>SSO yapılandırmasını test edin

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Jamf Pro döşemesini seçtiğinizde, SSO'yu yapılandırdığınız Jamf Pro hesabında otomatik olarak oturum açmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme için öğreticiler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory'deki uygulamalarda tek oturum açma](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
- [Azure AD ile Jamf Pro'u deneyin](https://aad.portal.azure.com/)