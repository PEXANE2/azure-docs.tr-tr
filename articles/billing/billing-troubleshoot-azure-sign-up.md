---
title: Azure kaydolma sorunlarını giderme
description: Microsoft Azure portal hesap merkezinde yeni bir hesap için kaydolmaya çalışırken bir sorun giderme.
author: v-miegge
manager: na
editor: v-jesits
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/12/2019
ms.author: v-miegge
ms.openlocfilehash: 57dc9c05778563bc0c5df1a8ac8895b51008b01f
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69657079"
---
# <a name="troubleshoot-azure-sign-up"></a>Azure kaydolma sorunlarını giderme

Microsoft Azure portal veya Azure Hesap Merkezi 'nde yeni bir hesaba kaydolmaya çalıştığınızda bir sorunla karşılaşabilirsiniz. Sorunu giderebilmeniz için önce aşağıdakileri doğrulayın:

- Azure hesabı profiliniz için verdiğiniz bilgiler (iletişim e-posta adresi, sokak adresi ve telefon numarası dahil) doğru.
- Kredi kartı bilgileri doğru.
- Aynı bilgilere sahip bir Microsoft hesabı zaten yok.

## <a name="resolutions"></a>Sunar

Hataları gidermek için, Azure 'a kaydolmaya çalıştığınızda karşılaştığınız sorunu seçin.

### <a name="error-we-cannot-proceed-with-sign-up-due-to-an-issue-with-your-account-please-contact-billing-support"></a>Hata: *Hesabınızla ilgili bir sorun nedeniyle kayıt işlemine devam edemezsiniz. Lütfen Faturalandırma desteğiyle görüşün.*

Sorunu çözmek için aşağıdaki adımları izleyin:

