---
title: PIM'de Azure kaynak rol atamalarını yenileme - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) Azure kaynak rol atamalarını nasıl genişletecek veya yenileyiniz öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 577c028582bc3b23d13e71522bb83db558065ee2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74022907"
---
# <a name="extend-or-renew-azure-resource-role-assignments-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rol atamalarını genişletme veya yenileme

Azure Etkin Dizin (Azure AD) Ayrıcalıklı Kimlik Yönetimi (PIM), Azure kaynaklarının erişim ve atama yaşam döngüsünü yönetmek için denetimler sağlar. Yöneticiler, başlangıç ve bitiş tarihi özelliklerini kullanarak roller atayabilir. Atama sonu yaklaştığında, Ayrıcalıklı Kimlik Yönetimi etkilenen kullanıcılara veya gruplara e-posta bildirimleri gönderir. Ayrıca, uygun erişimin korunmasını sağlamak için kaynağın yöneticilerine e-posta bildirimleri gönderir. Atamalar yenilenebilir ve erişim uzatılmasa bile süresi dolmuş durumda 30 güne kadar görünür kalabilir.

## <a name="who-can-extend-and-renew"></a>Kimler genişletebilir ve yenileyebilir?

Yalnızca kaynağın yöneticileri rol atamalarını genişletebilir veya yenileyebilir. Etkilenen kullanıcı veya grup, süresi dolmak üzere olan rolleri genişletmeyi ve süresi dolmuş rolleri yenilemeyi isteyebilir.

## <a name="when-are-notifications-sent"></a>Bildirimler ne zaman gönderilir?

Ayrıcalıklı Kimlik Yönetimi, yöneticilere ve etkilenen kullanıcıya veya görev süresi nin bitiminden bir gün önce 14 gün içinde sona eren rol gruplarına e-posta bildirimleri gönderir. Atama nın süresi resmi olarak sona erdiğinde ek bir e-posta gönderir.

Yöneticiler, bir kullanıcı veya grup, süresi dolan veya süresi dolan bir rol isteklerini genişletmeveya yenileme isteği verdiğinde bildirimler alır. Belirli bir yönetici isteği çözdüğünde, diğer tüm yöneticiler karar kararı ndan haberdar edilir (onaylanır veya reddedilir). Daha sonra talep eden kullanıcı ya da grup karar bildirilir.

## <a name="extend-role-assignments"></a>Rol atamalarını genişletme

Aşağıdaki adımlar, bir rol atamasının genişletilmesini veya yenilenmesini istemek, çözmek veya yönetmek için süreci ana hatlar.

### <a name="self-extend-expiring-assignments"></a>Süresi dolan atamaları kendi kendine uza

Bir role atanan kullanıcılar veya gruplar, süresi dolan rol atamalarını doğrudan kaynağın **Rollerim** sayfasındaki **Uygun** veya **Etkin** sekmesinden ve üst düzey Ayrıcalıklı Kimlik Yönetimi portalının **rollerim** sayfasından genişletebilir. Kullanıcılar veya gruplar, önümüzdeki 14 gün içinde süresi dolan uygun ve etkin (atanmış) rolleri genişletmeyi isteyebilir.

![Azure kaynakları - Bir Eylem sütunuyla uygun rolleri listeleyen rollerim sayfası](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-ui.png)

Atama bitiş tarihi 14 gün içinde olduğunda, **Genişletme** düğmesi kullanıcı arabiriminde etkin bir bağlantı haline gelir. Aşağıdaki örnekte, geçerli tarihin 27 Mart olduğunu varsayalım.

![Etkinleştirme veya Genişletme bağlantıları içeren eylem sütunu](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-within-14.png)

Bu rol atamasının uzatılmasını istemek için, istek formunu açmak için **Genişlet'i** seçin.

![Neden kutusuyla rol atama bölmesi genişletme](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-role-assignment-request.png)

Özgün atama yla ilgili bilgileri görüntülemek için **Atama ayrıntılarını**genişletin. Uzantı isteği için bir neden girin ve sonra **Genişlet'i**seçin.

>[!NOTE]
>Uzantının neden gerekli olduğu ve uzantının ne kadar süreyle verilmesi gerektiği (bu bilgilere sahipseniz) ayrıntılarını da dahil etmenizi öneririz.

![Atama ayrıntıları genişletilmiş olan rol atama bölmesi genişletme](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-form-complete.png)

Birkaç dakika içinde, kaynak yöneticileri uzantı isteğini gözden geçirmelerini isteyen bir e-posta bildirimi alır. Genişletme isteği zaten gönderildiyse, portalda bir Azure bildirimi görüntülenir.

![Zaten bekleyen bir rol atama uzantısı olduğunu açıklayan bildirim](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-failed-existing-request.png)

İsteğinizin durumunu görüntülemek veya iptal etmek için **Bekleyen istekler** sayfasına gidin.

![Azure kaynakları - Bekleyen istekler sayfası, bekleyen istekler sayfasını listeler ve İptal bağlantısı](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-cancel-request.png)

### <a name="admin-approved-extension"></a>Yönetici onaylı uzantısı

Bir kullanıcı veya grup rol atamasını uzatmak için bir istek gönderdiğinde, kaynak yöneticileri özgün atamanın ayrıntılarını ve isteğin nedenini içeren bir e-posta bildirimi alır. Bildirim, yöneticinin onaylaması veya reddetmesi isteğine doğrudan bir bağlantı içerir.

Yöneticiler, e-postadaki bağlantıyı izlemenin yanı sıra, Ayrıcalıklı Kimlik Yönetimi yönetim portalına giderek ve sol bölmedeki **Onay isteklerini** seçerek istekleri onaylayabilir veya reddedebilir.

