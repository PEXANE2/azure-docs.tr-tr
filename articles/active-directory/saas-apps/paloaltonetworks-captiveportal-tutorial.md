---
title: 'Öğretici: Palo Alto Networks Captive Portal ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Palo Alto Networks Captive Portal arasında tek oturum açma işlemlerini nasıl yapılandırıştırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 67a0b476-2305-4157-8658-2ec3625850d5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/25/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f926741bde3bdcc69cb4ea30f54daca79606047e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73160171"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks-captive-portal"></a>Öğretici: Palo Alto Networks Captive Portal ile Azure Active Directory entegrasyonu

Bu eğitimde, Palo Alto Networks Captive Portal'ı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.

Palo Alto Networks Captive Portal'ı Azure AD ile entegre ettiğinizde aşağıdaki avantajlardan yararlanırsınız:

* Azure AD'de, Palo Alto Networks Captive Portal'a kimlerin erişebileceğini kontrol edebilirsiniz.
* Kullanıcı Azure REKLAM hesaplarını kullanarak Palo Alto Networks Captive Portal'daki (tek oturum açma) kullanıcıları otomatik olarak oturum açabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda, Azure portalında yönetebilirsiniz.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek [için Azure Active Directory'deki uygulamalarda tek oturum açma'ya](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

