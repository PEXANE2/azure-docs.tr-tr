---
title: Azure VM 'lerinde bağlı VHD 'Ler ile VM 'lerin beklenmedik şekilde yeniden başlatmaları için sorun giderme Microsoft Docs
description: VM 'lerin beklenmedik şekilde yeniden başlatılmasıyla ilgili sorunları giderme.
keywords: SSH bağlantısı reddedildi, SSH hatası, Azure SSH, SSH bağlantısı başarısız oldu
services: virtual-machines
author: genlin
manager: dcscontentpm
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.service: virtual-machines
ms.topic: article
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: 3a06db1afd130d936af868d0d20632c3ec4fbfd2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75358535"
---
# <a name="troubleshoot-unexpected-reboots-of-vms-with-attached-vhds"></a>Bağlı VHD 'lerle VM 'lerin beklenmedik şekilde yeniden başlatmaları için sorun giderme

Bir Azure sanal makinesi (VM), aynı depolama hesabında bulunan çok sayıda bağlı VHD 'ye sahipse, tek bir depolama hesabı için ölçeklenebilirlik hedeflerini aşabilirsiniz, bu da VM 'nin beklenmedik şekilde yeniden başlatılmasına neden olabilir. Depolama hesabı için ölçeklenebilirlik hedeflerini aşan ani artışlar için depolama hesabının (**totalrequests** / **totalınress** / **TotalEgress**) dakika ölçümlerini denetleyin. Depolama hesabınızda azaltma yapılıp yapılmayacağını belirlemede yardım için bkz. [ölçümler, Percentkısıtıngerror 'da artış gösterir](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#metrics-show-an-increase-in-PercentThrottlingError) .

Genel olarak, bir sanal makineden bir VHD üzerindeki her tek giriş veya çıkış işlemi, temel alınan sayfa blobuna **sayfa al** veya **sayfa yerleştirme** işlemleri yapar. Bu nedenle, uygulamanızın belirli davranışına göre tek bir depolama hesabında kaç VHD 'nin olduğunu ayarlamak için ortamınız için tahmini ıOPS 'yi kullanabilirsiniz. Microsoft, tek bir depolama hesabında 40 veya daha az disk olmasını önerir. Standart depolama hesapları için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için bkz. [Standart depolama hesapları Için ölçeklenebilirlik hedefleri](../../storage/common/scalability-targets-standard-account.md). Premium sayfa BLOB depolama hesapları için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için bkz. [Premium sayfa BLOB depolama hesapları Için ölçeklenebilirlik hedefleri](../../storage/blobs/scalability-targets-premium-page-blobs.md).

Depolama hesabınız için ölçeklenebilirlik hedeflerini aşmanız durumunda, her bir hesaptaki etkinliği azaltmak için VHD 'nizi birden fazla depolama hesabına yerleştirin.
