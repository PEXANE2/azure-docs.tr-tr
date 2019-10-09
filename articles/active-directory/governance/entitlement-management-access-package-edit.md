---
title: Azure AD Yetkilendirme Yönetimi 'nde (Önizleme) var olan bir erişim paketini düzenleyin ve yönetin Azure Active Directory
description: Azure Active Directory yetkilendirme yönetimi 'nde (Önizleme) var olan bir erişim paketini düzenlemeyi ve yönetmeyi öğrenin.
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
ms.date: 09/26/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 864a6476ad894952a2ef877d534ffe8a86e07819
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170017"
---
# <a name="edit-and-manage-an-existing-access-package-in-azure-ad-entitlement-management-preview"></a>Azure AD Yetkilendirme Yönetimi 'nde var olan bir erişim paketini düzenleyin ve yönetin (Önizleme)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) yetkilendirme yönetimi şu anda genel önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmeyebilir veya kısıtlı özelliklere sahip olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Erişim paketi, erişim paketinin ömrü boyunca erişimi otomatik olarak yöneten kaynak ve ilkelerin bir kerelik kurulumunu yapmanızı sağlar. Bir erişim paketi Yöneticisi olarak, bir erişim paketindeki kaynakları, kullanıcının yeni kaynaklara erişimini sağlama veya önceki kaynaklardan erişimleri kaldırma konusunda endişelenmeden dilediğiniz zaman değiştirebilirsiniz. İlkeler de her zaman güncelleştirilemeyebilir, ancak ilke değişiklikleri yalnızca yeni erişimleri etkiler.

Bu makalede, mevcut erişim paketlerinin nasıl düzenleneceği ve yönetileceği açıklanmaktadır.

## <a name="add-resource-roles"></a>Kaynak rolleri ekleme

Kaynak rolü, bir kaynakla ilişkili izinlerin koleksiyonudur. Kaynakları kullanıcıların istemesi için kullanılabilir hale getirmenin yolu, erişim paketinize kaynak rolleri eklemektir. Gruplar, takımlar, uygulamalar ve SharePoint siteleri için kaynak rolleri ekleyebilirsiniz.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. Sol taraftaki menüden **kaynak rolleri**' ne tıklayın.

1. **Kaynak rolleri ekle** ' ye tıklayarak pakete erişmek için kaynak rolleri Ekle sayfasını açın.

    ![Erişim paketi-kaynak rolleri ekleme](./media/entitlement-management-access-package-edit/resource-roles-add.png)

1. Bir grup, takım, uygulama veya SharePoint sitesi eklemek istediğinize bağlı olarak, aşağıdaki kaynak rolü bölümlerinden birindeki adımları gerçekleştirin.

### <a name="add-a-group-or-team-resource-role"></a>Grup veya takım kaynağı rolü ekleme

Bir erişim paketi atandığında, yetkilendirme yönetiminin kullanıcıları otomatik olarak bir gruba veya Microsoft ekibine eklemesini sağlayabilirsiniz. 

- Bir grup veya takım bir erişim paketinin parçası olduğunda ve bu erişim paketine bir Kullanıcı atandığında, zaten mevcut değilse Kullanıcı o gruba veya takıma eklenir.
- Bir kullanıcının erişim paketi atamasının süresi dolarsa, o anda aynı grup veya takımı içeren başka bir erişim paketine bir atamaya sahip olmadıkları takdirde, bunlar gruptan veya takımdan kaldırılır.

Herhangi bir [Azure AD güvenlik grubunu veya Office 365 grubunu](../fundamentals/active-directory-groups-create-azure-portal.md)seçebilirsiniz.  Yöneticiler, kataloğa herhangi bir grup ekleyebilir; Katalog sahipleri, grubun sahibiyseniz, kataloğa herhangi bir grup ekleyebilir. Bir grup seçerken aşağıdaki Azure AD kısıtlamalarını aklınızda bulundurun:

- Konuk dahil bir Kullanıcı, bir grup veya takıma üye olarak eklendiğinde, bu grubun veya ekibin tüm diğer üyelerini görebilirler.
- Azure AD, Azure AD Connect kullanılarak Windows Server Active Directory eşitlenen veya Exchange Online 'da bir dağıtım grubu olarak oluşturulan bir grubun üyeliğini değiştiremiyor.  
- Dinamik grupların üyeliği üye ekleyerek veya kaldırarak güncelleştirilemez, bu nedenle dinamik grup üyelikleri yetkilendirme yönetimiyle kullanım için uygun değildir.

