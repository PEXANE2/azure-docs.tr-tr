---
title: Azure geçişi sunucu değerlendirmesi ile içeri aktarılan bir CSV dosyası kullanarak şirket içi sunucuları değerlendirme
description: Azure geçişi sunucu değerlendirmesi ' nde içeri aktarılan CSV dosyası kullanarak Azure 'a geçiş için şirket içi sunucuların nasıl keşfedileceğini açıklar
ms.topic: tutorial
ms.date: 09/14/2020
ms.openlocfilehash: 743f18ce72e3f14fe54e0bbadff254ea03fc6278
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90604232"
---
# <a name="tutorial-assess-servers-using-an-imported-csv-file"></a>Öğretici: içeri aktarılan bir CSV dosyası kullanarak sunucuları değerlendirme

Azure 'a geçiş sürecinizin bir parçası olarak şirket içi envanterinizi ve iş yüklerinizi keşfedeceksiniz. 

Bu öğreticide, içeri aktarılan bir virgülle ayrılmış değerler (CSV) dosyası kullanarak Azure geçişi: Sunucu değerlendirmesi aracı ile şirket içi makineleri değerlendirme gösterilmektedir. 

Bir CSV dosyası kullanıyorsanız, sunucuları bulup değerlendirmek için Azure geçişi gereci ayarlamanız gerekmez. Dosya içinde paylaştığınız verileri denetleyebilir ve verilerin büyük bölümü isteğe bağlıdır. Bu yöntem şu durumlarda yararlı olur:

- Gereci dağıtmadan önce hızlı, ilk değerlendirme oluşturmak istersiniz.
- Azure geçişi gerecini kuruluşunuza dağıtamazsınız.
- Şirket içi sunuculara erişime izin veren kimlik bilgilerini paylaşamazsınız.
- Güvenlik kısıtlamaları, Gereç tarafından toplanan verileri Azure 'a toplamayı ve göndermeyi önler.

> [!NOTE]
> CSV dosyası kullanılarak içeri aktarılan sunucuları geçiremezsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure hesabı ayarlama
> * Azure geçişi projesi ayarlama
> * CSV dosyası hazırlama
> * Dosyayı içeri aktarma
> * Sunucuları değerlendir

> [!NOTE]
> Öğreticiler senaryo denemek için en hızlı yolu gösterir ve mümkün olan yerlerde varsayılan seçenekleri kullanır. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- Tek bir CSV dosyasında ve bir Azure geçişi projesinde en fazla 20.000 sunucu ekleyebilirsiniz. 
- CSV dosyasında belirtilen işletim sistemi adları, [desteklenen adları](#supported-operating-system-names)içermelidir ve bu adlara uymalıdır.


## <a name="prepare-an-azure-user-account"></a>Azure Kullanıcı hesabı hazırlama

Bir Azure geçişi projesi oluşturmak için şunları içeren bir hesap gerekir:
- Azure aboneliğinde katkıda bulunan veya sahip izinleri.
- Azure Active Directory uygulamaları kaydetme izinleri.

Henüz ücretsiz bir Azure hesabı oluşturduysanız, aboneliğinizin sahibi olursunuz. Abonelik sahibi değilseniz, izinleri aşağıdaki şekilde atamak için sahibiyle birlikte çalışın:

1. Azure portal, "abonelikler" araması yapın ve **Hizmetler**altında **abonelikler**' i seçin.

    ![Azure aboneliğini aramak için arama kutusu](./media/tutorial-discover-import/search-subscription.png)

2. **Abonelikler** sayfasında, Azure geçişi projesi oluşturmak istediğiniz aboneliği seçin. 
3. Abonelikte **erişim denetimi (IAM)**  >  **erişimi denetle**' yi seçin.
4. **Erişimi denetle**' de ilgili Kullanıcı hesabını arayın.
5. **Rol ataması Ekle**' de, **Ekle**' ye tıklayın.

    ![Erişimi denetlemek ve rol atamak için bir kullanıcı hesabı arayın](./media/tutorial-discover-import/azure-account-access.png)

6. **Rol ataması Ekle**' de, katkıda bulunan veya sahip rolünü seçin ve hesabı (örneğimizde azmigrateuser) seçin. Daha sonra **Kaydet**'e tıklayın.

    ![Hesaba rol atamak için rol ataması Ekle sayfasını açar](./media/tutorial-discover-import/assign-role.png)

7. Portalda, kullanıcılar için arama yapın ve **Hizmetler**altında **Kullanıcılar**' ı seçin.
8. **Kullanıcı ayarları**' nda, Azure AD kullanıcılarının uygulamaları kaydedebildiğini doğrulayın (varsayılan olarak **Evet** ' e ayarlanır).

    ![Kullanıcıların Active Directory uygulamalar kaydedebildiğini Kullanıcı ayarlarında doğrula](./media/tutorial-discover-import/register-apps.png)



