---
title: Azure kanıtlama genel bakış
description: Güvenilir yürütme ortamları (TEEs) için bir çözüm olan Microsoft Azure kanıtlama konusuna genel bakış
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.custom: references_regions
ms.openlocfilehash: a84308ba06a38cea475fcb1bae022da16424a731
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90033005"
---
# <a name="microsoft-azure-attestation-preview"></a>Microsoft Azure Doğrulama (önizleme)

Microsoft Azure kanıtlama (Önizleme), [ıntel® Software Guard uzantıları](https://www.intel.com/content/www/us/en/architecture-and-technology/software-guard-extensions.html) (SGX) şifreleme ve [sanallaştırma tabanlı güvenlik](/windows-hardware/design/device-experiences/oem-vbs) (VBS) şifrelemesi gibi güvenilir yürütme ortamlarının (TEEs) atlaması için bir çözümdür. Şifreleme kanıtlama, bir kuşın güvenli ve güvenilir olduğunu doğrulamaya yönelik bir işlemdir.

Kanıtlama, yazılım ikililerinin güvenilir bir platformda düzgün bir şekilde örneğinin oluşturulmasını gösteren bir işlemdir. Uzak bağlı olan taraflar daha sonra yalnızca, güvenilen donanımlarda yalnızca ilgili yazılımın çalıştığını güvenle elde edebilir. Azure kanıtlama, kanıtlama için birleştirilmiş bir müşteriye yönelik hizmet ve çerçevedir.

Azure kanıtlama, [Azure gizli bilgi işlem](../confidential-computing/overview.md) ve akıllı sınır koruması gibi son teknoloji güvenlik paradigmalarına sunar. Müşteriler makinenin konumunu, söz konusu makinedeki bir sanal makinenin (VM) ve bu VM 'nin üzerinde çalıştığı ortamın bulunduğu ortamı bağımsız olarak doğrulama özelliği istiyor. Azure kanıtlama, bu ve pek çok ek müşteri isteğini güçlendirilecektir.

Azure kanıtlama, işlem varlıklarından gelen kanıtları alır, bunları bir dizi talebe dönüştürür, yapılandırılabilir ilkelere göre doğrular ve talep tabanlı uygulamalar için şifreleme provaları üretir (örneğin, bağlı olan taraflar ve denetim yetkilileri).

## <a name="use-cases"></a>Kullanım örnekleri

Azure kanıtlama, birden çok ortam ve farklı kullanım durumları için kapsamlı kanıtlama hizmetleri sağlar.

### <a name="sgx-attestation"></a>SGX kanıtlama

SGX, belirli Intel CPU modellerinde desteklenen donanım sınıfı yalıtımına başvurur. SGX, kodun, SGX enclaven olarak bilinen ayıklanmış bölmeleri çalışmasına olanak sağlar. Daha sonra, erişim ve bellek izinleri, doğru yalıtımıyla minimum saldırı yüzeyi sağlamak için donanımla yönetilir.

İstemci uygulamaları, bu kuşkuların içinde gerçekleşmesi için güvenlik duyarlı görevlerin temsilci olarak oluşturulması ile SGX 'in avantajlarından faydalanmak için tasarlanabilir. Bu tür uygulamalar daha sonra, kuşve duyarlı verilere erişme özelliği için düzenli olarak güven sağlamak üzere Azure kanıtlama özelliğini kullanabilir.

### <a name="vbs-attestation"></a>VBS kanıtlama

VBS, Hyper-V tabanlı bir şifreleme bellek koruması için yazılım tabanlı bir mimaridir. Konak yönetici kodunun yanı sıra yerel ve bulut hizmeti yöneticilerinin bir VBS enckor içindeki verilere erişmesini veya yürütmesini etkilemelerini önler.

Azure kanıtlama, SGX teknolojisine benzer şekilde, yapılandırılmış ilkelere karşı ve bir sertifika bildiriminin geçerlilik kanıtı olarak verilmesine olanak sağlar.

### <a name="open-enclave"></a>Açık şifreleme
[Açık Enclave](https://openenclave.io/sdk/) (OE), geliştiricilerin t tabanlı uygulamalar oluşturmalarına yönelik tek bir Birleşik kuşatma soyutlaması oluşturmaya hedeflenmiş bir kitaplıklar koleksiyonudur. Platform karmaşıklığını en aza indiren bir evrensel güvenli uygulama modeli sunar. Microsoft, democratizing donanım tabanlı kuşatma teknolojilerine ve Azure 'da upal 'ın artması gibi önemli bir atlama pulu olarak görüntüler.

OE, bir kuşatma bulgusu doğrulaması için özel gereksinimleri standart hale getirir. Bu, Azure kanıtlama için yüksek düzeyde bir kanıtlama tüketicisi olan OE 'yi nitelendirir.

## <a name="azure-attestation-can-run-in-a-tee"></a>Azure kanıtlama, bir t 'de çalışabilir

Azure kanıtlama, aşağıdaki eylemleri gerçekleştirdiğinden gizli bilgi Işlem senaryolarında kritik öneme sahiptir:

- Şifreleme bulgusunda geçerli olup olmadığını doğrular.
- Şifreleme kanıtlarını müşteri tanımlı bir ilkeye göre değerlendirir.
- Kiracıya özgü ilkeleri yönetir ve depolar.
- , Şifreleme ile etkileşim kurmak için bağlı olan taraflar tarafından kullanılan bir belirteç oluşturur ve imzalar.

Azure kanıtlama, iki tür ortamda çalışacak şekilde oluşturulmuştur:
- Bir SGX etkin t 'de çalışan Azure kanıtlama.
- Azure kanıtlama, t olmayan bir sürümünde çalışıyor.

Azure kanıtlama müşterileri, Microsoft 'un güvenilir bilgi işlem tabanı (TCB) dışında kullanılması için bir gereksinim ifade edidi. Bu, VM yöneticileri, ana bilgisayar yöneticileri ve Microsoft geliştiricileri gibi Microsoft varlıklarının kanıtlama isteklerini, ilkelerini ve Azure kanıtlama tarafından verilen belirteçleri değiştirmesini önlemektir. Azure kanıtlama Ayrıca, teklif doğrulaması, belirteç oluşturma ve belirteç imzalama gibi Azure kanıtlama özelliklerinin bir SGX şifreliğine taşındığı t 'de çalışacak şekilde oluşturulmuştur.

## <a name="why-use-azure-attestation"></a>Azure kanıtlama neden kullanılmalıdır?

Azure kanıtlama, TEEs, aşağıdaki avantajları sağladığından, kanıtlamaktan için tercih edilen seçenektir: 

- SGX ve vbs enclaven gibi birden çok TEEs kanıtlamaktan için Birleşik çerçeve
- Belirteç oluşturmayı kısıtlamak için özel kanıtlama sağlayıcılarının ve ilkelerinin yapılandırılmasına izin veren çok kiracılı hizmet
- Kullanıcılardan yapılandırma olmadan test olabilen varsayılan sağlayıcıları sunar
- , Bir SGX encde uygulamasıyla birlikte kullanımda olan verilerini korur
- Hizmet Düzeyi Sözleşmesi (SLA) sunan yüksek oranda kullanılabilir hizmet

## <a name="business-continuity-and-disaster-recovery-bcdr-support"></a>İş sürekliliği ve olağanüstü durum kurtarma (BCDR) desteği

Azure kanıtlama için [Iş sürekliliği ve olağanüstü durum kurtarma](/azure/best-practices-availability-paired-regions) (BCDR), bir bölgedeki önemli kullanılabilirlik sorunlarından veya olağanüstü durum olaylarından kaynaklanan hizmet kesintilerini azaltmanıza olanak sağlar.

Aşağıda şu anda BCDR tarafından desteklenen bölgeler verilmiştir
- Doğu ABD 2 => Orta ABD eşleştirilmiş.
- Orta ABD => Doğu ABD 2 eşleştirilmiş.

İki bölgede dağıtılan kümeler, normal koşullarda bağımsız olarak çalışır. Bir bölgenin hatası veya kesilmesi durumunda aşağıdakiler gerçekleşir:

- Azure kanıtlama BCDR, müşterilerin kurtarmak için ek bir adım yapması gerekmeyen sorunsuz yük devretme sağlayacak
- Bölge için [Azure Traffic Manager](../traffic-manager/index.yml) , sistem durumu araştırmasının düştüğü olduğunu algılayacak ve uç noktayı eşleştirilmiş bölgeye geçecek
- Mevcut bağlantılar çalışmayacak ve iç sunucu hatası ya da zaman aşımı sorunları alacak
- Tüm denetim düzlemi işlemleri engellenir. Müşteriler, birincil bölgede kanıtlama sağlayıcıları ve güncelleştirme ilkeleri oluşturamayacak
- Onaylamasını sağlar çağrıları dahil tüm veri düzlemi işlemleri birincil bölgede çalışmaya devam edecektir

## <a name="next-steps"></a>Sonraki adımlar
- [Azure kanıtlama temel kavramları](basic-concepts.md) hakkında bilgi edinin
- [Kanıtlama ilkesi yazma ve imzalama](author-sign-policy.md)
- [PowerShell kullanarak Azure kanıtlama ayarlama](quickstart-powershell.md)

