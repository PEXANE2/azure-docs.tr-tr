---
title: MongoDB hesabı için Azure Cosmos DB API 'nizin Mongo sürümünü yükseltin
description: MongoDB için mevcut Azure Cosmos DB API 'SI için MongoDB tel protokolü sürümünü sorunsuz şekilde yükseltme
author: jasonwhowell
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: guide
ms.date: 09/22/2020
ms.author: jasonh
ms.openlocfilehash: eb12fc909b5165cbc759bbb7c531864cde16bb88
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93096317"
---
# <a name="upgrade-the-mongodb-wire-protocol-version-of-your-azure-cosmos-dbs-api-for-mongodb-account"></a>MongoDB hesabı için Azure Cosmos DB API 'nizin MongoDB kablo protokol sürümünü yükseltin
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

Bu makalede, MongoDB hesabı için Azure Cosmos DB API 'sinin tel Protokolü sürümünün nasıl yükseltileceği açıklanır. Kablo protokol sürümünü yükselttikten sonra, MongoDB için Azure Cosmos DB API 'sindeki en son işlevselliği kullanabilirsiniz. Yükseltme işlemi, hesabınızın kullanılabilirliğini kesintiye uğramaz ve herhangi bir noktada, her zaman veritabanı kapasitesini azaltır. Bu işlemden, mevcut veri veya dizinlerin hiçbiri etkilenmeyecektir.

>[!Note]
> Şu anda yalnızca sunucu sürümü 3,2 ' i kullanan hesaplar 3,6 sürümüne yükseltilebilir. Hesabınız yükseltme seçeneğini göstermezse lütfen [bir destek bileti dosyası](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)yapın.

## <a name="upgrading-from-version-32-to-36"></a>3,2 sürümünden 3,6 sürümüne yükseltme

### <a name="benefits-of-upgrading-to-version-36"></a>3\.6 sürümüne yükseltmenin avantajları

Sürüm 3,6 ' de bulunan yeni özellikler aşağıda verilmiştir:
- Gelişmiş performans ve kararlılık
- Yeni veritabanı komutları desteği
- Varsayılan ve yeni toplama aşamalarına göre toplama işlem hattı desteği
- Değişiklik akışları için destek
- Bileşik dizinler için destek
- Şu işlemler için çapraz bölüm desteği: güncelleştirme, silme, sayma ve sıralama
- Aşağıdaki toplama işlemleri için gelişmiş performans: $count, $skip, $limit ve $group
- Joker karakter oluşturma artık destekleniyor

### <a name="changes-from-version-32"></a>Sürüm 3,2 ' den değişiklikler

- **RequestRateIsLarge hatalar kaldırılmıştır** . İstemci uygulamasından gelen istekler artık 16500 hata döndürmeyecektir. Bunun yerine istekler, zaman aşımını tamamlayana veya yerine getirene kadar sürdürülecek.
- İstek başına zaman aşımı 60 saniyeye ayarlanır.
- Yeni hat Protokolü sürümünde oluşturulan MongoDB koleksiyonları yalnızca `_id` Varsayılan olarak dizinli özelliğe sahip olur.

### <a name="action-required"></a>Eylem gerekiyor

Sürüm 3,6 ' e yükseltme için veritabanı hesabı uç noktası son eki şu biçime güncelleştirilir:

```
<your_database_account_name>.mongo.cosmos.azure.com
```

Bu veritabanı hesabıyla bağlanan Uygulamalarınız ve sürücülerinizin mevcut uç noktasını değiştirmeniz gerekir. **MongoDB sürüm 3,6 ' deki özelliklere yalnızca yeni uç noktayı kullanan bağlantılar erişebilir** . Önceki uç nokta sonekine sahip olmalıdır `.documents.azure.com` .

>[!Note]
> Hesabınız bir Sogeign, kamu veya sınırlı Azure bulutu 'nda oluşturulduysa, bu uç nokta hafif farklılıklara sahip olabilir.