## <a name="set-up-a-project"></a>Proje ayarlama

Yoksa yeni bir Azure geçişi projesi ayarlayın.

1. Azure portalı > **Tüm hizmetler** bölümünde **Azure Geçişi**’ni arayın.
2. **Hizmetler** altında **Azure Geçişi**’ni seçin.
3. **Genel bakış**bölümünde **proje oluştur**' u seçin.
5. **Proje oluştur**' da Azure aboneliğinizi ve kaynak grubunuzu seçin. Yoksa, bir kaynak grubu oluşturun.
6. **Proje ayrıntıları**' nda projeyi oluşturmak istediğiniz proje adını ve coğrafi konumu belirtin. [Kamu](migrate-support-matrix.md#supported-geographies-public-cloud) ve [kamu bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için desteklenen coğrafi lıkları gözden geçirin.

   ![Proje adı ve bölgesi için kutular](./media/tutorial-discover-import/new-project.png)

7. **Oluştur**’u seçin.
8. Azure Geçişi projesinin dağıtılması için birkaç dakika bekleyin.

**Azure geçişi: Sunucu değerlendirmesi** Aracı, varsayılan olarak yeni projeye eklenir.

![Varsayılan olarak eklenen sunucu değerlendirmesi aracını gösteren sayfa](./media/tutorial-discover-import/added-tool.png)

## <a name="prepare-the-csv"></a>CSV 'yi hazırlama

CSV şablonunu indirin ve buna sunucu bilgilerini ekleyin.

### <a name="download-the-template"></a>Şablonu indirme

1. **Geçiş hedefleri**  >  **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**' nde **bul**' u seçin.
2. **Makine bul**' da **CSV kullanarak içeri aktar**' ı seçin.
3. CSV şablonunu indirmek için **İndir** ' i seçin. Alternatif olarak, [doğrudan indirebilirsiniz](https://go.microsoft.com/fwlink/?linkid=2109031).

    ![CSV şablonunu indir](./media/tutorial-discover-import/download-template.png)

### <a name="add-server-information"></a>Sunucu bilgileri ekleme

Sunucu verilerini toplayın ve CSV dosyasına ekleyin.

