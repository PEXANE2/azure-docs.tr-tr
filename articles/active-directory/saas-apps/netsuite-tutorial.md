---
title: 'Öğretici: NetSuite ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve NetSuite arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/22/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6a920e58f1ffd4c3e3e9769bf6346100a8677b90
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76760070"
---
# <a name="tutorial-integrate-azure-ad-single-sign-on-sso-with-netsuite"></a>Öğretici: Azure AD tek oturum açma (SSO) ile NetSuite'i tümleştirin

Bu eğitimde, NetSuite'i Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. NetSuite'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* NetSuite erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla NetSuite'te otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda, yani Azure portalında yönetin.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için [bkz.](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* NetSuite tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. 

NetSuite destekler:

* IDP tarafından başlatılan SSO.
* JIT (tam zamanında) kullanıcı sağlama.
* [Otomatik kullanıcı sağlama](NetSuite-provisioning-tutorial.md).
* NetSuite'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimlerini uygulayabilirsiniz. Oturum denetimleri Koşullu Erişim'den itibaren genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeri olduğundan, yalnızca bir örnek bir kiracı da yapılandırılabilir.

## <a name="add-netsuite-from-the-gallery"></a>Galeriden NetSuite ekle

NetSuite'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden NetSuite'i yönetilen SaaS uygulamaları listenize aşağıdakileri yaparak ekleyin:

1. [Azure portalında](https://portal.azure.com) bir iş veya okul hesabı veya kişisel bir Microsoft hesabıyla oturum açın.
1. Sol bölmede **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni bir uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **NetSuite** yazın.
1. Sonuç bölmesinde **NetSuite'i**seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-netsuite"></a>NetSuite için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu NetSuite ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile NetSuite'teki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu NetSuite ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)
    * Azure AD oturum açma yı kullanıcı B.Simon ile test etmek için [bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)  
    * Kullanıcı B.Simon'Un Azure AD tek oturum açma kullanmasını sağlamak için [Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)
1. [NetSuite SSO'yu](#configure-netsuite-sso) uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
    * NetSuite'te, kullanıcının Azure AD gösterimine bağlı bir kullanıcı B.Simon muadili olması için [NetSuite test kullanıcısını oluşturun.](#create-the-netsuite-test-user)
1. Yapılandırmanın çalıştığını doğrulamak için [SSO'yu test](#test-sso) edin.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdakileri yapın:

1. Azure [portalında,](https://portal.azure.com/) **NetSuite** uygulama tümleştirme sayfasında, **Yönet** bölümünü arayın ve ardından **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** bölmesinde **SAML'yi**seçin.
1. **SAML bölmesiyle Tek Oturum Açma'da** **Temel SAML Yapılandırmasının**yanındaki **Düzenleme** ("kalem") simgesini seçin.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, **Yanıtla URL** metin kutusuna aşağıdaki biçimlerden birine bir URL yazın:

    ||
    |-|
    | `https://<Account ID>.NetSuite.com/saml2/acs`|
    | `https://<Account ID>.na1.NetSuite.com/saml2/acs`|
    | `https://<Account ID>.na2.NetSuite.com/saml2/acs`|
    | `https://<Account ID>.sandbox.NetSuite.com/saml2/acs`|
    | `https://<Account ID>.na1.sandbox.NetSuite.com/saml2/acs`|
    | `https://<Account ID>.na2.sandbox.NetSuite.com/saml2/acs`|

    * ** < Netsuite Configuration altında `Account ID` ** 8. Tam etki alanını bulacaksınız (bu durumda system.na0.netsuite.com gibi).

        ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/domain-value.png)

        > [!NOTE]
        > Önceki URL'lerde değerler gerçek değildir. Bunları gerçek Yanıt URL'si ile güncelleştirin. Değeri almak için [NetSuite Client destek ekibine](http://www.netsuite.com/portal/services/support-services/suitesupport.shtml)başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen biçimlere de bakabilirsiniz.

1. NetSuite uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, NetSuite uygulaması aşağıda gösterilen SAML yanıtı geri geçirilecek birkaç daha fazla öznitelikleri bekliyor. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak özniteliği |
    | ---------------| --------------- |
    | account  | `account id` |

    > [!NOTE]
    > Hesap özniteliğinin değeri gerçek değildir. Bu öğreticide daha sonra açıklandığı gibi, bu değeri güncelleştireceksiniz.

1. SAML İmza Sertifikası bölümünde SAML ile tek oturum açma'da Federation Metadata XML'i bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için İndir'i seçin.

    ![Sertifika İndirme bağlantısı](common/metadataxml.png)

1. **NetSuite'i Ayarla** bölümünde, gereksiniminize bağlı olarak uygun URL'yi veya URL'leri kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalının sol bölmesinde **Azure Etkin Dizin** > **Kullanıcıları** > **Tüm kullanıcıları**seçin.

1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

1. **Kullanıcı** özellikleri bölmesinde aşağıdaki adımları izleyin:

   a. **Ad** kutusuna, **B.Simon**girin.  
   b. Kullanıcı **adı** kutusuna (örneğin, username@companydomain.extension B.Simon@contoso.com) girin.  
   c. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.  
   d. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Kullanıcı B.Simon'ın NetSuite'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **NetSuite'i**seçin.
1. Genel bakış bölmesinde, **Yönet** bölümünü arayın ve ardından **Kullanıcılar ve gruplar** bağlantısını seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi** seçin ve ardından **Atama Ekle** bölmesinde Kullanıcılar **ve gruplar'ı**seçin.

    !["Kullanıcı ekle" düğmesi](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** bölmesinde, **Kullanıcılar** açılır listesinde **B.Simon'ı**seçin ve ardından ekranın altındaki **Seç** düğmesini seçin.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, aşağıdakileri yapın:

   a. Rolü **Seç** bölmesinde, açılır listede, kullanıcı için uygun rolü seçin.  
   b. Ekranın alt kısmında **Seç** düğmesini seçin.
1. Atama **Ekle** bölmesinde Atama **düğmesini** seçin.

## <a name="configure-netsuite-sso"></a>NetSuite SSO'ya yapılandır

1. Tarayıcınızda yeni bir sekme açın ve NetSuite şirket sitenizde yönetici olarak oturum açın.

2. Üstteki gezinti çubuğunda **Kurulum'u**seçin ve ardından **Şirket** > **Özelliklerini Etkinleştir'i**seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-setupsaml.png)

3. Sayfanın ortasındaki araç çubuğunda **SuiteCloud'u**seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-suitecloud.png)

4. **Kimlik Doğrulamayı Yönet**altında, NetSuite'te SAML tek oturum açma seçeneğini etkinleştirmek için **SAML Tek Oturum Açma** onay kutusunu seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-ticksaml.png)

5. Üst teki gezinti çubuğunda **Kurulum'u**seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-setup.png)

6. Kurulum **Görevleri** listesinde **Tümleştirme'yi**seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-integration.png)

7. **Kimlik Doğrulamayı Yönet**altında **SAML Tek Oturum Açma'yı**seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-saml.png)

8. **NETSuite Configuration**altında **SAML Kurulum** bölmesinde aşağıdakileri yapın:

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-saml-setup.png)
  
    a. Birincil **Kimlik Doğrulama Yöntemi** onay kutusunu seçin.

    b. **SAMLV2 Identity Provider Metadata**altında, **Yükle IDP Meta veri dosyasını**seçin ve ardından Azure portalından indirdiğiniz meta veri dosyasını yüklemek için **Gözat'ı** seçin.

    c. **Gönder**’i seçin.

9. NetSuite üst gezinti çubuğunda **Kurulum'u**seçin ve ardından **Şirket** > **Bilgileri'ni**seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-com.png)

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-account-id.png)

    b. Şirket **Bilgileri** bölmesinde, sağ sütunda **Hesap Kimliği** değerini kopyalayın.

    c. NetSuite hesabından kopyaladığınız **Hesap Kimliğini** Azure AD'deki **Öznitelik Değeri** kutusuna yapıştırın.

