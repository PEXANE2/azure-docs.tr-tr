---
title: Kuyruk depolama için güvenlik önerileri
titleSuffix: Azure Storage
description: Kuyruk depolama için güvenlik önerileri hakkında bilgi edinin. Bu kılavuzun uygulanması, paylaşılan sorumluluk modelinizde açıklandığı gibi güvenlik yükümlülüklerinizi karşılamanız için yardımcı olur.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 12/12/2019
ms.author: tamram
ms.custom: security-recommendations
ms.openlocfilehash: 1315e1f81ee32a544b623b7f3ffad61a7e7275d8
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75486293"
---
# <a name="security-recommendations-for-queue-storage"></a>Kuyruk depolama için güvenlik önerileri

Bu makale kuyruk depolaması için güvenlik önerileri içerir. Bu önerilerin uygulanması, paylaşılan sorumluluk modelinizde açıklandığı gibi güvenlik yükümlülüklerinizi karşılaalmanıza yardımcı olur. Microsoft 'un hizmet sağlayıcısı sorumluluklarını karşılama hakkında daha fazla bilgi için, [bulut bilgi işlem Için paylaşılan sorumlulukları](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/225366/1/Shared%20Responsibility%20for%20Cloud%20Computing-2019-10-25.pdf)okuyun.

Bu makaleye eklenen önerilerden bazıları Azure Güvenlik Merkezi tarafından otomatik olarak izlenebilir. Azure Güvenlik Merkezi, Azure 'daki kaynaklarınızı korumaya yönelik ilk savunma hattınızdır. Azure Güvenlik Merkezi hakkında bilgi için bkz. [Azure Güvenlik Merkezi nedir?](../../security-center/security-center-intro.md).

Azure Güvenlik Merkezi, olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli olarak analiz eder. Daha sonra bunları nasıl ele almak için öneriler sağlar. Azure Güvenlik Merkezi önerileri hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'Nde güvenlik önerileri](../../security-center/security-center-recommendations.md).

## <a name="data-protection"></a>Veri koruması

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
| Azure Resource Manager dağıtım modelini kullanma | Üst erişim denetimi (RBAC) ve denetim, Kaynak Yöneticisi tabanlı dağıtım ve idare, yönetilen kimliklere erişim ve erişim gibi önemli güvenlik geliştirmeleri için Azure Resource Manager dağıtım modelini kullanarak yeni depolama hesapları oluşturun. gizli dizileri ve Azure depolama verilerine ve kaynaklarına erişim için Azure AD tabanlı kimlik doğrulama ve yetkilendirme için Azure Key Vault. Mümkünse, Azure Resource Manager kullanmak için klasik dağıtım modelini kullanan mevcut depolama hesaplarını geçirin. Azure Resource Manager hakkında daha fazla bilgi için bkz. [Azure Resource Manager genel bakış](/azure/azure-resource-manager/resource-group-overview). | - |
| Tüm depolama hesaplarınızda **Güvenli aktarım gerekli** seçeneğini etkinleştirin | **Güvenli aktarım gerekli** seçeneğini etkinleştirdiğinizde, depolama hesabında yapılan tüm isteklerin güvenli bağlantılar üzerinden gerçekleşmesi gerekir. HTTP üzerinden yapılan tüm istekler başarısız olur. Daha fazla bilgi için bkz. [Azure Storage 'da güvenli aktarım gerektir](../common/storage-require-secure-transfer.md). | [Evet](../../security-center/security-center-sql-service-recommendations.md) |
| Tüm depolama hesaplarınız için Gelişmiş tehdit korumasını etkinleştirin | Azure depolama için Gelişmiş tehdit koruması, depolama hesaplarına yönelik olağan dışı ve potansiyel olarak zararlı girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Güvenlik uyarıları, etkinlik gerçekleştiğinde Azure Güvenlik Merkezi 'nde tetiklenir ve ayrıca, şüpheli etkinliklerin ayrıntıları ve tehditleri araştırıp düzeltilmesi ile ilgili öneriler ile abonelik yöneticilerine e-posta aracılığıyla gönderilir. Daha fazla bilgi için bkz. [Azure depolama Için Gelişmiş tehdit koruması](../common/storage-advanced-threat-protection.md). | [Evet](../../security-center/security-center-sql-service-recommendations.md) |
| Paylaşılan erişim imzası (SAS) belirteçlerini yalnızca HTTPS bağlantılarıyla sınırla | İstemci kuyruk verilerine erişmek için SAS belirteci kullandığında HTTPS gerektirme, gizlice dinleme riskini en aza indirmenize yardımcı olur. Daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](../common/storage-sas-overview.md). | - |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
| Kuyruk verilerine erişim yetkisi vermek için Azure Active Directory (Azure AD) kullanın | Azure AD, istekleri kuyruk depolamaya yetkilendirmek için paylaşılan anahtar üzerinde üstün güvenlik ve kullanım kolaylığı sağlar. Daha fazla bilgi için bkz. [Azure Active Directory kullanarak Azure bloblarına ve kuyruklara erişim yetkisi verme](../common/storage-auth-aad.md). | - |
| RBAC aracılığıyla bir Azure AD güvenlik sorumlusuna izin atarken en az ayrıcalık sorumlusunu göz önünde bulundurun | Bir Kullanıcı, Grup veya uygulamaya bir rol atarken, bu güvenlik sorumlusuna yalnızca onların görevlerini gerçekleştirmesi için gerekli olan izinleri verin. Kaynaklara erişimi kısıtlamak, verilerinizin yanlışlıkla ve kötü amaçlı olarak kötüye kullanımını önlemeye yardımcı olur. | - |
| Hesap erişim anahtarlarınızı Azure Key Vault ile güvenli hale getirin | Microsoft, Azure depolama 'ya istekleri yetkilendirmek için Azure AD kullanılmasını önerir. Ancak, paylaşılan anahtar yetkilendirmesi kullanmanız gerekiyorsa, hesap anahtarlarınızın Azure Key Vault sağlayın. Anahtar kasasındaki anahtarları uygulamanıza kaydetmek yerine çalışma zamanında alabilirsiniz. | - |
| Hesap anahtarlarınızı düzenli aralıklarla yeniden oluşturun | Hesap anahtarlarını döndürmek, verilerinizi kötü amaçlı aktörlere sunma riskini düzenli aralıklarla azaltır. | - |
| SAS 'ye izin atarken en az ayrıcalık sorumlusunu göz önünde bulundurun | SAS oluştururken, yalnızca istemcisinin işlevini gerçekleştirmesi için gerekli olan izinleri belirtin. Kaynaklara erişimi kısıtlamak, verilerinizin yanlışlıkla ve kötü amaçlı olarak kötüye kullanımını önlemeye yardımcı olur. | - |
| İstemcilere yaptığınız tüm SAS için bir iptal planı vardır | SAS tehlikeye girerse, bu SAS 'ı mümkün olan en kısa sürede iptal etmek isteyeceksiniz. Bir Kullanıcı temsilciliğini iptal etmek için, bu anahtarla ilişkili tüm imzaları hızlıca geçersiz kılmak üzere Kullanıcı temsili anahtarını iptal edin. Depolanan bir erişim ilkesiyle ilişkili bir hizmet SAS 'sini iptal etmek için, saklı erişim ilkesini silebilir, ilkeyi yeniden adlandırabilir veya süre sonu saatini geçmişteki bir zamana dönüştürebilirsiniz. Daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](../common/storage-sas-overview.md).  | - |
| Bir hizmet SAS, depolanan bir erişim ilkesiyle ilişkilendirilmediği zaman aşımı süresini bir saat veya daha az olarak ayarlayın | Depolanan bir erişim ilkesiyle ilişkilendirilmemiş bir hizmet SAS iptal edilemez. Bu nedenle, süre sonu süresini kısıtlamak için SAS bir saat için geçerli veya daha az önerilir. | - |

