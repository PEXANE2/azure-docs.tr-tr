---
title: Yetenekleri kullanmak için gereken RBAC izinleri
titleSuffix: Azure Network Watcher
description: Ağ İzleyicisi özellikleriyle çalışmak için hangi Azure rol tabanlı erişim denetimi izinlerinin gerekli olduğunu öğrenin.
services: network-watcher
documentationcenter: ''
author: damendo
ms.service: network-watcher
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: damendo
ms.openlocfilehash: f8743f19d6cd262ad140659be55a4fc57e842564
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840562"
---
# <a name="role-based-access-control-permissions-required-to-use-network-watcher-capabilities"></a>Ağ İzleyicisi özelliklerini kullanmak için gereken rol tabanlı erişim denetimi izinleri

Azure rol tabanlı erişim denetimi (RBAC), kuruluşunuzun üyelerine yalnızca atanan sorumluluklarını tamamlamaları için gereken belirli eylemleri atamanızı sağlar. Ağ İzleyicisi özelliklerini kullanmak için, Azure'da oturum açtığınız hesabın [Sahibine,](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#owner) [Katılımcısına](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#contributor)veya [Ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#network-contributor) yerleşik rollerine atanması veya izleyen bölümlerdeki her Ağ İzleyicisi özelliği için listelenen eylemlere atanan özel bir [role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) atanması gerekir. Ağ İzleyicisi'nin yetenekleri hakkında daha fazla bilgi edinmek için [Bkz. Ağ İzleyicisi nedir?](network-watcher-monitoring-overview.md)

## <a name="network-watcher"></a>Ağ İzleyicisi

| Eylem                                                              | Açıklama                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/read                              | Ağ izleyicisi alma                                          |
| Microsoft.Network/networkWatchers/write                             | Ağ izleyicisi oluşturma veya güncelleştirme                             |
| Microsoft.Network/networkWatchers/delete                            | Ağ izleyicisini silme                                       |

## <a name="nsg-flow-logs"></a>NSG akış günlükleri

| Eylem                                                              | Açıklama                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/configureFlowLog/action           | Akış Günlüğü yapılandırma                                           |
| Microsoft.Network/networkWatchers/queryFlowLogStatus/action         | Akış günlüğü için sorgu durumu                                    |

## <a name="connection-troubleshoot"></a>Bağlantı sorunlarını giderme

| Eylem                                                              | Açıklama                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/connectivityCheck/action          | Bağlantı sorun giderme testi başlatma
| Microsoft.Network/networkWatchers/queryTroubleshootResult/action    | Bağlantı sorun giderme testinin sorgu sonuçları                |
| Microsoft.Network/networkWatchers/sorun giderme/eylem               | Bağlantı sorun giderme testi çalıştırma                             |

## <a name="connection-monitor"></a>Bağlantı monitörü

| Eylem                                                              | Açıklama                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/connectionMonitors/start/action   | Bağlantı monitörü başlatma                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/stop/action    | Bağlantı monitörünü durdurma                                      |
| Microsoft.Network/networkWatchers/connectionMonitors/query/action   | Bağlantı monitörünü sorgula                                     |
| Microsoft.Network/networkWatchers/connectionMonitors/read           | Bağlantı monitörü alın                                       |
| Microsoft.Network/networkWatchers/connectionMonitors/write          | Bağlantı izleyicisi oluşturma                                    |
| Microsoft.Network/networkWatchers/connectionMonitors/delete         | Bağlantı monitörünü silme                                    |

## <a name="packet-capture"></a>Paket yakalama

| Eylem                                                              | Açıklama                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Paket yakalama durumunu sorgula                           |
| Microsoft.Network/networkWatchers/packetCaptures/stop/action        | Paket yakalamayı durdurma                                          |
| Microsoft.Network/networkWatchers/packetCaptures/read               | Paket yakalama alma                                           |
| Microsoft.Network/networkWatchers/packetCaptures/write              | Paket yakalama oluşturma                                        |
| Microsoft.Network/networkWatchers/packetCaptures/delete             | Paket yakalamayı silme                                        |

## <a name="ip-flow-verify"></a>IP akışı doğrulama

| Eylem                                                              | Açıklama                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/ipFlowVerify/action               | IP akışını doğrulama                                              |

## <a name="next-hop"></a>Sonraki atlama

| Eylem                                                              | Açıklama                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/nextHop/action                    | Bir VM'den bir sonraki atlamayı alın                                     |

## <a name="network-security-group-view"></a>Ağ güvenlik grubu görünümü

| Eylem                                                              | Açıklama                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/securityGroupView/action          | Güvenlik gruplarını görüntüleme                                           |

## <a name="topology"></a>Topoloji

| Eylem                                                              | Açıklama                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/topology/action                   | Topoloji alın                                                   |

## <a name="reachability-report"></a>Ulaşılabilirlik raporu

| Eylem                                                              | Açıklama                                                           |
| ---------                                                           | -------------                                                  |
| Microsoft.Network/networkWatchers/azureReachabilityReport/action    | Azure erişim raporu alın                               |


## <a name="additional-actions"></a>Ek eylemler

Ağ İzleyici yetenekleri de aşağıdaki eylemleri gerektirir:

| Eylem(ler)                                                           | Açıklama                                                    |
| ---------                                                           | -------------                                                  |
| Microsoft.Authorization/\*/Oku                                     | RBAC rol atamaları ve ilke tanımları getirmek için kullanılır          |
| Microsoft.Resources/subscriptions/resourceGroups/Read               | Abonelikteki tüm kaynak gruplarını sayısallandırmak için kullanılır    |
| Microsoft.Storage/storageAccounts/Read                              | Belirtilen depolama hesabının özelliklerini almak için kullanılır   |
| Microsoft.Storage/storageAccounts/listServiceSas/Action, </br> Microsoft.Storage/storageAccounts/listAccountSas/Action, <br> Microsoft.Storage/storageAccounts/listKeys/Action| [Depolama hesabına güvenli erişim](https://docs.microsoft.com/azure/storage/common/storage-sas-overview) sağlayan paylaşılan erişim imzalarını (SAS) getirmek ve depolama hesabına yazmak için kullanılır |
| Microsoft.Compute/virtualMachines/Oku, </br> Microsoft.Compute/virtualMachines/Write| VM'de oturum açmak, paket yakalama yapmak ve depolama hesabına yüklemek için kullanılır|
| Microsoft.Compute/virtualMachines/extensions/Oku </br> Microsoft.Compute/virtualMachines/extensions/Write| Ağ İzleyici uzantısı nın mevcut olup olmadığını denetlemek ve gerekirse yüklemek için kullanılır |
| Microsoft.Compute/virtualMachineScaleSets/Oku, </br> Microsoft.Compute/virtualMachineScaleSets/Write| Sanal makine ölçek kümelerine erişmek, paket yakalamalar yapmak ve bunları depolama hesabına yüklemek için kullanılır|
| Microsoft.Compute/virtualMachineScaleSets/uzantılar/Oku, </br> Microsoft.Compute/virtualMachineScaleSets/extensions/Write| Ağ İzleyici uzantısı nın mevcut olup olmadığını denetlemek ve gerekirse yüklemek için kullanılır |
| Microsoft.Insights/alertRules/*                                     | Metrik uyarıları ayarlamak için kullanılır                                     |
| Microsoft.Support/*                                                 | Network Watcher'dan destek biletleri oluşturmak ve güncellemek için kullanılır |
