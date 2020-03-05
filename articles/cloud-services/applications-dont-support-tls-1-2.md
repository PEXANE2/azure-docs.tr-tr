---
title: TLS 1,2 ' i desteklemeyen uygulamaların neden olduğu sorunları giderme | Microsoft Docs
description: TLS 1,2 desteklemeyen uygulamaların neden olduğu sorunları giderme
services: cloud-services
documentationcenter: ''
author: mimckitt
manager: vashan
editor: ''
tags: top-support-issue
ms.assetid: ''
ms.service: cloud-services
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 01/17/2020
ms.author: tagore
ms.openlocfilehash: a9d15a94421694583562f433c20413fcc84697c7
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78270856"
---
# <a name="troubleshooting-applications-that-dont-support-tls-12"></a>TLS 1,2 desteklemeyen uygulamaların sorunlarını giderme
Bu makalede, eski TLS protokollerinin (TLS 1,0 ve 1,1) nasıl etkinleştirileceği ve Windows Server 2019 bulut hizmeti Web ve çalışan rolleri üzerinde ek protokolleri desteklemek üzere eski şifre paketlerinin uygulanması açıklanmaktadır. 

TLS 1,0 ve TLS 1,1 ' nin kullanım dışı bırakılması sırasında müşterilerimizin, kullanımdan kalkmayı planlayabilmeleri için eski protokolleri ve şifre paketlerini desteklemesi gerekebilecek bazı adımları anladık.  Bu eski değerleri yeniden etkinleştirmenizi önermeyiz, ancak müşterilere yardımcı olmaya yönelik rehberlik sunuyoruz. Müşterilerin bu makalede özetlenen değişiklikleri uygulamadan önce gerileme riskini değerlendirmelerini öneririz. 

> [!NOTE]
> Konuk işletim sistemi ailesi 6 sürümleri, 1.0/1.1 şifrelemelerini devre dışı bırakarak TLS 1,2 ' i zorlar. 


## <a name="dropping-support-for-tls-10-tls-11-and-older-cipher-suites"></a>TLS 1,0, TLS 1,1 ve daha eski şifre paketlerinin desteğini bırakma 
Microsoft, sınıfının en iyisi şifrelemeyi kullanma taahhüdünü desteklemek için, Microsoft 2017 'un Haziran 'da TLS 1,0 ve 1,1 ' den uzağa geçiş başlatması için planlar duyurmuştur.   Bu ilk duyuru söz konusu olduğunda, Microsoft, Microsoft Edge ve Internet Explorer 11 ' in desteklenen sürümlerinde varsayılan olarak Aktarım Katmanı Güvenliği 'ni (TLS) 1,0 ve 1,1 ' 2020 nin ilk yarısında devre dışı bırakma amacımızı duyurdu.  Apple, Google ve Mozilla 'daki benzer Duyurular sektörün işaret olduğu yönü belirtir.   

## <a name="tls-configuration"></a>TLS yapılandırması  
Windows Server 2019 Cloud Server görüntüsü, TLS 1,0 ile yapılandırılmıştır ve kayıt defteri düzeyinde TLS 1,1 devre dışı bırakılır. Bu, Windows 'un bu sürümüne dağıtılan ve TLS için Windows Stack ile ilgili uygulamaların TLS 1,0 ve TLS 1,1 iletişimine izin vermeyeceği anlamına gelir.   

Sunucu Ayrıca sınırlı bir şifre paketleri kümesiyle birlikte gelir: 

```
    TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 
    TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 
    TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
    TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 
    TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256 
    TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384 
    TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 
    TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 
```

## <a name="step-1-create-the-powershell-script-to-enable-tls-10-and-tls-11"></a>1\. Adım: TLS 1,0 ve TLS 1,1 ' i etkinleştirmek için PowerShell betiğini oluşturma 

Eski protokolleri ve şifre paketlerini sağlayan bir komut dosyası oluşturmak için aşağıdaki kodu örnek olarak kullanın. Bu belgenin amaçları doğrultusunda, bu betiğin adı: **Tlssettings. ps1**. Sonraki adımlarda kolay erişim için bu betiği yerel masaüstünüzde depolayın. 