![Azure kaynakları - Onaylamak veya reddetmek için istekleri sayfa listeleme isteklerini ve bağlantılarını onaylayın](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-approve-grid.png)

Yönetici Onayla veya **Reddet'i**seçtiğinde, denetim günlükleri için bir iş gerekçesi sağlamak için bir alanla birlikte isteğin ayrıntıları gösterilir. **Approve**

![İstek nedeni, atama türü, başlangıç zamanı, bitiş saati ve neden ile rol atama isteğini onaylama](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-approve-blade.png)

Rol atamasını genişletmek için bir isteği onaylarken, kaynak yöneticileri yeni bir başlangıç tarihi, bitiş tarihi ve atama türü seçebilir. Yönetici belirli bir görevi tamamlamak için sınırlı erişim sağlamak istiyorsa atama türünü değiştirmek gerekebilir (örneğin, bir gün). Bu örnekte, yönetici atamayı **Uygun'dan** **Etkin'e**değiştirebilir. Bu, etkinleştirmelerine gerek kalmadan istekte bulunabilecekleri anlamına gelir.

### <a name="admin-initiated-extension"></a>Yönetici başlatılan uzantısı

Bir role atanan bir kullanıcı rol ataması için uzantı talep etmezse, yönetici atamayı kullanıcı adına genişletebilir. Rol atamasının yönetim uzantıları onay gerektirmez, ancak bildirimler, görev süresi uzatıldıktan sonra diğer tüm yöneticilere gönderilir.

Rol atamasını genişletmek için, Ayrıcalıklı Kimlik Yönetimi'nde kaynak rolüne veya atama görünümüne göz atın. Uzantı gerektiren atamayı bulun. Ardından eylem sütununda **Genişlet'i** seçin.

![Azure kaynakları - uzanacak bağlantılarla uygun rolleri listeleyen atamalar sayfası](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-extend.png)

## <a name="renew-role-assignments"></a>Rol atamalarını yenileme

Kavramsal olarak bir uzantı isteme işlemine benzer olsa da, süresi dolmuş bir rol atamasını yenileme işlemi farklıdır. Aşağıdaki adımları kullanarak, atamalar ve yöneticiler gerektiğinde süresi dolmuş rollere erişimi yenileyebilir.

### <a name="self-renew"></a>Kendini yenileme

Kaynaklara artık erişemeyen kullanıcılar, süresi 30 güne kadar süresi dolmuş atama geçmişine erişebilir. Bunu yapmak için, sol bölmedeki **Rollerim'e** göz atarlar ve ardından Azure kaynak rolleri bölümünde **Süresi Dolmuş roller** sekmesini seçerler.

![Roller sayfam - Süresi dolmuş roller sekmesi](media/pim-resource-roles-renew-extend/aadpim-rbac-renew-from-myroles.png)

**Uygun rolleriçin**varsayılan olarak gösterilen rollerin listesi. Uygun ve Etkin atanmış roller arasında geçiş yapmak için açılır menüyü kullanın.

Listedeki rol atamalarından herhangi biri için yenileme isteğinde bulunmak için, **Eylemi Yenile'yi** seçin. Sonra istek için bir neden sağlayın. Kaynak yöneticisinin onaylamaya veya reddetmeye karar vermesine yardımcı olabilecek herhangi bir ek bağlam veya iş gerekçesine ek olarak bir süre sağlamak yararlıdır.

![Neden kutusunu gösteren rol atama bölmelerini yenileme](media/pim-resource-roles-renew-extend/aadpim-rbac-renew-request-form.png)

İstek gönderildikten sonra, kaynak yöneticilerine rol atamasını yenilemek için bekleyen bir istek bildirilir.

### <a name="admin-approves"></a>Yönetici onaylar

Kaynak yöneticileri, yenileme isteğine e-posta bildirimindeki bağlantıdan veya Azure portalından Ayrıcalıklı Kimlik Yönetimi'ne erişerek ve sol bölmeden **Onay isteklerini** seçerek erişebilir.

![Azure kaynakları - Onaylamak veya reddetmek için istekleri sayfa listeleme isteklerini ve bağlantılarını onaylayın](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-approve-grid.png)

Bir yönetici Onayla veya **Reddet'i** seçtiğinde, denetim günlükleri için bir iş gerekçesi sağlamak için isteğin ayrıntıları bir alanla birlikte gösterilir. **Deny**

![İstek nedeni, atama türü, başlangıç zamanı, bitiş saati ve neden ile rol atama isteğini onaylama](media/pim-resource-roles-renew-extend/aadpim-rbac-extend-admin-approve-blade.png)

Rol atamasını yenileme isteğini onaylarken, kaynak yöneticilerinin yeni bir başlangıç tarihi, bitiş tarihi ve atama türü girmeleri gerekir.

### <a name="admin-renew"></a>Yönetici yenileme

Kaynak yöneticileri, kaynağın sol gezinti menüsündeki **Üyeler** sekmesinden süresi dolmuş rol atamalarını yenileyebilir. Ayrıca, bir kaynak rolünün **Süresi Dolmuş** rolleri sekmesi içinden süresi dolmuş rol atamalarını da yenileyebilirler.

Süresi dolan tüm rol atamalarının listesini görüntülemek için **Üyeler** ekranında **Süresi Dolmuş rolleri**seçin.

![Azure kaynakları - Üyeler sayfası, yenileme bağlantılarıyla süresi dolmuş rolleri listele](media/pim-resource-roles-renew-extend/aadpim-rbac-renew-from-member-blade.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rolleri için istekleri onaylama veya reddetme](pim-resource-roles-approval-workflow.md)
- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
