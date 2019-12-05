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
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74815571"
---
# <a name="troubleshoot-the-speech-devices-sdk"></a>Konuşma Cihazları SDK’sında sorun giderme

Bu makalede, konuşma cihazları SDK 'sını kullandığınızda karşılaşabileceğiniz sorunları çözmenize yardımcı olacak bilgiler sağlanmaktadır.

## <a name="certificate-failures"></a>Sertifika hataları

Konuşma hizmetini kullanırken sertifika hatalarıyla karşılaşırsanız, cihazınızın doğru tarih ve saate sahip olduğundan emin olun:

1. Git **ayarları**. Altında **sistem**seçin **tarih ve saat**.

    ![Tarih ve saat ayarları altında seçin](media/speech-devices-sdk/qsg-12.png)

1. Tutun **otomatik tarih ve saat** seçeneği belirlenmiş. Altında **Select saat dilimi**, geçerli saat diliminizde seçin.

    ![Tarih ve saat dilimi seçenekleri belirleyin](media/speech-devices-sdk/qsg-13.png)

    Dev Seti'nın zaman zaman bilgisayarınızda eşleştiğini gördüğünüzde, dev Seti internet'e bağlı.

## <a name="next-steps"></a>Sonraki adımlar

* [Sürüm notlarını gözden geçirin](devices-sdk-release-notes.md)