```Powershell
#******************* FUNCTION THAT ACTUALLY UPDATES KEYS; WILL RETURN REBOOT FLAG IF CHANGES *********************** 
 
Function Set-CryptoSetting {  
    param (  
        $regKeyName,  
        $value,  
        $valuedata,  
        $valuetype       
    )  
    
    $restart = $false 
  
    # Check for existence of registry key, and create if it does not exist  
    If (!(Test-Path -Path $regKeyName)) {  
        New-Item $regKeyName | Out-Null  
    }  
 
    # Get data of registry value, or null if it does not exist  
    $val = (Get-ItemProperty -Path $regKeyName -Name $value -ErrorAction SilentlyContinue).$value  
 
    If ($val -eq $null) {  
        # Value does not exist - create and set to desired value  
        New-ItemProperty -Path $regKeyName -Name $value -Value $valuedata -PropertyType $valuetype | Out-Null  
        $restart = $true 
    } 
 
    Else {  
        # Value does exist - if not equal to desired value, change it  
        If ($val -ne $valuedata) {  
            Set-ItemProperty -Path $regKeyName -Name $value -Value $valuedata  
            $restart = $true  
        }  
    }  
 
    $restart  
}  
 
#*************************************************************************************************************** 
 
#****************************** CIPHERSUITES FOR OS VERSIONS WINDOWS 10 AND ABOVE ****************************** 
 
function Get-BaseCipherSuitesWin10Above() 
{ 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256," 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384," 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256," 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384," 
 
# Legacy cipher suites 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA_P256," 
        $cipherorder += "TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA_P256," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256," 
        $cipherorder += "TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256," 
        $cipherorder += "TLS_RSA_WITH_AES_256_GCM_SHA384,"  
        $cipherorder += "TLS_RSA_WITH_AES_128_GCM_SHA256,"  
        $cipherorder += "TLS_RSA_WITH_AES_256_CBC_SHA256,"  
        $cipherorder += "TLS_RSA_WITH_AES_128_CBC_SHA256,"  
        $cipherorder += "TLS_RSA_WITH_AES_256_CBC_SHA," 
        $cipherorder += "TLS_RSA_WITH_AES_128_CBC_SHA" 
 
 return $cipherorder 
} 
 

#*************************************************************************************************************** 
 
 
#********************************************** REGISTRY KEYS **************************************************** 
 
 
function Get-RegKeyPathToEnable() 
{ 
    $regKeyPath = @( 
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2",         
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client",  
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" , 
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1",  
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Client", 
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" , 
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0",  
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Client",  
        "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" 
    ) 
    return $regKeyPath 
} 
 
#*************************************************************************************************************** 
 
$localRegistryPath = @() 
 
# Enable TLS 1.2, TLS 1.1 and TLS 1.0 
$localRegistryPath += Get-RegKeyPathToEnable 
 
#******************* CREATE THE REGISTRY KEYS IF THEY DON'T EXIST******************************** 
 
# Check for existence of the registry keys, and create if they do not exist  
For ($i = 0; $i -lt $localRegistryPath.Length; $i = $i + 1) {  
   Write-Host "Checking for existing of key: $($localRegistryPath[$i]) Severity Level: Information"
   If (!(Test-Path -Path $localRegistryPath[$i])) {
        New-Item $localRegistryPath [$i] | Out-Null
    Write-Host "Creating key: $($localRegistryPath[$i]) Severity Level: Information"
    }
}  
 
#********************************* EXPLICITLY Enable TLS12,  TLS11 and TLS10********************************* 
 
For ($i = 0; $i -lt $localRegistryPath.Length; $i = $i + 1) { 
    if ($localRegistryPath[$i].Contains("Client") -Or $localRegistryPath[$i].Contains("Server")) { 
      Write-Host "Enabling this key: $($localRegistryPath[$i]) Severity: Information "
        $result = Set-CryptoSetting $localRegistryPath[$i].ToString() Enabled 1 DWord   
        $result = Set-CryptoSetting $localRegistryPath[$i].ToString() DisabledByDefault 0 DWord  
        $reboot = $reboot -or $result 
    } 
} 
 
#**************************************** SET THE CIPHER SUITE ORDER******************************** 
 
$cipherlist = @() 
 
# Set cipher suite order 
$cipherlist += Get-BaseCipherSuitesWin10Above 
$CipherSuiteRegKey = "HKLM:\SOFTWARE\Policies\Microsoft\Cryptography\Configuration\SSL\00010002"  
 
if (!(Test-Path -Path $CipherSuiteRegKey))  
{  
    New-Item $CipherSuiteRegKey | Out-Null  
    $reboot = $True  
    Write-Host "Creating key: $($CipherSuiteRegKey) Severity: Information "
}  
 
#Set-ItemProperty -Path $CipherSuiteRegKey -Name Functions -Value $cipherorder  
Set-ItemProperty -Path $CipherSuiteRegKey -Name Functions -Value $cipherlist  
#********************************************* REBOOT ******************************************* 
 
Write-Host "A reboot is required in order for changes to effect"  
Write-Host "Rebooting now..."  
shutdown.exe /r /t 5 /c "Crypto settings changed" /f /d p:2:4
```

