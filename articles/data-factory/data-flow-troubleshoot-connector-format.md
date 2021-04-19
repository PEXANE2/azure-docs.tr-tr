---
title: Veri akışlarının eşlenmesinde bağlayıcı ve biçimlendirme sorunlarını giderme
description: Azure Data Factory bağlayıcı ve biçimle ilgili veri akışı sorunlarını nasıl giderebileceğinizi öğrenin.
author: linda33wj
ms.author: jingwang
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 04/12/2021
ms.openlocfilehash: 7596bf3182f59758eeee66e248404e98c18da2e7
ms.sourcegitcommit: 3ed0f0b1b66a741399dc59df2285546c66d1df38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2021
ms.locfileid: "107715547"
---
# <a name="troubleshoot-connector-and-format-issues-in-mapping-data-flows-in-azure-data-factory"></a>Azure Data Factory veri akışlarını eşlerken bağlayıcı ve biçimlendirme sorunlarını giderme


Bu makalede, Azure Data Factory (ADF) ' de veri akışlarını eşlemek için bağlayıcı ve biçimle ilgili sorun giderme yöntemleri incelenmektedir.


## <a name="cosmos-db--json"></a>JSON & Cosmos DB

### <a name="support-customized-schemas-in-the-source"></a>Kaynakta özelleştirilmiş şemaları destekleme

#### <a name="symptoms"></a>Belirtiler
Cosmos DB/JSON 'dan diğer veri depolarına veri taşımak veya aktarmak için ADF veri akışını kullanmak istediğinizde, kaynak verilerinin bazı sütunları eksik olabilir. 

#### <a name="cause"></a>Nedeni 
Şema boş bağlayıcılar (her satırın sütun numarası, sütun adı ve sütun veri türü diğerleriyle karşılaştırılırken farklı olabilir) için, varsayılan olarak ADF, şemayı çıkarması için örnek satırları (örneğin, Top 100 veya 1000 satır verisi) kullanır ve çıkarılan sonuç verileri okumak için bir şema olarak kullanılır. Bu nedenle, veri depolarınızın örnek satırlarda görünmeyen ek sütunları varsa, bu ek sütunların verileri okunamaz, taşınamaz veya havuz veri depolarına aktarılmaz.

#### <a name="recommendation"></a>Öneri
Varsayılan davranışın üzerine yazmak ve ek alanlara getirmek için ADF, kaynak şemayı özelleştirmeniz için seçenekler sağlar. Veri akışı kaynak projeksiyonundaki verileri okumak için şema-çıkarsanda eksik olan ek/eksik sütunları belirtebilirsiniz ve özelleştirilmiş şemayı ayarlamak için aşağıdaki seçeneklerden birini uygulayabilirsiniz. Genellikle, **-1 seçeneği** daha tercih edilir.

- **Seçenek-1**: karmaşık şemalarda milyonlarca satır içeren bir büyük dosya, tablo veya kapsayıcı olabilecek özgün kaynak verileriyle karşılaştırıldığında, okumak istediğiniz tüm sütunları içeren birkaç satırla geçici bir tablo/kapsayıcı oluşturabilir ve sonra aşağıdaki işleme geçebilirsiniz: 

    1. Tüm şemayı almak için örnek dosya/tablolarla **Içeri aktarma projeksiyonunu** sağlamak üzere veri akışı kaynak **hata ayıklama ayarlarını** kullanın. Aşağıdaki resimdeki adımları izleyebilirsiniz:<br/>

        ![Önerinin 1-1 seçeneğini gösteren ekran görüntüsü](./media/data-flow-troubleshoot-connector-format/customize-schema-option-1-1.png)<br/>
         1. Veri akışı tuvalindeki **hata ayıklama ayarları** ' nı seçin.
         1. Açılır bölmede, **Cosmossource** sekmesinin altındaki **örnek tablo** ' yı seçin. 
         1. **Tablo bloğunda tablonuzun** adını girin.
         1. Ayarlarınızı kaydetmek için **Kaydet** ' i seçin.
         1. **Içeri aktarma projeksiyonu** seçin.<br/>  
    
    1. **Hata ayıklama ayarlarını** geri kalan veri taşıma/dönüştürme için kaynak veri kümesini kullanacak şekilde değiştirin. Aşağıdaki resimdeki adımlarla geçiş yapabilirsiniz:<br/>

        ![Önerinin 1-2 seçeneğini gösteren ekran görüntüsü](./media/data-flow-troubleshoot-connector-format/customize-schema-option-1-2.png) <br/>   
         1. Veri akışı tuvalindeki **hata ayıklama ayarları** ' nı seçin.
         1. Açılır bölmede, **cosmossource** sekmesinin altındaki **kaynak veri kümesi** ' ni seçin.
         1. Ayarlarınızı kaydetmek için **Kaydet** ' i seçin.<br/>
    
    Daha sonra, ADF veri akışı çalışma zamanı, özgün veri deposundan verileri okumak için özelleştirilmiş şemayı kabul eder ve kullanır. <br/>

- **Seçenek-2**: kaynak verilerin ŞEMASı ve DSL diline alışkın değilseniz, verileri okumak üzere ek/eksik sütunlar eklemek için veri akışı kaynak betiğini el ile güncelleştirebilirsiniz. Aşağıdaki resimde bir örnek gösterilmektedir: 

    ![Önerinin 2 seçeneğini gösteren ekran görüntüsü](./media/data-flow-troubleshoot-connector-format/customize-schema-option-2.png)

## <a name="next-steps"></a>Sonraki adımlar
Sorun giderme konusunda daha fazla yardım için şu kaynaklara bakın:

*  [Azure Data Factory veri akışlarını eşleme sorunlarını giderme](data-flow-troubleshoot-guide.md)
*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Data Factory için Stack Overflow Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)