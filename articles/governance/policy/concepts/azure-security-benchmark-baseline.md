---
title: Azure Güvenlik kıyaslaması için Azure Ilkesi güvenlik temeli
description: Azure Ilkesi güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: azure-policy
ms.topic: conceptual
ms.date: 07/02/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 909d423b31bb76d8d6aaed994d1c9f7372cbc01f
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89069791"
---
# <a name="azure-policy-security-baseline-for-azure-security-benchmark"></a>Azure Güvenlik kıyaslaması için Azure Ilkesi güvenlik temeli

Bu güvenlik temeli [Azure Güvenlik kıyaslayıcılarından](../../../security/benchmarks/overview.md) Azure ilkesine kılavuzluk uygular. Azure Güvenlik kıyaslaması, bulut Çözümlerinizi Azure 'da nasıl güvence altına almak için öneriler sağlar. İçerik, Azure Güvenlik kıyaslaması tarafından tanımlanan **Uyumluluk etki alanları** ve **güvenlik denetimlerine** ve Azure ilkesi için geçerli olan ilgili kılavuza göre gruplandırılır. Azure Ilkesi için geçerli olmayan **denetimler** dışlandı. Azure Ilkesinin Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için, [tam Azure ilkesi güvenlik temeli eşleme dosyasına](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)bakın.

Azure Güvenlik kıyaslama denetimlerinin yerleşik girişim aracılığıyla yerleşik ilke tanımlarına eşlenmesi için bkz. [mevzuat uyumluluğu: Azure Güvenlik kıyaslaması](../samples/azure-security-benchmark.md).

Azure Ilkesi _sorumluluk_yerine terim _sahipliğini_ kullanır. _Sahiplik_hakkında daha fazla bilgi için bkz. bulutta [Azure ilke Ilke tanımları](./definition-structure.md#type) ve [paylaşılan sorumluluk](../../../security/fundamentals/shared-responsibility.md).


## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](../../../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Kılavuz**: Azure ilkesi olay kaynağını, tarihi, kullanıcıyı, zaman damgasını, kaynak adreslerini, hedef adreslerini ve diğer yararlı öğeleri içerecek şekilde otomatik olarak etkinleştirilen etkinlik günlüklerini kullanır.

* [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../../../azure-monitor/platform/diagnostic-settings.md)

* [Azure 'da günlüğe kaydetme ve farklı günlük türlerini anlama](../../../azure-monitor/platform/platform-logs-overview.md)


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve erişim denetimi](../../../security/benchmarks/security-control-identity-access-control.md).*

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın. 

Ayrıca, [Azure AD Privileged Identity Management](../../../active-directory/privileged-identity-management/pim-configure.md) ayrıcalıklı rolleri veya [Azure Resource Manager](../../../azure-resource-manager/management/overview.md)kullanarak tam zamanında/tam erişim çözümünü de etkinleştirebilirsiniz.


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure kaynaklarını açmak ve YAPıLANDıRMAK için MFA Ile Paws (ayrıcalıklı erişim iş istasyonları) kullanın.

* [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [Azure 'da MFA 'yı etkinleştirme](../../../active-directory/authentication/howto-mfa-getstarted.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](../../../security/benchmarks/security-control-data-protection.md).*

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Kılavuz**: Azure ilkesine erişimi denetlemek için Azure rol tabanlı erişim denetimi (Azure RBAC) kullanın.

* [Azure Ilkesinde Azure RBAC Izinleri](../overview.md#rbac-permissions-in-azure-policy)

* [Azure RBAC 'yi yapılandırma](../../../role-based-access-control/role-assignments-portal.md)


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure ilkesinde değişiklik gerçekleşirken uyarı oluşturmak için etkinlik günlükleri Ile Azure izleyici 'yi kullanın.

* [Azure etkinlik günlüğü olayları için uyarı oluşturma](../../../azure-monitor/platform/alerts-activity-log.md)


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](../../../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Kılavuz**: Azure kaynaklarına Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler verirsiniz. Uyumluluk ve tutarlı etiket yönetimini raporlamak ve zorlamak için Azure Ilkesi _değiştirme_ efektini kullanın.

* [Öğretici: ilke oluşturma ve yönetme](../tutorials/create-and-manage.md)

* [Öğretici: etiket yönetimini yönetme](../tutorials/govern-tags.md)


**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynakları envanterini tanımlama ve sürdürme

**Rehberlik**: Kurumsal gereksinimlerinize göre onaylanan ilke tanımlarının ve ilke atamalarının envanterini oluşturun.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aboneliklerinizde oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın.

* [Azure Ilkesini yapılandırma ve yönetme](../tutorials/create-and-manage.md)


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](../../../security/benchmarks/overview.md)
- [Azure güvenlik temelleri](../../../security/benchmarks/security-baselines-overview.md) hakkında daha fazla bilgi edinin
