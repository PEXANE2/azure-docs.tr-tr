---
title: PowerShell işlevlerini kullanarak şirket içi uzak kaynakları yönetme
description: Bir PowerShell işlev uygulamasını şirket içi kaynaklara bağlamak için Azure Röle'deki Karma Bağlantıları nasıl yapılandırabileceğinizi ve şirket içi kaynağı uzaktan yönetmek için nasıl kullanılacağını öğrenin.
author: eamono
ms.topic: conceptual
ms.date: 9/5/2019
ms.author: eamono
ms.openlocfilehash: 36fc4c873dccfe9fa814bddccd829ed04207f095
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74226927"
---
# <a name="managing-hybrid-environments-with-powershell-in-azure-functions-and-app-service-hybrid-connections"></a>Azure İşlevleri ve Uygulama Hizmeti Karma Bağlantılarında PowerShell ile karma ortamları yönetme

Azure App Service Karma Bağlantılar özelliği, diğer ağlardaki kaynaklara erişim sağlar. Bu özellik hakkında daha fazla bilgi için [Karma Bağlantılar](../app-service/app-service-hybrid-connections.md) belgelerinde bulabilirsiniz. Bu makalede, şirket içi bir sunucuhedefleyen PowerShell işlevleriçalıştırmak için bu yeteneğin nasıl kullanılacağı açıklanmaktadır. Bu sunucu daha sonra bir Azure PowerShell işlevinden şirket içi ortamdaki tüm kaynakları yönetmek için kullanılabilir.


## <a name="configure-an-on-premises-server-for-powershell-remoting"></a>PowerShell remoting için şirket içi sunucu yapılandırma

Aşağıdaki komut dosyası PowerShell remoting sağlar ve yeni bir güvenlik duvarı kuralı ve winrm https dinleyici oluşturur. Test amacıyla, kendi imzalı bir sertifika kullanılır. Üretim ortamında, imzalı bir sertifika kullanmanızı öneririz.

```powershell
# For configuration of WinRM, see
# https://docs.microsoft.com/windows/win32/winrm/installation-and-configuration-for-windows-remote-management.

# Enable PowerShell remoting.
Enable-PSRemoting -Force

# Create firewall rule for WinRM. The default HTTPS port is 5986.
New-NetFirewallRule -Name "WinRM HTTPS" `
                    -DisplayName "WinRM HTTPS" `
                    -Enabled True `
                    -Profile "Any" `
                    -Action "Allow" `
                    -Direction "Inbound" `
                    -LocalPort 5986 `
                    -Protocol "TCP"

# Create new self-signed-certificate to be used by WinRM.
$Thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME  -CertStoreLocation Cert:\LocalMachine\My).Thumbprint

