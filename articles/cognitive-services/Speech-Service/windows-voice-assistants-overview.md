---
title: Windows üzerinde sesli yardımcılar-genel bakış
titleSuffix: Azure Cognitive Services
description: Kullanılabilir yetenekler ve geliştirme kaynakları dahil olmak üzere Windows 'daki sesli yardımcılar hakkında genel bakış.
services: cognitive-services
author: cfogg6
manager: trrwilson
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: travisw
ms.openlocfilehash: fbcb262fee6a2cc62bfe64e8a8589c92b4fe2b17
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82997452"
---
# <a name="voice-assistants-on-windows"></a>Windows üzerinde sesli yardımcılar

Windows 10 sürüm 2004 ve üzerinde, sesli yardım uygulamaları, tam bir ses özellikli yardımcı deneyim elde etmek için Windows Konuşma API 'Lerinden yararlanabilir.

## <a name="voice-assistant-features"></a>Ses Yardımcısı özellikleri

Ses Aracısı uygulamaları, sorunsuz, sesli kullanımlı bir deneyim sağlamak için konuşulan bir anahtar sözcükle etkinleştirilebilir. Ses etkinleştirme, uygulama kapatıldığında ve ekran kilitlendiğinde işe yarar.

Ayrıca, Windows, kullanıcıların sesli etkinleştirmeye ve kilit etkinleştirmenin üzerine uygulama başına temelinde denetimini sağlayan bir ses etkinleştirme gizlilik ayarları kümesi sağlar.

Ses etkinleştirildikten sonra, Windows birden çok etkin aracıyı düzgün bir şekilde yönetir ve kesintiye uğrarsa veya devre dışı bırakılmışsa her sesli yardımcıyı bilgilendirir. Bu, uygulamaların kesintileri ve diğer aracıları arası olayları doğru şekilde yönetmesine olanak tanır.

## <a name="how-does-voice-activation-work"></a>Ses etkinleştirme nasıl çalışır?

Aracı etkinleştirme çalışma zamanı (AAR), Windows 'da, konuşulan bir anahtar sözcük veya düğmeye basma üzerinde uygulama etkinleştirmeyi yöneten devam eden bir işlemdir. Sistemde kayıtlı en az bir uygulama olduğu sürece Windows ile başlar. Uygulamalar, Windows SDK içindeki konuşma API 'Leri aracılığıyla AAR ile etkileşim kurar.

Kullanıcı bir anahtar sözcüğü kulakça, sistem üzerindeki yazılım veya donanım anahtar sözcük spour, bir anahtar sözcük KIMLIĞI sağlayarak bir anahtar sözcüğünün algılandığını bildirir. AAR, bir BackgroundService 'e karşılık gelen uygulama KIMLIĞIYLE uygulamayı başlatmak için bir istek gönderir.

### <a name="registration"></a>Kayıt

Bir sesi etkinleştirilmiş bir uygulama ilk kez çalıştırıldığında, konuşma API 'Leri aracılığıyla uygulama KIMLIĞI ve anahtar sözcük bilgilerini kaydeder. AAR, genel eşlemede tüm konfigürasyonları, sistemdeki donanım veya yazılım anahtar sözcük kasalarıyla kaydederek uygulamanın anahtar sözcüğünü algılamasına izin verir. Uygulama, [arka plan hizmeti ile de kaydedilir](https://docs.microsoft.com/windows/uwp/launch-resume/register-a-background-task).

Bu, bir uygulamanın bir kez çalıştırılıncaya ve kaydın tamamlanmasını izin verildiğinde, bir uygulamanın sesle etkinleştirimeyeceği anlamına gelir.

### <a name="receiving-an-activation"></a>Bir etkinleştirme alma

AAR 'ten istek alındığında, arka plan hizmeti uygulamayı başlatır. Uygulama, ' de `App.xaml.cs` benzersiz bir olay bağımsız değişkeniyle birlikte OnBackgroundActivated yaşam döngüsü yöntemi aracılığıyla bir sinyal alır. Bu bağımsız değişken, uygulamaya AAR tarafından etkinleştirildiğini ve anahtar sözcük doğrulamayı başlatması gerektiğini söyler.

Uygulama anahtar sözcüğünü başarıyla doğrularsa, ön planda gösterilmesi bir istek yapabilir. Bu istek başarılı olduğunda, uygulama Kullanıcı ARABIRIMINI görüntüler ve kullanıcıyla etkileşime devam eder.

AAR, anahtar sözcükleri konuşulan etkin uygulamalara hala bildirir. İçindeki `App.xaml.cs`yaşam döngüsü yöntemi boyunca işaret etmek yerine, konuşma API 'lerinde bir olay aracılığıyla işaret eder.

### <a name="keyword-verification"></a>Anahtar sözcük doğrulama

Uygulamanın başlamasını tetikleyen anahtar sözcük, anahtar sözcük modelini basitleştirecek düşük güç tüketimine ulaştı. Bu, anahtar sözcük spour 'in yüksek güç etkisi olmadan "Always On" olmasına olanak sağlar, ancak aynı zamanda anahtar sözcük, hiçbir anahtar kelimeden bir anahtar sözcük algıladığı halde "yanlış kabul eder" anlamına gelir. Bu nedenle, ses etkinleştirme sisteminin uygulamayı arka planda başlattığında: uygulamanın, kullanıcının geçerli oturumunu kesmeden önce anahtar sözcüğünün konuşulmasını doğrulama şansı vermek için. AAR, anahtar sözcüğünün biriktirmeden önce birkaç saniye sonra sesi kaydeder ve uygulamayı bu şekilde erişilebilir hale getirir. Uygulama bunu, aynı ses üzerinde daha güvenilir bir anahtar sözcük spour çalıştırmak için kullanabilir.

## <a name="next-steps"></a>Sonraki adımlar

- **Tasarım yönergelerini gözden geçirin:** [Tasarım kılavuzlarımız](windows-voice-assistants-best-practices.md) , Windows 10 ' da ses etkinleştirmeye yönelik en iyi deneyimler sağlamak için gereken önemli işi temel alır.
- **Başlarken sayfasını ziyaret edin:** Windows 'da sesli yardımcılar uygulamaya başlama adımları için, Uygulama Kılavuzu ' na giriş aracılığıyla geliştirme ortamınızı ayarlamayı [kullanmaya başlayın.](how-to-windows-voice-assistants-get-started.md)
- **Örnek uygulamayı deneyin**: Bu özellikleri Ağızdan yaşamak Için [UWP Voice Yardımcısı örnek](windows-voice-assistants-faq.md#the-uwp-voice-assistant-sample) sayfasını ziyaret edin ve örnek istemciyi çalışır şekilde çalıştırmak için adımları izleyin.
