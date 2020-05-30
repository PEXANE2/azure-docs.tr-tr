---
title: Azure Otomasyonu ile veritabanlarını yönetme
description: Azure Otomasyonu hizmetinin Azure SQL veritabanı 'nı ölçekte yönetmek için nasıl kullanılabileceği hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: juliemsft
ms.author: jrasnick
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.openlocfilehash: 527626b1ff423a50f7a6c695c5b2ff2e2f17d640
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84189824"
---
# <a name="manage-databases-in-azure-sql-database-by-using-azure-automation"></a>Azure Otomasyonu 'Nu kullanarak Azure SQL veritabanı 'nda veritabanlarını yönetme

[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Bu kılavuz size Azure Otomasyonu hizmetini ve Azure SQL veritabanı 'nda veritabanlarının yönetimini basitleştirmek için nasıl kullanılabileceğini tanıtacaktır.

## <a name="about-azure-automation"></a>Azure Otomasyonu hakkında

[Azure Otomasyonu](https://azure.microsoft.com/services/automation/) , işlem otomasyonu aracılığıyla bulut yönetimini basitleştirecek bir Azure hizmetidir. Azure Otomasyonu, uzun süre çalışan, el ile, hata açısından açık ve sık tekrarlanan görevler kullanmak, kuruluşunuzun güvenilirliğini, verimliliğini ve değer süresini artırmak için otomatikleştirilebilir. Kullanmaya başlama hakkında bilgi için bkz. [Azure Otomasyonu girişi](../../automation/automation-intro.md)

Azure Otomasyonu, yüksek güvenilirlik ve yüksek kullanılabilirliğe sahip bir iş akışı yürütme altyapısı sağlar ve kuruluşunuz büyüdükçe gereksinimlerinizi karşılayacak şekilde ölçeklendirilir. Azure Otomasyonu 'nda, süreçler el ile, üçüncü taraf sistemlere veya zamanlanan aralıklarda, görevlerin tam olarak gerektiğinde gerçekleşmesini sağlayabilirsiniz.

İşlem yükünü düşürün ve BT/DevOps personelini, bulut yönetim görevlerinizi Azure Otomasyonu tarafından otomatik olarak çalışacak şekilde taşıyarak iş değeri ekleyen işlere odaklanmak üzere boşaltın.

## <a name="how-azure-automation-can-help-manage-your-databases"></a>Azure Otomasyonu veritabanlarınızı yönetmenize nasıl yardımcı olabilir

Azure Otomasyonu ile, Azure SQL veritabanı 'ndaki veritabanlarını [Azure PowerShell araçlarında](/powershell/azure/overview)kullanılabilen [PowerShell cmdlet 'lerini](https://docs.microsoft.com/powershell/module/servicemanagement/azure/#sql) kullanarak yönetebilirsiniz. Azure Otomasyonu, bu Azure SQL veritabanı PowerShell cmdlet 'lerini kullanıma sunulmuştur. bu sayede, hizmet içinde tüm SQL veritabanı yönetim görevlerinizi gerçekleştirebilirsiniz. Bu cmdlet 'leri Azure Otomasyonu 'nda diğer Azure hizmetleri için cmdlet 'lerle de eşleştirerek, Azure hizmetlerinde ve üçüncü taraf sistemlerde karmaşık görevleri otomatikleştirebilirsiniz.

Azure Otomasyonu Ayrıca, PowerShell kullanarak SQL komutları vererek doğrudan SQL sunucularıyla iletişim kurabilir.

[Azure Otomasyonu](../../automation/automation-runbook-gallery.md) için Runbook ve modül galerileri, Microsoft 'un yanı sıra Azure Otomasyonu 'na aktarabileceğiniz topluluk için çeşitli runbook 'lar sunar. Bir tane kullanmak için, Galeriden bir runbook indirin veya Runbook 'tan veya Azure portal Otomasyon hesabınızdan doğrudan runbook 'ları içeri aktarabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Otomasyonu ile ilgili temel bilgileri ve Azure SQL veritabanı 'nı yönetmek için nasıl kullanılabileceğini öğrendiğinize göre, Azure Otomasyonu hakkında daha fazla bilgi edinmek için bu bağlantıları izleyin.

- [Azure Otomasyonu genel bakış](../../automation/automation-intro.md)
- [İlk runbook’um](../../automation/learn/automation-tutorial-runbook-graphical.md)
