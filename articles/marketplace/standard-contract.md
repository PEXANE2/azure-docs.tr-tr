---
title: Microsoft ticari Market için standart sözleşme
description: Azure Marketi ve Iş Ortağı Merkezi 'nde AppSource için standart sözleşme
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: trkeya
ms.author: trkeya
ms.date: 05/20/2020
ms.openlocfilehash: 20a257bde6022249fd7b2ab875b94f356234b490
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94488885"
---
# <a name="standard-contract-for-microsoft-commercial-marketplace"></a>Microsoft ticari Market için standart sözleşme

Microsoft, Microsoft ticari Market için standart bir sözleşme sunmaktadır. Bu, müşterilerin tedarik sürecini basitleştirmeye, yazılım satıcıları için yasal Karmaşıklığın azaltılmasına ve Market 'teki işlemleri kolaylaştırmaya yardımcı olur. Tek bir market yayımcısı olarak özel hüküm ve koşullar oluşturmanız yerine, yazılımınızı [standart sözleşme](https://go.microsoft.com/fwlink/?linkid=2041178)kapsamında sunmayı seçebilirsiniz ve bu da müşterilerin yalnızca her zaman bir kez ve kabul etmesi gerekir.

Teklif hüküm ve koşulları, teklif Iş Ortağı Merkezi 'nde oluşturulurken tanımlanmıştır. Kendi özel hüküm ve koşullarınızı sağlamak yerine Microsoft ticari Market için standart sözleşmeyi kullanmayı seçebilirsiniz.

>[!Note]
>Microsoft ticari Market için standart sözleşmeyi kullanarak bir teklif yayımladığınızda, kendi özel hüküm ve koşullarınızı kullanamazsınız. Çözümünüzü standart sözleşme kapsamında *ya* da kendi hüküm ve koşullarınız için sunun. Özel hüküm ve koşullar, teklif düzeyinde tanımlanmıştır ve tüm planlara uygulanır; Iş Ortağı Merkezi ' nde teklifinizin **Özellikler** sayfasında özel hüküm ve koşullarınızı yazın. Standart sözleşmenin şartlarını değiştirmek isterseniz, bunu standart sözleşme değişiklikleri aracılığıyla yapabilirsiniz.

## <a name="standard-contract-amendments"></a>Standart Sözleşme düzeltme açıklamaları

Standart Sözleşme, yayımcıların basitlik için standart sözleşmeyi seçmesini ve ürün ya da işletmelerinin Özelleştirilmiş şartlarını seçmesine izin verir. Müşterilerin, Microsoft standart sözleşmesini gözden geçirmiş ve kabul etmiş olmaları durumunda yalnızca sözleşmenin değişiklik yapması gerekir.

Ticari Market yayımcıları için iki çeşit değişiklik bulunur:

* Evrensel değişiklik: Bu değişiklik, tüm müşteriler için standart sözleşmeye evrensel olarak uygulanır. Evrensel değişiklik, satın alma akışındaki teklifin her müşterisi için gösterilir. Müşteriler teklifinizi kullanabilmeniz için standart sözleşmenin ve değişikliğin koşullarını kabul etmelidir.

* Özel değişiklik: Bu değişiklik, yalnızca Azure kiracı kimlikleri aracılığıyla belirli müşterileri hedefleyen standart sözleşmeye yönelik özel değişiklik amaçlıdır. Yayımcılar hedeflemek istedikleri kiracıyı seçebilirler. Yalnızca kiracının müşterilerine, teklifin satın alma akışındaki özel değişiklik koşulları sunulur.  Müşterilerin teklifinizi kullanabilmesi için önce standart sözleşme ve değişiklik koşullarını kabul etmesi gerekir.

>[!Note]
>Bu iki tür değişiklik yığını birbirlerinin üzerine. Özel değişiklik ile hedeflenen müşteriler, satın alma sırasında standart sözleşmeye evrensel değişiklik de alır. Değişiklik sayısı, boşluklar dahil 4000 karakterle sınırlıdır.

Aşağıdaki teklif türleri için Microsoft ticari Marketi için standart sözleşme özelliğinden yararlanabilirsiniz: Azure uygulamaları (çözüm şablonları ve yönetilen uygulamalar), sanal makineler ve SaaS.

## <a name="customer-experience"></a>Müşteri deneyimi

Azure Market veya AppSource 'ta bulma deneyimi sırasında müşteriler, Microsoft ticari Market ve tüm evrensel değişiklikler için sunulan standart sözleşme olarak teklifle ilişkili koşulları görebilirler.

![Azure portal müşteri bulma deneyimi.](media/marketplace-publishers-guide/azure-discovery-process.png)

Azure portal satın alma işlemi sırasında müşteriler, Microsoft ticari Market ve herhangi bir evrensel ve/veya kiracıya özgü değişiklik için standart sözleşme olarak teklifle ilişkili koşulları görebilirler.

![Azure portal müşteri satın alma deneyimi.](media/marketplace-publishers-guide/azure-purchase-process.png)

## <a name="api"></a>API

Müşteriler bir teklif şartlarını almak ve kabul etmek için Get-AzureRmMarketplaceTerms kullanabilir. Standart Sözleşme ve ilişkili değişiklik, cmdlet 'in çıkışında döndürülür.
