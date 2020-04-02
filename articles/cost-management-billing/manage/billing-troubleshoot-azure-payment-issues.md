---
title: Azure ödeme sorunlarını giderme
description: Microsoft Azure portalında veya hesap merkezinde ödeme bilgileri hesabını güncelleştirirken oluşan bir sorunu çözümleme.
author: v-miegge
ms.reviewerr: dcscontentpm
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: jaserano
ms.openlocfilehash: 59cad232ae4d16b8f74942e1e6026171ae1b8269
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80408428"
---
# <a name="troubleshoot-azure-payment-issues"></a>Azure ödeme sorunlarını giderme

Microsoft Azure portalında veya Azure hesap merkezinde ödeme bilgileri hesabını güncelleştirmeye çalıştığınızda bir sorun veya hatayla karşılaşabilirsiniz.

Sorununuzu çözmek için aşağıda hatanıza en çok benzeyen konu başlığını seçin.

## <a name="my-credit-card-was-declined-when-i-tried-to-sign-up-for-azure"></a>Azure’a kaydolmayı denediğimde kredi kartım reddedildi

Reddedilen kartla ilgili sorunları gidermek için bkz. [Azure’a kaydolurken reddedilen kart ile ilgili sorunları giderme](troubleshoot-declined-card.md).

## <a name="unable-to-see-subscriptions-under-my-account-to-update-the-payment-method"></a>Ödeme yöntemini güncelleştirmek için hesabım altında abonelikler görüntülenemiyor

Abonelikler için kullanılandan farklı bir e-posta kimliği kullanıyor olabilirsiniz.

Bu sorunu gidermek için bkz. [Azure portalı veya Azure hesap merkezi için Abonelik bulunamadı oturum açma hatası](no-subscriptions-found.md).

## <a name="unable-to-use-a-virtual-or-prepaid-credit-or-debit-card-as-a-payment-method"></a>Bir sanal veya ön ödemeli kredi ya da banka kartı ödeme yöntemi olarak kullanılamıyor.

*   Azure abonelikleri için sanal veya ön ödemeli kredi kartı desteklenmez.
*   Azure abonelikleri için ATM kartı desteklenmez.

Daha fazla bilgi için bkz. [Azure kaydı sırasında reddedilen bir kartın sorunlarını giderme](troubleshoot-declined-card.md).

## <a name="unable-to-remove-a-credit-card-from-a-saved-billing-payment-method"></a>Kredi kartı, kaydedilen faturalama ödeme yönteminden kaldırılamıyor

Tasarım gereği bir kredi kartını Etkin abonelikten kaldıramazsınız.

Mevcut bir kartın silinmesi gerekiyorsa, eski ödeme aracının başarıyla silinebilmesi için aboneliğe yeni bir kart eklenmelidir; aksi takdirde aboneliği iptal etmeniz gerekir. Bu işlem sonucunda abonelik kalıcı olarak silinir ve kart kaldırılır.

## <a name="unable-to-delete-an-old-payment-method-after-adding-a-new-payment-method"></a>Yeni bir ödeme yöntemi eklendikten sonra eski ödeme yöntemi silinemiyor

Yeni ödeme aracı abonelikle ilişkili olmayabilir. Ödeme aracının abonelikle ilişkilendirilmesine yardımcı olmak için bkz. [Azure için kredi kartı ekleme, güncelleştirme ya da kaldırma](change-credit-card.md).

## <a name="unable-to-delete-a-payment-method-because-of-cannot-delete-payment-method-error"></a>*Ödeme yöntemi silinemiyor* hatası nedeniyle ödeme yöntemi silinemiyor

Bu, kalan bakiyeden kaynaklanır. Ödeme yöntemini silmeden önce kalan tüm bakiyeyi ödeyin.

## <a name="unable-to-make-payment-for-a-subscription"></a>Abonelik için ödeme yapılamıyor

Hata iletisini alırsanız: *Ödemenin süresi geçti. Ödeme yönteminiz ile ilgili bir sorun var* veya *Ne yazık ki bilgiler kaydedilemiyor. Tarayıcıyı kapatıp yeniden deneyin.* Bu durumda kart ilgili finans kurumu tarafından reddedildiğinden kartta bekleyen bir ödeme vardır.

Kredi kartının ödeme yapmak için yeterli bakiyesi olduğunu doğrulayın. Yoksa ödeme yapmak için başka bir kart kullanın veya sorunu çözmek için finans kurumunuza ulaşın.

Lütfen aşağıdaki sorunlar için bankanızla görüşün:

- Uluslararası işlemler etkin değil.
- Kartın kredi limiti var ve bakiyenin kapatılması gerekiyor.
- Kartta yinelenen ödeme etkin.

## <a name="unable-to-change-payment-method-because-of-browser-issues-browser-does-not-respond-does-not-load-and-so-on"></a>Tarayıcı sorunları nedeniyle ödeme yöntemi değiştirilemiyor (tarayıcı yanıt vermiyor, yüklenmiyor vb.)

Tüm etkin Azure oturumlarını kapatın ve sonra Microsoft Edge veya Internet Explorer’da InPrivate oturumu başlatmak için [Microsoft Edge’de InPrivate gözatma makalesindeki](https://support.microsoft.com/help/4026200/microsoft-edge-browse-inprivate) adımları izleyin.

Özel oturumda, kredi kartı bilgilerini güncelleştirmek veya değiştirmek için [Kredi kartını değiştirme](change-credit-card.md) bölümündeki adımları izleyin.

Aşağıdakileri de yapmayı deneyebilirsiniz:

- Tarayıcınızı yenileyin
- Başka bir tarayıcı kullanın
- Önbelleğe alınmış tanımlama bilgilerini silin

## <a name="my-subscription-is-still-disabled-after-updating-the-payment-method"></a>Ödeme yöntemini güncelleştirdikten sonra aboneliğim devre dışı kalmaya devam ediyor.

Bu sorun, kalan bakiyeden kaynaklanır. Ödeme yöntemini silmeden önce kalan tüm bakiyeyi ödeyin.

## <a name="unable-to-change-payment-method-because-of-an-xml-error-response-page"></a>XML hata yanıt sayfası nedeniyle ödeme yöntemi değiştirilemiyor

Yeni bir kredi kartı eklemek için [Azure portalını](https://portal.azure.com/) kullanıyorsanız bu iletiyi alırsınız.

Kart ayrıntılarını eklemek için hesap yöneticisinin e-posta adresini kullanarak Azure Hesap portalında oturum açın.

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
