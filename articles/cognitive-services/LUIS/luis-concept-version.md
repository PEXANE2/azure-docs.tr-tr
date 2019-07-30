---
title: Sürüm oluşturma-LUSıS
titleSuffix: Azure Cognitive Services
description: LUSıS 'deki sürümler geleneksel programlama sürümlerindeki sürümlere benzerdir. Her sürüm, uygulamanın zaman içindeki bir anlık görüntüdür. Uygulamada değişiklik yapmadan önce yeni bir sürüm oluşturun. Tam uygulamaya dönmek daha kolay hale gelir ve ardından önceki bir duruma ve uygulamanın amacını ve araslarını kullanmayı deneyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: 2e13efa70d0344defeb306a92ac405439635e929
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68619690"
---
# <a name="understand-how-and-when-to-use-a-luis-version"></a>Bir LUSıS sürümünün nasıl ve ne zaman kullanılacağını anlayın

LUSıS 'deki sürümler geleneksel programlama sürümlerindeki sürümlere benzerdir. Her sürüm, uygulamanın zaman içindeki bir anlık görüntüdür. Uygulamada değişiklik yapmadan önce yeni bir sürüm oluşturun. Tam sürüme dönmek daha kolay hale gelir ve daha sonra amaçları ve kullanımları önceki bir duruma kaldırmaya çalışır.

[Sürümleriyle](luis-how-to-manage-versions.md)aynı uygulamanın farklı modellerini oluşturun. 

## <a name="version-id"></a>Sürüm KIMLIĞI
Sürüm KIMLIĞI, karakterler, rakamlar veya '. ' karakterlerinden oluşur ve 10 karakterden uzun olamaz.

## <a name="initial-version"></a>İlk sürüm
İlk sürüm (0,1) varsayılan etkin sürümdür. 

## <a name="active-version"></a>Etkin sürüm
Etkin olarak [bir sürüm ayarlamak](luis-how-to-manage-versions.md#set-active-version) için, bu, şu anda [lusıs](luis-reference-regions.md) Web sitesinde düzenlenmekte ve test edilmiştir. Bir sürümü, verilerine erişmek, güncelleştirmeleri yapmak ve test etmek ve yayımlamak için etkin olarak ayarlayın.

Şu anda etkin olan sürümün adı, uygulama adından sonra sol bölmede görüntülenir. 

[![Etkin sürümü Değiştir](./media/luis-concept-version/version-in-nav-bar-inline.png)](./media/luis-concept-version/version-in-nav-bar-expanded.png#lightbox)

## <a name="versions-and-publishing-slots"></a>Sürümler ve yayımlama Yuvaları
Aşamada ve ürün yuvalarına yayımlayabilirsiniz. Her yuva farklı bir sürüme veya aynı sürüme sahip olabilir. Bu, botlar veya başka bir LUARA uygulama çağıran uygulamalar tarafından kullanılabilen uç nokta aracılığıyla model sürümleri arasındaki değişiklikleri doğrulamak için kullanışlıdır. 

## <a name="clone-a-version"></a>Sürümü Kopyala
Var olan bir sürümün kopyasını oluşturmak ve yeni bir sürüm olarak kaydetmek için bir sürümü klonlayın. Yeni sürüm için bir başlangıç noktası olarak, var olan sürümle aynı içeriği kullanmak için bir sürümü kopyalayın. Bir sürümü klonladıktan sonra, yeni sürüm **etkin** sürüm olur. 

## <a name="import-and-export-a-version"></a>Bir sürümü içeri ve dışarı aktarma
Uygulama düzeyinde bir sürümü içeri aktarabilirsiniz. Bu sürüm etkin sürüm olur ve uygulama dosyasının "VersionId" özelliğindeki sürüm KIMLIĞINI kullanır. Ayrıca, sürüm düzeyinde mevcut bir uygulamaya de aktarabilirsiniz. Yeni sürüm etkin sürüm olur. 

Uygulama düzeyinde bir sürümü dışarı aktarabilir veya sürüm düzeyinde bir sürümü dışarı aktarabilirsiniz. Tek fark, uygulama düzeyinde dışarı aktarılmış sürümün sürüm düzeyinde şu anda etkin olan sürümüdür, **[Ayarlar](luis-how-to-manage-versions.md)** sayfasında dışarı aktarılacak herhangi bir sürümü seçebilirsiniz. 

Aktarılan dosya, uygulama alındıktan sonra geri alındığından, makine tarafından öğrenilen bilgiler içermez. Aktarılan dosya, ortak çalışan içermiyor. sürüm yeni uygulamaya aktarıldığında bu yeniden eklemeniz gerekir.

## <a name="export-each-version-as-app-backup"></a>Her sürümü uygulama yedeklemesi olarak dışarı aktar
LUSıS uygulamanızı yedeklemek için, **[Ayarlar](luis-how-to-manage-versions.md)** sayfasındaki her sürümü dışarı aktarın.

## <a name="delete-a-version"></a>Bir sürümü Sil
Ayarlar sayfasındaki sürümler listesinden etkin sürüm dışındaki tüm sürümleri silebilirsiniz. 

## <a name="version-availability-at-the-endpoint"></a>Uç noktada sürüm kullanılabilirliği
Eğitilen sürümler, uygulama [uç](luis-glossary.md#endpoint)noktanıza otomatik olarak kullanılamaz. Uygulamanın uç noktanıza kullanılabilmesi için bir sürümü [yayımlamanız](luis-how-to-publish-app.md) veya yeniden yayımlamanız gerekir. **Hazırlama** ve **üretime**yayınlayabilirsiniz ve bu sayede, en fazla uygulamanın uç noktada kullanılabilir olan iki sürümü sağlayabilirsiniz. Bir uç noktada uygulamanın daha fazla sürümü varsa, sürümü dışarı aktarıp yeni bir uygulamaya yeniden içeri aktarmanız gerekir. Yeni uygulamanın farklı bir uygulama KIMLIĞI vardır.

## <a name="collaborators"></a>Ortak çalışanlar
Sahip ve tüm [ortak çalışanlar](luis-how-to-collaborate.md) , uygulamanın tüm sürümlerine tam erişime sahiptir.

## <a name="next-steps"></a>Sonraki adımlar

Bkz. uygulama ayarları sayfasında [sürüm oluşturma](luis-how-to-manage-versions.md) ekleme. 

Modele [amaçları](luis-concept-intent.md) nasıl tasarlayacağınızı öğrenin.
