---
title: 'Hızlı başlangıç: Azure portal ile karma mod örneği oluşturma'
titleSuffix: Azure Database Migration Service
description: Karma modda Azure veritabanı geçiş hizmeti 'nin bir örneğini oluşturmak için Azure portal kullanın.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: quickstart
ms.date: 12/17/2019
ms.openlocfilehash: 64d4998e287f9981c666dee54fc3b67886791bbf
ms.sourcegitcommit: f2149861c41eba7558649807bd662669574e9ce3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75708300"
---
# <a name="quickstart-create-a-hybrid-mode-instance-with-azure-portal--azure-database-migration-service"></a>Hızlı başlangıç: Azure portal & Azure veritabanı geçiş hizmeti ile karma mod örneği oluşturma

Azure veritabanı geçiş hizmeti karma modu, bulutta çalışan bir Azure veritabanı geçiş hizmeti örneğiyle birlikte şirket içinde barındırılan bir geçiş çalışanı kullanarak veritabanı geçişlerini yönetir. Karma mod özellikle, şirket içi ağ ile Azure arasında siteden siteye bağlantı olmaması veya sınırlı siteden siteye bağlantı bant genişliği olduğu senaryolar için yararlıdır.

>[!NOTE]
>Şu anda, karma modda çalışan Azure veritabanı geçiş hizmeti, ' ye SQL Server geçişleri destekler:
>
>- Neredeyse sıfır kapalı kalma süresi (çevrimiçi) olan Azure SQL veritabanı yönetilen örneği.
>- Azure SQL veritabanı 'nın bazı kapalı kalma süresi (çevrimdışı) ile tek veritabanı.
>- Neredeyse sıfır kapalı kalma süresi (çevrimiçi) ile Azure CosmosDB 'ye MongoDb.
>- MongoDb, bazı kapalı kalma süresi (çevrimdışı) ile Azure CosmosDB 'ye.

Bu hızlı başlangıçta, karma modda Azure veritabanı geçiş hizmeti 'nin bir örneğini oluşturmak için Azure portal kullanırsınız. Daha sonra, şirket içi ağınızda karma çalışanı indirir, yükler ve ayarlar. Önizleme sırasında, Azure veritabanı geçiş hizmeti karma modunu kullanarak SQL Server şirket içi örneğinden Azure SQL veritabanı 'na veri geçirebilirsiniz.

> [!NOTE]
> Azure veritabanı geçiş hizmeti karma yükleyicisi Microsoft Windows Server 2012 R2, Windows Server 2016, Windows Server 2019 ve Windows 10 ' da çalışır.

