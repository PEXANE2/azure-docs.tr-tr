---
title: Birden çok kapsayıcıdan dosyaları kopyalama
description: Azure Veri Fabrikası'nı kullanarak dosyaları birden çok kapsayıcıdan kopyalamak için bir çözüm şablonu nasıl kullanacağınızı öğrenin.
services: data-factory
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: anandsub
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 11/1/2018
ms.openlocfilehash: 0c4c26ba163f83483b3eb48e51d91f9a919a887c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75439829"
---
# <a name="copy-files-from-multiple-containers-with-azure-data-factory"></a>Azure Veri Fabrikası ile birden çok kapsayıcıdaki dosyaları kopyalama

Bu makalede, dosya depoları arasında birden çok kapsayıcıdan dosyaları kopyalamak için kullanabileceğiniz bir çözüm şablonu açıklanmaktadır. Örneğin, veri gölünüzü AWS S3'ten Azure Veri Gölü Deposu'na geçirmek için kullanabilirsiniz. Veya şablonu, bir Azure Blob depolama hesabından diğerine her şeyi çoğaltmak için kullanabilirsiniz.

> [!NOTE]
> Dosyaları tek bir kapsayıcıdan kopyalamak istiyorsanız, tek bir kopyalama etkinliği olan bir ardışık iş aktarım oluşturmak için [Veri Kopyalama Aracı'nı](copy-data-tool.md) kullanmak daha verimlidir. Bu makaledeki şablon, bu basit senaryo için gerekenden daha fazladır.

## <a name="about-this-solution-template"></a>Bu çözüm şablonu hakkında

Bu şablon, kaynak depolama alanınızdaki kapsayıcıları doğrular. Daha sonra bu kapsayıcıları hedef mağazaya kopyalar.

Şablon üç etkinlik içerir:
- **GetMetadata** kaynak depolama deponuzu tarar ve konteyner listesini alır.
- **ForEach,** **GetMetadata** etkinliğinden kapsayıcı listesini alır ve ardından liste üzerinde yinelenir ve her kapsayıcıyı Kopyalama etkinliğine geçirir.
- Her kapsayıcıyı kaynak depolama deposundan hedef depoya **kopyalayın.**

Şablon aşağıdaki parametreleri tanımlar:
- *SourceFileFolder,* veri kaynağı mağazanızın kapsayıcıların listesini alabileceğiniz klasör yoludur. Yol, birden çok kapsayıcı klasörü içeren kök dizinidir. Bu parametrenin varsayılan `sourcefolder`değeri .
- *SourceFileDirectory,* veri kaynağı mağazanızın kök dizininin altındaki alt klasör yoludur. Bu parametrenin varsayılan `subfolder`değeri .
- *DestinationFileFolder,* dosyaların hedef mağazanızda kopyalanacağı klasör yoludur. Bu parametrenin varsayılan `destinationfolder`değeri .
- *HedefFileDirectory,* dosyaların hedef mağazanızda kopyalanacağı alt klasör yoludur. Bu parametrenin varsayılan `subfolder`değeri .

## <a name="how-to-use-this-solution-template"></a>Bu çözüm şablonu nasıl kullanılır?

1. Dosya Mağazaları şablonu **arasında birden çok dosya kapsayıcısı kopyala'ya** gidin. Kaynak depolama mağazanıza **yeni** bir bağlantı oluşturun. Kaynak depolama deposu, dosyaları birden çok kapsayıcıdan kopyalamak istediğiniz yerdir.

    ![Kaynağa yeni bir bağlantı oluşturma](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image1.png)

2. Hedef depolama mağazanıza **yeni** bir bağlantı oluşturun.

    ![Hedefe yeni bir bağlantı oluşturma](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image2.png)

3. **Bu şablonu kullan'ı**seçin.

    ![Bu şablonu kullan](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image3.png)
    
4. Aşağıdaki örnekte olduğu gibi, ardışık hattı görürsünüz:

    ![Boru hattını göster](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image4.png)

5. **Hata Ayıklama'yı**seçin, **Parametreleri**girin ve ardından **Bitir'i**seçin.

    ![İşlem hattını çalıştırma](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image5.png)

6. Sonucu gözden geçirin.

    ![Sonucu gözden geçirin](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image6.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Veri Fabrikası ile bir denetim tablosu kullanarak veritabanından toplu kopya](solution-template-bulk-copy-with-control-table.md)

- [Azure Veri Fabrikası ile birden çok kapsayıcıdaki dosyaları kopyalama](solution-template-copy-files-multiple-containers.md)
