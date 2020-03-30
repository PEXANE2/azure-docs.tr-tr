---
title: Blob depolama için güvenlik önerileri
titleSuffix: Azure Storage
description: Blob depolama için güvenlik önerileri hakkında bilgi edinin. Bu kılavuzun uygulanması, paylaşılan sorumluluk modelinde açıklandığı gibi güvenlik yükümlülüklerinizin yerine getirilmesine yardımcı olacaktır.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: tamram
ms.custom: security-recommendations
ms.openlocfilehash: 8dcefdfba3aba632e0592f0dea30c7e523ba81dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061474"
---
# <a name="security-recommendations-for-blob-storage"></a>Blob depolama için güvenlik önerileri

Bu makalede, Blob depolama için güvenlik önerileri içerir. Bu önerilerin uygulanması, paylaşılan sorumluluk modelimizde açıklandığı gibi güvenlik yükümlülüklerinizin yerine getirilmesine yardımcı olacaktır. Microsoft'un hizmet sağlayıcı sorumluluklarını yerine getirmek için ne yaptığı hakkında daha fazla bilgi için [bulut bilgi işlem için Paylaşılan sorumlulukları](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/225366/1/Shared%20Responsibility%20for%20Cloud%20Computing-2019-10-25.pdf)okuyun.

Bu makalede yer alan önerilerden bazıları Azure Güvenlik Merkezi tarafından otomatik olarak izlenebilir. Azure Güvenlik Merkezi, Azure'daki kaynaklarınızı korumada ilk savunma hattıdır. Azure Güvenlik Merkezi hakkında bilgi için [Azure Güvenlik Merkezi nedir'](../../security-center/security-center-intro.md)e bakın.

Azure Güvenlik Merkezi, olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli aralıklarla analiz eder. Daha sonra bunları nasıl ele almak için öneriler sağlar. Azure Güvenlik Merkezi önerileri hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'ndeki Güvenlik önerilerine](../../security-center/security-center-recommendations.md)bakın.

