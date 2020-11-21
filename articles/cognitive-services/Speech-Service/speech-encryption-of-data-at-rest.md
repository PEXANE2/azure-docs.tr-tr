---
title: Bekleyen verilerin konuşma hizmeti şifrelemesi
titleSuffix: Azure Cognitive Services
description: Microsoft, Microsoft tarafından yönetilen şifreleme anahtarları sunar ve ayrıca bilişsel hizmetler aboneliklerinizi, müşteri tarafından yönetilen anahtarlar (CMK) olarak adlandırılan kendi Anahtarlarınıza göre yönetmenizi sağlar. Bu makalede, konuşma hizmeti için bekleyen veri şifrelemesi ele alınmaktadır.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: egeaney
ms.openlocfilehash: aa0fe33dff0161767b74546aad49003d8fc70c16
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95015265"
---
# <a name="speech-service-encryption-of-data-at-rest"></a>Bekleyen verilerin konuşma hizmeti şifrelemesi

Konuşma hizmeti, verileri buluta kalıcı hale geldiğinde otomatik olarak şifreler. Konuşma hizmeti şifrelemesi, verilerinizi korur ve kurumsal güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur.

## <a name="about-cognitive-services-encryption"></a>Bilişsel hizmetler şifreleme hakkında

[Fıps 140-2](https://en.wikipedia.org/wiki/FIPS_140-2) uyumlu [256 bit AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) şifrelemesi kullanılarak veriler şifrelenir ve şifresi çözülür. Şifreleme ve şifre çözme saydamdır, bu da şifreleme ve erişimin sizin için yönetilip yönetilmediğini belirtir. Verileriniz varsayılan olarak koruma altındadır ve şifrelemeden yararlanmak için kodunuzu veya uygulamalarınızı değiştirmenize gerek yoktur.

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Özel Konuşma Tanıma ve özel ses kullandığınızda, konuşma hizmeti aşağıdaki verileri bulutta saklayabilir:  

* Konuşma izleme verileri-yalnızca özel uç noktanıza yönelik izlemeyi etkinleştirirseniz
* Eğitim ve test verileri karşıya yüklendi

Varsayılan olarak, verileriniz Microsoft 'un depolama alanında depolanır ve aboneliğiniz Microsoft tarafından yönetilen şifreleme anahtarlarını kullanır. Ayrıca kendi depolama hesabınızı hazırlama seçeneğiniz de vardır. Mağaza erişimi yönetilen kimlik tarafından yönetilir ve konuşma hizmeti konuşma izleme verileri, özelleştirme eğitimi verileri ve özel modeller gibi kendi verilerinize doğrudan erişemez.

Yönetilen kimlik hakkında daha fazla bilgi için bkz. [Yönetilen kimlikler nelerdir](../../active-directory/managed-identities-azure-resources/overview.md).

## <a name="bring-your-own-storage-byos-for-customization-and-logging"></a>Özelleştirme ve günlüğe kaydetme için kendi depolama alanınızı (BYOS) getirme

Kendi depolama alanınızı getirmek üzere erişim istemek için, [konuşma hizmetini doldurun ve kendi depolama alanını getir (KCG) istek formunu alın](https://aka.ms/cogsvc-cmk). Onaylandığında, özelleştirme ve günlüğe kaydetme için gereken verileri depolamak üzere kendi depolama hesabınızı oluşturmanız gerekir. Bir depolama hesabı eklerken, konuşma hizmeti kaynağı sistem tarafından atanmış bir yönetilen kimliği etkinleştirir. Sistem tarafından atanan yönetilen kimlik etkinleştirildikten sonra, bu kaynak Azure Active Directory (AAD) ile kaydedilir. Kaydolduktan sonra, yönetilen kimliğe depolama hesabına erişim verilir. Burada Yönetilen kimlikler hakkında daha fazla bilgi edinebilirsiniz. Yönetilen kimlik hakkında daha fazla bilgi için bkz. [Yönetilen kimlikler nelerdir](../../active-directory/managed-identities-azure-resources/overview.md).

> [!IMPORTANT]
> Sistem tarafından atanan yönetilen kimlikleri devre dışı bırakırsanız, depolama hesabına erişim kaldırılır. Bu, konuşma hizmetinin depolama hesabına erişmesi gereken bölümlerinin çalışmayı durdurmasına neden olur.  

Konuşma hizmeti şu anda Müşteri Kasası desteklememektedir. Ancak, müşteri verileri KCG kullanılarak depolanabilir ve [müşteri kasası](../../security/fundamentals/customer-lockbox-overview.md)için benzer veri denetimleri elde etmenizi sağlar. Konuşma hizmeti verilerinin, konuşma kaynağının oluşturulduğu bölgede kaldığından ve işlendiğini aklınızda bulundurun. Bu, bekleyen veriler ve iletim verileri için geçerlidir. Özel Konuşma Tanıma ve özel ses gibi özelleştirme özellikleri kullanılırken, tüm müşteri verileri, KCG (kullanılıyorsa) ve konuşma hizmeti kaynağının bulunduğu bölgede aktarılır, depolanır ve işlenir.

> [!IMPORTANT]
> Microsoft, konuşma modellerini geliştirmek için müşteri **verilerini kullanmaz.** Ayrıca, uç nokta günlüğü devre dışıysa ve hiçbir özelleştirme kullanılmazsa, hiçbir müşteri verisi depolanmaz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmeti-kendi depolama alanınızı getir (KCG) istek formu](https://aka.ms/cogsvc-cmk)
* [Yönetilen kimlikler nelerdir?](../../active-directory/managed-identities-azure-resources/overview.md)