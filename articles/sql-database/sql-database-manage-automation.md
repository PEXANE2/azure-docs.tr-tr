---
title: Azure Otomasyonu ile veritabanlarını yönetme
description: Azure Otomasyon hizmetinin Azure SQL veritabanlarını ölçekte yönetmek için nasıl kullanılabileceğini öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: juliemsft
ms.author: jrasnick
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.openlocfilehash: 9d826a75f05cf2031565f89e21d7f3667ecc8f17
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73822815"
---
# <a name="managing-azure-sql-databases-using-azure-automation"></a>Azure Otomasyonu'ni kullanarak Azure SQL veritabanlarını yönetme

Bu kılavuz, azure otomasyon hizmeti ve Azure SQL veritabanlarınızın yönetimini kolaylaştırmak için nasıl kullanılabileceğini tanıtacaktır.

## <a name="what-is-azure-automation"></a>Azure Otomasyonu Nedir?

[Azure Otomasyon,](https://azure.microsoft.com/services/automation/) süreç otomasyonu yoluyla bulut yönetimini basitleştirmek için bir Azure hizmetidir. Azure Otomasyonu'nu kullanarak, uzun süreli, manuel, hataya açık ve sık sık tekrarlanan görevler, kuruluşunuz için güvenilirliği, verimliliği ve değeri artırma süresini artırmak için otomatikhale getirilebilir.

Azure Otomasyonu, yüksek güvenilirlik ve yüksek kullanılabilirlik sunan bir iş akışı yürütme motoru sağlar ve kuruluşunuz büyüdükçe ihtiyaçlarınızı karşılayacak şekilde ölçeklendirilir. Azure Otomasyonu'nda işlemler, görevlerin tam olarak gerektiğinde gerçekleşmesi için, işlemler el ile, üçüncü taraf sistemler tarafından veya zamanlanmış aralıklarla çalıştırılabilir.

Azure Otomasyonu tarafından otomatik olarak çalıştırılacak bulut yönetimi görevlerinizi taşıyarak iş değeri katan işlere odaklanmaları için BT / DevOps personelini daha da serbest leştirin.

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Azure Otomasyonu Azure SQL veritabanlarının yönetilmesine nasıl yardımcı olabilir?

Azure SQL Veritabanı, [Azure PowerShell araçlarında](/powershell/azure/overview)bulunan [Azure SQL Veritabanı PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/module/servicemanagement/azure/#sql) kullanılarak Azure Otomasyonu'nda yönetilebilir. Azure Automation, hizmet dahilindeki tüm SQL DB yönetim görevlerinizi gerçekleştirebilmeniz için kutudan bu Azure SQL Database PowerShell cmdlet'lere sahiptir. Azure hizmetleri ve üçüncü taraf sistemlerdeki karmaşık görevleri otomatikleştirmek için azure otomasyonundaki bu cmdlet'leri diğer Azure hizmetleri için cmdletlerle eşleştirebilirsiniz.

Azure Automation, PowerShell kullanarak SQL komutları vererek DOĞRUDAN SQL sunucularıyla iletişim kurabilme özelliğine de sahiptir.

[Azure Otomasyon runbook galerisi,](https://azure.microsoft.com/blog/20../../introducing-the-azure-automation-runbook-gallery/) Azure SQL veritabanlarının, diğer Azure hizmetlerinin ve üçüncü taraf sistemlerinin yönetimini otomatikleştirmek için çeşitli ürün ekibi ve topluluk runbook'ları içerir. Galeri runbook'ları şunlardır:

- [SQL sorgularını SQL Server veritabanına karşı çalıştırma](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
- [Bir Azure SQL Veritabanını zamanlamada dikey olarak ölçeklendir (yukarı veya aşağı) ölçeklendirin](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
- [Veritabanı maksimum boyutuna yaklaşırsa bir SQL tablosunu truncate](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
- [Azure SQL Veritabanındaki dizin tabloları çok parçalıysa](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Sonraki adımlar

Azure Otomasyonu'nun temellerini ve Azure SQL veritabanlarını yönetmek için nasıl kullanılabileceğini öğrendiğiniz için, Azure Otomasyonu hakkında daha fazla bilgi edinmek için bu bağlantıları izleyin.

- [Azure Otomasyonuna Genel Bakış](../automation/automation-intro.md)
- [İlk runbook’um](../automation/automation-first-runbook-graphical.md)
- [Azure Otomasyonu: Buluttaki SQL Aracınız](https://azure.microsoft.com/blog/20../../azure-automation-your-sql-agent-in-the-cloud/) 