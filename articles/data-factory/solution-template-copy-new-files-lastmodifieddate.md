---
title: Yeni ve değiştirilmiş dosyaları LastModifiedDate göre Kopyala
description: Azure Data Factory ile yeni ve değiştirilmiş dosyaları LastModifiedDate göre kopyalamak için bir çözüm şablonu kullanmayı öğrenin.
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
ms.openlocfilehash: 971d311dfb54d417a8f66f504d01f08f8bcfc48b
ms.sourcegitcommit: b5ff5abd7a82eaf3a1df883c4247e11cdfe38c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74942090"
---
# <a name="copy-new-and-changed-files-by-lastmodifieddate-with-azure-data-factory"></a>Azure Data Factory ile yeni ve değiştirilmiş dosyaları LastModifiedDate göre Kopyala

Bu makalede, yeni ve değiştirilmiş dosyaları yalnızca dosya tabanlı bir mağazadan hedef depoya LastModifiedDate ile kopyalamak için kullanabileceğiniz bir çözüm şablonu açıklanmaktadır. 

## <a name="about-this-solution-template"></a>Bu çözüm şablonu hakkında

Bu şablon ilk olarak yeni ve değiştirilmiş dosyaları yalnızca özniteliklerini **LastModifiedDate**seçer ve ardından bu seçili dosyaları veri kaynağı deposundan veri hedef deposuna kopyalar.

Şablon bir etkinlik içerir:
- Yeni ve değiştirilmiş dosyaları yalnızca bir dosya deposundan hedef depoya LastModifiedDate göre kopyalamak için **Kopyala** .

Şablon altı parametreyi tanımlar:
-  *FolderPath_Source* , kaynak depodan dosyaları okuyabileceğiniz klasör yoludur. Varsayılan değeri kendi klasör yolınızla değiştirmeniz gerekir.
-  *Directory_Source* , kaynak depodan dosyaları okuyabileceğiniz alt klasör yoludur. Varsayılan değeri kendi alt klasör yolınızla değiştirmeniz gerekir.
-  *FolderPath_Destination* , dosyaları hedef depoya kopyalamak istediğiniz klasör yoludur. Varsayılan değeri kendi klasör yolınızla değiştirmeniz gerekir.
-  *Directory_Destination* , dosyaları hedef depoya kopyalamak istediğiniz alt klasör yoludur. Varsayılan değeri kendi alt klasör yolınızla değiştirmeniz gerekir.
-  *LastModified_From* , LastModifiedDate özniteliği bu tarih saat değerine eşit veya daha büyük olan dosyaları seçmek için kullanılır.  Son kez kopyalanamayan yeni dosyaları seçmek için bu tarih saat değeri, işlem hattının son kez tetiklendiği zaman olabilir. ' 2019-02-01T00:00:00Z ' varsayılan değerini UTC saat dilimindeki beklenen LastModifiedDate yerine değiştirebilirsiniz. 
-  *LastModified_To* , LastModifiedDate özniteliği bu tarih saat değerinden önce olan dosyaları seçmek için kullanılır. Son kez kopyalanamayan yeni dosyaları seçmek için bu tarih saat değeri şimdiki saat olabilir.  ' 2019-02-01T00:00:00Z ' varsayılan değerini UTC saat dilimindeki beklenen LastModifiedDate yerine değiştirebilirsiniz. 

## <a name="how-to-use-this-solution-template"></a>Bu çözüm şablonunu kullanma

1. Şablona git **yalnızca LastModifiedDate ile yeni dosyaları Kopyala**. Kaynak depolama deponuza **Yeni** bir bağlantı oluşturun. Kaynak depolama deposu, dosyaları içinden kopyalamak istediğiniz yerdir.

    ![Kaynağa yeni bir bağlantı oluşturun](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate1.png)
    
2. Hedef deponuza **Yeni** bir bağlantı oluşturun. Hedef Depo, dosyaları kopyalamak istediğiniz yerdir. 

    ![Hedefe yeni bir bağlantı oluşturun](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate3.png)

3. **Bu şablonu kullan**'ı seçin.

    ![Bu şablonu kullan](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate4.png)
    
4. Aşağıdaki örnekte gösterildiği gibi panelde kullanılabilir işlem hattı görüntülenir:

    ![İşlem hattını gösterme](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate5.png)

