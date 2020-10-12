---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 07/08/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b2ff542d2782293e89b66e5d25cb67a9bcde6da8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "75772840"
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
4. Hizmet uç noktalarına ağ bağlantısını denetlemek için Test-StorageSyncNetworkConnectivity cmdlet 'ini kullanın. Daha fazla bilgi için bkz. [hizmet uç noktalarına ağ bağlantısını test](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy#test-network-connectivity-to-service-endpoints)etme.    

5. Ağ bağlantısı sorunlarını gidermeyle ilgili ek yardım için ağ yöneticinize başvurun.
