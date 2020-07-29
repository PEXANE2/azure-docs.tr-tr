---
title: Birden çok laboratuvarda ve aboneliğe Azure DevTest Labs kullanımı
description: Birden çok laboratuvarda ve aboneliğe Azure DevTest Labs kullanımı rapor etme hakkında bilgi edinin.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 1d2663113e929145308f5a5712b968f3551668c2
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87287286"
---
# <a name="report-azure-devtest-labs-usage-across-multiple-labs-and-subscriptions"></a>Birden çok laboratuvarda ve aboneliğe ilişkin Azure DevTest Labs kullanımını raporla

Çoğu büyük kuruluş, kullanımdaki eğilimleri ve aykırı değerleri görselleştirerek bu kaynaklarla daha etkili olması için kaynak kullanımını izlemek ister. Kaynak kullanımına göre laboratuvar sahipleri ve yöneticileri, [kaynak kullanımını ve maliyetlerini geliştirmek](../cost-management-billing/manage/getting-started.md)için laboratuvarları özelleştirebilir. Azure DevTest Labs, kullanım desenlerine daha derin bakmak için laboratuvar başına kaynak kullanımını indirebilirsiniz. Bu kullanım desenleri, verimliliği artırmak için değişiklikleri Pinpoint 'e yardımcı olabilir. Çoğu kurum hem bireysel Laboratuvar kullanımı hem de [birden çok laboratuvarda ve aboneliğin](/azure/architecture/cloud-adoption/decision-guides/subscriptions/)genel kullanımını ister. 

Bu makalede, kaynak kullanım bilgilerinin birden çok laboratuvarda ve aboneliğe nasıl işleneceği açıklanır.

![Kullanım bildirme](./media/report-usage-across-multiple-labs-subscriptions/report-usage.png)

## <a name="individual-lab-usage"></a>Bireysel Laboratuvar kullanımı

Bu bölümde, tek bir laboratuvarın kaynak kullanımının nasıl dışarı aktarılacağı açıklanmaktadır.

DevTest Labs kaynak kullanımını dışa aktarmaya başlamadan önce, kullanım verilerinin depolanacağı farklı dosyalara izin vermek için bir Azure depolama hesabı ayarlamanız gerekir. Verilerin dışarı aktarılmasını yürütmek için kullanılan iki yol vardır:

* [DevTest Labs REST API](/rest/api/dtl/labs/exportresourceusage) 
* PowerShell az. Resource Module WITH eylemi, laboratuvar kaynak KIMLIĞI ve gerekli parametreler ile [AzResourceAction komutunu çağırır](/powershell/module/az.resources/invoke-azresourceaction?view=azps-2.5.0&viewFallbackFrom=azps-2.3.2) `exportResourceUsage` . 

    [Kişisel verileri dışarı aktarma veya silme](personal-data-delete-export.md) makalesi, dışarı aktarılmış veriler hakkında ayrıntılı bilgi içeren bir örnek PowerShell betiği içerir. 

    > [!NOTE]
    > Tarih parametresi bir zaman damgası içermez, böylece veriler laboratuvarın bulunduğu saat dilimine bağlı olarak gece yarısından itibaren her şeyi içerir.

Dışarı aktarma işlemi tamamlandıktan sonra, blob depolamada farklı kaynak bilgilerine sahip birden fazla CSV dosyası olur.
  
Şu anda iki CSV dosyası vardır:

* *virtualmachines.csv* -laboratuvardaki sanal makinelerle ilgili bilgileri içerir
* *disks.csv* -laboratuvardaki farklı disklerle ilgili bilgileri içerir 

Bu dosyalar, laboratuvar adı, laboratuvar benzersiz KIMLIĞI, yürütülen tarih ve tam ya da dışa aktarma isteğine bağlı başlangıç tarihi altındaki *labresourceusage* blob kapsayıcısında depolanır. Örnek bir blob yapısı şöyle olabilir:

* `labresourceusage/labname/1111aaaa-bbbb-cccc-dddd-2222eeee/<End>DD26-MM6-2019YYYY/full/virtualmachines.csv`
* `labresourceusage/labname/1111aaaa-bbbb-cccc-dddd-2222eeee/<End>DD-MM-YYYY/26-6-2019/20-6-2019<Start>DD-MM-YYYY/virtualmachines.csv`

