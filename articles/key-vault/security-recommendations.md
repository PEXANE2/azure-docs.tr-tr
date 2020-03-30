---
title: Azure Anahtar Kasası için güvenlik önerileri
description: Azure Key Vault için güvenlik önerileri. Bu kılavuzun uygulanması, paylaşılan sorumluluk modelimizde açıklandığı gibi güvenlik yükümlülüklerinizin yerine getirilmesine yardımcı olacaktır
services: key-vault
author: barclayn
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: article
ms.date: 09/30/2019
ms.author: barclayn
ms.custom: security-recommendations
ms.openlocfilehash: 9fd41ae9b61a9c74fd5d99dd71199cfde06f5cb2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78192379"
---
# <a name="security-recommendations-for-azure-key-vault"></a>Azure Anahtar Kasası için güvenlik önerileri

Bu makale, Azure Anahtar Kasası için güvenlik önerileri içerir. Bu önerilerin uygulanması, paylaşılan sorumluluk modelimizde açıklandığı gibi güvenlik yükümlülüklerinizin yerine getirilmesine yardımcı olacaktır. Microsoft'un hizmet sağlayıcı sorumluluklarını yerine getirmek için ne yaptığı hakkında daha fazla bilgi için [bulut bilgi işlem için Paylaşılan sorumlulukları](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91)okuyun.

Bu makalede yer alan önerilerden bazıları Azure Güvenlik Merkezi tarafından otomatik olarak izlenebilir. Azure Güvenlik Merkezi, Azure'daki kaynaklarınızı korumada ilk savunma hattıdır. Olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli aralıklarla analiz eder. Daha sonra bunları nasıl ele almak için öneriler sağlar.

- Azure Güvenlik Merkezi önerileri hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'ndeki Güvenlik önerilerine](../security-center/security-center-recommendations.md)bakın.
- Azure Güvenlik Merkezi hakkında bilgi için [Azure Güvenlik Merkezi nedir'e bakın?](../security-center/security-center-intro.md)

## <a name="data-protection"></a>Veri koruma

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
|Yumuşak silmeyi etkinleştirme | [Yumuşak silme](key-vault-ovw-soft-delete.md) silinmiş kasaları ve kasa nesnelerini kurtarmanızı sağlar |  - |
| Kasa verilerine erişimi sınırlandırın  | En az ayrıcalık ilkesine uyun ve kuruluşunuzun hangi üyelerinin kasa verilerine erişebilen bilgileri sınırlandırın |  - |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
| Katılımcı erişimi olan kullanıcı sayısını sınırlama | Bir kullanıcının önemli bir kasa yönetim düzlemi için Katkıda Bulunan İzinleri varsa, anahtar kasa erişim ilkesi ayarlayarak kullanıcı veri düzlemine erişim izni verebilir. Anahtar kasalarınıza katkıda bulunan ların rol erişimine kimin sahip olduğunu sıkı bir şekilde kontrol edmelisiniz. Yalnızca erişim ihtiyacı olanlara erişebilmesini ve kasalarınıza erişebilmesini ve yönetebilmesini sağlayın. [Anahtar kasasına Güvenli erişimi](key-vault-secure-your-key-vault.md) okuyabilirsiniz | - |

## <a name="monitoring"></a>İzleme

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
 Key Vault'taki tanılama günlükleri etkinleştirilmelidir | Günlükleri etkinleştirin ve bir yıla kadar saklayın. Bu, bir güvenlik olayı meydana geldiğinde veya ağınız tehlikeye girdiğinde, araştırma amacıyla etkinlik izlerini yeniden oluşturmanıza olanak tanır. | [Evet](../security-center/security-center-identity-access.md) |
| Azure Anahtar kasa günlüklerinize kimlerin erişebileceğini kısıtlama | [Key Vault günlükleri,](key-vault-logging.md) kasanızın oluşturulması veya silinmesi, anahtarlar, sırlar gibi kasanızda gerçekleştirilen faaliyetler le ilgili bilgileri kaydeder ve bir soruşturma sırasında kullanılabilir |  - |

## <a name="networking"></a>Ağ Oluşturma

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
|Ağ maruziyetini sınırlandırın | Ağ erişimi, kasa erişimi gerektiren çözümler tarafından kullanılan sanal ağlarla sınırlı olmalıdır. [Azure Anahtar Kasası için Sanal ağ hizmeti uç noktaları hakkındaki](key-vault-overview-vnet-service-endpoints.md) bilgileri gözden geçirin | - |

## <a name="next-steps"></a>Sonraki adımlar

Ek güvenlik gereksinimleri olup olmadığını görmek için uygulama sağlayıcınıza danışın. Güvenli uygulamalar geliştirme hakkında daha fazla bilgi için [Güvenli Geliştirme Belgeleri'ne](../security/fundamentals/abstract-develop-secure-apps.md)bakın.
