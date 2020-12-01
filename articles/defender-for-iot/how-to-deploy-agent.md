---
title: Güvenlik aracılarını seçme ve dağıtma
description: IoT cihazlarında IoT güvenlik aracıları için Defender 'ı seçme ve dağıtma hakkında bilgi edinin.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 2634ed819b8818632c58b9e471b0f26190e5f16b
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96339974"
---
# <a name="select-and-deploy-a-security-agent-on-your-iot-device"></a>IoT cihazınızda bir güvenlik Aracısı seçin ve dağıtın

IoT için Defender, IoT cihazlarından veri izleyip toplayacak güvenlik aracıları için başvuru mimarileri sağlar.
Daha fazla bilgi için bkz. [Güvenlik Aracısı başvuru mimarisi](security-agent-architecture.md).

Aracılar açık kaynaklı projeler olarak geliştirilmiştir ve iki şekilde kullanılabilir: <br> [C](https://aka.ms/iot-security-github-c)ve [C#](https://aka.ms/iot-security-github-cs).

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Güvenlik Aracısı türleri karşılaştırması
> * Desteklenen aracı platformlarını bul
> * Çözümünüz için doğru aracı seçin

## <a name="understand-security-agent-options"></a>Güvenlik Aracısı seçeneklerini anlama

IoT güvenlik Aracısı 'nın her bir Defender özelliği aynı özellik kümesini sunar ve benzer yapılandırma seçeneklerini destekler.

C tabanlı güvenlik aracısının daha düşük bir bellek parmak izi vardır ve daha az kullanılabilir kaynağa sahip cihazlarda ideal seçenektir.

|     | C tabanlı güvenlik Aracısı | C# tabanlı güvenlik Aracısı |
| --- | ----------- | --------- |
| **Açık kaynak** | [GitHub](https://aka.ms/iot-security-github-c) 'da [MIT Lisansı](https://en.wikipedia.org/wiki/MIT_License) altında kullanılabilir | [GitHub](https://aka.ms/iot-security-github-cs) 'da [MIT Lisansı](https://en.wikipedia.org/wiki/MIT_License) altında kullanılabilir |
| **Geliştirme dili**    | C | C# |
| **Desteklenen Windows platformları?** | Hayır | Yes |
| **Windows önkoşulları** | --- | [WMI](/windows/desktop/wmisdk/) |
| **Desteklenen Linux platformları?** | Evet, x64 ve x86 | Evet, yalnızca x64 |
| **Linux önkoşulları** | libunwind8, libcurl3, UUID-Runtime, auditd, audispd-eklentiler | libunwind8, libcurl3, UUID-Runtime, auditd, audispd-Eklentiler, sudo, netstat, Iptables |
| **Disk ayak izi** | 10,5 MB | 90 MB |
| **Bellek ayak izi (Ortalama)** | 5,5 MB | 33 MB |
| **IoT Hub için [kimlik doğrulaması](concept-security-agent-authentication-methods.md)** | Yes | Yes |
| **Güvenlik verileri [toplama](how-to-agent-configuration.md#supported-security-events)** | Yes | Yes |
| **Olay toplama** | Yes | Yes |
| **[Güvenlik modülü ikizi](concept-security-module.md) aracılığıyla uzaktan yapılandırma** | Yes | Yes |

## <a name="security-agent-installation-guidelines"></a>Güvenlik Aracısı yükleme yönergeleri

**Windows** Için: Install SecurityAgent.ps1 betiğinin bir yönetici PowerShell penceresinden yürütülmesi gerekir.

**Linux** için: InstallSecurityAgent.sh, süper kullanıcı olarak çalıştırılmalıdır. "Sudo" ile yükleme komutunun önek olarak önerilmenizi öneririz.

## <a name="choose-an-agent-flavor"></a>Bir aracı türü seçin

Doğru aracıyı seçmek için IoT cihazlarınızla ilgili aşağıdaki soruları yanıtlayın:

- _Windows Server_ veya _Windows IoT Core_ kullanıyor musunuz?

    [Windows Için C# tabanlı bir güvenlik Aracısı dağıtın](how-to-deploy-windows-cs.md).

- X86 mimarisine sahip bir Linux dağıtımı kullanıyor musunuz?

    [Linux Için C tabanlı bir güvenlik Aracısı dağıtın](how-to-deploy-linux-c.md).

- X64 mimarisine sahip bir Linux dağıtımı kullanıyor musunuz?

    Her iki aracı türleri de kullanılabilir. <br>
    [Linux Için C tabanlı bir güvenlik Aracısı dağıtın](how-to-deploy-linux-c.md) ve/veya [Linux için C# tabanlı bir güvenlik Aracısı dağıtın](how-to-deploy-linux-cs.md).

Her iki aracı türüler de aynı özellik kümesini sunar ve benzer yapılandırma seçeneklerini destekler.
Daha fazla bilgi için bkz. [Güvenlik Aracısı karşılaştırması](how-to-deploy-agent.md#understand-security-agent-options) .

## <a name="supported-platforms"></a>Desteklenen platformlar

Aşağıdaki liste, şu anda desteklenen tüm platformları içerir.

|IoT Aracısı için Defender |İşletim Sistemi |Mimari |
|--------------|------------|--------------|
|C|Ubuntu 16.04 |    x64|
|C|Ubuntu 18.04 |    x64, ARMv7|
|C|Debian 9 |    x64, x86|
|C#|Ubuntu 16.04     |x64|
|C#|Ubuntu 18.04    |x64, ARMv7|
|C#|Debian 9    |x64|
|C#|Windows Server 2016|    x64|
|C#|Windows 10 IoT Core, derleme 17763    |x64|
|

## <a name="next-steps"></a>Sonraki adımlar

Yapılandırma seçenekleri hakkında daha fazla bilgi edinmek için, aracı yapılandırması için nasıl yapılır kılavuzuna ilerleyin.
> [!div class="nextstepaction"]
> [Aracı yapılandırması nasıl yapılır Kılavuzu](./how-to-agent-configuration.md)