---
title: "Öğretici: JIRA için Kantega SSO 'SU ile Azure Active Directory tümleştirme | Microsoft Docs"
description: JIRA için Azure Active Directory ve Kantega SSO arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.openlocfilehash: 9643d0e63e85a9b500021a415e3cdaf3edc756c5
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97608740"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-jira"></a>Öğretici: JIRA için Kantega SSO ile tümleştirme Azure Active Directory

Bu öğreticide, JIRA için Azure Active Directory (Azure AD) ile Kantega SSO 'yu nasıl tümleştirileceğini öğreneceksiniz.
JIRA için Kantega SSO 'SU Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* JIRA için Kantega SSO 'ya erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla JIRA (çoklu oturum açma) için Kantega SSO 'ya otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../manage-apps/what-is-single-sign-on.md).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

JIRA için Kantega SSO 'SU ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* JIRA çoklu oturum açma özellikli abonelik için Kantega SSO

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* JIRA için Kantega SSO **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-kantega-sso-for-jira-from-the-gallery"></a>Galeriden JIRA için Kantega SSO 'SU ekleme

JIRA için Kantega SSO 'yu Azure AD 'ye tümleştirmeyi yapılandırmak için, galerideki JIRA için Kantega SSO 'SU ile yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden JIRA için Kantega SSO eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Jira Için KANTEGA SSO** yazın, bir sonuç panelinden **Jira Için Kantega SSO** 'yu seçin, sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Sonuç listesinde JIRA için Kantega SSO](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon** adlı bir test kullanıcısına göre JIRA Için Kantega IÇIN Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve JIRA için Kantega SSO 'SU içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

JIRA için Kantega SSO 'SU ile Azure AD çoklu oturum açmayı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[JIRA çoklu oturum açma Için Kantega SSO](#configure-kantega-sso-for-jira-single-sign-on)** 'yu, uygulama tarafında tek Sign-On ayarlarını yapılandırmak için yapılandırın.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. **[Jira test kullanıcısı Için Kantega SSO oluşturma](#create-kantega-sso-for-jira-test-user)** -kullanıcının Azure AD gösterimine bağlanmış JIRA Için KANTEGA SSO 'Su Içinde Britta Simon 'un bir karşılığı.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

JIRA için Kantega SSO 'SU ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Jira uygulama tümleştirmesi Için KANTEGA SSO** sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile tek Sign-On ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    !["Tanımlayıcı" ve "Yanıt U R L" metin kutusu vurgulanmış ve "Kaydet" düğmesi seçili olan "temel S A M L yapılandırmasını" gösteren ekran görüntüsü.](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![JIRA etki alanı ve URL 'Ler çoklu oturum açma bilgileri için Kantega SSO](common/metadata-upload-additional-signon.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve Sign-On URL 'siyle güncelleştirin. Bu değerler, Öğreticinin ilerleyen kısımlarında açıklanan Jira eklentisinin yapılandırması sırasında alınır.

6. **SAML Ile tek Sign-On ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **JIRA için up Tega SSO 'Yu ayarlama** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-kantega-sso-for-jira-single-sign-on"></a>JIRA tek Sign-On için Kantega SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, JIRA şirket içi sunucunuzda yönetici olarak oturum açın.

1. Dişli üzerine gelin ve **eklentilere** tıklayın.

    !["COG" simgesinin seçili olduğunu ve açılan listeden "eklentileri" gösteren ekran görüntüsü.](./media/kantegassoforjira-tutorial/addon1.png)

1. Eklentiler sekmesi bölümünde **yeni eklentiler bul**' a tıklayın. **JIRA (saml & Kerberos) Için Kantega SSO** 'yu arayın ve yeni SAML **eklentisini yüklemek için Kaldır düğmesine tıklayın** .

    ![Arama kutusuna ve "Install" düğmesine seçili "JIRA (S A M L & Kerberos) için" yeni eklentiler bul "bölümünü gösteren ekran görüntüsü.](./media/kantegassoforjira-tutorial/addon2.png)

1. Eklenti yüklemesi başlar.

    ![Eklentiyi "yükleme" iletişim kutusunun gösterildiği ekran görüntüsü.](./media/kantegassoforjira-tutorial/addon3.png)

1. Yükleme tamamlandıktan sonra. **Kapat**’a tıklayın.

    !["Yüklü ve hazırlanmaya başlamaya!" gösteren ekran görüntüsü "Kapat" eylemi seçili iletişim kutusu.](./media/kantegassoforjira-tutorial/addon33.png)

1.  **Yönet**'e tıklayın.

    !["Yönet" düğmesi seçili "Kantega S S O" uygulama sayfasını gösteren ekran görüntüsü.](./media/kantegassoforjira-tutorial/addon34.png)
    
1. Yeni eklenti **tümleştirmeler** altında listelenmiştir. Yeni eklentiyi yapılandırmak için **Yapılandır** ' a tıklayın.

    ![Sol taraftaki gezinti menüsünde "TÜMLEŞTIRMELER" ve "Eklentileri Yönet" bölümünde seçilen "Yapılandır" düğmesinin gösterildiği ekran görüntüsü.](./media/kantegassoforjira-tutorial/addon35.png)

1. **SAML** bölümünde. **Kimlik sağlayıcısı ekle** açılır listesinden **Azure ACTIVE DIRECTORY (Azure AD)** öğesini seçin.

    !["Kimlik sağlayıcısı ekle" açılır listesini "Azure Active Directory (Azure A D)" seçiliyken gösteren ekran görüntüsü.](./media/kantegassoforjira-tutorial/addon4.png)

1. Abonelik düzeyini **temel** olarak seçin.

    !["Temel" seçiliyken "Azure A D 'yi hazırlama" bölümünü gösteren ekran görüntüsü.](./media/kantegassoforjira-tutorial/addon5.png)

1. **Uygulama özellikleri** bölümünde aşağıdaki adımları gerçekleştirin: 

    !["App ı D U R L" metin kutusu ve Kopyala düğmesi vurgulanmış ve "Ileri" düğmesi seçili "uygulama özellikleri" bölümünü gösteren ekran görüntüsü.](./media/kantegassoforjira-tutorial/addon6.png)

    1. **Uygulama KIMLIĞI URI** değerini kopyalayın ve Azure Portal IÇINDEKI **temel SAML yapılandırması** bölümünde **kimlik, yanıt URL 'si ve Sign-On URL 'si** olarak kullanın.

    1. **İleri**’ye tıklayın.

1. **Meta veri içeri aktarma** bölümünde aşağıdaki adımları gerçekleştirin: 

    !["Bilgisayarımdaki meta veri dosyası" bölümünün seçildiği "meta veri alma" bölümünü gösteren ekran görüntüsü.](./media/kantegassoforjira-tutorial/addon7.png)

    1. Bilgisayarımdaki **meta veri dosyasını** seçin ve Azure Portal 'ten indirdiğiniz meta veri dosyasını karşıya yükleyin.

    1. **İleri**’ye tıklayın.

1. **Ad ve SSO konumu** bölümünde aşağıdaki adımları gerçekleştirin:

    !["Kimlik sağlayıcı adı" metin kutusu vurgulanmış ve "Ileri" düğmesi seçili olan "ad ve S S O konumunu" gösteren ekran görüntüsü.](./media/kantegassoforjira-tutorial/addon8.png)

    1. Kimlik sağlayıcısı **adı** metin kutusuna kimlik sağlayıcısının adını ekleyin (ör. Azure AD).

    1. **İleri**’ye tıklayın.

1. Imzalama sertifikasını doğrulayın ve **İleri**' ye tıklayın.

    !["Ileri" düğmesi seçili "Imza doğrulama" bölümünü gösteren ekran görüntüsü.](./media/kantegassoforjira-tutorial/addon9.png)

1. **Jira Kullanıcı hesapları** bölümünde aşağıdaki adımları uygulayın:

    !["Jira 'nın dahili dizini 'nde Kullanıcı oluşturma" seçeneği vurgulanmış ve "Ileri" düğmesi seçili olan "JIRA Kullanıcı hesaplarını" gösteren ekran görüntüsü.](./media/kantegassoforjira-tutorial/addon10.png)

    1. **Gerekirse JIRA 'nın Iç dizininde kullanıcı oluştur** ' u seçin ve Kullanıcı için grubun uygun adını girin (birden çok No olabilir. virgülle ayrılmış gruplar).

    1. **İleri**’ye tıklayın.

1. **Finish (Son)** düğmesine tıklayın.

    !["Son" düğmesinin seçili olduğu "Özet" bölümünü gösteren ekran görüntüsü.](./media/kantegassoforjira-tutorial/addon11.png)

1. **Azure AD Için bilinen etki alanları** bölümünde aşağıdaki adımları uygulayın:

    ![Tek Sign-On yapılandırma](./media/kantegassoforjira-tutorial/addon12.png)

    1. Sayfanın sol panelinden **bilinen etki alanları ' nı** seçin.

    2. **Bilinen etki alanları** metin kutusuna etki alanı adını girin.

    3. **Kaydet**’e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    1. **Ad** alanına **Brittasıon** girin.

    1. **Kullanıcı adı** alanına yazın `brittasimon@yourcompanydomain.extension` . Örneğin, BrittaSimon@contoso.com

    1. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, JIRA için Kantega SSO 'SU erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı SEÇIN ve ardından **Jira için Kantega SSO**' yı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **JIRA Için Kantega SSO**' yı seçin.

    ![Uygulamalar listesindeki JIRA bağlantısı için Kantega SSO 'SU](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-kantega-sso-for-jira-test-user"></a>JIRA test kullanıcısı için Kantega SSO oluştur

Azure AD kullanıcılarının JIRA 'da oturum açmasını sağlamak için, JIRA ' da sağlanması gerekir. JIRA için Kantega SSO 'da, sağlama el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. JIRA şirket içi sunucunuzda yönetici olarak oturum açın.

1. Dişli 'ye gelin ve **Kullanıcı yönetimine** tıklayın.

    !["COG" simgesinin seçili olduğunu ve açılan listeden "Kullanıcı Yönetimi" seçeneğini gösteren ekran görüntüsü.](./media/kantegassoforjira-tutorial/user1.png) 

1. **Kullanıcı yönetimi** sekmesi bölümünde **Kullanıcı oluştur**' a tıklayın.

    !["Kullanıcı oluşturma" düğmesinin seçili olduğu "Kullanıcı Yönetimi" bölümünü gösteren ekran görüntüsü.](./media/kantegassoforjira-tutorial/user2.png) 

1. **"Yeni Kullanıcı Oluştur"** iletişim sayfasında, aşağıdaki adımları uygulayın:

    ![Çalışan Ekle](./media/kantegassoforjira-tutorial/user3.png) 

    1. **E-posta adresi** metin kutusuna, gibi kullanıcının e-posta adresini yazın Brittasimon@contoso.com .

    2. **Tam ad** metin kutusuna, Britta Simon gibi kullanıcının tam adını yazın.

    3. Kullanıcı **adı** metin kutusuna, gibi kullanıcının e-postasını yazın Brittasimon@contoso.com .

    4. **Parola** metin kutusuna kullanıcının parolasını yazın.

    5. **Kullanıcı oluştur**' a tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde JIRA kutucuğunun Kantega SSO 'SU ' ne tıkladığınızda, SSO 'yu ayarladığınız JIRA için Kantega SSO 'ya otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](./tutorial-list.md)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

- [Azure Active Directory Koşullu erişim nedir?](../conditional-access/overview.md)