### <a name="how-to-upgrade"></a>Yükseltme

1. İlk olarak, Azure portal gidin ve MongoDB hesabına genel bakış dikey penceresi için Azure Cosmos DB API 'sine gidin. Sunucu sürümünüzün olduğunu doğrulayın `3.2` . 

    :::image type="content" source="./media/mongodb-version-upgrade/1.png" alt-text="MongoDB hesabına genel bakış ile Azure portal" border="false":::

2. Soldaki seçeneklerden `Features` Dikey pencereyi seçin. Bu, veritabanı hesabınız için kullanılabilen hesap düzeyi özelliklerini açığa çıkarır.

    :::image type="content" source="./media/mongodb-version-upgrade/2.png" alt-text="MongoDB hesabına genel bakış ile Azure portal" border="false":::

3. `Upgrade to Mongo server version 3.6`Satıra tıklayın. Bu seçeneği görmüyorsanız, hesabınız bu yükseltme için uygun olmayabilir. Bu durumda, lütfen [bir destek bileti](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) dosyası sağlayın.

    :::image type="content" source="./media/mongodb-version-upgrade/3.png" alt-text="MongoDB hesabına genel bakış ile Azure portal" border="false":::

4. Bu belirli yükseltmeyle ilgili olarak görünen bilgileri gözden geçirin. Yükseltmenin, bu bölümde vurgulanan şekilde, yalnızca uygulamalarınız güncelleştirilmiş uç noktası kullanana kadar tamamlanacağını unutmayın. `Enable`İşlemi başlatmak için hazırsanız açık ' a tıklayın.

    :::image type="content" source="./media/mongodb-version-upgrade/4.png" alt-text="MongoDB hesabına genel bakış ile Azure portal" border="false":::

5. İşlem başladıktan sonra, `Features` menü yükseltmenin durumunu gösterir. Durum,,, olarak değişir `Pending` `In Progress` `Upgraded` . Bu işlem, veritabanı hesabının mevcut işlevlerini veya işlemlerini etkilemez.

    :::image type="content" source="./media/mongodb-version-upgrade/5.png" alt-text="MongoDB hesabına genel bakış ile Azure portal" border="false":::

6. Yükseltme tamamlandıktan sonra durum olarak gösterilir `Upgraded` . İşlemi tamamlamak için gerçekleştirmeniz gereken sonraki adımlar ve eylemler hakkında daha fazla bilgi edinmek için buraya tıklayın. İsteğiniz işlenirken bir sorun oluşursa lütfen [desteğe başvurun](https://azure.microsoft.com/en-us/support/create-ticket/) .

    :::image type="content" source="./media/mongodb-version-upgrade/6.png" alt-text="MongoDB hesabına genel bakış ile Azure portal" border="false":::

7. **Veritabanı hesabınızın yükseltilen sürümünü kullanmaya başlamak için** dikey pencereye geri dönün `Overview` ve uygulamanızda kullanmak üzere yeni bağlantı dizesini kopyalayın. Uygulamalar, yeni uç noktaya bağlandıklarında yükseltilen sürümü kullanmaya başlar. Mevcut bağlantılar kesintiye uğramayacak ve bu işlem sizin için uygun şekilde güncelleştirilemeyebilir. Tutarlı bir deneyim sağlamak için tüm uygulamalarınızın yeni uç noktayı kullanması gerekir.

    :::image type="content" source="./media/mongodb-version-upgrade/7.png" alt-text="MongoDB hesabına genel bakış ile Azure portal" border="false":::

## <a name="next-steps"></a>Sonraki adımlar

- [MongoDB sürüm 3,6 ' in](mongodb-feature-support-36.md)desteklenen ve desteklenmeyen özellikleri hakkında bilgi edinin.
- Daha fazla bilgi için [Mongo 3,6 sürüm özelliklerini](https://devblogs.microsoft.com/cosmosdb/azure-cosmos-dbs-api-for-mongodb-now-supports-server-version-3-6/) inceleyin
