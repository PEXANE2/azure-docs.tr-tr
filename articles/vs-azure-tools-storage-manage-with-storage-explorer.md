---
title: Depolama Gezgini ile başlayın | Microsoft Dokümanlar
description: Depolama Gezgini ile Azure depolama kaynaklarını yönetme
services: storage
author: cawaMS
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.date: 11/08/2019
ms.author: cawa
ms.openlocfilehash: 7886d5a1ad0745550767b7d6f19592ca3c84b00a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279799"
---
# <a name="get-started-with-storage-explorer"></a>Depolama Gezgini ile çalışmaya başlama

## <a name="overview"></a>Genel Bakış

Microsoft Azure Depolama Gezgini, Windows, macOS ve Linux'taki Azure Depolama verileriyle çalışmayı kolaylaştıran bağımsız bir uygulamadır. Bu makalede, Azure depolama alan hesaplarınıza bağlanmanın ve yönetmenin çeşitli yollarını öğreneceksiniz.

![Microsoft Azure Depolama Gezgini][0]

## <a name="prerequisites"></a>Ön koşullar

# <a name="windows"></a>[Windows](#tab/windows)

Windows desteği Depolama Gezgini'nin aşağıdaki sürümleri:

* Windows 10 (önerilir)
* Windows 8
* Windows 7

Windows'un tüm sürümleri için Depolama Gezgini .NET Framework 4.6.2 veya sonraki sürümlerini gerektirir.

# <a name="macos"></a>[Macos](#tab/macos)

macOS'un aşağıdaki sürümleri Depolama Gezgini'ni destekler:

* macOS 10.12 Sierra ve sonraki sürümleri

# <a name="linux"></a>[Linux](#tab/linux)

