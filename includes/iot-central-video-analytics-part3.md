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
ms.openlocfilehash: 60a62733a17d1a3dcc4ba80ed7ceb1c37c8ac5d6
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91877239"
---
## <a name="create-the-azure-iot-edge-gateway-device"></a>Azure IoT Edge ağ geçidi cihazı oluşturma

Video analizi-nesne ve hareket algılama uygulaması bir **LVA Edge nesne algılayıcısı** cihaz şablonu ve **LVA Edge hareketi algılama** cihaz şablonu içerir. Bu bölümde, dağıtım bildirimini kullanarak bir ağ geçidi cihaz şablonu oluşturur ve ağ geçidi cihazını IoT Central uygulamanıza eklersiniz.

### <a name="create-a-device-template-for-the-lva-edge-gateway"></a>LVA Edge Gateway için bir cihaz şablonu oluşturma

Dağıtım bildirimini içeri aktarmak ve **LVA Edge Gateway** cihaz şablonunu oluşturmak için:

1. IoT Central uygulamanızda **cihaz şablonları**' na gidin ve **+ Yeni**' yi seçin.

1. **Şablon türü seç** sayfasında **Azure IoT Edge** kutucuğunu seçin. Ardından **İleri: Özelleştir**' i seçin.

1. **Azure IoT Edge dağıtım bildirimini karşıya yükle** sayfasında, *LVA Edge Gateway* 'i şablon adı olarak girin ve **ağ geçidi cihazını aşağı akış cihazlarıyla**denetleyin.

    Henüz dağıtım bildirimine gözamayın. Bunu yaparsanız, Dağıtım Sihirbazı her modül için bir arabirim bekler, ancak yalnızca **Lvaedgegatewaymodule**arabirimini kullanıma sunmalıdır. Daha sonraki bir adımda bildirimi karşıya yüklersiniz.

    :::image type="content" source="./media/iot-central-video-analytics-part3/upload-deployment-manifest.png" alt-text="Dağıtım bildirimini karşıya yükleme":::

    Şunu seçin: **İleri: Gözden Geçir**.

1. **İnceleme** sayfasında **Oluştur**' u seçin.

### <a name="import-the-device-capability-model"></a>Cihaz yetenek modeli 'ni içeri aktarma

Cihaz şablonu bir cihaz yetenek modeli içermelidir. **LVA Edge Gateway** sayfasında **içeri aktarma yetenek modeli** kutucuğunu seçin. Daha önce oluşturduğunuz *LVA-yapılandırma* klasörüne gidin ve dosyadaki *LvaEdgeGatewayDcm.js* seçin.

**LVA Edge Gateway** cihaz şablonu artık **LVA Edge Gateway modülünü** üç arabirimle birlikte içermektedir: **cihaz bilgileri**, **LVA Edge Gateway ayarları**ve **LVA Edge Gateway arabirimi**.
