---
title: include dosyası
description: include dosyası
services: virtual-machines
author: barclayn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/13/2019
ms.author: barclayn
ms.custom: include file
ms.openlocfilehash: d0ede337f2ae17f28cd02df7ed35919bd9955e7a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74048558"
---
Bu makale, Azure Sanal Makineleri için güvenlik önerileri içerir. Ortak sorumluluk için modelimizde açıklanan güvenlik yükümlülüklerini yerine getirmeye yardımcı olmak için bu önerileri uygulayın. Öneriler, web uygulaması çözümleriniz için genel güvenliği artırmanıza da yardımcı olur. Microsoft'un hizmet sağlayıcı sorumluluklarını yerine getirmek için ne yaptığı hakkında daha fazla bilgi için bulut [bilgi işlem için Paylaşılan sorumluluklara](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91)bakın.

Bu makalenin önerilerinden bazıları Azure Güvenlik Merkezi tarafından otomatik olarak ele alınabilir. Azure Güvenlik Merkezi, Azure'daki kaynaklarınız için ilk savunma hattıdır. Olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli aralıklarla analiz eder. Daha sonra güvenlik açıklarını nasıl gidereceklerini önerir. Daha fazla bilgi için [Azure Güvenlik Merkezi'ndeki Güvenlik önerilerine](../articles/security-center/security-center-recommendations.md)bakın.

Azure Güvenlik Merkezi hakkında genel bilgi için azure [güvenlik merkezi nedir?](../articles/security-center/security-center-intro.md)

