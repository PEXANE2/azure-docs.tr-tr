---
title: Veri konuşma hizmeti şifrelemesi
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti veri şifrelemesi.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: 3ccc9820f38a8c32d0b390663eb6b4430b42e8f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79372364"
---
# <a name="speech-service-encryption-of-data-at-rest"></a>Veri konuşma hizmeti şifrelemesi

Konuşma Hizmeti, verilerinizi bulutta kalıcı hale geldiğinde otomatik olarak şifreler. Konuşma hizmeti şifrelemesi verilerinizi korur ve kuruluş güvenliği ve uyumluluk taahhütlerinizi yerine getirmenize yardımcı olur.

## <a name="about-cognitive-services-encryption"></a>Bilişsel Hizmetler şifrelemesi hakkında

Veriler [FIPS 140-2](https://en.wikipedia.org/wiki/FIPS_140-2) uyumlu [256 bit AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) şifreleme kullanılarak şifrelenir ve şifresi çözülür. Şifreleme ve şifre çözme saydamdır, bu da şifreleme ve erişimin sizin için yönetildiği anlamına gelir. Verileriniz varsayılan olarak güvenlidir ve şifrelemeden yararlanmak için kodunuzu veya uygulamalarınızı değiştirmeniz gerekmez.

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Özel Konuşma ve Özel Ses kullandığınızda, Konuşma hizmeti aşağıdaki verileri bulutta depolayabilir:  

* Konuşma izleme verileri - yalnızca özel bitiş noktanız için izleme yi açarsanız
* Yüklenen eğitim ve test verileri

Varsayılan olarak, verileriniz Microsoft'un depolama alanında depolanır ve aboneliğiniz Microsoft tarafından yönetilen şifreleme anahtarlarını kullanır. Ayrıca kendi depolama hesabınızı hazırlama seçeneğiniz de var. Mağazaya erişim Yönetilen Kimlik tarafından yönetilir ve Konuşma hizmeti konuşma izleme verileri, özelleştirme eğitim verileri ve özel modeller gibi kendi verilerinize doğrudan erişemez.

Yönetilen Kimlik hakkında daha fazla bilgi için yönetilen [kimliklerin neler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)olduğunu görün.

## <a name="bring-your-own-storage-byos-for-customization-and-logging"></a>Özelleştirme ve günlüğe kaydetme için kendi depolama alanınızı (BYOS) getirin

Kendi depolama alanınızı getirmek için erişim talebinde bulunmak için Konuşma hizmetini doldurun ve gönderin [- kendi depolama alanınızı (BYOS) talep formu getirin.](https://aka.ms/cogsvc-cmk) Onaylandıktan sonra, özelleştirme ve günlüğe kaydetme için gereken verileri depolamak için kendi depolama hesabınızı oluşturmanız gerekir. Bir depolama hesabı eklerken, Konuşma hizmeti kaynağı yönetilen kimlik atanmış bir sistem sağlar. Atanan yönetilen kimlik sistem etkinleştirildikten sonra, bu kaynak Azure Etkin Dizini'ne (AAD) kaydedilir. Kaydedildikten sonra, yönetilen kimlik depolama hesabına erişim hakkı verilecektir. Yönetilen Kimlikler hakkında daha fazla bilgi için burada bulabilirsiniz. Yönetilen Kimlik hakkında daha fazla bilgi için yönetilen [kimliklerin neler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)olduğunu görün.

> [!IMPORTANT]
> Yönetilen kimlikler atanan sistemi devre dışı btanırsanız, depolama hesabına erişim kaldırılır. Bu, Konuşma hizmetinin depolama hesabına erişim gerektiren bölümlerinin çalışmayı durdurmasına neden olur.  

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

BYOS şu anda şu bölgelerde kullanılabilir:

* ABD Orta Güney
* Batı ABD 2
* Doğu ABD

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmeti - kendi depolama alanınızı (BYOS) talep formuna getirin](https://aka.ms/cogsvc-cmk)
* [Yönetilen kimlikler nelerdir.](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)
