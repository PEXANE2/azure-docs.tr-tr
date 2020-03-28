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
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76021121"
---
## <a name="create-base-resources"></a>Temel kaynaklar oluşturma

İleti yönlendirmesini yapılandırmadan önce bir IoT hub'ı, depolama hesabı ve Hizmet Veri Gönderi kuyruğu oluşturmanız gerekir. Bu kaynaklar, bu öğreticinin Bölüm 1'i için kullanılabilen dört makaleden biri kullanılarak oluşturulabilir: Azure portalı, Azure Kaynak Yöneticisi şablonu, Azure CLI veya Azure PowerShell.

Tüm kaynaklar için aynı kaynak grubunu ve konumunu kullanın. Daha sonra sonunda, kaynak grubunu silerek tüm kaynakları tek adımda kaldırabilirsiniz.

Aşağıda aşağıdaki bölümlerde gerçekleştirilecek adımların bir özeti verilmiştir: 

1. Kaynak [grubu](../articles/azure-resource-manager/management/overview.md)oluşturun.

2. S1 katmanında IoT hub'ı oluşturun. IOT hub'ınızı bir tüketici grubu ekleyin. Tüketici grubu Azure Stream Analytics tarafından veriler alınırken kullanılır.

   > [!NOTE]
   > Bu öğreticiyi tamamlamak için ücretli bir katmanda bir Iot hub kullanmanız gerekir. Ücretsiz katman yalnızca bir bitiş noktası ayarlamanızı sağlar ve bu öğretici birden çok uç nokta gerektirir.
   > 

3. Standard_LRS çoğaltmasıyla standart bir V1 depolama hesabı oluşturun.

4. Service Bus ad alanı ve kuyruğu oluşturun.

5. Hub'ınıza iletiler gönderen simülasyon cihazı için cihaz kimliği oluşturun. Test aşaması için anahtarı kaydedin. (Kaynak Yöneticisi şablonu oluşturuyorsanız, bu işlem şablonu dağıttıktan sonra yapılır.)