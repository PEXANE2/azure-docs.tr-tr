---
title: Özel Komutlar uygulamanızı test etme
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel komutlar uygulamasını test etmek için farklı yaklaşımlar öğreneceksiniz.
services: cognitive-services
author: xiaojul
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: xiaojul
ms.custom: devx-track-csharp
ms.openlocfilehash: 9c0bd21f55fee4d8487826deae23093ede293c8c
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95021822"
---
# <a name="test-your-custom-commands-application"></a>Özel komutlar uygulamanızı test etme

Bu makalede, özel komutlar uygulamasını test etmek için farklı yaklaşımlar öğreneceksiniz.

## <a name="test-in-the-portal"></a>Portalda test etme

Portalda test, özel komut uygulamanızın beklendiği gibi çalıştığını denetmanın en basit ve en hızlı yoludur. Uygulama başarıyla eğitilirken, `Test` teste başlamak için düğmeye tıklayın.

> [!div class="mx-imgBorder"]
> ![Portalda test etme](media/custom-commands/create-basic-test-chat.png)

## <a name="test-with-windows-voice-assistant-client"></a>Windows Ses Yardımcısı Istemcisiyle test etme

Windows Ses Yardımcısı Istemcisi, özel bir istemci uygulaması oluşturmadan önce Botunuzla etkileşimi test etmek için C# ' ta bir Windows Presentation Foundation (WPF) uygulamasıdır.

Araç, özel bir komut uygulama KIMLIĞI kabul edebilir. Bu, özel komutlar hizmetinde barındırılan görev tamamlamayı veya komut ve denetim senaryosunu test etmenize olanak tanır.

İstemciyi kurmak için [Windows Voice Yardımcısı istemcisini](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/tree/master/clients/csharp-wpf)kullanıma alın.

> [!div class="mx-imgBorder"]
> ![WVAC profil oluştur](media/custom-commands/conversation.png)

## <a name="test-with-speech-sdk-enabled-client-applications"></a>Konuşma SDK 'Sı etkin istemci uygulamalarıyla test etme 
Konuşma yazılım geliştirme seti (SDK), konuşma özellikli uygulamalar geliştirmenize imkan tanıyan birçok konuşma hizmeti özelliği sunar. Ayrıca birçok programlama dilinde ve tüm platformlarda kullanılabilir.

Konuşma SDK ile Evrensel Windows Platformu (UWP) istemci uygulaması ayarlamak ve özel komut uygulamanızla bütünleştirmek için:  
- [Nasıl yapılır: konuşma SDK kullanarak istemci uygulamasıyla tümleştirme](./how-to-custom-commands-setup-speech-sdk.md)
- [Nasıl yapılır: etkinliği istemci uygulamasına gönderme](./how-to-custom-commands-send-activity-to-client.md)
- [Nasıl yapılır: Web uç noktalarını ayarlama](./how-to-custom-commands-setup-web-endpoints.md)

Diğer programlama dilleri ve platformları için:
- [Konuşma SDK 'Sı programlama dilleri, platformlar, senaryo kapasiteleri](./speech-sdk.md)
- [Ses Yardımcısı örnek kodları](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da örneklere bakın](https://aka.ms/speech/cc-samples)