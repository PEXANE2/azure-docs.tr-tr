---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/06/2019
ms.author: alkohli
ms.openlocfilehash: 348f7bdd333da4f4a6cb41a438b7aee08d6a6bbb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188833"
---
İstemcinin işletim sistemine bağlı olarak, aygıta uzaktan bağlanma yordamları farklıdır.

### <a name="remotely-connect-from-a-windows-client"></a>Windows istemcisinden uzaktan bağlanma

Başlamadan önce, Windows istemcinizin Windows PowerShell 5.0 veya daha sonra çalıştırdığından emin olun.

Bir Windows istemcisinden uzaktan bağlanmak için aşağıdaki adımları izleyin.

1. Bir Windows PowerShell oturumunu yönetici olarak çalıştırın.
2. Windows Uzaktan Yönetim hizmetinin istemcinizde olduğundan emin olun. Komut istemine şunları yazın:

    `winrm quickconfig`

3. Aygıt IP adresine bir değişken atayın.

    $ip = "<device_ip>"

    Cihazınızın IP adresiyle değiştirin. `<device_ip>`

4. Cihazınızın IP adresini istemcinin güvenilen ana bilgisayarlar listesine eklemek için aşağıdaki komutu yazın:

    `Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force`

5. Aygıtta bir Windows PowerShell oturumu başlatın:

    `Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell`

6. İstendiğinde parolayı sağlayın. Yerel web Web UI'da oturum açmaiçin kullanılan parolayı kullanın. Varsayılan yerel web UI parolası *Password1'dir.* Uzaktan PowerShell kullanarak aygıta başarıyla bağlandığınızda, aşağıdaki örnek çıktıyı görürsünüz:  

    ```
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    
    PS C:\WINDOWS\system32> winrm quickconfig
    WinRM service is already running on this machine.
    PS C:\WINDOWS\system32> $ip = "10.100.10.10"
    PS C:\WINDOWS\system32> Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force
    PS C:\WINDOWS\system32> Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell

    WARNING: The Windows PowerShell interface of your device is intended to be used only for the initial network configuration. Please engage Microsoft Support if you need to access this interface to troubleshoot any potential issues you may be experiencing. Changes made through this interface without involving Microsoft Support could result in an unsupported configuration.
    [10.100.10.10]: PS>
    ```

### <a name="remotely-connect-from-a-linux-client"></a>Bir Linux istemcisinden uzaktan bağlanma

Bağlanmak için kullanacağınız Linux istemcisi üzerinde:

- SSH remoting özelliğini almak [için GitHub'dan Linux için en son PowerShell Core'u yükleyin.](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-linux?view=powershell-6) 
- [Paketi yalnızca `gss-ntlmssp` NTLM modülünden yükleyin.](https://github.com/Microsoft/omi/blob/master/Unix/doc/setup-ntlm-omi.md) Ubuntu istemcileri için aşağıdaki komutu kullanın:
    - `sudo apt-get install gss-ntlmssp`

Daha fazla bilgi için [PowerShell'in SSH üzerinden remoting'e](https://docs.microsoft.com/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core?view=powershell-6)gidin.

Bir NFS istemcisinden uzaktan bağlanmak için aşağıdaki adımları izleyin.

1. PowerShell oturumunu açmak için şunları yazın:

    `sudo pwsh`
 
2. Uzak istemciyi kullanarak bağlanmak için şunları yazın:

    `Enter-PSSession -ComputerName $ip -Authentication Negotiate -ConfigurationName Minishell -Credential ~\EdgeUser`

    İstendiğinde, cihazınızda oturum açmada kullanılan parolayı sağlayın.
 
> [!NOTE]
> Bu yordam Mac OS üzerinde çalışmıyor.
