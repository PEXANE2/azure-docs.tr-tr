---
title: Gruplar & uygulamalarda erişim incelemesi oluşturma - Azure AD
description: Azure Active Directory erişim incelemelerinde grup üyelerinin veya uygulama erişiminin erişim incelemesini nasıl oluşturabilirsiniz öğrenin.
services: active-directory
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/08/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9e01951b6147cfc39fe6c46035db822071bda3aa
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80984088"
---
# <a name="create-an-access-review-of-groups-and-applications-in-azure-ad-access-reviews"></a>Azure AD erişim incelemelerinde grupların ve uygulamaların erişim incelemesi oluşturma

Çalışanlar ve misafirler için gruplara ve uygulamalara erişim zaman içinde değişir. Yöneticiler, eski erişim atamalarıyla ilişkili riski azaltmak için, grup üyeleri veya uygulama erişimi için erişim incelemeleri oluşturmak için Azure Etkin Dizini'ni (Azure AD) kullanabilir. Erişimi düzenli olarak gözden geçirmeniz gerekiyorsa, yinelenen erişim incelemeleri de oluşturabilirsiniz. Bu senaryolar hakkında daha fazla bilgi [Manage guest access](manage-guest-access-with-access-reviews.md)için [bkz.](manage-user-access-with-access-reviews.md)

Bu makalede, grup üyeleri veya uygulama erişimi için bir veya daha fazla erişim incelemesi nasıl oluşturulacak açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

- Azure AD Premium P2
- Genel yönetici veya Kullanıcı yöneticisi

