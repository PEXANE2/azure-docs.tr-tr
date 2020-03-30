---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Kutuyu Yapılandırın | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Kutu arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 1c959595-6e57-4954-9c0d-67ba03ee212b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c1397b4189a9c2c15e3878687ea8c67c1da7567f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77058578"
---
# <a name="tutorial-configure-box-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Kutuyu yapılandır

Bu öğreticinin amacı, Azure AD'den Kutu'ya kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamadan çıkarmak için Kutu ve Azure AD'de gerçekleştirmeniz gereken adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Kutu ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

- Azure AD kiracı
- Bir Kutu İş planı veya daha iyisi

> [!NOTE]
> Bu öğreticideki adımları sınadığınızda, bir üretim ortamı *kullanmamanızı* öneririz.

Bu öğreticideki adımları sınamak için aşağıdaki önerileri izleyin:

- Gerekli olmadıkça üretim ortamınızı kullanmayın.
- Azure AD deneme ortamınız yoksa, bir [aylık deneme sürümü alabilirsiniz.](https://azure.microsoft.com/pricing/free-trial/)

## <a name="assigning-users-to-box"></a>Kullanıcıları Kutuya Atama 

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki kullanıcıların ve/veya grupların Kutu uygulamanıza erişilmesi gereken kullanıcıları temsil edeceğine karar vermeniz gerekir. Karar verildikten sonra, bu kullanıcıları buradaki yönergeleri izleyerek Kutu uygulamanıza atayabilirsiniz:

[Bir kurumsal uygulamaya kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

## <a name="assign-users-and-groups"></a>Kullanıcıları ve grupları atama
Azure portalındaki **Kullanıcılar ve Gruplar > Kutusu** sekmesi, kutuya hangi kullanıcılara ve gruplara erişim izni verilmesi gerektiğini belirtmenize olanak tanır. Bir kullanıcının veya grubun atanması aşağıdaki şeylerin oluşmasına neden olur:

* Azure AD, atanan kullanıcının (doğrudan atama veya grup üyeliğiyle) Kutu'da kimlik doğrulaması için izin verir. Bir kullanıcı atanmazsa, Azure AD kutuda oturum açmalarına izin vermez ve Azure AD oturum açma sayfasında bir hata döndürür.
* Kutu için bir uygulama kutucuğu kullanıcının [uygulama başlatıcısına](../manage-apps/end-user-experiences.md)eklenir.
* Otomatik sağlama etkinleştirilirse, atanan kullanıcılar ve/veya gruplar otomatik olarak sağlanacak sağlama kuyruğuna eklenir.
  
  * Yalnızca kullanıcı nesneleri sağlanacak şekilde yapılandırıldıysa, doğrudan atanan tüm kullanıcılar sağlama kuyruğuna yerleştirilir ve atanan tüm grupların üyesi olan tüm kullanıcılar sağlama kuyruğuna yerleştirilir. 
  * Grup nesneleri sağlanacak şekilde yapılandırıldıysa, atanan tüm grup nesneleri Kutu'ya ve bu grupların üyesi olan tüm kullanıcılara verilir. Grup ve kullanıcı üyelikleri Box'a yazıldıktan sonra korunur.

SAML tabanlı kimlik doğrulaması sırasında Kutu'ya hangi kullanıcı özniteliklerinin (veya taleplerin) sunulduğunu yapılandırmak için **Öznitelikler > Tek Oturum** Açma sekmesini ve kullanıcı ve grup özniteliklerinin sağlama işlemleri sırasında Azure AD'den Kutu'ya akışını yapılandırmak için **Kullanılabilirlik >** sekmesini kullanabilirsiniz.

### <a name="important-tips-for-assigning-users-to-box"></a>Kullanıcıları Kutuya atamak için önemli ipuçları 

*   Sağlama yapılandırmasını sınamak için Kutu'ya atanan tek bir Azure AD kullanıcısının olması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

*   Bir kullanıcıyı kutuya atarken, geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan Erişim" rolü sağlama için çalışmaz.

## <a name="enable-automated-user-provisioning"></a>Otomatik Kullanıcı Sağlama'yı etkinleştirme

Bu bölüm, Azure REKLAM'ınızı Box'ın kullanıcı hesabı sağlama API'sine bağlama ve sağlama hizmetini Azure AD'deki kullanıcı ve grup atamasına göre Kutu'da atanmış kullanıcı hesapları oluşturacak, güncelleştirecek ve devre dışı edecek şekilde yapılandırmaya yöneliktir.

Otomatik sağlama etkinleştirilirse, atanan kullanıcılar ve/veya gruplar otomatik olarak sağlanacak sağlama kuyruğuna eklenir.
    
 * Yalnızca kullanıcı nesneleri sağlanacak şekilde yapılandırılırsa, doğrudan atanan kullanıcılar sağlama kuyruğuna yerleştirilir ve atanan grupların üyesi olan tüm kullanıcılar sağlama kuyruğuna yerleştirilir. 
    
 * Grup nesneleri sağlanacak şekilde yapılandırıldıysa, atanan tüm grup nesneleri Kutu'ya ve bu grupların üyesi olan tüm kullanıcılara verilir. Grup ve kullanıcı üyelikleri Box'a yazıldıktan sonra korunur.

> [!TIP] 
> [Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyerek, SAML tabanlı Kutu için Tek Oturum Açma özelliğini de etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-account-provisioning"></a>Otomatik kullanıcı hesabı sağlama yapılandırmak için:

Bu bölümün amacı, Active Directory kullanıcı hesaplarının Kutu'ya nasıl sağlandığını ana hatlarını ortaya çıkarmaktır.

1. Azure [portalında,](https://portal.azure.com)Tüm uygulamalar > **Azure Active Directory > Enterprise Apps'a** göz atın.

2. Kutuyu tek oturum açma için zaten yapılandırıldıysanız, arama alanını kullanarak Kutu örneğinizi arayın. Aksi takdirde, **Ekle'yi** seçin ve uygulama galerisinde **Kutu'yu** arayın. Arama sonuçlarından Kutu'yu seçin ve uygulama listenize ekleyin.

3. Kutu örneğini seçin ve ardından **Sağlama** sekmesini seçin.

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın. 

    ![Sağlama](./media/box-userprovisioning-tutorial/provisioning.png)

5. Yönetici **Kimlik Bilgileri** bölümünün altında, yeni bir tarayıcı penceresinde Kutu giriş iletişim kutusunu açmak için **Yetkilendirme'yi** tıklatın.

6. Kutu **sayfasına erişim sağlamak için** Giriş'te, gerekli kimlik bilgilerini sağlayın ve ardından **Yetkilendi'yi**tıklatın. 
   
    ![Otomatik kullanıcı sağlamayı etkinleştirme](./media/box-userprovisioning-tutorial/IC769546.png "Otomatik kullanıcı sağlamayı etkinleştirme")

7. Bu işlemi yetkilendirmek ve Azure portalına dönmek için **Kutu'ya Erişim** Izni'ni tıklatın. 
   
    ![Otomatik kullanıcı sağlamayı etkinleştirme](./media/box-userprovisioning-tutorial/IC769549.png "Otomatik kullanıcı sağlamayı etkinleştirme")

8. Azure portalında, Azure AD'nin Kutu uygulamanıza bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Kutu hesabınızda Team Admin izinleri olduğundan emin olun ve **"Yetki"** adımını yeniden deneyin.

9. **Bildirim E-postaalanında** ödeme hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin.

10. **Kaydet'i tıklatın.**

11. Eşlemeler bölümünde, **Azure Etkin Dizin Kullanıcılarını Kutuya Senkronize Et'i seçin.**

12. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den Kutu'ya eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleşme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemi için Kutu' daki kullanıcı hesaplarıyla çalışmak için kullanılır. Herhangi bir değişiklik yapmak için Kaydet düğmesini seçin.

13. Kutu için Azure AD sağlama hizmetini etkinleştirmek için, **On** Ayarlar bölümünde **KiSama Durumunu**

14. **Kaydet'i tıklatın.**

Bu, Kullanıcılar ve Gruplar bölümünde Kutu'ya atanan kullanıcıların ve/veya grupların ilk eşitlemasını başlatır. İlk eşitlemenin gerçekleştirilemi, hizmet yürütülürken yaklaşık her 40 dakikada bir meydana gelen sonraki eşitlemelerden daha uzun sürüyor. Kutu uygulamanızda sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik günlüklerine olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

Kutu nuzda kiracınızda, senkronize edilmiş kullanıcılar **Yönetici Konsolunda** **Yönetilen Kullanıcılar** altında listelenir.

![Entegrasyon durumu](./media/box-userprovisioning-tutorial/IC769556.png "Entegrasyon durumu")


## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Tek Oturum Açma'yı Yapılandır](box-tutorial.md)
