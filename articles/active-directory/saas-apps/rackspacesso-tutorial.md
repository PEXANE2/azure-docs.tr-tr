---
title: 'Öğretici: Rackspace SSO ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Rackspace SSO arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 36b398be-2f7e-4ce8-9031-53587299bc4a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: jeedes
ms.openlocfilehash: 98a160b361e316c87e61855825c2cc36fdbb06d1
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81870295"
---
# <a name="tutorial-azure-active-directory-integration-with-rackspace-sso"></a>Öğretici: Rackspace SSO ile Azure Active Directory entegrasyonu

Bu eğitimde, Rackspace SSO'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edilebildiğini öğrenirsiniz.
Rackspace SSO'yu Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Rackspace SSO erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Rackspace SSO'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini Rackspace SSO ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Rackspace SSO tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Rackspace SSO **SP** başlatılan SSO destekler

## <a name="adding-rackspace-sso-from-the-gallery"></a>Galeriden Rackspace SSO ekleme

Rackspace SSO'nun Azure AD'ye entegrasyonunu yapılandırmak için, raf alanı SSO'yu galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Rackspace SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Rackspace SSO**yazın, sonuç panelinden **Rackspace SSO'yu** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Raf alanı SSO sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Rackspace SSO ile yapılandırıp test esiniz.
Rackspace ile tek oturum açma kullanırken, Rackspace kullanıcıları Rackspace portalında ilk oturum açtıklarında otomatik olarak oluşturulur. 

Azure AD oturumaçmaişlemlerini Rackspace SSO ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Rackspace SSO Tek Oturum Açma](#configure-rackspace-sso-single-sign-on)** -uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Rackspace Denetim Masası'nda Özellik Eşleme'yi ayarlayın](#set-up-attribute-mapping-in-the-rackspace-control-panel)** ve Rackspace rolleri Azure AD kullanıcılarına atayın.
1. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Rackspace SSO ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Rackspace SSO** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** bölümünde, [URL'den](https://login.rackspace.com/federate/sp.xml) indirebileceğiniz servis sağlayıcı meta **veri dosyasını** yükleyin ve aşağıdaki adımları gerçekleştirin:

    a. **Meta veri dosyalarını yükle'yi**tıklatın.

    ![image](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosuna** tıklayın ve **Yükle'yi**tıklatın.

    ![image](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla yüklendikten sonra, gerekli url'ler otomatik olarak doldurulur.

    d. Oturum **Açma URL** metin kutusuna bir URL yazın:`https://login.rackspace.com/federate/`

    ![Rackspace SSO Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)   

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

Bu dosya, gerekli Kimlik Federasyonu yapılandırma ayarlarını doldurmak için Rackspace'e yüklenir.

### <a name="configure-rackspace-sso-single-sign-on"></a>Raf Alanı SSO Tek Oturum Açma'yı Yapılandır

**Rackspace SSO** tarafında tek oturum açma yapılandırmak için:

1. [Denetim Masasına Kimlik Sağlayıcı Ekle'deki belgelere](https://developer.rackspace.com/docs/rackspace-federation/gettingstarted/add-idp-cp/) bakın
1. Bu adımlar aracılığıyla size yol açacaktır:
    1. Yeni bir Kimlik Sağlayıcısı Oluşturma
    1. Oturum açma yaparken kullanıcıların şirketinizi tanımlamak için kullanacağı bir e-posta etki alanı belirtin.
    1. Azure denetim panelinden daha önce indirilen **Federasyon Metadata XML'ini** yükleyin.

Bu, Azure ve Rackspace'in bağlanması için gereken temel SSO ayarlarını doğru şekilde yapılandıracaktır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı `brittasimon@yourcompanydomain.extension`türünde. Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Rackspace SSO'ya erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Rackspace SSO'yu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Rackspace SSO'yu**seçin.

    ![Uygulamalar listesindeki Rackspace SSO bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="set-up-attribute-mapping-in-the-rackspace-control-panel"></a>Rackspace denetim panelinde Öznitelik Eşleme'yi ayarlama

Rackspace, Rackspace rollerini ve gruplarını tek oturum açma kullanıcılarınıza atamak için bir **Öznitelik Eşleme İlkesi** kullanır. **Öznitelik Eşleme İlkesi,** Azure AD SAML taleplerini Rackspace'in gerektirdiği kullanıcı yapılandırma alanlarına çevirir. Daha fazla belge Rackspace [Öznitelik Temelleri Haritalama belgelerinde](https://developer.rackspace.com/docs/rackspace-federation/appendix/map/)bulunabilir. Bazı hususlar:

* Azure REKLAM gruplarını kullanarak farklı düzeylerde Rackspace erişimi atamak istiyorsanız, Grupların Azure **Rackspace SSO** Tek Oturum Açma ayarlarında hak iddia etmesini etkinleştirmeniz gerekir. **Öznitelik Eşleme İlkesi** daha sonra bu grupları istenen Rackspace rolleri ve gruplarıyla eşleştirmek için kullanılır:

    ![Gruplar talep ayarları](common/sso-groups-claim.png)

* Varsayılan olarak, Azure AD, SamL talebinde Azure REKLAM Gruplarının UID'sini Grubun adı ile karşılatarak gönderir. Ancak, şirket içi Etkin Dizininizi Azure AD ile eşitlüyorsanız, grupların gerçek adlarını gönderme seçeneğiniz vardır:

    ![Gruplar talep adı ayarları](common/sso-groups-claims-names.png)

Aşağıdaki örnek **Öznitelik Eşleme İlkesi** şunları gösterir:
1. Rackspace kullanıcısının adını SAML `user.name` talebine ayarlama. Herhangi bir talep kullanılabilir, ancak bunu kullanıcının e-posta adresini içeren bir alana ayarlamak en yaygın dır.
1. Bir Azure REKLAM `admin` `billing:admin` Grubu'nu Grup Adı veya Grup UID ile eşleştirerek Rackspace rollerini ve kullanıcı üzerinde ayarlama. Alandaki *bir ikame* kullanılır ve `remote` kural ifadelerinin sonuçları yla değiştirilir. `"{0}"` `roles`
1. Rackspace'in `"{D}"` SAML değişiminde standart ve tanınmış SAML taleplerini arayarak ek SAML alanlarını almasına izin vermek için *varsayılan ikame* yi kullanma.

```yaml
---
mapping:
    rules:
    - local:
        user:
          domain: "{D}"
          name: "{At(http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name)}"
          email: "{D}"
          roles:
              - "{0}"
          expire: "{D}"
      remote:
          - path: |
              (
                if (mapping:get-attributes('http://schemas.microsoft.com/ws/2008/06/identity/claims/groups')='7269f9a2-aabb-9393-8e6d-282e0f945985') then ('admin', 'billing:admin') else (),
                if (mapping:get-attributes('http://schemas.microsoft.com/ws/2008/06/identity/claims/groups')='MyAzureGroup') then ('admin', 'billing:admin') else ()
              )
            multiValue: true
  version: RAX-1
```
> [!TIP]
> İlke dosyanızı düzenlerken YAML sözdizimini doğrulayan bir metin düzenleyicisi kullandığınızdan emin olun.

Daha fazla örnek için Rackspace [Attribute Mapping Basics belgelerine](https://developer.rackspace.com/docs/rackspace-federation/appendix/map/) bakın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Rackspace SSO döşemesini tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz Rackspace SSO'da oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

**Rackspace SSO** Tek oturum açma ayarlarında **Doğrula** düğmesini de kullanabilirsiniz:

   ![SSO Doğrulama Düğmesi](common/sso-validate-sign-on.png)

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