## <a name="data-protection"></a>Veri koruma

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
| Azure Kaynak Yöneticisi dağıtım modelini kullanma | Üstün erişim denetimi (RBAC) ve denetim, Kaynak Yöneticisi tabanlı dağıtım ve yönetişim, yönetilen kimliklere erişim, erişim dahil olmak üzere önemli güvenlik geliştirmeleri için Azure Kaynak Yöneticisi dağıtım modelini kullanarak yeni depolama hesapları oluşturun sırlar için Azure Key Vault'a ve Azure Depolama verilerine ve kaynaklarına erişmek için Azure AD tabanlı kimlik doğrulama ve yetkilendirmeye. Mümkünse, Azure Kaynak Yöneticisi'ni kullanmak için klasik dağıtım modelini kullanan varolan depolama hesaplarını geçirin. Azure Kaynak Yöneticisi hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi'ne genel bakış](/azure/azure-resource-manager/resource-group-overview)bilgisi ne bürünülsin. | - |
| Tüm depolama hesaplarınızda **Güvenli aktarım seçeneğini** etkinleştirin | **Güvenli aktarım seçeneğini** etkinleştirdiğinizde, depolama hesabına karşı yapılan tüm istekler güvenli bağlantılar üzerinden gerçekleşmelidir. HTTP üzerinden yapılan tüm istekler başarısız olur. Daha fazla bilgi için bkz. Azure [Depolama'da güvenli aktarım talep edin.](../common/storage-require-secure-transfer.md) | [Evet](../../security-center/security-center-sql-service-recommendations.md) |
| Tüm depolama hesaplarınız için gelişmiş tehdit koruması sağlama | Azure Depolama için gelişmiş tehdit koruması, depolama hesaplarına erişmek veya bunlardan yararlanmak için olağandışı ve zararlı olabilecek girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Etkinlikteki anormallikler oluştuğunda Azure Güvenlik Merkezi'nde güvenlik uyarıları tetiklenir ve abonelik yöneticilerine e-posta yoluyla gönderilir ve şüpheli etkinliklerin ayrıntıları ve tehditleri nasıl araştırıp düzelteceklerine ilişkin öneriler sunulur. Daha fazla bilgi için Azure [Depolama için Gelişmiş tehdit koruması 'na](../common/storage-advanced-threat-protection.md)bakın. | [Evet](../../security-center/security-center-sql-service-recommendations.md) |
| Blob verileri için yumuşak silmeyi açma | Yumuşak silme, blob verilerini silindikten sonra kurtarmanızı sağlar. Yumuşak silme hakkında daha fazla bilgi için [Azure Depolama lekeleri için Yumuşak silme'ye](storage-blob-soft-delete.md)bakın. | - |
| İş açısından kritik verileri değişmez bloblarda depolama | Blob verilerini WORM (Bir Kez Yaz, Çok Oku) durumunda depolamak için yasal tutmaları ve zaman tabanlı bekletme ilkelerini yapılandırın. Değişmez olarak depolanan lekeler okunabilir, ancak bekletme aralığı süresince değiştirilemez veya silinemez. Daha fazla bilgi için, [değişmez depolama alanıyla iş açısından kritik blob verilerini depolayın'a](storage-blob-immutable-storage.md)bakın. | - |
| Paylaşılan erişim imzası (SAS) belirteçlerini yalnızca HTTPS bağlantılarıyla sınırlandırın | Bir istemci blob verilerine erişmek için bir SAS belirteci kullandığında HTTPS'yi gerektirmek, gizlice dinleme riskini en aza indirmeye yardımcı olur. Daha fazla bilgi için bkz: [Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni.](../common/storage-sas-overview.md) | - |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
| Blob verilerine erişimi yetkilendirmek için Azure Etkin Dizini 'ni (Azure AD) kullanın | Azure AD, Blob depolama alanına istekleri yetkilendirmek için Paylaşılan Anahtar üzerinden üstün güvenlik ve kullanım kolaylığı sağlar. Daha fazla bilgi için bkz. Azure [Etkin Dizin'i kullanarak Azure blob'larına ve kuyruklarına erişimi yetkilendirme.](../common/storage-auth-aad.md) | - |
| RBAC aracılığıyla bir Azure REKLAM güvenlik ilkesine izin atarken en az ayrıcalığın ilkesini unutmayın | Bir kullanıcıya, gruba veya uygulamaya rol atarken, bu güvenlik ilkesine yalnızca görevlerini gerçekleştirmeleri için gerekli olan izinleri verir. Kaynaklara erişimi sınırlamak, verilerinizin hem kasıtsız hem de kötü amaçlı kötüye kullanılmasını önlemeye yardımcı olur. | - |
| İstemcilere blob verilerine sınırlı erişim sağlamak için bir kullanıcı delegasyonu SAS kullanın | Bir kullanıcı delegasyonu SAS, Azure Etkin Dizin (Azure AD) kimlik bilgileri ve SAS için belirtilen izinlerle güvence altına alınmıştır. Bir kullanıcı delegasyonu SAS kapsamı ve işlevi açısından bir hizmet SAS benzer, ancak hizmet SAS üzerinde güvenlik yararları sunuyor. Daha fazla bilgi için bkz: [Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni.](../common/storage-sas-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) | - |
| Azure Key Vault ile hesap erişim anahtarlarınızı güvenli hale | Microsoft, talepleri Azure Depolama'ya yetkilendirmek için Azure AD'yi kullanmanızı önerir. Ancak, Paylaşılan Anahtar yetkilendirmesini kullanmanız gerekiyorsa, hesap anahtarlarınızı Azure Key Vault ile güvence altına alın. Anahtarları uygulamanızla kaydetmek yerine, çalışma zamanında anahtar kasasından alabilirsiniz. Azure Anahtar Kasası hakkında daha fazla bilgi için [Azure Anahtar Kasası'na genel bakış](../../key-vault/key-vault-overview.md)bilgisi ne rendelededir. | - |
| Hesap anahtarlarınızı düzenli aralıklarla yeniden oluşturun | Hesap anahtarlarını düzenli aralıklarla döndürmek, verilerinizi kötü amaçlı aktörlere maruz bırakma riskini azaltır. | - |
| Bir SAS'a izin atarken en az ayrıcalık ilkesini aklınızda bulundurun | Bir SAS oluştururken, yalnızca istemcinin işlevini gerçekleştirmesi için gereken izinleri belirtin. Kaynaklara erişimi sınırlamak, verilerinizin hem kasıtsız hem de kötü amaçlı kötüye kullanılmasını önlemeye yardımcı olur. | - |
| Müşterilere vermiş olduğunuz herhangi bir SAS için bir iptal planı var | Bir SAS tehlikeye girerse, mümkün olan en kısa sürede bu SAS iptal etmek isteyeceksiniz. Bir kullanıcı delegasyonu SAS'ı iptal etmek için, bu anahtarla ilişkili tüm imzaları hızla geçersiz ksaymak için kullanıcı delegasyonu anahtarını iptal edin. Depolanan bir erişim ilkesiyle ilişkili bir hizmet SAS'ı iptal etmek için, depolanan erişim ilkesini silebilir, ilkeyi yeniden adlandırabilir veya son kullanma süresini geçmişte olan bir saatle değiştirebilirsiniz. Daha fazla bilgi için bkz: [Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni.](../common/storage-sas-overview.md)  | - |
| Bir hizmet SAS depolanan bir erişim ilkesiyle ilişkili değilse, son kullanma süresini bir saat veya daha az olarak ayarlayın | Depolanan bir erişim ilkesiyle ilişkili olmayan bir hizmet SAS'sı iptal edilemez. Bu nedenle, SAS'ın bir saat veya daha kısa bir süre için geçerli olması için son kullanma süresinin sınırlandırılması önerilir. | - |
| Kapsayıcılara ve bloblara anonim genel okuma erişimini sınırlandırın | Anonim, bir kapsayıcıya genel okuma erişimi ve blobs herhangi bir istemciye bu kaynaklara salt okunur erişim verir. Senaryonuz gerektirmedikçe herkese açık okuma erişimini etkinleştirmekten kaçının. | - |

## <a name="networking"></a>Ağ Oluşturma

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
| Güvenlik duvarı kurallarını etkinleştirme | Güvenlik duvarı kurallarını, depolama hesabınıza erişimi, belirtilen IP adreslerinden veya aralıklarından veya Azure Sanal Ağı'ndaki (VNet) alt ağlar listesinden kaynaklanan isteklerle sınırlamak için yapılandırın. Güvenlik duvarı kurallarını yapılandırma hakkında daha fazla bilgi için [Azure Dosya Eşitleme proxy ve güvenlik duvarı ayarlarına](../files/storage-sync-files-firewall-and-proxy.md)bakın. | - |
| Güvenilen Microsoft hizmetlerinin depolama hesabına erişmesine izin verme | Talepler Azure Sanal Ağı 'nda (VNet) veya izin verilen genel IP adreslerinden gelen bir hizmetten kaynaklanmadığı sürece, depolama hesabınız için güvenlik duvarı kurallarını açmak varsayılan olarak gelen veri isteklerini engeller. Engellenen istekler arasında diğer Azure hizmetlerinden, Azure portalından, günlük ve metrik hizmetleri gibi hizmetlerden gelenler yer almaktadır. Güvenilen Microsoft hizmetlerinin depolama hesabına erişmesine izin vermek için bir özel durum ekleyerek diğer Azure hizmetlerinden gelen isteklere izin verebilirsiniz. Güvenilen Microsoft hizmetleri için özel durum ekleme hakkında daha fazla bilgi için [Azure Dosya Eşitleme proxy ve güvenlik duvarı ayarlarına](../files/storage-sync-files-firewall-and-proxy.md)bakın.| - |
| Özel uç noktaları kullanma | Özel bir bitiş noktası, Azure Sanal Ağınızdan (VNet) depolama hesabına özel bir IP adresi atar. Özel bir bağlantı üzerinden VNet'iniz ve depolama hesabınız arasındaki tüm trafiği güvence altına alar. Özel uç noktaları hakkında daha fazla bilgi için Azure [Özel Bitiş Noktası'nı kullanarak bir depolama hesabına özel bağlan'a](../../private-link/create-private-endpoint-storage-portal.md)bakın. | - |
| Ağ erişimini belirli ağlarla sınırlandırın | Ağ erişimini, erişim gerektiren istemcileri barındıran ağlarla sınırlamak, kaynaklarınızın ağ saldırılarına maruz kalmasını azaltır. | [Evet](../../security-center/security-center-sql-service-recommendations.md) |

## <a name="loggingmonitoring"></a>Günlük /İzleme

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
| İsteklerin nasıl yetkilendirilmesini izleme | Azure Depolama'ya karşı yapılan her isteğin nasıl yetkilendirilebildiğini izlemek için Azure Depolama günlüğe kaydetmeyi etkinleştirin. Günlükler, bir isteğin anonim olarak mı, OAuth 2.0 belirteci kullanılarak mı, Paylaşılan Anahtar kullanılarak mı yoksa paylaşılan erişim imzası (SAS) kullanılarak mı yapıldığını gösterir. Daha fazla bilgi için Azure [Depolama analitiği günlüğe](../common/storage-analytics-logging.md)kaydedin. | - |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure güvenlik belgeleri](https://docs.microsoft.com//azure/security/)
- [Güvenli geliştirme belgeleri](https://docs.microsoft.com/azure/security/develop/).
