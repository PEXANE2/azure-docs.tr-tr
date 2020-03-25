---
title: Azure kaydı ile ilgili sorunları giderme
description: Microsoft Azure portalı hesap merkezinde yeni hesaba kaydolmaya çalışırken oluşan bir sorunu çözümleme.
author: v-miegge
ms.reviewer: dcscontentpm
tags: billing
ms.service: cost-management-billing
ms.topic: troubleshooting
ms.date: 02/12/2020
ms.author: v-miegge
ms.openlocfilehash: 04e7e3e2818968cd14cd5caf606155171a25e5e9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79202837"
---
# <a name="troubleshoot-azure-sign-up"></a>Azure kaydı ile ilgili sorunları giderme

Microsoft Azure portalında veya Azure hesap merkezinde yeni bir hesaba kaydolmaya çalıştığınızda bir sorunla karşılaşabilirsiniz. Sorunu gidermeden önce aşağıdakileri doğrulayın:

- Azure hesap profiliniz için sağladığınız bilgilerin (iletişim e-posta adresi, sokak adresi ve telefon numarası gibi) doğru olduğunu.
- Kredi kartı bilgilerinin doğru olduğunu.
- Önceden aynı bilgilere sahip bir Microsoft hesabınız olmadığını.

## <a name="resolutions"></a>Çözümler

Hataları çözmek için, Azure’a kaydolmaya çalışırken karşılaştığınız sorunu seçin.

### <a name="error-we-cannot-proceed-with-sign-up-due-to-an-issue-with-your-account-please-contact-billing-support"></a>Hata: *Hesabınızla ilgili bir sorun nedeniyle kayıt işlemine devam edilemiyor. Lütfen faturalama desteğine başvurun.*

Sorunu çözmek için şu adımları izleyin:

