---
title: Büyük veri kümeleriyle çalışma
description: Azure Kaynak Grafında çalışırken büyük veri kümelerinin nasıl alınacağını ve kontrol olduğunu anlayın.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/01/2019
ms.topic: conceptual
ms.service: resource-graph
ms.openlocfilehash: 4da890a5ef7acb44d0e8628dc4ec3904f6a065e4
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2019
ms.locfileid: "71980277"
---
# <a name="working-with-large-azure-resource-data-sets"></a>Büyük Azure Kaynak veri kümeleriyle çalışma

Azure Kaynak Grafiği, Azure ortamınızdaki kaynaklarla çalışmak ve bunlarla ilgili bilgi almak için tasarlanmıştır. Kaynak Grafiği, binlerce kaydı sorgularken bile bu verilerin hızlı bir şekilde alınmasını sağlar. Kaynak grafiğinde bu büyük veri kümeleriyle çalışmak için çeşitli seçenekler bulunur.

Yüksek bir sıklıkta sorgularla çalışmaya ilişkin yönergeler için bkz. [Kısıtlanmış istekler Için rehberlik](./guidance-for-throttled-requests.md).

## <a name="data-set-result-size"></a>Veri kümesi sonuç boyutu

Varsayılan olarak, kaynak Graph tüm sorgular yalnızca **100** kayıt döndürüyor şekilde kısıtlar. Bu denetim, büyük veri kümelerine yol açacak istemeden yapılan sorgulardan hem kullanıcıyı hem de hizmeti korur. Bu olay çoğu zaman bir müşteri, kaynakları belirli ihtiyaçlarına uygun şekilde bulmak ve filtrelemek için sorguları deneydiğinde oluşur. Bu denetim, sonuçları sınırlamak için [üst](/azure/kusto/query/topoperator) veya Azure Veri Gezgini Dil işleçlerini [sınırlandırtan](/azure/kusto/query/limitoperator) farklıdır.

> [!NOTE]
> **İlk**olarak, sonuçları `asc` veya `desc` olan en az bir sütuna göre sıralamak önerilir. Sıralama yapmadan döndürülen sonuçlar rastgele ve tekrarlanabilir değildir.

Varsayılan sınır, kaynak Graph ile etkileşimde bulunmak için tüm yöntemler aracılığıyla geçersiz kılınabilir. Aşağıdaki örneklerde veri kümesi boyut sınırının _200_olarak nasıl değiştirileceği gösterilmektedir:

```azurecli-interactive
az graph query -q "project name | order by name asc" --first 200 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "project name | order by name asc" -First 200
```

[REST API](/rest/api/azureresourcegraph/resources/resources), Denetim **$top** ve **queryrequestoptions**'ın bir parçasıdır.

_En kısıtlayıcı_ olan denetim kazanacaktır. Örneğin, sorgunuz **top** veya **limit** işleçlerini kullanıyorsa ve **ilk**olarak daha fazla kayıt ile sonuçlanacaksa, döndürülen en fazla kayıt sayısı **birincisine**eşittir. Benzer şekilde, **top** veya **limit** **birinciden**küçükse, döndürülen kayıt kümesi **top** veya **limit**tarafından yapılandırılan daha küçük bir değer olacaktır.

**Önce** Şu anda izin verilen en fazla _5000_değeri vardır.

## <a name="skipping-records"></a>Kayıtlar atlanıyor

Büyük veri kümeleriyle çalışma için sonraki seçenek, **atlama** denetimidir. Bu denetim, sorgunuzun sonuçları döndürmeden önce tanımlanan kayıt sayısını atlamasını veya atlamasını sağlar. **Skip** , sonuçları, sonuç kümesinin ortasında bir yerde bir yere alacağınız anlamlı bir şekilde sıralayan sorgular için yararlıdır. Gereken sonuçlar döndürülen veri kümesinin sonunda ise, farklı bir sıralama yapılandırması kullanmak ve bunun yerine veri kümesinin en üstünden sonuçları almak daha etkilidir.

> [!NOTE]
> **Atla**kullanırken sonuçları en az bir sütundan `asc` veya `desc` ' ye göre sıralamak önerilir. Sıralama yapmadan döndürülen sonuçlar rastgele ve tekrarlanabilir değildir.

Aşağıdaki örneklerde, bir sorgunun neden olacağı ilk _10_ kaydın nasıl atlanacağını, bunun yerine 11. kayıt ile döndürülen sonuç kümesini başlatmak gösterilmektedir:

```azurecli-interactive
az graph query -q "project name | order by name asc" --skip 10 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "project name | order by name asc" -Skip 10
```

[REST API](/rest/api/azureresourcegraph/resources/resources), Denetim **$Skip** ve **queryrequestoptions**'ın bir parçasıdır.

## <a name="paging-results"></a>Disk belleği sonuçları

Bir sonuç kümesini işlenmek üzere daha küçük kayıt kümelerine bölmek gerektiğinde veya bir sonuç kümesi, döndürülen en fazla _1000_ kayıt değerini aşacağından, sayfalama kullanın. [REST API](/rest/api/azureresourcegraph/resources/resources) **queryresponse** , bir sonuç kümesinin parçalanmış olduğunu belirten değerler sağlar: **resultkesildi** ve **$skipToken**.
**Resultkesildi** , yanıtta ek kayıtlar döndürülmediğinde tüketiciyi bildiren bir Boole değeridir. **Count** özelliği **totalRecords** özelliğinden daha az olduğunda bu durum da tanımlanabilir. **totalRecords** sorguyla eşleşen kaç kayıt olduğunu tanımlar.

**Resultkesilme** **değeri true**olduğunda **$skipToken** özelliği yanıtta ayarlanır. Bu değer, sorguyla eşleşen bir sonraki kayıt kümesini almak için aynı sorgu ve abonelik değerleriyle birlikte kullanılır.

Aşağıdaki örneklerde, ilk 3000 kaydın nasıl atlanması ve Azure CLı ve Azure PowerShell ile atlandıktan sonra **ilk** 1000 kayıtlarının nasıl **döndürüleceği** gösterilmektedir:

```azurecli-interactive
az graph query -q "project id, name | order by id asc" --first 1000 --skip 3000
```

```azurepowershell-interactive
Search-AzGraph -Query "project id, name | order by id asc" -First 1000 -Skip 3000
```

> [!IMPORTANT]
> Sayfalama 'un çalışması için sorgunun **ID** alanını **projesi** gerekir. Sorguda eksik ise, yanıt **$skipToken**içermez.

Bir örnek için, REST API belgeleri içindeki [Sonraki sayfa sorgusuna](/rest/api/azureresourcegraph/resources/resources#next-page-query) bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Başlangıç sorgularında](../samples/starter.md)kullanılan dil.
- Gelişmiş [sorgularda](../samples/advanced.md)gelişmiş kullanımlar bölümüne bakın.
- [Kaynakları keşfetmeye](explore-resources.md)öğrenin.