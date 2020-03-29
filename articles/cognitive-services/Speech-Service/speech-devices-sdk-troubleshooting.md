---
title: Konuşma Cihazları SDK Sorun Giderme - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Bu makalede, Konuşma Aygıtları SDK'yı kullandığınızda karşılaşabileceğiniz sorunları çözmenize yardımcı olacak bilgiler sağlanıyor.
services: cognitive-services
author: mswellsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: wellsi
ms.openlocfilehash: c590e0972de289a36890a75b220eddbded701ea8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74815571"
---
# <a name="troubleshoot-the-speech-devices-sdk"></a>Konuşma Cihazları SDK’sında sorun giderme

Bu makalede, Konuşma Aygıtları SDK'yı kullandığınızda karşılaşabileceğiniz sorunları çözmenize yardımcı olacak bilgiler sağlanıyor.

## <a name="certificate-failures"></a>Sertifika hataları

Konuşma hizmetini kullanırken sertifika hataları alırsanız, cihazınızın doğru tarih ve saate sahip olduğundan emin olun:

1. **Ayarlar'a**gidin. **Sistem**altında, **Tarih & saat**seçin.

    ![Ayarlar'ın altında Tarih & saat](media/speech-devices-sdk/qsg-12.png)

1. Otomatik **tarih & saat** seçeneğini seçili tutun. **Saat dilimini seç'in**altında, geçerli saat dilimini seçin.

    ![Tarih ve saat dilimi seçeneklerini seçin](media/speech-devices-sdk/qsg-13.png)

    Dev kitinin süresinin bilgisayarınızdaki saatle eşleştiğini gördüğünüzde, dev kiti internete bağlanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Sürüm notlarını gözden geçirme](devices-sdk-release-notes.md)
