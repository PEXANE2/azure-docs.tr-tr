---
title: Microsoft Azure Stack hub hazırlık Denetleyicisi aracını kullanarak sertifika oluşturma | Microsoft Docs
description: Sertifika isteklerinin nasıl oluşturulacağını ve sonra Azure Stack hub hazırlık Denetleyicisi Aracı kullanılarak Azure Stack Edge GPU cihazınızdan sertifika alma ve yüklemeyi açıklar.
services: Azure Stack Edge
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: ea560cf7d61a208cf29aef977aea6d1584a7cdb7
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89146055"
---
# <a name="create-certificates-for-your-azure-stack-edge-using-azure-stack-hub-readiness-checker-tool"></a>Azure Stack hub hazırlık Denetleyicisi aracını kullanarak Azure Stack Edge için sertifikalar oluşturma 

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

Bu makalede Azure Stack hub hazırlık Denetleyicisi Aracı kullanılarak Azure Stack Edge için nasıl sertifika oluşturacağınız açıklanır. 

## <a name="using-azure-stack-hub-readiness-checker-tool"></a>Azure Stack hub hazırlık Denetleyicisi aracını kullanma

Bir Azure Stack Edge cihaz dağıtımı için sertifika Imzalama Istekleri (CSR) oluşturmak üzere Azure Stack hub hazırlık Denetleyicisi aracını kullanın. Bu istekleri, Azure Stack Edge cihazına sipariş ettikten sonra ve cihazın gelmesi için bekleyen bir şekilde oluşturabilirsiniz. 

> [!NOTE]
> Bu aracı yalnızca, üretim cihazları için değil, test veya geliştirme amacıyla kullanın. 

Aşağıdaki sertifikaları istemek için Azure Stack hub hazırlık Denetleyicisi aracı 'nı (AzsReadinessChecker) kullanabilirsiniz:

- Azure Resource Manager sertifikası
- Yerel UI sertifikası
- Düğüm sertifikası
- Blob sertifikası
- VPN sertifikası


## <a name="prerequisites"></a>Önkoşullar

Azure Stack Edge cihaz dağıtımı için CSR 'Ler oluşturmak için aşağıdakileri yaptığınızdan emin olun: 

