---
title: Konuşma Cihazları SDK mikrofon dizi önerileri
titleSuffix: Azure Cognitive Services
description: Konuşma Cihazları SDK mikrofon dizisi önerileri. Bu dizi geometrileri Microsoft Ses Yığını ile kullanım için önerilir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: erhopf
ms.openlocfilehash: a87bdd7a55036e8b70f0bc5816d2b587c1569202
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77168133"
---
# <a name="speech-devices-sdk-microphone-array-recommendations"></a>Konuşma Cihazları SDK Mikrofon dizi önerileri

Bu makalede, Konuşma Aygıtları SDK için bir mikrofon dizisi tasarlamayı öğrenirsiniz.

Konuşma Aygıtları SDK, mikrofon geometrisi ve bileşen seçimi de dahil olmak üzere aşağıdaki yönergelere göre tasarlanmış bir mikrofon dizisiyle en iyi şekilde çalışır. Rehberlik de entegrasyon ve elektrik hususlar ı verilir.

## <a name="microphone-geometry"></a>Mikrofon geometrisi

Aşağıdaki dizi geometrileri Microsoft Ses Yığını ile kullanmak için önerilir. Ses kaynaklarının konumu ve ortam gürültüsünün reddi, belirli uygulamalara, kullanıcı senaryolarına ve aygıt formu faktörüne bağımlı olan daha fazla sayıda mikrofonla geliştirilir.

|     | Dairesel Dizi |     | Doğrusal Dizi |     |
| --- | -------------- | --- | ------------ | --- |
|     | <img src="media/speech-devices-sdk/7-mic-c.png" alt="7 mic circular array" width="150"/> | <img src="media/speech-devices-sdk/4-mic-c.png" alt="4 mic circular array" width="150"/> | <img src="media/speech-devices-sdk/4-mic-l.png" alt="4 mic linear array" width="150"/> | <img src="media/speech-devices-sdk/2-mic-l.png" alt="2 mic linear array" width="150"/> |
| \#Mikrofon | 7 | 4 | 4 | 2 |
| Geometri | 6 Dış, 1 Merkez, Yarıçap = 42,5 mm, Eşit Aralıklı | 3 Dış, 1 Merkez, Yarıçap = 42,5 mm, Eşit Aralıklı | Uzunluk = 120 mm, Aralık = 40 mm | Aralık = 40 mm |

Mikrofon kanalları, yukarıdaki her dizi için gösterilen numaralandırmaya göre, 0'dan artan şekilde sıralanmalıdır. Microsoft Ses Yığını, yankı iptali gerçekleştirmek için ek bir ses oynatma akışı gerektirir.

## <a name="component-selection"></a>Bileşen seçimi

Mikrofon bileşenleri, gürültü ve bozulma içermeyen bir sinyali doğru bir şekilde çoğaltmak için seçilmelidir.

Mikrofon seçerken önerilen özellikler şunlardır:

| Parametre | Önerilen |
| --------- | ----------- |
| Snr | \>= 65 dB (1 kHz sinyal 94 dBSPL, A ağırlıklı gürültü) |
| Genlik Eşleştirme | ± 1 dB @ 1 kHz |
| Faz Eşleştirme | ± 2° @ 1 kHz |
| Akustik Aşırı Yük Noktası (AOP) | \>= 120 dBSPL (THD = %10) |
| Bit Hızı | En az 24 bit |
| Örnekleme Oranı | En az 16 kHz\* |
| Frekans Yanıtı | ± 3 dB, 200-8000 Hz Yüzer Maske\* |
| Güvenilirlik | Depolama Sıcaklık Aralığı -40°C - 70°C<br />Çalışma Sıcaklığı Aralığı -20°C ile 55°C arası |

\*_Yüksek kaliteli iletişim (VoIP) uygulamaları için daha yüksek örnekleme oranları veya "daha geniş" frekans aralıkları gerekebilir_

Kullanılan bileşenlerin performansını bozmamak için iyi bileşen seçimi iyi elektroakustik entegrasyon ile eşleştirilmiş olmalıdır. Benzersiz kullanım örnekleri de ek gereksinimler (örneğin: çalışma sıcaklığı aralıkları) gerektirebilir.

## <a name="microphone-array-integration"></a>Mikrofon dizi entegrasyonu

Bir aygıta entegre edildiğinde mikrofon dizisinin performansı bileşen belirtiminden farklıdır. Mikrofonların entegrasyondan sonra iyi bir şekilde eşleştirilmesini sağlamak önemlidir. Bu nedenle, herhangi bir sabit kazanç veya EQ'dan sonra ölçülen cihaz performansı aşağıdaki önerileri karşılamalıdır:

| Parametre          | Önerilen                                        |
| ------------------ | -------------------------------------------------- |
| Snr                | \>63 dB (1 kHz sinyal 94 dBSPL, A ağırlıklı gürültü) |
| Çıkış Hassasiyeti | -26 dBFS/Pa @ 1 kHz (tavsiye edilir)                  |
| Genlik Eşleştirme | ± 2 dB, 200-8000 Hz                                |
| THD%\*             | ≤ %1, 200-8000 Hz, 94 dBSPL, 5.             |
| Frekans Yanıtı | ± 6 dB, 200-8000 Hz Yüzer Maske\*\*              |

