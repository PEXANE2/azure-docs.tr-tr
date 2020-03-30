---
title: Windows VM'lerde Azure Dosyaları paylaşımı oluşturma ve kullanma
description: Bu hızlı başlangıçta, Azure portalında bir Azure Files paylaşımı nı kurup bir Windows sanal makinesine bağlarsınız. Dosyalar paylaşımına bağlanır, Dosyalar paylaşımına bir dosya yüklersiniz. Ardından Dosyalar paylaşımının anlık görüntüsünü alır, Dosyalar paylaşımındaki dosyayı değiştirir ve Dosyalar paylaşımının önceki anlık görüntüsünü geri yükleyin.
author: roygara
ms.service: storage
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 6bbab0ee2eefe6e86c150d5bddab4f8e91a7c92d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75463898"
---
# <a name="quickstart-create-and-manage-azure-files-share-with-windows-virtual-machines"></a>Hızlı başlatma: Windows sanal makineleriyle Azure Dosyaları paylaşımıoluşturma ve yönetme

Makale, Azure Dosyaları paylaşımı oluşturma ve kullanma nın temel adımlarını göstermektedir. Bu hızlı başlangıçta, hizmetin nasıl çalıştığını deneyimlemeniz için azure dosyaları paylaşımını hızla ayarlamaya önem verilir. Azure dosya paylaşımlarını kendi ortamınızda oluşturmak ve kullanmak için daha ayrıntılı talimatlara ihtiyacınız varsa, bkz. Windows [ile Azure dosya paylaşımı kullan.](storage-how-to-use-files-windows.md)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

Bu hızlı başlatmada, aşağıdaki öğeleri ayarlarsınız:

- Azure depolama hesabı ve Azure dosya paylaşımı
- Bir Windows Server 2016 Datacenter VM

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Azure dosya paylaşımıyla çalışabilmek için önce bir Azure depolama hesabı oluşturmanız gerekir. Genel amaçlı bir v2 depolama hesabı, tüm Azure Depolama hizmetlerine erişim sağlar: blobs, dosyalar, kuyruklar ve tablolar. Hızlı başlangıç genel amaçlı bir v2 depolama hesabı oluşturur, ancak herhangi bir depolama hesabı türü oluşturma adımları benzerdir. Bir depolama hesabında sınırsız sayıda paylaşım olabilir. Bir paylaşım, depolama hesabının kapasite limitlerine kadar sınırsız sayıda dosyayı depolayabilir.

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

### <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma

Sonra, bir dosya paylaşımı oluşturacaksınız.

1. Azure depolama hesabı dağıtımı tamamlandığında **kaynağa git'i**seçin.
1. Depolama hesabı bölmesinden **Dosyalar'ı** seçin.

    ![Dosyaları Seçin](./media/storage-files-quick-create-use-windows/click-files.png)

1. **Dosya Paylaş'ı**seçin.

    ![Dosya paylaşımı ekle düğmesini seçin](./media/storage-files-quick-create-use-windows/create-file-share.png)

1. Yeni dosya *paylaşımıqsfileshare* > **Kota** için "1" girin > **oluştur'u**seçin. Kota en fazla 5 TiB olabilir, ancak bu hızlı başlangıç için yalnızca 1 GiB gerekir.
1. Yerel makinenizde *qsTestFile* adlı yeni bir txt dosyası oluşturun.
1. Yeni dosya paylaşımını seçin, ardından dosya paylaşım konumunda, **Yükle'yi**seçin.

    ![Dosyayı karşıya yükleme](./media/storage-files-quick-create-use-windows/create-file-share-portal5.png)

1. .txt dosyanızı oluşturduğunuz konuma göz atın > *qsTestFile.txt'yi* seçin > **Yükleyin'** i seçin.

Şimdiye kadar, Azure'da bir dosya içeren bir Azure depolama hesabı ve dosya paylaşımı oluşturdunuz. Daha sonra, bu hızlı başlangıçta şirket içi sunucuyu temsil etmek için Windows Server 2016 Datacenter ile Azure VM'yi oluşturursunuz.

### <a name="deploy-a-vm"></a>VM'yi dağıtma

