---
title: İstenilen Durum Yapılandırmasını kullanarak kimlik bilgilerini Azure'a geçirin
description: PowerShell İstenen Durum Yapılandırması 'nı (DSC) kullanarak kimlik bilgilerini Azure sanal makinelerine nasıl güvenli bir şekilde aktarabilirsiniz öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: bobbytreed
manager: carmonm
editor: ''
tags: azure-resource-manager
keywords: Dsc
ms.assetid: ea76b7e8-b576-445a-8107-88ea2f3876b9
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 05/02/2018
ms.author: robreed
ms.openlocfilehash: f7edbd0fd8791829a2d9ffaa4e7c0ee0e561cc5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73748981"
---
# <a name="pass-credentials-to-the-azure-dscextension-handler"></a>Kimlik bilgilerini Azure DSCExtension işleyicisine geçirin

Bu makale, Azure için İstenilen Durum Yapılandırması (DSC) uzantısını kapsar. DSC uzantı işleyicisine genel bir bakış için [bkz.](dsc-overview.md)

 

## <a name="pass-in-credentials"></a>Kimlik bilgilerinde geçiş

Yapılandırma işleminin bir parçası olarak, kullanıcı hesapları ayarlamanız, hizmetlere erişmeniz veya bir programı kullanıcı bağlamında yüklemeniz gerekebilir. Bunları yapmak için kimlik bilgileri sağlamanız gerekir.

Parametreli yapılandırmalar ayarlamak için DSC'yi kullanabilirsiniz. Parametreli yapılandırmada kimlik bilgileri yapılandırmaya aktarılır ve .mof dosyalarında güvenli bir şekilde depolanır. Azure uzantılı işleyici, sertifikaların otomatik yönetimini sağlayarak kimlik bilgisi yönetimini basitleştirir.

Aşağıdaki DSC yapılandırma komut dosyası, belirtilen parolaya sahip yerel bir kullanıcı hesabı oluşturur:

```powershell
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )
    Node localhost {
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        }
    }
}
```

Yapılandırmanın bir parçası olarak **düğüm localhost'u** eklemek önemlidir. Uzantı işleyicisi özellikle **düğüm localhost** deyimini arar. Bu deyim eksikse, aşağıdaki adımlar çalışmaz. Ayrıca typecast **[PsCredential]** dahil etmek önemlidir. Bu özel tür, kimlik bilgisini şifrelemek için uzantıyı tetikler.

Bu komut dosyasını Azure Blob depolama alanına yayımlamak için:

`Publish-AzVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Azure DSC uzantısını ayarlamak ve kimlik belgesini sağlamak için:

```powershell
$configurationName = 'Main'
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = 'user_configuration.ps1.zip'
$vm = Get-AzVM -Name 'example-1'

$vm = Set-AzVMDscExtension -VMName $vm -ConfigurationArchive $configurationArchive -ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzVM
```

## <a name="how-a-credential-is-secured"></a>Kimlik bilgisi nasıl güvenli dir?

Bu kodu niçin çalıştırılması bir kimlik bilgisi ister. Kimlik bilgisi sağlandıktan sonra kısa bir süre bellekte saklanır. Kimlik bilgileri **Set-AzVMDscExtension** cmdlet kullanılarak yayımlandığında, kimlik bilgileri HTTPS üzerinden VM'ye aktarılır. Azure, VM'de yerel VM sertifikasını kullanarak diskte şifrelenmiş kimlik bilgileri depolar. Kimlik bilgisi kısa bir süre bellekte deşifre edilir ve dsc'ye geçirmek için yeniden şifrelenir.

Bu işlem, [uzantı işleyicisi olmadan güvenli yapılandırmaları kullanmaktan](/powershell/scripting/dsc/pull-server/securemof)farklıdır. Azure ortamı, yapılandırma verilerini sertifikalar aracılığıyla güvenli bir şekilde iletmenin bir yolunu sunar. DSC uzantılı işleyicisini kullandığınızda, **ConfigurationData'da** **$CertificatePath** veya **$CertificateID**/ **bir $Thumbprint** girişi sağlamanız gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

- Azure [DSC uzantı işleyicisine giriş](dsc-overview.md)alın.
- [DSC uzantısı için Azure Kaynak Yöneticisi şablonuna](dsc-template.md)göz atın.
- PowerShell DSC hakkında daha fazla bilgi için [PowerShell dokümantasyon merkezine](/powershell/scripting/dsc/overview/overview)gidin.
- PowerShell DSC'yi kullanarak yönetebileceğiniz daha fazla işlevsellik ve daha fazla DSC kaynağı için [PowerShell galerisine](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0)göz atın.