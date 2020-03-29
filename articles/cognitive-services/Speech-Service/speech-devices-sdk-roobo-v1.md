---
title: Konuşma Cihazları SDK Roobo Smart Ses Dev Kit v1 - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma Cihazları SDK, Roobo Smart Audio Dev Kit v1 ile başlamak için ön koşullar ve talimatlar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 9add5b063b67ddcc4cd5bf93e7f5b570b004e5ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74815583"
---
# <a name="device-roobo-smart-audio-dev-kit"></a>Cihaz: Roobo Smart Ses Dev Kiti

Bu makalede Roobo Smart Audio Dev Kit için cihaza özel bilgiler verilmektedir.

## <a name="set-up-the-development-kit"></a>Geliştirme kitini ayarlama

1. Geliştirme kitinde iki mikro USB konektörü vardır. Sol konektör geliştirme kitigüç ve aşağıdaki resimde Güç olarak vurgulanır. Doğru olanı kontrol etmektir ve resimde Hata Ayıklama olarak işaretlenir.

    ![Dev kitini bağlama](media/speech-devices-sdk/qsg-1.png)

1. Güç bağlantı noktasını bir PC'ye veya güç adaptörüne bağlamak için bir mikro USB kablosu kullanarak geliştirme kitini güçe bağlayın. Yeşil güç göstergesi üst tahtanın altında yanar.

1. Geliştirme kitini denetlemek için hata ayıklama bağlantı noktasını ikinci bir mikro USB kablosu kullanarak bilgisayara bağlayın. Güvenilir iletişim sağlamak için yüksek kaliteli bir kablo kullanmak esastır.

1. Geliştirme kitinizi dairesel veya doğrusal yapılandırma için yönlendirin.

    |Geliştirme kiti yapılandırması|Yön|
    |-----------------------------|------------|
    |Dairesel|Dik, tavana bakan mikrofonlar ile|
    |Doğrusal|Yan tarafında, mikrofonlar size dönük (aşağıdaki resimde gösterilmiştir)|

    ![Lineer dev kiti yönlendirmesi](media/speech-devices-sdk/qsg-2.png)

1. Sertifikaları yükleyin ve ses aygıtının izinlerini ayarlayın. Komut İstemi penceresinde aşağıdaki komutları yazın:

   ```powershell
   adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/
   adb shell
   cd /data/
   chmod 777 roobo_setup.sh
   ./roobo_setup.sh
   exit
   ```

    > [!NOTE]
    > Bu komutlar Android `adb.exe`Studio yüklemebir parçası olan Android Hata Ayıklama Köprüsü kullanın. Bu araç C:\Users\[kullanıcı adı]\AppData\Local\Android\Sdk\platform-tools adresinde bulunur. Bu dizini, çağırmak `adb`için daha uygun hale getirmek için yolunuza ekleyebilirsiniz. Aksi takdirde, çağıran her komutta adb.exe yüklemeniziçin tam `adb`yolu belirtmeniz gerekir.
    >
    > Bir hata `no devices/emulators found` görürseniz, USB kablonuzun bağlı olup olmadığını kontrol edin ve yüksek kaliteli bir kablodur. Bilgisayarınızın `adb devices` bir aygıt listesini döndüreceği için geliştirme kitiyle konuşup konuşamamasını denetlemek için kullanabilirsiniz.
    >
    > [!TIP]
    > Geliştirme kitinin mikrofonlarıyla çalıştığından emin olmak için bilgisayarınızın mikrofonunu ve hoparlörünü sessize alın. Bu şekilde, aygıtı bilgisayardan gelen ses ile yanlışlıkla tetiklemezsiniz.

1. Dev kitine bir hoparlör takmak istiyorsanız, ses hattına bağlayabilirsiniz. 3.5mm analog fiş ile kaliteli bir hoparlör seçmelisiniz.

    ![Vysor ses](media/speech-devices-sdk/qsg-14.png)

## <a name="development-information"></a>Geliştirme bilgileri

Daha fazla geliştirme bilgisi için [Roobo geliştirme kılavuzuna](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf)bakın.

## <a name="audio"></a>Ses

Roobo flash belleğe tüm ses yakalar bir araç sağlar. Ses sorunlarını gidermenize yardımcı olabilir. Aracın bir sürümü her geliştirme kiti yapılandırması için sağlanır. [Roobo sitesinde](https://ddk.roobo.com/)cihazınızı seçin ve sayfanın altındaki **Roobo Tools** bağlantısını seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Android örnek uygulamasını çalıştırın](speech-devices-sdk-android-quickstart.md)