1. Ardından, portalın sol tarafındaki menüyü genişletin ve Azure portalın sol üst köşesindeki **Kaynak oluştur**’u seçin.
1. **Azure Market** kaynaklarının listesi üzerindeki arama kutusunda, **Windows Server 2016 Datacenter**’ı arayıp seçin, ardından **Oluştur**’u seçin.
1. Temel **Bilgiler** sekmesinde, **Proje ayrıntıları**altında, bu hızlı başlangıç için oluşturduğunuz kaynak grubunu seçin.

   ![Portal dikey penceresinde VM’niz ile ilgili temel bilgileri girin](./media/storage-files-quick-create-use-windows/vm-resource-group-and-subscription.png)

1. **Örnek ayrıntıları**altında, VM *qsVM*adını.
1. **Bölge**, **Kullanılabilirlik seçenekleri**, **Görüntü** ve **Boyut** için varsayılan ayarları değiştirmeden bırakın.
1. **Yönetici hesabı**altında, **Kullanıcı adı** olarak *VMadmin* ekleyin ve VM için bir **Parola** girin.
1. **Gelen bağlantı noktası kuralları** altında **Seçilen bağlantı noktalarına izin ver**'i, sonra aşağı açılan listeden **RDP (3389)** ve **HTTP** değerlerini seçin.
1. **İncele ve oluştur**’u seçin.
1. **Oluştur'u**seçin. Yeni bir sanal makinenin oluşturulması birkaç dakika sürebilir.

1. VM dağıtımınız tamamlandıktan sonra **kaynağa git'i**seçin.

Bu noktada yeni bir sanal makine oluşturdunuz ve bir veri diskini kullanıma açtınız. Şimdi VM’ye bağlanmanız gerekir.

### <a name="connect-to-your-vm"></a>Sanal makinenize bağlanma

1. Sanal makine özellikleri sayfasında **Bağlan'ı** seçin.

   ![Portaldan bir Azure sanal makinesine bağlanma](./media/storage-files-quick-create-use-windows/connect-vm.png)

1. Sanal **makineye Bağlan** sayfasında, *3389* **numaralı bağlantı noktası** üzerinden IP **adresine** bağlanmak için varsayılan seçenekleri tutun ve **RDP dosyasını indir'i**seçin.
1. İndirilen RDP dosyasını açın ve istendiğinde **Bağlan'ı** seçin.
1. **Windows Güvenliği** penceresinde **Diğer seçenekler**'i ve ardından **Başka bir hesap kullanın**'ı seçin. Kullanıcı adı, &lt;&gt; sanal makine için oluşturduğunuz VM yönetici kullanıcı adı olan *localhost\username*olarak yazın. Sanal makine için oluşturduğunuz parolayı girin ve ardından **Tamam'ı**seçin.

   ![Diğer seçenekler](./media/storage-files-quick-create-use-windows/local-host2.png)

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. bağlantıyı oluşturmak için **Evet** veya **Devam** et'i seçin.

## <a name="map-the-azure-file-share-to-a-windows-drive"></a>Azure dosya paylaşımını bir Windows sürücüsüyle eşle

1. Azure portalında *qsfileshare* fileshare'e gidin ve **Bağlan'ı**seçin.
1. İkinci kutunun içeriğini kopyalayın ve **Not Defteri'ne**yapıştırın.

   ![Azure Dosyaları Bağlan bölmesinden UNC adı](./media/storage-files-quick-create-use-windows/portal_netuse_connect2.png)

1. VM'de **Dosya Gezgini'ni** açın ve penceredeki **bu bilgisayarı** seçin. Bu seçim, şeritte bulunan menüleri değiştirir. **Bilgisayar** menüsünde **Harita ağ sürücüsünü**seçin.
1. Sürücü harfini seçin ve UNC adını girin. Bu quickstart adlandırma önerilerini takip ettiyseniz, **Not Defteri'nden** * \\qsstorageacct.file.core.windows.net\qsfileshare* kopyalayın.

   Her iki onay kutusunun da işaretli olduğundan emin olun.

   ![“Ağ Sürücüsüne Bağlan” iletişim kutusunun ekran görüntüsü](./media/storage-files-quick-create-use-windows/mountonwindows10.png)

1. **Bitiş'i**seçin.
1. Windows **Security** iletişim kutusunda:

   - Not Defteri'nden AZURE\ ile hazırlanmış depolama hesabı adını kopyalayın ve kullanıcı adı olarak **Windows Security** iletişim kutusuna yapıştırın. Bu hızlı başlangıçta adlandırma önerilerini izlediyseniz, *AZURE\qsstorageacct'i kopyalayın.*
   - Not Defteri'nden depolama hesabı anahtarını kopyalayın ve Parola olarak **Windows Security** iletişim kutusuna yapıştırın.

      ![Azure Dosyaları Bağlan bölmesinden UNC adı](./media/storage-files-quick-create-use-windows/portal_netuse_connect3.png)

