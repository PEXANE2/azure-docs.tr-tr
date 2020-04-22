---
title: include dosyası
titleSuffix: Azure
description: include dosyası
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: beffb2babefd86c2807e21e9337cba66f42fcfc2
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678475"
---
Yapılandırmaya başlamadan önce, gerekli modülleri yükleyin ve içe aktarın. PowerShell'e modül yüklemek için Yönetici ayrıcalıklarına ihtiyacınız vardır.

1. Az modüllerini yükleyin ve içe aktarın.
    ```powershell
    Install-Module Az -AllowClobber
    Import-Module Az
    ```
1. Az.Peering modüllerini yükleyin ve içe aktarın.
    ```powershell
    Install-Module -Name Az.Peering -AllowClobber
    Import-Module Az.Peering
    ```
1. Bu komutu kullanarak modüllerin düzgün aktarıldığını doğrulayın:
    ```powershell
    Get-Module
    ```
1. Bu komutu kullanarak Azure hesabınızda oturum açın:
    ```powershell
    Connect-AzAccount
    ```
1. Hesabın aboneliklerini denetleyin ve bir eşleme oluşturmak istediğiniz aboneliği seçin.
    ```powershell
    Get-AzSubscription
    Select-AzSubscription -SubscriptionId "subscription-id"
    ```
1. Zaten bir kaynak grubunuz yoksa, bir eşleme oluşturmadan önce bir kaynak oluşturmanız gerekir. Bunu aşağıdaki komutu çalıştırarak yapabilirsiniz:

    ```powershell
    New-AzResourceGroup -Name "PeeringResourceGroup" -Location "Central US"
    ```
> [!IMPORTANT]
> ASN ve aboneliğinizi ilişkilendirmiyorsanız, Eş Eş [ASN'deki](../howto-subscription-association-powershell.md)adımları izleyin. Bu eylem bir akran istemek için gereklidir.

> [!NOTE]
> Kaynak grubunun konumu, bir eşleme ayarlamayı seçtiğiniz konumdan bağımsızdır.
&nbsp;