- Verileri toplamak için, VMware vSphere veya yapılandırma yönetimi veritabanınız (CMDB) gibi şirket içi sunucu yönetimi için kullandığınız araçlardan dışarı aktarabilirsiniz.
- Örnek verileri gözden geçirmek için [örnek dosyayı](https://go.microsoft.com/fwlink/?linkid=2108405)indirin.

Aşağıdaki tabloda, doldurulacak dosya alanları özetlenmektedir:

**Alan adı** | **Girilmesi** | **Ayrıntılar**
--- | --- | ---
**Sunucu adı** | Yes | Tam etki alanı adını (FQDN) belirtmeyi öneririz.
**IP adresi** | No | Sunucu adresi.
**Çekirdekler** | Yes | Sunucuya ayrılan işlemci çekirdekleri sayısı.
**Bellek** | Yes | Sunucuya ayrılan toplam RAM (MB).
**İşletim sistemi adı** | Yes | Sunucu işletim sistemi. <br/> [Bu](#supported-operating-system-names) listedeki adları eşleşen veya içeren işletim sistemi adları değerlendirme tarafından tanınır.
**İşletim sistemi sürümü** | No | Sunucu işletim sistemi sürümü.
**İşletim sistemi mimarisi** | No | Sunucu işletim sistemi mimarisi <br/> Geçerli değerler şunlardır: x64, x86, AMD64, 32-bit veya 64 bit
**Disk sayısı** | No | Ayrı disk ayrıntıları sağlanmışsa gerekli değildir.
**Disk 1 boyutu**  | No | Maksimum disk boyutu (GB cinsinden).<br/>Şablona [sütun ekleyerek](#add-multiple-disks) daha fazla disk için ayrıntılar ekleyebilirsiniz. En fazla sekiz disk ekleyebilirsiniz.
**Disk 1 okuma Ops** | No | Saniye başına disk okuma işlemi.
**Disk 1 yazma Ops** | No | Saniye başına disk yazma işlemi.
**Disk 1 okuma aktarım hızı** | No | Diskten saniyede okunan veriler, MB/saniye cinsinden.
**Disk 1 yazma aktarım hızı** | No | Saniyede diske yazılan veriler (MB/saniye).
**CPU kullanım yüzdesi** | No | Kullanılan CPU yüzdesi.
**Bellek kullanım yüzdesi** | No | Kullanılan RAM yüzdesi.
**Toplam disk okuma Ops** | No | Saniye başına disk okuma işlemi.
**Toplam disk yazma Ops** | No | Saniye başına disk yazma işlemi.
**Toplam disk okuma performansı** | No | Diskten MB/saniye cinsinden okunan veriler.
**Toplam disk yazma performansı** | No | Diske yazılan veriler MB/saniye cinsinden.
**Aktarım sırasında ağ** | No | Sunucu tarafından saniyede MB cinsinden alınan veriler.
**Ağ çıkış performansı** | No | Sunucu tarafından saniye başına MB cinsinden aktarılan veriler.
**Bellenim türü** | No | Sunucu üretici yazılımı. Değerler "BIOS" veya "UEFı" olabilir.
**MAC adresi**| No | Sunucu MAC adresi.


### <a name="add-operating-systems"></a>İşletim sistemleri ekleme

Değerlendirme, belirli işletim sistemi adlarını tanır. Belirttiğiniz ad, [desteklenen adlar listesindeki](#supported-operating-system-names)dizelerden biriyle tam olarak eşleşmelidir.

### <a name="add-multiple-disks"></a>Birden çok disk Ekle

Şablon, ilk disk için varsayılan alanlar sağlar. En fazla sekiz diske benzer sütunlar ekleyebilirsiniz.

Örneğin, ikinci bir disk için tüm alanları belirtmek için şu sütunları ekleyin:

- Disk 2 boyutu
- Disk 2 okuma Ops
- Disk 2 yazma Ops
- Disk 2 okuma performansı
- Disk 2 yazma işleme


## <a name="import-the-server-information"></a>Sunucu bilgilerini içeri aktarma

CSV şablonuna bilgi ekledikten sonra, CSV dosyasını sunucu değerlendirmesi içine aktarın.

1. Azure geçişi 'nde, **bulma makineler**bölümünde, tamamlanan şablona gidin.
2. **İçeri aktar**'ı seçin.
3. İçeri aktarma durumu gösterilir.
    - Durum durumunda uyarı görünürse, bunları çözebilir ya da bunları bilmeden devam edebilirsiniz.
    - Değerlendirme doğruluğunu artırmak için, Uyarılar bölümünde önerildiği gibi sunucu bilgilerini geliştirebilirsiniz.
    - Uyarıları görüntülemek ve onarmak için **uyarı ayrıntılarını İndir ' i seçin. CSV**. Bu işlem CSV 'yi dahil edilen uyarılarla indirir. Uyarıları gözden geçirin ve sorunları gerektiği şekilde giderin.
    - Durum, içeri aktarma durumunun **başarısız**olması için durumda görünüyorsa, içeri aktarmaya devam edebilmeniz için önce bu hataları çözmeniz gerekir:
        1. Artık hata ayrıntılarını içeren CSV 'yi indirin.
        1. Hataları gözden geçirin ve gerektiği şekilde çözün. 
        1. Değiştirilen dosyayı yeniden karşıya yükleyin.
4. İçeri aktarma durumu **tamamlandığında**, sunucu bilgileri içeri aktarılır. İçeri aktarma işleminin tamamlanmamış gibi görünmediğini yenileyin.

## <a name="update-server-information"></a>Sunucu bilgilerini güncelleştir

Sunucu için verileri aynı **sunucu adıyla**yeniden içeri aktararak sunucu bilgilerini güncelleştirebilirsiniz. **Sunucu adı** alanını değiştiremezsiniz. Sunucuları silme işlemi şu anda desteklenmiyor.

## <a name="verify-servers-in-the-portal"></a>Portalda sunucuları doğrulama

Azure portal, bulma sonrasında sunucuların göründüğünü doğrulamak için:

1. Azure geçişi panosunu açın.
2. **Azure geçişi-sunucular**  >  **Azure geçişi: Sunucu değerlendirmesi** sayfasında, **bulunan sunucuların**sayısını görüntüleyen simgeyi seçin.
3. **Içeri aktarma tabanlı** sekmesini seçin.



## <a name="supported-operating-system-names"></a>Desteklenen işletim sistemi adları

CSV 'de belirtilen işletim sistemi adları içermesi ve eşleşmesi gerekir. Aksi takdirde, bunları değerlendiremeyeceksiniz. 

**A-H** | **I-R** | **S-T** | **U-Z**
--- | --- | --- | ---
Apple Mac OS X 10<br/>Asıanux 3<br/>Asıanux 4<br/>Asıanux 5<br/>CentOS<br/>CentOS 4/5<br/>CoreOS Linux<br/>Borçlu GNU/Linux 4<br/>Borçlu GNU/Linux 5<br/>Borçlu GNU/Linux 6<br/>Borçlu GNU/Linux 7<br/>De, GNU/Linux 8<br/>FreeBSD | IBM OS/2<br/>MS-DOS<br/>Novell NetWare 5<br/>Novell NetWare 6<br/>Oracle Linux<br/>Oracle Linux 4/5<br/>Oracle Solaris 10<br/>Oracle Solaris 11<br/>Red Hat Enterprise Linux 2<br/>Red Hat Enterprise Linux 3<br/>Red Hat Enterprise Linux 4<br/>Red Hat Enterprise Linux 5<br/>Red Hat Enterprise Linux 6<br/>Red Hat Enterprise Linux 7<br/>Red Hat Fedora | SCO OpenServer 5<br/>SCO OpenServer 6<br/>SCO UnixWare 7<br/> Serenlik sistemleri eComStation 1<br/>Çevre sistemleri eComStation <br/>Sun Microsystems Solaris 8<br/>Sun Microsystems Solaris 9<br/><br/>SUSE Linux Enterprise 10<br/>SUSE Linux Enterprise 11<br/>SUSE Linux Enterprise 12<br/>SUSE Linux Enterprise 8/9<br/>SUSE Linux Enterprise 11<br/>SUSE openSUSE | Ubuntu Linux<br/>VMware ESXi 4<br/>VMware ESXi 5<br/>VMware ESXi 6<br/>Windows 10<br/>Windows 2000<br/>Windows 3<br/>Windows 7<br/>Windows 8<br/>Windows 95<br/>Windows 98<br/>Windows NT<br/>Windows Server (R) 2008<br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016<br/>Windows Server 2019<br/>Windows Server eşiği<br/>Windows Vista<br/>Windows Web Server 2008 R2<br/>Windows XP Professional

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Azure geçişi projesi oluşturma 
> * İçeri aktarılan bir CSV dosyası kullanan sunucular bulundu. Şimdi, [Azure VM 'Lerine VMware VM geçişi](tutorial-assess-vmware.md)için bir değerlendirme çalıştırın.
