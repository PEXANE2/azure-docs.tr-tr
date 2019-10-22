---
title: 'İçeri aktarma verileri: modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Mevcut bulut veri hizmetlerinden bir makine öğrenimi ardışık düzenine veri yüklemek için Azure Machine Learning hizmetinde veri alma modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: fef7d686479b24b0402ab6f1e6990df74231b8d6
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693146"
---
# <a name="import-data-module"></a>Veri modülünü içeri aktarma

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

Mevcut bulut veri hizmetlerinden bir makine öğrenimi ardışık düzenine veri yüklemek için bu modülü kullanın.  

İlk olarak, okuduğunuz bulut tabanlı depolama türünü seçin ve ek ayarları sona erdirin. İstediğiniz verileri tanımladıktan ve kaynağa bağlandıktan sonra, [verileri Içeri aktarın](./import-data.md) her bir sütunun veri türünü içerdiği değerlere göre alır ve verileri Azure Machine Learning çalışma alanınıza yükler. [Içeri aktarma verilerinin](./import-data.md) çıktısı, herhangi bir ardışık düzen ile kullanılabilen bir veri kümesidir.

  
Kaynak verileriniz değişirse, [verileri Içeri aktar](./import-data.md)' ı yeniden çalıştırarak veri kümesini yenileyebilir ve yeni veri ekleyebilirsiniz. Ancak, işlem hattını her çalıştırdığınızda kaynaktan yeniden okumak istemiyorsanız, **önbelleğe alınmış sonuçları kullan** seçeneğini true olarak seçin. Bu seçenek belirlendiğinde modül, işlem hattının, aynı kaynak ve aynı giriş seçeneklerini kullanarak daha önce çalıştırılıp çalıştırılmadığını denetler. Önceki bir çalıştırma bulunursa, verileri kaynaktan yeniden yükleme yerine önbellekteki veriler kullanılır.
 

## <a name="data-sources"></a>Veri kaynakları

Veri Içeri aktarma modülü aşağıdaki veri kaynaklarını destekler. Ayrıntılı yönergeler ve her bir veri kaynağını kullanma örnekleri için bağlantılara tıklayın. 
 
Verilerinizi nasıl veya nerede depolayabileceğiniz konusunda emin değilseniz, veri bilimi işlemindeki genel veri senaryolarına yönelik bu kılavuza bakın: [Azure Machine Learning gelişmiş analizler Için senaryolar](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-plan-sample-scenarios). 


|Veri kaynağı| Birlikte kullanın|
|-----------|-----------|  
|[HTTP aracılığıyla Web URL 'SI](./import-from-web-url-via-http.md)|HTTP kullanan ve CSV, TSV, ARFF veya SvmLight biçimlerinde sağlanmış olan bir Web URL 'sinde barındırılan verileri alın|  
|[Azure Blob depolamadan içeri aktarma](./import-from-azure-blob-storage.md) |Azure Blob hizmetinde depolanan verileri al|  
|[Azure SQL veritabanından içeri aktar](./import-from-azure-sql-database.md) |Azure SQL veritabanından veri al|

## <a name="how-to-configure-import-data"></a>Içeri aktarma verilerini yapılandırma
 
1. **Veri alım** modülünü işlem hattınızı ekleyin. Bu modülü, arabirimindeki **veri girişi ve çıkış** kategorisinde bulabilirsiniz.

1. **Veri kaynağı**' na tıklayın ve okuduğunuz bulut tabanlı depolama türünü seçin. 

    Ek ayarlar, seçtiğiniz depolama türüne ve depolamanın güvenli olup olmadığına bağlıdır. Hesap adını, dosya türünü veya kimlik bilgilerini sağlamanız gerekebilir. Bazı kaynaklar kimlik doğrulaması gerektirmez; başkaları için hesap adını, bir anahtarı veya kapsayıcı adını bilmeniz gerekebilir.

1. Veri kümesini art arda çalışma sırasında yeniden kullanmak üzere önbelleğe almak istiyorsanız **önbelleğe alınmış sonuçları kullan** seçeneğini belirleyin.

    Modül parametrelerinde başka değişiklik olmadığı varsayılarak, işlem hattı yalnızca modülün ilk kez çalıştırıldığı sırada verileri yükler ve bundan sonra veri kümesinin önbelleğe alınmış bir sürümünü kullanır.

    İşlem hattını her çalıştırdığınızda verileri yeniden yüklemeniz gerekiyorsa bu seçeneğin işaretini kaldırın.

1. İşlem hattını çalıştırma.

    Verileri Içeri aktar, verileri arayüze yüklediğinde, her sütunun veri türünü, sayısal veya kategorik, içerdiği değerlere göre alır.

    - Bir üst bilgi varsa, çıkış veri kümesinin sütunlarını adlandırmak için üst bilgi kullanılır.

    - Verilerde mevcut sütun başlıkları yoksa, yeni sütun adları Sütun1, col2 biçimi kullanılarak üretilir,... , Coln *.

## <a name="results"></a>Sonuçlar

İçeri aktarma tamamlandığında, çıktı veri kümesine tıklayın ve verilerin başarıyla içeri aktarılmadığını görmek için **Görselleştir** ' i seçin.

İşlem hattı her çalıştırıldığında yeni bir veri kümesini içeri aktarmak yerine yeniden kullanmak üzere verileri kaydetmek istiyorsanız, çıkışa sağ tıklayın ve veri **kümesi olarak kaydet**' i seçin. Veri kümesi için bir ad seçin. Kaydedilen veri kümesi verileri kaydetme sırasında korur ve işlem hattında veri kümesi değişse bile, ardışık düzen yeniden çalıştırıldığında veriler güncellenmez. Bu, verilerin anlık görüntülerini almak için yararlı olabilir.

Verileri içeri aktardıktan sonra, modelleme ve analiz için bazı ek hazırlıklar gerekebilir:


- Sütun adlarını değiştirmek, bir sütunu farklı bir veri türü olarak işlemek veya bazı sütunların Etiketler ya da Özellikler olduğunu göstermek için [meta verileri Düzenle](./edit-metadata.md) ' yi kullanın.

- Modelde dönüştürülecek veya kullanılacak sütunların bir alt kümesini seçmek için [veri kümesindeki sütunları seç '](./select-columns-in-dataset.md) i kullanın. Dönüştürülen veya kaldırılan sütunlar, [sütun Ekle](./add-columns.md) modülü kullanılarak, özgün veri kümesine kolayca yeniden katılabilir.  

- Veri kümesini bölmek, örnekleme gerçekleştirmek veya ilk n satırı almak için [bölüm ve örnek](./partition-and-sample.md) kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 