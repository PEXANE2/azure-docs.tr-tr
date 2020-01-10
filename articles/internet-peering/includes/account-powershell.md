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
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774230"
---
Yapılandırmaya başlamadan önce gerekli modülleri yükleyip içeri aktarmalısınız. PowerShell 'e modül yüklemek için yönetici ayrıcalıklarına sahip olmanız gerekir.

1. Yükleyip içeri aktar az Module
    ```powershell
    Install-Module Az -AllowClobber
    Import-Module Az
    ```
1. Install ve Import az. eşleme Modülü
    ```powershell
    Install-Module -Name Az.Peering -AllowClobber
    Import-Module Az.Peering
    ```
1. Aşağıdaki komutu kullanarak modüllerin doğru içeri aktarıldığını doğrulayın.
    ```powershell
    Get-Module
    ```
1. Aşağıdaki komutu kullanarak Azure hesabınızda oturum açın.
    ```powershell
    Connect-AzAccount
    ```
1. Hesap için abonelikleri denetleyin ve eşleme oluşturmak istediğiniz aboneliği seçin.
    ```powershell
    Get-AzSubscription
    Select-AzSubscription -SubscriptionId "subscription-id"
    ```
1. Henüz bir kaynak grubunuz yoksa, eşleme oluşturmadan önce bir tane oluşturmanız gerekir. Aşağıdaki komutu çalıştırarak bunu yapabilirsiniz:

    ```powershell
    New-AzResourceGroup -Name "PeeringResourceGroup" -Location "Central US"
    ```
> [!IMPORTANT]
> ASN ve aboneliğiniz henüz ilişkilendirilmediyse, [EŞDÜZEY ASN 'Yi ilişkilendir](../howto-subscription-association-powershell.md)adımlarını izleyin. Eşleme istemek için bu gereklidir.

> [!NOTE]
> Kaynak grubunun konumu, bir eşleme ayarlamayı seçtiğiniz konumdan bağımsızdır.
&nbsp;
