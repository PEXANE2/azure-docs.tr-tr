---
title: Veri akışı parçacıklarını kullanarak satırları kaldırma ve null değerleri bulma
description: Veri akışlarında kod parçacıklarını kullanarak satırları kolayca kaldırma ve null değerleri bulma hakkında bilgi edinin
services: data-factory
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/30/2020
ms.author: makromer
ms.openlocfilehash: 841484a647d2737d621c75ebe63f65f2de829a26
ms.sourcegitcommit: 67e8e1caa8427c1d78f6426c70bf8339a8b4e01d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2020
ms.locfileid: "91666526"
---
# <a name="dedupe-rows-and-find-nulls-using-data-flow-snippets"></a>Veri akışı parçacıklarını kullanarak satırları kaldırma ve null değerleri bulma

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Veri akışlarını eşleme bölümünde kod parçacıkları kullanarak, yinelenen verileri kaldırma ve null filtreleme gibi genel görevleri çok daha kolay bir şekilde gerçekleştirebilirsiniz. Bu nasıl yapılır Kılavuzu, veri akışı komut parçacıkları kullanarak bu işlevlerin işlem hatlarınızı kolayca nasıl ekleneceğini açıklar.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4GnhH]

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

1. Yeni bir işlem hattı oluşturmak için **+ Yeni Işlem hattı** ' nı seçin.

2. Veri akışı etkinliği ekleyin.

3. Kaynak dönüşümü ekleme ve veri kümelerinizin birine bağlama

    ![Kaynak kod parçacığı 2](media/data-flow/snippet-adf-2.png)

4. Yinelenenleri kaldırma ve null denetim parçacıkları, veri akışı şeması drlarından yararlanan genel desenler kullanır, bu nedenle veri kümesinizden herhangi bir şema ile veya önceden tanımlanmış bir şemaya sahip olmayan veri kümeleriyle çalışır.

5. [Veri akışı betiği belgeleri sayfasına gidin ve kod parçacığını ayrı satırlar için kopyalayın.](https://docs.microsoft.com/azure/data-factory/data-flow-script#distinct-row-using-all-columns)

6. Veri akışı Tasarımcısı Kullanıcı arabiriminizde, veri akışı grafiğinin arkasındaki betik düzenleyicisini açmak için sağ üst köşedeki betik düğmesine tıklayın.

    ![Kaynak kod parçacığı 3](media/data-flow/snippet-adf-3.png)

7. ```source1```Betiğiniz tanımınızda, ENTER tuşuna basın ve ardından kod parçacığına yapıştırın.

8. Yapıştırılan kodun önüne "source1" yazarak, bu yapıştırılan kod parçacığını grafikte oluşturduğunuz önceki kaynak dönüşümüne bağlayacaksınız.

9. Alternatif olarak, grafikteki yeni dönüştürme düğümünden gelen akışı seçerek tasarımcıda yeni dönüşümü de bağlayabilirsiniz.

    ![Kaynak kod parçacığı 4](media/data-flow/snippet-adf-4.png)

10. Artık veri akışınız, tüm sütun değerlerinde genel bir karma kullanarak, tüm satırlara göre gruplayan toplam dönüşümü kullanarak kaynağınızdan yinelenen satırları kaldırır.
    
11. Daha sonra, verilerinizi null içeren ve null olmayan bir akışa sahip bir akışa bölmek için bir kod parçacığı ekleyeceğiz.

12. [Kod parçacığı kitaplığına geri dönün ve bu sefer NULL denetimleri için kodu kopyalayın.](https://docs.microsoft.com/azure/data-factory/data-flow-script#check-for-nulls-in-all-columns)

13. Veri akışı tasarımcısında, komut dosyası ' na tıklayın ve ardından bu yeni dönüştürme kodunu en alta yapıştırın ve yapıştırılan kod parçacığının önüne bu dönüşümün adını yazarak önceki dönüşümünüze bağlayarak.

14. Veri akışı grafiğiniz artık şuna benzer görünmelidir:

    ![Kaynak kod parçacığı 1](media/data-flow/snippet-adf-1.png)

  Artık, veri akışı betik kitaplığından var olan kod parçacıklarını gerçekleştirerek ve bunları mevcut tasarımınıza ekleyerek genel yinelenenleri kaldırma ve NULL denetimleri olan bir çalışma veri akışınız vardır.

## <a name="next-steps"></a>Sonraki adımlar

* Veri akışı [dönüştürmelerini](concepts-data-flow-overview.md)eşleme kullanarak veri akışı mantığınızın geri kalanını oluşturun.
