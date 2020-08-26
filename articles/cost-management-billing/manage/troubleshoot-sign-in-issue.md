---
title: Azure aboneliği oturum açma sorunlarını giderme
description: Azure portalında veya Azure hesap merkezinde oturum açamama sorunlarının çözülmesine yardımcı olur.
services: cost-management-billing
author: v-miegge
manager: dcscontentpm
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: troubleshooting
ms.date: 08/20/2020
ms.author: v-miegge
ms.openlocfilehash: 3ee600cb72d06781f87c8f68640576afa50cea06
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88686505"
---
# <a name="troubleshoot-azure-subscription-sign-in-issues"></a>Azure aboneliği oturum açma sorunlarını giderme

Bu kılavuz, Azure portalında veya Azure hesap merkezinde oturum açamama sorunlarının çözülmesine yardımcı olur.

> [!NOTE]
> Yeni bir Azure hesabına kaydolmayla ilgili sorun yaşıyorsanız bkz. [Azure aboneliğine kaydolma sorunlarını giderme](https://docs.microsoft.com/azure/cost-management-billing/manage/troubleshoot-azure-sign-up).

## <a name="page-hangs-in-the-loading-status"></a>Sayfa, yükleme durumunda yanıt vermemeye başlıyor

İnternet tarayıcınızın sayfası yanıt vermemeye başlıyorsa Azure portalına ulaşana kadar aşağıdaki adımlardan her birini deneyin.

- Sayfayı yenileyin.
- Farklı bir İnternet tarayıcısı kullanın.
- Tarayıcınızda özel gözatma modunu kullanın:

   - **Microsoft Edge:** **Ayarlar**’ı (profil resminizin yanındaki üç nokta) açın, **Yeni InPrivate pencere**’yi seçin ve [Azure portalına](https://portal.azure.com/) veya [Azure hesap merkezine](https://account.azure.com/Subscriptions) gidip burada oturum açın. 
   - **Chrome:** **Gizli** modu seçin.
   - **Safari:** **Dosya**’yı ve sonra **Yeni Özel Pencere**’yi seçin.

- Önbelleği temizleyin ve İnternet tanımlama bilgilerini silin:

   - **Microsoft Edge:** **Ayarlar**’ı açıp **Gizlilik ve Hizmetler**’i seçin. **Gözatma Verilerini Temizle** bölümündeki adımları izleyin. **Gözatma geçmişi**, **İndirme geçmişi** ve **Önbelleğe alınan resimler ve dosyalar** onay kutularının işaretli olduğundan emin olun ve **Sil**’i seçin.
   - **Chrome:** **Ayarlar**’ı ve sonra **Gizlilik ve Güvenlik** bölümünden **Gözatma verilerini temizle**’yi seçin.

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>Otomatik şekilde farklı kullanıcı olarak oturum açtınız

İnternet tarayıcısında birden çok kullanıcı hesabı kullanıyorsanız bu sorun oluşabilir.

Sorunu çözmek için aşağıdaki yöntemlerden birini deneyin:

- Önbelleği temizleyin ve İnternet tanımlama bilgilerini silin.

   - **Microsoft Edge:** **Ayarlar**’ı açıp **Gizlilik ve Hizmetler**’i seçin. **Gözatma Verilerini Temizle** bölümündeki adımları izleyin. **Gözatma geçmişi**, **İndirme geçmişi**, **Tanımlama bilgileri** ve **Önbelleğe alınan resimler ve dosyalar** onay kutularının işaretli olduğundan emin olun ve **Sil**’i seçin.
   - **Chrome:** **Ayarlar**’ı ve sonra **Gizlilik ve Güvenlik** bölümünden **Gözatma verilerini temizle**’yi seçin.
- Tarayıcı ayarlarınızı varsayılana sıfırlayın.
- Tarayıcınızda özel gözatma modunu kullanın. 
   - **Microsoft Edge:** **Ayarlar**’ı (profil resminizin yanındaki üç nokta) açın, **Yeni InPrivate pencere**’yi seçin ve [Azure portalına](https://portal.azure.com/) veya [Azure hesap merkezine](https://account.azure.com/Subscriptions) gidip burada oturum açın. 
   - **Chrome:** **Gizli** modu seçin.
   - **Safari:** **Dosya**’yı ve sonra **Yeni Özel Pencere**’yi seçin.

## <a name="i-can-sign-in-but-i-see-the-error-no-subscriptions-found"></a>Oturum açabiliyorum, ancak “Abonelik bulunamadı” hatasını görüyorum

Yanlış dizinde seçim yaptıysanız veya hesabınız yeterli izinlere sahip değilse bu sorun oluşur.

**Senaryo 1:** [Azure portalında](https://portal.azure.com/) oturum açarken hata alıyorsunuz

Bu sorunu düzeltmek için:

- Sağ üst köşede hesabınızı seçerek doğru Azure dizininin seçildiğini doğrulayın.
- Doğru Azure dizini seçili olduğu halde hata iletisini almaya devam ediyorsanız hesabınızın [Sahip olarak eklenmesini](https://docs.microsoft.com/azure/cost-management-billing/manage/add-change-subscription-administrator) sağlayın.

**Senaryo 2:** [Azure Hesap Merkezi](https://account.windowsazure.com/Subscriptions)’nde oturum açarken hata alıyorsunuz

Kullandığınız hesabın Hesap Yöneticisi olup olmadığını denetleyin. Hesap Yöneticisinin kim olduğunu doğrulamak için şu adımları izleyin:

1.  [Azure portalındaki Abonelikler görünümünde](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) oturum açın.
1.  Denetlemek istediğiniz aboneliği ve sonra **Ayarlar**’ı seçin.
1.  **Özellikler**’i seçin. Aboneliğin hesap yöneticisi, **Hesap Yöneticisi** kutusunda görüntülenir.

## <a name="additional-help-resources"></a>Ek yardım kaynakları

Azure Faturalama ve Abonelikler için diğer sorun giderme makaleleri

- [Reddedilen kart](https://docs.microsoft.com/azure/cost-management-billing/manage/troubleshoot-declined-card)
- [Abonelik kaydı sorunları](https://docs.microsoft.com/azure/cost-management-billing/manage/troubleshoot-azure-sign-up)
- [Abonelik bulunamadı](https://docs.microsoft.com/azure/cost-management-billing/manage/no-subscriptions-found)
- [Kurumsal maliyet görünümü devre dışı](https://docs.microsoft.com/azure/cost-management-billing/manage/enterprise-mgmt-grp-troubleshoot-cost-view)
- [Azure Faturalama belgeleri](https://docs.microsoft.com/azure/cost-management-billing/)

## <a name="contact-us-for-help"></a>Yardım için bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
