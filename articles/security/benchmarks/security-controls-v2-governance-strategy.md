---
title: Azure Güvenlik kıyaslaması v2-Idare ve strateji
description: Azure Güvenlik kıyaslaması v2 Idare ve strateji
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/13/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: e8a5196bf71712caae1218933ed13345f4cecd99
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90059394"
---
# <a name="security-control-governance-and-strategy"></a>Güvenlik denetimi: Idare ve strateji

Yedekleme ve kurtarma, farklı hizmet katmanlarındaki veri ve yapılandırma yedeklemelerinin gerçekleştirilmesini, doğrulanmasını ve korunmasını sağlamak için denetimleri ele alır.

## <a name="gs-1-define-asset-management-and-protection-strategy"></a>GS-1: varlık yönetimi ve koruma stratejisini tanımlama

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

Note: bulut ve şirket içi varlık yönetimi ve koruma yaklaşımınız, uygulama hizmeti/barındırma modeli, iş riskleri ve uyumluluk gereksinimi gibi birden çok etkene bağlı olarak farklı olabilir. 

- [Azure Güvenlik mimarisi önerisi-depolama, veri ve şifreleme](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Azure güvenlik temelleri-Azure veri güvenliği, şifreleme ve depolama](../fundamentals/encryption-overview.md)

