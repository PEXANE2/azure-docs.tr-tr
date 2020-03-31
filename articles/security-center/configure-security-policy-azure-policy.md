---
title: REST API'sini kullanarak Azure İlkesi güvenlik ilkeleri oluşturma ve değiştirme
description: REST API aracılığıyla Azure İlkesi ilkesi yönetimi hakkında bilgi edinin.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: c218b5dc8ca3bfa0358a9b6a0d4867696762a8d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77430950"
---
# <a name="configure-a-security-policy-in-azure-policy-using-the-rest-api"></a>REST API'sini kullanarak Azure İlkesi'nde bir güvenlik ilkesini yapılandırma

Azure Güvenlik Merkezi, Azure İlkesi ile yerel tümleştirmenin bir parçası olarak, ilke atamaları oluşturmak için Azure İlkesi'nin REST API'sinden yararlanmanızı sağlar. Aşağıdaki yönergeler, ilke atamalarının oluşturulmasının yanı sıra varolan atamaların özelleştirilmesinde size yol kat eder. 

Azure İlkesi'ndeki önemli kavramlar: 

- **İlke tanımı** bir kuraldır 

- **Girişim,** ilke tanımları (kurallar) koleksiyonudur 

- **Atama,** bir girişimin veya politikanın belirli bir kapsama (yönetim grubu, abonelik, vb.) uygulanmasıdır. 

Güvenlik Merkezi, tüm güvenlik ilkelerini içeren yerleşik bir girişime sahiptir. Güvenlik Merkezi'nin Azure kaynaklarınızdaki ilkelerini değerlendirmek için, yönetim grubunda veya değerlendirmek istediğiniz abonelikte bir atama oluşturmanız gerekir.

Yerleşik girişim, Varsayılan olarak Güvenlik Merkezi'nin tüm ilkelerini etkinleştirmiştir. Yerleşik girişimden belirli ilkeleri devre dışı bırakmayı seçebilirsiniz. Örneğin, **web uygulaması güvenlik duvarı**dışındaki Güvenlik Merkezi'nin tüm ilkelerini uygulamak için, ilkeğin efekt parametresinin değerini Devre Dışı **Bırakılmış**olarak değiştirin. 

## <a name="api-examples"></a>API örnekleri

Aşağıdaki örneklerde, bu değişkenleri değiştirin:

- **{scope}** ilkeyi uyguladığınız yönetim grubunun veya aboneliğinin adını girin.
- **{policyAssignmentName}** [ilgili ilke atamasının adını](#policy-names)girin.
- **{name}** adınızı veya ilke değişikliğini onaylayan yöneticinin adını girin.

Bu örnek, yerleşik Güvenlik Merkezi girişimini bir abonelik veya yönetim grubunda nasıl atayabileceğinizi gösterir
 
 ```
    PUT  
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 

    Request Body (JSON) 

    { 

      "properties":{ 

    "displayName":"Enable Monitoring in Azure Security Center", 

    "metadata":{ 

    "assignedBy":"{Name}" 

    }, 

    "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 

    "parameters":{}, 

    } 

    } 
 ```

Bu örnek, aşağıdaki ilkeler devre dışı bırakılmış bir abonelik için yerleşik Güvenlik Merkezi girişimini nasıl atayabileceğinizi gösterir: 

- Sistem güncellemeleri ("systemUpdatesMonitoringEffect") 

- Güvenlik yapılandırmaları ("systemConfigurationsMonitoringEffect") 

- Uç nokta koruması ("endpointProtectionMonitoringEffect") 

 ```
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 
    
    Request Body (JSON) 
    
    { 
    
      "properties":{ 
    
    "displayName":"Enable Monitoring in Azure Security Center", 
    
    "metadata":{ 
    
    "assignedBy":"{Name}" 
    
    }, 
    
    "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 
    
    "parameters":{ 
    
    "systemUpdatesMonitoringEffect":{"value":"Disabled"}, 
    
    "systemConfigurationsMonitoringEffect":{"value":"Disabled"}, 
    
    "endpointProtectionMonitoringEffect":{"value":"Disabled"}, 
    
    }, 
    
     } 
    
    } 
 ```
Bu örnek, bir atamayı nasıl kaldırabileceğinizi gösterir:
 ```
    DELETE   
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 
 ```

## <a name="policy-names-reference"></a>İlke adları başvurusu<a name="policy-names"></a>

|Güvenlik Merkezi'nde ilke adı|Azure İlkesi'nde görüntülenen ilke adı |İlke etkisi parametre adı|
|----|----|----|
|SQL Şifrelemesi |Azure Güvenlik Merkezi'nde şifrelenmemiş SQL veritabanını izleme |sqlEncryptionMonitoringEffect| 
|SQL Denetimi |Azure Güvenlik Merkezi'nde denetlenmeyen SQL veritabanını izleme |sqlAuditingMonitoringEffect|
|Sistem güncelleştirmeleri |Azure Güvenlik Merkezi'nde eksik sistem güncelleştirmelerini izleme |systemUpdatesMonitoringEffect|
|Depolama şifrelemesi |Depolama hesapları için eksik blob şifrelemesini denetle |storageEncryptionMonitoringEffect|
|JIT Ağ erişimi |Azure Güvenlik Merkezi'nde olası ağ erişimini tam zamanında (JIT) izleme |jitNetworkAccessMonitoringEffect |
|Uyarlamalı uygulama denetimleri |Azure Güvenlik Merkezi'nde olası uygulama Beyaz Liste'yi izleyin |adaptifApplicationControlsMonitoringEffect|
|Ağ güvenlik grupları |Azure Güvenlik Merkezi'nde izin verilen ağ erişimini izleme |ağSecurityGroupsMonitoringEffect| 
|Güvenlik yapılandırmaları |Azure Güvenlik Merkezi'ndeki işletim sistemi güvenlik açıklarını izleme |systemConfigurationsMonitoringEffect| 
|Uç nokta koruması |Azure Güvenlik Merkezi'nde eksik Uç Nokta Koruması'nı izleme |endpointProtectionMonitoringEtkisi |
|Disk şifrelemesi |Azure Güvenlik Merkezi'nde şifrelenmemiş VM Diskleri izleme |diskEncryptionMonitoringEffect|
|Güvenlik açığı değerlendirmesi |Azure Güvenlik Merkezi'ndeki VM Güvenlik Açıklarını Izleyin |güvenlik açığıAssessmentMonitoringEffect|
|Web uygulaması güvenlik duvarı |Azure Güvenlik Merkezi'nde korumasız web uygulamasını izleme |webApplicationFirewallMonitoringEtkisi |
|Yeni nesil güvenlik duvarı |Azure Güvenlik Merkezi'nde korumasız ağ uçnoktalarını izleme| |


## <a name="next-steps"></a>Sonraki adımlar

Diğer ilgili materyaller için aşağıdaki makalelere bakın: 

- [Özel güvenlik ilkeleri](custom-security-policies.md)
- [Güvenlik ilkesine genel bakış](tutorial-security-policy.md)