1. Hesap Yöneticisi kimlik bilgilerini kullanarak [Azure Hesap Merkezi](https://account.azure.com/Profile) ' nde oturum açın.

2. **Ayrıntıları Düzenle**' yi seçin.

3. Tüm adres alanlarının tamamlandığından ve geçerli olduğundan emin olun.

4. Azure aboneliğine kaydolduğunuzda, kredi kartı kaydıyla ilgili faturalandırma adresinin banka kayıtlarıyla eşleştiğinden emin olun.

Hata iletisini almaya devam ederseniz, farklı bir tarayıcı kullanarak kaydolmayı deneyin.

### <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>İlerleme çubuğu, *karta göre kimlik doğrulama* bölümüne askıda kalıyor.

Karta göre kimlik doğrulamasını gerçekleştirmek için, tarayıcınız için üçüncü taraf tanımlama bilgilerine izin verilmelidir.

![Kartla kimlik doğrulama](./media/billing-troubleshoot-azure-sign-up/identify-verification-by-card.png)
 
Tarayıcınızın tanımlama bilgisi ayarlarını güncelleştirmek için aşağıdaki adımları kullanın.

1. Chrome kullanıyorsanız **Ayarlar** > **Gelişmiş ayarlar** > Gizlilikiçeriği > **ayarlarını**göster ' i seçin. **Üçüncü taraf tanımlama bilgilerini ve site verilerini engelle**seçimini kaldırın.

2. Microsoft Edge kullanıyorsanız, **Ayarlar** > **Gelişmiş ayarlar** >  > görünümü tanımlama bilgilerini**Engelle**' yi seçin.

3. Azure kaydolma sayfasını yenileyin ve sorunun çözümlenip çözümlenmediğini denetleyin.

4. Yenileme sorunu çözmezse, tarayıcıyı kapatın ve yeniden başlatın ve sonra yeniden deneyin.

### <a name="credit-card-form-doesnt-support-my-billing-address"></a>Kredi kartı formu faturalandırma adresimi desteklemiyor

Fatura adresiniz **hakkında** bölümünde seçtiğiniz ülkede yer almalıdır. Doğru ülkeyi seçtiğinizden emin olun.

### <a name="no-text-messages-or-calls-during-sign-up-account-verification"></a>Kaydolma hesabı doğrulaması sırasında metin iletisi veya çağrısı yok

İşlem genellikle hızlı olsa da, doğrulama kodunun teslim edilmesi dört dakika kadar sürebilir. Doğrulama için girdiğiniz telefon numarası hesap için bir kişi numarası olarak depolanmaz.

Ek ipuçları aşağıda verilmiştir:

- Telefon doğrulama işlemi için bir IP üzerinden ses (VoIP) telefon numarası kullanılamaz.
- Açılan menüde seçtiğiniz ülke kodu dahil olmak üzere girdiğiniz telefon numarasını çift işaretleyin.
- Telefonunuz metin iletileri (SMS) almazsa **beni çağır** seçeneğini deneyin.
- Telefonunuzun Birleşik Devletler tabanlı bir telefon numarasından çağrı veya SMS iletileri alabilmesi için emin olun.

Metin iletisini veya telefon aramasını aldığınızda, metin kutusuna aldığınız kodu girin.

### <a name="credit-card-declined-or-not-accepted"></a>Kredi kartı reddedildi veya kabul edilmedi

Sanal veya ön ödemeli kredi veya banka kartları, Azure abonelikleri için ödeme olarak kabul edilmez. Kartınızın reddediyor olabileceği diğer şeyleri görmek için, [Azure kayıt sırasında](https://support.microsoft.com/help/4042960)banka kartınız veya kredi kartınız reddedildi.

### <a name="free-trial-is-not-available"></a>Ücretsiz deneme yok

Geçmişte bir Azure aboneliği kullandınız mı? Azure Kullanım Koşulları sözleşmesi ücretsiz deneme etkinleştirmesini yalnızca Azure'daki yeni bir kullanıcı için sınırlar. Başka türde bir Azure aboneliğiniz varsa, ücretsiz denemeyi etkinleştiremezsiniz. [Kullandıkça Öde aboneliğine](https://azure.microsoft.com/offers/ms-azr-0003p/)kaydolmasını göz önünde bulundurun.

### <a name="i-saw-a-charge-on-my-free-trial-account"></a>Ücretsiz deneme hesabımın üzerinde bir ücret görmem

Kaydolduktan sonra kredi kartı hesabınızda küçük bir doğrulama tutmayı görebilirsiniz. Bu, üç ila beş gün içinde kaldırılır. Maliyetleri yönetme hakkında endişeli değilseniz, [beklenmeyen maliyetleri önlemek](billing-getting-started.md)için daha fazla bilgi edinin.

### <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>MSDN, BizSpark, Bizmini Plus veya MPN gibi Azure avantaj planı etkinleştirilemiyor

Doğru oturum açma kimlik bilgilerini kullandığınızdan emin olun. Ardından, uygun olduğunuzdan emin olmak için avantaj programını kontrol edin.

- MSDN 
  - [MSDN hesabı sayfanızda](https://msdn.microsoft.com/subscriptions/manage/default.aspx)uygunluk durumunuzu doğrulayın.
  - Durumunuzu doğrulayamıyorum [MSDN abonelikleri müşteri hizmetleri merkezleriyle](https://msdn.microsoft.com/library/aa493452.aspx)iletişim kurun.
- Startup’lar için Microsoft
  - Başlangıç için Microsoft 'un uygunluk durumunuzu doğrulamak üzere [Startups portalında](https://startups.microsoft.com/#start-two) oturum açın.
  - Durumunuzu doğrulayamıyorum, [Microsoft 'un Startups forumları](https://www.microsoftpartnercommunity.com/t5/Microsoft-for-Startups/ct-p/Microsoft_Startups)hakkında yardım alabilirsiniz.
- MPN 
  - Uygunluk durumunuzu doğrulamak için [MPN portalında](https://mspartner.microsoft.com/Pages/Locale.aspx) oturum açın. Uygun [bulut platformu uzmanlarınız](https://mspartner.microsoft.com/pages/membership/cloud-platform-competency.aspx)varsa, daha fazla avantaj elde edebilirsiniz.
  - Durumunuzu doğrulayamıyorum [MPN desteğine](https://mspartner.microsoft.com/Pages/Support/Premium/contact-support.aspx)başvurun.

### <a name="cant-activate-new-azure-in-open-subscription"></a>Açık abonelikte yeni Azure etkinleştirilemiyor

Açık abonelikte bir Azure oluşturmak için, kendisiyle ilişkili açık belirteçte en az bir Azure olan geçerli bir çevrimiçi hizmet etkinleştirme (OSA) anahtarınız olmalıdır. OSA anahtarınız yoksa, [Microsoft Pinpoint](http://pinpoint.microsoft.com/)' te listelenen Microsoft iş ortaklarından biriyle iletişime geçin.

### <a name="error-you-are-not-eligible-for-an-azure-subscription"></a>Hata: *Azure aboneliği için uygun değilsiniz*

Bu sorunu çözmek için, aşağıdaki öğelerin doğru olup olmadığını iki kez kontrol edin:

- Azure hesabı profiliniz için verdiğiniz bilgiler (iletişim e-posta adresi, sokak adresi ve telefon numarası dahil) doğru.
- Kredi kartı bilgileri doğru.
- Aynı bilgileri kullanan bir Microsoft hesabı zaten yok.

### <a name="error-your-current-account-type-is-not-supported"></a>Hata: *Geçerli hesap türü desteklenmiyor*

Bu sorun, hesap [yönetilmeyen bir Azure AD dizinine](../active-directory/users-groups-roles/directory-self-service-signup.md)kayıtlıysa ve KURULUŞUNUZUN Azure AD dizininde değilse meydana gelebilir. 

Bu sorunu çözmek için, başka bir hesap kullanarak Azure hesabını kaydolun veya yönetilmeyen AD dizinini alın. Daha fazla bilgi için, bkz. [Azure Active Directory yönetilmeyen bir dizini yönetici olarak alma](../active-directory/users-groups-roles/domains-admin-takeover.md).
 
## <a name="additional-help-resources"></a>Ek Yardım kaynakları

Azure Faturalandırma ve abonelikleriyle ilgili diğer sorun giderme makaleleri

- [Reddedilen kart](billing-troubleshoot-declined-card.md)
- [Abonelik oturum açma sorunları](billing-troubleshoot-sign-in-issue.md)
- [Abonelik bulunamadı](billing-no-subscriptions-found.md)
- [Kurumsal maliyet görünümü devre dışı](billing-enterprise-mgmt-grp-troubleshoot-cost-view.md)

## <a name="contact-us-for-help"></a>Yardım için bizimle iletişime geçin

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure faturalama belgeleri](index.md)
