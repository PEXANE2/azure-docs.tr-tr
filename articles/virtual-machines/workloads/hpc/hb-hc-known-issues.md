---
title: HB Serisi ve HC Serisi VM 'lerle ilgili bilinen sorunlar-Azure sanal makineleri | Microsoft Docs
description: Azure 'da HB Serisi VM boyutlarıyla ilgili bilinen sorunlar hakkında bilgi edinin.
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
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67707791"
---
# <a name="known-issues-with-hb-series-and-hc-series-vms"></a>HB serisi ve HC serisi sanal makineler ile ilgili bilinen sorunlar

Bu makalede, HB Serisi ve HC Serisi VM 'Leri kullanırken en yaygın sorunlar ve çözümler sağlanmaktadır.

## <a name="dram-on-hb-series"></a>DRAM on HB Serisi

HB Serisi VM 'Ler Şu anda Konuk VM 'lere yalnızca 228 GB RAM kullanıma sunabilir. Bunun nedeni, sayfaların Konuk sanal makine için ayrılmış olan (NUMA etki alanları) yerel DRAM 'ye atanmasını engellemek için Azure hiper yöneticisinin bilinen bir sınırlamasıdır.

## <a name="accelerated-networking"></a>Hızlandırılmış Ağ

Azure hızlandırılmış ağ Iletişimi Şu anda etkin değil, ancak önizleme dönemi boyunca ilerliyoruz. Bu özellik desteklenmiş olduğunda müşterileri bilgilendireceğiz.

## <a name="qp0-access-restriction"></a>qp0 erişim kısıtlaması

Güvenlik açıklarına neden olan alt düzey donanım erişimini engellemek için, 0 kuyruk çifti Konuk VM 'Ler tarafından erişilemez. Bu, genellikle yalnızca ConnectX-5 NIC yönetimi ile ilişkili eylemleri ve ıdiagnet gibi bazı InfiniBand tanılamayı çalıştırıyor, ancak son kullanıcı uygulamalarının kendisini etkilemez.

## <a name="ud-transport"></a>UD taşıması

Başlatılırken, HB ve HC Serisi dinamik olarak bağlı taşımayı (DCT) desteklemez. DCT desteği zaman içinde uygulanır. Güvenilir bağlantı (RC) ve güvenilir olmayan veri birimi (UD) aktarımları desteklenir.

## <a name="gss-proxy"></a>GSS proxy

GSS proxy 'Si, SP1 ile kullanıldığında önemli bir performans ve yanıt verme cezası olarak bildirimde bulunan CentOS/RHEL 7,5 ' de bilinen bir hataya sahiptir. Bu, ile azaltılabilir:

```console
sed -i 's/GSS_USE_PROXY="yes"/GSS_USE_PROXY="no"/g' /etc/sysconfig/nfs
```

## <a name="cache-cleaning"></a>Önbellek Temizleme

HPC sistemlerinde, bir sonraki kullanıcıya aynı düğüm atanmadan önce, bir iş bittikten sonra belleği temizlemek genellikle yararlı olur. Linux 'ta uygulama çalıştırdıktan sonra, herhangi bir uygulama çalıştırmamasına rağmen arabellek belleğinizin arttığı sırada kullanılabilir belleğinizin azaldığı fark edebilirsiniz.

![Komut isteminin ekran görüntüsü](./media/known-issues/cache-cleaning-1.png)

Kullanmak `numactl -H` , belleğin hangi numlik ile arabelleğe alındığına (muhtemelen büyük olasılıkla) sahip olduğunu gösterir. Linux 'ta kullanıcılar, arabelleğe alınmış veya önbelleğe alınmış belleği ' ücretsiz ' olarak döndürmek için önbellekleri üç yolla temizleyebilir. Kök olmanız veya sudo izinleriniz olması gerekir.

```console
echo 1 > /proc/sys/vm/drop_caches [frees page-cache]
echo 2 > /proc/sys/vm/drop_caches [frees slab objects e.g. dentries, inodes]
echo 3 > /proc/sys/vm/drop_caches [cleans page-cache and slab objects]
```

![Komut isteminin ekran görüntüsü](./media/known-issues/cache-cleaning-2.png)

## <a name="kernel-warnings"></a>Çekirdek uyarıları

Linux altında bir HB Serisi VM 'yi önyüklerken aşağıdaki çekirdek uyarı iletilerini görebilirsiniz.

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

Bu uyarıyı yoksayabilirsiniz. Bunun nedeni, Azure hiper yöneticinin zaman içinde ele alacak bilinen bir sınırlamasıdır.

## <a name="next-steps"></a>Sonraki adımlar

Azure 'da [yüksek performanslı bilgi işlem](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) hakkında daha fazla bilgi edinin.
