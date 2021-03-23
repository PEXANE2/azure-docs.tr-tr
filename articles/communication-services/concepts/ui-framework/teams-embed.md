---
title: UI çerçevesi takımları ekleme
titleSuffix: An Azure Communication Services quickstart
description: Bu belgede, Azure Iletişim Hizmetleri Kullanıcı arabirimi çerçevesi ekip ekleme özelliğinin, anahtar çağırma deneyimleri oluşturmak için nasıl kullanılabileceğini öğreneceksiniz.
author: tophpalmer
ms.author: chpalm
ms.date: 11/16/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 99263695aa0842daf59f23cda115dcb5b27b7add
ms.sourcegitcommit: ba3a4d58a17021a922f763095ddc3cf768b11336
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804134"
---
# <a name="teams-embed"></a>Ekip ekleme

[!INCLUDE [Public Preview Notice](../../includes/private-preview-include.md)]


Ekip ekleme, ortak işletmeden işletmeye ve işletmeler arası çağrı etkileşimlerine odaklanan bir Azure Iletişim Hizmetleri özelliğidir. Takımlar ekleme sistemi, [video ve sesli çağrı](../voice-video-calling/calling-sdk-features.md), ancak takımlar, Microsoft ekipleri toplantılarına dayalı olarak tüm Kullanıcı deneyimini sunmak için Azure 'un arama temel elemanlarına sistem derlemeleri katıştırır.

Ekip ekleme istemci kitaplıkları kapalı kaynağıdır ve bu özellikleri bir anahtar, bileşik biçimde kullanıma hazır hale getirir. Ekipleri uygulamanızın tuvaline gömdüğünüzde ve istemci kitaplığı, tüm Kullanıcı deneyimini oluşturur. Bu Kullanıcı deneyimi Microsoft ekipleri toplantılarına çok benzer olduğundan, aşağıdaki avantajlardan yararlanabilirsiniz:

- Azaltılan geliştirme süresi ve mühendislik karmaşıklığı
- Ekipler ile son kullanıcı hakkında benzerlik
- [Takımlar Son Kullanıcı eğitim içeriğini](https://support.microsoft.com/office/meetings-in-teams-e0b0ae21-53ee-4462-a50d-ca9b9e217b67) yeniden kullanma yeteneği

Takımlar ekleme, takımlar toplantılarında desteklenen özelliklerin çoğunu sağlar; örneğin:

- Kullanıcının ses ve video cihazlarını yapılandırdığı ön toplantı deneyimi
- Ses ve video cihazlarını yapılandırmak için toplantı içi deneyim
- [Video arka planları](https://support.microsoft.com/office/change-your-background-for-a-teams-meeting-f77a2381-443a-499d-825e-509a140f4780): katılımcıların arka planlarını bulanıklaştırmasını veya değiştirmesini sağlar
- Video galerisinin büyük galerisiyle birlikte mod, odak, sabitleme ve Spotlight [Için birden çok seçenek](https://support.microsoft.com/office/using-video-in-microsoft-teams-3647fc29-7b92-4c26-8c2d-8a596904cdae)
- [Içerik paylaşımı](https://support.microsoft.comoffice/share-content-in-a-meeting-in-teams-fcc2bf59-aecd-4481-8f99-ce55dd836ce8#ID0EABAAA=Mobile): katılımcıların kendi ekranını paylaşmasına izin verme

Diğer Azure Iletişim SDK 'larına kıyasla bu kullanıcı arabirimi hakkında daha fazla bilgi için, bkz. [UI SDK 'sı kavramı tanıtımı](ui-sdk-overview.md). 
