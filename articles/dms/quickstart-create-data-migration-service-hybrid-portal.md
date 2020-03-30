---
title: 'Hızlı başlangıç: Azure portalı ile karma mod örneği oluşturma'
titleSuffix: Azure Database Migration Service
description: Karma modda Azure Veritabanı Geçiş Hizmeti örneği oluşturmak için Azure portalını kullanın.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: quickstart
ms.date: 03/13/2020
ms.openlocfilehash: dd3e77610749eb5d146b0c0b7cf9d307fba0dd83
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79370245"
---
# <a name="quickstart-create-a-hybrid-mode-instance-with-azure-portal--azure-database-migration-service"></a>Hızlı başlatma: Azure portalı & Azure Veritabanı Geçiş Hizmeti ile karma mod örneği oluşturma

Azure Veritabanı Geçiş Hizmeti karma modu, bulutta çalışan Azure Veritabanı Geçiş Hizmeti örneğiyle birlikte şirket içinde barındırılan bir geçiş çalışanı kullanarak veritabanı geçişlerini yönetir. Karma mod, şirket içi ağ ile Azure arasında siteden siteye bağlantı eksikliği olan veya siteden siteye bağlantı bant genişliğinin sınırlı olduğu senaryolar için özellikle yararlıdır.

>[!NOTE]
>Şu anda, karma modda çalışan Azure Veritabanı Geçiş Hizmeti SQL Server geçişlerini destekler:
>
>- Azure SQL Veritabanı, sıfıra yakın kapalı kalma süresi (çevrimiçi) ile yönetilen örnek.
>- Azure SQL Veritabanı bazı kapalı kalma süresi (çevrimdışı) ile tek veritabanı.
>- MongoDb'dan Azure CosmosDB'ye neredeyse sıfır kapalı kalma süresiyle (çevrimiçi).
>- MongoDb'dan Azure CosmosDB'ye biraz kapalı kalma süresi (çevrimdışı) ile birlikte.

Bu Quickstart'ta, azure veritabanı geçiş hizmetinin karma modda bir örneğini oluşturmak için Azure portalını kullanırsınız. Daha sonra, karma çalışanı şirket içi ağınızda indirir, yükler ve ayarlarsınız. Önizleme sırasında, verileri SQL Server'ın şirket içi bir örneğinden Azure SQL Veritabanı'na geçirmek için Azure Veritabanı Geçiş Hizmeti karma modunu kullanabilirsiniz.

> [!NOTE]
> Azure Veritabanı Geçiş Hizmeti karma yükleyicisi Microsoft Windows Server 2012 R2, Window Server 2016, Windows Server 2019 ve Windows 10'da çalışır.

> [!IMPORTANT]
> Azure Veritabanı Geçiş Hizmeti karma yükleyici ,NET 4.7.2 veya daha sonra gerektirir. .NET'in en son sürümlerini bulmak için [Download .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework) sayfasına bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Web tarayıcınızı açın, [Microsoft Azure portalına gidin](https://portal.azure.com/) ve kimlik bilgilerinizi girerek portalda oturum açın.

Varsayılan görünüm hizmet panonuzu içerir.

## <a name="register-the-resource-provider"></a>Kaynak sağlayıcısını kaydetme

Azure Veritabanı Geçiş Hizmeti'nin ilk örneğini oluşturmadan önce Microsoft.DataMigration kaynak sağlayıcısını kaydedin.

1. Azure portalında, **Abonelikler'i**seçin, Azure Veritabanı Geçiş Hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve ardından **Kaynak sağlayıcıları**seçin.

    ![Kaynak sağlayıcısını ara](media/quickstart-create-data-migration-service-hybrid-portal/dms-portal-search-resource-provider.png)

2. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/quickstart-create-data-migration-service-hybrid-portal/dms-portal-register-resource-provider.png)

## <a name="create-an-instance-of-the-service"></a>Hizmetin bir örneğini oluşturma

1. Azure Veritabanı Geçiş Hizmeti'nin bir örneğini oluşturmak için kaynak**oluşturun** + seçin.

2. "Geçiş" için Market'te arama yapın, **Azure Veritabanı Geçiş Hizmeti'ni**seçin ve ardından Azure Veritabanı **Geçiş Hizmeti** ekranında **Oluştur'u**seçin.

3. **Geçiş Hizmeti Oluştur** ekranında:

    - Azure Veritabanı Geçiş Hizmeti örneğini tanımlamak için unutulmaz ve benzersiz bir **Hizmet Adı** seçin.
    - Örneği oluşturmak istediğiniz Azure **Aboneliğini** seçin.
    - Yeni bir **Kaynak Grubu** seçin ya da yeni bir tane oluşturun.
    - Kaynak veya hedef sunucunuza en yakın **Konum**’u seçin.
    - **Servis modu için**Karma **(Önizleme) seçeneğini belirleyin.**

         ![Geçiş hizmeti oluşturma - temel bilgiler](media/quickstart-create-data-migration-service-hybrid-portal/dms-create-service-basics.png)

4. **İncele ve oluştur**’u seçin.

