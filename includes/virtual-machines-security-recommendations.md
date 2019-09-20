---
title: include dosyası
description: include dosyası
services: virtual-machines
author: barclayn
ms.service: virtual-machines
ms.topic: include
ms.date: 09/19/2019
ms.author: barclayn
ms.custom: include file
ms.openlocfilehash: 60d4957f65c124de683c6d156561de3b52aaec94
ms.sourcegitcommit: a7a9d7f366adab2cfca13c8d9cbcf5b40d57e63a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71163949"
---
Bu makale, Azure sanal makineler için güvenlik önerileri içerir. Bu önerilerin uygulanması, paylaşılan sorumluluk modelinizde açıklandığı şekilde güvenlik yükümlülüklerinizi karşılamanız ve Web uygulaması çözümleriniz için genel güvenliği iyileştirmenize yardımcı olacaktır. Microsoft 'un hizmet sağlayıcısı sorumluluklarını karşılama hakkında daha fazla bilgi için, [bulut bilgi işlem Için paylaşılan sorumlulukları](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf)okuyun.

Bu makaleye eklenen önerilerden bazıları Azure Güvenlik Merkezi tarafından otomatik olarak izlenebilir. Azure Güvenlik Merkezi, Azure 'daki kaynaklarınızı korumaya yönelik ilk savunma hattınızdır. Olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli olarak analiz eder. Daha sonra bunları nasıl ele almak için öneriler sağlar.

- Azure Güvenlik Merkezi önerileri hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'Nde güvenlik önerileri](../articles/security-center/security-center-recommendations.md).
- Azure Güvenlik Merkezi hakkında bilgi için bkz. [Azure Güvenlik Merkezi nedir?](../articles/security-center/security-center-intro.md)

## <a name="recommendations"></a>Öneriler

