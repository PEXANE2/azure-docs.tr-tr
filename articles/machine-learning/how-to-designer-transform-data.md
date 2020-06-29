---
title: Verileri dönüştürme
titleSuffix: Azure Machine Learning
description: Kendi veri kümelerinizi oluşturmak için Azure Machine Learning tasarımcısında verileri dönüştürmeyi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
author: peterclu
ms.author: peterlu
ms.date: 06/28/2020
ms.custom: designer
ms.openlocfilehash: ea0ba58c3eb38a54be5ff777519ca12d6a2cf9ce
ms.sourcegitcommit: 581aaca8956b1717b7bc1c1d7710c782c22e6320
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2020
ms.locfileid: "85517558"
---
# <a name="transform-data-in-azure-machine-learning-designer-preview"></a>Azure Machine Learning tasarımcısında veri dönüştürme (Önizleme)
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

Bu makalede, Machine Learning için kendi verilerinizi hazırlayabilmeniz için Azure Machine Learning tasarımcısında veri kümelerini dönüştürmeyi ve kaydetmeyi öğreneceksiniz.

İki veri kümesi hazırlamak için örnek [yetişkinlere yönelik gelir ikili sınıflandırma](sample-designer-datasets.md) veri kümesini kullanacaksınız: yalnızca Birleşik Devletler ve ABD dışı yetişkinler 'den görselleştirmenizdeki bilgilerini içeren bir veri kümesi.

Bu makalede şunları öğreneceksiniz:

1. Bir veri kümesini eğitim için hazırlamak üzere dönüştürün.
1. Elde edilen veri kümelerini bir veri deposuna aktarın.
1. Sonuçları görüntüleyin.

Bu nasıl yapılır, [Tasarımcı modellerini yeniden eğitme](how-to-retrain-designer.md) makalesi için bir önkoşuldur. Bu makalede, işlem hattı parametreleriyle birden çok modeli eğitmek için dönüştürülmüş veri kümelerini nasıl kullanacağınızı öğreneceksiniz.

## <a name="transform-a-dataset"></a>Veri kümesini dönüştürme

Bu bölümde, örnek veri kümesini içeri aktarmayı ve verileri ABD ve ABD dışı veri kümelerine nasıl böceğinizi öğreneceksiniz. Kendi verilerinizi tasarımcıya aktarma hakkında daha fazla bilgi için bkz. [verileri içeri aktarma](how-to-designer-import-data.md).

### <a name="import-data"></a>Veri içeri aktarma

Örnek veri kümesini içeri aktarmak için aşağıdaki adımları kullanın.

1. <a href="https://ml.azure.com?tabs=jre" target="_blank">Ml.Azure.com</a>'de oturum açın ve birlikte çalışmak istediğiniz çalışma alanını seçin.

1. Tasarımcıya gidin. Yeni bir işlem hattı oluşturmak için kullanımı **kolay-prebuild modüllerini** seçin.

1. İşlem hattını çalıştırmak için varsayılan bir işlem hedefi seçin.

1. Ardışık düzen tuvalinin sol tarafında bir veri kümesi ve modül paleti bulunur. **Veri kümelerini**seçin. Ardından **örnekler** bölümünü görüntüleyin.

1. **Yetişkin Census gelir ikili sınıflandırma** veri kümesini tuvale sürükleyip bırakın.

1. **Yetişkinlere yönelik gelir** veri kümesi modülünü seçin.

1. Tuvalin sağında görüntülenen Ayrıntılar bölmesinde, **çıktılar**' i seçin.

1. Görselleştir simgesini seçin ![Görselleştir simgesi](media/how-to-designer-transform-data/visualize-icon.png).

1. Veri kümesini araştırmak için veri önizleme penceresini kullanın. "Yerel-ülke" sütun değerlerini göz önünde edin.

### <a name="split-the-data"></a>Verileri bölme

Bu bölümde, "yerel-ülke" sütununda "Birleşik Devletler" içeren satırları tanımlamak ve ayırmak için [bölünmüş veri modülünü](algorithm-module-reference/split-data.md) kullanırsınız. 

