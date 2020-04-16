---
title: LastModifiedDate'e göre yeni ve değiştirilmiş dosyaları kopyalama
description: Azure Veri Fabrikası ile LastModifiedDate'e kadar yeni ve değiştirilen dosyaları kopyalamak için bir çözüm şablonunu nasıl kullanacağınızı öğrenin.
services: data-factory
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: shwang
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 3/8/2019
ms.openlocfilehash: 979977b2dd2eb5742d4e488623c79cb91427f055
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414810"
---
# <a name="copy-new-and-changed-files-by-lastmodifieddate-with-azure-data-factory"></a>Azure Veri Fabrikası ile LastModifiedDate'e göre yeni ve değiştirilmiş dosyaları kopyalama
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, yeni ve değiştirilen dosyaları yalnızca LastModifiedDate tarafından dosya tabanlı bir mağazadan bir hedef mağazaya kopyalamak için kullanabileceğiniz bir çözüm şablonu açıklanmaktadır. 

## <a name="about-this-solution-template"></a>Bu çözüm şablonu hakkında

Bu şablon ilk olarak yeni ve değiştirilen dosyaları yalnızca özniteliklerine göre seçer **LastModifiedDate**, ve sonra bu seçili dosyaları veri kaynağı deposundan veri hedef deposuna kopyalar.

Şablon bir etkinlik içerir:
- Yalnızca LastModifiedDate tarafından yeni ve değiştirilen dosyaları bir dosya deposundan hedef deposuna kopyalamak için **kopyalayın.**

Şablon altı parametre tanımlar:
-  *FolderPath_Source,* kaynak depodaki dosyaları okuyabileceğiniz klasör yoludur. Varsayılan değeri kendi klasör yolunuzla değiştirmeniz gerekir.
-  *Directory_Source,* kaynak depodaki dosyaları okuyabileceğiniz alt klasör yoludur. Varsayılan değeri kendi alt klasör yolunuzla değiştirmeniz gerekir.
-  *FolderPath_Destination,* dosyaları hedef mağazaya kopyalamak istediğiniz klasör yoludur. Varsayılan değeri kendi klasör yolunuzla değiştirmeniz gerekir.
-  *Directory_Destination,* dosyaları hedef depoya kopyalamak istediğiniz alt klasör yoludur. Varsayılan değeri kendi alt klasör yolunuzla değiştirmeniz gerekir.
-  *LastModified_From,* LastModifiedDate özniteliği bu datetime değerine sonra veya eşit olan dosyaları seçmek için kullanılır.  Yalnızca son kez kopyalanmamış olan yeni dosyaları seçmek için, bu tarih saati değeri ardışık düzenin en son tetiklendiği zaman olabilir. UTC saat diliminde beklenen LastModiDate'e '2019-02-01T00:00:00Z' varsayılan değerini değiştirebilirsiniz. 
-  *LastModified_To,* LastModifiedDate özniteliği bu tarih değerinden önce olan dosyaları seçmek için kullanılır. Yalnızca son kez kopyalanmamış olan yeni dosyaları seçmek için bu tarih saati değeri geçerli saat olabilir.  UTC saat diliminde beklenen LastModiDate'e '2019-02-01T00:00:00Z' varsayılan değerini değiştirebilirsiniz. 

## <a name="how-to-use-this-solution-template"></a>Bu çözüm şablonu nasıl kullanılır?

1. Şablona git **Yalnızca LastModifiedDate'e göre yeni dosyaları kopyala.** Kaynak depolama mağazanıza **yeni** bir bağlantı oluşturun. Kaynak depolama deposu, dosyaları kopyalamak istediğiniz yerdir.

    ![Kaynağa yeni bir bağlantı oluşturma](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate1.png)
    
2. Hedef mağazanıza **yeni** bir bağlantı oluşturun. Hedef mağaza, dosyaları kopyalamak istediğiniz yerdir. 

    ![Hedefe yeni bir bağlantı oluşturma](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate3.png)

3. **Bu şablonu kullan'ı**seçin.

    ![Bu şablonu kullan](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate4.png)
    
4. Aşağıdaki örnekte gösterildiği gibi, panelde kullanılabilir ardışık hattı görürsünüz:

    ![Boru hattını göster](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate5.png)

