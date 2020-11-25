---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 07/08/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b26d4af29a92fb0f14c52e76a7eae1d0073a3aa0
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "96005356"
---
Sunucudan Azure Dosya Eşitleme hizmetine erişilemediğinde bu hata oluşabilir. Aşağıdaki adımları uygulayarak bu hataya giderebilirsiniz:

1. Windows hizmetinin `FileSyncSvc.exe` güvenlik duvarınız tarafından engellenmediğinden emin olun.
2. Bağlantı noktası 443 ' nin Azure Dosya Eşitleme hizmetine giden bağlantılara açık olduğunu doğrulayın. Bunu `Test-NetConnection` cmdlet 'i ile yapabilirsiniz. Aşağıdaki `<azure-file-sync-endpoint>` yer tutucusunun URL'si [Azure Dosya Eşitleme ara sunucu ve güvenlik duvarı ayarları](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall) belgesinde bulunabilir. 

    ```powershell
    Test-NetConnection -ComputerName <azure-file-sync-endpoint> -Port 443
    ```

3. Ara sunucu yapılandırmasının beklendiği gibi ayarlandığından emin olun. Bu, `Get-StorageSyncProxyConfiguration` cmdlet'iyle yapılabilir. Azure Dosya Eşitleme için ara sunucu yapılandırmasıyla ilgili daha fazla bilgiyi [Azure Dosya Eşitleme ara sunucu ve güvenlik duvarı ayarları](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall) belgesinde bulabilirsiniz.

    ```powershell
    $agentPath = "C:\Program Files\Azure\StorageSyncAgent"
    Import-Module "$agentPath\StorageSync.Management.ServerCmdlets.dll"
    Get-StorageSyncProxyConfiguration
    ```
4. Hizmet uç noktalarına ağ bağlantısını denetlemek için Test-StorageSyncNetworkConnectivity cmdlet 'ini kullanın. Daha fazla bilgi için bkz. [hizmet uç noktalarına ağ bağlantısını test](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#test-network-connectivity-to-service-endpoints)etme.    

5. Ağ bağlantısı sorunlarını gidermeyle ilgili ek yardım için ağ yöneticinize başvurun.