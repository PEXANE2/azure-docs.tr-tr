---
title: 'Öğretici: Azure Active Directory tek oturum açma (SSO) entegrasyonu ile Jisc Öğrenci Seçmen Kaydı | Microsoft Dokümanlar'
description: Azure Active Directory ve Jisc Öğrenci Seçmen Kaydı arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a95d016d-ec53-4b3c-9a51-e0cac0791e49
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/26/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7de54b507630174107994f1434f17120f3abffbb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74807475"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-jisc-student-voter-registration"></a>Öğretici: Azure Active Directory tek oturum açma (SSO) entegrasyonu ve Jisc Öğrenci Seçmen Kaydı

Bu eğitimde, Jisc Öğrenci Seçmen Kaydı'nı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Jisc Öğrenci Seçmen Kaydı'nı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Jisc Öğrenci Seçmen Kaydı'na erişimi olan denetimi.
* Kullanıcılarınızın Azure AD hesaplarıyla Jisc Öğrenci Seçmen Kaydı'nda otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Jisc Öğrenci Seçmen Kaydı tek oturum açma (SSO) aboneliği ni etkinleştirdi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Jisc Öğrenci Seçmen Kaydı **SP** başlatılan SSO destekler
* Jisc Öğrenci Seçmen Kaydı **Just In Time** kullanıcı sağlama destekler

## <a name="adding-jisc-student-voter-registration-from-the-gallery"></a>Galeriden Jisc Öğrenci Seçmen Kaydı Ekleme

Jisc Öğrenci Seçmen Kaydı'nın Azure AD'ye entegrasyonunu yapılandırmak için galeriden Yönetilen SaaS uygulamaları listenize Jisc Öğrenci Seçmen Kaydı eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Jisc Öğrenci Seçmen Kaydı** yazın.
1. Sonuç panelinden **Jisc Öğrenci Seçmen Kaydı'nı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-jisc-student-voter-registration"></a>Jisc Öğrenci Seçmen Kaydı için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu Jisc Öğrenci Seçmen Kaydı ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Jisc Öğrenci Seçmen Kaydı'ndaki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Jisc Öğrenci Seçmen Kaydı ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Jisc Öğrenci Seçmen Kayıt SSO'yu yapılandırır](#configure-jisc-student-voter-registration-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Jisc Öğrenci Seçmen Kaydı test kullanıcısını oluşturun](#create-jisc-student-voter-registration-test-user)** - Kullanıcının Azure AD temsiline bağlı Jisc Öğrenci Seçmen Kaydı'nda B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Jisc Öğrenci Seçmen Kaydı** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanın değerlerini girin:

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://www.studentvoterregistration.ac.uk/consent`

1. Jisc Öğrenci Seçmen Kaydı uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, Jisc Öğrenci Seçmen Kayıt uygulaması aşağıda gösterilen SAML yanıtı geri geçirilmesi için birkaç daha fazla öznitelikleri bekliyor. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı |  Kaynak Özniteliği|
    | ---------|  --------- |
    | Postakodu | user.postalcode |
    | Benzersiz Kullanıcı Tanımlayıcısı | user.objectid |

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Jisc Öğrenci Seçmen Kaydı** Nı Ayarla bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Jisc Öğrenci Seçmen Kaydı'na erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Başvuru listesinde **Jisc Öğrenci Seçmen Kaydı'nı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-jisc-student-voter-registration-sso"></a>Cisc Öğrenci Seçmen Kaydı SSO'nun yapılandırılması

**Jisc Öğrenci Seçmen Kaydı** tarafında tek oturum açma işlemi yapmak için, indirilen **Federasyon Metadata XML'ini** ve uygun kopyalanmış URL'leri Azure portalından [Jisc Öğrenci Seçmen Kaydı destek ekibine](mailto:studentvote@jisc.ac.uk)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

### <a name="create-jisc-student-voter-registration-test-user"></a>Jisc Öğrenci Seçmen Kayıt testi kullanıcı oluşturma

Bu bölümde, B.Simon adlı bir kullanıcı Jisc Öğrenci Seçmen Kaydı oluşturulur. Jisc Öğrenci Seçmen Kaydı, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Jisc Öğrenci Seçmen Kaydı'nda bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Jisc Öğrenci Seçmen Kaydı döşemesini tıklattığınızda, SSO'yu kurduğunuz Jisc Öğrenci Seçmen Kaydı'nda otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Jisc Öğrenci Seçmen Kaydı'nı deneyin](https://aad.portal.azure.com/)