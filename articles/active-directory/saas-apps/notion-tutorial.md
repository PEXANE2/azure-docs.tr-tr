---
title: 'Öğretici: kavram ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve kavram arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/05/2021
ms.author: jeedes
ms.openlocfilehash: 520eb25bcb138c96b24166816d3374255fb7c3b2
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99493997"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-notion"></a>Öğretici: kavram ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, kavramı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Kavramı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de kavram erişimine erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla kavram halinde otomatik olarak oturum açmalarına olanak tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Çoklu oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Kavram **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler
* Kavram **, tam zamanında** Kullanıcı sağlamayı destekler
> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle tek bir kavram çalışma alanının tek bir kiracıda yapılandırılabilmesini sağlayabilirsiniz.


## <a name="adding-notion-from-the-gallery"></a>Galeriden kavram ekleme

Kavramın Azure AD ile tümleştirilmesini yapılandırmak için galerideki yönetilen SaaS uygulamaları listenize bir kavram eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **kavram** yazın.
1. Sonuçlar panelinden **kavram** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-sso-for-notion"></a>Kavram için Azure AD SSO 'yu yapılandırın ve test edin

**B. Simon** adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu kavram ile yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ile ilgili Kullanıcı arasında kavram olarak bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu kavram ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Kavram SSO 'Yu yapılandırma](#configure-notion-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. Kullanıcının Azure AD gösterimine bağlı bir kavram halinde B. Simon 'ya karşılık gelen bir kavramı olan kavram **[testi kullanıcısı oluşturun](#create-notion-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **kavram** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    **Yanıt URL** 'si metin kutusuna, kavram çalışma alanı **ayarlarınızı & Üyeler** > **güvenlik & kimliği** > **Çoklu oturum açma URL**'si:`https://www.notion.so/sso/saml/<CUSTOM_ID>`

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusuna aşağıdaki URL 'yi girin:`https://www.notion.so/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek yanıt URL 'si ve Sign-On URL 'siyle güncelleştirin. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. Kavram uygulama, SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML onayları bekler. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir.

    ![image](common/default-attributes.png)

1. Daha fazlasına ek olarak, kavram uygulaması aşağıda gösterilen SAML yanıtına daha fazla özniteliğe geri geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.
    
    | Name |  Kaynak özniteliği|
    | ----------- | --------- |
    | e-posta | Kullanıcı. Mail |
    | firstName | Kullanıcı. |
    | lastName | User. soyadı |


1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'sini** kopyalamak için Kopyala düğmesine tıklayın. **Üyeler** güvenlik & Identity & **kavram** çalışma alanı ayarlarınıza gidin  >  ve kopyaladığınız değeri **IDP meta veri URL 'si** alanına yapıştırın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

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

Bu bölümde, kavram erişimine erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **kavram**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-notion-sso"></a>Kavram SSO 'yu Yapılandır

**Üyeler** güvenlik & Identity & **kavram** çalışma alanı ayarlarınıza gidin  >  ve **IDP meta** veri URL 'si alanına kopyaladığınız **uygulama Federasyon meta verileri URL 'sini** yapıştırın.

Aynı ayarlar sayfasında, **e-posta etki alanları** altında, kuruluşunuzun e-posta etki alanlarını eklemek Için **desteğe başvurun** ' e tıklayın.

E-posta etki alanlarınız onaylandıktan ve eklendikten sonra **SAML etkinleştirme** ÖZELLIĞINI kullanarak SAML SSO 'yu etkinleştirin.

Başarılı sınamadan sonra SAML SSO 'yu **zorla** geçiş 'yi kullanarak da zorlayabilirsiniz. Lütfen kavram çalışma alanı yönetimlerinin, e-posta ile oturum açma yeteneğini korudığına, ancak diğer tüm üyelerin kavram halinde oturum açmak için SAML SSO 'yu kullanması gerekecektir.

### <a name="create-notion-test-user"></a>Kavram testi kullanıcısı oluştur

Bu bölümde, Britta Simon adlı bir Kullanıcı kavram halinde oluşturulmuştur. Kavram, tam zamanında Kullanıcı sağlamayı destekler ve varsayılan olarak etkindir. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir kullanıcı zaten kavram halinde yoksa, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

#### <a name="sp-initiated"></a>SP başlatıldı:

* Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu, oturum açma akışını başlatabileceğiniz kavram oturum açma URL 'sine yeniden yönlendirilir.  

* Doğrudan kavram oturum açma URL 'sine gidin ve oturum açma akışını buradan başlatın.

#### <a name="idp-initiated"></a>IDP başlatıldı:

* Azure portal **Bu uygulamayı test et** ' e tıklayın ve SSO 'yu ayarladığınız kavramda otomatik olarak oturum açmış olmanız gerekir 

Uygulamayı dilediğiniz modda test etmek için Microsoft My Apps ' i de kullanabilirsiniz. Uygulamalarım kutucuğunda tıkladığınızda, SP modunda yapılandırıldıysa, oturum açma akışını başlatmak için uygulama oturum açma sayfasına yönlendirilirsiniz ve ıDP modunda yapılandırıldıysa, SSO 'yu ayarladığınız kavramda otomatik olarak oturum açmış olmanız gerekir. Uygulamalarım hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](../user-help/my-apps-portal-end-user-access.md).


## <a name="next-steps"></a>Sonraki adımlar

Kavramı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).