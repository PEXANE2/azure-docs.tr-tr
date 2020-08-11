---
title: Gruplar & uygulamalar için erişim gözden geçirmesi oluşturma-Azure AD
description: Azure Active Directory erişim gözden geçirmeleriyle grup üyelerinin veya uygulama erişiminin erişim incelemesini oluşturmayı öğrenin.
services: active-directory
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 08/09/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9bb290106c5ceafe8c636bbeeab38b74ea475eb4
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88056202"
---
# <a name="create-an-access-review-of-groups-and-applications-in-azure-ad-access-reviews"></a>Azure AD erişim gözden geçirmeleriyle gruplar ve uygulamalar için erişim gözden geçirmesi oluşturma

Çalışanlar için gruplara ve uygulamalara erişim ve zaman içinde konuklar değişiklikleri. Yöneticiler, eski erişim atamalarıyla ilişkili riski azaltmak için Azure Active Directory (Azure AD) kullanarak Grup üyeleri veya uygulama erişimi için erişim gözden geçirmeleri oluşturabilir. Erişimi düzenli olarak gözden geçirmeniz gerekiyorsa yinelenen erişim İncelemeleri de oluşturabilirsiniz. Bu senaryolar hakkında daha fazla bilgi için bkz. [Kullanıcı erişimini yönetme](manage-user-access-with-access-reviews.md) ve [konuk erişimini yönetme](manage-guest-access-with-access-reviews.md).

Bu makalede, Grup üyeleri veya uygulama erişimi için bir veya daha fazla erişim incelemesi oluşturma açıklanır.

## <a name="prerequisites"></a>Önkoşullar

- Azure AD Premium P2
- Genel yönetici veya Kullanıcı Yöneticisi

Daha fazla bilgi için bkz. [Lisans gereksinimleri](access-reviews-overview.md#license-requirements).

## <a name="create-one-or-more-access-reviews"></a>Bir veya daha fazla erişim incelemesi oluşturun

1. Azure portal oturum açın ve [Identity idare sayfasını](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/)açın.

1. Sol menüde, **erişim gözden geçirmeleri**' na tıklayın.

1. Yeni erişim gözden geçirmesi oluşturmak için **Yeni erişim gözden geçirmesi** ' na tıklayın.

    ![Kimlik yönetimi 'nde erişim İncelemeleri bölmesi](./media/create-access-review/access-reviews.png)

1. Erişim gözden geçirmesini adlandırın. İsteğe bağlı olarak, incelemeye bir açıklama verin. Ad ve açıklama gözden geçirenlere gösterilir.

    ![Erişim incelemesi oluşturma-adı ve açıklamayı Gözden geçirme](./media/create-access-review/name-description.png)

1. **Başlangıç tarihini**ayarlayın. Varsayılan olarak, bir erişim gözden geçirmesi bir kez gerçekleşir, oluşturulduğu zaman başlatılır ve bir ayda sonlanır. Başlangıç ve bitiş tarihlerini, gelecekte bir erişim incelemesi başlatacak şekilde ve son olarak istediğiniz sayıda güne dönüştürebilirsiniz.

    ![Erişim incelemesi oluşturma-başlangıç ve bitiş tarihleri](./media/create-access-review/start-end-dates.png)

1. Erişim gözden geçirmesini yinelenen hale getirmek için **Sıklık** ayarını **bir saatten** **haftalık**, **aylık**, **üç aylık**, **yarı yıllık**veya **yıllık**olarak değiştirin. Her bir yinelenen serinin gözden geçiricilerin giriş için kaç gün sonra açık olacağını tanımlamak için **süre** kaydırıcısını veya metin kutusunu kullanın. Örneğin, aylık bir gözden geçirme için ayarlayabileceğiniz en uzun süre, çakışan incelemelerden kaçınmak için 27 gün olabilir.

1. Yinelenen erişim gözden geçirme serisinin nasıl sonlandıralınacağını belirtmek için **bitiş** ayarını kullanın. Seriler üç şekilde bitemez: sürekli olarak gözden geçirmeler başlatacak şekilde, belirli bir tarihe kadar veya tanımlanan sayıda oluşumdan sonra sürekli olarak çalışır. Siz, başka bir Kullanıcı Yöneticisi veya başka bir genel yönetici, **ayarları**, bu tarihte sona erecek şekilde değiştirerek, oluşturulduktan sonra seriyi durdurabilir.

1. **Kullanıcılar** bölümünde, erişim incelemesinin uygulandığı kullanıcıları belirtin. Erişim incelemeleri bir grubun üyeleri veya bir uygulamaya atanmış kullanıcılar için olabilir. Üye olan veya uygulamaya erişimi olan tüm kullanıcıları gözden geçirmek yerine, yalnızca üye olan (veya uygulamaya atanan) Konuk kullanıcıları gözden geçirmek için erişim incelemesini daha fazla kapsama aktarabilirsiniz.

    ![Erişim incelemesi oluşturma-kullanıcılar](./media/create-access-review/users.png)

1. **Grup** bölümünde, üyeliğini gözden geçirmek istediğiniz bir veya daha fazla grup seçin.

    > [!NOTE]
    > Birden fazla grup seçilmesi birden çok erişim incelemesi oluşturacaktır. Örneğin, beş grup seçilmesi beş ayrı erişim incelemesi oluşturacaktır.
    
    ![Erişim incelemesi oluşturma-Grup seçme](./media/create-access-review/select-group.png)

1. **Uygulamalar** bölümünde (adım 8 ' de **bir uygulamaya atanmış** ' i seçtiyseniz), erişimi incelemek istediğiniz uygulamaları seçin.

    > [!NOTE]
    > Birden fazla uygulama seçilmesi birden çok erişim incelemesi oluşturacaktır. Örneğin beş uygulama seçildiğinde beş ayrı erişim incelemesi oluşturulur.
    
    ![Erişim incelemesi oluşturma-uygulama seçme](./media/create-access-review/select-application.png)

1. **Gözden geçirenler** bölümünde, kapsamdaki tüm kullanıcıları gözden geçirmek için bir ya da daha fazla kişi seçin. Ya da üyelerin kendi erişimini incelemesini seçebilirsiniz. Kaynak bir grup ise, Grup sahiplerini gözden geçirmesini isteyebilirsiniz. Ayrıca, gözden geçirenlerin erişimi onayladıklarında bir neden vermesini de isteyebilirsiniz.

    ![Erişim incelemesi oluşturma-gözden geçirenler](./media/create-access-review/reviewers.png)

1. **Programlar** bölümünde, kullanmak istediğiniz programı seçin. **Varsayılan program** her zaman vardır.

    ![Erişim incelemesi oluşturma-programlar](./media/create-access-review/programs.png)

    Farklı amaçlar için erişim incelemelerini, programlar halinde düzenleyerek nasıl izleneceğini ve toplayacağınızı basitleştirebilirsiniz. Her erişim incelemesi bir programla bağlantılı olabilir. Daha sonra bir denetçi için rapor hazırlarken, belirli bir girişim için kapsamdaki erişim incelemelerine odaklanabilirsiniz. Programlar ve erişim gözden geçirme sonuçları, genel yönetici, Kullanıcı Yöneticisi, güvenlik yöneticisi veya güvenlik okuyucusu rolündeki kullanıcılar tarafından görülebilir.

    Programların listesini görmek için, erişim İncelemeleri sayfasına gidin ve **Programlar**' ı seçin. Genel yönetici veya Kullanıcı Yöneticisi rollüyorsanız, ek programlar oluşturabilirsiniz. Örneğin, her uyumluluk girişimi veya iş hedefi için bir program olmasını seçebilirsiniz. Artık bir programa ihtiyacınız yoksa ve onunla bağlantılı denetimleri yoksa, onu silebilirsiniz.

### <a name="upon-completion-settings"></a>Tamamlama ayarlarından sonra

1. Bir gözden geçirme tamamlandıktan sonra ne olacağını belirtmek için **Tamamlama Ayarları** bölümüne bakın.

    ![Erişim gözden geçirmesi oluşturma-tamamlanma ayarları](./media/create-access-review/upon-completion-settings.png)

1. Reddedilen kullanıcılar için erişimi otomatik olarak kaldırmak istiyorsanız, **etkinleştirilecek** **sonuçları kaynağa otomatik uygula** ' yı ayarlayın. Gözden geçirme tamamlandığında sonuçları el ile uygulamak istiyorsanız, anahtarı **devre dışı**olarak ayarlayın.

1. Gözden geçiren tarafından gözden geçirilmemiş kullanıcılar için gözden geçirme süresi içinde gözden **geçirmeli yanıt** verme listesini kullanın. Bu ayar, gözden geçirenler tarafından el ile gözden geçirilmiş kullanıcıları etkilemez. Son Gözden geçirenin kararı reddederse, kullanıcının erişimi kaldırılır.

    - **Değişiklik yok** -kullanıcının erişimini değiştirmeden bırak
    - **Erişimi kaldır** -kullanıcının erişimini kaldır
    - **Erişimi onayla** -kullanıcının erişimini Onayla
    - **Öneriler alın** -kullanıcının devam eden erişimini reddetme veya onaylama konusunda sistemin önerisini alın

### <a name="advanced-settings"></a>Gelişmiş ayarlar

1. Ek ayarları belirtmek için **Gelişmiş ayarlar** bölümünü genişletin.

    ![Erişim incelemesi oluşturma-Gelişmiş ayarlar](./media/create-access-review/advanced-settings-preview.png)

1. Kullanıcının erişim bilgilerini temel alarak sistem önerilerini gözden geçirenlere göstermek üzere **etkinleştirilecek** **önerileri göster** ' i ayarlayın.

1. Gözden geçirenin onay için bir neden vermesini gerektirmesini **sağlamak** için onay için bir **neden gerektir '** i ayarlayın.

1. Bir erişim incelemesi başladığında ve bir gözden geçirme tamamlandığında yöneticilere, Azure **ad 'nin gözden** geçirenlere e-posta bildirimleri göndermesini sağlamak için **posta bildirimleri** ayarlayın.

1. Azure AD 'nin, gözden geçirenleri tamamlamadıkları gözden geçirenler için sürmekte olan erişim gözden geçirmeleri gönderme anımsatıcıları **sağlamak üzere** **anımsatıcıları** ayarlayın. 

    >[!NOTE]
    > Varsayılan olarak, Azure AD, henüz yanıt vermemiş olan gözden geçirenlere bitiş tarihine kadar otomatik olarak bir anımsatıcı gönderir

1. Önizle Gözden geçirenlere gönderilen e-postanın içeriği İnceleme adı, kaynak adı, son tarih vb. gözden geçirme ayrıntılarına göre otomatik olarak oluşturulur. Ek yönergeler veya iletişim bilgileri gibi ek bilgilere iletişim kurmak için bir yönteme ihtiyacınız varsa, bu ayrıntıları gözden geçiren için ek içerikte, davet ve anımsatıcı e-postalarına gönderilen gözden geçirenler e-postalarına dahil edilecek şekilde belirtebilirsiniz. Aşağıdaki Vurgulanan bölümde bu bilgilerin görüntüleneceği yer verilmiştir.

    ![Kullanıcılara bir gruba erişimi gözden geçirme](./media/create-access-review/review-users-access-group.png)

## <a name="start-the-access-review"></a>Erişim gözden geçirmesini Başlat

Erişim gözden geçirmesi ayarlarını belirttikten sonra **Başlat**' a tıklayın. Erişim incelemesi, listenizde durumunun bir göstergesi olacak şekilde görünür.

![Erişim gözden geçirmeleri ve durumlarının listesi](./media/create-access-review/access-reviews-list.png)

Varsayılan olarak, Azure AD gözden geçirenlere İnceleme başladıktan kısa bir bir e-posta gönderir. Azure AD 'ye e-posta gönderme seçeneğini belirlerseniz, gözden geçirenlere bir erişim incelemesinin tamamlanmasını beklediğini bildirin. Bunlara, [gruplara veya uygulamalara erişimi gözden geçirme](perform-access-review.md)yönergelerini gösterebilirsiniz. Gözden geçirmeniz konuklarınız kendi erişimini gözden geçirmeleri için ise, bunları [gruplara veya uygulamalara yönelik erişimin nasıl incelendiğinin](review-your-access.md)talimatlarını gösterir.

Konukları gözden geçirenler olarak atadıysanız ve daveti kabul etmediyse, bu kullanıcılar, gözden geçirenlerden önce daveti kabul etmeleri gerektiğinden, erişim incelemelerinden e-posta almamalıdır.

## <a name="access-review-status-table"></a>Erişim gözden geçirmesi durum tablosu

| Durum | Tanım |
|--------|------------|
|NotStarted | İnceleme oluşturuldu, Kullanıcı keşfi başlamak için bekliyor. |
|Başlatılıyor   | Gözden geçirinin parçası olan tüm kullanıcıları tanımlamak için Kullanıcı keşfi devam ediyor. |
|Başlatılıyor | İnceleme başlatılıyor. E-posta bildirimleri etkinse, e-postalar gözden geçirenlere gönderilir. |
|Ediyor | İnceleme başlatıldı. E-posta bildirimlerinin etkin olduğu e-postalar gözden geçirenlere gönderilmiştir. Gözden geçirenler, son tarihe kadar karar gönderebilir. |
|Tamamlan | Gözden geçirme tamamlandı ve gözden geçirme sahibine e-postalar gönderiliyor. |
|Oto gözden geçirme | İnceleme bir sistem inceleme aşamasında. Sistem, öneriler veya önceden yapılandırılmış kararlar temelinde incelenebilecek kullanıcılar için kararları kaydediyor. |
|Oto gözden geçirildi | Gözden geçirilmemiş tüm kullanıcılar için sistem tarafından kararlar kaydedilir. İnceleme, otomatik uygulama etkinse **uygulamaya** devam etmek için hazırlayın. |
|Uygulamayı | Onaylanan kullanıcılar için erişim değişikliği olmayacaktır. |
|Uygulandı | Varsa, reddedilen kullanıcılar kaynak veya dizinden kaldırılmıştır. |

## <a name="create-reviews-via-apis"></a>API 'Ler aracılığıyla incelemeler oluşturma

API 'Leri kullanarak erişim İncelemeleri de oluşturabilirsiniz. Azure portal grup ve uygulama kullanıcılarının erişim incelemelerini yönetmek için ne yapmanız gerekir Microsoft Graph API 'Leri kullanılarak da yapılabilir. Daha fazla bilgi için bkz. [Azure AD erişim gözden GEÇIRMELERI API başvurusu](https://docs.microsoft.com/graph/api/resources/accessreviews-root?view=graph-rest-beta). Kod örneği için bkz. [Microsoft Graph aracılığıyla Azure AD erişim gözden geçirmeleri alma örneği](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/m-p/236096).

## <a name="next-steps"></a>Sonraki adımlar

- [Gruplara veya uygulamalara erişimi gözden geçirme](perform-access-review.md)
- [Grupları veya uygulamaları kendiniz için erişimi gözden geçirin](review-your-access.md)
- [Grupların veya uygulamaların erişim incelemesini tamamlar](complete-access-review.md)