1. Hesap yöneticisi kimlik bilgilerini kullanarak [Azure hesap merkezinde](https://account.azure.com/Profile) oturum açın.

2. **Ayrıntıları düzenle** seçeneğini belirleyin.

3. Tüm adres alanlarının doldurulduğundan ve geçerli olduğundan emin olun.

4. Azure aboneliğine kaydolduğunuzda, kredi kartı kaydı ile ilişkili fatura adresinin, banka kayıtlarınızla aynı olduğundan emin olun.

Hata iletisini almaya devam ederseniz, farklı bir tarayıcı kullanarak kaydolmayı deneyin.

### <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>İlerleme çubuğu, *Kartla kimlik doğrulama* bölümüne takılı kalıyor.

Kartla kimlik doğrulamayı tamamlamak için tarayıcınız için üçüncü taraf tanımlama bilgilerine izin verilmelidir.

![Kartla kimlik doğrulama](./media/troubleshoot-azure-sign-up/identify-verification-by-card.png)

Tarayıcınızın tanımlama bilgisi ayarlarını güncelleştirmek için aşağıdaki adımları kullanın.

1. Chrome kullanıyorsanız **Ayarlar** > **Gelişmiş ayarları göster** > **Gizlilik** > **İçerik ayarları** seçeneklerini belirleyin. **Üçüncü taraf tanımlama bilgilerini ve site verilerini engelle** seçeneğinin işaretini kaldırın.

2. Microsoft Edge kullanıyorsanız **Ayarlar** > **Gelişmiş ayarları göster** > **Tanımlama Bilgileri** > **Tanımlama bilgilerini engelleme** seçeneklerini belirleyin.

3. Azure kaydolma sayfasını yenileyin ve sorunun çözümlenip çözümlenmediğini denetleyin.

4. Yenileme sonucunda sorun çözülmediyse, tarayıcıdan çıkıp tarayıcıyı yeniden başlatın ve tekrar deneyin.

### <a name="credit-card-form-doesnt-support-my-billing-address"></a>Kredi kartı formu, fatura adresimi desteklemiyor

Fatura adresiniz, **Hakkınızda** bölümünde seçtiğiniz ülkede bulunmalıdır. Doğru ülkeyi seçtiğinizden emin olun.

### <a name="no-text-messages-or-calls-during-sign-up-account-verification"></a>Kayıt hesabı doğrulaması sırasında SMS mesajı veya arama yok

İşlem genellikle hızlı olsa da, doğrulama kodunun teslim edilmesi dört dakika kadar sürebilir. Doğrulama için girdiğiniz telefon numarası, hesap için ilgili kişi numarası olarak depolanmaz.

Burada birkaç ek ipucu bulabilirsiniz:

- IP üzerinden ses (VoiP) telefon numarası, telefon doğrulama işlemi için kullanılamaz.
- Açılan menüde seçtiğiniz ülke kodu da dahil olmak üzere girdiğiniz telefon numarasını iki kez kontrol edin.
- Telefonunuza SMS mesajı gelmezse **Beni ara** seçeneğini deneyin.
- Telefonunuzun Birleşik Devletler’de bulunan bir telefon numarasından arama veya SMS mesajı alabildiğinden emin olun.

SMS mesajını veya telefon aramasını aldığınızda, gelen kodu metin kutusuna girin.

### <a name="credit-card-declined-or-not-accepted"></a>Kredi kartı reddedildi veya kabul edilmedi

Azure abonelikleri için sanal veya ön ödemeli kredi kartı ya da ATM kartı desteklenmez. Kartınızın reddedilmesine başka nelerin neden olabileceğini görmek için, ATM kartınız veya kredi kartınız [Azure kaydında reddedildi](https://support.microsoft.com/help/4042960) bölümüne bakın.

### <a name="free-trial-is-not-available"></a>Ücretsiz Deneme mevcut değildir

Geçmişte bir Azure aboneliği kullandınız mı? Azure Kullanım Koşulları sözleşmesi ücretsiz deneme etkinleştirmesini yalnızca Azure'daki yeni bir kullanıcı için sınırlar. Başka türde bir Azure aboneliğiniz varsa, ücretsiz denemeyi etkinleştiremezsiniz. [Kullandıkça Öde aboneliğine](https://azure.microsoft.com/offers/ms-azr-0003p/) kaydolmayı deneyin.

### <a name="i-saw-a-charge-on-my-free-trial-account"></a>Ücretsiz Deneme hesabımda ücret uygulandığını gördüm

Kaydolduktan sonra kredi kartı hesabınızda küçük bir doğrulama blokajı olduğunu görebilirsiniz. Bu, üç ila beş gün içinde kaldırılır. Maliyetleri yönetme konusunda endişeleriniz varsa [beklenmeyen maliyetleri önleme](getting-started.md) ile ilgili daha fazla bilgi edinin.

### <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>MSDN, BizSpark, BizSparkPlus veya MPN gibi Azure avantaj planı etkinleştirilemiyor

Doğru oturum açma kimlik bilgilerini kullandığınızdan emin olun. Ardından, uygun olduğunuzdan emin olmak için avantaj programını kontrol edin.

- MSDN
  - [MSDN hesabı sayfanızda](https://msdn.microsoft.com/subscriptions/manage/default.aspx) uygunluk durumunuzu doğrulayın.
  - Durumunuzu doğrulayamıyorsanız [MSDN Abonelikleri Müşteri Hizmetleri Merkezleri](https://msdn.microsoft.com/library/aa493452.aspx) ile iletişim kurun.
- Startup’lar için Microsoft
  - Startup’lar için Microsoft’a ilişkin uygunluk durumunuzu doğrulamak için [Startup’lar için Microsoft portalına](https://startups.microsoft.com/#start-two) kaydolun.
  - Durumunuzu doğrulayamazsanız, [Startup’lar için Microsoft forumlarından](https://www.microsoftpartnercommunity.com/t5/Microsoft-for-Startups/ct-p/Microsoft_Startups) yardım alabilirsiniz.
- MPN
  - Uygunluk durumunuzu doğrulamak için [MPN portalında](https://mspartner.microsoft.com/Pages/Locale.aspx) oturum açın. Uygun [Bulut Platformu Yetkinliklerine](https://mspartner.microsoft.com/pages/membership/cloud-platform-competency.aspx) sahipseniz, ek avantajlar için uygun olabilirsiniz.
  - Durumunuzu doğrulayamazsanız [MPN Desteği](https://mspartner.microsoft.com/Pages/Support/Premium/contact-support.aspx)’ne başvurun.

### <a name="cant-activate-new-azure-in-open-subscription"></a>Yeni Open ile Azure aboneliği etkinleştirilemiyor

Open ile Azure aboneliği oluşturmak için, kendisiyle ilişkilendirilmiş en az bir Open ile Azure belirteci olan geçerli bir Çevrimiçi Hizmet Etkinleştirmesi (OSA) anahtarınız olmalıdır. OSA anahtarınız yoksa, [Microsoft Pinpoint](https://pinpoint.microsoft.com/)’te listelenen Microsoft İş Ortaklarından biriyle iletişim kurun.

### <a name="error-you-are-not-eligible-for-an-azure-subscription"></a>Hata: *Azure aboneliği için uygun değilsiniz*

Bu sorunu çözmek için, aşağıdaki öğelerin doğru olup olmadığını iki kez kontrol edin:

- Azure hesap profiliniz için sağladığınız bilgilerin (iletişim e-posta adresi, sokak adresi ve telefon numarası gibi) doğru olduğunu.
- Kredi kartı bilgilerinin doğru olduğunu.
- Önceden aynı bilgileri kullanan bir Microsoft hesabınız olmadığını.

### <a name="error-your-current-account-type-is-not-supported"></a>Hata: *Geçerli hesap türünüz desteklenmiyor*

Hesap bir [yönetilmeyen Azure AD dizininde](../../active-directory/users-groups-roles/directory-self-service-signup.md) kayıtlıysa ve kuruluşunuzun Azure AD dizininde değilse bu sorun oluşabilir.

Bu sorunu çözmek için, başka bir hesap kullanarak Azure hesabına kaydolun veya yönetilmeyen AD dizinini devralın. Daha fazla bilgi için bkz. [Azure Active Directory’de yönetilmeyen bir dizini yönetici olarak devralma](../../active-directory/users-groups-roles/domains-admin-takeover.md).

## <a name="additional-help-resources"></a>Ek yardım kaynakları

Azure Faturalama ve Abonelikler için diğer sorun giderme makaleleri

- [Reddedilen kart](troubleshoot-declined-card.md)
- [Abonelik oturum açma sorunları](troubleshoot-sign-in-issue.md)
- [Abonelik bulunamadı](no-subscriptions-found.md)
- [Kurumsal maliyet görünümü devre dışı](enterprise-mgmt-grp-troubleshoot-cost-view.md)

## <a name="contact-us-for-help"></a>Yardım için bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Faturalama belgeleri](../../billing/index.md)
