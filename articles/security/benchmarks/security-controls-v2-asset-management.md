---
title: Azure Güvenlik kıyaslama v2-varlık yönetimi
description: Azure Güvenlik kıyaslaması v2 varlık yönetimi
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/20/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: f0c2fe78c32357798e1f9acb43f5867df9148b38
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2020
ms.locfileid: "97368911"
---
# <a name="security-control-v2-asset-management"></a>Güvenlik denetimi v2: varlık yönetimi

Varlık yönetimi, Azure kaynakları üzerinde güvenlik görünürlüğü ve idare sağlamak için denetimleri ele alır. Bu, güvenlik personeli izinleri, varlık envanterine güvenlik erişimi ve hizmet ve kaynaklar için onayları yönetme (envanter, izleme ve düzeltme) konularında öneriler içerir.

## <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>AM-1: Güvenlik ekibinin varlıklarla ilgili riskleri görebildiğinden emin olun

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| ÖÖ-1 | 1,1, 1,2 | CM-8, PM-5 |

Azure Güvenlik Merkezi 'ni kullanarak güvenlik risklerini izleyebilmek için güvenlik Ekiplerinizde Azure kiracınızda ve aboneliklerinizde güvenlik okuyucusu izinleri verildiğinden emin olun. 

Güvenlik ekibi sorumluluklarının yapılandırmasına bağlı olarak, güvenlik risklerinin izlenmesi merkezi güvenlik ekibinin veya yerel bir ekibin sorumluluğunda olabilir. Gerçi güvenlik içgörüleri ve risklerinin kuruluş içinde her zaman merkezi bir konumda toplanması gerekir. 

Güvenlik Okuyucusu izinleri kiracının tamamına (Kök Yönetim Grubu) geniş kapsamlı olarak uygulanabileceği gibi, izinlerin kapsamı yönetim gruplarıyla veya belirli aboneliklerle de sınırlanabilir. 

Not: İş yükleri ve hizmetlerin görünürlüğünü elde etmek için ek izinler gerekebilir. 

