---
title: Sanal ağınızdaki Azure Veri Gezgini kümenize erişim, alma ve çalışma sorunu giderme
description: Azure Veri Gezgini kümenizin sanal ağınızdaki bağlantı, yutma, küme oluşturma ve çalışma yla ilgili sorun giderme
author: basaba
ms.author: basaba
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/24/2020
ms.openlocfilehash: b50b971a3b1980ad35a1a939bdf25f1c9e6ac7ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241666"
---
# <a name="troubleshoot-access-ingestion-and-operation-of-your-azure-data-explorer-cluster-in-your-virtual-network"></a>Sanal ağınızdaki Azure Veri Gezgini kümenize erişim, alma ve çalışma sorunu giderme

Bu bölümde, [Sanal Ağınıza](/azure/virtual-network/virtual-networks-overview)dağıtılan bir küme için bağlantı, operasyonel ve küme oluşturma sorunlarını nasıl gidereceklerini öğreniyorsunuz.

## <a name="access-issues"></a>Erişim sorunları

Genel (cluster.region.kusto.windows.net) veya özel (private-cluster.region.kusto.windows.net) bitiş noktasını kullanarak kümeye erişirken bir sorununuz varsa ve bunun sanal ağ kurulumuyla ilgili olduğundan şüpheleniyorsanız, aşağıdaki adımları sorunu giderin.

### <a name="check-tcp-connectivity"></a>TCP bağlantısını denetleyin

İlk adım, Windows veya Linux OS kullanarak TCP bağlantısı nın denetlenen bir şekilde denetlenen'i içerir.

