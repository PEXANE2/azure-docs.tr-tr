---
title: Azure DevTest Labs kullanımı birden fazla laboratuvar ve abonelikte
description: Azure DevTest Labs kullanımını birden çok laboratuvar ve abonelikte nasıl bildireceklerini öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: tanmayeekamath
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: takamath
ms.openlocfilehash: 912f510f6380c0ba1eb92b7c485091801123558e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169174"
---
# <a name="report-azure-devtest-labs-usage-across-multiple-labs-and-subscriptions"></a>Azure DevTest Labs kullanımını birden çok laboratuvar ve abonelikte bildirin

Çoğu büyük kuruluş, kullanımdaki eğilimleri ve aykırıları görselleştirerek kaynak kullanımını bu kaynaklarla daha etkili olacak şekilde izlemek ister. Kaynak kullanımına bağlı olarak laboratuvar sahipleri veya [yöneticileri, kaynak kullanımını ve maliyetlerini artırmak](https://docs.microsoft.com/azure/billing/billing-getting-started)için laboratuvarları özelleştirebilir. Azure DevTest Labs'da, kullanım kalıplarına daha derin bir tarihsel bakış sağlamak için laboratuvar başına kaynak kullanımını indirebilirsiniz. Bu kullanım desenleri, verimliliği artırmak için değişiklikleri saptamaya yardımcı olabilir. Çoğu işletme, birden çok laboratuvar [ve abonelik](https://docs.microsoft.com/azure/architecture/cloud-adoption/decision-guides/subscriptions/)genelinde hem bireysel laboratuvar kullanımı hem de genel kullanım ister. 

Bu makalede, birden çok laboratuvar ve abonelikarasında kaynak kullanım bilgilerinin nasıl işlenir tartışıldığı açıklanmıştır.

![Kullanım bildirme](./media/report-usage-across-multiple-labs-subscriptions/report-usage.png)

## <a name="individual-lab-usage"></a>Bireysel laboratuvar kullanımı

Bu bölümde, tek bir laboratuvar için kaynak kullanımının nasıl dışa aktarılacağı anlatılıyor.

DevTest Labs'ın kaynak kullanımını dışa aktarmadan önce, kullanım verilerini içeren farklı dosyaların depolanmasına izin vermek için bir Azure Depolama hesabı ayarlamanız gerekir. Veri dışa aktarma işlemini gerçekleştirmenin iki yaygın yolu vardır:

* [DevTest Labs REST API](https://docs.microsoft.com/rest/api/dtl/labs/exportresourceusage) 
* PowerShell Az.Resource modülü [Invoke-AzResourceAction](https://docs.microsoft.com/powershell/module/az.resources/invoke-azresourceaction?view=azps-2.5.0&viewFallbackFrom=azps-2.3.2) eylemi `exportResourceUsage`ile , laboratuar kaynak kimliği ve gerekli parametreler. 

    Kişisel [veri yi dışa aktarma veya silme](personal-data-delete-export.md) makalesi, dışa aktarılan veriler hakkında ayrıntılı bilgi içeren örnek bir PowerShell komut dosyası içerir. 

    > [!NOTE]
    > Tarih parametresi bir zaman damgası içermez, bu nedenle veriler, laboratuvarın bulunduğu saat dilimine göre gece yarısından itibaren her şeyi içerir.

Dışa aktarma tamamlandıktan sonra, farklı kaynak bilgileri ile blob depolama birden fazla CSV dosyaları olacaktır.
  
Şu anda iki CSV dosyaları vardır:

* *virtualmachines.csv* - laboratuvarda sanal makineler hakkında bilgi içerir
* *disks.csv* - laboratuarda farklı diskler hakkında bilgi içerir 

Bu dosyalar laboratuvar adı altında *labresourceusage* blob konteyner saklanır, Lab benzersiz kimliği, çalıştırılan tarih, ve ya tam veya dışa aktarma isteğine dayalı başlangıç tarihi. Örnek bir blob yapısı olacaktır:

* `labresourceusage/labname/1111aaaa-bbbb-cccc-dddd-2222eeee/<End>DD26-MM6-2019YYYY/full/virtualmachines.csv`
* `labresourceusage/labname/1111aaaa-bbbb-cccc-dddd-2222eeee/<End>DD-MM-YYYY/26-6-2019/20-6-2019<Start>DD-MM-YYYY/virtualmachines.csv`

## <a name="exporting-usage-for-all-labs"></a>Tüm laboratuvarlar için kullanım verme

Birden fazla laboratuvar için kullanım bilgilerini dışa aktarmak için 

* [Azure Fonksiyonları](https://docs.microsoft.com/azure/azure-functions/), PowerShell dahil olmak üzere birçok dilde kullanılabilir veya 
* [Azure Otomasyon runbook,](https://docs.microsoft.com/azure/automation/)dışa aktarma kodunu yazmak için PowerShell, Python veya özel bir grafik tasarımcısı kullanın.

Bu teknolojileri kullanarak, belirli bir tarih ve saatte tüm laboratuarlarda tek tek laboratuvar dışa aktarma yürütebilirsiniz. 

Azure işleviniz verileri uzun vadeli depolama alanına itmelidir. Birden çok laboratuvar için veri dışa aktarırken dışa aktarma biraz zaman alabilir. Performansa yardımcı olmak ve bilgilerin çoğaltılması olasılığını azaltmak için, her laboratuarı paralel olarak yürütmenizi öneririz. Paralellik gerçekleştirmek için Azure İşlevlerini eşit olarak çalıştırın. Ayrıca Azure Fonksiyonları'nın sunduğu zamanlayıcı tetikleyicisinden de yararlanın.

## <a name="using-a-long-term-storage"></a>Uzun süreli depolama alanı kullanma

Uzun vadeli depolama, farklı laboratuvarlardan gelen dışa aktarım bilgilerini tek bir veri kaynağında birleştirir. Uzun süreli depolama alanını kullanmanın bir diğer avantajı da yineleme ve maliyeti azaltmak için dosyaları depolama hesabından kaldırabilmektir. 

Uzun süreli depolama, örneğin, herhangi bir metin işleme yapmak için kullanılabilir: 

* dost adlar ekleme
* karmaşık gruplandırmalar oluşturma
* verileri toplama.

Bazı yaygın depolama çözümleri şunlardır: [SQL Server,](https://azure.microsoft.com/services/sql-database/) [Azure Veri Gölü](https://azure.microsoft.com/services/storage/data-lake-storage/)ve [Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). Hangi uzun vadeli depolama çözümlerini seçtiğinizi seçmek tercihe bağlıdır. Verileri görselleştirirken etkileşim kullanılabilirliği açısından ne sunduğuna bağlı olarak aracı seçmeyi düşünebilirsiniz.

## <a name="visualizing-data-and-gathering-insights"></a>Verileri görselleştirme ve içgörü toplama

Kullanım verilerini görüntülemek ve kullanım verimliliğini doğrulamak için öngörüler toplamak için uzun süreli depolama alanınıza bağlanmak için seçtiğiniz bir veri görselleştirme aracını kullanın. Örneğin, [Power BI](https://docs.microsoft.com/power-bi/power-bi-overview) kullanım verilerini düzenlemek ve görüntülemek için kullanılabilir. 

Tek bir konum arabirimi içinde kaynaklarınızı oluşturmak, bağlamak ve yönetmek için [Azure Veri Fabrikası'nı](https://azure.microsoft.com/services/data-factory/) kullanabilirsiniz. Daha fazla denetim gerekiyorsa, tek tek kaynak tek bir kaynak grubu içinde oluşturulabilir ve Veri Fabrikası hizmetinden bağımsız olarak yönetilebilir.  

## <a name="next-steps"></a>Sonraki Adımlar

Sistem kurulduktan ve veriler uzun süreli depolamaya taşındıktan sonra, bir sonraki adım verilerin yanıtlaması gereken soruları bulmaktır. Örnek: 

-   VM boyutu kullanımı nedir?

    Kullanıcılar yüksek performanslı (daha pahalı) VM boyutlarını seçiyor mu?
-   Hangi Market görüntüleri kullanılıyor?

    [Paylaşılan Resim Galerisi](../virtual-machines/windows/shared-image-galleries.md) veya Resim [fabrikası](image-factory-create.md)gibi ortak bir Resim deposu oluşturulmalıdır, özel görüntüler en yaygın VM tabanı mı.
-   Hangi özel görüntüler kullanılıyor veya kullanılmadı?
