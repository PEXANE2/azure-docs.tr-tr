---
title: Azure Medya Hizmetleri v3 hesaplarını yönetme | Microsoft Dokümanlar
description: Azure'da medya içeriğini yönetmeye, şifrelemeye, kodlamaya, analiz etmeye ve akışa başlamak için bir Medya Hizmetleri hesabı oluşturmanız gerekir. Bu makalede, Azure Media Services v3 hesaplarının nasıl yönetilenbir şekilde yönetileni açıklanmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 07/08/2019
ms.author: juliako
ms.openlocfilehash: 08579f7ba952bb4ebcba1595508612affb852528
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75980373"
---
# <a name="manage-azure-media-services-v3-accounts"></a>Azure Medya Hizmetleri v3 hesaplarını yönetme

Azure'da medya içeriğini yönetmeye, şifrelemeye, kodlamaya, analiz etmeye ve akışa başlamak için bir Medya Hizmetleri hesabı oluşturmanız gerekir. Media Services hesabı oluştururken, bir Azure Depolama hesabı kaynağının adını sağlamanız gerekir. Belirtilen depolama hesabı, Media Services hesabınıza eklenir. Media Services hesabı ve onunla ilişkili tüm depolama hesaplarının aynı Azure aboneliğinde olması gerekir. Daha fazla bilgi için [Depolama hesapları'na](storage-account-concept.md)bakın.

## <a name="moving-a-media-services-account-between-subscriptions"></a>Medya Hizmetleri hesabını abonelikler arasında taşıma 

Bir Medya Hizmetleri hesabını yeni bir aboneliğe taşımanız gerekiyorsa, önce Medya Hizmetleri hesabını içeren tüm kaynak grubunu yeni aboneliğe taşımanız gerekir. Ekli tüm kaynakları taşımalısınız: Azure Depolama hesapları, Azure CDN profilleri vb. Daha fazla bilgi için [bkz.](../../azure-resource-manager/management/move-resource-group-and-subscription.md) Azure'daki tüm kaynaklarda olduğu gibi, kaynak grubu hareketlerinin tamamlanması biraz zaman alabilir.

> [!NOTE]
> Media Services v3 çoklu kira modelini destekler.

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Farklı bir aboneye geçmeden önce hesabınızdaki tüm verilerin yedeklerini oluşturun.
* Tüm Akış Uç Noktalarını ve canlı akış kaynaklarını durdurmanız gerekir. Kullanıcılarınız, kaynak grubu taşıması süresince içeriğinize erişemez. 

> [!IMPORTANT]
> Hareket başarıyla tamamlanana kadar Akış Bitiş Noktası'nı başlatmayın.

### <a name="troubleshoot"></a>Sorun giderme 

Bir Medya Hizmetleri hesabı veya ilişkili bir Azure Depolama hesabı kaynak grubu hareketinden sonra "bağlantısı kesilirse" Depolama Hesabı anahtarlarını döndürmeyi deneyin. Depolama Hesabı anahtarlarını döndürmek Medya Hizmetleri hesabının "bağlantısı kesilmiş" durumunu çözmezse, Medya Hizmetleri hesabındaki "Destek + sorun giderme" menüsünden yeni bir destek isteği gönderin.  

## <a name="next-steps"></a>Sonraki adımlar

[Hesap oluşturma](create-account-cli-quickstart.md)