# <a name="windows"></a>[Windows](#tab/windows)

   1. Kümeye bağlanan makineye [TCping'i](https://www.elifulkerson.com/projects/tcping.php) indirin.
   2. Aşağıdaki komutu kullanarak hedef kaynak makineden ping:

    ```cmd
     C:\> tcping -t yourcluster.kusto.windows.net 443 
    
     ** Pinging continuously.  Press control-c to stop **
    
     Probing 1.2.3.4:443/tcp - Port is open - time=100.00ms
     ```

# <a name="linux"></a>[Linux](#tab/linux)

   1. Kümeye bağlanan makineye *netcat* yükleme

    ```bash
    $ apt-get install netcat
     ```

   2. Aşağıdaki komutu kullanarak hedef kaynak makineden ping:

     ```bash
     $ netcat -z -v yourcluster.kusto.windows.net 443
    
     Connection to yourcluster.kusto.windows.net 443 port [tcp/https] succeeded!
     ```
---

Test başarılı olmazsa, aşağıdaki adımları ile devam edin. Sınama başarılı olursa, sorun bir TCP bağlantısı sorunu nedeniyle değil. Daha fazla sorun gidermek için [operasyonel sorunlara](#cluster-creation-and-operations-issues) gidin.

### <a name="check-the-network-security-group-nsg"></a>Ağ Güvenlik Grubunu (NSG) kontrol edin

   Kümenin alt ağına bağlı [Ağ Güvenlik Grubu'nun](/azure/virtual-network/security-overview) (NSG) istemci makinenin IP'sinden bağlantı noktası 443'e erişime izin veren bir gelen kuralı olup olmadığını kontrol edin.

### <a name="check-route-table"></a>Rota tablosunu denetle

   Kümenin alt ağında güvenlik duvarına kuvvet tüneli kurulumu varsa (varsayılan rota '0.0.0.0/0' içeren [bir rota tablosuna](/azure/virtual-network/virtual-networks-udr-overview) sahip alt ağ), makine IP adresinin VirtualNetwork/Internet'e [sonraki atlama türüne](/azure/virtual-network/virtual-networks-udr-overview) sahip bir rotaya sahip olduğundan emin olun. Bu yol asimetrik rota sorunlarını önlemek için gereklidir.

## <a name="ingestion-issues"></a>Yutma sorunları

Yutma sorunları yaşıyorsanız ve bunun sanal ağ kurulumuyla ilgili olduğundan şüpheleniyorsanız, aşağıdaki adımları gerçekleştirin.

### <a name="check-ingestion-health"></a>Yutma durumunu kontrol edin

[Küme leme ölçümlerinin](/azure/data-explorer/using-metrics#ingestion-health-and-performance-metrics) sağlıklı bir durum gösterdiğinden denetleyin.

### <a name="check-security-rules-on-data-source-resources"></a>Veri kaynağı kaynaklarındaki güvenlik kurallarını denetleme

Ölçümler veri kaynağından (Olay/IoT Hub'ları için*işlenen olaylar* ölçümü) hiçbir olayın işlenmediğini gösteriyorsa, veri kaynağı kaynaklarının (Olay Hub'ı veya Depolama) güvenlik duvarı kuralları veya hizmet bitiş noktalarında kümenin alt ağına erişime izin verdiğinden emin olun.

### <a name="check-security-rules-configured-on-clusters-subnet"></a>Kümenin alt ağına yapılandırılan güvenlik kurallarını denetleme

Kümenin alt ağına NSG, UDR ve güvenlik duvarı kurallarının düzgün şekilde yapılandırıldığından emin olun. Ayrıca, tüm bağımlı uç noktalar için ağ bağlantısını test edin. 

## <a name="cluster-creation-and-operations-issues"></a>Küme oluşturma ve işlem sorunları

Küme oluşturma veya işlem sorunları yaşıyorsanız ve sanal ağ kurulumuyla ilgili olduğundan şüpheleniyorsanız, sorunu gidermek için aşağıdaki adımları izleyin.

### <a name="diagnose-the-virtual-network-with-the-rest-api"></a>Sanal ağı REST API ile tanılama

[ARMClient](https://chocolatey.org/packages/ARMClient) PowerShell kullanarak REST API aramak için kullanılır. 

1. ARMClient ile giriş yapın

   ```powerShell
   armclient login
   ```

1. Tanılama işlemini başlatın

    ```powershell
    $subscriptionId = '<subscription id>'
    $clusterName = '<name of cluster>'
    $resourceGroupName = '<resource group name>'
    $apiversion = '2019-11-09'
    
    armclient post "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Kusto/clusters/$clusterName/diagnoseVirtualNetwork?api-version=$apiversion" -verbose
    ```

1. Yanıtı kontrol edin

    ```powershell
    HTTP/1.1 202 Accepted
    ...
    Azure-AsyncOperation: https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Kusto/locations/{location}/operationResults/{operation-id}?api-version=2019-11-09
    ...
    ```

1. Operasyonun tamamlanmasını bekleyin

    ```powershell
    armclient get https://management.azure.com/subscriptions/$subscriptionId/providers/Microsoft.Kusto/locations/{location}/operationResults/{operation-id}?api-version=2019-11-09
    
    {
      "id": "/subscriptions/{subscription-id}/providers/Microsoft.Kusto/locations/{location}/operationresults/{operation-id}",
      "name": "{operation-name}",
      "status": "[Running/Failed/Completed]",
      "startTime": "{start-time}",
      "endTime": "{end-time}",
      "properties": {...}
    }
    ```
    
   *Durum* özelliği *Tamamlananı*gösterene kadar bekleyin, özellikler *alanı* gösterilmelidir:

    ```powershell
    {
      "id": "/subscriptions/{subscription-id}/providers/Microsoft.Kusto/locations/{location}/operationresults/{operation-id}",
      "name": "{operation-name}",
      "status": "Completed",
      "startTime": "{start-time}",
      "endTime": "{end-time}",
      "properties": {
        "Findings": [...]
      }
    }
    ```

*Bulgular* özelliği boş bir sonuç gösteriyorsa, bu tüm ağ testlerinin geçtiği ve bağlantı nın kopmama anlamına gelir. Aşağıdaki hata gösterilirse, *Giden bağımlılık '{dependencyName}:{port}' tatmin olmayabilir (Giden)* küme bağımlı hizmet bitiş noktalarına ulaşamaz. Aşağıdaki adımları ile devam edin.

### <a name="check-network-security-group-nsg"></a>Ağ Güvenlik Grubunu Denetle (NSG)

Ağ Güvenlik [Grubu'nun](/azure/virtual-network/security-overview) [VNet dağıtımı için Bağımlılıklar](/azure/data-explorer/vnet-deployment#dependencies-for-vnet-deployment) yönergelerine göre doğru şekilde yapılandırıldığından emin olun

### <a name="check-route-table"></a>Rota tablosunu denetle

Kümenin alt netinde güvenlik duvarına (varsayılan rota '0.0.0.0/0' içeren [bir rota tablosuna](/azure/virtual-network/virtual-networks-udr-overview) sahip alt ağ) kuvvet tüneli kurulumu varsa, [yönetim IP adreslerinin](vnet-deployment.md#azure-data-explorer-management-ip-addresses)ve [sistem durumu izleme IP adreslerinin](vnet-deployment.md#health-monitoring-addresses) [sonraki atlama türü](/azure/virtual-network/virtual-networks-udr-overview##next-hop-types-across-azure-tools) *Internet'e*ve kaynak [adresi önekinin](/azure/virtual-network/virtual-networks-udr-overview#how-azure-selects-a-route) *'management-ip/32'* ve *'health-monitoring-ip/32'* ile bir rotaya sahip olduğundan emin olun. Bu rota asimetrik rota sorunlarını önlemek için gereklidir.

### <a name="check-firewall-rules"></a>Güvenlik duvarı kurallarını denetle

Tünel alt ağı giden trafiği bir güvenlik duvarına zorlarsanız, güvenlik duvarı [yla giden trafiği güvence altına almak](/azure/data-explorer/vnet-deployment#securing-outbound-traffic-with-firewall)ta açıklandığı gibi tüm bağımlılıklara (örneğin, *.blob.core.windows.net)* izin verildiğinden emin olun.
