---
title: Depolama Gezgini kullanmaya başlayın | Microsoft Docs
description: Depolama Gezgini ile Azure depolama kaynaklarını yönetmeye başlayın. Azure Depolama Gezgini indirin ve yükleyin, bir depolama hesabına veya hizmetine bağlanın ve daha fazlasını yapın.
services: storage
author: cawaMS
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.date: 11/08/2019
ms.author: cawa
ms.openlocfilehash: 3a8fe3ded6608059cc6ad50901ffe6df5dcf1b08
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102441597"
---
# <a name="get-started-with-storage-explorer"></a>Depolama Gezgini ile çalışmaya başlama

## <a name="overview"></a>Genel Bakış

Microsoft Azure Depolama Gezgini; Windows, macOS ve Linux’ta Azure Depolama ile çalışmayı kolaylaştıran bir tek başına uygulamadır.

Bu makalede, Azure depolama hesaplarınızı bağlamak ve yönetmek için kullanabileceğiniz çeşitli yollar öğreneceksiniz.

:::image type="content" alt-text="Microsoft Azure Depolama Gezgini" source="./vs-storage-explorer-overview.png":::

## <a name="prerequisites"></a>Önkoşullar

# <a name="windows"></a>[Windows](#tab/windows)

Aşağıdaki Windows destek sürümleri Depolama Gezgini:

* Windows 10 (önerilir)
* Windows 8
* Windows 7

Tüm Windows sürümleri için Depolama Gezgini en az .NET Framework 4.7.2 gerektirir.

# <a name="macos"></a>[macOS](#tab/macos)

MacOS desteğinin aşağıdaki sürümleri Depolama Gezgini:

* macOS 10,12 Sierra ve sonraki sürümleri

# <a name="linux"></a>[Linux](#tab/linux)

Depolama Gezgini, Linux 'un en yaygın dağıtımları için [ek mağazada](https://snapcraft.io/storage-explorer) kullanılabilir. Bu yükleme için ek depolama önerilir. Depolama Gezgini Snap, yeni sürümler Snap Store 'da yayımlandığında tüm bağımlılıklarını ve güncelleştirmelerini yüklüyor.

