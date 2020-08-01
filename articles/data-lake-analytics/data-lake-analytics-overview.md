---
title: Azure Data Lake Analytics’e Genel Bakış
description: Data Lake Analytics, herhangi bir ölçekteki bulut verilerinizden elde edilen içgörüleri kullanarak işinizi yönlendirmenize olanak tanır.
services: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 1e1d443a-48a2-47fb-bc00-bf88274222de
ms.service: data-lake-analytics
ms.topic: overview
ms.date: 06/23/2017
ms.openlocfilehash: b86b569b1b14b943cdc6dcc805a7b7a208d03447
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475128"
---
# <a name="what-is-azure-data-lake-analytics"></a>Azure Data Lake Analytics nedir?

Azure Data Lake Analytics, büyük verileri kolaylaştıran, isteğe bağlı bir analiz işi hizmetidir. Donanım dağıtma, yapılandırma ve ayarlama işlemleri yerine, verilerinizi dönüştürmek ve değerli öngörüleri ayıklamak için sorgular yazarsınız. Analiz hizmeti sadece ne kadar güce ihtiyacınız olduğunu ayarlayarak her ölçekteki işin üstesinden gelmenizi sağlar. Yalnızca işiniz çalıştırıldığında ücret ödersiniz; bu da hizmeti uygun maliyetli kılar. 

## <a name="azure-data-lake-analytics-recent-update-information"></a>Azure Data Lake Analytics son güncelleştirme bilgileri

Azure Data Lake Analytics hizmeti belirli amaçlarla düzenli aralıklarla güncelleştirilir. Bu hizmet için bileşen güncelleştirme, bileşen Beta önizleme vb. desteği sağlamaya devam ediyoruz. 

- Son güncelleştirme genel bilgileri için [Data Lake Analytics yenilikler nelerdir?](data-lake-analytics-whats-new.md)bölümüne bakın.
- Her güncelleştirme ayrıntıları için [Azure Data Lake Analytics sürüm notuna](https://github.com/Azure/AzureDataLake/tree/master/docs/Release_Notes)bakın.

## <a name="dynamic-scaling"></a>Dinamik ölçeklendirme
  
Data Lake Analytics, kaynakları dinamik olarak sağlar ve terabayt ve hatta petabayt boyutlarındaki veriler üzerinde analiz gerçekleştirmenize olanak tanır. Yalnızca kullanılan işlem gücü kadar ödeme yaparsınız. Depolanan verilerin veya kullanılan işlem kaynaklarının boyutunu artırdıkça veya azalttıkça yeniden kod yazmanız gerekmez. 

## <a name="develop-faster-debug-and-optimize-smarter-using-familiar-tools"></a>Alıştığınız araçları kullanarak daha hızlı geliştirin, hataları ayıklayın ve akıllı iyileştirmeler gerçekleştirin
  
Data Lake Analytics, Visual Studio ile derin bir şekilde tümleşir. Kodunuzu çalıştırmak, hatalarını ayıklamak ve ayarlamak için bilindik araçları kullanabilirsiniz. U-SQL işlerinizin görselleştirmeleri, kodunuzun ölçekli olarak nasıl çalıştırıldığını görmenize olanak tanır; bu sayede, kolayca performans sorunlarını tanımlayabilir ve masrafları iyileştirebilirsiniz.

## <a name="u-sql-simple-and-familiar-powerful-and-extensible"></a>U-SQL: Basit ve alışılmış, güçlü ve genişletilebilir
  
Data Lake Analytics, SQL'nin alışılmış, basit ve açıklayıcı yapısını C# dilinin ifade gücüyle genişleten bir sorgu dili olan U-SQL'yi içerir. U-SQL dili, Microsoft'un iç eksabayt ölçekli veri gölünü çalıştıran dağıtılmış çalışma zamanının aynısını kullanır. Artık SQL ve .NET geliştiricileri, tüm verilerini sahip oldukları mevcut becerileri kullanarak işleyebilir ve çözümleyebilir.

## <a name="integrates-seamlessly-with-your-it-investments"></a>BT yatırımlarınızla sınırsız şekilde tümleştirilir
  
Data Lake Analytics kimlik, yönetim ve güvenlik için var olan BT yatırımlarınızı kullanır. Bu yaklaşım, veri yönetimini basitleştirir ve geçerli veri uygulamalarınızı genişletmeyi kolaylaştırır. Data Lake Analytics, kullanıcı yönetimi ve izinleri için Active Directory ile tümleştirilmiştir ve yerleşik izleme ve denetim olanaklarıyla sağlanır.

## <a name="affordable-and-cost-effective"></a>Ekonomik ve uygun maliyetli

Data Lake Analytics, büyük veri iş yüklerini çalıştırmaya yönelik uygun maliyetli bir çözümdür. Veri işlendiğinde, iş başına ücret ödersiniz. Hiçbir donanım, lisans veya hizmete özgü destek sözleşmesi gerekli değildir. Sistem, işin başlayıp tamamlanmasıyla otomatik olarak ölçeğini büyütür veya küçültür; böylece, hiçbir zaman ihtiyacınızdan fazlası için ücret ödemezsiniz. [Maliyetleri denetleme ve tasarruf etme hakkında daha fazla bilgi edinin](https://aka.ms/adlasavemoney).

## <a name="works-with-all-your-azure-data"></a>Tüm Azure verilerinizle çalışır
  
Data Lake Analytics en yüksek performans, aktarım hızı ve paralelleştirme için Azure Data Lake Storage ve Azure depolama Blobları, Azure SQL veritabanı, Azure SYNAPSE Analytics (eskiden Azure SQL veri ambarı) ile birlikte kullanılabilir.


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake Analytics yenilikleri](data-lake-analytics-whats-new.md) kullanarak en son Azure Data Lake Analytics güncelleştirme bölümüne bakın.
* [Azure Portal](data-lake-analytics-get-started-portal.md)  |  [Azure PowerShell](data-lake-analytics-get-started-powershell.md)  |  [CLI](data-lake-analytics-get-started-cli.md) kullanarak Data Lake Analytics kullanmaya başlama
* Azure Data Lake Analytics [Azure Portal](data-lake-analytics-manage-use-portal.md)  |  [Azure PowerShell](data-lake-analytics-manage-use-powershell.md)  |  [CLI](data-lake-analytics-manage-use-cli.md)  |  [Azure .NET SDK 'sını](data-lake-analytics-manage-use-dotnet-sdk.md) kullanarak yönetin  |  [Node.js](data-lake-analytics-manage-use-nodejs.md)
* [Data Lake Analytics ile maliyetleri denetleme ve tasarruf etme](https://1drv.ms/f/s!AvdZLquGMt47h213Hg3rhl-Tym1c)
