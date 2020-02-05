---
title: Azure VMware çözümleri (AVS)-AVS Hizmetleri için güvenlik
description: AVS hizmetlerinin güvenliği için paylaşılan sorumluluk modellerini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d2d45f827d4165175a4a5429f0b9393a55e2ff1e
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025002"
---
# <a name="avs-security-overview"></a>AVS güvenliğine genel bakış

Bu makalede, Azure VMware çözümünde AVS hizmeti, altyapısı ve veri merkezi tarafından güvenliğin nasıl uygulandığı hakkında genel bakış sunulmaktadır. Veri koruma ve güvenlik, ağ güvenliği ve güvenlik açıklarına ve düzeltme eklerinin nasıl yönetildiğini öğrenirsiniz.

## <a name="shared-responsibility"></a>Paylaşılan sorumluluk

AVS tarafından Azure VMware çözümü, güvenlik için paylaşılan bir sorumluluk modeli kullanır. Bulutta güvenilen güvenlik, müşterilerin ve Microsoft 'un bir hizmet sağlayıcısı olarak Microsoft tarafından sağlanan sorumlulukları aracılığıyla elde edilir. Bu sorumluluk matrisi daha yüksek güvenlik sağlar ve tek hata noktalarını ortadan kaldırır.

## <a name="azure-infrastructure"></a>Azure altyapısı

Azure altyapı güvenliği konuları, veri merkezleri ve ekipman konumunu içerir.

### <a name="datacenter-security"></a>Veri merkezi güvenliği

Microsoft, Azure 'u destekleyen fiziksel tesislerin tasarlanmasıyla, oluşturulmasına ve oluşturulmasına yönelik bir bölümün tamamına sahiptir. Bu ekip, son derece fiziksel güvenliği korumak için yatırım yapmış. Fiziksel güvenlik hakkında daha fazla bilgi için bkz. [Azure tesisler, şirket içi ve fiziksel güvenlik](../security/azure-physical-security.md).

### <a name="equipment-location"></a>Ekipman konumu

AVS özel bulutlarınızı çalıştıran çıplak donanım donanımı, Azure veri merkezi konumlarında barındırılır. Ekipmanların olduğu yerlerde, erişim kazanmak için biyometrik tabanlı iki öğeli kimlik doğrulaması gerekir.

## <a name="dedicated-hardware"></a>Adanmış donanım

AVS hizmeti 'nin bir parçası olarak, tüm AVS müşterileri, diğer kiracı donanımlarından fiziksel olarak yalıtılmış yerel ve ayrılmış diskler içeren adanmış çıplak konaklar alırlar. Her düğümde vSAN ile birlikte bir ESXi Hiper Yöneticisi çalışır. Düğümler, müşterinin adanmış VMware vCenter ve NSX aracılığıyla yönetilir. Kiracılar arasında donanım paylaşılmayan ek bir yalıtım katmanı ve güvenlik koruması sağlar.

## <a name="data-security"></a>Veri güvenliği

Müşteriler verilerinin denetimini ve sahipliğini tutar. Müşteri verilerinin veri Stewardship, müşterinin sorumluluğundadır.

### <a name="data-protection-for-data-at-rest-and-data-in-motion-within-internal-networks"></a>Bekleyen veriler ve iç ağlarda hareket halindeki veriler için veri koruması

AVS özel bulut ortamındaki bekleyen veriler için vSAN şifrelemesini kullanabilirsiniz. vSAN şifrelemesi, kendi sanal ağınızda veya şirket içinde VMware sertifikalı dış anahtar yönetim sunucuları (KMS) ile birlikte kullanılabilir. Veri şifreleme anahtarlarını kendiniz denetlersiniz. VSphere, AVS özel bulutu 'nda hareket halindeki veriler için tüm vmkernel trafiği (vMotion trafiği dahil) için kablo üzerinden şifrelemeyi destekler.

### <a name="data-protection-for-data-that-is-required-to-move-through-public-networks"></a>Ortak ağlarda gezinmek için gereken veriler için veri koruması

Ortak ağlar üzerinden taşınan verileri korumak için, AVS özel bulutlarınız için IPSec ve SSL VPN tünelleri oluşturabilirsiniz. 128-Byte ve 256-Byte AES gibi yaygın şifreleme yöntemleri desteklenir. Aktarım içindeki veriler (kimlik doğrulama, yönetim erişimi ve müşteri verileri dahil) standart şifreleme mekanizmaları (SSH, TLS 1,2 ve Secure RDP) ile şifrelenir. Hassas bilgileri taşımaya yönelik iletişim standart şifreleme mekanizmalarını kullanır.

### <a name="secure-disposal"></a>Güvenli elden çıkarma

