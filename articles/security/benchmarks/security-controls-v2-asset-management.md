---
title: Azure Güvenlik kıyaslama v2-varlık yönetimi
description: Azure Güvenlik kıyaslaması v2 varlık yönetimi
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/13/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: ba010c0dd3384af895c68d980fcae788c2d6d45a
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90059389"
---
# <a name="security-control-asset-management"></a>Güvenlik denetimi: varlık yönetimi

Varlık yönetimi, Azure kaynakları üzerinde güvenlik görünürlüğü ve idare sağlamak için denetimleri ele alır. Bu, güvenlik personeli izinleri, varlık envanterine güvenlik erişimi ve hizmet ve kaynaklar için onayları yönetme (envanter, izleme ve düzeltme) konularında öneriler içerir.

## <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>ÖÖ-1: güvenlik ekibinin, varlıklar için riskleri görebilirliği sağlayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| ÖÖ-1 | 1,1, 1,2, 1,3, 1,4, 9,1, 12,1 | CM-7, CM-8, CM-11, PM-5 |

Azure Güvenlik Merkezi 'ni kullanarak güvenlik risklerini izleyebilmek için güvenlik Ekiplerinizde Azure kiracınızda ve aboneliklerinizde güvenlik okuyucusu izinleri verildiğinden emin olun. 

Güvenlik ekibinin sorumluluklarına bağlı olarak, güvenlik risklerini izlemek, merkezi bir güvenlik ekibinin veya yerel ekibin sorumluluğunda olabilir. Bu şekilde, güvenlik öngörüleri ve riskler her zaman bir kuruluş içinde toplanmalıdır. 

Güvenlik okuyucusu izinleri bir kiracının tamamına (kök yönetim grubu) veya yönetim gruplarına veya belirli aboneliklere göre uygulanabilir. 

Note: iş yüklerinin ve hizmetlerin görünürlüğünü almak için ek izinler gerekebilir. 

