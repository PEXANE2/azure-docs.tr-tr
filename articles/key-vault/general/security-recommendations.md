---
title: Azure Key Vault için güvenlik önerileri
description: Azure Key Vault için güvenlik önerileri. Bu kılavuzun uygulanması, paylaşılan sorumluluk modelinizde açıklandığı şekilde güvenlik yükümlülüklerinizi karşılamanız için yardımcı olur
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: article
ms.date: 09/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 0da1a3019124f62aba6a959ce9104c85bd85d3fc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81616483"
---
# <a name="security-recommendations-for-azure-key-vault"></a>Azure Key Vault için güvenlik önerileri

Bu makale Azure Key Vault için güvenlik önerileri içerir. Bu önerilerin uygulanması, paylaşılan sorumluluk modelinizde açıklandığı gibi güvenlik yükümlülüklerinizi karşılaalmanıza yardımcı olur. Microsoft 'un hizmet sağlayıcısı sorumluluklarını karşılama hakkında daha fazla bilgi için, [bulut bilgi işlem Için paylaşılan sorumlulukları](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91)okuyun.

Bu makaleye eklenen önerilerden bazıları Azure Güvenlik Merkezi tarafından otomatik olarak izlenebilir. Azure Güvenlik Merkezi, Azure 'daki kaynaklarınızı korumaya yönelik ilk savunma hattınızdır. Olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli olarak analiz eder. Daha sonra bunları nasıl ele almak için öneriler sağlar.

- Azure Güvenlik Merkezi önerileri hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'Nde güvenlik önerileri](../../security-center/security-center-recommendations.md).
- Azure Güvenlik Merkezi hakkında bilgi için bkz. [Azure Güvenlik Merkezi nedir?](../../security-center/security-center-intro.md)

## <a name="data-protection"></a>Veri koruma

| Öneri | Açıklamalar | Güvenlik Merkezi |
|-|----|--|
|Geçici silmeyi etkinleştir | [Geçici silme](overview-soft-delete.md)) Silinen kasaların ve kasa nesnelerinin kurtarılmasına olanak tanır |  - |
| Kasa verilerine erişimi sınırlandırma  | En az ayrıcalık ilkesini izleyin ve kuruluşunuzun hangi üyelerinin kasa verilerine erişimi olduğunu sınırlayın |  - |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Öneri | Açıklamalar | Güvenlik Merkezi |
|-|----|--|
| Katkıda bulunan erişimi olan kullanıcı sayısını sınırlayın | Bir kullanıcının bir Anahtar Kasası yönetim düzlemine katkıda bulunan izinleri varsa, Kullanıcı Key Vault erişim ilkesi ayarlayarak kendilerine veri düzlemine erişim izni verebilir. Anahtar kasalarınıza katılımcı rolü erişimi olan kişiyi sıkı bir şekilde denetleyebilirsiniz. Yalnızca erişim izni olan kişilerin kasalarınıza erişebildiğinden ve yönetmesine izin aldığından emin olun. [Bir Anahtar Kasası Için güvenli erişimi](secure-your-key-vault.md)okuyabilirsiniz) | - |

## <a name="monitoring"></a>İzleme

| Öneri | Açıklamalar | Güvenlik Merkezi |
|-|----|--|
 Key Vault'taki tanılama günlükleri etkinleştirilmelidir | Günlükleri etkinleştirin ve bir yıla kadar saklayın. Bu, bir güvenlik olayı gerçekleştiğinde veya ağınızın güvenliği tehlikeye atılırsa araştırma amaçlarıyla etkinlik izlerini yeniden oluşturmayı sağlar. | [Evet](../../security-center/security-center-identity-access.md) |
| Azure Anahtar Kasası günlüklerinizi kimlerin erişebileceğini kısıtlayın | [Key Vault Günlükler](logging.md)) kasanız, anahtarlar ve gizli dizileri oluşturma veya silme gibi kasalarınızda gerçekleştirilen etkinliklerle ilgili bilgileri kaydedin ve bir araştırma sırasında kullanılabilir |  - |

## <a name="networking"></a>Ağ

| Öneri | Açıklamalar | Güvenlik Merkezi |
|-|----|--|
|Ağ pozlamasını sınırlandırma | Ağ erişimi, kasa erişimi gerektiren çözümler tarafından kullanılan sanal ağlarla sınırlı olmalıdır. [Azure Key Vault Için sanal ağ hizmeti uç noktaları](overview-vnet-service-endpoints.md)hakkındaki bilgileri gözden geçirin) | - |

## <a name="next-steps"></a>Sonraki adımlar

Ek güvenlik gereksinimleri olup olmadığını görmek için uygulama sağlayıcınıza danışın. Güvenli uygulamalar geliştirme hakkında daha fazla bilgi için bkz. [Güvenli Geliştirme belgeleri](../../security/fundamentals/abstract-develop-secure-apps.md).