- [Bulut benimseme çerçevesi-Azure veri güvenliği ve şifreleme en iyi uygulamaları](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

- [Azure Güvenlik kıyaslaması-varlık yönetimi](/azure/security/benchmarks/security-controls-v2-asset-management)

- [Azure Güvenlik kıyaslaması-veri koruma](/azure/security/benchmarks/security-controls-v2-data-protection)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-2-define-security-posture-management-strategy"></a>GS-2: güvenlik sonrası yönetim stratejisini tanımlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| GS-2 | 20, 3, 5 | RA, CM, SC |

Bireysel varlıklarınızla ve barındırıldığı ortamda riskleri sürekli olarak ölçün ve azaltabilirsiniz. Yayımlanan uygulamalar, ağ giriş ve çıkış noktaları, Kullanıcı ve yönetici uç noktaları vb. gibi yüksek değerli varlıkların ve yüksek oranda ortaya çıkarılan saldırı yüzeylerinin önceliklerini belirleyin.

- [Azure Güvenlik kıyaslaması-Posture ve güvenlik açığı yönetimi](/azure/security/benchmarks/security-controls-v2-posture-vulnerability-management)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-3-align-organization-roles-responsibilities-and-accountabilities"></a>GS-3: kuruluş rollerini, sorumlulukları ve accountacakları hizalayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| GS-3 | Yok | PL, PM |

Güvenlik kuruluşunuzdaki roller ve sorumlulukların açık bir stratejisini belgelemenizi ve iletdiğinizden emin olun. Güvenlik kararları, paylaşılan sorumluluk modeli eğitimi ve bulut güvenliği için teknik eğitim sağlamak için açık sorumluluk sağlama önceliklerini belirleyin. 

- [Azure Güvenlik En Iyi Yöntem 1 – Insanlar: ekipleri bulut güvenlik yolculuğunda eğitin](https://aka.ms/AzSec1)

- [Azure Güvenlik En Iyi uygulaması 2-Insanlar: bulut güvenlik teknolojisindeki takımları eğitin](https://aka.ms/AzSec2)

- [Azure Güvenlik En Iyi Yöntem 3-Işlem: bulut güvenlik kararları için sorumluluk atama](https://aka.ms/AzSec3)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-4-define-network-security-strategy"></a>GS-4: ağ güvenlik stratejisini tanımlayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| GS-4 | 9 | CA, SC |

Kuruluşunuzun genel güvenlik erişimi denetim stratejisinin bir parçası olarak bir Azure ağ güvenlik yaklaşımı oluşturun.  

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   Merkezi ağ yönetimi ve güvenlik sorumluluğu

-   Kurumsal segmentasyon stratejisi ile hizalanan sanal ağ kesimleme modeli

-   Farklı tehdit ve saldırı senaryolarında düzeltme stratejisi

-   Internet Edge ve giriş ve çıkış stratejisi

-   Karma bulut ve şirket içi bağlantı stratejisi

-   Güncel ağ güvenlik yapıtları (ör. ağ diyagramları, başvuru ağ mimarisi)

Note: bulut ve şirket içi ağ güvenlik yaklaşımınız, uygulama hizmeti modeli, tehdit pozlaması ve karma ağ kurulumu gibi birden çok etkene bağlı olarak farklı olabilir.

- [Azure Güvenlik En Iyi Yöntem 11 mimarisi. Tek Birleşik güvenlik stratejisi](https://aka.ms/AzSec11)

- [Azure Güvenlik kıyaslaması-ağ güvenliği](/azure/security/benchmarks/security-controls-v2-network-security)

- [Azure ağ güvenliğine genel bakış](../fundamentals/network-overview.md)

- [Kurumsal ağ mimarisi stratejisi](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-5-define-identity-and-privileged-access-strategy"></a>GS-5: kimlik ve ayrıcalıklı erişim stratejisi tanımlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| GS-5 | 16, 4 | AC, AU, SC |

Kuruluşunuzun genel güvenlik erişimi denetim stratejisinin bir parçası olarak bir Azure kimliği ve ayrıcalıklı erişim yaklaşımları oluşturun.  

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   Merkezi bir kimlik ve kimlik doğrulama sistemi ve diğer iç ve dış kimlik sistemleriyle karşılıklı bağlantısı

-   Farklı kullanım durumlarında ve koşullarda güçlü kimlik doğrulama yöntemleri

-   Yüksek ayrıcalıklı kullanıcıların korunması

-   Anomali Kullanıcı etkinlikleri izleme ve işleme  

-   Kullanıcı kimliği ve erişim gözden geçirme ve mutabakat süreci

Note: bulut ve şirket içi için kimlik ve ayrıcalıklı erişim yaklaşımınız, veri/uygulama erişim yolu, hizmet modeli ve müşteri/iş ortağı erişim stratejisi gibi birden çok etkene bağlı olarak farklı olabilir.

- [Azure Güvenlik kıyaslaması-kimlik yönetimi](/azure/security/benchmarks/security-controls-v2-identity-management)

- [Azure Güvenlik kıyaslaması-ayrıcalıklı erişim](/azure/security/benchmarks/security-controls-v2-privileged-access)

- [Azure Güvenlik En Iyi Yöntem 11 mimarisi. Tek Birleşik güvenlik stratejisi](https://aka.ms/AzSec11)

- [Azure Identity Management güvenliğine genel bakış](../fundamentals/identity-management-overview.md) 

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-6-define-logging-and-threat-response-strategy"></a>GS-6: günlüğe kaydetme ve tehdit yanıt stratejisini tanımlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| GS-6 | 19 | ıR, AU, RA, SC |

Uyumluluk gereksinimlerini karşılaırken tehditleri hızlı bir şekilde tespit etmek ve düzeltmek için bir günlüğe kaydetme ve tehdit yanıt stratejisi oluşturun. Tümleştirme ve el ile adımlar yerine tehditlere odaklanabilmeleri için yüksek kaliteli uyarılar ve sorunsuz deneyimlerle analistleri sağlamayı önceliklendirin. 

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   Güvenlik işlemleri (SecOps) kuruluşun rolü ve sorumlulukları 

-   NıST veya başka bir sektör çerçevesiyle hizalanan iyi tanımlanmış bir olay yanıt işlemi 

-   Tehdit algılamayı, olay yanıtını ve uyumluluk gereksinimlerini desteklemek için günlüğü yakalama ve bekletme

-   SıEM, yerel Azure özellikleri ve diğer kaynakları kullanarak tehditler hakkında merkezi görünürlük ve bağıntı bilgileri 

-   Müşterilerinizle, Tedarikçilerinizden ve genel ilgi şahıslarla iletişim ve bildirim planı

-   Günlüğe kaydetme ve tehdit algılama, Forensics ve saldırı düzeltme ve doğrulama gibi olay işleme için Azure yerel ve üçüncü taraf platformlarının kullanımı

-   Olayları ve olay sonrası etkinliklerini (öğrenilmiş dersler ve kanıt bekletme gibi) işleme için süreçler

Note: bulut ve şirket içi tehdit algılama yaklaşımınız, uyumluluk gereksinimi, tehdit dünyası ve algılama ve düzeltme özelliği gibi birden çok etkene bağlı olarak farklı olabilir. 

- [Azure Güvenlik kıyaslaması-günlüğe kaydetme ve tehdit algılama](/azure/security/benchmarks/security-controls-v2-logging-threat-detection)

- [Azure Güvenlik kıyaslaması-olay yanıtı](/azure/security/benchmarks/security-controls-v2-incident-response)

- [Azure Güvenlik En Iyi Yöntem 4-Işlem. Bulut için olay yanıtı Işlemini Güncelleştir](https://aka.ms/AzSec11)

- [Azure benimseme çerçevesi, günlüğe kaydetme ve raporlama kararı Kılavuzu](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Azure Kurumsal ölçek, yönetim ve izleme](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

## <a name="gs-7-define-backup-and-recovery-strategy"></a>GS-7: yedekleme ve kurtarma stratejisini tanımlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| GS-7 | 10 | CP |

Kuruluşunuz için bir Azure yedekleme ve kurtarma stratejisi kurun. 

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   İş dayanıklılık hedeflerinize uygun olarak kurtarma süresi hedefi (RTO) ve kurtarma noktası hedefi (RPO) tanımları

-   Uygulamalarınızda ve altyapı kurulumunda artıklık tasarımı

-   Erişim denetimi ve veri şifrelemesi kullanılarak yedeklemenin korunması

Note: bulut ve şirket içi yedekleme ve kurtarma yaklaşımınız, altyapı yedekliliği, uygulama hizmeti/barındırma modeli ve uyumluluk gereksinimleri gibi birden çok etkene bağlı olarak farklı olabilir.

- [Azure Güvenlik kıyaslaması-yedekleme ve kurtarma](/azure/security/benchmarks/security-controls-v2-backup-recovery)

- [Azure Iyi mimari çerçeve-Azure uygulamaları için yedekleme ve olağanüstü durum kurtarma](/azure/architecture/framework/resiliency/backup-and-recovery)

- [Azure benimseme çerçevesi-iş sürekliliği ve olağanüstü durum kurtarma](/azure/cloud-adoption-framework/ready/enterprise-scale/business-continuity-and-disaster-recovery)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Tüm paydaşlar](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)

