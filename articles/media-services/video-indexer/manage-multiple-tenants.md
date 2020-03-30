---
title: Video Dizinleyici ile birden çok kiracıyı yönetme - Azure
description: Bu makalede, Video Indexer ile birden çok kiracı yönetmek için farklı tümleştirme seçenekleri önerir.
services: media-services
documentationcenter: ''
author: ika-microsoft
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 05/15/2019
ms.author: ikbarmen
ms.openlocfilehash: 18f2cf3daa281400151ba223e1735e7138d97e8e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76990513"
---
# <a name="manage-multiple-tenants"></a>Birden çok kiracıyı yönetme

Bu makalede, Video Indexer ile birden çok kiracı yönetmek için farklı seçenekler anlatılmaktadır. Senaryonuz için en uygun yöntemi seçin:

* Kiracı başına Video Indexer hesabı
* Tüm kiracılar için tek video indexer hesabı
* Kiracı başına Azure aboneliği

## <a name="video-indexer-account-per-tenant"></a>Kiracı başına Video Indexer hesabı

Bu mimari kullanırken, her kiracı için bir Video Indexer hesabı oluşturulur. Kiracılar kalıcı ve işlem katmanında tam yalıtıma sahiptir.  

![Kiracı başına Video Indexer hesabı](./media/manage-multiple-tenants/video-indexer-account-per-tenant.png)

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Müşteriler depolama hesaplarını paylaşmaz (müşteri tarafından el ile yapılandırılmadığı sürece).
* Müşteriler bilgi işlem işlemini (ayrılmış birimleri) paylaşmaz ve birbirlerinin iş yapma sürelerini etkilemez.
* Video Dizinleyici hesabını silerek kiracıyı sistemden kolayca kaldırabilirsiniz.
* Kiracılar arasında özel modelleri paylaşma olanağı yoktur.

    Özel modelleri paylaşmak için iş gereksinimi olmadığından emin olun.
* Kiracı başına birden çok Video Dizinleyici (ve ilişkili Medya Hizmetleri) hesabı nedeniyle yönetilmesi daha zordur.

> [!TIP]
> [Video Indexer Geliştirici Portalı'nda](https://api-portal.videoindexer.ai/) sisteminiz için bir yönetici kullanıcısı oluşturun ve kiracılarınıza ilgili [hesap erişim jetonunu](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Account-Access-Token)sağlamak için Yetkilendirme API'sini kullanın.

## <a name="single-video-indexer-account-for-all-users"></a>Tüm kullanıcılar için Tek Video Indexer hesabı

Bu mimari kullanırken, müşteri kiracı yalıtımsorumludur. Tüm kiracıların tek bir Azure Medya Hizmeti hesabı yla tek bir Video Indexer hesabı kullanması gerekir. İçerik yüklerken, ararken veya silerken, müşterinin kiracı için uygun sonuçları filtrelemesi gerekir.

![Tüm kullanıcılar için Tek Video Indexer hesabı](./media/manage-multiple-tenants/single-video-indexer-account-for-all-users.png)

Bu seçenekle, özelleştirme modelleri (Kişi, Dil ve Markalar) modelleri kiracıya göre filtreleyerek kiracılar arasında paylaşılabilir veya yalıtılabilir.

[Video yüklerken,](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?)kiracı başına farklı bir bölüm özniteliği belirtebilirsiniz. Bu, [arama API'sinde](https://api-portal.videoindexer.ai/docs/services/operations/operations/Search-videos?)yalıtıma izin verir. Arama API'sinde bölüm özniteliğini belirterek yalnızca belirtilen bölümün sonuçlarını alırsınız. 

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* İçerik ve özelleştirme modellerini kiracılar arasında paylaşma becerisi.
* Bir kiracı diğer kiracıların performansını etkiler.
* Müşterinin Video Indexer'ın üzerine karmaşık bir yönetim katmanı oluşturması gerekir.

> [!TIP]
> Kiracı işlerine [öncelik](upload-index-videos.md) vermek için öncelik özniteliğini kullanabilirsiniz.

## <a name="azure-subscription-per-tenant"></a>Kiracı başına Azure aboneliği 

Bu mimari kullanırken, her kiracının kendi Azure aboneliği olur. Her kullanıcı için, kiracı aboneliğinde yeni bir Video Indexer hesabı oluşturursunuz.

![Kiracı başına Azure aboneliği](./media/manage-multiple-tenants/azure-subscription-per-tenant.png)

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Bu, fatura ayırma yı etkinleştiren tek seçenektir.
* Bu tümleştirme, kiracı başına Video Indexer hesabından daha fazla yönetim yüküne sahiptir. Faturalandırma bir gereklilik değilse, bu makalede açıklanan diğer seçeneklerden birini kullanmanız önerilir.

## <a name="next-steps"></a>Sonraki adımlar

[Genel bakış](video-indexer-overview.md)
