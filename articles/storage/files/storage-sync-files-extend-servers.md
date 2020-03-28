---
title: Öğretici - Azure Dosya Eşitlemi ile Windows dosya sunucularını genişletme | Microsoft Dokümanlar
description: Windows dosya sunucularını Azure Dosya Eşitleme ile baştan sona nasıl genişletacağınızı öğrenin.
author: roygara
ms.service: storage
ms.topic: tutorial
ms.date: 10/23/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: eb00234fb7522c763dbaa910bee99cf327bebaf1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77597907"
---
# <a name="tutorial-extend-windows-file-servers-with-azure-file-sync"></a>Öğretici: Windows dosya sunucularını Azure Dosya Eşitleme ile genişletme

Makale, Azure Dosya Eşitlemeyi kullanarak bir Windows sunucusunun depolama kapasitesini genişletmek için temel adımları göstermektedir. Öğreticide Windows Server bir Azure sanal makinesi (VM) olarak yer alsa da, bu işlemi genellikle şirket içi sunucularınız için yaparsınız. Azure Dosya Eşitle'sini dağıtma yönergelerini, [Azure Dosya Eşitle'sini dağıt(Azure Dosya Eşitlemi)](storage-sync-files-deployment-guide.md) makalesinde kendi ortamınızda bulabilirsiniz.

> [!div class="checklist"]
> * Depolama Eşitleme Hizmetini Dağıtma
> * Windows Server’ı Azure Dosya Eşitleme ile kullanmaya hazırlama
> * Azure Dosya Eşitleme aracısını yükleme
> * Windows Server'ı Depolama Eşitleme Hizmetine Kaydedin
> * Bir eşitleme grubu ve bir bulut uç noktası oluşturma
> * Sunucu uç noktası oluşturma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

Bu öğretici için, Azure Dosya Eşitlemeyi'ni dağıtmadan önce aşağıdakileri yapmanız gerekir:

- Azure depolama hesabı ve dosya paylaşımı oluşturma
- Windows Server 2016 Datacenter VM'i ayarlama
- Windows Server VM'yi Azure Dosya Eşitlemi için hazırlama

### <a name="create-a-folder-and-txt-file"></a>Klasör ve .txt dosyası oluşturma

Yerel bilgisayarınızda _EşitlenecekDosyalar_ adlı yeni bir klasör oluşturun ve _testbelgem.txt_ adlı bir metin dosyası ekleyin. Bu dosyayı daha sonra bu öğreticide dosya paylaşımına yüklersiniz.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

### <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma

Bir Azure depolama hesabı dağıttıktan sonra bir dosya paylaşımı oluşturursunuz.

1. Azure portalında **kaynağa git'i**seçin.
1. Depolama hesabı bölmesinden **Dosyalar'ı** seçin.

    ![Dosyaları Seçin](./media/storage-sync-files-extend-servers/click-files.png)

1. + **Dosya Paylaş'ı**seçin.

    ![Dosya paylaşımı ekle düğmesini seçin](./media/storage-sync-files-extend-servers/create-file-share-portal2.png)

1. Ad yeni dosya payı _afsfileshare_. **Kota**için "1" girin ve sonra **Oluştur'u**seçin. Kota en fazla 5 TiB olabilir, ancak bu öğretici için yalnızca 1 GB gerekir.

    ![Yeni dosya paylaşımı için bir ad ve kota belirtin](./media/storage-sync-files-extend-servers/create-file-share-portal3.png)

1. Yeni dosya paylaşımını seçin. Dosya paylaşım konumunda **Yükle'yi**seçin.

    ![Dosyayı karşıya yükleme](./media/storage-sync-files-extend-servers/create-file-share-portal5.png)

1. .txt dosyanızı oluşturduğunuz _FilesToSync_ klasörüne göz atın, _mytestdoc.txt'yi_ seçin ve **Yükle'yi**seçin.

    ![Dosya paylaşımına göz atın](./media/storage-sync-files-extend-servers/create-file-share-portal6.png)

Bu noktada, içinde bir dosya bulunan bir depolama hesabı ve dosya paylaşımı oluşturdunuz. Ardından, bu öğreticide şirket içi sunucuyu temsil etmek için Windows Server 2016 Datacenter ile bir Azure VM dağıtırsınız.

### <a name="deploy-a-vm-and-attach-a-data-disk"></a>VM dağıtma ve veri diskini kullanıma açma

