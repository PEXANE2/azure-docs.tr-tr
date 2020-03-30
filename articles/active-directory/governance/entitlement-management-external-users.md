---
title: Azure AD yetkilendirme yönetiminde harici kullanıcılar için erişimi yönetin - Azure Etkin Dizini
description: Azure Active Directory yetkilendirme yönetiminde harici kullanıcıların erişimini yönetmek için belirtebileceğiniz ayarlar hakkında bilgi edinin.
services: active-directory
documentationCenter: ''
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 03/22/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: af0f68ca9520c1715463212da80aaabed48f8269
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128690"
---
# <a name="govern-access-for-external-users-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde harici kullanıcılar için erişimi yönetin

Azure AD yetkilendirme yönetimi, kuruluşunuz dışındaki kişilerle başka bir dizinde işbirliği yapmak için [Azure AD'yi işletmeden işletmeye (B2B)](../b2b/what-is-b2b.md) kullanır. Azure AD B2B ile harici kullanıcılar kendi dizini için kimlik doğrulaması verir, ancak dizininizde bir gösterime sahiptir. Dizininizdeki gösterim, kullanıcının kaynaklarınıza erişim atanmasını sağlar.

Bu makalede, dış kullanıcıların erişimini yönetmek için belirtebileceğiniz ayarlar açıklanmaktadır.

## <a name="how-entitlement-management-can-help"></a>Yetkilendirme yönetimi nasıl yardımcı olabilir?

[Azure AD B2B](../b2b/what-is-b2b.md) davet deneyimini kullanırken, kaynak dizininize getirmek ve birlikte çalışmak istediğiniz harici konuk kullanıcıların e-posta adreslerini zaten bilmeniz gerekir. Bu, daha küçük veya kısa vadeli bir proje üzerinde çalışırken ve tüm katılımcıları zaten tanıyorsanız harika çalışır, ancak çalışmak istediğiniz çok sayıda kullanıcınız varsa veya katılımcılar zaman içinde değişirse bunu yönetmek daha zordur.  Örneğin, başka bir kuruluşla çalışıyor olabilirsiniz ve bu kuruluşla bir iletişim noktanız olabilir, ancak zaman içinde bu kuruluştan ek kullanıcıların da erişilmesi gerekir.

Yetkilendirme yönetimi yle, belirttiğiniz kuruluşlardaki kullanıcıların bir erişim paketini kendi kendine isteyebilmelerine olanak tanıyan bir ilke tanımlayabilirsiniz. Onay gerekip gerekmediğini ve erişim için bir son kullanma tarihi belirleyebilirsiniz. Onay gerekiyorsa, dış kuruluştan bir veya daha fazla kullanıcıyı dizininize davet edebilir ve bunları onaylayıcı olarak atayabilirsiniz - çünkü büyük olasılıkla kuruluşlarındaki hangi dış kullanıcıların erişmeye ihtiyacı olduğunu bileceklerdir. Erişim paketini yapılandırdıktan sonra, erişim paketinin bağlantısını dış kuruluştaki ilgili kişiye (sponsor) gönderebilirsiniz. Bu kişi dış kuruluştaki diğer kullanıcılarla paylaşabilir ve erişim paketini istemek için bu bağlantıyı kullanabilir. Bu kuruluştan dizininize davet edilmiş kullanıcılar da bu bağlantıyı kullanabilir.

Bir istek onaylandığında, yetkilendirme yönetimi kullanıcıya gerekli erişimi sağlar ve bu da dizininizde zaten yoksa kullanıcıyı davet etmeyi içerebilir. Azure AD, onlar için otomatik olarak bir B2B konuk hesabı oluşturur. Bir yöneticinin, diğer kuruluşlara davetlere izin vermek veya engellemek için [b2B izin verme veya reddetme listesi](../b2b/allow-deny-list.md) ayarlayarak, daha önce hangi kuruluşların işbirliği için izin verilebilenleri sınırlamış olabileceğini unutmayın.  Kullanıcıya izin verme veya engelleme listesi tarafından izin verilmezse, kullanıcı davet edilmez.

Dış kullanıcının erişiminin sonsuza kadar sürmesini istemediğiniz için, ilkede 180 gün gibi bir son kullanma tarihi belirtirsiniz. 180 gün sonra, erişimleri genişletilmezse, yetkilendirme yönetimi bu erişim paketiyle ilişkili tüm erişimi kaldırır. Varsayılan olarak, yetkilendirme yönetimi aracılığıyla davet edilen kullanıcının başka bir erişim paketi ataması yoksa, son atamalarını kaybettiğinde konuk hesabının 30 gün boyunca oturum açması engellenir ve daha sonra kaldırılır. Bu gereksiz hesapların çoğalmasını önler. Aşağıdaki bölümlerde açıklandığı gibi, bu ayarlar yapılandırılabilir.

## <a name="how-access-works-for-external-users"></a>Dış kullanıcılar için erişim nasıl çalışır?

Aşağıdaki diyagram ve adımlar, harici kullanıcılara bir erişim paketine nasıl erişim verildiğine genel bir bakış sağlar.

![Harici kullanıcıların yaşam döngüsünü gösteren diyagram](./media/entitlement-management-external-users/external-users-lifecycle.png)

1. İşbirliği yapmak istediğiniz Azure REKLAM dizini veya etki alanı için [bağlı bir kuruluş eklersiniz.](entitlement-management-organization.md)

1. Dizininizde, [dizininizde olmayan kullanıcılar için](entitlement-management-access-package-create.md#for-users-not-in-your-directory)bir ilke içeren bir erişim paketi oluşturursunuz.

1. Erişim paketini istemek için kullanıcılarıyla paylaşabilecekleri harici kuruluştaki kişinize Bir [Erişimim portalı bağlantısı](entitlement-management-access-package-settings.md) gönderirsiniz.

1. Harici bir kullanıcı (Bu örnekte**İstekçi A)** erişim paketine [erişim istemek](entitlement-management-request-access.md) için Erişim portalım bağlantısını kullanır. Kullanıcının nasıl imzaladığı, bağlı kuruluşta tanımlanan dizinin veya etki alanının kimlik doğrulama türüne bağlıdır.

1. Bir onaylayıcı [isteği onaylar](entitlement-management-request-approve.md) (veya istek otomatik olarak onaylanır).

1. İstek [teslim durumuna](entitlement-management-process.md)gider.

1. B2B davet işlemini kullanarak, bu örnekte dizininizde **(İstekli A (Konuk)** bir konuk kullanıcı hesabı oluşturulur. İzin [veren bir liste veya reddet listesi](../b2b/allow-deny-list.md) tanımlanırsa, liste ayarı uygulanır.

1. Konuk kullanıcıya erişim paketindeki tüm kaynaklara erişim atanır. Azure AD'de ve diğer Microsoft Çevrimiçi Hizmetlerinde veya bağlı SaaS uygulamalarında değişiklik yapılması biraz zaman alabilir. Daha fazla bilgi için, [değişikliklerin ne zaman uygulandığına](entitlement-management-access-package-resources.md#when-changes-are-applied)bakın.

1. Harici kullanıcı, erişimlerinin [teslim](entitlement-management-process.md)edildiğini belirten bir e-posta alır.

1. Kaynaklara erişmek için dış kullanıcı e-postadaki bağlantıyı tıklatabilir veya davet işlemini tamamlamak için doğrudan dizin kaynaklarından herhangi biri ne kadar erişmeye çalışabilirsiniz.

1. İlke ayarlarına bağlı olarak, zaman geçtikçe, dış kullanıcı için erişim paketi atamasının süresi doluyor ve dış kullanıcının erişimi kaldırılır.

1. Harici kullanıcı ayarlarının yaşam döngüsüne bağlı olarak, harici kullanıcının artık erişim paketi atamaları olmadığında, dış kullanıcının oturum açması engellenir ve konuk kullanıcı hesabı dizininizden kaldırılır.

## <a name="settings-for-external-users"></a>Harici kullanıcılar için ayarlar

Kuruluşunuzun dışındaki kişilerin erişim paketleri isteyebilmesini ve bu erişim paketlerindeki kaynaklara erişebilmesini sağlamak için, doğru şekilde yapılandırıldığından doğrulamanız gereken bazı ayarlar vardır.

### <a name="enable-catalog-for-external-users"></a>Dış kullanıcılar için kataloğu etkinleştirme

- Varsayılan olarak, yeni bir [katalog](entitlement-management-catalog-create.md)oluşturduğunuzda, dış kullanıcıların katalogda erişim paketleri istemesine izin vermek etkinleştirilir. Harici **kullanıcılar için Etkin'in** **Evet**olarak ayarlandıklarına emin olun.

    ![Katalog ayarlarını değiştir](./media/entitlement-management-shared/catalog-edit.png)

### <a name="configure-your-azure-ad-b2b-external-collaboration-settings"></a>Azure AD B2B dış işbirliği ayarlarınızı yapılandırın

- Konukların diğer konukları rehberinize davet etmesine izin vermek, konuk davetlerinin yetkilendirme yönetimi dışında gerçekleşebileceği anlamına gelir. Konukları yalnızca uygun şekilde yönetilen davetlere izin vermek için **Hayır'a** **davet edebilirler.**
- B2B izin listesini kullanıyorsanız, yetkilendirme yönetimini kullanarak ortak olmak istediğiniz herhangi bir etki alanının listeye eklenmiştir. Alternatif olarak, B2B reddet listesini kullanıyorsanız, ortak olmak istediğiniz herhangi bir etki alanının listeye eklenmediğinden emin olmalısınız.
- **Tüm kullanıcılar (tüm** bağlı kuruluşlar + herhangi bir yeni dış kullanıcı) için bir yetkilendirme yönetimi ilkesi oluşturursanız, sahip olduğunuz liste ayarlarına izin veren veya reddeden herhangi bir B2B öncelikli olacaktır. Bu nedenle, kullanıyorsanız bu politikaya eklemek istediğiniz etki alanlarını izin listenize eklediğinizden emin olun ve bir reddedilme listesi kullanıyorsanız bunları reddet listenizden hariç tutun.
- **Tüm kullanıcıları (tüm** bağlı kuruluşlar + herhangi bir yeni dış kullanıcı) içeren bir yetkilendirme yönetimi ilkesi oluşturmak istiyorsanız, öncelikle dizininiz için tek seferlik parola kimlik doğrulamasını etkinleştirmeniz gerekir. Daha fazla bilgi için [e-posta bir kerelik parola kimlik doğrulaması (önizleme)](../b2b/one-time-passcode.md#opting-in-to-the-preview)bakın.
- Azure AD B2B dış işbirliği ayarları hakkında daha fazla bilgi için [B2B dış işbirliğini etkinleştir mesi'ye bakın ve konukları kimlerin davet edebileceğini yönetin.](../b2b/delegate-invitations.md)

    ![Azure AD dış işbirliği ayarları](./media/entitlement-management-external-users/collaboration-settings.png)

### <a name="review-your-conditional-access-policies"></a>Koşullu Erişim ilkelerinizi gözden geçirin

- Yeni konuk kullanıcıların karşılayamayabileceği Koşullu Erişim ilkelerinden, dizininizde oturum açmalarını engelleyeceği nden, konukları herhangi bir Koşullu Erişim politikasından hariç tuttunuz. Örneğin, konukların kayıtlı bir aygıtı yoktur, bilinen bir konumda değildir ve çok faktörlü kimlik doğrulama (MFA) için yeniden kaydolmak istemezler, bu nedenle bu gereksinimleri koşullu erişim ilkesine eklemek konukların yetkilendirmeyi kullanmasını engeller Yönetimi. Daha fazla bilgi için bkz: [Azure Etkin Dizin Koşullu Erişim'deki koşullar nelerdir?](../conditional-access/concept-conditional-access-conditions.md)

    ![Azure AD Koşullu Erişim ilkesi ayarları hariç tutar](./media/entitlement-management-external-users/conditional-access-exclude.png)

### <a name="review-your-sharepoint-online-external-sharing-settings"></a>SharePoint Online dış paylaşım ayarlarınızı inceleyin

- Harici kullanıcılar için erişim paketlerinize SharePoint Online sitelerini eklemek istiyorsanız, kuruluş düzeyindeki dış paylaşım ayarınızın **Herkes** (kullanıcılar oturum açma gerektirmez) veya **Yeni ve mevcut konuklara** (konukların oturum açmaları veya doğrulama kodu sağlamaları) ayarlandığınızdan emin olun. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/sharepoint/turn-external-sharing-on-or-off#change-the-organization-level-external-sharing-setting)

- Yetkilendirme yönetimi dışında herhangi bir dış paylaşımı kısıtlamak istiyorsanız, dış paylaşım ayarını **Varolan misafirleriçin**ayarlayabilirsiniz. Daha sonra, yalnızca yetki yönetimi aracılığıyla davet edilen yeni kullanıcılar bu sitelere erişebilir. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/sharepoint/turn-external-sharing-on-or-off#change-the-organization-level-external-sharing-setting)

- Site düzeyi ayarlarının konuk erişimini sağladığından emin olun (daha önce listelenen seçenek seçimleri). Daha fazla bilgi için, [bir site için dış paylaşımı aç veya kapat' a](https://docs.microsoft.com/sharepoint/change-external-sharing-site)bakın.

### <a name="review-your-office-365-group-sharing-settings"></a>Office 365 grup paylaşım ayarlarınızı gözden geçirin

- Harici kullanıcılar için erişim paketlerinize Office 365 gruplarını eklemek istiyorsanız, **kullanıcıların kuruluşa yeni misafirler eklemesine izin** vererek konuk erişimine izin vermek üzere A.B.D.'ye ayarlandıklarına emin olun. **On** Daha fazla bilgi için [bkz.](https://docs.microsoft.com/office365/admin/create-groups/manage-guest-access-in-groups?view=o365-worldwide#manage-groups-guest-access)

- Harici kullanıcıların SharePoint Online sitesine ve bir Office 365 grubuyla ilişkili kaynaklara erişebilmesini istiyorsanız, SharePoint Online dış paylaşımını açtığınızdan emin olun. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/sharepoint/turn-external-sharing-on-or-off#change-the-organization-level-external-sharing-setting)

- PowerShell'deki dizin düzeyinde Office 365 gruplarının konuk ilkesini nasıl ayarlayınız hakkında bilgi için [bkz.](../users-groups-roles/groups-settings-cmdlets.md#example-configure-guest-policy-for-groups-at-the-directory-level)

### <a name="review-your-teams-sharing-settings"></a>Takımlar paylaşım ayarlarınızı inceleyin

- Dış kullanıcılar için erişim paketlerinize Takımlar eklemek istiyorsanız, Microsoft **Teams'deki Konuk Erişimine İzin Ver'in** konuk erişimine izin vermek için **Aç'a** ayarlandıklarına emin olun. Daha fazla bilgi için microsoft [teams yönetici merkezinde konuk erişimini yapılandır'](/microsoftteams/set-up-guests#configure-guest-access-in-the-teams-admin-center)a bakın.

## <a name="manage-the-lifecycle-of-external-users"></a>Harici kullanıcıların yaşam döngüsünü yönetme

Bir erişim paketi isteği onaylanan bir diziniçin davet edilen harici bir kullanıcının artık erişim paketi atamaları olmadığında ne olacağını seçebilirsiniz. Bu, kullanıcı tüm erişim paketi atamalarından feragat ederse veya son erişim paketi atamasının süresi dolduğunda gerçekleşebilir. Varsayılan olarak, harici bir kullanıcının artık herhangi bir erişim paketi ataması yoksa, dizininize oturum açmaları engellenir. 30 gün sonra konuk kullanıcı hesapları dizininizden kaldırılır.

**Önkoşul rolü:** Genel yönetici veya Kullanıcı yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Yetkilendirme yönetimi** bölümünde **Ayarlar'ı**tıklatın.

1. **Düzenle**’ye tıklayın.

    ![Harici kullanıcıların yaşam döngüsünü yönetmek için ayarlar](./media/entitlement-management-external-users/settings-external-users.png)

1. Dış **kullanıcıların yaşam döngüsünü yönet** bölümünde, dış kullanıcılar için farklı ayarları seçin.

1. Harici bir kullanıcı herhangi bir erişim paketlerine son atamasını kaybettiğinde, bu dizine oturum açmalarını engellemek istiyorsanız, **Blok dış kullanıcının bu dizine Evet** olarak oturum açmasını ayarlayın. **Yes**

    > [!NOTE]
    > Bir kullanıcının bu dizinde oturum açması engellenirse, kullanıcı erişim paketini yeniden isteyemez veya bu dizinde ek erişim isteyemez. Daha sonra diğer erişim paketlerine erişim istemeleri gerekiyorsa, oturum açmalarını engelleyen yapılandırma yapmayın.

1. Harici bir kullanıcı herhangi bir erişim paketine son atamasını kaybettiğinde, bu dizinde konuk kullanıcı hesabını kaldırmak istiyorsanız, **dış kullanıcıyı** **Evet**olarak kaldır'ı ayarlayın.

    > [!NOTE]
    > Yetkilendirme yönetimi yalnızca yetkilendirme yönetimi aracılığıyla davet edilen hesapları kaldırır. Ayrıca, bu kullanıcı paket atamalarına erişmeyen bu dizindeki kaynaklara eklense bile, kullanıcının oturum açması engellenir ve bu dizinden kaldırılır. Konuk, erişim paketi atamaları almadan önce bu dizinde mevcutsa, bunlar kalır. Ancak, konuk bir erişim paketi ataması yoluyla davet edildiyse ve davet edildikten sonra da OneDrive for Business veya SharePoint Online sitesine atandıysa, yine de kaldırılır.

1. Bu dizindeki konuk kullanıcı hesabını kaldırmak istiyorsanız, kaldırılmadan önceki gün sayısını ayarlayabilirsiniz. Konuk kullanıcı hesabını herhangi bir erişim paketine son atamalarını kaybeder kaybetmez kaldırmak istiyorsanız, **dış kullanıcıyı bu dizinden** **0'a**çıkarmadan önce gün sayısını ayarlayın.

1. **Kaydet**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Bağlı kuruluş ekleme](entitlement-management-organization.md)
- [Dizininizde olmayan kullanıcılar için](entitlement-management-access-package-request-policy.md#for-users-not-in-your-directory)
- [Sorun giderme](entitlement-management-troubleshoot.md)
