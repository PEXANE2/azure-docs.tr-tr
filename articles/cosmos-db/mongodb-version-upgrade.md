---
title: MongoDB hesabı için Azure Cosmos DB API 'nizin Mongo sürümünü yükseltin
description: MongoDB için mevcut Azure Cosmos DB API 'SI için MongoDB tel protokolü sürümünü sorunsuz şekilde yükseltme
author: christopheranderson
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 03/02/2021
ms.author: chrande
ms.openlocfilehash: 1818838a68c2712336a3515b2a82b5fdd518d237
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "101661180"
---
# <a name="upgrade-the-api-version-of-your-azure-cosmos-db-api-for-mongodb-account"></a>MongoDB hesabı için Azure Cosmos DB API 'nizin API sürümünü yükseltme
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

Bu makalede, MongoDB hesabı için Azure Cosmos DB API 'sinin API sürümünün nasıl yükseltileceği açıklanır. Yükseltmeden sonra, MongoDB için Azure Cosmos DB API 'sindeki en son işlevselliği kullanabilirsiniz. Yükseltme işlemi, hesabınızın kullanılabilirliğini kesintiye uğramaz ve herhangi bir noktada, her zaman veritabanı kapasitesini azaltır. Bu işlemden, mevcut veri veya dizinlerin hiçbiri etkilenmeyecektir. 

Yeni bir API sürümüne yükseltirken, üretim iş yüklerini yükseltmeden önce geliştirme ve test iş yükleri ile başlayın. MongoDB hesabı için Azure Cosmos DB API 'nizi yükseltmeden önce istemcilerinizi, yükseltmekte olduğunuz API sürümüyle uyumlu bir sürüme yükseltmeniz önemlidir.

>[!Note]
> Şu anda yalnızca sunucu sürümü 3,2 ' i kullanan hesaplar 3,6 veya 4,0 sürümüne yükseltilebilir. Hesabınız yükseltme seçeneğini göstermezse lütfen [bir destek bileti dosyası](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)yapın.

## <a name="upgrading-to-40-or-36"></a>4,0 veya 3,6 sürümüne yükseltme

### <a name="benefits-of-upgrading-to-version-40"></a>4,0 sürümüne yükseltmenin avantajları

Sürüm 4,0 ' de bulunan yeni özellikler aşağıda verilmiştir:
- Parçalara ayrılmamış koleksiyonlar içinde çok belgeli işlemler için destek.
- Yeni toplama işleçleri
- Gelişmiş tarama performansı
- Daha hızlı, daha verimli depolama

### <a name="benefits-of-upgrading-to-version-36"></a>3\.6 sürümüne yükseltmenin avantajları

Sürüm 3,6 ' de bulunan yeni özellikler aşağıda verilmiştir:
- Gelişmiş performans ve kararlılık
- Yeni veritabanı komutları desteği
- Varsayılan ve yeni toplama aşamalarına göre toplama işlem hattı desteği
- Değişiklik Akışları Desteği
- Bileşik Dizin desteği
- Şu işlemler için bölümler arası destek: güncelleştirme, silme, sayma ve sıralama
- Şu toplama işlemleri için gelişmiş performans: $count, $skip, $limit ve $group
- Joker karakter dizinleme artık destekleniyor

### <a name="changes-from-version-32"></a>Sürüm 3,2 ' den değişiklikler

- Varsayılan olarak, [sunucu tarafı yeniden deneme (SSR)](prevent-rate-limiting-errors.md) özelliği etkinleştirilir, böylece istemci uygulamasından gelen istekler 16500 hata döndürmeyecektir. Bunun yerine, istekler tamamlanana kadar ya da ikinci zaman aşımı 60 ' i tıklatana kadar sürdürülecek.
- İstek başına zaman aşımı 60 saniye olarak ayarlanmıştır.
- Yeni kablo protokol sürümünde oluşturulan MongoDB koleksiyonlarında, varsayılan olarak yalnızca `_id` özelliği dizine alınır.

### <a name="action-required-when-upgrading-from-32"></a>3,2 'den yükseltirken gereken eylem

3,2 sürümünden yükseltirken, veritabanı hesabı uç noktası son eki şu biçime güncelleştirilir:

```
<your_database_account_name>.mongo.cosmos.azure.com
```

Sürüm 3,2 ' den yükseltiyorsanız, bu veritabanı hesabıyla bağlanan uygulamalarınızda ve sürücülerinizin mevcut uç noktasını değiştirmeniz gerekir. **Yalnızca yeni uç noktayı kullanan bağlantılar yenı API sürümündeki özelliklere erişebilir**. Önceki 3,2 uç noktasının soneki olmalıdır `.documents.azure.com` .