5. Gözden **Geçir + oluştur** sekmesinde, Şartları gözden geçirin, sağlanan diğer bilgileri doğrulayın ve sonra **Oluştur'u**seçin.

    ![Geçiş hizmeti oluşturma - Gözden Geçirin + oluştur](media/quickstart-create-data-migration-service-hybrid-portal/dms-create-service-review-and-create.png)

    Birkaç dakika sonra, karma modda Azure Veritabanı Geçiş Hizmeti örneğiniz oluşturulur ve ayarlanmaya hazır olur. Azure Veritabanı Geçiş Hizmeti örneği aşağıdaki resimde gösterildiği gibi görüntüler:

    ![Azure Veritabanı Geçiş Hizmeti karma modu örneği](media/quickstart-create-data-migration-service-hybrid-portal/dms-instance-hybrid-mode.png)

6. Hizmet oluşturulduktan sonra **Özellikler'i**seçin ve ardından Azure Veritabanı Geçiş Hizmeti karma işçisini yüklemek için kullanacağınız **Kaynak Kimliği** kutusunda görüntülenen değeri kopyalayın.

    ![Azure Veritabanı Geçiş Hizmeti karma mod özellikleri](media/quickstart-create-data-migration-service-hybrid-portal/dms-copy-resource-id.png)

## <a name="create-azure-app-registration-id"></a>Azure Uygulama kayıt kimliği oluşturma

Şirket içi karma çalışanın bulutta Azure Veritabanı Geçiş Hizmeti ile iletişim kurmak için kullanabileceği bir Azure Uygulaması kayıt kimliği oluşturmanız gerekir.

1. Azure portalında **Azure Etkin Dizin'i**seçin, **Uygulama kayıtlarını**seçin ve ardından **Yeni kayıt'ı**seçin.
2. Uygulama için bir ad belirtin ve desteklenen **hesap türleri**altında uygulamayı kimlerin kullanabileceğini belirtmek için desteklenen hesap türünü seçin.

    ![Azure Veritabanı Geçiş Hizmeti karma modu kayıt uygulaması](media/quickstart-create-data-migration-service-hybrid-portal/dms-register-application.png)

3. **Uri'yi Yeniden Yönlendirme (isteğe bağlı)** alanları için varsayılan değerleri kullanın ve ardından **Kaydol'u**seçin.

4. Uygulama Kimliği kaydı tamamlandıktan sonra, karma işçiyi yüklerken kullanacağınız **Uygulama (istemci) kimliğine**bir not açın.

5. Azure portalında Azure Veritabanı Geçiş Hizmeti'ne gidin, **Access denetimi (IAM)** seçeneğini belirleyin ve ardından katılımcıyı Uygulama Kimliği'ne erişmek için **rol atamaekle'yi** seçin.

    ![Azure Veritabanı Geçiş Hizmeti karma modu katılımcı rolü ata](media/quickstart-create-data-migration-service-hybrid-portal/dms-app-assign-contributor.png)

6. Rol olarak **Katılımcı'yı** seçin, **Azure AD kullanıcısına veya hizmet sorumlusuna**erişim atayın ve ardından Uygulama Kimliği adını seçin.

    ![Azure Veritabanı Geçiş Hizmeti karma modu katılımcı rol ayrıntılarını atadı](media/quickstart-create-data-migration-service-hybrid-portal/dms-add-role-assignment.png)

7. Azure Veritabanı Geçiş Hizmeti kaynağındaki Uygulama Kimliği için rol atamasını kaydetmek için **Kaydet'i** seçin.

## <a name="download-and-install-the-hybrid-worker"></a>Karma işçiyi indirin ve kurutun

1. Azure portalında, Azure Veritabanı Geçiş Hizmeti örneğinize gidin.

2. **Ayarlar**altında, **Karma'yı**seçin ve ardından karma çalışanı indirmek için **Installer karşıdan yükleme'yi** seçin.

    ![Azure Veritabanı Geçiş Hizmeti karma işçi indir](media/quickstart-create-data-migration-service-hybrid-portal/dms-installer-download.png)

3. Azure Veritabanı Geçiş Hizmeti karma işçisini barındıracak sunucudaki ZIP dosyasını ayıklayın.

    > [!IMPORTANT]
    > Azure Veritabanı Geçiş Hizmeti karma yükleyici ,NET 4.7.2 veya daha sonra gerektirir. .NET'in en son sürümlerini bulmak için [Download .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework) sayfasına bakın.

4. Install **klasöründe, dmsSettings.json** dosyasını bulun ve açın, **ApplicationId** ve **resourceId'yi**belirtin ve ardından dosyayı kaydedin.

    ![Azure Veritabanı Geçiş Hizmeti karma alt ayarları](media/quickstart-create-data-migration-service-hybrid-portal/dms-settings.png)

