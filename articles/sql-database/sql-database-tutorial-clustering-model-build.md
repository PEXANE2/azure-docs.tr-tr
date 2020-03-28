---
title: "Öğretici: R'de bir kümeleme modeli oluşturma"
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Bu üç bölümlük öğretici serinin ikinci bölümünde, Azure SQL Veritabanı Makine Öğrenme Hizmetleri (önizleme) ile R'de kümeleme gerçekleştirmek için bir K-Means modeli oluşturursunuz.
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
ms.date: 07/29/2019
ms.openlocfilehash: 0a73a2bc3fa76c945cf699133a41b38a9983a234
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80345805"
---
# <a name="tutorial-build-a-clustering-model-in-r-with-azure-sql-database-machine-learning-services-preview"></a>Öğretici: Azure SQL Veritabanı Makine Öğrenme Hizmetleri ile R'de bir kümeleme modeli oluşturun (önizleme)

Bu üç bölümlük öğretici serinin ikinci bölümünde, kümeleme gerçekleştirmek için R bir K-Means modeli oluşturacaksınız. Bu serinin bir sonraki bölümünde, bu modeli Azure SQL Database Machine Learning Services (önizleme) içeren bir SQL veritabanında dağıtacaksınız.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

Bu makalede, nasıl öğreneceksiniz:

> [!div class="checklist"]
> * K-Means algoritması için küme sayısını tanımlama
> * Kümeleme yapma
> * Sonuçları analiz etme

Birinci [bölümde,](sql-database-tutorial-clustering-model-prepare-data.md)kümeleme gerçekleştirmek için azure SQL veritabanından verileri nasıl hazırlayacağınızı öğrendiniz.

[Üçüncü bölümde,](sql-database-tutorial-clustering-model-deploy.md)yeni verilere dayanarak R'de kümeleme gerçekleştirebilen bir Azure SQL veritabanında depolanmış yordamı nasıl oluşturabileceğinizi öğreneceksiniz.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Bu öğretici Bölüm iki bölüm [**bir**](sql-database-tutorial-clustering-model-prepare-data.md) ve ön koşulları tamamlamış varsayar.

## <a name="define-the-number-of-clusters"></a>Küme sayısını tanımlama

Müşteri verilerinizi kümelemek için, verileri gruplandırmanın en basit ve en iyi bilinen yollarından biri olan **K-Means** kümeleme algoritmasını kullanırsınız.
[K-Means clustering algoritması için tam bir kılavuzda K-Means](https://www.kdnuggets.com/2019/05/guide-k-means-clustering-algorithm.html)hakkında daha fazla bilgi edinebilirsiniz.

Algoritma iki girişi kabul eder: veri kendisi ve oluşturmak için küme sayısını temsil eden önceden tanımlanmış bir sayı "*k*" .
Çıktı, kümeler arasında bölümlenmiş giriş verileriyle *k* kümeleridir.

Algoritmanın kullanacağı küme sayısını belirlemek için, ayıklanan küme sayısına göre karelerin içindeki grupgrupları toplamının bir çizimini kullanın. Kullanılacak uygun küme sayısı, çizimin bükülme veya "dirseği"ndedir.

```r
# Determine number of clusters by using a plot of the within groups sum of squares,
# by number of clusters extracted. 
wss <- (nrow(customer_data) - 1) * sum(apply(customer_data, 2, var))
for (i in 2:20)
    wss[i] <- sum(kmeans(customer_data, centers = i)$withinss)
plot(1:20, wss, type = "b", xlab = "Number of Clusters", ylab = "Within groups sum of squares")
```

![Dirsek grafiği](./media/sql-database-tutorial-clustering-model-build/elbow-graph.png)

Grafik dayanarak, *k = 4* denemek için iyi bir değer olacağını görünüyor. Bu *k* değeri müşterileri dört kümeye gruplayacaktır.

## <a name="perform-clustering"></a>Kümeleme yapma

Aşağıdaki R komut dosyasında, RevoScaleR paketinde K-Means işlevi olan **rxKmeans**işlevini kullanacaksınız.

```r
# Output table to hold the customer group mappings.
# This is a table where the cluster mappings will be saved in the database.
return_cluster = RxSqlServerData(table = "return_cluster", connectionString = connStr);
# Set the seed for the random number generator for predictability
set.seed(10);
# Generate clusters using rxKmeans and output key / cluster to a table in SQL database
# called return_cluster
clust <- rxKmeans( ~ orderRatio + itemsRatio + monetaryRatio + frequency,
                   customer_returns,
                   numClusters=4,
                   outFile=return_cluster,
                   outColName="cluster",
                   extraVarsToWrite=c("customer"),
                   overwrite=TRUE);

# Read the customer returns cluster table from the database
customer_cluster <- rxDataStep(return_cluster);
```

## <a name="analyze-the-results"></a>Sonuçları analiz etme

Şimdi K-Means kullanarak kümeleme yaptık, bir sonraki adım sonucu analiz etmek ve herhangi bir işlem yapılabilir bilgi bulabilirsiniz görmektir.

**Clust** nesnesi K-Means kümeleme sonuçlarını içerir.

```r
#Look at the clustering details to analyze results
clust
```

```results
Call:
rxKmeans(formula = ~orderRatio + itemsRatio + monetaryRatio + 
    frequency, data = customer_returns, outFile = return_cluster, 
    outColName = "cluster", extraVarsToWrite = c("customer"), 
    overwrite = TRUE, numClusters = 4)
Data: customer_returns
Number of valid observations: 37336
Number of missing observations: 0 
Clustering algorithm:  

K-means clustering with 4 clusters of sizes 31675, 671, 2851, 2139
Cluster means:
   orderRatio   itemsRatio monetaryRatio frequency
1 0.000000000 0.0000000000    0.00000000  0.000000
2 0.007451565 0.0000000000    0.04449653  4.271237
3 1.008067345 0.2707821817    0.49515232  1.031568
4 0.000000000 0.0004675082    0.10858272  1.186068
Within cluster sum of squares by cluster:
         1          2          3          4
    0.0000  1329.0160 18561.3157   363.2188
```

Dört küme [anlamı, birinci bölümde](sql-database-tutorial-clustering-model-prepare-data.md#separate-customers)tanımlanan değişkenler kullanılarak verilmiştir:

* *orderRatio* = iade sipariş oranı (toplam sipariş sayısına karşılık kısmen veya tamamen iade edilen toplam sipariş sayısı)
* *itemsRatio* = iade madde oranı (geri dönen toplam madde sayısı ile satın alınan madde sayısı)
* *monetaryRatio* = iade tutarı oranı (iade edilen kalemlerin toplam parasal tutarı ile satın alınan tutar)
* *frekans* = dönüş frekansı

K-Means kullanarak veri madenciliği genellikle sonuçların daha fazla analizini ve her kümeyi daha iyi anlamak için daha fazla adım gerektirir, ancak bazı iyi müşteri adayları sağlayabilir.
Bu sonuçları yorumlamanın birkaç yolu şunlardır:

* Küme 1 (en büyük küme) etkin olmayan bir müşteri grubu gibi görünüyor (tüm değerler sıfırdır).
* Küme 3, dönüş davranışı açısından öne çıkan bir grup gibi görünün.

## <a name="clean-up-resources"></a>Kaynakları temizleme

***Bu öğreticiye devam etmeyecekseniz,*** Azure SQL Veritabanı sunucunuzdaki tpcxbb_1gb veritabanını silin.

Azure portalından aşağıdaki adımları izleyin:

1. Azure portalındaki sol menüden Tüm **kaynakları** veya **SQL veritabanlarını**seçin.
1. **Ada göre Filtre'ye...** alanına **tpcxbb_1gb**girin ve aboneliğinizi seçin.
1. **tpcxbb_1gb** veritabanınızı seçin.
1. **Genel Bakış** sayfasında **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici serinin ikinci bölümünde, şu adımları tamamladınız:

* K-Means algoritması için küme sayısını tanımlama
* Kümeleme yapma
* Sonuçları analiz etme

Oluşturduğunuz makine öğrenimi modelini dağıtmak için bu öğretici serinin üçüncü bölümünü izleyin:

> [!div class="nextstepaction"]
> [Öğretici: Azure SQL Veritabanı Makine Öğrenme Hizmetleri ile R'de bir kümeleme modeli dağıtma (önizleme)](sql-database-tutorial-clustering-model-deploy.md)