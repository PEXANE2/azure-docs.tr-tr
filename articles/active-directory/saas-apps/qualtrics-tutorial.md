---
title: 'Öğretici: SAP Qualtrics ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SAP Qualtrics arasında tek oturum açma yı nasıl yapılandırılayarıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 4df889ab-2685-4d15-a163-1ba26567eeda
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/03/2020
ms.author: jeedes
ms.openlocfilehash: 2e27d020bd25f234d084fba770e234bdccb40a99
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81682385"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sap-qualtrics"></a>Öğretici: SAP Qualtrics ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, SAP Qualtrics'in Azure Etkin Dizin (Azure AD) ile nasıl entegre edileceğinizi öğreneceksiniz. SAP Qualtrics'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* SAP Qualtrics'e erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla SAP Qualtrics'te otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portalı.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için gerekli olanlar:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Tek oturum açma (SSO) için etkinleştirilen BIR SAP Qualtrics aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* SAP Qualtrics, **SP** ve **IDP'nin** başlattığı SSO'yı destekler.
* SAP Qualtrics, **Just In Time** kullanıcı sağlamayı destekler.
* SAP Qualtrics'i yapılandırıldıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi koşullu erişimden genişler. Daha fazla bilgi için bkz. [Microsoft Bulut Uygulama Güvenliği ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="add-sap-qualtrics-from-the-gallery"></a>Galeriden SAP Qualtrics ekleme

SAP Qualtrics'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden SAP Qualtrics'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabı veya kişisel bir Microsoft hesabı kullanarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol bölmede Azure **Etkin Dizini'ni**seçin.
1. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **SAP Qualtrics** yazın.
1. Sonuçlardan **SAP Qualtrics'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-sap-qualtrics"></a>SAP Qualtrics için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak SAP Qualtrics ile yapılandırın ve test edin. SSO'nun çalışması için, BIR Azure AD kullanıcısı ile SAP Qualtrics'teki ilgili kullanıcı arasında bağlantılı bir ilişki kurmanız gerekir.

Azure AD SSO'yu SAP Qualtrics ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)
    1. Azure AD oturum açma'yı B.Simon ile test etmek için [bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için [Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)
1. [Sap Qualtrics SSO'yu](#configure-sap-qualtrics-sso) uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
    1. [SAP Qualtrics'teki](#create-sap-qualtrics-test-user) B.Simon'ın bir örneğine sahip olmak için bir SAP Qualtrics test kullanıcısı oluşturun ve kullanıcının Azure AD gösterimine bağlanın.
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için [SSO'yu test](#test-sso) edin.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **SAP Qualtrics** uygulama tümleştirme sayfasında **Yönet** bölümünü bulun. **Tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için kalem simgesini seçin.

   ![Saml sayfası yla Tek Oturum Açma'nın ekran görüntüsü, kalem simgesi vurgulanmış](common/edit-urls.png)

1. **SAML** ile tek oturum açma'da, **uygulamayı IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:
    
    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanan bir URL yazın:

    `https://< DATACENTER >.qualtrics.com`
   
    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanan bir URL yazın:

    `https://< DATACENTER >.qualtrics.com/login/v1/sso/saml2/default-sp`

    c. **Röle Durumu** metin kutusuna, aşağıdaki deseni kullanan bir URL yazın:

    `https://< brandID >.< DATACENTER >.qualtrics.com`

1. **Ek URL'ler ayarla'yı**seçin ve uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna, aşağıdaki deseni kullanan bir URL yazın:

    `https://< brandID >.< DATACENTER >.qualtrics.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı, Yanıt URL'si ve Röle Durumu ile güncelleştirin. Bu değerleri almak için [Qualtrics Client destek ekibine](https://www.qualtrics.com/support/)başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum** açma'da, App Federation **Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyalama simgesini seçin.

    ![Kopya simgesi vurgulanmış SAML İmzalama Sertifikası ekran görüntüsü](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizin** > **Kullanıcıları** > **Tüm kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından parolayı aşağı yazın.
   1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Sap Qualtrics'e erişim sağlayarak B.Simon'ın Azure tek oturum açma'yı kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar** > **Tüm uygulamaları**seçin.
1. Uygulamalar listesinde **SAP Qualtrics'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   ![Kullanıcılar ve gruplar vurgulanırken Yönet bölümünün ekran görüntüsü](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin. Ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar**seçin.

    ![Kullanıcı ekle ile Kullanıcılar ve gruplar sayfasının ekran görüntüsü](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinden **B.Simon'ı** seçin. Ardından ekranın alt kısmında **Seç'i** seçin.
1. SAML iddiasında, **Rolü Seç** iletişim kutusunda herhangi bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Ardından ekranın alt kısmında **Seç'i** seçin.
1. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

## <a name="configure-sap-qualtrics-sso"></a>SAP Qualtrics SSO'nun yapılandırılması

SAP Qualtrics tarafında tek oturum açma yapılandırmak için, kopyalanan **App Federation Metadata Url'sini** Azure portalından [SAP Qualtrics destek ekibine](https://www.qualtrics.com/support/)gönderin. Destek ekibi, SAML SSO bağlantısının her iki tarafta da düzgün şekilde ayarlanınmasını sağlar.

### <a name="create-sap-qualtrics-test-user"></a>SAP Qualtrics test kullanıcıoluşturma

SAP Qualtrics, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. İşlem yapmak için ek bir işlem yoktur. SAP Qualtrics'te bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panel'i kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Panel'deki SAP Qualtrics karosu seçtiğinizde, SSO'yu kurduğunuz SAP Qualtrics'te otomatik olarak oturum açmış olursunuz. Daha fazla bilgi [için, Windows Portalım portalından Oturum Aç ve uygulamaları başlat'](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)a bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile SAP Qualtrics'i deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [SAP Qualtrics'i gelişmiş görünürlük ve kontrollerle koruyun](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

