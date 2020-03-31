---
title: Azure Ağ İzleyicisinde Paket yakalamaya giriş | Microsoft Dokümanlar
description: Bu sayfa, Ağ İzleyicipaket yakalama özelliğine genel bir bakış sağlar
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
ms.openlocfilehash: 1f17463125cead64bd58a2d07e53eee4d3cfcd70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840817"
---
# <a name="introduction-to-variable-packet-capture-in-azure-network-watcher"></a>Azure Ağ İzleyicisi'nde değişken paket yakalamaya giriş

Network Watcher değişken paket yakalama ve sanal bir makineden gelen trafiği izlemek için paket yakalama oturumları oluşturmanıza olanak sağlar. Paket yakalama, ağ anomalilerinin hem reaktif hem de proaktif olarak tanılenmesine yardımcı olur. Diğer kullanım alanları arasında ağ istatistiklerinin toplanması, ağ ihlalleri hakkında bilgi edinme, istemci-sunucu iletişimini hata ayıklama ve çok daha fazlası yer almaktadır.

Paket yakalama, Network Watcher aracılığıyla uzaktan başlatılan sanal bir makine uzantısıdır. Bu özellik, paket yakalamayı istenilen sanal makinede el ile çalıştırma nın yükünü hafifletir ve bu da değerli zamandan tasarruf sağlar. Paket yakalama portalı, PowerShell, CLI veya REST API üzerinden tetiklenebilir. Paket yakalamanın nasıl tetiklenebildiğinin bir örneği Sanal Makine uyarılarıdır. İzlemek istediğiniz trafiği yakalamanızı sağlamak için yakalama oturumu için filtreler sağlanır. Filtreler 5-tuple (protokol, yerel IP adresi, uzak IP adresi, yerel bağlantı noktası ve uzak bağlantı noktası) bilgilerine dayanır. Yakalanan veriler yerel diskte veya depolama blob'unda depolanır. Abonelik başına bölge başına 10 paket yakalama oturumu sınırı vardır. Bu sınır yalnızca oturumlar için geçerlidir ve VM'de veya depolama hesabında yerel olarak kaydedilen paket yakalama dosyaları için geçerli değildir.

> [!IMPORTANT]
> Paket yakalama sanal bir `AzureNetworkWatcherExtension`makine uzantısı gerektirir. Uzantıyı Windows VM'de yüklemek [için Windows için Azure Network Watcher Agent sanal makine uzantısını](../virtual-machines/windows/extensions-nwa.md) ziyaret edin ve Linux VM için Linux için Azure Network [Watcher Agent sanal makine uzantısını](../virtual-machines/linux/extensions-nwa.md)ziyaret edin.

Yakaladığınız bilgileri yalnızca istediğiniz bilgilere düşürmek için, paket yakalama oturumu için aşağıdaki seçenekler kullanılabilir:

**Yapılandırmayı yakalama**

|Özellik|Açıklama|
|---|---|
|**Paket başına maksimum bayt (bayt)** | Yakalanan her paketteki bayt sayısı boş bırakılırsa tüm baytlar yakalanır. Yakalanan her paketteki bayt sayısı boş bırakılırsa tüm baytlar yakalanır. Yalnızca IPv4 üstbilginiz gerekiyorsa – burada 34'u belirtin |
|**Oturum başına maksimum bayt (bayt)** | Değere ulaşıldıktan sonra yakalanan toplam bayt sayısı sona erer.|
|**Zaman sınırı (saniye)** | Paket yakalama oturumunda bir zaman kısıtlaması ayarlar. Varsayılan değer 18000 saniye veya 5 saattir.|

**Filtreleme (isteğe bağlı)**

|Özellik|Açıklama|
|---|---|
|**Protokolü** | Paket yakalama için filtreleme protokolü. Kullanılabilir değerler TCP, UDP ve Tümü'tür.|
|**Yerel IP adresi** | Bu değer, paket yakalamayı yerel IP adresinin bu filtre değeriyle eşleştiği paketlere filtreler.|
|**Yerel bağlantı noktası** | Bu değer, paket yakalamayı yerel bağlantı noktasının bu filtre değeriyle eşleştiği paketlere filtreler.|
|**Uzak IP adresi** | Bu değer, paket yakalamayı uzak IP'nin bu filtre değeriyle eşleştiği paketlere filtreler.|
|**Uzak bağlantı noktası** | Bu değer, paket yakalamayı uzak bağlantı noktasının bu filtre değeriyle eşleştiği paketlere filtreler.|

### <a name="next-steps"></a>Sonraki adımlar

[Azure portalında paket yakalamayı yönet'i](network-watcher-packet-capture-manage-portal.md) veya [PowerShell ile PowerShell'i yönet'i](network-watcher-packet-capture-manage-powershell.md)ziyaret ederek paket yakalamaları portal üzerinden nasıl yönetebileceğinizi öğrenin.

[Uyarı tetiklenen paket yakalama](network-watcher-alert-triggered-packet-capture.md) yı ziyaret ederek sanal makine uyarılarına dayalı proaktif paket yakalamaları oluşturmayı öğrenin

<!--Image references-->
[1]: ./media/network-watcher-packet-capture-overview/figure1.png













