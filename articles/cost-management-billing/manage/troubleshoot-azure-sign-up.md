---
title: Azure portalında veya Azure hesap merkezinde yeni bir hesap için kaydolduğunuzda oluşan sorunları giderme
description: Microsoft Azure portalı hesap merkezinde yeni hesaba kaydolmaya çalışırken oluşan bir sorunu çözümleme.
services: cost-management-billing
author: v-miegge
manager: dcscontentpm
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: v-miegge
ms.openlocfilehash: 2f2d690cd22cbd37af46c89b3bd19492ae9034b4
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84344124"
---
# <a name="troubleshoot-issues-when-you-sign-up-for-a-new-account-in-azure-portal-or-azure-account-center"></a>Azure portalında veya Azure hesap merkezinde yeni bir hesap için kaydolduğunuzda oluşan sorunları giderme

Microsoft Azure portalında veya Azure hesap merkezinde yeni bir hesaba kaydolmaya çalıştığınızda bir sorunla karşılaşabilirsiniz. Bu kısa kılavuz size, kaydolma aşaması ve her adımdaki yaygın sorunlar ile ilgili izlenecek yolu gösterir.

> [!NOTE]
> Zaten bir hesabınız varsa ve oturum açma sorunlarını gidermeye yönelik rehberlik arıyorsanız bkz. [Azure aboneliği oturum açma sorunlarını giderme](https://docs.microsoft.com/azure/cost-management-billing/manage/troubleshoot-sign-in-issue).

## <a name="before-you-begin"></a>Başlamadan önce

Kaydolma aşamasına başlamadan önce aşağıdakileri doğrulayın:

- Azure Hesap Profilinizdeki bilgilerin (iletişim e-posta adresi, sokak adresi ve telefon numarası gibi) doğru olduğunu.
- Kredi kartı bilgilerinizin doğru olduğunu.
- Önceden aynı bilgilere sahip bir Microsoft hesabınız olmadığını.

## <a name="guided-walkthrough-of-azure-sign-up"></a>Azure kaydolma için kılavuzlu izlenecek yol

Azure kaydolma deneyimi dört bölümden oluşur:

- Hakkınızda
- Telefonla kimlik doğrulama
- Kartla kimlik doğrulama
- Sözleşme

Bu izlenecek yol, Azure hesabına kaydolmak için gereken doğru bilgilere ilişkin örnekler sunar. Her bölümde bazı yaygın sorunlar ve bunların çözümü de yer alır.

## <a name="about-you"></a>Hakkınızda

![Hakkınızda](./media/troubleshoot-azure-sign-up/1.png)
 
### <a name="common-issues-and-solutions"></a>Yaygın sorunlar ve çözümleri

#### <a name="you-see-the-message-we-cannot-proceed-with-sign-up-due-to-an-issue-with-your-account-please-contact-billing-support"></a>“Hesabınızla ilgili bir sorun nedeniyle kayıt işlemine devam edilemiyor. Lütfen faturalama desteğine başvurun” iletisini görürsünüz

Bu sorunu çözmek için bu adımları izleyin:

1.  Hesap yöneticisi kimlik bilgilerini kullanarak [Azure hesap merkezinde](https://account.azure.com/Profile) oturum açın.
1.  **Ayrıntıları düzenle** seçeneğini belirleyin.
1.  Tüm adres alanlarının doldurulduğunu ve geçerli olduğunu doğrulayın.
1.  Azure aboneliğine kaydolduğunuzda, kredi kartı kaydı ile ilişkili fatura adresinin, banka kayıtlarınızla aynı olduğunu doğrulayın.

İletiyi almaya devam ederseniz farklı bir tarayıcı kullanarak kaydolmayı deneyin.

InPrivate gözatma kullanılabilir mi?

#### <a name="free-trial-is-not-available"></a>Ücretsiz deneme mevcut değildir

Geçmişte bir Azure aboneliği kullandınız mı? Azure Kullanım Koşulları sözleşmesi ücretsiz deneme etkinleştirmesini yalnızca Azure'daki yeni bir kullanıcı için sınırlar. Başka türde bir Azure aboneliğiniz varsa, ücretsiz denemeyi etkinleştiremezsiniz. [Kullandıkça Öde aboneliğine](https://azure.microsoft.com/offers/ms-azr-0003p/) kaydolmayı deneyin.

#### <a name="you-see-the-message-you-are-not-eligible-for-an-azure-subscription"></a>“Azure aboneliği için uygun değilsiniz” iletisini görürsünüz

Bu sorunu çözmek için, aşağıdaki öğelerin doğru olup olmadığını iki kez kontrol edin:

- Azure hesap profiliniz için sağladığınız bilgilerin (iletişim e-posta adresi, sokak adresi ve telefon numarası gibi) doğru olduğunu.
- Kredi kartı bilgilerinin doğru olduğunu.
- Önceden aynı bilgileri kullanan bir Microsoft hesabınız olmadığını.

#### <a name="you-see-the-message-your-current-account-type-is-not-supported"></a>“Geçerli hesap türünüz desteklenmiyor” iletisini görürsünüz

Hesap bir [yönetilmeyen Azure AD dizininde](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-self-service-signup) kayıtlıysa ve kuruluşunuzun Azure AD dizininde değilse bu sorun oluşabilir.
Bu sorunu çözmek için, başka bir hesap kullanarak Azure hesabına kaydolun veya yönetilmeyen AD dizinini devralın. Daha fazla bilgi için bkz. [Azure Active Directory’de yönetilmeyen bir dizini yönetici olarak devralma](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-admin-takeover).

## <a name="identity-verification-by-phone"></a>Telefonla kimlik doğrulama

![Telefonla kimlik doğrulama](./media/troubleshoot-azure-sign-up/2.png)
 
SMS mesajını veya telefon aramasını aldığınızda, gelen kodu metin kutusuna girin.

### <a name="common-issues-and-solutions"></a>Yaygın sorunlar ve çözümleri

#### <a name="no-verification-text-message-or-phone-call"></a>Doğrulama metin mesajı veya telefon araması gelmedi

Kaydolma doğrulama işlemi genellikle hızlı olsa da, doğrulama kodunun teslim edilmesi dört dakika kadar sürebilir.

Burada birkaç ek ipucu bulabilirsiniz:

- Gereksinimleri karşıladığı sürece doğrulama için herhangi bir telefon numarası kullanabilirsiniz. Doğrulama için girdiğiniz telefon numarası, hesap için ilgili kişi numarası olarak depolanmaz.
  - IP üzerinden ses (VoiP) telefon numarası, telefon doğrulama işlemi için kullanılamaz.
  - Telefonunuzun Birleşik Devletler’de bulunan bir telefon numarasından arama veya SMS mesajı alabildiğini kontrol edin.
- Açılan menüde seçtiğiniz ülke kodu da dahil olmak üzere girdiğiniz telefon numarasını iki kez kontrol edin.
- Telefonunuza SMS mesajı gelmezse **Beni ara** seçeneğini deneyin.

## <a name="identity-verification-by-card"></a>Kartla kimlik doğrulama

![Kartla kimlik doğrulama](./media/troubleshoot-azure-sign-up/3.png)
 
### <a name="common-issues-and-solutions"></a>Yaygın sorunlar ve çözümleri

#### <a name="credit-card-declined-or-not-accepted"></a>Kredi kartı reddedildi veya kabul edilmedi

Azure abonelikleri için sanal veya ön ödemeli kredi kartı ya da ATM kartı desteklenmez. Kartınızın reddedilmesine başka nelerin neden olabileceğini görmek için bkz. [Azure kaydı sırasında reddedilen bir kartın sorunlarını giderme](https://docs.microsoft.com/azure/cost-management-billing/manage/troubleshoot-declined-card).

#### <a name="credit-card-form-doesnt-support-my-billing-address"></a>Kredi kartı formu, fatura adresimi desteklemiyor

Fatura adresiniz, **Hakkınızda** bölümünde seçtiğiniz ülkede olmalıdır. Doğru ülkeyi seçtiğinizi doğrulayın.

#### <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>İlerleme çubuğu, kartla kimlik doğrulama bölümüne takılı kalıyor

Kartla kimlik doğrulamayı tamamlamak için tarayıcınız için üçüncü taraf tanımlama bilgilerine izin verilmelidir.

Tarayıcınızın tanımlama bilgisi ayarlarını güncelleştirmek için aşağıdaki adımları kullanın.

1. Tanımlama bilgisi ayarlarını güncelleştirin.
   - **Chrome** kullanıyorsanız:
     - **Ayarlar** > **Gelişmiş ayarları göster** > **Gizlilik** > **İçerik ayarları** seçeneklerini belirleyin. **Üçüncü taraf tanımlama bilgilerini ve site verilerini engelle** seçeneğinin işaretini kaldırın.

   - **Microsoft Edge** kullanıyorsanız:
     - **Ayarlar** > **Gelişmiş ayarları göster** > **Tanımlama Bilgileri** > **Tanımlama bilgilerini engelleme** seçeneklerini belirleyin.

1. Azure kaydolma sayfasını yenileyin ve sorunun çözümlenip çözümlenmediğini denetleyin.
1. Yenileme sonucunda sorun çözülmediyse tarayıcıdan çıkıp tarayıcıyı yeniden başlatın ve tekrar deneyin.

### <a name="i-saw-a-charge-on-my-free-trial-account"></a>Ücretsiz deneme hesabımda ücret uygulandığını gördüm

Kaydolduktan sonra kredi kartı hesabınızda küçük bir geçici doğrulama blokajı olduğunu görebilirsiniz. Bu blokaj, üç ile beş gün içinde kaldırılır. Maliyetleri yönetme konusunda endişeleriniz varsa [beklenmeyen maliyetleri önleme](https://docs.microsoft.com/azure/cost-management-billing/manage/getting-started) ile ilgili daha fazla bilgi edinin.

## <a name="agreement"></a>Sözleşme

Sözleşmeyi tamamlayın.

## <a name="other-issues"></a>Diğer sorunlar

### <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>MSDN, BizSpark, BizSparkPlus veya MPN gibi Azure avantaj planı etkinleştirilemiyor

Doğru oturum açma kimlik bilgilerini kullanıp kullanmadığınızı kontrol edin. Ardından, uygun olduğunuzu doğrulamak için avantaj programını kontrol edin.
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

## <a name="additional-help-resources"></a>Ek yardım kaynakları

Azure Faturalama ve Abonelikler için diğer sorun giderme makaleleri

- [Reddedilen kart](https://docs.microsoft.com/azure/cost-management-billing/manage/troubleshoot-declined-card)
- [Abonelik oturum açma sorunları](https://docs.microsoft.com/azure/cost-management-billing/manage/troubleshoot-sign-in-issue)
- [Abonelik bulunamadı](https://docs.microsoft.com/azure/cost-management-billing/manage/no-subscriptions-found)
- [Kurumsal maliyet görünümü devre dışı](https://docs.microsoft.com/azure/cost-management-billing/manage/enterprise-mgmt-grp-troubleshoot-cost-view)

## <a name="contact-us-for-help"></a>Yardım için bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="find-out-more-about-azure-cost-management"></a>Azure Maliyet Yönetimi hakkında daha fazla bilgi edinin

- [Azure Maliyet Yönetimi ve Faturalandırma belgeleri](https://docs.microsoft.com/azure/cost-management-billing)