Azure aboneliğiniz yoksa, ücretsiz [bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD'yi Palo Alto Networks Captive Portal ile tümleştirmek için aşağıdaki öğelere ihtiyacınız vardır:

* Azure Active Directory aboneliği. Azure AD'niz yoksa, bir aylık [deneme](https://azure.microsoft.com/pricing/free-trial/)sürümü alabilirsiniz.
* Palo Alto Networks Captive Portal tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

Palo Alto Networks Captive Portal şu senaryoları destekler:

* **IDP tarafından başlatılan tek oturum açma**
* **Tam zamanında kullanıcı sağlama**

## <a name="add-palo-alto-networks-captive-portal-from-the-gallery"></a>Galeriden Palo Alto Networks Tutsak Portal ekle

Başlamak için galeride Palo Alto Networks Captive Portal'ı yönetilen SaaS uygulamaları listenize ekleyin:

1. Azure [portalında,](https://portal.azure.com)sol menüde **Azure Etkin Dizini'ni**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamaları** > seçin**Tüm uygulamalar**.

    ![Menüdeki Kurumsal uygulamalar seçeneği](common/enterprise-applications.png)

3. **Yeni uygulama**’yı seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Palo Alto Networks Captive Portal'ı**girin. Arama sonuçlarında **Palo Alto Networks - Captive Portal'ı**seçin ve ardından **Ekle'yi**seçin.

     ![Palo Alto Networks - Tutsak Portal sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

*Britta Simon*adlı bir test kullanıcısına göre Palo Alto Networks Captive Portal ile Azure AD tek oturum açma işlemini yapılandırın ve test ekarsınız. Tek oturum açmanın işe yaraması için, Palo Alto Networks Captive Portal'da bir Azure AD kullanıcısı ile aynı kullanıcı arasında bir ilişki kurmanız gerekir. 

Palo Alto Networks Captive Portal ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki görevleri tamamlayın:

1. **[Azure AD tek oturum](#configure-azure-ad-single-sign-on)** açma yapılandırma : Kullanıcının bu özelliği kullanmasını etkinleştirin.
2. **[Palo Alto Networks Captive Portal tek oturum](#configure-palo-alto-networks-captive-portal-single-sign-on)** açma : Uygulamadaki tek oturum açma ayarlarını yapılandırın.
3. **[Bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user)**: Azure AD tek oturum açma işlemini kullanıcı *Britta Simon*ile test edin.
4. **[Azure AD test kullanıcısını atama](#assign-the-azure-ad-test-user)**: Britta Simon'ı Azure AD tek oturum açma işlemini kullanacak şekilde ayarlayın.
5. **Bir Palo Alto Networks Captive Portal test kullanıcısı oluşturun**: Azure AD kullanıcısına bağlı Palo Alto Networks Captive Portal'da bir meslektaşı kullanıcı *Britta Simon* oluşturun.
6. **[Test tek oturum açma](#test-single-sign-on)**: Yapılandırmanın çalıştığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

İlk olarak, Azure portalında Azure AD oturum açma'yı etkinleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Palo Alto Networks - Captive Portal** uygulama tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** bölmesinde **SAML'yi**seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML bölmeli Tek Oturum Açma'da** **kalemi Edit** simgesini seçin.

    ![Kalemle Edit simgesi](common/edit-urls.png)

4. Temel **SAML Yapılandırma** bölmesinde aşağıdaki adımları tamamlayın:

    ![Palo Alto Networks Tutsak Portal Temel SAML Yapılandırma bölmesi](common/idp-intiated.png)

   1. **Tanımlayıcı**için, desene `https://<customer_firewall_host_name>/SAML20/SP`sahip bir URL girin.

   2. **YanıtURL'si için**desene `https://<customer_firewall_host_name>/SAML20/SP/ACS`sahip bir URL girin.

      > [!NOTE]
      > Bu adımdaki yer tutucu değerlerini gerçek tanımlayıcı ve yanıt URL'leriyle güncelleştirin. Gerçek değerleri almak için [Palo Alto Networks Captive Portal Client destek ekibiyle](https://support.paloaltonetworks.com/support)iletişime geçin.

5. **SAML İmzaSertifikası** bölümünde, **Federasyon Metadata XML**yanında, **İndir'i**seçin. İndirilen dosyayı bilgisayarınıza kaydedin.

    ![Federasyon Metadata XML indirme linki](common/metadataxml.png)

### <a name="configure-palo-alto-networks-captive-portal-single-sign-on"></a>Yapılandırılan Palo Alto Networks Captive Portal tek oturum açma

Ardından, Palo Alto Networks Captive Portal'da tek oturum açın:

1. Farklı bir tarayıcı penceresinde, palo Alto Networks web sitesinde yönetici olarak oturum açın.

2. **Aygıt** sekmesini seçin.

    ![Palo Alto Networks web sitesi Cihaz sekmesi](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin1.png)

3. Menüde **SAML Identity Provider'ı**seçin ve sonra **Içe Aktar'ı**seçin.

    ![İçe Aktarma düğmesi](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin2.png)

4. **SAML Identity Provider Server Profile Alma** iletişim kutusunda aşağıdaki adımları tamamlayın:

    ![Palo Alto Networks'e tek oturum açma](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin3.png)

    1. **Profil Adı**için **AzureAD-CaptivePortal**gibi bir ad girin.
    
    2. Kimlik **Sağlayıcı Meta verilerinin**yanında **Gözat'ı**seçin. Azure portalında indirdiğiniz metadata.xml dosyasını seçin.
    
    3. **Tamam'ı**seçin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Ardından, Azure portalında *Britta Simon* adında bir test kullanıcısı oluşturun:

1. Azure portalında Azure **Etkin Dizin** > **Kullanıcıları** > Tüm**kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. **Yeni kullanıcıyı**seçin.

    ![Yeni kullanıcı düğmesi](common/new-user.png)

3. **Kullanıcı** bölmesinde aşağıdaki adımları tamamlayın:

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    1. **Adı**için, **BrittaSimon**girin.
  
    2. **Kullanıcı adı için** **BrittaSimon\>\@\<your_company_domain**girin. Örneğin, **BrittaSimon\@contoso.com.**

    3. **Parola**için bir parola girin. Girdiğiniz parolanın kaydını tutmanızı öneririz. Parolayı görüntülemek için **Parolayı Göster** onay kutusunu seçebilirsiniz.

    4. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Ardından, Britta Simon'ın Azure tek oturum açma işlemini kullanabilmesi için Palo Alto Networks Captive Portal'a erişim izni ver:

1. Azure portalında **Kurumsal uygulamaları** > tüm**uygulamaları**seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Palo Alto Networks - Captive Portal'ı**girin ve ardından uygulamayı seçin.

    ![Palo Alto Networks - Uygulama listesinde Tutsak Portal bağlantısı](common/all-applications.png)

3. **Menüde, Kullanıcıları ve grupları**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle'yi**seçin. Ardından, **Atama Ekle** bölmesinde, Kullanıcılar **ve gruplar**seçin.

    ![Atama ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** bölmesinde, **Kullanıcı** listesinde **Britta Simon'ı**seçin. **Seç**’i seçin.

6. SAML iddiasına rol değeri eklemek için, **Rol Bölmesini** Seç'te, kullanıcı için ilgili rolü seçin. **Seç**’i seçin.

7. Atama **Ekle** bölmesinde **Atama'yı**seçin.

### <a name="create-a-palo-alto-networks-captive-portal-test-user"></a>Palo Alto Networks Captive Portal test kullanıcısı oluşturma

Ardından, Palo Alto Networks Captive Portal'da *Britta Simon* adında bir kullanıcı oluşturun. Palo Alto Networks Captive Portal, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümdeki görevleri tamamlamanız gerekmez. Bir kullanıcı Palo Alto Networks Captive Portal'da zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

> [!NOTE]
> El ile bir kullanıcı oluşturmak istiyorsanız, [Palo Alto Networks Captive Portal Client destek ekibine](https://support.paloaltonetworks.com/support)başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Palo Alto Networks Captive Portal, Windows VM'deki güvenlik duvarının arkasına yüklenir. Palo Alto Networks Captive Portal'da tek oturum açma yı test etmek için Uzak Masaüstü Protokolü (RDP) kullanarak Windows VM'de oturum açın. RDP oturumunda, bir tarayıcı açın ve herhangi bir web sitesine gidin. SSO URL'si açılır ve kimliğiniz ini sizden istenir. Kimlik doğrulaması tamamlandığında web sitelerine erişebilirsiniz.

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi edinmek için şu makalelere bakın:

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme yle ilgili eğitimler](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory'deki uygulamalarda tek oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Etkin Dizinde Koşullu Erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

