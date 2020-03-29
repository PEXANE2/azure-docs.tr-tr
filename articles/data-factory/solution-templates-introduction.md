---
title: Şablonlara genel bakış
description: Azure Veri Fabrikası'na hızlı bir şekilde başlamak için önceden tanımlanmış bir şablonu nasıl kullanacağınızı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: daperlov
author: djpmsft
manager: anandsub
ms.custom: seo-lt-2019
ms.date: 01/04/2019
ms.openlocfilehash: f150234ecd3446858e8a6aa7d224eb3ad3d0efd3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74927335"
---
# <a name="templates"></a>Şablonlar

Şablonlar, Veri Fabrikası ile hızlı bir şekilde başlamanızı sağlayan önceden tanımlanmış Azure Veri Fabrikası ardışık hatlarıdır. Şablonlar, Veri Fabrikası'nda yeni olduğunuzda ve hızlı bir şekilde başlamak istediğinizde kullanışlıdır. Bu şablonlar, veri tümleştirme projeleri oluşturmak için geliştirme süresini azaltarak geliştirici üretkenliğini artırır.

## <a name="create-data-factory-pipelines-from-templates"></a>Şablonlardan Veri Fabrikası ardışık hatları oluşturma

Bir şablondan aşağıdaki iki şekilde veri fabrikası ardışık hattı oluşturmaya başlayabilirsiniz:

1.  Şablon galerisini açmak için Genel Bakış sayfasında **şablondan ardışık kaynak oluştur'u** seçin.

    ![Genel Bakış sayfasından şablon galerisini açma](media/solution-templates-introduction/templates-intro-image1.png)

1.  Kaynak Gezgini'ndeki Yazar **+** sekmesinde, şablon galerisini açmak için şablondan sonra **Pipeline'ı** seçin.

    ![Yazar sekmesinden şablon galerisini açma](media/solution-templates-introduction/templates-intro-image2.png)

## <a name="template-gallery"></a>Şablon Galerisi

![Şablon galerisi](media/solution-templates-introduction/templates-intro-image3.png)

### <a name="out-of-the-box-data-factory-templates"></a>Kutunun dışında Veri Fabrikası şablonları

Veri Fabrikası, veri fabrikası ardışık sistem şablonlarını kaydetmek için Azure Kaynak Yöneticisi şablonlarını kullanır. Tüm Kaynak Yöneticisi şablonlarını, kutu Veri Fabrikası şablonları dışında kullanılan bildirim dosyasıyla birlikte [resmi Azure Veri Fabrikası GitHub repo'sunda](https://github.com/Azure/Azure-DataFactory/tree/master/templates)görebilirsiniz. Microsoft tarafından sağlanan önceden tanımlanmış şablonlar aşağıdaki öğeleri içerir, ancak bunlarla sınırlı değildir:

-   Şablonları kopyala:

    -   [Veritabanından toplu kopya](solution-template-bulk-copy-with-control-table.md)
    
    -   [LastModifiedDate parametresine göre yeni dosyaları kopyalama](solution-template-copy-new-files-lastmodifieddate.md)

    -   [Dosya tabanlı mağazalar arasında birden çok dosya kapsayıcısını kopyalama](solution-template-copy-files-multiple-containers.md)

    -   [Dosyaları taşıma](solution-template-move-files.md)

    -   [Veritabanından Delta kopyası](solution-template-delta-copy-with-control-table.md)

    -   Kaynaktan \<\> hedefe \<kopyalama\>

        -   [Amazon S3'ten Azure Data Lake Store Gen 2'ye](solution-template-migration-s3-azure.md)

        -   Google Büyük Sorgu'dan Azure Data Lake Store Gen 2'ye

        -   HDF'den Azure Data Lake Store Gen 2'ye

        -   Netezza'dan Azure Data Lake Store Gen 1'e

        -   SQL Server'dan azure SQL Veritabanı'na

        -   SQL Server'dan azure SQL Veri Ambarı'na

        -   Oracle'dan Azure SQL Veri Ambarı'na

-   SSIS şablonları

    -   SSIS paketlerini çalıştırmak için Azure-SSIS Tümleştirme Çalışma Süresini zamanlayın

-   Şablonları dönüştürme

    -   [Azure Databricks ile ETL](solution-template-databricks-notebook.md)

### <a name="my-templates"></a>Şablonlarım

Ayrıca, Pipeline sekmesinde şablon olarak **kaydet'i** seçerek bir ardışık hattı şablon olarak kaydedebilirsiniz.

![Bir ardışık hattı şablon olarak kaydetme](media/solution-templates-introduction/templates-intro-image4.png)

Şablon Galerisi'nin **Şablonlarım** bölümünde şablon olarak kaydedilen ardışık bölümleri görüntüleyebilirsiniz. Bunları Kaynak Gezgini'ndeki **Şablonlar** bölümünde de görebilirsiniz.

![Şablonlarım](media/solution-templates-introduction/templates-intro-image5.png)

> [!NOTE]
> Şablonlarım özelliğini kullanmak için GIT tümleştirmesini etkinleştirmeniz gerekir. Hem Azure DevOps GIT hem de GitHub desteklenir.
