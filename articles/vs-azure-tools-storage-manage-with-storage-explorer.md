---
title: Depolama Gezgini kullanmaya başlayın | Microsoft Docs
description: Depolama Gezgini ile Azure depolama kaynaklarını yönetme
services: storage
author: cawaMS
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.date: 04/22/2019
ms.author: cawa
ms.openlocfilehash: 2b923a381be9d6cdb1a83f582a541c9e17e117a4
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71086640"
---
# <a name="get-started-with-storage-explorer"></a>Depolama Gezgini kullanmaya başlayın

## <a name="overview"></a>Genel Bakış

Microsoft Azure Depolama Gezgini, Windows, macOS ve Linux 'ta Azure Depolama verileriyle çalışmayı kolaylaştıran tek başına bir uygulamadır. Bu makalede, Azure depolama hesaplarınızı bağlamak ve yönetmek için kullanabileceğiniz çeşitli yollar öğreneceksiniz.

![Microsoft Azure Depolama Gezgini][0]

## <a name="prerequisites"></a>Önkoşullar

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Depolama Gezgini, Windows 'un aşağıdaki sürümlerinde desteklenir:

* Windows 10 (önerilir)
* Windows 8
* Windows 7

Tüm Windows sürümleri için, .NET Framework 4.6.2 veya üzeri gereklidir.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Depolama Gezgini, macOS 'un aşağıdaki sürümlerinde desteklenir:

