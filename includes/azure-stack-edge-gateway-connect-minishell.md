---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/08/2021
ms.author: alkohli
ms.openlocfilehash: 0ad760caedffa97599548b8dd1b59a887b5690af
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105105482"
---
İstemci işletim sistemine bağlı olarak, cihaza uzaktan bağlanma yordamları farklıdır.

### <a name="remotely-connect-from-a-windows-client"></a>Bir Windows istemcisinden uzaktan bağlanma


#### <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakilerden emin olun:

- Windows istemciniz Windows PowerShell 5,0 veya üstünü çalıştırıyor.
- Windows istemciniz, cihazda yüklü olan düğüm sertifikasına karşılık gelen imzalama zincirine (kök sertifika) sahiptir. Ayrıntılı yönergeler için bkz. [Windows istemcinizdeki sertifikayı yükler](../articles/databox-online/azure-stack-edge-gpu-manage-certificates.md#import-certificates-on-the-client-accessing-the-device).
- `hosts` `C:\Windows\System32\drivers\etc` Windows istemciniz için konumunda bulunan dosyası aşağıdaki biçimde düğüm sertifikasına karşılık gelen bir girdiye sahiptir:

    `<Device IP>    <Node serial number>.<DNS domain of the device>`

    Dosya için örnek bir giriş aşağıda verilmiştir `hosts` :
 
    `10.100.10.10    1HXQG13.wdshcsso.com`
  

#### <a name="detailed-steps"></a>Ayrıntılı adımlar

Bir Windows istemcisinden uzaktan bağlanmak için aşağıdaki adımları izleyin.

1. Yönetici olarak bir Windows PowerShell oturumu çalıştırın.
2. Windows Uzaktan Yönetimi hizmetinin istemciniz üzerinde çalıştığından emin olun. Komut istemine şunları yazın:

    `winrm quickconfig`

    Daha fazla bilgi için bkz. [Windows Uzaktan Yönetimi Için yükleme ve yapılandırma](/windows/win32/winrm/installation-and-configuration-for-windows-remote-management#quick-default-configuration).

3. Cihaz IP adresine bir değişken atayın.

    $ip = "<device_ip>"

    `<device_ip>`CIHAZıNıZıN IP adresiyle değiştirin.

4. Cihazınızın IP adresini istemcinin güvenilen konaklar listesine eklemek için aşağıdaki komutu yazın:

    `Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force`

5. Cihazda bir Windows PowerShell oturumu başlatın:

    `Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell -UseSSL`

    Güven ilişkisiyle ilgili bir hata görürseniz, cihazınıza yüklenen düğüm sertifikasının imzalama zincirinin cihazınıza erişen istemciye de yüklenip yüklenmediğini kontrol edin.

    > [!NOTE] 
    > `-UseSSL`Seçeneğini kullandığınızda, *https* üzerinden PowerShell aracılığıyla uzaktan iletişim oluşturursunuz. PowerShell aracılığıyla uzaktan bağlanmak için her zaman *https* kullanmanızı öneririz. Bir *http* oturumu en güvenli bağlantı yöntemi olmasa da, güvenilir ağlarda kabul edilebilir.

6. İstendiğinde parolayı girin. Yerel Web Kullanıcı arabiriminde oturum açmak için kullanılan parolayı kullanın. Varsayılan yerel Web UI parolası *Parola1*'dır. Uzak PowerShell kullanarak cihaza başarıyla bağlandığınızda aşağıdaki örnek çıktıyı görürsünüz:  

    ```
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    
    PS C:\WINDOWS\system32> winrm quickconfig
    WinRM service is already running on this machine.
    PS C:\WINDOWS\system32> $ip = "10.100.10.10"
    PS C:\WINDOWS\system32> Set-Item WSMan:\localhost\Client\TrustedHosts $ip -Concatenate -Force
    PS C:\WINDOWS\system32> Enter-PSSession -ComputerName $ip -Credential $ip\EdgeUser -ConfigurationName Minishell -UseSSL

    WARNING: The Windows PowerShell interface of your device is intended to be used only for the initial network configuration. Please engage Microsoft Support if you need to access this interface to troubleshoot any potential issues you may be experiencing. Changes made through this interface without involving Microsoft Support could result in an unsupported configuration.
    [10.100.10.10]: PS>
    ```

### <a name="remotely-connect-from-a-linux-client"></a>Linux istemcisinden uzaktan bağlanma

Bağlanmak için kullanacağınız Linux istemcisinde:

- SSH uzaktan iletişim özelliğini almak için GitHub 'dan [Linux için en son PowerShell Core 'U yükler](/powershell/scripting/install/installing-powershell-core-on-linux) . 
- [Yalnızca `gss-ntlmssp` NTLM modülünden paketi yükler](https://github.com/Microsoft/omi/blob/master/Unix/doc/setup-ntlm-omi.md). Ubuntu istemcileri için aşağıdaki komutu kullanın:
    - `sudo apt-get install gss-ntlmssp`

Daha fazla bilgi için [SSH üzerinden PowerShell uzaktan iletişimi](/powershell/scripting/learn/remoting/ssh-remoting-in-powershell-core)konusuna gidin.

NFS istemcisinden uzaktan bağlanmak için aşağıdaki adımları izleyin.

1. PowerShell oturumunu açmak için şunu yazın:

    `pwsh`
 
2. Uzak istemciyi kullanarak bağlanmak için şunu yazın:

    `Enter-PSSession -ComputerName $ip -Authentication Negotiate -ConfigurationName Minishell -Credential ~\EdgeUser`

    İstendiğinde, cihazınızda oturum açmak için kullanılan parolayı belirtin.
 
> [!NOTE]
> Bu yordam Mac OS üzerinde çalışmaz.