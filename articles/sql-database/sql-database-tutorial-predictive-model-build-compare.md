---
title: 'Öğretici: Tren ve R tahminmodelleri karşılaştırın'
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Bu üç bölümlük öğretici serinin ikinci bölümünde, Azure SQL Veritabanı Makine Öğrenme Hizmetleri (önizleme) ile R'de iki tahmine dayalı model oluşturacak ve ardından en doğru modeli seçeceksiniz.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 07/26/2019
ms.openlocfilehash: 52f4347fa545df88f11878709b4061662d5faffe
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80345743"
---
# <a name="tutorial-create-a-predictive-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Öğretici: Azure SQL Veritabanı Makine Öğrenme Hizmetleri ile R'de tahmine dayalı bir model oluşturun (önizleme)

Bu üç bölümlük öğretici serinin ikinci bölümünde, R'de iki tahminmodeli oluşturacak ve en doğru modeli seçeceksiniz. Bu serinin bir sonraki bölümünde, bu modeli Azure SQL Database Machine Learning Services (önizleme) içeren bir SQL veritabanında dağıtacaksınız.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

Bu makalede, nasıl öğreneceksiniz:

> [!div class="checklist"]
> * İki makine öğrenme modeli eğitin
> * Her iki modelden de öngörülerde bulunun
> * En doğru modeli seçmek için sonuçları karşılaştırın

[Birinci bölümde,](sql-database-tutorial-predictive-model-prepare-data.md)örnek bir veritabanını nasıl içe aktarabileceğinizi ve daha sonra Verileri R'de bir tahminmodelinin eğitimi için nasıl hazırlayacağınızı öğrendiniz.

[Üçüncü bölümde,](sql-database-tutorial-predictive-model-deploy.md)modeli veritabanında nasıl depoladığınızı ve ardından bir ve ikinci bölümlerde geliştirdiğiniz R komut dosyalarından depolanmış yordamlar oluşturmayı öğreneceksiniz. Depolanan yordamlar, yeni verilere dayalı öngörüler yapmak için bir SQL veritabanında çalışır.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Bu öğretici Bölüm iki bölüm [**bir**](sql-database-tutorial-predictive-model-prepare-data.md) ve ön koşulları tamamlamış varsayar.

## <a name="train-two-models"></a>İki modeli eğitin

Kayak kiralama verileri için en iyi modeli bulmak için iki farklı model (doğrusal regresyon ve karar ağacı) oluşturun ve hangisinin daha doğru tahmin ettiğini görün. Bu serinin birinci `rentaldata` bölümünde oluşturduğunuz veri çerçevesini kullanırsınız.

```r
#First, split the dataset into two different sets:
# one for training the model and the other for validating it
train_data = rentaldata[rentaldata$Year < 2015,];
test_data  = rentaldata[rentaldata$Year == 2015,];

#Use the RentalCount column to check the quality of the prediction against actual values
actual_counts <- test_data$RentalCount;

#Model 1: Use rxLinMod to create a linear regression model, trained with the training data set
model_linmod <- rxLinMod(RentalCount ~  Month + Day + WeekDay + Snow + Holiday, data = train_data);

#Model 2: Use rxDTree to create a decision tree model, trained with the training data set
model_dtree  <- rxDTree(RentalCount ~ Month + Day + WeekDay + Snow + Holiday, data = train_data);
```

## <a name="make-predictions-from-both-models"></a>Her iki modelden de öngörülerde bulunun

Her eğitimli modeli kullanarak kiralama sayılarını tahmin etmek için bir tahmin işlevi kullanın.

```r
#Use both models to make predictions using the test data set.
predict_linmod <- rxPredict(model_linmod, test_data, writeModelVars = TRUE, extraVarsToWrite = c("Year"));

predict_dtree  <- rxPredict(model_dtree,  test_data, writeModelVars = TRUE, extraVarsToWrite = c("Year"));

#To verify it worked, look at the top rows of the two prediction data sets.
head(predict_linmod);
head(predict_dtree);
```

```results
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1         27.45858          42       2     11     4      0       0
2        387.29344         360       3     29     1      0       0
3         16.37349          20       4     22     4      0       0
4         31.07058          42       3      6     6      0       0
5        463.97263         405       2     28     7      1       0
6        102.21695          38       1     12     2      1       0
    RentalCount_Pred  RentalCount  Month  Day  WeekDay  Snow  Holiday
1          40.0000          42       2     11     4      0       0
2         332.5714         360       3     29     1      0       0
3          27.7500          20       4     22     4      0       0
4          34.2500          42       3      6     6      0       0
5         645.7059         405       2     28     7      1       0
6          40.0000          38       1     12     2      1       0
```

## <a name="compare-the-results"></a>Sonuçları karşılaştırın

Şimdi hangi modellerin en iyi tahminleri verdiğini görmek istiyorsunuz. Bunu yapmanın hızlı ve kolay bir yolu, eğitim verilerinizdeki gerçek değerler le öngörülen değerler arasındaki farkı görüntülemek için temel bir çizim işlevi kullanmaktır.

```r
#Use the plotting functionality in R to visualize the results from the predictions
par(mfrow = c(2, 1));
plot(predict_linmod$RentalCount_Pred - predict_linmod$RentalCount, main = "Difference between actual and predicted. rxLinmod");
plot(predict_dtree$RentalCount_Pred  - predict_dtree$RentalCount,  main = "Difference between actual and predicted. rxDTree");
```

![İki modeli karşılaştırma](./media/sql-database-tutorial-predictive-model-build-compare/compare-models.png)

Bu karar ağacı modeli iki model daha doğru gibi görünüyor.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyle devam etmeyecekseniz, TutorialDB veritabanını Azure SQL Veritabanı sunucunuzdan silin.

Azure portalından aşağıdaki adımları izleyin:

1. Azure portalındaki sol menüden Tüm **kaynakları** veya **SQL veritabanlarını**seçin.
1. **Ada göre Filtre...** alanına **TutorialDB'yi**girin ve aboneliğinizi seçin.
1. TutorialDB veritabanınızı seçin.
1. **Genel Bakış** sayfasında **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici serinin ikinci bölümünde, şu adımları tamamladınız:

* İki makine öğrenme modeli eğitin
* Her iki modelden de öngörülerde bulunun
* En doğru modeli seçmek için sonuçları karşılaştırın

Oluşturduğunuz makine öğrenimi modelini dağıtmak için bu öğretici serinin üçüncü bölümünü izleyin:

> [!div class="nextstepaction"]
> [Öğretici: Azure SQL Veritabanı Makine Öğrenme Hizmetleri ile R'de tahmine dayalı bir model dağıtma (önizleme)](sql-database-tutorial-predictive-model-deploy.md)
