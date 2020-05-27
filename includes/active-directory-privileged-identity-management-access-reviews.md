---
title: include dosyası
description: include dosyası
services: active-directory
author: rolyon
ms.service: active-directory
ms.topic: include
ms.date: 05/26/2020
ms.author: barclayn
ms.custom: include file
ms.openlocfilehash: ee02200f1fb32d430a858d218e27534561ebc24a
ms.sourcegitcommit: 95269d1eae0f95d42d9de410f86e8e7b4fbbb049
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83868043"
---
## <a name="create-one-or-more-access-reviews"></a>Bir veya daha fazla erişim incelemesi oluşturun

1. Yeni erişim gözden geçirmesi oluşturmak için **Yeni** ' ye tıklayın.

1. Erişim gözden geçirmesini adlandırın. İsteğe bağlı olarak, incelemeye bir açıklama verin. Ad ve açıklama gözden geçirenlere gösterilir.

    ![Erişim incelemesi oluşturma-adı ve açıklamayı Gözden geçirme](./media/active-directory-privileged-identity-management-access-reviews/name-description.png)

1. **Başlangıç tarihini**ayarlayın. Varsayılan olarak, bir erişim gözden geçirmesi bir kez gerçekleşir, oluşturulduğu zaman başlatılır ve bir ayda sonlanır. Başlangıç ve bitiş tarihlerini, gelecekte bir erişim incelemesi başlatacak şekilde ve son olarak istediğiniz sayıda güne dönüştürebilirsiniz.

    ![Başlangıç tarihi, sıklık, süre, bitiş, zaman sayısı ve bitiş tarihi](./media/active-directory-privileged-identity-management-access-reviews/start-end-dates.png)

1. Erişim gözden geçirmesini yinelenen hale getirmek için **Sıklık** ayarını **bir saatten** **haftalık**, **aylık**, **üç aylık**, **yıllık**veya **yarı yıllık**olarak değiştirin. Her bir yinelenen serinin gözden geçiricilerin giriş için kaç gün sonra açık olacağını tanımlamak için **süre** kaydırıcısını veya metin kutusunu kullanın. Örneğin, aylık bir gözden geçirme için ayarlayabileceğiniz en uzun süre, çakışan incelemelerden kaçınmak için 27 gün olabilir.

1. Yinelenen erişim gözden geçirme serisinin nasıl sonlandıralınacağını belirtmek için **bitiş** ayarını kullanın. Seriler üç şekilde bitemez: sürekli olarak gözden geçirmeler başlatacak şekilde, belirli bir tarihe kadar veya tanımlanan sayıda oluşumdan sonra sürekli olarak çalışır. Siz, başka bir Kullanıcı Yöneticisi veya başka bir genel yönetici, **ayarları**, bu tarihte sona erecek şekilde değiştirerek, oluşturulduktan sonra seriyi durdurabilir.

1. **Kullanıcılar** bölümünde, üyeliğini gözden geçirmek istediğiniz bir veya daha fazla rol seçin.

    ![Rol üyeliğini gözden geçirmek için kullanıcılara kapsam](./media/active-directory-privileged-identity-management-access-reviews/users.png)

    > [!NOTE]
    > - Burada seçilen roller hem [kalıcı hem de uygun rolleri](../articles/active-directory/privileged-identity-management/pim-how-to-add-role-to-user.md)içerir.
    > - Birden fazla rol seçilmesi birden çok erişim incelemesi oluşturacaktır. Örneğin beş rol seçilmesi beş ayrı erişim incelemesi oluşturacaktır.

    **Azure AD rolleri**için erişim gözden geçirmesi oluşturuyorsanız, aşağıda gözden geçirme üyeliği listesini gösteren bir örnek gösterilir.

    ![Üyelik bölmesini İnceleme Azure AD rollerini listeleme](./media/active-directory-privileged-identity-management-access-reviews/review-membership.png)

    **Azure Kaynak rolleri**için erişim gözden geçirmesi oluşturuyorsanız aşağıdaki görüntüde üyelik İnceleme listesinin bir örneği gösterilir.

    ![Üyelik bölmesini İnceleme Azure Kaynak rollerini listeleme](./media/active-directory-privileged-identity-management-access-reviews/review-membership-azure-resource-roles.png)

1. **Gözden geçirenler** bölümünde, tüm kullanıcıları gözden geçirmek için bir veya daha fazla kişi seçin. Ya da üyelerin kendi erişimini incelemesini seçebilirsiniz.

    ![Seçili kullanıcıların veya üyelerin gözden geçirenler listesi (self)](./media/active-directory-privileged-identity-management-access-reviews/reviewers.png)

    - **Seçili kullanıcılar** -kimlerin erişimi olduğunu bilmiyorsanız bu seçeneği kullanın. Bu seçenekle, tamamlanacak bir kaynak sahibine veya grup yöneticisine incelemeyi atayabilirsiniz.
    - **Üyeler (self)** -bu seçeneği, kullanıcıların kendi rol atamalarını gözden geçirmesini sağlamak için kullanın.

### <a name="upon-completion-settings"></a>Tamamlama ayarlarından sonra

1. Bir gözden geçirme tamamlandıktan sonra ne olacağını belirtmek için **Tamamlama Ayarları** bölümüne bakın.

    ![Otomatik olarak uygulanacak ve gözden geçirmemelidir](./media/active-directory-privileged-identity-management-access-reviews/upon-completion-settings.png)

1. Reddedilen kullanıcılar için erişimi otomatik olarak kaldırmak istiyorsanız, **etkinleştirilecek** **sonuçları kaynağa otomatik uygula** ' yı ayarlayın. Gözden geçirme tamamlandığında sonuçları el ile uygulamak istiyorsanız, anahtarı **devre dışı**olarak ayarlayın.

1. Gözden geçiren tarafından gözden geçirilmemiş kullanıcılar için gözden geçirme süresi içinde gözden **geçirmeli yanıt** verme listesini kullanın. Bu ayar, gözden geçirenler tarafından el ile gözden geçirilmiş kullanıcıları etkilemez. Son Gözden geçirenin kararı reddederse, kullanıcının erişimi kaldırılır.

    - **Değişiklik yok** -kullanıcının erişimini değiştirmeden bırak
    - **Erişimi kaldır** -kullanıcının erişimini kaldır
    - **Erişimi onayla** -kullanıcının erişimini Onayla
    - **Öneriler alın** -kullanıcının devam eden erişimini reddetme veya onaylama konusunda sistemin önerisini alın

### <a name="advanced-settings"></a>Gelişmiş ayarlar

1. Ek ayarları belirtmek için **Gelişmiş ayarlar** bölümünü genişletin.

    ![Önerileri göster, onay için neden iste, posta bildirimleri ve anımsatıcılar için Gelişmiş ayarlar](./media/active-directory-privileged-identity-management-access-reviews/advanced-settings.png)

1. Kullanıcının erişim bilgilerini temel alarak sistem önerilerini gözden geçirenlere göstermek üzere **etkinleştirilecek** **önerileri göster** ' i ayarlayın.

1. Gözden geçirenin onay için bir neden vermesini gerektirmesini **sağlamak** için onay için bir **neden gerektir '** i ayarlayın.

1. Bir erişim incelemesi başladığında ve bir gözden geçirme tamamlandığında yöneticilere, Azure **ad 'nin gözden** geçirenlere e-posta bildirimleri göndermesini sağlamak için **posta bildirimleri** ayarlayın.

1. Azure AD 'nin, gözden geçirenleri tamamlamadıkları gözden geçirenler için sürmekte olan erişim gözden geçirmeleri gönderme anımsatıcıları **sağlamak üzere** **anımsatıcıları** ayarlayın.
