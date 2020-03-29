---
title: Dosyaları dosya tabanlı depolama arasında taşıma
description: Azure Veri Fabrikası'nı kullanarak dosyaları dosya tabanlı depolama arasında taşımak için bir çözüm şablonu nasıl kullanacağınızı öğrenin.
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
ms.openlocfilehash: b3165daa06ed975df9ccb677699d3ceb449327ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74941961"
---
# <a name="move-files-with-azure-data-factory"></a>Azure Veri Fabrikası ile dosyaları taşıma

Bu makalede, dosyaları dosya tabanlı mağazalar arasında bir klasörden diğerine taşımak için kullanabileceğiniz bir çözüm şablonu açıklanmaktadır. Bu şablonu kullanmanın yaygın senaryolarından biri: Dosyalar sürekli olarak kaynak mağazanızın iniş klasörüne bırakılır. Bir zamanlama tetikleyicisi oluşturarak, ADF ardışık alanı bu dosyaları düzenli aralıklarla kaynaktan hedef depoya taşıyabilir.  ADF ardışık almanın "hareketli dosyaları" elde etme yolu, dosyaları iniş klasöründen almak, her birini hedef depodaki başka bir klasöre kopyalamak ve aynı dosyaları kaynak depodaki iniş klasöründen siler.

> [!NOTE]
> Bu şablonun klasörleri taşımak yerine dosyaları taşımak için tasarladığını unutmayın.  Yalnızca bir klasör yolu içermesini sağlamak için veri kümesini değiştirerek klasörü taşımak ve ardından bir klasörü temsil eden aynı veri kümesine başvurmak için kopyalama etkinliğini ve etkinliği silmek istiyorsanız, çok dikkatli olmanız gerekir. Bunun nedeni, kopyalama işlemi ile silme işlemi arasında klasöre yeni dosyaların gelmediğinden emin olmak zorunda olduğunuziçindir. Kopyalama etkinliğinizin kopyalama işini tamamladığı ancak Silme etkinliğine bakılmadığını niçin klasöre gelen yeni dosyalar varsa, Delete etkinliği kopyalanmamış olan bu yeni gelen dosyayı silebilir tüm klasörü silerek henüz hedef.

## <a name="about-this-solution-template"></a>Bu çözüm şablonu hakkında

Bu şablon, dosyaları kaynak dosya tabanlı mağazanızdan alır. Daha sonra her birini hedef mağazaya taşır.

Şablon beş etkinlik içerir:
- **GetMetadata,** kaynak deposundaki klasörünüzden dosyalar ve alt klasörler de dahil olmak üzere nesnelerin listesini alır. Nesneleri özyinelemeli olarak almaz. 
- Yalnızca dosyaları seçmek için **GetMetadata** etkinliğinden nesneler listesine **filtre uygulayın.** 
- **ForEach,** **Filtre** etkinliğinden dosya listesini alır ve ardından liste üzerinde yinelenir ve her dosyayı Kopyalama ve Sil etkinliğine geçirir.
- Bir dosyayı kaynaktan hedef mağazaya **kopyalayın.**
- **Sil,** kaynak deposundan aynı dosyayı siler.

Şablon dört parametre tanımlar:
- *SourceStore_Location,* kaynak mağazanızın dosyalarını taşımak istediğiniz klasör yoludur. 
- *SourceStore_Directory,* kaynak mağazanızın dosyalarını taşımak istediğiniz alt klasör yoludur.
- *DestinationStore_Location,* dosyaları taşımak istediğiniz hedef mağazanızın klasör yoludur. 
- *DestinationStore_Directory,* dosyaları taşımak istediğiniz hedef mağazanızın alt klasör yoludur.

## <a name="how-to-use-this-solution-template"></a>Bu çözüm şablonu nasıl kullanılır?

1. **Dosyaları Taşı** şablonuna gidin. Varolan bağlantıyı seçin veya dosyaları taşımak istediğiniz kaynak dosya mağazanıza **yeni** bir bağlantı oluşturun. **DataSource_Folder** ve **DataSource_File** kaynak dosya mağazanızın aynı bağlantısına atıfta bulunduğuna dikkat edin.

    ![Kaynağa yeni bir bağlantı oluşturma](media/solution-template-move-files/move-files1.png)

2. Varolan bağlantıyı seçin veya dosyaları taşımak istediğiniz hedef dosya mağazanıza **yeni** bir bağlantı oluşturun.

    ![Hedefe yeni bir bağlantı oluşturma](media/solution-template-move-files/move-files2.png)

3. **Bu şablon sekmesini kullan'ı** seçin.
    
4. Aşağıdaki örnekte olduğu gibi, ardışık hattı görürsünüz:

    ![Boru hattını göster](media/solution-template-move-files/move-files4.png)

5. **Hata Ayıklama'yı**seçin, **Parametreleri**girin ve ardından **Bitir'i**seçin.   Parametreler, dosyaları taşımak istediğiniz klasör yoludur ve dosyaları taşımak istediğiniz klasör yoludur. 

    ![İşlem hattını çalıştırma](media/solution-template-move-files/move-files5.png)

6. Sonucu gözden geçirin.

    ![Sonucu gözden geçirin](media/solution-template-move-files/move-files6.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Fabrikası ile LastModifiedDate'e göre yeni ve değiştirilmiş dosyaları kopyalama](solution-template-copy-new-files-lastmodifieddate.md)

- [Azure Veri Fabrikası ile birden çok kapsayıcıdaki dosyaları kopyalama](solution-template-copy-files-multiple-containers.md)