Storage Explorer, Linux'un en yaygın dağıtımları için [Snap Store'da](https://snapcraft.io/storage-explorer) kullanılabilir. Bu yükleme için Snap Store'u öneririz. Depolama Gezgini snap, yeni sürümler Snap Store'da yayınlandığında tüm bağımlılıklarını ve güncelleştirmelerini yükler.

Desteklenen dağıtımlar [için, tutturan yükleme sayfasına](https://snapcraft.io/docs/installing-snapd)bakın.

Depolama Gezgini bir parola yöneticisi nin kullanılmasını gerektirir. Parola yöneticisine el ile bağlanmanız gerekebilir. Aşağıdaki komutu çalıştırarak Storage Explorer'ı sisteminizin parola yöneticisine bağlayabilirsiniz:

```bash
snap connect storage-explorer:password-manager-service :password-manager-service
```

Depolama Explorer da *.tar.gz* indirme olarak kullanılabilir. Bağımlılıkları el ile yüklemeniz gerekir. Linux desteği *.tar.gz* kurulumu aşağıdaki dağıtımları:

* Ubuntu 18,04 x64 x
* Ubuntu 16,04 x64 x
* Ubuntu 14,04 x64 x

*.tar.gz* yüklemesi diğer dağıtımlarda çalışabilir, ancak yalnızca listelenenler resmi olarak desteklenir.

Linux'a Depolama Gezgini yüklemekonusunda daha fazla yardım için Azure Depolama Gezgini sorun giderme kılavuzundaki [Linux bağımlılıklarına](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting#linux-dependencies) bakın.

---

## <a name="download-and-install"></a>İndirme ve yükleme

Depolama Gezgini'ni indirmek ve yüklemek için [Azure Depolama Gezgini'ne](https://www.storageexplorer.com)bakın.

## <a name="connect-to-a-storage-account-or-service"></a>Bir depolama hesabı veya hizmetine bağlanmak

Depolama Gezgini depolama hesaplarına bağlamak için birçok yol sağlar. Genel olarak ya yapabilirsiniz:

* [Aboneliklerinize ve kaynaklarına erişmek için Azure'da oturum açın](#sign-in-to-azure)
* [Belirli bir Depolama veya CosmosDB kaynağı ekleme](#attach-a-specific-resource)

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

> [!NOTE]
> Oturum açtıktan sonra kaynaklara tam olarak erişmek için Storage Explorer'ın hem yönetim (Azure Kaynak Yöneticisi) hem de veri katmanı izinleri gerekir. Bu, depolama hesabınıza, hesaptaki kapsayıcılara ve kapsayıcılarda bulunan verilere erişim sağlayan Azure Etkin Dizin (Azure AD) izinlerine ihtiyacınız olduğu anlamına gelir. Yalnızca veri katmanında izinlerin varsa, [Azure AD aracılığıyla bir kaynak eklemeyi](#add-a-resource-via-azure-ad)düşünün. Depolama Gezgini'nin gerektirdiği belirli izinler hakkında daha fazla bilgi için [Azure Depolama Gezgini sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting#rbac-permissions-issues)bakın.

1. Depolama Gezgini'nde**Hesap Yönetimini** **Görüntüle'yi** > seçin veya **Hesapları Yönet** düğmesini seçin.

    ![Hesapları Yönet][1]

1. **ACCOUNT MANAGEMENT** artık oturum açmış olduğunuz tüm Azure hesaplarını görüntüler. Başka bir hesaba bağlanmak için **hesap ekle'yi**seçin.

1. **Azure Depolamaya**Bağlan'da, ulusal bir bulutta veya Azure Yığınında oturum açabilmek için **Azure ortamından** bir Azure bulutu seçin. Ortamınızı seçtikten sonra **İleri'yi**seçin.

    ![Oturum açma seçeneği][2]

    Depolama Gezgini oturum açmanız için bir sayfa açar. Daha fazla bilgi için [bkz.](/azure-stack/user/azure-stack-storage-connect-se)

1. Bir Azure hesabında başarılı bir şekilde oturum imzaladıktan sonra, hesap ve bu hesapla ilişkili Azure abonelikleri **ACCOUNT MANAGEMENT**altında görünür. Seçiminizi listelenen Azure aboneliklerinin tümü veya hiçbiri arasında geçiş yapmak için **Tüm abonelikleri** seçin. Birlikte çalışmak istediğiniz Azure aboneliklerini seçin ve ardından **Uygula**’yı seçin.

    ![Azure aboneliklerini seçme][3]

    **EXPLORER,** seçili Azure abonelikleriyle ilişkili depolama hesaplarını görüntüler.

    ![Seçili Azure abonelikleri][4]

### <a name="attach-a-specific-resource"></a>Belirli bir kaynak ekleme

Depolama Gezgini'nde bir kaynağa eklemenin birkaç yolu vardır:

* [Azure AD üzerinden kaynak ekleyin.](#add-a-resource-via-azure-ad) Yalnızca veri katmanında izinlerin varsa, blob kapsayıcısı veya Azure Veri Gölü Depolama Gen2 Blob depolama kapsayıcısı eklemek için bu seçeneği kullanın.
* [Bağlantı dizesi kullanın.](#use-a-connection-string) Depolama hesabına bağlantı dizeniz varsa bu seçeneği kullanın. Depolama Gezgini hem anahtar hem de [paylaşılan erişim imza](storage/common/storage-dotnet-shared-access-signature-part-1.md) bağlantı dizelerini destekler.
* [Paylaşılan erişim imzası URI kullanın.](#use-a-shared-access-signature-uri) Blob kapsayıcısına, dosya paylaşımına, sıraya veya tabloya [paylaşılan bir erişim imzası URI'niz](storage/common/storage-dotnet-shared-access-signature-part-1.md) varsa, kaynağa eklemek için kullanın. Paylaşılan erişim imzası URI'yi elde etmek için [Storage Explorer'ı](#generate-a-sas-in-storage-explorer) veya [Azure portalını](https://portal.azure.com)kullanabilirsiniz.
* [Bir ad ve anahtar kullanın.](#use-a-name-and-key) Depolama hesabınızdaki hesap anahtarlarından birini biliyorsanız, hızlı bir şekilde bağlanmak için bu seçeneği kullanabilirsiniz. [Azure portalında](https://portal.azure.com) **Ayarlar** > **Erişim anahtarlarını** seçerek anahtarlarınızı depolama hesabı sayfasında bulun.
* [Yerel bir emülatöre takın.](#attach-to-a-local-emulator) Kullanılabilir Azure Depolama emülatörlerinden birini kullanıyorsanız, emülatörünüze kolayca bağlanmak için bu seçeneği kullanın.
* [Bir bağlantı dizesini kullanarak Azure Cosmos DB hesabına bağlanın.](#connect-to-an-azure-cosmos-db-account-by-using-a-connection-string) CosmosDB örneğine bağlantı dizeniz varsa bu seçeneği kullanın.
* [URI tarafından Azure Veri Gölü Deposu'na bağlanın.](#connect-to-azure-data-lake-store-by-uri) Uri to Azure Data Lake Store'unvarsa bu seçeneği kullanın.

#### <a name="add-a-resource-via-azure-ad"></a>Azure AD ile kaynak ekleme

1. **Azure Depolamaya Bağlan'ı**açmak için **Bağlan** simgesini seçin.

    ![Azure Storage’a bağlanma seçeneği][9]

1. Henüz yapmadıysanız, kaynağa erişimi olan Azure hesabında oturum aç'ta oturum **aç** seçeneğini kullanın. Oturum açmadıktan sonra **Azure Depolama'ya bağlan'a**dönün.

1. **Azure Etkin Dizini (Azure AD) aracılığıyla kaynak ekle'yi**seçin ve ardından **İleri'yi**seçin.

1. Bir Azure hesabı ve kiracı seçin. Bu değerler, eklemek istediğiniz Depolama kaynağına erişebilmeli. **Sonraki'ni**seçin.

1. Eklemek istediğiniz kaynak türünü seçin. Bağlanmak için gereken bilgileri girin. 

   Bu sayfaya girdiğiniz bilgiler, ne tür kaynak eklediğinize bağlıdır. Doğru kaynak türünü seçtiğinizden emin olun. Gerekli bilgileri girdikten sonra **İleri'yi**seçin.

1. Tüm bilgilerin doğru olduğundan emin olmak için **Bağlantı Özetini** gözden geçirin. Eğer varsa, **Bağlan'ı**seçin. Aksi takdirde, yanlış bilgileri düzeltmek için önceki sayfalara dönmek için **Geri'yi** seçin.

Bağlantı başarıyla eklendikten sonra, kaynak ağacı bağlantıyı temsil eden düğüme gider. Kaynak **Yerel & Ekli** > **Depolama Hesapları** > **(Ekli Kapsayıcılar)** > **Blob Kapları**altında görünür. Depolama Gezgini bağlantınızı ekleyemiyorsa veya bağlantıyı başarıyla ekledikten sonra verilerinize erişemiyorsanız, [Azure Depolama Gezgini sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting)bakın.

#### <a name="use-a-connection-string"></a>Bağlantı dizesi kullanma

1. **Azure Depolamaya Bağlan'ı**açmak için **Bağlan** simgesini seçin.

    ![Azure Storage’a bağlanma seçeneği][9]

1. **Bağlantı dizesi kullan'ı**seçin ve sonra **İleri'yi**seçin.

1. Bağlantınız için bir görüntü adı seçin ve bağlantı dizenizi girin. Ardından **İleri'yi**seçin.

1. Tüm bilgilerin doğru olduğundan emin olmak için **Bağlantı Özetini** gözden geçirin. Eğer varsa, **Bağlan'ı**seçin. Aksi takdirde, yanlış bilgileri düzeltmek için önceki sayfalara dönmek için **Geri'yi** seçin.

Bağlantı başarıyla eklendikten sonra, kaynak ağacı bağlantıyı temsil eden düğüme gider. Kaynak Yerel **& Ekli** > **Depolama Hesapları**altında görünür. Depolama Gezgini bağlantınızı ekleyemiyorsa veya bağlantıyı başarıyla ekledikten sonra verilerinize erişemiyorsanız, [Azure Depolama Gezgini sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting)bakın.

#### <a name="use-a-shared-access-signature-uri"></a>Paylaşılan erişim imzası URI kullanma

1. **Azure Depolamaya Bağlan'ı**açmak için **Bağlan** simgesini seçin.

    ![Azure Storage’a bağlanma seçeneği][9]

1. **Paylaşılan erişim imzası (SAS) URI'yi kullan'ı**seçin ve ardından **İleri'yi**seçin.

1. Bağlantınız için bir görüntü adı seçin ve paylaşılan erişim imzanız URI'yi girin. Eklediğiniz kaynak türü için hizmet bitiş noktası otomatik doldurma gerekir. Özel bir bitiş noktası kullanıyorsanız, bu olmayabilir. **Sonraki'ni**seçin.

1. Tüm bilgilerin doğru olduğundan emin olmak için **Bağlantı Özetini** gözden geçirin. Eğer varsa, **Bağlan'ı**seçin. Aksi takdirde, yanlış bilgileri düzeltmek için önceki sayfalara dönmek için **Geri'yi** seçin.

Bağlantı başarıyla eklendikten sonra, kaynak ağacı bağlantıyı temsil eden düğüme gider. Kaynak, Yerel **& Ekli** > **Depolama Hesapları** > **(Ekli Kapsayıcılar)** > altında,*iliştirdiğiniz kapsayıcı türüne ait hizmet düğümünde*görünür. Depolama Gezgini bağlantınızı ekleyemiyorsa, [Azure Depolama Gezgini sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting)bakın. Bağlantıyı başarıyla ekledikten sonra verilerinize erişemiyorsanız sorun giderme kılavuzuna bakın.

#### <a name="use-a-name-and-key"></a>Ad ve anahtar kullanma

1. **Azure Depolamaya Bağlan'ı**açmak için **Bağlan** simgesini seçin.

    ![Azure Storage’a bağlanma seçeneği][9]

1. **Bir depolama hesabı adı ve anahtarı kullan'ı seçin**ve sonra **İleri'yi**seçin.

1. Bağlantınız için bir görüntü adı seçin.

1. Depolama hesabı adınızı ve erişim anahtarlarından birini girin.

1. Kullanılacak **Depolama etki alanını** seçin ve ardından **İleri'yi**seçin.

1. Tüm bilgilerin doğru olduğundan emin olmak için **Bağlantı Özetini** gözden geçirin. Eğer varsa, **Bağlan'ı**seçin. Aksi takdirde, yanlış bilgileri düzeltmek için önceki sayfalara dönmek için **Geri'yi** seçin.

Bağlantı başarıyla eklendikten sonra, kaynak ağacı bağlantıyı temsil eden düğüme gider. Kaynak Yerel **& Ekli** > **Depolama Hesapları**altında görünür. Depolama Gezgini bağlantınızı ekleyemiyorsa veya bağlantıyı başarıyla ekledikten sonra verilerinize erişemiyorsanız, [Azure Depolama Gezgini sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting)bakın.

#### <a name="attach-to-a-local-emulator"></a>Yerel bir emülatöre ekleme

Depolama Gezgini şu anda iki resmi Depolama emülatörlerini destekler:

* [Azure Depolama emülatörü](storage/common/storage-use-emulator.md) (yalnızca Windows)
* [Azurite](https://github.com/azure/azurite) (Windows, macOS veya Linux)

Emülatörünüzüz varsayılan bağlantı noktalarında dinliyorsa, emülatöre erişmek için **Emülatör - Varsayılan Bağlantı Noktaları** düğümunu kullanabilirsiniz. **Emülatör** arayın - **Yerel & Ekli** > **Depolama Hesapları**altında Varsayılan Bağlantı Noktaları.

Bağlantınız için farklı bir ad kullanmak istiyorsanız veya emülatörünüzün varsayılan bağlantı noktalarında çalışmıyorsa aşağıdaki adımları izleyin:

1. Emülatörünüzü başlatın. Her hizmet `AzureStorageEmulator.exe status` türü için bağlantı noktalarını görüntülemek için komutu girin.

   > [!IMPORTANT]
   > Depolama Gezgini emülatörünüzü otomatik olarak çalıştırmaz. Elle başlamalısın.

1. **Azure Depolamaya Bağlan'ı**açmak için **Bağlan** simgesini seçin.

    ![Azure Storage’a bağlanma seçeneği][9]

1. **Yerel bir emülatöre ekle'yi**seçin ve sonra **İleri'yi**seçin.

1. Bağlantınız için bir ekran adı seçin ve emülatörünüzün her hizmet türü için dinlediği bağlantı noktalarını girin. **Yerel Emülatöre ekle,** çoğu emülatör için varsayılan bağlantı noktası değerlerini önerir. **Resmi** emülatörlerin hiçbiri şu anda Dosyalar hizmetini desteklemediği için dosya bağlantı noktası boştur. Kullandığınız emülatör Dosyaları destekliyorsa, kullanmak üzere bağlantı noktasına girebilirsiniz. Ardından **İleri'yi**seçin.

1. Bağlantı **Özetini** gözden geçirin ve tüm bilgilerin doğru olduğundan emin olun. Eğer varsa, **Bağlan'ı**seçin. Aksi takdirde, yanlış bilgileri düzeltmek için önceki sayfalara dönmek için **Geri'yi** seçin.

Bağlantı başarıyla eklendikten sonra, kaynak ağacı bağlantıyı temsil eden düğüme gider. Düğüm Yerel & **Ekli** > **Depolama Hesapları**altında görünmelidir. Depolama Gezgini bağlantınızı ekleyemiyorsa veya bağlantıyı başarıyla ekledikten sonra verilerinize erişemiyorsanız, [Azure Depolama Gezgini sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting)bakın.

#### <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Bağlantı dizesini kullanarak Azure Cosmos DB hesabına bağlanma

Azure aboneliği aracılığıyla Azure Cosmos DB hesaplarını yönetmek yerine, bir bağlantı dizesini kullanarak Azure Cosmos DB'ye bağlanabilirsiniz. Bağlanmak için şu adımları uygulayın:

1. **EXPLORER**altında, **Yerel & Ekli'ni**genişletin, **Cosmos DB Hesapları'na**sağ tıklayın ve Azure **Cosmos DB'ye Bağlan'ı**seçin.

    ![Bağlantı dizesiyle Azure Cosmos DB’ye bağlanma][21]

1. Azure Cosmos DB API'sini seçin, **Bağlantı String** verilerinizi girin ve azure Cosmos DB hesabını bağlamak için **Tamam'ı** seçin. Bağlantı dizesini nasıl alınız hakkında bilgi için [bir Azure Cosmos hesabını yönet'e](https://docs.microsoft.com/azure/cosmos-db/manage-account)bakın.

    ![Bağlantı dizesi][22]

#### <a name="connect-to-azure-data-lake-store-by-uri"></a>URI'ye göre Azure Veri Gölü Deposu'na bağlanın

Aboneliğinizde olmayan bir kaynağa erişebilirsiniz. Size uri kaynağını vermek için bu kaynağa erişimi olan birine ihtiyacınız vardır. Oturum açtıktan sonra URI'yi kullanarak Data Lake Store'a bağlanın. Bağlanmak için şu adımları uygulayın:

1. **EXPLORER**altında, **Yerel & Ekli**genişletmek.

1. Data **Lake Storage Gen1'e**sağ tıklayın ve **Veri Gölü Depolama Gen1'e Bağlan'ı**seçin.

    ![Data Lake Store bağlam menüsüne bağlanın](./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-explorer-connect-data-lake-storage.png)

1. URI'yi girin ve ardından **Tamam'ı**seçin. Veri Gölü Deposu'nuz **Veri Gölü Depolama'nın**altında görünür.

    ![Data Lake Store sonucuna bağlanın](./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-explorer-attach-data-lake-finished.png)

Bu örnekte Veri Gölü Depolama Gen1 kullanır. Azure Veri Gölü Depolama Gen2 artık kullanılabilir. Daha fazla bilgi için [Bkz. Azure Veri Gölü Depolama Gen1 nedir.](./data-lake-store/data-lake-store-overview.md)

## <a name="generate-a-shared-access-signature-in-storage-explorer"></a>Depolama Gezgini'nde paylaşılan erişim imzası oluşturma<a name="generate-a-sas-in-storage-explorer"></a>

### <a name="account-level-shared-access-signature"></a>Hesap düzeyi paylaşılan erişim imzası

1. Paylaşmak istediğiniz depolama hesabına sağ tıklayın ve ardından **Paylaşılan Erişim İmzasını Al'ı**seçin.

    ![Paylaşılan erişim imza bağlamı menüsü seçeneğini alın][14]

1. Paylaşılan Erişim İmzası'nda, hesap için istediğiniz zaman dilimini ve izinleri belirtin ve ardından **Oluştur'u**seçin. **Shared Access Signature**

    ![Paylaşılan erişim imzası alma][15]

1. **Bağlantı dizesini** veya ham **Sorgu dizesini** panonuza kopyalayın.

### <a name="service-level-shared-access-signature"></a>Hizmet düzeyi paylaşılan erişim imzası

Hizmet düzeyinde paylaşılan bir erişim imzası alabilirsiniz. Daha fazla bilgi için bir [blob kapsayıcı için SAS alın](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)bakın.

## <a name="search-for-storage-accounts"></a>Depolama hesapları arama

Depolama kaynağı bulmak için **EXPLORER** bölmesinde arama yapabilirsiniz.

Arama kutusuna metin girdiğinizde, Depolama Gezgini o noktaya kadar girdiğiniz arama değeriyle eşleşen tüm kaynakları görüntüler. Bu örnek, **uç noktaları**için bir arama gösterir:

![Depolama hesabı araması][23]

> [!NOTE]
> Aramanızı hızlandırmak için, aradığınız öğeyi içermeyen aboneliklerin select'ini seçmek için **Hesap Yönetimi'ni** kullanın. Ayrıca bir düğümü sağ tıklatabilir ve belirli bir düğümden aramaya başlamak için **Buradan Ara'yı** seçebilirsiniz.
>
>

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama Gezgini ile Azure Blob depolama kaynaklarını yönetme](vs-azure-tools-storage-explorer-blobs.md)
* [Azure Depolama Gezgini'ni kullanarak verilerle çalışma](./cosmos-db/storage-explorer.md)
* [Depolama Gezgini ile Azure Veri Gölü Deposu kaynaklarını yönetme](./data-lake-store/data-lake-store-in-storage-explorer.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Overview.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ManageAccounts.png
[2]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-azure-environment.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/account-panel-subscriptions-apply.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SubscriptionNode.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog.png
[7]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/PortalAccessKeys.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccessKeys.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-AddWithKeySelected.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-NameAndKeyPage.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount-Detach.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-shared-access-signature-for-storage-explorer.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/create-shared-access-signature-for-storage-explorer.png
[16]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-WithConnStringOrSASSelected.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-1.png
[18]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithSASAccount.png
[19]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-2.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ServiceAttachedWithSAS.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-cosmos-db-by-connection-string.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connection-string-for-cosmos-db.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-explorer-search-for-resource.png
