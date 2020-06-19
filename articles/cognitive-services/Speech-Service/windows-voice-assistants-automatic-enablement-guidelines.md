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
ms.openlocfilehash: b9cbb873066131264732d6f46320461bae8c3188
ms.sourcegitcommit: 55b2bbbd47809b98c50709256885998af8b7d0c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84981770"
---
# <a name="privacy-guidelines-for-voice-assistants-on-windows"></a>Windows 'da sesli Yardımcılar için Gizlilik yönergeleri

Kullanıcılara, kendi ses verilerinin nasıl toplandığı ve kullanıldığı hakkında net bilgiler verilmesi ve bu koleksiyonun ne şekilde ve bu koleksiyonun nasıl gerçekleştikleri üzerinde denetim verildikleri önemli öneme sahiptir. Bu temel gizlilik modelleri-- *Açıklama* ve *onay* --özellikle, kullanımları her zaman dinleyerek Windows ile tümleştirilmiş sesli Yardımcılar için önemlidir.

Windows üzerinde sesli yardımcılar oluşturan geliştiriciler, yardımcısının dinleme yeteneklerini yansıtan uygulamaları içinde açık Kullanıcı arabirimi öğeleri içermelidir.

> [!NOTE]
> Uygulama güncelleştirmelerinden sonra da dahil olmak üzere bir yardımcı uygulama için uygun bir açıklama ve onay sağlamama hatası, gizlilik sorunları çözümlenene kadar bu yardımcının ses etkinleştirme için kullanılamaz hale gelmesine neden olabilir.

## <a name="minimum-requirements-for-feature-inclusion"></a>Özellik ekleme için minimum gereksinimler

Windows kullanıcıları içindeki yardımcı uygulamalarının kullanılabilirliğini görebilir ve denetleyebilir **`Settings > Privacy > Voice activation`** .

 > [!div class="mx-imgBorder"]
 > [![Gizlilik-uygulama-listeleme](media/voice-assistants/windows_voice_assistant/privacy-app-listing.png "Bir yardımcı uygulama için Windows Voice Activation gizlilik ayarı girdisi")](media/voice-assistants/windows_voice_assistant/privacy-app-listing.png#lightbox)

Bu listeye eklenmek üzere uygun hale getirmek için, winvoiceassistants@microsoft.com kullanmaya başlamak üzere Microsoft 'a başvurun. Varsayılan olarak, kullanıcıların ' de **`Settings > Privacy > Voice Activation`** bir uygulamanın protokol bağlantısı olan ' de yeni bir Yardımcısı için ses etkinleştirmeyi açıkça etkinleştirmesi gerekir `ms-settings:privacy-voiceactivation` . Çalışma alanına izin verilen bir uygulama, çalıştırma ve API 'leri kullandıktan sonra listede görüntülenir `Windows.ApplicationModel.ConversationalAgent` . Uygulama kullanıcıdan mikrofon onayı aldıktan sonra, ses etkinleştirme ayarları değiştirilebilir olacak.

Windows Gizlilik ayarları, ses etkinleştirmenin nasıl çalıştığı ve izin denetlemek için standart Kullanıcı arabirimine sahip olduğu ve izin denetimi için standart Kullanıcı arabirimine sahip olduğu için, her ikisinin de yerine Bu izin verilen listede olmadığı sürece yardımcı olur:

* Kullanıcının ses etkinleştirme veya yardımcı tarafından ses verileri işleme hakkında yanlış müşteri adayı veya bilgilendirme
* Etkilenmemesi başka bir yardımcı ile kesintiye uğramaz
* İlgili diğer tüm Microsoft ilkelerini bölün

Yukarıdakilerden herhangi biri bulunursa, Microsoft, sorunlar çözülene kadar izin verilen listeden bir yardımcıyı kaldırabilir.

> [!NOTE]
> Her durumda, sesli etkinleştirme izni mikrofon izni gerektirir. Bir yardımcı uygulamanın mikrofon erişimi yoksa, ses etkinleştirme için uygun olmayacaktır ve devre dışı durumundaki sesli etkinleştirme gizlilik ayarları 'nda görüntülenir.

## <a name="additional-requirements-for-inclusion-in-microphone-consent"></a>Mikrofon onayı ekleme için ek gereksinimler

Kullanıcıların ses etkinleştirmesine izin vermesini kolaylaştırmak ve daha sorunsuz hale getirmek isteyen yardım yazarları, ayarlar sayfasına fazladan bir yolculuğa gerek kalmadan açıklama ve onayı yeterince karşılamak üzere ek gereksinimler karşılaarak bunu yapabilir. Onaylandığında, Kullanıcı yardımcı uygulamasına mikrofon izni verdiğinde ses etkinleştirme hemen kullanılabilir hale gelir. Buna hak kazanmak için, bir yardımcı uygulamanın mikrofon onayı **sorulmadan önce** aşağıdakileri yapması gerekir (örneğin, API 'yi kullanarak `AppCapability.RequestAccessAsync` ):

1. Uygulamanın, *uygulamanın çalışmadığı*ve kullanıcının onayını beğenmesinin yanı sıra bir anahtar sözcük için kullanıcının sesini dinlemek istediğinizin açık ve belirgin bir gösterge sağlayın
1. Veri kullanımı ve gizlilik ilkeleri hakkında, resmi gizlilik bildirimine bağlantı gibi ilgili bilgileri ekleyin
1. Her türlü yönergeden veya önde gelen bir ifade kullanmaktan kaçının (örneğin, "aşağıdaki komut isteminde Evet 'e tıklayın") ses yakalama davranışını kapatan deneyim akışında

Bir uygulama yukarıdaki tüm koşulları yerine getirdiklerinde, mikrofon onayı ile ses etkinleştirme özelliğini etkinleştirmek uygun olur. winvoiceassistants@microsoft.comDaha fazla bilgi edinmek ve ilk kullanım deneyimini gözden geçirmek için iletişim kurun.

> [!NOTE]
> Kilit üzerinde ses etkinleştirme, mikrofon erişimiyle otomatik olarak etkinleştirilmeye uygun değildir ve yine de bir yardımcı için daha fazla kilit erişimini etkinleştirmek üzere bir kullanıcının ses etkinleştirme gizlilik sayfasını ziyaret etmesini gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Windows 'da sesli Yardımcılar için en iyi yöntemler hakkında bilgi edinin](windows-voice-assistants-best-practices.md)