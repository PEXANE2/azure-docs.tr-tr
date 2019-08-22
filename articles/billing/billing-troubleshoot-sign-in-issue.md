---
title: Azure aboneliği oturum açma sorunlarını giderme
description: Azure portal veya Azure Hesap Merkezi 'nde oturum açabilmeniz gereken sorunları gidermeye yardımcı olur.
author: v-miegge
manager: na
editor: na
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/12/2019
ms.author: v-miegge
ms.openlocfilehash: ca641813e8b01a39d31a56e3730424b0fa1d6436
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69657053"
---
# <a name="troubleshoot-azure-subscription-sign-in-issues"></a>Azure aboneliği oturum açma sorunlarını giderme

Bu kılavuz, Azure portal veya Azure Hesap Merkezi 'nde oturum açabilmeniz gereken sorunları gidermeye yardımcı olur. 

## <a name="issues"></a>Sorunlar

### <a name="page-hangs-in-the-loading-status"></a>Sayfa yükleme durumunda askıda kalıyor

Internet tarayıcı sayfanız askıda kalırsa, Azure portal gelene kadar aşağıdaki adımlardan her birini deneyin.

- Sayfayı yenileyin.
- Farklı bir internet tarayıcısı kullanın.
- Tarayıcınız için özel gözatma modunu kullanın. Internet Explorer için: **Araçlar** [](https://account.azure.com/Subscriptions) [](https://portal.azure.com/) güvenlik InPrivate Gözatma ' ya tıklayın ve Azure Portal veya Azure Hesap Merkezi ' ne gidin ve oturum açın. >  > 

### <a name="you-are-automatically-signed-in-as-a-different-user"></a>Otomatik olarak farklı bir kullanıcı olarak oturum açtınız

Bu sorun, bir internet tarayıcısında birden fazla kullanıcı hesabı kullanırsanız oluşabilir.

Sorunu çözmek için aşağıdaki yöntemlerden birini deneyin:

- Önbelleği temizleyin ve Internet tanımlama bilgilerini silin. Internet Explorer 'da **Araçlar** > **Internet seçenekleri** > **Sil**' e tıklayın. Geçici dosyalar, tanımlama bilgileri, parola ve göz atma geçmişi onay kutularının seçili olduğundan emin olun ve ardından Sil ' e tıklayın.
- Internet Explorer ayarlarını, yaptığınız tüm kişisel ayarları geri döndürecek şekilde sıfırlayın. **Araçlar**Internet seçenekleriGelişmiş'etıklayın>Kişiselayarlarısilkutusunu>sıfırlayın'ıseçin. >  > 
- Tarayıcınız için özel gözatma modunu kullanın. Internet Explorer için:  **Araçlar** [](https://account.azure.com/Subscriptions) [](https://portal.azure.com/) güvenlik InPrivate Gözatma ' ya tıklayın ve Azure Portal veya Azure Hesap Merkezi ' ne gidin ve oturum açın. >  > 

### <a name="i-can-sign-in-but-i-see-no-subscriptions-found"></a>Oturum açabilir, ancak *abonelik bulunamadı*

Yanlış dizin seçtiyseniz veya hesabınızın yeterli izinlere sahip değilse bu sorun oluşur.

**Senaryo 1:** [Azure Portal](https://portal.azure.com/) hata iletisi alındı

Bu sorunu gidermek için:

- Doğru Azure directory sağ üst köşedeki hesabınızı tıklayarak seçili olduğundan emin olun.
- Doğru Azure dizini seçiliyse ancak yine de hata iletisini alıyorsanız, hesabınızın [sahip olarak eklenmesini](billing-add-change-azure-subscription-administrator.md)sağlayabilirsiniz.

**Senaryo 2:** [Azure Hesap Merkezi](https://account.windowsazure.com/Subscriptions) hata iletisi alındı

Kullandığınız hesabın hesap yöneticisi olup olmadığını denetleyin. Hesap Yöneticisi olan doğrulamak için aşağıdaki adımları izleyin:

1. Oturum [Azure portalında abonelikleri görmek](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

2. Kontrol edin ve ardından altına bakın istediğiniz aboneliği seçin **ayarları**.

3. Seçin **özellikleri**. Aboneliğin Hesap Yöneticisi görüntülenen **Hesap Yöneticisi** kutusu.

## <a name="additional-help-resources"></a>Ek Yardım kaynakları

Azure Faturalandırma ve abonelikleriyle ilgili diğer sorun giderme makaleleri

- [Reddedilen kart](billing-troubleshoot-declined-card.md)
- [Abonelik Kaydolma sorunları](billing-troubleshoot-azure-sign-up.md)
- [Abonelik bulunamadı](billing-no-subscriptions-found.md)
- [Kurumsal maliyet görünümü devre dışı](billing-enterprise-mgmt-grp-troubleshoot-cost-view.md)

## <a name="contact-us-for-help"></a>Yardım için bizimle iletişime geçin

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure faturalama belgeleri](index.md)
