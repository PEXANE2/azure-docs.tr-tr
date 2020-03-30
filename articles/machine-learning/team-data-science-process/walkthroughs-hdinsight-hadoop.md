---
title: Hive kullanarak Azure HDInsight Hadoop'ta Analitik - Ekip Veri Bilimi Süreci
description: Tahmine dayalı analitik yapmak için Azure HDInsight Hadoop'ta Hive'ı kullanan Ekip Veri Bilimi Süreci'ne örnekler.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: f72ea6ed5f0eec076d181ef56c99c4f1308a7741
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75864171"
---
# <a name="hdinsight-hadoop-data-science-walkthroughs-using-hive-on-azure"></a>Azure'da Hive'ı kullanarak HDInsight Hadoop veri bilimi walkthroughs 

Bu geçiş ler, tahmine dayalı analizler yapmak için Bir HDInsight Hadoop kümesiyle Hive'ı kullanır. Ekip Veri Bilimi Sürecinde belirtilen adımları izlerler. Ekip Veri Bilimi Sürecine genel bir bakış için [bkz.](overview.md) Azure HDInsight'a giriş için [bkz.](../../hdinsight/hadoop/apache-hadoop-introduction.md)

Ekip Veri Bilimi İşlemi'ni yürüten ek veri bilimi gözden geçirmeleri, kullandıkları **platforma** göre gruplandırılır. Bu örneklerin bir öğeleştirme için [Takım Veri Bilimi İşlemi'ni yürüten Walkthroughs'a](walkthroughs.md) bakın.


## <a name="predict-taxi-tips-using-hive-with-hdinsight-hadoop"></a>HDInsight Hadoop ile Hive kullanarak taksi ipuçlarını tahmin edin

[HDInsight Hadoop kümelerini kullanın,](hive-walkthrough.md) new york taksilerinden gelen verileri tahmin etmek için kullanır: 

- İpucunun ödenip ödenmemesi 
- Bahşiş tutarlarının dağılımı

Senaryo, [Azure HDInsight Hadoop kümesi](https://azure.microsoft.com/services/hdinsight/)ile Hive kullanılarak uygulanır. Kamuya açık bir NYC taksi gezisi ve ücret veri setinden mühendis verilerini nasıl depolayabileceğinizi, keşfedecek ve özellik kölçün. Modelleri oluşturmak ve dağıtmak için Azure Machine Learning'i de kullanırsınız.

## <a name="predict-advertisement-clicks-using-hive-with-hdinsight-hadoop"></a>HDInsight Hadoop ile Hive kullanarak reklam tıklamalarını tahmin edin

[1-TB veri kümesi gözden geçiriminde Azure HDInsight Hadoop Kümelerini kullanın,](hive-criteo-walkthrough.md) bir ipucunun ödenip ödenmediğini ve beklenen tutarları tahmin etmek için herkese açık bir [Criteo](https://labs.criteo.com/downloads/download-terabyte-click-logs/) tıklama veri kümesi kullanır. Senaryo, örnek verileri depolamak, keşfetmek, özellik mühendisi ve aşağı yada kapatmak için [Azure HDInsight Hadoop kümesiyle](https://azure.microsoft.com/services/hdinsight/) Hive kullanılarak uygulanır. Azure Machine Learning'i, kullanıcının bir reklamı tıklayıp tıklamadığını tahmin eden ikili bir sınıflandırma modeli oluşturmak, eğitmek ve puanlamak için kullanır. Bu modellerden birinin Web hizmeti olarak nasıl yayımlandırılabildiğini gösteren iznin sonucu gelir.


## <a name="next-steps"></a>Sonraki adımlar

Ekip Veri Bilimi Süreci'ni oluşturan temel bileşenlerin tartışılması [için, Takım Veri Bilimi Süreci'ne genel bakış](overview.md)bölümüne bakın.

Veri bilimi projelerinizi yapılandırmak için kullanabileceğiniz Ekip Veri Bilimi Süreci yaşam döngüsünü tartışmak [için, Takım Veri Bilimi Süreci yaşam döngüsüne](lifecycle.md)bakın. Yaşam döngüsü, projelerin genellikle yürütüldüğünde takip ettiği adımları, başından sonuna kadar özetler. 

