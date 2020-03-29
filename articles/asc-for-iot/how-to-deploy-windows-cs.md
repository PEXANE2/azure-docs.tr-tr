---
title: IoT aracısı için Azure Güvenlik Merkezi'nin Windows yüklemesi | Microsoft Dokümanlar
description: IoT aracısı için Azure Güvenlik Merkezi'ni 32 bit veya 64 bit Windows aygıtlarına nasıl yükleyebilirsiniz hakkında bilgi edinin.
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
ms.openlocfilehash: acc99f260931de7fd8c7566a3ff6daf43f34c5ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68597217"
---
# <a name="deploy-an-azure-security-center-for-iot-c-based-security-agent-for-windows"></a>Windows için IoT için Azure Güvenlik Merkezi C# tabanlı güvenlik aracısını dağıtma

Bu kılavuzda, IoT C# tabanlı güvenlik aracısı için Azure Güvenlik Merkezi'nin Windows'a nasıl yükleneceğimiz açıklanmaktadır.

Bu kılavuzda şunların nasıl yapıldığını öğrenirsiniz: 
> [!div class="checklist"]
> * Yükleme
> * Dağıtımı doğrulama
> * Aracıyı kaldırın
> * Sorun giderme 

## <a name="prerequisites"></a>Ön koşullar

Diğer platformlar ve aracı tatları için [bkz.](how-to-deploy-agent.md)

1. Yüklemek istediğiniz makinede yerel yönetici hakları. 

1. Aygıt için [bir güvenlik modülü oluşturun.](quickstart-create-security-twin.md)

## <a name="installation"></a>Yükleme 

Güvenlik aracısını yüklemek için aşağıdaki iş akışını kullanın:

1. Aygıta IoT Windows C# aracısı için Azure Güvenlik Merkezi'ni yükleyin. IoT [GitHub deposu](https://github.com/Azure/Azure-IoT-Security-Agent-CS)için Azure Güvenlik Merkezi'nden makinenize en son sürümü indirin.

1. Paketin içeriğini ayıklayın ve /Install klasörüne gidin.

1. Windows PowerShell'i Yönetici olarak açın. 
1. Çalıştırarak InstallSecurityAgent komut dosyasına çalışan izinler ekleyin:<br>
    ```
    Unblock-File .\InstallSecurityAgent.ps1
    ```
    
    sonra çalıştırın:

    ```
    .\InstallSecurityAgent.ps1 -Install -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -cl <certificate location kind>
    ```
    
    Örnek:
    
    ```
    .\InstallSecurityAgent.ps1 -Install -aui Device -aum SymmetricKey -f c:\Temp\Key.txt -hn MyIotHub.azure-devices.net -di Mydevice1 -cl store
    ```
    
    Kimlik doğrulama parametreleri hakkında daha fazla bilgi için kimlik [doğrulamayı nasıl yapılandırılatır'a](concept-security-agent-authentication-methods.md)bakın.

Bu komut dosyası aşağıdaki eylemleri yapar:

- Ön koşulları yükler.

- Bir hizmet kullanıcısı ekler (etkileşimli oturum açma devre dışı).

- Aracıyı **Sistem Hizmeti**olarak yükler.

- Aracıyı sağlanan kimlik doğrulama parametreleriyle yapılandırır.


Ek yardım için PowerShell'de Yardım Al komutunu kullanın <br>Yardım Al örneği:  
    ```Get-Help .\InstallSecurityAgent.ps1```

### <a name="verify-deployment-status"></a>Dağıtım durumunu doğrulama

- Aracı dağıtım durumunu çalıştırarak kontrol edin:<br>
    ```sc.exe query "ASC IoT Agent"```

### <a name="uninstall-the-agent"></a>Aracıyı kaldırın

Aracıyı kaldırmak için:

1. Aşağıdaki PowerShell komut dosyasını - **mod** parametresi kaldır'a ayarlanmış olarak **çalıştırın.**  

    ```
    .\InstallSecurityAgent.ps1 -Uninstall
    ``` 

## <a name="troubleshooting"></a>Sorun giderme

Aracı başlatılamazsa, daha fazla bilgi almak için günlüğe kaydetmeyi (günlüğe kaydetme varsayılan olarak *kapalıdır)* açın.

Günlüğe kaydetmeyi açmak için:

1. Standart bir dosya düzenleyicisi kullanarak düzenlemek için yapılandırma dosyasını (General.config) açın.

1. Aşağıdaki değerleri edin:

   ```xml
   <add key="logLevel" value="Debug" />
   <add key="fileLogLevel" value="Debug"/> 
   <add key="diagnosticVerbosityLevel" value="Some" /> 
   <add key="logFilePath" value="IoTAgentLog.log" />
   ```

    > [!NOTE]
    > Sorun giderme tamamlandıktan sonra oturumu **kapatmanızı** öneririz. **Oturum** açma, günlük dosya boyutunu ve veri kullanımını artırır. 

1. Aşağıdaki PowerShell veya komut satırını çalıştırarak aracıyı yeniden başlatın:

    **PowerShell**
     ```
     Restart-Service "ASC IoT Agent"
     ```
     
   or

    **Cmd**
     ```
     sc.exe stop "ASC IoT Agent" 
     sc.exe start "ASC IoT Agent" 
     ```

1. Hata hakkında daha fazla bilgi için günlük dosyasını gözden geçirin.

   Günlük dosya konumu:`%WinDir%/System32/IoTAgentLog.log`


## <a name="next-steps"></a>Sonraki adımlar
- IoT hizmetine [Genel Bakış](overview.md) için Azure Güvenlik Merkezi'ni okuyun
- IoT [Mimarisi](architecture.md) için Azure Güvenlik Merkezi hakkında daha fazla bilgi edinin
- [Hizmeti](quickstart-onboard-iot-hub.md) etkinleştirme
- [SSS'yi](resources-frequently-asked-questions.md) okuyun
- [Uyarıları](concept-security-alerts.md) anlama