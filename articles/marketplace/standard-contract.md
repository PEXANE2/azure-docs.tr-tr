---
title: Standart Sözleşme | Azure Marketi
description: Azure Market ve AppSource için standart sözleşme
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: dsindona
ms.openlocfilehash: facb500299f7196e0e1387c3a7267a917d13a3a6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81681458"
---
# <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>Microsoft ticari Market için standart sözleşme

Microsoft, müşterilerin tedarik sürecini basitleştirmek ve yazılım satıcılarının yasal karmaşıklığını azaltmak için Microsoft ticari Market 'e yönelik standart bir sözleşme sunarak Market 'teki işlemleri kolaylaştırmaya yardımcı olur. Ticari Market yayımcıları, özel hüküm ve koşulları ortadan kaldırmaktansa, müşterilerin yalnızca her bir kez ve kabul etmesi gereken standart sözleşme altında yazılımlarını sunmayı tercih edebilir. Standart Sözleşme şurada bulunabilir: [https://go.microsoft.com/fwlink/?linkid=2041178](https://go.microsoft.com/fwlink/?linkid=2041178).

Teklif hüküm ve koşulları, teklif Iş Ortağı Merkezi 'nde oluşturulurken tanımlanmıştır. Kendi özel hüküm ve koşullarınızı sağlamak yerine Microsoft ticari Market için standart sözleşmeyi kullanmayı seçebilirsiniz.

>[!Note]
>Microsoft ticari Market için standart sözleşmeyi kullanarak bir teklif yayımladığınızda, kendi özel hüküm ve koşullarınızı kullanamazsınız. Bu bir "veya" senaryosudur. Çözümünüzü standart sözleşme kapsamında *ya* da kendi hüküm ve koşullarınız için sunun. Standart sözleşmenin koşullarını değiştirmek isterseniz, standart sözleşme değişiklikleri aracılığıyla bunu yapabilirsiniz.

## <a name="standard-contract-amendments"></a>Standart Sözleşme düzeltme açıklamaları

Standart Sözleşme, yayımcıların basitlik için standart sözleşmeyi seçmesini ve ürün ya da işletmelerinin Özelleştirilmiş şartlarını seçmesine izin verir. Müşterilerin, Microsoft standart sözleşmesini gözden geçirmiş ve kabul etmiş olmaları durumunda yalnızca sözleşmenin değişiklik yapması gerekir.

Ticari Market yayımcıları için iki çeşit değişiklik bulunur:

* Evrensel değişiklik: Bu değişiklik, tüm müşteriler için standart sözleşmeye evrensel olarak uygulanır. Evrensel değişiklik, satın alma akışındaki teklifin her müşterisi için gösterilir. Müşteriler teklifinizi kullanabilmeniz için standart sözleşmenin ve değişikliğin koşullarını kabul etmelidir.

* Özel değişiklik: Bu değişiklik, yalnızca Azure kiracı kimlikleri aracılığıyla belirli müşterileri hedefleyen standart sözleşmeye yönelik özel değişiklik amaçlıdır. Yayımcılar hedeflemek istedikleri kiracıyı seçebilirler. Yalnızca kiracının müşterilerine, teklifin satın alma akışındaki özel değişiklik koşulları sunulur.  Müşterilerin teklifinizi kullanabilmesi için önce standart sözleşme ve değişiklik koşullarını kabul etmesi gerekir.

>[!Note]
>Bu iki tür değişiklik yığını birbirlerinin üzerine. Özel değişiklik ile hedeflenen müşteriler, satın alma sırasında standart sözleşmeye evrensel değişiklik de alır.

Microsoft ticari Market için standart sözleşmede aşağıdaki teklif türleri için yararlanabilirsiniz: Azure uygulamaları (çözüm şablonları ve yönetilen uygulamalar), sanal makineler, kapsayıcılar, kapsayıcı uygulamaları, IoT Edge modüller ve SaaS.

## <a name="customer-experience"></a>Müşteri deneyimi

Azure Market veya AppSource 'ta bulma deneyimi sırasında müşteriler, Microsoft ticari Market ve tüm evrensel değişiklikler için sunulan standart sözleşme olarak teklifle ilişkili koşulları görebilirler.

![Azure portal müşteri bulma deneyimi.](media/marketplace-publishers-guide/azure-discovery-process.png)

Azure portal satın alma işlemi sırasında müşteriler, Microsoft ticari Market ve herhangi bir evrensel ve/veya kiracıya özgü değişiklik için standart sözleşme olarak teklifle ilişkili koşulları görebilirler.

![Azure portal müşteri satın alma deneyimi.](media/marketplace-publishers-guide/azure-purchase-process.png)

## <a name="api"></a>API

Müşteriler, bir teklifin şartlarını almak ve kabul etmek için Get-Azurermmarket Placeterms ' i kullanabilir. Standart Sözleşme ve ilişkili değişiklik, cmdlet 'in çıkışında döndürülür.