# Create WinRM HTTPS listener.
$Cmd = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:COMPUTERNAME ""; CertificateThumbprint=""$Thumbprint""}"
cmd.exe /C $Cmd
```

## <a name="create-a-powershell-function-app-in-the-portal"></a>Portalda bir PowerShell işlev uygulaması oluşturma

App Service Karma Bağlantılar özelliği yalnızca Temel, Standart ve Yalıtılmış fiyatlandırma planlarında kullanılabilir. PowerShell ile işlev uygulamasını oluşturduğunuzda, bu planlardan birini oluşturun veya seçin.

1. Azure [portalında](https://portal.azure.com)+ Soldaki menüde **kaynak oluştur '** u seçin ve ardından **İşlev uygulamasını**seçin.

1. **Barındırma planı** **için, App Service planını**seçin ve ardından Uygulama Hizmeti **planını/Konumu'nü**seçin.

1. Yeni , Uygulama **Hizmeti planı** adı **oluştur'u**seçin, yakınınızdaki bir [bölgede](https://azure.microsoft.com/regions/) veya işlevlerinizin erişebileceği diğer hizmetlerin yakınında bir **Konum** seçin ve ardından **Fiyatlandırma katmanını**seçin.

1. S1 Standart planını seçin ve sonra **Uygula'yı**seçin.

1. Planı oluşturmak için **Tamam'ı** seçin ve aşağıdaki ekran görüntüsünden hemen sonra tabloda belirtildiği şekilde kalan **İşlev Uygulaması** ayarlarını yapılandırın:

    ![PowerShell Core fonksiyon uygulaması](./media/functions-hybrid-powershell/create-function-powershell-app.png)  

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler: `a-z`, `0-9`, ve `-`.  | 
    | **Abonelik** | Aboneliğiniz | Bu yeni işlev uygulamasının oluşturulduğu abonelik. |
    | **Kaynak Grubu** |  myResourceGroup | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. Önerilen değeri de kullanabilirsiniz. |
    | **İşletim Sistemi** | Tercih Edilen İşletim Sistemi | Windows’u seçin. |
    | **Çalışma zamanı yığını** | Tercih edilen dil | PowerShell Core'u seçin. |
    | **Depolama** |  Genel olarak benzersiz bir ad |  İşlev uygulamanız tarafından kullanılan bir depolama hesabı oluşturun. Depolama hesabı adları 3 ila 24 karakter uzunluğunda olmalıdır ve yalnızca sayılar ve küçük harfler içerebilir. Var olan bir hesabı da kullanabilirsiniz.
    | **Uygulama Bilgileri** | Varsayılan | Desteklenen en yakın bölgede aynı *Uygulama adının* Uygulama Öngörüleri kaynağını oluşturur. Bu ayarı genişleterek, **Yeni kaynak adını** değiştirebilir veya verilerinizi depolamak istediğiniz Bir Azure [coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/) bölgesinde farklı bir **Konum** seçebilirsiniz. |

1. Ayarlarınız doğrulandıktan sonra **Oluştur'u**seçin.

1. Portalın sağ üst köşesindeki **Bildirim** simgesini seçin ve "Dağıtım başarılı oldu" iletisini bekleyin.

1. Yeni işlev uygulamanızı görüntülemek için **Kaynağa git**’i seçin. **Panoya Sabitle'yi**de seçebilirsiniz. Sabitleme, panonuzdan bu işlev uygulaması kaynağına geri dönmeyi kolaylaştırır.

## <a name="create-a-hybrid-connection-for-the-function-app"></a>İşlev uygulaması için karma bağlantı oluşturma

Karma bağlantılar, işlev uygulamasının ağ bölümünden yapılandırılır:

1. İşlev **uygulamasındaPlatform özellikleri** sekmesini seçin ve ardından **Ağ'ı**seçin. 
   ![Platform ağı için Uygulamaya Genel Bakış](./media/functions-hybrid-powershell/app-overview-platform-networking.png)  
1. **Karma bağlantı uç noktalarınızı yapılandır'ı**seçin.
   ![Ağ](./media/functions-hybrid-powershell/select-network-feature.png)  
1. **Karma bağlantı ekle'yi**seçin.
   ![Karma Bağlantı](./media/functions-hybrid-powershell/hybrid-connection-overview.png)  
1. Aşağıdaki ekran görüntüsünden hemen sonra gösterildiği gibi karma bağlantı yla ilgili bilgileri girin. Uzak komutları çalıştırırken sunucuyu daha sonra hatırlamayı kolaylaştırmak için **Endpoint Ana Bilgisayar** ayarı şirket içi sunucunun ana bilgisayarının adı yla eşleşmesini sağlama seçeneğiniz vardır. Bağlantı noktası, sunucuda daha önce tanımlanan varsayılan Windows uzaktan yönetim hizmeti bağlantı noktasıyla eşleşir.
  ![Karma Bağlantı Ekle](./media/functions-hybrid-powershell/add-hybrid-connection.png)  

    **Hibrid bağlantı adı**: ContosoHybridOnPremisesServer
    
    **Endpoint Host**: finance1
    
    **Bitiş Noktası Bağlantı Noktası**: 5986
    
    **Servicebus namespace**: Yeni Oluştur
    
    **Konum**: Kullanılabilir bir konum seçin
    
    **Adı**: contosopowershellhybrid

5. Karma bağlantı oluşturmak için **Tamam'ı** seçin.

## <a name="download-and-install-the-hybrid-connection"></a>Karma bağlantıyı indirin ve kurun

1. .msi dosyasını bilgisayarınıza yerel olarak kaydetmek için **bağlantı yöneticisini karşıdan yükle'yi** seçin.
![Yükleyiciyi indirin](./media/functions-hybrid-powershell/download-hybrid-connection-installer.png)  
1. .msi dosyasını yerel bilgisayarınızdan şirket içi sunucuya kopyalayın.
1. Hizmeti şirket içi sunucuya yüklemek için Karma Bağlantı Yöneticisi yükleyicisini çalıştırın.
![Karma Bağlantı Yükle](./media/functions-hybrid-powershell/hybrid-installation.png)  
1. Portaldan, karma bağlantıyı açın ve ardından ağ geçidi bağlantı dizesini panoya kopyalayın.
![Karma bağlantı dizelerini kopyalama](./media/functions-hybrid-powershell/copy-hybrid-connection.png)  
1. Karma Bağlantı Yöneticisi UI'yi şirket içi sunucuda açın.
![Açık Hibrit Bağlantı UI](./media/functions-hybrid-powershell/hybrid-connection-ui.png)  
1. El **Ile Gir** düğmesini seçin ve bağlantı dizesini panodan yapıştırın.
![Bağlantıyı yapıştır](./media/functions-hybrid-powershell/enter-manual-connection.png)  
1. Bağlı olarak görünmüyorsa, PowerShell'den Hibrit Bağlantı Yöneticisi'ni yeniden başlatın.
    ```powershell
    Restart-Service HybridConnectionManager
    ```

## <a name="create-an-app-setting-for-the-password-of-an-administrator-account"></a>Yönetici hesabının parolası için uygulama ayarı oluşturma

1. İşlev **uygulamasındaki Platform özellikleri** sekmesini seçin.
1. **Genel Ayarlar**altında **Yapılandırma'yı**seçin.
![Platform yapılandırması seçin](./media/functions-hybrid-powershell/select-configuration.png)  
1. Parola için yeni bir ayar oluşturmak için **Yeni uygulama ayarını** genişletin.
1. _Ayar ContosoUserPassword_adını ve şifreyi girin.
1. **Tamam'ı** seçin ve ardından parolayı işlev uygulamasında depolamak için kaydedin.
![Parola için uygulama ayarı ekleme](./media/functions-hybrid-powershell/add-appsetting-password.png)  

## <a name="create-a-function-http-trigger-to-test"></a>Test etmek için bir işlev http tetikleyici oluşturma

1. İşlev uygulamasından yeni bir HTTP tetikleme işlevi oluşturun.
![Yeni HTTP tetikleyicisi oluşturma](./media/functions-hybrid-powershell/create-http-trigger-function.png)  
1. Şablondaki PowerShell kodunu aşağıdaki kodla değiştirin:

    ```powershell
    # Input bindings are passed in via param block.
    param($Request, $TriggerMetadata)
    
    # Write to the Azure Functions log stream.
    Write-Output "PowerShell HTTP trigger function processed a request."
    
    # Note that ContosoUserPassword is a function app setting, so I can access it as $env:ContosoUserPassword.
    $UserName = "ContosoUser"
    $securedPassword = ConvertTo-SecureString  $Env:ContosoUserPassword -AsPlainText -Force
    $Credential = [System.management.automation.pscredential]::new($UserName, $SecuredPassword)
    
    # This is the name of the hybrid connection Endpoint.
    $HybridEndpoint = "finance1"
    
    $Script = {
        Param(
            [Parameter(Mandatory=$True)]
            [String] $Service
        )
        Get-Service $Service
    }
    
    Write-Output "Scenario 1: Running command via Invoke-Command"
    Invoke-Command -ComputerName $HybridEndpoint `
                   -Credential $Credential `
                   -Port 5986 `
                   -UseSSL `
                   -ScriptBlock $Script `
                   -ArgumentList "*" `
                   -SessionOption (New-PSSessionOption -SkipCACheck)
    ```

3. İşlevi test etmek için **Kaydet** ve **Çalıştır'ı** seçin.
![İşlev uygulamasını test edin](./media/functions-hybrid-powershell/test-function-hybrid.png)  

## <a name="managing-other-systems-on-premises"></a>Diğer sistemlerin şirket içinde yönetilmesi

Bağlı şirket içi sunucuyu yerel ortamdaki diğer sunuculara ve yönetim sistemlerine bağlanmak için kullanabilirsiniz. Bu, PowerShell işlevlerinizi kullanarak veri merkezi işlemlerinizi Azure'dan yönetmenize olanak tanır. Aşağıdaki komut dosyası, sağlanan kimlik bilgileri altında çalışan bir PowerShell yapılandırma oturumu kaydeder. Bu kimlik bilgileri uzak sunucularda bir yönetici için olmalıdır. Daha sonra yerel sunucu veya veri merkezi diğer uç noktalara erişmek için bu yapılandırmayı kullanabilirsiniz.

```powershell
# Input bindings are passed in via param block.
param($Request, $TriggerMetadata)