Daha fazla bilgi için bkz. grupları ve [Office 365 gruplarını ve Microsoft ekiplerini](/microsoftteams/office-365-groups) [karşılaştırın](/office365/admin/create-groups/compare-groups) .

1. **Pakete erişmek için kaynak rolleri ekle** sayfasında, **gruplar ve takımlar** ' a tıklayarak Grup Seç bölmesini açın.

1. Erişim paketine eklemek istediğiniz grupları ve takımları seçin.

    ![Erişim paketi-kaynak rolleri ekleme-Grup seçme](./media/entitlement-management-access-package-edit/group-select.png)

1. **Seçin**’e tıklayın.

    Grubu veya takımı seçtiğinizde, **alt tür** sütunu aşağıdaki alt türlerinden birini listeler:

    |  |  |
    | --- | --- |
    | Güvenlik | Kaynaklara erişim vermek için kullanılır. |
    | Dağılı | Bir grup kişiye bildirim göndermek için kullanılır. |
    | O365 | Takımlar etkin olmayan Office 365 grubu. Şirket içindeki ve dışındaki kullanıcılar arasında işbirliği için kullanılır. |
    | Takım | Takımlar etkin olan Office 365 grubu. Şirket içindeki ve dışındaki kullanıcılar arasında işbirliği için kullanılır. |

1. **Rol** listesinde **Owner** veya **member**' ı seçin.

    Genellikle üye rolünü seçersiniz. Sahip rolünü seçerseniz, kullanıcıların başka Üyeler veya sahipler eklemesine veya kaldırmasına izin verir.

    ![Erişim paketi-bir grup veya takım için kaynak rolü ekleme](./media/entitlement-management-access-package-edit/group-role.png)

1. **Ekle**' ye tıklayın.

    Erişim paketine mevcut atamaları olan tüm kullanıcılar, eklendiğinde bu grubun veya ekibin üyeleri otomatik olarak olur.

### <a name="add-an-application-resource-role"></a>Uygulama kaynağı rolü ekleme

Bir kullanıcıya erişim paketi atandığında, Azure AD 'nin hem SaaS uygulamaları hem de kuruluşunuzun Azure AD 'ye federe uygulamalar dahil olmak üzere bir Azure AD kurumsal uygulamasına erişimini otomatik olarak atamasını sağlayabilirsiniz. Federasyon çoklu oturum açma aracılığıyla Azure AD ile tümleştirilen uygulamalar için Azure AD, uygulamaya atanan kullanıcılar için Federasyon belirteçleri verir.

Uygulamalar birden çok role sahip olabilir. Bir erişim paketine bir uygulama eklerken, bu uygulamanın birden fazla rolü varsa, bu kullanıcılar için uygun rolü belirtmeniz gerekir.  Uygulama geliştiriyorsanız, [SAML belirtecinde verilen rol talebinin nasıl yapılandırılacağı](../develop/active-directory-enterprise-app-role-management.md)hakkında makalesinde, bu rollerin uygulamalarınıza nasıl sağlandığını öğrenmek için daha fazla bilgi edinebilirsiniz.

Uygulama rolü bir erişim paketinin parçası olduktan sonra:

- Bir Kullanıcı bu erişim paketine atandığında, zaten mevcut değilse Kullanıcı söz konusu uygulama rolüne eklenir.
- Bir kullanıcının erişim paketi atamasının süresi dolarsa, bu uygulama rolünü içeren başka bir erişim paketine atanması gerekmedikçe erişimleri uygulamadan kaldırılır.

Bir uygulama seçerken bazı konular aşağıda verilmiştir:

- Uygulamalarda Ayrıca, rollerine de atanmış gruplar bulunabilir.  Bir erişim paketindeki uygulama rolü yerine bir grup eklemeyi seçebilirsiniz, ancak bundan sonra uygulama kullanıcıya erişim portalındaki erişim paketinin bir parçası olarak görünmez.

1. **Pakete erişmek için kaynak rolleri ekle** sayfasında **uygulamalar** ' a tıklayarak uygulama Seç bölmesini açın.

1. Erişim paketine eklemek istediğiniz uygulamaları seçin.

    ![Erişim paketi-kaynak rolleri ekleme-uygulama seçme](./media/entitlement-management-access-package-edit/application-select.png)

1. **Seçin**’e tıklayın.

