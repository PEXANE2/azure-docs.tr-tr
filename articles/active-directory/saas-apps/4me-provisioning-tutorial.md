---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için 4me'yi yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını otomatik olarak sağlama ve sağlamadan sağlama 4me'ye yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/3/2019
ms.author: jeedes
ms.openlocfilehash: 423ba8c7aea9659a4c91f68a01392954c2ba6db2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77059194"
---
# <a name="tutorial-configure-4me-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için 4me yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları 4me'ye otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için 4me ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir 4me kiracı](https://www.4me.com/trial/)
* Yönetici izinleri ile 4me bir kullanıcı hesabı.

## <a name="add-4me-from-the-gallery"></a>Galeriden 4me ekle

Azure AD ile otomatik kullanıcı sağlama için 4me'yi yapılandırmadan önce, yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden 4me eklemeniz gerekir.

**Azure AD uygulama galerisinden 4me eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, sonuç panelinde **4me, 4me'yi**girin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın. **4me**

    ![Sonuç listesinde 4me](common/search-new-app.png)

## <a name="assigning-users-to-4me"></a>4me kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların 4me'ye erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek 4me'ye atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-4me"></a>4me'ye kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için tek bir Azure AD kullanıcısının 4me'ye atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı 4me'ye atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configuring-automatic-user-provisioning-to-4me"></a>Otomatik kullanıcı sağlamanın 4me olarak yapılandırılması 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak 4me'deki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca, [4me tek oturum](4me-tutorial.md)açma öğreticisinde verilen talimatları izleyerek, 4me için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-4me-in-azure-ad"></a>Azure AD'de 4me için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar **listesinde, 4me'yi**seçin.

    ![Uygulamalar listesindeki 4me bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. 4me hesabınızın **Kiracı URL'sini** ve **Gizli Jetonunu** almak için, Adım 6'da açıklandığı gibi izbinizi izleyin.

6. 4me Yönetici Konsolunuzda oturum açın. **Ayarlar'a**gidin.

    ![4me Ayarları](media/4me-provisioning-tutorial/4me01.png)

    Arama çubuğuna **uygulamaları** yazın.

    ![4me uygulamaları](media/4me-provisioning-tutorial/4me02.png)

    Gizli Belirteç ve SCIM bitiş noktasını almak için **SCIM** açılır noktasını açın.

    ![4me SCIM](media/4me-provisioning-tutorial/4me03.png)

7. Adım 5'te gösterilen alanları doldurarak, Azure AD'nin 4me bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, 4me hesabınızda Yönetici izinleri olduğundan emin olun ve yeniden deneyin.

    ![Belirteç](common/provisioning-testconnection-tenanturltoken.png)

8. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

9. **Kaydet**'e tıklayın.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını 4me eşitle'yi**seçin.

    ![4me Kullanıcı Eşlemeleri](media/4me-provisioning-tutorial/4me-user-mapping.png)
    
11. **Öznitelik Eşleme** bölümünde Azure AD'den 4me'ye eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleşme** özellikleri olarak seçilen öznitelikler, güncelleme işletmesi için 4me' deki kullanıcı hesaplarıyla çalışmak için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![4me Kullanıcı Eşlemeleri](media/4me-provisioning-tutorial/4me-user-attributes.png)
    
12. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını 4me eşitle'yi**seçin.

    ![4me Kullanıcı Eşlemeleri](media/4me-provisioning-tutorial/4me-group-mapping.png)
    
13. **Öznitelik Eşleme** bölümünde Azure AD'den 4me'ye eşitlenen grup özniteliklerini gözden geçirin. **Eşleşme** özellikleri olarak seçilen öznitelikler, güncelleştirme işletmesi için 4me' deki gruplarla aynı ş Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![4me Grup Haritalama](media/4me-provisioning-tutorial/4me-group-attribute.png)

14. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

15. 4me için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Kisa** **Durumu'nu Açiciyi** değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

16. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek 4me sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

17. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. İlerlemeyi izlemek ve Azure AD sağlama hizmeti tarafından 4me üzerinde gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna bağlı bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Bağlayıcı Sınırlamaları

* 4me test ve üretim ortamları için farklı SCIM uç noktası URL'lerine sahiptir. Eski **.qa** ile sona ererken ikincisi **.com** ile sona erer
* 4me oluşturulan Gizli Belirteçler nesilden bir ay bir son kullanma tarihi var.
* 4me **DELETE** işlemlerini desteklemiyor

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)