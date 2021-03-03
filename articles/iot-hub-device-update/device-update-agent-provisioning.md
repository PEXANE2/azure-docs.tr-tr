---
title: Azure IoT Hub Aracısı için cihaz güncelleştirmesini sağlama | Microsoft Docs
description: Azure IoT Hub Aracısı için cihaz güncelleştirmesi sağlama
author: ValOlson
ms.author: valls
ms.date: 2/16/2021
ms.topic: how-to
ms.service: iot-hub-device-update
ms.openlocfilehash: 79bac3f057412f3973121f48cd735f72d0a97d04
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101680016"
---
# <a name="device-update-agent"></a>Cihaz Güncelleştirme Aracısı

IoT Hub cihaz güncelleştirmesi iki güncelleştirme biçimini destekler: görüntü tabanlı ve paket tabanlı. 

* Görüntü güncelleştirmeleri, cihazın bitiş durumunda daha yüksek bir güven düzeyi sağlar. Paket ve bağımlılıklarıyla aynı zorluklara sahip olmadığından, bir üretim öncesi ortamı ve üretim ortamı arasında görüntü güncelleştirme sonuçlarını çoğaltmak genellikle daha kolay olur. Atomik doğası nedeniyle, bir A/B yük devretme modelini de kolayca benimseyebilirler. 
* Paket tabanlı güncelleştirmeler, cihazdaki yalnızca belirli bir bileşeni veya uygulamayı değiştirecek olan güncelleştirmeler için geçerlidir. Bu nedenle, bant genişliğinin daha düşük tüketimine lider ve güncelleştirmeyi indirme ve yükleme süresini azaltmaya yardımcı olur. Paket güncelleştirmeleri genellikle bir güncelleştirme uygulanırken cihazların daha az kapalı kalma süresine izin verir ve görüntü oluşturma yükünden kaçınır. 

Cihaz güncelleştirme aracısını oluşturma, çalıştırma ve değiştirme hakkında aşağıdaki bağlantıları izleyin.

## <a name="build-the-device-update-agent"></a>Cihaz güncelleştirme aracısını oluşturma

Cihaz güncelleştirme aracısını kaynaktan [derlemek](https://github.com/Azure/iot-hub-device-update/blob/main/docs/agent-reference/how-to-build-agent-code.md) için yönergeleri izleyin.

## <a name="run-the-device-update-agent"></a>Cihaz güncelleştirme aracısını çalıştırma

Aracı başarılı bir şekilde oluşturulduktan sonra aracıyı [çalıştıralım](https://github.com/Azure/iot-hub-device-update/blob/main/docs/agent-reference/how-to-run-agent.md) zamanı.

## <a name="modifying-the-device-update-agent"></a>Cihaz güncelleştirme aracısını değiştirme

Şimdi, aracının yansımanıza dahil etmek için gerekli değişiklikleri yapın.  Yönergeler için cihaz güncelleştirme aracısını [değiştirme](https://github.com/Azure/iot-hub-device-update/blob/main/docs/agent-reference/how-to-modify-the-agent-code.m) bölümüne bakın.

### <a name="troubleshooting-guide"></a>Sorun Giderme Kılavuzu

Sorunlarla karşılaşırsanız, olası sorunların engelini kaldırmaya ve Microsoft 'a sağlamak üzere gerekli bilgileri toplamanıza yardımcı olmak için IoT Hub [sorun giderme kılavuzu](troubleshoot-device-update.md) Için cihaz güncelleştirmesini gözden geçirin.

## <a name="next-steps"></a>Sonraki Adımlar

IoT Hub yönelik cihaz güncelleştirmesinin kolay bir gösterimi için önceden oluşturulmuş görüntüleri ve ikili dosyaları kullanın.  

[Görüntü güncelleştirme: Raspberry Pi 3 B + başvuru yocto ile çalışmaya başlama](device-update-raspberry-pi.md)

[Görüntü güncelleştirme: Ubuntu (18,04 x64) simülatörü başvuru Aracısı 'Nı kullanmaya başlama](device-update-simulator.md)

[Paket güncelleştirmesi: Ubuntu Server 18,04 x64 paket Aracısı 'nı kullanmaya başlama](device-update-ubuntu-agent.md)

