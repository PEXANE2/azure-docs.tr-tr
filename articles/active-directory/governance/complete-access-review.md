---
title: Gruplar & uygulamaların erişim incelemesini tamamlayın - Azure AD
description: Azure Active Directory erişim incelemelerinde grup üyelerinin veya uygulama erişiminin erişim incelemesini nasıl tamamlayabilirsiniz öğrenin.
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
ms.date: 07/23/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 266234f2872cfe99509d564c9460bfba4a0e2bf2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75932538"
---
# <a name="complete-an-access-review-of-groups-and-applications-in-azure-ad-access-reviews"></a>Azure AD erişim incelemelerinde grupların ve uygulamaların erişim incelemesini tamamlayın

Yönetici olarak, [grupların veya uygulamaların bir erişim incelemesini oluşturursunuz](create-access-review.md) ve gözden geçirenler [erişim incelemesini gerçekleştirir.](perform-access-review.md) Bu makalede, erişim incelemesinin sonuçlarını nasıl göreceğiz ve sonuçları nasıl uygulayacağı açıklanmaktadır.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="prerequisites"></a>Ön koşullar

- Azure AD Premium P2
- Genel yönetici, Kullanıcı yöneticisi, Güvenlik yöneticisi veya Güvenlik okuyucu

Daha fazla bilgi için Lisans [gereksinimlerine](access-reviews-overview.md#license-requirements)bakın.

## <a name="view-an-access-review"></a>Erişim incelemesini görüntüleme

Gözden geçirenler değerlendirmelerini tamamlarken ilerlemeyi izleyebilirsiniz.

1. Azure portalında oturum açın ve [Kimlik Yönetimi sayfasını](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/)açın.

1. Sol menüde, **Yorumlara Eriş'i**tıklatın.

1. Listede, bir erişim incelemesini tıklatın.

    Bir dizi erişim incelemesini görüntülemek için erişim incelemesine gidin ve Planlanan incelemelerde yaklaşan oluşumları bulabilirsiniz.

    Genel **Bakış** sayfasında ilerlemeyi görebilirsiniz. İnceleme tamamlanana kadar dizinde erişim hakları değiştirilmez.

    ![Access ilerlemeyi gözden geçirir](./media/complete-access-review/overview-progress.png)

1. Bir erişim incelemesini zamanlanan bitiş tarihine ulaşmadan durdurmak istiyorsanız, **Durdur** düğmesini tıklatın.

    Bir incelemeyi durdurduğunda, gözden geçirenler artık yanıt veremez. Durdurulduktan sonra incelemeyi yeniden başlatamazsınız.

1. Erişim incelemesiyle artık ilgilenmiyorsanız, **Sil** düğmesini tıklatarak silme düğmesini silebilirsiniz.

## <a name="apply-the-changes"></a>Değişiklikleri uygulayın

**Kaynağa Otomatik uygulama sonuçları** etkinleştirilmişse ve **Tamamlama ayarlarındaki**seçimlerinize bağlı olarak, incelemenin bitiş tarihinden sonra veya incelemeyi el ile durdurunuzda otomatik uygulama yürütülür.

İnceleme için **kaynağa Otomatik uygulama sonuçları** etkinleştirildiyse, değişiklikleri el ile uygulamak için **Uygula'yı** tıklatın. Gözden geçirmede bir kullanıcının erişimi reddedildiyse, **Uygula'yı**tıklattığınızda Azure AD, üyeliğini veya uygulama atamasını kaldırır.

![Erişim gözden geçirme değişikliklerini uygulama](./media/complete-access-review/apply-changes.png)

İncelemenin **durumu,** **Uygulama** ve son olarak uygulanan Sonuç durumuna gibi ara durumlar aracılığıyla Tamamlandı'dan **değişecektir.** Reddedilen kullanıcıların, varsa, grup üyeliğinden veya uygulama atamasından birkaç dakika içinde kaldırıldığını görmeyi bekleyebilirsiniz.

Yapılandırılmış otomatik uygulama incelemesi veya **Uygula'yı** seçmenin şirket içi dizinde veya dinamik bir gruptan kaynaklanan bir grup üzerinde etkisi yoktur. Şirket içinde oluşan bir grubu değiştirmek istiyorsanız, sonuçları indirin ve bu değişiklikleri grubun bu dizindeki temsiline uygulayın.

## <a name="retrieve-the-results"></a>Sonuçları alma

Tek seferlik erişim incelemesi nin sonuçlarını görüntülemek için **Sonuçlar** sayfasını tıklatın. Yalnızca bir kullanıcının erişimini görüntülemek için, Arama kutusuna, erişimi gözden geçirilen bir kullanıcının görüntü adını veya kullanıcı ana adını yazın.

![Erişim incelemesi için sonuçları alma](./media/complete-access-review/retrieve-results.png)

Yinelenen etkin bir erişim incelemesinin ilerlemesini görüntülemek için **Sonuçlar** sayfasını tıklatın.

Yinelenen bir erişim incelemesinin tamamlanmış bir örneğinin sonuçlarını görüntülemek için, **Gözden Geçir geçmişini**tıklatın ve ardından, örneğin başlangıç ve bitiş tarihine bağlı olarak tamamlanan erişim inceleme örnekleri listesinden belirli örneği seçin. Bu örneğin sonuçları **Sonuçlar** sayfasından elde edilebilir.

Erişim incelemesinin tüm sonuçlarını almak için **İndir** düğmesini tıklatın. Ortaya çıkan CSV dosyası Excel'de veya UTF-8 kodlanmış CSV dosyalarını açan diğer programlarda görüntülenebilir.

## <a name="remove-users-from-an-access-review"></a>Kullanıcıları bir erişim incelemesinden kaldırma

 Varsayılan olarak, silinmiş bir kullanıcı Azure AD’de 30 gün boyunca silinmiş olarak kalır ve bu süre boyunca gerekirse bir yönetici tarafından geri alınabilir.  30 gün sonra bu kullanıcı kalıcı olarak silinir.  Ayrıca, bir Genel Yönetici bu süreye ulaşılmadan önce Azure Active Directory portalını kullanarak [kısa süre önce silinmiş bir kullanıcıyı kalıcı olarak silebilir](../fundamentals/active-directory-users-restore.md).  Bir kullanıcı kalıcı olarak silindikten sonra, bu kullanıcıya ilişkin sonraki veriler etkin erişim gözden geçirmelerinden kaldırılır.  Silinmiş kullanıcılara ilişkin denetim bilgileri, denetim günlüğünde kalır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD erişim gözden geçirmeleriyle kullanıcı erişimini yönetme](manage-user-access-with-access-reviews.md)
- [Azure AD erişim gözden geçirmeleriyle konuk erişimini yönetme](manage-guest-access-with-access-reviews.md)
- [Grupların veya uygulamaların erişim gözden geçirmesini oluşturma](create-access-review.md)
- [Bir Azure AD yönetici rolündeki kullanıcılar için erişim gözden geçirmesi oluşturma](../privileged-identity-management/pim-how-to-start-security-review.md)