1. Tuvalin solundaki modül paletinde, **veri dönüştürme** bölümünü genişletin ve **bölünmüş veri** modülünü bulun.

1. **Verileri Böl** modülünü tuvale sürükleyin ve modülün veri kümesi modülünün altına bırakın.

1. DataSet modülünü **bölünmüş veri** modülüne bağlayın.

1. **Bölünmüş veri** modülünü seçin.

1. Tuvalin sağ tarafındaki modül ayrıntıları bölmesinde **bölme modunu** **normal ifade**olarak ayarlayın.

1. **Normal ifadeyi**girin: `\"native-country" United-States` .

    **Normal ifade** modu bir değer için tek bir sütunu sınar. Bölünmüş veri modülü hakkında daha fazla bilgi için, ilgili [algoritma modülü başvurusu sayfasına](algorithm-module-reference/split-data.md)bakın.

İşlem hatlarınız şöyle görünmelidir:

:::image type="content" source="./media/how-to-designer-transform-data/split-data.png"alt-text="İşlem hattının ve bölünmüş veri modülünün nasıl yapılandırılacağını gösteren ekran görüntüsü":::


## <a name="save-the-datasets"></a>Veri kümelerini kaydetme

İşlem hattınızda verileri ayırmak üzere ayarlandığına göre, veri kümelerinin nerede kalıcı olduğunu belirtmeniz gerekir. Bu örnekte, veri kümenizi bir veri deposuna kaydetmek için **verileri dışarı aktar** modülünü kullanın. Veri depoları hakkında daha fazla bilgi için bkz. [Azure Storage Services 'A bağlanma](how-to-access-data.md)

1. Tuvalin solundaki modül paletinde, **veri girişi ve çıkış** bölümünü genişletin ve **verileri dışarı aktarma** modülünü bulun.

1. **Bölünmüş veri** modülünün altına Iki **dışarı aktarma veri** modülünü sürükleyip bırakın.

1. **Bölünen veri** modülünün her çıkış bağlantı noktasını farklı bir **dışarı aktarma veri** modülüne bağlayın.

    Ardışık düzen şuna benzer görünmelidir:

    ![Dışarı aktarma veri modüllerinin nasıl bağlanacağını gösteren ekran görüntüsü](media/how-to-designer-transform-data/export-data-pipeline.png).

1. **Bölünmüş veri** modülünün en *sol*bağlantı noktasına bağlı olan **verileri dışarı aktar** modülünü seçin.

    **Bölünmüş veri** modülüne göre çıkış bağlantı noktalarının sırası. İlk çıkış bağlantı noktası, normal ifadenin doğru olduğu satırları içerir. Bu durumda, ilk bağlantı noktası ABD tabanlı gelir için satırlar içerir ve ikinci bağlantı noktası ABD tabanlı olmayan gelir için satırlar içerir.

1. Tuvalin sağ tarafındaki modül ayrıntıları bölmesinde, aşağıdaki seçenekleri ayarlayın:
    
    **Veri deposu türü**: Azure Blob depolama

    **Veri deposu**: mevcut bir veri deposunu seçin veya Şimdi bir tane oluşturmak Için "yeni veri deposu" seçeneğini belirleyin

    **Yol**:`/data/us-income`

    **Dosya biçimi**: CSV

    > [!NOTE]
    > Bu makalede, geçerli Azure Machine Learning çalışma alanına kayıtlı bir veri deposuna erişiminizin olduğu varsayılır. Bir veri deposu ayarlama hakkında yönergeler için bkz. [Azure Storage Services 'A bağlanma](how-to-access-data.md#azure-machine-learning-studio).

    Bir veri deposu yoksa, şimdi bir tane oluşturabilirsiniz. Örneğin, bu makalede veri kümeleri, çalışma alanıyla ilişkili varsayılan BLOB depolama hesabına kaydedilir. Veri kümelerini, `azureml` adlı yeni bir klasörde kapsayıcıya kaydeder `data` .

1.  **Bölünmüş veri** modülünün en *sağ*bağlantı noktasına bağlı olan **verileri dışarı aktar** modülünü seçin.

1. Tuvalin sağ tarafındaki modül ayrıntıları bölmesinde, aşağıdaki seçenekleri ayarlayın:
    
    **Veri deposu türü**: Azure Blob depolama

    **Veri deposu**: Yukarıdaki ile aynı veri deposunu seçin

    **Yol**:`/data/non-us-income`

    **Dosya biçimi**: CSV

1. **Bölünmüş verilerin** sol bağlantı noktasına bağlı **dışarı aktarma veri** modülünün **yolu** olduğunu doğrulayın `/data/us-income` .

1. Doğru bağlantı noktasına bağlı **dışarı aktarma verileri** modülünün **yolu** olduğunu doğrulayın `/data/non-us-income` .

    Ardışık düzen ve ayarlarınız şöyle görünmelidir:
    
    ![Dışarı aktarma veri modüllerinin nasıl yapılandırılacağını gösteren ekran görüntüsü](media/how-to-designer-transform-data/us-income-export-data.png).

### <a name="submit-the-run"></a>Çalıştırmayı gönder

İşlem hattınızda verileri bölüye ve dışarı aktarmaya yönelik bir işlem hattı çalıştırması gönderilir.

1. Tuvalin üst kısmında **Gönder**' i seçin.

1. İşlem **hattı çalıştırmasını ayarla** iletişim kutusunda **Yeni oluştur** ' u seçerek bir deneme oluşturun.

    Denemeleri ilişkili işlem hattı çalıştırmalarını birlikte mantıksal olarak gruplayın. Bu işlem hattını gelecekte çalıştırırsanız, günlüğe kaydetme ve izleme amaçlarıyla aynı denemeyi kullanmanız gerekir.

1. "Split-Census-Data" gibi açıklayıcı bir deneme adı sağlayın.

1. **Gönder**’i seçin.

## <a name="view-results"></a>Sonuçları görüntüleme

İşlem hattı çalışmayı bitirdikten sonra, Azure portal BLOB depolama alanına giderek sonuçlarınızı görüntüleyebilirsiniz. Ayrıca, verilerinizin doğru şekilde bölündüğü onaylanacak şekilde **bölünmüş veri** modülünün ara sonuçlarını görüntüleyebilirsiniz.

1. **Bölünmüş veri** modülünü seçin.

1. Tuvalin sağ tarafındaki modül ayrıntıları bölmesinde, **çıktılar + Günlükler**' i seçin. 

1. Sonuçlar DataSet1 ' ın yanındaki Görselleştir simgesini görselleştirin ![ simgesini seçin ](media/how-to-designer-transform-data/visualize-icon.png) . **Results dataset1** 

1. "Yerel-ülke" sütununun yalnızca "Amerika Birleşik Devletleri" değerini içerdiğini doğrulayın.

1. Sonuçlar DataSet2 ' ın yanındaki Görselleştir simgesini görselleştirin ![ simgesini seçin ](media/how-to-designer-transform-data/visualize-icon.png) . **Results dataset2** 

1. "Yerel-ülke" sütununun "Amerika Birleşik Devletleri" değerini içermediğini doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu bölümün 2. bölümünde devam etmek istiyorsanız, [modelleri Azure Machine Learning tasarımcı Ile yeniden eğitme](how-to-retrain-designer.md).

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir veri kümesini dönüştürmeyi ve kayıtlı bir veri deposuna kaydetmeyi öğrendiniz.

Makine öğrenimi modellerini eğitemak üzere dönüştürülmüş veri kümelerini ve işlem hattı parametrelerinizi kullanmak için, bu nasıl yapılır serisinin bir sonraki bölümüne [Azure Machine Learning tasarımcı Ile yeniden eğitme](how-to-retrain-designer.md) ile devam edin.
