---
title: PıM-Azure Active Directory 'de Azure Kaynak rol atamalarını genişletme veya yenileme | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure Kaynak rol atamalarını genişletmeyi veya yenilemeyi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 04/02/2018
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9eeba148945f7aa52dd32edc0fec4c45e2ab0748
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804142"
---
# <a name="extend-or-renew-azure-resource-role-assignments-in-pim"></a>PıM 'de Azure Kaynak rol atamalarını genişletme veya yenileme

Azure Active Directory (Azure AD) Privileged Identity Management (PıM), Azure kaynakları için erişim ve atama yaşam döngüsünü yönetmek üzere yeni denetimler sunmaktadır. Yöneticiler, başlangıç ve bitiş tarih-saat özelliklerini kullanarak üyelik atayabilir. Atama bitişi yaklaşırsa, PıM etkilenen kullanıcılara veya gruplara e-posta bildirimleri gönderir. Ayrıca, uygun erişimin korunduğundan emin olmak için kaynağın yöneticilerine e-posta bildirimleri gönderir. Atamalar yenilenebilir ve erişim genişletilmese bile 30 güne kadar, zaman aşımına uğradı.

## <a name="who-can-extend-and-renew"></a>Kimler genişletebilir ve yenileyebilirim?

Rol atamalarını yalnızca kaynağın yöneticileri genişletebilir veya yenileyebilirler. Etkilenen üye, sona ermek üzere olan rolleri genişletmeyi isteyebilir ve zaten kullanım süreleri dolan rolleri yenileme isteğinde bulunabilir.

## <a name="when-are-notifications-sent"></a>Bildirimler ne zaman gönderilir?

PıM, son geçerlilik süresi dolmadan önce 14 gün içinde ve bir gün içinde süresi dolan roller ve etkilenen rollerin üyelerine e-posta bildirimleri gönderir. Atama resmi olarak sona erdiğinde ek bir e-posta gönderir. 

Yöneticiler, süresi dolan veya süresi dolan rolün bir üyesi genişletmek veya yenilemek istediğinde bildirim alır. Belirli bir yönetici isteği çözümlediğinde, diğer tüm yöneticilere çözümleme kararı (onaylanan veya reddedildi) bildirilir. Daha sonra, istenen üyeye karar bildirilir. 

## <a name="extend-role-assignments"></a>Rol atamalarını uzat

Aşağıdaki adımlarda bir rol atamasının uzantısı veya yenilenmesini isteme, çözme veya yönetme işlemi ana hatlarıyla verilmiştir. 

### <a name="member-extend"></a>Üye genişletme

Rol atamasının üyeleri, süresi dolan rol atamalarını bir kaynağın **rollerim** sayfasındaki **uygun** veya **etkin** sekmesinden ve PIM portalının en üst düzey **Roller** sayfasından genişletebilir. Üyeler, sonraki 14 gün içinde süresi dolan uygun ve etkin (atanan) rolleri genişletmeyi isteyebilir.

![Azure kaynakları-rollerim sayfası bir eylem sütunuyla uygun rolleri listeleme](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-ui.png)

Atama bitiş tarihi-saati 14 gün içinde olduğunda, **genişletilecek** düğme Kullanıcı arabirimindeki etkin bir bağlantı haline gelir. Aşağıdaki örnekte, geçerli tarihin 27 Mart olduğunu varsayalım.

![Etkinleştirilecek veya genişletilir bağlantıları olan eylem sütunu](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-within-14.png)

Bu rol atamasının bir uzantısını istemek için **Genişlet** ' i seçerek istek formunu açın.

![Rol atama bölmesini bir neden kutusuyla Genişlet](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-role-assignment-request.png)

Orijinal atama hakkında bilgi görüntülemek için **atama ayrıntıları**' nı genişletin. Uzantı isteği için bir neden girin ve **Genişlet**' i seçin.

>[!Note]
>Uzantının neden gerekli olduğuna ilişkin ayrıntıları ve uzantının ne kadar süreyle verileceğini (Bu bilgilere sahipseniz) dahil etmenizi öneririz.

![Rol atama bölmesini, atama ayrıntıları genişletilmiş olarak Genişlet](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-form-complete.png)

Kaynak yöneticileri, bir süre içinde, uzantı isteğini gözden geçirmesini isteyen bir e-posta bildirimi alır. Genişletme isteği zaten gönderilirse, hatayı açıklayan Azure portal üst kısmında bir bildirim bildirimi görüntülenir.

![Zaten var olan bir bekleyen rol atama uzantısının olduğunu açıklayan bildirim](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-failed-existing-request.png)

İsteğinizin durumunu görüntülemek veya iptal etmek için sol bölmedeki **bekleyen istekler** sayfasına gidin.

![Azure kaynakları-bekleyen istenen ve Iptal için bir bağlantının listelendiği bekleyen istekler sayfası](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-cancel-request.png)

### <a name="admin-approve"></a>Yönetici onaylama

Bir üye bir rol atamasını genişletme isteği gönderdiğinde, kaynak yöneticileri özgün atamanın ayrıntılarını ve isteğin nedenini içeren bir e-posta bildirimi alır. Bildirim, yöneticinin onaylama veya reddetme isteğine doğrudan bir bağlantı içerir. 

Yöneticiler, e-postadaki bağlantıyı izleyen bir ek olarak, PıM yönetim portalına giderek ve sol bölmedeki **Istekleri Onayla** ' yı seçerek istekleri onaylayabilir veya reddedebilirler.

