---
title: Azure purview 'da veri kaynaklarını yönetme (Önizleme)
description: Yeni veri kaynaklarını kaydetme, veri kaynakları koleksiyonlarını yönetme ve kaynakları Azure purview (Önizleme) içinde görüntüleme hakkında bilgi edinin.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/25/2020
ms.openlocfilehash: 90a873b7de6ccc1ba21a05bf4c0e288ed668cac0
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101694470"
---
# <a name="manage-data-sources-in-azure-purview-preview"></a>Azure purview 'da veri kaynaklarını yönetme (Önizleme)

Bu makalede, yeni veri kaynaklarını kaydetme, veri kaynakları koleksiyonlarını yönetme ve kaynakları Azure purview (Önizleme) içinde görüntüleme hakkında bilgi edineceksiniz.

## <a name="register-a-new-source"></a>Yeni bir kaynak Kaydet

Yeni bir kaynağı kaydetmek için aşağıdaki adımları kullanın.

1. Purview Studio 'Yu açın ve **kaynakları kaydet** kutucuğunu seçin.

   :::image type="content" source="media/manage-data-sources/purview-studio.png" alt-text="Azure purview Studio":::

1. **Kaydet**' i seçin ve ardından bir kaynak türü seçin. Bu örnek, Azure Blob depolamayı kullanır. **Devam**’ı seçin.

   :::image type="content" source="media/manage-data-sources/select-source-type.png" alt-text="Kaynakları Kaydet sayfasında bir veri kaynağı türü seçin":::

1. **Kaynakları kaydet** sayfasında formu doldurun. Kaynağınız için bir ad seçin ve ilgili bilgileri girin. Hesap seçme yönteminiz olarak **Azure aboneliğinden** seçeneğini belirlediyseniz, aboneliğinizdeki kaynaklar bir açılan listede görüntülenir. Alternatif olarak, kaynak bilgilerinizi el ile de girebilirsiniz.

   :::image type="content" source="media/manage-data-sources/register-sources-form.png" alt-text="Veri kaynağı bilgileri formu":::

1. **Son**'u seçin.

## <a name="view-sources"></a>Kaynakları görüntüle

Tüm kayıtlı kaynakları Azure purview Studio 'nun **kaynaklar** sekmesinde görüntüleyebilirsiniz. İki görünüm türü vardır: harita görünümü ve liste görünümü.

### <a name="map-view"></a>Harita görünümü

Harita görünümü ' nde tüm kaynaklarınızın ve koleksiyonlarınızın tümünü görebilirsiniz. Aşağıdaki görüntüde, bir Azure Blob Depolama kaynağı vardır. Her kaynak kutucuğunda kaynağı düzenleyebilir, yeni bir tarama başlatabilir veya kaynak ayrıntılarını görüntüleyebilirsiniz.

:::image type="content" source="media/manage-data-sources/map-view.png" alt-text="Azure purview veri kaynağı eşleme görünümü":::

### <a name="list-view"></a>Liste görünümü

Liste görünümünde, sıralanabilir kaynak listesini görebilirsiniz. Düzenleme, yeni tarama başlatma veya silme seçenekleri için kaynağın üzerine gelin.

:::image type="content" source="media/manage-data-sources/list-view.png" alt-text="Azure purview veri kaynağı liste görünümü":::

## <a name="manage-collections"></a>Koleksiyonları Yönet

Veri kaynaklarınızı koleksiyonlar halinde gruplandırabilirsiniz. Yeni bir koleksiyon oluşturmak için Azure purview Studio 'nun *kaynaklar* sayfasında **+ Yeni koleksiyon** ' u seçin. Koleksiyona bir ad verin ve üst öğe olarak *hiçbiri* ' ni seçin. Yeni koleksiyon harita görünümünde görüntülenir.

Bir koleksiyona kaynak eklemek için, kaynak üzerinde **düzenleme** kurşun kalem ' i seçin ve **Koleksiyon Seç** açılan menüsünden bir koleksiyon seçin.

Bir koleksiyon hiyerarşisi oluşturmak için, daha üst düzey koleksiyonlar alt düzey koleksiyonlara üst öğe olarak atayın. Aşağıdaki görüntüde, *fabrikam* bir Azure Blob depolama veri kaynağı içeren *finans* koleksiyonunun bir üst öğesidir. Düzeyler arasındaki oka eklenen daireye tıklayarak koleksiyonları daraltabilir veya genişletebilirsiniz.

:::image type="content" source="media/manage-data-sources/collections.png" alt-text="Azure purview Studio 'da koleksiyonların hiyerarşisi":::

Üst öğe için *hiçbiri* seçeneğini belirleyerek bir hiyerarşiden kaynakları kaldırabilirsiniz. Üst öğe olmayan kaynaklar harita görünümündeki noktalı bir kutuda gruplandırılır ve bunları üst öğeye bağlamayı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Çeşitli veri kaynaklarını nasıl kaydedeceğinizi ve tarayacağını öğrenin:

* [Azure Data Lake Storage 2. Nesil](register-scan-adls-gen2.md)
* [Power BI kiracı](register-scan-power-bi-tenant.md)
* [Azure SQL Veritabanı](register-scan-azure-sql-database.md)
