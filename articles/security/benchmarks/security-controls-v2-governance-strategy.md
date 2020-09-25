---
title: Azure Güvenlik kıyaslaması v2-Idare ve strateji
description: Azure Güvenlik kıyaslaması v2 Idare ve strateji
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/20/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 255e5533dc19036624042a0b82116c4dc107d762
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91336233"
---
# <a name="security-control-v2-governance-and-strategy"></a>Güvenlik denetimi v2: Idare ve strateji

İdare ve strateji, farklı bulut güvenlik işlevlerine, Birleşik teknik stratejiye ve destekleyici ilke ve standartlara yönelik roller ve sorumluluklar oluşturma gibi güvenlik güvencesini sağlayan ve güvenlik güvencesi sağlamak için yönergeler sağlar.

## <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: varlık yönetimi ve veri koruma stratejisini tanımlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| GS-1 | 2, 13 | SC, AC |

Sistemleri ve verileri sürekli izleme ve koruma için açık bir strateji belgelemenizi ve iletdiğinizden emin olun. İş açısından kritik veri ve sistemlerinin bulunmasını, değerlendirmesini, korunmasını ve izlenmesini önceliklendirin. 

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   Veri sınıflandırma standardı, iş riskleriyle uyumlu

-   Risk ve varlık envanterinde güvenlik kuruluşu görünürlüğü 

-   Azure hizmetlerinin kullanım için güvenlik kuruluşu onayı 

-   Varlıkların yaşam döngülerinde güvenliği

-   Kurumsal veri sınıflandırmasına uygun olarak gerekli erişim denetimi stratejisi

-   Azure yerel ve üçüncü taraf veri koruma özellikleri kullanımı

-   Geçiş içi ve REST kullanım örnekleri için veri şifreleme gereksinimleri

-   Uygun şifreleme standartları

Daha fazla bilgi için aşağıdaki başvurulara bakın:
- [Azure Güvenlik mimarisi önerisi-depolama, veri ve şifreleme](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Azure güvenlik temelleri-Azure veri güvenliği, şifreleme ve depolama](../fundamentals/encryption-overview.md)

