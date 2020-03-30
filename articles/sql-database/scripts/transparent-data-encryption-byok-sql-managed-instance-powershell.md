---
title: 'PowerShell: BYOK TDE etkinleştir - Azure SQL Veritabanı Yönetilen Örneği '
description: PowerShell'i kullanarak istirahatte şifreleme için BYOK Saydam Veri Şifrelemesini (TDE) kullanmaya başlamak için Azure SQL Yönetilen Örneği'ni nasıl yapılandırarak nasıl yapılandırılayarılamayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: vanto, carlrab
ms.date: 11/05/2019
ms.openlocfilehash: ddffda5229c9c0d33c563e3ae7b4a884f0f92dff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73691399"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault"></a>Azure Anahtar Kasası'ndan kendi anahtarınızı kullanarak Yönetilen Örnekte Saydam Veri Şifrelemeyi Yönetin

Bu PowerShell komut dosyası örneği, Azure Key Vault'tan bir anahtar kullanarak Saydam Veri Şifrelemesini (TDE) Azure SQL Yönetilen Örneği için müşteri tarafından yönetilen anahtarla yapılandırır. Bu genellikle TDE için Kendi Anahtarınızı Getir senaryosu olarak adlandırılır. Müşteri tarafından yönetilen anahtarla TDE hakkında daha fazla bilgi edinmek için TDE'ye bakın [Kendi Anahtarınızı Azure SQL'e Getirin.](../transparent-data-encryption-byok-azure-sql.md)

## <a name="prerequisites"></a>Ön koşullar

- Varolan Yönetilen Örnek. Bkz. [Azure SQL Veritabanı yönetilen bir örnek oluşturmak için PowerShell'i kullanın.](sql-database-create-configure-managed-instance-powershell.md)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Hem PowerShell'i yerel olarak hem de Azure Cloud Shell'i kullanmak için AZ PowerShell 2.3.2 veya daha sonraki bir sürüm gerekiyor. Yükseltmeniz gerekiyorsa, [Azure PowerShell modüllerini yükle'ye](/powershell/azure/install-az-ps)bakın veya geçerli kullanıcı için modülü yüklemek için aşağıdaki örnek komut dosyasını çalıştırın:

`Install-Module -Name Az -AllowClobber -Scope CurrentUser`

PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-scripts"></a>Örnek komut dosyaları

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.ps1 "Set up BYOK TDE for SQL Managed Instance")]

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
