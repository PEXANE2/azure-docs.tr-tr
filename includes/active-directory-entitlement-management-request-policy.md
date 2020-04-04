---
title: include dosyası
description: include dosyası
services: active-directory
author: msaburnley
ms.service: active-directory
ms.topic: include
ms.date: 03/30/2020
ms.author: ajburnle
ms.custom: include file
ms.openlocfilehash: 7fd716be397d9ef6b9d6132cd4470f653f3cea0f
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80655930"
---
## <a name="for-users-in-your-directory"></a>Dizininizdeki kullanıcılar için

Dizininizdeki kullanıcıların bu erişim paketini isteyebilmesine izin vermek istiyorsanız aşağıdaki adımları izleyin. İstek ilkesini tanımlarken, tek tek kullanıcıları veya daha yaygın kullanıcı gruplarını belirtebilirsiniz. Örneğin, kuruluşunuzun zaten **Tüm çalışanlar**gibi bir grubu olabilir.  Bu grup, erişim isteyebilecek kullanıcılar için ilkeye eklenirse, bu grubun herhangi bir üyesi erişim isteyebilir.

1. Erişim **talebinde bulunabilecek Kullanıcılar** bölümünde, **dizininizdeki kullanıcılar için'** i tıklatın.

    Bu seçeneği seçtiğinizde, dizininizdeki kimler bu erişim paketini isteyebilir daha fazla rafine yeni seçenekler görünür.

    ![Erişim paketi - İstekler - Dizininizdeki kullanıcılar için](./media/active-directory-entitlement-management-request-policy/for-users-in-your-directory.png)

