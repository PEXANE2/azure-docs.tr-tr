---
title: 'Öğretici: TINFOIL SECURITY ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve TINFOIL SECURITY arasında tek oturum açma yı nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71d3078b553843922cd51e4e0f43ea84b6dcde16
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74170758"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-tinfoil-security"></a>Öğretici: TINFOIL SECURITY ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, TINFOIL SECURITY ile Azure Etkin Dizini (Azure AD) ile nasıl entegre edileceksiniz. TINFOIL SECURITY'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* TINFOIL SECURITY erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla TINFOIL SECURITY'de otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* TINFOIL SECURITY tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* TINFOIL GÜVENLİk **IDP** başlatılan SSO destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-tinfoil-security-from-the-gallery"></a>Galeriden TINFOIL GÜVENLİğİ ekleme

TINFOIL SECURITY'nin Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize TINFOIL SECURITY eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **TINFOIL SECURITY** yazın.
1. Sonuç panelinden **TINFOIL SECURITY'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-tinfoil-security"></a>TINFOIL SECURITY için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak TINFOIL SECURITY ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile TINFOIL SECURITY'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu TINFOIL SECURITY ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[TINFOIL SECURITY SSO'yu yapılandırın](#configure-tinfoil-security-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[TINFOIL SECURITY test kullanıcısını oluşturun](#create-tinfoil-security-test-user)** - Kullanıcının Azure AD gösterimine bağlı TINFOIL SECURITY'de B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **TINFOIL SECURITY** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, uygulama önceden yapılandırılmıştır ve gerekli URL'ler Azure ile önceden doldurulmuştür. Kullanıcının **Kaydet** düğmesini tıklatarak yapılandırmayı kaydetmesi gerekir.

1. Ziyaret uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, Visitly uygulaması, aşağıda gösterilen SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak Özniteliği |
    | ------------------- | -------------|
    | accountid | UXXXXXXXXXXXXX |

    > [!NOTE]
    > Daha sonra öğreticide açıklanan hesap değeri alırsınız.

1. **SAML İmzalama Sertifikası** bölümünde, **SAML İmzalama Sertifikası** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![SAML İmza Sertifikasını Edin](common/edit-certificate.png)

1. **SAML İmza Sertifikası** **bölümünde, Parmak İzi Değerini** kopyalayın ve bilgisayarınıza kaydedin.

    ![Parmak Izi değerini kopyala](common/copy-thumbprint.png)

1. **TINFOIL SECURITY'yi ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın TINFOIL SECURITY'ye erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **TINFOIL SECURITY'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-tinfoil-security-sso"></a>TINFOIL GÜVENLİk SSO'nun yapılandırılması

1. Farklı bir web tarayıcısı penceresinde, TINFOIL SECURITY şirket sitenizde yönetici olarak oturum açın.

1. Üstteki araç çubuğunda **Hesabım'ı**tıklatın.

    ![Pano](./media/tinfoil-security-tutorial/ic798971.png "Pano")

1. **Güvenlik'i**tıklatın.

    ![Güvenlik](./media/tinfoil-security-tutorial/ic798972.png "Güvenlik")

1. Tek **Oturum Açma** yapılandırma sayfasında aşağıdaki adımları gerçekleştirin:

    ![Tek Oturum Açma](./media/tinfoil-security-tutorial/ic798973.png "Çoklu Oturum Açma")

    a. **SAML'yi Etkinleştir'i**seçin.

    b. **Manuel Yapılandırma'yı**tıklatın.

    c. **SAML Post URL** textbox'ına Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın

    d. **SAML Sertifika Parmak İzi** metin kutusuna, **SAML İmza Sertifikası** bölümünden kopyalamış olduğunuz **Parmak İzi** değerini yapıştırın.
  
    e. **Hesap Kimliğinizi** kopyalayın ve Azure portalındaki Kullanıcı **Öznitelikleri & Talepler** bölümü altındaki Kaynak **Öznitelik** metin kutusundaki değeri yapıştırın.

    f. **Kaydet**'e tıklayın.

### <a name="create-tinfoil-security-test-user"></a>TINFOIL SECURITY test kullanıcıoluşturma

Azure AD kullanıcılarının TINFOIL SECURITY'de oturum açabilmeleri için, tinfoil security'de oturum açmaları gerekir. TINFOIL GÜVENLİk söz konusu olduğunda, sağlama manuel bir görevdir.

**Bir kullanıcının sağlanması için aşağıdaki adımları gerçekleştirin:**

1. Kullanıcı bir Kurumsal hesabın bir parçasıysa, kullanıcı hesabının oluşturulması için [TINFOIL SECURITY destek ekibine başvurmanız](https://www.tinfoilsecurity.com/contact) gerekir.

1. Kullanıcı düzenli bir TINFOIL SECURITY SaaS kullanıcısıysa, kullanıcı kullanıcının sitelerinden herhangi biri için bir ortak layıcı ekleyebilirsiniz. Bu, yeni bir TINFOIL SECURITY kullanıcı hesabı oluşturmak için belirtilen e-postaya davet gönderme işlemini tetikler.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için TINFOIL SECURITY tarafından sağlanan diğer TINFOIL SECURITY kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki TINFOIL SECURITY döşemesini tıklattığınızda, SSO'yu kurduğunuz TINFOIL SECURITY'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure REKLAM ile TINFOIL GÜVENLİğİ'ni deneyin](https://aad.portal.azure.com/)