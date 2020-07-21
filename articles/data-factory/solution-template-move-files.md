---
title: Dosya tabanlı depolama arasında dosyaları taşıma
description: Azure Data Factory kullanarak dosya tabanlı depolama arasında dosya taşımak için bir çözüm şablonu kullanmayı öğrenin.
services: data-factory
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: shwang
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 7/12/2019
ms.openlocfilehash: f6baea73c0c4964bb3937304603a2a92a13d52b2
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86522729"
---
# <a name="move-files-with-azure-data-factory"></a>Dosyaları Azure Data Factory taşıma

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

ADF kopyalama etkinliği, depolama depoları arasında ikili dosyalar kopyalanırken "taşıma" senaryosunda yerleşik desteğe sahiptir.  Bunu etkinleştirmenin yolu, kopyalama etkinliğinde "deleteFilesAfterCompletion" değerini doğru olarak ayarlamanıza olanak tanır. Bunu yaptığınızda, kopyalama etkinliği iş tamamlandıktan sonra veri kaynağı deposundan dosyaları siler. 

Bu makalede, aynı senaryoya ulaşmak için ADF esnek denetim akışını ve kopyalama etkinliği ve silme etkinliğini kullanan başka bir yaklaşım olarak çözüm şablonu açıklanmaktadır. Bu şablonu kullanmanın yaygın senaryolarından biri: dosyalar sürekli olarak kaynak deponuzda bir giriş klasörüne bırakılır. ADF işlem hattı, bir zamanlama tetikleyicisi oluşturarak bu dosyaları kaynaktan hedef depoya düzenli olarak taşıyabilir.  ADF işlem hattının "dosyaları taşıma" yolu, her birini hedef depodaki başka bir klasöre kopyalayarak ve ardından kaynak deposundaki giriş klasöründen aynı dosyaları silerek, giriş klasöründen dosyaları alıyor.

> [!NOTE]
> Bu şablonun klasörleri taşımak yerine dosyaları taşımak için tasarlandığını unutmayın.  Veri kümesini yalnızca bir klasör yolu içerecek şekilde değiştirerek klasörü taşımak ve sonra kopyalama etkinliğini ve silme etkinliğini bir klasörü temsil eden aynı veri kümesine başvuracak şekilde kullanmak istiyorsanız, çok dikkatli olmanız gerekir. Bunun nedeni, kopyalama işlemi ve silme işlemi arasında klasöre ulaşan yeni dosyalar OLMADıĞıNDAN emin olmanızı sağlar. Kopyalama etkinliğinizi kopyalama işini tamamlamış ancak silme etkinliği henüz tamamlanmadığında, klasörde klasöre ulaşan yeni dosyalar varsa, DELETE etkinliğinin tüm klasörü silerek henüz hedefe kopyalanmamış olan bu yeni gelen dosyayı silmesi mümkündür.

## <a name="about-this-solution-template"></a>Bu çözüm şablonu hakkında

Bu şablon, kaynak dosya tabanlı deponuzdan dosyaları alır. Ardından, bunların her birini hedef depoya taşımaları.

Şablon beş etkinlik içerir:
- **GetMetadata** , kaynak deposundaki klasörünüzdeki dosya ve alt klasörler dahil olmak üzere nesnelerin listesini alır. Nesneleri özyinelemeli olarak alamaz. 
- **Filtre** filtresi yalnızca dosyaları seçmek Için **GetMetadata** etkinliğinden nesneler listesine filtre uygulayın. 
- **ForEach** , **filtre** etkinliğinden dosya listesini alır ve sonra liste üzerinde dolaşır ve her dosyayı kopyalama etkinliğine ve silme etkinliğine geçirir.
- **Kopya** bir dosyayı kaynaktan hedef depoya kopyalar.
- **Delete** , kaynak depodan aynı dosyayı siler.

Şablon dört parametre tanımlar:
- *SourceStore_Location* , dosyaları taşımak istediğiniz kaynak mağazalarınızın klasör yoludur. 
- *SourceStore_Directory* , dosyaları taşımak istediğiniz kaynak deponuzın alt klasör yoludur.
- *DestinationStore_Location* , dosyaları taşımak istediğiniz hedef mağazalarınızın klasör yoludur. 
- *DestinationStore_Directory* , dosyaları taşımak istediğiniz hedef deponuzın alt klasör yoludur.

## <a name="how-to-use-this-solution-template"></a>Bu çözüm şablonunu kullanma

1. **Dosyaları taşı** şablonuna gidin. Mevcut bağlantıyı seçin veya dosyaları içinden taşımak istediğiniz kaynak dosya deponuza **Yeni** bir bağlantı oluşturun. **DataSource_Folder** ve **DataSource_File** kaynak dosya deponuzda aynı bağlantıya başvuru olduğunu unutmayın.

    ![Kaynağa yeni bir bağlantı oluşturun](media/solution-template-move-files/move-files1.png)

2. Mevcut bağlantıyı seçin veya dosyaları taşımak istediğiniz hedef dosya deponuza **Yeni** bir bağlantı oluşturun.

    ![Hedefe yeni bir bağlantı oluşturun](media/solution-template-move-files/move-files2.png)

3. **Bu şablonu kullan** sekmesini seçin.
    
4. Aşağıdaki örnekte olduğu gibi ardışık düzeni görürsünüz:

    ![İşlem hattını gösterme](media/solution-template-move-files/move-files4.png)

5. **Hata Ayıkla**' yı seçin, **parametreleri**girin ve ardından **son**' u seçin.   Parametreler, dosyaları taşımak istediğiniz klasör yolu ve dosyaları taşımak istediğiniz klasör yoludur. 

    ![İşlem hattını çalıştırma](media/solution-template-move-files/move-files5.png)

6. Sonucu gözden geçirin.

    ![Sonucu gözden geçirin](media/solution-template-move-files/move-files6.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory ile yeni ve değiştirilmiş dosyaları LastModifiedDate göre Kopyala](solution-template-copy-new-files-lastmodifieddate.md)

- [Azure Data Factory ile birden çok kapsayıcıdan dosya kopyalama](solution-template-copy-files-multiple-containers.md)