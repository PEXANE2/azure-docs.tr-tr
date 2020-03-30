---
title: Erişim gözden geçirmeleri nedir? - Azure Etkin Dizin | Microsoft Dokümanlar
description: Azure Active Directory erişim yorumlarını kullanarak, kuruluşunuzdaki yönetim, risk yönetimi ve uyumluluk girişimlerini karşılamak için grup üyeliğini ve uygulama erişimini denetleyebilirsiniz.
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
ms.date: 01/10/2020
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5499c8808c3916842071df1f03a865efd98719f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262093"
---
# <a name="what-are-azure-ad-access-reviews"></a>Azure AD erişim gözden geçirmesi nedir?

Azure Etkin Dizin (Azure AD) erişim incelemeleri, kuruluşların grup üyeliklerini, kurumsal uygulamalara erişimi ve rol atamalarını verimli bir şekilde yönetmesini sağlar. Yalnızca doğru kişilerin sürekli erişime sahip olduğundan emin olmak için kullanıcının erişimi düzenli olarak gözden geçirilebilir.

Erişim değerlendirmelerine hızlı bir genel bakış sağlayan bir video aşağıda veda edebilirsiniz:

>[!VIDEO https://www.youtube.com/embed/kDRjQQ22Wkk]

## <a name="why-are-access-reviews-important"></a>Erişim değerlendirmeleri neden önemlidir?

Azure AD, kuruluşunuz içinde ve iş ortakları gibi harici kuruluşlardan gelen kullanıcılarla dahili olarak işbirliği yapmanızı sağlar. Kullanıcılar gruplara katılabilir, konukları davet edebilir, bulut uygulamalarına bağlanabilir ve işlerinden veya kişisel aygıtlarından uzaktan çalışabilir. Self servisin gücünden yararlanmanın kolaylığı, daha iyi erişim yönetimi yeteneklerine ihtiyaç duyulmasına yol açmıştır.

- Yeni çalışanlar katıldıkça, üretken olmak için doğru erişime sahip olmalarını nasıl sağlarsınız?
- İnsanlar takımları hareket ettirirken veya şirketten ayrıldıkça, özellikle de misafirleri kapsıyorsa, eski erişimlerinin kaldırılmasını nasıl sağlıyorsunuz?
- Aşırı erişim hakları, erişim üzerinde kontrol eksikliğini gösterdiğinden, denetim bulgularına ve uzlaşmalara yol açabilir.
- Kaynaklarının kimlere erişimi olduğunu düzenli olarak gözden geçirmelerini sağlamak için kaynak sahipleriyle proaktif olarak etkileşimde bulunmanız gerekir.

## <a name="when-to-use-access-reviews"></a>Erişim yorumları ne zaman kullanılır?

- **Ayrıcalıklı rollerde çok fazla kullanıcı:** Kaç kullanıcının yönetim erişimi olduğunu, kaç tanesinin Global Yöneticiler olduğunu ve yönetim görevi için atandıktan sonra kaldırılmayan davetli veya iş ortakları olup olmadığını denetlemek iyi bir fikirdir. Genel Yöneticiler gibi [Azure AD rollerindeki](../privileged-identity-management/pim-how-to-perform-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) rol atama kullanıcılarını veya Azure AD Ayrıcalıklı Kimlik [Yönetimi (PIM)](../privileged-identity-management/pim-configure.md) deneyiminde Kullanıcı Erişimi Yöneticisi gibi [Azure kaynakları rollerini](../privileged-identity-management/pim-resource-roles-perform-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) düzeltebilirsiniz.
- **Otomasyon mümkün olmadığında:** Güvenlik gruplarında veya Office 365 Gruplarında dinamik üyelik için kurallar oluşturabilirsiniz, ancak İk verileri Azure AD'de değilse veya kullanıcılar değiştirmelerini eğitmek için gruptan ayrıldıktan sonra hala erişime ihtiyaç duyuyorsa ne olur? Daha sonra, hala erişime ihtiyaç duyankişilerin sürekli erişime sahip olması gerektiğinden emin olmak için bu grup üzerinde bir inceleme oluşturabilirsiniz.
- **Bir grup yeni bir amaç için kullanıldığında:** Azure AD ile senkronize edilecek bir grubunuz varsa veya Satış ekibi grubundaki herkes için uygulama Salesforce'u etkinleştirmeyi planlıyorsanız, grup sahibinden grup üyeliğini farklı bir risk içeriğinde kullanılmadan önce gözden geçirmesini istemek yararlı olacaktır.
- **İş açısından kritik veri erişimi:** Belirli kaynaklar için BT dışındaki kişilerden düzenli olarak oturum açmalarını ve denetim amaçları için neden erişime ihtiyaç duyduklarına dair bir gerekçe vermelerini istemek gerekebilir.
- **Bir ilkenin özel durum listesini korumak için:** İdeal bir dünyada, kuruluşunuzun kaynaklarına erişimi güvence altına almak için tüm kullanıcılar erişim ilkelerini izler. Ancak, bazen özel durumlar yapmanızı gerektiren iş durumları vardır. BT yöneticisi olarak, bu görevi yönetebilir, ilke özel durumlarının denetlenmesini önleyebilir ve denetçilere bu özel durumların düzenli olarak gözden geçirildiklerine dair kanıt sağlayabilirsiniz.
- **Grup sahiplerinden gruplarında hala konuklara ihtiyaç duyduklarından haber** vermelerini isteyin: Çalışan erişimi, şirket içi bazı IAM ile otomatikleştirilmiş olabilir, ancak davetli misafirlere davet edilmeyebilir. Bir grup, konuklara işletmeye duyarlı içeriğe erişim izni veriyorsa, konukların yasal bir iş erişimi ne rendesi olduğunu onaylamak grup sahibinin sorumluluğundadır.
- **İncelemeler düzenli olarak yinelesin:** Haftalık, aylık, üç aylık veya yıllık gibi belirli frekanslarda kullanıcıların yinelenen erişim yorumlarını ayarlayabilirsiniz ve gözden geçirenler her incelemenin başında bildirilir. Gözden geçirenler, kolay bir arayüzle ve akıllı öneriler yardımıyla erişimi onaylayabilir veya reddedebilir.

## <a name="where-do-you-create-reviews"></a>Yorumları nerede oluşturuyorsunuz?

İncelemek istediğiniz şeye bağlı olarak, erişim incelemenizi Azure AD erişim incelemelerinde, Azure AD kurumsal uygulamalarında (önizlemede) veya Azure AD PIM'de oluşturursunuz.

| Kullanıcıların erişim hakları | Gözden geçirenler olabilir | Oluşturulan gözden geçirme | Gözden geçiren deneyim |
| --- | --- | --- | --- |
| Güvenlik grubu üyeleri</br>Ofis grubu üyeleri | Belirtilen gözden geçirenler</br>Grup sahipleri</br>Kendi kendine inceleme | Azure AD erişim gözden geçirmeleri</br>Azure AD grupları | Erişim paneli |
| Bağlı bir uygulamaya atanmış | Belirtilen gözden geçirenler</br>Kendi kendine inceleme | Azure AD erişim gözden geçirmeleri</br>Azure AD kurumsal uygulamaları (önizlemede) | Erişim paneli |
| Azure AD rolü | Belirtilen gözden geçirenler</br>Kendi kendine inceleme | [Azure AD PIM](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) | Azure portalında |
| Azure kaynak rolü | Belirtilen gözden geçirenler</br>Kendi kendine inceleme | [Azure AD PIM](../privileged-identity-management/pim-resource-roles-start-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) | Azure portalında |


## <a name="create-access-reviews"></a>Erişim incelemeleri oluşturma

Erişim incelemeleri oluşturmak için aşağıdaki adımları izleyin:

1. Erişim incelemelerini yönetmek için [Azure portalına](https://portal.azure.com) gidin ve Global yönetici veya Kullanıcı yöneticisi olarak oturum açın.

1. **Azure Active Directory**'yi bulun ve seçin.

      ![Azure Active Directory için Azure portalı araması](media/access-reviews-overview/search-azure-active-directory.png)

1. **Kimlik Yönetişimini**seçin.

1. Başlangıç sayfasında, **erişim incelemesi oluştur** düğmesini tıklatın.

   ![Erişim incelemeleri başlangıç sayfası](./media/access-reviews-overview/access-reviews-overview-create-access-reviews.png) 


## <a name="learn-about-access-reviews"></a>Erişim değerlendirmeleri hakkında bilgi edinin

Erişim incelemeleri oluşturma ve gerçekleştirme hakkında daha fazla bilgi edinmek için şu kısa demoya göz kulak oylanın:

>[!VIDEO https://www.youtube.com/embed/6KB3TZ8Wi40]

Kuruluşunuzdaki erişim yorumlarını dağıtmaya hazırsanız, videoda bulunan bu adımları takip edin ve dahili olarak yönetenizi eğitin ve ilk erişim incelemenizi oluşturun!

>[!VIDEO https://www.youtube.com/embed/X1SL2uubx9M]

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

### <a name="how-many-licenses-must-you-have"></a>Kaç lisansın olmalı?

Dizininizin, aşağıdaki görevleri yerine getirecek çalışanlarınız kadar en az sayıda Azure AD Premium P2 lisansına sahip olduğundan emin olun:

- Gözden geçiren olarak atanan üye ve konuk kullanıcılar
- Kendi kendini gözden geçiren üye ve konuk kullanıcılar
- Erişim incelemesi gerçekleştiren grup sahipleri
- Erişim incelemesi gerçekleştiren uygulama sahipleri

Azure AD Premium P2 lisansları aşağıdaki görevler için gerekli **değildir:**

- Erişim incelemelerini ayarlayan, ayarları yapılandıran veya incelemelerden alınan kararları uygulayan Global Administrator veya User Administrator rollerine sahip kullanıcılar için lisans gerekmez.

Kendi kuruluşunuzun kullanıcılarından birine atadığınız her ücretli Azure AD Premium P2 lisansı için, Dış Kullanıcı İzni kapsamında en fazla beş konuk kullanıcıyı davet etmek için Azure AD işletmeden işletmeye (B2B) kullanabilirsiniz. Bu konuk kullanıcılar Azure AD Premium P2 özelliklerini de kullanabilir. Daha fazla bilgi için Azure [AD B2B işbirliği lisanslama kılavuzuna](../b2b/licensing-guidance.md)bakın.

Lisanslar hakkında daha fazla bilgi için Azure [Etkin Dizin portalını kullanarak lisans atayın veya kaldırın'a](../fundamentals/license-users-groups.md)bakın.

### <a name="example-license-scenarios"></a>Örnek lisans senaryoları

Burada, sahip olduğunuz lisans sayısını belirlemenize yardımcı olacak bazı örnek lisans senaryoları verilmiştir.

| Senaryo | Hesaplama | Lisans sayısı |
| --- | --- | --- |
| Bir yönetici, 75 kullanıcı ve 1 grup sahibiyle A Grubu'nun bir erişim incelemesini oluşturur ve grup sahibini gözden geçiren olarak atar. | Gözden geçiren olarak grup sahibi için 1 lisans | 1 |
| Bir yönetici, 500 kullanıcı ve 3 grup sahibiyle B Grubu'nun bir erişim incelemesini oluşturur ve 3 grup sahibini gözden geçiren olarak atar. | Gözden geçiren olarak her grup sahibi için 3 lisans | 3 |
| Yönetici, 500 kullanıcıyla B Grubu'nun bir erişim incelemesini oluşturur. Bunu kendi kendini gözden geçirmeye itiyor. | Kendi kendini gözden geçiren her kullanıcı için 500 lisans | 500 |
| Bir yönetici, 50 üye kullanıcı ve 25 konuk kullanıcıyla C Grubu'nun bir erişim incelemesini oluşturur. Bunu kendi kendini gözden geçirmeye itiyor. | Kendi kendini gözden geçiren her kullanıcı için 50 lisans.<br/>(konuk kullanıcılar gerekli 1:5 oranı kapsamındadır) | 50 |
| Bir yönetici, 6 üye kullanıcı ve 108 konuk kullanıcıyla D Grubu'nun bir erişim incelemesini oluşturur. Bunu kendi kendini gözden geçirmeye itiyor. | Her kullanıcı için kendi kendini gözden geçiren olarak 6 lisans + 16 ek lisans gerekli 1:5 oranında tüm 108 konuk kullanıcıları kapsayacak şekilde. 6 5=30\*konuk kullanıcıyı kapsayan 6 lisans. Geri kalan (108-6\*5)=78 konuk kullanıcı için 78/5=16 ek lisans gereklidir. Bu nedenle toplamda 6+16=22 lisans gereklidir. | 22 |

## <a name="next-steps"></a>Sonraki adımlar

- [Grupların veya uygulamaların erişim gözden geçirmesini oluşturma](create-access-review.md)
- [Bir Azure AD yönetici rolündeki kullanıcılar için erişim gözden geçirmesi oluşturma](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)
- [Gruplara veya uygulamalara erişimi gözden geçirme](perform-access-review.md)
- [Grupların veya uygulamaların erişim gözden geçirmesini tamamlama](complete-access-review.md)
