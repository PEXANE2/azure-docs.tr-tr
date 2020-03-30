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
ms.openlocfilehash: 3a5f7157ef8f3645dd03ec93684238dd8bbc067e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75774230"
---
Yapılandırmaya başlamadan önce, gerekli modülleri yükleyin ve içe aktarın. PowerShell'e modül yüklemek için Yönetici ayrıcalıklarına ihtiyacınız olacaktır.

1. Az modül yükleme ve alma
    ```powershell
    Install-Module Az -AllowClobber
    Import-Module Az
    ```
1. Az.Peering modüllerini yükleme ve alma
    ```powershell
    Install-Module -Name Az.Peering -AllowClobber
    Import-Module Az.Peering
    ```
1. Aşağıdaki komutu kullanarak modüllerin ince içe aktarılanın.
    ```powershell
    Get-Module
    ```
1. Aşağıdaki komutu kullanarak Azure hesabınızda oturum açın.
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
> ASN ve aboneliğinizi zaten ilişkilendirmiyorsanız, Eş [İlişkistasN Ile](../howto-subscription-association-powershell.md)ilişkilendirme adımlarını izleyin. Bu bir akran istemek için gereklidir.

> [!NOTE]
> Kaynak grubunun konumu, bir eşleme ayarlamayı seçtiğiniz konumdan bağımsızdır.
&nbsp;
