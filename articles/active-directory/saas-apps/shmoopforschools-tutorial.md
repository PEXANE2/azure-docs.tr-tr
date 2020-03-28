---
title: 'Öğretici: Okullar için Shmoop ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Shmoop For Schools arasında tek oturum açma işlemlerini nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1d75560a-55b3-42e9-bda1-92b01c572d8e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 06ce2e51f72b9b4f709bf26ce6d92ad3861d36db
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74081638"
---
# <a name="tutorial-integrate-shmoop-for-schools-with-azure-active-directory"></a>Öğretici: Azure Active Directory ile Okullar için Shmoop Entegre

Bu eğitimde, Shmoop For Schools'u Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Shmoop For Schools'u Azure AD ile entegre ettiğinizde şunları yapabilirsiniz:

* Okullar için Shmoop erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Shmoop For Schools'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Shmoop For Schools tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Shmoop For **Schools, SP** tarafından başlatılan SSO'yu destekliyor
* Okullar için Shmoop **Just In Time** kullanıcı sağlama destekler

## <a name="adding-shmoop-for-schools-from-the-gallery"></a>Galeriden Okullar için Shmoop Ekleme

Shmoop For Schools'un Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Shmoop For Schools'u eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama **kutusuna Okullar İçin Shmoop** yazın.
1. Sonuçlar panelinden **Okullar İçin Shmoop'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-shmoop-for-schools"></a>Okullar için Shmoop için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'yu **B.Simon**adlı bir test kullanıcısı kullanarak Okullar Için Shmoop ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Shmoop For Schools'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Okullar Için Shmoop ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
2. **[Shmoop For Schools SSO'yu yapılandırır](#configure-shmoop-for-schools-sso)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
    * **[Shmoop For Schools test kullanıcısını oluşturun](#create-shmoop-for-schools-test-user)** - Shmoop For Schools'da B.Simon'ın bir örneğine sahip olmak için kullanıcının Azure AD temsiline bağlı.
3. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Okullar **için Shmoop** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://schools.shmoop.com/public-api/saml2/start/<uniqueid>`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://schools.shmoop.com/<uniqueid>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak [için Okullar Müşteri destek ekibine](mailto:support@shmoop.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. Okullar için Shmoop uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

    > [!NOTE]
    > Shmoop for School kullanıcılar için iki rolü destekler: **Öğretmen** ve **Öğrenci.** Kullanıcılara uygun rollerin atanabilmesi için bu rolleri Azure AD'de ayarlayın. Azure AD'deki rolleri nasıl yapılandırılayacağımı anlamak için [buraya](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management)bakın.

1. Yukarıdakilere ek olarak, Shmoop For Schools uygulaması, saml yanıtında aşağıda gösterilen birkaç özniteliğin daha geri geçirilmesini beklemaktadır. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı |  Kaynak Özniteliği|
    | --------- | --------------- |
    | rol      | user.assignedroles |

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

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

Bu bölümde, B.Simon'ın Shmoop For Schools'a erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde Okullar **için Shmoop'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-shmoop-for-schools-sso"></a>Okullar SSO için Shmoop Yapıla

**Okullar için Shmoop** tarafında tek oturum açma yapılandırmak [için, Okullar için Shmoop destek ekibine](mailto:support@shmoop.com)App Federation **Metadata Url'sini** göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

### <a name="create-shmoop-for-schools-test-user"></a>Okullar için Shmoop test kullanıcısı oluşturun

Bu bölümde, B.Simon adlı bir kullanıcı Shmoop For Schools'da oluşturulmuştur. Shmoop For Schools, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Shmoop For Schools'da bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

> [!NOTE]
> El ile bir kullanım oluşturmanız gerekiyorsa, [Okullar için Shmoop destek ekibine](mailto:support@shmoop.com)başvurun.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Okullar Için Shmoop döşemesini tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz Okullar için Shmoop'ta oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Okullar için Shmoop'u deneyin](https://aad.portal.azure.com/)