5. **Hata Ayıklama'yı**seçin, **Parametreler** için değer yazın ve **Finish'i**seçin.  Aşağıdaki resimde parametreleri aşağıdaki gibi ayarlıyoruz.
   - **FolderPath_Source** = kaynak klasörü
   - **Directory_Source** = alt klasör
   - **FolderPath_Destination** = hedef klasörü
   - **Directory_Destination** = alt klasör
   - **LastModified_From** = 2019-02-01T00:00:00Z
   - **LastModified_To** = 2019-03-01T00:00:00Z
    
    Örnek, son kez değiştirilen dosyaların **(2019-02-01T00:00:00Z** - **2019-03-01T00:00:00Z)** kaynak yol **kaynak klasöründen/alt klasörüne** kopyalanacağını gösteriyor. **destinationfolder/subfolder**  Bunları kendi parametrelerinizle değiştirebilirsiniz.

    ![İşlem hattını çalıştırma](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate6.png)

6. Sonucu gözden geçirin. Yalnızca yapılandırılan zaman alanı içinde değiştirilen dosyaların hedef mağazaya kopyalandığını görürsünüz.

    ![Sonucu gözden geçirin](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate7.png)
    
7. Artık bu ardışık ardışık ardışık ardışık ardışık ardışık ardışık ardışık her zaman yeni ve değiştirilen dosyaları yalnızca LastModifiedDate tarafından periyodik olarak kopyalayabilmesi için, bu ardışık pencere tetikleyicisini atabilirsiniz.  **Tetikleyici Ekle'yi**seçin ve **Yeni/Edit'i**seçin.

    ![Sonucu gözden geçirin](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate8.png)
    
8. **Tetikleyiciekle** penceresinde **+ Yeni'** yi seçin.

9. Tetik türü için **Yuvarlanma Penceresi'ni** seçin, **her 15 dakikada bir** yineleme olarak ayarlayın (istediğiniz zamana değiştirebilirsiniz). Etkinleştirilmiş kutu için **Evet'i** seçin ve ardından **Tamam'ı**seçin.

    ![Tetikleyici oluşturma](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate10.png)    
    
10. **Tetikleyici Çalıştır Parametreleri'nin** değerini aşağıdaki gibi ayarlayın ve **Finish'i**seçin.
    - **FolderPath_Source** = **kaynak klasörü**.  Kaynak veri deposunda klasörünüzle değiştirebilirsiniz.
    - **Directory_Source** = **alt klasör.**  Kaynak veri deposunda alt klasörünüzle değiştirebilirsiniz.
    - **FolderPath_Destination** = **hedef klasörü**.  Hedef veri deposunda klasörünüzle değiştirebilirsiniz.
    - **Directory_Destination** = **alt klasör.**  Hedef veri deposunda alt klasörünüzle değiştirebilirsiniz.
    - **LastModified_From** =  **LastModified_From\@trigger().outputs.windowStartTime**.  Bu, ardışık boru hattının en son tetiklediği zamanı belirleyen tetikleyiciden gelen bir sistem değişkenidir.
    - **LastModified_To** = **LastModified_To\@tetikleyici().outputs.windowEndTime**.  Bu, bu kez boru hattının tetiklenme zamanını belirleyen tetikleyiciden gelen bir sistem değişkenidir.
    
    ![Giriş parametreleri](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate11.png)
    
11. **Tümünü Yayımla**.
    
    ![Tümlerini Yayınla](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate12.png)

12. Veri kaynağı deposunun kaynak klasöründe yeni dosyalar oluşturun.  Artık ardışık hattın otomatik olarak tetiklenmesini bekliyorsunuz ve yalnızca yeni dosyalar hedef mağazaya kopyalanacak.

13. Sol gezinti panelinde **Monitör** sekmesini seçin ve tetikleyicinin tekrarı her 15 dakikada bir ayarlanmışsa yaklaşık 15 dakika bekleyin. 

14. Sonucu gözden geçirin. Ardışık alanınızın her 15 dakikada bir otomatik olarak tetikleneceğini ve yalnızca kaynak deposundan yeni veya değiştirilen dosyaların her ardışık ardışık çalışmadaki hedef mağazaya kopyalanacağını görürsünüz.

    ![Sonucu gözden geçirin](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate15.png)
    
## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory'ye giriş](introduction.md)
