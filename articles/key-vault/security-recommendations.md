---
title: Azure Key Vault için güvenlik önerileri
description: Azure Key Vault için güvenlik önerileri. Bu kılavuzun uygulanması, paylaşılan sorumluluk modelinizde açıklandığı şekilde güvenlik yükümlülüklerinizi karşılamanız için yardımcı olur
services: key-vault
author: barclayn
manager: rkarlin
ms.service: key-vault
ms.topic: article
ms.date: 09/23/2019
ms.author: barclayn
ms.custom: security-recommendations
ms.openlocfilehash: 90965618b1d2cb126d56453d46a5bebc85319c0f
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71219634"
---
# <a name="security-recommendations-for-azure-key-vault"></a>Azure Key Vault için güvenlik önerileri

Bu makale Azure Key Vault için güvenlik önerileri içerir. Bu önerilerin uygulanması, paylaşılan sorumluluk modelinizde açıklandığı gibi güvenlik yükümlülüklerinizi karşılaalmanıza yardımcı olur. Microsoft 'un hizmet sağlayıcısı sorumluluklarını karşılama hakkında daha fazla bilgi için, [bulut bilgi işlem Için paylaşılan sorumlulukları](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf)okuyun.

Bu makaleye eklenen önerilerden bazıları Azure Güvenlik Merkezi tarafından otomatik olarak izlenebilir. Azure Güvenlik Merkezi, Azure 'daki kaynaklarınızı korumaya yönelik ilk savunma hattınızdır. Olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli olarak analiz eder. Daha sonra bunları nasıl ele almak için öneriler sağlar.

- Azure Güvenlik Merkezi önerileri hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'Nde güvenlik önerileri](../security-center/security-center-recommendations.md).
- Azure Güvenlik Merkezi hakkında bilgi için bkz. [Azure Güvenlik Merkezi nedir?](../security-center/security-center-intro.md)

## <a name="recommendations"></a>Öneriler

| Category | Öneri | Açıklamalar | Güvenlik Merkezi |
|-|-|----|--|
| Veri koruma |Geçici silmeyi etkinleştir | [Geçici silme](key-vault-ovw-soft-delete.md) , silinen kasaların ve kasa nesnelerinin kurtarılmasına olanak tanır |  - |
| Veri koruma | Kasa verilerine erişimi sınırlandırma  | En az ayrıcalık ilkesini izleyin ve kuruluşunuzun hangi üyelerinin kasa verilerine erişimi olduğunu sınırlayın |  - |
| Kimlik ve erişim yönetimi | Katkıda bulunan erişimi olan kullanıcı sayısını sınırlayın | Bir kullanıcının bir Anahtar Kasası yönetim düzlemine katkıda bulunan izinleri varsa, Kullanıcı Key Vault erişim ilkesi ayarlayarak kendilerine veri düzlemine erişim izni verebilir. Anahtar kasalarınıza katılımcı rolü erişimi olan kişiyi sıkı bir şekilde denetleyebilirsiniz. Yalnızca erişim izni olan kişilerin kasalarınıza erişebildiğinden ve yönetmesine izin aldığından emin olun. [Anahtar kasasına güvenli erişimi](key-vault-secure-your-key-vault.md) okuyabilirsiniz | - |
| İzleme | Key Vault tanılama günlükleri etkinleştirilmelidir | Günlükleri etkinleştirmek ve bunları bir yıla kadar korur. Bu, etkinlik kayıtlarını araştırma amacıyla bir güvenlik olayı ortaya veya ağınızın tehlikeye yeniden oluşturmanıza olanak sağlar. | [Evet](../security-center/security-center-identity-access.md) |
| İzleme | Azure Anahtar Kasası günlüklerinizi kimlerin erişebileceğini kısıtlayın | [Key Vault Günlükler](key-vault-logging.md) , kasalarınızda gerçekleştirilen ve kasa, anahtar ve gizli dizileri oluşturma veya silme gibi, araştırma sırasında kullanılabilir olan etkinlikler hakkındaki bilgileri kaydeder |  - |
| Ağ |Ağ pozlamasını sınırlandırma | Ağ erişimi, kasa erişimi gerektiren çözümler tarafından kullanılan sanal ağlarla sınırlı olmalıdır. [Azure Key Vault Için sanal ağ hizmeti uç noktalarına](key-vault-overview-vnet-service-endpoints.md) ilişkin bilgileri gözden geçirin | - |

## <a name="next-steps"></a>Sonraki adımlar

Ek güvenlik gereksinimleri olup olmadığını görmek için uygulama sağlayıcınıza danışın. Güvenli uygulamalar geliştirme hakkında daha fazla bilgi için bkz. [Güvenli Geliştirme belgeleri](../security/fundamentals/abstract-develop-secure-apps.md).