Daha fazla bilgi için Lisans [gereksinimlerine](access-reviews-overview.md#license-requirements)bakın.

## <a name="create-one-or-more-access-reviews"></a>Bir veya daha fazla erişim incelemesi oluşturma

1. Azure portalında oturum açın ve [Kimlik Yönetimi sayfasını](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/)açın.

1. Sol menüde, **Yorumlara Eriş'i**tıklatın.

1. Yeni bir erişim incelemesi oluşturmak için **Yeni erişim incelemesini** tıklatın.

    ![Kimlik Yönetimi'nde access incelemeleri bölmesi](./media/create-access-review/access-reviews.png)

1. Erişim incelemesini adlandırın. İsteğe bağlı olarak, gözden geçirmeye bir açıklama verin. Ad ve açıklama gözden geçirenlere gösterilir.

    ![Erişim incelemesi oluşturma - İnceleme adı ve açıklaması](./media/create-access-review/name-description.png)

1. Başlangıç **tarihini**ayarlayın. Varsayılan olarak, bir erişim incelemesi bir kez oluşur, oluşturulduğu anda başlar ve bir ay içinde sona erer. Başlangıç ve bitiş tarihlerini, gelecekte bir erişim incelemesi başlangıcı ve istediğiniz kadar gün olmak üzere değiştirebilirsiniz.

    ![Erişim incelemesi oluşturma - Başlangıç ve bitiş tarihleri](./media/create-access-review/start-end-dates.png)

1. Erişim incelemesini yinelenen yapmak **için, Sıklık** ayarını **Bir defadan** **Haftalık,** **Aylık,** **Üç Aylık,** **Altı aylık**veya **Yıllık**olarak değiştirin. Yinelenen serinin her incelemesinin gözden geçirenlerin girişi için kaç gün açık olacağını tanımlamak için **Süre** kaydırıcısını veya metin kutusunu kullanın. Örneğin, bir aylık inceleme için ayarlayabildiğiniz en yüksek süre, çakışan incelemeleri önlemek için 27 gündür.

1. Yinelenen erişim gözden geçirme serisinin nasıl sona erdirilen ini belirtmek için **Bitiş** ayarını kullanın. Seri üç şekilde sona erebilir: yorumları süresiz olarak, belirli bir tarihe kadar veya belirli sayıda olay tamamlandıktan sonra sürekli olarak çalışır. Siz, başka bir Kullanıcı yöneticisi veya başka bir Global yönetici, **ayarlardaki**tarihi değiştirerek, bu tarihte sona erecek şekilde, oluşturulduktan sonra seriyi durdurabilirsiniz.

1. **Kullanıcılar** bölümünde, erişim incelemesinin uygulandığı kullanıcıları belirtin. Erişim incelemeleri bir grubun üyeleri veya bir uygulamaya atanan kullanıcılar için olabilir. Erişim gözden geçirmesini, üye olan veya uygulamaya erişimi olan tüm kullanıcıları gözden geçirmek yerine yalnızca üye olan (veya uygulamaya atanmış) konuk kullanıcıları gözden geçirmek için kapsamınıza girebilirsiniz.

    ![Erişim incelemesi oluşturma - Kullanıcılar](./media/create-access-review/users.png)

1. **Grup** bölümünde, üyeliği gözden geçirmek istediğiniz bir veya daha fazla grup seçin.

    > [!NOTE]
    > Birden fazla grup seçmek, birden çok erişim incelemesi oluşturur. Örneğin, beş grup seçmek beş ayrı erişim incelemesi oluşturur.
    
    ![Erişim incelemesi oluşturma - Grubu seçin](./media/create-access-review/select-group.png)

1. **Uygulamalar** bölümünde (8. adımda **bir uygulamaya Atanmış'ı** seçtiyseniz), erişimi gözden geçirmek istediğiniz uygulamaları seçin.

    > [!NOTE]
    > Birden fazla uygulama seçmek birden çok erişim incelemesi oluşturur. Örneğin, beş uygulama seçmek beş ayrı erişim incelemesi oluşturur.
    
    ![Erişim incelemesi oluşturma - Uygulamayı seçin](./media/create-access-review/select-application.png)

1. Gözden **Geçirenler** bölümünde, kapsamdaki tüm kullanıcıları gözden geçirmek için bir veya daha fazla kişi seçin. Veya üyelerin kendi erişimlerini gözden geçirmelerini seçebilirsiniz. Kaynak bir grupsa, grup sahiplerinden gözden geçirmelerini isteyebilirsiniz. Ayrıca, gözden geçirenlerin erişimi onayladıklarında bir neden sağlamasını da gerektirebilirsiniz.

    ![Erişim incelemesi oluşturma - Gözden Geçirenler](./media/create-access-review/reviewers.png)

1. **Programlar** bölümünde, kullanmak istediğiniz programı seçin. **Varsayılan Program** her zaman mevcuttur.

    ![Erişim incelemesi oluşturma - Programlar](./media/create-access-review/programs.png)

    Farklı amaçlar için erişim değerlendirmelerini programlarhalinde düzenleyerek izleme ve toplama yolunuz basitleştirebilirsiniz. Her erişim incelemesi bir programa bağlanabilir. Daha sonra bir denetçi için raporlar hazırlarken, belirli bir girişim için kapsamdaki erişim incelemelerini odaklayabilirsiniz. Programlar ve erişim inceleme sonuçları Global yönetici, Kullanıcı yöneticisi, Güvenlik yöneticisi veya Güvenlik okuyucu rolündeki kullanıcılar tarafından görülebilir.

    Programların listesini görmek için erişim incelemeleri sayfasına gidin ve **Programlar'ı**seçin. Global yönetici veya Kullanıcı yöneticisi rolündeyseniz, ek programlar oluşturabilirsiniz. Örneğin, her uyumluluk girişimi veya iş hedefi için bir programa sahip olmayı seçebilirsiniz. Artık bir programa ihtiyacınız yoksa ve programa bağlı herhangi bir denetimi yoksa, silebilirsiniz.

### <a name="upon-completion-settings"></a>Tamamlanma ayarları üzerine

1. İnceleme tamamlandıktan sonra ne olacağını belirtmek **için, Tamamlanın ayarları** bölümünü genişletin.

    ![Erişim incelemesi oluşturma - Tamamlanma ayarları üzerine](./media/create-access-review/upon-completion-settings.png)

1. Otomatik olarak kaldırmak istiyorsanız, reddedilen kullanıcılar için erişim, **Otomatik etkinleştirmek** **için kaynağa sonuçları uygulayın** ayarlayın. İnceleme tamamlandığında sonuçları el ile uygulamak istiyorsanız, anahtarı Devre **Dışı Bırak'a**ayarlayın.

1. Gözden **geçiren** tarafından gözden geçirilmedir kullanıcılar için ne olacağını belirtmek için gözden geçiren değil listesini kullanın. Bu ayar, gözden geçirenler tarafından el ile gözden geçirilen kullanıcıları etkilemez. Son gözden geçirenin kararı Reddedilirse, kullanıcının erişimi kaldırılır.

    - **Değişiklik yok** - Kullanıcının erişimini değiştirmeden bırakın
    - **Erişimi kaldırma** - Kullanıcının erişimini kaldırma
    - **Erişimi onayla** - Kullanıcının erişimini onayla
    - **Öneriler alın** - Kullanıcının sürekli erişimini reddetme veya onaylama konusunda sistemin önerisini alın

### <a name="advanced-settings"></a>Gelişmiş ayarlar

1. Ek ayarlar belirtmek için **Gelişmiş ayarlar** bölümünü genişletin.

    ![Erişim incelemesi oluşturma - Gelişmiş ayarlar](./media/create-access-review/advanced-settings.png)

1. Gözden geçirenlere kullanıcının erişim bilgilerini temel alan sistem önerilerini göstermek **için** **önerileri göster'i** ayarlayın.

1. Gözden geçirenin onay için bir neden sağlamasını gerektirecek **etkinleştirmek** için **onay üzerinde neden gerektirin.**

1. **Posta bildirimlerini** Azure AD'nin erişim incelemesi başladığında gözden geçirenlere ve inceleme tamamlandığında yöneticilere e-posta bildirimleri göndermesini **etkinleştirecek** şekilde ayarlayın.

1. Azure AD'nin incelemesini tamamlamamış gözden geçirenlere devam eden erişim incelemelerini anımsatımlarını göndermesini **etkinleştirmek** için **Anımsatıcıları** ayarlayın.

    Varsayılan olarak, bitiş tarihine kadar olan sürenin yarısına ulaşıldığında, Azure AD henüz yanıt vermemiş olan gözden geçirenlere bir anımsatıcı gönderir.

## <a name="start-the-access-review"></a>Erişim incelemesini başlatma

Erişim incelemesi ayarlarını belirttikten sonra **Başlat'ı**tıklatın. Erişim incelemesi listenizde durumunun bir göstergesiyle görünür.

![Erişim incelemeleri listesi ve durumları](./media/create-access-review/access-reviews-list.png)

Varsayılan olarak, Azure AD inceleme başladıktan kısa bir süre sonra gözden geçirenlere bir e-posta gönderir. E-postayı Azure AD'nin göndermemesini seçerseniz, gözden geçirenlere bir erişim incelemesinin tamamlanmasını beklediğini bildirdiğinden emin olun. Gruplara [veya uygulamalara erişimi gözden geçirme](perform-access-review.md)yönergelerini onlara gösterebilirsiniz. İncelemeniz konukların kendi erişimlerini gözden geçirmeleri içinse, onlara [gruplara veya uygulamalara erişimi](review-your-access.md)nasıl gözden geçireceklerine yönelik talimatları gösterin.

Konukları gözden geçiren olarak atadıysanız ve daveti kabul etmedilerse, önce daveti incelemeden önce kabul etmeleri gerektiğinden erişim incelemelerinden bir e-posta almayacaktır.

## <a name="access-review-status-table"></a>Erişim gözden geçirme durum tablosu

| Durum | Tanım |
|--------|------------|
|Başlatılmama | İnceleme oluşturuldu, kullanıcı bulma başlatmak için bekliyor. |
|Başlatılıyor   | İncelemenin bir parçası olan tüm kullanıcıları tanımlamak için kullanıcı bulma işlemi devam ediyor. |
|Başlatılıyor | İnceleme başlıyor. E-posta bildirimleri etkinse, e-postalar gözden geçirenlere gönderilir. |
|Devam Ediyor | İnceleme başladı. E-posta bildirimleri etkinse, gözden geçirenlere e-postalar gönderilir. Gözden geçirenler kararları vade tarihine kadar sunabilirler. |
|Tamamla | İnceleme tamamlanıyor ve e-postalar inceleme sahibine gönderiliyor. |
|AutoReviewing | Gözden geçirme bir sistem gözden geçirme aşamasındadır. Sistem, önerilere veya önceden yapılandırılmış kararlara dayalı olarak gözden geçirilmeyen kullanıcılar için kararları kaydediyor. |
|Otomatik Yorumlanan | Kararlar, gözden geçirilemeyen tüm kullanıcılar için sistem tarafından kaydedilmiştir. Otomatik Uygulama etkinse **Uygulama'ya** devam etmek için gözden geçirme hazırdır. |
|Uygulama | Onaylanan kullanıcılar için erişimde değişiklik olmayacaktır. |
|Uygulandı | Reddedilen kullanıcılar, varsa, kaynaktan veya dizinden kaldırıldı. |

## <a name="create-reviews-via-apis"></a>API'ler aracılığıyla incelemeler oluşturma

API'leri kullanarak da erişim incelemeleri oluşturabilirsiniz. Azure portalındaki grupların ve uygulama kullanıcılarının erişim değerlendirmelerini yönetmek için yaptığınız şey Microsoft Graph API'leri kullanılarak da yapılabilir. Daha fazla bilgi için, [Azure AD erişim incelemeleri API başvurusuna](https://docs.microsoft.com/graph/api/resources/accessreviews-root?view=graph-rest-beta)bakın. Bir kod örneği için, [Microsoft Graph üzerinden Azure AD erişim incelemelerini alma örneği'ne](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/m-p/236096)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Gruplara veya uygulamalara erişimi gözden geçirme](perform-access-review.md)
- [Gruplara veya uygulamalara kendiniz için erişimi gözden geçirin](review-your-access.md)
- [Grupların veya uygulamaların erişim gözden geçirmesini tamamlama](complete-access-review.md)
