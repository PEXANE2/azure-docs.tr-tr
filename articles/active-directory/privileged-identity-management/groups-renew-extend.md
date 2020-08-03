---
title: Privileged Identity Management-Azure AD | içindeki üye atamalarının zaman aşımı grubu sahibini yenileme | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde rol atanabilir grup atamalarını genişletmeyi veya yenilemeyi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: pim
ms.date: 06/30/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0cfbb0be22dee4550050d6af10314f3a3bb1f583
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87505997"
---
# <a name="extend-or-renew-privileged-access-group-assignments-preview-in-privileged-identity-management"></a>Privileged Identity Management 'da ayrıcalıklı erişim grubu atamalarını (Önizleme) genişletme veya yenileme

Azure Active Directory (Azure AD) Privileged Identity Management (PıM), ayrıcalıklı erişim grupları için erişim ve atama yaşam döngüsünü yönetmek üzere denetimler sağlar. Yöneticiler, başlangıç ve bitiş tarih-saat özelliklerini kullanarak roller atayabilir. Atama sonu yaklaşırsa, Privileged Identity Management etkilenen kullanıcılara veya gruplara e-posta bildirimleri gönderir. Ayrıca, uygun erişimin korunduğundan emin olmak için kaynağın yöneticilerine e-posta bildirimleri gönderir. Atamalar yenilenebilir ve erişim genişletilmese bile 30 güne kadar, zaman aşımına uğradı.

## <a name="who-can-extend-and-renew"></a>Kimler genişletebilir ve yenileyebilirler

Yalnızca kaynağın yöneticileri ayrıcalıklı erişim grubu atamalarını genişletebilir veya yenileyebilirler. Etkilenen Kullanıcı veya Grup, sona ermek üzere olan atamaları genişletmeyi ve zaten kullanım süreleri dolan atamaları yenileme isteğinde bulunabilir.

## <a name="when-notifications-are-sent"></a>Bildirimler gönderildiğinde

Privileged Identity Management, e-posta bildirimlerini kullanıcılara ve süresi dolan ayrıcalıklı erişim grubu atamalarının etkilenen kullanıcılarına gönderir:

- Süre sonundan önce 14 gün içinde
- Süresi dolmadan bir gün önce
- Bir atamanın süresi dolarsa

Yöneticiler, bir kullanıcı veya Grup, süresi dolan veya süresi dolan bir atamayı uzatmak veya yenilemek istediğinde bildirim alır. Bir yönetici isteği çözümlediğinde, tüm yöneticilere ve istekte bulunan kullanıcıya onay veya reddetme bildirilir.

## <a name="extend-group-assignments"></a>Grup atamalarını Genişlet

Aşağıdaki adımlarda, bir grup atamasının uzantısı veya yenilenmesini isteme, çözme veya yönetme süreci ana hatlarıyla verilmiştir.

### <a name="self-extend-expiring-assignments"></a>Süresi dolan atamaları kendi kendine uzat

Ayrıcalıklı erişim grubuna atanan kullanıcılar, süresi dolan grup atamalarını grup için **atamalar** sayfasındaki **uygun** veya **etkin** sekmesinden genişletebilir. Kullanıcılar veya gruplar önümüzdeki 14 gün içinde süresi dolan uygun ve etkin atamaları genişletmeyi isteyebilir.

![Rollerim sayfası bir eylem sütunuyla uygun assgnments listeleme](media/groups-renew-extend/self-extend-group-assignment.png)

Atama bitiş tarihi-saati 14 gün içinde olduğunda **Genişlet** komutu kullanılabilir. Bir grup atamasının uzantısını istemek için **Genişlet** ' i seçerek istek formunu açın.

![Grup atama bölmesini bir neden kutusuyla ve ayrıntılarla Genişlet](media/groups-renew-extend/extend-request-details-group-assignment.png)

>[!NOTE]
>Uzantının neden gerekli olduğuna ilişkin ayrıntıları ve uzantının ne kadar süreyle verileceğini (Bu bilgilere sahipseniz) dahil etmenizi öneririz.

Yöneticiler, bir süre içinde, uzantı isteğini gözden geçirdikleri isteyen bir e-posta bildirimi alır. Genişletme isteği zaten gönderilmezse portalda bir Azure bildirimi görüntülenir.

İsteğiniz durumunu görüntülemek veya iptal etmek için, Grup atama için **bekleyen istekler** sayfasını açın.

![Ayrıcalıklı erişim grubu atamaları-Iptal edilecek bağlantıyı gösteren bekleyen istekler sayfası](media/groups-renew-extend/group-assignment-extend-cancel-request.png)

### <a name="admin-approved-extension"></a>Yönetici onaylı uzantısı

Bir kullanıcı veya grup bir grup atamasını genişletme isteği gönderdiğinde, Yöneticiler özgün atamanın ayrıntılarını ve isteğin nedenini içeren bir e-posta bildirimi alır. Bildirim, yöneticinin onaylama veya reddetme isteğine doğrudan bir bağlantı içerir.

