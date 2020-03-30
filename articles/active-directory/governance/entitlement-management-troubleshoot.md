---
title: Sorun giderme yetkilendirme yönetimi - Azure AD
description: Azure Active Directory yetkilendirme yönetimini gidermenize yardımcı olmak için kontrol etmeniz gereken bazı öğeler hakkında bilgi edinin.
services: active-directory
documentationCenter: ''
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.subservice: compliance
ms.date: 03/22/2020
ms.author: barclayn
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7c38e1a61827da547bb39a699a0e92043e63466c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128467"
---
# <a name="troubleshoot-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetimiyle ilgili sorun giderme

Bu makalede, Azure Etkin Dizin (Azure AD) yetkilendirme yönetimini sorun gidermenize yardımcı olmak için denetlemeniz gereken bazı öğeler açıklanmaktadır.

## <a name="administration"></a>Yönetim

* Yetkilendirme yönetimini yapılandırırken erişim reddedilen bir ileti alırsanız ve bir Global yöneticisiyseniz, dizininizin [Azure AD Premium P2 (veya EMS E5) lisansına](entitlement-management-overview.md#license-requirements)sahip olduğundan emin olun.

* Erişim paketleri oluştururken veya görüntülerken erişim reddedilen bir ileti alırsanız ve bir Katalog oluşturucu grubunun üyesiyseniz, ilk erişim paketinizi oluşturmadan önce [bir katalog oluşturmanız](entitlement-management-catalog-create.md) gerekir.

## <a name="resources"></a>Kaynaklar

* Uygulamalara yönelik roller uygulamanın kendisi tarafından tanımlanır ve Azure AD’de yönetilir. Bir uygulamanın kaynak rolü yoksa, yetkilendirme yönetimi kullanıcıları **Varsayılan Erişim** rolüne atar.

    Azure portalının, uygulama olarak seçilemeyen hizmetler için hizmet ilkelerini de gösterebileceğini unutmayın.  Özellikle **Exchange Online** ve **SharePoint Online,** dizinde kaynak rolü olan uygulamalar değil, hizmetlerdir, bu nedenle bir erişim paketine dahil edilemezler.  Bunun yerine, söz konusu hizmetlere erişmesi gereken kullanıcıya uygun lisansı ayarlamak için grup tabanlı lisanslamayı kullanın.

* Grubun herhangi bir erişim paketinde bir kaynak olması için Azure AD’de değiştirilebilir olması gerekir.  Şirket içi Active Directory’den kaynaklanan gruplar kaynak olarak atanamaz çünkü onların sahip veya üye öznitelikleri Azure AD’de değiştirilemez.   Exchange Online’da Dağıtım grupları olarak oluşan gruplar da Azure AD’de değiştirilemez. 

* SharePoint Online belge kitaplıkları ve tek tek belgeleri kaynak olarak eklenemez.  Bunun yerine, bir [Azure REKLAM güvenlik grubu](../fundamentals/active-directory-groups-create-azure-portal.md)oluşturun, bu grubu ve erişim paketindeki site rolünü ekleyin ve SharePoint Online'da belge kitaplığına veya belgeye erişimi denetlemek için bu grubu kullanın.

* Bir erişim paketiyle yönetmek istediğiniz kaynağa önceden atanmış kullanıcılar varsa, kullanıcıların erişim paketine uygun ilkeyle atandığından emin olun. Örneğin zaten grupta kullanıcıları olan bir erişim paketine grup eklemek isteyebilirsiniz. Gruptaki söz konusu kullanıcılara sürekli erişim gerekiyorsa, gruba erişimi kaybetmemeleri için erişim paketlerine yönelik uygun bir ilkeye sahip olmaları gerekir. Erişim paketini atamak için kullanıcıların bu kaynağı içeren erişim paketini istemelerini sağlayabilir veya onları doğrudan erişim paketine atayabilirsiniz. Daha fazla bilgi için erişim [paketi için değişiklik isteği ve onay ayarlarına](entitlement-management-access-package-request-policy.md)bakın.

* Ekibin bir üyesini kaldırdığınızda bu üye Office 365 Grubundan da kaldırılır. Ekibin sohbet işlevinden kaldırma işlemi geciktirilebilir. Daha fazla bilgi için [Grup üyeliğine](https://docs.microsoft.com/microsoftteams/office-365-groups#group-membership)bakın.

* Dizininizin birden çok coğrafi bölge için yapılandırılmadığından emin olun. Yetkilendirme yönetimi şu anda SharePoint Online için birden çok coğrafi konumu desteklemiyor. SharePoint Online sitelerinin yetkilendirme yönetimiyle idare edilmesi için varsayılan coğrafi konumda yer almaları gerekir. Daha fazla bilgi için [OneDrive ve SharePoint Online'daki Çoklu Coğrafi Özellikler'e](https://docs.microsoft.com/Office365/Enterprise/multi-geo-capabilities-in-onedrive-and-sharepoint-online-in-office-365)bakın.

## <a name="access-packages"></a>Erişim paketleri

* Bir erişim paketini veya ilkeyi silmeye çalışır ve etkin atamalar olduğunu belirten bir hata iletisi görürseniz, atamaları olan herhangi bir kullanıcı görmezseniz, yakın zamanda silinen kullanıcıların hala atamaları olup olmadığını kontrol edin. Kullanıcı silindikten sonraki 30 günlük süre boyunca, kullanıcı hesabı geri yüklenebilir.   

## <a name="external-users"></a>Dış kullanıcılar

* Harici bir kullanıcı bir erişim paketine erişim isteğinde bulunmak istediğinde, erişim paketi için **Erişim portalım bağlantısını** kullandığından emin olun. Daha fazla bilgi [için, erişim paketi istemek için Paylaş bağlantısına](entitlement-management-access-package-settings.md)bakın. Harici bir kullanıcı **yalnızca myaccess.microsoft.com** ziyaret ederse ve Erişim portalımın tamamını kullanmıyorsa, kendi kuruluşlarında kullanılabilir erişim paketlerini görür, kuruluşunuzda değil.

* Harici bir kullanıcı bir erişim paketine erişim talebinde bulunamıyorsa veya kaynaklara erişemiyorsa, [ayarlarınızı harici kullanıcılar için](entitlement-management-external-users.md#settings-for-external-users)kontrol ettiğinizden emin olun.

* Daha önce dizininizde oturum açmamış olan yeni bir dış kullanıcı SharePoint Online sitesini içeren bir erişim paketi alırsa, SharePoint Online’da hesabı sağlanana kadar erişim paketi tam olarak teslim edilmemiş olarak gösterilir. Paylaşım ayarları hakkında daha fazla bilgi için [SharePoint Online dış paylaşım ayarlarınızı inceleyin'](entitlement-management-external-users.md#review-your-sharepoint-online-external-sharing-settings)e bakın.

## <a name="requests"></a>İstekler

* Bir kullanıcı bir erişim paketine erişim isteğinde bulunmak istediğinde, erişim paketi için **Erişim portalım bağlantısını** kullandığından emin olun. Daha fazla bilgi [için, erişim paketi istemek için Paylaş bağlantısına](entitlement-management-access-package-settings.md)bakın.

* Gizli moda ayarlanmış tarayıcınızla Erişimim portalını açarsanız, bu durum oturum açma davranışıyla çakışabilir. Erişimim portalını ziyaret ederken tarayıcınızda gizli modu kullanmamanızı öneririz.

* Henüz dizininizde yer almayan bir kullanıcı erişim paketi istemek için Erişimim portalında oturum açarsa, kurumsal hesabını kullanarak kimliğini doğruladığından emin olun. Kurumsal hesap kaynak dizinindeki bir hesap olabileceği gibi, erişim paketinin ilkelerinden birine eklenmiş olan dizindeki bir hesap da olabilir. Kullanıcının hesabı kurumsal hesap değilse veya kimlik doğrulaması yaptığı dizin ilkeye eklenmemişse, kullanıcı erişim paketini görmez. Daha fazla bilgi için [bkz.](entitlement-management-request-access.md)

* Kullanıcının kaynak dizininde oturum açması engellenirse Erişimim portalında erişim isteyemez. Kullanıcının erişim isteyebilmesi için önce kullanıcının profilinden oturum açma engelini kaldırmanız gerekir. Oturum açma bloğunu kaldırmak için Azure portalında **Azure Etkin Dizini'ni**tıklatın, **Kullanıcılar'ı**tıklatın, kullanıcıyı tıklatın ve ardından **Profil'i**tıklatın. **Ayarlar** bölümünü değiştirin ve **Engelle oturum açMasını** **No**olarak değiştirin. Daha fazla bilgi için Azure [Etkin Dizin'i kullanarak kullanıcının profil bilgilerini ekle veya güncelleştir'e](../fundamentals/active-directory-users-profile-azure-portal.md)bakın.  Ayrıca, kullanıcının [kimlik koruma ilkesi](../identity-protection/howto-unblock-user.md)nedeniyle engellenip engellenmediğini de kontrol edebilirsiniz.

* Erişimim portalında, bir kullanıcı hem istekte bulunan hem de onaylayan sayılsa, Onaylar sayfasında erişim paketi isteğini **görmez.** Bu davranış bilerek ayarlanmıştır çünkü kullanıcı kendi isteğini onaylayamaz. İstediği erişim paketinin ilkede yapılandırılmış ek onaylayanları bulunduğundan emin olun. Daha fazla bilgi için erişim [paketi için değişiklik isteği ve onay ayarlarına](entitlement-management-access-package-request-policy.md)bakın.

### <a name="view-a-requests-delivery-errors"></a>İsteğin teslim hatalarını görüntüleme

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Access paketlerini** tıklatın ve ardından erişim paketini açın.

1. **İstekleri**tıklatın.

1. Görüntülemek istediğiniz isteği seçin.

    İstekte herhangi bir teslim hatası varsa, istek durumu **teslim edilmemiş** veya **kısmen teslim**edilir.

    İstek ayrıntı bölmesinde herhangi bir teslim hatası varsa, teslim hataları sayısı olacaktır.

1. İsteğin tüm teslim hatalarını görmek için sayım'ı tıklatın.

### <a name="reprocess-a-request"></a>İsteği yeniden işleme

Bir istek bir hatayla karşılaşırsa, yeniden denemek için isteği yeniden işleyebilirsiniz. Yalnızca Teslim durumu **başarısız** olan veya **Kısmen teslim edilen** ve bir haftadan kısa bir süre içinde tamamlanmış bir tarih olan bir isteği yeniden işleyebilirsiniz.

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Access paketlerini** tıklatın ve ardından erişim paketini açın.

1. **İstekleri**tıklatın.

1. Yeniden işlemek istediğiniz isteği tıklatın.

1. İstek ayrıntıları bölmesinde, **yeniden işleme isteğini**tıklatın.

    ![Başarısız bir isteği yeniden işleme](./media/entitlement-management-troubleshoot/reprocess-request.png)

### <a name="cancel-a-pending-request"></a>Bekleyen isteği iptal etme

Yalnızca henüz teslim edilmemiş veya teslimi başarısız olan bekleyen bir isteği iptal edebilirsiniz.

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Access paketlerini** tıklatın ve ardından erişim paketini açın.

1. **İstekleri**tıklatın.

1. İptal etmek istediğiniz isteği tıklatın.

1. İstek ayrıntıları bölmesinde, **İstek İptal'i**tıklatın.

## <a name="multiple-policies"></a>Birden çok ilke

* Yetkilendirme yönetimi en az ayrıcalık en iyi uygulamaları izler. Bir kullanıcı, geçerli birden çok ilke içeren bir erişim paketine erişim istediğinde, yetkilendirme yönetimi genel ilkelere göre daha katı veya daha spesifik ilkelere öncelik verebilmesini sağlamaya yardımcı olacak mantık içerir. Bir ilke genelse, yetkilendirme yönetimi ilkeyi istekte bulundurmayabilir veya otomatik olarak daha katı bir ilke seçebilir.

* Örneğin, her iki ilkenin de istekte bulunana uygulandığı dahili çalışanlar için iki ilkeiçeren bir erişim paketi düşünün. İlk ilke, isteği içeren belirli kullanıcılar içindir. İkinci ilke, istekte bulunanın üyesi olduğu bir dizindeki tüm kullanıcılar içindir. Bu senaryoda, daha katı olduğundan, ilk ilke istekçi için otomatik olarak seçilir. İstekçiye ikinci ilkeyi seçme seçeneği verilmez.

* Birden çok ilke uygulandığında, otomatik olarak seçilen ilke veya istekte bulunan ilkeler aşağıdaki öncelik mantığını temel alar:

    | İlke önceliği | Kapsam |
    | --- | --- |
    | P1 | Dizininizdeki belirli kullanıcılar ve gruplar VEYA Belirli bağlı kuruluşlar |
    | P2 | Dizininizdeki tüm üyeler (konuklar hariç) |
    | P3 | Dizininizdeki tüm kullanıcılar (konuklar dahil) OR Belirli bağlantılı kuruluşlar |
    | P4 | Tüm bağlı kuruluşlar VEYA Tüm kullanıcılar (tüm bağlı kuruluşlar + herhangi bir yeni dış kullanıcı) |
    
    Herhangi bir ilke daha yüksek öncelik kategorisindeyse, daha düşük öncelik kategorileri yoksayılır. Aynı önceliğe sahip birden çok ilkenin istekte bulunma şekline [bir](entitlement-management-request-access.md#select-a-policy)örnek için bkz.

## <a name="next-steps"></a>Sonraki adımlar

- [Dış kullanıcılara yönelik idare erişimi](entitlement-management-external-users.md)
- [Kullanıcıların yetkilendirme yönetiminde nasıl erişebildiğini görüntüleme](entitlement-management-reports.md)
