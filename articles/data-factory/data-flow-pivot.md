---
title: Haritalama veri akışı Pivot Dönüşümü
description: Azure Veri Fabrikası eşleme veri akışı Pivot Dönüşümü'ni kullanarak satırlardan sütunlara veri döndürme
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/30/2019
ms.openlocfilehash: 8f23b5e61e1aee83172a12466fac8d5b5003fea8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74930295"
---
# <a name="azure-data-factory-pivot-transformation"></a>Azure veri fabrikası pivot dönüşümü


ADF Veri Akışı'nda Pivot'u, bir veya daha fazla gruplandırma sütununun ayrı satır değerlerinin tek tek sütunlara dönüştürüldüğü bir toplama olarak kullanın. Esasen, satır değerlerini yeni sütunlara döndürebilirsiniz (verileri meta verilere dönüştürün).

![Pivot seçenekleri](media/data-flow/pivot1.png "pivot 1")

## <a name="group-by"></a>Gruplandırma ölçütü:

![Pivot seçenekleri](media/data-flow/pivot2.png "pivot 2")

İlk olarak, özet toplama için gruplandırmak istediğiniz sütunları ayarlayın. Sütun listesinin yanındaki + işaretiyle 1'den fazla sütun ayarlayabilirsiniz.

## <a name="pivot-key"></a>Pivot tuşu

![Pivot seçenekleri](media/data-flow/pivot3.png "pivot 3")

Pivot Key, ADF'nin satırdan sütuna döneceği sütundur. Varsayılan olarak, bu alanın veri kümesindeki her benzersiz değer bir sütuna döner. Ancak, isteğe bağlı olarak sütun değerlerine döndürmek istediğiniz veri kümesinden değerleri girebilirsiniz. Bu, oluşturulacak yeni sütunları belirleyecek sütundur.

## <a name="pivoted-columns"></a>Döndürülen sütunlar

![Pivot seçenekleri](media/data-flow/pivot4.png "pivot 4")

Son olarak, döndürülen değerler için kullanmak istediğiniz toplamayı ve sütunların dönüşümden yeni çıktı projeksiyonunda nasıl görüntülenmesini istediğinizi seçeceksiniz.

(İsteğe bağlı) Satır değerlerinden her yeni sütun adına eklenecek bir önek, orta ve sonek içeren bir adlandırma deseni ayarlayabilirsiniz.

Örneğin, "Bölge" ile "Satışlar"ı döndürmek, her satış değerinden yeni sütun değerlerine neden olur, yani "25", "50", "1000", vb. Ancak, "Satışlar-" bir önek değeri ayarlarsanız, her sütun değeri değerin başına "Satış-" ekler.

![Pivot seçenekleri](media/data-flow/pivot5.png "pivot 5")

Sütun Düzenini "Normal" olarak ayarlamak, döndürülen sütunların tümünün birleştirilmiş değerleriyle bir araya gelecektir. Sütun düzenlemesini "Yanal" olarak ayarlamak, sütun ve değer arasında geçiş yapacaktır.

### <a name="aggregation"></a>Toplama

Pivot değerleri için kullanmak istediğiniz toplamayı ayarlamak için, Pivoted Columns bölmesinin altındaki alana tıklayın. ADF Veri Akışı ifade oluşturucusuna girerek, bir toplama ifadesi oluşturabileceğiniz ve yeni toplanan değerleriniz için açıklayıcı bir takma ad verebileceğiniz bir ifade oluşturucuya girersiniz.

İfade Oluşturucu'daki pivoted sütun dönüşümlerini açıklamak için ADF https://aka.ms/dataflowexpressionsVeri Akışı İfadeSi Dilini kullanın: .

## <a name="pivot-metadata"></a>Pivot meta verileri

Özet dönüştürme, gelen verilerinize dayalı dinamik yeni sütun adları oluşturur. Pivot Key, her yeni sütun adı için değerleri üretir. Tek tek değerler belirtmez ve Pivot Anahtarınızdaki her benzersiz değer için dinamik sütun adları oluşturmak isterseniz, Kullanıcı Arabirimi meta verileri Inspect'de görüntülemez ve Lavabo dönüşümüiçin sütun yayılımı olmaz. Pivot Key için değerler ayarlarsanız, ADF yeni sütun adlarını belirleyebilir ve bu sütun adları İncele ve Betme eşlemesinde sizin için kullanılabilir.

### <a name="generate-a-new-model-from-dynamic-columns"></a>Dinamik sütunlardan yeni bir model oluşturma

Özet, satır değerlerini temel alan dinamik olarak yeni sütun adları oluşturur. Bu yeni sütunları, veri akışınızda daha sonra başvurulan meta verilere dönüştürebilirsiniz. Bunu yapmak için Veri Önizleme sekmesini tıklatın. Pivot dönüşümünüzün oluşturduğu tüm yeni sütunlar, tablo başlığında "sürüklenmiş" simgesiyle görünür. Bu yeni sütunları meta verilere dönüştürmek ve veri akışınızın modelinin bir parçası haline getirmek için "Harita sürüklendi" düğmesine tıklayın.

![Sütunları özetleme](media/data-flow/newpivot1.png "Harita sürüklendi Pivot sütunları")

### <a name="landing-new-columns-in-sink"></a>Lavabo'da yeni sütunlar iniş

Pivot'ta dinamik sütun adlarıyla bile, yeni sütun adlarınızı ve değerlerinizi hedef mağazanıza batırabilirsiniz. Lavabo ayarlarınızda "Şema Drift'e İzin Ver"i ayarlamanız gerekiyor. Sütun meta verilerinizde yeni dinamik adları görmezsiniz, ancak şema kayması seçeneği verileri elde etmenizi sağlar.

### <a name="view-metadata-in-design-mode"></a>Meta verileri tasarım modunda görüntüleme

Yeni sütun adlarını Denetle'de meta veri olarak görüntülemek istiyorsanız ve sütunların Lavabo dönüşümüne açıkça yayınılmasını görmek istiyorsanız, Pivot Key sekmesinde açık Değerler ayarlayın.

### <a name="how-to-rejoin-original-fields"></a>Özgün alanlara yeniden katılma
Özet dönüştürme yalnızca toplama, gruplandırma ve pivot eylemkullanılan sütunları yansıtacak. Akışınızın önceki adımındaki diğer sütunları eklemek istiyorsanız, önceki adımdan yeni bir Dal kullanın ve akışı özgün meta verilere bağlamak için kendi kendine birleştirme deseni kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Sütun değerlerini satır değerlerine dönüştürmek için [unpivot dönüşümdeneyin.](data-flow-unpivot.md) 
