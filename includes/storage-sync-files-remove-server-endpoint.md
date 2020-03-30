---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/31/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: be3ce42ccdb9bedd02b8dead2426ac629fa12ef2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72391822"
---
Hayır: bir sunucu bitiş noktasını kaldırmak sunucunun yeniden başlatılması gibi değildir! Sunucu bitiş noktasını kaldırmak ve yeniden oluşturmak, Azure Dosya Eşitlemenin eşitlenmesi, bulut katmanlama veya diğer yönleriyle ilgili sorunları gidermek için neredeyse hiçbir zaman uygun bir çözüm değildir. Sunucu bitiş noktasını kaldırmak yıkıcı bir işlemdir. Katmanlı dosyaların sunucu bitiş noktası ad alanının dışında bulunması durumunda veri kaybına neden olabilir. Daha fazla bilgi için [katmanlı dosyaların neden sunucu bitiş noktası ad alanının dışında var olduğunu](../articles/storage/files/storage-files-faq.md#afs-tiered-files-out-of-endpoint) görün. Veya sunucu bitiş noktası ad alanı içinde bulunan katmanlı dosyalar için erişilemez dosyalarneden olabilir. Sunucu bitiş noktası yeniden oluşturulduğunda bu sorunlar çözülmez. Bulut katmanlama etkinleştirilmiş olsa bile, katmanlı dosyalar sunucu bitiş noktası ad alanınızda bulunabilir. Bu nedenle, azure dosya eşitlemesini bu klasörle kullanmayı durdurmak istemiyorsanız veya bir Microsoft mühendisi tarafından açıkça talimat verilmedikçe sunucu bitiş noktasını kaldırmamanızı öneririz. Sunucu uç noktalarını kaldırma hakkında daha fazla bilgi [için](../articles/storage/files/storage-sync-files-server-endpoint.md#remove-a-server-endpoint)bkz.    