>[!Note]
> Hesabınız bir Sogeign, kamu veya sınırlı Azure bulutu 'nda oluşturulduysa, bu uç nokta hafif farklılıklara sahip olabilir.

## <a name="how-to-upgrade"></a>Yükseltme

1. Azure portal gidin ve MongoDB hesabına genel bakış dikey penceresinde Azure Cosmos DB API 'sine gidin. Geçerli sunucu sürümünüzün beklediğinizi doğrulayın.

    :::image type="content" source="./media/mongodb-version-upgrade/1.png" alt-text="MongoDB hesabına genel bakış ile Azure portal" border="false":::

2. Soldaki seçeneklerden `Features` Dikey pencereyi seçin. Bu, veritabanı hesabınız için kullanılabilen hesap düzeyi özelliklerini açığa çıkarır.

    :::image type="content" source="./media/mongodb-version-upgrade/2.png" alt-text="Özellikler dikey penceresinde MongoDB hesabına genel bakış ile Azure portal" border="false":::

3. `Upgrade Mongo server version`Satıra tıklayın. Bu seçeneği görmüyorsanız, hesabınız bu yükseltme için uygun olmayabilir. Bu durumda, lütfen [bir destek bileti](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) dosyası sağlayın.

    :::image type="content" source="./media/mongodb-version-upgrade/3.png" alt-text="Seçeneklerle Özellikler dikey penceresi." border="false":::

4. Yükseltme hakkında bilgi için görünen bilgileri gözden geçirin. `Enable`İşlemi başlatmak için hazırsanız açık ' a tıklayın.

    :::image type="content" source="./media/mongodb-version-upgrade/4.png" alt-text="Genişletilmiş Yükseltme Kılavuzu." border="false":::

5. İşlem başladıktan sonra, `Features` menü yükseltmenin durumunu gösterir. Durum,,, olarak değişir `Pending` `In Progress` `Upgraded` . Bu işlem, veritabanı hesabının mevcut işlevlerini veya işlemlerini etkilemez.

    :::image type="content" source="./media/mongodb-version-upgrade/5.png" alt-text="Başlatıldıktan sonra yükseltme durumu." border="false":::

6. Yükseltme tamamlandıktan sonra durum olarak gösterilir `Upgraded` . İşlemi tamamlamak için gerçekleştirmeniz gereken sonraki adımlar ve eylemler hakkında daha fazla bilgi edinmek için buraya tıklayın. İsteğiniz işlenirken bir sorun oluşursa lütfen [desteğe başvurun](https://azure.microsoft.com/en-us/support/create-ticket/) .

    :::image type="content" source="./media/mongodb-version-upgrade/6.png" alt-text="Yükseltilen hesap durumu." border="false":::

7. 
    1. 3,2 ' den yükselttiyseniz, dikey pencereye geri dönün `Overview` ve uygulamanızda kullanmak üzere yeni bağlantı dizesini kopyalayın. 3,2 çalıştıran eski bağlantı dizesi kesintiye uğramayacak. Tutarlı bir deneyim sağlamak için tüm uygulamalarınızın yeni uç noktayı kullanması gerekir.
    2. 3,6 ' den yükselttiyseniz, var olan bağlantı dizeniz belirtilen sürüme yükseltilecektir ve kullanmaya devam edilmelidir.

    :::image type="content" source="./media/mongodb-version-upgrade/7.png" alt-text="Yeni genel bakış dikey penceresi." border="false":::


## <a name="how-to-downgrade"></a>Düşürme
Ayrıca, ' yükseltme yöntemi ' bölümündeki adımlar aracılığıyla hesabınızı 4,0 ' den 3,6 ' e indirgeyede indirebilirsiniz. 

3,2 'den (4,0 veya 3,6) sürümüne yükselttiyseniz ve 3,2 sürümüne geri dönmek istiyorsanız, önceki (3,2) Bağlantı dizenizi kullanarak `accountname.documents.azure.com` Etkin yükseltme sonrası sürümü olan ana bilgisayar ile yeniden geçmeniz yeterlidir.


## <a name="next-steps"></a>Sonraki adımlar

- [MongoDB sürüm 4,0 ' in](mongodb-feature-support-40.md)desteklenen ve desteklenmeyen özellikleri hakkında bilgi edinin.
- [MongoDB sürüm 3,6 ' in](mongodb-feature-support-36.md)desteklenen ve desteklenmeyen özellikleri hakkında bilgi edinin.
- Daha fazla bilgi için [Mongo 3,6 sürüm özelliklerini](https://devblogs.microsoft.com/cosmosdb/azure-cosmos-dbs-api-for-mongodb-now-supports-server-version-3-6/) inceleyin
