---
title: 'Öğretici: Salesforce korumalı alanı ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Salesforce korumalı alanı arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/16/2020
ms.author: jeedes
ms.openlocfilehash: 277acbc84ab435ce1076c30a1e49f6ffdd2a0586
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88543729"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-salesforce-sandbox"></a>Öğretici: Salesforce korumalı alanı ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Salesforce korumalı alanını Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Salesforce korumalı alanını Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Salesforce korumalı alana erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Salesforce korumalı alanı ' na otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Salesforce korumalı alanı çoklu oturum açma (SSO) etkin abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Salesforce korumalı alanı **SP ve ıDP** tarafından başlatılan SSO 'yu destekler
* Salesforce korumalı alanı **, tam zamanında** Kullanıcı sağlamasını destekler
* Salesforce korumalı alanı [ **Otomatik** Kullanıcı sağlamayı destekler](salesforce-sandbox-provisioning-tutorial.md)
* Salesforce korumalı alanını yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve bu verileri korumayı koruyan oturum denetimleri uygulayabilirsiniz. Oturum denetimleri koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Galeriden Salesforce korumalı alanı ekleme

Salesforce korumalı alanının Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden Salesforce korumalı alanını yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Salesforce korumalı alanı** yazın.
1. Sonuçlar panelinden **Salesforce korumalı alanı** ' nı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-salesforce-sandbox"></a>Salesforce korumalı alanı için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu Salesforce korumalı alanı ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve Salesforce korumalı alanında ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Salesforce korumalı alanı ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Salesforce korumalı alanı SSO 'Yu yapılandırma](#configure-salesforce-sandbox-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    * Kullanıcının Azure AD gösterimine bağlı olan Salesforce korumalı alanında B. Simon 'a sahip olmak için **[Salesforce korumalı alanı test kullanıcısı oluşturun](#create-salesforce-sandbox-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Salesforce korumalı alan** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, **hizmet sağlayıcısı meta verileri dosyanız** varsa ve **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    a. **Meta veri dosyasını karşıya yükle**' ye tıklayın.

    ![Meta veri dosyasını karşıya yükle](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosu** ' na tıklayın ve **karşıya yükle**' ye tıklayın.

    ![meta veri dosyası seçin](common/browse-upload-metadata.png)

    > [!NOTE]
    > Hizmet sağlayıcı meta veri dosyasını, öğreticide daha sonra açıklanan Salesforce korumalı alan yönetim portalından alırsınız.

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra, yanıt URL **'si** değeri **yanıt URL 'si** metin kutusuna otomatik olarak doldurulur.

    ![image](common/both-replyurl.png)

    > [!Note]
    > **Yanıt URL 'si** değeri otomatik olarak yoksa, gereksinime göre değeri el ile girin.

5. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **Salesforce korumalı alanını ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Azure çoklu oturum açma özelliğini kullanarak Salesforce korumalı alana erişim vererek B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Salesforce korumalı alanı**' nı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-salesforce-sandbox-sso"></a>Salesforce korumalı alanı SSO 'yu yapılandırma

1. Tarayıcınızda yeni bir sekme açın ve Salesforce korumalı alanı yönetici hesabınızda oturum açın.

2. Sayfanın sağ üst köşesindeki **Ayarlar simgesi** altında **Kurulum** ' a tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/configure1.png)

3. Sol gezinti bölmesindeki **Ayarlar** ' a gidin, ilgili bölümü genişletmek için **kimlik** ' e tıklayın. Ardından **Çoklu oturum açma ayarları**' na tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

4. **Çoklu oturum açma ayarları** sayfasında **Düzenle** düğmesine tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/configure3.png)

5. **SAML etkin**' i seçin ve ardından **Kaydet**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

6. SAML çoklu oturum açma ayarlarınızı yapılandırmak için **meta veri dosyasından yeni**' ye tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

7. Azure portal indirdiğiniz meta veri XML dosyasını karşıya yüklemek için **Dosya Seç** ' e tıklayın ve **Oluştur**' a tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/xmlchoose.png)

8. **SAML çoklu oturum açma ayarları** sayfasında, alanlar otomatik olarak doldurulur ve Kaydet ' e tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/salesforcexml.png)

9. **Çoklu oturum açma ayarları** sayfasında, hizmet sağlayıcısı meta veri dosyasını Indirmek Için **meta verileri indir** düğmesine tıklayın. Yukarıda açıklandığı gibi gereken URL 'Leri yapılandırmak için Azure portal **temel SAML yapılandırması** bölümünde bu dosyayı kullanın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/configure4.png)

10. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdakiler için Önkoşullar verilmiştir:

    a. Doğrulanmış bir etki alanınız olmalıdır.

    b. Salesforce korumalı alanında etki alanınızı yapılandırmanız ve etkinleştirmeniz gerekir, bunun adımları Bu öğreticinin ilerleyen kısımlarında açıklanmıştır.

    c. Azure portal, **temel SAML yapılandırması** bölümünde **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:
  
    ![Salesforce korumalı alan etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/both-signonurl.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak değeri yazın:`https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    > [!NOTE]
    > Etki alanını etkinleştirdikten sonra bu değer Salesforce Sandbox portalından kopyalanmalıdır.

11. **SAML Imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** ' na tıklayın ve ardından XML dosyasını bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

12. Tarayıcınızda yeni bir sekme açın ve Salesforce korumalı alanı yönetici hesabınızda oturum açın.

13. Sayfanın sağ üst köşesindeki **Ayarlar simgesi** altında **Kurulum** ' a tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/configure1.png)

14. Sol gezinti bölmesindeki **Ayarlar** ' a gidin, ilgili bölümü genişletmek için **kimlik** ' e tıklayın. Ardından **Çoklu oturum açma ayarları**' na tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

15. **Çoklu oturum açma ayarları** sayfasında **Düzenle** düğmesine tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/configure3.png)

16. **SAML etkin**' i seçin ve ardından **Kaydet**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

17. SAML çoklu oturum açma ayarlarınızı yapılandırmak için **meta veri dosyasından yeni**' ye tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

18. Meta veri XML dosyasını yüklemek için **Dosya Seç** ' e tıklayın ve **Oluştur**' a tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/xmlchoose.png)

19. **SAML çoklu oturum açma ayarları** sayfasında, alanlar otomatik olarak doldurulur, yapılandırmanın adını yazın (örneğin: *SPSSOWAAD_Test*), **ad** metin kutusuna ve Kaydet ' e tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

20. Etki alanınızı Salesforce korumalı alanı üzerinde etkinleştirmek için aşağıdaki adımları uygulayın:

    > [!NOTE]
    > Etki alanını etkinleştirmeden önce Salesforce korumalı alanında aynısını oluşturmanız gerekir. Daha fazla bilgi için bkz. [etki alanı adınızı tanımlama](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US). Etki alanı oluşturulduktan sonra lütfen doğru yapılandırıldığından emin olun.

21. Salesforce korumalı alanı 'ndaki sol gezinti bölmesinde, ilgili bölümü genişletmek için **Şirket ayarları** ' na tıklayın ve ardından **etki alanım**' a tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/sf-my-domain.png)

22. **Kimlik doğrulama yapılandırması** bölümünde, **Düzenle**' ye tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/sf-edit-auth-config.png)

23. Kimlik doğrulama **yapılandırması** bölümünde, **kimlik doğrulama hizmeti**olarak, Salesforce korumalı alanında SSO yapılandırması sırasında ayarladığınız SAML çoklu oturum açma ayarının adını seçin ve **Kaydet**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/salesforce-sandbox-tutorial/configure2.png)

### <a name="create-salesforce-sandbox-test-user"></a>Salesforce korumalı alanı test kullanıcısı oluştur

Bu bölümde, Salesforce korumalı alanında Britta Simon adlı bir Kullanıcı oluşturulur. Salesforce korumalı alanı, varsayılan olarak etkinleştirilen tam zamanında sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir kullanıcı zaten Salesforce korumalı alanında yoksa, Salesforce korumalı alanına erişmeye çalıştığınızda yeni bir tane oluşturulur. Salesforce korumalı alanı otomatik Kullanıcı sağlamayı da destekler, Ayrıca, otomatik Kullanıcı sağlamayı yapılandırma hakkında daha [fazla ayrıntı bulabilirsiniz](salesforce-sandbox-provisioning-tutorial.md) .

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Salesforce korumalı alanı kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Salesforce korumalı alanında otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [ SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Salesforce korumalı alanını deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/protect-salesforce)

- [Kullanıcı sağlamayı yapılandırma](salesforce-sandbox-provisioning-tutorial.md)

- [Gelişmiş görünürlük ve denetimlerle Salesforce korumalı alanını koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