Desteklenen dağıtımlar için [ `snapd` yükleme sayfasına](https://snapcraft.io/docs/installing-snapd)bakın.

Depolama Gezgini, parola Yöneticisi 'nin kullanılmasını gerektirir. Parola yöneticisine el ile bağlanmanız gerekebilir. Aşağıdaki komutu çalıştırarak, Depolama Gezgini sisteminizin parola yöneticisine bağlayabilirsiniz:

```bash
snap connect storage-explorer:password-manager-service :password-manager-service
```

Depolama Gezgini *. tar. gz* Download olarak da kullanılabilir. *. Tar. gz* kullanıyorsanız, bağımlılıkları el ile yüklemelisiniz. Linux support *. tar. gz* yüklemesi için aşağıdaki dağıtımlar:

* Ubuntu 20,04 x64
* Ubuntu 18,04 x64
* Ubuntu 16,04 x64

*. Tar. gz* yüklemesi diğer dağıtımlar üzerinde çalışabilir, ancak yalnızca bu listelenmiş olanlar resmi olarak desteklenmektedir.

Linux üzerinde Depolama Gezgini yükleme hakkında daha fazla yardım için, Azure Depolama Gezgini sorun giderme kılavuzunda [Linux bağımlılıkları](./storage/common/storage-explorer-troubleshooting.md#linux-dependencies) bölümüne bakın.

---

## <a name="download-and-install"></a>İndirme ve yükleme

Depolama Gezgini indirmek ve yüklemek için bkz. [Azure Depolama Gezgini](https://www.storageexplorer.com).

## <a name="connect-to-a-storage-account-or-service"></a>Bir depolama hesabı veya hizmetine bağlanmak

Depolama Gezgini Azure kaynaklarına bağlanmak için çeşitli yollar sağlar:

* [Aboneliklerinize ve kaynaklarına erişmek için Azure 'da oturum açın](#sign-in-to-azure)
* [Tek bir Azure depolama kaynağına iliştirme](#attach-to-an-individual-resource)
* [CosmosDB kaynağına iliştirme](#connect-to-azure-cosmos-db)

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

> [!NOTE]
> Oturum açtıktan sonra kaynakları tam olarak erişmek için Depolama Gezgini hem yönetim (Azure Resource Manager) hem de veri katmanı izinleri gerektirir. Bu, depolama hesabınıza, hesaptaki kapsayıcılara ve kapsayıcılardaki verilere erişmek için Azure Active Directory (Azure AD) izinlerinizin olması gerektiği anlamına gelir. Yalnızca veri katmanında izinleriniz varsa, bir kaynağa eklenirken **Azure Active Directory (Azure AD) kullanarak oturum aç** seçeneğini seçmeyi göz önünde bulundurun. Depolama Gezgini gerekli olan belirli izinler hakkında daha fazla bilgi için, bkz. [Azure Depolama Gezgini sorun giderme kılavuzu](./storage/common/storage-explorer-troubleshooting.md#azure-rbac-permissions-issues).

1. Depolama Gezgini, hesap yönetimini **görüntüle**' yi seçin  >   veya **hesapları Yönet** düğmesini seçin.

    :::image type="content" alt-text="Hesapları yönetme" source ="./vs-storage-explorer-manage-accounts.png":::

1. **Hesap yönetimi** artık oturum açtığınız tüm Azure hesaplarını görüntülüyor. Başka bir hesaba bağlanmak için **Hesap Ekle...** seçeneğini belirleyin.

1. **Azure depolama 'Ya Bağlan** iletişim kutusu açılır. **Kaynak Seç** panelinde, **abonelik**' ı seçin.

    :::image type="content" alt-text="Bağlan iletişim kutusu" source="./vs-storage-explorer-connect-dialog.png":::

1. **Azure ortamı Seç** panelinde, oturum açmak Için bir Azure ortamı seçin. Genel Azure 'da, ulusal bir bulutta veya bir Azure Stack örneğinde oturum açabilirsiniz. Sonra **İleri**’yi seçin.

    :::image type="content" alt-text="Oturum açma seçeneği" source="./vs-storage-explorer-connect-environment.png":::

    > [!TIP]
    > Azure Stack hakkında daha fazla bilgi için bkz. [Depolama Gezgini Azure Stack aboneliğine veya depolama hesabına bağlanma](/azure-stack/user/azure-stack-storage-connect-se).

1. Depolama Gezgini, oturum açmak için bir Web sayfası açar.

1. Azure hesabıyla başarıyla oturum açtıktan sonra hesap **yönetimi** altında bu hesapla ilişkili hesap ve Azure abonelikleri görüntülenir. Birlikte çalışmak istediğiniz Azure aboneliklerini seçin ve ardından **Uygula**’yı seçin.

    :::image type="content" alt-text="Azure aboneliklerini seçme" source="./vs-storage-explorer-account-panel.png":::

1. **Gezgin** seçili Azure abonelikleriyle ilişkili depolama hesaplarını görüntüler.

    :::image type="content" alt-text="Seçili Azure abonelikleri" source="./vs-storage-explorer-subscription-node.png":::

### <a name="attach-to-an-individual-resource"></a>Tek bir kaynağa iliştirme

Depolama Gezgini, çeşitli kimlik doğrulama yöntemlerini kullanarak Azure Data Lake Storage 2. kapsayıcısı gibi ayrı kaynaklara bağlanmanızı sağlar. Bazı kimlik doğrulama yöntemleri yalnızca belirli kaynak türleri için desteklenir.

| Kaynak türü    | Azure AD | Hesap adı ve anahtarı | Paylaşılan Erişim İmzası (SAS)  | Ortak (anonim) |
|------------------|----------|----------------------|--------------------------------|--------------------|
| Depolama hesapları | Yes      | Yes                  | Evet (bağlantı dizesi veya URL) | No                 |
| Blob kapsayıcıları  | Yes      | Hayır                   | Evet (URL)                      | Yes                |
| Gen2 kapsayıcıları  | Yes      | Hayır                   | Evet (URL)                      | Yes                |
| Gen2 dizinleri | Yes      | Hayır                   | Evet (URL)                      | Yes                |
| Dosya paylaşımları      | Hayır       | Hayır                   | Evet (URL)                      | No                 |
| Kuyruklar           | Yes      | Hayır                   | Evet (URL)                      | No                 |
| Tables           | Hayır       | Hayır                   | Evet (URL)                      | No                 |
 
Depolama Gezgini, öykünücü tarafından yapılandırılan bağlantı noktalarını kullanarak [yerel bir depolama öykünücüsüne](#local-storage-emulator) da bağlanabilir.

Tek bir kaynağa bağlanmak için sol taraftaki araç çubuğundan **Bağlan** düğmesini seçin. Ardından, bağlanmak istediğiniz kaynak türü için yönergeleri izleyin.

:::image type="content" alt-text="Azure Storage’a bağlanma seçeneği" source="./vs-storage-explorer-connect-button.png":::

Bir depolama hesabına bağlantı başarıyla eklendiğinde, **Yerel & bağlı**  >  **depolama hesapları** altında yeni bir ağaç düğümü görüntülenir.

Diğer kaynak türlerinde, **Yerel & bağlı**  >  **depolama hesapları**  >  **(ekli kapsayıcılar)** altına yeni bir düğüm eklenir. Düğüm, türüyle eşleşen bir Grup düğümü altında görünür. Örneğin, Azure Data Lake Storage 2. kapsayıcısına yeni bir bağlantı, **BLOB kapsayıcıları** altında görünür.

Depolama Gezgini bağlantınızı ekleyemedik veya bağlantıyı başarıyla ekledikten sonra verilerinize erişemiyorsanız, bkz. [Azure Depolama Gezgini sorun giderme kılavuzu](./storage/common/storage-explorer-troubleshooting.md).

Aşağıdaki bölümlerde, tek tek kaynaklara bağlanmak için kullanabileceğiniz farklı kimlik doğrulama yöntemleri açıklanır.

#### <a name="azure-ad"></a>Azure AD

Depolama Gezgini, Azure hesabınızı kullanarak aşağıdaki kaynak türlerine bağlanabilir:
* Blob kapsayıcıları
* Azure Data Lake Storage 2. kapsayıcılar
* Azure Data Lake Storage 2. dizinleri
* Kuyruklar
 
Kaynak için veri katmanı erişiminiz varsa ancak yönetim katmanı erişimi yoksa, Azure AD tercih edilen seçenektir.

1. [Yukarıda açıklanan adımları](#sign-in-to-azure)kullanarak en az bir Azure hesabında oturum açın.
1. **Azure depolama 'Ya Bağlan** Iletişim kutusunun **Kaynak Seç** panelinde **BLOB kapsayıcı**, **ADLS 2. kapsayıcısı** veya **kuyruk**' u seçin.
1. **Azure Active Directory (Azure AD) kullanarak oturum aç** ' ı seçin ve **İleri ' yi** seçin.
1. Bir Azure hesabı ve kiracı seçin. Hesap ve kiracının, iliştirmek istediğiniz depolama kaynağına erişimi olmalıdır. **İleri**’yi seçin.
1. Bağlantınız için bir görünen ad ve kaynağın URL 'SI girin. **İleri**’yi seçin.
1. **Özet** panelinde bağlantı bilgilerinizi gözden geçirin. Bağlantı bilgileri doğruysa, **Bağlan**' ı seçin.

#### <a name="account-name-and-key"></a>Hesap adı ve anahtarı

Depolama Gezgini, depolama hesabının adını ve anahtarını kullanarak bir depolama hesabına bağlanabilir.

Hesap anahtarlarınızı [Azure Portal](https://portal.azure.com)bulabilirsiniz. Depolama hesabı sayfanızı açın ve **Ayarlar**  >  **erişim Anahtarlar**' ı seçin.

1. **Azure depolama 'Ya Bağlan** Iletişim kutusunun **Kaynak Seç** panelinde **depolama hesabı**' nı seçin.
1. **Hesap adı ve anahtarı** ' nı seçin ve **İleri ' yi** seçin.
1. Bağlantınız için bir görünen ad, hesap adı ve hesap anahtarlarından birini girin. Uygun Azure ortamını seçin. **İleri**’yi seçin.
1. **Özet** panelinde bağlantı bilgilerinizi gözden geçirin. Bağlantı bilgileri doğruysa, **Bağlan**' ı seçin.

#### <a name="shared-access-signature-sas-connection-string"></a>Paylaşılan erişim imzası (SAS) bağlantı dizesi

Depolama Gezgini, paylaşılan erişim Imzası (SAS) olan bir bağlantı dizesi kullanarak bir depolama hesabına bağlanabilir. SAS bağlantı dizesi şuna benzer:

```text
SharedAccessSignature=sv=2020-04-08&ss=btqf&srt=sco&st=2021-03-02T00%3A22%3A19Z&se=2020-03-03T00%3A22%3A19Z&sp=rl&sig=fFFpX%2F5tzqmmFFaL0wRffHlhfFFLn6zJuylT6yhOo%2FY%3F;
BlobEndpoint=https://contoso.blob.core.windows.net/;
FileEndpoint=https://contoso.file.core.windows.net/;
QueueEndpoint=https://contoso.queue.core.windows.net/;
TableEndpoint=https://contoso.table.core.windows.net/;
```

1. **Azure depolama 'Ya Bağlan** Iletişim kutusunun **Kaynak Seç** panelinde **depolama hesabı**' nı seçin.
1. **Paylaşılan erişim imzası (SAS)** öğesini seçin ve **İleri ' yi** seçin.
1. Bağlantınız için bir görünen ad ve depolama hesabı için SAS bağlantı dizesi girin. **İleri**’yi seçin.
1. **Özet** panelinde bağlantı bilgilerinizi gözden geçirin. Bağlantı bilgileri doğruysa, **Bağlan**' ı seçin.

#### <a name="shared-access-signature-sas-url"></a>Paylaşılan erişim imzası (SAS) URL 'SI

Depolama Gezgini, SAS URI 'SI kullanarak aşağıdaki kaynak türlerine bağlanabilir:
* Blob kapsayıcı
* Azure Data Lake Storage 2. kapsayıcı veya Dizin
* Dosya paylaşımı
* Kuyruk
* Tablo

SAS URI 'SI şöyle görünür:

```text
https://contoso.blob.core.windows.net/container01?sv=2020-04-08&st=2021-03-02T00%3A30%3A33Z&se=2020-03-03T00%3A30%3A33Z&sr=c&sp=rl&sig=z9VFdWffrV6FXU51T8b8HVfipZPOpYOFLXuQw6wfkFY%3F
```

1. **Azure depolama 'Ya Bağlan** Iletişim kutusunun **Kaynak Seç** panelinde, bağlanmak istediğiniz kaynağı seçin.
1. **Paylaşılan erişim imzası (SAS)** öğesini seçin ve **İleri ' yi** seçin.
1. Bağlantınız için bir görünen ad ve kaynak için SAS URI 'SI girin. **İleri**’yi seçin.
1. **Özet** panelinde bağlantı bilgilerinizi gözden geçirin. Bağlantı bilgileri doğruysa, **Bağlan**' ı seçin.

#### <a name="local-storage-emulator"></a>Yerel depolama öykünücüsü

Depolama Gezgini, bir Azure Storage öykünücüsüne bağlanabilir. Şu anda iki desteklenen öykünücü vardır:

* [Azure depolama öykünücüsü](storage/common/storage-use-emulator.md) (yalnızca Windows)
* [Azurite](https://github.com/azure/azurite) (Windows, MacOS veya Linux)

Öykünücüsü varsayılan bağlantı noktalarını dinliyorsa, öykünücüsizin erişmek için **Yerel & bağlı**  >  **depolama hesapları**  >  **öykünücüsü-varsayılan bağlantı noktaları** düğümünü kullanabilirsiniz.

Bağlantınız için farklı bir ad kullanmak istiyorsanız veya öykünücü varsayılan bağlantı noktalarında çalışmıyorsa:

1. Öykünücüyü başlatın.

   > [!IMPORTANT]
   > Depolama Gezgini öykünücüyü otomatik olarak başlatmıyor. El ile başlatmanız gerekir.

1. **Azure depolama 'Ya Bağlan** Iletişim kutusunun **Kaynak Seç** panelinde **yerel depolama öykünücüsü**' nü seçin.
1. Bağlantınız için bir görünen ad ve kullanmak istediğiniz her bir öykünülmüş hizmet için bağlantı noktası numarası girin. Hizmetini kullanmak istemiyorsanız, karşılık gelen bağlantı noktasını boş bırakın. **İleri**’yi seçin.
1. **Özet** panelinde bağlantı bilgilerinizi gözden geçirin. Bağlantı bilgileri doğruysa, **Bağlan**' ı seçin.

### <a name="connect-to-azure-cosmos-db"></a>Azure Cosmos DB’ye bağlanma

Depolama Gezgini Ayrıca Azure Cosmos DB kaynaklara bağlanmayı destekler.

#### <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Bağlantı dizesi kullanarak bir Azure Cosmos DB hesabına bağlanma

Bir Azure aboneliği aracılığıyla Azure Cosmos DB hesaplarını yönetmek yerine, bir bağlantı dizesi kullanarak Azure Cosmos DB bağlanabilirsiniz. Bağlanmak için şu adımları uygulayın:

1. **Gezgin** altında, **eklenen yerel &**' i genişletin, **Cosmos DB hesaplar**' a sağ tıklayın ve Azure Cosmos DB ' **ye Bağlan**' ı seçin

    ![Bağlantı dizesiyle Azure Cosmos DB’ye bağlanma][21]

1. Azure Cosmos DB API 'sini seçin, **bağlantı dizesi** verilerinizi girin ve sonra Azure Cosmos DB hesabını bağlamak için **Tamam** ' ı seçin. Bağlantı dizesini alma hakkında daha fazla bilgi için bkz. [Azure Cosmos hesabını yönetme](./cosmos-db/how-to-manage-database-account.md).

    ![Bağlantı dizesi][22]

#### <a name="connect-to-azure-data-lake-store-by-uri"></a>URI 'ye göre Azure Data Lake Store bağlanma

Aboneliğinizde olmayan bir kaynağa erişebilirsiniz. Size Kaynak URI 'sini sağlamak için bu kaynağa erişimi olan bir kişi olması gerekir. Oturum açtıktan sonra, URI 'yi kullanarak Data Lake Store bağlanın. Bağlanmak için şu adımları uygulayın:

1. **Gezgin** altında, **Yerel & ekli**' ı genişletin.

1. **Data Lake Storage 1.** öğesine sağ tıklayın ve **Data Lake Storage 1. Bağlan**' ı seçin.

    ![Data Lake Store bağlam menüsüne Bağlan](./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-explorer-connect-data-lake-storage.png)

1. URI 'yi girip **Tamam**' ı seçin. Data Lake Store **Data Lake Storage** altında görünür.

    ![Data Lake Store sonuca Bağlan](./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-explorer-attach-data-lake-finished.png)

Bu örnek Data Lake Storage 1. kullanır. Azure Data Lake Storage 2. artık kullanılabilir. Daha fazla bilgi için bkz. [Azure Data Lake Storage 1. nedir?](./data-lake-store/data-lake-store-overview.md).

## <a name="generate-a-shared-access-signature-in-storage-explorer"></a>Depolama Gezgini paylaşılan erişim imzası oluşturma<a name="generate-a-sas-in-storage-explorer"></a>

### <a name="account-level-shared-access-signature"></a>Hesap düzeyinde paylaşılan erişim imzası

1. Paylaşmak istediğiniz depolama hesabına sağ tıklayın ve ardından **paylaşılan erişim Imzasını al**' ı seçin.

    ![Paylaşılan erişim imzasını al bağlam menü seçeneği][14]

1. **Paylaşılan erişim imzası**' nda, hesap için istediğiniz zaman çerçevesini ve Izinleri belirtip **Oluştur**' u seçin.

    ![Paylaşılan erişim imzası al][15]

1. **Bağlantı dizesini** veya ham **sorgu dizesini** panonuza kopyalayın.

### <a name="service-level-shared-access-signature"></a>Hizmet düzeyi paylaşılan erişim imzası

Hizmet düzeyinde paylaşılan erişim imzası alabilirsiniz. Daha fazla bilgi için bkz. [bir blob kapsayıcısı IÇIN SAS edinme](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container).

## <a name="search-for-storage-accounts"></a>Depolama hesapları arama

Bir depolama kaynağını bulmak için **Gezgin** bölmesinde arama yapabilirsiniz.

Arama kutusuna metin girerken Depolama Gezgini, bu noktaya kadar girdiğiniz arama değeriyle eşleşen tüm kaynakları görüntüler. Bu örnekte, **uç noktaları** için arama gösterilmektedir:

![Depolama hesabı araması][23]

> [!NOTE]
> Aramanızı hızlandırmak için, aradığınız öğeyi içermeyen aboneliklerin seçimini kaldırmak için **Hesap yönetimi** 'ni kullanın. Ayrıca, bir düğüme sağ tıklayıp belirli bir düğümden aramayı başlatmak için **buradan ara** ' yı seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama Gezgini ile Azure Blob depolama kaynaklarını yönetme](vs-azure-tools-storage-explorer-blobs.md)
* [Azure Depolama Gezgini'ni kullanarak verilerle çalışma](./cosmos-db/storage-explorer.md)
* [Depolama Gezgini ile Azure Data Lake Store kaynaklarını yönetme](./data-lake-store/data-lake-store-in-storage-explorer.md)

[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-shared-access-signature-for-storage-explorer.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/create-shared-access-signature-for-storage-explorer.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-cosmos-db-by-connection-string.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connection-string-for-cosmos-db.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-explorer-search-for-resource.png
