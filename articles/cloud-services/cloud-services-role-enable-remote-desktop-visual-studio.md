---
title: Visual Studio'u kullanma, Rol Için Uzak Masaüstü'nü etkinleştirin (Azure Bulut Hizmetleri)
description: Azure bulut hizmeti uygulamanızı uzak masaüstü bağlantılarına izin verecek şekilde yapılandırma
services: cloud-services
author: ghogen
manager: jillfra
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.topic: conceptual
ms.workload: azure-vs
ms.date: 03/06/2018
ms.author: ghogen
ms.openlocfilehash: f4622e44c795182ee68c617f335c9e1651d3adcc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294380"
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-visual-studio"></a>Visual Studio'u kullanarak Azure Bulut Hizmetlerinde Rol Için Uzak Masaüstü Bağlantısını Etkinleştirin

> [!div class="op_single_selector"]
> * [Azure portalında](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [Powershell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

Uzak Masaüstü, Azure'da çalışan bir rolün masaüstüne erişmenizi sağlar. Çalışırken uygulamanızla ilgili sorunları gidermek ve tanılamak için Uzak Masaüstü bağlantısını kullanabilirsiniz.

Visual Studio'nun bulut hizmetleri için sağladığı yayımlama sihirbazı, sağladığınız kimlik bilgilerini kullanarak yayımlama işlemi sırasında Uzak Masaüstü'nü etkinleştirme seçeneğini içerir. Visual Studio 2017 sürüm 15.4 ve daha önceki sürümlerini kullanırken bu seçeneği kullanmak uygundur.

Ancak Visual Studio 2017 sürüm 15.5 ve sonraki sürümlerde, yalnızca tek bir geliştirici olarak çalışmadığınız sürece Yayımlama sihirbazı aracılığıyla Uzak Masaüstü'nü etkinleştirmenizi önlemeniz önerilir. Projenin diğer geliştiriciler tarafından açılabileceği herhangi bir durum için, Bunun yerine Uzak Masaüstü'nü Azure portalı, PowerShell üzerinden veya sürekli dağıtım iş akışında bir sürüm ardışık hattından etkinleştirin. Bu öneri, Visual Studio'nun bulut hizmeti VM'de Uzak Masaüstü ile nasıl iletişim kurduğundaki bir değişiklikten kaynaklanmaktadır ve bu makalede açıklandığı gibi.

## <a name="configure-remote-desktop-through-visual-studio-2017-version-154-and-earlier"></a>Visual Studio 2017 sürüm 15.4 ve daha önceki sürüm aracılığıyla Uzak Masaüstünü Yapılandırın

Visual Studio 2017 sürüm 15.4 ve daha önceki sürümlerini kullanırken, yayımlama sihirbazındaki **tüm roller için Uzak Masaüstü etkinleştir** seçeneğini kullanabilirsiniz. Sihirbazı Visual Studio 2017 sürüm 15.5 ve sonraki sürümlerle kullanmaya devam edebilirsiniz, ancak Uzak Masaüstü seçeneğini kullanmayın.

1. Visual Studio'da, Solution Explorer'da bulut hizmeti projenizi sağ tıklayarak ve Yayımla'yı seçerek yayımlama sihirbazını **başlatın.**

2. Gerekirse Azure aboneliğinizde oturum açın ve **İleri'yi**seçin.

3. **Ayarlar** sayfasında, **tüm roller için Uzak Masaüstü'nü Etkinleştir'i**seçin ve ardından Uzak Masaüstü **Yapılandırma** iletişim kutusunu açmak için **Ayarlar...** bağlantısını seçin.

4. İletişim kutusunun alt kısmında **Daha Fazla Seçenek'i**seçin. Bu komut, uzak masaüstü üzerinden bağlanırken kimlik bilgilerini şifreleyebilmeniz için sertifika oluşturduğunuz veya seçtiğiniz bir açılır liste görüntüler.

   > [!Note]
   > Uzak bir masaüstü bağlantısı için ihtiyacınız olan sertifikalar, diğer Azure işlemleri için kullandığınız sertifikalardan farklıdır. Uzaktan erişim sertifikasının özel bir anahtarı olmalıdır.

5. Listeden bir sertifika seçin veya ** &lt;Oluştur'u seçin... &gt;**. Yeni bir sertifika oluşturuyorsanız, istendiğinde yeni sertifika için uygun bir ad sağlayın ve **Tamam'ı**seçin. Yeni sertifika açılır liste kutusunda görünür.

6. Bir kullanıcı adı ve parola sağlayın. Varolan bir hesabı kullanamazsınız. Yeni hesabın kullanıcı adı olarak "Administrator" kullanmayın.

7. Hesabın süresinin dolacağı ve sonra Uzak Masaüstü bağlantılarının engellendiği bir tarih seçin.

8. Gerekli tüm bilgileri sağladıktan sonra **Tamam'ı**seçin. Visual Studio, seçilen sertifikakullanılarak şifrelenen `.cscfg` `.csdef` parola da dahil olmak üzere uzak masaüstü ayarlarını projenize ve dosyalarına ekler.

9. **Sonraki** düğmesini kullanarak kalan adımları tamamlayın ve bulut hizmetinizi yayınlamaya hazır olduğunuzda **Yayımla'yı** seçin. Yayımlamaya hazır değilseniz, değişiklikleri kaydetmek istendiğinde **İptal** et ve **Evet'i** yanıtla'yı seçin. Bulut hizmetinizi daha sonra bu ayarlarla yayımlayabilirsiniz.

## <a name="configure-remote-desktop-when-using-visual-studio-2017-version-155-and-later"></a>Visual Studio 2017 sürüm 15.5 ve sonrası kullanırken Uzak Masaüstünü yapılandırın

Visual Studio 2017 sürüm 15.5 ve sonrası sürüm ile yayımlama sihirbazını bir bulut hizmeti projesiyle kullanmaya devam edebilirsiniz. Yalnızca tek bir geliştirici olarak çalışıyorsanız, **tüm roller için Uzak Masaüstü etkinleştir** seçeneğini de kullanabilirsiniz.

Bir ekibin parçası olarak çalışıyorsanız, bunun yerine [Azure portalı](cloud-services-role-enable-remote-desktop-new-portal.md) veya [PowerShell'i](cloud-services-role-enable-remote-desktop-powershell.md)kullanarak Azure bulut hizmetinde uzak masaüstünü etkinleştirmelisiniz.

Bu öneri, Visual Studio 2017 sürüm 15.5 ve daha sonra bulut hizmeti VM ile iletişim nasıl bir değişiklik kaynaklanmaktadır. Yayımlama sihirbazı aracılığıyla Uzak Masaüstü'nü etkinleştirirken, Visual Studio'nun önceki sürümleri "RDP eklentisi" adı verilen aracılığıyla VM ile iletişim kurar. Visual Studio 2017 sürüm 15.5 ve daha sonra daha güvenli ve daha esnek "RDP uzantısı" kullanarak iletişim kurar. Bu değişiklik, Uzak Masaüstü'nü etkinleştirmek için Azure portalı ve PowerShell yöntemlerinin RDP uzantısını da kullanmasıyla da uyumlu.

Visual Studio RDP uzantısı ile iletişim kurduğunda, TLS üzerinden düz metin şifresi iletir. Ancak, projenin yapılandırma dosyaları yalnızca basit metin deşifre edilebilir yalnızca başlangıçta şifrelemek için kullanılan yerel sertifika ile şifresini deşifre edilebilir şifreli bir parola depolar.

Bulut hizmeti projesini her seferinde aynı geliştirme bilgisayarından dağıtırsanız, bu yerel sertifika kullanılabilir. Bu durumda, yayımlama sihirbazındaki **tüm roller için Uzak Masaüstü etkinleştir** seçeneğini kullanmaya devam edebilirsiniz.

Ancak siz veya diğer geliştiriciler bulut hizmeti projesini farklı bilgisayarlardan dağıtmak istiyorsanız, diğer bilgisayarlar parolanın şifresini çözmek için gerekli sertifikaya sahip olmaz. Sonuç olarak, aşağıdaki hata iletisini görürsünüz:

```output
Applying remote desktop protocol (RDP) extension.
Certificate with thumbprint [thumbprint] doesn't exist.
```

Bulut hizmetini her dağıttyaptığınızda parolayı değiştirebilirsiniz, ancak bu eylem Uzak Masaüstü'nü kullanması gereken herkes için sakıncalı hale gelir.

Projeyi bir ekiple paylaşıyorsanız, yayımlama sihirbazındaki seçeneği temizlemek ve bunun yerine Uzak Masaüstü'nü doğrudan [Azure portalı](cloud-services-role-enable-remote-desktop-new-portal.md) üzerinden veya [PowerShell'i](cloud-services-role-enable-remote-desktop-powershell.md)kullanarak etkinleştirmek en iyisidir.

### <a name="deploying-from-a-build-server-with-visual-studio-2017-version-155-and-later"></a>Visual Studio 2017 sürüm 15.5 ve sonrası ile bir yapı sunucusundan dağıtım

Visual Studio 2017 sürüm 15.5 veya sonraki sürümlerin yapı aracısında yüklü olduğu bir yapı sunucusundan (örneğin, Azure DevOps Hizmetleri ile) bir bulut hizmeti projesi dağıtabilirsiniz. Bu düzenlemeyle dağıtım, şifreleme sertifikasının bulunduğu aynı bilgisayardan gerçekleşir.

Azure DevOps Hizmetleri'nin RDP uzantısını kullanmak için yapı ardışık sisteminize aşağıdaki ayrıntıları ekleyin:

1. Dağıtımın RDP eklentisi yerine RDP uzantısı ile çalıştığından emin olmak için MSBuild bağımsız değişkenlerinizi ekleyin. `/p:ForceRDPExtensionOverPlugin=true` Örnek:

    ```
    msbuild AzureCloudService5.ccproj /t:Publish /p:TargetProfile=Cloud /p:DebugType=None
        /p:SkipInvalidConfigurations=true /p:ForceRDPExtensionOverPlugin=true
    ```

1. Yapı adımlarından sonra **Azure Bulut Hizmeti Dağıtım** adımını ekleyin ve özelliklerini ayarlayın.

1. Dağıtım adımından sonra bir **Azure Powershell** adımı ekleyin, **Ekran adı** özelliğini "Azure Dağıtımını Etkinleştir: RDP Uzantısını Etkinleştir" (veya başka bir uygun ad) olarak ayarlayın ve uygun Azure aboneliğinizi seçin.

1. **Komut Dosyası Türünü** "Satır İçi" olarak ayarlayın ve aşağıdaki kodu Satır **İçi Komut Dosyası** alanına yapıştırın. (Ayrıca bu komut `.ps1` dosyasıyla projenizde bir dosya oluşturabilir, **Komut Dosyası Türü'ni** "Komut Dosyası Yolu" olarak ayarlayabilir ve **Dosya'yı** işaret etmek için Komut Dosyası Yolunu ayarlayabilirsiniz.)

    ```ps
    Param(
        [Parameter(Mandatory=$True)]
        [string]$username,

        [Parameter(Mandatory=$True)]
        [string]$password,

        [Parameter(Mandatory=$True)]
        [string]$serviceName,

        [Datetime]$expiry = ($(Get-Date).AddYears(1))
    )

    Write-Host "Service Name: $serviceName"
    Write-Host "User Name: $username"
    Write-Host "Expiry: $expiry"

    $securepassword = ConvertTo-SecureString -String $password -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential $username,$securepassword

    # Try to remote existing RDP Extensions
    try
    {
        $existingRDPExtension = Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
        if ($existingRDPExtension -ne $null)
        {
            Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
        }
    }
    catch
    {
    }

    Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry -Verbose
    ```

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Uzak Masaüstü'nü kullanarak Azure Rolüne bağlanma

Bulut hizmetinizi Azure'da yayımladıktan ve Uzak Masaüstü'nü etkinleştirdikten sonra, bulut hizmeti VM'ye giriş yapmak için Visual Studio Server Explorer'ı kullanabilirsiniz:

1. Sunucu Gezgini'nde Azure **düğümünü** genişletin ve ardından bir bulut hizmeti nin düğümünü ve rollerinden birini örnek listesini görüntülemek için genişletin.

2. Bir örnek düğümünü sağ tıklatın ve **Uzak Masaüstünü Kullanarak Bağlan'ı**seçin.

3. Daha önce oluşturduğunuz kullanıcı adını ve parolayı girin. Artık uzak oturumunuza giriş yaptınız.

## <a name="additional-resources"></a>Ek kaynaklar

[Cloud Services’ı Yapılandırma](cloud-services-how-to-configure-portal.md)
