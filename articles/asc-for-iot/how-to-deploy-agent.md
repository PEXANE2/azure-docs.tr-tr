---
title: Güvenlik aracılarını seçme ve dağıtma
description: IoT aygıtlarında IoT güvenlik aracıları için Azure Güvenlik Merkezi'ni nasıl seçip dağıtıladığını öğrenin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: c3b514c79be87cd136375b4853226426965f4185
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81311211"
---
# <a name="select-and-deploy-a-security-agent-on-your-iot-device"></a>IoT aygıtınızda bir güvenlik aracısı seçme ve dağıtma

Azure Güvenlik Merkezi ioT aygıtlarından verileri izleyen ve toplayan güvenlik aracıları için başvuru mimarileri sağlar.
Daha fazla bilgi için [Güvenlik aracısı başvuru mimarisine](security-agent-architecture.md)bakın.

Aracılar açık kaynak projeleri olarak geliştirilmiştir ve iki farklı tatta mevcuttur: <br> [C](https://aka.ms/iot-security-github-c), ve [C#](https://aka.ms/iot-security-github-cs).

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Güvenlik aracısı tatlarını karşılaştırın
> * Desteklenen aracı platformlarını keşfedin
> * Çözümünüz için doğru ajan lezzetini seçin

## <a name="understand-security-agent-options"></a>Güvenlik aracısı seçeneklerini anlama

Her Azure Güvenlik Merkezi ioT güvenlik aracısı flavor aynı özellik kümesini sunar ve benzer yapılandırma seçeneklerini destekler.

C tabanlı güvenlik aracısı daha düşük bellek ayak izine sahiptir ve daha az kullanılabilir kaynağa sahip aygıtlar için ideal bir seçimdir.

|     | C tabanlı güvenlik aracısı | C# tabanlı güvenlik aracısı |
| --- | ----------- | --------- |
| Açık kaynak | [GitHub'da](https://aka.ms/iot-security-github-cs) [MIT lisansı](https://en.wikipedia.org/wiki/MIT_License) altında kullanılabilir | [GitHub'da](https://aka.ms/iot-security-github-c) [MIT lisansı](https://en.wikipedia.org/wiki/MIT_License) altında kullanılabilir |
| Geliştirme dili    | C | C# |
| Desteklenen Windows platformları? | Hayır | Evet |
| Windows ön koşullar | --- | [WMI](https://docs.microsoft.com/windows/desktop/wmisdk/) |
| Desteklenen Linux platformları? | Evet, x64 ve x86 | Evet, sadece x64 |
| Linux önkoşulları | libunwind8, libcurl3, uuid-runtime, denetlenmiş, audispd-eklentileri | libunwind8, libcurl3, uuid-runtime, denetlenmiş, audispd-eklentileri, sudo, netstat, iptables |
| Disk ayak izi | 10,5 MB | 90 MB |
| Bellek ayak izi (ortalama) | 5,5 MB | 33 MB |
| IoT Hub'ına [kimlik doğrulama](concept-security-agent-authentication-methods.md) | Evet | Evet |
| Güvenlik veri [toplama](how-to-agent-configuration.md#supported-security-events) | Evet | Evet |
| Olay toplama | Evet | Evet |
| [Güvenlik modülü ikizi](concept-security-module.md) aracılığıyla uzaktan yapılandırma | Evet | Evet |
|

## <a name="security-agent-installation-guidelines"></a>Güvenlik aracısı yükleme yönergeleri

**Windows**için : Install SecurityAgent.ps1 komut dosyası bir Administrator PowerShell penceresinden yürütülmelidir.

**Linux**için : InstallSecurityAgent.sh süperkullanıcı olarak çalıştırılmalıdır. Yükleme komutunu "sudo" ile öne kaldırmanızı öneririz.

## <a name="choose-an-agent-flavor"></a>Bir ajan lezzet seçin

Doğru aracıyı seçmek için IoT aygıtlarınız hakkındaki aşağıdaki soruları yanıtlayın:

- _Windows Server_ veya _Windows IoT Core_kullanıyor musunuz?

    [Windows için C# tabanlı bir güvenlik aracısı dağıtın.](how-to-deploy-windows-cs.md)

- X86 mimarisine sahip bir Linux dağıtımı mı kullanıyorsunuz?

    [Linux için C tabanlı bir güvenlik aracısı dağıtın.](how-to-deploy-linux-c.md)

- X64 mimarisine sahip bir Linux dağıtımı mı kullanıyorsunuz?

    Her iki ajan tatlar kullanılabilir. <br>
    [Linux için C tabanlı bir güvenlik aracısı dağıtın](how-to-deploy-linux-c.md) ve/veya [Linux için C# tabanlı bir güvenlik aracısı dağıtın.](how-to-deploy-linux-cs.md)

Her iki aracı tatları da aynı özellik kümesini sunar ve benzer yapılandırma seçeneklerini destekler.
Daha fazla bilgi edinmek için [Güvenlik aracısı karşılaştırmasına](how-to-deploy-agent.md#understand-security-agent-options) bakın.

## <a name="supported-platforms"></a>Desteklenen platformlar

Aşağıdaki liste, şu anda desteklenen tüm platformları içerir.

|IoT aracısı için Azure Güvenlik Merkezi |İşletim Sistemi |Mimari |
|--------------|------------|--------------|
|C|Ubuntu 16.04 |    x64|
|C|Ubuntu 18.04 |    x64, ARMv7|
|C|Debian 9 |    x64, x86|
|C#|Ubuntu 16.04     |x64|
|C#|Ubuntu 18.04    |x64, ARMv7|
|C#|Debian 9    |x64|
|C#|Windows Server 2016|    x64|
|C#|Windows 10 IoT Core, 17763 inşa    |x64|
|

## <a name="next-steps"></a>Sonraki adımlar

Yapılandırma seçenekleri hakkında daha fazla bilgi edinmek için aracı yapılandırması için nasıl yapIlenler kılavuzuna devam edin.
> [!div class="nextstepaction"]
> [Aracı yapılandırma nasıl yol gösterin](./how-to-agent-configuration.md)
