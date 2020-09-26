---
title: Apache Spark temel kavramlar
description: Azure SYNAPSE Analytics 'teki Apache Spark ve farklı kavramlara giriş.
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: spark
ms.date: 04/15/2020
ms.author: euang
ms.reviewer: euang
ms.openlocfilehash: 74e85906742207d6cde0b7c4cc5c021c23ee4c7b
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91260147"
---
# <a name="apache-spark-in-azure-synapse-analytics-core-concepts"></a>Azure SYNAPSE Analytics temel kavramları Apache Spark

Apache Spark, büyük veri analizi uygulamalarının performansını artırmak üzere bellek içi işlemeyi destekleyen paralel bir işleme çerçevesidir. Azure SYNAPSE Analytics 'te Apache Spark, Microsoft 'un buluttaki Apache Spark uygulamalarından biridir. 

Azure SYNAPSE, Azure 'da Spark özellikleri oluşturmayı ve yapılandırmayı kolaylaştırır. Azure SYNAPSE, burada belgelenen Bu Spark yeteneklerinin farklı bir uygulamasını sağlar.

## <a name="spark-pools-preview"></a>Spark havuzları (Önizleme)

Azure portal Spark Havuzu (Önizleme) oluşturulur. Bu, bir Spark havuzunun, verileri işleyen bir Spark örneği oluşturmak için kullanıldığı bir tanımıdır. Bir Spark havuzu oluşturulduğunda, bu yalnızca meta veriler olarak bulunur ve hiçbir kaynak Tüketilmekte, çalışmıyor veya ücretlendirililmez. Spark havuzunda Spark örneğinin özelliklerini denetleyen bir dizi özellik vardır. Bu özellikler, ad, boyut, ölçekleme davranışı ve yaşam süresi ile sınırlı değildir.

Spark havuzları oluşturmayla ilişkili dolar veya kaynak maliyeti olmadığından, herhangi bir sayıda farklı yapılandırma ile herhangi bir numara oluşturulabilir. İzinler, kullanıcıların yalnızca bazılarına erişime izin veren Spark havuzlarına da uygulanabilir.

En iyi uygulama, geliştirme ve hata ayıklama için kullanılabilecek daha küçük Spark havuzları oluşturmak ve ardından üretim iş yüklerini çalıştırmaya yönelik daha büyük bir uygulamadır.

Spark havuzu oluşturmayı okuyabilir ve bunların tüm özelliklerini nasıl görebileceğiniz [SYNAPSE Analytics 'Te Spark havuzlarıyla çalışmaya başlayın](../quickstart-create-apache-spark-pool-portal.md)

## <a name="spark-instances"></a>Spark örnekleri

Spark örnekleri, bir Spark havuzuna bağlandığınızda, bir oturum oluşturup bir iş çalıştırdığınızda oluşturulur. Birden çok kullanıcının tek bir Spark havuzuna erişimi olabileceğinden, bağlanan her kullanıcı için yeni bir Spark örneği oluşturulur. 

İkinci bir işi gönderdiğinizde, havuzda kapasite varsa, var olan Spark örneğinin kapasitesi de vardır. Ardından, var olan örnek işi işleyecek. Aksi takdirde, kapasite havuz düzeyinde kullanılabiliyorsa yeni bir Spark örneği oluşturulur.

## <a name="examples"></a>Örnekler

### <a name="example-1"></a>Örnek 1

- SP1 adlı bir Spark havuzu oluşturursunuz; 20 düğümden oluşan sabit bir küme boyutuna sahiptir.
- Bir not defteri işi gönderirseniz, iş işlemek için bir Spark örneği olan 10 düğümleri kullanan bir J1, SI1 oluşturulur.
- Artık havuzda kapasite olduğundan ve örnek, J2, SI1 tarafından işlendiği için, daha sonra 10 düğümü kullanan başka bir iş J2.
- J2 11 düğümleri isteniyorsa, SP1 veya SI1 'de kapasite yoktur. Bu durumda, J2 bir not defterinden geliyorsa iş reddedilir; J2 bir Batch işinden geliyorsa sıraya alınır.

### <a name="example-2"></a>Örnek 2

- Spark havuz çağrısı SP2 oluşturursunuz; Otomatik ölçeklendirme etkinleştirilmiş 10 – 20 düğümleri vardır
- İşi işlemek için bir Spark örneği, SI1 olan J1 bir not defteri işi gönderebilirsiniz.
- Artık, örnek otomatik olarak 20 düğüme ve J2 işleme olan havuzda kapasite olduğundan, J2.

### <a name="example-3"></a>Örnek 3

- SP1 adlı bir Spark havuzu oluşturursunuz; 20 düğümden oluşan sabit bir küme boyutuna sahiptir.
- Bir not defteri işi gönderirseniz, iş işlemek için bir Spark örneği olan 10 düğümleri kullanan bir J1, SI1 oluşturulur.
- Farklı bir kullanıcı olan J3, işi işlemek için yeni bir Spark örneği, SI2 olan bir Işi gönderir.
- Artık havuzda kapasite olduğundan ve örnek J2, SI1 tarafından işlendiği için, J2 tarafından kullanılan başka bir iş (10 düğüm) gönderebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
- [Apache Spark belgeleri](https://spark.apache.org/docs/2.4.4/)
