---
title: Mongo DB için Azure Cosmos DB API 'sindeki yaygın hataların sorunlarını giderme
description: Bu belgede, MongoDB için Azure Cosmos DB API 'sinde karşılaşılan yaygın sorunları gidermeye yönelik yollar ele alınmaktadır.
author: christopheranderson
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: troubleshooting
ms.date: 07/15/2020
ms.author: chrande
ms.openlocfilehash: 06a06d275ba6f5ded475ffd693ee61e7a72b9516
ms.sourcegitcommit: 02b1179dff399c1aa3210b5b73bf805791d45ca2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98127711"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API 'sindeki yaygın sorunları giderme
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

Aşağıdaki makalede, MongoDB için Azure Cosmos DB API kullanılarak gerçekleştirilen dağıtımlar için yaygın hatalar ve çözümler açıklanmaktadır.

>[!Note]
> Azure Cosmos DB MongoDB altyapısını barındırmıyor. MongoDB kablo protokolünün bir uygulamasını sağlar. Bu nedenle, bu hatalardan bazıları yalnızca MongoDB için Azure Cosmos DB API 'sinde bulunur. 

## <a name="common-errors-and-solutions"></a>Genel hatalar ve çözümleri

| Kod       | Hata                | Açıklama  | Çözüm  |
|------------|----------------------|--------------|-----------|
| 2 | Belirtilen order by öğesine karşılık gelen dizin yolu hariç tutulur veya order by sorgusunda, kendisine sunulabilecek bir bileşik dizin yoktur. | Sorgu dizine alınmamış bir alana göre sıralama istiyor. | Denenen sıralama sorgusu için eşleşen bir dizin (veya bileşik dizin) oluşturun. |
| 13 | Yetkisiz | İsteğin tamamlanma izinleri eksik. | Veritabanınız ve koleksiyonunuz için uygun izinleri oluşturduğunuzdan emin olun.  |
| 16 | Invalidlength | Belirtilen isteğin uzunluğu geçersiz. | Açıkla () işlevini kullanıyorsanız, yalnızca bir işlem sağladığınızdan emin olun. |
| 26 | NamespaceNotFound | Sorguda başvurulmakta olan veritabanı veya koleksiyon bulunamıyor. | Veritabanınızın/koleksiyon adınızın sorgunuzun adıyla tam olarak eşleştiğinden emin olun.|
| 50 | ExceededTimeLimit | İstek 60 saniyelik yürütme zaman aşımı süresini aştı. |  Bu hatanın pek çok nedeni olabilir. Bu nedenlerin biri, şu anda ayrılmış olan istek birimleri kapasitesinin isteği tamamlaması için yeterli olmadığı durumlarda oluşur. Bu durum söz konusu koleksiyonun veya veritabanının istek birimlerini artırarak çözülebilir. Diğer durumlarda bu hata, büyük bir isteği daha küçük olanlara bölerek de çalıştırılabilir. Bu hatayı almış bir yazma işlemini yeniden denemek yinelenen bir yazmaya neden olabilir.|
| 61 | ShardKeyNotFound | Talebinizdeki belge koleksiyonun parça anahtarını (Azure Cosmos DB bölüm anahtarı) içermiyordu. | Koleksiyonda koleksiyonun parça anahtarının kullanıldığından emin olun.|
| 66 | Immutablefield | İstek, sabit bir alanı değiştirmeye çalışıyor | "kimlik" alanları sabittir. İsteğiniz bu alanı güncelleştirmeye çalışmadığından emin olun. |
| 67 | Cannotcreateındex | Dizin oluşturma isteği tamamlanamıyor. | Kapsayıcıda en fazla 500 tek alan dizini oluşturulabilir. Bileşik dizine kadar en fazla sekiz alan bulunabilir (Birleşik dizinler 3.6 + sürümünde desteklenir). |
| 115 | CommandNotSupported | Denenen istek desteklenmiyor. | Hatada ek ayrıntılar sağlanmalıdır. Bu işlevsellik dağıtımlarınız için önemliyse, lütfen [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)bir destek bileti oluşturarak bize bildirin. |
| 11000 | DuplicateKey | Eklediğiniz belgenin parça anahtarı (Azure Cosmos DB bölüm anahtarı) koleksiyonda zaten var veya benzersiz bir dizin alanı kısıtlaması ihlal edildi. | Var olan bir belgeyi güncelleştirmek için Update () işlevini kullanın. Benzersiz dizin alanı kısıtlaması ihlal edilirse, belgeyi parça/bölümde bulunmayan bir alan değeri ile ekleyin veya güncelleştirin. |
| 16500 | TooManyRequests  | Kullanılan toplam istek birimi sayısı, koleksiyon için sağlanan istek birimi hızından fazla ve kısıtlanmış. | Azure portaldan bir kapsayıcıya veya kapsayıcı kümesine atanan aktarım hızını ölçeklendirmeyi göz önünde bulundurabilir veya işlemi yeniden deneyebilirsiniz. SSR 'yi (sunucu tarafı yeniden deneme) etkinleştirirseniz Azure Cosmos DB, bu hata nedeniyle başarısız olan istekleri otomatik olarak yeniden dener. |
| 16501 | ExceededMemoryLimit | Çok kiracılı bir hizmet olarak, işlem istemcinin bellek işlem biriminden çıktı. | Daha kısıtlayıcı sorgu ölçütleri aracılığıyla işlemin kapsamını azaltın veya [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)destek ekibiyle iletişime geçin. Örnek: `db.getCollection('users').aggregate([{$match: {name: "Andy"}}, {$sort: {age: -1}}]))` |
| 40324 | Tanınmayan ardışık düzen aşaması adı. | Toplama işlem hattı talebinizdeki aşama adı tanınmadı. | Tüm toplama işlem hattı adlarının isteğiniz içinde geçerli olduğundan emin olun. |
| - | MongoDB kablo sürümü sorunları | MongoDB sürücülerinin eski sürümleri, bağlantı dizelerindeki Azure Cosmos hesabının adını algılayamaz. | The MongoDB bağlantı dizesi için Cosmos DB API 'sinin sonunda *appname = @**AccountName** @* ekleyin; burada ***AccountName*** , Cosmos DB hesabınızın adıdır. |
| - | MongoDB istemci ağ sorunları (yuva veya endOfStream özel durumları gibi)| Ağ isteği başarısız oldu. Bu, genellikle MongoDB istemcisinin kullanmaya çalışan etkin olmayan bir TCP bağlantısı nedeniyle oluşur. MongoDB sürücüleri genellikle bağlantı havuzlamayı kullanır ve bu, bir istek için kullanılan havuzdan seçilen rastgele bir bağlantıyla sonuçlanır. Etkin olmayan bağlantılar genellikle Azure Cosmos DB dört dakika sonra sona zaman aşımı. | Uygulama kodunuzda bu başarısız istekleri yeniden deneyebilir, MongoDB istemci (sürücü) ayarlarınızı dört dakikalık zaman aşımı penceresinden önce etkin olmayan TCP bağlantılarına değiştirebilir veya etkin bir durumda olan TCP bağlantılarını sürdürmek üzere işletim sistemi KeepAlive ayarlarınızı yapılandırabilirsiniz. |

## <a name="next-steps"></a>Sonraki adımlar

- [Studio 3T](mongodb-mongochef.md) 'ı Azure Cosmos DB MongoDB IÇIN API 'si ile nasıl kullanacağınızı öğrenin.
- MongoDB için Azure Cosmos DB API 'SI ile [Robo 3T kullanmayı](mongodb-robomongo.md) öğrenin.
- MongoDB için Azure Cosmos DB API 'siyle MongoDB [örneklerini](mongodb-samples.md) gezin.
