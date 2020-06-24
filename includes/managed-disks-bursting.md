---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 04/27/2020
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: 2af8b429b6addf6da32b34773525c51a36624e78
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85242123"
---
Azure 'da, hem sanal makinelere hem de disklere patlama olarak adlandırılan disk depolama ıOPS ve MB/s performansını artırma olanağı sunuyoruz. Busting, beklenmeyen disk trafiğini işleme veya toplu işleri işleme gibi birçok senaryoda faydalıdır. Hem VM 'niz hem de diskiniz üzerinde harika bir taban ve performans elde etmek için VM ve disk düzeyi patlaması etkin bir şekilde yararlanabilir. Bu şekilde, hem VM 'niz hem de diskiniz üzerinde harika temel performans ve performans elde edebilirsiniz. 

Lütfen disklere ve VM 'Lere gömülmüş bir diğerinden bağımsız olduğunu unutmayın. Bir disk diski aldıysanız, diskinizin aşırı çalışmasına izin vermek için bir patlama sanal makinesine ihtiyacınız yoktur. Bir patlama sanal ağınız varsa, sanal makinenizin veri bloğu ile tamamlanmasına izin vermek için bir diski diske almanız gerekmez. 