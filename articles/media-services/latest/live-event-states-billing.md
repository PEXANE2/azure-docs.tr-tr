---
title: Azure Medya Hizmetleri'nde LiveEvent durumları ve faturalandırma | Microsoft Dokümanlar
description: Bu konu, Azure Medya Hizmetleri LiveEvent durumlarına ve faturalandırmaya genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 10/24/2019
ms.author: juliako
ms.openlocfilehash: e4eee3b9f3f97bf7cd7a7b61425ec5c9a3a198ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76543084"
---
# <a name="live-event-states-and-billing"></a>Canlı Etkinlik durumları ve faturalandırma

Azure Medya Hizmetleri'nde, Bir Canlı Etkinlik, durumu **Çalışır duruma**geçer geçmez faturalandırmaya başlar. Hizmette akan bir video olmasa bile faturalandırılırsınız. Canlı Etkinliğin faturalandırmasını durdurmak için Canlı Etkinliği durdurmanız gerekir. Canlı Transkripsiyon, Live Event ile aynı şekilde faturalandırılır.

Live **EventEncodingType** [Live Event'inizde](https://docs.microsoft.com/rest/api/media/liveevents) Standart veya Premium1080p olarak ayarlandığında, Medya Hizmetleri giriş akışı kaybolduktan 12 saat sonra hala **Çalışan** durumdaki tüm Live Event'i otomatik olarak kapatır ve çalışan **Canlı Çıkış**yoktur. Ancak, Canlı Etkinlik **Çalışan** durumunda olduğu süre için faturalandırılırsınız.

> [!NOTE]
> Geçiş Canlı Etkinlikler otomatik olarak kapatılmaz ve aşırı faturalandırmayı önlemek için API aracılığıyla açıkça durdurulmalıdır. 

## <a name="states"></a>Durumlar

Canlı Etkinlik aşağıdaki durumlardan birinde olabilir.

|Durum|Açıklama|
|---|---|
|**Durduruldu**| Bu, oluşturulduktan sonraki Canlı Olay'ın ilk durumudur (otomatik başlatma doğru ayarlanmadığı sürece.) Bu durumda faturalandırma oluşmaz. Bu durumda, Canlı Olay özellikleri güncelleştirilebilir, ancak akışa izin verilmez.|
|**Başlatılıyor**| Canlı Etkinlik başlatılıyor ve kaynaklar tahsis ediliyor. Bu durumda faturalandırma oluşmaz. Bu durumda güncelleştirmeler veya akışa izin verilmez. Bir hata oluşursa, Canlı Olay Durduruldu durumuna geri döner.|
|**Çalışıyor**| Canlı Etkinlik kaynakları ayrıldı, yutma ve önizleme URL'leri oluşturuldu ve canlı akış lar alabilme yeteneğine sahip. Bu noktada, faturalandırma etkindir. Daha fazla faturalandırmayı durdurmak için Canlı Etkinlik kaynağında Durdur'u açıkça aramalısınız.|
|**Durduruluyor**| Canlı Etkinlik durduruluyor ve kaynaklar yok ediliyor. Bu geçici durumda faturalandırma oluşmaz. Bu durumda güncelleştirmeler veya akışa izin verilmez.|
|**Silme**| Canlı Etkinlik siliniyor. Bu geçici durumda faturalandırma oluşmaz. Bu durumda güncelleştirmeler veya akışa izin verilmez.|

Canlı Etkinliği oluştururken Canlı Transkripsiyonları etkinleştirmeyi seçebilirsiniz. Bunu yaparsanız, Canlı Etkinlik **Çalışan** durumda olduğunda Canlı Transkripsiyonlar için faturalandırılırsınız. Canlı Etkinlik'te ses akışı olmasa bile faturalandırılacağınızı unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Canlı akışa genel bakış](live-streaming-overview.md)
- [Canlı akış eğitimi](stream-live-tutorial-with-api.md)
