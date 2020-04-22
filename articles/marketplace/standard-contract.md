---
title: Standart Sözleşme | Azure Marketi
description: Azure Marketi ve AppSource için Standart Sözleşme
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: dsindona
ms.openlocfilehash: facb500299f7196e0e1387c3a7267a917d13a3a6
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81681458"
---
# <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>Microsoft ticari pazar için Standart Sözleşme

Müşteriler için satın alma işlemini kolaylaştırmak ve yazılım satıcıları için yasal karmaşıklığı azaltmak için Microsoft, pazardaki işlemleri kolaylaştırmak için Microsoft ticari pazar yeri için standart bir sözleşme sunar. Ticari pazar yeri yayıncıları, özel hüküm ve koşulları oluşturmayerine, müşterilerin yalnızca bir kez inceleyip kabul etmeleri gereken Standart Sözleşme kapsamında yazılımlarını sunmayı seçebilirler. Standart Sözleşme burada bulunabilir: [https://go.microsoft.com/fwlink/?linkid=2041178](https://go.microsoft.com/fwlink/?linkid=2041178).

Teklif, İş Ortağı Merkezi'nde teklif oluşturulurken teklifin hüküm ve koşulları tanımlanır. Kendi özel hüküm ve koşullarınızı sağlamak yerine Microsoft ticari pazarı için Standart Sözleşme'yi kullanmayı seçebilirsiniz.

>[!Note]
>Microsoft ticari pazarı için Standart sözleşmeyi kullanarak bir teklif yayımladıktan sonra, kendi özel hüküm ve koşullarınızı kullanamazsınız. Bu bir "veya" senaryosudur. Çözümünüzü Standart Sözleşme kapsamında *veya* kendi hüküm ve koşullarınız altında sunarsınız. Standart Sözleşme'nin koşullarını değiştirmek isterseniz, bunu Standart Sözleşme Değişiklikleri yoluyla yapabilirsiniz.

## <a name="standard-contract-amendments"></a>Standart Sözleşme Değişiklikleri

Standart Sözleşme Değişiklikleri, yayıncıların basitlik için standart sözleşmeyi ve ürünleri veya işleri için özelleştirilmiş koşulları seçmelerine olanak sağlar. Müşterilerin yalnızca Microsoft Standart Sözleşmesi'ni gözden geçirip kabul etmişlerse sözleşmedeki değişiklikleri gözden geçirmeleri gerekir.

Ticari pazar yeri yayıncıları için iki tür değişiklik vardır:

* Evrensel Değişiklikler: Bu değişiklikler tüm müşteriler için Standart Sözleşme'ye evrensel olarak uygulanır. Evrensel değişiklikler, teklifin her müşterisine satın alma akışında gösterilir. Müşteriler, teklifinizi kullanabilmeleri için Standart Sözleşme'nin koşullarını ve değişikliği kabul etmelidir.

* Özel Değişiklikler: Bu değişiklikler, Standart Sözleşme'de belirli müşterileri yalnızca Azure kiracı tbm'leri aracılığıyla hedefleyen özel değişikliklerdir. Yayıncılar hedeflemek istedikleri kiracıyı seçebilirler. Yalnızca kiracının müşterilerine teklifin satın alma akışında özel değişiklik koşulları sunulacaktır.  Müşteriler, teklifinizi kullanabilmeleri için Standart Sözleşme'nin ve değişiklik koşullarını kabul etmelidir.

>[!Note]
>Bu iki değişiklik türü üst üste yığ. Özel değişikliklerle hedeflenen müşteriler, satın alma sırasında Standart Sözleşme'de evrensel değişiklik de elde edeceklerdir.

Azure Uygulamaları (Çözüm Şablonları ve Yönetilen Uygulamalar), Sanal Makineler, Kapsayıcılar, Kapsayıcı Uygulamaları, IoT Edge Modülleri ve SaaS: Aşağıdaki teklif türleri için Microsoft ticari pazarı için Standart Sözleşme'den yararlanabilirsiniz.

## <a name="customer-experience"></a>Müşteri deneyimi

Azure pazar yeri veya AppSource'daki keşif deneyimi sırasında, müşteriler teklifle ilişkili koşulları Microsoft ticari pazarı için Standart Sözleşme ve evrensel değişiklikler olarak görebilir.

![Azure portalı müşteri bulma deneyimi.](media/marketplace-publishers-guide/azure-discovery-process.png)

Azure portalındaki satın alma işlemi sırasında müşteriler, teklifle ilişkili koşulları Microsoft ticari pazarı için Standart Sözleşme ve evrensel ve/veya kiracıya özel değişiklikler olarak görebilir.

![Azure portalı müşteri satın alma deneyimi.](media/marketplace-publishers-guide/azure-purchase-process.png)

## <a name="api"></a>API

Müşteriler, bir teklifin koşullarını almak ve kabul etmek için AzureRmMarketplaceTerms'i kullanabilir. Standart Sözleşme ve ilgili değişiklikler cmdlet çıktısı iade edilecektir.
