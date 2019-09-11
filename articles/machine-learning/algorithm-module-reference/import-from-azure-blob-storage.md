---
title: 'Azure Blob depolamadan içeri aktarma: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Bu konu başlığı altında, bir Machine Learning denemesinin içindeki verileri kullanabilmeniz için Azure Machine Learning hizmetindeki Azure Blob depolama modülünden Içeri aktarma Işleminin nasıl kullanılacağı açıklanmaktadır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: fea64070c496379351bb75f2a38aba9b4db70dcd
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128714"
---
# <a name="import-from-azure-blob-storage-module"></a>Azure Blob depolama modülünden içeri aktarma

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

Bu modülü, verileri bir Machine Learning denemesinde kullanabilmeniz için Azure Blob depolama alanındaki verileri okumak üzere kullanın.  

Azure Blob hizmeti, ikili veriler de dahil olmak üzere büyük miktarda veriyi depolamaya yöneliktir. Azure Blob 'larına, HTTP veya HTTPS kullanılarak her yerden erişilebilir. BLOB depolama türüne bağlı olarak kimlik doğrulaması gerekebilir. 

- Genel bloblara herkes veya SAS URL 'SI olan kullanıcılar erişebilir.
- Özel Bloblar, oturum açma ve kimlik bilgileri gerektirir.

Blob depolamadan içeri aktarma, verilerin **Blok Blobu** biçimini kullanan bloblarda depolanmasını gerektirir. Blob 'da depolanan dosyaların virgülle ayrılmış (CSV) veya sekmeyle ayrılmış (TSV) biçimleri kullanması gerekir. Dosyayı okurken, kayıtlar ve ilgili öznitelik başlıkları bir veri kümesi olarak belleğe satır olarak yüklenir.


Şemanın beklenen şekilde olduğundan emin olmak için verilerinizi içeri aktarmadan önce profilinizi kesinlikle öneririz. İçeri aktarma işlemi, şemayı belirleyebilmek için bazı baş satırları tarar, ancak sonraki satırlarda ek sütunlar veya hatalara neden olan veriler bulunabilir.



## <a name="manually-set-properties-in-the-import-data-module"></a>Verileri Içeri aktarma modülündeki özellikleri el ile ayarlama

Aşağıdaki adımlarda, içeri aktarma kaynağının el ile nasıl yapılandırılacağı açıklanır.

1. **Veri alma** modülünü denemenize ekleyin. Bu modülü arabirimde, **veri girişinde ve çıktıda** bulabilirsiniz

2. **Veri kaynağı**Için **Azure Blob depolama**' yı seçin.

3. **Kimlik doğrulama türü**için, bilgilerin ortak bir veri kaynağı olarak sağlandığını biliyorsanız **ortak (SAS URL)** öğesini seçin. Bir SAS URL 'SI, Azure Storage yardımcı programını kullanarak oluşturabileceğiniz genel erişim için zamana bağlanan bir URL 'dir.

    Aksi takdirde **Hesap**' ı seçin.

4. Verileriniz bir SAS URL 'SI kullanılarak erişilebilen bir **genel** Blobun Ise, URL dizesinde indirme ve kimlik doğrulama için gereken tüm bilgiler bulunduğundan ek kimlik bilgilerine ihtiyacınız yoktur.

    **URI** alanında, hesabı ve genel blobu TANıMLAYAN tam URI 'yi yazın veya yapıştırın.



5. Verileriniz **özel** bir hesabalıyorsa, hesap adı ve anahtar dahil olmak üzere kimlik bilgilerini sağlamanız gerekir.

    - **Hesap adı**için, erişmek istediğiniz blobu içeren hesabın adını yazın veya yapıştırın.

        Örneğin, depolama hesabının tam URL 'si ise `http://myshared.blob.core.windows.net`, yazmanız `myshared`gerekir.

    - **Hesap anahtarı**için, hesapla ilişkili depolama erişim anahtarını yapıştırın.

        Erişim anahtarını bilmiyorsanız, bu makaledeki "Azure depolama hesaplarınızı yönetme" bölümüne bakın: [Azure depolama hesapları hakkında](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

6. **Kapsayıcı, dizin veya blob yolu**için almak istediğiniz belirli bir Blobun adını yazın.

    Örneğin, **data01. csv** adlı bir dosyayı **mymldata**adlı bir hesapta yer alarak bulunan kapsayıcıya yüklediğinizde, dosyanın tam URL 'si:. `http://mymldata.blob.core.windows.net/trainingdata/data01.txt`

    Bu nedenle, **kapsayıcı, dizin veya Blobun alan yolunda**şunu yazarsınız:`trainingdata/data01.csv`

    Birden çok dosyayı içeri aktarmak için joker karakterleri `*` (yıldız işareti) veya `?` (soru işareti) kullanabilirsiniz.

    Örneğin, kapsayıcının `trainingdata` uyumlu biçimdeki birden çok dosya içerdiği varsayılarak, ile `data`başlayan tüm dosyaları okumak ve bunları tek bir veri kümesine birleştirmek için aşağıdaki belirtimi kullanabilirsiniz:

    `trainingdata/data*.csv`

    Kapsayıcı adlarında joker karakterler kullanamazsınız. Birden çok kapsayıcıdan dosyaları içeri aktarmanız gerekiyorsa, her kapsayıcı için **veri Içeri aktarma** modülünün ayrı bir örneğini kullanın ve ardından [Satırları Ekle](./add-rows.md) modülünü kullanarak veri kümelerini birleştirin.

    > [!NOTE]
    > Bu seçeneği belirlediyseniz, **önbelleğe alınmış sonuçları kullanın**, kapsayıcıdaki dosyalarda yaptığınız tüm değişiklikler, denemesinde verilerin yenilenmesini tetiklemez.

7. **BLOB dosya biçimi**için blob 'da depolanan verilerin biçimini belirten bir seçenek belirleyin, böylece Azure Machine Learning verileri uygun şekilde işleyebilir. Aşağıdaki biçimler desteklenir:

    - **CSV**: Virgülle ayrılmış değerler (CSV), Azure Machine Learning dosyaları dışa ve içe aktarmak için varsayılan depolama biçimidir. Veriler zaten bir başlık satırı içeriyorsa, bu seçeneği seçtiğinizden, **dosyanın üst bilgi satırına sahip**olduğundan veya üstbilginin bir veri satırı olarak değerlendirildiğinden emin olun.

       

    - **TSV**: Sekmeyle ayrılmış değerler (TSV), birçok makine öğrenimi aracı tarafından kullanılan bir biçimdir. Veriler zaten bir başlık satırı içeriyorsa, bu seçeneği seçtiğinizden, **dosyanın üst bilgi satırına sahip**olduğundan veya üstbilginin bir veri satırı olarak değerlendirildiğinden emin olun.

       

    - **ARFF**: Bu biçim, dosyaları WEKA araç kümesi tarafından kullanılan biçimde içe aktarmayı destekler. 

   

8. Denemeyi çalıştırın.


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 