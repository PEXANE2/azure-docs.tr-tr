---
title: include dosyası
description: include dosyası
services: iot-hub
ms.service: iot-hub
author: dominicbetts
ms.topic: include
ms.date: 02/17/2019
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 2514325c93f0f72aa979f871e86e465093479402
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69558726"
---
## <a name="customize-and-extend-the-device-management-actions"></a>Cihaz yönetimi eylemlerini özelleştirme ve genişletme

IoT çözümleriniz, aygıt ikizi ve buluttan aygıta yöntem ilkellerini kullanarak tanımlı aygıt yönetim desenleri kümesini genişletebilir veya özel desenler ietkinleştirebilir. Aygıt yönetimi eylemlerine diğer örnekler arasında fabrika sıfırlama, firmware güncelleştirmesi, yazılım güncelleştirmesi, güç yönetimi, ağ ve bağlantı yönetimi ve veri şifreleme sayılabilir.

## <a name="device-maintenance-windows"></a>Cihaz bakım pencereleri

Genellikle, aygıtları kesintileri ve kapalı kalma süresini en aza indiren bir anda eylemler gerçekleştirecek şekilde yapılandırırsınız. Aygıt bakım pencereleri, aygıtın yapılandırmasını güncelleştirmesi gereken zamanı tanımlamak için yaygın olarak kullanılan bir desendir. Arka uç çözümleriniz, aygıtınızda bakım penceresi sağlayan bir ilke tanımlamak ve etkinleştirmek için aygıt ikizinin istenen özelliklerini kullanabilir. Bir aygıt bakım penceresi ilkesini aldığında, iipolitikasının durumunu bildirmek için aygıt ikizinin bildirilen özelliğini kullanabilir. Arka uç uygulaması daha sonra aygıtların ve her ilkenin uyumluluğunu kanıtlamak için aygıt ikiz sorgularını kullanabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aygıtta uzaktan yeniden başlatmayı tetiklemek için doğrudan bir yöntem kullandınız. Aygıttan gelen son yeniden başlatma saatini bildirmek için bildirilen özellikleri kullandınız ve aygıtın buluttan son yeniden başlatma saatini bulmak için aygıt ikizini sorguladınız.

IoT Hub ve hava firmware güncellemesi üzerinden uzaktan gibi cihaz yönetimi desenleri ile çalışmaya başlamak için, [bir firmware güncelleştirmesi nasıl yapılacağını](../articles/iot-hub/tutorial-firmware-update.md)görün.

IoT çözümünüzü nasıl genişleteceğimize ve yöntem çağrılarını birden çok cihazda nasıl zamanlayabilirsiniz öğrenmek için [Zamanlama ve yayın işlerine](../articles/iot-hub/iot-hub-node-node-schedule-jobs.md)bakın.