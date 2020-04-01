---
title: 'Öğretici: Azure Active Directory tek oturum açma (SSO) entegrasyonu ile Concur Travel ve Expense | Microsoft Dokümanlar'
description: Azure Active Directory ile Concur Travel and Expense arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 6401ace4-71c0-4778-8b15-900f2f5f0c4c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9dddd9f6904aa5ef7840850792aeabf04666dddc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72373429"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-concur-travel-and-expense"></a>Öğretici: Concur Travel ve Expense ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Concur Travel ve Expense'u Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Concur Travel ve Expense'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Eşzamanlı Seyahat ve Gider'e erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Concur Travel ve Giderler'de otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Concur Seyahat ve Gider aboneliği.
* Concur kullanıcı hesabınız altında bir "Şirket Yöneticisi" rolü. [Concur SSO Self Servis Aracı'na](https://www.concursolutions.com/nui/authadmin/ssoadmin)giderek doğru erişime sahip olup olmadığını test edebilirsiniz. Erişiminiz yoksa, lütfen Concur destek veya uygulama proje yöneticisine başvurun. 

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su yapılandırın ve test esiniz.

* Concur Seyahat ve Gider **IDP** ve **SP** SSO başlatılan destekler
* Concur Travel and Expense, SSO'nun hem üretim hem de uygulama ortamında test edilmesine destek veriyor 

> [!NOTE]
> Bu uygulamanın tanımlayıcısı, üç bölgenin her biri için sabit bir dize değeridir: ABD, EMEA ve Çin. Bu nedenle, her bölge için tek bir kiracıda yalnızca bir örnek yapılandırılabilir. 

## <a name="adding-concur-travel-and-expense-from-the-gallery"></a>Galeriden Concur Seyahat ve Gider Ekleme

Concur Travel ve Expense'un Azure AD'ye entegrasyonunu yapılandırmak için, yönetilen SaaS uygulamaları listenize galeriden Concur Seyahat ve Gider eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Concur Seyahat ve Gider** yazın.
1. Sonuç panelinden **Concur Seyahat ve Gider'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-concur-travel-and-expense"></a>Concur Travel ve Expense için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'yu **B.Simon**adlı bir test kullanıcısı kullanarak Concur Travel ve Giderler ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Ilgili kullanıcı arasında Concur Travel and Expense'da bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Concur Travel ve Expense ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Koncur Seyahat ve Gider SSO'yu yapılandırın](#configure-concur-travel-and-expense-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[Kullanıcının](#create-concur-travel-and-expense-test-user)** Azure REKLAM gösterimine bağlı Concur Travel and Expense'da B.Simon'ın bir muadili olması için Concur Seyahat ve Gider testi kullanıcısı oluşturun.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Concur Seyahat ve Gider** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde uygulama **IDP** tarafından başlatılan modda önceden yapılandırılmıştır ve gerekli URL'ler azure ile önceden doldurulmuş durumdadır. Kullanıcının **Kaydet** düğmesini tıklatarak yapılandırmayı kaydetmesi gerekir.

    > [!NOTE]
    > Tanımlayıcı (Entity ID) ve Yanıt URL'si (İddia Tüketici Hizmeti URL'si) bölgeye özgüdir. Lütfen Concur kuruluşunuzun veri merkezine göre seçim inizi seçin. Concur kuruluşunuzun veri merkezini bilmiyorsanız, lütfen Concur desteğine başvurun. 

5. **SAML sayfasıyla Tek Oturum** Açma'da, ayarları ayarlamak için **Kullanıcı Atfısı** için edit/kalem simgesini tıklatın. Benzersiz Kullanıcı Tanımlayıcısı'nın Concur kullanıcı login_id eşleşmesi gerekir. Genellikle **user.userprincipalname'yi** **user.mail**olarak değiştirmelisiniz.

    ![Kullanıcı Özniteliğini Edit](common/edit-attribute.png)

6. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federasyon Metadata XML'i** bulun ve meta verileri indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

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

Bu bölümde, B.Simon'ın Concur Travel and Expense'a erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Başvuru **listesinde, Concur Seyahat ve Gider'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-concur-travel-and-expense-sso"></a>Yapılandırma Koncur Seyahat ve Gider SSO

1. **Concur Travel and Expense** tarafında tek oturum açma yapmak için, indirilen **Federasyon Metadata XML'ini** [Concur SSO Self Servis Aracı'na](https://www.concursolutions.com/nui/authadmin/ssoadmin) yüklemeniz ve "Şirket Yöneticisi" rolü olan bir hesapla oturum açmanız gerekir. 

1. **Ekle**’ye tıklayın.
1. IdP'niz için "Azure AD (ABD)" gibi özel bir ad girin. 
1. **XML Dosyasını Yükle'yi** tıklatın ve daha önce indirdiğiniz **Federation Metadata XML'i** ekleyin.
1. Değişikliği kaydetmek için **Meta Veri Ekle'yi** tıklatın.

    ![Concur SSO self servis aracı ekran görüntüsü](./media/concur-travel-and-expense-tutorial/add-metadata-concur-self-service-tool.png)

### <a name="create-concur-travel-and-expense-test-user"></a>Concur Seyahat ve Gider test kullanıcısı oluşturma

Bu bölümde, Concur Seyahat ve Gider B.Simon adlı bir kullanıcı oluşturun. Concur Seyahat ve Gider platformundaki kullanıcıları eklemek için Concur destek ekibiyle birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir. 

> [!NOTE]
> B.Simon'ın Concur giriş kimliğinin, B.Simon'ın Azure AD'deki benzersiz tanımlayıcısı ile eşleşmesi gerekir. Örneğin, B.Simon'ın Azure AD benzersiz identifer. `B.Simon@contoso.com` B.Simon'ın Concur giriş kimliğinin `B.Simon@contoso.com` de olması gerekiyor. 

## <a name="configure-concur-mobile-sso"></a>Yapılandırma Concur Mobil SSO
Concur mobil SSO'yu etkinleştirmek için, Concur destek ekibi **Kullanıcı erişim URL'sini**vermeniz gerekir. Azure AD'den **Kullanıcı erişim URL'si** almak için aşağıdaki adımları izleyin:
1. Kurumsal **uygulamalara** gidin
1. **Concur Seyahat ve Gider'i** tıklatın
1. **Özellikler'i** tıklatın
1. **Kullanıcı erişim URL'sini** kopyalayın ve bu URL'yi Concur desteğine verin

> [!NOTE]
> SSO'yu yapılandırmak için Self Servis seçeneği mevcut değildir, bu nedenle mobil SSO'yu etkinleştirmek için Concur destek ekibiyle birlikte çalışın. 

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Concur Seyahat ve Gider döşemesini tıklattığınızda, SSO'yu kurduğunuz Concur Seyahat ve Gider'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Aynı Seyahat ve Giderleri Deneyin](https://aad.portal.azure.com/)

