---
title: Bekleyen verilerin konuşma hizmeti şifrelemesi
titleSuffix: Azure Cognitive Services
description: Bekleyen verilerin konuşma hizmeti şifrelemesi.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: 3ccc9820f38a8c32d0b390663eb6b4430b42e8f0
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79372364"
---
# <a name="speech-service-encryption-of-data-at-rest"></a>Bekleyen verilerin konuşma hizmeti şifrelemesi

Konuşma hizmeti, verileri buluta kalıcı hale geldiğinde otomatik olarak şifreler. Konuşma hizmeti şifrelemesi, verilerinizi korur ve kurumsal güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur.

## <a name="about-cognitive-services-encryption"></a>Bilişsel hizmetler şifreleme hakkında

[Fıps 140-2](https://en.wikipedia.org/wiki/FIPS_140-2) uyumlu [256 bit AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) şifrelemesi kullanılarak veriler şifrelenir ve şifresi çözülür. Şifreleme ve şifre çözme saydamdır, bu da şifreleme ve erişimin sizin için yönetilip yönetilmediğini belirtir. Verileriniz varsayılan olarak güvenlidir ve şifreleme avantajlarından yararlanmak için kodunuzu veya uygulamalarınızı değiştirmeniz gerekmez.

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Özel Konuşma Tanıma ve özel ses kullandığınızda, konuşma hizmeti aşağıdaki verileri bulutta saklayabilir:  

* Konuşma izleme verileri-yalnızca özel uç noktanıza yönelik izlemeyi etkinleştirirseniz
* Eğitim ve test verileri karşıya yüklendi

Varsayılan olarak, verileriniz Microsoft 'un depolama alanında depolanır ve aboneliğiniz Microsoft tarafından yönetilen şifreleme anahtarlarını kullanır. Ayrıca kendi depolama hesabınızı hazırlama seçeneğiniz de vardır. Mağaza erişimi yönetilen kimlik tarafından yönetilir ve konuşma hizmeti konuşma izleme verileri, özelleştirme eğitimi verileri ve özel modeller gibi kendi verilerinize doğrudan erişemez.

Yönetilen kimlik hakkında daha fazla bilgi için bkz. [Yönetilen kimlikler nelerdir](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

## <a name="bring-your-own-storage-byos-for-customization-and-logging"></a>Özelleştirme ve günlüğe kaydetme için kendi depolama alanınızı (BYOS) getirme

Kendi depolama alanınızı getirmek üzere erişim istemek için, [konuşma hizmetini doldurun ve kendi depolama alanını getir (KCG) istek formunu alın](https://aka.ms/cogsvc-cmk). Onaylandığında, özelleştirme ve günlüğe kaydetme için gereken verileri depolamak üzere kendi depolama hesabınızı oluşturmanız gerekir. Bir depolama hesabı eklerken, konuşma hizmeti kaynağı sistem tarafından atanmış bir yönetilen kimliği etkinleştirir. Sistem tarafından atanan yönetilen kimlik etkinleştirildikten sonra, bu kaynak Azure Active Directory (AAD) ile kaydedilir. Kaydolduktan sonra, yönetilen kimliğe depolama hesabına erişim verilir. Burada Yönetilen kimlikler hakkında daha fazla bilgi edinebilirsiniz. Yönetilen kimlik hakkında daha fazla bilgi için bkz. [Yönetilen kimlikler nelerdir](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

> [!IMPORTANT]
> Sistem tarafından atanan yönetilen kimlikleri devre dışı bırakırsanız, depolama hesabına erişim kaldırılır. Bu, konuşma hizmetinin depolama hesabına erişmesi gereken bölümlerinin çalışmayı durdurmasına neden olur.  

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

BYOS Şu anda şu bölgelerde kullanılabilir:

* ABD Orta Güney
* Batı ABD 2
* Doğu ABD

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmeti-kendi depolama alanınızı getir (KCG) istek formu](https://aka.ms/cogsvc-cmk)
* [Yönetilen kimlikler nelerdir?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)
