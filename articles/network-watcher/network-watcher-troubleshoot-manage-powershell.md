---
title: Azure VNet ağ geçidi ve bağlantılarını giderin - Azure PowerShell
titleSuffix: Azure Network Watcher
description: Bu sayfa, Azure Ağ İzleyicisi sorun giderme PowerShell cmdlet nasıl kullanılacağını açıklar
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: damendo
ms.openlocfilehash: 0723ddc9b0e2f15d5c8e51c96d51f58f1313493a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78673662"
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a>Azure Ağ İzleyicisi PowerShell kullanarak Sanal Ağ Ağ Ağ Geçidi ve Bağlantıları Sorun Giderme

> [!div class="op_single_selector"]
> - [Portal](diagnose-communication-problem-between-networks.md)
> - [Powershell](network-watcher-troubleshoot-manage-powershell.md)
> - [Azure CLI](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Ağ İzleyicisi, Azure'daki ağ kaynaklarınızı anlamayla ilgili olarak birçok özellik sağlar. Bu özelliklerden biri kaynak sorun gidermedir. Kaynak sorun giderme portalı, PowerShell, CLI veya REST API üzerinden çağrılabilir. Ağ İzleyicisi çağrıldığında, Sanal Ağ Ağ Ağ Geçidi'nin veya Bağlantının sistem durumunu inceler ve bulgularını döndürür.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryo, Ağ İzleyicisi oluşturmak için [Ağ İzleyicisi Oluştur'daki](network-watcher-create.md) adımları zaten izlediğinizi varsayar.

Desteklenen ağ geçidi türlerinin listesi ziyaret, [Desteklenen Ağ Geçidi türleri.](network-watcher-troubleshoot-overview.md#supported-gateway-types)

## <a name="overview"></a>Genel Bakış

Kaynak sorun giderme, Sanal Ağ Ağ Ağ Geçitleri ve Bağlantıları ile ortaya çıkan sorun giderme olanağı sağlar. Kaynak sorun giderme için bir istek yapıldığında, günlükler sorgulanır ve denetlenir. Denetim tamamlandığında, sonuçlar döndürülür. Kaynak sorun giderme istekleri, sonucu döndürmek için birden çok dakika sürebilir uzun çalışan istekler vardır. Sorun giderme günlükleri belirtilen bir depolama hesabında bir kapsayıcıda depolanır.

## <a name="retrieve-network-watcher"></a>Ağ İzleyicisini Al

İlk adım, Ağ İzleyicisi örneğini almaktır. Değişken `$networkWatcher` adım 4'te `Start-AzNetworkWatcherResourceTroubleshooting` cmdlet'e geçirilir.

```powershell
$networkWatcher = Get-AzNetworkWatcher -Location "WestCentralUS" 
```

## <a name="retrieve-a-virtual-network-gateway-connection"></a>Sanal Ağ Ağ Geçidi Bağlantısı Alma

Bu örnekte, bağlantıda kaynak sorun giderme çalıştırılıyor. Ayrıca bir Sanal Ağ Ağ Ağ Geçidi geçirebilirsiniz.

```powershell
$connection = Get-AzVirtualNetworkGatewayConnection -Name "2to3" -ResourceGroupName "testrg"
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Kaynak sorun giderme, kaynağın durumu yla ilgili verileri döndürür, ayrıca günlükleri gözden geçirilecek bir depolama hesabına kaydeder. Bu adımda, bir depolama hesabı oluştururuz, varolan bir depolama hesabı varsa, bunu kullanabilirsiniz.

```powershell
$sa = New-AzStorageAccount -Name "contosoexamplesa" -SKU "Standard_LRS" -ResourceGroupName "testrg" -Location "WestCentralUS"
Set-AzCurrentStorageAccount -ResourceGroupName $sa.ResourceGroupName -Name $sa.StorageAccountName
$sc = New-AzStorageContainer -Name logs
```

## <a name="run-network-watcher-resource-troubleshooting"></a>Ağ İzleyicikaynak sorun giderme çalıştırın

Cmdlet ile `Start-AzNetworkWatcherResourceTroubleshooting` kaynakları giderme. Cmdlet'i Network Watcher nesnesini, Bağlantı Kimliği'ni veya Sanal Ağ Ağ Ağ Geçidi'ni, depolama hesap kimliğini ve sonuçları depolama yolunu geçiyoruz.

> [!NOTE]
> Cmdlet `Start-AzNetworkWatcherResourceTroubleshooting` uzun çalışıyor ve tamamlanması birkaç dakika sürebilir.

```powershell
Start-AzNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath "$($sa.PrimaryEndpoints.Blob)$($sc.name)"
```

Cmdlet'i çalıştırdıktan sonra, Ağ İzleyicisi durumu doğrulamak için kaynağı gözden geçirir. Sonuçları kabuka döndürür ve sonuçların günlüklerini belirtilen depolama hesabında saklar.

## <a name="understanding-the-results"></a>Sonuçları anlama

Eylem metni, sorunun nasıl çözüleceği konusunda genel yönerge ler sağlar. Sorun için bir eylem yapılabilirse, ek yönergeyle birlikte bir bağlantı sağlanır. Ek bir kılavuz bulunmadığı durumlarda, yanıt bir destek örneği açmak için url sağlar.  Yanıtın özellikleri ve nelerin dahil olduğu hakkında daha fazla bilgi için [Network Watcher Sorun Giderme Sorunu Özeti'ne genel bakışı](network-watcher-troubleshoot-overview.md) ziyaret edin

Azure depolama hesaplarından dosya indirme yle ilgili talimatlar için [,.NET'i kullanarak Azure Blob depolama alanına başlayın'a](../storage/blobs/storage-dotnet-how-to-use-blobs.md)bakın. Kullanılabilecek bir diğer araç da Depolama Gezgini'dir. Depolama Gezgini hakkında daha fazla bilgiyi aşağıdaki bağlantıda bulabilirsiniz: [Depolama Gezgini](https://storageexplorer.com/)

## <a name="next-steps"></a>Sonraki adımlar

VPN bağlantısını durduran ayarlar değiştirildiyse, ağ güvenlik grubunu ve söz konusu olabilecek güvenlik kurallarını izlemek için [Ağ Güvenlik Gruplarını Yönet'e](../virtual-network/manage-network-security-group.md) bakın.
