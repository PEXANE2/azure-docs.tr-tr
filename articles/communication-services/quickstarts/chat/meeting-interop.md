---
title: Azure Iletişim hizmetlerinde takımlar birlikte çalışabilirliği ile çalışmaya başlama
titleSuffix: An Azure Communication Services quickstart
description: Bu hızlı başlangıçta, Azure Iletişim sohbeti SDK 'Sı ile bir takım toplantısına nasıl katılacağınızı öğreneceksiniz.
author: askaur
ms.author: askaur
ms.date: 03/10/2021
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: d7ea3b67c3ce85ce104d16785e4e5f4d45b138f6
ms.sourcegitcommit: bed20f85722deec33050e0d8881e465f94c79ac2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2021
ms.locfileid: "105106810"
---
# <a name="quickstart-join-your-chat-app-to-a-teams-meeting"></a>Hızlı başlangıç: sohbet uygulamanızı bir takımlar toplantısı 'na ekleme

> [!IMPORTANT]
> [Ekipte kiracı birlikte çalışabilirliğini](../../concepts/teams-interop.md)etkinleştirmek/devre dışı bırakmak için [Bu formu](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR21ouQM6BHtHiripswZoZsdURDQ5SUNQTElKR0VZU0VUU1hMOTBBMVhESS4u)doldurun.

JavaScript SDK 'sını kullanarak sohbet çözümünüzü Microsoft ekiplerine bağlayarak Azure Iletişim Hizmetleri 'ni kullanmaya başlayın. 

## <a name="prerequisites"></a>Önkoşullar 

1. Bir [takımlar dağıtımı](/deployoffice/teams-install). 
2. Çalışan bir [sohbet uygulaması](./get-started.md). 

## <a name="enable-teams-interoperability"></a>Takımların birlikte çalışabilirliğini etkinleştir 

Bir Konuk Kullanıcı olarak toplantıya katılan bir Iletişim Hizmetleri kullanıcısı, Toplantı sohbetine yalnızca takımlar toplantı çağrısına katıldığında erişebilir. Bir ekip hizmetine bir Iletişim Hizmetleri kullanıcısının nasıl ekleneceğini öğrenmek için [takımlar birlikte çalışma](../voice-video-calling/get-started-teams-interop.md) belgelerine bakın.

Bu özelliği kullanmak için her iki varlığın sahip olduğu kuruluşun bir üyesi olmanız gerekir.

[!INCLUDE [Join Teams meetings](./includes/meeting-interop-javascript.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir Iletişim Hizmetleri aboneliğini temizleyip kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler. [Kaynakları Temizleme](../create-communication-resource.md#clean-up-resources)hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

- [Sohbet Hero örneğine](../../samples/chat-hero-sample.md) göz atın
- [Sohbet 'in nasıl çalıştığı](../../concepts/chat/concepts.md) hakkında daha fazla bilgi edinin