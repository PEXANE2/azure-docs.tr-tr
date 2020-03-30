---
title: Azure Blockchain Service genel muhasebe sürümleri, yama, & yükseltme
description: Sistem yaması ve sistem tarafından yönetilen ve kullanıcı tarafından yönetilen yükseltmelerle ilgili ilkeler de dahil olmak üzere Azure Blockchain Hizmeti'nde desteklenen genel muhasebe sürümlerine genel bakış.
ms.date: 11/20/2019
ms.topic: conceptual
ms.reviewer: janders
ms.openlocfilehash: 8d4a77699dd80743acfadd4d72d6d75bc1939b3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74325180"
---
# <a name="supported-azure-blockchain-service-ledger-versions"></a>Desteklenen Azure Blockchain Hizmeti genel muhasebe sürümleri

Azure Blockchain Hizmeti, Azure Blockchain Hizmeti'nde konsorsiyum olarak tanımlanan bir grup bilinen katılımcı içinde özel işlemlerin işlenmesi için tasarlanmış Ethereum tabanlı [Quorum](https://www.goquorum.com/developers) defterini kullanır.

Şu anda Azure Blockchain [Hizmeti, Quorum sürüm 2.2.3](https://github.com/jpmorganchase/quorum/releases/tag/v2.2.3) ve [Tessera işlem yöneticisini](https://github.com/jpmorganchase/tessera)destekler.

## <a name="managing-updates-and-upgrades"></a>Güncelleştirmeleri ve yükseltmeleri yönetme

Quorum sürüm büyük, minör ve yama sürümleri ile yapılır. Örneğin, Quorum sürümü 2.0.1 ise, sürüm türü aşağıdaki gibi kategorize edilir:

|Ana | İkincil  | Patch  |
| :--- | :----- | :----- |
| 2 | 0 | 1 | 

Azure Blockchain Hizmeti, Quorum'un yama sürümlerini, Quorum'dan kullanıma sunulduktan sonraki 30 gün içinde mevcut çalışan üyelere otomatik olarak güncelleştirir.

## <a name="availability-of-new-ledger-versions"></a>Yeni genel muhasebe sürümlerinin kullanılabilirliği

Azure Blockchain Hizmeti, Quorum genel muhasebesinin en son büyük ve küçük sürümlerini, Quorum üreticisinden yayımlandıktan sonraki 60 gün içinde sağlar. Konsorsiyumların yeni bir üye ve konsorsiyum temin ederken aralarından seçim yapabilmek için en fazla dört küçük sürüm sağlanır. Büyük veya küçük bir sürümden yükseltme şu anda desteklenmez. Örneğin, sürüm 2.x çalıştırıyorsanız, sürüm 3.x'e yükseltme şu anda desteklenmez. Benzer şekilde, sürüm 2.2'yi çalıştırıyorsanız, sürüm 2.3'e yükseltme şu anda desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Blockchain Hizmetinde Sınırlar](limits.md)
