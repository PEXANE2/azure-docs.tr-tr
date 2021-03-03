---
title: Mongo DB için Azure Cosmos DB API 'sindeki yaygın hataların sorunlarını giderme
description: Bu belgede, MongoDB için Azure Cosmos DB API 'sinde karşılaşılan yaygın sorunları gidermeye yönelik yollar ele alınmaktadır.
author: christopheranderson
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: troubleshooting
ms.date: 07/15/2020
ms.author: chrande
ms.openlocfilehash: 13ac90ae70262f2f6781f5f40ec67da4bf74ab68
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101661282"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API 'sindeki yaygın sorunları giderme
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

Aşağıdaki makalede, MongoDB için Azure Cosmos DB API kullanılarak gerçekleştirilen dağıtımlar için yaygın hatalar ve çözümler açıklanmaktadır.

>[!Note]
> Azure Cosmos DB MongoDB altyapısını barındırmıyor. MongoDB [kablo protokol sürümü 4,0](mongodb-feature-support-40.md), [3,6](mongodb-feature-support-36.md)ve [tel protokol sürüm 3,2](mongodb-feature-support.md)için eski destek desteği sunar. Bu nedenle, bu hatalardan bazıları yalnızca MongoDB için Azure Cosmos DB API 'sinde bulunur.

## <a name="common-errors-and-solutions"></a>Genel hatalar ve çözümleri