- [Bulut benimseme çerçevesi-Azure veri güvenliği ve şifreleme en iyi uygulamaları](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

- [Azure Güvenlik kıyaslaması-varlık yönetimi](/azure/security/benchmarks/security-benchmark-v2-asset-management)

- [Azure Güvenlik kıyaslaması-veri koruma](/azure/security/benchmarks/security-benchmark-v2-data-protection)

**Sorumluluk**: müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: Kurumsal segmentleme stratejisini tanımlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| GS-2 | 4, 9, 16 | AC, CA, SC |

Kimlik, ağ, uygulama, abonelik, yönetim grubu ve diğer denetimlerin birleşimini kullanarak varlıklara erişimi parçalara ayırma konusunda kurumsal çapta bir strateji oluşturun.

Güvenlik ayrımı gereksinimini, birbirleriyle iletişim kurması gereken sistemlerin günlük işlemlerini etkinleştirme gereksinimiyle dikkatle dengeleyin ve verilere erişin.

Segmentleme stratejisinin ağ güvenliği, kimlik ve erişim modelleri ve uygulama izni/erişim modelleri ve insan işlem denetimleri gibi denetim türleri arasında tutarlı bir şekilde uygulandığından emin olun.

- [Azure 'da segmentasyon stratejisi ile ilgili kılavuz (video)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Azure 'da segmentasyon stratejisi ile ilgili kılavuz (belge)](/security/compass/governance#enterprise-segmentation-strategy)

- [Kurumsal segmentasyon stratejisi ile ağ segmentlemesini hizalayın](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Sorumluluk**: müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: güvenlik sonrası yönetim stratejisini tanımlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| GS-3 | 20, 3, 5 | RA, CM, SC |

Bireysel varlıklarınızla ve barındırıldığı ortamda riskleri sürekli olarak ölçün ve azaltabilirsiniz. Yayımlanan uygulamalar, ağ giriş ve çıkış noktaları, Kullanıcı ve yönetici uç noktaları vb. gibi yüksek değerli varlıkların ve yüksek oranda ortaya çıkarılan saldırı yüzeylerinin önceliklerini belirleyin.

- [Azure Güvenlik kıyaslaması-Posture ve güvenlik açığı yönetimi](/azure/security/benchmarks/security-benchmark-v2-posture-vulnerability-management)

**Sorumluluk**: müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: kuruluş rollerini, sorumlulukları ve accountacakları hizalayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| GS-4 | Yok | PL, PM |

Güvenlik kuruluşunuzdaki roller ve sorumlulukların açık bir stratejisini belgelemenizi ve iletdiğinizden emin olun. Güvenlik kararlarını açık bir şekilde yapın, paylaşılan sorumluluk modelinde herkese eğitilendir ve bulutu güvenli hale getirmek için teknik ekipleri teknolojide eğitin.

- [Azure Güvenlik En Iyi Yöntem 1 – Insanlar: ekipleri bulut güvenlik yolculuğunda eğitin](https://aka.ms/AzSec1)

- [Azure Güvenlik En Iyi uygulaması 2-Insanlar: bulut güvenlik teknolojisindeki takımları eğitin](https://aka.ms/AzSec2)

- [Azure Güvenlik En Iyi Yöntem 3-Işlem: bulut güvenlik kararları için sorumluluk atama](https://aka.ms/AzSec3)

**Sorumluluk**: müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-5-define-network-security-strategy"></a>GS-5: ağ güvenlik stratejisini tanımlayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| GS-5 | 9 | CA, SC |

Kuruluşunuzun genel güvenlik erişimi denetim stratejisinin bir parçası olarak bir Azure ağ güvenlik yaklaşımı oluşturun.  

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   Merkezi ağ yönetimi ve güvenlik sorumluluğu

-   Kurumsal segmentasyon stratejisi ile hizalanan sanal ağ kesimleme modeli

-   Farklı tehdit ve saldırı senaryolarında düzeltme stratejisi

-   Internet Edge ve giriş ve çıkış stratejisi

-   Karma bulut ve şirket içi bağlantı stratejisi

-   Güncel ağ güvenlik yapıtları (ör. ağ diyagramları, başvuru ağ mimarisi)

Daha fazla bilgi için aşağıdaki başvurulara bakın:

- [Azure Güvenlik En Iyi Yöntem 11 mimarisi. Tek Birleşik güvenlik stratejisi](https://aka.ms/AzSec11)

- [Azure Güvenlik kıyaslaması-ağ güvenliği](/azure/security/benchmarks/security-benchmark-v2-network-security)

- [Azure ağ güvenliğine genel bakış](../fundamentals/network-overview.md)

- [Kurumsal ağ mimarisi stratejisi](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Sorumluluk**: müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: kimlik ve ayrıcalıklı erişim stratejisi tanımlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| GS-6 | 16, 4 | AC, AU, SC |

Kuruluşunuzun genel güvenlik erişimi denetim stratejisinin bir parçası olarak bir Azure kimliği ve ayrıcalıklı erişim yaklaşımları oluşturun.  

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   Merkezi bir kimlik ve kimlik doğrulama sistemi ve diğer iç ve dış kimlik sistemleriyle karşılıklı bağlantısı

-   Farklı kullanım durumlarında ve koşullarda güçlü kimlik doğrulama yöntemleri

-   Yüksek ayrıcalıklı kullanıcıların korunması

-   Anomali Kullanıcı etkinlikleri izleme ve işleme  

-   Kullanıcı kimliği ve erişim gözden geçirme ve mutabakat süreci

Daha fazla bilgi için aşağıdaki başvurulara bakın:

- [Azure Güvenlik kıyaslaması-kimlik yönetimi](/azure/security/benchmarks/security-benchmark-v2-identity-management)

- [Azure Güvenlik kıyaslaması-ayrıcalıklı erişim](/azure/security/benchmarks/security-benchmark-v2-privileged-access)

- [Azure Güvenlik En Iyi Yöntem 11 mimarisi. Tek Birleşik güvenlik stratejisi](https://aka.ms/AzSec11)

- [Azure Identity Management güvenliğine genel bakış](../fundamentals/identity-management-overview.md) 

**Sorumluluk**: müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: günlüğe kaydetme ve tehdit yanıt stratejisini tanımlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| GS-7 | 19 | ıR, AU, RA, SC |

Uyumluluk gereksinimlerini karşılaırken tehditleri hızlı bir şekilde tespit etmek ve düzeltmek için bir günlüğe kaydetme ve tehdit yanıt stratejisi oluşturun. Tümleştirme ve el ile adımlar yerine tehditlere odaklanabilmeleri için yüksek kaliteli uyarılar ve sorunsuz deneyimlerle analistleri sağlamayı önceliklendirin. 

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   Güvenlik işlemleri (SecOps) kuruluşun rolü ve sorumlulukları 

-   NıST veya başka bir sektör çerçevesiyle hizalanan iyi tanımlanmış bir olay yanıt işlemi 

-   Tehdit algılamayı, olay yanıtını ve uyumluluk gereksinimlerini desteklemek için günlüğü yakalama ve bekletme

-   SıEM, yerel Azure özellikleri ve diğer kaynakları kullanarak tehditler hakkında merkezi görünürlük ve bağıntı bilgileri 

-   Müşterilerinizle, Tedarikçilerinizden ve genel ilgi şahıslarla iletişim ve bildirim planı

-   Günlüğe kaydetme ve tehdit algılama, Forensics ve saldırı düzeltme ve doğrulama gibi olay işleme için Azure yerel ve üçüncü taraf platformlarının kullanımı

-   Olayları ve olay sonrası etkinliklerini (öğrenilmiş dersler ve kanıt bekletme gibi) işleme için süreçler

Daha fazla bilgi için aşağıdaki başvurulara bakın:
- [Azure Güvenlik kıyaslaması-günlüğe kaydetme ve tehdit algılama](/azure/security/benchmarks/security-benchmark-v2-logging-threat-detection)

- [Azure Güvenlik kıyaslaması-olay yanıtı](/azure/security/benchmarks/security-benchmark-v2-incident-response)

- [Azure Güvenlik En Iyi Yöntem 4-Işlem. Bulut için olay yanıtı Işlemini Güncelleştir](https://aka.ms/AzSec11)

- [Azure benimseme çerçevesi, günlüğe kaydetme ve raporlama kararı Kılavuzu](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Azure Kurumsal ölçek, yönetim ve izleme](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Sorumluluk**: müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-8-define-backup-and-recovery-strategy"></a>GS-8: yedekleme ve kurtarma stratejisini tanımlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| GS-8 | 10 | CP |

Kuruluşunuz için bir Azure yedekleme ve kurtarma stratejisi kurun. 

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   İş dayanıklılık hedeflerinize uygun olarak kurtarma süresi hedefi (RTO) ve kurtarma noktası hedefi (RPO) tanımları

-   Uygulamalarınızda ve altyapı kurulumunda artıklık tasarımı

-   Erişim denetimi ve veri şifrelemesi kullanılarak yedeklemenin korunması

Daha fazla bilgi için aşağıdaki başvurulara bakın:
- [Azure Güvenlik kıyaslaması-yedekleme ve kurtarma](/azure/security/benchmarks/security-benchmark-v2-backup-recovery)

- [Azure Iyi mimari çerçeve-Azure uygulamaları için yedekleme ve olağanüstü durum kurtarma](/azure/architecture/framework/resiliency/backup-and-recovery)

- [Azure benimseme çerçevesi-iş sürekliliği ve olağanüstü durum kurtarma](/azure/cloud-adoption-framework/ready/enterprise-scale/business-continuity-and-disaster-recovery)

**Sorumluluk**: müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

