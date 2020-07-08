---
title: Depolama eşitleme hizmeti dağıtma
description: Depolama eşitleme hizmeti olan Azure Dosya Eşitleme bulut kaynağını dağıtma. Geçiş belgeleri arasında paylaşılan ortak bir metin bloğu.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 248b61530b80a00aa10272dcb3d28b85c6ee04e5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82143574"
---
Bu adımda, Azure abonelik kimlik bilgilerinizin olması gerekir.

Azure Dosya Eşitleme için yapılandırılacak çekirdek kaynağa *depolama eşitleme hizmeti*denir. Artık aynı dosya kümesini eşitlenen tüm sunucular için veya gelecekte yalnızca bir tane dağıtmanızı öneririz. Yalnızca verileri hiçbir zaman değiş tokuşu gereken farklı sunucu kümeleriniz varsa (örneğin: aynı Azure dosya paylaşımının eşitlenmesi), birden çok depolama eşitleme hizmeti oluşturun. Aksi halde, tek bir depolama eşitleme hizmeti en iyi uygulamadır.

Konumunuzda yakın olan depolama eşitleme hizmetiniz için bir Azure bölgesi seçin. Diğer tüm bulut kaynakları aynı bölgede dağıtılmalıdır.
Yönetimi basitleştirmek için aboneliğinizde eşitleme ve depolama kaynaklarını barındıran yeni bir kaynak grubu oluşturun.

Daha fazla bilgi için, Azure Dosya Eşitleme dağıtma hakkında makalesindeki [depolama eşitleme hizmeti 'ni dağıtma hakkında bölümüne](../articles/storage/files/storage-sync-files-deployment-guide.md#deploy-the-storage-sync-service) bakın. Makalenin yalnızca bu bölümünü izleyin. Sonraki adımlarda makalenin diğer bölümlerine bağlantılar olacaktır.