* macOS 10,12 "Sierra" ve sonraki sürümleri

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Depolama Gezgini, en yaygın Linux dağıtımları için [Snap deposunda](https://snapcraft.io/storage-explorer) kullanılabilir ve önerilen yükleme yöntemidir. Depolama Gezgini Snap, yeni sürümler Snap Store 'da yayımlandığında tüm bağımlılıklarını ve güncelleştirmelerini otomatik olarak kurar.

Desteklenen dağıtımların listesi için, [snapd yükleme sayfasına](https://snapcraft.io/docs/installing-snapd)gidin.

Depolama Gezgini, Depolama Gezgini doğru çalışmadan önce el ile bağlanmanız gerekebilecek bir parola yöneticisinin kullanılmasını gerektirir. Aşağıdaki komutu çalıştırarak, Depolama Gezgini sisteminizin parola yöneticisine bağlayabilirsiniz:

```bash
snap connect storage-explorer:password-manager-service :password-manager-service
```

Depolama Gezgini. tar. gz indirme olarak da kullanılabilir, ancak bağımlılıkları el ile yüklemek zorunda olacaksınız. . Tar. gz yüklemesi, Linux 'un aşağıdaki dağıtımlarında desteklenir:

* Ubuntu 18,04 x64
* Ubuntu 16,04 x64
* Ubuntu 14,04 x64

. Tar. gz yüklemesi diğer dağıtımlar üzerinde çalışabilir, ancak yalnızca bu listelenmiş olanlar resmi olarak desteklenmektedir.

Linux üzerinde Depolama Gezgini yükleme hakkında daha fazla yardım için bkz. [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting#linux-dependencies).

---

## <a name="download-and-install"></a>İndir ve yükle

[Depolama Gezgini’ni indirip yükleme](https://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Bir depolama hesabı veya hizmetine bağlanmak

Depolama Gezgini depolama hesaplarına bağlamak için birçok yol sağlar. Genel olarak şunlardan birini yapabilirsiniz:

* [Aboneliklerinize ve kaynaklarına erişmek için Azure 'da oturum açın](#sign-in-to-azure)
* [Belirli bir depolama alanı veya CosmosDB kaynağı iliştirme](#attach-a-specific-resource)

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

> [!NOTE]
> Oturum açtıktan sonra kaynakları tam olarak erişmek için Depolama Gezgini hem yönetim (Azure Resource Manager) hem de veri katmanı izinleri gerektirir. Bu, depolama hesabınıza, hesaptaki kapsayıcılara ve kapsayıcılardaki verilere erişmenizi sağlayan Azure Active Directory (Azure AD) izinlerinin olması gerektiği anlamına gelir. Yalnızca veri katmanında izinleriniz varsa, [Azure AD aracılığıyla bir kaynak eklemeyi](#add-a-resource-via-azure-ad)düşünün. Depolama Gezgini gerekli olan belirli izinler hakkında daha fazla bilgi için bkz. [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#role-based-access-control-permission-issues).

1. Depolama Gezgini, hesap yönetimi paneline gitmek için **hesapları Yönet** ' i seçin.

    ![Hesapları Yönet][1]

2. Sol bölmede artık oturum açmış olduğunuz tüm Azure hesapları görüntülenir. Başka bir hesaba bağlanmak için **Hesap Ekle**' yi seçin.

3. Ulusal bir bulutta veya bir Azure Stack oturum açmak istiyorsanız, kullanmak istediğiniz Azure bulutunu seçmek için **Azure ortamı** açılır listesini seçin. Ortamınızı seçtikten sonra **oturum aç** düğmesini seçin. Daha fazla bilgi için bkz. [Azure Stack aboneliğine bağlanma Depolama Gezgini](/azure-stack/user/azure-stack-storage-connect-se).

    ![Oturum açma seçeneği][2]

4. Azure hesabıyla başarıyla oturum açtıktan sonra, bu hesapla ilişkili hesap ve Azure abonelikleri sol bölmeye eklenir. Birlikte çalışmak istediğiniz Azure aboneliklerini seçin ve ardından **Uygula**’yı seçin. ( **Tüm abonelikler** seçildiğinde, listede listelenen Azure aboneliklerinin tümü veya hiçbiri arasında seçim yapılır.)

    ![Azure aboneliklerini seçme][3]

    Sol bölmede seçili Azure abonelikleriyle ilişkili depolama hesapları gösterilir.

    ![Seçili Azure abonelikleri][4]

### <a name="attach-a-specific-resource"></a>Belirli bir kaynağı iliştirme

Depolama Gezgini bir kaynağa eklemenin birden çok yolu vardır:

* [Azure AD aracılığıyla bir kaynak ekleyin](#add-a-resource-via-azure-ad). Yalnızca veri katmanında izinleriniz varsa, bir blob kapsayıcısı veya Azure Data Lake Storage 2. blob depolama kapsayıcısı eklemek için bu seçeneği kullanın.
* [Bir bağlantı dizesi kullanın](#use-a-connection-string). Depolama hesabına yönelik bir bağlantı dizeniz varsa bu seçeneği kullanın. Depolama Gezgini hem anahtar hem de [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) bağlantı dizelerini destekler.
* [SAS URI 'Si kullanın](#use-a-sas-uri). Bir blob kapsayıcısına, dosya paylaşımında, kuyruğa veya tabloya bir [SAS URI 'si](storage/common/storage-dotnet-shared-access-signature-part-1.md) varsa, kaynağa eklemek için bunu kullanın. SAS URI 'sini almak için [Depolama Gezgini](#generate-a-sas-in-storage-explorer) veya [Azure Portal](https://portal.azure.com)kullanabilirsiniz.
* [Bir ad ve anahtar kullanın](#use-a-name-and-key). Depolama hesabınızda hesap anahtarlarından birini biliyorsanız, bu seçeneği kullanarak hızlı bir şekilde bağlantı oluşturabilirsiniz. Depolama hesabınızın anahtarları [Azure Portal](https://portal.azure.com)depolama hesabı **erişim anahtarları** panelinde bulunur.
* [Yerel bir öykünücüye ekleyin](#attach-to-a-local-emulator). Kullanılabilir Azure Storage öykünücülerinden birini kullanıyorsanız, öykünücüye kolayca bağlanmak için bu seçeneği kullanın.
* [Bir bağlantı dizesi kullanarak bir Azure Cosmos DB hesabına bağlanın](#connect-to-an-azure-cosmos-db-account-by-using-a-connection-string). CosmosDB örneğine bir bağlantı dizeniz varsa bu seçeneği kullanın.
* [URI 'ye göre Azure Data Lake Store bağlanın](#connect-to-azure-data-lake-store-by-uri). Azure Data Lake Store URI 'SI varsa bu seçeneği kullanın.

#### <a name="add-a-resource-via-azure-ad"></a>Azure AD aracılığıyla kaynak ekleme

1. Sol taraftaki dikey araç çubuğunda **Bağlan** düğmesini seçerek **Bağlan** iletişim kutusunu açın:

    ![Azure Storage’a bağlanma seçeneği][9]

2. Daha önce yapmadıysanız, kaynağa erişimi olan Azure hesabı 'nda oturum açmak için **Azure hesabı ekle** seçeneğini kullanın. Oturum açtıktan sonra **Bağlan** iletişim kutusuna geri dönün.

3. **Azure Active Directory (Azure AD) aracılığıyla Kaynak Ekle**' yi seçin ve ardından **İleri**' yi seçin.

4. Eklemek istediğiniz depolama kaynağına erişimi olan Azure hesabı ve kiracıyı seçin. **İleri**’yi seçin.

5. Eklemek istediğiniz kaynak türünü seçin ve ardından bağlanmak için gereken bilgileri girin. Bu sayfaya girdiğiniz bilgiler, eklediğiniz kaynak türüne bağlıdır. Doğru kaynak türünü seçtiğinizden emin olun. Gerekli bilgileri girdikten sonra **İleri**' yi seçin.

6. Bağlantı özetini gözden geçirin ve tüm bilgilerin doğru olduğundan emin olun. Varsa, **Bağlan**' ı seçin. Aksi takdirde, yanlış bilgileri onarmak üzere önceki sayfalara dönmek için **geri** düğmesini kullanın.

Bağlantı başarıyla eklendikten sonra kaynak ağacı otomatik olarak bağlantıyı temsil eden düğüme gider. Bu düğüme gitmezse, **Yerel & ekli** > **depolama hesapları** >  **(ekli kapsayıcılar)**  > **BLOB kapsayıcıları**' na bakın. Depolama Gezgini bağlantınızı ekleyemedik veya bağlantıyı başarıyla ekledikten sonra verilerinize erişemiyorsanız, [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting)'na bakın.

#### <a name="use-a-connection-string"></a>Bağlantı dizesi kullanın

1. Sol taraftaki dikey araç çubuğunda **Bağlan** düğmesini seçerek **Bağlan** iletişim kutusunu açın:

    ![Azure Storage’a bağlanma seçeneği][9]

2. **Bir bağlantı dizesi kullan**' ı seçin ve ardından **İleri**' yi seçin.

3. Bağlantınız için bir görünen ad seçin ve Bağlantı dizenizi girin. Sonra **İleri**’yi seçin.

4. Bağlantı özetini gözden geçirin ve tüm bilgilerin doğru olduğundan emin olun. Varsa, **Bağlan**' ı seçin. Aksi takdirde, yanlış bilgileri onarmak üzere önceki sayfalara dönmek için **geri** düğmesini kullanın.

Bağlantı başarıyla eklendikten sonra kaynak ağacı otomatik olarak bağlantıyı temsil eden düğüme gider. Bu düğüme gitmezse, **Yerel & ekli** > **depolama hesapları**bölümüne bakın. Depolama Gezgini bağlantınızı ekleyemedik veya bağlantıyı başarıyla ekledikten sonra verilerinize erişemiyorsanız, [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting)'na bakın.

#### <a name="use-a-sas-uri"></a>SAS URI 'SI kullan

1. Sol taraftaki dikey araç çubuğunda **Bağlan** düğmesini seçerek **Bağlan** iletişim kutusunu açın:

    ![Azure Storage’a bağlanma seçeneği][9]

2. **Paylaşılan erişim imzası (SAS) URI 'Si kullan**' ı seçin ve ardından **İleri**' yi seçin.

3. Bağlantınız için bir görünen ad seçin ve SAS URI 'nizi girin. İliştirmekte olduğunuz kaynak türü için hizmet uç noktası otomatik olarak doldurulur. Özel bir uç nokta kullanıyorsanız, bu mümkün olmayabilir. **İleri**’yi seçin.

4. Bağlantı özetini gözden geçirin ve tüm bilgilerin doğru olduğundan emin olun. Varsa, **Bağlan**' ı seçin. Aksi takdirde, yanlış bilgileri onarmak üzere önceki sayfalara dönmek için **geri** düğmesini kullanın.

Bağlantı başarıyla eklendikten sonra kaynak ağacı otomatik olarak bağlantıyı temsil eden düğüme gider. Bu düğüme gitmezse,*eklenen kapsayıcı türü için* **Yerel & bağlı** > **depolama hesapları** >  **(ekli kapsayıcılar)**  > hizmet düğümüne bakın. Depolama Gezgini bağlantınızı ekleyemedik veya bağlantıyı başarıyla ekledikten sonra verilerinize erişemiyorsanız, [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting)'na bakın.

#### <a name="use-a-name-and-key"></a>Ad ve anahtar kullan

1. Sol taraftaki dikey araç çubuğunda **Bağlan** düğmesini seçerek **Bağlan** iletişim kutusunu açın:

    ![Azure Storage’a bağlanma seçeneği][9]

2. **Depolama hesabı adı ve anahtarı kullan**' ı seçin ve ardından **İleri**' yi seçin.

3. Bağlantınız için bir görünen ad seçin.

4. Depolama hesabı adınızı ve erişim anahtarlarından birini girin.

5. Kullanılacak **depolama alanını** seçin ve ardından **İleri**' yi seçin.

6. Bağlantı özetini gözden geçirin ve tüm bilgilerin doğru olduğundan emin olun. Varsa, **Bağlan**' ı seçin. Aksi takdirde, yanlış bilgileri onarmak üzere önceki sayfalara dönmek için **geri** düğmesini kullanın.

Bağlantı başarıyla eklendikten sonra kaynak ağacı otomatik olarak bağlantıyı temsil eden düğüme gider. Bu düğüme gitmezse, **Yerel & ekli** > **depolama hesapları**bölümüne bakın. Depolama Gezgini bağlantınızı ekleyemedik veya bağlantıyı başarıyla ekledikten sonra verilerinize erişemiyorsanız, [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting)'na bakın.

#### <a name="attach-to-a-local-emulator"></a>Yerel öykünücüye iliştirme

Depolama Gezgini Şu anda iki adet resmi depolama öykünücüsünü desteklemektedir:
* [Azure depolama öykünücüsü](storage/common/storage-use-emulator.md) (Yalnızca Windows)
* [Azurite](https://github.com/azure/azurite) (Windows, macOS veya Linux)

Öykünücü, varsayılan bağlantı noktalarını dinliyorsa, öykünücüsünüzde hızlıca erişmek için **öykünücü-varsayılan bağlantı noktaları** düğümünü ( **Yerel & bağlı** > **depolama hesapları**altında bulunur) kullanabilirsiniz.

Bağlantınız için farklı bir ad kullanmak istiyorsanız veya öykünücü varsayılan bağlantı noktalarında çalışmıyorsa, şu adımları izleyin:

1. Öykünücüyü başlatın. Bunu yaptığınızda, her bir hizmet türü için öykünücü dinlediği bağlantı noktalarını unutmayın.

   > [!IMPORTANT]
   > Depolama Gezgini öykünücüyü otomatik olarak başlatmıyor. El ile başlatmanız gerekir.

2. Sol taraftaki dikey araç çubuğunda **Bağlan** düğmesini seçerek **Bağlan** iletişim kutusunu açın:

    ![Azure Storage’a bağlanma seçeneği][9]

3. **Yerel bir öykünücüye Ekle**' yi seçin ve ardından **İleri**' yi seçin.

4. Bağlantınız için bir görünen ad seçin ve her bir hizmet türü için öykünücünüzün dinlediği bağlantı noktalarını girin. Metin kutuları, çoğu Öykünücüde varsayılan bağlantı noktası değerleriyle başlayacaktır. **Dosya bağlantı noktası** kutusu, resmi öykünücülerinden hiçbiri şu anda dosyalar hizmetini desteklemediğinden boş bırakılır. Kullanmakta olduğunuz öykünücü dosyaları destekliyorsa, kullanılmakta olan bağlantı noktasını girebilirsiniz. Sonra **İleri**’yi seçin.

5. Bağlantı özetini gözden geçirin ve tüm bilgilerin doğru olduğundan emin olun. Varsa, **Bağlan**' ı seçin. Aksi takdirde, yanlış bilgileri onarmak üzere önceki sayfalara dönmek için **geri** düğmesini kullanın.

Bağlantı başarıyla eklendikten sonra kaynak ağacı otomatik olarak bağlantıyı temsil eden düğüme gider. Bu düğüme gitmezse, **Yerel & ekli** > **depolama hesapları**bölümüne bakın. Depolama Gezgini bağlantınızı ekleyemedik veya bağlantıyı başarıyla ekledikten sonra verilerinize erişemiyorsanız, [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting)'na bakın.

#### <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Bağlantı dizesi kullanarak bir Azure Cosmos DB hesabına bağlanma

Bir Azure aboneliği aracılığıyla Azure Cosmos DB hesaplarını yönetmeye alternatif olarak, bir bağlantı dizesi kullanarak Azure Cosmos DB da bağlanabilirsiniz. Bunu yapmak için şu adımları uygulayın:

1. Kaynak ağacının sol tarafında **yerel ve bağlı**' i genişletin, **Azure Cosmos DB hesaplar**' a sağ tıklayın ve **Azure Cosmos DB Bağlan**' ı seçin.

    ![Bağlantı dizesiyle Azure Cosmos DB’ye bağlanma][21]

2. Azure Cosmos DB API 'sini seçin, **bağlantı dizesi** verilerinizi girin ve sonra Azure Cosmos DB hesabını bağlamak için **Tamam** ' ı seçin. Bağlantı dizesinin nasıl alınacağı hakkında bilgi için bkz. [bağlantı dizesini alma](https://docs.microsoft.com/azure/cosmos-db/manage-account).

    ![Bağlantı dizesi][22]

#### <a name="connect-to-azure-data-lake-store-by-uri"></a>URI 'ye göre Azure Data Lake Store bağlanma

Aboneliğinizde olmayan bir kaynağa erişmek istiyorsanız, kaynak URI 'sini sağlamak için o kaynağa erişebilen birine ihtiyacınız vardır. Oturum açtıktan sonra, URI 'yi kullanarak Data Lake Store bağlanabilirsiniz. Bunu yapmak için şu adımları uygulayın:

1. Depolama Gezgini'ni açın.
2. Sol bölmede, **Yerel ve Ekli** öğesini genişletin.
3. **Data Lake Store**öğesine sağ tıklayın. Kısayol menüsünde **Data Lake Store Bağlan**' ı seçin.

    ![Data Lake Store bağlam menüsüne Bağlan](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach.png)

4. URI’yi girin. Araç, yeni girdiğiniz URL 'nin konumuna gider.

    ![Data Lake Store bağlamına Bağlan iletişim kutusu](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

    ![Data Lake Store sonuca Bağlan](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-attach-finish.png)


## <a name="generate-a-sas-in-storage-explorer"></a>Depolama Gezgini SAS oluşturma

### <a name="account-level-sas"></a>Hesap düzeyi SAS

1. Paylaşmak istediğiniz depolama hesabına sağ tıklayın ve ardından **paylaşılan erişim Imzasını al**' ı seçin.

    ![SAS alma içerik menüsü seçeneği][14]

2. **Paylaşılan erişim Imzası oluştur** iletişim kutusunda, hesap için istediğiniz zaman çerçevesini ve izinleri belirtin ve ardından **Oluştur**' u seçin.

    ![SAS Al iletişim kutusu][15]

3. Artık **bağlantı dizesini** veya ham **sorgu dizesini** panonuza kopyalayabilirsiniz.

### <a name="service-level-sas"></a>Hizmet düzeyi SAS

[Depolama Gezgini bir blob kapsayıcısı için SAS alma](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)

## <a name="search-for-storage-accounts"></a>Depolama hesapları arama

Bir depolama kaynağı bulmanız ve nerede olduğunu bilmiyorsanız, kaynağı aramak için sol bölmenin üst kısmındaki arama kutusunu kullanabilirsiniz.

Arama kutusuna yazarken sol bölmede Bu noktaya kadar girdiğiniz arama değeriyle eşleşen tüm kaynaklar görüntülenir. Örneğin, aşağıdaki ekran görüntüsünde **uç noktalar** için bir arama gösterilmektedir:

![Depolama hesabı araması][23]

> [!NOTE]
> Aramanızı hızlandırmak için, aradığınız öğeyi içermeyen aboneliklerin seçimini kaldırmak için hesap yönetimi panelini kullanın. Ayrıca, bir düğüme sağ tıklayıp belirli bir düğümden aramayı başlatmak için **buradan ara** ' yı seçebilirsiniz.
>
>

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama Gezgini ile Azure Blob depolama kaynaklarını yönetme](vs-azure-tools-storage-explorer-blobs.md)
* [Depolama Gezgini Azure Cosmos DB yönetme (Önizleme)](./cosmos-db/storage-explorer.md)
* [Depolama Gezgini ile Azure Data Lake Store kaynaklarını yönetme](./data-lake-store/data-lake-store-in-storage-explorer.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Overview.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ManageAccounts.png
[2]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-SignInSelected.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccountPanel.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SubscriptionNode.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog.png
[7]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/PortalAccessKeys.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccessKeys.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-AddWithKeySelected.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-NameAndKeyPage.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount-Detach.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/GetSharedAccessSignature.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SharedAccessSignatureDialog.png
[16]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-WithConnStringOrSASSelected.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-1.png
[18]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithSASAccount.png
[19]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-2.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ServiceAttachedWithSAS.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-db-by-connection-string.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connection-string.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Search.png
