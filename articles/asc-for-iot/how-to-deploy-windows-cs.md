---
title: Windows cihaza C# Aracısı 'nı yükler
description: 32-bit veya 64 bit Windows cihazlarına IoT Aracısı için Azure Güvenlik Merkezi 'nin nasıl yükleneceği hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 4d7d2f0a423a50f85160a856480eaa973be7e2b0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81537619"
---
# <a name="deploy-an-azure-security-center-for-iot-c-based-security-agent-for-windows"></a>Windows için IoT için Azure Güvenlik Merkezi C# tabanlı güvenlik aracısını dağıtma

Bu kılavuzda, Windows 'da IoT C# tabanlı güvenlik aracısının Azure Güvenlik Merkezi 'nin nasıl yükleneceği açıklanmaktadır.

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Yükleme
> * Dağıtımı doğrulama
> * Aracıyı kaldırın
> * Sorun giderme

## <a name="prerequisites"></a>Ön koşullar

Diğer platformlar ve aracı türleri için bkz. [doğru güvenlik aracısını seçme](how-to-deploy-agent.md).

1. Yüklemek istediğiniz makinede yerel yönetici hakları.

1. Cihaz için [bir güvenlik modülü oluşturun](quickstart-create-security-twin.md) .

## <a name="installation"></a>Yükleme

Güvenlik aracısını yüklemek için aşağıdaki iş akışını kullanın:

1. Cihaza IoT Windows C# Aracısı için Azure Güvenlik Merkezi 'ni yükler. IoT [GitHub deposu](https://github.com/Azure/Azure-IoT-Security-Agent-CS)Için Azure Güvenlik Merkezi ' nden makinenize en son sürümü indirin.

1. Paketin içeriğini ayıklayın ve/Install klasörüne gidin.

1. Windows PowerShell 'i yönetici olarak açın.
1. Şu komutu çalıştırarak ınstallsecurityagent betiğine çalışan izinleri ekleyin:

    ```
    Unblock-File .\InstallSecurityAgent.ps1
    ```

    ardından şunu çalıştırın:

    ```
    .\InstallSecurityAgent.ps1 -Install -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -cl <certificate location kind>
    ```

    Örneğin:

    ```
    .\InstallSecurityAgent.ps1 -Install -aui Device -aum SymmetricKey -f c:\Temp\Key.txt -hn MyIotHub.azure-devices.net -di Mydevice1 -cl store
    ```

    Kimlik doğrulama parametreleri hakkında daha fazla bilgi için bkz. [kimlik doğrulamasını yapılandırma](concept-security-agent-authentication-methods.md).

Bu betik aşağıdaki eylemleri yapar:

* Önkoşulları kurar.
* Bir hizmet kullanıcısı ekler (etkileşimli oturum açma devre dışı).
* Aracıyı bir **sistem hizmeti**olarak kurar.
* Aracıyı, belirtilen kimlik doğrulama parametreleriyle yapılandırır.

Ek Yardım için PowerShell 'de Get-Help komutunu kullanın.

Get-Help örneği:```Get-Help .\InstallSecurityAgent.ps1```

### <a name="verify-deployment-status"></a>Dağıtım durumunu doğrula

Şunu çalıştırarak aracı dağıtım durumunu denetleyin:

```sc.exe query "ASC IoT Agent"```

### <a name="uninstall-the-agent"></a>Aracıyı kaldırın

Aracıyı kaldırmak için:

1. Aşağıdaki PowerShell betiğini, **-Mode** parametresi **Uninstall**olarak ayarlanmış şekilde çalıştırın.

    ```
    .\InstallSecurityAgent.ps1 -Uninstall
    ```

## <a name="troubleshooting"></a>Sorun giderme

Aracı başlamazsa, daha fazla bilgi almak için günlüğü açın (varsayılan olarak günlüğe kaydetme *kapalıdır* ).

Günlüğe kaydetmeyi açmak için:

1. Standart bir dosya Düzenleyicisi kullanarak düzenleme için yapılandırma dosyasını (genel. config) açın.

1. Aşağıdaki değerleri düzenleyin:

   ```xml
   <add key="logLevel" value="Debug" />
   <add key="fileLogLevel" value="Debug"/>
   <add key="diagnosticVerbosityLevel" value="Some" />
   <add key="logFilePath" value="IoTAgentLog.log" />
   ```

    > [!NOTE]
    > Sorun giderme işlemi tamamlandıktan sonra oturumu **kapatmayı** öneririz. Günlüğe kaydetmeyi **bırakmak günlük dosyası** boyutunu ve veri kullanımını artırır.

1. Aşağıdaki PowerShell veya komut satırını çalıştırarak aracıyı yeniden başlatın:

    **PowerShell**

     ```
     Restart-Service "ASC IoT Agent"
     ```

   or

    **CMD**

     ```
     sc.exe stop "ASC IoT Agent"
     sc.exe start "ASC IoT Agent"
     ```

1. Hata hakkında daha fazla bilgi için günlük dosyasını gözden geçirin. Günlük dosyası, betiği çalıştırdığımız çalışma dizininde mevcut olacaktır. 

   Günlük dosyası konumu:`.\IoTAgentLog.log`

## <a name="next-steps"></a>Sonraki adımlar

* IoT hizmetine [genel bakış](overview.md) Için Azure Güvenlik Merkezi 'ni okuyun
* IoT [mimarisi](architecture.md) Için Azure Güvenlik Merkezi hakkında daha fazla bilgi edinin
* [Hizmeti](quickstart-onboard-iot-hub.md) etkinleştirme
* [SSS](resources-frequently-asked-questions.md) 'yi okuyun
* [Uyarıları](concept-security-alerts.md) anlama
