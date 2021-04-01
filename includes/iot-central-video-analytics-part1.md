---
title: include dosyası
description: include dosyası
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 10/06/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 2f3e4bf640b8da31a7fa4d818b94b0372d3026b8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96763436"
---
Örnek uygulama, iki sanal cihaz ve bir IoT Edge ağ geçidi içerir. Aşağıdaki öğreticiler, denemek ve ağ geçidinin yeteneklerini anlamak için iki yaklaşım gösterir:

* Azure VM 'de IoT Edge ağ geçidini oluşturun ve sanal bir kamerayı bağlayın.
* IoT Edge ağ geçidini Intel NUC gibi gerçek bir cihazda oluşturun ve gerçek bir kamerayı bağlayın.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure IoT Central video analizi uygulama şablonunu kullanarak bir perakende mağaza uygulaması oluşturun
> * Uygulama ayarlarını özelleştirme
> * IoT Edge ağ geçidi cihazı için bir cihaz şablonu oluşturma
> * IoT Central uygulamanıza bir ağ geçidi cihazı ekleme

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici serisini tamamlayabilmeniz için şunlar gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa, [Azure kaydolma sayfasında](https://aka.ms/createazuresubscription)bir tane oluşturabilirsiniz.
* Gerçek bir kamera kullanıyorsanız, IoT Edge cihaz ile kamera arasında bağlantınız olması gerekir ve **gerçek zamanlı akış protokolü** kanalının olması gerekir.

## <a name="initial-setup"></a>İlk kurulum

Bu öğreticilerde, çeşitli yapılandırma dosyalarını güncelleştirir ve kullanırsınız. Bu dosyaların ilk sürümleri [LVA-Gateway](https://github.com/Azure/live-video-analytics/tree/master/ref-apps/lva-edge-iot-central-gateway) GitHub deposunda bulunur. Depo, dağıttığınız hizmetlerden yapılandırma değerlerini kaydetmek için indirmeniz ve kullanmanız için bir [karalama defteri](https://github.com/Azure/live-video-analytics/blob/master/ref-apps/lva-edge-iot-central-gateway/setup/Scratchpad.txt) metin dosyası içerir. Bu dosya öğreticilerde sonraki adımları tamamlamanıza yardımcı olur.

Bu dosyaların kopyalarını kaydetmek için yerel makinenizde *LVA-yapılandırma* adlı bir klasör oluşturun. Ardından, aşağıdaki bağlantıların her birine sağ tıklayın ve dosyayı *LVA-yapılandırma* klasörüne kaydetmek Için **farklı kaydet** ' i seçin:
