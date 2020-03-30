---
title: Kullanım Koşulları - Azure Active Directory | Microsoft Dokümanlar
description: Erişime girmeden önce çalışanlara veya konuklara bilgi sunmak için Azure Active Directory kullanım koşullarını kullanmaya başlayın.
services: active-directory
ms.service: active-directory
ms.subservice: compliance
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jocastel
ms.collection: M365-identity-device-management
ms.openlocfilehash: a2f06a7c88a7c17f5f93201192664c2d4a97564e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480972"
---
# <a name="azure-active-directory-terms-of-use"></a>Azure Active Directory kullanım koşulları

Azure AD kullanım koşulları, kuruluşların bilgileri son kullanıcılara sunmak için kullanabileceği basit bir yöntem sağlar. Bu sunum, kullanıcıların yasal gereksinimler veya uyumluluk gereksinimleriyle ilgili bildirimleri görmesi sağlar. Bu makalede, kullanım koşulları ile başlamak için nasıl açıklanmaktadır.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview-videos"></a>Videolara genel bakış

Aşağıdaki video, kullanım koşullarına hızlı bir genel bakış sağlar.

>[!VIDEO https://www.youtube.com/embed/tj-LK0abNao]

Ek videolar için bkz:
- [Azure Etkin Dizini'nde kullanım koşulları nasıl dağıtılır?](https://www.youtube.com/embed/N4vgqHO2tgY)
- [Azure Active Directory'de kullanım koşulları nasıl kullanıma sunulur?](https://www.youtube.com/embed/t_hA4y9luCY)

## <a name="what-can-i-do-with-terms-of-use"></a>Kullanım koşullarıyla ne yapabilirim?

Azure AD kullanım koşulları aşağıdaki özelliklere sahiptir:

- Çalışanların veya konukların erişime girmeden önce kullanım koşullarınızı kabul etmesini talep edin.
- Çalışanların veya konukların erişime girmeden önce her cihazdaki kullanım koşullarınızı kabul etmesini zorunlu kınlayın.
- Çalışanların veya konukların kullanım koşullarınızı yinelenen bir programda kabul etmesini talep edin.
- Azure Çok Faktörlü Kimlik Doğrulama'ya (MFA) güvenlik bilgilerini kaydetmeden önce çalışanların veya konukların kullanım koşullarınızı kabul etmesini talep edin.
- Azure AD self servis parola sıfırlama (SSPR) güvenlik bilgilerini kaydetmeden önce çalışanların kullanım koşullarınızı kabul etmesini zorunlu kılın.
- Kuruluşunuzdaki tüm kullanıcılar için genel kullanım koşullarını sunun.
- Bir kullanıcı özniteliklerine (ör. doktorlarla hemşirelere ya da yurtiçi ve uluslararası çalışanlara, [dinamik grupları](../users-groups-roles/groups-dynamic-membership.md) kullanarak) sunun.
- Salesforce gibi yüksek iş etkisi uygulamalarına erişirken belirli kullanım koşullarını sunun.
- Farklı dillerde kullanım koşulları sunar.
- Kullanım koşullarınızı kabul eden veya kabul etmeyenleri listele.
- Gizlilik yönetmeliklerinin karşılanmasında yardımcı olun.
- Uyumluluk ve denetim için kullanım koşullarının günlüklerini görüntüleyin.
- [Microsoft Graph API'lerini](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/agreement) kullanarak kullanım koşullarını oluşturun ve yönetin (şu anda önizlemede).

## <a name="prerequisites"></a>Ön koşullar

Azure AD kullanım koşullarını kullanmak ve yapılandırmak için şunları yapmış olmalısınız:

- Azure AD Premium P1, P2, EMS E3 veya EMS E5 aboneliği.
   - Bu aboneliklerden birine sahip değilseniz [Azure AD Premium'u alabilir](../fundamentals/active-directory-get-started-premium.md) veya [Azure AD Premium deneme sürümünü etkinleştirebilirsiniz](https://azure.microsoft.com/trial/get-started-active-directory/).
- Yapılandırmak istediğiniz dizin için aşağıdaki yönetici hesaplarından biri:
   - Genel Yönetici
   - Güvenlik Yöneticisi
   - Koşullu Erişim Yöneticisi

## <a name="terms-of-use-document"></a>Kullanım koşulları belgesi

Azure AD kullanım koşulları, içeriği sunmak için PDF biçimini kullanır. Bu PDF dosyası, kullanıcıların oturum açtığı sırada son kullanıcı sözleşmelerini toplamanıza olanak sağlayan herhangi bir içerik (örneğin mevcut sözleşme belgeleri) olabilir. Mobil cihazlardaki kullanıcıları desteklemek için PDF'de önerilen yazı tipi boyutu 24 noktadır.

## <a name="add-terms-of-use"></a>Kullanım koşulları ekleme

Kullanım koşulları belgenizi kesinleştirdikten sonra, eklemek için aşağıdaki yordamı kullanın.

1. Azure'da Global Administrator, Güvenlik Yöneticisi veya Koşullu Erişim Yöneticisi olarak oturum açın.
1. **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.

   ![Koşullu Erişim - Kullanım koşulları bıçak](./media/terms-of-use/tou-blade.png)

1. **Yeni koşullar**'a tıklayın.

   ![Kullanım koşulları ayarlarınızı belirtmek için yeni kullanım terimi bölmesi](./media/terms-of-use/new-tou.png)

1. **Ad** kutusuna, Azure portalında kullanılacak kullanım koşulları için bir ad girin.
1. Görüntü **ad** kutusuna, kullanıcıların oturum verirken gördükleri bir başlık girin.
1. **Kullanım Koşulları belgesi**için, kesinleşmiş kullanım koşullarıPDF'nize göz atın ve seçin.
1. Kullanım koşulları belgenizin dilini seçin. Dil seçeneğini kullanarak her biri farklı dilde olan birden fazla kullanım koşulunu karşıya yükleyebilirsiniz. Bir son kullanıcının göreceği kullanım koşulları sürümü, kullanıcının tarayıcı tercihlerine bağlıdır.
1. Son kullanıcıların kullanım koşullarını kabul etmeden önce görüntülemelerini sağlamak için, kullanıcıların kullanım koşullarını **A'ya** **genişletmelerini** gerektirin.
1. Son kullanıcıların erişdikleri her cihazda kullanım koşullarınızı kabul etmesini sağlamak için, **kullanıcıların her cihazda On'a onay vermelerini** gerektirin. **On** Bu seçenek etkinse, kullanıcıların ek uygulamalar yüklemeleri gerekebilir. Daha fazla bilgi için [cihaz başına kullanım koşullarına](#per-device-terms-of-use)bakın.
1. Bir zamanlamada kullanım izinlerinin süresinin dolmasını istiyorsanız, **Geçerli Lik onaylarını** **On**olarak ayarlayın. A'ya ayarlandığında, iki ek zamanlama ayarı görüntülenir.

   ![Başlangıç tarihini, sıklığını ve süresini ayarlamak için izin ayarlarının süresi nin dolması](./media/terms-of-use/expire-consents.png)

1. Kullanım sürelerinin sona **ermesinin** zamanlamasını belirtmek için Başlangıç Süresi ve **Sıklık** ayarlarını kullanın. Aşağıdaki tablo, birkaç örnek ayarı için sonucu gösterir:

   | Başlangıç tarihinden itibaren geçerlilik süresi | Frequency | Sonuç |
   | --- | --- | --- |
   | Bugünün tarihi  | Aylık | Bugünden itibaren, kullanıcılar kullanım koşullarını kabul etmeli ve her ay yeniden kabul etmelidir. |
   | Gelecekte tarih  | Aylık | Bugünden itibaren, kullanıcıların kullanım koşullarını kabul etmesi gerekir. Gelecek tarih oluştuğunda, izinlerin süresi dolacak ve kullanıcıların her ay yeniden kabul etmesi gerekir.  |

   Örneğin, geçerlilik süresi1 **Ocak'tan** başlayan tarihten itibaren ve sıklığı **Aylık**olarak ayarlarsanız, iki kullanıcı için son kullanma tarihleri nin nasıl gerçekleşebileceğini aşağıda bulabilirsiniz:

   | Kullanıcı | İlk kabul tarihi | İlk bitiş tarihi | İkinci bitiş tarihi | Üçüncü bitiş tarihi |
   | --- | --- | --- | --- | --- |
   | Alice | 1 Oca | 1 Şubat | Mar 1 | Nisan 1 |
   | Bob | 15 Oca | 1 Şubat | Mar 1 | Nisan 1 |

1. Kullanıcının kullanım koşullarını yeniden kabul etmesi için gün sayısını belirtmek için **yeniden kabul için (gün) ayar gerektirmeden önce Süre'yi** kullanın. Bu, kullanıcıların kendi zamanlamalarını izlemelerine olanak tanır. Örneğin, süreyi **30** güne ayarlarsanız, iki kullanıcı için son kullanma tarihleri şu şekilde oluşabilir:

   | Kullanıcı | İlk kabul tarihi | İlk bitiş tarihi | İkinci bitiş tarihi | Üçüncü bitiş tarihi |
   | --- | --- | --- | --- | --- |
   | Alice | 1 Oca | 31 Ocak | Mar 2 | Nisan 1 |
   | Bob | 15 Oca | Şub 14 | Mar 16 | 15 Nisan |

   **Yeniden kabul (gün)** ayarlarını birlikte gerektirmeden önce **Sona Kullanma Onayları** ve Süresi'ni kullanmak mümkündür, ancak genellikle birini veya diğerini kullanırsınız.

1. **Koşullu Erişim**altında, kullanım koşullarını uygulamak için şablonu seçmek için **Koşullu Erişim ilkesi yle Uygula şablon** listesini kullanın.

   ![İlke şablonu seçmek için Koşullu Erişim açılır listesi](./media/terms-of-use/conditional-access-templates.png)

   | Şablon | Açıklama |
   | --- | --- |
   | **Tüm konuklar için bulut uygulamalarına erişim** | Tüm konuklar ve tüm bulut uygulamaları için Koşullu Erişim ilkesi oluşturulur. Bu ilke Azure portalını etkiler. Bu oluşturulduktan sonra oturum açmanız ve oturum açmanız gerekebilir. |
   | **Tüm kullanıcılar için bulut uygulamalarına erişim** | Koşullu Erişim ilkesi tüm kullanıcılar ve tüm bulut uygulamaları için oluşturulur. Bu ilke Azure portalını etkiler. Bu oluşturulduktan sonra, oturum açmanız ve oturum açmanız gerekecektir. |
   | **Özel ilke** | Bu kullanım koşullarının uygulanacağı kullanıcıları, grupları ve uygulamaları seçin. |
   | **Koşullu Erişim ilkesini daha sonra oluşturma** | Bu kullanım koşulları, Koşullu Erişim ilkesi oluşturulurken hibe denetim listesinde görünür. |

   >[!IMPORTANT]
   >Koşullu Erişim ilkesi denetimleri (kullanım koşulları dahil) hizmet hesaplarında ki zorlamayı desteklemez. Koşullu Erişim ilkesindeki tüm hizmet hesaplarını hariç almanızı öneririz.

    Özel Koşullu Erişim ilkeleri, belirli bir bulut uygulamasına veya kullanıcı grubuna kadar parçalı kullanım koşullarını sağlar. Daha fazla bilgi için [Bkz. Quickstart: Bulut uygulamalarına erişmeden önce kullanım koşullarının kabul edilmesini zorunlu kullanabilirsiniz.](require-tou.md)

1. **Oluştur'u**tıklatın.

   Özel bir Koşullu Erişim şablonu seçtiyseniz, özel Koşullu Erişim ilkesini oluşturmanıza olanak tanıyan yeni bir ekran görüntülenir.

   ![Özel Koşullu Erişim ilkesi şablonunu seçtiyseniz Yeni Koşullu Erişim bölmesi](./media/terms-of-use/custom-policy.png)

   Şimdi yeni kullanım koşullarınızı görmelisiniz.

   ![Kullanım açısından listelenen yeni kullanım koşulları](./media/terms-of-use/create-tou.png)

## <a name="view-report-of-who-has-accepted-and-declined"></a>Kimin kabul edip reddettiği raporunu görüntüleme

Kullanım Koşulları dikey penceresinin kabul eden ve reddeden kullanıcı sayısını gösterdiğini fark edeceksiniz. Bu sayımlar ve kabul edilen/reddedilenler kullanım koşullarının ömrü boyunca saklanır.

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.

   ![Kullanıcı gösterisi sayısını listeleyen kullanım koşulları kabul edilmiş ve reddedilmiş](./media/terms-of-use/view-tou.png)

1. Kullanım koşulları için, kullanıcılar için geçerli durumu görüntülemek için **Kabul edilen** veya **Reddedilen** altındaki numaraları tıklatın.

   ![Kullanım koşulları, kabul eden kullanıcıları listeleyen onaylar bölmesi](./media/terms-of-use/accepted-tou.png)

1. Tek bir kullanıcının geçmişini görüntülemek için elipsis (**...**) seçeneğini tıklatın ve ardından **Geçmişi Görüntüleyin.**

   ![Bir kullanıcı için Geçmiş bağlam menüsünü görüntüleme](./media/terms-of-use/view-history-menu.png)

   Görünüm geçmişi bölmesinde, tüm kabullerin, reddetmelerin ve son kullanma tarihlerinin geçmişini görürsünüz.

   ![Geçmiş bölmesini görüntüle, bir kullanıcı için geçmiş kabul, reddetme ve son kullanma gün geçmişlerini listeler](./media/terms-of-use/view-history-pane.png)

## <a name="view-azure-ad-audit-logs"></a>Azure AD denetim günlüklerini görüntüleme

Ek etkinliği görüntülemek istiyorsanız, Azure REKLAM kullanım koşulları denetim günlüklerini içerir. Her kullanıcı onayı, denetim günlüklerinde **30 gün**boyunca depolanan bir olayı tetikler. Bu günlükleri portalda görüntüleyebilir veya .csv dosyası olarak indirebilirsiniz.

Azure AD denetim günlüklerine başlamak için aşağıdaki yordamı kullanın:

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.
1. Bir kullanım koşulları seçin.
1. **Denetim günlüklerini görüntüle'yi**tıklatın.

   ![Görünüm denetim günlükleri seçeneği vurgulanmış kullanım koşulları bıçak](./media/terms-of-use/audit-tou.png)

1. Azure AD denetim günlükleri ekranında, belirli denetim günlüğü bilgilerini hedeflemek için sağlanan listeleri kullanarak bilgilere filtre uygulayabilirsiniz.

   Ayrıca **İndir**'e tıklayarak bilgileri yerel olarak kullanmak üzere bir .csv dosyasında indirebilirsiniz.

   ![Azure AD denetim günlükleri ekran giriş tarihi, hedef ilkesi, tarafından başlatılan ve etkinlik](./media/terms-of-use/audit-logs-tou.png)

   Bir günlük tıklattığınızda, ek etkinlik ayrıntılarıyla birlikte bir bölme görüntülenir.

   ![Etkinlik gösteren bir günlük için etkinlik ayrıntıları, etkinlik durumu, tarafından başlatılan, hedef ilkesi](./media/terms-of-use/audit-log-activity-details.png)

## <a name="what-terms-of-use-looks-like-for-users"></a>Kullanıcılar için kullanım koşulları nasıl görünür?

Bir kullanım koşulları oluşturulduktan ve uygulandıktan sonra, kapsam da olan kullanıcılar oturum açma sırasında aşağıdaki ekranı görür.

![Bir kullanıcı imzaladığında ortaya çıkan örnek kullanım koşulları](./media/terms-of-use/user-tou.png)

Kullanıcılar kullanım koşullarını görüntüleyebilir ve gerekirse yakınlaştırmak ve uzaklaştırmak için düğmeleri kullanabilir.

![Yakınlaştırma düğmeleriyle kullanım koşullarının görünümü](./media/terms-of-use/zoom-buttons.png)

Aşağıdaki ekranda kullanım koşullarının mobil cihazlarda nasıl göründüğü gösterilmektedir.

![Bir kullanıcı mobil cihazda giriş yaptığında ortaya çıkan örnek kullanım koşulları](./media/terms-of-use/mobile-tou.png)

Kullanıcıların kullanım koşullarını yalnızca bir kez kabul etmeleri gerekir ve sonraki oturum açmalarda kullanım koşullarını bir daha görmezler.

### <a name="how-users-can-review-their-terms-of-use"></a>Kullanıcılar kullanım koşullarını nasıl gözden geçirebilirler?

Kullanıcılar aşağıdaki yordamı kullanarak kabul ettikleri kullanım koşullarını inceleyebilir ve görebilirler.

1. Oturum [https://myapps.microsoft.com](https://myapps.microsoft.com)aç.
1. Sağ üst köşede adınızı tıklatın ve **Profil'i**seçin.

   ![Kullanıcı bölmesi açık MyApps sitesi](./media/terms-of-use/tou14.png)

1. Profil sayfanızda **Kullanım koşullarını gözden geçir**'e tıklayın.

   ![Kullanım koşullarını gözden geçirme bağlantısını gösteren bir kullanıcının profil sayfası](./media/terms-of-use/tou13a.png)

1. Buradan, kabul ettiğiniz kullanım koşullarını gözden geçirebilirsiniz.

## <a name="edit-terms-of-use-details"></a>Kullanım koşulları ayrıntılarını edin

Kullanım koşullarının bazı ayrıntılarını değiştirebilirsiniz, ancak varolan bir belgeyi değiştiremezsiniz. Aşağıdaki yordam, ayrıntıların nasıl duygulanıldığını açıklar.

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.
1. Yeniden yapmak istediğiniz kullanım koşullarını seçin.
1. **Terimleri Edit'i**tıklatın.
1. Kullanım koşullarını edit bölmesinde, adı, görüntü adını değiştirin veya kullanıcıların değerleri genişletmesini zorunlu kılmaktadır.

   PDF belgesi gibi değiştirmek istediğiniz başka ayarlar varsa, kullanıcıların her cihazda onay vermesi, izinsüresinin sona ermesi, yeniden kabulden önceki süre veya Koşullu Erişim ilkesi varsa, yeni bir kullanım koşulları oluşturmanız gerekir.

   ![Adı gösteren kullanım bölmesini ve seçenekleri genişletme yi tıklatın](./media/terms-of-use/edit-tou.png)

1. Değişikliklerinizi kaydetmek için **Kaydet'i** tıklatın.

   Değişikliklerinizi kaydedince, kullanıcıların bu değişiklikleri yeniden kabul etmesi gerekmez.

## <a name="add-a-terms-of-use-language"></a>Kullanım koşulları dili ekleme

Aşağıdaki yordam, kullanım koşulları dilinin nasıl ekleyeceğini açıklar.

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.
1. Yeniden yapmak istediğiniz kullanım koşullarını seçin.
1. Ayrıntılar bölmesinde **Diller** sekmesini tıklatın.

   ![Seçili kullanım koşulları ve ayrıntılar bölmesinde Diller sekmesini gösterme](./media/terms-of-use/languages-tou.png)

1. **Dil ekle'yi**tıklatın.
1. Kullanım koşulları dil bölmesinde, yerelleştirilmiş PDF'nizi yükleyin ve dili seçin.

   ![Yerelleştirilmiş PDF'leri yükleme seçenekleriyle kullanım koşulları dil bölmesi ekleme](./media/terms-of-use/language-add-tou.png)

1. Dili eklemek için **Ekle'yi** tıklatın.

## <a name="per-device-terms-of-use"></a>Cihaz başına kullanım koşulları

**Kullanıcıların her cihaz ayarında onay almalarını** zorunlu kullanabilirsiniz, son kullanıcıların eriştisikleri her cihazda kullanım koşullarınızı kabul etmelerini gerektirmenize olanak tanır. Son kullanıcının cihazını Azure AD'ye kaydetmesi gerekir. Aygıt kaydedildiğinde, aygıt kimliği her aygıtın kullanım koşullarını uygulamak için kullanılır.

Burada desteklenen platformlar ve yazılım bir listesidir.

> [!div class="mx-tableFixed"]
> |  | iOS | Android | Windows 10 | Diğer |
> | --- | --- | --- | --- | --- |
> | **Yerel uygulama** | Evet | Evet | Evet |  |
> | **Microsoft Edge** | Evet | Evet | Evet |  |
> | **Internet Explorer** | Evet | Evet | Evet |  |
> | **Chrome (uzantılı)** | Evet | Evet | Evet |  |

Cihaz başına kullanım koşulları aşağıdaki kısıtlamalara sahiptir:

- Bir aygıt yalnızca bir kiracıya katılabilir.
- Bir kullanıcının cihazına katılmak için izinleri olmalıdır.
- Intune Kayıt uygulaması desteklenmez.
- Azure AD B2B kullanıcıları desteklenmez.

Kullanıcının aygıtı birleştirilmezse, aygıtlarına katılmaları için gereken bir ileti alır. Onların deneyimi platform ve yazılım bağlı olacaktır.

### <a name="join-a-windows-10-device"></a>Bir Windows 10 cihazını ekleme

Bir kullanıcı Windows 10 ve Microsoft Edge kullanıyorsa, [aygıtlarına katılmak](../user-help/user-help-join-device-on-network.md#to-join-an-already-configured-windows-10-device)için aşağıdakilere benzer bir ileti alır.

![Windows 10 ve Microsoft Edge - Cihazınızın kayıtlı olması gerektiğini belirten ileti](./media/terms-of-use/per-device-win10-edge.png)

Chrome kullanıyorlarsa, [Windows 10 Hesapları uzantısını](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji)yüklemeleri istenir.

### <a name="register-an-ios-device"></a>bir iOS aygıtı kaydetme

Bir kullanıcı bir iOS aygıtı kullanıyorsa, Microsoft [Authenticator uygulamasını](https://apps.apple.com/us/app/microsoft-authenticator/id983156458)yüklemeleri istenir.

### <a name="register-an-android-device"></a>Android cihazı kaydetme

Bir kullanıcı Android cihazı kullanıyorsa, [Microsoft Authenticator uygulamasını](https://play.google.com/store/apps/details?id=com.azure.authenticator)yüklemeleri istenir.

### <a name="browsers"></a>Browsers (Tarayıcılar)

Bir kullanıcı desteklenmeyen tarayıcı kullanıyorsa, farklı bir tarayıcı kullanması istenir.

![Cihazınızın kaydedildiğini belirten ileti, ancak tarayıcı desteklenmiyor](./media/terms-of-use/per-device-browser-unsupported.png)

## <a name="delete-terms-of-use"></a>Kullanım koşullarını silme

Aşağıdaki yordamı kullanarak eski kullanım koşullarını silebilirsiniz.

1. Azure'da oturum açın ve **Kullanım Koşulları**'na erişmek için [https://aka.ms/catou](https://aka.ms/catou) sayfasına gidin.
1. Kaldırmak istediğiniz kullanım koşullarını seçin.
1. **Koşulları sil**'e tıklayın.
1. Devam etmek isteyip istemediğinizi soran iletide **Evet**'e tıklayın.

   ![Kullanım koşullarını silmek için onay isteyen ileti](./media/terms-of-use/delete-tou.png)

   Artık kullanım koşullarınızı görmemelisiniz.

## <a name="deleted-users-and-active-terms-of-use"></a>Silinen kullanıcılar ve etkin kullanım koşulları

Varsayılan olarak, silinmiş bir kullanıcı Azure AD'de 30 gün boyunca silinmiş durumda kalır ve bu süre boyunca gerekirse bir yönetici tarafından geri alınabilir. 30 gün sonra bu kullanıcı kalıcı olarak silinir. Ayrıca, bir Genel Yönetici bu süreye ulaşılmadan önce Azure Active Directory portalını kullanarak [kısa süre önce silinmiş bir kullanıcıyı kalıcı olarak silebilir](../fundamentals/active-directory-users-restore.md). Bir kullanıcı kalıcı olarak silinmişse, bu kullanıcı yla ilgili sonraki veriler etkin kullanım terimlerinden kaldırılır. Silinmiş kullanıcılara ilişkin denetim bilgileri, denetim günlüğünde kalır.

## <a name="policy-changes"></a>İlke değişiklikleri

Koşullu Erişim ilkeleri hemen yürürlüğe girer. Bu durumda, yönetici "üzgün bulutlar" veya "Azure AD belirteç sorunları" görmeye başlar. Yöneticinin yeni ilkeyi karşılamak için oturum açmaları ve yeniden oturum açmaları gerekir.

> [!IMPORTANT]
> Aşağıdaki durumlarda kapsam dahilindeki kullanıcıların yeni bir ilkeyi karşılamak için oturumu kapatıp yeniden oturum açmaları gerekir:
>
> - koşullu erişim ilkesi kullanım koşullarında etkinleştirilir
> - veya ikinci bir kullanım koşulları belgesi oluşturulduğunda

## <a name="b2b-guests-preview"></a>B2B konukları (Önizleme)

Çoğu kuruluş, çalışanlarının kuruluşlarının kullanım koşullarını ve gizlilik bildirimlerini kabul etmeleri için bir işlem le sonuçlamışlardır. Ancak, SharePoint veya Teams aracılığıyla eklendiğinde Azure AD'dan işletmeye (B2B) misafirler için aynı izinleri nasıl uygulayabilirsiniz? Koşullu Erişim ve kullanım koşullarını kullanarak, doğrudan B2B konuk kullanıcılarına yönelik bir ilke uygulayabilirsiniz. Davet kullanım akışı sırasında, kullanıcı kullanım koşulları ile sunulur. Bu destek şu anda önizlemededir.

Kullanım koşulları yalnızca kullanıcının Azure AD'de bir konuk hesabı olduğunda görüntülenir. SharePoint Online şu anda bir belgeyi veya kullanıcının konuk hesabı olmasını gerektirmeyen bir klasörü paylaşmak için geçici bir [dış paylaşım alıcısı deneyimine](/sharepoint/what-s-new-in-sharing-in-targeted-release) sahiptir. Bu durumda, kullanım koşulları görüntülenmez.

![Kullanıcılar ve gruplar bölmesi - Tüm konuk kullanıcılar seçeneği işaretli sekme ekle](./media/terms-of-use/b2b-guests.png)

## <a name="support-for-cloud-apps-preview"></a>Bulut uygulamaları desteği (Önizleme)

Kullanım koşulları, Azure Bilgi Koruması ve Microsoft Intune gibi farklı bulut uygulamaları için kullanılabilir. Bu destek şu anda önizlemededir.

### <a name="azure-information-protection"></a>Azure Information Protection

Azure Bilgi Koruması uygulaması için Koşullu Erişim ilkesini yapılandırabilir ve kullanıcı korumalı bir belgeye eriştığında kullanım koşulları talep edebilirsiniz. Bu, bir kullanıcının korumalı bir belgeye ilk kez erişmeden önce bir kullanım koşullarını tetikler.

![Microsoft Azure Bilgi Koruması uygulaması yla bulut uygulamaları bölmesi seçili](./media/terms-of-use/cloud-app-info-protection.png)

### <a name="microsoft-intune-enrollment"></a>Microsoft Intune Kaydı

Microsoft Intune Kayıt uygulaması için koşullu erişim ilkesini yapılandırabilir ve Intune'a bir aygıtın kaydedilmesinden önce kullanım koşulları talep edebilirsiniz. Daha fazla bilgi için, [kuruluşunuzun blog gönderisi için doğru Terimler çözümlerini seçme yi](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409)okuyun' a bakın.

![Microsoft Intune uygulaması seçili bulut uygulamaları bölmesi](./media/terms-of-use/cloud-app-intune.png)

> [!NOTE]
> Intune Kayıt [uygulaması, cihaz başına kullanım koşulları](#per-device-terms-of-use)için desteklenmez.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S: Kullanıcının kullanım koşullarını kabul edip etmediğini veya ne zaman kabul ettiğini nasıl görebilirim?**<br />
C: Kullanım koşulları bıçak, **Kabul**altında numarasını tıklatın. Azure AD denetim günlüklerinde kabul eden etkinliği görüntüleyebilir veya arama yapabilirsiniz. Daha fazla bilgi için bkz. [View Azure AD audit logs](#view-azure-ad-audit-logs)

**S: Bilgiler ne kadar süreyle depolanır?**<br />
C: Kullanıcı kullanım raporunda sayılır ve kabul edilen/reddedilenler kullanım koşullarının ömrü boyunca saklanır. Azure AD denetim günlükleri 30 gün boyunca saklanır.

**S: Azure REKLAM denetim günlüklerine karşı kullanım raporunda neden farklı sayıda onay görüyorum?**<br />
C: Kullanım koşulları raporu bu kullanım koşullarının kullanım ömrü boyunca saklanırken, Azure REKLAM denetim günlükleri 30 gün boyunca saklanır. Ayrıca, kullanım koşulları raporu yalnızca kullanıcıların geçerli onay durumunu görüntüler. Örneğin, bir kullanıcı reddeder ve sonra kabul ederse, kullanım koşulları raporu yalnızca kullanıcının kabul ettiğini gösterir. Geçmişi görmeniz gerekiyorsa, Azure AD denetim günlüklerini kullanabilirsiniz.

**S: Kullanım koşullarının ayrıntılarını edinirsem, kullanıcıların tekrar kabul etmesini gerektirir mi?**<br />
C: Hayır, bir yönetici kullanım koşullarının ayrıntılarını (ad, görüntü adı, kullanıcıların genişlemesini veya bir dil eklemesini gerektiriyorsa), kullanıcıların yeni terimleri yeniden kabul etmesini gerektirmez.

**S: Varolan bir kullanım koşulları belgesini güncelleyebilir miyim?**<br />
C: Şu anda varolan bir kullanım koşullarını belgeyi güncelleştiremezsiniz. Kullanım koşulları belgesini değiştirmek için, yeni bir kullanım koşulları örneği oluşturmanız gerekir.

**S: Köprüler PDF belgesinin kullanım koşullarına uygunsa, son kullanıcılar bunları tıklatabilecek mi?**<br />
C: Evet, son kullanıcılar ek sayfalara köprüler seçebilir, ancak belge içindeki bölümlere bağlantılar desteklenmez.

**S: Bir kullanım koşulları belgesi birden çok dili destekleyebilir mi?**<br />
C: Evet. Şu anda bir yöneticinin tek bir kullanım koşulları için yapılandırabileceği 108 farklı dil vardır. Yönetici birden çok PDF belgesi yükleyebilir ve bu belgeleri karşılık gelen bir dille etiketleyebilir (en fazla 108). Son kullanıcılar oturum açtığında, tarayıcı dil tercihlerine bakar ve eşleşen belgeyi görüntüleriz. Eşleşme yoksa, yüklenen ilk belge olan varsayılan belgeyi görüntüleriz.

**S: Kullanım koşulları ne zaman tetiklenir?**<br />
C: Kullanım koşulları oturum açma deneyimi sırasında tetiklenir.

**S: Kullanım koşullarını hangi uygulamalara hedefleyebilirim?**<br />
C: Modern kimlik doğrulamasını kullanarak kurumsal uygulamalarda Koşullu Erişim ilkesi oluşturabilirsiniz. Daha fazla bilgi için bkz. [Kurumsal uygulamalar](./../manage-apps/view-applications-portal.md).

**S: Belirli bir kullanıcı veya uygulamaya birden çok kullanım koşulları belgesi ekleyebilir miyim?**<br />
C: Evet, bu grupları veya uygulamaları hedefleyen birden çok Koşullu Erişim ilkeleri oluşturarak. Bir kullanıcı birden çok kullanım süresi kapsamına düşerse, aynı anda bir kullanım koşullarını kabul ederler.

**S: Bir kullanıcı kullanım koşullarını reddederse ne olur?**<br />
C: Kullanıcının uygulamaya erişimi engellenir. Kullanıcının erişim için yeniden oturum açmaları ve şartları kabul etmesi gerekir.

**S: Daha önce kabul edilmiş bir kullanım koşullarını kabul etmek mümkün mü?**<br />
C: [Daha önce kabul edilen kullanım koşullarını gözden geçirebilirsiniz,](#how-users-can-review-their-terms-of-use)ancak şu anda kabul etmenin bir yolu yoktur.

**S: Intune hüküm ve koşullarını da kullanırsam ne olur?**<br />
C: Hem Azure REKLAM kullanım koşullarını hem de [Intune hüküm ve koşullarını](/intune/terms-and-conditions-create)yapılandırmışsanız, kullanıcının her ikisini de kabul etmesi gerekir. Daha fazla bilgi için, [kuruluşunuz blog gönderisi için doğru Terimler çözümlerini seçme'ye](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409)bakın.

**S: Kullanım hizmeti koşulları kimlik doğrulama için hangi uç noktaları kullanır?**<br />
C: Kullanım koşulları kimlik doğrulaması için aşağıdaki https://tokenprovider.termsofuse.identitygovernance.azure.com https://account.activedirectory.windowsazure.comuç noktaları kullanır: ve . Kuruluşunuzun kayıt için URL'lere izin veren bir listesi varsa, oturum açma için Azure AD bitiş noktalarıyla birlikte bu uç noktaları izin listenize eklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı başlatma: Bulut uygulamalarına erişmeden önce kullanım koşullarının kabul edilmesini zorunlu kılmasını zorunlu kılması](require-tou.md)
- [Azure Etkin Dizini'nde Koşullu Erişim için en iyi uygulamalar](best-practices.md)
