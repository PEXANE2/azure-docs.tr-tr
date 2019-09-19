---
title: Azure Güvenlik Duvarı için altyapı FQDN 'SI
description: Azure Güvenlik duvarında altyapı FQDN 'leri hakkında bilgi edinin
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 09/20/2019
ms.author: victorh
ms.openlocfilehash: 5b134058e1924bae030338411226a9a6aff46557
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71130195"
---
# <a name="infrastructure-fqdns"></a>Altyapı FQDN'leri

Azure Güvenlik Duvarı'nda varsayılan olarak izin verilen altyapı FQDN'leri için yerleşik bir kural koleksiyonu bulunur. Bu FQDN'ler platforma özgüdür ve başka amaçlarla kullanılamaz. 

Aşağıdaki hizmetler yerleşik kural koleksiyonuna dahildir:

- Depolama platformu görüntü deposuna (PIR) erişim işlem
- Yönetilen diskler durum depolama erişimi
- Azure Tanılama ve günlüğe kaydetme (MDS)

## <a name="overriding"></a>Si 

Bu yerleşik altyapı kuralı koleksiyonunu, son işlenen bir reddetme tüm uygulama kuralı koleksiyonu oluşturarak geçersiz kılabilirsiniz. Bu kural her zaman altyapı kuralı koleksiyonundan önce işlenir. Altyapı kuralı koleksiyonunda bulunmayan öğeler varsayılan olarak reddedilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik duvarını dağıtmayı ve yapılandırmayı](tutorial-firewall-deploy-portal.md)öğrenin.