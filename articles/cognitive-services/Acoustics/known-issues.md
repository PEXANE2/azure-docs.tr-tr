---
title: Project Acoustics eklentisi bilinen sorunlar
titlesuffix: Azure Cognitive Services
description: Project Acoustics'te aşağıdaki bilinen sorunları yaşayabilirsiniz.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: b71b93f271608d946d964f70dae9eefbef77e87b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72243033"
---
# <a name="project-acoustics-known-issues"></a>Project Acoustics bilinen sorunlar
Bu makalede, Project Acoustics'i kullandığınızda karşılaşabileceğiniz sorunlar açıklanmaktadır.

## <a name="acoustic-parameters-are-lost-when-you-rename-a-scene"></a>Bir sahneyi yeniden adlandırdığınızda akustik parametreler kaybolur

Bir sahneyi yeniden adlarsanız, o sahneye ait tüm akustik parametreler otomatik olarak yeni sahneye aktarılmayı zedebilirsiniz. Ama hala eski varlık dosyasında varlar. Sahne dosyanızın yanındaki *Editör* dizinindeki *[SceneName]_AcousticParameters.asset* dosyasını arayın. Dosyayı yeni sahne adını yansıtacak şekilde yeniden adlandırın.

## <a name="deploy-to-android-bug-from-some-unity-versions"></a>Bazı Unity sürümlerinden Android'e dağıt hatası

Unity'nin bazı sürümlerinde Android'e ses eklentileri dağıtma konusunda bir [hata](https://issuetracker.unity3d.com/issues/android-ios-audiosource-playing-through-google-resonance-audio-sdk-with-spatializer-enabled-does-not-play-on-built-player) vardır. Bu hatadan etkilenen bir sürümü kullanmadığınızdan emin olun.

## <a name="could-not-find-metadata-file-systemsecuritydll-error"></a>"meta veri dosyası System.Security.dll bulamadım" hatası

**Player** ayarlarındaki **Komut Dosyası Çalışma Zamanı Sürümünün** *.NET 4.x Eşdeğer*olduğundan emin olun ve Unity'yi yeniden başlatın.

## <a name="authentication-problems-when-connecting-to-azure"></a>Azure'a bağlanırken kimlik doğrulama sorunları

Şuna bakın:
- Azure hesabınız için doğru kimlik bilgilerini kullandınız.
- Hesabınız fırında istediğiniz düğüm türünü destekler.
- Sistem saatiniz doğru ayarlandı.

## <a name="the-bake-tab-still-shows-deleting-after-you-cancel"></a>Fırın sekmesi, iptal ettikten sonra da "silme"yi gösterir
Project Acoustics, başarılı bir şekilde tamamlanmasıveya iptal inden sonra bir iş için tüm Azure kaynaklarını temizler. Bu işlem 5 dakika kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Unity](unity-quickstart.md) veya [Unreal](unreal-quickstart.md) örnek içeriğini deneyin.
