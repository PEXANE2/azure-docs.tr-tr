---
title: Konuşma Cihazları SDK’sını edinme
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti çok çeşitli aygıtlar ve ses kaynaklarıyla çalışır. Şimdi, eşleşen donanım ve yazılım ile konuşma uygulamalarınızı bir sonraki seviyeye taşıyabilirsiniz. Bu makalede, Konuşma Aygıtları SDK'ya nasıl erişeceğinizi ve geliştirmeye nasıl başlayacağınızı öğreneceksiniz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/14/2019
ms.author: erhopf
ms.openlocfilehash: 756ec976c4643c1cd80552b7fa552b70fd1f5d24
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81391265"
---
# <a name="get-the-cognitive-services-speech-devices-sdk"></a>Bilişsel Hizmetler Konuşma Cihazları SDK alın

Konuşma Aygıtları SDK, amaca yönelik geliştirme kitleri ve çeşitli mikrofon dizisi yapılandırmalarıyla çalışmak üzere tasarlanmış önceden ayarlanmış bir kitaplıktır.

## <a name="choose-a-development-kit"></a>Geliştirme kiti seçin

|Cihazlar|Belirtim|Açıklama|Senaryolar|
|--|--|--|--|
|[URbetter T11 Geliştirme Kurulu](https://www.alibaba.com/product-detail/URBETTER-Dev-Kits-for-Microsoft-Speech_62358746402.html)![URbetter DDK](media/speech-devices-sdk/device-urbetter.jpg)|7 Mikrofon Dizisi, ARM SOC, WIFI, Ethernet, HDMI, USB Kamera. <br>Linux|Microsoft Mic dizilerini uyarlayan ve HDMI/Ethernet ve daha fazla USB çevre birimi gibi genişletilmiş G/Ç'yi destekleyen endüstri düzeyinde konuşma aygıtları SDK|Konuşma Transkripsiyon, Eğitim, Hastane, Robotlar, OTT Kutusu, Ses Ajan, Sürücü Thru|
|[Roobo Akıllı Ses Dev Kiti](https://ddk.roobo.com)<br>[Kurulum](speech-devices-sdk-roobo-v1.md) / [Quickstart](speech-devices-sdk-android-quickstart.md)![Roobo Akıllı Ses Dev Kiti](media/speech-devices-sdk/device-roobo-v1.jpg)|7 Mikrofon Dizisi, ARM SOC, WIFI, Ses Çıkış, IO. <br>[Android](speech-devices-sdk-android-quickstart.md)|Yüksek kaliteli transkripsiyon ve konuşma senaryoları geliştirmek için Microsoft Mic Array ve ön işleme SDK uyarlamak için ilk Konuşma Cihazları SDK|Konuşma Transkripsiyon, Akıllı Hoparlör, Ses Agent, Giyilebilir|
|[Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/)<br>[Kurulum](https://docs.microsoft.com/azure/Kinect-dk/set-up-azure-kinect-dk) / [Quickstart](speech-devices-sdk-windows-quickstart.md)![Azure Kinect DK](media/speech-devices-sdk/device-azure-kinect-dk.jpg)|7 Mic Array RGB ve Derinlik kameraları. <br>[Windows](speech-devices-sdk-windows-quickstart.md)/[Linux](speech-devices-sdk-linux-quickstart.md)|Gelişmiş bilgisayar görüşü ve konuşma modelleri oluşturmak için gelişmiş yapay zeka (AI) sensörlü bir geliştirici kiti. Sınıfının en iyisi uzamsal mikrofon dizisini ve derinlik kamerasını bir video kamera ve yönlendirme sensörüyle birleştirir ve hepsi birden fazla mod, seçenek ve SDK'ya sahip küçük bir cihazda çeşitli bilgi işlem türlerine uyum sağlar.|Konuşma Transkripsiyon, Robotik, Akıllı Bina|
|Roobo Akıllı Ses Dev Kit 2<br>[Kurulum](speech-devices-sdk-roobo-v2.md)<br>![Roobo Akıllı Ses Dev Kit 2](media/speech-devices-sdk/device-roobo-v2.jpg)|7 Mikrofon Dizisi, ARM SOC, WIFI, Bluetooth, IO. <br>Linux|2. nesil Konuşma Cihazları SDK, uygun maliyetli referans tasarımında alternatif işletim sistemi ve daha fazla özellik sağlar.|Konuşma Transkripsiyon, Akıllı Hoparlör, Ses Agent, Giyilebilir|


## <a name="download-the-speech-devices-sdk"></a>Konuşma Cihazları SDK indirin

Konuşma [Cihazları SDK'yı](https://aka.ms/sdsdk-download)indirin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma Cihazları SDK ile başlayın](https://aka.ms/sdsdk-quickstart)
