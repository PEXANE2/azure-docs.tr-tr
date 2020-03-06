---
title: Konuşma cihazları SDK Roobo akıllı ses geliştirme seti v2-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma cihazları SDK 'sını kullanmaya başlama önkoşulları ve yönergeler, Roobo akıllı ses geliştirme seti v2.
services: cognitive-services
author: anushapatnala
manager: wellsi
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: v-anusp
ms.openlocfilehash: 5cf851bc9333004c0e14713cde44f470fb8c0c02
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78304291"
---
# <a name="device-roobo-smart-audio-dev-kit-v2"></a>Cihaz: Roobo Smart Audio Dev Kit V2

Bu makalede, Roobo Smart Audio dev Kit2 için cihaza özgü bilgiler sağlanmaktadır.

## <a name="set-up-the-development-kit"></a>Geliştirme Seti ' ayarlayın

1. Geliştirme setinde iki mikro USB bağlayıcısı vardır. Sol bağlayıcı, geliştirme setini güçlendirin ve aşağıdaki görüntüde güç olarak vurgulanır. Bu, doğru bir denetimdir ve görüntüde hata ayıklama olarak işaretlenir. 
    Geliştirme Seti](media/speech-devices-sdk/roobo-v2-connections.png) bağlama ![
1. Güç bağlantı noktasını bir BILGISAYARA veya güç bağdaştırıcısına bağlamak için mikro USB kablosu kullanarak geliştirme setini güçlendirin. Üst panonun altında yeşil bir güç göstergesi açılır.
1. Geliştirme setini denetlemek için, ikinci bir mikro USB kablosu kullanarak hata ayıklama bağlantı noktasını bir bilgisayara bağlayın. Güvenilir iletişimler sağlamak için yüksek kaliteli bir kablonun kullanılması önemlidir.
1. Yukarıda gösterildiği gibi, üst düzey mikrofonlarla, geliştirme setinizi döngüye alarak hemen yola ayırın


## <a name="development-information"></a>Geliştirme bilgileri

Daha fazla geliştirme bilgisi için, [Roobo geliştirme kılavuzuna](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf)bakın.

## <a name="audio-recordplay"></a>Ses kaydı/yürütme

DDK2 ses işlemleri aşağıdaki yollarla gerçekleştirilebilir:
* ALSA açık kaynak kitaplıklarını ve bunların uygulamalarını kullanın.
* Uygulama geliştirmeyi yapmak için appmainprog arabirimini kullanın. DDK2 Audio ile ilgili yazılım çerçevesi standart ALSA çerçevesini kullanır, libasound kullanabilirsiniz. Bu nedenle doğrudan yazılım geliştirme. Bu nedenle, ses kaydetmek ve oynatmak için ALSA 'nın arecord ve aplay 'ı doğrudan kullanabilirsiniz.
