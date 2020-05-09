---
title: Windows 'da sesli Yardımcılar için Gizlilik yönergeleri
titleSuffix: Azure Cognitive Services
description: Ses etkinleştirmeyi varsayılan olarak bir ses Aracısı için etkinleştirme yönergeleri.
services: cognitive-services
author: cfogg6
manager: trrwilson
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: travisw
ms.openlocfilehash: 436367ede4f4be323b5334a201b1c9fb8f7f28e8
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82997518"
---
# <a name="privacy-guidelines-for-voice-assistants-on-windows"></a>Windows 'da sesli Yardımcılar için Gizlilik yönergeleri

Kullanıcılara, kendi ses verilerinin nasıl toplandığı ve kullanıldığı hakkında net bilgiler verilmesi ve bu koleksiyonun ne şekilde ve bu koleksiyonun nasıl gerçekleştikleri üzerinde denetim verildikleri önemli öneme sahiptir. Bu temel gizlilik modelleri-- *Açıklama* ve *onay* --özellikle, kullanımları her zaman dinleyerek Windows ile tümleştirilmiş sesli Yardımcılar için önemlidir.

Windows üzerinde sesli yardımcılar oluşturan geliştiriciler, yardımcısının dinleme yeteneklerini yansıtan uygulamaları içinde açık Kullanıcı arabirimi öğeleri içermelidir.

> [!NOTE]
> Uygulama güncelleştirmelerinden sonra da dahil olmak üzere bir yardımcı uygulama için uygun bir açıklama ve onay sağlamama hatası, gizlilik sorunları çözümlenene kadar bu yardımcının ses etkinleştirme için kullanılamaz hale gelmesine neden olabilir. 

## <a name="minimum-requirements-for-feature-inclusion"></a>Özellik ekleme için minimum gereksinimler

Windows kullanıcıları içindeki **`Settings > Privacy > Voice activation`** yardımcı uygulamalarının kullanılabilirliğini görebilir ve denetleyebilir.

 > [!div class="mx-imgBorder"]
 > [![Gizlilik-uygulama-listeleme](media/voice-assistants/windows_voice_assistant/privacy-app-listing.png "Bir yardımcı uygulama için Windows Voice Activation gizlilik ayarı girdisi")](media/voice-assistants/windows_voice_assistant/privacy-app-listing.png#lightbox)

Bu listeye eklenmek üzere uygun hale gelmesi için bir uygulamanın şunları yapmanız gerekir:

1. Belirgin bir şekilde, bir anahtar sözcüğü dinleyeceği ve uygulama çalışmadığı zaman ve bu anahtar sözcüğünün ne olduğu
1. Bir bağlantı veya ilgili gizlilik ilkelerine başvuru dahil olmak üzere kullanıcının ses verilerinin nasıl kullanılacağına ilişkin bir açıklama ekleyin
1. Kullanıcılara, tüm uygulama içi ayarlara ek olarak, ' de, isteğe bağlı olarak doğrudan erişim **`Settings > Privacy > Voice activation`** `ms-settings:privacy-voiceactivation` için bir protokol bağlantısı dahil olmak üzere, içindeki gizlilik seçimlerini görüntüleyip değiştiremeyeceğini bildirin

Bu gereksinimleri karşıladıktan ve Microsoft 'tan onay aldıktan sonra, `Windows.ApplicationModel.ConversationalAgent` API 'lere kaydolduktan sonra bir yardımcı uygulama, sesli etkinleştirme uygulamaları listesinde görünür ve kullanıcılar anahtar sözcüğü etkinleştirme için uygulamaya izin verebilir. Varsayılan olarak, bu ayarlardan `Off` her ikisi de kullanıcının etkinleştirilecek ayarlar sayfasını el ile ziyaret etmesini gerektirir.

> [!NOTE]
> Her durumda, sesli etkinleştirme izni mikrofon izni gerektirir. Bir yardımcı uygulamanın mikrofon erişimi yoksa, ses etkinleştirme için uygun olmayacaktır ve devre dışı durumundaki sesli etkinleştirme gizlilik ayarları 'nda görüntülenir.

## <a name="additional-requirements-for-inclusion-in-microphone-consent"></a>Mikrofon onayı ekleme için ek gereksinimler

Kullanıcıların ses etkinleştirmesine izin daha kolay ve daha sorunsuz hale getirmek isteyen yardım yazarları, yukarıda yer alan birkaç ek gereksinimi karşılaarak bunu yapabilir. Bu, bir yardımcı uygulamanın standart, cihaz kilidi açılmış ses etkinleştirme ayarı, uygulamaya bir kez (ve `On` yalnızca bir kez) için varsayılan olarak bir defa erişim verilir. Bu, bir yardımcıyı açmadan önce ayarlara daha fazla seyahat gereksinimini ortadan kaldırır.

Ek gereksinimler, bir yardımcı uygulamanın şunları sağlamalıdır:

1. Mikrofon onayı **sorulmadan önce** (örneğin, `AppCapability.RequestAccessAsync` API 'yi kullanarak), kullanıcıya, uygulama çalışmadığı zaman ve kullanıcının onayını beğense bile, yardımcı uygulamanın bir anahtar sözcük için bir kullanıcının seslerine dinlemek istediğlerine yönelik belirgin bir gösterge sağlayın
2. Mikrofon erişimi istenmeden veya `Windows.ApplicationModel.ConversationalAgent` API 'leri kullanmadan **önce** veri kullanımı ve gizlilik ilkelerine ilişkin tüm ilgili bilgileri ekleyin
3. Her türlü yönergeden veya önde gelen bir ifade kullanmaktan kaçının (örneğin, "aşağıdaki komut isteminde Evet 'e tıklayın"), deneyim akışında ses yakalama davranışını kapatma ve izin isteme

Bu gereksinimler karşılandıktan sonra uygun bir yardımcı uygulama, mikrofon erişimi verildiğinde bir `enabled` durumda ses etkinleştirmeye uygun uygulamalar listesinde görünür.

> [!NOTE]
> Kilit üzerinde ses etkinleştirme, mikrofon erişimiyle otomatik olarak etkinleştirilmeye uygun değildir ve yine de bir yardımcı için daha fazla kilit erişimini etkinleştirmek üzere bir kullanıcının ses etkinleştirme gizlilik sayfasını ziyaret etmesini gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Windows 'da sesli Yardımcılar için en iyi yöntemler hakkında bilgi edinin](windows-voice-assistants-best-practices.md)