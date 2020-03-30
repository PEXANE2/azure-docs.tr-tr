---
title: Blob Depolama'dan SQL Veritabanına veri kopyalama - Azure
description: Bu öğretici, Blob depolamadan SQL veritabanına verileri kopyalamak için Azure Veri Fabrikası ardışık alanında Kopyalama Etkinliği'ni nasıl kullanacağınızı gösterir.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: ''
editor: ''
ms.assetid: e4035060-93bf-4e8d-bf35-35e2d15c51e0
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: cc2f0a513219a671dd8a75ee00af4fc9d4c6a68a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75979737"
---
# <a name="tutorial-copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Öğretici: Veri Fabrikasını kullanarak Blob Depolama'dan SQL Veritabanına veri kopyalama
> [!div class="op_single_selector"]
> * [Genel bakış ve ön koşullar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Kopyalama Sihirbazı](data-factory-copy-data-wizard-tutorial.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [Powershell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager şablonu](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API'si](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz. [kopyalama etkinliği öğreticisi](../quickstart-create-data-factory-dot-net.md).

Bu öğreticide, Blob depolamadan SQL veritabanına veri kopyalamak için bir ardışık işlem aygıtı oluşturursunuz.

Kopyalama Etkinliği, Azure Data Factory’de veri hareketini gerçekleştirir. Bu etkinlik, çeşitli veri depolama alanları arasında güvenli, güvenilir ve ölçeklenebilir bir yolla veri kopyalayabilen genel olarak kullanılabilir bir hizmet tarafından desteklenir. Kopyalama etkinliği hakkında ayrıntılı bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın.  

> [!NOTE]
> Veri Fabrikası hizmetine ayrıntılı bir genel bakış için [Azure Veri Fabrikası'na Giriş makalesine](data-factory-introduction.md) bakın.
>
>

## <a name="prerequisites-for-the-tutorial"></a>Öğretici için ön koşullar
Bu öğreticiye başlamadan önce aşağıdaki önkoşullara sahip olmanız gerekir:

* **Azure aboneliği.**  Bir aboneliğiniz yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için [Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/) makalesine bakın.
* **Azure Depolama Hesabı**. Bu öğreticide blob depolamasını **kaynak** veri deposu olarak kullanırsınız. Azure depolama hesabınız yoksa, bir tane oluşturma adımları için [bir depolama hesabı oluştur](../../storage/common/storage-account-create.md) makalesine bakın.
* **Azure SQL Veritabanı**. Bu öğreticide **hedef** veri deposu olarak bir Azure SQL veritabanı kullanıyorsunuz. Öğreticide kullanabileceğiniz bir Azure SQL veritabanınız yoksa, bir [Azure SQL Veritabanı oluşturmak ve oluşturmak için nasıl yapılandırabileceğinizi](../../sql-database/sql-database-get-started.md) görün.
* **SQL Server 2012/2014 veya Visual Studio 2013**. Örnek bir veritabanı oluşturmak ve veritabanındaki sonuç verilerini görüntülemek için SQL Server Management Studio veya Visual Studio'yu kullanırsınız.  

## <a name="collect-blob-storage-account-name-and-key"></a>Blob depolama hesap adı ve anahtarı toplamak
Bu öğreticiyi yapmak için Azure depolama hesabınızın hesap adı ve hesap anahtarına ihtiyacınız var. Azure depolama hesabınız için **hesap adını** ve **hesap anahtarını** not edin.

1. [Azure portalına](https://portal.azure.com/)giriş yapın.
2. Sol menüdeki **Tüm hizmetler'e** tıklayın ve **Depolama Hesapları'nı**seçin.

    ![Gözat - Depolama hesapları](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/browse-storage-accounts.png)
3. Depolama **Hesapları** bıtır'da, bu öğreticide kullanmak istediğiniz **Azure depolama hesabını** seçin.
4. **AYARLAR**altında **Access tuşları** bağlantısını seçin.
5. **Depolama hesabı adı** metin kutusunun yanındaki **kopyala** (resim) düğmesini tıklatın ve bir yere kaydedin/yapıştırın (örneğin: metin dosyasında).
6. Anahtarı kopyalamak veya not almak için önceki adımı **yineleyin1.**

    ![Depolama erişim anahtarı](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/storage-access-key.png)
7. **X'e**tıklayarak tüm bıçakları kapatın.

## <a name="collect-sql-server-database-user-names"></a>SQL sunucusu, veritabanı, kullanıcı adları toplama
Bu öğreticiyi yapmak için Azure SQL sunucusu, veritabanı ve kullanıcı adlarına ihtiyacınız vardır. Azure SQL veritabanınız için **sunucu,** **veritabanı**ve **kullanıcı** adlarını not edin.

1. Azure **portalında,** soldaki **Tüm hizmetleri** tıklatın ve **SQL veritabanlarını**seçin.
2. SQL **veritabanları blade,** bu öğretici kullanmak istediğiniz **veritabanını** seçin. **Veritabanı adını**not edin.  
3. SQL **veritabanı** bıçak, **AYARLAR**altında **Özellikler'i** tıklatın.
4. **SERVER NAME** ve SERVER **ADMIN Gİrİş**değerlerini not edin.
5. **X'e**tıklayarak tüm bıçakları kapatın.

## <a name="allow-azure-services-to-access-sql-server"></a>Azure hizmetlerinin SQL sunucusuna erişmesine izin ver
Veri Fabrikası hizmetinin Azure SQL sunucunuza erişebilmesi **için** Azure SQL sunucunuz için **Azure hizmetlerine erişim** eki açık olduğundan emin olun. Bu ayarı doğrulamak ve etkinleştirmek için aşağıdaki adımları uygulayın:

1. Soldaki **Tüm Hizmetler** Hub'ını tıklatın ve **SQL sunucularını**tıklatın.
2. Sunucunuzu seçin ve **AYARLAR** altındaki **Güvenlik Duvarı**’na tıklayın.
3. **Güvenlik Duvarı ayarları** dikey penceresinde, **Azure hizmetlerine erişime izin ver** için **AÇIK**’a tıklayın.
4. **X'e**tıklayarak tüm bıçakları kapatın.

## <a name="prepare-blob-storage-and-sql-database"></a>Blob Depolama ve SQL Veritabanı Hazırlama
Şimdi, aşağıdaki adımları gerçekleştirerek Azure blob depolama nızı ve Azure SQL veritabanınızı öğreticiye hazırlayın:  

1. Not Defteri'ni başlatın. Aşağıdaki metni kopyalayın ve sabit diskinizdeki **C:\ADFGetStarted** klasörüne **emp.txt** olarak kaydedin.

    ```
    John, Doe
    Jane, Doe
    ```
2. **Adftutorial** kapsayıcısını oluşturmak ve **emp.txt** dosyasını kapsayıcıya yüklemek için [Azure Depolama Gezgini](https://storageexplorer.com/) gibi araçları kullanın.

3. Azure SQL Veritabanı’nınizde **emp** tablosu oluşturmak için aşağıdaki SQL betiğini kullanın.  

    ```SQL
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

    **Bilgisayarınızda SQL Server 2012/2014 yüklüyse:** Azure SQL sunucunuza bağlanmak ve SQL komut dosyasını çalıştırmak için [SQL Server Management Studio'yu kullanarak Azure SQL Veritabanını Yönetme](../../sql-database/sql-database-manage-azure-ssms.md) yönergelerini izleyin.

    İstemcinizin Azure SQL sunucusuna erişim izni yoksa, makinenizden (IP adresi) erişim izni vermek için Azure SQL sunucunuzun güvenlik duvarını yapılandırmanız gerekir. Azure SQL sunucunuzun güvenlik duvarını yapılandırmaya yönelik adımlar için [bu makaleye](../../sql-database/sql-database-configure-firewall-settings.md) bakın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Ön koşulları tamamladınız. Aşağıdaki yollardan birini kullanarak bir veri fabrikası oluşturabilirsiniz. Öğreticiyi gerçekleştirmek için en üstteki açılır listedeki seçeneklerden birini veya aşağıdaki bağlantıları tıklatın.     

* [Kopyalama Sihirbazı](data-factory-copy-data-wizard-tutorial.md)
* [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
* [Powershell](data-factory-copy-activity-tutorial-using-powershell.md)
* [Azure Resource Manager şablonu](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
* [.NET API'si](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> Bu öğreticideki veri işlem hattı, bir kaynak veri deposundaki verileri hedef veri deposuna kopyalar. Çıkış verileri üretmek için giriş verilerini dönüştürmez. Azure Data Factory kullanarak verileri dönüştürme hakkında bir öğretici için bkz. [Öğretici: Hadoop kümesi kullanarak verileri dönüştürmek için ilk işlem hattınızı oluşturma](data-factory-build-your-first-pipeline.md).
>
> Bir etkinliğin çıkış veri kümesini diğer etkinliğin giriş veri kümesi olarak ayarlayarak iki etkinliği zincirleyebilir, yani bir etkinliği diğerinden sonra çalıştırılmasını sağlayabilirsiniz. Ayrıntılı bilgi için bkz. [Data Factory’de zamanlama ve yürütme](data-factory-scheduling-and-execution.md).