1. Aşağıdaki seçeneklerden birini belirtin:

    |  |  |
    | --- | --- |
    | **Belirli kullanıcılar ve gruplar** | Dizininizde yalnızca bu erişim paketini talep edebilmek için belirttiğiniz kullanıcıları ve grupları istiyorsanız bu seçeneği belirleyin. |
    | **Tüm üyeler (konuklar hariç)** | Dizininizdeki tüm üye kullanıcıların bu erişim paketini isteyebilmesini istiyorsanız bu seçeneği belirleyin. Bu seçenek, dizininize davet etmiş olabileceğiniz konuk kullanıcıları içermez. |
    | **Tüm kullanıcılar (konuklar dahil)** | Dizininizdeki tüm üye kullanıcıların ve konuk kullanıcıların bu erişim paketini isteyebilmesini istiyorsanız bu seçeneği belirleyin. |

    Konuk kullanıcılar, [Azure AD B2B](../articles/active-directory/b2b/what-is-b2b.md)ile dizininize davet edilen harici kullanıcılara başvurur. Üye kullanıcılar ve konuk kullanıcılar arasındaki farklar hakkında daha fazla bilgi için Azure [Etkin Dizin'de varsayılan kullanıcı izinleri nelerdir?](../articles/active-directory/fundamentals/users-default-permissions.md)

1. **Belirli kullanıcıları ve grupları**seçtiyseniz, kullanıcı ve gruplar **ekle'yi**tıklatın.

1. Kullanıcıları ve grupları seç bölmesinde, eklemek istediğiniz kullanıcıları ve grupları seçin.

    ![Erişim paketi - İstekler - Kullanıcıları ve grupları seçin](./media/active-directory-entitlement-management-request-policy/select-users-groups.png)

1. Kullanıcıları ve grupları eklemek için **Seç'i** tıklatın.

1. [Onay](#approval) bölümüne atlayın.

## <a name="for-users-not-in-your-directory"></a>Dizininizde olmayan kullanıcılar için

 **Dizininizde olmayan kullanıcılar,** başka bir Azure AD dizininde veya etki alanında olan kullanıcılar anlamına gelir. Bu kullanıcılar henüz dizininize davet edilmemiş olabilir. Azure REKLAM dizinleri, **İşbirliği kısıtlamalarındaki**davetlere izin verecek şekilde yapılandırılmalıdır. Daha fazla bilgi için [B2B dış işbirliğini etkinleştir mecnuna bakın ve konukları kimlerin davet edebileceğini yönetin.](../articles/active-directory/b2b/delegate-invitations.md)

> [!NOTE]
> İsteği onaylanan veya otomatik olarak onaylanan dizininizde henüz bulunmayan bir kullanıcı için bir konuk kullanıcı hesabı oluşturulur. Konuk davet edilecektir, ancak davet e-postası almayacaktır. Bunun yerine, erişim paketi atamaları teslim edildiğinde bir e-posta alırlar. Varsayılan olarak, daha sonra bu konuk kullanıcının artık herhangi bir erişim paketi ataması yoksa, son atamaları nın süresi dolduğundan veya iptal edildiğinden, konuk kullanıcı hesabının oturum açması engellenir ve daha sonra silinir. Konuk kullanıcıların süresiz olarak dizininizde kalmasını istiyorsanız, erişim paketi atamaları olmasa bile, yetkilendirme yönetimi yapılandırmanızın ayarlarını değiştirebilirsiniz. Konuk kullanıcı nesnesi hakkında daha fazla bilgi için Azure [Etkin Dizin B2B işbirliği kullanıcısının özellikleri](../articles/active-directory/b2b/user-properties.md)bölümüne bakın.

Dizininizde olmayan kullanıcıların bu erişim paketini istemesine izin vermek istiyorsanız aşağıdaki adımları izleyin:

1. Erişim **talebinde bulunabilecek Kullanıcılar** bölümünde, **dizininizde olmayan kullanıcılar için'i**tıklatın.

    Bu seçeneği seçtiğinizde, yeni seçenekler görüntülenir.

    ![Erişim paketi - İstekler - Dizininizde olmayan kullanıcılar için](./media/active-directory-entitlement-management-request-policy/for-users-not-in-your-directory.png)

1. Aşağıdaki seçeneklerden birini belirtin:

    |  |  |
    | --- | --- |
    | **Belirli bağlı kuruluşlar** | Yöneticinizin daha önce ekleyen kuruluşlar listesinden seçim yapmak istiyorsanız bu seçeneği belirleyin. Seçili kuruluşlardan tüm kullanıcılar bu erişim paketini isteyebilir. |
    | **Tüm bağlı kuruluşlar** | Bağlı kuruluşlarınızdaki tüm kullanıcılar bu erişim paketini talep edebilirse bu seçeneği belirleyin. |
    | **Tüm kullanıcılar (Tüm bağlı kuruluşlar + herhangi bir yeni dış kullanıcı)** | Bağlı kuruluşlarınızdaki tüm kullanıcılar bu erişim paketini talep edebilirse ve B2B liste ayarlarına izin verirse veya reddederse, yeni harici kullanıcılar için öncelikli olmalıdırysa bu seçeneği belirleyin. |

    Bağlı kuruluş, ilişki içinde olduğunuz harici bir Azure REKLAM dizini veya etki alanıdır.

1. **Belirli bağlı kuruluşlar**seçtiyseniz, yöneticinizin daha önce ekleyen bağlı kuruluşlar listesinden seçim yapmak için **dizin ekle'yi** tıklatın.

1. Daha önce bağlı bir kuruluş için arama yapmak için adı veya etki alanı adını yazın.

    ![Erişim paketi - İstekler - Dizin seçin](./media/active-directory-entitlement-management-request-policy/select-directories.png)

    İşbirliği yapmak istediğiniz kuruluş listede yoksa, yöneticinizden bu kuruluşa bağlı kuruluş olarak eklemesini isteyebilirsiniz. Daha fazla bilgi için [bkz.](../articles/active-directory/governance/entitlement-management-organization.md)

1. Bağlı kuruluşlarınızı seçtikten sonra **Seç'i**tıklatın.

    > [!NOTE]
    > Seçili bağlı kuruluşlardan tüm kullanıcılar bu erişim paketini talep edebilecektir. Bu, azure B2B izin veren veya reddeden liste tarafından engellenen bu etki alanları sürece, kuruluşla ilişkili tüm alt etki alanlarından Azure AD kullanıcıları içerir. Daha fazla bilgi için [bkz.](../articles/active-directory/b2b/allow-deny-list.md)

1. [Onay](#approval) bölümüne atlayın.

## <a name="none-administrator-direct-assignments-only"></a>Yok (yalnızca yönetici doğrudan atamaları)

Erişim isteklerini atlamak ve yöneticilerin bu erişim paketine doğrudan belirli kullanıcıları atamasına izin vermek istiyorsanız aşağıdaki adımları izleyin. Kullanıcıların erişim paketini istemesi gerekmeyecek. Yaşam döngüsü ayarlarını yine de ayarlayabilirsiniz, ancak istek ayarları yoktur.

1. Erişim **isteğinde bulunabilen Kullanıcılar'da** **Yok 'u tıklatın (yalnızca yönetici doğrudan atamaları**.

    ![Erişim paketi - İstekler - Yalnızca yönetici doğrudan atamaları yok](./media/active-directory-entitlement-management-request-policy/none-admin-direct-assignments-only.png)

    Erişim paketini oluşturduktan sonra, belirli iç ve dış kullanıcıları doğrudan erişim paketine atayabilirsiniz. Harici bir kullanıcı belirtirseniz, dizininizde bir konuk kullanıcı hesabı oluşturulur. Doğrudan bir kullanıcı atama hakkında bilgi için, [erişim paketi için atamaları görüntüle, ekleme ve kaldırma](../articles/active-directory/governance/entitlement-management-access-package-assignments.md)bilgisine bakın.

1. [İstekleri Etkinleştir](#enable-requests) bölümüne atlayın.

## <a name="approval"></a>Onay

Onay bölümünde, kullanıcılar bu erişim paketini istediğinde onay gerekip gerekmediğini belirtirsiniz. Onay ayarları aşağıdaki şekilde çalışır:

- Seçili onaylayanlardan veya geri dönüş onaylayanlardan yalnızca birinin tek aşamalı onay isteğini onaylaması gerekir. 
- Her aşamadan seçilen onaylayıcılardan yalnızca birinin 2 aşamalı onay isteğini onaylaması gerekir.
- Onaylayan, politikanın erişimi yöneten kişiye bağlı olarak Yönetici, İç sponsor veya Dış sponsor olabilir.
- Seçilen her onaylayıcının onayı tek veya 2 aşamalı onay için gerekli değildir.
- Onay kararı, talebi ilk olarak hangi onaylayanın gözden geçirirse ona göre karara bağlıdır.

İstek ilkesine onaylayıcıların nasıl ekleyeceğiniz hakkında bir gösteri için aşağıdaki videoyu izleyin:

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE4cZfg]

İstek ilkesine çok aşamalı onay eklemenin nasıl yapılacağını göstermek için aşağıdaki videoyu izleyin:

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE4d1Jw]

Erişim paketi istekleriiçin onay ayarlarını belirtmek için aşağıdaki adımları izleyin:

1. Seçili kullanıcılardan gelen istekler için onay istemek için Onay **İsteği'ni** **Evet**olarak ayarlayın. Veya isteklerin otomatik olarak onaylanması için, geçiş ini **No**olarak ayarlayın.

1. Kullanıcıların erişim paketini istemek için bir gerekçe sağlamalarını sağlamak için, **İstek İsteme veya yaslamayı** **Evet**olarak ayarlayın.
    
1. Şimdi isteklerin tek veya 2 aşamalı onay gerektirip gerekmeyeceğini belirleyin. Tek aşama onayı için **kaç aşamalı** geçişini **1'e** ayarlayın veya 2 aşamalı onay için geçişini **2'ye** ayarlayın.

    ![Erişim paketi - İstekler - Onay ayarları](./media/active-directory-entitlement-management-request-policy/approval.png)

Kaç aşamaya ihtiyacınız olduğunu seçtikten sonra onaylayıcıeklemek için aşağıdaki adımları kullanın: 

### <a name="single-stage-approval"></a>Tek aşamalı onay

1. İlk **Onaylayıcı**ekle :
    
    İlke dizininizdeki kullanıcıların erişimini yönetecek şekilde ayarlanmışsa, **Yönetici'yi onaylayan olarak**seçebilirsiniz. Veya açılır menüden belirli onaylayıcıları seç seçeneğini seçtikten sonra **onaylayıcıları ekle'yi** tıklatarak belirli bir kullanıcı ekleyin.
    
    ![Erişim paketi - İstekler - Dizindeki kullanıcılar için - İlk Onaylayıcı](./media/active-directory-entitlement-management-request-policy/approval-single-stage-first-approver-manager.png)

    Bu ilke, dizininizde olmayan kullanıcıların erişimini yönetecek şekilde ayarlanmışsa, **Dış sponsor** veya **Dahili sponsor**seçebilirsiniz. Veya belirli onaylayıcıları seç'in altındaki **Onaylayıcılar** veya Gruplar Ekle'yi tıklatarak belirli bir kullanıcı ekleyin.
    
    ![Erişim paketi - İstekler - Dizin dışında kullanıcılar için - İlk Onaylayıcı](./media/active-directory-entitlement-management-request-policy/out-directory-first-approver.png)
    
1. **Yönetici'yi** ilk onaylayıcı olarak seçtiyseniz, geri dönüş onaylayıcısı olmak için dizininizdeki bir veya daha fazla kullanıcı veya grubu seçmek için **geri dönüş ekle'yi** tıklatın. Yetkilendirme yönetimi erişim isteyen kullanıcı için yöneticiyi bulamazsa, geri dönüş onaylayıcıları isteği alır.

    Yönetici, **Yönetici** özniteliğini kullanarak yetkilendirme yönetimi tarafından bulunur. Öznitelik, Kullanıcının Azure AD'deki profilinde dir. Daha fazla bilgi için Azure [Etkin Dizin'i kullanarak kullanıcının profil bilgilerini ekle veya güncelleştir'e](../articles/active-directory/fundamentals/active-directory-users-profile-azure-portal.md)bakın.

1. **Belirli onaylayıcılar seçin'i**seçtiyseniz, onaylayıcı olmak için dizininizdeki bir veya daha fazla kullanıcı veya grubu seçmek için **onaylayıcıekle'yi** tıklatın.

1. Karar altında kutusunda **kaç gün içinde yapılmalıdır?**

    Bu süre içinde bir istek onaylanmazsa, otomatik olarak reddedilir. Kullanıcının erişim paketi için başka bir istek göndermesi gerekir.

1. Onaylayanların kararları için bir gerekçe sağlamalarını istemek için, **Onaylayıcıyı Evet**olarak ayarlamayı ayarlayın.

    Gerekçe, diğer onaylayıcılar ve talepçi tarafından görülebilir.

### <a name="2-stage-approval"></a>2 aşamalı onay

2 aşamalı bir onay seçtiyseniz, ikinci bir onaylayıcı eklemeniz gerekir.

1. İkinci **Onaylayıcı**ekle : 
    
    Kullanıcılar dizininizdeyse, belirli onaylayıcıları seç altında **Onaylayıcıları Ekle'yi** tıklatarak ikinci onaylayıcı olarak belirli bir kullanıcı ekleyin.

    ![Erişim paketi - İstekler - Dizindeki kullanıcılar için - İkinci Onaylayıcı](./media/active-directory-entitlement-management-request-policy/in-directory-second-approver.png)

    Kullanıcılar dizininizde yoksa, ikinci onaylayıcı olarak **Dahili sponsoru** veya **Dış sponsor'u** seçin. Onaylayıcıyı seçtikten sonra geri dönüş onaylayıcılarını ekleyin.

    ![Erişim paketi - İstekler - Dizin dışında kullanıcılar için - İkinci Onaylayıcı](./media/active-directory-entitlement-management-request-policy/out-directory-second-approver.png) 

1. Karar kapsamında ikinci onaylayanın isteği kutusunda onaylaması gereken gün sayısını **kaç gün içinde belirtin?** 

1. **Onaylayıcı** yımını Evet veya **Hayır**olarak ayarlamayı ayarlayın.

### <a name="alternate-approvers"></a>Alternatif onaylayıcılar

İstekleri onaylayan birinci ve ikinci onaylayıcıları belirtmeye benzer şekilde alternatif onaylayıcılar belirtebilirsiniz. Alternatif onaylayıcıların olması, isteklerin süresi dolmadan önce onaylanmasına veya reddedilmesine (zaman aşımı) yardımcı olur. Alternatif onaylayıcıları ilk onaylayıcıyı ve ikinci onaylayıcıyı 2 aşamalı onay için listeleyebilirsiniz. 

Alternatif onaylayıcılar belirterek, birinci veya ikinci onaylayanların isteği onaylayamayacağı veya reddedememesi durumunda, bekleyen istek, ilke kurulumu sırasında belirttiğiniz iletme zamanlamasına göre alternatif onaylayıcılara iletilir. Bekleyen isteği onaylamak veya reddetmek için bir e-posta alırlar.

İstek alternatif onaylayıcılara iletildikten sonra, birinci veya ikinci onaylayıcılar isteği yine de onaylayabilir veya reddedebilir. Alternatif onaylayıcılar, bekleyen isteği onaylamak veya reddetmek için aynı Erişimim sitesini kullanır.

Kişileri veya kişi gruplarını onaylayan ve alternatif onaylayıcı olarak listeleyebiliriz. Lütfen birinci, ikinci ve alternatif onaylayıcılar olacak farklı kişi kümelerini listelediğinden emin olun.
Örneğin, Gamze ve Bob'u İlk Onaylayıcı(lar) olarak listelediyseniz, Carol ve Dave'i alternatif onaylayıcılar olarak listele. Bir erişim paketine alternatif onaylayıcılar eklemek için aşağıdaki adımları kullanın:

1. İlk Onaylayan, İkinci Onaylayıcı veya her ikisi altında **gelişmiş istek ayarlarını göster'i**tıklatın.

    ![Erişim paketi - İlke - Gelişmiş istek ayarlarını göster](./media/active-directory-entitlement-management-request-policy/alternate-approvers-click-advanced-request.png)

1. **Herhangi bir işlem yapılmazsa, alternatif onaylayıcılara geçirin?** **Yes**

1. **Alternatif onaylayıcılar ekle'yi** tıklatın ve listeden alternatif onaylayıcıyı seçin.

    ![Erişim paketi - İlke - Alternatif Onaylayıcılar Ekle](./media/active-directory-entitlement-management-request-policy/alternate-approvers-add.png)

1. **İleri'de kaç gün sonra alternatif onaylayıcı(lar)** kutusunda, onaylayanların bir isteği onaylaması veya reddetmesi gereken gün sayısı koyun. İstek süresinden önce hiçbir onaylayıcı isteği onaylamamış veya reddetmemişse, istek süresi (zaman aşımı) sona erer ve kullanıcı erişim paketi için başka bir istek göndermek zorundadır. 

    İstekler, istek süresinin yarı ömrüne ulaşmasından bir gün sonra alternatif onaylayıcılara iletilebilir. Bu örnekte, istek süresi 14 gündür. Yani, istek süresi 7. Bu nedenle istek 8. Ayrıca, istekler istek süresinin son gününde iletilemeyebilir. Yani örnekte, en son istek iletilebilir gün 13.

## <a name="enable-requests"></a>İstekleri etkinleştirme

1. Erişim paketinin istek ilkesindeki kullanıcıların istekte bulunması için hemen kullanılabilir hale verilmesini istiyorsanız, Geçiş Yap'ı **Evet'e**taşıyın.

    Erişim paketini oluşturmayı bitirdikten sonra gelecekte her zaman etkinleştirebilirsiniz.

    **Yok'u (yalnızca yönetici doğrudan atamaları)** seçtiyseniz ve etkinleştirmeyi **Hayır**olarak ayarlarsanız, yöneticiler bu erişim paketini doğrudan atayamaz.

    ![Erişim paketi - İlke- İlke ayarını etkinleştir](./media/active-directory-entitlement-management-request-policy/enable-requests.png)

1. **İleri**’ye tıklayın.