## <a name="step-2-create-a-command-file"></a>2\. Adım: komut dosyası oluşturma 

Aşağıdaki kullanılarak **Runtlssettings. cmd** ADLı bir cmd dosyası oluşturun. Sonraki adımlarda kolay erişim için bu betiği yerel masaüstünüzde depolayın. 

```cmd
PowerShell -ExecutionPolicy Unrestricted %~dp0TLSsettings.ps1
REM This line is required to ensure the startup tasks does not block the role from starting in case of error.  DO NOT REMOVE!!!! 
EXIT /B 0
```

## <a name="step-3-add-the-startup-task-to-the-roles-service-definition-csdef"></a>Adım 3: başlangıç görevini rolün hizmet tanımına (csdef) ekleme 

Aşağıdaki kod parçacığını mevcut hizmet tanımı dosyanıza ekleyin. 


```
    <Startup> 
        <Task executionContext="elevated" taskType="simple" commandLine="RunTLSSettings.cmd"> 
        </Task> 
    </Startup> 
```

Çalışan rolünü ve Web rolünü gösteren bir örnek aşağıda verilmiştir. 

```
<?xmlversion="1.0"encoding="utf-8"?> 
<ServiceDefinitionname="CloudServiceName"xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition"schemaVersion="2015-04.2.6"> 
    <WebRolename="WebRole1"vmsize="Standard_D1_v2"> 
        <Sites> 
            <Sitename="Web"> 
                <Bindings> 
                    <Bindingname="Endpoint1"endpointName="Endpoint1"/> 
                </Bindings> 
            </Site> 
        </Sites> 
        <Startup> 
            <Taske xecutionContext="elevated" taskType="simple" commandLine="RunTLSSettings.cmd"> 
            </Task> 
        </Startup> 
        <Endpoints> 
            <InputEndpointname="Endpoint1"protocol="http"port="80"/> 
        </Endpoints> 
    </WebRole> 
<WorkerRolename="WorkerRole1"vmsize="Standard_D1_v2"> 
    <Startup> 
        <Task executionContext="elevated" taskType="simple" commandLine="RunTLSSettings.cmd"> 
        </Task> 
    </Startup> 
</WorkerRole> 
</ServiceDefinition> 
```

## <a name="step-5-add-the-scripts-to-your-cloud-service"></a>5\. Adım: bulut hizmetinize betikleri ekleyin 

1) Visual Studio 'da WebRole ' e sağ tıklayın
2) **Ekle**’yi seçin
3) **Varolan öğeyi** Seç
4) Dosya Gezgini 'nde, **Tlssettings. ps1** ve **runtlssettings. cmd** dosyalarını depoladığınız masaüstünüzde gezinin 
5) Cloud Services projenize eklemek için iki dosyayı seçin

## <a name="step-6-enable-copy-to-output-directory"></a>6\. Adım: çıkış dizinine kopyalamayı etkinleştirme

Betiklerin Visual Studio 'dan gönderilen her güncelleştirme ile karşıya yüklendiğinden emin olmak için *Çıkış Dizinine Kopyala* ayarının *her zaman Kopyala* olarak ayarlanması gerekir

1) WebRole ' ın altında RunTLSSettings. cmd ' ye sağ tıklayın.
2) **Özellikleri** seçin
3) Özellikler sekmesinde, *her zaman kopyalamak* Için *Çıkış Dizinine Kopyala* ' yı değiştirin.
4) **Tlssettings. ps1** için adımları yineleyin

## <a name="step-7-publish--validate"></a>7\. Adım: yayımlama & doğrulama

Yukarıdaki adımlar tamamlandığına göre, güncelleştirmeyi mevcut bulut hizmetinize yayımlayın. 

Uç noktalarınızın TLS durumunu doğrulamak için [Ssllabs](https://www.ssllabs.com/) kullanabilirsiniz 

 