![Azure kaynakları-istekleri onaylama sayfası istekleri ve onaylama veya reddetme bağlantıları](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-approve-grid.png)

Yönetici **Onayla** veya **Reddet**seçeneğini belirlediğinde, istek ayrıntıları denetim günlükleri için gerekçe sağlamak üzere bir alanla birlikte gösterilir.

![İstek sahibi nedeni, atama türü, başlangıç zamanı, bitiş zamanı ve nedenle rol atama isteğini onaylayın](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-approve-blade.png)

Rol atamasını genişletme isteği onaylarken, kaynak yöneticileri yeni bir başlangıç tarihi, bitiş tarihi ve atama türü seçebilir. Yönetici belirli bir görevi (örneğin, bir gün) tamamlamaya sınırlı erişim sağlamak isterse, atama türünü değiştirmek gerekli olabilir. Bu örnekte, yönetici atamayı **uygun** durumundan **etkin**olarak değiştirebilir. Bu, etkinleştirmelerine gerek kalmadan istek sahibine erişim sağlayabilecekleri anlamına gelir.

### <a name="admin-extend"></a>Yöneticiyi genişlet

Bir rol üyesi bir rol üyeliği uzantısı istiyorsa, bir yönetici üye adına bir atamayı genişletebilir. Rol üyeliğinin yönetim uzantılarına onay gerekmez, ancak bildirimler rol genişletildikten sonra diğer tüm yöneticilere gönderilir.

Rol üyeliğini genişletmek için, PıM 'de kaynak rolüne veya üye görünümüne gidin. Uzantı gerektiren üyeyi bulun. Ardından Eylem sütununda **Genişlet** ' i seçin.

![Azure kaynakları-uzatma bağlantılarıyla uygun rolleri listeleyerek Üyeler sayfası](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-extend.png)

## <a name="renew-role-assignments"></a>Rol atamalarını Yenile

Kavramsal olarak bir uzantı isteme işlemine benzer olsa da, zaman aşımına uğradı bir rol atamasını yenileme işlemi farklıdır. Aşağıdaki adımları kullanarak, Üyeler ve yöneticiler gerektiğinde zaman aşımına uğradı rollere erişimi yenileyebilirler.

### <a name="member-renew"></a>Üye yenileme

Artık kaynaklara erişemeyen Üyeler, en fazla 30 günlük atama geçmişine erişebilir. Bunu yapmak için, sol bölmedeki **rollerim** 'e gözatıp, sonra da Azure Kaynak rolleri bölümünde **zaman aşımına uğradı roller** sekmesini seçin.

![Rollerim sayfası-zaman aşımına uğradı roller sekmesi](media/pim-resource-roles-renew-extend/aadpim-rbac-renew-from-myroles.png)

Gösterilen roller listesi, varsayılan olarak **uygun rollerdir**. Uygun ve etkin atanan roller arasında geçiş yapmak için açılan menüyü kullanın.

Listedeki rol atamalarından herhangi biri için yenileme **isteğinde bulunan yenileme eylemini seçin** . Ardından istek için bir neden belirtin. Kaynak yöneticisinin onaylama veya reddetme kararı vermesini sağlayan ek bağlama ek olarak bir süre sağlamak yararlı olur.

![Neden kutusunu gösteren rol atama bölmesini Yenile](media/pim-resource-roles-renew-extend/aadpim-rbac-renew-request-form.png)

İstek gönderildikten sonra, kaynak yöneticilerine bir rol atamasını yenilemeye yönelik bekleyen bir istek bildirilir.

### <a name="admin-approves"></a>Yönetici onaylar

Kaynak yöneticileri, e-posta bildiriminde bulunan bağlantıdan veya Azure portal PıM 'ye erişerek ve sol bölmedeki **Istekleri Onayla** ' yı seçerek, yenileme isteğine erişebilir.

![Azure kaynakları-istekleri onaylama sayfası istekleri ve onaylama veya reddetme bağlantıları](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-approve-grid.png)

Yönetici **Onayla** veya **Reddet**seçeneğini belirlediğinde, istek ayrıntıları denetim günlükleri için gerekçe sağlamak üzere bir alanla birlikte gösterilir.

![İstek sahibi nedeni, atama türü, başlangıç zamanı, bitiş zamanı ve nedenle rol atama isteğini onaylayın](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-approve-blade.png)

Rol atamasını yenileme isteği onaylanırken, kaynak yöneticilerinin yeni bir başlangıç tarihi, bitiş tarihi ve atama türü girmesi gerekir. 

### <a name="admin-renew"></a>Yöneticiyi yenile

Kaynak yöneticileri, bir kaynağın sol gezinti menüsündeki **Üyeler** sekmesinden süre dolma rol atamalarını yenileyebilirler. Ayrıca, bir kaynak rolünün **süresi geçen** roller sekmesinden süresi geçen rol atamalarını yenileyebilirler.

Tüm süre dolma rolü atamalarının listesini görüntülemek için, **Üyeler** ekranında, **son kullanma tarihi olan roller**' i seçin.

![Azure kaynakları-roller sayfası, yenileme bağlantıları olan vadesi geçen rolleri listeleme](media/pim-resource-roles-renew-extend/aadpim-rbac-renew-from-member-blade.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PıM 'de Azure Kaynak rolleri için istekleri onaylama veya reddetme](pim-resource-roles-approval-workflow.md)
- [PıM 'de Azure Kaynak rolü ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