- [Güvenlik Okuyucusu Rolüne Genel Bakış](../../role-based-access-control/built-in-roles.md#security-reader)

- [Azure Yönetim Gruplarına Genel Bakış](../../governance/management-groups/overview.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>AM-2: Güvenlik ekibinin varlık envanterine ve meta verilerine erişebildiğinden emin olun

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| ÖÖ-2 | 1,1, 1,2, 1,4, 1,5, 9,1, 12,1 | CM-8, PM-5 |

Güvenlik ekiplerinin Azure 'daki varlıkların sürekli güncelleştirilmiş envanterini erişimi olduğundan emin olun. Güvenlik ekipleri genellikle kuruluşlarının ortaya çıkabilecek risklerden olası etkilenme durumunu değerlendirmek ve sürekli güvenlik geliştirmelerine yönelik giriş sağlamak için bu envantere ihtiyaç duyar. 

Azure Güvenlik Merkezi envanter özelliği ve Azure Kaynak Grafiği, aboneliklerinizde Azure Hizmetleri, uygulamalar ve ağ kaynakları dahil tüm kaynakları sorgulayabilir ve bulabilir.  

Azure'daki etiketleri ve diğer meta verileri (ad, açıklama ve kategori) kullanarak varlıkları kuruluşunuzun sınıflandırma uygulamalarına göre mantıksal olarak düzenleyin.  

- [Azure Kaynak Grafı Gezgini ile sorgu oluşturma](../../governance/resource-graph/first-query-portal.md)

- [Azure Güvenlik Merkezi varlık envanteri yönetimi](../../security-center/asset-inventory.md)

- [Varlıkları etiketleme hakkında daha fazla bilgi için bkz. Kaynak adlandırma ve etiketleme karar Kılavuzu](/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=%2fazure%2fazure-resource-manager%2fmanagement%2ftoc.json)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="am-3-use-only-approved-azure-services"></a>AM-3: Yalnızca onaylı Azure hizmetlerini kullanın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| ÖÖ-3 | 2,3, 2,4 | CM-7, CM-8 |

Kullanıcılarınızın ortamınızda sağlayabileceği hizmetleri denetlemek ve kısıtlamak için Azure İlkesi'ni kullanın. Abonelikler içindeki kaynakları sorgulamak ve bulmak için Azure Kaynak Grafı'nı kullanın.  Ayrıca Azure İzleyici'yi kullanarak onaylanmamış hizmetler algılandığında uyarı tetikleme amacıyla kurallar oluşturabilirsiniz.

- [Azure Ilkesini yapılandırma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](../../governance/policy/samples/index.md)

- [Azure Kaynak Grafı Gezgini ile sorgu oluşturma](../../governance/resource-graph/first-query-portal.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)  

## <a name="am-4-ensure-security-of-asset-lifecycle-management"></a>VY-4: Varlık yaşam döngüsü yönetiminin güvenliğini sağlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| ÖÖ-4 | 2,3, 2,4, 2,5 | CM-7, CM-8, CM-10, CM-11 |

Potansiyel olarak yüksek etkili değişiklikler için varlık yaşam döngüsü yönetimi işlemlerine yönelik güvenlik ilkeleri oluşturun veya güncelleştirin. Bu değişiklikler kimlik sağlayıcıları ve erişim, veri duyarlılığı, ağ yapılandırması ve yönetici ayrıcalığı ataması ile ilgili değişikliklerdir.

Artık gerekmeyen Azure kaynaklarını kaldırın.

- [Azure kaynak grubunu ve kaynağını silme](../../azure-resource-manager/management/delete-resource-group.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)  

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>ÖÖ-5: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| ÖÖ-5 | 2.9 | AC-3 |

"Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşim kurmasına izin vermek için Azure AD koşullu erişimi kullanın.

- [Azure Kaynak Yöneticisi 'ne erişimi engellemek için koşullu erişimi yapılandırma](../../role-based-access-control/conditional-access-azure-management.md)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)  

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

## <a name="am-6-use-only-approved-applications-in-compute-resources"></a>HAR-6: yalnızca işlem kaynaklarında onaylanan uygulamaları kullanın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP 800-53 R4 ID 'leri |
|--|--|--|--|
| ÖÖ-6 | 2,6, 2,7 | AC-3, CM-7, CM-8, CM-10, CM-11 |

Yalnızca yetkili yazılımın yürütüldüğünü ve tüm yetkisiz yazılımların Azure sanal makinelerinde yürütülmesini engellediği emin olun.

Uygulama izin verilenler listesini bulup oluşturmak için Azure Güvenlik Merkezi (ASC) Uyarlamalı uygulama denetimlerini kullanın. Ayrıca, yalnızca yetkili yazılımın yürütüldüğünü ve tüm yetkisiz yazılımların Azure sanal makinelerinde yürütülmesini engellediği sağlamak için ASC Uyarlamalı uygulama denetimleri de kullanabilirsiniz.

Windows ve Linux VM 'lerinizin envanter bilgilerinin toplanmasını otomatikleştirmek için Azure Otomasyonu Değişiklik İzleme ve envanterini kullanın. Yazılım adı, sürüm, yayımcı ve yenileme süresi Azure portal kullanılabilir. Yazılım Yükleme tarihini ve diğer bilgileri almak için, Konuk düzeyinde tanılamayı etkinleştirin ve Windows olay günlüklerini Log Analytics çalışma alanına yönlendirin.

Betiklerin türüne bağlı olarak, kullanıcıların Azure işlem kaynaklarında betikleri yürütme yeteneğini sınırlamak için işletim sistemine özgü yapılandırma veya üçüncü taraf kaynakları kullanabilirsiniz. 

Onaylanmamış yazılımları bulup belirlemek için bir üçüncü taraf çözümü de kullanabilirsiniz.

- [Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerini kullanma](../../security-center/security-center-adaptive-application.md)

- [Azure Otomasyonu Değişiklik İzleme ve envanterini anlayın](../../automation/change-tracking/overview.md)

- [Windows ortamlarında PowerShell betiği yürütmeyi denetleme](/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6)

**Sorumluluk**: Müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)  

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)