| Kod       | Hata                | Açıklama  | Çözüm  |
|------------|----------------------|--------------|-----------|
| 2 | Belirtilen order by öğesine karşılık gelen dizin yolu hariç tutulur veya order by sorgusunda, kendisine sunulabilecek bir bileşik dizin yoktur. | Sorgu dizine alınmamış bir alana göre sıralama istiyor. | Denenen sıralama sorgusu için eşleşen bir dizin (veya bileşik dizin) oluşturun. |
| 13 | Yetkisiz | İsteğin tamamlanması için gerekli izinler yok. | Veritabanınız ve koleksiyonunuz için uygun izinleri oluşturduğunuzdan emin olun.  |
| 16 | Invalidlength | Belirtilen isteğin uzunluğu geçersiz. | Açıkla () işlevini kullanıyorsanız, yalnızca bir işlem sağladığınızdan emin olun. |
| 26 | NamespaceNotFound | Sorguda başvurulan veritabanı veya koleksiyon bulunamadı. | Veritabanı/Koleksiyon adınızın sorgunuzdakiyle bire bir eşleştiğinden emin olun.|
| 50 | ExceededTimeLimit | İstek 60 saniyelik yürütme zaman aşımı süresini aştı. |  Bu hatanın birçok nedeni olabilir. Nedenlerinden biri geçerli ayrılmış istek birimi kapasitesinin isteği tamamlamak için yeterli olmamasıdır. Bu durum söz konusu koleksiyonun veya veritabanının istek birimlerini artırarak çözülebilir. Diğer durumlarda, büyük bir istek daha küçük isteklere bölünerek bu hatadan kaçınılabilir. Bu hatayı alan yazma işlemini yeniden denemek, yinelenen yazma işlemine neden olabilir. <br><br>RU’ları etkilemeden büyük miktarda veri silmeye çalışıyorsanız: <br>-TTL 'yi (zaman damgasına göre) kullanmayı düşünün: [MongoDB için Azure Cosmos DB API 'si ile veri süresi sonu](./mongodb-time-to-live.md) <br>-Silme işlemini gerçekleştirmek için Imleci/toplu Iş boyutunu kullanın. Tek seferde tek bir belge getirip bir döngü aracılığıyla bunu silebilirsiniz. Bu, üretim uygulamanızı etkilemeden verileri yavaş bir şekilde silmenize yardımcı olur.|
| 61 | ShardKeyNotFound | İsteğinizdeki belge, koleksiyonun parça anahtarını (Azure Cosmos DB bölüm anahtarı) içermiyor. | Koleksiyonun parça anahtarının istekte kullanıldığından emin olun.|
| 66 | ImmutableField | İstek, sabit bir alanı değiştirmeye çalışıyor | "kimlik" alanları sabittir. İsteğiniz bu alanı güncelleştirmeye çalışmadığından emin olun. |
| 67 | CannotCreateIndex | Dizin oluşturma isteği tamamlanamıyor. | Bir kapsayıcıda en fazla 500 tek alan dizini oluşturulabilir. Bileşik dizinde en fazla sekiz alan bulunabilir (bileşik dizinler 3.6+ sürümünde desteklenir). |
| 115 | CommandNotSupported | Denenen istek desteklenmiyor. | Hatada ek ayrıntılar sağlanıyor olmalıdır. Bu işlevsellik dağıtımlarınız için önemliyse, lütfen [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)bir destek bileti oluşturarak bize bildirin. |
| 11000 | DuplicateKey | Eklediğiniz belgenin parça anahtarı (Azure Cosmos DB bölüm anahtarı) koleksiyonda zaten var veya bir benzersiz dizin alanı kısıtlaması ihlal edildi. | Mevcut belgeyi güncelleştirmek için update() işlevini kullanın. Benzersiz dizin alanı kısıtlaması ihlal edildiyse, parçada/bölümde var olmayan bir alan değeri ekleyin veya belgeyi bu değerle güncelleştirin. |
| 16500 | TooManyRequests  | Kullanılan toplam istek birimi sayısı, koleksiyon için sağlanan istek birimi hızından fazla ve kısıtlanmış. | Azure portaldan bir kapsayıcıya veya kapsayıcı kümesine atanan aktarım hızını ölçeklendirmeyi göz önünde bulundurabilir veya işlemi yeniden deneyebilirsiniz. [SSR](prevent-rate-limiting-errors.md) 'yi (sunucu tarafı yeniden deneme) etkinleştirirseniz Azure Cosmos DB, bu hata nedeniyle başarısız olan istekleri otomatik olarak yeniden dener. |
| 16501 | ExceededMemoryLimit | Çok kiracılı bir hizmet olarak, işlem istemcinin bellek hizmet birimini aşıyor. | Daha kısıtlayıcı sorgu ölçütleriyle işlemin kapsamını daraltın veya [Azure portaldan](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) destekle iletişime geçin. Örnek: `db.getCollection('users').aggregate([{$match: {name: "Andy"}}, {$sort: {age: -1}}]))` |
| 40324 | Tanınmayan işlem hattı aşaması adı. | Toplama işlem hattı isteğinizdeki aşama adı tanınmadı. | İsteğinizdeki tüm toplama işlem hattı adlarının geçerli olduğundan emin olun. |
| - | MongoDB kablo sürümü sorunları | MongoDB sürücülerinin eski sürümleri bağlantı dizelerinde Azure Cosmos hesabının adını algılayamıyor. | The MongoDB bağlantı dizesi için Cosmos DB API 'sinin sonunda *appname = @**AccountName** @* ekleyin; burada ***AccountName*** , Cosmos DB hesabınızın adıdır. |
| - | MongoDB istemci ağı sorunları (yuva veya endOfStream özel durumları gibi)| Ağ isteği başarısız oldu. Bu genellikle MongoDB istemcisinin kullanmaya çalıştığı bir TCP bağlantısının etkin olmamasından kaynaklanır. MongoDB sürücüleri genellikle bağlantı havuzunu kullanır ve bu, istek için havuzdan seçilen rastgele bir bağlantının kullanılmasıyla sonuçlanır. Etkin olmayan bağlantılar normalde Azure Cosmos DB ucunda dört dakika sonra zaman aşımına uğrar. | Uygulama kodunuzda bu başarısız istekleri yeniden deneyebilir, MongoDB istemci (sürücü) ayarlarınızı dört dakikalık zaman aşımı penceresinden önce etkin olmayan TCP bağlantılarına değiştirebilir veya etkin bir durumda olan TCP bağlantılarını sürdürmek üzere işletim sistemi KeepAlive ayarlarınızı yapılandırabilirsiniz.<br><br>Bağlantı iletilerini önlemek için, bağlantı dizesini değiştirip maxConnectionIdleTime değerini 1-2 dakika olarak ayarlamayı isteyebilirsiniz.<br>-Mongo sürücüsü: *Maxıdletimems yapılandırma = 120000* <br>-Node.JS: *Sockettimeoutms = 120000*, *oto reconnect* = true, *KeepAlive* = true, *keepaliveınitialdelay* = 3 dakika yapılandırın

## <a name="next-steps"></a>Sonraki adımlar

- [Studio 3T](mongodb-mongochef.md) 'ı Azure Cosmos DB MongoDB IÇIN API 'si ile nasıl kullanacağınızı öğrenin.
- MongoDB için Azure Cosmos DB API 'SI ile [Robo 3T kullanmayı](mongodb-robomongo.md) öğrenin.
- MongoDB için Azure Cosmos DB API 'siyle MongoDB [örneklerini](mongodb-samples.md) gezin.