## <a name="exporting-usage-for-all-labs"></a>Tüm laboratuvarlara yönelik kullanımı dışarı aktarma

Birden çok laboratuvarın kullanım bilgilerini dışarı aktarmak için kullanmayı düşünün 

* [Azure işlevleri](../azure-functions/index.yml), PowerShell dahil olmak üzere birçok dilde kullanılabilir veya 
* [Azure Otomasyonu runbook](../automation/index.yml)'u, dışa aktarma kodunu yazmak için PowerShell, Python veya özel bir grafik tasarlayıcı kullanın.

Bu teknolojileri kullanarak, belirli bir tarih ve saatte tek tek Laboratuvar dışarı aktarmaları gerçekleştirebilir. 

Azure işleviniz, verileri daha uzun süreli depolamaya göndermelidir. Birden çok laboratuvarda veri verirken dışarı aktarma biraz zaman alabilir. Performansa yardımcı olmak ve bilgilerin çoğaltılme olasılığını azaltmak için her bir laboratuvarı paralel olarak yürütmenizi öneririz. Paralellik gerçekleştirmek için Azure Işlevlerini zaman uyumsuz olarak çalıştırın. Ayrıca, Azure Işlevlerinin sunduğu Zamanlayıcı tetikleyicisinden yararlanın.

## <a name="using-a-long-term-storage"></a>Uzun süreli depolama kullanma

Uzun süreli bir depolama, farklı laboratuvarlardan dışarı aktarma bilgilerini tek bir veri kaynağına birleştirir. Uzun vadeli depolamayı kullanmanın başka bir avantajı, çoğaltma ve maliyeti azaltmak için dosyaları depolama hesabından kaldırabiliyor. 

Uzun vadeli depolama, herhangi bir metin düzenlemesi yapmak için kullanılabilir, örneğin: 

* kolay adlar ekleme
* karmaşık gruplandırmaları oluşturma
* verileri toplama.

Bazı yaygın depolama çözümleri şunlardır: [SQL Server](https://azure.microsoft.com/services/sql-database/), [Azure Data Lake](https://azure.microsoft.com/services/storage/data-lake-storage/)ve [Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). Hangi uzun vadeli depolama çözümünün tercih edileceğini seçme tercihi, tercihe bağlıdır. Verileri görselleştirirken etkileşim kullanılabilirliği açısından ne sunduğuna bağlı olarak aracı seçmeyi düşünebilirsiniz.

## <a name="visualizing-data-and-gathering-insights"></a>Verileri görselleştirme ve öngörüleri toplama

Kullanım verilerini göstermek ve kullanım verimliliğini doğrulamak için öngörüleri toplamak üzere uzun süreli depolamaya bağlanmak için tercih ettiğiniz bir veri görselleştirme aracını kullanın. Örneğin, kullanım verilerini düzenlemek ve göstermek için [Power BI](/power-bi/power-bi-overview) kullanılabilir. 

Tek bir konum arabirimindeki kaynaklarınızı oluşturmak, bağlamak ve yönetmek için [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) kullanabilirsiniz. Daha fazla denetim gerekliyse, tek bir kaynak grubu içinde tek kaynak oluşturulabilir ve Data Factory hizmetten bağımsız olarak yönetilebilir.  

## <a name="next-steps"></a>Sonraki Adımlar

Sistem kurulduktan ve veriler uzun süreli depolamaya taşındıktan sonra, bir sonraki adım, verilerin yanıt vermesi gereken sorularla karşılaşmaktır. Örnek: 

-   VM boyutu kullanımı nedir?

    Kullanıcılar yüksek performanslı (daha pahalı) VM boyutlarını seçmidir?
-   Hangi Market görüntüleri kullanılıyor?

    Özel görüntüler en yaygın VM tabanıyla, ortak bir görüntü deposu [paylaşılan görüntü Galerisi](../virtual-machines/windows/shared-image-galleries.md) veya [görüntü fabrikası](image-factory-create.md)gibi inşa edilmelidir.
-   Hangi özel görüntüler kullanılıyor veya kullanılmıyor?
