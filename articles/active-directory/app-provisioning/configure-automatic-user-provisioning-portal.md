---
title: Azure AD'deki kurumsal uygulamalar için kullanıcı sağlama yönetimi
description: Azure Etkin Dizini'ni kullanarak kurumsal uygulamalar için kullanıcı hesabı sağlamayı nasıl yönetebilirsiniz öğrenin
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/25/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54f9bd1afeebedf4cbf37d75d9c57f3d8be0f288
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264134"
---
# <a name="managing-user-account-provisioning-for-enterprise-apps-in-the-azure-portal"></a>Azure portalında kurumsal uygulamalar için kullanıcı hesabı sağlama sağlama

Bu makalede, otomatik kullanıcı hesabı sağlama ve destekleyen uygulamalar için de-provisioning yönetme için genel adımlar açıklanmaktadır. *Kullanıcı hesabı sağlama,* bir uygulamanın yerel kullanıcı profili deposunda kullanıcı hesabı kayıtları oluşturma, güncelleştirme ve/veya devre dışı bırakma eylemidir. Çoğu bulut ve SaaS uygulaması, kullanıcıların rolünü ve izinlerini kullanıcının kendi yerel kullanıcı profili deposunda saklar ve bu tür bir kullanıcı kaydının kullanıcının yerel deposunda bulunması, tek oturum açma ve işe erişim için *gereklidir.* Otomatik kullanıcı hesabı sağlama hakkında daha fazla bilgi edinmek için Azure [Active Directory ile SaaS Uygulamalarında Kullanıcı Sağlama ve Deprovisioning'i otomatikleştir' e](user-provisioning.md)bakın.

> [!IMPORTANT]
> Azure Etkin Dizin (Azure AD), Azure AD ile otomatik olarak sağlanması için etkinleştirilen binlerce önceden tümleşik uygulama içeren bir galeriye sahiptir. [SaaS uygulamalarını Azure Active Directory ile nasıl tümleştirdiğinize ilişkin öğreticiler listesinde](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/)uygulamanıza özel kurulum öğreticisini bularak başlamalısınız. Büyük olasılıkla, sağlama bağlantısını oluşturmak için hem uygulamayı hem de Azure AD'yi yapılandırmak için adım adım kılavuz bulacaksınız.

## <a name="finding-your-apps-in-the-portal"></a>Uygulamalarınızı portalda bulma

Bir dizinde tek oturum açma için yapılandırılan tüm uygulamaları görüntülemek ve yönetmek için Azure Active Directory portalını kullanın. Kurumsal uygulamalar, kuruluşunuz içinde dağıtılan ve kullanılan uygulamalardır. Kurumsal uygulamalarınızı görüntülemek ve yönetmek için aşağıdaki adımları izleyin:

1. Azure [Active Directory portalını](https://aad.portal.azure.com)açın.
1. Sol bölmeden **Enterprise uygulamalarını** seçin. Galeriden eklenen uygulamalar da dahil olmak üzere, yapılandırılan tüm uygulamaların listesi gösterilir.
1. Raporları görüntüleyebileceğiniz ve uygulama ayarlarını yönetebileceğiniz kaynak bölmesini yüklemek için herhangi bir uygulamayı seçin.
1. Seçili uygulama için kullanıcı hesabı sağlama ayarlarını yönetmek için **Sağlama'yı** seçin.

   ![Kullanıcı hesabı sağlama ayarlarını yönetmek için sağlama ekranı](./media/configure-automatic-user-provisioning-portal/enterprise-apps-provisioning.png)

## <a name="provisioning-modes"></a>Sağlama modları

**Sağlama** bölmesi, kurumsal bir uygulama için desteklenen sağlama modlarını gösteren ve bunları yapılandırmanızı sağlayan bir **Mode** menüsüyle başlar. Kullanılabilir seçenekler şunlardır:

* **Otomatik** - Azure AD, kullanıcı hesaplarının bu uygulamaya otomatik API tabanlı sağlanmasını veya bunların sağlanmasının kaldırılmasını destekliyorsa bu seçenek gösterilir. Yöneticilere yardımcı olan bir arabirim görüntülemek için bu modu seçin:

  * Azure AD'yi uygulamanın kullanıcı yönetimi API'sine bağlanmak için yapılandırın
  * Azure AD ile uygulama arasında kullanıcı hesabı verilerinin nasıl akması gerektiğini tanımlayan hesap eşlemeleri ve iş akışları oluşturun
  * Azure AD sağlama hizmetini yönetme

* **El Kitabı** - Azure AD, kullanıcı hesaplarının bu uygulamaya otomatik olarak sağlanmasını desteklemiyorsa bu seçenek gösterilir. Bu durumda, uygulamada depolanan kullanıcı hesabı kayıtları, kullanıcı yönetimi ve bu uygulama tarafından sağlanan sağlama yeteneklerine (SAML Just-In-Time sağlama dahil olabilir) dayalı harici bir işlem kullanılarak yönetilmelidir.

## <a name="configuring-automatic-user-account-provisioning"></a>Otomatik kullanıcı hesabı sağlama nın yapılandırılması

Yönetici kimlik bilgileri, eşlemeler, başlatma ve durdurma ve eşitleme ayarlarını belirtmek için **Otomatik** seçeneğini belirleyin.

### <a name="admin-credentials"></a>Yönetici Kimlik Bilgileri

Azure AD'nin uygulamanın kullanıcı yönetimi API'sine bağlanması için gereken kimlik bilgilerini girmek için **Yönetici Kimlik Bilgilerini** genişletin. Gerekli giriş uygulamaya bağlı olarak değişir. Belirli uygulamalar için kimlik bilgisi türleri ve gereksinimleri hakkında bilgi edinmek [için, belirli](user-provisioning.md)bir uygulama için yapılandırma öğreticibakın.

Azure AD'nin sağlanan kimlik bilgilerini kullanarak uygulamanın sağlama uygulamasına bağlanmaya çalışması için kimlik bilgilerini sınamak için **Test Bağlantısı'nı** seçin.

### <a name="mappings"></a>Eşlemeler

Kullanıcı hesapları sağlandığında veya güncelleştirildiğinde Azure AD ile hedef uygulama arasında akan kullanıcı özniteliklerini görüntülemek ve güncellemek için **Eşlemeleri** genişletin.

Azure AD kullanıcı nesneleri ile her SaaS uygulamasının kullanıcı nesneleri arasında önceden yapılandırılmış bir eşleme kümesi vardır. Bazı uygulamalar, Gruplar veya Kişiler gibi diğer nesne türlerini yönetir. Harita düzenleyicisini sağdaki gibi görüntülemek ve özelleştirebileceğiniz bir eşleme seçmek için tabloda bir eşleme seçin.

![Öznitelik Eşleme ekranını gösterir](./media/configure-automatic-user-provisioning-portal/enterprise-apps-provisioning-mapping.png)

Desteklenen özelleştirmeler şunlardır:

* SaaS uygulamasının kullanıcı nesnesine Azure AD kullanıcı nesnesi gibi belirli nesneler için eşlemeleri etkinleştirme ve devre dışı bırakma.
* Azure AD kullanıcı nesnesinden gelen öznitelikleri uygulamanın kullanıcı nesnesine düzenleme. Öznitelik eşleme hakkında daha fazla bilgi için [bkz.](customize-application-attributes.md#understanding-attribute-mapping-types)
* Azure AD'nin hedeflenen uygulamada çalıştırdığı sağlama eylemlerini filtreleme. Azure AD'nin nesneleri tam olarak eşitletmesini yerine, çalışan eylemleri sınırlayabilirsiniz.

  Örneğin, yalnızca **Güncelleştirme** ve Azure AD'yi seçin, yalnızca bir uygulamada varolan kullanıcı hesaplarını güncelleştirir, ancak yenilerini oluşturmaz. Yalnızca **Oluştur'u** seçin ve Azure yalnızca yeni kullanıcı hesapları oluşturur, ancak varolanları güncelleştirmez. Bu özellik, yöneticilerin hesap oluşturma ve iş akışlarını güncelleştirmek için farklı eşlemeler oluşturmasına olanak tanır.

* Yeni bir öznitelik eşleme ekleme. **Öznitelik Eşleme** bölmesinin altındaki **Yeni Eşleme Ekle'yi** seçin. **Atnitelik'i Edit** formunu doldurun ve listeye yeni eşlemi eklemek için **Tamam'ı** seçin.

### <a name="settings"></a>Ayarlar

**Sağlama** ekranının **Ayarlar** alanında seçili uygulama için Azure AD sağlama hizmetini başlatAbilir ve durdurabilirsiniz. Ayrıca, sağlama önbelleğini temizlemeyi ve hizmeti yeniden başlatmayı da seçebilirsiniz.

Bir uygulama için ilk kez sağlama etkinleştiriliyorsa, **Sağlama Durumunu** **Açık**olarak değiştirerek hizmeti açın. Bu değişiklik, Azure AD sağlama hizmetinin bir başlangıç döngüsünü çalıştırmasını sağlar. **Kullanıcılar ve gruplar** bölümünde atanan kullanıcıları okur, onlar için hedef uygulamayı sorgular ve ardından Azure AD **Eşlemeleri** bölümünde tanımlanan sağlama eylemlerini çalıştırır. Bu işlem sırasında, sağlama hizmeti, hangi kullanıcı hesaplarını yönettiğiyle ilgili verileri önbelleğe alınmış olarak depolar, bu nedenle atama kapsamına girmemiş hedef uygulamalar içinde yönetilmeyen hesaplar, sağlamayı ayırma işlemlerinden etkilenmez. İlk döngüden sonra, sağlama hizmeti kullanıcı ve grup nesnelerini kırk dakikalık bir aralıkla otomatik olarak eşitler.

Sağlama hizmetini duraklatmak için **Sağlama Durumunu** **Kapalı** olarak değiştirin. Bu durumda Azure, uygulamadaki hiçbir kullanıcı veya grup nesnesi oluşturmaz, güncellemez veya kaldırmaz. Durumu **Tekrar A'ya** çevirin ve servis kaldığı yerden devam eder.

**Geçerli durumu temizleyin ve eşitlemesi yeniden başlatın,** ilk döngüyü tetikler. Hizmet daha sonra kaynak sistemdeki tüm kullanıcıları yeniden değerlendirecek ve sağlama kapsamında olup olmadıklarını belirleyecektir. Bu, uygulamanız şu anda karantinadayken veya öznitelik eşlemelerinizde bir değişiklik yapmanız gerektiğinde yararlı olabilir. İlk çevrimin tamamlanmasının, değerlendirilmesi gereken nesne sayısı nedeniyle tipik artımlı döngüden daha uzun sürdüğünü unutmayın. [Burada](application-provisioning-when-will-provisioning-finish-specific-user.md)ilk ve artımlı döngülerin performansı hakkında daha fazla bilgi edinebilirsiniz. 