- [Güvenlik okuyucusu rolüne genel bakış](../../role-based-access-control/built-in-roles.md#security-reader)

- [Azure Yönetim Grupları 'ye Genel Bakış](../../governance/management-groups/overview.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>-2: güvenlik ekibinin varlık envanterini ve meta verileri erişimine sahip olduğundan emin olun

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| ÖÖ-2 | 1,5 | CM-8, PM-5 |

Güvenlik ekiplerinin Azure 'daki varlıkların sürekli güncelleştirilmiş envanterini erişimi olduğundan emin olun. Güvenlik ekipleri genellikle bu envanterin, kuruluşlarının riskli olma olasılığını değerlendirmek ve sürekli güvenlik geliştirmeleri için bir girdi olarak gereklidir. 

Azure Güvenlik Merkezi envanter özelliği ve Azure Kaynak Grafiği, aboneliklerinizde Azure Hizmetleri, uygulamalar ve ağ kaynakları dahil tüm kaynakları sorgulayabilir ve bulabilir.  

Etiketleri ve Azure 'daki diğer meta verileri (ad, açıklama ve kategori) kullanarak kuruluşunuzun sınıflandırışına göre mantıksal olarak düzenleyin.  

- [Azure Kaynak Grafiği Gezgini ile sorgu oluşturma](../../governance/resource-graph/first-query-portal.md)

- [Azure Güvenlik Merkezi varlık envanteri yönetimi](../../security-center/asset-inventory.md)

- [Varlıkları etiketleme hakkında daha fazla bilgi için bkz. Kaynak adlandırma ve etiketleme karar Kılavuzu](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="am-3-use-only-approved-azure-services"></a>HAR-3: yalnızca onaylanan Azure hizmetlerini kullanma

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| ÖÖ-3 | 2,3, 2,4 | CM-7, CM-8 |

Kullanıcıların ortamınızda hangi hizmetleri sağlayabileceğini denetlemek ve kısıtlamak için Azure Ilkesi ' ni kullanın. Azure Kaynak Grafiği 'ni kullanarak aboneliklerindeki kaynakları sorgulama ve bulma.  Onaylanmamış bir hizmet algılandığında uyarıları tetiklemeye yönelik kurallar oluşturmak için Azure Izleyici 'yi de kullanabilirsiniz.

- [Azure Ilkesini yapılandırma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](/azure/governance/policy/samples/not-allowed-resource-types)

- [Azure Kaynak Grafiği Gezgini ile sorgu oluşturma](../../governance/resource-graph/first-query-portal.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)  

## <a name="am-4-ensure-security-of-asset-lifecycle-management"></a>ÖÖ-4: varlık yaşam döngüsü yönetiminin güvenliğinin yapıldığından emin olun

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| ÖÖ-4 | 2,3, 2,4, 2,5 | CM-7, CM-8, CM-10, CM-11 |

Potansiyel olarak yüksek etkili değişiklikler için varlık yaşam döngüsü yönetimi işlemlerine yönelik güvenlik ilkeleri oluşturun veya güncelleştirin. Bu değişiklikler: kimlik sağlayıcıları ve erişim, veri duyarlılığı, ağ yapılandırması ve yönetim ayrıcalıkları atama değişiklikleri içerir.

Artık gerekli olmadığında Azure kaynaklarını kaldırın.

- [Azure kaynak grubunu ve kaynağını Sil](../../azure-resource-manager/management/delete-resource-group.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)  

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>ÖÖ-5: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| ÖÖ-5 | 2.9 | AC-3 |

"Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşim kurmasına izin vermek için Azure AD koşullu erişimi kullanın.

- [Azure Kaynak Yöneticisi 'ne erişimi engellemek için koşullu erişimi yapılandırma](../../role-based-access-control/conditional-access-azure-management.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)  

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

## <a name="am-6-use-only-approved-applications-in-compute-resources"></a>HAR-6: yalnızca işlem kaynaklarında onaylanan uygulamaları kullanın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| ÖÖ-6 | 2,6 | AC-3, CM-7, CM-8, CM-10, CM-11 |

Yalnızca yetkili yazılımın yürütüldüğünü ve tüm yetkisiz yazılımların Azure sanal makinelerinde yürütülmesini engellediği emin olun.

Uygulama izin verilenler listesini bulup oluşturmak için Azure Güvenlik Merkezi (ASC) Uyarlamalı uygulama denetimlerini kullanın. Ayrıca, yalnızca yetkili yazılımın yürütüldüğünü ve tüm yetkisiz yazılımların Azure sanal makinelerinde yürütülmesini engellediği sağlamak için ASC Uyarlamalı uygulama denetimleri de kullanabilirsiniz.

Windows ve Linux VM 'lerinizin envanter bilgilerinin toplanmasını otomatikleştirmek için Azure Otomasyonu Değişiklik İzleme ve envanterini kullanın. Yazılım adı, sürüm, yayımcı ve yenileme süresi Azure portal kullanılabilir. Yazılım Yükleme tarihini ve diğer bilgileri almak için, Konuk düzeyinde tanılamayı etkinleştirin ve Windows olay günlüklerini Log Analytics çalışma alanına yönlendirin.

Betiklerin türüne bağlı olarak, kullanıcıların Azure işlem kaynaklarında betikleri yürütme yeteneğini sınırlamak için işletim sistemine özgü yapılandırma veya üçüncü taraf kaynakları kullanabilirsiniz. 

Onaylanmamış yazılımları bulup belirlemek için bir üçüncü taraf çözümü de kullanabilirsiniz.

- [Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerini kullanma](../../security-center/security-center-adaptive-application.md)

- [Azure Otomasyonu Değişiklik İzleme ve envanterini anlayın](../../automation/change-tracking.md)

- [Windows ortamlarında PowerShell betiği yürütmeyi denetleme](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)  

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

