---
title: 'Dışarı aktarma verileri: modül başvurusu'
titleSuffix: Azure Machine Learning service
description: İşlem hatlarınızdan sonuçları, ara verileri ve çalışma verilerini Azure Machine Learning dışında bulut depolama hedeflerine kaydetmek için Azure Machine Learning hizmetinde verileri dışarı aktar modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: b7b4b9de1e91279243e35f1b71f1ef6d2244e9e0
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693727"
---
# <a name="export-data-module"></a>Veri modülünü dışarı aktarma

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

İşlem hatlarınızdan sonuçları, ara verileri ve çalışma verilerini Azure Machine Learning dışında bulut depolama hedeflerine kaydetmek için bu modülü kullanın.

Bu modül, verilerinizi aşağıdaki bulut veri hizmetlerine vermeyi veya kaydetmeyi destekler:


- **Azure Blob depolama 'Ya aktarma**: verileri Azure 'daki blob hizmetine kaydeder. Blob hizmetindeki veriler, genel olarak paylaşılabilir veya güvenli uygulama veri depolarında kaydedilebilir.

  
## <a name="how-to-configure-export-data"></a>Dışarı aktarma verilerini yapılandırma

1. **Veri aktarma** modülünü arabirimindeki işlem hattınızı ekleyin. Bu modülü **giriş ve çıkış** kategorisinde bulabilirsiniz.

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

    3. Özel hesaplar için **Hesap**' ı seçin ve işlem hattının depolama hesabına yazabilmesi için hesap adını ve hesap anahtarını sağlayın.

         - **Hesap adı**: verileri kaydetmek istediğiniz hesabın adını yazın veya yapıştırın. Örneğin, depolama hesabının tam URL 'SI `http://myshared.blob.core.windows.net` `myshared` yazmanız gerekir.

        - **Hesap anahtarı**: hesapla ilişkili depolama erişim anahtarını yapıştırın.

        -  **Kapsayıcı, dizin veya blob yolu**: verilecek verilerin depolanacağı Blobun adını yazın. Örneğin, işlem hattınızı sonuçları **mymldata**adlı bir hesaptaki kapsayıcı **tahminlerinin** **results01. csv** adlı yeni bir bloba kaydetmek için, blob 'un tam URL 'si `http://mymldata.blob.core.windows.net/predictions/results01.csv` olur.

            Bu nedenle, **kapsayıcı, dizin veya Blobun alan yolunda**kapsayıcı ve BLOB adını şu şekilde belirtirsiniz: `predictions/results01.csv`

        - Zaten mevcut olmayan bir Blobun adını belirtirseniz Azure, blobu sizin için oluşturur.

       -  Mevcut bir bloba yazarken, **Azure Blob depolama yazma modu**özelliği ayarlanarak Blobun geçerli içeriğinin üzerine yazılmasını belirtebilirsiniz. Varsayılan olarak, bu özellik **hata**olarak ayarlanır, yani aynı ada sahip mevcut bir blob dosyası bulunduğunda hata oluşur.


    4. **BLOB dosyası Için dosya biçimi**için verilerin depolanacağı biçimi seçin.

        - **CSV**: virgülle ayrılmış değerler (CSV) varsayılan depolama biçimidir. Sütun başlıklarını verilerle birlikte dışarı aktarmak için, **BLOB üst bilgisi satırını yaz**seçeneğini belirleyin.  Azure Machine Learning ' de kullanılan virgülle ayrılmış biçim hakkında daha fazla bilgi için bkz. [CSV 'ye dönüştürme](./convert-to-csv.md).

        - **TSV**: sekmeyle ayrılmış değerler (TSV) biçimi birçok makine öğrenimi araçlarıyla uyumludur. Sütun başlıklarını verilerle birlikte dışarı aktarmak için, **BLOB üst bilgisi satırını yaz**seçeneğini belirleyin.  

 
    5. **Önbelleğe alınmış sonuçları kullan**: işlem hattını her çalıştırdığınızda blob dosyasına sonuçları yeniden yazmayı önlemek istiyorsanız bu seçeneği belirleyin. Modül parametrelerinde başka bir değişiklik yoksa, işlem hattı yalnızca modülün ilk kez çalıştırıldığı sırada veya verilerde değişiklik olduğunda sonuçları yazar.

    6. İşlem hattını çalıştırma.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 