---
title: dosya dahil etme
description: Microsoft ticari Market standart sözleşmesi için metin dosyasını dahil et
documentationcenter: partner-center-commercial-marketplace
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: include
ms.date: 01/22/2020
ms.custom: include file
ms.openlocfilehash: 5cbddaed1078ade005c3d60df3face9580bf6609
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86121987"
---
Microsoft, ticari Market için standart bir sözleşme şablonu sağlar.

- **Microsoft ticari Market için standart sözleşme kullanılsın mı?**

Microsoft, müşterilerin tedarik sürecini basitleştirmek ve yazılım satıcılarının yasal karmaşıklığını azaltmak için Microsoft ticari Market 'e yönelik standart bir sözleşme sunarak Market 'teki işlemleri kolaylaştırmaya yardımcı olur. Ticari Market yayımcıları, özel hüküm ve koşulları ortadan kaldırmaktansa, müşterilerin yalnızca her bir kez ve kabul etmesi gereken standart sözleşme altında yazılımlarını sunmayı tercih edebilir. Standart Sözleşme şurada bulunabilir: https://go.microsoft.com/fwlink/?linkid=2041178 .

"Ticari Market için standart sözleşmeyi kullan" onay kutusunu seçerek, kendi özel hüküm ve koşullarınızı sağlamak yerine standart sözleşmeyi kullanmayı seçebilirsiniz.

![Standart Sözleşme onay kutusunu kullanma](./media/use-standard-contract.png)

> [!NOTE]
> Microsoft ticari Market için standart sözleşmeyi kullanarak bir teklif yayımladığınızda, kendi özel hüküm ve koşullarınızı kullanamazsınız. Bu bir "veya" senaryosudur. Çözümünüzü standart sözleşme kapsamında **ya** da kendi hüküm ve koşullarınız için sunun. Standart sözleşmenin şartlarını değiştirmek isterseniz, bunu standart sözleşme değişiklikleri aracılığıyla yapabilirsiniz.

**Standart Sözleşme düzeltme açıklamaları**

Standart anlaşma değişikliği, yayımcıların basitlik için standart sözleşme koşullarını seçmesini ve ürün ya da işletmelerinin koşullarını özelleştirmesini sağlar. Müşterilerin, Microsoft standart sözleşmesini gözden geçirmiş ve kabul etmiş olmaları durumunda yalnızca sözleşmenin değişiklik yapması gerekir.

Ticari Market yayımcıları için iki çeşit değişiklik bulunur:

- Evrensel değişiklik: Bu değişiklik, tüm müşteriler için standart sözleşmeye evrensel olarak uygulanır. Evrensel değişiklik, satın alma akışındaki teklifin her müşterisi için gösterilir. Müşteriler teklifinizi kullanabilmeniz için standart sözleşmenin ve değişikliğin koşullarını kabul etmelidir.
- Özel değişiklik: Bu değişiklik, yalnızca Azure kiracı kimlikleri aracılığıyla belirli müşterileri hedefleyen standart sözleşmeye yönelik özel değişiklik amaçlıdır. Yayımcılar hedeflemek istedikleri kiracıyı seçebilirler. Yalnızca kiracının müşterilerine, teklifin satın alma akışındaki özel değişiklik koşulları sunulur.  Müşterilerin teklifinizi kullanabilmesi için önce standart sözleşme ve değişiklik koşullarını kabul etmesi gerekir.

>[!NOTE]
> Bu iki tür değişiklik yığını birbirlerinin üzerine. Özel değişiklik ile hedeflenen müşteriler, satın alma sırasında standart sözleşmeye evrensel değişiklik de alır.

**Evrensel değişiklik Microsoft 'un ticari Market 'e yönelik standart sözleşme**: bu kutuya evrensel düzeltme koşullarını girin. Teklif başına tek bir evrensel düzeltme sağlayabilirsiniz. Bu kutuya sınırsız sayıda karakter girebilirsiniz. Bu terimler, bulma ve satın alma akışında AppSource, Azure Marketi 'nde müşterilere ve/veya Azure portal görüntülenir.

**Microsoft 'un ticari marketi Için standart sözleşmeye yönelik özel değişiklik koşulları**: **özel değişiklik koşulları Ekle**seçeneğini belirleyerek başlayın. Teklif başına en fazla 10 özel düzeltme terimi sağlayabilirsiniz.

- **Özel değişiklik koşulları**: özel değişiklik koşulları kutusunda özel değişiklik koşullarınızı girin. Bu kutuya sınırsız sayıda karakter girebilirsiniz. Yalnızca bu özel şartlar için belirttiğiniz kiracı kimliklerinin müşterileri, Azure portal teklifin satın alma akışındaki özel değişiklik hükümleriyle birlikte sunulacaktır.  
- **Kiracı kimlikleri** (gerekli): her özel değişiklik, en fazla 20 Kiracı kimliğini hedefleyebilir. Özel bir değişiklik eklerseniz, en az bir kiracı KIMLIĞI sağlamanız gerekir. Kiracı kimliği, Azure 'da müşterinizin kimliğini tanımlar. Müşterinizden bu KIMLIĞI sorabilir ve portal.azure.com > Azure Active Directory > özelliklerine giderek bulabilecekleri. Dizin KIMLIĞI değeri kiracı KIMLIĞIDIR (örneğin, 50c464d3-4930-494c-963c-1e951d15360e). Ayrıca, [Microsoft Azure ve Office 365 KIRACı kimliği olan](https://www.whatismytenantid.com)etki alanı adı URL 'sini kullanarak KURULUŞUNUZUN Kiracı kimliğini de arayabilirsiniz.
- **Açıklama** (isteğe bağlı): Isteğe bağlı olarak, Kiracı kimliği için, değişiklik yaparken hedeflediğiniz müşteriyi belirlemenize yardımcı olan kolay bir açıklama sağlayın.

**Hüküm ve koşullar**

Kendi özel hüküm ve koşullarınızı sağlamak istiyorsanız, bunları hüküm ve koşullar alanına girmeye karar verebilirsiniz. Bu alana en fazla 10.000 karakter girebilirsiniz. Hüküm ve koşullarınız daha uzun bir açıklama gerektiriyorsa, hüküm ve koşullarınızın bulunabileceği bu alana tek bir URL bağlantısı girin. Bu, müşterilere etkin bir bağlantı olarak görüntülenecektir.

Müşterilerinizin teklifinizi deneyebilmeleri için önce bu koşulları kabul etmesi gerekir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.
