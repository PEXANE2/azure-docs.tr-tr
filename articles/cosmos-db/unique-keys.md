---
title: Azure Cosmos DB'de benzersiz tuşları kullanın
description: Azure Cosmos veritabanı için benzersiz anahtarları nasıl tanımlayıp kullanacağınızı öğrenin. Bu makalede, benzersiz anahtarların nasıl veri bütünlüğü katmanı ekleyeceğini de açıklanmaktadır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.reviewer: sngun
ms.openlocfilehash: f234579c6fb2b6f1bc0cd518b87ea69fae30093a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74869842"
---
# <a name="unique-key-constraints-in-azure-cosmos-db"></a>Azure Cosmos DB'de benzersiz anahtar kısıtlamaları

Benzersiz anahtarlar, bir Azure Cosmos kapsayıcısına veri bütünlüğü katmanı ekler. Bir Azure Cosmos kapsayıcısı oluşturduğunuzda benzersiz bir anahtar ilkesi oluşturursunuz. Benzersiz anahtarlarla, mantıksal bir bölüm içindeki bir veya daha fazla değerin benzersiz olduğundan emin olun. Ayrıca [bölüm tuşu](partition-data.md)başına benzersizlik garanti edebilirsiniz.

Benzersiz bir anahtar ilkesine sahip bir kapsayıcı oluşturduktan sonra, özgün anahtar kısıtlaması tarafından belirtildiği gibi, mantıksal bir bölüm içinde yinelenen bir sonuç la sonuçlanan yeni bir öğenin oluşturulması veya güncelleştirmesi engellenir. Benzersiz anahtarla birleştirilmiş bölüm anahtarı, bir öğenin kapsayıcı kapsamındaki benzersizliğini garanti eder.

Örneğin, e-posta adresi olan bir Azure Cosmos `CompanyID` kapsayıcısını benzersiz anahtar kısıtlaması ve bölüm anahtarı olarak düşünün. Kullanıcının e-posta adresini benzersiz bir anahtarla yapılandırdığınızda, her öğenin belirli `CompanyID`bir anahtar içinde benzersiz bir e-posta adresi vardır. Yinelenen e-posta adresleriyle ve aynı bölüm anahtar değeriyle iki öğe oluşturulamaz. 

Aynı ad, soyad ve e-posta adresine sahip değil, aynı e-posta adresine sahip öğeler oluşturmak için, benzersiz anahtar ilkesine daha fazla yol ekleyin. Yalnızca e-posta adresini temel alan benzersiz bir anahtar oluşturmak yerine, ad, soyad ve e-posta adresinin birleşimiile benzersiz bir anahtar da oluşturabilirsiniz. Bu anahtar bileşik benzersiz anahtar olarak bilinir. Bu durumda, belirli `CompanyID` bir değer içinde üç değerin her benzersiz kombinasyonuna izin verilir. 

Örneğin, kapsayıcı, her öğenin benzersiz anahtar kısıtlamasını onurlandırdığı aşağıdaki değerlere sahip öğeler içerebilir.

|Şirket Kimliği|Ad|Soyadı|E-posta adresi|
|---|---|---|---|
|Contoso|Gaby|Duperre|gaby@contoso.com |
|Contoso|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|İvan|Duperre|gaby@fabrikam.com|
|Fabrkam|   |Duperre|gaby@fabraikam.com|
|Fabrkam|   |   |gaby@fabraikam.com|

Önceki tabloda listelenen kombinasyonları içeren başka bir öğe eklemeye çalışırsanız, bir hata alırsınız. Hata, benzersiz anahtar kısıtlamasının karşılanmadığını gösterir. Ya da `Resource with specified ID or name already exists` `Resource with specified ID, name, or unique index already exists` bir iade iletisi olarak alırsınız. 

## <a name="define-a-unique-key"></a>Benzersiz bir anahtar tanımlama

Benzersiz anahtarları yalnızca bir Azure Cosmos kapsayıcısı oluşturduğunuzda tanımlayabilirsiniz. Benzersiz bir anahtar, mantıksal bir bölüme kapsamlıdır. Önceki örnekte, kapsayıcıyı posta koduna göre bölümlere ayırırsanız, her mantıksal bölümde yinelenen öğelerle karşılaşırsınız. Benzersiz anahtarlar oluştururken aşağıdaki özellikleri göz önünde bulundurun:

* Varolan bir kapsayıcıyı farklı bir benzersiz anahtar kullanmak için güncelleştiremezsiniz. Başka bir deyişle, bir kapsayıcı benzersiz bir anahtar ilkesi yle oluşturulduktan sonra, ilke değiştirilemez.

* Varolan bir kapsayıcı için benzersiz bir anahtar ayarlamak için, benzersiz anahtar kısıtlaması içeren yeni bir kapsayıcı oluşturun. Verileri varolan kapsayıcıdan yeni kapsayıcıya taşımak için uygun veri geçiş aracını kullanın. SQL kapsayıcıları için verileri taşımak için [Veri Geçişi aracını](import-data.md) kullanın. MongoDB kapları için, verileri taşımak için [mongoimport.exe veya mongorestore.exe](mongodb-migrate.md) kullanın.

* Benzersiz bir anahtar ilkesi en fazla 16 yol değerine sahip olabilir. Örneğin, değerler `/firstName`, , `/lastName`ve `/address/zipCode`. Her benzersiz anahtar ilkesinde en fazla 10 benzersiz anahtar kısıtlaması veya birleşimi olabilir. Her benzersiz dizin kısıtlaması için birleştirilmiş yollar 60 baytı geçmemelidir. Önceki örnekte, ad, soyad ve e-posta adresi birlikte bir kısıtlama vardır. Bu kısıtlama, 16 olası yolun 3'ünü kullanır.

* Bir kapsayıcının benzersiz bir anahtar ilkesi olduğunda, bir öğeyi oluşturmak, güncelleştirmek ve silmek için [İstek Birimi (RU)](request-units.md) ücretleri biraz daha yüksektir.

* Seyrek benzersiz anahtarlar desteklenmez. Bazı benzersiz yol değerleri eksikse, bunlar benzersizlik kısıtlamasında yer alan null değerleri olarak kabul edilir. Bu nedenle, bu kısıtlamayı karşılamak için null değeri olan yalnızca tek bir öğe olabilir.

* Benzersiz anahtar adları büyük/küçük harf duyarlıdır. Örneğin, benzersiz anahtar kısıtlaması ayarlanmış bir `/address/zipcode`kapsayıcı düşünün. Verilerinizin bir alanı `ZipCode`varsa, Azure Cosmos DB ile aynı `zipcode` olmadığı için benzersiz anahtar `ZipCode`olarak "null" ekler. Bu servis talebi duyarlılığı nedeniyle, yinelenen "null" benzersiz anahtar kısıtlamasını ihlal ettiği için ZipCode'a sahip diğer tüm kayıtlar eklenemez.

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal bölümler](partition-data.md) hakkında daha fazla bilgi edinin
* Kapsayıcı oluştururken [benzersiz anahtarları nasıl tanımlayacağımı](how-to-define-unique-keys.md) keşfedin
