---
title: Depolama eşitleme hizmeti dağıtma
description: Depolama eşitleme hizmeti olan Azure Dosya Eşitleme bulut kaynağını dağıtın. Geçiş belgeleri arasında paylaşılan ortak bir metin bloğu.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: bd4af178d6e70d910d4643fc30c29fc8eb85303c
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043149"
---
Bu adımda, Azure abonelik kimlik bilgilerinizin olması gerekir.

Azure Dosya Eşitleme için yapılandırılacak çekirdek kaynağa *depolama eşitleme hizmeti* denir. Artık aynı dosya kümesini eşitlenen tüm sunucular için veya gelecekte yalnızca bir tane dağıtmanızı öneririz. Yalnızca verileri hiçbir zaman değiş tokuşu gereken farklı sunucu kümelerine sahipseniz, birden çok depolama eşitleme hizmeti oluşturun. Örneğin, aynı Azure dosya paylaşımının hiçbir zaman eşitlenmemesi gereken sunucularınız olabilir. Aksi halde, tek bir depolama eşitleme hizmeti en iyi uygulamadır.

Konumunuzda yakın olan depolama eşitleme hizmetiniz için bir Azure bölgesi seçin. Diğer tüm bulut kaynakları aynı bölgede dağıtılmalıdır. Yönetimi basitleştirmek için aboneliğinizde eşitleme ve depolama kaynaklarını barındıran yeni bir kaynak grubu oluşturun.

Daha fazla bilgi için, Azure Dosya Eşitleme dağıtma hakkında makalesindeki [depolama eşitleme hizmeti 'ni dağıtma hakkında bölümüne](../articles/storage/files/storage-sync-files-deployment-guide.md#deploy-the-storage-sync-service) bakın. Makalenin yalnızca bu bölümünü izleyin. Sonraki adımlarda makalenin diğer bölümlerine bağlantılar olacaktır.