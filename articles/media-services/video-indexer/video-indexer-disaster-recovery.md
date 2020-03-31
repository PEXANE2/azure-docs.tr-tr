---
title: Video Indexer failover ve felaket kurtarma
titleSuffix: Azure Media Services
description: Bölgesel bir veri merkezi hatası veya felaket oluşursa, ikincil bir Video Dizinleyici hesabına nasıl geçemeyeceğinızı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79499612"
---
# <a name="video-indexer-failover-and-disaster-recovery"></a>Video Indexer failover ve felaket kurtarma

Azure Medya Hizmetleri Video Dizinleyici, bölgesel bir veri merkezi kesintisi veya hatası varsa, hizmetin anında başarısız lığını sağlamaz. Bu makalede, uygulamalar için en iyi kullanılabilirliği sağlamak için ortamınızı nasıl yapılandırılamayınız ve bir felaket oluşursa kurtarma süresini en aza indirmek için nasıl yapılandırılacak açıklanmaktadır.

Azure'un yalıtım ve kullanılabilirlik ilkelerinden yararlanmak için iş sürekliliği olağanüstü durum kurtarmayı (BCDR) bölgesel çiftler arasında yapılandırmanızı öneririz. Daha fazla bilgi için Azure [eşleştirilmiş bölgelere](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)bakın.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliği. Henüz bir Azure aboneliğiniz yoksa, Azure [ücretsiz deneme](https://azure.microsoft.com/free/)sürümüne kaydolun.

## <a name="failover-to-a-secondary-account"></a>İkincil bir hesaba geçme

BCDR uygulamak için, fazlalık işlemek için iki Video Dizinleyici hesapları olması gerekir.

1. Azure'a bağlı iki Video Dizinleyici hesabı oluşturun (bkz. [video dizinleyici hesabı oluştur).](connect-to-azure.md) Birincil bölgeniz için bir hesap, eşleştirilmiş azure bölgesi için de diğer in' i oluşturun.
1. Birincil bölgenizde bir hata varsa, ikincil hesabı kullanarak dizin oluşturmaya geçin.

> [!TIP]
> Hizmet bildirimlerinde [etkinlik günlüğü uyarıları oluştur'a](../../service-health/alerts-activity-log-service-notifications.md)göre, hizmet durumu bildirimleri için etkinlik günlüğü uyarıları ayarlayarak BCDR'yi otomatikleştirebilirsiniz.

Birden çok kiracı kullanma hakkında bilgi [için](manage-multiple-tenants.md)bkz. BCDR'yi uygulamak için şu iki seçenekten birini seçin: [Kiracı başına Video Indexer hesabı](manage-multiple-tenants.md#video-indexer-account-per-tenant) veya kiracı başına Azure [aboneliği.](manage-multiple-tenants.md#azure-subscription-per-tenant)

## <a name="next-steps"></a>Sonraki adımlar

[Azure'a bağlı bir Video Dizinleyici hesabını yönetin.](manage-account-connected-to-azure.md)
