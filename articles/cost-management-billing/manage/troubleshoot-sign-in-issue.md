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
ms.openlocfilehash: 915b4fb289b6af6f4aac84d8bfa4d491a7f77efe
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99050479"
---
# <a name="troubleshoot-azure-subscription-sign-in-issues"></a>Azure aboneliği oturum açma sorunlarını giderme

Bu kılavuz, Azure portalında veya Azure hesap merkezinde oturum açamama sorunlarının çözülmesine yardımcı olur.

> [!NOTE]
> Yeni bir Azure hesabına kaydolmayla ilgili sorun yaşıyorsanız bkz. [Azure aboneliğine kaydolma sorunlarını giderme](./troubleshoot-azure-sign-up.md).

## <a name="page-hangs-in-the-loading-status"></a>Sayfa, yükleme durumunda yanıt vermemeye başlıyor

İnternet tarayıcınızın sayfası yanıt vermemeye başlıyorsa Azure portalına ulaşana kadar aşağıdaki adımlardan her birini deneyin.

- Sayfayı yenileyin.
- Farklı bir İnternet tarayıcısı kullanın.
- Tarayıcınızda özel gözatma modunu kullanın:

   - **Edge:** **Ayarları** açın (profil resminiz için üç nokta), **Yeni InPrivate pencere**' yi seçin ve [Azure Portal](https://portal.azure.com/)gidip oturum açın. 
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
   - **Edge:** **Ayarları** açın (profil resminiz için üç nokta), **Yeni InPrivate pencere**' yi seçin ve [Azure Portal](https://portal.azure.com/)gidip oturum açın. 
   - **Chrome:** **Gizli** modu seçin.
   - **Safari:** **Dosya**’yı ve sonra **Yeni Özel Pencere**’yi seçin.

## <a name="i-can-sign-in-but-i-see-the-error-no-subscriptions-found"></a>Oturum açabiliyorum, ancak “Abonelik bulunamadı” hatasını görüyorum

Yanlış dizinde seçim yaptıysanız veya hesabınız yeterli izinlere sahip değilse bu sorun oluşur.

**Senaryo 1:** [Azure portalında](https://portal.azure.com/) oturum açarken hata alıyorsunuz

Bu sorunu düzeltmek için:

- Sağ üst köşede hesabınızı seçerek doğru Azure dizininin seçildiğini doğrulayın.
- Doğru Azure dizini seçili olduğu halde hata iletisini almaya devam ediyorsanız hesabınızın [Sahip olarak eklenmesini](./add-change-subscription-administrator.md) sağlayın.

**Senaryo 2:** [Azure Hesap Merkezi](https://account.windowsazure.com/Subscriptions)’nde oturum açarken hata alıyorsunuz

Kullandığınız hesabın Hesap Yöneticisi olup olmadığını denetleyin. Hesap Yöneticisinin kim olduğunu doğrulamak için şu adımları izleyin:

1.  [Azure portalındaki Abonelikler görünümünde](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) oturum açın.
1.  Denetlemek istediğiniz aboneliği ve sonra **Ayarlar**’ı seçin.
1.  **Özellikler**’i seçin. Aboneliğin hesap yöneticisi, **Hesap Yöneticisi** kutusunda görüntülenir.

## <a name="additional-help-resources"></a>Ek yardım kaynakları

Azure Faturalama ve Abonelikler için diğer sorun giderme makaleleri

- [Reddedilen kart](./troubleshoot-declined-card.md)
- [Abonelik kaydı sorunları](./troubleshoot-azure-sign-up.md)
- [Abonelik bulunamadı](./no-subscriptions-found.md)
- [Kurumsal maliyet görünümü devre dışı](./enterprise-mgmt-grp-troubleshoot-cost-view.md)
- [Azure Faturalama belgeleri](../index.yml)

## <a name="contact-us-for-help"></a>Yardım için bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).