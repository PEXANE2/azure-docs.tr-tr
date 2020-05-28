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
ms.openlocfilehash: cff98c0acfe06a9dbf9e3d7c7dae1b2411823d28
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84051797"
---
# <a name="transparent-data-encryption-tde-in-a-sql-managed-instance-using-your-own-key-from-azure-key-vault"></a>Azure Key Vault kendi anahtarınızı kullanarak bir SQL yönetilen örneği içinde Saydam Veri Şifrelemesi (TDE)
[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqlmi.md)]

Bu PowerShell betiği örneği, Azure Key Vault bir anahtar kullanarak Azure SQL yönetilen örneği için müşteri tarafından yönetilen anahtarla Saydam Veri Şifrelemesi (TDE) yapılandırır. Bu, genellikle TDE Kendi Anahtarını Getir senaryosu olarak adlandırılır. Müşteri tarafından yönetilen anahtarla TDE hakkında daha fazla bilgi edinmek için bkz. [tde kendi anahtarını getir Azure SQL 'e](../../database/transparent-data-encryption-byok-overview.md).

## <a name="prerequisites"></a>Ön koşullar

- Mevcut bir SQL yönetilen örneği. Bkz. [Azure SQL yönetilen örneği oluşturmak Için PowerShell kullanma](create-configure-managed-instance-powershell.md).

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

PowerShell 'in yerel olarak veya Azure Cloud Shell kullanılarak kullanılması AZ PowerShell 2.3.2 veya sonraki bir sürümü gerektirir. Yükseltmeniz gerekiyorsa, bkz. [Azure PowerShell modülünü Install](/powershell/azure/install-az-ps)veya geçerli kullanıcının modülünü yüklemek için aşağıdaki örnek betiği çalıştırın:

`Install-Module -Name Az -AllowClobber -Scope CurrentUser`

PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-scripts"></a>Örnek betikler

[!code-powershell-interactive[main](../../../../powershell_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.ps1 "Set up BYOK TDE for SQL Managed Instance")]

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL yönetilen örnek PowerShell betiği örnekleri, [Azure SQL yönetilen örnek PowerShell betikleri](../../database/powershell-script-content-guide.md)içinde bulunabilir.