# Write to the Azure Functions log stream.
Write-Host "PowerShell HTTP trigger function processed a request."

# Note that ContosoUserPassword is a function app setting, so I can access it as $env:ContosoUserPassword.
$UserName = "ContosoUser"
$SecuredPassword = ConvertTo-SecureString  $Env:ContosoUserPassword -AsPlainText -Force
$Credential = [System.management.automation.pscredential]::new($UserName, $SecuredPassword)

# This is the name of the hybrid connection Endpoint.
$HybridEndpoint = "finance1"

# The remote server that will be connected to run remote PowerShell commands on
$RemoteServer = "finance2".

Write-Output "Use hybrid connection server as a jump box to connect to a remote machine"

# We are registering an endpoint that runs under credentials ($Credential) that has access to the remote server.
$SessionName = "HybridSession"
$ScriptCommand = {
    param (
        [Parameter(Mandatory=$True)]
        $SessionName)

    if (-not (Get-PSSessionConfiguration -Name $SessionName -ErrorAction SilentlyContinue))
    {
        Register-PSSessionConfiguration -Name $SessionName -RunAsCredential $Using:Credential
    }
}

Write-Output "Registering session on hybrid connection jumpbox"
Invoke-Command -ComputerName $HybridEndpoint `
               -Credential $Credential `
               -Port 5986 `
               -UseSSL `
               -ScriptBlock $ScriptCommand `
               -ArgumentList $SessionName `
               -SessionOption (New-PSSessionOption -SkipCACheck)