10. Kullanıcılar NetSuite'te tek oturum açma gerçekleştirmeden önce NetSuite'te uygun izinlere atanmalıdır. Bu izinleri atamak için aşağıdakileri yapın:

    a. Üst teki gezinti çubuğunda **Kurulum'u**seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-setup.png)

    b. Sol **bölmede, Kullanıcılar/Roller'i**seçin ve ardından **Rolleri Yönet'i**seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-manage-roles.png)

    c. **Yeni Rol**seçin.

    d. Yeni rol için bir **Ad** girin.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-new-role.png)

    e. **Kaydet'i**seçin.

    f. Üst gezinti çubuğunda **İzinler'i**seçin. Ardından **Kurulum'u**seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-sso.png)

    g. **SAML Tek Oturum Açma'yı**seçin ve sonra **Ekle'yi**seçin.

    h. **Kaydet'i**seçin.

    i. Üstteki gezinti çubuğunda **Kurulum'u**seçin ve ardından **Kurulum Yöneticisi'ni**seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-setup.png)

    j. Sol **bölmede, Kullanıcıları/Rolleri**seçin ve ardından **Kullanıcıları Yönet'i**seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-manage-users.png)

    k. Bir test kullanıcısı seçin, **Edit'i**seçin ve ardından **Access** sekmesini seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-edit-user.png)

    l. **Roller** bölmesinde, oluşturduğunuz uygun rolü atayın.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-add-role.png)

    m. **Kaydet'i**seçin.

### <a name="create-the-netsuite-test-user"></a>NetSuite test kullanıcısını oluşturun

Bu bölümde, NetSuite'te B.Simon adında bir kullanıcı oluşturulur. NetSuite, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yok. NetSuite'te bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki NetSuite döşemesini seçtiğinizde, SSO'yu kurduğunuz NetSuite'te otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [Azure AD ile NetSuite'i deneyin](https://aad.portal.azure.com/)
- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [NetSuite'i gelişmiş görünürlük ve kontrollerle nasıl koruyabilirsiniz?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)