1. **Rol** listesinde bir uygulama rolü seçin.

    ![Erişim paketi-bir uygulama için kaynak rolü ekleme](./media/entitlement-management-access-package-edit/application-role.png)

1. **Ekle**' ye tıklayın.

    Erişim paketine mevcut atamalara sahip tüm kullanıcılara, eklendiğinde bu uygulamaya otomatik olarak erişim verilir.

### <a name="add-a-sharepoint-site-resource-role"></a>SharePoint site kaynağı rolü ekleme

Azure AD, kullanıcılara bir SharePoint Online sitesine veya SharePoint Online site koleksiyonuna erişim paketi atandığında otomatik olarak erişim atayabilir.

1. **Pakete erişmek için kaynak rolleri ekle** sayfasında **SharePoint siteleri** ' ne tıklayarak SharePoint Online siteleri Seç bölmesini açın.

1. Erişim paketine eklemek istediğiniz SharePoint Online sitelerini seçin.

    ![Erişim paketi-kaynak rolleri ekleme-SharePoint Online siteleri seçin](./media/entitlement-management-access-package-edit/sharepoint-site-select.png)

1. **Seçin**’e tıklayın.

1. **Rol** listesinde bir SharePoint Online site rolü seçin.

    ![Erişim paketi-SharePoint Online sitesi için kaynak rolü ekleme](./media/entitlement-management-access-package-edit/sharepoint-site-role.png)

1. **Ekle**' ye tıklayın.

    Erişim paketine mevcut atamalara sahip olan tüm kullanıcılara, eklendiğinde bu SharePoint Online sitesine otomatik olarak erişim verilir.

## <a name="remove-resource-roles"></a>Kaynak rollerini kaldırma

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. Sol taraftaki menüden **kaynak rolleri**' ne tıklayın.

1. Kaynak rolleri listesinde, kaldırmak istediğiniz kaynak rolünü bulun.

1. Üç noktaya ( **...** ) ve ardından **kaynak rolünü kaldır**' a tıklayın.

    Erişim paketine mevcut atamaları olan tüm kullanıcılar, kaldırıldığında bu kaynak rolü için otomatik olarak erişimi iptal eder.

## <a name="add-a-new-policy"></a>Yeni ilke Ekle

Ne şekilde bir erişim paketi isteyebilen bir ilke oluşturmaktır. Farklı Kullanıcı kümelerine, farklı onay ve sona erme ayarlarına sahip atamalar verilmesini sağlamak istiyorsanız, tek bir erişim paketi için birden çok ilke oluşturabilirsiniz. Tek bir ilke, aynı erişim paketine iç ve dış kullanıcı atamak için kullanılamaz. Bununla birlikte, aynı erişim paketinde bir tane olmak üzere, biri iç kullanıcılar ve diğeri dış kullanıcılar için olmak üzere iki ilke oluşturabilirsiniz. Bir kullanıcı için uygulanan birden çok ilke varsa, kendilerine atanması gereken ilkeyi seçme istekleri sırasında istenir.

Aşağıdaki diyagramda, var olan bir erişim paketine yönelik bir ilke oluşturmak için üst düzey işlem gösterilmektedir.

![İlke işlemi oluşturma](./media/entitlement-management-access-package-edit/policy-process.png)

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. **İlkeler** ' e ve ardından **ilke Ekle**' ye tıklayın.

1. İlke için bir ad ve açıklama yazın.

    ![Ad ve açıklama ile ilke oluştur](./media/entitlement-management-access-package-edit/policy-name-description.png)

1. **Erişim Isteyebilen kullanıcılar**için seçiminize bağlı olarak, aşağıdaki ilke bölümlerinden birindeki adımları gerçekleştirin.

[!INCLUDE [Entitlement management policy](../../../includes/active-directory-entitlement-management-policy.md)]

## <a name="edit-an-existing-policy"></a>Var olan bir ilkeyi düzenleme

Bir ilkeyi dilediğiniz zaman düzenleyebilirsiniz. Bir ilkenin sona erme tarihini değiştirirseniz, zaten bekleyen bir onaylama veya onaylanan durumda olan isteklerin sona erme tarihi değişmez.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. **İlkeler** ' e tıklayın ve ardından düzenlemek istediğiniz ilkeye tıklayın.

    **İlke ayrıntıları** bölmesi sayfanın altında açılır.

    ![Erişim paketi-Ilke ayrıntıları bölmesi](./media/entitlement-management-access-package-edit/policy-details.png)

