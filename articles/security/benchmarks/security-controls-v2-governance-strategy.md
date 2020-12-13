---
title: Azure Güvenlik kıyaslaması v2-Idare ve strateji
description: Azure Güvenlik kıyaslaması v2 Idare ve strateji
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/20/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 935a6b41152ff914889e299b6455fbb30b2f0447
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2020
ms.locfileid: "97369064"
---
# <a name="security-control-v2-governance-and-strategy"></a>Güvenlik denetimi v2: Idare ve strateji

İdare ve strateji, farklı bulut güvenlik işlevlerine, Birleşik teknik stratejiye ve destekleyici ilke ve standartlara yönelik roller ve sorumluluklar oluşturma gibi güvenlik güvencesini sağlayan ve güvenlik güvencesi sağlamak için yönergeler sağlar.

## <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: Varlık yönetimi ve veri koruma stratejisi tanımlayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| GS-1 | 2, 13 | SC, AC |

Sistemleri ve verileri sürekli izleme ve koruma için açık bir strateji belgelemenizi ve iletdiğinizden emin olun. İş açısından kritik verilerin ve sistemlerin bulma, değerlendirme, koruma ve izleme süreçleri için öncelik belirleyin. 

Bu strateji kapsamında aşağıdaki öğeler için rehberlik, ilkeler ve standartlar belgelenmiş olmalıdır: 

- İş risklerine göre veri sınıflandırma standardı

- Riskler ve varlık envanteriyle ilgili güvenlik kuruluşu görünürlüğü 

- Güvenlik kuruluşunun kullanılmasını onayladığı Azure hizmetleri 

- Varlıkların yaşam döngüsü boyunca güvenliği

- Kurumsal verilerin sınıflandırmasına uygun olarak gerekli erişim denetimi stratejisi

- Azure yerel ve üçüncü taraf veri koruma özelliklerinin kullanımı

- Aktarımdaki ve bekleyen veri kullanım örneklerine yönelik veri şifreleme gereksinimleri

- Uygun şifreleme standartları

Daha fazla bilgi için aşağıdaki başvuruları inceleyin:
- [Azure Güvenlik Mimarisi Önerileri: Depolama, veri ve şifreleme](/azure/architecture/framework/security/storage-data-encryption?bc=%2fsecurity%2fcompass%2fbreadcrumb%2ftoc.json&toc=%2fsecurity%2fcompass%2ftoc.json)

