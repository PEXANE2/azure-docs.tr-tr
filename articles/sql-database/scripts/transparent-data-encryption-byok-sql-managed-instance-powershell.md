---
title: 'PowerShell: BYOK TDE-Azure SQL veritabanı yönetilen örneğini etkinleştirme | Microsoft Docs'
description: Azure SQL yönetilen örneğini, PowerShell kullanarak bekleyen şifreleme için BYOK Saydam Veri Şifrelemesi (TDE) kullanmaya başlamak üzere nasıl yapılandıracağınızı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: vanto, carlrab
ms.date: 11/04/2019
ms.openlocfilehash: 0413216bc666aff504193d6723d46a6ee26be8ee
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73500052"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault"></a>Yönetilen bir örnekteki Saydam Veri Şifrelemesi, Azure Key Vault kendi anahtarınızı kullanarak yönetin

Bu PowerShell betiği örneği, Azure Key Vault bir anahtar kullanarak Azure SQL yönetilen örneği için Kendi Anahtarını Getir (Önizleme) senaryosunda Saydam Veri Şifrelemesi (TDE) yapılandırır. TDE Kendi Anahtarını Getir (BYOK) desteği hakkında daha fazla bilgi edinmek için bkz. [tde kendi anahtarını getir Azure SQL 'e](../transparent-data-encryption-byok-azure-sql.md).

## <a name="prerequisites"></a>Önkoşullar

- Mevcut bir yönetilen örnek. Bkz. [Azure SQL veritabanı yönetilen örneği oluşturmak Için PowerShell kullanma](sql-database-create-configure-managed-instance-powershell.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Her iki PowerShell 'i yerel olarak veya Azure Cloud Shell kullanmak için AZ PowerShell 1.1.1-Preview veya daha sonraki bir önizleme sürümü gerekir. Yükseltmeniz gerekiyorsa, bkz. [Azure PowerShell modülünü Install](/powershell/azure/install-az-ps)veya aşağıdaki örnek betiği çalıştırarak modülü yüklemek için.

`Install-Module -Name Az.Sql -RequiredVersion 1.1.1-preview -AllowPrerelease -Force`

PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-scripts"></a>Örnek komut dosyaları

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.ps1 "Set up BYOK TDE for SQL Managed Instance")]

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