## <a name="networking"></a>Networking (Ağ İletişimi)

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
| Güvenlik duvarı kurallarını etkinleştir | Depolama hesabınıza erişimi, belirtilen IP adreslerinden veya aralıklarından kaynaklanan isteklerle veya bir Azure sanal ağı 'ndaki (VNet) bir alt ağ listesinden oluşan isteklerle sınırlamak için güvenlik duvarı kurallarını yapılandırın. Güvenlik duvarı kurallarını yapılandırma hakkında daha fazla bilgi için bkz. [Azure dosya eşitleme proxy ve güvenlik duvarı ayarları](../files/storage-sync-files-firewall-and-proxy.md). | - |
| Güvenilen Microsoft hizmetlerinin depolama hesabına erişmesine izin ver | İstekler bir Azure sanal ağı (VNet) içinde veya izin verilen ortak IP adreslerinden bir hizmetten kaynaklanmadığı takdirde, depolama hesabınız için Güvenlik Duvarı kurallarının etkinleştirilmesi, varsayılan olarak gelen istekleri engeller. Engellenen istekleri, Azure portalından, günlük ve ölçüm hizmetlerden, diğer Azure hizmetlerinden gelen içerir ve benzeri. Güvenilen Microsoft hizmetlerinin depolama hesabına erişmesine izin vermek için bir özel durum ekleyerek diğer Azure hizmetlerinden gelen isteklere izin verebilirsiniz. Güvenilen Microsoft Hizmetleri için bir özel durum ekleme hakkında daha fazla bilgi için bkz. [Azure dosya eşitleme proxy ve güvenlik duvarı ayarları](../files/storage-sync-files-firewall-and-proxy.md).| - |
| Özel uç noktaları kullan | Özel bir uç nokta, Azure sanal ağınızdan (VNet) depolama hesabına özel bir IP adresi atar. VNet ve depolama hesabı arasındaki tüm trafiğin özel bir bağlantı üzerinden güvenliğini sağlar. Özel uç noktalar hakkında daha fazla bilgi için bkz. [Azure özel uç nokta kullanarak bir depolama hesabına özel olarak bağlanma](../../private-link/create-private-endpoint-storage-portal.md). | - |
| Belirli ağlarla ağ erişimini sınırlandırma | Erişim gerektiren istemcileri barındıran ağlarla ağ erişimini kısıtlamak, kaynaklarınızın ağ saldırılarına maruz kalmasını azaltır. | [Evet](../../security-center/security-center-sql-service-recommendations.md) |

## <a name="loggingmonitoring"></a>Günlüğe kaydetme/Izleme

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
| İsteklerin nasıl yetkilendirildiğini izleme | Azure depolama 'ya karşı yapılan her isteği nasıl yetkilendirdiğini izlemek için Azure depolama günlüğü 'nü etkinleştirin. Günlükler, bir isteğin bir OAuth 2,0 belirteci kullanılarak, paylaşılan anahtar kullanılarak veya paylaşılan erişim imzası (SAS) kullanılarak yapılıp yapılmadığını belirtir. Daha fazla bilgi için bkz. [Azure Storage Analytics günlüğü](../common/storage-analytics-logging.md). | - |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik belgeleri](https://docs.microsoft.com//azure/security/)
- [Güvenli Geliştirme belgeleri](https://docs.microsoft.com/azure/security/develop/).
