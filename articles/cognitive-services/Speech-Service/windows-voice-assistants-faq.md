---
title: Windows 'da sesli yardımcılar-SSS
titleSuffix: Azure Cognitive Services
description: Windows Voice Agent geliştirme sırasında sıklıkla ortaya gelmiş olan genel sorular.
services: cognitive-services
author: cfogg6
manager: trrwilson
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: travisw
ms.openlocfilehash: 3cb4dd16592f3af962b32fe3005d2ad4b241d831
ms.sourcegitcommit: ba8df8424d73c8c4ac43602678dae4273af8b336
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84457023"
---
# <a name="samples-and-faqs"></a>Örnekler ve SSS

## <a name="the-uwp-voice-assistant-sample"></a>UWP ses Yardımcısı örneği

UWP ses Yardımcısı örneği, Windows 'da

- Windows Konuşma API 'Lerinin kullanımını gösterir
- Windows üzerinde bir ses Aracısı için en iyi uygulamaları görüntüleme
- MVP Aracısı uygulamanız için uyarlanabilir bir uygulama ve yeniden kullanılabilir bileşenler sağlama
- bir uçtan uca sesli aracı deneyimi için Windows Konuşma ve özel komutlarla birlikte doğrudan hat konuşmadan birlikte nasıl kullanılabilir

Örnek uygulamayla birlikte sunulan belgeler, doğru Temizleme ile başlatma ön koşullarını kullanarak, ses etkinleştirme ve aracı yönetiminin kod yolunu gösterir.

> [!div class="nextstepaction"]
> [UWP örneği için GitHub deposunu ziyaret edin](https://aka.ms/MVA/sample)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-do-i-contact-microsoft-for-resources-like-limited-access-feature-tokens-and-keyword-model-files"></a>Nasıl yaparım? sınırlı erişim özelliği belirteçleri ve anahtar sözcük modeli dosyaları gibi kaynaklar için Microsoft ile iletişim kurun mi?

winvoiceassistants@microsoft.comBu kaynakları istemek için iletişim kurun.

### <a name="my-app-is-showing-in-a-small-window-when-i-activate-it-by-voice-how-can-i-transition-from-the-compact-view-to-a-full-application-window"></a>Uygulamam, sesli olarak etkinleştirdiğimde küçük bir pencerede gösteriliyor. Kompakt görünümden tam uygulama penceresine nasıl geçiş yapabilirim?

Uygulamanız ses tarafından ilk kez etkinleştirildiğinde, kompakt bir görünümde başlatılır. Lütfen Windows 'daki sesli Yardımcılar için farklı görünümler ve bunlar arasında geçiş [yapmak üzere ses etkinleştirme önizlemesinin tasarım kılavuzunu](windows-voice-assistants-best-practices.md#design-guidance-for-voice-activation-preview) okuyun.

Kompakt görünümden geçişi tam uygulama görünümüne getirmek için appView API 'sini kullanın `TryEnterViewModeAsync` :

`var appView = ApplicationView.GetForCurrentView();
 await appView.TryEnterViewModeAsync(ApplicationViewMode.Default);`

### <a name="why-are-voice-assistant-features-on-windows-only-enabled-for-uwp-applications"></a>Windows 'da ses Yardımcısı özellikleri neden yalnızca UWP uygulamaları için etkinleştirilmiştir?

Ses etkinleştirme gibi özelliklerle ilişkili gizlilik riskleri göz önüne alındığında, UWP platformunun özellikleri Windows üzerinde ses Yardımcısı özelliklerinin yeterince güvenli olmasını sağlar.

### <a name="the-uwp-voice-assistant-sample-uses-direct-line-speech-do-i-have-to-use-direct-line-speech-for-my-voice-assistant-on-windows"></a>UWP ses Yardımcısı örneği doğrudan hat konuşmayı kullanır. Windows 'da sesli yardımım için doğrudan hat konuşmayı kullanmak zorunda mıyım?

UWP örnek uygulaması, Windows konuşma aracı özelliğiyle iletişim kutusu hizmetinin nasıl kullanılacağına ilişkin bir gösteri olarak doğrudan hat konuşma ve konuşma Hizmetleri SDK 'Sı kullanılarak geliştirilmiştir. Ancak, yerel ve bulut anahtar sözcüğü doğrulama, konuşmadan metne dönüştürme, bot iletişim kutusu ve metinden konuşmaya dönüştürme için herhangi bir hizmeti kullanabilirsiniz.