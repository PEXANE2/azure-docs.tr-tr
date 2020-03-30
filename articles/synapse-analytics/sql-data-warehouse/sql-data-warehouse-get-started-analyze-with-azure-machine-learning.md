---
title: Azure Machine Learning ile veri çözümleme
description: Azure Sinaps'ta depolanan verilere dayalı bir tahmine dayalı makine öğrenimi modeli oluşturmak için Azure Machine Learning'i kullanın.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/05/2020
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
tag: azure-Synapse
ms.openlocfilehash: 07570fd456d7f5a75a6b5a3ee635605a5d40072a
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350565"
---
# <a name="analyze-data-with-azure-machine-learning"></a>Azure Machine Learning ile veri çözümleme
> [!div class="op_single_selector"]
> * [Güç BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Makine Öğrenimi](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Bu öğretici, Azure Sinaps'ta depolanan verilere dayalı bir tahmine dayalı makine öğrenimi modeli oluşturmak için Azure Machine Learning'i kullanır. Özellikle, bir müşterinin bisiklet alma olasılığı hakkında tahminde bulunarak Adventure Works adlı bisiklet satış mağazası için hedeflenen bir pazarlama kampanyası oluşturulur.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Integrating-Azure-Machine-Learning-with-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticide ilerleyebilmeniz için şunlar gereklidir:

* AdventureWorksDW örnek verileriyle önceden yüklenmiş bir SQL havuzu. Bunu sağlamak için [bir SQL havuzu oluştur'a](create-data-warehouse-portal.md) bakın ve örnek verileri yüklemeyi seçin. Bir veri ambarınız olmasına karşın örnek verileriniz yoksa [örnek verileri elle yükleyebilirsiniz](load-data-from-azure-blob-storage-using-polybase.md).

## <a name="1-get-the-data"></a>1. Verileri alın
Veriler AdventureWorksDW veritabanında bulunan dbo.vTargetMail görünümündedir. Bu verileri okumak için:

1. [Azure Machine Learning Studio](https://studio.azureml.net/)'da oturum açıp denemelerim öğesine tıklayın.
2. Ekranın sol alt kısmında **+Yenİ'yi** tıklatın ve **Boş Deneme'yi**seçin.
3. Denemeniz için bir ad girin: Hedeflenen Pazarlama.
4. Veri Girişi altında **Alma veri** modüllerini sürükleyin ve modül bölmesinden **çıktıyı** tuvale sürükleyin.
5. Özellikler bölmesinde SQL havuzunuzun ayrıntılarını belirtin.
6. İstediğiniz verileri okumak için veritabanı **sorgusunu** belirtin.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Deneme tuvalinin altındaki **Çalıştır'ı** tıklatarak denemeyi çalıştırın.

![Denemeyi çalıştırma](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1-reader-new.png)

Denemeyi çalıştırma işlemi başarıyla sonlandıktan sonra, Okuyucu modülünün altındaki çıkış bağlantı noktasına tıklayıp içeri aktarılan verileri görmek için **Visualiza (Görselleştir)** seçeneğine tıklayın.

![İçeri aktarılan verileri görüntüleme](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3-readerdata-new.png)

## <a name="2-clean-the-data"></a>2. Verileri temizleme
Verileri temizlemek için modelle ilgili olmayan bazı sütunları kaldırın. Bunu yapmak için:

1. **Veri Dönüştürme < Manipülasyon** u altında Veri Kümesi **modülündeki Sütunları Seç'i** tuvale sürükleyin. Bu modülü **Alma Veri** modülüne bağlayın.
2. Hangi sütunları kaldırmak istediğinizi belirtmek için Properties (Özellikler) bölmesindeki **Launch column selector (Sütun seçiciyi başlat)** öğesine tıklayın.

   ![Proje Columns (Proje Sütunları)](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4-projectcolumns-new.png)
3. Şu iki sütunu dışlayın: CustomerAlternateKey ve GeographyKey.

   ![Gereksiz sütunları kaldırma](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5-columnselector-new.png)

## <a name="3-build-the-model"></a>3. Modeli oluşturun
Biz verilerin %80'ini Machine Learning modelini eğitmek ve %20'sini de modeli test etmek üzere kullanacak şekilde 80'e 20 oranında böleceğiz. Bu ikili sınıflandırma problemi için "İki Sınıflı" algoritmaları kullanacağız.

1. **Split (Bölme)** modülünü tuvale sürükleyin.
2. Özellikler bölmesine, ilk çıktı veri kümesindeki satırların Kesir için 0,8 girin.

   ![Verileri eğitim ve test kümesi olarak bölme](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6-split-new.png)
3. **Two-Class Boosted Decision Tree (İki Sınıflı Gelişmiş Karar Ağacı)** modülünü tuvale sürükleyin.
4. Tren **Modeli** modülünü tuvale sürükleyin ve **iki sınıf artırılmış karar ağacı** (ML algoritması) ve **Split** (algoritmayı eğitmek için veri) modüllerine bağlayarak girişleri belirtin. 

     ![Train Model (Model Eğitme) modülünü bağlama](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7-train-new.png)
5. Ardından Properties (Özellikler) bölmesindeki **Launch column selector (Sütun seçiciyi başlat)** öğesine tıklayın. Tahminde bulunulacak sütun olarak **BikeBuyer** sütununu seçin.

   ![Tahminde bulunulacak sütunu seçme](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8-traincolumnselector-new.png)

## <a name="4-score-the-model"></a>4. Puanı model
Şimdi modelin test verileri üzerindeki işlevini test edeceğiz. Hangisinin daha iyi sonuç verdiğini görmek üzere kendi seçtiğimiz algoritmayla başka bir algoritmayı karşılaştıracağız.

1. **Skor Modeli** modülünü tuvale sürükleyin ve **Train Model** ve Split **Data** modüllerine bağlayın.

   ![Modeli puanlama](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9-score-new.png)
2. **Two-Class Bayes Point Machine (İki Sınıflı Bayes Noktası Makinesi)** modülünü deneme tuvaline sürükleyin. Bu algoritma ile Two-Class Boosted Decision Tree'nin (İki Sınıflı Gelişmiş Karar Ağacı'nın) işlevlerini karşılaştıracağız.
3. Train Model (Model Eğitme) ve Score Model (Model Puanlama) modüllerini kopyalayıp tuvale yapıştırın.
4. İki algoritmayı karşılaştırmak için **Evaluate Model (Model Değerlendirme)** modülünü tuvale sürükleyin.
5. Deneyi **çalıştırın.**

   ![Denemeyi çalıştırma](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10-evaluate-new.png)
6. Evaluate Model (Model Değerlendirme) modülünün altında bulunan çıkış bağlantı noktasına ve ardından Visualize (Görselleştir) düğmesine tıklayın.

   ![Değerlendirme sonuçlarını görselleştirme](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11-evalresults-new.png)

Sağlanan ölçümler ROC eğrisi, hassas geri çağırma diyagramı ve kaldırma eğrisidir. Bu ölçümlere bakarak birinci modelin ikinciye göre daha iyi sonuç verdiğini görebiliriz. İlk modelin tahmin ettiği ne lere bakmak için Puan Modeli'nin çıkış bağlantı noktasını tıklatın ve Visualize'ı tıklatın.

![Puanlama sonuçlarını görselleştirme](./media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12-scoreresults-new.png)

Test veri kümenize iki sütunun daha eklendiğini göreceksiniz.

* Puanlanmış Olasılıklar: müşterinin bir bisiklet alıcısı olma olasılığı.
* Puanlanmış Etiketler: model tarafından yapılan sınıflandırma; bisiklet alıcısı (1) veya değil (0). Etiketlemeye ilişkin bu olasılık eşiği %50 olarak belirlenmiş olup ayarlanabilir.

BikeBuyer (gerçek) sütununu Puanlanmış Etiketler (tahmin) ile karşılaştırarak modelin ne derece iyi sonuç verdiğini görebilirsiniz. Ardından, bu modeli yeni müşteriler için öngörülerde bulunmak ve bu modeli bir web hizmeti olarak yayımlamak veya sonuçları Azure Synapse'ye geri yazmak için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Tahmine dayalı makine öğrenimi modellerinin oluşturulmasına ilişkin daha fazla bilgi edinmek için bkz. [Azure'da Machine Learning'e giriş](https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/).