> [!IMPORTANT]
> Azure veritabanı geçiş hizmeti karma yükleyicisi, .NET 4.7.2 veya üstünü gerektirir. En son .NET sürümlerini bulmak için [.NET Framework indirme](https://dotnet.microsoft.com/download/dotnet-framework) sayfasına bakın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Web tarayıcınızı açın, [Microsoft Azure portalına gidin](https://portal.azure.com/) ve kimlik bilgilerinizi girerek portalda oturum açın.

Varsayılan görünüm hizmet panonuzu içerir.

## <a name="register-the-resource-provider"></a>Kaynak sağlayıcısını kaydetme

Azure veritabanı geçiş hizmeti 'nin ilk örneğinizi oluşturmadan önce Microsoft. DataMigration kaynak sağlayıcısını kaydedin.

1. Azure portal, **abonelikler**' i seçin, Azure veritabanı geçiş hizmeti örneğini oluşturmak istediğiniz aboneliği seçin ve ardından **kaynak sağlayıcıları**' nı seçin.

    ![Arama kaynak sağlayıcısı](media/quickstart-create-data-migration-service-hybrid-portal/dms-portal-search-resource-provider.png)

2. "migration" araması yapın ve **Microsoft.DataMigration** öğesinin sağ tarafındaki **Kaydet**'i seçin.

    ![Kaynak sağlayıcısını kaydetme](media/quickstart-create-data-migration-service-hybrid-portal/dms-portal-register-resource-provider.png)

## <a name="create-an-instance-of-the-service"></a>Hizmetin bir örneğini oluşturma

1. Azure veritabanı geçiş hizmeti 'nin bir örneğini oluşturmak için +**kaynak oluştur** ' u seçin.

2. Market 'Te "geçiş" araması yapın, **Azure veritabanı geçiş hizmeti**' ni seçin ve ardından **Azure veritabanı geçiş hizmeti** ekranında **Oluştur**' u seçin.

3. **Geçiş Hizmeti Oluştur** ekranında:

    - Azure veritabanı geçiş hizmeti örneğinizi tanımlamak için hatırlayabileceğiniz ve benzersiz bir **hizmet adı** seçin.
    - Örneği oluşturmak istediğiniz Azure **Aboneliğini** seçin.
    - Yeni bir **Kaynak Grubu** seçin ya da yeni bir tane oluşturun.
    - Kaynak veya hedef sunucunuza en yakın **Konum**’u seçin.
    - **Hizmet modu**için **karma (Önizleme)** öğesini seçin.

           ![Create migration service - basics](media/quickstart-create-data-migration-service-hybrid-portal/dms-create-service-basics.png)

4. **İncele ve oluştur**’u seçin.

5. **Gözden geçir + oluştur** sekmesinde, koşulları gözden geçirin, belirtilen diğer bilgileri doğrulayın ve ardından **Oluştur**' u seçin.

    ![Geçiş hizmeti oluşturma-Inceleme ve oluşturma](media/quickstart-create-data-migration-service-hybrid-portal/dms-create-service-review-and-create.png)

    Birkaç dakika sonra, karma moddaki Azure veritabanı geçiş hizmeti örneğiniz oluşturulup ayarlanmaya hazırlandıktan sonra. Azure veritabanı geçiş hizmeti örneği, aşağıdaki görüntüde gösterildiği gibi görüntülenir:

    ![Azure veritabanı geçiş hizmeti karma mod örneği](media/quickstart-create-data-migration-service-hybrid-portal/dms-instance-hybrid-mode.png)

6. Hizmet oluşturulduktan sonra **Özellikler**' i seçin ve ardından Azure veritabanı geçiş hizmeti karma çalışanını yüklemek Için kullanacağınız **kaynak kimliği** kutusunda görüntülenecek değeri kopyalayın.

    ![Azure veritabanı geçiş hizmeti karma mod özellikleri](media/quickstart-create-data-migration-service-hybrid-portal/dms-copy-resource-id.png)

## <a name="create-azure-app-registration-id"></a>Azure uygulama kayıt KIMLIĞI oluşturma

Şirket içi hibrit çalışanının, Bulutta Azure veritabanı geçiş hizmeti ile iletişim kurmak için kullanabileceği bir Azure uygulama kayıt KIMLIĞI oluşturmanız gerekir.

1. Azure portal **Azure Active Directory**' i seçin, **uygulama kayıtları**' yı seçin ve sonra **Yeni kayıt**' yi seçin.
2. Uygulama için bir ad belirtin ve ardından **Desteklenen hesap türleri**altında, uygulamayı kullanabilecek kişileri belirtmek Için desteklenecek hesap türünü seçin.

    ![Azure veritabanı geçiş hizmeti karma mod kayıt uygulaması](media/quickstart-create-data-migration-service-hybrid-portal/dms-register-application.png)

3. **Yeniden yönlendirme URI 'si (isteğe bağlı)** alanları için varsayılan değerleri kullanın ve ardından **Kaydet**' i seçin.

4. Uygulama KIMLIĞI kaydı tamamlandıktan sonra, karma çalışanı yüklerken kullanacağınız **uygulama (istemci) kimliğini**bir yere getirin.

5. Azure portal Azure veritabanı geçiş hizmeti ' ne gidin, **erişim denetimi (IAM)** seçeneğini belirleyin ve ardından uygulama kimliğine katkıda bulunan erişimi atamak için **rol ataması Ekle** ' yi seçin.

    ![Azure veritabanı geçiş hizmeti karma modu katkıda bulunan rolü atama](media/quickstart-create-data-migration-service-hybrid-portal/dms-app-assign-contributor.png)

6. Rol olarak **katkıda bulunan** ' i seçin, **Azure AD kullanıcısına veya hizmet sorumlusu**'na erişim ATAYıN ve ardından uygulama kimliği adını seçin.

    ![Azure veritabanı geçiş hizmeti karma mod katılımcı rolü ayrıntıları atama](media/quickstart-create-data-migration-service-hybrid-portal/dms-add-role-assignment.png)

7. Azure veritabanı geçiş hizmeti kaynağında uygulama KIMLIĞI için rol atamasını kaydetmek üzere **Kaydet** ' i seçin.

## <a name="download-and-install-the-hybrid-worker"></a>Karma çalışanı indirme ve yükleme

1. Azure portal Azure veritabanı geçiş hizmeti örneğinize gidin.

2. **Ayarlar**altında **karma**' i seçin ve ardından karma çalışanı indirmek için **yükleyici indir** ' i seçin.

    ![Azure veritabanı geçiş hizmeti karma çalışanı indirmesi](media/quickstart-create-data-migration-service-hybrid-portal/dms-installer-download.png)

3. Azure veritabanı geçiş hizmeti karma çalışanını barındıracak sunucuda ZIP dosyasını ayıklayın.

    > [!IMPORTANT]
    > Azure veritabanı geçiş hizmeti karma yükleyicisi, .NET 4.7.2 veya üstünü gerektirir. En son .NET sürümlerini bulmak için [.NET Framework indirme](https://dotnet.microsoft.com/download/dotnet-framework) sayfasına bakın.

4. Install klasöründe **Dmssettings. JSON** dosyasını bulup açın, **ApplicationId** ve **RESOURCEID**belirtin ve ardından dosyayı kaydedin.

    ![Azure veritabanı geçiş hizmeti karma çalışanı ayarları](media/quickstart-create-data-migration-service-hybrid-portal/dms-settings.png)

5. Aşağıdaki komutu kullanarak, Azure veritabanı geçiş hizmeti 'nin karma çalışanından iletişimin kimliğini doğrulamak için kullanabileceği bir sertifika oluşturun.

    ```
    <drive>:\<folder>\Install>DMSWorkerBootstrap.exe -a GenerateCert
    ```

    Install klasöründe bir sertifika oluşturulur.

    ![Azure veritabanı geçiş hizmeti karma çalışanı sertifikası](media/quickstart-create-data-migration-service-hybrid-portal/dms-certificate.png)

6. Azure portal, uygulama KIMLIĞI ' ne gidin, **Yönet**altında sertifika **& gizli**dizileri ' ni seçin ve sonra oluşturduğunuz ortak sertifikayı seçmek için **sertifikayı karşıya yükle** ' yi seçin.

    ![Azure veritabanı geçiş hizmeti karma çalışanı sertifikası karşıya yükleme](media/quickstart-create-data-migration-service-hybrid-portal/dms-app-upload-certificate.png)

7. Aşağıdaki komutu çalıştırarak, Azure veritabanı geçiş hizmeti karma çalışanını şirket içi sunucunuza yüklersiniz:

    ```
    <drive>:\<folder>\Install>DMSWorkerBootstrap.exe -a Install -IAcceptDMSLicenseTerms
    ```

    > [!NOTE]
    > Install komutunu çalıştırırken aşağıdaki parametreleri de kullanabilirsiniz:
    >
    > - **-TelemetryOptOut** -çalışanın telemetri göndermesini durduruyor ancak yerel olarak en düşük düzeyde günlüğe yazmaya devam ediyor.  Yükleyici hala telemetri gönderiyor.
    > - **-p {ınstalllocation}** . , Varsayılan olarak "C:\Program Files\databasemigrationservicehibrit" olan yükleme yolunu değiştirmeye izin vermez.

8. Yükleyici hatasız çalışırsa, hizmet Azure veritabanı geçiş hizmeti 'nde çevrimiçi bir durum gösterir ve veritabanlarınızı geçirmeye hazırsınız demektir.

    ![Azure veritabanı geçiş hizmeti çevrimiçi](media/quickstart-create-data-migration-service-hybrid-portal/dms-instance-hybrid-mode-online.png)

## <a name="uninstall-azure-database-migration-service-hybrid-mode"></a>Azure veritabanı geçiş hizmeti karma modunu kaldır

Şu anda, Azure veritabanı geçiş hizmeti karma modunun kaldırılması, şu komutu kullanarak yalnızca şirket içi sunucunuzdaki Azure veritabanı geçiş hizmeti karma çalışan yükleyicisi aracılığıyla desteklenir:

```
<drive>:\<folder>\Install>DMSWorkerBootstrap.exe -a uninstall
```

> [!NOTE]
> Uninstall komutunu çalıştırırken, generateCert iş akışı tarafından oluşturulan Uyarp sertifikasını tutan "-ReuseCert" parametresini de kullanabilirsiniz.  Bu, daha önce oluşturulmuş ve karşıya yüklenen aynı sertifikayı kullanmayı mümkün.

## <a name="set-up-the-azure-database-migration-service-hybrid-worker-using-powershell"></a>PowerShell kullanarak Azure veritabanı geçiş hizmeti karma çalışanını ayarlama

Azure veritabanı geçiş hizmeti karma çalışanını Azure portal aracılığıyla yüklemenin yanı sıra, karma modda yeni bir Azure veritabanı geçiş hizmeti örneği oluşturduktan sonra çalışan yükleme adımlarını otomatik hale getirmek için kullanabileceğiniz bir [PowerShell betiği](https://techcommunity.microsoft.com/gxcuf89792/attachments/gxcuf89792/MicrosoftDataMigration/119/1/DMS_Hybrid_Script.zip) sağlıyoruz. Betik:

1. Yeni bir Uyarp oluşturur.
2. Yükleyiciyi indirir.
3. GenerateCert iş akışını çalıştırır.
4. Sertifikayı karşıya yükler.
5. Azure veritabanı geçiş hizmeti örneğiniz için uygun olarak uyarlana ekler.
6. Yüklemesi iş akışını çalıştırır.

Bu betik, Kullanıcı ortamda gerekli tüm izinlere zaten sahip olduğunda hızlı prototipleme için tasarlanmıştır. Üretim ortamınızda, uyumlu olduğunu ve sertifikayı farklı gereksinimlere sahip olabileceğini unutmayın, bu nedenle komut dosyası başarısız olabilir.

> [!IMPORTANT]
> Bu betik, Azure veritabanı geçiş hizmeti 'nin karma modda var olan bir örneği olduğunu ve kullanılan Azure hesabının kiracıda bir Uyarps oluşturma ve abonelikte RBAC 'yi değiştirme izinlerine sahip olduğunu varsayar.

Komut dosyasının en üstündeki parametreleri doldurmanız ve sonra betiği bir yönetici PowerShell örneğinden çalıştırmanız yeterlidir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [SQL Server çevrimiçi bir Azure SQL veritabanı yönetilen örneğine geçirin](tutorial-sql-server-managed-instance-online.md)
> [Azure sql veritabanı 'nda SQL Server tek bir veritabanına veya havuza alınmış veritabanına geçirin](tutorial-sql-server-to-azure-sql.md)
