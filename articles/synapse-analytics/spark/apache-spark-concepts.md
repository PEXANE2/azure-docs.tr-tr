---
title: Azure Synapse Analytics'te Apache Spark - Temel Kavramlar
description: Bu makale, Azure Synapse Analytics'te Apache Spark'a ve farklı kavramlara giriş sağlar.
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: euang
ms.reviewer: euang
ms.openlocfilehash: 3cf654e77bf68c5194a0213d4452242b5c44e234
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81423673"
---
# <a name="apache-spark-in-azure-synapse-analytics-core-concepts"></a>Azure Synapse Analytics Temel Konseptlerinde Apache Spark

Apache Spark, büyük veri analitik uygulamaların performansını artırmak için bellek içi işlemeyi destekleyen paralel bir işleme çerçevesidir. Azure Synapse Analytics'teki Apache Spark, Microsoft'un Bulut'taki Apache Spark uygulamalarından biridir. 

Azure Synapse, Azure'da Spark özellikleri oluşturmayı ve yapılandırmayı kolaylaştırır. Azure Synapse, burada belgelenen bu Spark özelliklerinin farklı bir uygulamasını sağlar.

## <a name="spark-pools-preview"></a>Kıvılcım havuzları (önizleme)

Azure portalında bir Kıvılcım havuzu (önizleme) oluşturulur. Anlık olarak verileri işleyen bir Kıvılcım örneği oluşturmak için kullanılan bir Kıvılcım havuzunun tanımıdır. Bir Kıvılcım havuzu oluşturulduğunda, yalnızca meta veri olarak bulunur; hiçbir kaynak tüketilmez, çalıştırılır veya ücretlendirilmez. Kıvılcım havuzu, Bir Kıvılcım örneğinin özelliklerini kontrol eden bir dizi özelliktedir; bu özellikler ad, boyut, ölçekleme davranışı, yaşama zamanı içerir ancak bunlarla sınırlı değildir.

Spark havuzları oluşturmayla ilişkili bir dolar veya kaynak maliyeti olmadığından, herhangi bir sayı farklı yapılandırmaları herhangi bir sayı ile oluşturulabilir. İzinler, kullanıcıların yalnızca bazılarına erişmesine izin veren Spark havuzlarına da uygulanabilir, diğerlerine değil.

En iyi yöntem, geliştirme ve hata ayıklama için kullanılabilecek daha küçük Spark havuzları ve daha sonra üretim iş yüklerini çalıştırmak için daha büyük havuzlar oluşturmaktır.

Spark havuzu oluşturmayı okuyabilir ve tüm özelliklerini buradan görebilirsiniz [Synapse Analytics'teki Spark havuzları ile başlayın](apache-spark-notebook-create-spark-use-sql.md#create-an-apache-spark-pool)

## <a name="spark-instances"></a>Kıvılcım örnekleri

Bir Kıvılcım havuzuna bağlandığınızda, oturum oluşturduğunuzda ve bir iş çalıştırdığınızda, kıvılcım örnekleri oluşturulur. Birden çok kullanıcı tek bir Spark havuzuna erişebildiğinizden, bağlanan her kullanıcı için yeni bir Spark örneği oluşturulur. 

İkinci bir iş gönderdiğinizzaman, havuzda kapasite varsa, varolan Kıvılcım örneğinin de kapasitesi vardır, varolan örnek işi işleyecek; değilse ve havuz düzeyinde kapasite varsa, yeni bir Kıvılcım örneği oluşturulur.

## <a name="examples"></a>Örnekler

### <a name="example-1"></a>Örnek 1

- SP1 adında bir Kıvılcım havuzu oluşturursunuz; 20 düğüm sabit küme boyutuna sahiptir.
- Bir not defteri iş göndermek, J1 10 düğümleri, bir Kıvılcım örneği kullanan, SI1 işi işlemek için oluşturulur.
- Şimdi havuzda hala kapasite ve örnek, J2, SI1 tarafından işlenir, çünkü 10 düğüm kullanan başka bir iş, J2 gönderin.
- J2 11 düğüm isteseydi, SP1 veya SI1'de kapasite olmazdı. Bu durumda, J2 bir not defterinden geliyorsa, iş reddedilir; J2 toplu iş geliyorsa, o zaman sıraya olacaktır.

### <a name="example-2"></a>Örnek 2

- Bir Spark havuzu arama SP2 oluşturursunuz; bir otomatik ölçek etkin 10 - 20 düğümleri vardır
- Bir not defteri iş göndermek, J1 10 düğümleri, bir Kıvılcım örneği, SI1, işi işlemek için oluşturulur kullanır.
- Şimdi başka bir iş göndermek, J2, 10 düğümleri kullanır, örneğin otomatik 20 düğüm ve işlemler J2 büyür havuzda hala kapasite olduğu için.

### <a name="example-3"></a>Örnek 3

- SP1 adında bir Kıvılcım havuzu oluşturursunuz; 20 düğüm sabit küme boyutuna sahiptir.
- Bir not defteri iş göndermek, J1 10 düğümleri, bir Kıvılcım örneği kullanan, SI1 işi işlemek için oluşturulur.
- Başka bir kullanıcı, U2, 10 düğüm, yeni bir Kıvılcım örneği, SI2, işi işlemek için oluşturulan kullanan bir İş, J3 gönderir.
- Şimdi havuzda hala kapasite ve örnek, J2, SI1 tarafından işlenir, çünkü 10 düğüm kullanan başka bir iş, J2 gönderin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
- [Apache Kıvılcım Dokümantasyon](https://spark.apache.org/docs/2.4.4/)