## <a name="create-a-share-snapshot"></a>Paylaşım anlık görüntüsü oluşturma

Artık sürücünün haritasını yaptığınıza göre, bir anlık görüntü oluşturabilirsiniz.

1. Portalda, dosya paylaşımınıza gidin ve **anlık görüntü oluştur'u**seçin.

   ![Anlık görüntü oluşturma](./media/storage-files-quick-create-use-windows/create-snapshot.png)

1. VM'de *qstestfile.txt'yi* açın ve "bu dosya değiştirildi" > kaydedin ve dosyayı kapatın.
1. Başka bir anlık görüntü oluşturun.

## <a name="browse-a-share-snapshot"></a>Paylaşım anlık görüntüsüne göz atın

1. Dosya paylaşımınızda **anlık görüntüleri görüntüle'yi**seçin.
1. Dosya **paylaşımı anlık görüntüleri** bölmesinde, listedeki ilk anlık görüntüseçin.

   ![Zaman damgaları listesinde seçilen anlık görüntü](./media/storage-files-quick-create-use-windows/snapshot-list.png)

1. Bu anlık görüntü için bölmede, *qsTestFile.txt*seçin.

## <a name="restore-from-a-snapshot"></a>Anlık görüntüden geri yükleme

1. Dosya paylaşımı anlık görüntüsünden *qsTestFile'a*sağ tıklayın ve **Geri Yükle** düğmesini seçin.
1. **Orijinal dosyayı Üzerine Yaz'ı**seçin.

   ![İndirme ve Geri Yükleme düğmeleri](./media/storage-files-quick-create-use-windows/snapshot-download-restore-portal.png)

1. VM'de dosyayı açın. Değiştirilmemiş sürüm geri yüklendi.

## <a name="delete-a-share-snapshot"></a>Paylaşım anlık görüntüsünü silme

1. Dosya paylaşımınızda **anlık görüntüleri görüntüle'yi**seçin.
1. Dosya **paylaşımı anlık görüntüleri** bölmesinde, listedeki son anlık görüntüseçin ve **Sil'i**tıklatın.

   ![Sil düğmesi](./media/storage-files-quick-create-use-windows/portal-snapshots-delete.png)

## <a name="use-a-share-snapshot-in-windows"></a>Windows'da paylaşım anlık görüntüsü kullanma

Şirket içi VSS anlık görüntülerinde olduğu gibi, Önceki Sürümler sekmesini kullanarak monte edilmiş Azure dosya paylaşımınızdan anlık görüntüleri görüntüleyebilirsiniz.

1. Dosya Gezgini'nde, monte edilmiş paylaşımı bulun.

   ![Dosya Gezgini'nde montajlı paylaşım](./media/storage-files-quick-create-use-windows/snapshot-windows-mount.png)

1. *qsTestFile.txt'yi* seçin ve menüden Özellikler'i sağ tıklayın ve **Özellikler'i** > seçin.

   ![Seçilen dizin için sağ tıklama menüsü](./media/storage-files-quick-create-use-windows/snapshot-windows-previous-versions.png)

1. Bu dizine ait paylaşım anlık görüntülerinin listesini görmek için **Önceki Sürümler**'i seçin.

1. Anlık görüntüaçmak için **Aç'ı** seçin.

   ![Önceki Sürümler sekmesi](./media/storage-files-quick-create-use-windows/snapshot-windows-list.png)

## <a name="restore-from-a-previous-version"></a>Önceki sürümü geri yükleme

1. **Geri Yükle'yi**seçin. Bu eylem, tüm dizinin içeriğini, paylaşım anlık görüntüsünün oluşturulduğu anda özgün konuma özyinelemeyle kopyalar.

   ![Uyarı iletisindeki](./media/storage-files-quick-create-use-windows/snapshot-windows-restore.png) geri yükleme düğmesi Not:Dosyanız değişmediyse, bu dosya anlık görüntüyle aynı sürüm olduğundan, bu dosya için önceki bir sürümü görmezsiniz. Bu, windows dosya sunucusunda bu şekilde çalışır ile tutarlıdır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure dosya paylaşımını Windows'da kullanma](storage-how-to-use-files-windows.md)
