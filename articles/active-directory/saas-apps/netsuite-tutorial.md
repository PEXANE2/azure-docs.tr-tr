---
title: 'Öğretici: NetSuite ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve NetSuite arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/17/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e462197bac854004aaf2d2f0f96e121ed081581a
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68967249"
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>Öğretici: NetSuite ile tümleştirme Azure Active Directory

Bu öğreticide, NetSuite 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
NetSuite 'i Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* NetSuite 'e erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla NetSuite (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini NetSuite ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* NetSuite çoklu oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* NetSuite, **IDP** tarafından başlatılan SSO 'yu destekliyor
* NetSuite **, tam zamanında** Kullanıcı sağlamasını destekler
* NetSuite [Otomatik Kullanıcı sağlamasını](NetSuite-provisioning-tutorial.md) destekler

## <a name="adding-netsuite-from-the-gallery"></a>Galeriden NetSuite ekleme

NetSuite 'in Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden NetSuite 'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden NetSuite eklemek için aşağıdaki adımları uygulayın:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Netsuite**yazın, sonuç panelinden **Netsuite** ' i seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

     ![Sonuç listesinde NetSuite](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına dayanarak Netsuite ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve NetSuite içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açma 'yı NetSuite ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. **[NetSuite çoklu oturum açmayı yapılandırma](#configure-netsuite-single-sign-on)** uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. **[Netsuite test kullanıcısı oluşturun](#create-netsuite-test-user)** -Netsuite 'Te kullanıcının Azure AD gösterimine bağlı Britta Simon 'un bir karşılığı olacak.
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı NetSuite ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Netsuite** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![NetSuite etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-reply.png)

    **Yanıt URL** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:

    `https://<tenant-name>.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.NetSuite.com/saml2/acs`

    `https://<tenant-name>.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na1.sandbox.NetSuite.com/saml2/acs`

    `https://<tenant-name>.na2.sandbox.NetSuite.com/saml2/acs`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek yanıt URL 'siyle güncelleştirin. Değeri almak için [Netsuite istemci desteği ekibine](https://www.netsuite.com/portal/services/support-services/suitesupport.shtml) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. NetSuite uygulaması, SAML onaylamalarını belirli bir biçimde bekliyor. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu özniteliklerin değerlerini, uygulama tümleştirme sayfasındaki **Kullanıcı öznitelikleri** bölümünden yönetebilirsiniz. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **Kullanıcı öznitelikleri** Iletişim kutusunu açmak için **Düzenle** düğmesine tıklayın.

    ![image](common/edit-attribute.png)

6. **Kullanıcı öznitelikleri** Iletişim kutusundaki **Kullanıcı talepleri** bölümünde, yukarıdaki görüntüde gösterildiği gibi, **Düzen simgesini** kullanarak talepleri DÜZENLEYIN veya aşağıdaki resimde gösterildiği gibi SAML belirteci özniteliğini yapılandırmak için **yeni talep Ekle** ' yi kullanarak talepleri ekleyin ve aşağıdaki adımları gerçekleştirin:
    
    | Ad | Kaynak özniteliği | 
    | ---------------| --------------- |
    | hesap  | `account id` |

    a. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' ye tıklayın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını yazın.

    c. **Ad alanını** boş bırakın.

    d. **Öznitelik**olarak kaynak seçin.

    e. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini yazın.

    f. Tıklayın **Tamam**

    g. **Kaydet**’e tıklayın.

    >[!NOTE]
    >Hesap özniteliğinin değeri gerçek değil. Bu değeri, Öğreticinin ilerleyen kısımlarında açıklanan şekilde güncelleşceksiniz.

4. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **NetSuite 'ı ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-netsuite-single-sign-on"></a>NetSuite çoklu oturum açmayı yapılandırma

1. Tarayıcınızda yeni bir sekme açın ve NetSuite şirket sitenizde yönetici olarak oturum açın.

2. Sayfanın üst kısmındaki araç çubuğunda, **Kurulum**' a tıklayın, ardından **şirkete** gidin ve **özellikleri etkinleştir**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-setupsaml.png)

3. Sayfanın ortasındaki araç çubuğunda **Suitecloud**' a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-suitecloud.png)

4. **Kimlik doğrulamasını Yönet** bölümünde, Netsuite 'TEKI SAML çoklu oturum açma seçeneğini etkinleştirmek IÇIN **SAML çoklu oturum açma** ' yı seçin.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-ticksaml.png)

5. Sayfanın üst kısmındaki araç çubuğunda, **Kurulum**' a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-setup.png)

6. **Kurulum görevleri** listesinden **tümleştirme**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-integration.png)

