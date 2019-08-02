---
title: Azure ayırmalarını otomatik olarak yenileme
description: Rezervasyon indirimlerini almaya devam etmek için Azure ayırmalarını otomatik olarak nasıl yenileyebileceğinizi öğrenin.
services: billing
author: bandersmsft
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: banders
ms.openlocfilehash: c19c6af68bcde753ec9bed990e08aa81eabdd37d
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68679464"
---
# <a name="automatically-renew-reservations"></a>Rezervasyonları otomatik olarak Yenile

Rezervasyonları, var olan bir ayırma sona erdiğinde otomatik olarak bir değiştirme satın alacak şekilde yenileyebilirsiniz. Otomatik yenileme, rezervasyon indirimlerini almaya devam etmenin kolay bir yolunu sağlar. Ayrıca, bir ayırmanın sona erme süresini yakından izlemenize gerek kalmanızı sağlar. Otomatik yenilemeyle, el ile yenilemenize gerek kalmadan tasarruf etme avantajlarının kaybolmasını önleyebilirsiniz. Yenileme ayarı varsayılan olarak kapalıdır. Yenileme ayarını, mevcut ayırmanın sonuna kadar her zaman etkinleştirin veya devre dışı bırakın.

Bir ayırmayı yenilemek, mevcut ayırma sona erdiğinde yeni bir ayırma oluşturur. Mevcut ayırmanın terimini genişletmez.

Dilediğiniz zaman otomatik olarak yenilemek için kabul edin. Yenileme fiyatı, mevcut ayırmanın süresi dolmadan 30 gün önce kullanılabilir. Yenileme süresi dolduktan 30 günden daha uzun bir yenileme etkinleştirdiğinizde, yenileme maliyetlerini süresi dolmadan 30 gün önce açıklayan bir e-posta gönderilir. Ayırma fiyatı, yenileme fiyatını kilitletime ve yenileme süresi arasında değişebilir. Bu durumda, yenileme maliyetiniz iki maliyetin düşüktür. Rezervasyon miktarındaki değişiklikler yapabilirsiniz. Bunu yaparsanız, yenileme, miktar değişikliği sırasında piyasa fiyatı kümesini kullanacak şekilde güncelleştirilir.

Yenileme yükümlülüğü yoktur ve var olan ayırma süresi dolmadan yenileme işlemini dilediğiniz zaman devre dışı bırakabilirsiniz.

## <a name="set-up-renewal"></a>Yenilemeyi ayarla

Azure portal > **ayırmaları**' ne gidin.

1. Ayırmayı seçin.
2. **Yenileme**' ye tıklayın.
3. **Süre dolduktan sonra otomatik olarak yeni bir ayırma satın al**seçeneğini belirleyin.  
  ![Ayırma yenilemeyi gösteren örnek](./media/billing-reservation-renew/reservation-renewal.png)

## <a name="if-you-dont-renew"></a>Yenilemezseniz

Hizmetleriniz normal şekilde çalışmaya devam eder. Rezervasyon süresi dolduktan sonra kullanımınız için Kullandıkça Öde tarifesine göre ücretlendirilirsiniz.

## <a name="required-renewal-permissions"></a>Gerekli yenileme izinleri

Bir ayırmayı yenilemek için aşağıdaki koşullar gereklidir:

- Mevcut ayırmanın sahibi olmanız gerekir.
- Ayırmanın tek bir abonelik veya kaynak grubu kapsamında olması halinde, aboneliğin sahibi olmanız gerekir.
- Paylaşılan bir kapsamı varsa aboneliğin sahibi olmanız gerekir.

## <a name="default-renewal-settings"></a>Varsayılan yenileme ayarları

Varsayılan olarak, yenileme süresi dolan rezervasyondan tüm özellikleri devralır. Bir rezervasyon yenileme satın alımı aynı SKU, bölge, kapsam, faturalandırma aboneliği, terim ve miktara sahiptir.

Ancak, tasarrufunuzu iyileştirmek için yenileme rezervasyonu satın alma miktarını güncelleştirebilirsiniz.

## <a name="when-the-new-reservation-is-purchased"></a>Yeni rezervasyon satın alındığında

Mevcut rezervasyon sona erdiğinde yeni bir rezervasyon satın alınır. İki ayırma arasındaki gecikmeyi engellemeye çalışırız. Sürekliliği, maliyetlerinizin tahmin edilebilir olmasını sağlar ve indirimleri almaya devam edersiniz.

## <a name="changing-parent-reservation-after-setting-renewal"></a>Yenileme ayarından sonra üst ayırmayı değiştirme

Süresi dolan rezervasyonda aşağıdaki değişikliklerden birini yaparsanız, rezervasyon yenileme iptal edilir:

- Böl
- Birleştir
- Ayırma bir hesaptan diğerine aktarılıyor
- Bir WebDirect aboneliğinden bir kurumsal anlaşma (EA) aboneliğine veya başka bir satın alma yöntemine ayırmayı aktarma
- Kaydı yenileyin

Yeni ayırma, yenileme sırasında süresi dolan rezervasyondan kapsam ve örnek boyutu esneklik ayarını devralır.

## <a name="new-reservation-permissions"></a>Yeni ayırma izinleri

Azure, izinleri süresi dolan ayırmayla yeni ayırmaya kopyalar. Ayrıca, rezervasyon satın alma aboneliğinin abonelik hesabı yöneticisinin yeni ayırmaya erişimi vardır.

## <a name="potential-renewal-problems"></a>Olası yenileme sorunları

Azure, şu durumlarda yenileme işlemini işleyemeyebilir:

- Ödeme toplanamıyor
- Yenileme sırasında bir sistem hatası oluşuyor
- Süresi dolan SKU yenileme sırasında etkin değil
- EA farklı bir EA 'da yenilendi

Yukarıdaki koşullardan biri gerçekleşirse ve yenileme devre dışı bırakılmışsa bir e-posta bildirimi alırsınız.

## <a name="renewal-notification"></a>Yenileme bildirimi

E-postalar, satın alma yönteminize bağlı olarak farklı kişilere gönderilir:

- EA müşterileri-e-postalar, EA portalında ayarlanan bildirim kişilerine gönderilir.
- Kullandıkça Öde tarifelerine sahip bireysel abonelik müşterileri-e-postalar, hesap yöneticileri olarak ayarlanan kullanıcılara gönderilir.
- Bulut çözümü sağlayıcısı müşterileri-e-postalar iş ortağı bildirim ekibine gönderilir.

## <a name="next-steps"></a>Sonraki adımlar
- Azure ayırmaları hakkında daha fazla bilgi edinmek için bkz. [Azure rezervasyonları nelerdir?](billing-save-compute-costs-reservations.md)