5. Azure Veritabanı Geçiş Hizmeti'nin, aşağıdaki komutu kullanarak karma çalışanın iletişimini doğrulamak için kullanabileceği bir sertifika oluşturun.

    ```
    <drive>:\<folder>\Install>DMSWorkerBootstrap.exe -a GenerateCert
    ```

    Yükle klasöründe bir sertifika oluşturulur.

    ![Azure Veritabanı Geçiş Hizmeti karma işçi sertifikası](media/quickstart-create-data-migration-service-hybrid-portal/dms-certificate.png)

6. Azure portalında, Uygulama Kimliği'ne gidin, **Yönet**altında , **Sertifikalı & sırları**seçin ve ardından oluşturduğunuz herkese açık sertifikayı seçmek için Yükle **sertifikasını** seçin.

    ![Azure Veritabanı Geçiş Hizmeti karma işçi sertifikası yükleme](media/quickstart-create-data-migration-service-hybrid-portal/dms-app-upload-certificate.png)

7. Aşağıdaki komutu çalıştırarak azure veritabanı geçiş hizmeti karma işçisini şirket içi sunucunuza yükleyin:

    ```
    <drive>:\<folder>\Install>DMSWorkerBootstrap.exe -a Install -IAcceptDMSLicenseTerms -d
    ```

    > [!NOTE]
    > Yükleme komutunu çalıştırırken, aşağıdaki parametreleri de kullanabilirsiniz:
    >
    > - **-TelemetryOptOut - Telemetri** göndermeyi durdurur ancak yerel olarak minimum düzeyde günlüğe kaydetmeye devam ediyor.  Yükleyici hala telemetri gönderir.
    > - **-p {InstallLocation}**. Varsayılan olarak "C:\Program Files\DatabaseMigrationServiceHybrid" olan yükleme yolunu değiştirmeyi sağlar.

8. Yükleyici hatasız çalışırsa, hizmet Azure Veritabanı Geçiş Hizmeti'nde çevrimiçi bir durum gösterir ve veritabanlarınızı taşımaya hazırsınız.

    ![Azure Veritabanı Geçiş Hizmeti çevrimiçi](media/quickstart-create-data-migration-service-hybrid-portal/dms-instance-hybrid-mode-online.png)

## <a name="uninstall-azure-database-migration-service-hybrid-mode"></a>Azure Veritabanı Geçiş Hizmeti karma modunu kaldırma

Şu anda, Azure Veritabanı Geçiş Hizmeti karma modunu kaldırmak yalnızca şirket içi sunucunuzdaki Azure Veritabanı Geçiş Hizmeti karma alt yükleyicisi aracılığıyla aşağıdaki komutu kullanarak desteklenir:

```
<drive>:\<folder>\Install>DMSWorkerBootstrap.exe -a uninstall
```

> [!NOTE]
> Kaldır komutunu çalıştırırken, oluşturulan Cert iş akışı tarafından oluşturulan AdApp sertifikasını tutan "-Yeniden Kullan" parametresini de kullanabilirsiniz.  Bu, daha önce oluşturulan ve yüklenen aynı sertifikayı kullanmanızı sağlar.

## <a name="set-up-the-azure-database-migration-service-hybrid-worker-using-powershell"></a>PowerShell'i kullanarak Azure Veritabanı Geçiş Hizmeti karma işçisini ayarlama

Azure portalı üzerinden Azure Veritabanı Geçiş Hizmeti karma işçisini yüklemenin yanı sıra, karma modda Azure Veritabanı Geçiş Hizmeti'nin yeni bir örneğini oluşturduktan sonra çalışan yükleme adımlarını otomatikleştirmek için kullanabileceğiniz bir [PowerShell komut dosyası](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/119/1/DMS_Hybrid_Script.zip) sağlarız. Komut dosyası:

1. Yeni bir AdApp oluşturur.
2. Yükleyiciindirir.
3. GenerateCert iş akışını çalıştırın.
4. Sertifikayı yükler.
5. AdApp'ı Azure Veritabanı Geçiş Hizmeti örneğinize katkıda bulunan kişi olarak ekler.
6. Yükleme iş akışını çalıştırın.

Bu komut dosyası, kullanıcı zaten ortamda gerekli tüm izinlere sahip olduğunda hızlı prototipleme için tasarlanmıştır. Üretim ortamınızda, Komut dosyasının başarısız olması için AdApp ve Cert'in farklı gereksinimleri olabileceğini unutmayın.

> [!IMPORTANT]
> Bu komut dosyası, karma modda Azure Veritabanı Geçiş Hizmeti'nin varolan bir örneğinin olduğunu ve kullanılan Azure hesabının kiracıda AdApps oluşturma ve abonelikte RBAC'ı değiştirme izinlerine sahip olduğunu varsayar.

Komut dosyasının üst kısmındaki parametreleri doldurun ve ardından komut dosyasını bir Administrator PowerShell örneğinden çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [SQL Server'ı Azure SQL Veritabanı'na geçirin örnek çevrimiçi](tutorial-sql-server-managed-instance-online.md)
> [Geçir SQL Server'ı tek bir veritabanına veya Azure SQL Veritabanı'nda birleştirilmiş veritabanına çevrimdışı](tutorial-sql-server-to-azure-sql.md) olarak geçirin
