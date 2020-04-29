---
title: Konuşma cihazları SDK-konuşma hizmeti sorunlarını giderme
titleSuffix: Azure Cognitive Services
description: Bu makalede, konuşma cihazları SDK 'sını kullandığınızda karşılaşabileceğiniz sorunları çözmenize yardımcı olacak bilgiler sağlanmaktadır.
services: cognitive-services
author: mswellsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: wellsi
ms.openlocfilehash: c590e0972de289a36890a75b220eddbded701ea8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74815571"
---
# <a name="troubleshoot-the-speech-devices-sdk"></a>Konuşma Cihazları SDK’sında sorun giderme

Bu makalede, konuşma cihazları SDK 'sını kullandığınızda karşılaşabileceğiniz sorunları çözmenize yardımcı olacak bilgiler sağlanmaktadır.

## <a name="certificate-failures"></a>Sertifika sorunları

Konuşma hizmetini kullanırken sertifika hatalarıyla karşılaşırsanız, cihazınızın doğru tarih ve saate sahip olduğundan emin olun:

1. **Ayarlar**' a gidin. **Sistem**altında **Tarih & saat**' i seçin.

    ![Ayarlar altında Tarih & saat ' i seçin](media/speech-devices-sdk/qsg-12.png)

1. **Otomatik tarih & saat** seçeneğini seçili tutun. **Saat dilimi Seç**altında geçerli saat diliminizi seçin.

    ![Tarih ve saat dilimi seçeneklerini belirleyin](media/speech-devices-sdk/qsg-13.png)

    Geliştirme Seti zamanının, BILGISAYARıNıZDAKI saat ile eşleştiğini gördüğünüzde, geliştirme seti internet 'e bağlanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Sürüm notlarını gözden geçirin](devices-sdk-release-notes.md)
