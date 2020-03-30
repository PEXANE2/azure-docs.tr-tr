---
title: include dosyası
description: include dosyası
services: active-directory
author: rolyon
ms.service: active-directory
ms.topic: include
ms.date: 04/29/2019
ms.author: rolyon
ms.custom: include file
ms.openlocfilehash: d791c4ba46587ac5709d72cb31bc76f087118b03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67476245"
---
## <a name="create-one-or-more-access-reviews"></a>Bir veya daha fazla erişim incelemesi oluşturma

1. Yeni bir erişim incelemesi oluşturmak için **Yeni'yi** tıklatın.

1. Erişim incelemesini adlandırın. İsteğe bağlı olarak, gözden geçirmeye bir açıklama verin. Ad ve açıklama gözden geçirenlere gösterilir.

    ![Erişim incelemesi oluşturma - İnceleme adı ve açıklaması](./media/active-directory-privileged-identity-management-access-reviews/name-description.png)

1. Başlangıç **tarihini**ayarlayın. Varsayılan olarak, bir erişim incelemesi bir kez oluşur, oluşturulduğu anda başlar ve bir ay içinde sona erer. Başlangıç ve bitiş tarihlerini, gelecekte bir erişim incelemesi başlangıcı ve istediğiniz kadar gün olmak üzere değiştirebilirsiniz.

    ![Başlangıç tarihi, sıklık, süre, bitiş, kaç kez ve bitiş tarihi](./media/active-directory-privileged-identity-management-access-reviews/start-end-dates.png)

1. Erişim incelemesini yinelenen yapmak **için, Sıklık** ayarını **Bir defadan** **Haftalık,** **Aylık,** **Üç Aylık,** **Yıllık**veya Altı Aylık olarak **değiştirin.** Yinelenen serinin her incelemesinin gözden geçirenlerin girişi için kaç gün açık olacağını tanımlamak için **Süre** kaydırıcısını veya metin kutusunu kullanın. Örneğin, bir aylık inceleme için ayarlayabildiğiniz en yüksek süre, çakışan incelemeleri önlemek için 27 gündür.

1. Yinelenen erişim gözden geçirme serisinin nasıl sona erdirilen ini belirtmek için **Bitiş** ayarını kullanın. Seri üç şekilde sona erebilir: yorumları süresiz olarak, belirli bir tarihe kadar veya belirli sayıda olay tamamlandıktan sonra sürekli olarak çalışır. Siz, başka bir Kullanıcı yöneticisi veya başka bir Global yönetici, **ayarlardaki**tarihi değiştirerek, bu tarihte sona erecek şekilde, oluşturulduktan sonra seriyi durdurabilirsiniz.

1. **Kullanıcılar** bölümünde, üyeliği gözden geçirmek istediğiniz bir veya daha fazla rol seçin.

    ![Kullanıcıların rol üyeliğini gözden geçirmek için kapsamı](./media/active-directory-privileged-identity-management-access-reviews/users.png)

    > [!NOTE]
    > Birden fazla rol seçmek, birden çok erişim incelemesi oluşturur. Örneğin, beş rol seçmek beş ayrı erişim incelemesi oluşturur.

    Azure AD rollerinin bir erişim incelemesi oluşturuyorsanız, aşağıdaki gözden geçirme üyelik listesinin bir örneğini gösterir.

    ![Seçebileceğiniz Azure AD rollerini listeleyen üyelik bölmelerini gözden geçirin](./media/active-directory-privileged-identity-management-access-reviews/review-membership.png)

    Azure kaynak rollerinin bir erişim incelemesi oluşturuyorsanız, aşağıdaki gözden geçirme üyelik listesinin bir örneğini gösterir.

    ![Seçebileceğiniz Azure kaynak rollerini listeleyen üyelik bölmelerini gözden geçirin](./media/active-directory-privileged-identity-management-access-reviews/review-membership-azure-resource-roles.png)

1. Gözden **Geçirenler** bölümünde, tüm kullanıcıları gözden geçirmek için bir veya daha fazla kişi seçin. Veya üyelerin kendi erişimlerini gözden geçirmelerini seçebilirsiniz.

    ![Seçilen kullanıcıların veya üyelerin gözden geçirenler listesi (kendi kendine)](./media/active-directory-privileged-identity-management-access-reviews/reviewers.png)

    - **Seçili kullanıcılar** - Kimlertarafından erişilmeihtiyacı olduğunu bilmiyorsanız bu seçeneği kullanın. Bu seçenekle, incelemeyi tamamlanması için bir kaynak sahibine veya grup yöneticisine atayabilirsiniz.
    - **Üyeler (kendi kendine)** - Kullanıcıların kendi rol atamalarını gözden geçirmesini sağlamak için bu seçeneği kullanın.

### <a name="upon-completion-settings"></a>Tamamlanma ayarları üzerine

1. İnceleme tamamlandıktan sonra ne olacağını belirtmek **için, Tamamlanın ayarları** bölümünü genişletin.

    ![Otomatik uygulama için tamamlanma ayarları sonra ve yanıt vermiyor gözden geçirmelisiniz](./media/active-directory-privileged-identity-management-access-reviews/upon-completion-settings.png)

1. Reddedilen kullanıcıların erişimini otomatik olarak kaldırmak istiyorsanız, **Otomatik'i kaynak için** **Etkinleştir'e**uygulayın' ı ayarlayın. İnceleme tamamlandığında sonuçları el ile uygulamak istiyorsanız, anahtarı Devre **Dışı Bırak'a**ayarlayın.

1. Gözden **geçiren** tarafından gözden geçirilmedir kullanıcılar için ne olacağını belirtmek için gözden geçiren değil listesini kullanın. Bu ayar, gözden geçirenler tarafından el ile gözden geçirilen kullanıcıları etkilemez. Son gözden geçirenin kararı Reddedilirse, kullanıcının erişimi kaldırılır.

    - **Değişiklik yok** - Kullanıcının erişimini değiştirmeden bırakın
    - **Erişimi kaldırma** - Kullanıcının erişimini kaldırma
    - **Erişimi onayla** - Kullanıcının erişimini onayla
    - **Öneriler alın** - Kullanıcının sürekli erişimini reddetme veya onaylama konusunda sistemin önerisini alın

### <a name="advanced-settings"></a>Gelişmiş ayarlar

1. Ek ayarlar belirtmek için **Gelişmiş ayarlar** bölümünü genişletin.

    ![Öneriler göstermek için gelişmiş ayarlar, onay için neden, posta bildirimleri ve anımsatıcılar gerektirir](./media/active-directory-privileged-identity-management-access-reviews/advanced-settings.png)

1. Gözden geçirenlere kullanıcının erişim bilgilerini temel alan sistem önerilerini göstermek **için** **önerileri göster'i** ayarlayın.

1. Gözden geçirenin onay için bir neden sağlamasını gerektirecek **etkinleştirmek** için **onay üzerinde neden gerektirin.**

1. **Posta bildirimlerini** Azure AD'nin erişim incelemesi başladığında gözden geçirenlere ve inceleme tamamlandığında yöneticilere e-posta bildirimleri göndermesini **etkinleştirecek** şekilde ayarlayın.

1. Azure AD'nin incelemesini tamamlamamış gözden geçirenlere devam eden erişim incelemelerini anımsatımlarını göndermesini **etkinleştirmek** için **Anımsatıcıları** ayarlayın.
