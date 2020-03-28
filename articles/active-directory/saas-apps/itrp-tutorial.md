---
title: 'Öğretici: ITRP ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Bu eğitimde, Azure Active Directory ve ITRP arasında tek oturum açma yı nasıl yapılandırabileceğinizi öğreneceksiniz.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e09716a3-4200-4853-9414-2390e6c10d98
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: d44391624e29d2bdd182bb07452e0e8def2d1407
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67656687"
---
# <a name="tutorial-azure-active-directory-integration-with-itrp"></a>Öğretici: ITRP ile Azure Active Directory entegrasyonu

Bu eğitimde, ITRP'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Bu tümleştirme şu avantajları sağlar:

* BTRP'ye kimlerin erişebileceğini denetlemek için Azure AD'yi kullanabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla BTRP'de (tek oturum açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz: Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek [için Azure Active Directory'deki uygulamalarda tek](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)oturum açma'ya bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini BTRP ile yapılandırmak için şunları yapmanız gerekir:

* Azure AD aboneliği. Azure REKLAM ortamınız [yoksa, ücretsiz](https://azure.microsoft.com/free/)bir hesap alabilirsiniz.
* Tek oturum açma özelliği etkin olan bir ITRP aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD oturum açma işlemlerini bir test ortamında yapılandıracak ve sınayacaksınız.

* ITRP, SP tarafından başlatılan SSO'yı destekler.

## <a name="add-itrp-from-the-gallery"></a>Galeriden ITRP ekleme

ITRP'nin Azure AD'ye entegrasyonunu ayarlamak için, BTRP'yi galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com), sol bölmede, **Azure Etkin Dizini'ni**seçin:

    ![Azure Active Directory'yi seçin](common/select-azuread.png)

2. Kurumsal **uygulamalara** > git**Tüm uygulamalar:**

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

3. Uygulama eklemek için pencerenin üst kısmında **Yeni uygulama'yı** seçin:

    ![Yeni uygulama seçin](common/add-new-app.png)

4. Arama kutusuna **ITRP**girin. Arama sonuçlarında **ITRP'yi** seçin ve ardından **Ekle'yi**seçin.

     ![Arama sonuçları](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Britta Simon adlı bir test kullanıcısı kullanarak Azure AD oturum açma işlemini BTRP ile yapılandıracak ve sınayacaksınız.
Tek oturum açmayı etkinleştirmek için, bir Azure REKLAM kullanıcısı ile BTRP'deki ilgili kullanıcı arasında bir ilişki kurmanız gerekir.

Azure AD oturumlarını BTRP ile yapılandırmak ve test etmek için şu adımları tamamlamanız gerekir:

1. Azure AD oturumunu, kullanıcılarınız için özelliği etkinleştirmek için **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafında ITRP tek oturum açma yı **[yapılandırın.](#configure-itrp-single-sign-on)**
3. Azure AD tek oturum açma'yı test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Azure AD test kullanıcısını, kullanıcı için Azure AD oturum açmayı etkinleştirmek için **[atayın.](#assign-the-azure-ad-test-user)**
5. Kullanıcının Azure AD gösterimine bağlı **[bir ITRP test kullanıcısı oluşturun.](#create-an-itrp-test-user)**
6. Yapılandırmanın çalıştığını doğrulamak için **[tek oturum](#test-single-sign-on)** açma yı test edin.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini BTRP ile yapılandırmak için şu adımları izleyin:

1. Azure [portalında](https://portal.azure.com/), ITRP uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin:

    ![Tek oturum açma'yı seçin](common/select-sso.png)

2. Tek **oturum** açma yöntemi iletişim kutusunu seç'te, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin:

    ![Tek bir oturum açma yöntemi seçin](common/select-saml-option.png)

3. **SAML** ile Tek Oturum Açma'da, **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini seçin:

    ![Düzenle simgesi](common/edit-urls.png)

4. Temel **SAML Yapılandırma** iletişim kutusunda aşağıdaki adımları izleyin.

    ![Temel SAML Yapılandırma iletişim kutusu](common/sp-identifier.png)

    1. **URL'de Oturum Aç** kutusuna, bu desene bir URL girin:
    
       `https://<tenant-name>.itrp.com`

    1. Tanımlayıcı **(Entity ID)** kutusuna, bu desene bir URL girin:

       `https://<tenant-name>.itrp.com`

    > [!NOTE]
    > Bu değerler yer tutuculardır. Gerçek oturum açma URL'sini ve tanımlayıcıyı kullanmanız gerekir. Değerleri almak için [ITRP destek ekibine](https://www.itrp.com/support) başvurun. Azure portalındaki **Temel SAML Yapılandırma** iletişim kutusunda gösterilen desenlere de başvurabilirsiniz.

5. **SAML İmzalama Sertifikası** bölümünde, **SAML İmzalama Sertifikası** iletişim kutusunu açmak için **Edit** simgesini seçin:

    ![Düzenle simgesi](common/edit-certificate.png)

6. **SAML İmzalama Sertifikası** iletişim kutusunda, **Thumbprint** değerini kopyalayın ve kaydedin:

    ![Parmak Izi değerini kopyalama](common/copy-thumbprint.png)

7. **ITRP'yi Ayarla** bölümünde, gereksinimlerinize göre uygun URL'leri kopyalayın:

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    1. **Giriş URL'si**.

    1. **Azure AD Tanımlayıcısı**.

    1. **Çıkış URL'si**.

### <a name="configure-itrp-single-sign-on"></a>ITRP tek oturum açma yapılandırma

1. Yeni bir web tarayıcısı penceresinde, BTRP şirket sitenizde yönetici olarak oturum açın.

1. Pencerenin üst kısmında **Ayarlar** simgesini seçin:

    ![Ayarlar simgesi](./media/itrp-tutorial/ic775570.png "Ayarlar simgesi")

1. Sol bölmede Tek **Oturum Açma'yı**seçin:

    ![Tek Oturum Açma'yı seçin](./media/itrp-tutorial/ic775571.png "Tek Oturum Açma'yı seçin")

1. Tek **Oturum Açma** yapılandırma bölümünde aşağıdaki adımları izleyin.

    ![Tek İşaret-On bölümü](./media/itrp-tutorial/ic775572.png "Tek İşaret-On bölümü")

    ![Tek İşaret-On bölümü](./media/itrp-tutorial/ic775573.png "Tek İşaret-On bölümü")

    1. **Etkin**'i seçin.

    1. Uzaktan **çıkış URL** kutusuna, Azure portalından kopyaladığınız **Logout URL** değerini yapıştırın.

    1. **SAML SSO URL** kutusuna Azure portalından kopyaladığınız **Giriş URL** değerini yapıştırın.

    1. Sertifika **parmak izi** kutusuna, Azure portalından kopyaladığınız sertifikanın **Parmak Izi** değerini yapıştırın.

    1. **Kaydet'i**seçin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni** seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin:

    ![Tüm kullanıcılar'ı seçin](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** seçin:

    ![Yeni kullanıcıyı seçin](common/new-user.png)

3. **Kullanıcı** iletişim kutusunda aşağıdaki adımları izleyin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    1. **Ad** **kutusuna BrittaSimon**girin.
  
    1. Kullanıcı **adı** kutusuna, **\<şirketinizin alan\< BrittaSimon@> girin. uzantısı>**. (Örneğin, BrittaSimon@contoso.com.)

    1. **Parolayı Göster'i**seçin ve ardından **Parola** kutusundaki değeri yazın.

    1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'A ITRP erişimi vererek Azure tek oturum açma işlemini kullanasınız.

1. Azure portalında **Kurumsal uygulamaları**seçin, **Tüm uygulamaları**seçin ve ardından **ITRP'yi**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **ITRP'yi**seçin.

    ![Başvuru listesi](common/all-applications.png)

3. Sol **bölmede, Kullanıcıları ve grupları**seçin:

    ![Kullanıcı ve gruplar'ı seçin](common/users-groups-blade.png)

4. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı ekle seçeneğini belirleme](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, kullanıcı listesinde **Britta Simon'ı** seçin ve ardından pencerenin altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında, **Rolü Seç** iletişim kutusunda bir rol değeri bekliyorsanız, listeden kullanıcı için uygun rolü seçin. Pencerenin altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

### <a name="create-an-itrp-test-user"></a>BIR ITRP test kullanıcısı oluşturma

Azure AD kullanıcılarının ITRP'de oturum açabilmesini sağlamak için bunları ITRP'ye eklemeniz gerekir. Bunları el ile eklemeniz gerekir.

Bir kullanıcı hesabı oluşturmak için şu adımları izleyin:

1. ITRP kiracınızla oturum açın.

1. Pencerenin üst **kısmında, Kayıtlar** simgesini seçin:

    ![Kayıtlar simgesi](./media/itrp-tutorial/ic775575.png "Kayıtlar simgesi")

1. Menüde **Kişiler'i**seçin:

    ![Kişileri Seçin](./media/itrp-tutorial/ic775587.png "Kişileri Seçin")

1. Yeni bir kişi**+** eklemek için artı işaretini ( ) seçin:

    ![Artı işaretini seçin](./media/itrp-tutorial/ic775576.png "Artı işaretini seçin")

1. Yeni **Kişi Ekle** iletişim kutusunda aşağıdaki adımları izleyin.

    ![Yeni Kişi iletişim kutusu ekle](./media/itrp-tutorial/ic775577.png "Yeni Kişi iletişim kutusu ekle")

    1. Eklemek istediğiniz geçerli bir Azure REKLAM hesabının adını ve e-posta adresini girin.

    1. **Kaydet'i**seçin.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için BTRP tarafından sağlanan herhangi bir kullanıcı hesabı oluşturma aracını veya API'yi kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Artık Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı test etmeniz gerekir.

Access Paneli'ndeki ITRP döşemesini seçtiğinizde, SSO'yu kurduğunuz ITRP örneğinde otomatik olarak oturum açmalısınız. Access Paneli hakkında daha fazla bilgi için [Access'e bakın ve Uygulamalarım portalındaki uygulamaları kullanın.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme öğreticileri](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
