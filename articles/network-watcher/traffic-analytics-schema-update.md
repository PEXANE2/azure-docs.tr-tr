---
title: Azure Trafik Analizi şeması güncellemesi - Mart 2020 | Microsoft Dokümanlar
description: Trafik Analitiği şemasında yeni alanlar içeren örnek sorgular.
services: network-watcher
documentationcenter: na
author: vinigam
manager: agummadi
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/06/2020
ms.author: vinigam
ms.openlocfilehash: 4fe981576e3f6e58b0886d9c0d2eb2915d8b7720
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80396618"
---
# <a name="sample-queries-with-new-fields-in-the-traffic-analytics-schema-august-2019-schema-update"></a>Trafik Analitiği şemasında yeni alanlar içeren örnek sorgular (Ağustos 2019 şema güncelleştirmesi)

[Trafik Analitiği günlük şema](https://docs.microsoft.com/azure/network-watcher/traffic-analytics-schema) aşağıdaki yeni alanları içerir: **SrcPublicIPs_s**, **DestPublicIPs_s**, **NSGRule_s**. Yeni alanlar kaynak ve hedef IP'ler hakkında bilgi sağlar ve sorguları basitleştirir.

Önümüzdeki birkaç ay içinde, aşağıdaki eski alanlar amortismana sunulacak: **VMIP_s**, **Subscription_g**, **Region_s**, **NSGRules_s**, **Subnet_s**, **VM_s**, **NIC_s**, **PublicIPs_s**, **FlowCount_d**.

Aşağıdaki üç örnek, eski alanların yenileriyle nasıl değiştirilmeye kadar değiştirilebildiğini gösterir.

## <a name="example-1-vmip_s-subscription_g-region_s-subnet_s-vm_s-nic_s-and-publicips_s-fields"></a>Örnek 1: VMIP_s, Subscription_g, Region_s, Subnet_s, VM_s, NIC_s ve PublicIPs_s alanları

AzurePublic ve ExternalPublic akışları için **FlowDirection_s** alanından kaynak ve hedef servis taleplerini çıkarmak zorunda değiliz. Ayrıca bir ağ sanal cihaz için **FlowDirection_s** alanı kullanmak uygunsuz olabilir.

```Old Kusto query
AzureNetworkAnalytics_CL
| where SubType_s == "FlowLog" and FASchemaVersion_s == "1"
| extend isAzureOrExternalPublicFlows = FlowType_s in ("AzurePublic", "ExternalPublic")
| extend SourceAzureVM = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'O', VM_s, "N/A"), VM1_s),
SourceAzureVMIP = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'O', VM_s, "N/A"), SrcIP_s),
SourceAzureVMSubscription = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'O', Subscription_g, "N/A"), Subscription1_g),
SourceAzureRegion = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'O', Region_s, "N/A"), Region1_s),
SourceAzureSubnet = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'O', Subnet_s, "N/A"), Subnet1_s),
SourceAzureNIC = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'O', NIC_s, "N/A"), NIC1_s),
DestAzureVM = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'I', VM_s, "N/A"), VM2_s),
DestAzureVMIP = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'I', VM_s, "N/A"), DestIP_s),
DestAzureVMSubscription = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'I', Subscription_g, "N/A"), Subscription2_g),
DestAzureRegion = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'I', Region_s, "N/A"), Region2_s),
DestAzureSubnet = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'I', Subnet_s, "N/A"), Subnet2_s),
DestAzureNIC = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'I', NIC_s, "N/A"), NIC2_s),
SourcePublicIPsAggregated = iif(isAzureOrExternalPublicFlows and FlowDirection_s == 'I', PublicIPs_s, "N/A"),
DestPublicIPsAggregated = iif(isAzureOrExternalPublicFlows and FlowDirection_s == 'O', PublicIPs_s, "N/A")
```


```New Kusto query
AzureNetworkAnalytics_CL
| where SubType_s == "FlowLog" and FASchemaVersion_s == "2"
| extend SourceAzureVM = iif(isnotempty(VM1_s), VM1_s, "N/A"),
SourceAzureVMIP = iif(isnotempty(SrcIP_s), SrcIP_s, "N/A"),
SourceAzureVMSubscription = iif(isnotempty(Subscription1_g), Subscription1_g, "N/A"),
SourceAzureRegion = iif(isnotempty(Region1_s), Region1_s, "N/A"),
SourceAzureSubnet = iif(isnotempty(Subnet1_s), Subnet1_s, "N/A"),
SourceAzureNIC = iif(isnotempty(NIC1_s), NIC1_s, "N/A"),
DestAzureVM = iif(isnotempty(VM2_s), VM2_s, "N/A"),
DestAzureVMIP = iif(isnotempty(DestIP_s), DestIP_s, "N/A"),
DestAzureVMSubscription = iif(isnotempty(Subscription2_g), Subscription2_g, "N/A"),
DestAzureRegion = iif(isnotempty(Region2_s), Region2_s, "N/A"),
DestAzureSubnet = iif(isnotempty(Subnet2_s), Subnet2_s, "N/A"),
DestAzureNIC = iif(isnotempty(NIC2_s), NIC2_s, "N/A"),
SourcePublicIPsAggregated = iif(isnotempty(SrcPublicIPs_s), SrcPublicIPs_s, "N/A"),
DestPublicIPsAggregated = iif(isnotempty(DestPublicIPs_s), DestPublicIPs_s, "N/A")
```

## <a name="example-2-nsgrules_s-field"></a>Örnek 2: NSGRules_s alanı

Eski alan biçimini kullandı:

<Endeksi değeri 0)>|<NSG_ RuleName>|<Flow Direction>|<Flow Status>|<FlowCount ProcessedByRule>