\*\*_THD'yi ölçmek için düşük distorsiyon hoparlörü gereklidir (örn. Neumann KH120)_

\*\*_Yüksek kaliteli iletişim (VoIP) uygulamaları için "daha geniş" frekans aralıkları gerekli olabilir_

## <a name="speaker-integration-recommendations"></a>Hoparlör tümleştirme önerileri

Hoparlör içeren konuşma tanıma cihazları için yankı iptali gerekli olduğundan, hoparlör seçimi ve entegrasyonu için ek öneriler sağlanır.

| Parametre | Önerilen |
| --------- | ----------- |
| Doğrusallık Hususlar | Hoparlör referansı sonrası doğrusal olmayan işlem yapılmaz, aksi takdirde donanım tabanlı bir geri dönüş başvuru akışı gereklidir |
| Hoparlör Loopback | WASAPI, özel API'ler, özel ALSA eklentisi (Linux) veya firmware kanalı üzerinden sağlanan |
| THD% | 3. Oktav Bantlar minimum 5 Sipariş, 70 dBA Oynatma @ 0.8 m ≤ 6.3%, 315-500 Hz ≤ 5%, 630-5000 Hz |
| Mikrofonlara Yankı Bağlama | \>-10 dB TCLw kullanarak ITU-T G.122 Ek B.4 yöntemi, mikrofon seviyesine normalleştirilmiş<br />TCLw = TCLwmeasured \+ (Ölçülen Seviye - Hedef Çıkış Hassasiyeti)<br />TCLw = TCLwmeasured \+ (Ölçülen Seviye - (-26)) |

## <a name="integration-design-architecture"></a>Entegrasyon tasarım mimarisi

Mikrofonları bir aygıta tümleştirirken mimari için aşağıdaki yönergeler gereklidir:

| Parametre | Öneri |
| --------- | -------------- |
| Mikrofon Bağlantı Noktası Benzerliği | Tüm mikrofon bağlantı noktaları dizi içinde aynı uzunluktadır |
| Mikrofon Bağlantı Noktası Boyutları | Bağlantı noktası boyutu Ø0.8-1.0 mm. Bağlantı Noktası Uzunluğu \< / Bağlantı Noktası Çapı 2 |
| Mikrofon Sızdırmazlık         | Contalar düzgün bir şekilde yığın halinde uygulanmaktadır. Köpük \> contalar için %70 sıkıştırma oranı öner |
| Mic Güvenilirlik     | Hasır toz ve girişi önlemek için kullanılmalıdır (alt taşımalı mikrofonlar için PCB ile conta/üst kapak sızdırmazlık arasında) |
| Mikrofon İzolasyon       | Kauçuk contalar ve titreşim ayrıştırma yapısı, özellikle entegre hoparlörler nedeniyle herhangi bir titreşim yolları izole etmek için |
| Örnekleme Saati      | Cihaz sesi, düşük sürüklenme ile gerginlik ve bırakma ücretsiz olmalıdır |
| Kayıt Yeteneği   | Cihaz, tek tek kanal ham akışlarını aynı anda kaydedebilmelidir |
| USB                 | Tüm USB ses giriş [aygıtları, USB Ses Aygıtları Rev3 Spec'e](https://www.usb.org/document-library/usb-audio-devices-rev-30-and-adopters-agreement) göre tanımlayıcılar ayarlamalıdır |
| Mikrofon Geometrisi | Sürücüler [Mikrofon Dizisi Geometri Tanımlayıcılarını](https://docs.microsoft.com/windows-hardware/drivers/audio/ksproperty-audio-mic-array-geometry) doğru uygulamalı |
| Keşfedilebilirlik     | Aygıtlarda keşfedilemeyen veya denetlenebilen donanım, firmware veya üçüncü taraf yazılım tabanlı doğrusal olmayan ses işleme algoritmaları veya cihazdan |
| Yakalama Biçimi      | Yakalama biçimleri en az 16 kHz örnekleme hızı kullanmalı ve önerilen 24 bit derinlik |

## <a name="electrical-architecture-considerations"></a>Elektrik mimarisi ile ilgili hususlar

Uygun olduğu durumlarda, diziler bir USB ana bilgisayara (Microsoft Ses Yığınını çalıştıran Bir SoC gibi) ve Konuşma hizmetlerine veya diğer uygulamalara arabirimlere bağlanabilir.

PDM'den TDM dönüştürmeye gibi donanım bileşenleri, mikrofonların dinamik aralığının ve SNR'sinin yeniden örnekleyiciler içinde korunmasını sağlamalıdır.

Yüksek hızlı USB Audio Class 2.0, daha yüksek numune hızlarında ve bit derinliklerinde yedi kanala kadar gerekli bant genişliğini sağlamak için herhangi bir ses McUs'u içinde desteklenmelidir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma cihazları SDK hakkında daha fazla bilgi edinin](speech-devices-sdk.md)
