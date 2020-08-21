---
title: Azure SQL yönetilen örneği 'nde Machine Learning Services (Önizleme)
description: Bu makalede, Azure SQL yönetilen örneği 'ne genel bakış veya Machine Learning Services sağlanmaktadır.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: carlrab, davidph
manager: cgronlun
ms.date: 06/03/2020
ms.openlocfilehash: ad25a9a62757e1e031ce8b93a44e6f2ada4d9964
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88689516"
---
# <a name="machine-learning-services-in-azure-sql-managed-instance-preview"></a>Azure SQL yönetilen örneği 'nde Machine Learning Services (Önizleme)

Machine Learning Services, hem Python hem de R betiklerini destekleyen, veritabanı içi makine öğrenimi sağlayan Azure SQL yönetilen örneği (Önizleme) özelliğidir. Özelliği, yüksek performanslı tahmine dayalı analiz ve makine öğrenimi için Microsoft Python ve R paketleri içerir. İlişkisel veriler saklı yordamlar aracılığıyla betiklerde, Python veya R deyimlerini içeren T-SQL betiği veya T-SQL içeren Python veya R kodu kullanılarak kullanılabilir.

> [!IMPORTANT]
> Machine Learning Services, genel önizleme aşamasında olan bir Azure SQL Yönetilen Örneği özelliğidir.
> Bu Önizleme işlevi başlangıçta, ABD, Asya Avrupa ve Avustralya 'daki sınırlı sayıda bölgede daha sonra eklenen ek bölgelere sahiptir.
>
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> Aşağıdaki [önizlemeye kaydolun](#signup) .

## <a name="what-is-machine-learning-services"></a>Machine Learning Services nedir?

Azure SQL yönetilen örneği 'nde Machine Learning Services, veritabanında Python ve R betikleri çalıştırmanızı sağlar. Bu özelliği kullanarak verileri hazırlayın, temizleyebilir, özellik Mühendisliği yapabilir, makine öğrenimi modellerini bir veritabanı içinde eğitin, değerlendirebilir ve dağıtabilirsiniz. Özelliği, verilerin bulunduğu yerde betikleri çalıştırır ve verilerin ağ üzerinden başka bir sunucuya aktarımını ortadan kaldırır.

Azure SQL yönetilen örneği 'nde R/Python desteğiyle Machine Learning Services kullanın:

- **Veri hazırlama ve genel amaçlı veri işleme yapmak Için r ve Python betiklerini çalıştırın** . artık, r/Python betiklerinizi, verileri başka bir sunucuya taşımak yerine r ve Python betikleri çalıştırmak yerine verilerinizin bulunduğu Azure SQL yönetilen örneğine getirebilirsiniz. Gecikme, güvenlik ve uyumlulukla ilgili veri taşıma ve ilişkili sorunlar gereksinimini ortadan kaldırabilirsiniz.

- **Makine öğrenimi modellerini veritabanında eğitme** -herhangi bir açık kaynak algoritmasını kullanarak modeller eğitebilirsiniz. Veritabanı dışına çıkan örnek veri kümelerine güvenmek yerine, öğreticinizi kolayca veri kümesinin tamamına ölçeklendirebilirsiniz.

- **Modellerinizi ve betikleri saklı yordamlar halinde üretime dağıtma** -betikler ve eğitilen modeller yalnızca T-SQL saklı yordamlarına katıştırarak çalıştırılabilir. Azure SQL yönetilen örneği 'ne bağlanan uygulamalar, yalnızca bir saklı yordam çağırarak, bu modellerdeki tahminlerden ve zekadan yararlanabilir. Ayrıca, yüksek oranda eşzamanlı gerçek zamanlı Puanlama senaryolarında hızlı Puanlama için modelleri kullanıma hazır hale getirmek üzere yerel T-SQL tahmın işlevini de kullanabilirsiniz.

Python ve R 'nin temel dağıtımları Machine Learning Services eklenmiştir. Microsoft paketlerine yönelik iptallerini ve microsoftml 'yi Python için, Microsoft [paketlerine,](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler) [microsoftml](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-microsoftml), [olapr](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-olapr)ve R için [sqlrutils](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-sqlrutils) gibi açık kaynaklı paketleri ve çerçeveleri yükleyebilir ve bu [paketlerin yanı](https://docs.microsoft.com/sql/advanced-analytics/python/ref-py-revoscalepy) sıra, Microsoft paketlerine yönelik iptallerini ve [microsoftml](https://docs.microsoft.com/sql/advanced-analytics/python/ref-py-microsoftml) 'yi de kullanabilirsiniz.

<a name="signup"></a>

## <a name="sign-up-for-the-preview"></a>Önizleme için kaydolun

Bu sınırlı genel önizleme, [Azure önizleme koşullarına](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)tabidir. 

Önizleme programını birleştirmek ve bu koşulları kabul etmek istiyorsanız, konumunda bir Azure destek bileti oluşturarak kayıt isteğinde bulunabilir [**https://azure.microsoft.com/support/create-ticket/**](https://azure.microsoft.com/support/create-ticket/) . 

1. Aşağıdaki seçenekleri belirleyin:
   - Sorun türü- **Teknik**
   - Abonelik- *aboneliğinizi seçin*
   - Hizmet- **SQL veritabanı yönetilen örneği**
   - Özet- *isteğinizin kısa bir açıklamasını girin*
   - Sorun türü- **SQL yönetilen örneği için Machine Learning Services (Önizleme)**
   - Sorun alt türü- **diğer sorun veya "nasıl yapılır" soruları**

1. **İleri: çözümler**' e tıklayın.

1. Önizleme hakkındaki bilgileri okuyun, sonra **Ayrıntılar**' a tıklayın.

1. **Açıklama**' da, önizlemede kaydetmek istediğiniz mantıksal sunucu adı, bölge ve abonelik kimliği dahil olmak üzere isteğinizin ayrıntılarını girin. Diğer ayrıntıları uygun şekilde girin.

1. İşiniz bittiğinde **İleri**' ye tıklayın ve **ardından Oluştur ' a tıklayın.**

Programa kaydınız yapıldıktan sonra Microsoft sizi genel önizleme sürümüne alacak ve var olan veya yeni veritabanınız için Machine Learning Services işlevini etkinleştirecektir.

Genel önizleme sırasında SQL Yönetilen Örneği için Machine Learning Services işlevinin üretim iş yükleri için kullanılması önerilmez.

## <a name="next-steps"></a>Sonraki adımlar

- [SQL Server Machine Learning Services arasındaki önemli farklılıkları](machine-learning-services-differences.md)inceleyin.
- Machine Learning Services 'de Python kullanmayı öğrenmek için bkz. [Python betiklerini çalıştırma](https://docs.microsoft.com/sql/machine-learning/tutorials/quickstart-python-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=sql-server-ver15).
- Machine Learning Services 'de R 'yi nasıl kullanacağınızı öğrenmek için bkz. [Run r Scripts](https://docs.microsoft.com/sql/machine-learning/tutorials/quickstart-r-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=sql-server-ver15).
- Diğer SQL platformlarında makine öğrenimi hakkında daha fazla bilgi için bkz. [SQL Machine Learning belgeleri](https://docs.microsoft.com/sql/machine-learning/).
