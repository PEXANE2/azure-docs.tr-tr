---
title: Konuşma Cihazları SDK Roobo Smart Ses Dev Kit v2 - Konuşma Servisi
titleSuffix: Azure Cognitive Services
description: Konuşma Cihazları SDK, Roobo Smart Audio Dev Kit v2 ile başlamak için ön koşullar ve talimatlar.
services: cognitive-services
author: anushapatnala
manager: wellsi
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: v-anusp
ms.openlocfilehash: 2f325c80877068c53d690bd7ff74f768dab3a174
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80371587"
---
# <a name="device-roobo-smart-audio-dev-kit-v2"></a>Cihaz: Roobo Smart Ses Dev Kit v2

Bu makalede Roobo Smart Audio Dev Kit2 için cihaza özel bilgiler verilmektedir.

## <a name="set-up-the-development-kit"></a>Geliştirme kitini ayarlama

1. Geliştirme kitinde iki mikro USB konektörü vardır. Sol konektör geliştirme kitigüç ve aşağıdaki resimde Güç olarak vurgulanır. Doğru olanı kontrol etmektir ve resimde Hata Ayıklama olarak işaretlenir. 
    ![Dev kitini bağlama](media/speech-devices-sdk/roobo-v2-connections.png)
1. Güç bağlantı noktasını bir PC'ye veya güç adaptörüne bağlamak için bir mikro USB kablosu kullanarak geliştirme kitini güçe bağlayın. Yeşil güç göstergesi üst tahtanın altında yanar.
1. Geliştirme kitini denetlemek için hata ayıklama bağlantı noktasını ikinci bir mikro USB kablosu kullanarak bilgisayara bağlayın. Güvenilir iletişim sağlamak için yüksek kaliteli bir kablo kullanmak esastır.
1. Geliştirme kitinizi dairesel olarak yönlendirin - Yukarıda gösterildiği gibi tavana bakan mikrofonlarla dik


## <a name="development-information"></a>Geliştirme bilgileri

Daha fazla geliştirme bilgisi için [Roobo geliştirme kılavuzuna](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf)bakın.

## <a name="audio-recordplay"></a>Ses Kaydı/Oynatma

DDK2 ses işlemleri aşağıdaki şekilde gerçekleştirilebilir:
* ALSA Açık kaynak kitaplıklarını ve uygulamalarını kullanın.
* Uygulama geliştirme yapmak için appmainprog arabirimini kullanın. DDK2 ses - ilgili yazılım çerçevesi standart ALSA çerçeve kullanır, libasound kullanabilirsiniz. Yani doğrudan yazılım geliştirmek için. Böylece, doğrudan ses kaydetmek ve oynatmak için ALSA'nın kayıt ve kaydını kullanabilirsiniz.