AVS hizmetinizin süresi dolarsa veya sonlandırılırsa, verilerinizi kaldırmanız veya silmeniz sizin sorumluluğunuzdadır. AVS, tüm müşteri verilerini Müşteri Anlaşmasında belirtilen şekilde silmek veya geri döndürmek için, geçerli yasaların kişisel verilerin bir kısmını veya tamamını korumaları gerekir. Tüm kişisel verileri bekletmek için gerekliyse, AVS verileri arşivleyecektir ve müşteri verilerinin daha fazla işlemeden oluşmasını engellemek için makul ölçüler uygular.

### <a name="data-location"></a>Veri Konumu

AVS özel bulutlarınızı ayarlarken, dağıtılacağı Azure bölgesini seçersiniz. Veri geçişini veya site dışı veri yedeklemesini gerçekleştirmediğiniz takdirde VMware sanal makine verileri bu fiziksel veri merkezinden taşınamaz. Ayrıca, gereksinimlerinize uygun olan iş yüklerini ve verileri birden çok Azure bölgesinde depolamayı da barındırabilirsiniz.

AVS özel bulutu 'nda yerleşik olan müşteri verileri, hiper yakınsama düğümlerinde, kiracı yöneticisinin açık eylemi olmadan konum çapraz geçiş yapmaz. İş yüklerinizi yüksek oranda kullanılabilir bir şekilde uygulamak sizin sorumluluğunuzdadır.

### <a name="data-backups"></a>Veri yedeklemeleri

AVS müşteri verilerini yedeklemeli veya arşivetmez. AVS, yönetim sunucularının yüksek kullanılabilirliğini sağlamak için vCenter ve NSX verilerinin düzenli olarak yedeklenmesini gerçekleştirir. Yedeklemeden önce tüm veriler, VMware API 'Leri kullanılarak vCenter kaynağında şifrelenir. Şifrelenmiş veriler Azure Blob 'a taşınır ve depolanır. Yedeklemeler için şifreleme anahtarları, Azure 'daki AVS sanal ağında çalışan yüksek güvenlikli bir AVS ile yönetilen kasada depolanır.

## <a name="network-security"></a>Ağ Güvenliği

AVS çözümü, ağ güvenliği katmanlarını kullanır.

### <a name="azure-edge-security"></a>Azure Edge güvenliği

AVS Hizmetleri, Azure tarafından sunulan temel ağ güvenliğinin üzerine kurulmuştur. Azure, anormal giriş veya çıkış trafiği desenleri ve dağıtılmış hizmet reddi (DDoS) saldırıları ile ilişkili ağ tabanlı saldırılara algılama ve zamanında yanıt verme konusunda derinlemesine savunma teknikleri uygular. Bu güvenlik denetimi, AVS özel bulut ortamları ve AVS tarafından geliştirilen denetim düzlemi yazılımı için geçerlidir.

### <a name="segmentation"></a>Ayrılmasını

AVS hizmeti, AVS özel bulut ortamınızdaki kendi özel ağlarınızla erişimi kısıtlayan mantıksal olarak ayrı katman 2 ağları içerir. Bir güvenlik duvarı kullanarak AVS özel bulut ağlarınızı daha da koruyabilirsiniz. AVS portalı, şirket içi AVS özel bulut trafiği, AVS özel bulut trafiği, Internet 'e genel trafik ve IPSec VPN üzerinden şirket içi ağ trafiği dahil olmak üzere tüm ağ trafiği için tam ve NS ağ trafiği denetimleri tanımlamanızı sağlar veya ExpressRoute bağlantısı.

## <a name="vulnerability-and-patch-management"></a>Güvenlik Açığı ve düzeltme eki yönetimi

AVS, yönetilen VMware yazılımlarının (ESXi, vCenter ve NSX) düzenli güvenlik düzeltme ekiyle sorumludur.

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

Müşteriler, tercih edilen olarak Multi-Factor Authentication veya SSO kullanarak Azure hesabı (Azure AD 'de) için kimlik doğrulaması yapabilir. Azure portal, AVS portalını kimlik bilgilerini yeniden girmeye gerek kalmadan başlatabilirsiniz.

AVS, AVS özel bulut vCenter için bir kimlik kaynağının isteğe bağlı yapılandırmasını destekler. Bir şirket [içi kimlik kaynağını](set-vcenter-identity.md), AVS özel bulutu için yeni bir kimlik kaynağını veya [Azure AD](azure-ad.md)'yi kullanabilirsiniz.

Varsayılan olarak, müşterilere, AVS özel bulutu 'nda vCenter 'ın günlük işlemleri için gerekli olan ayrıcalıklar verilir. Bu izin düzeyi vCenter 'a yönetim erişimi içermez. Yönetim erişimi geçici olarak gerekliyse, yönetim görevlerini tamamladıktan sonra ayrıcalıklarınızı sınırlı bir süre için [ilerletebilirsiniz](escalate-private-cloud-privileges.md) .