# Script to run on the jump box to run against the second machine.
$RemoteScriptCommand = {
    param (
        [Parameter(Mandatory=$True)]
        $ComputerName)
        # Write out the hostname of the hybrid connection server.
        hostname
        # Write out the hostname of the remote server.
        Invoke-Command -ComputerName $ComputerName -Credential $Using:Credential -ScriptBlock {hostname} `
                        -UseSSL -Port 5986 -SessionOption (New-PSSessionOption -SkipCACheck)
}

Write-Output "Running command against remote machine via jumpbox by connecting to the PowerShell configuration session"
Invoke-Command -ComputerName $HybridEndpoint `
               -Credential $Credential `
               -Port 5986 `
               -UseSSL `
               -ScriptBlock $RemoteScriptCommand `
               -ArgumentList $RemoteServer `
               -SessionOption (New-PSSessionOption -SkipCACheck) `
               -ConfigurationName $SessionName
```

Bu komut dosyasındaki aşağıdaki değişkenleri ortamınızdan gelen geçerli değerlerle değiştirin:
* $HybridEndpoint
* $RemoteServer

Önceki iki senaryoda, Azure İşlevleri ve Karma Bağlantılar'da PowerShell'i kullanarak şirket içi ortamlarınızı bağlayabilir ve yönetebilirsiniz. [İşlevlerde Hibrit Bağlantılar](../app-service/app-service-hybrid-connections.md) ve [PowerShell](./functions-reference-powershell.md)hakkında daha fazla bilgi edinmenizi öneririz.

Azure İşlevler aracılığıyla şirket ortamınıza bağlanmak için Azure [sanal ağlarını](./functions-create-vnet.md) da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [PowerShell işlevleriyle çalışma hakkında daha fazla bilgi edinin](functions-reference-powershell.md)
