---
title: Azure VM'lerde ekli VH'ler ile VM'lerin beklenmeyen yeniden başlatmalarını sorun giderme | Microsoft Dokümanlar
description: VM'lerin beklenmeyen yeniden başlatmalarını giderme sorunu nasıl giderilir.
keywords: ssh bağlantısı reddedildi, ssh hatası, azure ssh, SSH bağlantısı başarısız oldu
services: virtual-machines
author: genlin
manager: dcscontentpm
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines
ms.topic: article
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: 3a06db1afd130d936af868d0d20632c3ec4fbfd2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75358535"
---
# <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Bağlı VHD'lerle VM'lerin beklenmeyen yeniden başlatmalarını giderme

Bir Azure Sanal Makine 'de (VM) aynı depolama hesabında çok sayıda bağlı VHD varsa, tek bir depolama hesabının ölçeklenebilirlik hedeflerini aşarak VM'nin beklenmedik bir şekilde yeniden başlatılmasına neden olabilirsiniz. Depolama hesabının ölçeklenebilirlik hedeflerini aşan ani artışlar için**TotalIngress**/depolama hesabının dakika ölçümlerini **(Toplam**/**İstektoplamEgress)** denetleyin. Bkz. Ölçümler, depolama hesabınızda azaltma olup olmadığını belirlemede yardımcı olmak için [YüzdeThrottlingError'ta bir artış gösterir.](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#metrics-show-an-increase-in-PercentThrottlingError)

Genel olarak, Sanal Makine'den bir VHD'deki her bir giriş veya çıktı işlemi, temel sayfa blob'undaki **Sayfa yı al** veya Sayfa **koy** işlemleri anlamına gelir. Bu nedenle, uygulamanızın belirli davranışına bağlı olarak tek bir depolama hesabında kaç VHD'niz olabileceğini ayarlamak için ortamınız için tahmini IOPS'yi kullanabilirsiniz. Microsoft, tek bir depolama hesabında 40 veya daha az disk olmasını önerir. Standart depolama hesapları için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için, [standart depolama hesapları için Ölçeklenebilirlik hedeflerine](../../storage/common/scalability-targets-standard-account.md)bakın. Premium sayfa blob depolama hesapları için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için, [premium sayfa blob depolama hesapları için Ölçeklenebilirlik hedeflerine](../../storage/blobs/scalability-targets-premium-page-blobs.md)bakın.

Depolama hesabınıziçin ölçeklenebilirlik hedeflerini aşıyorsanız, her bir hesaptaki etkinliği azaltmak için VHD'lerinizi birden çok depolama hesabına yerleştirin.