- Windows 10 veya Windows Server 2016 veya sonraki bir sürümünü çalıştıran bir istemcdiniz. 
- Microsoft Azure Stack hub hazırlık Denetleyicisi Aracı 1.2002.1133.85 Bu sistemdeki [PowerShell Galerisi](https://aka.ms/AzsReadinessChecker) indirdiniz. Bu paketi aramanız gerekebilir. Aracın yalnızca bu sürümü Azure Stack Edge cihazları için sertifika oluşturabilir.
- Sertifikalar için aşağıdaki bilgilere sahipsiniz:
  - Cihaz adı
  - Düğüm seri numarası
  - Dış tam etki alanı adı (FQDN)

## <a name="generate-certificate-signing-requests"></a>Sertifika imzalama istekleri oluşturma

Azure Stack Edge cihaz sertifikalarını hazırlamak için aşağıdaki adımları kullanın:

1. PowerShell 'i yönetici olarak çalıştırın (5,1 veya üzeri).
2. Azure Stack hub hazırlık Denetleyicisi aracını yükler. PowerShell isteminde şunu yazın: 

    ```azurepowershell
    Install-Module -Name Microsoft.AzureStack.ReadinessChecker -RequiredVersion 1.2002.1133.85 -Force
    ```

    Yüklü sürümü doğrulamak için şunu yazın:  

    ```azurepowershell
    Get-InstalledModule -Name Microsoft.AzureStack.ReadinessChecker  | ft Name, Version 
    ```

3. Mevcut değilse, tüm sertifikalar için bir dizin oluşturun. Şunu yazın: 
    
    ```azurepowershell
    New-Item "C:\certrequest" -ItemType Directory
    ``` 
    
4. Bir sertifika isteği oluşturmak için aşağıdaki bilgileri sağlayın. VPN sertifikası oluşturuyorsanız, bu girişlerin bazıları uygulanmaz. 
    
    |Giriş |Açıklama  |
    |---------|---------|
    |`OutputRequestPath`|Sertifika isteklerinin oluşturulmasını istediğiniz yerel istemcinizdeki dosya yolu.        |
    |`DeviceName`|Cihazınızın yerel Web Kullanıcı arabirimindeki **cihazlar** sayfasında Cihazınızın adı. <br> Bu alan, bir VPN sertifikası için gerekli değildir.         |
    |`NodeSerialNumber`|Cihazınızın yerel Web Kullanıcı arabirimindeki **ağ** sayfasında bulunan cihaz düğümünün seri numarası. <br> Bu alan, bir VPN sertifikası için gerekli değildir.       |
    |`ExternalFQDN`|Cihazınızın yerel Web Kullanıcı arabirimindeki **cihazlar** sayfasındaki DNSDomain değeri.         |
    |`RequestType`|İstek türü `MultipleCSR` , çeşitli uç noktalar için farklı sertifikalar veya `SingleCSR` tüm uç noktalar için tek bir sertifika olabilir. <br> Bu alan, bir VPN sertifikası için gerekli değildir.     |

    VPN sertifikası dışındaki tüm sertifikalar için şunu yazın: 
    
    ```azurepowershell
    $edgeCSRparams = @{
        CertificateType = 'AzureStackEdgeDEVICE'
        DeviceName = 'myTEA1'
        NodeSerialNumber = 'VM1500-00025'
        externalFQDN = 'azurestackedge.contoso.com'
        requestType = 'MultipleCSR'
        OutputRequestPath = "C:\certrequest"
    }
    New-AzsCertificateSigningRequest @edgeCSRparams
    ```

    VPN sertifikası oluşturuyorsanız şunu yazın: 

    ```azurepowershell
    $edgeCSRparams = @{
        CertificateType = 'AzureStackEdgeVPN'
        externalFQDN = 'azurestackedge.contoso.com'
        OutputRequestPath = "C:\certrequest"
    }
    New-AzsCertificateSigningRequest @edgeCSRparams
    ```

    
5. Sertifika istek dosyalarını yukarıdaki OutputRequestPath parametresinde belirttiğiniz dizin altında bulabilirsiniz. `MultipleCSR`Parametresini kullanırken, uzantısı olan 4 dosya görürsünüz `.req` . Dosyalar aşağıdaki gibidir:

    
    |Dosya adları  |Sertifika isteği türü  |
    |---------|---------|
    |İle başlangıç `DeviceName`     |Yerel Web Kullanıcı arabirimi sertifika isteği      |
    |İle başlangıç `NodeSerialNumber`     |Düğüm sertifikası isteği         |
    |Başlangıç `login`     |Azure Resource Manager uç noktası sertifika isteği       |
    |Başlangıç `wildcard`     |BLOB depolama sertifikası isteği; cihazda oluşturabileceğiniz tüm depolama hesaplarını kapsadığından bir joker karakter içeriyor.          |
    |Başlangıç `AzureStackEdgeVPNCertificate`     |VPN istemci sertifikası isteği.         |

    Ayrıca bir INF klasörü görürsünüz. Bu bir Management. <Edge-aygı> Information dosyası, sertifika ayrıntılarını açıklayan şifresiz metin olarak içerir.  


6. Bu dosyaları sertifika yetkilinize (iç veya ortak) gönderebilirsiniz. Sertifika YETKILISININ, [düğüm sertifikaları](azure-stack-edge-j-series-manage-certificates.md#node-certificates), [uç nokta SERTIFIKALARı](azure-stack-edge-j-series-manage-certificates.md#endpoint-certificates)ve [Yerel UI sertifikalarına](azure-stack-edge-j-series-manage-certificates.md#local-ui-certificates)yönelik Azure Stack uç sertifika gereksinimlerini karşılayan üretilmiş isteğinizi kullanarak sertifika üretdiğinizden emin olun.

## <a name="prepare-certificates-for-deployment"></a>Dağıtım için sertifikaları hazırlama

Sertifika yetkilinizden (CA) aldığınız sertifika dosyaları içeri aktarılmalıdır ve Azure Stack Edge cihazının sertifika gereksinimleriyle eşleşen özelliklerle birlikte verilmelidir. Aşağıdaki adımları, sertifika imzalama isteklerini oluşturduğunuz sistemde doldurun.

- Sertifikaları içeri aktarmak için [Azure Stack Edge cihazınıza erişen istemcilerde sertifikaları Içeri aktarma](azure-stack-edge-j-series-manage-certificates.md#import-certificates-on-the-client-accessing-the-device)' daki adımları izleyin.

- Sertifikaları dışarı aktarmak için [Azure Stack Edge cihazına erişen istemciden sertifikaları dışarı aktarma](azure-stack-edge-j-series-manage-certificates.md#import-certificates-on-the-client-accessing-the-device)' daki adımları izleyin.


## <a name="validate-certificates"></a>Sertifikaları doğrula

İlk olarak, uygun bir klasör yapısı oluşturacak ve sertifikaları ilgili klasörlere yerleştirtireceksiniz. Yalnızca aracı kullanarak sertifikaları doğrularsınız.

1. PowerShell 'i yönetici olarak çalıştırın.

2. Uygun klasör yapısını oluşturmak için, komut isteminde şunu yazın:

    `New-AzsCertificateFolder -CertificateType AzureStackEdge -OutputPath "$ENV:USERPROFILE\Documents\AzureStackCSR"`

3. PFX parolasını güvenli bir dizeye dönüştürün. Şunu yazın:       

    `$pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString` 

4. Ardından, sertifikaları doğrulayın. Şunu yazın:

    `Invoke-AzsCertificateValidation -CertificateType AzureStackEdge -DeviceName mytea1 -NodeSerialNumber VM1500-00025 -externalFQDN azurestackedge.contoso.com -CertificatePath $ENV:USERPROFILE\Documents\AzureStackCSR\AzureStackEdge -pfxPassword $pfxPassword`

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack Edge cihazınızı dağıtma](azure-stack-edge-gpu-deploy-prep.md)
