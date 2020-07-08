---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için kutuyu Yapılandır | Microsoft Docs'
description: Azure Active Directory ve Box arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77058578"
---
# <a name="tutorial-configure-box-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için yapılandırma kutusu

Bu öğreticinin amacı, Box ve Azure AD 'de gerçekleştirmeniz gereken adımları, Azure AD 'den Box ' a otomatik olarak sağlamak ve devre dışı bırakmak için gerekli adımları gösterir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesi 'ni kutusuyla birlikte yapılandırmak için aşağıdaki öğeler gereklidir:

- Bir Azure AD kiracısı
- Bir Box Iş planı veya daha iyi

> [!NOTE]
> Bu öğreticideki adımları test ettiğinizde, bir üretim *ortamı kullanmanızı öneririz* .

Bu öğreticideki adımları test etmek için aşağıdaki önerileri izleyin:

- Gerekli olmadığı takdirde üretim ortamınızı kullanmayın.
- Bir Azure AD deneme ortamınız yoksa, bir [aylık deneme sürümü](https://azure.microsoft.com/pricing/free-trial/)alabilirsiniz.

## <a name="assigning-users-to-box"></a>Box 'a Kullanıcı atama 

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişim alacağını belirleyebilmek için "atamalar" adlı bir kavram kullanır. Otomatik Kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya "atanmış" olan kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların, Box uygulamanıza erişmesi gereken kullanıcıları temsil ettiğini belirlemeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları Box uygulamanıza atayabilirsiniz:

[Kurumsal uygulamaya Kullanıcı veya Grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

## <a name="assign-users-and-groups"></a>Kullanıcıları ve grupları ata
Azure portal kutu **> kullanıcılar ve gruplar** sekmesi, Box 'ın hangi kullanıcılara ve gruplara erişim izni verileceğini belirtmenize olanak tanır. Bir Kullanıcı ya da grup ataması aşağıdaki işlemlerin oluşmasına neden olur:

* Azure AD, atanan kullanıcıya (doğrudan atama ya da grup üyeliğiyle) kimlik doğrulaması yapmak için izin verir. Bir Kullanıcı atanmamışsa Azure AD, oturum açma ve Azure AD oturum açma sayfasında bir hata döndüren bir hata döndürür.
* Kullanıcının [uygulama](../manage-apps/end-user-experiences.md)başlatıcısında Box için bir uygulama kutucuğu eklenir.
* Otomatik sağlama etkinse, atanan kullanıcılar ve/veya grupları otomatik olarak sağlanacak sağlama kuyruğuna eklenir.
  
  * Yalnızca kullanıcı nesneleri sağlanacak şekilde yapılandırıldıysa, tüm doğrudan atanan kullanıcılar sağlama kuyruğuna yerleştirilir ve atanan grupların üyesi olan tüm kullanıcılar sağlama kuyruğuna yerleştirilir. 
  * Grup nesneleri sağlanacak şekilde yapılandırıldıysa, tüm atanan Grup nesneleri, Box ve bu grupların üyesi olan tüm kullanıcılar tarafından sağlanır. Grup ve Kullanıcı üyelikleri, kutusuna yazıldıktan sonra korunur.

SAML tabanlı kimlik doğrulaması sırasında hangi kullanıcı özniteliklerinin (veya talepler) kullanıma sunulduğunu yapılandırmak için **öznitelikler > çoklu oturum açma** sekmesini ve Kullanıcı ve grup özniteliklerinin sağlama Işlemleri SıRASıNDA Azure AD 'den Box 'a nasıl akabileceğinizi yapılandırmak için **öznitelikler > sağlama** sekmesini kullanabilirsiniz.

### <a name="important-tips-for-assigning-users-to-box"></a>Box 'a Kullanıcı atamaya yönelik önemli ipuçları 

*   Sağlama yapılandırmasını test etmek için tek bir Azure AD kullanıcısının Box 'a atanmasını öneririz. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcıyı Box 'a atarken geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-automated-user-provisioning"></a>Otomatik Kullanıcı sağlamayı etkinleştir

Bu bölüm, Azure AD 'nizi Box 'ın Kullanıcı hesabı sağlama API 'sine bağlama ve sağlama hizmetini, Azure AD 'de Kullanıcı ve grup atamasını temel alan Box 'ta atanan kullanıcı hesaplarını oluşturmak, güncelleştirmek ve devre dışı bırakmak üzere yapılandırmayı gösterir.

Otomatik sağlama etkinse, atanan kullanıcılar ve/veya grupları otomatik olarak sağlanacak sağlama kuyruğuna eklenir.
    
 * Yalnızca kullanıcı nesneleri sağlanacak şekilde yapılandırıldıysa, doğrudan atanan kullanıcılar sağlama kuyruğuna yerleştirilir ve atanan grupların üyesi olan tüm kullanıcılar sağlama kuyruğuna yerleştirilir. 
    
 * Grup nesneleri sağlanacak şekilde yapılandırıldıysa, tüm atanan Grup nesneleri, Box ve bu grupların üyesi olan tüm kullanıcılar tarafından sağlanır. Grup ve Kullanıcı üyelikleri, kutusuna yazıldıktan sonra korunur.

> [!TIP] 
> Ayrıca, [Azure Portal](https://portal.azure.com)' de sağlanan yönergeleri Izleyerek, Box için SAML tabanlı çoklu oturum açma 'yı da tercih edebilirsiniz. Çoklu oturum açma özelliği otomatik sağlanmadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="to-configure-automatic-user-account-provisioning"></a>Otomatik Kullanıcı hesabı sağlamayı yapılandırmak için:

Bu bölümün amacı, Box 'ın Active Directory Kullanıcı hesaplarının sağlanmasını nasıl etkinleştireceğinizi özetler.

1. [Azure portal](https://portal.azure.com) **Azure Active Directory > Enterprise Apps > tüm uygulamalar** bölümüne gidin.

2. Çoklu oturum açma için zaten yapılandırılmış bir kutu varsa, arama alanını kullanarak Box örneğiniz için arama yapın. Aksi takdirde, uygulama galerisinde **Ekle** ve ara **kutusunu** seçin. Arama sonuçlarından Box ' ı seçin ve uygulama listenize ekleyin.

3. Box örneğinizi seçin, sonra **sağlama** sekmesini seçin.

4. **Sağlama modunu** **Otomatik**olarak ayarlayın. 

    ![alınıyor](./media/box-userprovisioning-tutorial/provisioning.png)

5. **Yönetici kimlik bilgileri** bölümünde, yeni bir tarayıcı penceresinde Box oturum açma iletişim kutusunu açmak için **Yetkilendir** ' e tıklayın.

6. **Box 'a erişim izni vermek Için oturum açma** sayfasında, gerekli kimlik bilgilerini sağlayın ve ardından **Yetkilendir**' e tıklayın. 
   
    ![Otomatik Kullanıcı sağlamayı etkinleştir](./media/box-userprovisioning-tutorial/IC769546.png "Otomatik Kullanıcı sağlamayı etkinleştir")

7. Bu işlemi yetkilendirmek ve Azure portal dönmek için, **kutuya erişim Izni ver** ' e tıklayın. 
   
    ![Otomatik Kullanıcı sağlamayı etkinleştir](./media/box-userprovisioning-tutorial/IC769549.png "Otomatik Kullanıcı sağlamayı etkinleştir")

8. Azure portal, Azure AD 'nin Box uygulamanıza bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Box hesabınızda Takım Yöneticisi izinlerine sahip olduğundan emin olun ve **"Yetkilendir"** adımını yeniden deneyin.

9. **Bildirim e-postası** alanında sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin.

10. Kaydet ' e tıklayın **.**

11. Eşlemeler bölümünde **Azure Active Directory Kullanıcıları kutuya zamanla** ' yı seçin.

12. **Öznitelik eşlemeleri** bölümünde, Azure AD 'den kutusunda bulunan Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için Box 'daki Kullanıcı hesaplarıyla eşleştirmek için kullanılır. Değişiklikleri uygulamak için Kaydet düğmesini seçin.

13. Kutusunu için Azure AD sağlama hizmetini etkinleştirmek üzere ayarlar bölümünde **sağlama durumunu** **Açık** olarak değiştirin

14. Kaydet ' e tıklayın **.**

Bu, kullanıcılar ve Gruplar bölümünde kutusuna atanmış tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, hizmetin çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. İşlem ilerlemesini izlemek ve bakım uygulamanızda sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan etkinlik günlüklerinin sağlanması için bağlantıları izlemek üzere **eşitleme ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

Box kiracınızda, eşitlenen kullanıcılar **Yönetim konsolundaki** **yönetilen kullanıcılar** altında listelenir.

![Tümleştirme durumu](./media/box-userprovisioning-tutorial/IC769556.png "Tümleştirme durumu")


## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırma](box-tutorial.md)
