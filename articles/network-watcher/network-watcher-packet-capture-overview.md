---
title: Azure ağ Izleyicisi 'nde paket yakalamaya giriş | Microsoft Docs
description: Bu sayfa, ağ Izleyicisi paket yakalama özelliğine genel bakış sağlar
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76840817"
---
# <a name="introduction-to-variable-packet-capture-in-azure-network-watcher"></a>Azure ağ Izleyicisi 'nde değişken paketi yakalamaya giriş

Ağ Izleyicisi değişken paket yakalama, bir sanal makineden gelen ve giden trafiği izlemek için paket yakalama oturumları oluşturmanıza olanak sağlar. Paket yakalama, ağ anomali ve proaktif olarak tanılamaya yardımcı olur. Diğer kullanımlar, istemci-sunucu iletişimlerinde hata ayıklamak ve çok daha fazlasını yapmak için ağ istatistiklerini toplamayı, ağ izinsiz kullanım hakkında bilgi kazanmanızı içerir.

Paket yakalama, ağ Izleyicisi aracılığıyla uzaktan başlatılan bir sanal makine uzantısıdır. Bu özellik, bir paket yakalamanın, istenen sanal makinede el ile çalıştırılması yükünü kolaylaştırır ve bu da değerli süreyi kaydeder. Paket yakalama, Portal, PowerShell, CLı veya REST API aracılığıyla tetiklenebilir. Paket yakalamanın tetiklenebilecek bir örnek sanal makine uyarılarıyla birlikte. İzlemek istediğiniz trafiği yakalamanızı sağlamak için yakalama oturumu için filtreler sağlanır. Filtreler, 5 demet (protokol, yerel IP adresi, uzak IP adresi, yerel bağlantı noktası ve uzak bağlantı noktası) bilgilerini temel alır. Yakalanan veriler yerel diskte veya bir Depolama Blobu 'nda depolanır. Her abonelik için bölge başına 10 paket yakalama oturumu sınırı vardır. Bu sınır yalnızca oturumlar için geçerlidir ve yerel olarak VM 'de veya bir depolama hesabında kayıtlı paket yakalama dosyaları için geçerli değildir.

> [!IMPORTANT]
> Paket yakalama, bir sanal makine uzantısı `AzureNetworkWatcherExtension`gerektirir. Windows VM 'ye uzantı yüklemek için bkz. [Windows Için Azure ağ Izleyicisi Aracısı sanal makine uzantısı](../virtual-machines/windows/extensions-nwa.md) ve Linux VM Için [Azure Ağ İzleyicisi Aracısı sanal makine uzantısı](../virtual-machines/linux/extensions-nwa.md)' nı ziyaret edin.

Yakaladığınız bilgileri yalnızca istediğiniz bilgilere düşürmek için, bir paket yakalama oturumunda aşağıdaki seçenekler kullanılabilir:

**Yakalama yapılandırması**

|Özellik|Açıklama|
|---|---|
|**Paket başına en fazla bayt (bayt)** | Her bir paketteki yakalanan bayt sayısı, boş bırakılırsa tüm baytlar yakalanır. Her bir paketteki yakalanan bayt sayısı, boş bırakılırsa tüm baytlar yakalanır. Yalnızca IPv4 üst bilgisine ihtiyacınız varsa – burada 34 belirtin |
|**Oturum başına en fazla bayt (bayt)** | Değer, oturum sona erdikten sonra yakalanan toplam bayt sayısı.|
|**Süre sınırı (saniye)** | Paket yakalama oturumunda bir saat kısıtlaması ayarlar. Varsayılan değer 18000 saniye veya 5 saattir.|

**Filtreleme (isteğe bağlı)**

|Özellik|Açıklama|
|---|---|
|**Protocol** | Paket yakalama için filtrelenecek protokol. Kullanılabilir değerler TCP, UDP ve hepsi.|
|**Yerel IP adresi** | Bu değer, paket yakalamayı, yerel IP adresinin bu filtre değeriyle eşleştiği paketlere göre filtreler.|
|**Yerel bağlantı noktası** | Bu değer, paket yakalamayı yerel bağlantı noktasının bu filtre değeriyle eşleştiği paketlere göre filtreler.|
|**Uzak IP adresi** | Bu değer, paket yakalamayı, uzak IP 'nin bu filtre değeriyle eşleştiği paketlere göre filtreler.|
|**Uzak bağlantı noktası** | Bu değer, paket yakalamayı uzak bağlantı noktasının bu filtre değeriyle eşleştiği paketlere göre filtreler.|

### <a name="next-steps"></a>Sonraki adımlar

Paket yakalamalarını [PowerShell Ile Yönet](network-watcher-packet-capture-manage-powershell.md)' i ziyaret ederek, Azure Portal veya PowerShell ile PowerShell ile paket [yakalamayı](network-watcher-packet-capture-manage-portal.md) yönetme hakkında bilgi edinin.

[Bir uyarı ile tetiklenen paket yakalama oluşturma](network-watcher-alert-triggered-packet-capture.md) ile sanal makine uyarılarını temel alan öngörülü paket yakalamaları oluşturmayı öğrenin

<!--Image references-->
[1]: ./media/network-watcher-packet-capture-overview/figure1.png













