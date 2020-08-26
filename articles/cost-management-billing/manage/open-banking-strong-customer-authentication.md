---
title: Azure müşterileri için Açık Bankacılık (P2D2) ve Güçlü Müşteri Kimlik Doğrulaması (SCA)
description: Bu makalede bazı Azure satın almalarında çok faktörlü kimlik doğrulamasının neden gerekli olduğu ve kimlik doğrulamasının nasıl tamamlanacağı açıklanır.
author: bandersmsft
ms.reviewer: judupont
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 08/20/2020
ms.author: banders
ms.openlocfilehash: b4c2db76405e7bad7653c1801eb2bcc5db82fe9b
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88684805"
---
# <a name="open-banking-psd2-and-strong-customer-authentication-sca-for-azure-customers"></a>Azure müşterileri için Açık Bankacılık (P2D2) ve Güçlü Müşteri Kimlik Doğrulaması (SCA)

14 Eylül 2019'dan itibaren [Avrupa Ekonomik Alanı](https://en.wikipedia.org/wiki/European_Economic_Area)'nın 31 ülkesinde/bölgesinde yer alan bankalarda çevrimiçi alışveriş yapan müşterinin ödemesi işlenmeden önce kimliğini doğrulaması gerekmektedir. Bu doğrulama, çevrimiçi satın alma işlemlerinizin güvenliğinden ve koruma altında olduğundan emin olmak için çok faktörlü kimlik doğrulaması gerektirir. Bu doğrulama gereksiniminin başlangıç tarihi bazı ülkelerde/bölgelerde geciktirilecektir. Daha fazla bilgi için bkz. [PSD2 hakkında Microsoft SSS](https://support.microsoft.com/en-us/help/4517854?preview).

## <a name="what-psd2-means-for-azure-customers"></a>Azure müşterileri açısından PSD2 ne anlama gelir?

Azure için ödeme yaparken [Avrupa Ekonomik Alanı](https://en.wikipedia.org/wiki/European_Economic_Area)'ndaki bir bankanın kredi kartını kullanıyorsanız, hesabınızın ödeme yöntemi için çok faktörlü kimlik doğrulamasını tamamlamanız gerekebilir. Azure hesabınıza kaydolurken veya Azure hesabınızı yükseltirken (o sırada alışveriş yapmıyor olsanız bile) çok faktörlü kimlik doğrulaması sınamasını tamamlamanız istenebilir. Ayrıca Azure hesabınızın ödeme yöntemini değiştirirken, harcama limitinizi kaldırırken veya Azure portalından anlık bir ödeme yaparken de (ödenmemiş bakiyeyi kapatma veya Azure kredisi satın alma gibi) çok faktörlü kimlik doğrulaması sağlamanız istenebilir.

Bankanız aylık Azure ücretlerinizi reddederse, Azure'dan durumu düzeltme yönergelerini de içeren bir vadesi geçmiş ödeme e-postası alırsınız. Azure portalında çok faktörlü kimlik doğrulaması sınamasını tamamlayabilir ve ödenmemiş borçları ödeyebilirsiniz.

## <a name="complete-multi-factor-authentication-in-the-azure-portal"></a>Azure portalında çok faktörlü kimlik doğrulamasını tamamlama

Aşağıdaki bölümlerde Azure portalında çok faktörlü kimlik doğrulamasının nasıl tamamlanacağı açıklanır. Size uygun olan bilgileri kullanın.

### <a name="change-the-active-payment-method-of-your-azure-account"></a>Azure hesabınızın etkin ödeme yöntemini değiştirme

Şu adımları izleyerek Azure hesabınızın etkin ödeme yöntemini değiştirebilirsiniz:

1. [Azure portalında](https://portal.azure.com) Hesap Yöneticisi olarak oturum açın ve **Maliyet Yönetimi + Faturalama**'ya gidin.
2. **Genel Bakış** sayfasında, **Aboneliklerim** kılavuzundan ilgili aboneliği seçin.
3. 'Faturalama' bölümünde **Ödeme yöntemleri**'ni seçin. Abonelik için etkin ödeme yöntemi olarak yeni kredi kartı ekleyebilir veya mevcut kartı ayarlayabilirsiniz. Bankanız çok faktörlü kimlik doğrulaması gerektiriyorsa, işlem sırasında kimlik doğrulaması sınamasını tamamlamanız istenir.

Diğer ayrıntılar için bkz. [Azure için kredi kartını ekleme, güncelleştirme veya kaldırma](change-credit-card.md).

### <a name="settle-outstanding-charges-for-azure-services"></a>Azure hizmetleri için ödenmemiş borçları ödeme

Banka ücretleri reddederse Azure portalında Azure hesabınızın durumu **Süresi geçti** olarak değişir. Şu adımları izleyerek hesabınızın durumunu denetleyebilirsiniz:

1. [Azure portalında](https://portal.azure.com/) Hesap Yöneticisi olarak oturum açın.
2. **Maliyet Yönetimi + Faturalama** araması yapın.
3. **Maliyet Yönetimi + Faturalama** **Genel Bakış** sayfasında, **Aboneliklerim** kılavuzundaki durum sütununu gözden geçirin.
4. Aboneliğiniz **Süresi geçti** olarak etiketlendiyse **Bakiyeyi ödeyin** bağlantısına tıklayın. İşlem sırasında çok faktörlü kimlik doğrulamasını tamamlamanız istenir.

### <a name="settle-outstanding-charges-for-marketplace-and-reservation-purchases"></a>Market ve rezervasyon satın almaları için ödenmemiş ücretleri ödeme

Market ve rezervasyon satın almaları, Azure hizmetlerinden ayrı faturalanır. Bankanız Market veya rezervasyon ücretlerini reddederse, faturanın süresi dolar ve Azure portalında **Şimdi ödeyin** seçeneği gösterilir. Şu adımları izleyerek süresi geçen Market ve rezervasyon faturalarınızı ödeyebilirsiniz:

1. [Azure portalında](https://portal.azure.com/) Hesap Yöneticisi olarak oturum açın.
2. **Maliyet Yönetimi + Faturalama** araması yapın.
3. 'Faturalama'nın altında **Faturalar**'ı seçin.
5. Abonelik açılan filtresinde Market veya rezervasyon satın alma işlemiyle ilişkili aboneliği seçin.
6. Faturalar tablosunda tür sütununu gözden geçirin. Tür **Azure Market ve Rezervasyonlar** ise vadesi gelen veya geçen faturalar için **Şimdi ödeyin** bağlantısı gösterilir. **Şimdi ödeyin** bağlantısının görünmemesi, faturanızın ödenmiş olduğu anlamına gelir. Şimdi ödeyin'i seçtikten sonra çok faktörlü kimlik doğrulamasını tamamlamanız istenir.

## <a name="next-steps"></a>Sonraki adımlar
- Azure faturasını ödemeniz gerekiyorsa bkz. [Azure aboneliğinizin süresi geçen bakiye sorununu çözme](resolve-past-due-balance.md).
