---
title: Project Acoustics eklentisi ile ilgili bilinen sorunlar
titlesuffix: Azure Cognitive Services
description: Project Acoustics için tasarımcı önizlemesi kullanırken aşağıdaki bilinen sorunlarla karşılaşabilirsiniz.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: a70d9ea1090f1c518f804dd28f3461918af965cd
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854276"
---
# <a name="project-acoustics-known-issues"></a>Project Acoustics bilinen sorunlar
Project Acoustics için tasarımcı önizlemesi kullanırken aşağıdaki bilinen sorunlarla karşılaşabilirsiniz.

## <a name="acoustic-parameters-are-lost-when-you-rename-a-scene"></a>Bir sahneyi yeniden adlandırdığınızda Akustik parametreler kayboluyor

Bir sahneyi yeniden adlandırırsanız, söz konusu sahneye ait olan tüm Akustik parametreler yeni sahneye otomatik olarak aktarılmaz. Ancak, hala eski varlık dosyasında mevcut olacaktır. Sahne dosyanızın yanındaki **Düzenleyici** dizini içindeki **SceneName_AcousticParameters. varlık** dosyasını bulun. Dosyanızı yeni sahnenin adını yansıtacak şekilde yeniden adlandırın.

## <a name="unity-crashes-when-closing-project"></a>Proje kapatılırken Unity kilitleniyor

Unity 'nin en son sürümlerinde (2018.2 +), projenizi kapattığınızda Unity 'nin çökmesine neden olan bilinen bir hata vardır. Bu, [Bu Unity sorunu](https://issuetracker.unity3d.com/issues/crash-on-assetdatabase-getassetimporterversions-when-closing-a-specific-unity-project)tarafından izlenir.

## <a name="deploying-to-android-from-some-unity-versions"></a>Bazı Unity sürümlerinden Android 'e dağıtım

Unity 'nin bazı sürümlerinde ses eklentilerini Android 'e dağıtmaya yönelik bir hata vardır. [Bu hatadan](https://issuetracker.unity3d.com/issues/android-ios-audiosource-playing-through-google-resonance-audio-sdk-with-spatializer-enabled-does-not-play-on-built-player)etkilenen bir sürüm kullanmadığınız emin olun.

## <a name="i-get-an-error-that-could-not-find-metadata-file-systemsecuritydll"></a>' Meta veri dosyası System. Security. dll bulunamadı ' hatasıyla ilgili bir hata alıyorum

Player ayarları 'ndaki Scripting Runtime sürümünün **.NET 4. x eşdeğeri**olarak ayarlandığından emin olun ve Unity 'yi yeniden başlatın.

## <a name="im-having-authentication-problems-when-connecting-to-azure"></a>Azure 'a bağlanırken kimlik doğrulama sorunlarıyla karşılaştım

Azure hesabınız için doğru kimlik bilgilerini kullandığınızı, hesabınızın Bake içinde istenen düğüm türünü desteklediğini ve sistem saatinizin doğru olduğunu denetleyin.

## <a name="canceling-a-bake-leaves-the-bake-tab-in-deleting-state"></a>Bir fırt iptal edildiğinde, "silme" durumundaki hazırlama sekmesi kalır
Project Acoustics, başarıyla tamamlandığında veya iptaldeki bir iş için tüm Azure kaynaklarını temizler. Bu, 5 dakikaya kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Unity](unity-quickstart.md) veya [gerçek olmayan](unreal-quickstart.md) örnek içeriğini deneyin

