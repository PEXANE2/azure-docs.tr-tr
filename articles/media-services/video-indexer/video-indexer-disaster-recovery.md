---
title: Video Indexer yük devretme ve olağanüstü durum kurtarma
titleSuffix: Azure Media Services
description: Bölgesel bir veri merkezi arızası veya olağanüstü durum oluşursa ikincil Video Indexer hesabına yük devretmeyi öğrenin.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 07/29/2019
ms.author: juliako
ms.openlocfilehash: 17c21900448fcb6d0a40fe5407f3b8bd62f9e3e4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79499612"
---
# <a name="video-indexer-failover-and-disaster-recovery"></a>Video Indexer yük devretme ve olağanüstü durum kurtarma

Azure Media Services Video Indexer, bölgesel bir veri merkezi kesintisi veya hatası varsa hizmetin anında yük devretmesini sağlamaz. Bu makalede, bir olağanüstü durum oluşursa uygulamalar için en iyi kullanılabilirliği sağlamak üzere bir yük devretme için ortamınızın nasıl yapılandırılacağı ve en aza küçültülmüş kurtarma süresi açıklanır.

Azure 'un yalıtım ve kullanılabilirlik ilkelerinden faydalanmasını sağlamak için iş sürekliliği olağanüstü durum kurtarma (BCDR) ' i bölgesel çiftler arasında yapılandırmanızı öneririz. Daha fazla bilgi için bkz. [Azure eşlenmiş bölgeler](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliği. Henüz bir Azure aboneliğiniz yoksa [Azure Ücretsiz deneme sürümü](https://azure.microsoft.com/free/)için kaydolun.

## <a name="failover-to-a-secondary-account"></a>İkincil bir hesaba yük devretme

BCDR 'yi uygulamak için, artıklığı işlemek üzere iki Video Indexer hesabınızın olması gerekir.

1. Azure 'a bağlı iki Video Indexer hesabı oluşturun (bkz. [video Indexer hesabı oluşturma](connect-to-azure.md)). Birincil bölgeniz ve diğeri eşleştirilmiş Azure bölgesi için bir hesap oluşturun.
1. Birincil bölgenizde bir hata varsa, ikincil hesabı kullanarak dizin oluşturma ' ya geçin.

> [!TIP]
> Hizmet [bildirimlerinde etkinlik günlüğü uyarıları oluşturma](../../service-health/alerts-activity-log-service-notifications.md)başına hizmet durumu bildirimleri için etkinlik günlüğü uyarılarını ayarlayarak BCDR 'yi otomatikleştirebilirsiniz.

Birden çok kiracı kullanma hakkında bilgi için bkz. [birden çok kiracıyı yönetme](manage-multiple-tenants.md). BCDR 'yi uygulamak için şu iki seçenekten birini seçin: [kiracı başına video Indexer hesap](manage-multiple-tenants.md#video-indexer-account-per-tenant) veya [kiracı başına Azure aboneliği](manage-multiple-tenants.md#azure-subscription-per-tenant).

## <a name="next-steps"></a>Sonraki adımlar

[Azure 'a bağlı bir video Indexer hesabını yönetin](manage-account-connected-to-azure.md).
