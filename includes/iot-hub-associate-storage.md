---
title: include dosyası
description: include dosyası
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 01/22/2019
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: ebc23ce4238c736442fbc4507e858876f9192fd9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76021106"
---
## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Azure Depolama hesabını IoT Hub ile ilişkilendirme

Benzetimli aygıt uygulaması bir dosyayı blob'a yükledığından, IoT hub'ınızla ilişkili bir [Azure Depolama](../articles/storage/common/storage-account-create.md) hesabınız olmalıdır. Bir Azure Depolama hesabını bir IoT hub'ı ile ilişkilendirdiğinizde, IoT hub'ı bir SAS URI oluşturur. Bir aygıt, bir dosyayı blob kapsayıcısına güvenli bir şekilde yüklemek için bu SAS URI'yi kullanabilir. IoT Hub hizmeti ve aygıt SDK'ları, SAS URI'yi oluşturan ve dosya yüklemek için kullanılacak bir aygıt için kullanılabilir hale getiren işlemi koordine eder.

[Azure portalını kullanarak dosya yüklemelerini yapılandırma yönergelerini](../articles/iot-hub/iot-hub-configure-file-upload.md)izleyin. Blob kapsayıcının IoT hub'ınızla ilişkili olduğundan ve dosya bildirimlerinin etkin olduğundan emin olun.

![Portalda Dosya Bildirimlerini Etkinleştir](./media/iot-hub-associate-storage/file-notifications-vs2019.png)