## <a name="general"></a>Genel

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
| Özel VM görüntüleri oluşturduğunuzda, en son güncelleştirmeleri uygulayın. | Resim oluşturmadan önce, işletim sistemi ve görüntünüzün bir parçası olacak tüm uygulamalar için en son güncelleştirmeleri yükleyin.  | - |
| VM'lerinizi güncel tutun. | Azure'daki Windows ve Linux bilgisayarlarınız için işletim sistemi güncelleştirmelerini yönetmek için Azure Otomasyonu'ndaki [Güncelleştirme Yönetimi](../articles/automation/automation-update-management.md) çözümlerini kullanabilirsiniz. | [Evet](../articles/security-center/security-center-apply-system-updates.md) |
| VM'lerinizi yedekle. | [Azure Yedekleme,](../articles/backup/backup-overview.md) uygulama verilerinizin korunmasına yardımcı olur ve en az işletme maliyetine sahiptir. Uygulama hataları verilerinizi bozabilir ve insan hataları uygulamalarınıza hata getirebilir. Azure Yedekleme, Windows ve Linux çalıştıran VM'lerinizi korur. | - |
| Daha fazla esneklik ve kullanılabilirlik için birden fazla VM kullanın. | VM'niz çok kullanılabilir olması gereken uygulamaları çalıştırıyorsa, birden fazla VM veya [kullanılabilirlik kümesi](../articles/virtual-machines/windows/manage-availability.md)kullanın. | - |
| İş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejisini benimseyin. | Azure Site Kurtarma, iş sürekliliğini desteklemek için tasarlanmış farklı seçenekler arasından seçim yapmanızı sağlar. Farklı çoğaltma ve başarısız senaryoları destekler. Daha fazla bilgi için [Site Kurtarma Hakkında'ya](../articles/site-recovery/site-recovery-overview.md)bakın. | - |

## <a name="data-security"></a>Veri güvenliği

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
| İşletim sistemi disklerini şifreleyin. | [Azure Disk Şifreleme,](../articles/security/azure-security-disk-encryption-overview.md) Windows ve Linux IaaS VM disklerinizi şifrelemenize yardımcı olur. Gerekli anahtarlar olmadan, şifrelenmiş disklerin içeriği okunamaz. Disk şifreleme, depolanan verileri, disk kopyalanırsa mümkün olabilecek yetkisiz erişime karşı korur.| [Evet](../articles/security-center/security-center-apply-disk-encryption.md) |
| Veri disklerini şifreleyin. | [Azure Disk Şifreleme,](../articles/security/azure-security-disk-encryption-overview.md) Windows ve Linux IaaS VM disklerinizi şifrelemenize yardımcı olur. Gerekli anahtarlar olmadan, şifrelenmiş disklerin içeriği okunamaz. Disk şifreleme, depolanan verileri, disk kopyalanırsa mümkün olabilecek yetkisiz erişime karşı korur.| -  |
| Yüklü yazılımı sınırlayın. | Yüklü yazılımı, çözümünüzü başarıyla uygulamak için gereken yazılımla sınırlandırın. Bu kılavuz, çözümünüzün saldırı yüzeyini azaltmaya yardımcı olur. | - |
| Virüsten koruma veya kötü amaçlı yazılımdan koruma kullanın. | Azure'da, Microsoft, Symantec, Trend Micro ve Kaspersky gibi güvenlik satıcılarının kötü amaçlı yazılımdan koruma yazılımlarını kullanabilirsiniz. Bu yazılım, VM'lerinizi kötü amaçlı dosyalara, reklam yazılımlarından ve diğer tehditlerden korumaya yardımcı olur. Microsoft Antimalware'i uygulama iş yüklerinize göre dağıtabilirsiniz. Temel güvenli ve varsayılan veya gelişmiş özel yapılandırmayı kullanın. Daha fazla bilgi için Azure [Bulut Hizmetleri ve Sanal Makineler için Microsoft Kötü Amaçlı Yazılımdan Koruma](../articles/security/azure-security-antimalware.md)bilgisine bakın. | - |
| Anahtarları ve sırları güvenli bir şekilde saklayın. | Uygulama sahiplerinize güvenli ve merkezi olarak yönetilen bir seçenek sunarak sırlarınızın ve anahtarlarınızın yönetimini basitleştirin. Bu yönetim, kazara bir uzlaşma veya sızıntı riskini azaltır. Azure Key Vault, anahtarlarınızı FIPS 140-2 Düzey 2 sertifikalı donanım güvenlik modüllerinde (HSM' ler) güvenli bir şekilde saklayabilir. Anahtarlarınızı ve sırlarınızı depolamak için FIPs 140.2 Level 3 kullanmanız gerekiyorsa, [Azure Özel HSM'yi](../articles/dedicated-hsm/overview.md)kullanabilirsiniz. | - |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi 

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
| VM kimlik doğrulamasını merkezileştirin. | [Azure Active Directory kimlik doğrulamasını](../articles/active-directory/develop/authentication-scenarios.md)kullanarak Windows ve Linux VM'lerinizin kimlik doğrulamasını merkezileştirebilirsiniz. | - |

## <a name="monitoring"></a>İzleme

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
| VM'lerinizi izleyin. | Azure Sanal M'lerinizin ve sanal makine ölçek kümelerinizin durumunu izlemek [için Sanal Makineler için Azure Monitörünü](../articles/azure-monitor/insights/vminsights-overview.md) kullanabilirsiniz. VM ile ilgili performans sorunları, kullanılabilirlik güvenlik ilkesini ihlal eden hizmet kesintisine neden olabilir. | - |

## <a name="networking"></a>Ağ Oluşturma

| Öneri | Yorumlar | Güvenlik Merkezi |
|-|----|--|
| Yönetim bağlantı noktalarına erişimi kısıtlayın. | Saldırganlar açık yönetim bağlantı noktaları için genel bulut IP aralıklarını tarar ve yaygın parolalar ve bilinen yamasız güvenlik açıkları gibi "kolay" saldırılar girişiminde bulunmayı dener. Azure VM'lerinize gelen trafiği kilitlemek için [tam zamanında (JIT) VM erişimi](../articles/security-center/security-center-just-in-time.md) kullanabilirsiniz, bu da gerektiğinde VM'lere kolay bağlantı sağlarken saldırılara maruz kalmayı azaltır. | - |
| Ağ erişimini sınırlandırın. | Ağ güvenlik grupları, ağ erişimini kısıtlamanızı ve açıkta kalan uç noktaların sayısını denetlemenize olanak tanır. Daha fazla bilgi için bkz: [Ağ güvenlik grubu oluştur, değiştir veya sil.](../articles/virtual-network/manage-network-security-group.md) | - |

## <a name="next-steps"></a>Sonraki adımlar

Ek güvenlik gereksinimleri hakkında bilgi edinmek için uygulama sağlayıcınıza danışın. Güvenli uygulamalar geliştirme hakkında daha fazla bilgi için [Güvenli geliştirme belgelerine](../articles/security/fundamentals/abstract-develop-secure-apps.md)bakın.
