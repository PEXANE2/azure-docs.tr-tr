---
title: Azure SYNAPSE Studio (Önizleme) bağlantısı PowerShell sorunlarını giderme
description: PowerShell kullanarak Azure SYNAPSE Studio bağlantısı sorunlarını giderme
author: julieMSFT
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: bbc985407a6cb56f4f1b539f514ab092b5f7d0de
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81431481"
---
# <a name="diagnose-azure-synapse-studio-preview-connectivity-issues-with-powershell-script"></a>PowerShell betiği ile Azure SYNAPSE Studio (Önizleme) bağlantı sorunlarını tanılama

Azure SYNAPSE Studio (Önizleme), bir Web API uç noktası kümesine düzgün şekilde çalışacak şekilde bağlıdır. Bu kılavuz, şu durumlarda bağlantı sorunlarının nedenlerini belirlemenize yardımcı olur:
- Azure SYNAPSE Studio 'Ya erişmek için yerel ağınızı (bir kurumsal güvenlik duvarının arkasındaki ağ gibi) yapılandırma.
- Azure SYNAPSE Studio 'Yu kullanarak bağlantı sorunları yaşıyor.

## <a name="prerequisite"></a>Önkoşul

* Windows üzerinde PowerShell 5,0 veya üzeri sürüm veya
* Windows veya Linux 'ta PowerShell Core 6,0 veya üzeri sürümü.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

Aşağıdaki bağlantıya sağ tıklayın ve "hedefi farklı kaydet" e tıklayın:

- [Test-AzureSynapse. ps1](https://go.microsoft.com/fwlink/?linkid=2119734)

Alternatif olarak, bağlantıyı doğrudan açabilir ve açılan betik dosyasını kaydedebilirsiniz. Daha sonra değiştireolabileceğinden, yukarıdaki bağlantının adresini kaydetmeyin.

Dosya Gezgini 'nde indirilen betik dosyasına sağ tıklayın ve "PowerShell ile Çalıştır" a tıklayın.

![İndirilen betik dosyasını PowerShell ile Çalıştır](media/troubleshooting-synapse-studio-powershell/run-with-powershell.png)

İstendiğinde, şu anda bir sorun yaşayan veya bağlantı için test etmek istediğiniz Azure SYNAPSE çalışma alanı adını girin ve ENTER tuşuna basın.

![Çalışma alanı adını girin](media/troubleshooting-synapse-studio-powershell/enter-workspace-name.png)

Tanılama oturumu başlatılır. Tamamlanmasını bekleyin.

![Tanılamada işlemin tamamlanmasını bekleyin](media/troubleshooting-synapse-studio-powershell/wait-for-diagnosis.png)

Sonda, bir tanılama özeti gösterilir. BILGISAYARıNıZ bir veya daha fazla uç noktaya bağlanamıyorsa, "Özet" bölümünde bazı öneriler gösterilir.

![Tanılama özetini gözden geçir](media/troubleshooting-synapse-studio-powershell/diagnosis-summary.png)

Ayrıca, bu oturum için bir tanılama günlük dosyası, sorun giderme betiği ile aynı klasörde oluşturulur. Konumu "genel ipuçları" bölümünde (`D:\TestAzureSynapse_2020....log`) gösterilir. Gerekirse bu dosyayı teknik desteğe gönderebilirsiniz.

Bir ağ yöneticisiyseniz ve Azure SYNAPSE Studio için güvenlik duvarı yapılandırmanızı ayarlarsanız, "Özet" bölümünün üzerinde gösterilen teknik ayrıntılar size yardımcı olabilir.

* "Geçti" ile işaretlenen tüm test öğeleri (istekler), HTTP durum kodundan bağımsız olarak bağlantı testlerini geçti demektir.
 Başarısız istekler için, nedeni, veya `NamedResolutionFailure` `ConnectFailure`gibi sarı renkle gösterilir. Bu nedenler, ağ ortamınızda yanlış yapılandırma olup olmadığını belirlemenize yardımcı olabilir.


## <a name="next-steps"></a>Sonraki adımlar
Önceki adımlar sorununuzu çözmeye yardımcı değilse, [bir destek bileti oluşturun](../../sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket.md).