Artık bir ağ güvenlik grubu (NSG) üzerinden veri toplamıyoruz. Güncelleştirilmiş şemada, **NSGList_s** yalnızca bir NSG içerir. Ayrıca **NSGRules** yalnızca bir kural içerir. Burada ve örnekte gösterildiği gibi diğer alanlarda karmaşık biçimlendirme kaldırıldı.

```Old Kusto query
AzureNetworkAnalytics_CL
| where SubType_s == "FlowLog" and FASchemaVersion_s == "1"
| extend NSGRuleComponents = split(NSGRules_s, "|")
| extend NSGName = NSGList_s // remains same
| extend NSGRuleName = NSGRuleComponents[1],
         FlowDirection = NSGRuleComponents[2],
         FlowStatus = NSGRuleComponents[3],
         FlowCountProcessedByRule = NSGRuleComponents[4]
| project NSGName, NSGRuleName, FlowDirection, FlowStatus, FlowCountProcessedByRule
```

```New Kusto query
AzureNetworkAnalytics_CL
| where SubType_s == "FlowLog" and FASchemaVersion_s == "2"
| extend NSGRuleComponents = split(NSGRules_s, "|")
| project NSGName = NSGList_s,
NSGRuleName = NSGRule_s ,
FlowDirection = FlowDirection_s,
FlowStatus = FlowStatus_s,
FlowCountProcessedByRule = AllowedInFlows_d + DeniedInFlows_d + AllowedOutFlows_d + DeniedOutFlows_d
```

## <a name="example-3-flowcount_d-field"></a>Örnek 3: FlowCount_d alanı

NSG'de veri gütmediğimiz **için, FlowCount_d** basitçe şöyledir:

**AllowedInFlows_d** + **DeniedInFlows_d** + **AllowedOutFlows_d** + **DeniedOutFlows_d**

Dört alandan yalnızca biri sıfırdan olacaktır. Diğer üç alan sıfır olacak. Alanlar durumu belirtmek için doldurulur ve akış yakalanan NIC sayısı.

Bu koşulları göstermek için:

- Akışa izin verilirse, "İzin Verilen" önceden belirlenmiş alanlardan biri doldurulur.
- Akış reddedilirse, "Reddedildi" önceden belirlenmiş alanlardan biri doldurulur.
- Akış gelen olsaydı, "InFlows_d" suffixed alanlardan biri doldurulur.
- Akış giden ise, "OutFlows_d" suffixed alanlardan biri doldurulur.

Koşullara bağlı olarak, dört alandan hangisinin doldurulacağını biliyoruz.

## <a name="next-steps"></a>Sonraki adımlar

- Sık sorulan soruların yanıtlarını almak için [Trafik Analizi SSS](traffic-analytics-faq.md)bölümüne bakın.
- İşlevsellikle ilgili ayrıntıları görmek için [Trafik Analizi belgelerine](traffic-analytics.md)bakın.
