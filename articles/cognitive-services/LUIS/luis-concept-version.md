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
ms.date: 09/02/2019
ms.author: diberry
ms.openlocfilehash: c519b030aaee58397766ecb8658e7af08b5986e1
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70256867"
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

Aktarılan dosya, uygulama alındıktan sonra geri alındığından, makine tarafından öğrenilen bilgiler içermez. İçe aktarılmış dosya, katkıda bulunan bilgilerini içermiyor.

## <a name="export-each-version-as-app-backup"></a>Her sürümü uygulama yedeklemesi olarak dışarı aktar
LUSıS uygulamanızı yedeklemek için, **[Ayarlar](luis-how-to-manage-versions.md)** sayfasındaki her sürümü dışarı aktarın.

## <a name="delete-a-version"></a>Bir sürümü Sil
Ayarlar sayfasındaki sürümler listesinden etkin sürüm dışındaki tüm sürümleri silebilirsiniz. 

## <a name="version-availability-at-the-endpoint"></a>Uç noktada sürüm kullanılabilirliği
Eğitilen sürümler, uygulama [uç](luis-glossary.md#endpoint)noktanıza otomatik olarak kullanılamaz. Uygulamanın uç noktanıza kullanılabilmesi için bir sürümü [yayımlamanız](luis-how-to-publish-app.md) veya yeniden yayımlamanız gerekir. **Hazırlama** ve **üretime**yayınlayabilirsiniz ve bu sayede, en fazla uygulamanın uç noktada kullanılabilir olan iki sürümü sağlayabilirsiniz. Bir uç noktada uygulamanın daha fazla sürümü varsa, sürümü dışarı aktarıp yeni bir uygulamaya yeniden içeri aktarmanız gerekir. Yeni uygulamanın farklı bir uygulama KIMLIĞI vardır.

## <a name="manage-multiple-versions-inside-the-same-app"></a>Aynı uygulama içinde birden çok sürümlerini yönetme
Başlayın [kopyalama](luis-how-to-manage-versions.md#clone-a-version), her yazar için bir temel sürüm. 

Her yazar uygulamanın kendi sürümünde değişiklik yapar. Her geliştirici modeliyle memnun olduğunda, yeni sürümleri JSON dosyasına dışarı aktarın.  

Değişiklikler için karşılaştırılabilir JSON biçimli dosyaları dışarı aktarılan uygulamalardır. Yeni sürümü tek bir JSON dosyası oluşturmak için dosyaları birleştirin. Değişiklik **VersionID** yeni birleştirilmiş sürüm belirtmek için JSON özelliği. Bu sürüm, özgün uygulamaya aktarma. 

Bu yöntem bir etkin sürüm, bir aşama sürümü ve bir yayımlanmış sürümüne sahip olmanızı sağlar. Etkin sürüm ile ilgili sonuçları, [etkileşimli test bölmesindeki](luis-interactive-test.md)yayımlanmış bir sürümle (aşama veya üretim) karşılaştırabilirsiniz.

## <a name="manage-multiple-versions-as-apps"></a>Birden çok sürümü uygulamaları yönetme
[Dışarı aktarma](luis-how-to-manage-versions.md#export-version) temel sürüm. Her geliştirici sürümünü alır. Uygulamayı alır kişi sürüm sahibidir. Bunların ne zaman yapılır dışarı aktarma sürümü uygulama değiştirme. 

Değişiklikler için temel dışarı aktarma ile karşılaştırılabilir JSON biçimli dosyaları dışarı aktarılan uygulamalardır. Yeni sürümü tek bir JSON dosyası oluşturmak için dosyaları birleştirin. Değişiklik **VersionID** yeni birleştirilmiş sürüm belirtmek için JSON özelliği. Bu sürüm, özgün uygulamaya aktarma.

## <a name="contributions-from-collaborators"></a>Ortak çalışanlarla katkı

[Ortak çalışanlarla](luis-how-to-collaborate.md)ilgili katkıları yazma hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Bkz. uygulama ayarları sayfasında [sürüm oluşturma](luis-how-to-manage-versions.md) ekleme. 

Modele [amaçları](luis-concept-intent.md) nasıl tasarlayacağınızı öğrenin.