1. İlkeyi düzenlemek için **Düzenle** ' ye tıklayın.

    ![Erişim paketi-ilkeyi Düzenle](./media/entitlement-management-access-package-edit/policy-edit.png)

1. İşiniz bittiğinde **Güncelleştir**' e tıklayın.

## <a name="directly-assign-a-user"></a>Doğrudan kullanıcı atama

Bazı durumlarda, kullanıcıların erişim paketini isteme işlemini istememeleri için bir erişim paketine doğrudan belirli kullanıcıları atamak isteyebilirsiniz. Kullanıcıları doğrudan atamak için, erişim paketinin yönetici doğrudan atamalarına izin veren bir ilkesi olması gerekir.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. Sol taraftaki menüde **atamalar**' a tıklayın.

1. Erişim paketine Kullanıcı Ekle ' yi açmak için **yeni atama** ' ya tıklayın.

    ![Atamalar-erişim paketine Kullanıcı ekleme](./media/entitlement-management-access-package-edit/assignments-add-user.png)

1. Erişim paketini atamak istediğiniz kullanıcıları seçmek için **Kullanıcı Ekle** ' ye tıklayın.

1. **Ilke Seç** listesinde, [none (yalnızca yönetici doğrudan atamaları)](#policy-none-administrator-direct-assignments-only) ayarı olan bir ilke seçin.

    Bu erişim paketinde bu ilke türü yoksa, **Yeni Ilke oluştur** ' a tıklayarak bir tane ekleyebilirsiniz.

1. Seçili kullanıcıların atamasının başlamasını ve bitmesini istediğiniz tarih ve saati ayarlayın. Bir bitiş tarihi sağlanmazsa, ilkenin süre sonu ayarları kullanılacaktır.

1. İsteğe bağlı olarak kayıt tutmaya yönelik doğrudan atamanız için bir gerekçe sağlar.

1. Seçili kullanıcıları doğrudan erişim paketine atamak için **Ekle** ' ye tıklayın.

    Birkaç dakika sonra, atamalar listesinde kullanıcıları görmek için **Yenile** ' ye tıklayın.

## <a name="view-who-has-an-assignment"></a>Kimin atamaya sahip olduğunu görüntüleme

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. Etkin atamaların listesini görmek için **atamalar** ' a tıklayın.

1. Ek ayrıntıları görmek için belirli bir atamaya tıklayın.

1. Tüm kaynak rollerinin düzgün şekilde sağlandığı atamaların listesini görmek için, filtre durumuna tıklayın ve **teslim etme**' yi seçin.

    **İstek** sayfasında, kullanıcının karşılık gelen isteğini bularak teslim hatalarıyla ilgili ek ayrıntıları görebilirsiniz.

1. Süre biten atamaları görmek için, filtre durumuna tıklayın ve süre **bitti**' yi seçin.

1. Filtrelenmiş listenin CSV dosyasını indirmek için **İndir**' e tıklayın.

## <a name="view-requests"></a>İstekleri görüntüle

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. **İstekler**' e tıklayın.

1. Ek ayrıntıları görmek için belirli bir isteğe tıklayın.

## <a name="view-a-requests-delivery-errors"></a>Bir isteğin teslim hatalarını görüntüleme

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. **İstekler**' e tıklayın.

1. Görüntülemek istediğiniz isteği seçin.

    İstekte herhangi bir teslim hatası varsa, istek durumu **teslim** edilmemiş olur ve alt durum **kısmen teslim**edilir.

    Herhangi bir teslim hatası varsa, isteğin ayrıntı bölmesinde teslim hatalarının sayısı olacaktır.

1. Tüm isteğin teslim hatalarını görmek için sayıma tıklayın.

## <a name="cancel-a-pending-request"></a>Bekleyen bir isteği iptal etme

Yalnızca henüz teslim edilmemiş bekleyen bir isteği iptal edebilirsiniz.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. **İstekler**' e tıklayın.

1. İptal etmek istediğiniz isteğe tıklayın

1. İstek ayrıntıları bölmesinde **Isteği Iptal et**' e tıklayın.

## <a name="copy-my-access-portal-link"></a>Erişim Portalı bağlantısını Kopyala

Dizininizdeki çoğu kullanıcı erişim portalı 'nda oturum açabilir ve istedikleri erişim paketleri listesini otomatik olarak görebilir. Ancak, henüz dizininizden olmayan dış iş ortağı kullanıcıları için, onlara bir erişim paketi istemek üzere kullanabilecekleri bir bağlantı göndermeniz gerekecektir. 

Bir iç iş ortağına gönderirken erişim portalı 'nın tamamını kopyalamanız önemlidir. Bu, iş ortağının, kendi isteklerini yapmak için dizininizin portalına erişmesini sağlar. 

Bağlantı, "myaccess" ile başlar, bir dizin ipucu içerir ve bir erişim paketi KIMLIĞIYLE biter. Bağlantının aşağıdakilerin tümünü içerdiğinden emin olun:

 `https://myaccess.microsoft.com/@<directory_hint>#/access-packages/<access_package_id>`

Erişim paketi dış kullanıcılar için etkinleştirildiğinden ve dış kullanıcının dizini için bir ilkeniz varsa, dış Kullanıcı erişim paketini istemek için erişim portalı bağlantısını kullanabilir.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. Genel Bakış sayfasında, **erişim portalı My bağlantısını**kopyalayın.

    ![Erişim paketine genel bakış-erişim portalı bağlantısı](./media/entitlement-management-shared/my-access-portal-link.png)

1. Dış iş ortağınıza e-posta gönderin veya bağlantıyı gönderin. Erişim paketini istemek için bağlantısını kullanıcılarla paylaşabilir.

## <a name="change-the-hidden-setting"></a>Gizli ayarı Değiştir

Erişim paketleri varsayılan olarak bulunabilir. Bu, bir ilke bir kullanıcının erişim paketini istemesine izin veriyorsa, erişim paketini otomatik olarak erişim portalı 'nda listelenmiş olarak görebilecekleri anlamına gelir.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. Genel Bakış sayfasında, **Düzenle**' ye tıklayın.

1. **Gizli** ayarını ayarlayın.

    **Hayır**olarak ayarlanırsa, erişim paketi kullanıcının erişim portalında listelenir.

    **Evet**olarak ayarlanırsa, erişim paketi kullanıcının erişim portalı 'nda listelenmez. Bir kullanıcının erişim paketine doğrudan **erişim portalı bağlantısı** varsa, erişim paketini görüntüleyebilecekleri tek yöntem.

## <a name="delete"></a>Sil

Erişim paketi yalnızca etkin kullanıcı atamaları yoksa silinebilir.

**Önkoşul rolü:** Genel yönetici, Kullanıcı Yöneticisi, Katalog sahibi veya erişim paketi Yöneticisi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol menüde, **erişim paketleri** ' ne tıklayın ve ardından erişim paketini açın.

1. Sol taraftaki menüde **atamalar** ' a tıklayın ve tüm kullanıcılar için erişimi kaldırın.

1. Sol menüde **genel bakış** ' a ve ardından **Sil**' e tıklayın.

1. Görüntülenen silme iletisinde **Evet**' e tıklayın.

## <a name="when-are-changes-applied"></a>Değişiklik ne zaman uygulandı

Yetkilendirme Yönetimi 'nde Azure AD, erişim paketinizdeki atama ve kaynaklara yönelik toplu değişiklikleri günde birkaç kez işleyecek. Bu nedenle, bir atama yaparsanız veya erişim paketinizin kaynak rollerini değiştirirseniz, bu değişikliğin Azure AD 'de yapılması ve bu değişiklikleri diğer Microsoft Online hizmetlerine veya bağlı SaaS uygulamasına yayılması için geçen süreyi 24 saate kadar sürebilir olmalıdır Yaptığınız değişiklik yalnızca birkaç nesneyi etkiliyorsa, değişikliğin Azure AD 'de uygulanması birkaç dakika sürer. Bu durumda, diğer Azure AD bileşenleri bu değişikliği algılayacak ve SaaS uygulamalarını güncelleştirmeyecektir. Değişiklik, binlerce nesneyi etkiliyorsa değişiklik daha uzun sürer. Örneğin, 2 uygulama ve 100 Kullanıcı Ataması olan bir erişim paketiniz varsa ve erişim paketine bir SharePoint site rolü eklemeye karar verirseniz, tüm kullanıcılar bu SharePoint sitesi rolünün parçası olana kadar bir gecikme olabilir. İlerlemeyi Azure AD denetim günlüğü, Azure AD sağlama günlüğü ve SharePoint site denetim günlükleri aracılığıyla izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [İşlem ve e-posta bildirimleri iste](entitlement-management-process.md)
