---
title: PowerShell işlevlerini kullanarak uzak şirket içi kaynakları yönetme
description: Bir PowerShell işlev uygulamasını şirket içi kaynaklara bağlamak için Azure Relay Karma Bağlantılar yapılandırmayı öğrenin. Bu, daha sonra şirket içi kaynağı uzaktan yönetmek için kullanılabilir.
author: eamono
ms.topic: conceptual
ms.date: 04/26/2020
ms.author: eamono
ms.openlocfilehash: 6034d1327d263eda49881af5eedf94ae06495128
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83122281"
---
# <a name="managing-hybrid-environments-with-powershell-in-azure-functions-and-app-service-hybrid-connections"></a>Azure Işlevleri 'nde PowerShell ile karma ortamları yönetme ve App Service Karma Bağlantılar

Azure App Service Karma Bağlantılar özelliği, diğer ağlardaki kaynaklara erişim sağlar. [Karma bağlantılar](../app-service/app-service-hybrid-connections.md) belgelerinde bu yetenek hakkında daha fazla bilgi edinebilirsiniz. Bu makalede, bir şirket içi sunucuyu hedefleyen PowerShell işlevlerini çalıştırmak için bu özelliğin nasıl kullanılacağı açıklanır. Bu sunucu daha sonra, şirket içi ortamdaki tüm kaynakları bir Azure PowerShell işlevinden yönetmek için kullanılabilir.


## <a name="configure-an-on-premises-server-for-powershell-remoting"></a>PowerShell uzaktan iletişim için bir şirket içi sunucu yapılandırma

Aşağıdaki betik PowerShell uzaktan iletişimini mümkün kılar ve yeni bir güvenlik duvarı kuralı ve WinRM HTTPS dinleyicisi oluşturur. Sınama amacıyla, otomatik olarak imzalanan bir sertifika kullanılır. Bir üretim ortamında, imzalı bir sertifika kullanmanızı öneririz.

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

App Service Karma Bağlantılar özelliği yalnızca temel, standart ve yalıtılmış fiyatlandırma planlarında kullanılabilir. PowerShell ile işlev uygulaması oluşturduğunuzda, bu planlardan birini oluşturun veya seçin.

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**'u seçin.

1. **Yeni** sayfasında, **işlem**  >  **işlev uygulaması**' yi seçin.

1. **Temel bilgiler** sayfasında, aşağıdaki tabloda belirtilen işlev uygulaması ayarlarını kullanın.

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ | ---------------- | ----------- |
    | **Abonelik** | Aboneliğiniz | Bu yeni işlev uygulamasının oluşturulduğu abonelik. |
    | **[Kaynak grubu](../azure-resource-manager/management/overview.md)** |  *myResourceGroup* | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. |
    | **İşlev Uygulaması adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler `a-z` (büyük/küçük harf duyarsız), `0-9` ve `-` .  |
    |**Yayımlama**| Kod | Kod dosyalarını veya Docker kapsayıcısını yayımlama seçeneği. |
    | **Çalışma zamanı yığını** | Tercih edilen dil | PowerShell Core ' u seçin. |
    |**Sürüm**| Sürüm numarası | Yüklü çalışma zamanının sürümünü seçin.  |
    |**Geli**| Tercih edilen bölge | Kendinize veya işlevinizin erişeceği diğer hizmetlere yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |

    :::image type="content" source="./media/functions-hybrid-powershell/function-app-create-basics.png" alt-text="İşlev uygulaması oluşturma-temel bilgiler." border="true":::

