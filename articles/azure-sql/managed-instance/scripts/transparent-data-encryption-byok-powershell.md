---
title: 'PowerShell: kendi anahtarını getir (BYOK) TDE etkinleştir'
titleSuffix: Azure SQL Managed Instance
description: Azure SQL yönetilen örneğini, PowerShell kullanarak bekleyen şifreleme için kendi anahtarını getir (BYOK) Saydam Veri Şifrelemesi (TDE) kullanmaya başlamak üzere nasıl yapılandıracağınızı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: vanto, carlrab
ms.date: 11/05/2019
ms.openlocfilehash: 348b69933ae6b3706cf16642a138f1b00247cb5a
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2020
ms.locfileid: "84219202"
---
# <a name="transparent-data-encryption-in-sql-managed-instance-using-your-own-key-from-azure-key-vault"></a>Azure Key Vault kendi anahtarınızı kullanarak SQL yönetilen örneği 'nde Saydam Veri Şifrelemesi

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqlmi.md)]

Bu PowerShell betiği örneği, Azure Key Vault bir anahtar kullanarak Azure SQL yönetilen örneği için müşteri tarafından yönetilen bir anahtarla Saydam Veri Şifrelemesi (TDE) yapılandırır. Bu, genellikle TDE için kendi anahtarını getir (BYOK) senaryosu olarak adlandırılır. Daha fazla bilgi edinmek için bkz. [Azure SQL saydam veri şifrelemesi ile müşteri tarafından yönetilen anahtar](../../database/transparent-data-encryption-byok-overview.md).

## <a name="prerequisites"></a>Önkoşullar

- Mevcut bir yönetilen örnek. Bkz. [yönetilen bir örnek oluşturmak Için PowerShell 'ı kullanma](create-configure-managed-instance-powershell.md).

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

PowerShell 'i yerel olarak veya Azure Cloud Shell kullanarak kullanmak Azure PowerShell 2.3.2 veya sonraki bir sürümü gerektirir. Yükseltmeniz gerekiyorsa, bkz. [Azure PowerShell modülünü Install](/powershell/azure/install-az-ps)veya geçerli kullanıcının modülünü yüklemek için aşağıdaki örnek betiği çalıştırın:

`Install-Module -Name Az -AllowClobber -Scope CurrentUser`

PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-scripts"></a>Örnek betikler

[!code-powershell-interactive[main](../../../../powershell_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.ps1 "Set up BYOK TDE for SQL Managed Instance")]

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

SQL yönetilen örneği için ek PowerShell betiği örnekleri, [Azure SQL yönetilen örnek PowerShell betikleri](../../database/powershell-script-content-guide.md)içinde bulunabilir.