7. **KIMLIK doğrulamasını Yönet** bölümünde **SAML çoklu oturum açma**' ya tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-saml.png)

8. **SAML kurulum** sayfasında **Netsuite yapılandırma** bölümünde aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-saml-setup.png)
  
    a. **BIRINCIL KIMLIK doğrulama yöntemini**seçin.

    b. **SAMLV2 ıDENTITY PROVIDER meta verileri**etiketli alan için **ıDP meta veri dosyasını karşıya yükle**' yi seçin. Ardından, Azure portal indirdiğiniz meta veri dosyasını karşıya yüklemek için **Araştır** ' a tıklayın.

    c. **Gönder**'e tıklayın.

9. NetSuite 'te **Kurulum** ' a tıklayın ve ardından üst gezinti menüsünde **Şirket bilgileri** ' ne tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-com.png)

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-account-id.png)

    b. Sağ sütundaki **Şirket bilgileri** SAYFASıNDA **hesap kimliğini**kopyalayın.

    c. NetSuite hesabından kopyaladığınız **hesap kimliğini** Azure AD 'de **öznitelik değeri** alanına yapıştırın. 

10. Kullanıcıların NetSuite 'te çoklu oturum açma işlemi gerçekleştirebilmesi için önce NetSuite 'e uygun izinlerin atanması gerekir. Bu izinleri atamak için aşağıdaki yönergeleri izleyin.

    a. Üst gezinti menüsünde, **Kurulum**' a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-setup.png)

    b. Sol gezinti menüsünde **Kullanıcılar/roller**' i seçin ve ardından **rolleri Yönet**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-manage-roles.png)

    c. **Yeni rol**' e tıklayın.

    d. Yeni rolünüz için bir **ad** yazın.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-new-role.png)

    e. **Kaydet**’e tıklayın.

    f. Üstteki menüde **izinler**' e tıklayın. Ardından **Kurulum**' a tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-sso.png)

    g. **SAML çoklu oturum açma**' yı seçin ve ardından **Ekle**' ye tıklayın.

    h. **Kaydet**’e tıklayın.

    i. Üst gezinti menüsünde, **Kurulum**' a ve ardından **Kurulum Yöneticisi**' ne tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-setup.png)

    j. Sol gezinti menüsünde **Kullanıcılar/roller**' i seçin ve ardından **Kullanıcıları Yönet**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-manage-users.png)

    k. Bir test kullanıcısı seçin. Ardından **Düzenle** ' ye tıklayın ve ardından **erişim** sekmesine gidin.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-edit-user.png)

    girişindeki. Roller iletişim kutusunda, oluşturduğunuz uygun rolü atayın.

    ![Çoklu oturum açmayı yapılandırın](./media/NetSuite-tutorial/ns-add-role.png)

    m. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına **bricompansıon\@yourcompanydomain. Extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d.           **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, NetSuite 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **Netsuite**' i seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Netsuite**' i yazın ve seçin.

    ![Uygulamalar listesindeki NetSuite bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-netsuite-test-user"></a>NetSuite test kullanıcısı oluştur

Bu bölümde, NetSuite 'te Britta Simon adlı bir Kullanıcı oluşturulur. NetSuite, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir Kullanıcı NetSuite 'de zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde NetSuite kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız NetSuite 'e otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Kullanıcı sağlamayı yapılandırma](NetSuite-provisioning-tutorial.md)

