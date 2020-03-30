---
title: CloudSimple tarafından Azure VMware Çözümü - CloudSimple Hizmetleri için Güvenlik
description: CloudSimple hizmetlerinin güvenliği için paylaşılan sorumluluk modellerini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1a33e20ec540a05885eb13a3828d28ffc9923fff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025002"
---
# <a name="cloudsimple-security-overview"></a>CloudSimple güvenlik genel bakış

Bu makalede, CloudSimple hizmeti, altyapısı ve veri merkezi tarafından Azure VMware Çözümünde güvenliğin nasıl uygulandığına genel bir bakış sunulmaktadır. Veri koruma ve güvenlik, ağ güvenliği ve güvenlik açıklarının ve düzeltme emüllerinin nasıl yönetildiği hakkında bilgi edinebilirsiniz.

## <a name="shared-responsibility"></a>Paylaşılan sorumluluk

CloudSimple tarafından Azure VMware Solution güvenlik için paylaşılan bir sorumluluk modeli kullanır. Bulutta güvenilen güvenlik, müşterilerin paylaşılan sorumlulukları ve hizmet sağlayıcısı olarak Microsoft aracılığıyla sağlanır. Bu sorumluluk matrisi daha yüksek güvenlik sağlar ve tek hata noktalarını ortadan kaldırır.

## <a name="azure-infrastructure"></a>Azure altyapısı

Azure altyapısı güvenliği yle ilgili hususlar arasında veri merkezleri ve ekipman konumu yer alır.

### <a name="datacenter-security"></a>Veri merkezi güvenliği

Microsoft, Azure'u destekleyen fiziksel tesisleri tasarlamaya, oluşturmaya ve çalıştırmaya adanmış bir bölüme sahiptir. Bu ekip son teknoloji fiziksel güvenliği korumaya yatırım yapar. Fiziksel güvenlik le ilgili ayrıntılar için [Azure tesislerine, tesislerinde ve fiziksel güvenliğe](../security/azure-physical-security.md)bakın.

### <a name="equipment-location"></a>Ekipman konumu

Özel Bulutlarınızı çalıştıran çıplak metal donanım donanımı Azure veri merkezi konumlarında barındırılır.  Bu ekipmanın bulunduğu kafesler, erişim için biyometrik tabanlı iki faktörlü kimlik doğrulaması gerektirir.

## <a name="dedicated-hardware"></a>Özel donanım

CloudSimple hizmetinin bir parçası olarak, tüm CloudSimple müşterileri, diğer kiracı donanımından fiziksel olarak izole edilmiş yerel ekli disklere sahip özel çıplak metal ana bilgisayarlara sahip olur. VSAN'lı bir ESXi hipervizörü her düğümde çalışır. Düğümler müşteriye özel VMware vCenter ve NSX aracılığıyla yönetilir. Donanımı kiracılar arasında paylaşmamak ek bir yalıtım katmanı ve güvenlik koruması sağlar.

## <a name="data-security"></a>Veri güvenliği

Müşteriler verilerinin denetimini ve sahipliğini korur. Müşteri verilerinin veri yönetimi müşterinin sorumluluğundadır.

### <a name="data-protection-for-data-at-rest-and-data-in-motion-within-internal-networks"></a>Kurumdaki veriler için veri koruması ve iç ağlarda hareket halindeki veriler

Özel Bulut ortamında veri dinlenmek için vSAN şifrelemek kullanabilirsiniz. vSAN şifreleme, kendi sanal ağınızda veya şirket içinde VMware sertifikalı harici anahtar yönetim sunucuları (KMS) ile çalışır.  Veri şifreleme anahtarlarını kendiniz kontrol esiniz. Özel Bulut içinde hareket halindeki veriler için vSphere, tüm vmkernel trafiği (vMotion trafiği dahil) için kablo üzerinden veri şifrelemesini destekler.

### <a name="data-protection-for-data-that-is-required-to-move-through-public-networks"></a>Ortak ağlarda hareket etmek için gereken veriler için Veri Koruması

Ortak ağlarda hareket eden verileri korumak için, Özel Bulutlarınız için IPsec ve SSL VPN tünelleri oluşturabilirsiniz. 128 bayt ve 256 bayt AES dahil olmak üzere yaygın şifreleme yöntemleri desteklenir. Aktarım daki veriler (kimlik doğrulama, yönetim erişimi ve müşteri verileri dahil) standart şifreleme mekanizmaları (SSH, TLS 1.2 ve Secure RDP) ile şifrelenir. Hassas bilgileri taşıyan iletişim, standart şifreleme mekanizmalarını kullanır.

### <a name="secure-disposal"></a>Güvenli Bertaraf

CloudSimple hizmetinizin süresi doluyorsa veya sonlandırılırsa, verilerinizi kaldırmak veya silmiş siniz. CloudSimple, yürürlükteki yasaların kişisel verilerin bir kısmını veya tamamını elinde tutması gerektiği durumlar dışında, müşteri sözleşmesinde sağlanan tüm müşteri verilerini silmek veya iade etmek için sizinle işbirliği yapacaktır. Herhangi bir kişisel veri saklamak için gerekirse, CloudSimple verileri arşivleyecek ve müşteri verilerinin başka bir işleme girmesini önlemek için makul önlemleri uygulayacaktır.

### <a name="data-location"></a>Veri Konumu

Özel Bulutlarınızı ayarlarken, dağıtılacakları Azure bölgesini seçersiniz. VMware sanal makine verileri, veri geçişi veya iş yeri dışında veri yedeklemegerçekleştirmediğiniz sürece bu fiziksel veri merkezinden taşınmaz. İhtiyaçlarınız için uygunsa iş yüklerini barındırabilir ve verileri birden çok Azure bölgesinde depolayabilirsiniz.

Private Cloud'da bulunan müşteri verileri, kiracı yöneticinin açık eylemi olmadan konumlarda geçiş yapmaz. İş yüklerinizi son derece uygun bir şekilde uygulamak sizin sorumluluğunuzdadır.

### <a name="data-backups"></a>Veri yedekleri

CloudSimple müşteri verilerini yedeklemez veya arşivlemez. CloudSimple, yönetim sunucularının yüksek kullanılabilirliğini sağlamak için vCenter ve NSX verilerinin periyodik yedeklemesini gerçekleştirir. Yedeklemeden önce, tüm veriler VMware API'leri kullanılarak vCenter kaynağında şifrelenir. Şifrelenmiş veriler Taşınır ve Azure blob'da depolanır. Yedeklemelerin şifreleme anahtarları, Azure'daki CloudSimple sanal ağında çalışan son derece güvenli cloudsimple yönetilen kasasında depolanır.

## <a name="network-security"></a>Ağ Güvenliği

CloudSimple çözümü ağ güvenliği katmanlarına dayanır.

### <a name="azure-edge-security"></a>Azure kenar güvenliği

CloudSimple hizmetleri, Azure tarafından sağlanan temel ağ güvenliğinin üzerine inşa edilmiştir. Azure, anormal giriş veya çıkış trafik desenleri ve dağıtılmış hizmet reddi (DDoS) saldırılarıyla ilişkili ağ tabanlı saldırılara algılama ve zamanında yanıt verme için derinlemesine savunma teknikleri uygular. Bu güvenlik denetimi, Özel Bulut ortamları ve CloudSimple tarafından geliştirilen kontrol uçağı yazılımı için geçerlidir.

### <a name="segmentation"></a>Kesimleme

CloudSimple hizmeti, Özel Bulut ortamınızda kendi özel ağlarınıza erişimi kısıtlayan Katman 2 ağlarını mantıksal olarak birbirinden ayırır. Güvenlik duvarı kullanarak Özel Bulut ağlarınızı daha fazla koruyabilirsiniz. CloudSimple portalı, Özel Bulut içi trafik, Özel Bulut trafiği, Internet'e genel trafik ve IPsec VPN üzerinden şirket içi ağ trafiği veya dahil olmak üzere tüm ağ trafiği için EW ve NS ağ trafiği denetim kurallarını tanımlamanıza olanak tanır ExpressRoute bağlantısı.

## <a name="vulnerability-and-patch-management"></a>Güvenlik açığı ve yama yönetimi

CloudSimple, yönetilen VMware yazılımının (ESXi, vCenter ve NSX) periyodik güvenlik yaması ile sorumludur.

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

Müşteriler, tercih edilen çok faktörlü kimlik doğrulamasını veya SSO'u kullanarak Azure hesaplarında (Azure AD'de) kimlik doğrulaması yapabilir. Azure portalından, kimlik bilgilerini yeniden girmeden CloudSimple portalını başlatabilirsiniz.

CloudSimple, Private Cloud vCenter için bir kimlik kaynağının isteğe bağlı yapılandırmasını destekler. Özel Bulut için yeni bir kimlik kaynağı veya [Azure AD](azure-ad.md)için şirket içi [kimlik kaynağı](set-vcenter-identity.md)kullanabilirsiniz.

Varsayılan olarak, müşterilere özel bulut içinde vCenter'ın günlük işlemleri için gerekli olan ayrıcalıklar verilir. Bu izin düzeyi vCenter'a yönetim erişimi içermez. Yönetim erişimi geçici olarak gerekliyse, yönetim görevlerini tamamlarken [ayrıcalıklarınızı](escalate-private-cloud-privileges.md) sınırlı bir süre için artırabilirsiniz.
