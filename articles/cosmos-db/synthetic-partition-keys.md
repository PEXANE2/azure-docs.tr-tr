---
title: Azure Cosmos DB'de sentetik bölme anahtarı oluşturma
description: Verileri ve iş yükünü bölüm tuşlarına eşit olarak dağıtmak için Azure Cosmos kaplarınızda sentetik bölüm tuşlarını nasıl kullanacağınızı öğrenin
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/03/2019
author: markjbrown
ms.author: mjbrown
ms.openlocfilehash: e8786c2d6e93c18a5bf9856a5555d6b528f842c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75441213"
---
# <a name="create-a-synthetic-partition-key"></a>Yapay bölüm anahtarı oluşturma

Yüzlerce veya binlerce farklı değere sahip bir bölüm anahtarına sahip olmak en iyi yöntemdir. Amaç, verilerinizi ve iş yükünüzü bu bölüm anahtar değerleriyle ilişkili öğeler arasında eşit olarak dağıtmaktır. Verilerinizde böyle bir özellik yoksa, sentetik bir *bölme anahtarı*oluşturabilirsiniz. Bu belge, Cosmos kapsayıcınız için sentetik bir bölme anahtarı oluşturmak için çeşitli temel teknikleri açıklar.

## <a name="concatenate-multiple-properties-of-an-item"></a>Bir öğenin birden çok özelliğini birleştirmeye

Birden çok özellik değerlerini tek bir yapay `partitionKey` özellik temalı bir araya getirerek bir bölüm anahtarı oluşturabilirsiniz. Bu anahtarlar sentetik anahtarlar olarak adlandırılır. Örneğin, aşağıdaki örnek belgeyi göz önünde bulundurun:

```JavaScript
{
"deviceId": "abc-123",
"date": 2018
}
```

Önceki belge için bir seçenek , /deviceId veya /date'i bölüm anahtarı olarak ayarlamaktır. Kapsayıcınızı aygıt kimliğine veya tarihe göre bölmek istiyorsanız bu seçeneği kullanın. Başka bir seçenek, bu iki değeri `partitionKey` bölüm anahtarı olarak kullanılan sentetik bir özelliğe dönüştürmektir.

```JavaScript
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

Gerçek zamanlı senaryolarda, veritabanında binlerce öğe olabilir. Sentetik anahtarı el ile eklemek yerine, değerleri birleştirmek için istemci tarafı mantığını tanımlayın ve sentetik anahtarı Cosmos kaplarınızdaki öğelere takın.

## <a name="use-a-partition-key-with-a-random-suffix"></a>Rasgele sonek içeren bir bölme anahtarı kullanma

İş yükünü daha eşit dağıtmak için başka bir olası strateji, bölüm anahtar değerinin sonunda rasgele bir sayı eklemektir. Öğeleri bu şekilde dağıttığınızda, bölümler arasında paralel yazma işlemleri gerçekleştirebilirsiniz.

Bir bölüm anahtarı nın bir tarihi temsil ediyorolması bir örnektir. 1 ile 400 arasında rasgele bir sayı seçebilir ve tarihe sonek olarak yapıştırabilirsiniz. Bu yöntem, , , `2018-08-09.1``2018-08-09.2`ve benzeri gibi `2018-08-09.400`bölüm anahtar değerleri ile sonuçlanır. Bölüm anahtarını rasgele yaptığınızdan, her gün kapsayıcıdaki yazma işlemleri birden çok bölüme eşit olarak yayılır. Bu yöntem daha iyi paralellik ve genel olarak daha yüksek iş elde etmek le sonuçlanır.

## <a name="use-a-partition-key-with-pre-calculated-suffixes"></a>Önceden hesaplanmış sonekler içeren bir bölme anahtarı kullanma 

Rasgele sonek stratejisi yazma iş bilgisini büyük ölçüde artırabilir, ancak belirli bir öğeyi okumak zordur. Öğeyi yazarken kullanılan sonek değerini bilmiyorsun. Tek tek öğeleri okumayı kolaylaştırmak için önceden hesaplanmış sonekler stratejisini kullanın. Öğeleri bölümler arasında dağıtmak için rasgele bir sayı kullanmak yerine, sorgulamak istediğiniz bir şeye göre hesaplanan bir sayı kullanın.

Kapsayıcının bir tarihi bölüm anahtarı olarak kullandığı önceki örneği göz önünde bulundurun. Şimdi, her öğenin `Vehicle-Identification-Number` `VIN`erişmek istediğimiz bir ) özelliği olduğunu varsayalım. Ayrıca, bugüne ek olarak öğeleri `VIN`bulmak için sık sık sorguları çalıştırdığınızı varsayalım. Uygulamanız öğeyi kapsayıcıya yazmadan önce, VIN'i temel alan bir karma sonek hesaplayabilir ve bölüm anahtar tarihine ekleyebilir. Hesaplama, eşit olarak dağıtılan 1 ile 400 arasında bir sayı oluşturabilir. Bu sonuç, rasgele sonek stratejisi yöntemi tarafından üretilen sonuçlara benzer. Bölüm anahtar değeri, hesaplanan sonuçla kaplanmış tarihtir.

Bu strateji ile, yazmaeşit bölüm anahtar değerleri arasında ve bölümler arasında yayılır. Belirli bir `Vehicle-Identification-Number`öğenin bölüm anahtar değerini hesaplayabildiğinizden, belirli bir öğeyi ve tarihi kolayca okuyabilirsiniz. Bu yöntemin yararı, tek bir sıcak bölüm anahtarı, yani, tüm iş yükünü alan bir bölüm anahtarı oluşturmaktan kaçınabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde bölümleme kavramı hakkında daha fazla bilgi edinebilirsiniz:

* [Mantıksal bölümler](partition-data.md)hakkında daha fazla bilgi edinin.
* [Azure Cosmos kapsayıcıları ve veritabanlarında iş ortası sağlama](set-throughput.md)hakkında daha fazla bilgi edinin.
* [Azure Cosmos kapsayıcısına nasıl iş veriş sağlamayı](how-to-provision-container-throughput.md)öğrenin.
* [Azure Cosmos veritabanında iş ortası sağlama](how-to-provision-database-throughput.md)yı öğrenin.