1. Azure portalına gidin ve soldaki menüyü genişletin. Sol üst köşede **kaynak oluştur'u** seçin.
1. **Azure Marketi** kaynakları listesinin üzerindeki arama **kutusunda, Windows Server 2016 Datacenter'ı** arayın ve sonuçlarda seçin. **Oluştur**’u seçin.
1. **Temel bilgiler** sekmesine gidin. **Proje ayrıntıları**altında, bu öğretici için oluşturduğunuz kaynak grubunu seçin.

   ![Portal bıçağına VM'inizle ilgili temel bilgileri girin](./media/storage-sync-files-extend-servers/vm-resource-group-and-subscription.png)

1. **Örnek ayrıntıları**altında, bir VM adı sağlayın. Örneğin, _myVM_kullanın.
1. **Bölge,** **Kullanılabilirlik seçenekleri,** **Resim**ve **Boyut**için varsayılan ayarları değiştirmeyin.
1. **Yönetici hesabı** altında VM için bir **Kullanıcı adı** ve **Parola** girin.
1. **Gelen bağlantı noktası kuralları altında,** **seçili bağlantı noktalarına izin ver'i** seçin ve ardından açılan menüden **RDP (3389)** ve **HTTP'yi** seçin.

1. VM’yi oluşturmadan önce bir veri diski oluşturmanız gerekir.

   1. **Sonraki:Diskleri**seçin.

      ![Veri diski ekleme](./media/storage-sync-files-extend-servers/vm-add-data-disk.png)

   1. **Diskler** sekmesinde, **Disk seçenekleri**altında varsayılanları bırakın.
   1. **DATA DISKSaltında,** **Yeni disk oluştur'u seçin ve yeni bir disk**ekleyin.

   1. Bu öğretici için **1 GB** olarak değiştirebileceğiniz **Boyut (GiB)** dışındaki varsayılan ayarları kullanın.

      ![Veri diski ayrıntıları](./media/storage-sync-files-extend-servers/vm-create-new-disk-details.png)

   1. **Tamam'ı**seçin.
1. **İncele ve oluştur**’u seçin.
1. **Oluştur'u**seçin.

   **Dağıtım'ın ilerlemesini**izlemek için **Bildirimler** simgesini seçebilirsiniz. Yeni bir VM oluşturmanın tamamlanması birkaç dakika sürebilir.

1. VM dağıtımınız tamamlandıktan sonra **kaynağa git'i**seçin.

   ![Kaynağa git](./media/storage-sync-files-extend-servers/vm-gotoresource.png)

Bu noktada yeni bir sanal makine oluşturdunuz ve bir veri diskini kullanıma açtınız. Sonra VM'ye bağlanın.

### <a name="connect-to-your-vm"></a>Sanal makinenize bağlanma

1. Azure portalında, sanal makine özellikleri **sayfasından Bağlan'ı** seçin.

   ![Portaldan bir Azure sanal makinesine bağlanma](./media/storage-sync-files-extend-servers/connect-vm.png)

1. Sanal **makineye Bağlan** sayfasında, bağlantı noktası 3389 üzerinden **IP adresine** bağlanmak için varsayılan seçenekleri tutun. **RDP dosyasını indir**'i seçin.

   ![RDP dosyasını indirme](./media/storage-sync-files-extend-servers/download-rdp.png)

1. İndirilen RDP dosyasını açın ve istendiğinde **Bağlan'ı** seçin.
1. **Windows Güvenliği** penceresinde **Diğer seçenekler**'i ve ardından **Başka bir hesap kullanın**'ı seçin. Kullanıcı adını *localhost\username*olarak yazın, sanal makine için oluşturduğunuz parolayı girin ve ardından **Tamam'ı**seçin.

   ![Diğer seçenekler](./media/storage-sync-files-extend-servers/local-host2.png)

1. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıyı oluşturmak için **Evet** veya **Devam et'i** seçin.

### <a name="prepare-the-windows-server"></a>Windows sunucusunu hazırlama

Windows Server 2016 Datacenter sunucusu için Internet Explorer Artırılmış Güvenlik Yapılandırması’nı devre dışı bırakın. Bu adım yalnızca ilk sunucu kaydı için gereklidir. Sunucu kaydedildikten sonra özelliği yeniden etkinleştirebilirsiniz.

Windows Server 2016 Datacenter VM'de Server Manager otomatik olarak açılır.  Sunucu Yöneticisi varsayılan olarak açılmıyorsa, Başlat Menüsü'nde arayın.

1. **Server Manager'da** **Yerel Sunucu'yı**seçin.

   ![Sunucu Yöneticisi kullanıcı arabiriminin sol tarafındaki "Yerel Sunucu"](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-1.png)

1. **Özellikler** bölmesine, **IE Gelişmiş Güvenlik Yapılandırması**bağlantısını seçin.  

    ![Sunucu Yöneticisi kullanıcı arabirimindeki "IE Artırılmış Güvenlik Yapılandırması" bölmesi](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-2.png)

1. **Internet Explorer Artırılmış Güvenlik Yapılandırması** iletişim kutusunda, **Yöneticiler** ve **Kullanıcılar** için **Kapalı** seçeneğini belirleyin.

    !["Kapalı" seçeneğinin işaretli olduğu Internet Explorer Artırılmış Güvenlik Yapılandırması açılır penceresi](media/storage-sync-files-extend-servers/prepare-server-disable-ieesc-3.png)

Şimdi VM’ye veri diski ekleyebilirsiniz.

### <a name="add-the-data-disk"></a>Veri diski ekleme

1. Windows Server **2016 Datacenter** VM'de yken, **Dosyalar ve depolama hizmetleri** > **Birimleri** > **Diskleri'ni**seçin.

    ![Veri diski](media/storage-sync-files-extend-servers/your-disk.png)

1. **Msft Virtual Disk** adlı 1 GB diske sağ tıklayın ve **Yeni birim**seçin.
1. Sihirbazı tamamlayın. Varsayılan ayarları kullanın ve atanan sürücü mektubunu not edin.
1. **Oluştur'u**seçin.
1. **Kapat**'ı seçin.

   Bu noktada, diski çevrimiçi duruma getirdiniz ve bir birim oluşturdunuz. Son eklenen veri diskinin varlığını doğrulamak için Windows Server VM'de Dosya Gezgini'ni açın.

1. VM'deki File **Explorer'da bu bilgisayarı** genişletin ve yeni sürücüyü açın. Bu örnekte yeni sürücü F: sürücüsüdür.
1. Sağ tıklatın ve **Yeni** > **Klasör'ü**seçin. Klasörü _EşitlenecekDosyalar_ olarak adlandırın.
1. **FilesToSync** klasörünü açın.
1. Sağ tıklayın ve **Yeni** > **Metin Belgesi**seçin. Metin dosyasını _TestDosyam_ olarak adlandırın.

    ![Yeni metin dosyası ekleme](media/storage-sync-files-extend-servers/new-file.png)

1. **Dosya Gezgini** ve **Sunucu Yöneticisi'ni**kapatın.

### <a name="download-the-azure-powershell-module"></a>Azure PowerShell modüllerini indirin

Ardından, Windows Server 2016 Datacenter VM'de Azure PowerShell modüllerini sunucuya yükleyin.

1. VM'de yükseltilmiş bir PowerShell penceresi açın.
1. Şu komutu çalıştırın:

   ```powershell
   Install-Module -Name Az
   ```

   > [!NOTE]
   > 2.8.5.201'den eski bir NuGet sürümünüz varsa, NuGet'in en son sürümünü indirmeniz ve yüklemeniz istenir.

   PowerShell galerisi varsayılan olarak PowerShellGet için güvenilir depo olarak yapılandırılmamıştır. PSGallery'yi ilk kez kullandığınızda aşağıdaki istemi görürsünüz:

   ```output
   Untrusted repository

   You are installing the modules from an untrusted repository. If you trust this repository, change its InstallationPolicy value by running the Set-PSRepository cmdlet.

   Are you sure you want to install the modules from 'PSGallery'?
   [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):
   ```

1. Yüklemeye devam etmek **için Herkese** **Evet** veya Evet yanıtını verin.

`Az` modülü, Azure PowerShell cmdlet’leri için toplu bir modüldür. Bu modülü yüklediğinizde kullanılabilir durumdaki tüm Azure Resource Manager modülleri indirilir ve cmdlet’leri kullanıma sunulur.

Bu noktada, eğitim için ortamınızı ayarladınız. Depolama Eşitleme Hizmetini dağıtmaya hazırsınız.

## <a name="deploy-the-service"></a>Hizmeti dağıtma

Azure Dosya Eşitlemeyi dağıtmak için, öncelikle bir **Depolama Eşitleme Hizmeti** kaynağını seçili aboneliğiniz için bir kaynak grubuna yerzein. Depolama Eşitleme Hizmeti, abonelik ve kaynak grubundan erişim izinlerini devralır.

1. Azure portalında kaynak **oluştur'u** seçin ve ardından **Azure Dosya Eşitlemeyi'ni**arayın.
1. Arama sonuçlarında **Azure Dosya Eşitleme**’yi seçin.
1. **Oluştur**’u seçerek **Depolama Eşitleme’yi Dağıt** sekmesini açın.

   ![Depolama Eşitleme’yi Dağıtma](media/storage-sync-files-extend-servers/afs-info.png)

   Açılan bölmeye aşağıdaki bilgileri girin:

   | Değer | Açıklama |
   | ----- | ----- |
   | **Adı** | Depolama Eşitleme Hizmeti için benzersiz bir ad (abonelik başına).<br><br>Bu öğretici için _afssyncservice02_ kullanın. |
   | **Abonelik** | Bu öğretici için kullandığınız Azure aboneliği. |
   | **Kaynak grubu** | Depolama Eşitleme Hizmeti içeren kaynak grubu.<br><br>Bu öğretici için _afsresgroup101918_ kullanın. |
   | **Konum** | Doğu ABD |

1. İşiniz bittiğinde **Oluştur**’u seçerek **Depolama Eşitleme Hizmeti**’ni dağıtın.
1. **Kaynaklara git**> **Bildirimler** sekmesini seçin.

## <a name="install-the-agent"></a>Aracıyı yükleme

Azure Dosya Eşitleme aracısı, Windows Server’ın bir Azure dosya paylaşımı ile eşitlenmesini sağlayan indirilebilir bir pakettir.

1. Windows **Server 2016 Datacenter** VM'de **Internet Explorer'ı**açın.
1. [Microsoft İndirme Merkezi](https://go.microsoft.com/fwlink/?linkid=858257)’ne gidin. Azure Dosya **Eşitleme Aracısı** bölümüne gidin ve **İndir'i**seçin.

   ![Eşitleme aracısını indirme](media/storage-sync-files-extend-servers/sync-agent-download.png)

1. StorageSyncAgent_V3_WS2016 için onay kutusunu **seçin. EXE** ve **Sonraki**seçin.

   ![Aracı seçme](media/storage-sync-files-extend-servers/select-agent.png)

1. Bir Kez > **Çalıştır** > **Open** **Aç'ı**seçin.
1. Henüz yapmadıysanız, PowerShell penceresini kapatın.
1. **Depolama Eşitleme Aracısı Kurulum Sihirbazı**’nda varsayılan ayarları kabul edin.
1. **Yükle**’yi seçin.
1. **Bitiş'i**seçin.

Azure Eşitleme Hizmeti’ni dağıttınız ve aracıyı Windows Server 2016 Datacenter sanal makinesine yüklediniz. Şimdi VM’yi Depolama Eşitleme Hizmeti’ne kaydetmeniz gerekir.

## <a name="register-windows-server"></a>Windows Server’ı kaydetme

Windows sunucunuzu Bir Depolama Eşitleme Hizmetine kaydetmek, sunucunuz (veya kümeniz) ile Depolama Eşitleme Hizmeti arasında bir güven ilişkisi kurar. Bir sunucu yalnızca bir Depolama Eşitleme Hizmetine kaydedilebilir. Bu Depolama Eşitleme Hizmeti ile ilişkili diğer sunucular ve Azure dosya paylaşımları ile eşitlenebilir.

Azure Dosya Eşitleme aracısını yükledikten sonra Sunucu Kayıt UI'sinin otomatik olarak açılması gerekir. Yoksa, dosya konumundan el ile açabilirsiniz:`C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe.`

1. VM'de Sunucu Kayıt UI açıldığında **Tamam'ı**seçin.
1. Başlamak için **Oturum Aç'ı** seçin.
1. Azure hesap kimlik bilgilerinizle oturum açın ve **Oturum Aç'ı**seçin.
1. Şu bilgileri belirtin:

   ![Sunucu Kaydı kullanıcı arabiriminin ekran görüntüsü](media/storage-sync-files-extend-servers/signin.png)

   | | |
   | ----- | ----- |
   | Değer | Açıklama |
   | **Azure Aboneliği** | Bu öğretici için Depolama Eşitleme Hizmetini içeren abonelik. |
   | **Kaynak Grubu** | Depolama Eşitleme Hizmeti içeren kaynak grubu. Bu öğretici için _afsresgroup101918_ kullanın. |
   | **Depolama Eşitleme Hizmeti** | Depolama Eşitleme Hizmetinin adı. Bu öğretici için _afssyncservice02_ kullanın. |

1. Sunucu kaydını tamamlamak için **Kaydol'u** seçin.
1. Kayıt işleminin bir parçası olarak, ek bir oturum açma nız istenir. Oturum açın ve **İleri'yi**seçin.
1. **Tamam'ı**seçin.

## <a name="create-a-sync-group"></a>Eşitleme grubu oluşturma

Eşitleme grubu, bir dosya kümesi için eşitleme topolojisini tanımlar. Eşitleme grubu, Azure dosya paylaşımını temsil eden bir bulut bitiş noktası içermelidir. Eşitleme grubu da bir veya daha fazla sunucu uç noktası içermelidir. Sunucu uç noktası, kayıtlı bir sunucu üzerindeki bir yolu temsil eder. Eşitleme grubu oluşturmak için:

1. Azure [portalında,](https://portal.azure.com/)Depolama Eşitleme Hizmeti'nden **+ Eşitle grubunu** seçin. Bu öğretici için *afssyncservice02* kullanın.

   ![Azure portalda yeni bir eşitleme grubu oluşturma](media/storage-sync-files-extend-servers/add-sync-group.png)

1. Bulut bitiş noktası olan bir eşitleme grubu oluşturmak için aşağıdaki bilgileri girin:

   | Değer | Açıklama |
   | ----- | ----- |
   | **Eşitleme grubu adı** | Bu ad Depolama Eşitleme Hizmetinde benzersiz olmalıdır, ancak size mantıklı gelen herhangi bir ad olabilir. Bu öğretici için *afssyncgroup* kullanın.|
   | **Abonelik** | Bu öğretici için Depolama Eşitleme Hizmetini dağıttığınız abonelik. |
   | **Depolama hesabı** | **Depolama hesabı seçin.** Görünen bölmede, oluşturduğunuz Azure dosya paylaşımına sahip depolama hesabını seçin. Bu öğretici için *afsstoracct101918* kullanın. |
   | **Azure dosya paylaşımı** | Oluşturduğunuz Azure dosya paylaşımının adı. Bu öğretici için *afsfileshare* kullanın. |

1. **Oluştur'u**seçin.

Eşitleme grubunuzu seçerseniz, artık bir **bulut uç noktanızın** olduğunu görebilirsiniz.

## <a name="add-a-server-endpoint"></a>Sunucu uç noktası ekleme

Sunucu bitiş noktası, kayıtlı bir sunucudaki belirli bir konumu temsil eder. Örneğin, sunucu hacmindeki bir klasör. Sunucu bitiş noktası eklemek için:

1. Yeni oluşturulan eşitleme grubunu seçin ve ardından **sunucu bitiş noktası ekle'yi**seçin.

   ![Eşitleme grubu bölmesine yeni bir sunucu uç noktası ekleme](media/storage-sync-files-extend-servers/add-server-endpoint.png)

1. Sunucu **bitiş noktası** ekle bölmesine, sunucu bitiş noktası oluşturmak için aşağıdaki bilgileri girin:

   | | |
   | ----- | ----- |
   | Değer | Açıklama |
   | **Kayıtlı sunucu** | Oluşturduğunuz sunucunun adı. Bu öğretici için *afsvm101918* kullanın. |
   | **Yol** | Oluşturduğunuz sürücüye giden Windows Server yolu. Bu öğreticide *f:\filestosync* kullanın. |
   | **Bulut Katmanlama** | Bu öğretici için devre dışı bırakın. |
   | **Birim Boş Alanı** | Bu öğretici için boş bırakın. |

1. **Oluştur'u**seçin.

Dosyalarınız artık Azure dosya paylaşımında ve Windows Server’da eşitlenmiş durumdadır.

![Azure Depolama başarıyla eşitlendi](media/storage-sync-files-extend-servers/files-synced-in-azurestorage.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure Dosya Eşitlemeyi'ni kullanarak bir Windows sunucusunun depolama kapasitesini genişletmek için temel adımları öğrendiniz. Azure Dosya Eşitleme dağıtımı planlamasına daha ayrıntılı bir bakış için bkz:

> [!div class="nextstepaction"]
> [Azure Dosya Eşitleme dağıtımı planlama](./storage-sync-files-planning.md)
