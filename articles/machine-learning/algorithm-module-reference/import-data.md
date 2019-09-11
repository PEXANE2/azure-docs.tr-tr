---
title: 'Verileri içeri aktar: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Mevcut bulut veri hizmetlerinden bir makine öğrenimi denemesine veri yüklemek için Azure Machine Learning hizmetinde veri alma modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: e8522c759efb0cc599bbcf8e7569f6045da571a8
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128780"
---
# <a name="import-data-module"></a>Veri modülünü içeri aktarma

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

Mevcut bulut veri hizmetlerinden bir makine öğrenimi denemesine veri yüklemek için bu modülü kullanın.  
Modül artık, bir depolama seçeneği seçmenize ve mevcut abonelikler arasında seçim yapmanıza ve tüm seçenekleri hızlıca yapılandırmak için bir Sihirbazı sunar. Mevcut bir veri bağlantısını düzenlemeniz mı gerekiyor? Sorun yok; Sihirbaz, sıfırdan tekrar başlamak zorunda kalmaması için önceki tüm yapılandırma ayrıntılarını yükler. 
  
İstediğiniz verileri tanımladıktan ve kaynağa bağlandıktan sonra, [verileri Içeri aktarın](./import-data.md) her bir sütunun veri türünü içerdiği değerlere göre alır ve verileri Azure Machine Learning çalışma alanınıza yükler. [Içeri aktarma verilerinin](./import-data.md) çıktısı, herhangi bir deneyle kullanılabilecek bir veri kümesidir.

  
Kaynak verileriniz değişirse, [verileri Içeri aktar](./import-data.md)' ı yeniden çalıştırarak veri kümesini yenileyebilir ve yeni veri ekleyebilirsiniz. Ancak, denemeyi her çalıştırdığınızda kaynaktan yeniden okumak istemiyorsanız, **önbelleğe alınmış sonuçları kullan** seçeneğini true olarak seçin. Bu seçenek belirlendiğinde modül, deneme 'nın daha önce aynı kaynak ve aynı giriş seçeneklerini kullanıp çalıştırılmadığını denetler. Önceki bir çalıştırma bulunursa, verileri kaynaktan yeniden yükleme yerine önbellekteki veriler kullanılır.
 

## <a name="data-sources"></a>Veri kaynakları

Veri Içeri aktarma modülü aşağıdaki veri kaynaklarını destekler. Ayrıntılı yönergeler ve her bir veri kaynağını kullanma örnekleri için bağlantılara tıklayın. 
 
Verilerinizi nasıl veya nerede depolayabileceğiniz konusunda emin değilseniz, veri bilimi işlemindeki yaygın veri senaryolarına yönelik bu kılavuza bakın:  [Azure Machine Learning gelişmiş analiz senaryoları](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-plan-sample-scenarios). 


|Veri kaynağı| Birlikte kullanın|
|-----------|-----------|  
|[HTTP aracılığıyla Web URL 'SI](./import-from-web-url-via-http.md)|HTTP kullanan ve CSV, TSV, ARFF veya SvmLight biçimlerinde sağlanmış olan bir Web URL 'sinde barındırılan verileri alın|  
|[Azure Blob depolamadan içeri aktarma](./import-from-azure-blob-storage.md) |Azure Blob hizmetinde depolanan verileri al|  

## <a name="how-to-use-import-data"></a>Içeri aktarma verileri nasıl kullanılır
 
1. **Veri alma** modülünü denemenize ekleyin. Bu modülü, arabirimindeki **veri girişi ve çıkış** kategorisinde bulabilirsiniz.

2. Sihirbaz kullanarak veri kaynağını yapılandırmak için **veri alma Sihirbazı 'Nı Başlat** ' a tıklayın.

    Sihirbaz hesap adını ve kimlik bilgilerini alır ve diğer seçenekleri yapılandırmanıza yardımcı olur. Var olan bir yapılandırmayı düzenliyorsanız, önce geçerli değerleri yükler.

3. Sihirbazı kullanmak istemiyorsanız **veri kaynağı**' na tıklayın ve okuduğunuz bulut tabanlı depolama türünü seçin. 

    Ek ayarlar, seçtiğiniz depolama türüne ve depolamanın güvenli olup olmadığına bağlıdır. Hesap adını, dosya türünü veya kimlik bilgilerini sağlamanız gerekebilir. Bazı kaynaklar kimlik doğrulaması gerektirmez; başkaları için hesap adını, bir anahtarı veya kapsayıcı adını bilmeniz gerekebilir.

4. Veri kümesini art arda çalışma sırasında yeniden kullanmak üzere önbelleğe almak istiyorsanız **önbelleğe alınmış sonuçları kullan** seçeneğini belirleyin.

    Modül parametrelerinde başka değişiklik olmadığı varsayılarak, deneme yalnızca modülün ilk kez çalıştırıldığı zaman verileri yükler ve bundan sonra veri kümesinin önbelleğe alınmış bir sürümünü kullanır.

    Denemeyi her çalıştırışınızda verileri yeniden yüklemeniz gerekiyorsa bu seçeneğin işaretini kaldırın.

5. Denemeyi çalıştırın.

    Verileri Içeri aktar, verileri arayüze yüklediğinde, her sütunun veri türünü, sayısal veya kategorik, içerdiği değerlere göre alır.

    - Bir üst bilgi varsa, çıkış veri kümesinin sütunlarını adlandırmak için üst bilgi kullanılır.

    - Verilerde mevcut sütun başlıkları yoksa, yeni sütun adları Sütun1, col2 biçimi kullanılarak üretilir,... , Coln *.

## <a name="results"></a>Sonuçlar

İçeri aktarma tamamlandığında, çıktı veri kümesine tıklayın ve verilerin başarıyla içeri aktarılmadığını görmek için **Görselleştir** ' i seçin.

Her denemede yeni bir veri kümesini içeri aktarmak yerine yeniden kullanmak üzere verileri kaydetmek istiyorsanız, çıkışa sağ tıklayın ve veri **kümesi olarak kaydet**' i seçin. Veri kümesi için bir ad seçin. Kaydedilen veri kümesi, kaydetme sırasında verileri korur ve denemeyle veri kümesi değiştiğinde bile deneme yeniden çalıştırıldığında veriler güncellenmez. Bu, verilerin anlık görüntülerini almak için yararlı olabilir.

Verileri içeri aktardıktan sonra, modelleme ve analiz için bazı ek hazırlıklar gerekebilir:


- Sütun adlarını değiştirmek, bir sütunu farklı bir veri türü olarak işlemek veya bazı sütunların Etiketler ya da Özellikler olduğunu göstermek için [meta verileri Düzenle](./edit-metadata.md) ' yi kullanın.

- Modelde dönüştürülecek veya kullanılacak sütunların bir alt kümesini seçmek için [veri kümesindeki sütunları seç '](./select-columns-in-dataset.md) i kullanın. Dönüştürülen veya kaldırılan sütunlar, [sütun Ekle](./add-columns.md) modülü kullanılarak, özgün veri kümesine kolayca yeniden katılabilir.  

- Veri kümesini bölmek, örnekleme gerçekleştirmek veya ilk n satırı almak için [bölüm ve örnek](./partition-and-sample.md) kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 