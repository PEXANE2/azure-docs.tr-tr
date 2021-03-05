---
title: Gruplar & uygulamalar için erişim gözden geçirmesini tamamlar-Azure AD
description: Azure Active Directory erişim gözden geçirmeleriyle grup üyelerinin veya uygulama erişiminin erişim incelemesini tamamlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: ajburnle
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 02/08/2021
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: e31b9ce115ebe0723a9d082aaddd8056e486ed27
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102176986"
---
# <a name="complete-an-access-review-of-groups-and-applications-in-azure-ad-access-reviews"></a>Azure AD erişim gözden geçirmeleri içindeki gruplar ve uygulamalar için bir erişim incelemesi tamamlamayı doldurun

Yönetici olarak, [gruplar veya uygulamalar için erişim gözden geçirmesi oluşturursunuz](create-access-review.md) ve gözden geçirenler [erişim incelemesini gerçekleştirir](perform-access-review.md). Bu makalede, erişim incelemesi sonuçlarının nasıl görüntüleneceği ve sonuçların nasıl uygulanacağı açıklanır.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="prerequisites"></a>Önkoşullar

- Azure AD Premium P2
- Genel yönetici, Kullanıcı Yöneticisi, güvenlik yöneticisi veya güvenlik okuyucusu

Daha fazla bilgi için bkz. [Lisans gereksinimleri](access-reviews-overview.md#license-requirements).

## <a name="view-an-access-review"></a>Erişim gözden geçirmesini görüntüleme

Gözden geçirenler incelemelerini tamamlamalarını gözlemlediği için ilerlemeyi izleyebilirsiniz.

1. Azure portal oturum açın ve [Identity idare sayfasını](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/)açın.

1. Sol menüde, **erişim gözden geçirmeleri**' na tıklayın.

1. Listede, bir erişim gözden geçirmesi ' na tıklayın.

    Bir erişim incelemesinin gelecekteki örneklerini görüntülemek için erişim gözden geçirmesi ' na gidin ve zamanlanan İncelemeler ' ı seçin.

    **Genel bakış** sayfasında, geçerli örneğin ilerlemesini görebilirsiniz. İnceleme tamamlanana kadar dizinde erişim hakkı değiştirilmez.

     ![Tüm şirket grubunu gözden geçirme](./media/complete-access-review/all-company-group.png)

    Geçerli altındaki tüm dikey pencereler yalnızca her bir inceleme örneğinin süresi boyunca görüntülenebilir. 

    Sonuçlar sayfasında, sonuçları durdurma, sıfırlama ve Indirme özelliği de dahil olmak üzere, örnekte gözden geçirme bölümündeki her kullanıcı hakkında daha fazla bilgi sağlanır.

    ![Microsoft 365 Gruplar genelinde konuk erişimini gözden geçirme](./media/complete-access-review/all-company-group-results.png)


    Microsoft 365 Gruplar genelinde konuk erişimini inceleyen bir erişim incelemesi görüntülüyorsanız, genel bakış dikey penceresinde her bir grup listelenir. 
   
    ![Microsoft 365 Gruplar genelinde konuk erişimini gözden geçirme](./media/complete-access-review/review-guest-access-across-365-groups.png)

    Bu grupta gözden geçirmeyi ilerleme durumunu görmek için bir gruba tıklayın, ayrıca durdurma, sıfırlama, uygulama ve silme.

   ![Microsoft 365 Gruplar genelinde konuk erişimini ayrıntılı olarak gözden geçirin](./media/complete-access-review/progress-group-review.png)

1. Zamanlanan bitiş tarihine ulaşılmadan önce bir erişim incelemesini durdurmak istiyorsanız **Durdur** düğmesine tıklayın.

    Bir gözden geçirmeyi durdurduğunuzda, gözden geçirenler artık yanıt veremeyecektir. Bir gözden geçirmeyi durdurduktan sonra yeniden çalıştıramazsınız.

1. Artık erişim incelemesinin ilgilenmiyorsanız **Sil** düğmesine tıklayarak bunu silebilirsiniz.

## <a name="apply-the-changes"></a>Değişiklikleri Uygula

Sonuçları, **tamamlanma ayarları**' nda yaptığınız seçimlere göre **otomatik olarak Uygula** özelliği etkinleştirildiyse, otomatik uygulama, gözden geçirme işleminin bitiş tarihinden sonra veya gözden geçirmeyi el ile durdurduğunuzda yürütülür.

Gözden geçirme için **sonuçların otomatik olarak uygulanması** etkinleştirilmemişse, gözden geçirme süresi dolduktan sonra **serinin** altındaki **Gözden geçirme geçmişi** ' ne gidin veya İnceleme erken durdurulduktan sonra, uygulamak istediğiniz gözden geçirme örneğine tıklayın.

![Erişim gözden geçirme değişikliklerini Uygula](./media/complete-access-review/apply-changes.png)

Değişiklikleri el ile uygulamak için **Uygula** ' ya tıklayın. Gözden geçirme sırasında bir kullanıcının erişimi reddedildiyse, **Uygula**' ya TıKLADıĞıNıZDA Azure AD, üyeliklerini veya uygulama atamasını kaldırır.

![Erişim gözden geçirme Değişiklikleri Uygula düğmesi](./media/complete-access-review/apply-changes-button.png)


İnceleme durumu, **uygulama** ve son olarak durum **sonucu uygulanan** gibi ara durumlar aracılığıyla **tamamlandı** olarak değişir. Grup üyeliğinden veya uygulama atamasından birkaç dakika içinde kaldırılan reddedilen kullanıcıları görmeyi beklemelisiniz.

Sonuçların el ile veya otomatik olarak uygulanması, şirket içi dizinde veya dinamik bir grupta yer alan bir grup üzerinde etkiye sahip değildir. Şirket içinde kaynak olan bir grubu değiştirmek istiyorsanız, sonuçları indirin ve bu değişiklikleri bu dizindeki grubun gösterimine uygulayın.

## <a name="retrieve-the-results"></a>Sonuçları alma

Bir kerelik erişim incelemesinin sonuçlarını görüntülemek için **sonuçlar** sayfasına tıklayın. Yalnızca bir kullanıcının erişimini görüntülemek için, arama kutusuna, erişimi gözden geçirilmiş bir kullanıcının görünen adını veya Kullanıcı asıl adını yazın.

![Erişim gözden geçirmesi için sonuçları alma](./media/complete-access-review/retrieve-results.png) 

Yinelenen etkin bir erişim incelemesinin ilerlemesini görüntülemek için **sonuçlar** sayfasına tıklayın.

Yinelenen bir erişim incelemesinin tamamlanan bir örneğinin sonuçlarını görüntülemek için, **Gözden geçirme geçmişi**' ne tıklayın ve ardından örnek başlangıç ve bitiş tarihine göre tamamlanmış erişim gözden geçirme örnekleri listesinden belirli örneği seçin. Bu örneğin sonuçları, **sonuçlar** sayfasından elde edilebilir.

Bir erişim incelemesinin tüm sonuçlarını almak için **İndir** düğmesine tıklayın. Elde edilen CSV dosyası Excel 'de veya UTF-8 ile kodlanmış CSV dosyalarını açan diğer programlarda görüntülenebilir.

## <a name="remove-users-from-an-access-review"></a>Erişim gözden geçirden Kullanıcı kaldırma

 Varsayılan olarak, silinmiş bir kullanıcı Azure AD’de 30 gün boyunca silinmiş olarak kalır ve bu süre boyunca gerekirse bir yönetici tarafından geri alınabilir.  30 gün sonra bu kullanıcı kalıcı olarak silinir.  Ayrıca, bir Genel Yönetici bu süreye ulaşılmadan önce Azure Active Directory portalını kullanarak [kısa süre önce silinmiş bir kullanıcıyı kalıcı olarak silebilir](../fundamentals/active-directory-users-restore.md).  Bir kullanıcı kalıcı olarak silindikten sonra, bu kullanıcıya ilişkin sonraki veriler etkin erişim gözden geçirmelerinden kaldırılır.  Silinmiş kullanıcılara ilişkin denetim bilgileri, denetim günlüğünde kalır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD erişim gözden geçirmeleriyle kullanıcı erişimini yönetme](manage-user-access-with-access-reviews.md)
- [Azure AD erişim gözden geçirmeleriyle konuk erişimini yönetme](manage-guest-access-with-access-reviews.md)
- [Gruplar veya uygulamalar için erişim gözden geçirmesi oluşturma](create-access-review.md)
- [Bir Azure AD yönetici rolündeki kullanıcılar için erişim gözden geçirmesi oluşturma](../privileged-identity-management/pim-how-to-start-security-review.md)
