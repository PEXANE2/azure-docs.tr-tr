---
title: Depolama Eşitleme Hizmeti Dağıtma
description: Azure Dosya Eşitleme bulut u kaynağını dağıtma. Bir Depolama Eşitleme Hizmeti. Geçiş dokümanları arasında paylaşılan ortak bir metin bloğu.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: fe74bae95784dcb63a80ee7e280c02de2eadeb5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80124736"
---
Bu adımda, Azure abonelik kimlik bilgilerinize ihtiyacınız var.

Azure Dosya Eşitlemeyi yapılandırmak için temel kaynak "Depolama Eşitleme Hizmeti" olarak adlandırılır.
Şirketteki tüm sunucular için şimdi veya gelecekte aynı dosya kümesini eşitleyen yalnızca bir tane dağıtmanızı öneririz. Yalnızca, veri alışverişinde bulunmaması gereken farklı sunucu kümeleri varsa birden çok Depolama Eşitleme Hizmeti oluşturun. (örneğin: aynı Azure dosya paylaşımını eşitle). Aksi takdirde tek bir Depolama Eşitleme Hizmeti en iyi uygulamadır.

Depolama Eşitleme Hizmetiniz için ofis konumunuza yakın bir Azure bölgesi seçin. Diğer tüm bulut kaynaklarının aynı bölgede dağıtılması gerekir.
Yönetimi basitleştirmek için aboneliğinizde eşitleme ve depolama kaynaklarını barındıran yeni bir kaynak grubu oluşturun.

Aşağıdaki makalede, Depolama Eşitleme Hizmeti'nin nasıl dağıtılancaya kadar dağıtılanınca açıklanmaktadır. Sadece dokümanın bu kısmını takip edin. Daha sonraki adımlarda bu dokümanın diğer alt bölümlerine bağlantılar olacaktır.

[Depolama Eşitleme Hizmeti'ni nasıl dağıtılayacağım öğrenin.](../articles/storage/files/storage-sync-files-deployment-guide.md#deploy-the-storage-sync-service)
