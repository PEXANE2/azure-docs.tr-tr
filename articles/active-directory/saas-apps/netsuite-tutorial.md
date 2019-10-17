---
title: 'Öğretici: NetSuite ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve NetSuite arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
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
ms.devlang: na
ms.topic: tutorial
ms.date: 09/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a9b834feacd6241e66e18a4f4e6aadc43e909c7
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72438502"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-netsuite"></a>Öğretici: NetSuite ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, NetSuite 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. NetSuite 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de NetSuite 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla NetSuite 'e otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* NetSuite çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* NetSuite, **IDP** tarafından başlatılan SSO 'yu destekliyor

* NetSuite **, tam zamanında** Kullanıcı sağlamasını destekler

* NetSuite [Otomatik Kullanıcı sağlamasını](NetSuite-provisioning-tutorial.md) destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı, tek bir kiracıda yalnızca bir örneğin yapılandırılabilmesini sağlamak için sabit bir dize değeridir.

## <a name="adding-netsuite-from-the-gallery"></a>Galeriden NetSuite ekleme

NetSuite 'in Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden NetSuite 'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Netsuite** yazın.
1. Sonuçlar panelinden **Netsuite** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-netsuite"></a>NetSuite için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu Netsuite ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve NetSuite içindeki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu NetSuite ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[NetSuite SSO 'Yu yapılandırma](#configure-netsuite-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. **[Netsuite test kullanıcısı oluşturun](#create-netsuite-test-user)** -bu, kullanıcının Azure AD gösterimine bağlı olan Netsuite 'e ait B. Simon 'a karşılık gelen bir.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Netsuite** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:

    | |
    |--|
    | `https://<tenant-name>.NetSuite.com/saml2/acs` |
    | `https://<tenant-name>.na1.NetSuite.com/saml2/acs` |
    | `https://<tenant-name>.na2.NetSuite.com/saml2/acs` |
    | `https://<tenant-name>.sandbox.NetSuite.com/saml2/acs` |
    | `https://<tenant-name>.na1.sandbox.NetSuite.com/saml2/acs` |
    | `https://<tenant-name>.na2.sandbox.NetSuite.com/saml2/acs` |

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek yanıt URL 'siyle güncelleştirin. Değeri almak için [Netsuite istemci desteği ekibine](http://www.netsuite.com/portal/services/support-services/suitesupport.shtml) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. NetSuite uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir. Kullanıcı öznitelikleri iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![image](common/edit-attribute.png)

1. NetSuite uygulaması, yukarıdakine ek olarak, SAML yanıtına daha fazla özniteliğin geri geçirilmesini bekler. Kullanıcı öznitelikleri iletişim kutusundaki Kullanıcı talepleri bölümünde aşağıdaki tabloda gösterildiği gibi SAML belirteci özniteliği eklemek için aşağıdaki adımları gerçekleştirin:

    | Adı | Kaynak özniteliği | 
    | ---------------| --------------- |
    | account  | `account id` |

    1. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' ye tıklayın.

    1. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını yazın.

    1. **Ad alanını** boş bırakın.

    1. **Öznitelik**olarak kaynak seçin.

    1. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini yazın.

    1. **Tamam 'a** tıklayın

    1. **Kaydet** düğmesine tıklayın.

    >[!NOTE]
    >Hesap özniteliğinin değeri gerçek değil. Bu değeri, Öğreticinin ilerleyen kısımlarında açıklanan şekilde güncelleşceksiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **NetSuite 'ı ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına username@companydomain.extension girin. Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, NetSuite 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Netsuite**' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-netsuite-sso"></a>NetSuite SSO 'yu yapılandırma

1. Tarayıcınızda yeni bir sekme açın ve NetSuite şirket sitenizde yönetici olarak oturum açın.

2. Sayfanın üst kısmındaki araç çubuğunda, **Kurulum**' a tıklayın, ardından **şirkete** gidin ve **özellikleri etkinleştir**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-setupsaml.png)

3. Sayfanın ortasındaki araç çubuğunda **Suitecloud**' a tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-suitecloud.png)

4. **Kimlik doğrulamasını Yönet** bölümünde, Netsuite 'TEKI SAML çoklu oturum açma seçeneğini etkinleştirmek IÇIN **SAML çoklu oturum açma** ' yı seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-ticksaml.png)

5. Sayfanın üst kısmındaki araç çubuğunda, **Kurulum**' a tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-setup.png)

6. **Kurulum görevleri** listesinden **tümleştirme**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-integration.png)

7. **KIMLIK doğrulamasını Yönet** bölümünde **SAML çoklu oturum açma**' ya tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-saml.png)

8. **SAML kurulum** sayfasında **Netsuite yapılandırma** bölümünde aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-saml-setup.png)
  
    a. **BIRINCIL KIMLIK doğrulama yöntemini**seçin.

    b. **SAMLV2 ıDENTITY PROVIDER meta verileri**etiketli alan için **ıDP meta veri dosyasını karşıya yükle**' yi seçin. Ardından, Azure portal indirdiğiniz meta veri dosyasını karşıya yüklemek için **Araştır** ' a tıklayın.

    c. **Gönder**' e tıklayın.

9. NetSuite 'te **Kurulum** ' a **tıklayın ve ardından** üst gezinti menüsünde **Şirket bilgileri** ' ne tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-com.png)

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-account-id.png)

    b. Sağ sütundaki **Şirket bilgileri** SAYFASıNDA **hesap kimliğini**kopyalayın.

    c. NetSuite hesabından kopyaladığınız **hesap kimliğini** Azure AD 'de **öznitelik değeri** alanına yapıştırın. 

10. Kullanıcıların NetSuite 'te çoklu oturum açma işlemi gerçekleştirebilmesi için önce NetSuite 'e uygun izinlerin atanması gerekir. Bu izinleri atamak için aşağıdaki yönergeleri izleyin.

    a. Üst gezinti menüsünde, **Kurulum**' a tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-setup.png)

    b. Sol gezinti menüsünde **Kullanıcılar/roller**' i seçin ve ardından **rolleri Yönet**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-manage-roles.png)

    c. **Yeni rol**' e tıklayın.

    d. Yeni rolünüz için bir **ad** yazın.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-new-role.png)

    e. **Kaydet** düğmesine tıklayın.

    f. Üstteki menüde **izinler**' e tıklayın. Ardından **Kurulum**' a tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-sso.png)

    g. **SAML çoklu oturum açma**' yı seçin ve ardından **Ekle**' ye tıklayın.

    h. **Kaydet** düğmesine tıklayın.

    i. Üst gezinti menüsünde, **Kurulum**' a ve ardından **Kurulum Yöneticisi**' ne tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-setup.png)

    j. Sol gezinti menüsünde **Kullanıcılar/roller**' i seçin ve ardından **Kullanıcıları Yönet**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-manage-users.png)

    k. Bir test kullanıcısı seçin. Ardından **Düzenle** ' ye tıklayın ve ardından **erişim** sekmesine gidin.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-edit-user.png)

    girişindeki. Roller iletişim kutusunda, oluşturduğunuz uygun rolü atayın.

    ![Çoklu oturum açmayı yapılandırma](./media/NetSuite-tutorial/ns-add-role.png)

    m. **Kaydet** düğmesine tıklayın.

### <a name="create-netsuite-test-user"></a>NetSuite test kullanıcısı oluştur

Bu bölümde, NetSuite 'te Britta Simon adlı bir Kullanıcı oluşturulur. NetSuite, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir Kullanıcı NetSuite 'de zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde NetSuite kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız NetSuite 'e otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile NetSuite 'i deneyin](https://aad.portal.azure.com/)