5. **Hata Ayıkla**' yı seçin, **parametrelerin** değerini yazın ve **son**' u seçin.  Aşağıdaki resimde parametreleri aşağıdaki şekilde ayarlayacağız.
   - **FolderPath_Source** = SourceFolder
   - **Directory_Source** = alt klasörü
   - **FolderPath_Destination** = DestinationFolder
   - **Directory_Destination** = alt klasörü
   - **LastModified_From** = 2019-02-01T00:00:00Z
   - **LastModified_To** = 2019-03-01T00:00:00Z
    
    Örnek, zaman aralığı içinde en son değiştirilen dosyaların (**2019-02-01T00:00:00Z** - **2019-03-01T00:00:00Z**) kaynak yolu **SourceFolder/alt** klasörünün hedef yolu **DestinationFolder/alt**klasörüne kopyalanacağını gösterir.  Bunları kendi parametreleriniz ile değiştirebilirsiniz.

    ![İşlem hattını çalıştırma](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate6.png)

6. Sonucu gözden geçirin. Yalnızca yapılandırılan TimeSpan içinde son değiştirilen dosyaların hedef depoya kopyalandığını görürsünüz.

    ![Sonucu gözden geçirin](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate7.png)
    
7. Artık, bu işlem hattını otomatikleştirmek için ardışık düzen bir Windows tetikleyicisi ekleyebilirsiniz, böylece işlem hattı her zaman yeni ve değiştirilmiş dosyaları yalnızca LastModifiedDate göre düzenli olarak kopyalayabilir.  **Tetikleyici Ekle**' yi seçin ve **Yeni/Düzenle**' yi seçin.

    ![Sonucu gözden geçirin](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate8.png)
    
8. **Tetikleyici Ekle** penceresinde **+ Yeni**' yi seçin.

9. Tetikleyici türü için **göz atın penceresini** seçin, **her 15 dakikada** bir yinelenme olarak ayarlayın (herhangi bir Aralık zamanına dönüştürebilirsiniz). Etkin kutu için **Evet** ' i seçin ve ardından **Tamam**' ı seçin.

    ![Tetikleyici oluşturma](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate10.png)    
    
10. **Tetikleyici çalıştırma parametrelerinin** değerini aşağıda gösterildiği gibi ayarlayın ve **son**' u seçin.
    -  = **SourceFolder** **FolderPath_Source** .  Kaynak veri deposundaki klasörünüzle değiştirebilirsiniz.
    - **Directory_Source** = **alt klasörü**.  Kaynak veri deposundaki alt klasörle değiştirebilirsiniz.
    - **FolderPath_Destination** = **DestinationFolder**.  Hedef veri deposundaki klasörünüzle değiştirebilirsiniz.
    - **Directory_Destination** = **alt klasörü**.  Hedef veri deposundaki alt klasörle değiştirebilirsiniz.
    - **LastModified_From** =   **\@tetikleyicisi (). çıktılar. windowstarttime**.  Bu, işlem hattının son kez tetiklendiği saati belirleyen tetikleyiciden bir sistem değişkenidir.
    - **LastModified_To** =  **\@tetikleyicisi (). çıktılar. windowbitişsaati**.  Bu süre, işlem hattının bu kez tetiklendiği saati belirleyen tetikleyiciden bir sistem değişkenidir.
    
    ![Giriş parametreleri](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate11.png)
    
11. **Tümünü Yayımla**.
    
    ![Tümünü Yayımla](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate12.png)

12. Veri kaynağı depolama alanının kaynak klasörünüzde yeni dosyalar oluşturun.  Artık işlem hattının otomatik olarak tetiklenmesi bekleniyor ve hedef depoya yalnızca yeni dosyalar kopyalanacaktır.

13. Sol gezinti panelinde **izleyici** sekmesini seçin ve tetikleyici yinelemesi her 15 dakikada bir olarak ayarlandıysa 15 dakika bekleyin. 

14. Sonucu gözden geçirin. İşlem hattınızı 15 dakikada bir otomatik olarak tetikleneceğini ve kaynak depodan yalnızca yeni veya değiştirilmiş dosyaları her işlem hattı çalıştırmasında hedef depoya kopyalacaksınız.

    ![Sonucu gözden geçirin](media/solution-template-copy-new-files-lastmodifieddate/copy-new-files-lastmodifieddate15.png)
    
## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory'ye giriş](introduction.md)
