---
title: Sorun Giderme Azure Synapse Studio (önizleme) bağlantısı PowerShell
description: PowerShell'i kullanarak Azure Synapse Studio bağlantısını giderme
author: julieMSFT
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: bbc985407a6cb56f4f1b539f514ab092b5f7d0de
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81431481"
---
# <a name="diagnose-azure-synapse-studio-preview-connectivity-issues-with-powershell-script"></a>PowerShell komut dosyasıyla Azure Synapse Studio (önizleme) bağlantı sorunlarını tanılayın

Azure Synapse Studio (önizleme), düzgün çalışması için bir dizi Web API uç noktasına bağlıdır. Bu kılavuz, şu anda bağlantı sorunlarının nedenlerini belirlemenize yardımcı olur:
- Azure Synapse Studio'ya erişmek için yerel ağınızı (kurumsal güvenlik duvarının arkasındaki ağ gibi) yapılandırma.
- Azure Synapse Studio'su kullanarak bağlantı sorunları yaşıyor.

## <a name="prerequisite"></a>Önkoşul

* Windows'da PowerShell 5.0 veya daha yüksek sürüm veya
* Windows veya Linux'ta PowerShell Core 6.0 veya daha yüksek sürüm.

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

Aşağıdaki bağlantıya sağ tıklayın ve "Hedefi olduğu gibi kaydet"e tıklayın:

- [Test-AzureSynapse.ps1](https://go.microsoft.com/fwlink/?linkid=2119734)

Alternatif olarak, bağlantıyı doğrudan açabilir ve açılan komut dosyası dosyasını kaydedebilirsiniz. İleride değişebileceğinden, yukarıdaki bağlantının adresini kaydetmeyin.

Dosya gezgininde, indirilen komut dosyası dosyasına sağ tıklayın ve "PowerShell ile Çalıştır"ı tıklatın.

![PowerShell ile indirilen komut dosyası dosyasını çalıştırın](media/troubleshooting-synapse-studio-powershell/run-with-powershell.png)

İstendiğinde, şu anda bir sorun yaşayan veya bağlantı için test etmek istediğiniz Azure Synapse çalışma alanı adını girin ve enter tuşuna basın.

![Çalışma alanı adını girin](media/troubleshooting-synapse-studio-powershell/enter-workspace-name.png)

Tanılama oturumuna başlanacak. Tamamlanmasını bekle.

![Tanının tamamlanmasını bekleyin](media/troubleshooting-synapse-studio-powershell/wait-for-diagnosis.png)

Sonunda, bir tanı özeti gösterilir. Bilgisayarınız uç noktalardan birine veya daha fazlasına bağlanamıyorsa, "Özet" bölümünde bazı öneriler gösterir.

![Tanı özetini gözden geçirme](media/troubleshooting-synapse-studio-powershell/diagnosis-summary.png)

Ayrıca, bu oturum için bir tanılama günlüğü dosyası sorun giderme komut dosyası ile aynı klasörde oluşturulur. Konumu "Genel ipuçları" bölümünde gösterilir`D:\TestAzureSynapse_2020....log`( ). Gerekirse bu dosyayı teknik desteğe gönderebilirsiniz.

Bir ağ yöneticisiyseniz ve Azure Synapse Studio için güvenlik duvarı yapılandırmanızı ayarlıyorsanız, yukarıda gösterilen teknik ayrıntılar "Özet" bölümünün size yardımcı olabilir.

* "Geçti" ile işaretlenmiş tüm test öğeleri (istekler), HTTP durum kodundan bağımsız olarak bağlantı testlerinden geçtikleri anlamına gelir.
 Başarısız istekler için, neden sarı olarak `NamedResolutionFailure` gösterilir, gibi veya `ConnectFailure`. Bu nedenler, ağ ortamınızda yanlış yapılandırmalar olup olmadığını anlamanıza yardımcı olabilir.


## <a name="next-steps"></a>Sonraki adımlar
Önceki adımlar sorunu gidermek için yardımcı olmazsa [bir destek bileti oluşturun.](../../sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket.md)
