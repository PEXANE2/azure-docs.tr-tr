---
title: Azure Active Directory işlemler başvuru kılavuzu
description: Bu işlemler başvuru kılavuzu, kimlik ve erişim yönetimi, kimlik doğrulama, idare ve işlemleri korumak için gerçekleştirmeniz gereken denetimleri ve eylemleri açıklar
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
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74535320"
---
# <a name="azure-active-directory-operations-reference-guide"></a>Azure Active Directory işlemler başvuru kılavuzu

Bu işlemler başvuru kılavuzu, güvenli hale getirmek ve aşağıdaki alanların bakımını yapmak için gerçekleştirmeniz gereken denetimleri ve eylemleri açıklar:

- **[Kimlik ve erişim yönetimi](active-directory-ops-guide-iam.md)** -kimliklerin yaşam döngüsünü ve yetkilendirmelerini yönetme özelliği.
- **[Kimlik doğrulama Yönetimi](active-directory-ops-guide-auth.md)** -kimlik bilgilerini yönetme, kimlik doğrulama deneyimini tanımlama, atama atama, kullanım ölçümü ve kurumsal güvenlik duruşunu temel alan erişim ilkelerini tanımlama özelliği.
- **[İdare](active-directory-ops-guide-govern.md)** özelliği, erişimi değerlendirme ve test etme yetkisi verilen ayrıcalıksız ve ayrıcalıklı kimlikler, denetim ve ortamda Denetim değişiklikleri yapma yeteneği sağlar.
- **[İşlemler](active-directory-ops-guide-ops.md)** -Azure Active Directory (Azure AD) işlemleri iyileştirin.

Buradaki bazı öneriler, tüm müşterilerin ortamları için geçerli olmayabilir. Örneğin, kuruluşunuz Parola karması eşitlemesi kullanıyorsa en iyi AD FS uygulamalar uygulanmayabilir.

> [!NOTE]
> Bu öneriler, yayımlama tarihi itibariyle geçerli olmakla kalmaz, zaman içinde değişebilir. Kuruluşlar, Microsoft ürün ve Hizmetleri zaman içinde geliştikçe kimlik uygulamalarını sürekli olarak değerlendirmelidir. Kuruluşlar farklı bir Azure AD Premium lisansına abone olduğunda öneriler değişebilir. Örneğin, Azure AD Premium P2 daha fazla idare önerisi içerecektir.

## <a name="stakeholders"></a>Proje Katılımcıları

Bu başvuru kılavuzundaki her bölüm, paydaşların anahtar görevlerini başarıyla planlamak ve uygulamak üzere atanmasını önerir. Aşağıdaki tabloda, bu kılavuzdaki tüm paydaşların listesi özetlenmektedir:

| Proje katılımcısı | Açıklama |
| :- | :- |
| IAM Işlemler ekibi | Bu ekip, kimlik ve erişim yönetimi sisteminin günlük işlemlerini yönetmeyi işler |
| Üretkenlik ekibi | Bu ekip, e-posta, dosya paylaşımı ve işbirliği, anlık mesajlaşma ve konferans gibi üretkenlik uygulamalarına sahiptir ve bunları yönetir. |
| Uygulama sahibi | Bu ekip, bir işletmeden belirli bir uygulamaya sahiptir ve genellikle bir kuruluştaki teknik bir perspektife aittir. |
| InfoSec mimarisi ekibi | Bu takım bir kuruluşun bilgi güvenliği uygulamalarını planlar ve tasarlıyor. |
| InfoSec Işlemler ekibi | Bu ekip çalışır ve INFOSEC mimari ekibinin uygulanan bilgi güvenliği uygulamalarını izler. |

## <a name="next-steps"></a>Sonraki adımlar

[Kimlik ve erişim yönetimi denetimleri ve eylemleri](active-directory-ops-guide-iam.md)ile çalışmaya başlayın.
