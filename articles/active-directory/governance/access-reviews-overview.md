---
title: Erişim gözden geçirmeleri nedir? -Azure Active Directory | Microsoft Docs
description: Azure Active Directory erişim gözden geçirmeleri kullanarak, kuruluşunuzdaki idare, risk yönetimi ve uyumluluk girişimlerini karşılamak için Grup üyeliği ve uygulama erişimini denetleyebilirsiniz.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.subservice: compliance
ms.date: 07/22/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: ec88d795d0e05c62f07ff415364ced651ad8f4bc
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87034626"
---
# <a name="what-are-azure-ad-access-reviews"></a>Azure AD erişim gözden geçirmesi nedir?

Azure Active Directory (Azure AD) erişim gözden geçirmeleri, kuruluşların grup üyeliklerini verimli bir şekilde yönetmesine, kurumsal uygulamalara erişime ve rol atamalarına olanak tanır. Kullanıcının erişimi, yalnızca doğru kişilerin erişmeye devam ettiğinden emin olmak için düzenli olarak incelenebilir.

Erişim incelemelerine hızlı bir genel bakış sağlayan bir video aşağıda verilmiştir:

>[!VIDEO https://www.youtube.com/embed/kDRjQQ22Wkk]

## <a name="why-are-access-reviews-important"></a>Erişim gözden geçirmeleri neden önemlidir?

Azure AD, kuruluşunuzun içinde ve iş ortakları gibi harici kuruluşlardaki kullanıcılarla işbirliği yapmanızı sağlar. Kullanıcılar gruplara katılabilir, konukları davet edebilir, bulut uygulamalarına bağlanabilir ve kendi iş veya kişisel cihazlarından uzaktan çalışabilir. Self Servis 'nin gücünden yararlanma kolaylığı, daha iyi erişim yönetimi özelliklerine ihtiyaç duymasına yol açmıştır.

- Yeni çalışanlar katılırsanız, doğru erişimi sağlamak için bunların üretken olmasını nasıl sağlayabilirsiniz?
- İnsanlar takımları taşıdıkça veya şirketten ayrıldığında, özellikle de konukları içeriyorsa eski erişiminin kaldırılmasına nasıl emin olabilirsiniz?
- Aşırı erişim hakları, erişim üzerinde denetim olmaması durumunda bulmalar ve bulguları denetlemeye yol açabilir.
- Kaynaklarına kimlerin erişimi olduğunu düzenli olarak gözden geçirdiklerinden emin olmak için kaynak sahipleriyle önceden önlem almanız gerekir.

## <a name="when-to-use-access-reviews"></a>Erişim gözden geçirmeleri ne zaman kullanılır?

- **Ayrıcalıklı rollerde çok fazla sayıda Kullanıcı:** Kaç kullanıcının yönetim erişimi olduğunu, bunların kaç tane genel yönetici olduğunu ve bir yönetim görevi için atandıktan sonra kaldırılmayan davet konukları veya iş ortakları olduğunu kontrol etmek iyi bir fikirdir. Genel Yöneticiler gibi [Azure AD rollerinde](../privileged-identity-management/pim-how-to-perform-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) rol atama kullanıcılarını veya [Azure AD Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md) deneyiminde Kullanıcı erişimi Yöneticisi gibi [Azure kaynakları rollerini](../privileged-identity-management/pim-resource-roles-perform-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) yeniden ayarlayabilirsiniz.
- **Otomasyon uygulanabilir olmadığında:** Güvenlik gruplarında veya Office 365 gruplarında dinamik üyelik için kurallar oluşturabilirsiniz, ancak HR verileri Azure AD 'de değilse veya kullanıcıların değiştirme işlerini eğitmek üzere gruptan ayrıldıktan sonra hala erişime ihtiyacı varsa ne olur? Daha sonra, hala erişime ihtiyacı olan kişilerin erişmeye devam ettiğinden emin olmak için bu grupta bir gözden geçirme oluşturabilirsiniz.
- **Yeni bir amaç için bir grup kullanıldığında:** Azure AD ile eşitlenecek bir grubunuz varsa veya satış ekibi grubundaki herkes için uygulama Salesforce 'ı etkinleştirmeyi planlıyorsanız, Grup sahibinden farklı bir risk içeriğinde kullanılan gruptan önce grup üyeliğini gözden geçirmesini istemek yararlı olacaktır.
- **İş açısından kritik veri erişimi:** belirli kaynaklar IÇIN, BT dışındaki kişilerin düzenli olarak oturumu açmasını ve denetim amacıyla erişmesi gereken nedenleri hakkında bir gerekçe vermesini istemek gerekebilir.
- **Bir ilkenin özel durum listesini korumak için:** İdeal bir dünyada, tüm kullanıcılar kuruluşunuzun kaynaklarına erişimi güvenli hale getirmek için erişim ilkelerini takip edecektir. Ancak bazen özel durumlar oluşturmanız gereken iş durumları vardır. BT Yöneticisi olarak, bu görevi yönetebilir, ilke özel durumlarının fazla ilerlemesinin önüne kaçabilir ve bu özel durumların düzenli olarak inceleneceğini prova ile Denetçiler sağlayabilirsiniz.
- **Grup sahiplerini, gruplarında hala Konuk olmaları gerektiğini onaylamasını isteyin:** Çalışan erişimi, bazı şirket içi ıAM ile otomatikleştirilebilir, ancak davet konukları konuklarıyla otomatik olabilir. Bir grup, iş açısından gizli içeriğe Konuklar erişimi veriyorsa, konukların hala erişim için meşru bir iş ihtiyacı olduğunu onaylamak için Grup sahibinin sorumluluğundadır.
- **Gözden geçirmeler düzenli aralıklarla Yinele:** Kullanıcılara haftalık, aylık, üç aylık veya yıllık olarak ayarlanan sıklıklara yönelik yinelenen erişim İncelemeleri ayarlayabilirsiniz ve her İnceleme başlangıcında gözden geçirenler bilgilendirilir. Gözden geçirenler kolay bir arabirimle ve akıllı önerilerin yardımıyla erişimi onaylayabilir veya reddedebilir.

## <a name="where-do-you-create-reviews"></a>İncelemeleri nerede oluşturursunuz?

Gözden geçirmek istediğinize bağlı olarak, Azure AD erişim gözden geçirmeleriyle, Azure AD kurumsal uygulamalarında (önizlemede) veya Azure AD PıM 'de erişim incelemenizi oluşturacaksınız.

| Kullanıcıların erişim hakları | Gözden geçirenler şu şekilde olabilir | İnceleme oluşturma tarihi | Gözden geçiren deneyim |
| --- | --- | --- | --- |
| Güvenlik grubu üyeleri</br>Office grubu üyeleri | Belirtilen gözden geçirenler</br>Grup sahipleri</br>Kendi kendini gözden geçirme | Azure AD erişim gözden geçirmeleri</br>Azure AD grupları | Erişim paneli |
| Bağlı bir uygulamaya atandı | Belirtilen gözden geçirenler</br>Kendi kendini gözden geçirme | Azure AD erişim gözden geçirmeleri</br>Azure AD kurumsal uygulamaları (önizlemede) | Erişim paneli |
| Azure AD rolü | Belirtilen gözden geçirenler</br>Kendi kendini gözden geçirme | [Azure AD PıM](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) | Azure portalı |
| Azure Kaynak rolü | Belirtilen gözden geçirenler</br>Kendi kendini gözden geçirme | [Azure AD PıM](../privileged-identity-management/pim-resource-roles-start-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) | Azure portalı |


## <a name="create-access-reviews"></a>Erişim incelemeleri oluşturma

Erişim İncelemeleri oluşturmak için aşağıdaki adımları izleyin:

1. Erişim incelemelerini yönetmek ve genel yönetici veya Kullanıcı Yöneticisi olarak oturum açmak için [Azure Portal](https://portal.azure.com) gidin.

1. **Azure Active Directory**'yi bulun ve seçin.

      ![Azure Active Directory arama Azure portal](media/access-reviews-overview/search-azure-active-directory.png)

1. **Identity idare**seçin.

1. Başlarken sayfasında, **erişim gözden geçirmesi oluştur** düğmesine tıklayın.

   ![Erişim İncelemeleri başlangıç sayfası](./media/access-reviews-overview/access-reviews-overview-create-access-reviews.png) 

### <a name="creating-access-review-on-a-group-that-can-be-assigned-to-azure-ad-role"></a>Azure AD rolüne atanabilecek bir grupta erişim incelemesi oluşturma
En yeni erişim gözden geçirmeleri sürümünüzde (gözden geçirenler varsayılan olarak **erişimime** yönlendirilir), bu durumda yalnızca genel yönetici rol atanabilir gruplar üzerinde erişim gözden geçirmesi oluşturabilir. Ancak, erişim gözden geçirmelerinden daha eski bir sürümünüzde (gözden geçirenler varsayılan olarak **erişim paneline** yönlendirilir), her Iki genel yönetici ve Kullanıcı Yöneticisi de rol atanabilir gruplar üzerinde erişim gözden geçirmesi oluşturabilir.  

Yeni deneyim, 1 Ağustos 2020 ' de tüm müşterilere alınacaktır, ancak daha önce yükseltmek istiyorsanız, lütfen buradan bir istek yapın; [erişim Onayumdaki Azure AD erişim İncelemeleri-güncelleştirilmiş gözden geçiren deneyimi](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR5dv-S62099HtxdeKIcgO-NUOFJaRDFDWUpHRk8zQ1BWVU1MMTcyQ1FFUi4u).

[Azure AD rollerine Grup atama hakkında daha fazla bilgi edinin](https://go.microsoft.com/fwlink/?linkid=2103037).

## <a name="learn-about-access-reviews"></a>Erişim incelemeleri hakkında bilgi edinin

Erişim gözden geçirmeleri oluşturma ve gerçekleştirme hakkında daha fazla bilgi edinmek için şu kısa tanıtımı izleyin:

>[!VIDEO https://www.youtube.com/embed/6KB3TZ8Wi40]

Kuruluşunuzda erişim gözden geçirmeleri dağıtmaya hazırsanız, bu adımları izleyerek, yöneticilerinizi eğitin, eğitmeniz ve ilk erişim incelemenizi oluşturmanız gerekir!

>[!VIDEO https://www.youtube.com/embed/X1SL2uubx9M]

## <a name="license-requirements"></a>Lisans gereksinimleri

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

### <a name="how-many-licenses-must-you-have"></a>Kaç lisansa sahip olmanız gerekir?

Aşağıdaki görevleri yerine getirmek istediğiniz çalışanlara sahip olduğunuz için, dizininizde en az sayıda Azure AD Premium P2 lisansa sahip olduğundan emin olun:

- Gözden geçirenler olarak atanan üye ve Konuk kullanıcılar
- Kendi kendine İnceleme gerçekleştiren üye ve Konuk kullanıcılar
- Erişim gözden geçirmesi gerçekleştiren Grup sahipleri
- Erişim gözden geçirmesi gerçekleştiren uygulama sahipleri

Aşağıdaki görevler için Azure AD Premium P2 lisansları gerekli **değildir** :

- Genel yönetici veya Kullanıcı Yöneticisi rollerine sahip kullanıcılar için erişim gözden geçirmeleri ayarlama, ayarları yapılandırma veya kararların kararlarını uygulama hakkında herhangi bir lisans gerekmez.

Kendi kuruluşunuzun kullanıcılarından birine atadığınız her ücretli Azure AD Premium P2 lisansı için, dış Kullanıcı tahsisatından beş adede kadar Konuk kullanıcıya davet etmek için Azure AD işletmeden işletmeye (B2B) kullanabilirsiniz. Bu Konuk kullanıcılar Azure AD Premium P2 özelliklerini de kullanabilir. Daha fazla bilgi için bkz. [Azure AD B2B işbirliği Lisanslama Kılavuzu](../b2b/licensing-guidance.md).

Lisanslar hakkında daha fazla bilgi için bkz. [Azure Active Directory portalını kullanarak lisans atama veya kaldırma](../fundamentals/license-users-groups.md).

### <a name="example-license-scenarios"></a>Örnek lisans senaryoları

Sahip olmanız gereken lisans sayısını belirlemenize yardımcı olacak bazı örnek lisans senaryoları aşağıda verilmiştir.

| Senaryo | Hesaplama | Lisans sayısı |
| --- | --- | --- |
| Yönetici, 75 Kullanıcı ve 1 Grup sahibi ile A grubu için bir erişim incelemesi oluşturur ve Grup sahibini gözden geçiren olarak atar. | 1 Grup sahibi için gözden geçiren olarak lisans | 1 |
| Yönetici, 500 Kullanıcı ve 3 Grup sahibi olan B grubunun erişim incelemesini oluşturur ve 3 Grup sahibini gözden geçirenler olarak atar. | Her grup sahibi için gözden geçirenler olarak 3 lisans | 3 |
| Yönetici, 500 Kullanıcıyla Grup B 'ye erişim incelemesi oluşturur. Kendi kendine İnceleme yapar. | Her Kullanıcı için kendi kendini gözden geçirenler olarak 500 lisans | 500 |
| Yönetici, 50 üye kullanıcıları ve 25 Konuk kullanıcıyla Grup C 'nin erişim incelemesini oluşturur. Kendi kendine İnceleme yapar. | Her Kullanıcı için kendi kendine gözden geçirenler olarak 50 lisans.<br/>(Konuk kullanıcılar gereken 1:5 oranına göre ele alınmıştır) | 50 |
| Yönetici, 6 üye kullanıcısı ve 108 Konuk kullanıcıyla Grup D 'ye yönelik bir erişim incelemesi oluşturur. Kendi kendine İnceleme yapar. | Her bir kullanıcı için kendi kendine gözden geçirenler olarak 6 lisans ve gerekli 1:5 oranında tüm 108 Konuk kullanıcıları kapsayacak ek lisanslar. 6 \* 5 = 30 Konuk kullanıcıyı kapsayan 6 lisans. Kalan (108-6 \* 5) = 78 Konuk kullanıcılar için 78/5 = 16 ek lisans gerekir. Bu nedenle Total, 6 + 16 = 22 lisans gerekir. | 22 |

## <a name="next-steps"></a>Sonraki adımlar

- [Gruplar veya uygulamalar için erişim gözden geçirmesi oluşturma](create-access-review.md)
- [Bir Azure AD yönetici rolündeki kullanıcılar için erişim gözden geçirmesi oluşturma](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)
- [Gruplara veya uygulamalara erişimi gözden geçirme](perform-access-review.md)
- [Grupların veya uygulamaların erişim incelemesini tamamlar](complete-access-review.md)
