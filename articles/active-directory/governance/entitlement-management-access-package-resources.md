---
title: Azure AD yetkilendirme yönetiminde bir erişim paketi için kaynak rollerini değiştirme - Azure Etkin Dizini
description: Azure Active Directory yetkilendirme yönetiminde varolan bir erişim paketinin kaynak rollerini nasıl değiştireceğinizi öğrenin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/15/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 102bbfbd1c02c93830f5c7fce89fe95d7fde54c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261898"
---
# <a name="change-resource-roles-for-an-access-package-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde bir erişim paketi için kaynak rollerini değiştirme

Erişim paketi yöneticisi olarak, kullanıcının yeni kaynaklara erişimini sağlama veya önceki kaynaklardan erişimlerini kaldırma endişesi duymadan istediğiniz zaman bir erişim paketindeki kaynakları değiştirebilirsiniz. Bu makalede, varolan bir erişim paketi için kaynak rollerinin nasıl değiştirilen açıklanmıştır.

Bu video, bir erişim paketinin nasıl değiştirileeceğine genel bir bakış sağlar.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE3LD4Z]

## <a name="check-catalog-for-resources"></a>Kaynaklar için kataloğu denetleyin

Bir erişim paketine kaynak eklemeniz gerekiyorsa, gereksinim duyduğunuz kaynakların katalogda bulunup bulunmadığını kontrol etmelisiniz. Bir erişim paketi yöneticisiyseniz, kataloğunuz ait olsa bile bir kataloğerekme kaynak ekleyemezsiniz. Katalogda bulunan kaynakları kullanmakla sınırlıdır.

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde **Katalog'u** tıklatın ve ardından kataloğu açın.

1. Sol menüde, bu katalogdaki kaynak listesini görmek için **Kaynaklar'ı** tıklatın.

    ![Katalogdaki kaynakların listesi](./media/entitlement-management-access-package-resources/catalog-resources.png)

1. Bir erişim paketi yöneticisiyseniz ve kataloğa kaynak eklemeniz gerekiyorsa, katalog sahibinden bunları eklemesini isteyebilirsiniz.

## <a name="add-resource-roles"></a>Kaynak rolleri ekleme

Kaynak rolü, kaynakla ilişkili izinler topluluğudur. Kullanıcıların talep edilebetmesi için kaynakları kullanılabilir hale getirmenin yolu, erişim paketinize kaynak rolleri eklemektir. Gruplar, takımlar, uygulamalar ve SharePoint siteleri için kaynak rolleri ekleyebilirsiniz.

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Access paketlerini** tıklatın ve ardından erişim paketini açın.

1. Sol menüde **Kaynak rolleri'ni**tıklatın.

1. Paket sayfasına erişmek için kaynak ekle rollerini açmak için **kaynak rolleri ekle'yi** tıklatın.

    ![Erişim paketi - Kaynak rolleri ekleme](./media/entitlement-management-access-package-resources/resource-roles-add.png)

1. Bir grup, takım, uygulama veya SharePoint sitesi eklemek isteyip istemediğinize bağlı olarak, aşağıdaki kaynak rol bölümlerinden birinde adımları gerçekleştirin.

## <a name="add-a-group-or-team-resource-role"></a>Grup veya takım kaynak rolü ekleme

Yetki yönetimi, microsoft teams'da bir gruba veya bir ekibe bir erişim paketi atandığında kullanıcıları otomatik olarak ekleyebilirsiniz. 

- Bir grup veya takım bir erişim paketinin parçası ysa ve kullanıcı bu erişim paketine atandığında, kullanıcı zaten mevcut değilse, bu gruba veya ekibe eklenir.
- Bir kullanıcının erişim paketi atamasının süresi dolduğunda, aynı grubu veya ekibi içeren başka bir erişim paketine ataması olmadığı sürece grup veya takımdan kaldırılır.

Herhangi bir [Azure AD güvenlik grubunu veya Office 365 Grubunu](../fundamentals/active-directory-groups-create-azure-portal.md)seçebilirsiniz. Yöneticiler herhangi bir grubu bir kataloğere ekleyebilir; katalog sahipleri, grubun sahibi yseler, kataloga herhangi bir grup ekleyebilir. Bir grup seçerken aşağıdaki Azure REKLAM kısıtlamalarını aklınızda bulundurun:

- Konuk da dahil olmak üzere bir kullanıcı bir gruba veya ekibe üye olarak eklendiğinde, o grubun veya takımın diğer tüm üyelerini görebilir.
- Azure AD, Azure AD Connect kullanarak Windows Server Active Directory'den senkronize edilen veya Exchange Online'da dağıtım grubu olarak oluşturulan bir grubun üyeliğini değiştiremez.  
- Dinamik grupların üyeliği bir üye eklenerek veya kaldırılarak güncelleştirilemez, bu nedenle dinamik grup üyelikleri yetki yönetimi yle kullanıma uygun değildir.

Daha fazla bilgi için grupları ve [Office 365 Gruplarını](https://docs.microsoft.com/microsoftteams/office-365-groups)ve Microsoft Takımlarını [Karşılaştır'a](https://docs.microsoft.com/office365/admin/create-groups/compare-groups) bakın.

1. Paket **sayfasına erişmek için kaynak rolleri ekle'de** Gruplar ve **Takımlar'ı** tıklatıp Gruplar bölmesini açın.

1. Erişim paketine eklemek istediğiniz grupları ve takımları seçin.

    ![Erişim paketi - Kaynak rolleri ekleme - Grupları seç](./media/entitlement-management-access-package-resources/group-select.png)

1. **Seç'i**tıklatın.

    Grubu veya takımı seçtikten sonra, **Alt tür** sütunu aşağıdaki alt tiplerden birini listeler:

    |  |  |
    | --- | --- |
    | Güvenlik | Kaynaklara erişim sağlamak için kullanılır. |
    | Dağıtım | Bir grup kişiye bildirim göndermek için kullanılır. |
    | O365 | Takımlar etkin olmayan Office 365 Grubu. Şirketinizin içinde ve dışında kullanıcılar arasında işbirliği için kullanılır. |
    | Takım | Takımlar etkin olan Office 365 Grubu. Şirketinizin içinde ve dışında kullanıcılar arasında işbirliği için kullanılır. |

1. **Rol** **listesinde, Sahibi** veya **Üyesi'ni**seçin.

    Genellikle Üye rolünü seçersiniz. Sahip rolünü seçerseniz, bu kullanıcıların diğer üyeleri veya sahipleri eklemesine veya kaldırmasına olanak sağlar.

    ![Erişim paketi - Bir grup veya takım için kaynak rolü ekleme](./media/entitlement-management-access-package-resources/group-role.png)

1. **Ekle**’ye tıklayın.

    Erişim paketine varolan atamaları olan kullanıcılar, eklendiğinde otomatik olarak bu grubun veya ekibin üyesi olur.

## <a name="add-an-application-resource-role"></a>Uygulama kaynağı rolü ekleme

Azure AD'nin, bir kullanıcıya bir erişim paketi atandığında hem SaaS uygulamaları hem de kuruluşunuzun Azure AD'ye aktarılan uygulamaları da dahil olmak üzere, kullanıcılara otomatik olarak bir Azure AD kurumsal uygulamasına erişim atamasını sağlayabilirsiniz. Azure AD ile federe tek oturum açma yoluyla tümleşen uygulamalarda, Azure AD uygulamaya atanan kullanıcılar için federasyon belirteçleri verir.

Uygulamaların birden çok rolü olabilir. Bir erişim paketine uygulama eklerken, bu uygulamanın birden fazla rolü varsa, bu kullanıcılar için uygun rolü belirtmeniz gerekir. Uygulamalar geliştiriyorsanız, bu rollerin uygulamalarınıza nasıl eklendirilebildiği hakkında daha fazla bilgi edinebilirsiniz: [Kurumsal uygulamalar için SAML belirtecinde verilen rol iddiasını yapılandırın.](../develop/active-directory-enterprise-app-role-management.md)

Bir uygulama rolü erişim paketinin bir parçası olduğunda:

- Bir kullanıcıya bu erişim paketi atandığında, kullanıcı zaten mevcut değilse, bu uygulama rolüne eklenir.
- Bir kullanıcının erişim paketi atamasının süresi dolduğunda, bu uygulama rolünü içeren başka bir erişim paketine ataması olmadığı sürece erişimleri uygulamadan kaldırılır.

Bir uygulama seçerken göz önünde bulundurulması gereken bazı noktalar şunlardır:

- Uygulamalar da rolleri atanmış gruplar olabilir.  Bir erişim paketinde uygulama rolü yerine bir grup eklemeyi seçebilirsiniz, ancak daha sonra uygulama, Access portalımdaki erişim paketinin bir parçası olarak kullanıcı tarafından görünmez.

1. Paket **sayfasına erişmek için kaynak rolleri** ekle'de, Uygulamaları Seç bölmesini açmak için **Uygulamalar'ı** tıklatın.

1. Erişim paketine eklemek istediğiniz uygulamaları seçin.

    ![Erişim paketi - Kaynak rolleri ekleme - Uygulamaları seçin](./media/entitlement-management-access-package-resources/application-select.png)

1. **Seç'i**tıklatın.

1. **Rol** listesinde bir uygulama rolü seçin.

    ![Erişim paketi - Uygulama için kaynak rolü ekleme](./media/entitlement-management-access-package-resources/application-role.png)

1. **Ekle**’ye tıklayın.

    Erişim paketine varolan atamaları olan kullanıcılara, eklendiğinde bu uygulamaya otomatik olarak erişim hakkı verilir.

## <a name="add-a-sharepoint-site-resource-role"></a>SharePoint sitesi kaynak rolü ekleme

Azure AD, kullanıcılara bir erişim paketi atandığında bir SharePoint Online sitesine veya SharePoint Online site koleksiyonuna otomatik olarak erişim atayabilir.

1. Paket **sayfasına erişmek için kaynak rolleri** ekle'de, Select SharePoint Online siteleri bölmesini açmak için **SharePoint sitelerini** tıklatın.

1. Erişim paketine eklemek istediğiniz SharePoint Online sitelerini seçin.

    ![Erişim paketi - Kaynak rolleri ekleme - SharePoint Online sitelerini seçin](./media/entitlement-management-access-package-resources/sharepoint-site-select.png)

1. **Seç'i**tıklatın.

1. **Rol** listesinde bir SharePoint Online site rolü seçin.

    ![Erişim paketi - SharePoint Online sitesi için kaynak rolü ekleme](./media/entitlement-management-access-package-resources/sharepoint-site-role.png)

1. **Ekle**’ye tıklayın.

    Erişim paketine varolan atamaları olan kullanıcılara, eklendiğinde bu SharePoint Online sitesine otomatik olarak erişim hakkı verilir.

## <a name="remove-resource-roles"></a>Kaynak rollerini kaldırma

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Access paketlerini** tıklatın ve ardından erişim paketini açın.

1. Sol menüde **Kaynak rolleri'ni**tıklatın.

1. Kaynak rolleri listesinde kaldırmak istediğiniz kaynak rolünü bulun.

1. Elipsis 'i (**...**) tıklatın ve sonra **kaynak rolünü kaldır'ı**tıklatın.

    Erişim paketinde varolan atamaları olan tüm kullanıcıların erişimi kaldırıldığında bu kaynak rolüne otomatik olarak iptal edilir.

## <a name="when-changes-are-applied"></a>Değişiklikler uygulandığında

Yetkilendirme yönetiminde Azure AD, erişim paketlerinizdeki atama ve kaynaklar için toplu değişiklikleri günde birkaç kez işler. Bu nedenle, bir atama yaparsanız veya erişim paketinizin kaynak rollerini değiştirirseniz, bu değişikliğin Azure AD'de yapılması 24 saate kadar sürebilir ve bu değişiklikleri diğer Microsoft Çevrimiçi Hizmetleri'nde veya bağlı SaaS'a yaymak için gereken süre 24 saat kadar sürebilir Uygulama. Değişikliğiniz yalnızca birkaç nesneyi etkiliyorsa, değişikliğin Azure AD'de uygulanması yalnızca birkaç dakika sürer ve ardından diğer Azure AD bileşenleri bu değişikliği algılar ve SaaS uygulamalarını günceller. Değişikliğiniz binlerce nesneyi etkiliyorsa, değişiklik daha uzun sürer. Örneğin, 2 uygulama ve 100 kullanıcı ataması içeren bir erişim paketiniz varsa ve erişim paketine bir SharePoint site rolü eklemeye karar verirseniz, tüm kullanıcılar bu SharePoint site rolünün bir parçası olana kadar gecikme olabilir. İlerlemeyi Azure AD denetim günlüğünden, Azure AD sağlama günlüğünden ve SharePoint site denetim günlüklerinden izleyebilirsiniz.

Ekibin bir üyesini kaldırdığınızda bu üye Office 365 Grubundan da kaldırılır. Ekibin sohbet işlevinden kaldırma işlemi geciktirilebilir. Daha fazla bilgi için [Grup üyeliğine](https://docs.microsoft.com/microsoftteams/office-365-groups#group-membership)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Etkin Dizini'ni kullanarak temel bir grup oluşturun ve üye ekleyin](../fundamentals/active-directory-groups-create-azure-portal.md)
- [Nasıl verilir: Kurumsal uygulamalar için SAML belirteci verilen rol talebi yapılandırma](../develop/active-directory-enterprise-app-role-management.md)
- [SharePoint Online'a Giriş](/sharepoint/introduction)