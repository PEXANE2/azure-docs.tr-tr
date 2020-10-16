---
title: dosya dahil etme
description: dosya dahil etme
services: iot-central
author: dominicbetts
ms.service: iot-central
ms.topic: include
ms.date: 10/06/2020
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 7246375468b3419c3d52ee3d5a51a95aa20050a9
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91877236"
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

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici serisini tamamlayabilmeniz için şunlar gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa, [Azure kaydolma sayfasında](https://aka.ms/createazuresubscription)bir tane oluşturabilirsiniz.
* Gerçek bir kamera kullanıyorsanız, IoT Edge cihaz ile kamera arasında bağlantınız olması gerekir ve **gerçek zamanlı akış protokolü** kanalının olması gerekir.

## <a name="initial-setup"></a>İlk kurulum

Bu öğreticilerde, çeşitli yapılandırma dosyalarını güncelleştirir ve kullanırsınız. Bu dosyaların ilk sürümleri [LVA-Gateway](https://github.com/Azure/live-video-analytics/tree/master/ref-apps/lva-edge-iot-central-gateway) GitHub deposunda bulunur. Depo Ayrıca, dağıttığınız hizmetlerden yapılandırma değerlerini kaydetmek için indirmeniz ve kullanmanız için bir karalama yazı metni dosyası içerir.

Bu dosyaların kopyalarını kaydetmek için yerel makinenizde *LVA-yapılandırma* adlı bir klasör oluşturun. Ardından, aşağıdaki bağlantıların her birine sağ tıklayın ve dosyayı *LVA-yapılandırma* klasörüne kaydetmek Için **farklı kaydet** ' i seçin:
