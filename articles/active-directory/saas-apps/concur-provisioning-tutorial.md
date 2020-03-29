---
title: "Öğretici: Azure Active Directory| ile otomatik kullanıcı sağlama için Concur'u yapılandırın| Microsoft Dokümanlar"
description: Azure Active Directory ve Concur arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: df47f55f-a894-4e01-a82e-0dbf55fc8af1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 441aa9805f2a453e22f207238315125d2a281838
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60280474"
---
# <a name="tutorial-configure-concur-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Concur'u yapılandır

Bu öğreticinin amacı, Azure AD'den Concur'a kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamamak için Concur ve Azure AD'de gerçekleştirmeniz gereken adımları size göstermektir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

*   Azure Etkin dizin kiracı.
*   Concur tek oturum açma özellikli abonelik.
*   Team Admin izinleriyle aynı fikirde olan bir kullanıcı hesabı.

## <a name="assigning-users-to-concur"></a>Kullanıcıları Concur'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Concur uygulamanıza erişilmesi gereken kullanıcıları temsil edeceğine karar vermeniz gerekir. Karar verildikten sonra, bu kullanıcıları aşağıdaki talimatları izleyerek Concur uygulamanıza atayabilirsiniz:

[Bir kurumsal uygulamaya kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-concur"></a>Kullanıcılarını Concur'a atamak için önemli ipuçları

*   Sağlama yapılandırmasını sınamak için tek bir Azure AD kullanıcısının Concur'a atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

*   Bir kullanıcıyı Concur'a atarken, geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan Erişim" rolü sağlama için çalışmaz.

## <a name="enable-user-provisioning"></a>Kullanıcı sağlamayı etkinleştirme

Bu bölüm, Azure REKLAM'ınızı Concur'un kullanıcı hesabı sağlama API'sine bağlamanız ve sağlama hizmetini Azure AD'deki kullanıcı ve grup atamasına bağlı olarak Concur'da atanmış kullanıcı hesapları oluşturacak, güncelleştirecek ve devre dışı edecek şekilde yapılandırmak için size rehberlik eder.

> [!Tip] 
> [Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyerek, Eşzamanlı olarak SAML tabanlı Tek Oturum Açma özelliğini de etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-user-account-provisioning"></a>Kullanıcı hesabı sağlama yapılandırmak için:

Bu bölümün amacı, Active Directory kullanıcı hesaplarının Concur'a nasıl sağlanabildiğini ana hatlar.

Gider Hizmeti'ndeki uygulamaları etkinleştirmek için, bir Web Hizmeti Yöneticisi profilinin uygun şekilde kurulumu ve kullanımı olmalıdır. T&E yönetim işlevleri için kullandığınız mevcut yönetici profilinize WS Yöneticisi rolünü eklemeyin.

Concur Danışmanları veya istemci yöneticisi farklı bir Web Hizmeti Yöneticisi profili oluşturmalı ve İstemci yöneticisi web hizmetleri yöneticisi işlevleri (örneğin, uygulamaları etkinleştirmek) için bu profili kullanmalıdır. Bu profiller istemci yöneticinin günlük T&E yönetici profilinden ayrı tutulmalıdır (T&E yönetici profili WSAdmin rolüne atanmamalıdır).

Uygulamayı etkinleştirmek için kullanılacak profili oluşturduğunuzda, kullanıcı profili alanlarına istemci yöneticisinin adını girin. Bu, profile sahiplik atar. Bir veya daha fazla profil oluşturulduktan sonra, istemcinin Web Hizmetleri menüsündeki Bir İş Ortağı Uygulaması için "*Etkinleştir*" düğmesini tıklatmak için bu profille oturum açması gerekir.

Aşağıdaki nedenlerle, bu eylem normal T&E uygulaması için kullandıkları profil ile yapılmamalıdır.

* İstemci, bir uygulama etkinleştirildikten sonra görüntülenen diyalog penceresinde *"Evet"* seçeneğini tıklayan kişi olmalıdır. Bu tıklama, istemcinin İş Ortağı uygulamasının verilerine erişmeye istekli olduğunu kabul eder, bu nedenle siz veya İş Ortağı bu Evet düğmesini tıklatamazsınız.

* T&E yönetici profilini kullanarak bir uygulamayı etkinleştiren bir istemci yöneticisi şirketten ayrılırsa (profilin devre dışı bırakılması ile sonuçlanır), uygulama başka bir etkin WS Yönetici profili yle etkinleştirilene kadar bu profili kullanan herhangi bir uygulama çalışmaz. Bu nedenle farklı WS Yönetici profilleri oluşturmanız gerekir.

* Bir yönetici şirketten ayrılırsa, ws yöneticisi profiline ilişkili ad, bu profilin etkinleştirilmesi gerekmediği için etkinleştirilen uygulamayı etkilemeden istenirse yedek yönetici olarak değiştirilebilir.

**Kullanıcı sağlama yapılandırmak için aşağıdaki adımları gerçekleştirin:**

1. **Concur** kiracınıza oturum açın.

2. **Yönetim** menüsünden **Web Hizmetleri'ni**seçin.
   
    ![Aynı fikirde kiracı](./media/concur-provisioning-tutorial/IC721729.png "Aynı fikirde kiracı")

3. Sol tarafta, Web **Hizmetleri** bölmesinden **İş Ortağı Uygulamasını Etkinleştir'i**seçin.
   
    ![Ortak Uygulamasını Etkinleştir](./media/concur-provisioning-tutorial/ic721730.png "Ortak Uygulamasını Etkinleştir")

4. Uygulamayı **Etkinleştir** listesinden **Azure Etkin Dizini'ni**seçin ve ardından **Etkinleştir'i**tıklatın.
   
    ![Microsoft Azure Active Directory](./media/concur-provisioning-tutorial/ic721731.png "Microsoft Azure Active Directory")

5. **Eylemi Onayla** iletişim kutusunu kapatmak için **Evet'i** tıklatın.
   
    ![Eylemi Onayla](./media/concur-provisioning-tutorial/ic721732.png "Eylemi Onayla")

6. Azure [portalında,](https://portal.azure.com)Tüm uygulamalar > **Azure Active Directory > Enterprise Apps'a** göz atın.

7. Tek oturum açma için Concur'u zaten yapılandırmışsanız, arama alanını kullanarak Concur örneğini arayın. Aksi takdirde, uygulama galerisinde **Ekle** ve **Concur'u** arayın'ı seçin. Arama sonuçlarından Concur'u seçin ve uygulama listenize ekleyin.

8. Concur örneğini seçin ve ardından **Sağlama** sekmesini seçin.

9. Sağlama **Modunu** **Otomatik**olarak ayarlayın. 
 
    ![Sağlama](./media/concur-provisioning-tutorial/provisioning.png)

10. Yönetici **Kimlik Bilgileri** bölümünün altında, Kullanıcı **adını** ve Concur yöneticinizin **parolasını** girin.

11. Azure portalında, Azure AD'nin Concur uygulamanıza bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Concur hesabınızın Team Admin izinlerine sahip olduğundan emin olun.

12. **Bildirim E-postaalanında** ödeme hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin.

13. **Kaydet'i tıklatın.**

14. Eşlemeler bölümünde, Aynı fikirde olmak **üzere Azure Etkin Dizin Kullanıcılarını Eşitle'yi seçin.**

15. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den Concur'a eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleç** özellikler olarak seçilen öznitelikler, güncelleştirme işlemleri için Concur'daki kullanıcı hesaplarıyla eşleşecek şekilde kullanılır. Herhangi bir değişiklik yapmak için Kaydet düğmesini seçin.

16. Aynı fikirde olmak için Azure AD sağlama hizmetini etkinleştirmek **On** **için, Ayarlar** bölümünde **KiSama Durumunu**

17. **Kaydet'i tıklatın.**

Artık bir test hesabı oluşturabilirsiniz. Hesabın Concur ile senkronize edildiğini doğrulamak için 20 dakika kadar bekleyin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Tek Oturum Açma'yı Yapılandır](concur-tutorial.md)

