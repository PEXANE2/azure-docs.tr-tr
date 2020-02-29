---
title: Azure Key Vault için güvenlik önerileri
description: Azure Key Vault için güvenlik önerileri. Bu kılavuzun uygulanması, paylaşılan sorumluluk modelinizde açıklandığı şekilde güvenlik yükümlülüklerinizi karşılamanız için yardımcı olur
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
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78192379"
---
# <a name="security-recommendations-for-azure-key-vault"></a>Azure Key Vault için güvenlik önerileri

Bu makale Azure Key Vault için güvenlik önerileri içerir. Bu önerilerin uygulanması, paylaşılan sorumluluk modelinizde açıklandığı gibi güvenlik yükümlülüklerinizi karşılaalmanıza yardımcı olur. Microsoft 'un hizmet sağlayıcısı sorumluluklarını karşılama hakkında daha fazla bilgi için, [bulut bilgi işlem Için paylaşılan sorumlulukları](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91)okuyun.

Bu makaleye eklenen önerilerden bazıları Azure Güvenlik Merkezi tarafından otomatik olarak izlenebilir. Azure Güvenlik Merkezi, Azure 'daki kaynaklarınızı korumaya yönelik ilk savunma hattınızdır. Olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli olarak analiz eder. Daha sonra bunları nasıl ele almak için öneriler sağlar.

- Azure Güvenlik Merkezi önerileri hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'Nde güvenlik önerileri](../security-center/security-center-recommendations.md).
- Azure Güvenlik Merkezi hakkında bilgi için bkz. [Azure Güvenlik Merkezi nedir?](../security-center/security-center-intro.md)

## <a name="data-protection"></a>Veri koruma

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
|Geçici silmeyi etkinleştir | [Geçici silme](key-vault-ovw-soft-delete.md) , silinen kasaların ve kasa nesnelerinin kurtarılmasına olanak tanır |  - |
| Kasa verilerine erişimi sınırlandırma  | En az ayrıcalık ilkesini izleyin ve kuruluşunuzun hangi üyelerinin kasa verilerine erişimi olduğunu sınırlayın |  - |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
| Katkıda bulunan erişimi olan kullanıcı sayısını sınırlayın | Bir kullanıcının bir Anahtar Kasası yönetim düzlemine katkıda bulunan izinleri varsa, Kullanıcı Key Vault erişim ilkesi ayarlayarak kendilerine veri düzlemine erişim izni verebilir. Anahtar kasalarınıza katılımcı rolü erişimi olan kişiyi sıkı bir şekilde denetleyebilirsiniz. Yalnızca erişim izni olan kişilerin kasalarınıza erişebildiğinden ve yönetmesine izin aldığından emin olun. [Anahtar kasasına güvenli erişimi](key-vault-secure-your-key-vault.md) okuyabilirsiniz | - |

## <a name="monitoring"></a>İzleme

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
 Key Vault tanılama günlükleri etkinleştirilmelidir | Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. | [Evet](../security-center/security-center-identity-access.md) |
| Azure Anahtar Kasası günlüklerinizi kimlerin erişebileceğini kısıtlayın | [Key Vault Günlükler](key-vault-logging.md) , kasalarınızda gerçekleştirilen ve kasa, anahtar ve gizli dizileri oluşturma veya silme gibi, araştırma sırasında kullanılabilir olan etkinlikler hakkındaki bilgileri kaydeder |  - |

## <a name="networking"></a>Ağ

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
|Ağ pozlamasını sınırlandırma | Ağ erişimi, kasa erişimi gerektiren çözümler tarafından kullanılan sanal ağlarla sınırlı olmalıdır. [Azure Key Vault Için sanal ağ hizmeti uç noktalarına](key-vault-overview-vnet-service-endpoints.md) ilişkin bilgileri gözden geçirin | - |

## <a name="next-steps"></a>Sonraki adımlar

Ek güvenlik gereksinimleri olup olmadığını görmek için uygulama sağlayıcınıza danışın. Güvenli uygulamalar geliştirme hakkında daha fazla bilgi için bkz. [Güvenli Geliştirme belgeleri](../security/fundamentals/abstract-develop-secure-apps.md).
