---
title: NSG denetimini otomatikleştirin - Güvenlik grubu görünümü
titleSuffix: Azure Network Watcher
description: Bu sayfa, bir Ağ Güvenlik Grubu denetiminin nasıl yapılandırılabildiğini anlatan yönergeler sağlar
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: 59c1b6e6c281a736a79d110bd7d943344bcd5130
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840987"
---
# <a name="automate-nsg-auditing-with-azure-network-watcher-security-group-view"></a>Azure Ağ İzleyicisi Güvenliği grup görünümüyle NSG denetimini otomatikleştirin

Müşteriler genellikle altyapılarının güvenlik duruşunu doğrulama zorluğuyla karşı karşıyadır. Bu zorluk, Azure'daki VM'leri için de farklı değildir. Ağ Güvenlik Grubu (NSG) kurallarına göre benzer bir güvenlik profiline sahip olmak önemlidir. Güvenlik Grubu Görünümü'ni kullanarak artık Bir NSG içindeki Bir VM'ye uygulanan kuralların listesini alabilirsiniz. Altın bir NSG güvenlik profili tanımlayabilir ve haftalık bir şekilde Güvenlik Grubu Görünümü'ni başlatabilir ve çıktıyı altın profille karşılaştırıp bir rapor oluşturabilirsiniz. Bu şekilde, öngörülen güvenlik profiline uymayan tüm VM'leri kolaylıkla tanımlayabilirsiniz.

Ağ Güvenlik Grupları'nı bilmiyorsanız, [Ağ Güvenliğine Genel Bakış'a](../virtual-network/security-overview.md)bakın.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryoda, bilinen iyi bir taban çizgisini sanal bir makine için döndürülen güvenlik grubu görünümü sonuçlarıyla karşılaştırırsınız.

Bu senaryo, Ağ İzleyicisi oluşturmak için [Ağ İzleyicisi Oluştur'daki](network-watcher-create.md) adımları zaten izlediğinizi varsayar. Senaryo da geçerli bir sanal makine ile bir Kaynak Grubu kullanılmak üzere var varsayar.

## <a name="scenario"></a>Senaryo

Bu makalede kapsanan senaryo, sanal bir makine için güvenlik grubu görünümünü alır.

Bu senaryoda şunları yapacaksınız:

- Bilinen iyi bir kural kümesini alma
- Rest API ile sanal bir makine alma
- Sanal makine için güvenlik grubu görünümü alın
- Yanıtı Değerlendir

## <a name="retrieve-rule-set"></a>Kural kümesini al

Bu örnekteki ilk adım, varolan bir taban çizgisiyle çalışmaktır. Aşağıdaki örnek, bu örnek için temel olarak kullanılan `Get-AzNetworkSecurityGroup` cmdlet kullanılarak varolan bir Ağ Güvenlik Grubu'ndan çıkarılan bazı json'dur.

```json
[
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "3389",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1000,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "default-allow-rdp",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/default-allow-rdp"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "111",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1010,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "MyRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/MyRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "112",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1020,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "My2ndRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/My2ndRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "5672",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Deny",
        "Priority":  1030,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "ThisRuleNeedsToStay",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/ThisRuleNeedsToStay"
    }
]
```

## <a name="convert-rule-set-to-powershell-objects"></a>Kural kümesini PowerShell nesnelerine dönüştürme

Bu adımda, bu örnek için Ağ Güvenlik Grubu'nda olması beklenen kurallarla daha önce oluşturulmuş bir json dosyasını okuyoruz.

```powershell
$nsgbaserules = Get-Content -Path C:\temp\testvm1-nsg.json | ConvertFrom-Json
```

## <a name="retrieve-network-watcher"></a>Ağ İzleyicisini Al

Bir sonraki adım, Ağ İzleyicisi örneğini almaktır. `$networkWatcher` Değişken cmdlet'e `AzNetworkWatcherSecurityGroupView` geçirilir.

```powershell
$networkWatcher = Get-AzResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
```

## <a name="get-a-vm"></a>VM alın

Sanal bir makine karşı `Get-AzNetworkWatcherSecurityGroupView` cmdlet çalıştırmak için gereklidir. Aşağıdaki örnekte bir VM nesnesi alır.

```powershell
$VM = Get-AzVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="retrieve-security-group-view"></a>Güvenlik grubu görünümünü alma

Bir sonraki adım, güvenlik grubu görünümü sonucunu almaktır. Bu sonuç, daha önce gösterilen "taban çizgisi" json ile karşılaştırılır.

```powershell
$secgroup = Get-AzNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="analyzing-the-results"></a>Sonuçları analiz etme

Yanıt, Ağ arabirimleri tarafından gruplandırılır. Döndürülen farklı türde kurallar etkili ve varsayılan güvenlik kurallarıdır. Sonuç, bir alt ağ veya sanal NIC üzerinde nasıl uygulandığına göre daha da bölünur.

Aşağıdaki PowerShell komut dosyası, Güvenlik Grubu Görünümü'nün sonuçlarını bir NSG'nin varolan çıktısı ile karşılaştırır. Aşağıdaki örnek, sonuçların cmdlet ile `Compare-Object` nasıl karşılaştırılabildiğini basit bir örnektir.

```powershell
Compare-Object -ReferenceObject $nsgbaserules `
-DifferenceObject $secgroup.NetworkInterfaces[0].NetworkInterfaceSecurityRules `
-Property Name,Description,Protocol,SourcePortRange,DestinationPortRange,SourceAddressPrefix,DestinationAddressPrefix,Access,Priority,Direction
```

Aşağıdaki örnek sonucudur. Karşılaştırmada ilk kural kümesinde yer alan iki kuralın bulunmadığını görebilirsiniz.

```
Name                     : My2ndRuleDoNotDelete
Description              : 
Protocol                 : *
SourcePortRange          : *
DestinationPortRange     : 112
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Allow
Priority                 : 1020
Direction                : Inbound
SideIndicator            : <=

Name                     : ThisRuleNeedsToStay
Description              : 
Protocol                 : TCP
SourcePortRange          : *
DestinationPortRange     : 5672
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Deny
Priority                 : 1030
Direction                : Inbound
SideIndicator            : <=
```

## <a name="next-steps"></a>Sonraki adımlar

Ayarlar değiştirildiyse, ağ güvenlik grubunu ve söz konusu güvenlik kurallarını izlemek için [Ağ Güvenlik Gruplarını Yönet'e](../virtual-network/manage-network-security-group.md) bakın.













