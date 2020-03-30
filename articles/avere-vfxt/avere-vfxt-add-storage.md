---
title: Avere vFXT depolama alanını yapılandırma - Azure
description: Azure için Avere vFXT'nize arka uç depolama sistemi ekleme
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: rohogue
ms.openlocfilehash: dfffef90201ba4bbb5a912df6101e8338012df44
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252616"
---
# <a name="configure-storage"></a>Depolama alanını yapılandırma

Bu adım, vFXT kümeniz için bir arka uç depolama sistemi kurar.

> [!TIP]
> Avere vFXT kümesiyle birlikte yeni bir Azure Blob kapsayıcısı oluşturduysanız, bu kapsayıcı zaten yapılandırılmış ve kullanıma hazırdır.

Kümenizle yeni bir Blob kapsayıcısı oluşturmadıysanız veya ek bir donanım veya bulut tabanlı depolama sistemi eklemek istiyorsanız bu yönergeleri izleyin.

İki ana görev vardır:

1. vFXT kümenizi varolan bir depolama sistemine veya Azure Depolama hesap kapsayıcısına bağlayan bir [çekirdek filer oluşturun.](#create-a-core-filer)

1. İstemcilerin monte edeceği yolu tanımlayan [bir ad alanı bağlantısı oluşturun.](#create-a-junction)

Bu adımlar Avere Kontrol Paneli'ni kullanır. Nasıl kullanılacağını öğrenmek için [vFXT kümesine Eriş'i](avere-vfxt-cluster-gui.md) okuyun.

## <a name="create-a-core-filer"></a>Çekirdek filer oluşturma

"Core filer" bir arka uç depolama sistemi için bir vFXT terimidir. Depolama NetApp veya Isilon gibi bir donanım NAS cihazı olabilir, ya da bir bulut nesne deposu olabilir. Çekirdek filers hakkında daha fazla bilgi [Avere küme ayarları kılavuzunda](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/settings_overview.html#managing-core-filers)bulunabilir.

Çekirdek filer eklemek için, çekirdek filers iki ana tür birini seçin:

* [NAS çekirdek filer](#nas-core-filer) - nasıl bir NAS çekirdek filer eklemek için açıklar
* [Azure Depolama bulut çekirdek filer](#azure-blob-storage-cloud-core-filer) - bir Azure Blob depolama kapsayıcısını bulut çekirdek filer olarak nasıl ekleyeceğinizaçık

### <a name="nas-core-filer"></a>NAS çekirdek filer

BIR NAS çekirdek filer bir şirket içi NetApp veya Isilon cihaz veya bulut bir NAS bitiş noktası olabilir. Depolama sistemi Avere vFXT kümesine güvenilir bir yüksek hızlı bağlantıya sahip olmalıdır - örneğin, 1GBps ExpressRoute bağlantısı (VPN değil) - ve kullanılan NAS dışaaklarına küme kökü erişimi vermelidir.

NAS çekirdek filer eklemek için aşağıdaki adımları izleyin:

1. Avere Denetim Masası'ndan üstteki **Ayarlar** sekmesini tıklatın.

1. Soldaki **Core Filer** > **Manage Core Filers'ı** tıklatın.

1. **Oluştur'u**tıklatın.

   ![Oluştur düğmesinin üzerinde imleç olan yeni çekirdek filer sayfasının ekran görüntüsü](media/avere-vfxt-add-core-filer-start.png)

1. Sihirbazda gerekli bilgileri doldurun:

   * Çekirdek filer'Inin adını söyle.
   * Varsa tam nitelikli bir etki alanı adı (FQDN) sağlayın. Aksi takdirde, çekirdek filer giderir bir IP adresi veya ana bilgisayar adı sağlayın.
   * Listeden filer sınıfını seçin. Emin değilseniz, **Diğer'i**seçin.

     ![Çekirdek filer adı ve tam nitelikli etki alanı adı ile yeni çekirdek filer sayfasının ekran görüntüsü](media/avere-vfxt-add-core-filer.png)
  
   * **İleri'yi** tıklatın ve bir önbellek ilkesi seçin.
   * **Filer Ekle'yi**tıklatın.
   * Daha ayrıntılı bilgi için, Avere küme ayarları kılavuzunda [yeni bir NAS çekirdek filer ekleme'ye](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_nas.html) bakın.

Ardından, [bir kavşak oluşturma](#create-a-junction)ya devam edin.  

### <a name="azure-blob-storage-cloud-core-filer"></a>Azure Blob Depolama bulut çekirdek filer

Azure Blob depolama alanını vFXT kümenizin arka uç depolama alanı olarak kullanmak için çekirdek filer olarak eklemek için boş bir kapsayıcıgerekir.

Kümenize Blob depolama alanı eklemek için şu görevler önemlidir:

* Bir depolama hesabı oluşturma (adım 1, aşağıda)
* Boş bir Blob kapsayıcısı oluşturma (adım 2-3)
* depolama erişim anahtarını vFXT kümesi için bulut kimlik bilgisi olarak ekleyin (adım 4-6)
* Blob kapsayıcısını vFXT kümesi için çekirdek filer olarak ekleyin (7-9 adım)
* İstemcilerin çekirdek filer'ı takmak için kullandıkları bir ad alanı bağlantısı oluşturma (Hem donanım hem de bulut depolama için aynı şekilde[bir bağlantı oluşturma)](#create-a-junction)

> [!TIP]
> Azure kümesi için bir Avere vFXT oluşturduğunuzda yeni bir Blob kapsayıcısı oluşturursanız, dağıtım şablonu kapsayıcıyı otomatik olarak çekirdek filer olarak yapılandırır. (Bu, istek üzerine kullanılabilen oluşturma komut dosyasını kullanırsanız da geçerlidir.) Daha sonra çekirdek filer yapılandırmanız gerekmez.
>
> Küme oluşturma aracı sizin için şu yapılandırma görevlerini yapar:
>
> * Sağlanan depolama hesabında yeni bir Blob kapsayıcısı oluşturur
> * Kapsayıcıyı çekirdek filer olarak tanımlar
> * Kapsayıcıya bir ad alanı bağlantısı oluşturur
> * Kümenin sanal ağında bir depolama hizmeti bitiş noktası oluşturur

Kümeyi oluşturduktan sonra Blob depolama alanı eklemek için aşağıdaki adımları izleyin.

1. Bu ayarlarla genel amaçlı bir V2 depolama hesabı oluşturun:

   * **Abonelik** - vFXT kümesi ile aynı
   * **Kaynak grubu** - vFXT küme grubuyla aynı (isteğe bağlı)
   * **Konum** - vFXT kümesi ile aynı
   * **Performans** - Standart (Premium depolama desteklenmez)
   * **Hesap türü** - Genel amaçlı V2 (StorageV2)
   * **Çoğaltma** - Yerel olarak yedekli depolama (LRS)
   * **Erişim katmanı** - Sıcak
   * **Güvenli aktarım gereklidir** - bu seçeneği devre dışı (varsayılan olmayan değer)
   * **Sanal ağlar** - gerekli değil

   Azure portalını kullanabilir veya aşağıdaki "Azure'a Dağıt" düğmesini tıklatabilirsiniz.

   [![depolama hesabı oluşturmak için düğme](media/deploytoazure.png)](https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAvere%2Fmaster%2Fsrc%2Fvfxt%2Fstorageaccount%2Fazuredeploy.json)

1. Hesap oluşturulduktan sonra depolama hesabı sayfasına göz atın.

   ![Azure portalında yeni depolama hesabı](media/avere-vfxt-new-storage-acct.png)

1. Yeni bir Blob kapsayıcısı oluşturun: Genel bakış sayfasında **Kapsayıcılar'ı** tıklatın ve ardından **+Kapsayıcı'yı**tıklatın. Herhangi bir kapsayıcı adını kullanın ve erişimin **Özel**olarak ayarlandıklarına emin olun.

   ![+kapsayıcı düğmesi daire içine alınmış ve açılan sayfada yeni bir kapsayıcı oluşturulmakta olan depolama blobs sayfası](media/avere-vfxt-new-blob.png)

1. **Ayarlar**altında **Access tuşlarına** tıklayarak Azure Depolama hesap anahtarını alın. Sağlanan anahtarlardan birini kopyalayın.

   ![Anahtarı kopyalamak için Azure portalı GUI](media/avere-vfxt-copy-storage-key.png)

1. Kümeniz için Avere Denetim Paneli'ni açın. **Ayarlar'ı**tıklatın, ardından soldaki gezinti bölmesindeki **Cluster** > **Cloud Kimlik Bilgilerini** açın. Bulut Kimlik Bilgileri sayfasında Kimlik **Bilgileri Ekle'yi**tıklatın.

   ![Bulut Kimlik Bilgileri yapılandırma sayfasındakimlik bilgileri ekle düğmesini tıklatın](media/avere-vfxt-new-credential-button.png)

1. Bulut çekirdeği filer için bir kimlik bilgisi oluşturmak için aşağıdaki bilgileri doldurun:

   | Alan | Değer |
   | --- | --- |
   | Kimlik bilgisi adı | herhangi bir açıklayıcı ad |
   | Hizmet türü | (Azure Depolama erişim anahtarını seçin) |
   | Kiracı | depolama hesabı adı |
   | Abonelik | abonelik kimliği |
   | Depolama Erişim Anahtarı | Azure depolama hesabı anahtarı (önceki adımda kopyalanır) |

   **Gönder'i**tıklatın.

   ![Avere Kontrol Paneli'nde tamamlanmış bulut kimlik bilgisi formu](media/avere-vfxt-new-credential-submit.png)

1. Sonra, çekirdek filer oluşturun. Avere Kontrol Paneli'nin sol **tarafında, Core Filer** >  **Manage Core Filers'ı**tıklatın.

1. **Çekirdek Filers** ayarlarını yönet sayfasındaki **Oluştur** düğmesini tıklatın.

1. Sihirbazı doldurun:

   * Filer türü **Bulut'u**seçin.
   * Yeni çekirdek filer'ı adlandırın ve **İleri'yi**tıklatın.
   * Varsayılan önbellek ilkesini kabul edin ve üçüncü sayfaya devam edin.
   * **Hizmet türünde,** Azure **depolama alanını**seçin.
   * Daha önce oluşturulan kimlik bilgisi seçin.
   * **Kova içeriğini** **Boş** olarak ayarlama
   * **Sertifika doğrulamasını** **Engelli** olarak değiştir
   * **Sıkıştırma modunu** **Yok** olarak değiştirme
   * **İleri**'ye tıklayın.
   * Dördüncü sayfada, storage_account_name/*container_name* *olarak*Kova adına konteynerin **adını** girin.
   * İsteğe bağlı olarak, **Şifreleme türünü** **Yok**olarak ayarlayın.  Azure Depolama varsayılan olarak şifrelenir.
   * **Filer Ekle'yi**tıklatın.

   Daha ayrıntılı bilgi için, Avere küme yapılandırma kılavuzunda [yeni bir bulut çekirdeği filer ekleme'yi](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_cloud.html>) okuyun.

Sayfa yenilenir veya yeni çekirdek filer görüntülemek için sayfanızı yenileyebilirsiniz.

Sonra, bir [kavşak oluşturmanız](#create-a-junction)gerekir.

## <a name="create-a-junction"></a>Bir kavşak oluşturma

Bağlantı, istemciler için oluşturduğunuz bir yoldur. İstemciler yolu bağlar ve seçtiğiniz hedefe ulaşır.

Örneğin, NetApp `/vfxt/files` çekirdek filer `/vol0/data` dışa aktarma ve `/project/resources` alt dizininizin eşlemi oluşturabilirsiniz.

Kavşaklar hakkında daha fazla bilgi [Avere küme yapılandırma kılavuzunun ad alanı bölümünde](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_namespace.html)bulunabilir.

Avere Denetim Masası arabiriminde aşağıdaki adımları izleyin:

* Sol üstteki **VServer** > **Ad Alanı'nı** tıklatın.
* / (ileri eğik çizgi) ile ``/vfxt/data``başlayan bir ad alanı yolu sağlayın, gibi .
* Çekirdek filer seçin.
* Çekirdek filer dışa aktarmayı seçin.
* **İleri**'ye tıklayın.

  ![Kavşak, çekirdek filer ve dışa aktarma için tamamlanan alanları içeren "Yeni bağlantı ekle" sayfasının ekran görüntüsü](media/avere-vfxt-add-junction.png)

Bağlantı birkaç saniye sonra görünür. Gerektiğinde ek kavşaklar oluşturun.

Bağlantı oluşturulduktan sonra, istemciler depolama sisteminden dosyalara erişmek için ad alanı yolunu kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Avere vFXT kümesini takma](avere-vfxt-mount-clients.md)
* Verileri yeni [bir Blob kapsayıcısına taşımanın](avere-vfxt-data-ingest.md) verimli yollarını öğrenin
