---
title: Azure VM yedeklemesinden dosya ve klasörleri kurtarma
description: Bu makalede, bir Azure sanal makine kurtarma noktasından dosya ve klasörleri nasıl kurtarış ediniröğrenin.
ms.topic: conceptual
ms.date: 03/01/2019
ms.openlocfilehash: 0e3061ea8fc26adcf39fe415cd9a662de739543a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273312"
---
# <a name="recover-files-from-azure-virtual-machine-backup"></a>Azure sanal makine yedeklemesinden dosyaları kurtarma

Azure Yedekleme, kurtarma noktaları olarak da bilinen Azure VM yedeklemelerinden [Azure sanal makinelerini (VM'leri) ve disklerini](./backup-azure-arm-restore-vms.md) geri yükleme olanağı sağlar. Bu makalede, bir Azure VM yedeklemesinden dosya ve klasörlerin nasıl kurtarılacak ları açıklanmaktadır. Dosya ve klasörleri geri getirmek yalnızca Kaynak Yöneticisi modeli kullanılarak dağıtılan ve Kurtarma hizmetleri kasasına karşı korunan Azure VM'leri için kullanılabilir.

> [!NOTE]
> Bu özellik, Kaynak Yöneticisi modelini kullanarak dağıtılan ve Kurtarma Hizmetleri kasasına karşı korunan Azure VM'leri için kullanılabilir.
> Şifrelenmiş bir VM yedeklemesinden dosya kurtarma desteklenmez.
>

## <a name="mount-the-volume-and-copy-files"></a>Ses düzeyini ekleme ve dosyaları kopyalama

Kurtarma noktasından dosyaları veya klasörleri geri yüklemek için sanal makineye gidin ve istediğiniz kurtarma noktasını seçin.

1. [Azure portalında](https://portal.Azure.com) oturum açın ve sol bölmede **Sanal makineler'e**tıklayın. Sanal makineler listesinden, sanal makinenin panosunu açmak için sanal makineyi seçin.

2. Sanal makinenin menüsünde, Yedekleme panosunu açmak için **Yedekleme'yi** tıklatın.

    ![Kurtarma Hizmetleri kasa yedek öğesi açık](./media/backup-azure-restore-files-from-vm/open-vault-for-vm.png)

3. Yedekleme panosu menüsünde **Dosya Kurtarma'yı**tıklatın.

    ![Dosya kurtarma düğmesi](./media/backup-azure-restore-files-from-vm/vm-backup-menu-file-recovery-button.png)

    **Dosya Kurtarma** menüsü açılır.

    ![Dosya kurtarma menüsü](./media/backup-azure-restore-files-from-vm/file-recovery-blade.png)

4. Kurtarma **noktası** açılır menüsünden, istediğiniz dosyaları tutan kurtarma noktasını seçin. Varsayılan olarak, en son kurtarma noktası zaten seçilir.

5. Dosyaları kurtarma noktasından kopyalamak için kullanılan yazılımı indirmek **için, Yürütülebilir'i İndir** (Windows Azure VM'ler için) veya **Download Script'i** (Linux Azure VM'leri için bir python komut dosyası oluşturulur) tıklatın.

    ![Oluşturulan parola](./media/backup-azure-restore-files-from-vm/download-executable.png)

    Azure, yürütülebilir veya komut dosyasını yerel bilgisayara indirir.

    ![çalıştırılabilir veya komut dosyası için ileti indirme](./media/backup-azure-restore-files-from-vm/run-the-script.png)

    Yürütülebilir veya komut dosyasını yönetici olarak çalıştırmak için, indirilen dosyayı bilgisayarınıza kaydetmeniz önerilir.

6. Yürütülebilir veya komut dosyası parola korumalıdır ve parola gerektirir. Dosya **Kurtarma** menüsünde, parolayı belleğe yüklemek için kopyala düğmesini tıklatın.

    ![Oluşturulan parola](./media/backup-azure-restore-files-from-vm/generated-pswd.png)

7. İndirme konumundan (genellikle İndirilenler klasöründen) çalıştırılabilir veya komut dosyasına sağ tıklayın ve Yönetici kimlik bilgileriyle çalıştırın. İstendiğinde, parolayı yazın veya parolayı bellekten yapıştırın ve **Enter**tuşuna basın. Geçerli parola girilen komut dosyası kurtarma noktasına bağlanır.

    ![Dosya kurtarma menüsü](./media/backup-azure-restore-files-from-vm/executable-output.png)

8. Linux makineleri için bir python komut dosyası oluşturulur. Bir komut dosyası indirmek ve ilgili / uyumlu Linux sunucusuna kopyalamak gerekir. 'yi yürütmek için izinleri değiştirmeniz ```chmod +x <python file name>```gerekebilir. Daha sonra python ```./<python file name>```dosyasını '' ile çalıştırın.

Komut dosyasının başarılı bir şekilde çalıştırıldığından emin olmak için [Erişim gereksinimleri](#access-requirements) bölümüne bakın.

### <a name="identifying-volumes"></a>Birimlerin tanımlanması

#### <a name="for-windows"></a>Windows için

Çalıştırılabilir çalıştırdığınızda, işletim sistemi yeni birimleri bağlar ve sürücü mektupları atar. Bu sürücülere göz atmak için Windows Gezgini veya Dosya Gezgini'ni kullanabilirsiniz. Birimlere atanan sürücü harfleri, özgün sanal makineyle aynı harfler olmayabilir. Ancak, birim adı korunur. Örneğin, orijinal sanal makinenin hacmi "Veri Diski`\`(E: )" ise, bu birim yerel bilgisayara "Veri Diski ('Herhangi bir harf': )`\`olarak eklenebilir. Dosyalarınızı veya klasörünüzü bulana kadar komut dosyası çıktısında belirtilen tüm birimlere göz atın.  

   ![Dosya kurtarma menüsü](./media/backup-azure-restore-files-from-vm/volumes-attached.png)

#### <a name="for-linux"></a>Linux için

Linux'ta, kurtarma noktasının birimleri komut dosyasının çalıştırıldığı klasöre monte edilir. Ekli diskler, birimler ve karşılık gelen montaj yolları buna göre gösterilir. Bu montaj yolları kök düzeyi erişimi olan kullanıcılar tarafından görülebilir. Komut dosyası çıktısında belirtilen birimlere göz atın.

  ![Linux Dosya kurtarma menüsü](./media/backup-azure-restore-files-from-vm/linux-mount-paths.png)

## <a name="closing-the-connection"></a>Bağlantıyı kapatma

Dosyaları tanımladıktan ve yerel bir depolama konumuna kopyaladıktan sonra, ek sürücüleri kaldırın (veya kaldırın). Sürücülerin sökülmesi için Azure portalındaki **Dosya Kurtarma** menüsünde **Diskleri Sökme'yi**tıklatın.

![Sökme diskleri](./media/backup-azure-restore-files-from-vm/unmount-disks3.png)

Diskler söküldükten sonra bir ileti alırsınız. Diskleri kaldırabilmeniz için bağlantının yenilenmesi birkaç dakika sürebilir.

Linux'ta, kurtarma noktasına bağlantı kesildikten sonra, işletim sistemi ilgili montaj yollarını otomatik olarak kaldırmaz. Montaj yolları "yetim" birimleri olarak var ve görünür, ancak dosyalara eriştiğinde/yazarken bir hata atmak. Bunlar el ile kaldırılabilir. Komut dosyası, çalıştırıldığında, önceki kurtarma noktalarından var olan bu tür birimleri tanımlar ve onay alınca temizler.

## <a name="special-configurations"></a>Özel yapılandırmalar

### <a name="dynamic-disks"></a>Dinamik diskler

Korumalı Azure VM'de aşağıdaki özelliklerden birine veya her ikisine sahip birimler varsa, çalıştırılabilir komut dosyasını aynı VM'de çalıştıramazsınız.

- Birden çok diski kapsayan birimler (yayılan ve çizgili birimler)
- Dinamik disklerde hataya dayanıklı hacimler (aynalı ve RAID-5 birimleri)

Bunun yerine, uyumlu bir işletim sistemi ile başka bir bilgisayarda çalıştırılabilir komut dosyası çalıştırın.

### <a name="windows-storage-spaces"></a>Windows Depolama Alanları

Windows Depolama Alanları, depolama alanını sanallaştırmanızı sağlayan bir Windows teknolojisidir. Windows Depolama Alanları ile endüstri standardı diskleri depolama havuzlarına gruplayabilirsiniz. Ardından, depolama alanları adı verilen sanal diskler oluşturmak için bu depolama havuzlarında kullanılabilir alanı kullanırsınız.

Korumalı Azure VM Windows Depolama Alanları kullanıyorsa, çalıştırılabilir komut dosyasını aynı VM'de çalıştıramazsınız. Bunun yerine, uyumlu bir işletim sistemine sahip başka bir makinede çalıştırılabilir komut dosyasını çalıştırın.

### <a name="lvmraid-arrays"></a>LVM/RAID dizileri

Linux'ta, Mantıksal birim yöneticisi (LVM) ve/veya yazılım RAID Dizileri, birden çok disk üzerinden mantıksal birimleri yönetmek için kullanılır. Korumalı Linux VM LVM ve/veya RAID Dizileri kullanıyorsa, komut dosyasını aynı VM'de çalıştıramazsınız. Bunun yerine, komut dosyasını uyumlu bir işletim sistemi olan ve korumalı VM'nin dosya sistemini destekleyen başka bir makinede çalıştırın.

Aşağıdaki komut dosyası çıktısı LVM ve/veya RAID Dizileri disklerini ve bölüm türündeki birimleri görüntüler.

   ![Linux LVM Çıkış menüsü](./media/backup-azure-restore-files-from-vm/linux-LVMOutput.png)

Bu bölümleri çevrimiçi duruma getirmek için aşağıdaki bölümlerdeki komutları çalıştırın.

#### <a name="for-lvm-partitions"></a>LVM bölümleri için

Birim grup adlarını fiziksel bir birim altında listelemek için:

```bash
#!/bin/bash
pvs <volume name as shown above in the script output>
```

Tüm mantıksal birimleri, adları ve bunların yollarını bir birim grubundaki listelemek için:

```bash
#!/bin/bash
lvdisplay <volume-group-name from the pvs command's results>
```

Mantıksal birimleri seçtiğiniz yola monte etmek için:

```bash
#!/bin/bash
mount <LV path> </mountpath>
```

#### <a name="for-raid-arrays"></a>RAID dizileri için

Aşağıdaki komut tüm raid diskleri hakkındaki ayrıntıları görüntüler:

```bash
#!/bin/bash
mdadm –detail –scan
```

 İlgili RAID diski,`/dev/mdm/<RAID array name in the protected VM>`

RAID diskinin fiziksel birimleri varsa montaj komutunu kullanın:

```bash
#!/bin/bash
mount [RAID Disk Path] [/mountpath]
```

RAID diskinde yapılandırılan başka bir LVM varsa, LVM bölümleri için önceki yordamı kullanın, ancak RAID Disk adının yerine birim adını kullanın.

## <a name="system-requirements"></a>Sistem gereksinimleri

### <a name="for-windows-os"></a>Windows İşletim Sistemi için

Aşağıdaki tablo, sunucu ve bilgisayar işletim sistemleri arasındaki uyumluluğu gösterir. Dosyaları kurtarırken, dosyaları önceki veya gelecekteki bir işletim sistemi sürümüne geri yükleyemezsiniz. Örneğin, bir Dosyayı Windows Server 2016 VM'den Windows Server 2012'ye veya Windows 8 bilgisayarına geri yükleyemezsiniz. Dosyaları VM'den aynı sunucu işletim sistemine veya uyumlu istemci işletim sistemine geri yükleyebilirsiniz.

|Sunucu İşletim Sistemi | Uyumlu istemci işletim sistemi  |
| --------------- | ---- |
| Windows Server 2019    | Windows 10 |
| Windows Server 2016    | Windows 10 |
| Windows Server 2012 R2 | Windows 8.1 |
| Windows Server 2012    | Windows 8  |
| Windows Server 2008 R2 | Windows 7   |

### <a name="for-linux-os"></a>Linux OS için

Linux'ta, dosyaları geri yüklemek için kullanılan bilgisayarın işletim sistemi korumalı sanal makinenin dosya sistemini desteklemelidir. Komut dosyasını çalıştırmak için bir bilgisayar seçerken, bilgisayarın uyumlu bir işletim sistemi olduğundan emin olun ve aşağıdaki tabloda tanımlanan sürümlerden birini kullanır:

|Linux İşletim Sistemi | Sürümler  |
| --------------- | ---- |
| Ubuntu | 12.04 ve üzeri |
| CentOS | 6.5 ve üzeri  |
| RHEL | 6.7 ve üzeri |
| Debian | 7 ve üzeri |
| Oracle Linux | 6.4 ve üzeri |
| SLES | 12 ve üzeri |
| openSUSE | 42.2 ve üzeri |

> [!NOTE]
> Biz SLES 12 SP4 işletim sistemi ile makinelerde dosya kurtarma komut dosyası çalışan bazı sorunlar bulduk ve Biz SLES ekibi ile araştırıyoruz.
> Şu anda, dosya kurtarma komut dosyası çalışan SLES 12 SP2 ve SP3 işletim sistemi sürümleri ile makineler üzerinde çalışıyor.
>

Komut dosyası, Python ve bash bileşenlerinin yürütmesini ve kurtarma noktasına güvenli bir şekilde bağlanmasını da gerektirir.

|Bileşen | Sürüm  |
| --------------- | ---- |
| bash | 4 ve üzeri |
| python | 2.6.6 ve üzeri  |
| TLS | 1.2 desteklenmelidir  |

## <a name="access-requirements"></a>Erişim gereksinimleri

Komut dosyasını kısıtlı erişime sahip bir bilgisayarda çalıştırın, aşağıdakilere erişin

- `download.microsoft.com`
- Kurtarma Hizmeti URL'leri (coğrafi ad, kurtarma hizmeti kasasının bulunduğu bölgeyi ifade eder)
  - `https://pod01-rec2.geo-name.backup.windowsazure.com`(Azure genel geos için)
  - `https://pod01-rec2.geo-name.backup.windowsazure.cn`(Azure China 21Vianet için)
  - `https://pod01-rec2.geo-name.backup.windowsazure.us`(Azure ABD Hükümeti için)
  - `https://pod01-rec2.geo-name.backup.windowsazure.de`(Azure Almanya için)
- Giden bağlantı noktaları 53 (DNS), 443, 3260

> [!NOTE]
>
> - İndirilen komut dosyası dosya adı, URL'de doldurulacak **coğrafi adı** olacaktır. Exampple için: İndirilen komut \'dosyası\'\_\'adı\'VMname geoname _\'GUID\'ile başlar , *ContosoVM_wcus_12345678* gibi
> - URL olacaktır <https://pod01-rec2.wcus.backup.windowsazure.com>"
>

Linux için, komut dosyası kurtarma noktasına bağlanmak için 'açık-iscsi' ve 'lshw' bileşenleri gerektirir. Komut dosyasının çalıştırıldığı bilgisayarda bileşenler yoksa, komut dosyası bileşenleri yüklemek için izin ister. Gerekli bileşenleri yüklemek için izin sağlayın.

`download.microsoft.com` Komut dosyasının çalıştırıldığı makine ile kurtarma noktasındaki veriler arasında güvenli bir kanal oluşturmak için kullanılan bileşenleri karşıdan yüklemeyapmak için erişim gereklidir.

Komut dosyasını, yedeklenmiş VM ile aynı (veya uyumlu) işletim sistemine sahip herhangi bir makinede çalıştırabilirsiniz. Uyumlu işletim sistemleri için [Uyumlu işletim sistemi tablosuna](backup-azure-restore-files-from-vm.md#system-requirements) bakın. Korumalı Azure sanal makine windows depolama alanlarını (Windows Azure VM'leri için) veya LVM/RAID Dizileri kullanıyorsa (Linux VM'ler için), aynı sanal makinede çalıştırılabilir veya komut dosyası çalıştıramazsınız. Bunun yerine, uyumlu bir işletim sistemine sahip başka bir makinede çalıştırılabilir veya komut dosyasını çalıştırın.

## <a name="file-recovery-from-virtual-machine-backups-having-large-disks"></a>Büyük disklere sahip Sanal makine yedeklemelerinden dosya kurtarma

Bu bölümde, 16'dan fazla diske sahip azure sanal makinelerin yedeklerinden dosya kurtarma nasıl gerçekleştiriliş gösterilmektedir ve her disk boyutu 32 TB'den büyüktür.

Dosya kurtarma işlemi yedeklemeden tüm diskleri iliştirdiğinden, çok sayıda disk (>16) veya büyük diskler (her biri > 32 TB) kullanıldığında, aşağıdaki eylem noktaları önerilir:

- Dosya kurtarma için ayrı bir geri yükleme sunucusu (Azure VM D2v3 VM) tutun. Bunu yalnızca dosya kurtarma için kullanabilir ve gerekli olmadığında kapatabilirsiniz. VM kendisi üzerinde önemli bir etkiye sahip olacağından orijinal makinenin geri kullanılması önerilmez.
- Ardından, dosya kurtarma işleminin başarılı olup olmadığını denetlemek için komut dosyasını bir kez çalıştırın.
- Dosya kurtarma işlemi askıda ysa (diskler hiç monte edilmemişse veya monte edilmişlerse ancak birimler görünmüyorsa), aşağıdaki adımları gerçekleştirin.
  - Geri yükleme sunucusu bir Windows VM ise:
    - İşletim sistemi WS 2012 veya daha yüksek olduğundan emin olun.
    - Kayıt defteri anahtarlarının geri yükleme sunucusunda aşağıda önerildiği gibi ayarlandığından emin olun ve sunucuyu yeniden başlatTınız. GUID yanındaki sayı 0001-0005 arasında değişebilir. Aşağıdaki örnekte, 0004. Parametre bölümüne kadar kayıt defteri anahtar yolunda ilerleyin.

    ![iscsi-reg-anahtar-changes.png](media/backup-azure-restore-files-from-vm/iscsi-reg-key-changes.png)

```registry
- HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Disk\TimeOutValue – change this from 60 to 1200
- HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Class\{4d36e97b-e325-11ce-bfc1-08002be10318}\0003\Parameters\SrbTimeoutDelta – change this from 15 to 1200
- HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Class\{4d36e97b-e325-11ce-bfc1-08002be10318}\0003\Parameters\EnableNOPOut – change this from 0 to 1
- HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Class\{4d36e97b-e325-11ce-bfc1-08002be10318}\0003\Parameters\MaxRequestHoldTime - change this from 60 to 1200
```

- Geri yükleme sunucusu bir Linux VM ise:
  - /etc/iscsi/iscsid.conf dosyasında, ayarı aşağıdakilerden değiştirin:
    - düğüm.conn[0].timeo.noop_out_timeout = 5 düğüm.conn[0].timeo.noop_out_timeout = 30
- Yukarıdaki değişikliği yaptıktan sonra, komut dosyasını yeniden çalıştırın. Bu değişikliklerle, dosya kurtarmanın başarılı olması son derece olasıdır.
- Kullanıcı her komut dosyası indirdinlerinde, Azure Yedekleme kurtarma noktasını karşıdan yüklemeye hazırlama işlemini başlatır. Büyük disklerde bu işlem önemli ölçüde zaman alacaktır. Art arda istek patlamaları varsa, hedef hazırlama bir indirme sarmalına girer. Bu nedenle, Portal/ Powershell / CLI bir komut dosyası indirmek için tavsiye edilir, 20-30 dakika bekleyin (sezgisel) ve sonra çalıştırın. Bu zamana kadar, hedef komut dosyasından bağlantı için hazır olması bekleniyor.
- Dosya kurtarmadan sonra, portala geri döndüğünüzden ve birim montajı yapamadığınız kurtarma noktaları için **Dağık'ı** tıklattığınızı unutmayın. Esasen, bu adım varolan tüm süreçleri/oturumları temizleyecek ve iyileşme şansını artıracaktır.

## <a name="troubleshooting"></a>Sorun giderme

Sanal makinelerden dosyaları kurtarırken sorun yaşıyorsanız, ek bilgi için aşağıdaki tabloyu kontrol edin.

| Hata İletisi / Senaryo | Muhtemel Neden | Önerilen eylem |
| ------------------------ | -------------- | ------------------ |
| Exe çıkışı: *Hedefe bağlanırken yakalanan özel durum* | Komut dosyası kurtarma noktasına erişemez    | Makinenin önceki erişim [gereksinimlerini](#access-requirements)karşılayıp karşılmadığını kontrol edin. |  
| Exe çıkışı: *Hedef zaten bir iSCSI oturumu aracılığıyla giriş yaptı.* | Komut dosyası zaten aynı makinede yürütülmüş ve sürücüler eklenmiş | Kurtarma noktasının birimleri zaten eklenmiş. Orijinal VM'nin aynı sürücü harfleriyle monte edilmeyebilirler. Dosyanız için dosya gezginindeki tüm kullanılabilir birimlere göz atın. |
| Exe çıkışı: *Diskler portal üzerinden söküldüğü/12 saat sınırını aştığı için bu komut dosyası geçersizdir. Portaldan yeni bir komut dosyası indirin.* |    Diskler portaldan söküldü veya 12 saatlik sınır aşıldı | Bu özel exe şimdi geçersiz ve çalıştırılamaz. Bu kurtarma noktası nın dosyalarına zamanında erişmek istiyorsanız, yeni bir exe için portalı ziyaret edin.|
| Exe'nin çalıştırıldığı makinede: Sökme düğmesi tıklatıldıktan sonra yeni birimler sökülmüyor | Makinedeki iSCSI başlatıcısı, hedefe olan bağlantısını yanıtlamıyor/yenilemiyor ve önbelleği koruyor. |  **Dismount**tıkladıktan sonra, birkaç dakika bekleyin. Yeni birimler sökülmüyorsa, tüm birimlere göz atın. Tüm birimlere göz atmak başlatıcıyı bağlantıyı yenilemeye zorlar ve birim, diskin kullanılamadığı bir hata iletisiyle sökülür.|
| Exe çıktısı: Komut dosyası başarılı bir şekilde çalıştırılır, ancak komut dosyası çıktısı üzerinde "Yeni birimler eklenmiş" görüntülenmez |    Bu geçici bir hatadır    | Birimler zaten eklenmiş olacak. Göz atmak için Explorer'ı açın. Komut dosyalarını her seferinde çalıştırmak için aynı makineyi kullanıyorsanız, makineyi yeniden başlatmayı düşünün ve liste sonraki exe çalıştırmalarında görüntülenmelidir. |
| Linux'a özgü: İstenilen hacimleri görüntüleyeme | Komut dosyasının çalıştırıldığı makinenin işletim sistemi, korumalı VM'nin temel dosya sistemini tanımayabilir | Kurtarma noktasının kilitlenme tutarlı mı yoksa dosya tutarlı mı olup olmadığını denetleyin. Dosya tutarlıysa, komut dosyasını işletim sistemi korumalı VM'nin dosya sistemini tanıyan başka bir makinede çalıştırın. |
| Windows'a özgü: İstenilen birimleri görüntüleyeme | Diskler eklenmiş olabilir, ancak birimler yapılandırılmamıştır | Disk yönetim ekranından, kurtarma noktasıyla ilgili ek diskleri tanımlayın. Bu disklerden herhangi biri çevrimdışı durumdaysa, diske sağ tıklayarak bunları çevrimiçi getirmeyi deneyin ve **Çevrimiçi'yi**tıklatın.|

## <a name="security"></a>Güvenlik

Bu bölümde, Azure VM yedeklemelerinden dosya kurtarma uygulaması için alınan çeşitli güvenlik önlemleri açıklanmaktadır.

### <a name="feature-flow"></a>Özellik akışı

Bu özellik, VM veya VM disklerinin tamamını geri yüklemeye gerek kalmadan ve minimum adım sayısıyla VM verilerine erişmek için oluşturulmuştür. VM verilerine erişim bir komut dosyası tarafından sağlanır (aşağıda gösterildiği gibi çalıştırıldığında kurtarma hacmibağlanır) ve tüm güvenlik uygulamalarının temel taşını oluşturur:

  ![Güvenlik Özellik Akışı](./media/backup-azure-restore-files-from-vm/vm-security-feature-flow.png)

### <a name="security-implementations"></a>Güvenlik uygulamaları

#### <a name="select-recovery-point-who-can-generate-script"></a>Kurtarma noktasını seçin (komut dosyası oluşturabilirsiniz)

Komut dosyası VM verilerine erişim sağlar, bu nedenle ilk etapta kimin oluşturabileceğini düzenlemek önemlidir. Azure portalında oturum açmanız ve komut dosyasını oluşturmak için [RBAC yetkili](backup-rbac-rs-vault.md#mapping-backup-built-in-roles-to-backup-management-actions) olmanız gerekir.

Dosya kurtarma VM geri yükleme ve diskler geri yükleme için gerekli yetkilendirme aynı düzeyde gerektirir. Başka bir deyişle, komut dosyasını yalnızca yetkili kullanıcılar VM verilerini görüntüleyebilir.

Oluşturulan komut dosyası, Azure Yedekleme hizmetiiçin resmi Microsoft sertifikasıyla imzalanır. Komut dosyasında herhangi bir kurcalama imza kırık anlamına gelir ve komut dosyası çalıştırmak için herhangi bir girişim işletim sistemi tarafından potansiyel bir risk olarak vurgulanır.

#### <a name="mount-recovery-volume-who-can-run-script"></a>Kurtarma Dağı hacmi (komut dosyalarını çalıştırabilenler)

Komut dosyasını yalnızca bir Yönetici çalıştırabilir ve komut dosyası nın yükseltilmiş modda çalışması gerekir. Komut dosyası yalnızca önceden oluşturulmuş bir adım kümesini çalıştırıyor ve herhangi bir dış kaynaktan giriş kabul etmiyor.

Komut dosyasını çalıştırmak için, yalnızca Azure portalında veya PowerShell/CLI'de komut dosyası nın üretilmesi sırasında yetkili kullanıcıya gösterilen bir parola gerekir. Bu, komut dosyasını karşıdan yükleyen yetkili kullanıcının da komut dosyasını çalıştırmakla yükümlü olduğundan emin olmak içindir.

#### <a name="browse-files-and-folders"></a>Dosya ve klasörlere göz atın

Dosya ve klasörlere göz atmak için komut dosyası makinedeki iSCSI başlatıcısını kullanır ve iSCSI hedefi olarak yapılandırılan kurtarma noktasına bağlanır. Burada, kişinin taklit etmeye/tüm bileşenleri taklit etmeye/sızdırmaya çalıştığı senaryoları hayal edebilirsiniz.

Her bileşenin diğerini doğrulaması için ortak bir CHAP kimlik doğrulama mekanizması kullanırız. Bu, sahte bir başlatıcının iSCSI hedefine bağlanmasının ve sahte bir hedefin komut dosyasının çalıştırıldığı makineye bağlanmasının son derece zor olduğu anlamına gelir.

Kurtarma hizmeti ile makine arasındaki veri akışı, TCP üzerinde güvenli bir TLS tüneli inşa edilerek korunur[(TLS 1.2](#system-requirements) komut dosyasının çalıştırıldığı makinede desteklenmelidir).

Üst/yedeklenmiş VM'de bulunan tüm dosya Erişim Denetim Listesi (ACL) monte edilmiş dosya sisteminde de korunur.

Komut dosyası bir kurtarma noktasına salt okunur erişim sağlar ve yalnızca 12 saat için geçerlidir. Erişimi daha önce kaldırmak istiyorsanız, Azure Portalı/PowerShell/CLI'de oturum açın ve bu kurtarma noktası için **montajdan kaldırma diskleri** gerçekleştirin. Komut dosyası hemen geçersiz kılınacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- Dosyaları geri alırken karşılaşılan sorunlar için [Sorun Giderme](#troubleshooting) bölümüne bakın
- [Powershell ile dosyaları](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#restore-files-from-an-azure-vm-backup) nasıl geri yükleyin öğrenin
- [Azure CLI ile dosyaları](https://docs.microsoft.com/azure/backup/tutorial-restore-files) nasıl geri yükleyin öğrenin
- VM geri yüklendikten sonra [yedeklemeleri](https://docs.microsoft.com/azure/backup/backup-azure-manage-vms) nasıl yöneteceğimize öğrenin