- [Azure Güvenliği Temelleri: Azure'da veri güvenliği, şifreleme ve depolama](../fundamentals/encryption-overview.md)

- [Bulut Benimseme Çerçevesi: Azure'da veri güvenliği ve şifreleme için en iyi deneyimler](../fundamentals/data-encryption-best-practices.md?bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json)

- [Azure Güvenlik Karşılaştırması: Varlık yönetimi](security-controls-v2-asset-management.md)

- [Azure Güvenlik Karşılaştırması: Veri koruma](security-controls-v2-data-protection.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: Kurumsal segmentasyon stratejisini tanımlayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| GS-2 | 4, 9, 16 | AC, CA, SC |

Kimlik, ağ, uygulama, abonelik, yönetim grubu ve diğer denetimlerin birleşimini kullanarak varlıklara erişimi parçalara ayırma konusunda kurumsal çapta bir strateji oluşturun.

Güvenlik ayrımı gereksinimiyle birbirleriyle iletişim kurma ve verilere erişme gereksinimine sahip olan sistemlerin günlük çalışmasını etkinleştirme gereksinimi arasında bir denge kurun.

Segmentasyon stratejisinin ağ güvenliği, kimlik ve erişim modelleri, uygulama izinleri/erişim modelleri ve insanlar tarafından gerçekleştirilen süreç denetimleri gibi farklı denetim türlerinde tutarlı bir şekilde uygulandığından emin olun.

- [Azure için segmentasyon stratejisi rehberi (video)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Azure için segmentasyon stratejisi rehberi (belge)](/security/compass/governance#enterprise-segmentation-strategy)

- [Ağ segmentasyonunu kurumsal segmentasyon stratejisiyle uyumlu hale getirme](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: Güvenlik duruşu yönetim stratejisini tanımlayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| GS-3 | 20, 3, 5 | RA, CM, SC |

Bireysel varlıklarınızla ve barındırıldığı ortamda riskleri sürekli olarak ölçün ve azaltabilirsiniz. Yayımlanmış uygulamalar, ağ giriş ve çıkış noktaları, kullanıcı ve yönetici uç noktaları gibi değeri yüksek varlıkları ve kullanıma açık olan saldırı yüzeylerini önceliklendirin.

- [Azure Güvenlik Karşılaştırması: Duruş ve güvenlik açığı yönetimi](security-controls-v2-posture-vulnerability-management.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: Kuruluş genelindeki rolleri, sorumlulukları ve hesap verilebilirlik durumlarını uyumlu hale getirin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| GS-4 | Yok | PL, PM |

Güvenlik kuruluşunuzdaki roller ve sorumlulukların açık bir stratejisini belgelemenizi ve iletdiğinizden emin olun. Güvenlik kararları için net bir hesap verilebilirlik süreci oluşturmayı önceliklendirin, herkesi paylaşılan sorumluluk modeli hakkında eğitin ve teknik ekiplere bulut ortamının güvenliğini sağlamaya yönelik eğitimler verin.

- [Azure Güvenliği En İyi Deneyimi 1 - İnsanlar: Ekipleri Bulut Güvenliği Yolculuğu Hakkında Eğitin](/azure/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey)

- [Azure Güvenliği En İyi Deneyimi 2 - İnsanlar: Ekipleri Bulut Güvenliği Teknolojileri Hakkında Eğitin](/azure/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology)

- [Azure Güvenliği En İyi Deneyimi 3 - Süreç: Bulut Güvenliği Kararları için Hesap Verilebilirlik Ataması Yapın](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-5-define-network-security-strategy"></a>GS-5: Ağ güvenlik stratejisini tanımlayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| GS-5 | 9 | CA, SC |

Kuruluşunuzun genel güvenlik erişimi denetim stratejisinin bir parçası olarak bir Azure ağ güvenlik yaklaşımı oluşturun.  

Bu strateji kapsamında aşağıdaki öğeler için rehberlik, ilkeler ve standartlar belgelenmiş olmalıdır: 

- Merkezi ağ yönetimi ve güvenlik sorumluluğu

- Kurumsal segmentasyon stratejisiyle uyumlu sanal ağ segmentasyon modeli

- Farklı tehdit ve saldırı senaryolarına yönelik düzeltme stratejisi

- İnternet uç düğümü ile giriş ve çıkış stratejisi

- Hibrit bulut ve şirket içi bağlantı stratejisi

- Güncel ağ güvenliği yapıtları (ağ diyagramları, başvuru amaçlı ağ mimarisi vb.)

Daha fazla bilgi için aşağıdaki başvuruları inceleyin:

- [Azure Güvenliği En İyi Deneyimi 11 - Mimari. Tek ve birleşik güvenlik stratejisi](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure Güvenlik Karşılaştırması: Ağ Güvenliği](security-controls-v2-network-security.md)

- [Azure ile ağ güvenliğine genel bakış](../fundamentals/network-overview.md)

- [Kurumsal ağ mimarisi stratejisi](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: Kimlik ve ayrıcalıklı erişim stratejisini tanımlayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| GS-6 | 16, 4 | AC, AU, SC |

Kuruluşunuzun genel güvenlik erişimi denetim stratejisinin bir parçası olarak bir Azure kimliği ve ayrıcalıklı erişim yaklaşımları oluşturun.  

Bu strateji kapsamında aşağıdaki öğeler için rehberlik, ilkeler ve standartlar belgelenmiş olmalıdır: 

- Merkezi bir kimlik ve kimlik doğrulaması sistemi ile diğer iç ve dış kimlik sistemleriyle olan bağlantısı

- Farklı kullanım örneklerine ve koşullara yönelik güçlü kimlik doğrulama yöntemleri

- Yüksek ayrıcalıklı kullanıcıların korunması

- Kullanıcı etkinlikleri için anomali izleme ve işleme  

- Kullanıcı kimliği ve erişim gözden geçirmesi ile uzlaştırma süreci

Daha fazla bilgi için aşağıdaki başvuruları inceleyin:

- [Azure Güvenlik Karşılaştırması: Kimlik yönetimi](security-controls-v2-identity-management.md)

- [Azure Güvenlik Karşılaştırması: Ayrıcalıklı erişim](security-controls-v2-privileged-access.md)

- [Azure Güvenliği En İyi Deneyimi 11 - Mimari. Tek ve birleşik güvenlik stratejisi](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure'da kimlik yönetim güvenliğine genel bakış](../fundamentals/identity-management-overview.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: Günlüğe kaydetme ve tehdit yanıtı stratejisi tanımlayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| GS-7 | 19 | ıR, AU, RA, SC |

Uyumluluk gereksinimlerini karşılaırken tehditleri hızlı bir şekilde tespit etmek ve düzeltmek için bir günlüğe kaydetme ve tehdit yanıt stratejisi oluşturun. Analistlerin tümleştirmeyle ve el ile uygulanan adımlarla ilgilenmek yerine tehditlere odaklanabilmelerini sağlamak için onlara yüksek kaliteli uyarılar ve sorunsuz deneyimler sağlamaya öncelik verin. 

Bu strateji aşağıdaki öğelerle ilgili olarak belgelenmiş rehberlik, ilke ve standartlar içermelidir: 

- Güvenlik operasyonları (SecOps) organizasyonunun rolü ve sorumlulukları 

- NIST veya diğer sektör çerçeveleriyle uyumlu iyi tanımlanmış bir olay yanıt süreci 

- Tehdit algılama, olay yanıtı ve uyumluluk gereksinimlerini karşılamak için günlük yakalama ve saklama

- SIEM, yerel Azure özellikleri ve diğer kaynaklar aracılığıyla merkezi görünürlük ve bilgi bağıntısı 

- Planı müşterilerinize, tedarikçilerinize ve diğer proje katılımcılarına iletme ve onları bilgilendirme

- Günlüğe kaydetme ve tehdit algılama, adli araştırma, saldırı önleme ve ortadan kaldırma gibi olay işleme adımları için yerel Azure ve üçüncü taraf platformları kullanma

- Olayları ve çıkarılan dersler ile kanıt koruma gibi olay sonrası etkinlikleri işlemeye yönelik süreçler

Daha fazla bilgi için aşağıdaki başvuruları inceleyin:
- [Azure Güvenlik Karşılaştırması: Günlüğe kaydetme ve tehdit algılama](security-controls-v2-logging-threat-detection.md)

- [Azure Güvenlik Karşılaştırması: Olay yanıtı](security-controls-v2-incident-response.md)

- [Azure Güvenliği En İyi Deneyimi 4 - Süreç. Bulut için Olay Yanıt Sürecini Güncelleştirme](/azure/cloud-adoption-framework/security/security-top-10#3-process-assign-accountability-for-cloud-security-decisions)

- [Azure Benimseme Çerçevesi, günlüğe kaydetme ve raporlama kararı rehberi](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Azure ile kurumsal ölçek, yönetim ve izleme](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-8-define-backup-and-recovery-strategy"></a>GS-8: yedekleme ve kurtarma stratejisini tanımlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| GS-8 | 10 | CP |

Kuruluşunuz için bir Azure yedekleme ve kurtarma stratejisi kurun. 

Bu strateji kapsamında aşağıdaki öğeler için rehberlik, ilkeler ve standartlar belgelenmiş olmalıdır: 

- İş dayanıklılık hedeflerinize uygun olarak kurtarma süresi hedefi (RTO) ve kurtarma noktası hedefi (RPO) tanımları

- Uygulamalarınızda ve altyapı kurulumunda artıklık tasarımı

- Erişim denetimi ve veri şifrelemesi kullanılarak yedeklemenin korunması

Daha fazla bilgi için aşağıdaki başvuruları inceleyin:
- [Azure Güvenlik kıyaslaması-yedekleme ve kurtarma](security-controls-v2-backup-recovery.md)

- [Azure Well-Architecture Framework-Azure uygulamaları için yedekleme ve olağanüstü durum kurtarma](/azure/architecture/framework/resiliency/backup-and-recovery)

- [Azure benimseme çerçevesi-iş sürekliliği ve olağanüstü durum kurtarma](/azure/cloud-adoption-framework/ready/enterprise-scale/business-continuity-and-disaster-recovery)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)