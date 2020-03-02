---
title: Depolama eşitleme hizmeti dağıtma
description: Azure Dosya Eşitleme bulut kaynağı dağıtılıyor. Depolama eşitleme hizmeti. Geçiş belgeleri arasında paylaşılan ortak bir metin bloğu.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 4468d8c6b57b38bdbaaa01208cfc4915f734f6bb
ms.sourcegitcommit: 5192c04feaa3d1bd564efe957f200b7b1a93a381
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2020
ms.locfileid: "78209460"
---
Bu adımda, Azure abonelik kimlik bilgilerinizin olması gerekir. Kullandığınız Azure aboneliği, StorSimple için kullandığınız birinden farklı olabilir.

Azure Dosya Eşitleme yapılandırılacak çekirdek kaynak "depolama eşitleme hizmeti" olarak adlandırılır.
Şirketteki tüm sunucular için, şimdi veya gelecekte aynı dosya kümesini eşitlerken yalnızca bir tane dağıtmanız önerilir. Birden fazla StorSimple gereciniz varsa, her biri için bir depolama eşitleme hizmeti kaynağı oluşturmayı düşünebilirsiniz. Ancak, hiçbir zaman veri alışverişi gereken farklı sunucu kümelerine sahipseniz yalnızca birden fazla depolama eşitleme hizmeti oluşturmanız gerekir. Aksi takdirde, tek bir depolama eşitleme hizmeti en iyi uygulamadır.

Office konumunuza yakın olan depolama eşitleme hizmetiniz için bir Azure bölgesi seçin. Diğer tüm bulut kaynakları aynı bölgede dağıtılmalıdır.
En iyi yöntem, aboneliğinizde daha kolay yönetim sağlamak üzere eşitleme ve depolama kaynaklarını barındırmak için yeni bir kaynak grubu oluşturmaktır.

Aşağıdaki makalede bir depolama eşitleme hizmetinin nasıl dağıtılacağı açıklanmaktadır. Yalnızca belge parçasını izleyin. Sonraki adımlarda bu belge diğer alt bölümleri için bağlantılar olacaktır.

[Depolama eşitleme hizmeti dağıtmayı öğrenin.](../articles/storage/files/storage-sync-files-deployment-guide.md#deploy-the-storage-sync-service)
