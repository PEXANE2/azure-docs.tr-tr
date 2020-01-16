---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/25/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 5680bd1c3fadab8edcde1634c708ca8683c0e99c
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76021121"
---
## <a name="create-base-resources"></a>Temel kaynaklar oluşturma

İleti yönlendirmeyi yapılandırmadan önce bir IoT Hub, bir depolama hesabı ve Service Bus kuyruğu oluşturmanız gerekir. Bu kaynaklar, Bu öğreticinin 1. bölümünde kullanılabilen dört makale kullanılarak oluşturulabilir: Azure portal, bir Azure Resource Manager şablonu, Azure CLı veya Azure PowerShell.

Tüm kaynaklar için aynı kaynak grubunu ve konumunu kullanın. Sonunda, kaynak grubunu silerek tüm kaynakları tek bir adımda kaldırabilirsiniz.

Aşağıdaki bölümlerde gerçekleştirilecek adımların özeti aşağıda verilmiştir: 

1. Bir [kaynak grubu](../articles/azure-resource-manager/management/overview.md) oluşturun.

2. S1 katmanında IoT hub'ı oluşturun. IOT hub'ınızı bir tüketici grubu ekleyin. Tüketici grubu Azure Stream Analytics tarafından veriler alınırken kullanılır.

   > [!NOTE]
   > Bu öğreticiyi tamamlayabilmeniz için ücretli bir katmanda IoT Hub 'ı kullanmanız gerekir. Ücretsiz katman yalnızca bir uç nokta ayarlamanıza izin verir ve bu öğretici birden fazla uç nokta gerektirir.
   > 

3. Standard_LRS çoğaltmasıyla standart bir V1 depolama hesabı oluşturun.

4. Service Bus ad alanı ve kuyruğu oluşturun.

5. Hub'ınıza iletiler gönderen simülasyon cihazı için cihaz kimliği oluşturun. Test aşaması için anahtarı kaydedin. (Kaynak Yöneticisi şablonu oluşturuyorsanız, bu, şablonu dağıttıktan sonra yapılır.)