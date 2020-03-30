---
title: Erişim incelemeleri ile konuk erişimini yönetme - Azure AD
description: Azure Active Directory erişim incelemeleri ile konuk kullanıcıları bir grubun üyesi veya bir uygulamaya atanmış olarak yönetme
services: active-directory
documentationcenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 12/13/2018
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8bf2f9503ae704110786a1e73aec3da18c17e4ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75932420"
---
# <a name="manage-guest-access-with-azure-ad-access-reviews"></a>Azure AD erişim gözden geçirmeleriyle konuk erişimini yönetme


Azure Etkin Dizin (Azure AD) ile [Azure AD B2B özelliğini](../b2b/what-is-b2b.md)kullanarak kuruluş sınırları arasında işbirliğini kolayca etkinleştirebilirsiniz. Diğer kiracılardan gelen konuk kullanıcılar yöneticiler veya [diğer kullanıcılar](../b2b/what-is-b2b.md) [tarafından davet](../b2b/add-users-administrator.md) edilebilir. Bu özellik, Microsoft hesapları gibi sosyal kimlikler için de geçerlidir.

Ayrıca, konuk kullanıcıların uygun erişime sahip olmasını da sağlayabilirsiniz. Konuklardan veya bir karar vericiden bir erişim incelemesine katılmalarını isteyebilir ve konukların erişimini yeniden onaylamalarını (veya doğrulaşmalarını) isteyebilirsiniz. Gözden geçirenler, Azure AD önerilerine dayanarak her kullanıcının erişiminin devam edip etmemesine yönelik girişler ekleyebilir. Erişim incelemesi tamamlandığında, değişiklikler yapabilir ve artık ihtiyacı olmayan misafirlerin erişimini kaldırabilirsiniz.

> [!NOTE]
> Bu belge, konuk kullanıcıların erişimini gözden geçirmeye odaklanır. Yalnızca misafirleri değil, tüm kullanıcıların erişimini gözden geçirmek istiyorsanız, [erişim yorumlarıyla kullanıcı erişimini yönet'e](manage-user-access-with-access-reviews.md)bakın. Kullanıcıların global yönetici gibi yönetimsel rollerdeki üyeliklerini gözden geçirmek istiyorsanız, Azure [AD Ayrıcalıklı Kimlik Yönetimi'nde bir erişim incelemesi başlat'a](../privileged-identity-management/pim-how-to-start-security-review.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

- Azure AD Premium P2

Daha fazla bilgi için, [Lisans gereksinimleri](access-reviews-overview.md#license-requirements).

## <a name="create-and-perform-an-access-review-for-guests"></a>Konuklar için bir erişim incelemesi oluşturma ve gerçekleştirme

İlk olarak, genel bir yönetici veya kullanıcı yöneticisi olarak, erişim incelemelerinizin kuruluşunuz için hazır olduğundan emin olmak için [Kimlik Yönetimi sayfasına](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/) gidin.

Azure AD, konuk kullanıcıları incelemek için çeşitli senaryolar sağlar.

Aşağıdakileri de gözden geçirebilirsiniz:

 - Azure AD'de bir veya daha fazla misafiri olan bir grup üye olarak.
 - Azure AD'ye bağlı ve kendisine bir veya daha fazla konuk kullanıcı atanmış bir uygulama. 

Daha sonra, her konuktan kendi erişimini gözden geçirmelerini mi yoksa bir veya daha fazla kullanıcıdan her konuğun erişimini gözden geçirmelerini isteyip istememeye karar verebilirsiniz.

 Bu senaryolar aşağıdaki bölümlerde ele alınmıştır.

### <a name="ask-guests-to-review-their-own-membership-in-a-group"></a>Konuklardan bir gruptaki kendi üyeliklerini gözden geçirmelerini isteyin

Bir gruba davet edilen ve gruba eklenen kullanıcıların erişime ihtiyaç duymaya devam etmesini sağlamak için erişim incelemelerini kullanabilirsiniz. Konuklardan bu gruptaki kendi üyeliklerini kolayca gözden geçirmelerini isteyebilirsiniz.

1. Grup için bir erişim incelemesi oluşturmak için, yalnızca konuk kullanıcı üyelerini ve üyelerin kendilerini gözden geçirmesini içerecek şekilde incelemeyi seçin. Daha fazla bilgi için [bkz.](create-access-review.md)

2. Her konuğdan kendi üyeliklerini gözden geçirmelerini isteyin. Varsayılan olarak, daveti kabul eden her konuk, Azure AD'den erişim incelemesine bağlantı içeren bir e-posta alır. Azure AD'de konuklara [gruplara veya uygulamalara erişimi nasıl gözden geçireceklerine](perform-access-review.md)ilişkin talimatlar vardır.

3. Gözden geçirenler bilgileri girdikten sonra erişim gözden geçirmesini durdurun ve değişiklikleri uygulayın. Daha fazla bilgi için bkz: [Grupların veya uygulamaların erişim incelemesini tamamla.](complete-access-review.md)

4. Sürekli erişim için kendi gereksinimini reddeden kullanıcılara ek olarak, yanıt vermeyen kullanıcıları da kaldırabilirsiniz. Yanıt vermeyen kullanıcılar artık e-posta alma potansiyeline sahiptir.

5. Grup erişim yönetimi için kullanılmadıysa, davetlerini kabul etmedikleri için incelemeye katılmak üzere seçilmemiş kullanıcıları da kaldırabilirsiniz. Kabul etmemek, davet edilen kullanıcının e-posta adresinin yazım hatası olduğunu gösterebilir. Bir grup dağıtım listesi olarak kullanılıyorsa, belki de bazı konuk kullanıcılar ilgili nesneler oldukları için katılmaları için seçilmemiş olabilir.

### <a name="ask-a-sponsor-to-review-a-guests-membership-in-a-group"></a>Bir sponsordan bir konuğun grup taki üyeliğini gözden geçirmesini isteyin

Bir grubun sahibi gibi bir sponsordan, bir konuğun bir gruba sürekli üyelik gereksinimini gözden geçirmesini isteyebilirsiniz.

1. Grup için bir erişim incelemesi oluşturmak için, yalnızca konuk kullanıcı üyelerini dahil etmek için incelemeyi seçin. Ardından bir veya daha fazla gözden geçiren belirtin. Daha fazla bilgi için [bkz.](create-access-review.md)

2. Gözden geçirenlerden bilgileri girmelerini isteyin. Varsayılan olarak, her biri Azure AD'den [gruplara veya uygulamalara erişimi gözden geçirdikleri](perform-access-review.md)erişim paneline bağlantı içeren bir e-posta alırlar.

3. Gözden geçirenler bilgileri girdikten sonra erişim gözden geçirmesini durdurun ve değişiklikleri uygulayın. Daha fazla bilgi için bkz: [Grupların veya uygulamaların erişim incelemesini tamamla.](complete-access-review.md)

### <a name="ask-guests-to-review-their-own-access-to-an-application"></a>Konuklardan bir uygulamaya kendi erişimlerini gözden geçirmelerini isteyin

Belirli bir uygulama için davet edilen kullanıcıların erişime ihtiyaç duymaya devam etmesini sağlamak için erişim incelemelerini kullanabilirsiniz. Konuklardan kendi erişim lerini gözden geçirmelerini kolayca isteyebilirsiniz.

1. Uygulama için bir erişim incelemesi oluşturmak için, yalnızca konukları dahil etmek ve kullanıcıların kendi erişimlerini gözden geçirmesi için incelemeyi seçin. Daha fazla bilgi için [bkz.](create-access-review.md)

2. Her konuğdan uygulamaya kendi erişimlerini gözden geçirmelerini isteyin. Varsayılan olarak, daveti kabul eden her konuk Azure AD'den bir e-posta alır. Bu e-postanın kuruluşunuzun erişim panelindeki erişim incelemesine bir bağlantısı vardır. Azure AD'de konuklara [gruplara veya uygulamalara erişimi nasıl gözden geçireceklerine](perform-access-review.md)ilişkin talimatlar vardır.

3. Gözden geçirenler bilgileri girdikten sonra erişim gözden geçirmesini durdurun ve değişiklikleri uygulayın. Daha fazla bilgi için bkz: [Grupların veya uygulamaların erişim incelemesini tamamla.](complete-access-review.md)

4. Sürekli erişim için kendi gereksinimini reddeden kullanıcılara ek olarak, yanıt vermeyen konuk kullanıcıları da kaldırabilirsiniz. Yanıt vermeyen kullanıcılar artık e-posta alma potansiyeline sahiptir. Ayrıca, özellikle yakın zamanda davet edilmedilerse, katılmak üzere seçilmemiş konuk kullanıcıları da kaldırabilirsiniz. Bu kullanıcılar davetlerini kabul etmediler ve bu nedenle uygulamaya erişemadılar. 

### <a name="ask-a-sponsor-to-review-a-guests-access-to-an-application"></a>Bir sponsordan bir konuğun başvuruya erişimini incelemesini isteyin

Bir başvurunun sahibi gibi bir sponsordan, konuğun uygulamaya sürekli erişim gereksinimini gözden geçirmesini isteyebilirsiniz.

1. Uygulama için bir erişim incelemesi oluşturmak için, yalnızca konukları dahil etmek için incelemeyi seçin. Ardından bir veya daha fazla kullanıcıyı gözden geçiren olarak belirtin. Daha fazla bilgi için [bkz.](create-access-review.md)

2. Gözden geçirenlerden bilgileri girmelerini isteyin. Varsayılan olarak, her biri Azure AD'den [gruplara veya uygulamalara erişimi gözden geçirdikleri](perform-access-review.md)erişim paneline bağlantı içeren bir e-posta alırlar.

3. Gözden geçirenler bilgileri girdikten sonra erişim gözden geçirmesini durdurun ve değişiklikleri uygulayın. Daha fazla bilgi için bkz: [Grupların veya uygulamaların erişim incelemesini tamamla.](complete-access-review.md)

### <a name="ask-guests-to-review-their-need-for-access-in-general"></a>Konuklardan genel olarak erişim gereklerini gözden geçirmelerini isteyin

Bazı kuruluşlarda, konuklar grup üyeliklerinden haberdar olmayabilir.

> [!NOTE]
> Azure portalının önceki sürümleri, Kullanıcı Kullanıcı Türü'ne sahip kullanıcılar tarafından yönetimerişimine izin vermemiş. Bazı durumlarda, dizininizdeki bir yönetici PowerShell'i kullanarak bir konuğun UserType değerini Üye olarak değiştirmiş olabilir. Bu değişiklik daha önce dizininizde oluştuysa, önceki sorgu, yönetimsel erişim haklarına sahip olan tüm konuk kullanıcıları içermeyebilir. Bu durumda, konuğun UserType'ını değiştirmeniz veya konuğu grup üyeliğine el ile eklemeniz gerekir.

1. Uygun bir grup zaten yoksa, Azure AD'de konuklarla birlikte üye olarak bir güvenlik grubu oluşturun. Örneğin, el ile korunan bir konuk üyeliğine sahip bir grup oluşturabilirsiniz. Veya, Contoso kiracısında Konuk Kullanıcı Türü öznitelik değerine sahip kullanıcılar için "Contoso'nun Konukları" gibi bir ada sahip dinamik bir grup oluşturabilirsiniz.  Verimlilik için, grubun ağırlıklı olarak konuk olduğundan emin olun - üye kullanıcıların gözden geçirilmesi gerekmediğinden üye kullanıcılara sahip bir grubu seçmeyin.  Ayrıca, grubun bir üyesi olan konuk kullanıcının grubun diğer üyelerini görebileceğini unutmayın.

2. Bu grup için bir erişim incelemesi oluşturmak için, gözden geçirenlerin üyelerin kendileri olmasını seçin. Daha fazla bilgi için [bkz.](create-access-review.md)

3. Her konuğdan kendi üyeliklerini gözden geçirmelerini isteyin. Varsayılan olarak, daveti kabul eden her konuk, Azure AD'den kuruluşunuzun erişim panelindeki erişim incelemesine bağlantı içeren bir e-posta alır. Azure AD'de konuklara [gruplara veya uygulamalara erişimi nasıl gözden geçireceklerine](perform-access-review.md)ilişkin talimatlar vardır.  Davetlerini kabul etmeyen konuklar inceleme sonuçlarında "Bildirilmeyecek" olarak görünecektir.

4. Gözden geçirenler giriş yaptıktan sonra erişim incelemesini durdurun. Daha fazla bilgi için bkz: [Grupların veya uygulamaların erişim incelemesini tamamla.](complete-access-review.md)

5. Reddedilen, incelemeyi tamamlamayan veya davetlerini daha önce kabul etmeyen konuklar için konuk erişimini kaldırın. Konuklardan bazıları incelemeye katılmak üzere seçilen kişilerse veya daha önce bir daveti kabul etmedilerse, Azure portalını veya PowerShell'i kullanarak hesaplarını devre dışı kullanabilirsiniz. Konuğun artık erişime ihtiyacı yoksa ve ilgili kişi değilse, konuk kullanıcı nesnesini silmek için Azure portalını veya PowerShell'i kullanarak kullanıcı nesnesini dizininizden kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Grupların veya uygulamaların erişim gözden geçirmesini oluşturma](create-access-review.md)







