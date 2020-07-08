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
ms.openlocfilehash: b5daf283df1ef5d6b42da5bf0a4652aedf2f6284
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84708748"
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

Önizleme programını birleştirmek ve bu koşulları kabul etmek istiyorsanız, konumunda bir Azure destek bileti oluşturarak kayıt isteğinde bulunabilir [https://azure.microsoft.com/support/create-ticket/](https://azure.microsoft.com/support/create-ticket/) . "Sorun türü" için "Teknik" seçeneğini belirleyin, "hizmet" için "SQL veritabanı yönetilen örneği" ni seçin ve "sorun türü" için "diğer" seçeneğini belirleyin. Şu bilgileri de ekleyerek SQL Yönetilen Örneği için Machine Learning sınırlı genel önizleme sürümüne kaydolmak istediğinizi belirten bir istek gönderin: mantıksal sunucu adı, bölge ve abonelik kimliği.

Programa kaydınız yapıldıktan sonra Microsoft sizi genel önizleme sürümüne alacak ve var olan veya yeni veritabanınız için Machine Learning Services işlevini etkinleştirecektir.

Genel önizleme sırasında SQL Yönetilen Örneği için Machine Learning Services işlevinin üretim iş yükleri için kullanılması önerilmez.

## <a name="next-steps"></a>Sonraki adımlar

- [SQL Server Machine Learning Services arasındaki önemli farklılıkları](machine-learning-services-differences.md)inceleyin.
- Machine Learning Services 'de Python kullanmayı öğrenmek için bkz. [Python betiklerini çalıştırma](https://docs.microsoft.com/sql/machine-learning/tutorials/quickstart-python-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=sql-server-ver15).
- Machine Learning Services 'de R 'yi nasıl kullanacağınızı öğrenmek için bkz. [Run r Scripts](https://docs.microsoft.com/sql/machine-learning/tutorials/quickstart-r-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=sql-server-ver15).
- Diğer SQL platformlarında makine öğrenimi hakkında daha fazla bilgi için bkz. [SQL Machine Learning belgeleri](https://docs.microsoft.com/sql/machine-learning/).
