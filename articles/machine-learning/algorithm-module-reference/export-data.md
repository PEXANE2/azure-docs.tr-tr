---
title: 'Verileri dışarı aktar: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Denemeleri Azure Machine Learning dışındaki bulut depolama hedeflerine sonuçları, ara verileri ve çalışma verilerini kaydetmek için Azure Machine Learning hizmetinde verileri dışarı aktar modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: a4fb539f4c86d27813b60964794fc1f398d3f2a4
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128771"
---
# <a name="export-data-module"></a>Veri modülünü dışarı aktarma

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

Bu modülü, denemeleri ' den Azure Machine Learning dışında bulut depolama hedeflerine sonuçları, ara verileri ve çalışma verilerini kaydetmek için kullanın.

Bu modül, verilerinizi aşağıdaki bulut veri hizmetlerine vermeyi veya kaydetmeyi destekler:


- **Azure Blob depolamaya aktarma**: Verileri Azure 'daki blob hizmetine kaydeder. Blob hizmetindeki veriler, genel olarak paylaşılabilir veya güvenli uygulama veri depolarında kaydedilebilir.

  
## <a name="how-to-configure-export-data"></a>Dışarı aktarma verilerini yapılandırma

1. **Veri dışa aktarma** modülünü arabirimindeki denemenize ekleyin. Bu modülü **giriş ve çıkış** kategorisinde bulabilirsiniz.

2. Dışarı aktarma **verilerini** , dışarı aktarmak istediğiniz verileri içeren modüle bağlayın.

3. **Verileri dışarı aktar** ' a çift tıklayarak **Özellikler** bölmesini açın.

4. **Veri hedefi**için, verilerinizi kaydedeceğiniz bulut depolama türünü seçin. Bu seçenekte herhangi bir değişiklik yaparsanız, diğer tüm özellikler sıfırlanır. Bu nedenle, önce bu seçeneği seçtiğinizden emin olun!

5. Belirtilen depolama hesabına erişmek için gerekli bir hesap adı ve kimlik doğrulama yöntemi sağlayın.

    **Azure Blob depolama 'Ya aktarma** işlemi, özel önizlemede tek seçenektir. Aşağıda modülün nasıl ayarlanacağı gösterilmektedir.
    1. Azure Blob hizmeti, ikili veriler de dahil olmak üzere büyük miktarda veriyi depolamaya yöneliktir. İki tür BLOB depolama alanı vardır: genel blob 'lar ve oturum açma kimlik bilgileri gerektiren Bloblar.

    2. Bir SAS URL 'SI aracılığıyla depolamanın erişimi desteklediğini biliyorsanız **kimlik doğrulama türü**için **ortak (SAS)** seçeneğini belirleyin.

          SAS URL 'SI, Azure Storage yardımcı programı kullanılarak oluşturulabilecek ve yalnızca sınırlı bir süre için kullanılabilen özel bir URL türüdür.  Kimlik doğrulaması ve indirme için gereken tüm bilgileri içerir.

        **URI**için, hesabı ve genel blobu TANıMLAYAN tam URI 'yi yazın veya yapıştırın.

        Dosya biçimi için CSV ve TSV desteklenir.

    3. Özel hesaplar için **Hesap**' ı seçin ve hesap adı ve hesap anahtarı ' nı belirtin, böylece deneme depolama hesabına yazabilir.

         - **Hesap adı**: Verileri kaydetmek istediğiniz hesabın adını yazın veya yapıştırın. Örneğin, depolama hesabının tam URL 'si ise `http://myshared.blob.core.windows.net`, yazmanız `myshared`gerekir.

        - **Hesap anahtarı**: Hesapla ilişkili depolama erişim anahtarını yapıştırın.

        -  **Kapsayıcı, dizin veya Blobun yolu**: İçe aktarılmış verilerin depolanacağı Blobun adını yazın. Örneğin, denemenizin sonuçlarını **mymldata**adlı bir hesaptaki kapsayıcı `http://mymldata.blob.core.windows.net/predictions/results01.csv` **tahminlerinin** **results01. csv** adlı yeni bir bloba kaydetmek için, Blobun tam URL 'si olur.

            Bu nedenle, **kapsayıcı, dizin veya Blobun alan yolunda**kapsayıcı ve BLOB adını aşağıdaki gibi belirtirsiniz:`predictions/results01.csv`

        - Zaten mevcut olmayan bir Blobun adını belirtirseniz Azure, blobu sizin için oluşturur.

       -  Mevcut bir bloba yazarken, **Azure Blob depolama yazma modu**özelliği ayarlanarak Blobun geçerli içeriğinin üzerine yazılmasını belirtebilirsiniz. Varsayılan olarak, bu özellik **hata**olarak ayarlanır, yani aynı ada sahip mevcut bir blob dosyası bulunduğunda hata oluşur.


    4. **BLOB dosyası Için dosya biçimi**için verilerin depolanacağı biçimi seçin.

        - **CSV**: Virgülle ayrılmış değerler (CSV) varsayılan depolama biçimidir. Sütun başlıklarını verilerle birlikte dışarı aktarmak için, **BLOB üst bilgisi satırını yaz**seçeneğini belirleyin.  Azure Machine Learning ' de kullanılan virgülle ayrılmış biçim hakkında daha fazla bilgi için bkz. [CSV 'ye dönüştürme](./convert-to-csv.md).

        - **TSV**: Sekmeyle ayrılmış değerler (TSV) biçimi birçok makine öğrenimi araçlarıyla uyumludur. Sütun başlıklarını verilerle birlikte dışarı aktarmak için, **BLOB üst bilgisi satırını yaz**seçeneğini belirleyin.  

 
    5. **Önbelleğe alınmış sonuçları kullan**: Denemeyi her çalıştırışınızda sonuçları blob dosyasına yeniden yazmayı önlemek istiyorsanız bu seçeneği belirleyin. Modül parametrelerinde başka bir değişiklik yoksa, deneme sonuçları yalnızca modülün ilk kez çalıştırıldığı zaman veya verilerde değişiklik olduğunda yazar.

    6. Denemeyi çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 