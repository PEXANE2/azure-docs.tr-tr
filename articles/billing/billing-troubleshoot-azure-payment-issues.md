---
title: Azure ödeme sorunlarını giderme
description: Microsoft Azure portal veya hesap merkezindeki ödeme bilgileri hesabını güncelleştirirken bir sorun giderme.
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
ms.author: jaserano
ms.openlocfilehash: 45f6f181ddbf7e76662f76c433d8ff68c18f0de1
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69657092"
---
# <a name="troubleshoot-azure-payment-issues"></a>Azure ödeme sorunlarını giderme

Microsoft Azure portal veya Azure Hesap Merkezi ' nde ödeme bilgileri hesabını güncelleştirmeye çalıştığınızda bir sorunla karşılaşabilirsiniz. Sorunu gidermek için aşağıdaki yönergeleri göz önünde bulundurun:

- Azure Hesap profiliniz (iletişim e-posta adresi, sokak adresi ve telefon numarası dahil) için belirttiğiniz bilgilerin doğru olduğundan emin olun.
- Kredi kartı bilgilerinin doğru olduğundan emin olun.
- Aynı bilgilerle oluşturulmuş başka bir Microsoft hesabınızın olmadığından emin olun.

## <a name="issues"></a>Sorunlar

Hataları gidermek için, Azure 'a kaydolmaya çalıştığınızda karşılaştığınız sorunu seçin.

### <a name="unable-to-remove-a-credit-card-from-a-saved-billing-payment-method"></a>Kaydedilmiş faturalandırma ödeme yönteminden bir kredi kartı kaldırılamıyor

Tasarıma göre, etkin abonelikten bir kredi kartı kaldıramazsınız.

Mevcut bir kartın silinmesi gerekiyorsa, eski ödeme aracının başarıyla silinebilmesi için aboneliğe yeni bir kart eklenmelidir ya da aboneliği iptal etmeniz gerekir. Bu, aboneliği kalıcı olarak siler ve kartı kaldırır.

### <a name="unable-to-delete-an-old-payment-method-after-adding-a-new-payment-method"></a>Yeni ödeme yöntemi eklendikten sonra eski ödeme yöntemi silinemiyor

Yeni ödeme aracı abonelikle ilişkili olmayabilir. Ödeme gerecini abonelikle ilişkilendirmenize yardımcı olmak için bkz. [Azure için kredi veya banka kartı ekleme, güncelleştirme veya kaldırma](billing-how-to-change-credit-card.md).

Reddedilen kartla ilgili sorunları gidermek için bkz. [Azure kaydı 'nda reddedilen bir kartın sorunlarını giderme](billing-troubleshoot-declined-card.md).

### <a name="unable-to-delete-a-payment-method-because-of-cannot-delete-payment-method-error"></a>Ödeme *yöntemi silinemiyor* hatası nedeniyle bir ödeme yöntemi silinemiyor

Bu, bekleyen bir denge nedeniyle oluşur. Ödeme yöntemini silmeden önce bekleyen bakiyelerin işaretini kaldırın.

### <a name="unable-to-see-subscriptions-under-my-account-to-update-the-payment-method"></a>Ödeme yöntemini güncelleştirmek için Hesabım altında abonelikler denetlenemiyor

Abonelikler için kullanılan olandan farklı bir e-posta KIMLIĞI kullanıyor olabilirsiniz.

Bu sorunu gidermek için bkz. [Azure Portal veya Azure Hesap Merkezi için abonelik bulunamadı oturum açma hatası](billing-no-subscriptions-found.md).

### <a name="unable-to-make-payment-for-a-subscription"></a>Abonelik için ödeme yapılamıyor

Hata iletisini alırsanız: *Ödemenin süresi geçti. Ödeme yönteminiz* ile ilgili bir sorun var veya *Üzgünüz, bilgiler kaydedilemedi. Tarayıcıyı kapatın ve yeniden deneyin.* , kartın finans kurumunuz tarafından reddedildiği için kartın bekleyen bir ödemesi vardır.

Kredi kartının ödeme yapmak için yeterli bakiyesi olduğunu doğrulayın. Değilse, ödeme yapmak için başka bir kart kullanın veya sorunu çözmek için finans kurumunuz ile iletişime geçin.

Lütfen aşağıdaki sorunlar için bankanızla görüşün:

- Uluslararası işlemler etkin değil.
- Kartın kredi limiti vardır ve Bakiyenin kapatılması gerekir.
- Kartın üzerinde bir yinelenen ödeme etkinleştirilmiştir.

### <a name="unable-to-change-payment-method-because-of-browser-issues-browser-does-not-respond-does-not-load-and-so-on"></a>Tarayıcı sorunları nedeniyle ödeme yöntemi değiştirilemiyor (tarayıcı yanıt vermiyor, yüklenmiyor, yüklenmedi vb.)

Tüm etkin Azure oturumlarının oturumunu kapatın ve Microsoft Edge veya Internet Explorer 'da bir InPrivate oturumu başlatmak için [Microsoft Edge 'de InPrivate bakış makalesindeki](https://support.microsoft.com/help/4026200/microsoft-edge-browse-inprivate) adımları izleyin.

Özel oturumda, kredi kartı bilgilerini güncelleştirmek veya değiştirmek için [kredi kartının nasıl değiştirileceği hakkında](billing-how-to-change-credit-card.md) adımları izleyin.

Şunları yapmayı da deneyebilirsiniz:

- Tarayıcınızı yenileyin
- Başka bir tarayıcı kullan
- Önbelleğe alınmış tanımlama bilgilerini sil

### <a name="my-subscription-is-still-disabled-after-updating-the-payment-method"></a>Ödeme yöntemi güncelleştirildikten sonra aboneliğim hala devre dışı bırakıldı.

Bu sorun, bekleyen bir denge nedeniyle oluşur. Ödeme yöntemini silmeden önce bekleyen bakiyelerin işaretini kaldırın.

### <a name="unable-to-change-payment-method-because-of-an-xml-error-response-page"></a>Bir XML hatası yanıt sayfası nedeniyle ödeme yöntemi değiştirilemiyor

Yeni bir kredi kartı eklemek için [Azure Portal](https://portal.azure.com/) kullanıyorsanız bu iletiyi alırsınız.

Kart ayrıntıları eklemek için hesap yöneticisinin e-posta adresini kullanarak Azure hesap portalında oturum açın.

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