Yöneticiler, e-postadaki bağlantıyı izleyen bir ek olarak, Privileged Identity Management yönetim portalına giderek ve sol bölmedeki **Istekleri Onayla** ' yı seçerek istekleri onaylayabilir veya reddedebilir.

![Ayrıcalıklı erişim grubu atamaları-istekleri onaylama sayfası istekleri ve onaylama veya reddetme bağlantıları](media/groups-renew-extend/group-assignment-extend-admin-approve.png)

Yönetici **Onayla** veya **Reddet**seçeneğini belirlediğinde, istek ayrıntıları, denetim günlükleri için bir iş denetimi sağlamak üzere bir alanla birlikte gösterilir.

![İstek sahibi nedeni, atama türü, başlangıç zamanı, bitiş zamanı ve nedenle Grup atama isteğini onaylama](media/groups-renew-extend/group-assignment-extend-admin-approve-reason.png)

Grup atamasını genişletme isteği onaylanırken, kaynak yöneticileri yeni bir başlangıç tarihi, bitiş tarihi ve atama türü seçebilirler. Yönetici belirli bir görevi (örneğin, bir gün) tamamlamaya sınırlı erişim sağlamak isterse, atama türünü değiştirmek gerekli olabilir. Bu örnekte, yönetici atamayı **uygun** durumundan **etkin**olarak değiştirebilir. Bu, etkinleştirmelerine gerek kalmadan istek sahibine erişim sağlayabilecekleri anlamına gelir.

### <a name="admin-initiated-extension"></a>Yönetici tarafından başlatılan uzantı

Gruba atanan bir Kullanıcı, Grup ataması için bir uzantı istemezse, yönetici kullanıcı adına bir atamayı genişletebilir. Grup atamasının yönetim uzantılarına onay gerekmez, ancak bildirimler atama genişletildikten sonra diğer tüm yöneticilere gönderilir.

Bir grup atamasını genişletmek için Privileged Identity Management ' de atama görünümüne gidin. Uzantı gerektiren atamayı bulun. Ardından Eylem sütununda **Genişlet** ' i seçin.

![Uzatma bağlantılarıyla uygun Grup atamalarını listeleyerek atamalar sayfası](media/groups-renew-extend/group-assignment-extend-admin-approve.png)

## <a name="renew-group-assignments"></a>Grup atamalarını Yenile

Kavramsal olarak bir uzantı isteme işlemine benzer olsa da, zaman aşımına uğradı bir grup atamasını yenileme işlemi farklıdır. Aşağıdaki adımları kullanarak, atamalar ve yöneticiler gerektiğinde zaman aşımına uğradı atamalara erişimi yenileyebilirler.

### <a name="self-renew"></a>Kendi kendine yenileme

Artık kaynaklara erişemeyen kullanıcılar, en fazla 30 günlük atama geçmişine erişebilir. Bunu yapmak için, sol bölmedeki **rollerim** 'e gözatıp, ardından süre **dolma atamaları** sekmesini seçin.

![Rollerim sayfası-zaman aşımına uğradı atamalar sekmesi](media/groups-renew-extend/groups-renew-from-my-roles.png)

Gösterilen atamaların listesi, **uygun atamalar**olarak varsayılan olarak gösterilir. Uygun ve etkin atamalar arasında geçiş yapmak için açılan menüyü kullanın.

Listedeki herhangi bir grup atamasının yenilenmesini istemek için **Yenile** eylemini seçin. Ardından istek için bir neden belirtin. Kaynak yöneticisinin onaylama veya reddetme kararı vermesine yardımcı olabilecek ek bağlam veya bir iş gerekçesinin yanı sıra bir süre sağlamak yararlı olur.

![Neden kutusunu gösteren Grup atama bölmesini Yenile](media/groups-renew-extend/groups-renew-request-form.png)

İstek gönderildikten sonra, kaynak yöneticilerine bir grup atamasını yenilemeye yönelik bekleyen bir istek bildirilir.

### <a name="admin-approves"></a>Yönetici onaylar

Kaynak yöneticileri, e-posta bildiriminde bulunan bağlantıdan veya Azure portal Privileged Identity Management erişerek ve sol bölmeden **Istekleri Onayla** ' yı seçerek yenileme isteğine erişebilir.

Yönetici **Onayla** veya **Reddet**seçeneğini belirlediğinde, denetim günlükleri için bir iş denetimi sağlamak üzere isteğin ayrıntıları bir alanla birlikte gösterilir.

Bir grup atamasını yenileme isteği onaylanırken, kaynak yöneticileri yeni bir başlangıç tarihi, bitiş tarihi ve atama türü girmelidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management ayrıcalıklı erişim grubu atamaları için istekleri onaylama veya reddetme](groups-approval-workflow.md)
- [Privileged Identity Management 'da ayrıcalıklı erişim grubu ayarlarını yapılandırma](groups-role-settings.md)