1. Ileri 'yi seçin **: barındırma**. **Barındırma** sayfasında, aşağıdaki ayarları girin.

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ | ---------------- | ----------- |
    | **[Depolama hesabı](../storage/common/storage-account-create.md)** |  Genel olarak benzersiz bir ad |  İşlev uygulamanız tarafından kullanılan bir depolama hesabı oluşturun. Depolama hesabı adları 3 ila 24 karakter uzunluğunda olmalı ve yalnızca rakamlar ve küçük harfler içerebilir. Ayrıca, [depolama hesabı gereksinimlerini](../azure-functions/functions-scale.md#storage-account-requirements)karşılaması gereken mevcut bir hesabı da kullanabilirsiniz. |
    |**İşletim sistemi**| Tercih edilen işletim sistemi | Çalışma zamanı yığını seçiminize göre sizin için bir işletim sistemi önceden seçilmiştir, ancak gerekirse ayarı değiştirebilirsiniz. |
    | **[Plan türü](../azure-functions/functions-scale.md)** | **App Service planı** | **App Service planı**' nı seçin. Bir App Service planı içinde çalıştırdığınızda [işlev uygulamanızın ölçeklendirmesini](../azure-functions/functions-scale.md) yönetmeniz gerekir.  |

    :::image type="content" source="./media/functions-hybrid-powershell/function-app-create-hosting.png" alt-text="Uygulama barındırma işlevi oluşturun." border="true":::

1. **İleri: izleme**öğesini seçin. **İzleme** sayfasında, aşağıdaki ayarları girin.

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ | ---------------- | ----------- |
    | **[Application Insights](../azure-functions/functions-monitoring.md)** | Varsayılan | En yakın desteklenen bölgede aynı *uygulama adının* Application Insights kaynağını oluşturur. Bu ayarı genişleterek veya **Yeni oluştur**' u seçerek, Application Insights adını değiştirebilir veya verilerinizi depolamak istediğiniz [Azure Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/) 'da farklı bir bölge seçebilirsiniz. |

    :::image type="content" source="./media/functions-hybrid-powershell/function-app-create-monitoring.png" alt-text="İşlev uygulaması Izleme oluşturma." border="true":::

1. Uygulama yapılandırma seçimlerini gözden geçirmek için **gözden geçir + oluştur** ' u seçin.

1. **Gözden geçir + oluştur** sayfasında ayarlarınızı gözden geçirin ve sonra işlev uygulamasını sağlamak ve dağıtmak için **Oluştur** ' u seçin.

1. Portalın sağ üst köşesindeki **Bildirimler** simgesini seçin ve **dağıtım başarılı** iletisini izleyin.

1. Yeni işlev uygulamanızı görüntülemek için **Kaynağa git**’i seçin. **Panoya sabitle ' yi**de seçebilirsiniz. Sabitleme, panonuzdan bu işlev uygulama kaynağına döndürülmesini kolaylaştırır.

## <a name="create-a-hybrid-connection-for-the-function-app"></a>İşlev uygulaması için karma bağlantı oluşturma

Karma bağlantılar, işlev uygulamasının ağ bölümünde yapılandırılır:

1. Az önce oluşturduğunuz işlev uygulamasındaki **Ayarlar** ' ın altında **ağ**' ı seçin. 
1. **Karma bağlantı uç noktalarınızı yapılandırın**' ı seçin.
   
    :::image type="content" source="./media/functions-hybrid-powershell/configure-hybrid-connection-endpoint.png" alt-text="Karma bağlantı uç noktalarını yapılandırın." border="true":::

1. **Karma bağlantı ekle**' yi seçin.
   
    :::image type="content" source="./media/functions-hybrid-powershell/hybrid-connection-overview.png" alt-text="Karma bağlantı ekleyin." border="true":::

1. Aşağıdaki ekran görüntüsünden hemen sonra gösterildiği gibi karma bağlantıyla ilgili bilgileri girin. Uzak komutları çalıştırırken sunucu daha sonra hatırlanmasını kolaylaştırmak için **uç nokta ana bilgisayar** ayarını şirket içi sunucunun ana bilgisayar adıyla eşleştirme seçeneğiniz vardır. Bağlantı noktası, sunucuda daha önce tanımlanan varsayılan Windows Uzaktan Yönetim hizmeti bağlantı noktasıyla eşleşir.
  
      :::image type="content" source="./media/functions-hybrid-powershell/add-hybrid-connection.png" alt-text="Karma bağlantı ekleyin." border="true":::

    | Ayar      | Önerilen değer  |
    | ------------ | ---------------- |
    | **Karma bağlantı adı** | ContosoHybridOnPremisesServer |
    | **Uç nokta Konağı** | finance1 |
    | **Uç nokta bağlantı noktası** | 5986 |
    | **ServiceBus ad alanı** | Yeni Oluştur |
    | **Konum** | Kullanılabilir bir konum seçin |
    | **Adı** | contosopowershellhibrit | 

1. Karma bağlantıyı oluşturmak için **Tamam ' ı** seçin.

## <a name="download-and-install-the-hybrid-connection"></a>Karma bağlantıyı indirme ve yükleme

1. *. Msi* dosyasını bilgisayarınıza yerel olarak kaydetmek için **bağlantı yöneticisini indir** ' i seçin.

    :::image type="content" source="./media/functions-hybrid-powershell/download-hybrid-connection-installer.png" alt-text="Yükleyiciyi indirin." border="true":::

1. *. Msi* dosyasını yerel bilgisayarınızdan şirket içi sunucuya kopyalayın.
1. Hizmeti şirket içi sunucuya yüklemek için Karma Bağlantı Yöneticisi yükleyicisini çalıştırın.

    :::image type="content" source="./media/functions-hybrid-powershell/hybrid-installation.png" alt-text="Karma bağlantıyı yükler." border="true":::

1. Portalda karma bağlantıyı açın ve ağ geçidi bağlantı dizesini panoya kopyalayın.

    :::image type="content" source="./media/functions-hybrid-powershell/copy-hybrid-connection.png" alt-text="Karma bağlantı dizesini kopyalayın." border="true":::

1. Şirket içi sunucuda Karma Bağlantı Yöneticisi Kullanıcı arabirimini açın.

    :::image type="content" source="./media/functions-hybrid-powershell/hybrid-connection-ui.png" alt-text="Karma bağlantı kullanıcı arabirimini açın." border="true":::

1. **El Ile gir** ' i seçin ve bağlantı dizesini panodan yapıştırın.

    :::image type="content" source="./media/functions-hybrid-powershell/enter-manual-connection.png" alt-text="Karma bağlantıyı yapıştırın." border="true":::

1. Karma Bağlantı Yöneticisi, bağlı olarak gösterilmezse PowerShell 'den yeniden başlatın.
    ```powershell
    Restart-Service HybridConnectionManager
    ```

## <a name="create-an-app-setting-for-the-password-of-an-administrator-account"></a>Yönetici hesabının parolası için bir uygulama ayarı oluşturma

1. İşlev uygulamanızın **Ayarlar** bölümünde **yapılandırma**' yı seçin. 
1. **+ Yeni uygulama ayarı**' nı seçin.

    :::image type="content" source="./media/functions-hybrid-powershell/select-configuration.png" alt-text="Yönetici hesabı için bir parola yapılandırın." border="true":::

1. Bu ayarı **Contosouserpassword**olarak adlandırın ve parolayı girin. **Tamam**’ı seçin.
1. Parolayı işlev uygulamasında depolamak için **Kaydet** ' i seçin.

    :::image type="content" source="./media/functions-hybrid-powershell/save-administrator-password.png" alt-text="Yönetici hesabının parolasını kaydedin." border="true":::

## <a name="create-a-function-http-trigger"></a>İşlev HTTP tetikleyicisi oluşturma

1. İşlev uygulamanızda **işlevler**' i seçin ve **+ Ekle**' yi seçin.

    :::image type="content" source="./media/functions-hybrid-powershell/create-http-trigger-function.png" alt-text="Yeni HTTP tetikleyicisi oluştur." border="true":::

1. **Http tetikleyici** şablonunu seçin.

    :::image type="content" source="./media/functions-hybrid-powershell/select-http-trigger-template.png" alt-text="HTTP tetikleyici şablonunu seçin." border="true":::

1. Yeni işlevi adlandırın ve **Işlev oluştur**' u seçin.

    :::image type="content" source="./media/functions-hybrid-powershell/create-new-http-function.png" alt-text="Yeni HTTP tetikleyici işlevini adlandırın ve oluşturun." border="true":::

## <a name="test-the-function"></a>İşlevi test etme

1. Yeni işlevde **kod + test**' i seçin. Şablondaki PowerShell kodunu aşağıdaki kodla değiştirin:

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

1. **Kaydet**’i seçin.

    :::image type="content" source="./media/functions-hybrid-powershell/save-http-function.png" alt-text="PowerShell kodunu değiştirin ve HTTP tetikleyici işlevini kaydedin." border="true":::

 1. **Test**' i seçin ve sonra işlevi test etmek için **Çalıştır** ' ı seçin. Testin başarılı olduğunu doğrulamak için günlükleri gözden geçirin.

     :::image type="content" source="./media/functions-hybrid-powershell/test-function-hybrid.png" alt-text="HTTP tetikleme işlevini test edin." border="true":::

## <a name="managing-other-systems-on-premises"></a>Şirket içi diğer sistemleri yönetme

Yerel ortamdaki diğer sunuculara ve yönetim sistemlerine bağlanmak için bağlı şirket içi sunucuyu kullanabilirsiniz. Bu, PowerShell işlevlerinizi kullanarak Azure 'dan veri merkezi işlemlerinizi yönetmenizi sağlar. Aşağıdaki betik, belirtilen kimlik bilgileri altında çalışan bir PowerShell yapılandırma oturumu kaydeder. Bu kimlik bilgileri, uzak sunuculardaki bir yönetici için olmalıdır. Daha sonra bu yapılandırmayı yerel sunucu veya veri merkezindeki diğer uç noktalara erişmek için kullanabilirsiniz.

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

Bu betikteki aşağıdaki değişkenleri ortamınızdaki ilgili değerlerle değiştirin:
* $HybridEndpoint
* $RemoteServer

Yukarıdaki iki senaryoda, Azure Işlevleri ve Karma Bağlantılar PowerShell kullanarak şirket içi ortamlarınızı bağlanabilir ve yönetebilirsiniz. İşlevlerde [karma bağlantılar](../app-service/app-service-hybrid-connections.md) ve [PowerShell](./functions-reference-powershell.md)hakkında daha fazla bilgi edinmenize önerilir.

Azure [sanal ağlarını](./functions-create-vnet.md) Azure işlevleri aracılığıyla şirket içi ortamınıza bağlanmak için de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [PowerShell işlevleriyle çalışma hakkında daha fazla bilgi edinin](functions-reference-powershell.md)
