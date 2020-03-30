---
title: Azure Active Directory işlemleri başvuru kılavuzu
description: Bu operasyon başvuru kılavuzu, kimlik ve erişim yönetimi, kimlik doğrulama, yönetim ve işlemleri güvence altına almak ve korumak için çekmeniz gereken denetimleri ve eylemleri açıklar
services: active-directory
author: martincoetzer
manager: daveba
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: fundamentals
ms.date: 10/31/2019
ms.author: martinco
ms.openlocfilehash: c815256e60e5ca6582220d247d587a4488c4a55a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74535320"
---
# <a name="azure-active-directory-operations-reference-guide"></a>Azure Active Directory işlemleri başvuru kılavuzu

Bu işlemler başvuru kılavuzu, aşağıdaki alanları korumak ve korumak için çekmeniz gereken denetimleri ve eylemleri açıklar:

- **[Kimlik ve erişim yönetimi](active-directory-ops-guide-iam.md)** - kimliklerin yaşam döngüsünü ve bunların yetkilerini yönetebilme becerisi.
- **[Kimlik doğrulama yönetimi](active-directory-ops-guide-auth.md)** - kimlik bilgilerini yönetme, kimlik doğrulama deneyimini tanımlama, temsilci atama, kullanımı ölçme ve erişim ilkelerini kurumsal güvenlik duruşuna dayalı olarak tanımlama becerisi.
- **[Yönetişim](active-directory-ops-guide-govern.md)** - çevreye verilen ayrıcalıklı olmayan ve ayrıcalıklı olmayan kimlikleri, denetim ve denetim değişikliklerini değerlendirme ve doğrulama becerisi.
- **[İşlemler](active-directory-ops-guide-ops.md)** - Işlemleri Azure Active Directory (Azure AD) optimize edin.

Buradaki bazı öneriler tüm müşterilerin ortamı için geçerli olmayabilir, örneğin, kuruluşunuz parola karma eşitlemesi kullanıyorsa AD FS en iyi uygulamaları uygulanmayabilir.

> [!NOTE]
> Bu öneriler yayımlama tarihi itibariyle geçerlidir, ancak zaman içinde değişebilir. Kuruluşlar, Microsoft ürünleri ve hizmetleri zaman içinde geliştikçe kimlik uygulamalarını sürekli olarak değerlendirmelidir. Kuruluşlar farklı bir Azure AD Premium lisansına abone olduğunda öneriler değişebilir. Örneğin, Azure AD Premium P2 daha fazla yönetim önerisi içerir.

## <a name="stakeholders"></a>Paydaş -ların

Bu başvuru kılavuzundaki her bölüm, önemli görevleri başarılı bir şekilde planlamak ve uygulamak için hissedarlar atamanızı önerir. Aşağıdaki tabloda bu kılavuzdaki tüm paydaşların listesi özetleme gösterilebilir:

| Paydaş | Açıklama |
| :- | :- |
| IAM Operasyon Ekibi | Bu ekip, Kimlik ve Erişim Yönetimi sisteminin günlük işlemlerini yönetir |
| Verimlilik Ekibi | Bu ekip, e-posta, dosya paylaşımı ve işbirliği, anlık ileti ve konferans gibi üretkenlik uygulamalarının sahibi ve yönetimidir. |
| Uygulama Sahibi | Bu ekip, bir işletmenin belirli uygulamasına ve genellikle bir kuruluştaki teknik perspektife sahip. |
| InfoSec Mimarlık Ekibi | Bu ekip, bir kuruluşun Bilgi Güvenliği uygulamalarını planlar ve tasarlar. |
| InfoSec Operasyon Ekibi | Bu ekip, InfoSec Architecture ekibinin uygulanan Bilgi Güvenliği uygulamalarını çalıştırıp izler. |

## <a name="next-steps"></a>Sonraki adımlar

Kimlik ve [erişim yönetimi denetimleri ve eylemleri](active-directory-ops-guide-iam.md)ile başlayın.
