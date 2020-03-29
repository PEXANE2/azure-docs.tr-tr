---
title: HB serisi ve HC serisi Sanal Makinelerile bilinen sorunlar - Azure Sanal Makineler | Microsoft Dokümanlar
description: Azure'da HB serisi VM boyutlarıyla ilgili bilinen sorunlar hakkında bilgi edinin.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/07/2019
ms.author: amverma
ms.openlocfilehash: 8d4b57fb2fee3849e102868c86fe3cab465fc70d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67707791"
---
# <a name="known-issues-with-hb-series-and-hc-series-vms"></a>HB serisi ve HC serisi sanal makineler ile ilgili bilinen sorunlar

Bu makalede, HB serisi ve HC serisi VM'ler kullanılırken en sık karşılaşılan sorunlar ve çözümler yer alabı.

## <a name="dram-on-hb-series"></a>HB serisi dram

HB serisi VM'ler şu anda konuk VM'lere yalnızca 228 GB RAM maruz bırakabilir. Bunun nedeni, sayfaların konuk VM için ayrılmış AMD CCX'in (NUMA etki alanları) yerel DRAM'ına atanmasını önlemek için Azure hipervizörünün bilinen bir sınırlamasıdır.

## <a name="accelerated-networking"></a>Hızlandırılmış Ağ

Azure Hızlandırılmış Ağ şu anda etkin değil, ancak Önizleme dönemi boyunca ilerledikçe etkinleştirilir. Bu özellik desteklendiğinde müşterilerimizi bilgilendiririz.

## <a name="qp0-access-restriction"></a>qp0 Erişim Kısıtlaması

Güvenlik açıklarına neden olabilecek düşük düzeyli donanım erişimini önlemek için, Sıra Çifti 0'a konuk VM'ler erişemez. Bu yalnızca genellikle ConnectX-5 NIC'nin yönetimiyle ilişkili eylemleri etkilemeli ve ibdiagnet gibi bazı InfiniBand tanılamalarını çalıştırmayı etkilemelidir, ancak son kullanıcı uygulamalarının kendileri çalışmamalıdır.

## <a name="ud-transport"></a>UD Taşımacılık

Lansmanda, HB- ve HC serisi Dinamik Bağlantılı Taşımayı (DCT) desteklemez. DCT desteği zaman içinde uygulanacaktır. Güvenilir Bağlantı (RC) ve Güvenilmez Datagram (UD) aktarımları desteklenir.

## <a name="gss-proxy"></a>GSS Proxy

GSS Proxy CentOS / RHEL 7.5 nfs ile kullanıldığında önemli bir performans ve duyarlılık cezası olarak tezahür edebilir bilinen bir hata vardır. Bu, şu şekilde azaltılabilir:

```console
sed -i 's/GSS_USE_PROXY="yes"/GSS_USE_PROXY="no"/g' /etc/sysconfig/nfs
```

## <a name="cache-cleaning"></a>Önbellek Temizleme

HPC sistemlerinde, bir sonraki kullanıcıya aynı düğüm atanmadan önce bir iş bittikten sonra belleği temizlemek genellikle yararlıdır. Linux'ta uygulamaları çalıştırdıktan sonra, herhangi bir uygulama çalıştırmamarağmen, arabellek belleğiniz artarken kullanılabilir belleğinizin aza dığını görebilirsiniz.

![Komut isteminin ekran görüntüsü](./media/known-issues/cache-cleaning-1.png)

Kullanarak `numactl -H` hangi NUMAnode(lar) bellek ile arabelleğe olduğunu gösterir (muhtemelen tüm). Linux'ta, kullanıcılar arabelleğe alınmış veya önbelleğe alınmış belleği 'ücretsiz'e döndürmek için önbellekleri üç şekilde temizleyebilir. Kök olmak ya da sudo izinleri olması gerekir.

```console
echo 1 > /proc/sys/vm/drop_caches [frees page-cache]
echo 2 > /proc/sys/vm/drop_caches [frees slab objects e.g. dentries, inodes]
echo 3 > /proc/sys/vm/drop_caches [cleans page-cache and slab objects]
```

![Komut isteminin ekran görüntüsü](./media/known-issues/cache-cleaning-2.png)

## <a name="kernel-warnings"></a>Çekirdek uyarıları

Linux altında HB serisi VM önyükleme yaparken aşağıdaki çekirdek uyarı iletilerini görebilirsiniz.

```console
[  0.004000] WARNING: CPU: 4 PID: 0 at arch/x86/kernel/smpboot.c:376 topology_sane.isra.3+0x80/0x90
[  0.004000] sched: CPU #4's llc-sibling CPU #0 is not on the same node! [node: 1 != 0]. Ignoring dependency.
[  0.004000] Modules linked in:
[  0.004000] CPU: 4 PID: 0 Comm: swapper/4 Not tainted 3.10.0-957.el7.x86_64 #1
[  0.004000] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007 05/18/2018
[  0.004000] Call Trace:
[  0.004000] [<ffffffffb8361dc1>] dump_stack+0x19/0x1b
[  0.004000] [<ffffffffb7c97648>] __warn+0xd8/0x100
[  0.004000] [<ffffffffb7c976cf>] warn_slowpath_fmt+0x5f/0x80
[  0.004000] [<ffffffffb7c02b34>] ? calibrate_delay+0x3e4/0x8b0
[  0.004000] [<ffffffffb7c574c0>] topology_sane.isra.3+0x80/0x90
[  0.004000] [<ffffffffb7c57782>] set_cpu_sibling_map+0x172/0x5b0
[  0.004000] [<ffffffffb7c57ce1>] start_secondary+0x121/0x270
[  0.004000] [<ffffffffb7c000d5>] start_cpu+0x5/0x14
[  0.004000] ---[ end trace 73fc0e0825d4ca1f ]---
```

Bu uyarıyı yoksayabilirsiniz. Bunun nedeni, Azure hipervizörünün zaman içinde ele alınacağı bilinen bir sınırlamadır.

## <a name="next-steps"></a>Sonraki adımlar

Azure'da [yüksek performanslı bilgi işlem](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) hakkında daha fazla bilgi edinin.