| Category | Öneri | Açıklamalar | Güvenlik Merkezi |
|-|-|----|--|
| Genel | Özel VM görüntülerini oluştururken en son güncelleştirmeleri kullanın | Görüntü oluşturmadan önce, tüm en son güncelleştirmelerin işletim sistemi ve görüntünüzün bir parçası olacak tüm uygulamalar için yüklendiğinden emin olun.  | - |
| Genel | VM 'lerinizi güncel tutun | Azure 'daki Windows ve Linux bilgisayarlarınıza yönelik işletim sistemi güncelleştirmelerini yönetmek için Azure Otomasyonu 'nda [güncelleştirme yönetimi](../articles/automation/automation-update-management.md) çözümünü kullanabilirsiniz. | [Evet](../articles/security-center/security-center-apply-system-updates.md) |
| Genel | Sanal makinelerinizi yedekleme | [Azure Backup](../articles/backup/backup-overview.md) , uygulama verilerinizi en düşük işletim maliyetleriyle korumanıza yardımcı olur. Uygulama hataları verilerinizi bozabilir ve insan hataları, uygulamalarınızda hatalar oluşturabilir. Azure Backup, Windows ve Linux çalıştıran sanal makineleriniz korunur. | - |
| Genel | Daha büyük esnekliği ve kullanılabilirlik için birden çok VM kullanma | VM 'niz yüksek kullanılabilirliğe sahip olması gereken uygulamalar çalıştırıyorsa birden çok VM veya [kullanılabilirlik kümesi](../articles/virtual-machines/windows/manage-availability.md) gerekir | - |
| Genel | İş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejisini benimseyin | Azure Site Recovery, iş sürekliliği gereksinimlerinizi desteklemek için tasarlanan farklı seçenekler arasından seçim yapmanıza olanak tanır. Farklı çoğaltmayı destekler ve senaryoları aşırı devreder. Daha fazla bilgi için bkz. [Site Recovery](../articles/site-recovery/site-recovery-overview.md) | - |
| Kimlik ve erişim yönetimi | VM kimlik doğrulamasını merkezileştirme | [Azure AD kimlik doğrulaması](../articles/active-directory/develop/authentication-scenarios.md)kullanarak Windows ve Linux sanal makinelerinizin kimlik doğrulamasını merkezileştirebilirsiniz. | - |
| Veri güvenliği | İşletim sistemi disklerini şifreleyin | [Azure disk şifrelemesi](../articles/security/azure-security-disk-encryption-overview.md) , Windows ve Linux IaaS sanal makine disklerinizi şifrelemenize yardımcı olur. Diskleri şifrelemek gerekli anahtarlar olmadan içeriği okunamaz hale getirir. Disk şifrelemesi, depolanan verileri, aksi takdirde, disk kopyalanırsa mümkün olmayan yetkisiz erişimlerden korur| [Evet](../articles/security-center/security-center-apply-disk-encryption.md) |
| Veri güvenliği | Veri disklerini şifreleme | [Azure disk şifrelemesi](../articles/security/azure-security-disk-encryption-overview.md) , Windows ve Linux IaaS sanal makine disklerinizi şifrelemenize yardımcı olur. Diskleri şifrelemek gerekli anahtarlar olmadan içeriği okunamaz hale getirir. Disk şifrelemesi, depolanan verileri, aksi takdirde, disk kopyalanırsa mümkün olmayan yetkisiz erişimlerden korur| -  |
| Veri güvenliği | Yüklü yazılımı sınırla | Çözümünüzü başarıyla uygulamak için gerekli olan yazılımı kısıtlamak, çözümünüzün saldırı yüzeyini azaltmaya yardımcı olur | - |
| Veri güvenliği | Virüsten koruma/kötü amaçlı yazılım kullanma | Azure 'da, Microsoft, Symantec, Trend Micro ve Kaspersky gibi güvenlik satıcılarından kötü amaçlı yazılımdan koruma yazılımı kullanabilirsiniz. Bu yazılım, sanal makinelerinizi kötü amaçlı dosyalardan, reklam yazılımlarından ve diğer tehditlerden korumanıza yardımcı olur. Microsoft kötü amaçlı yazılımdan koruma korumasını, uygulama iş yüklerinize göre temel güvenli veya gelişmiş özel yapılandırma ile dağıtabilirsiniz. Azure için Microsoft kötü amaçlı yazılımdan koruma hakkında daha fazla bilgi için bkz. [Azure Için Microsoft antimalware Cloud Services ve sanal makineler](../articles/security/azure-security-antimalware.md) | - |
| Veri güvenliği | Anahtarları ve gizli dizileri güvenli bir şekilde depolayın | Uygulamalar ve anahtarlarınızın yönetimini basitleştirip, uygulama sahiplerini güvenli bir merkezi olarak yönetilen bir seçenek sunarak, yanlışlıkla tehlikeye aşılma veya sızıntı riskini azaltmanızı sağlar. Azure Key Vault, anahtarlarınızı FIPS 140-2 düzey 2 ' ye sertifikalı donanım güvenlik modüllerinde (HSM 'ler) güvenli bir şekilde saklayabilir. FIPS 140,2 düzey 3 ' ü kullanarak Anahtarlarınızı ve sırlarınızı depolamanız gerekiyorsa [Azure ayrılmış HSM](../articles/dedicated-hsm/overview.md) 'yi kullanabilirsiniz | - |
| Ağ | Yönetim bağlantı noktalarına erişimi kısıtla | Saldırganlar açık yönetim bağlantı noktaları için genel bulut IP aralıklarını tarar ve ortak parolalar ve bilinen düzeltme eki yüklenmemiş güvenlik açıkları gibi "kolay" saldırıları dener. [Tam zamanında (JIT) sanal makine (VM) erişimi](../articles/security-center/security-center-just-in-time.md) , Azure VM 'lerinize gelen trafiği kilitlemek ve gerektiğinde VM 'lere bağlanmak için kolay erişim sağlarken saldırılara maruz kalma olasılığını azaltmak için kullanılabilir. | - |
| Ağ | Ağ erişimini sınırlandırma | Ağ güvenlik grupları ağ erişimini kısıtlayıp açığa çıkarılan uç noktaların sayısını denetlemenizi sağlar. Daha fazla bilgi için bkz. [ağ güvenlik grubu oluşturma, değiştirme veya silme](../articles/virtual-network/manage-network-security-group.md) | - |
| İzleme | Sanal makinelerinizi izleme | Azure sanal makinelerinizin ve sanal makine ölçek kümelerinin durumunu izlemek için [VM 'ler Için Azure İzleyicisi](../articles/azure-monitor/insights/vminsights-overview.md) 'ni kullanabilirsiniz. Bir VM ile ilgili performans sorunları, hizmetin kesintiye uğramasına neden olabilir ve bu da kullanılabilirliği güvenlik ilkesini ihlal eder. | - |

## <a name="next-steps"></a>Sonraki adımlar

Ek güvenlik gereksinimleri olup olmadığını görmek için uygulama sağlayıcınıza danışın. Güvenli uygulamalar geliştirme hakkında daha fazla bilgi için bkz. [Güvenli Geliştirme belgeleri](../articles/security/fundamentals/abstract-develop-secure-apps.md).