---
title: 'Öğretici: Paylocity ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve ödeme arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/08/2021
ms.author: jeedes
ms.openlocfilehash: a0d1debc7baa15aeb765f9539bbbf0774f22268d
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101647081"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-paylocity"></a>Öğretici: Paylocity ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Paylocity 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Azure AD ile Paylocity 'i tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Paylocity erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesapları ile Paylocity 'e otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Paylocity çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Paylocity **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler

## <a name="add-paylocity-from-the-gallery"></a>Galeriden Paylocity ekleyin

Paylocity 'in Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden, yönetilen SaaS uygulamaları listenize Paylocity eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Paylocity** yazın.
1. Sonuçlar panelinden **Paylocity** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-paylocity"></a>Azure AD SSO 'yu Paylocity için yapılandırın ve test edin

**B. Simon** adlı bir test kullanıcısı kullanarak Azure AD SSO 'Yu Paylocity ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve Paylocity ile ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu, Paylocity ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Paylocity SSO 'Yu yapılandırın](#configure-paylocity-sso)** .
    * Kullanıcı Azure AD gösterimine bağlı olan Paylocity 'de B. Simon 'ya karşılık gelen, **[Paylocity test kullanıcısı oluşturun](#create-paylocity-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **Paylocity** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, uygulama zaten Azure ile önceden tümleştirildiği için kullanıcının herhangi bir adım yapması gerekmez.

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL** 'si metin kutusuna URL 'yi yazın:`https://access.paylocity.com/`

1. **Kaydet**’e tıklayın.

1. Paylocity uygulaması, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/default-attributes.png)

1. Daha yukarıya ek olarak, Paylocity uygulaması aşağıda gösterilen SAML yanıtına daha fazla öznitelik geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak bu öznitelikleri gerçek değerlerle güncelleştirmeniz gerekir.

    | Name |  Kaynak özniteliği|
    | ---------------| --------------- |
    | İş ortağı kimliği | `P8000010` |
    | PaylocityUser | `user.mail`|
    | PaylocityEntity | < `PaylocityEntity` > |

    > [!NOTE]
    > PaylocityEntity, Paylıity şirket KIMLIĞIDIR.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **Düzenle simgesi**' ne tıklayın.

    !["Federasyon meta verileri X M L" için "Indir" eylemine sahip "S A M L Imzalama sertifikasını" gösteren ekran görüntüsü.](./media/paylocity-tutorial/edit-samlassertion.png)

1. **Imza seçeneğini** seçin **SAML yanıtı ve onaylama işareti olarak imzala** ve **Kaydet**' e tıklayın.

    ![SAML Imzalama sertifikası düzenleme](./media/paylocity-tutorial/saml-assertion.png)

1. **Paylıt ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

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

Bu bölümde, Azure çoklu oturum açma özelliğini kullanarak Paylocity 'e erişim vererek B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Paylocity**' i seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-paylocity-sso"></a>Paylocity SSO 'yu yapılandırma

**Paylıity** tarafında çoklu oturum açmayı yapılandırmak için

1. **Federasyon meta VERILERI XML**'i indirin.
1. Paylocity 'de, **İK & Bordro**  >  **Kullanıcı erişimi**  >  **SSO yapılandırması**' na gidin.
1. **SSO tümleştirmeleri** altında **SSO tümleştirmesi Ekle** ' yi seçin. Yeni bir çekmece açılır.
1. Açılan listeden SSO sağlayıcısı olarak **Microsoft Azure** ' yi seçin.
1. Açılan listeden **durum** ' u seçin.
1. Meta veri dosyasını bırakma alanına sürükleyin ve bırakın. Ödeyeninin, veren, yeniden yönlendirme ve bağlama URL 'Leri ve güvenlik sertifikalarını ayrıştırmaya çalışır.
1. Değişiklikleri onaylamak için **Kaydet** ' i seçin. Tümleştirme, **SSO tümleştirmeleri** altında görüntülenmelidir.

### <a name="create-paylocity-test-user"></a>Paylocity test kullanıcısı oluşturma

Bu bölümde, Paylocity 'de B. Simon adlı bir Kullanıcı oluşturacaksınız. Ödeme platformunda kullanıcıları eklemek için [Paylocity destek ekibi](mailto:service@paylocity.com) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

#### <a name="sp-initiated"></a>SP başlatıldı:

* Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu, oturum açma akışını başlatabileceğiniz Paylocity oturum açma URL 'sine yeniden yönlendirilir.  

* Paylocity oturum açma URL 'sine doğrudan gidin ve oturum akışını buradan başlatın.

#### <a name="idp-initiated"></a>IDP başlatıldı:

* Azure portal **Bu uygulamayı test et** ' e tıklayın ve SSO 'Yu ayarladığınız paylıya otomatik olarak oturum açmış olmanız gerekir. 

Uygulamayı dilediğiniz modda test etmek için Microsoft My Apps ' i de kullanabilirsiniz. Uygulamalarım içindeki Paylocity kutucuğuna tıkladığınızda, SP modunda yapılandırılmışsa, oturum açma akışını başlatmak için uygulama oturum açma sayfasına yönlendirilirsiniz ve ıDP modunda yapılandırıldıysa, SSO 'yu ayarladığınız Paylıkta otomatik olarak oturum açmış olmanız gerekir. Uygulamalarım hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

Paylıfeity yapılandırıldıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).