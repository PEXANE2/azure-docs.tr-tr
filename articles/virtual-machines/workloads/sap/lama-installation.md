---
title: Azure için SAP LaMa konektörü | Microsoft Dokümanlar
description: SAP LaMa'yı kullanarak Azure'da SAP Sistemlerini Yönetme
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: MSSedusch
manager: timlt
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/29/2019
ms.author: sedusch
ms.openlocfilehash: fda62ff0af29c7cf681d9438b02420d299535701
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80293939"
---
# <a name="sap-lama-connector-for-azure"></a>Azure için SAP LaMa bağlayıcısı

[1877727]:https://launchpad.support.sap.com/#/notes/1877727
[2343511]:https://launchpad.support.sap.com/#/notes/2343511
[2350235]:https://launchpad.support.sap.com/#/notes/2350235
[2562184]:https://launchpad.support.sap.com/#/notes/2562184
[2628497]:https://launchpad.support.sap.com/#/notes/2628497
[2445033]:https://launchpad.support.sap.com/#/notes/2445033
[2815988]:https://launchpad.support.sap.com/#/notes/2815988
[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png
[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md
[hana-ops-guide]:hana-vm-operations.md

> [!NOTE]
> Genel Destek Bildirimi: SAP LaMa veya Azure konektörü için desteğe ihtiyacınız varsa lütfen SAP bileşeni BC-VCM-LVM-HYPERV'de bir olay açın.

SAP LaMa birçok müşteri tarafından SAP manzaralarını çalıştırmak ve izlemek için kullanılır. SAP LaMa 3.0 SP05'ten bu yana varsayılan olarak Azure'a bir konektörle birlikte iletin. Sanal makineleri bulmak ve başlatmak, yönetilen diskleri kopyalamak ve başka bir yere yerleştirmek ve yönetilen diskleri silmek için bu bağlayıcıyı kullanabilirsiniz. Bu temel işlemlerle SAP LaMa'yı kullanarak SAP sistemlerini taşıyabilir, kopyalayabilir, klonlayabilir ve yenileyebilirsiniz.

Bu kılavuzda, SAP LaMa için Azure bağlayıcısını nasıl kurduğunuz, uyarlanabilir SAP sistemlerini yüklemek için kullanılabilecek sanal makineler oluşturma ve bunları nasıl yapılandırabileceğiniz açıklanmaktadır.

> [!NOTE]
> Konektör yalnızca SAP LaMa Enterprise Edition'da kullanılabilir

## <a name="resources"></a>Kaynaklar

Aşağıdaki SAP Notları Azure'daki SAP LaMa konusuyla ilgilidir:

| Not numarası | Başlık |
| --- | --- |
| [2343511] |SAP Yatay Yönetim (LaMa) için Microsoft Azure konektörü |
| [2350235] |SAP Peyzaj Yönetimi 3.0 - Kurumsal baskı |

Ayrıca [SAP LaMa için SAP Yardım Portalı](https://help.sap.com/viewer/p/SAP_LANDSCAPE_MANAGEMENT_ENTERPRISE)okuyun.

## <a name="general-remarks"></a>Genel açıklamalar

* Kurulum -> Ayarları -> Motoru'nda *Otomatik Montaj Noktası Oluşturmayı* etkinleştirmenizi unutmayın  
  SAP LaMa, SANAL Makine'de SAP Adaptif Uzantıları'nı kullanarak hacimleri birikiyorsa, bu ayar etkinleştirilemezse montaj noktasının bulunması gerekir.

* Yeni VM'ler ve SAP örnekleri hazır değilken IP adresinin "çalmasını" önlemek için ayrı alt ağ kullanın ve dinamik IP adresleri kullanmayın  
  SAP LaMa tarafından da kullanılan alt ağda dinamik IP adresi ayırma kullanırsanız, SAP LaMa ile sap sistemi hazırlamak başarısız olabilir. Sap sistemi hazırlanmamışsa, IP adresleri rezerve edilmez ve diğer sanal makinelere tahsis edilebilir.

* Yönetilen ana bilgisayarlarda oturum açarsanız, dosya sistemlerinin sökülmesini engellemeyin  
  Bir Linux sanal makinelerinde oturum açar ve çalışma dizini montaj noktasındaki bir dizin olarak değiştirirseniz ,örneğin /usr/sap/AH1/ASCS00/exe, ses çıkarılamaz ve bir yeniden yerleştirme veya hazırlanmamış başarısız olur.

* SUSE SLES Linux sanal makinelerinde CLOUD_NETCONFIG_MANAGE devre dışı kkullandığınızdan emin olun. Daha fazla bilgi için Bkz. [SUSE KB 7023633.](https://www.suse.com/support/kb/doc/?id=7023633)

## <a name="set-up-azure-connector-for-sap-lama"></a>SAP LaMa için Azure konektörü ayarlama

Azure konektörü SAP LaMa 3.0 SP05 olarak sevk edilir. SAP LaMa 3.0 için her zaman en son destek paketini ve yamasını yüklemenizi öneririz.

Azure bağlayıcısı, Azure kaynaklarınızı yönetmek için Azure Kaynak Yöneticisi API'sini kullanır. SAP LaMa, bu API'ye karşı kimlik doğrulamak için bir Hizmet Sorumlusu veya Yönetilen Kimlik kullanabilir. SAP LaMa'nız bir Azure VM'de [çalışıyorsa, Azure API'sine erişmek için Yönetilen Kimlik kullan](lama-installation.md#af65832e-6469-4d69-9db5-0ed09eac126d)bölümünde açıklandığı gibi Yönetilen Kimlik kullanmanızı öneririz. Bir Hizmet Sorumlusu kullanmak istiyorsanız, [Azure API'sine erişmek için Hizmet Sorumlusu nu kullan bölümündeki](lama-installation.md#913c222a-3754-487f-9c89-983c82da641e)adımları izleyin.

### <a name="use-a-service-principal-to-get-access-to-the-azure-api"></a><a name="913c222a-3754-487f-9c89-983c82da641e"></a>Azure API'sine erişmek için Hizmet Sorumlusu kullanma

Azure bağlayıcısı, Microsoft Azure'a karşı yetkilendirme yapmak için bir Hizmet Sorumlusu kullanabilir. SAP Peyzaj Yönetimi (LaMa) için bir Hizmet Sorumlusu oluşturmak için aşağıdaki adımları izleyin.

1. Şuraya gidin: https://portal.azure.com
1. Azure Active Directory bıçağını açın
1. Uygulama kayıtları için tıklayın
1. Yeni kayıt için tıklayın
1. Bir ad girin ve Kaydol'a tıklayın
1. Yeni Uygulamayı seçin ve Ayarlar sekmesinde Sertifikalar & sırlarına tıklayın
1. Yeni bir istemci sırrı oluşturun, yeni bir anahtar için açıklama girin, sırrın süresinin ne zaman dolduğunu seçin ve Kaydet'e tıklayın
1. Değeri yazın. Hizmet Sorumlusunun şifresi olarak kullanılır
1. Başvuru Kimliğini yazın. Hizmet Sorumlusunun kullanıcı adı olarak kullanılır

Hizmet Sorumlusunun Varsayılan olarak Azure kaynaklarına erişim izni yoktur. Bunlara erişmek için Hizmet Müdürüne izin vermeniz gerekir.

1. Şuraya gidin: https://portal.azure.com
1. Kaynak grupları bıçağını açma
1. Kullanmak istediğiniz kaynak grubunu seçin
1. Erişim denetimine (IAM) tıklayın
1. Rol Atama ekle'ye tıklayın
1. Katkıda Bulunan rolü seçin
1. Yukarıda oluşturduğunuz uygulamanın adını girin
1. Kaydet’e tıklayın.
1. SAP LaMa'da kullanmak istediğiniz tüm kaynak grupları için 3 ile 8 arası adımı yineleyin

### <a name="use-a-managed-identity-to-get-access-to-the-azure-api"></a><a name="af65832e-6469-4d69-9db5-0ed09eac126d"></a>Azure API'sine erişmek için Yönetilen Kimlik kullanma

Yönetilen Kimlik kullanabilmek için SAP LaMa örneğinizin sistem veya kullanıcı tarafından atanmış bir kimliğe sahip bir Azure VM'de çalışması gerekir. Yönetilen Kimlikler hakkında daha fazla bilgi için [Azure kaynakları için yönetilen kimlikler nedir?](../../../active-directory/managed-identities-azure-resources/overview.md) ve Azure [portalını kullanarak Bir VM'de Azure kaynakları için yönetilen kimlikleri yapılandırma'yı](../../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)okuyun.

Yönetilen Kimlik temerrüt olarak Azure kaynaklarına erişmek için izinlere sahip değildir. Bunlara erişmek için izin vermeniz gerekir.

1. Şuraya gidin: https://portal.azure.com
1. Kaynak grupları bıçağını açma
1. Kullanmak istediğiniz kaynak grubunu seçin
1. Erişim denetimine (IAM) tıklayın
1. Rol Ekle -> Ekle ataması'na tıklayın
1. Katkıda Bulunan rolü seçin
1. 'Access'e atamak' için 'Sanal Makine'yi seçin
1. SAP LaMa örneğinizin çalıştığı sanal makineyi seçin
1. Kaydet’e tıklayın.
1. SAP LaMa'da kullanmak istediğiniz tüm kaynak grupları için adımları yineleme

SAP LaMa Azure konektör yapılandırmanızda, Yönetilen Kimlik kullanımını etkinleştirmek için 'Yönetilen Kimliği Kullan'ı seçin. Atanmış bir kimlik kullanmak istiyorsanız, Kullanıcı Adı alanını boş bıraktığından emin olun. Kullanıcı tarafından atanan bir kimlik kullanmak istiyorsanız, kullanıcı tarafından atanan kimlik kimliğini Kullanıcı Adı alanına girin.

### <a name="create-a-new-connector-in-sap-lama"></a>SAP LaMa'da yeni bir bağlayıcı oluşturma

SAP LaMa web sitesini açın ve Altyapı'ya gidin. Bulut Yöneticileri sekmesine gidin ve Ekle'ye tıklayın. Microsoft Azure Bulut Bağdaştırıcısını seçin ve İleri'yi tıklatın. Aşağıdaki bilgileri girin:

* Etiket: Bağlayıcı örneği için bir ad seçin
* Kullanıcı Adı: Hizmet Müdürü Başvuru Kimliği veya sanal makinenin kullanıcı tarafından atanan kimliği. Daha fazla bilgi için [Sistem veya Kullanıcı Tarafından Atanan Kimlik Kullanma]
* Şifre: Servis Müdürü anahtarı/şifresi. Bir sistem veya kullanıcı tarafından atanan bir kimlik kullanıyorsanız, bu alanı boş bırakabilirsiniz.
* URL: Varsayılan ı koru`https://management.azure.com/`
* İzleme Aralığı (Saniye): En az 300 olmalıdır
* Yönetilen Kimliği Kullanma: SAP LaMa, Azure API'sine karşı kimlik doğrulamak için bir sistem veya kullanıcı tarafından atanan bir kimlik kullanabilir. Bkz. bu kılavuzda [Azure API'sine erişmek için Yönetilen Kimlik kullan.](lama-installation.md#af65832e-6469-4d69-9db5-0ed09eac126d)
* Abonelik Kimliği: Azure abonelik kimliği
* Azure Active Directory Kiracı Kimliği: Etkin Dizin kiracısının kimliği
* Proxy ana bilgisayar: SAP LaMa internete bağlanmak için bir proxy gerekiyorsa proxy Hostname
* Proxy bağlantı noktası: Proxy'nin TCP bağlantı noktası
* Maliyetlerden tasarruf etmek için Depolama Türünü değiştir: Azure Bağdaştırıcısı, diskler kullanılmadığında maliyetlerden tasarruf etmek için Yönetilen Disklerin depolama türünü değiştirmesi gerekiyorsa bu ayarı etkinleştirin. SAP örnek yapılandırmasında başvurulan veri diskleri için bağdaştırıcı, bir örnek hazırlanamadan disk türünü Standart Depolama olarak değiştirir ve örnek hazırlama sırasında özgün depolama türüne geri döner. SAP LaMa'da sanal bir makineyi durdurursanız, bağdaştırıcı işletim sistemi diski de dahil olmak üzere bağlı tüm disklerin depolama türünü Standart Depolama olarak değiştirir. SAP LaMa'da sanal bir makine başlatırsanız, bağdaştırıcı depolama türünü orijinal depolama türüne geri döndürür.

Girişinizi doğrulamak için Test Yapılandırması'nı tıklatın. Görmelisin.

Bağlantı başarılı: Microsoft bulutbağlantısı başarılı oldu. 7 kaynak grubu bulundu (yalnızca 10 grup talep edildi)

web sitesinin alt kısmında.

## <a name="provision-a-new-adaptive-sap-system"></a>Yeni bir adaptif SAP sistemi sağlama

Yeni bir sanal makineyi el ile dağıtabilir veya [hızlı başlatma deposunda](https://github.com/Azure/azure-quickstart-templates)Azure şablonlarından birini kullanabilirsiniz. [Sap NetWeaver ASCS, SAP](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-lama-ascs) [NetWeaver uygulama sunucuları](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-lama-apps)ve [veritabanı](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-lama-database)için şablonlar içerir. Ayrıca, sistem kopyalama/klonlama vb. bir parçası olarak yeni ana bilgisayarlar sağlamak için bu şablonları kullanabilirsiniz.

SAP LaMa ile yönetmek istediğiniz tüm sanal makineler için ayrı bir alt ağ kullanmanızı öneririz ve yeni sanal makineleri dağıtırken IP adresinin "çalınmasını" önlemek için dinamik IP adresleri kullanmayın ve SAP örnekleri hazırlıksız.

> [!NOTE]
> Mümkünse, sanal makineden diskleri ayırmak için uzun çalışma sürelerine neden olabilecek tüm sanal makine uzantılarını kaldırın.

Kullanıcı \<hanasid>adm, \<sapsid>adm ve grup sapsys aynı kimlik ve gid veya LDAP kullanmak ile hedef makinede var olduğundan emin olun. SAP NetWeaver (A)SCS'yi çalıştırmak için kullanılması gereken sanal makinelerde NFS sunucusunu etkinleştirin ve başlatın.

### <a name="manual-deployment"></a>El ile dağıtma

SAP LaMa, SAP Ana Bilgisayar Aracısı'nı kullanarak sanal makineyle iletişim kurar. Sanal makineleri el ile dağıtıyorsanız veya hızlı başlangıç deposundan Azure Kaynak Yöneticisi şablonunu kullanmıyorsanız, en son SAP Ana Bilgisayar Aracısını ve SAP Uyarlamalı Uzantıları'nı yüklediğinizden emin olun. Azure için gerekli yama düzeyleri hakkında daha fazla bilgi için SAP Note [2343511'e]bakın.

#### <a name="manual-deployment-of-a-linux-virtual-machine"></a>Linux Sanal Makine manuel dağıtım

SAP Note [2343511'de]listelenen desteklenen işletim sistemlerinden biriyle yeni bir sanal makine oluşturun. SAP örnekleri için ek IP yapılandırmaları ekleyin. Her örneğin en azından IP adresine ihtiyacı vardır ve sanal bir ana bilgisayar adı kullanılarak yüklenmesi gerekir.

SAP NetWeaver ASCS örneği /sapmnt/\<SAPSID>, /usr/sap/\<SAP / SAPSID>, /usr/sap/trans ve /usr/sap/\<sapsid>adm için disklere ihtiyaç duyar. SAP NetWeaver uygulama sunucularının ek disklere ihtiyacı yoktur. SAP örneğine ilişkin her şey ASCS'de depolanmış ve NFS üzerinden dışa aktarılmalıdır. Aksi takdirde, şu anda SAP LaMa kullanarak ek uygulama sunucuları eklemek mümkün değildir.

![Linux'ta SAP NetWeaver ASCS](media/lama/sap-lama-ascs-app-linux.png)

#### <a name="manual-deployment-for-sap-hana"></a>SAP HANA için manuel dağıtım

SAP Note [2343511'de]listelenen SAP HANA için desteklenen işletim sistemlerinden biriyle yeni bir sanal makine oluşturun. SAP HANA için bir ek IP yapılandırması ve HANA kiracı başına bir tane ekleyin.

SAP HANA 'nın /hana/shared, /hana/backup, /hana/data ve /hana/log için disklere ihtiyacı vardır

![LINUX üzerinde SAP HANA](media/lama/sap-lama-db-hana.png)

#### <a name="manual-deployment-for-oracle-database-on-linux"></a>Linux'ta Oracle Veritabanı için manuel dağıtım

SAP Note [2343511'de]listelenen Oracle veritabanları için desteklenen işletim sistemlerinden biriyle yeni bir sanal makine oluşturun. Oracle veritabanı için bir ek IP yapılandırması ekleyin.

Oracle veritabanı için /oracle, /home/oraod1 ve /home/oracle için disklere ihtiyaç duyar

![Linux üzerinde Oracle veritabanı](media/lama/sap-lama-db-ora-lnx.png)

#### <a name="manual-deployment-for-microsoft-sql-server"></a>Microsoft SQL Server için manuel dağıtım

SAP Note [2343511'de]listelenen Microsoft SQL Server için desteklenen işletim sistemlerinden biriyle yeni bir sanal makine oluşturun. SQL Server örneği için bir ek IP yapılandırması ekleyin.

SQL Server veritabanı sunucusu, veritabanı verileri için disklere ve c:\usr\sap için dosya ve disklere ihtiyaç duyar.

![Linux üzerinde Oracle veritabanı](media/lama/sap-lama-db-sql.png)

BIR SAP NetWeaver uygulama sunucusunu sistem kopya/klon hedefine veya olarak taşımak için kullanmak istediğiniz sanal bir makineye SQL Server için desteklenen bir Microsoft ODBC sürücüsü yüklediğinizden emin olun.

SAP LaMa, SQL Server'ın kendisini başka bir yere taşıyamaz, böylece bir veritabanı örneğini bir veritabanı örneğini veya sistem kopya/klon hedefi olarak taşımak için kullanmak istediğiniz sanal bir makinenin SQL Server'A önceden yüklenmesi gerekir.

### <a name="deploy-virtual-machine-using-an-azure-template"></a>Azure Şablonu Kullanarak Sanal Makineyi Dağıtma

Sanal makinelerin işletim sistemi için [SAP Yazılım Pazarı'ndan](https://support.sap.com/swdc) aşağıdaki en son mevcut arşivleri indirin:

1. SAPCAR 7.21
1. SAP HOST AGENT 7.21
1. SAP ADAPTIF UZATMA 1.0 EXT

Ayrıca Aşağıdaki bileşenleri [Microsoft Download Center'dan](https://www.microsoft.com/download) indirin

1. Microsoft Visual C++ 2010 Yeniden Dağıtılabilir Paket (x64) (yalnızca Windows)
1. SQL Server için Microsoft ODBC Sürücüsü (yalnızca SQL Server)

Şablonu dağıtmak için bileşenler gereklidir. Şablonda kullanılabilir hale getirmenin en kolay yolu, bunları bir Azure depolama hesabına yüklemek ve Paylaşılan Erişim İmzası (SAS) oluşturmaktır.

Şablonlar aşağıdaki parametrelere sahiptir:

* sapSystemId: SAP sistem kimliği. Disk düzenini oluşturmak için kullanılır (örneğin /usr/sap/\<sapsid>).

* computerName: Yeni sanal makinenin bilgisayar adı. Bu parametre SAP LaMa tarafından da kullanılır. Bu şablonu, sistem kopyasının bir parçası olarak yeni bir sanal makine sağlamak için kullandığınızda, SAP LaMa bu bilgisayar adını taşıyan ana bilgisayara ulaşılana kadar bekler.

* osType: Dağıtmak istediğiniz işletim sisteminin türü.

* dbtype: Veritabanının türü. Bu parametre, kaç ek IP yapılandırması eklenmesi gerektiğini ve disk düzeninin nasıl görünmesi gerektiğini belirlemek için kullanılır.

* sapSystemSize: Dağıtmak istediğiniz SAP Sisteminin boyutu. Sanal makine örneği türünü ve boyutunu belirlemek için kullanılır.

* adminUsername: Sanal makine için kullanıcı adı.

* adminPassword: Sanal makine için parola. Ayrıca SSH için ortak bir anahtar da sağlayabilirsiniz.

* sshKeyData: Sanal makineler için ortak SSH anahtarı. Yalnızca Linux işletim sistemleri için desteklenir.

* subnetId: Kullanmak istediğiniz alt netin kimliği.

* deployEmptyTarget: Sanal makineyi bir örneğin taşınması veya benzer bir şekilde hedef olarak kullanmak istiyorsanız boş bir hedef dağıtabilirsiniz. Bu durumda, ek diskler veya IP yapılandırmaları iliştirilir.

* sapcarLocation: Dağıttığınız işletim sistemiyle eşleşen sapcar uygulamasının konumu. sapcar diğer parametrelerde sağladığınız arşivleri ayıklamak için kullanılır.

* sapHostAgentArchiveLocation: SAP Host Agent arşivinin konumu. SAP Ana Bilgisayar Aracısı bu şablon dağıtımının bir parçası olarak dağıtılır.

* sapacExtLocation: SAP Adaptif Uzantılarının konumu. SAP Note [2343511,] Azure için gereken minimum yama düzeyini listeler.

* vcRedistLocation: SAP Adaptif Uzantıları yüklemek için gerekli olan VC Çalışma Zamanı'nın konumu. Bu parametre yalnızca Windows için gereklidir.

* odbcDriverLocation: Yüklemek istediğiniz ODBC sürücüsünün konumu. SQL Server için yalnızca Microsoft ODBC sürücüsü desteklenir.

* sapadmPassword: sapadm kullanıcı için şifre.

* sapadmId: Sapadm kullanıcısının Linux Kullanıcı Kimliği. Windows için gerekli değildir.

* sapsysGid: Sapsys grubunun Linux grup kimliği. Windows için gerekli değildir.

* _artifactsLocation: Bu şablonun gerektirdiği eserlerin bulunduğu temel URI. Şablon, eşlik eden komut dosyaları kullanılarak dağıtıldığında, abonelikteki özel bir konum kullanılır ve bu değer otomatik olarak oluşturulur. Yalnızca şablonu GitHub'dan dağıtmazsanız gereklidir.

* _artifactsLocationSasToken: _artifactsLocation erişmek için gerekli sasToken. Şablon, beraberindeki komut dosyaları kullanılarak dağıtıldığında, bir sasToken otomatik olarak oluşturulur. Yalnızca şablonu GitHub'dan dağıtmazsanız gereklidir.

### <a name="sap-hana"></a>SAP HANA

Aşağıdaki örneklerde, SAP HANA'yı sistem Kimliği HN1 ve SAP NetWeaver sistemini AH1 sistem kimliğiile yüklediğinizi varsayıyoruz. Sanal ana bilgisayar adları HANA örneği için hn1-db, SAP NetWeaver sistemi tarafından kullanılan HANA kiracı için ah1-db, SAP NetWeaver ASCS için ah1-ascs ve ilk SAP NetWeaver uygulama sunucusu için ah1-di-0'dır.

#### <a name="install-sap-netweaver-ascs-for-sap-hana-using-azure-managed-disks"></a>Azure Yönetilen Diskleri kullanarak SAP HANA için SAP NetWeaver ASCS'yi yükleyin

SAP Yazılım Sağlama Yöneticisi'ni (SWPM) başlatmadan önce, ASCS'nin sanal ana bilgisayar adının IP adresini takmanız gerekir. Önerilen yol sapacext kullanmaktır. IP adresini sapacext kullanarak monte ederseniz, yeniden başlatıldıktan sonra IP adresini yeniden monte ettiğinizden emin olun.

![Linux][Logo_Linux] Linux

```bash
# /usr/sap/hostctrl/exe/sapacext -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
/usr/sap/hostctrl/exe/sapacext -a ifup -i eth0 -h ah1-ascs -n 255.255.255.128
```

![Windows][Logo_Windows] Windows

```bash
# C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i "Ethernet 3" -h ah1-ascs -n 255.255.255.128
```

SWPM çalıştırın ve *ASCS Instance Host Name*için *ah1-ascs* kullanın.

![Linux][Logo_Linux] Linux  
/usr/sap/hostctrl/exe/host_profile adresinde bulunan SAP Ana Bilgisayar Aracısı profiline aşağıdaki profil parametresini ekleyin. Daha fazla bilgi için SAP Note [2628497'ye]bakın.
```
acosprep/nfs_paths=/home/ah1adm,/usr/sap/trans,/sapmnt/AH1,/usr/sap/AH1
```

#### <a name="install-sap-netweaver-ascs-for-sap-hana-on-azure-netappfiles-anf-beta"></a>AZURE NetAppFiles (ANF) BETA'ya SAP HANA için SAP NetWeaver ASCS'yi yükleyin

> [!NOTE]
> Bu işlevsellik ne de GA henüz. Daha fazla bilgi için SAP Note [2815988'e] bakın (yalnızca önizleme müşterileri tarafından görülebilir).
BC-VCM-LVM-HYPERV bileşeninde bir SAP olayını açın ve Azure NetApp Files önizlemesi için LaMa depolama bağdaştırıcısına katılma isteğinde bulunun

ANF, Azure için NFS sağlar. SAP LaMa bağlamında bu, ABAP Merkezi Hizmetler (ASCS) örneklerinin oluşturulmasını ve uygulama sunucularının sonraki yüklemesini kolaylaştırır. Daha önce ASCS örneğinin NFS sunucusu olarak da hareket etmesi ve acosprep/nfs_paths parametresi SAP Hostagent'ın host_profile eklenmesi gerekiyordu.

#### <a name="anf-is-currently-available-in-these-regions"></a>ANF şu anda şu bölgelerde kullanılabilir:

Avustralya Doğu, Orta ABD, Doğu ABD, Doğu ABD 2, Kuzey Avrupa, Güney Orta ABD, Batı Avrupa ve Batı ABD 2.

#### <a name="network-requirements"></a>Ağ Gereksinimleri

ANF, SAP sunucuları ile aynı VNET'in bir parçası olması gereken bir temsilci alt net gerektirir. İşte böyle bir yapılandırma için bir örnek.
Bu ekran VNET ve ilk alt net oluşturulmasını gösterir:

![SAP LaMa, Azure ANF için sanal ağ oluşturuyor ](media/lama/sap-lama-createvn-50.png)

Bir sonraki adım, Microsoft.NetApp/volumes için devredilen alt ağı oluşturur.

![SAP LaMa delege alt ağı ekleyin ](media/lama/sap-lama-addsubnet-50.png)

![SAP LaMa alt ağlar listesi ](media/lama/sap-lama-subnets.png)

Azure portalında bir NetApp hesabı oluşturulması gerekiyor:

![SAP LaMa NetApp hesabı oluşturmak ](media/lama/sap-lama-create-netappaccount-50.png)

![SAP LaMa NetApp hesabı oluşturuldu ](media/lama/sap-lama-netappaccount.png)

NetApp hesabında kapasite havuzu her havuz için disklerin boyutunu ve türünü belirtir:

![SAP LaMa NetApp kapasite havuzu oluşturmak ](media/lama/sap-lama-capacitypool-50.png)

![SAP LaMa NetApp kapasite havuzu oluşturuldu ](media/lama/sap-lama-capacitypool-list.png)

NFS birimleri artık tanımlanabilir. Bir havuzda birden çok sistem için birimler olacağından, kendi kendini açıklayan bir adlandırma şeması seçilmelidir. SID'nin eklenmesi, ilgili birimleri bir araya getirmek için yardımcı olur. ASCS ve AS örneği için aşağıdaki bağlar gereklidir: */sapmnt/\<\>SID*, */usr/sap/\<SID\>*, ve */home/\<sid\>adm*. İsteğe bağlı olarak, */usr/sap/trans,* en azından tek bir manzaranın tüm sistemleri tarafından kullanılan merkezi taşıma dizini için gereklidir.

> [!NOTE]
> BETA aşamasında, birimlerin adı abonelik içinde benzersiz olmalıdır.

![SAP LaMa bir birim 1 oluşturmak ](media/lama/sap-lama-createvolume-80.png)

![SAP LaMa bir hacim 2 oluşturmak ](media/lama/sap-lama-createvolume2-80.png)

![SAP LaMa bir hacim 3 oluşturmak ](media/lama/sap-lama-createvolume3-80.png)

Bu adımların diğer birimler için de yinelemi gerekir.

![Oluşturulan birimlerin SAP LaMa listesi ](media/lama/sap-lama-volumes.png)

Şimdi bu birimlerin SAP SWPM ile ilk yüklemenin gerçekleştirileceği sistemlere monte edilmesi gerekiyor.

Önce montaj noktaları oluşturulmalıdır. Bu durumda SID AN1 olduğundan aşağıdaki komutların yürütülmesi gerekir:

```bash
mkdir -p /home/an1adm
mkdir -p /sapmnt/AN1
mkdir -p /usr/sap/AN1
mkdir -p /usr/sap/trans
```
Sonraki ANF birimleri aşağıdaki komutlarla monte edilecektir:

```bash
# sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=3,tcp 9.9.9.132:/an1-home-sidadm /home/an1adm
# sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=3,tcp 9.9.9.132:/an1-sapmnt-sid /sapmnt/AN1
# sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=3,tcp 9.9.9.132:/an1-usr-sap-sid /usr/sap/AN1
# sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=3,tcp 9.9.9.132:/global-usr-sap-trans /usr/sap/trans
```
Montaj komutları da portaldan türetilebilir. Yerel montaj noktalarının ayarlanması gerekir.

Doğrulamak için df -h komutunu kullanın.

![SAP LaMa montaj noktaları Işletim Sistemi düzeyi ](media/lama/sap-lama-mounts.png)

Şimdi SWPM ile yükleme yapılmalıdır.

Aynı adımlar en az bir AS örneği için gerçekleştirilmelidir.

Başarılı kurulumdan sonra sistem SAP LaMa içinde keşfedilmelidir.

Montaj noktaları ASCS ve AS örneği için aşağıdaki gibi görünmelidir:

![LaMa ](media/lama/sap-lama-ascs.png) SAP LaMa montaj noktaları (Bu bir örnektir. IP adresleri ve dışa aktarma yolu daha önce kullanılanlardan farklıdır)


#### <a name="install-sap-hana"></a>SAP HANA yükleme

HDBLCM komut satırı aracını kullanarak SAP HANA'yı yüklerseniz, sanal bir ana bilgisayar adı sağlamak için parametre -hostname' yi kullanın. Veritabanının sanal ana bilgisayar adının IP adresini bir ağ arabirimine eklemeniz gerekir. Önerilen yol sapacext kullanmaktır. IP adresini sapacext kullanarak monte ederseniz, yeniden başlatıldıktan sonra IP adresini yeniden monte ettiğinizden emin olun.

HANA kiracısına bağlanmak için uygulama sunucuları tarafından kullanılan ad için başka bir sanal ana bilgisayar adı ve IP adresi ekleyin.

```bash
# /usr/sap/hostctrl/exe/sapacext -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
/usr/sap/hostctrl/exe/sapacext -a ifup -i eth0 -h hn1-db -n 255.255.255.128
/usr/sap/hostctrl/exe/sapacext -a ifup -i eth0 -h ah1-db -n 255.255.255.128
```

SWPM veritabanı örneği yüklemesini HANA sanal makinesinde değil, uygulama sunucusu sanal makinesinde çalıştırın. *SAP Sistemi*için iletişim Veritabanı *Veritabanı* veritabanı için *ah1-db* kullanın.

#### <a name="install-sap-netweaver-application-server-for-sap-hana"></a>SAP HANA için SAP NetWeaver Uygulama Sunucusu'nu Kurun

SAP Yazılım Sağlama Yöneticisi'ni (SWPM) başlatmadan önce, uygulama sunucusunun sanal ana bilgisayar adının IP adresini takmanız gerekir. Önerilen yol sapacext kullanmaktır. IP adresini sapacext kullanarak monte ederseniz, yeniden başlatıldıktan sonra IP adresini yeniden monte ettiğinizden emin olun.

![Linux][Logo_Linux] Linux

```bash
# /usr/sap/hostctrl/exe/sapacext -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
/usr/sap/hostctrl/exe/sapacext -a ifup -i eth0 -h ah1-di-0 -n 255.255.255.128
```

![Windows][Logo_Windows] Windows

```bash
# C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i "Ethernet 3" -h ah1-di-0 -n 255.255.255.128
```

HDB kullanıcı mağazasında anahtarı bulmak için kullanılan kimliği ayarlamak için SAP NetWeaver profil parametresi dbs/hdb/hdb_use_ident kullanılması önerilir. SWPM ile veritabanı örneği yüklemeden sonra bu parametreyi el ile ekleyebilir veya SWPM

```bash
# from https://blogs.sap.com/2015/04/14/sap-hana-client-software-different-ways-to-set-the-connectivity-data/
/sapdb/DVDs/IM_LINUX_X86_64/sapinst HDB_USE_IDENT=SYSTEM_COO
```

El ile ayarlarsanız, yeni HDB kullanıcı mağazası girişleri de oluşturmanız gerekir.

```bash
# run as <sapsid>adm
/usr/sap/AH1/hdbclient/hdbuserstore LIST
# reuse the port that was listed from the command above, in this example 35041
/usr/sap/AH1/hdbclient/hdbuserstore SET DEFAULT ah1-db:35041@AH1 SAPABAP1 <password>
```

*Birincil Uygulama Sunucusu Örneği'ndeki*PAS Instance Host *Adı* için *ah1-di-0'ı* kullanın.

#### <a name="post-installation-steps-for-sap-hana"></a>SAP HANA için Kurulum Sonrası Adımlar

Kiracı kopyasını, kiracıyı taşımaya veya sistem çoğaltma oluşturmaya çalışmadan önce SYSTEMDB'yi ve tüm kiracı veritabanlarını yedeklediğinizden emin olun.

### <a name="microsoft-sql-server"></a>Microsoft SQL Server

Aşağıdaki örneklerde, SAP NetWeaver sistemini sistem kimliği AS1 ile yüklediğinizi varsayıyoruz. Sanal ana bilgisayar adları SAP NetWeaver sistemi tarafından kullanılan SQL Server örneği için 1-db, SAP NetWeaver ASCS için as1-ascs ve ilk SAP NetWeaver uygulama sunucusu için as1-di-0'dır.

#### <a name="install-sap-netweaver-ascs-for-sql-server"></a>SQL Server için SAP NetWeaver ASCS'yi yükleyin

SAP Yazılım Sağlama Yöneticisi'ni (SWPM) başlatmadan önce, ASCS'nin sanal ana bilgisayar adının IP adresini takmanız gerekir. Önerilen yol sapacext kullanmaktır. IP adresini sapacext kullanarak monte ederseniz, yeniden başlatıldıktan sonra IP adresini yeniden monte ettiğinizden emin olun.

```bash
# C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i "Ethernet 3" -h as1-ascs -n 255.255.255.128
```

SWPM çalıştırın ve *ASCS Instance Host Name*için *as1-ascs* kullanın.

#### <a name="install-sql-server"></a>SQL Server Yükleme

Veritabanının sanal ana bilgisayar adının IP adresini bir ağ arabirimine eklemeniz gerekir. Önerilen yol sapacext kullanmaktır. IP adresini sapacext kullanarak monte ederseniz, yeniden başlatıldıktan sonra IP adresini yeniden monte ettiğinizden emin olun.

```bash
# C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i "Ethernet 3" -h as1-db -n 255.255.255.128
```

SQL sunucu sanal makinede SWPM veritabanı örneği yükleme çalıştırın. SQL Server'a bağlanmak için kullanılan ana bilgisayar adını geçersiz kılmak için SAPINST_USE_HOSTNAME=*as1-db'yi* kullanın. Sanal makineyi Azure Kaynak Yöneticisi şablonuna kullanarak dağıttıysanız, veritabanı veri dosyaları için kullanılan dizini *C:\sql\data* ve veritabanı günlük dosyası *c:\sql\log'a*ayarladığınızdan emin olun.

Kullanıcı *NT AUTHORITY\SYSTEM* SQL Server erişimi ve sunucu rolü *sysadmin*olduğundan emin olun. Daha fazla bilgi için SAP Note [1877727] ve [2562184'e]bakın.

#### <a name="install-sap-netweaver-application-server"></a>SAP NetWeaver Uygulama Sunucusu'nu Kurun

SAP Yazılım Sağlama Yöneticisi'ni (SWPM) başlatmadan önce, uygulama sunucusunun sanal ana bilgisayar adının IP adresini takmanız gerekir. Önerilen yol sapacext kullanmaktır. IP adresini sapacext kullanarak monte ederseniz, yeniden başlatıldıktan sonra IP adresini yeniden monte ettiğinizden emin olun.

```bash
# C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i "Ethernet 3" -h as1-di-0 -n 255.255.255.128
```

*Birincil Uygulama Sunucusu Örneği'nde*PAS Instance Host *Adı* için *as1-di-0'ı* kullanın.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="errors-and-warnings-during-discover"></a>Keşfet sırasında Hatalar ve Uyarılar

* SELECT izni reddedildi
  * [Microsoft] [ODBC SQL Server Sürücüsü] [SQL Server] SELECT izni 'log_shipping_primary_databases', veritabanı 'msdb', şema 'dbo' nesnesi üzerinde reddedildi. [SOAPFaultException]  
  SELECT izni 'log_shipping_primary_databases', veritabanı 'msdb', şema 'dbo' nesnesi üzerinde reddedildi.
  * Çözüm  
    *NT AUTHORITY\SYSTEM'in* SQL Server'a erişebilmesini unutmayın. Bkz. SAP Notu [2562184]


### <a name="errors-and-warnings-for-instance-validation"></a>Örnek Doğrulama için Hatalar ve Uyarılar

* HDB kullanıcı mağazasının doğrulanmasında bir özel durum ortaya çıktı  
  * bkz.  
    com.sap.nw.lm.aci.monitor.api.validation.RuntimeValidationException: ID 'RuntimeHDBConnectionValidator' (Doğrulama: 'VALIDATION_HDB_USERSTORE'): Hdbuserstore'u alamadım  
    HANA userstore doğru konumda değil
  * Çözüm  
    /usr/sap/AH1/hdbclient/install/installation.ini'nin doğru olduğundan emin olun

### <a name="errors-and-warnings-during-a-system-copy"></a>Sistem Kopyalama sırasında hatalar ve uyarılar

* Sistem sağlama adımı doğrularken bir hata oluştu
  * Neden: com.sap.nw.lm.aci.engine.base.api.util.exception.HAOperationException Arama '/usr/sap/hostctrl/exe/sapacext -a ShowHanaBackups -m HN 1 -f 50 -hn1-db -o seviye=0\;durum=5\;port=35013 pf=/usr/sap/hostctrl/exe/host_profile -R -T dev_lvminfo -u SİsteM -p kanca -r' | /usr/sap/hostctrl/exe/sapacext -a ShowHanaBackups -m HN1 -f 50 -h hn1-db -o level=0\;status=5\;port=35013 pf=/usr/sap/hostctrl/exe/host_profile -R -T dev_lvminfo -u System -p hook -r
  * Çözüm  
    Kaynak HANA sistemindeki tüm veritabanlarının yedekini alın

* Sistem Kopya Adım Veritabanı örneğinin *Başlangıcı*
  * Host Agent Operation '000D3A282BC91EE8A1D76CF1F92E2944' başarısız oldu (OperationException. FaultCode: '127', Mesaj: 'Komut yürütme başarısız oldu. : [Microsoft][ODBC SQL Server Driver][SQL Server]Kullanıcıveritabanı 'AS2'yi değiştirme izni ne var, veritabanı yok veya veritabanı erişim denetimlerine izin veren bir durumda değil.')
  * Çözüm  
    *NT AUTHORITY\SYSTEM'in* SQL Server'a erişebilmesini unutmayın. Bkz. SAP Notu [2562184]

### <a name="errors-and-warnings-during-a-system-clone"></a>Sistem Klonu Sırasında Hatalar ve Uyarılar

* Uygulama sunucusunun veya ASCS'nin Zorla *Kaydet ve Başlangıç Örneği* Aracısı'na örnek aracıyı kaydetmeye çalışırken hata oluştu
  * Örnek aracıyı kaydetmeye çalışırken hata oluştu. (RemoteException: 'Profilden örnek verileri\\yükleyemedi ' ascs\sapmnt\AS1\SYS\profile\AS1_D00_as1-di-0': Profil\\' as1-ascs\sapmnt\AS1\SYS\profile\AS1_D00_as1-di-0': Böyle bir dosya veya dizin yok.')
  * Çözüm  
   ASCS/SCS'deki sapmnt paylaşımının SAP_AS1_GlobalAdmin için Tam Erişime sahip olduğundan emin olun

* Adımdaki hata *Klon için Başlangıç Korumasını Etkinleştir*
  * Dosyayı açamayan\\' ascs\sapmnt\AS1\SYS\profile\AS1_D00_as1-di-0' Nedeni: Böyle bir dosya veya dizin yok
  * Çözüm  
    Uygulama sunucusunun bilgisayar hesabının profile yazma erişimi ne ihtiyacı var

### <a name="errors-and-warnings-during-create-system-replication"></a>Sistem Çoğaltma Oluşturma sırasında Hatalar ve Uyarılar

* Sistem Çoğaltma Oluştur'a tıkladığınızda özel durum
  * Neden: com.sap.nw.lm.aci.engine.base.api.util.exception.HAOperationException Arama '/usr/sap/hostctrl/exe/sapacext -a ShowHanaBackups -m HN 1 -f 50 -hn1-db -o seviye=0\;durum=5\;port=35013 pf=/usr/sap/hostctrl/exe/host_profile -R -T dev_lvminfo -u SİsteM -p kanca -r' | /usr/sap/hostctrl/exe/sapacext -a ShowHanaBackups -m HN1 -f 50 -h hn1-db -o level=0\;status=5\;port=35013 pf=/usr/sap/hostctrl/exe/host_profile -R -T dev_lvminfo -u System -p hook -r
  * Çözüm  
    sapacext>adm `<hanasid` olarak yürütülebilir test

* Depolama Adımı'nda tam kopya etkinleştirilemediğinde hata
  * Yol iStorageCopyData.storageVolumeCopyList:1 ve alan hedefiStorageSystemId için bir bağlam öznitelik iletisi rapor ederken bir hata oluştu
  * Çözüm  
    Uyarıları adım adım yoksayın ve yeniden deneyin. Bu sorun SAP LaMa'nın yeni bir destek paketinde/yamasında giderilir.

### <a name="errors-and-warnings-during-relocate"></a>Yer Değiştirme Sırasında Hatalar ve Uyarılar

* NFS yeniden ihracatı için yol '/usr/sap/AH1' izin verilmez.
  * Ayrıntılar için SAP Note [2628497'ye] bakın.
  * Çözüm  
    ASCS HostAgent Profiline ASCS dışa aktarma ekleyin. Bkz. SAP Notu [2628497]

* ASCS'nin taşınması sırasında uygulanmayan işlev
  * Komut Çıktısı: exportfs: host:/usr/sap/AX1: İşlev uygulanmadı
  * Çözüm  
    NFS sunucu hizmetinin yeniden konumdeğiştirilen hedef sanal makinede etkin olduğundan emin olun

### <a name="errors-and-warnings-during-application-server-installation"></a>Uygulama Sunucusu Kurulumu Sırasında Hatalar ve Uyarılar

* Hata yürütme SAPinst adımı: getProfileDir
  * HATA: (Son hata adım tarafından bildirilen: Yakalanan ESAPinstException modül arama: Adım ın doğrulayıcısı '| NW_DI|ind|ind|ind|ind|0|0| NW_GetSidFromProfiles|ind|ind|ind|ind|getSid|0| NW_readProfileDir|ind|ind|ind|ind|ind|readProfile|0|getProfileDir' bir \\hata bildirdi: Düğüm \as1-ascs\sapmnt\AS1\SYS\profil yok. Bu sorunu çözmek için sapinst'i etkileşimli modda başlatın)
  * Çözüm  
    SWPM'nin profile erişimi olan bir kullanıcıyla birlikte çalıştığını unutmayın. Bu kullanıcı Application Server Installation sihirbazında yapılandırılabilir

* Hata yürütme SAPinst adım: askUnicode
  * HATA: (Son hata adım tarafından bildirilen: Yakalanan ESAPinstException modül arama: Adım ın doğrulayıcısı '| NW_DI|ind|ind|ind|ind|0|0| NW_GetSidFromProfiles|ind|ind|ind|ind|getSid|0| NW_getUnicode|ind|ind|ind|ind|ind|ind|unicode|0|askUnicode' bir hata bildirdi: Bu sorunu çözmek için etkileşimli modda SAPinst'i başlatın)
  * Çözüm  
    Yeni bir SAP çekirdeği kullanıyorsanız, SWPM sistemin artık ASCS'nin ileti sunucusunu kullanarak tek kodlu bir sistem olup olmadığını belirleyemez. Daha fazla bilgi için SAP Note [2445033'e] bakın.  
    Bu sorun SAP LaMa'nın yeni bir destek paketinde/yamasında giderilir.  
    Bu sorunu çözmek için SAP sisteminizin varsayılan profilinde profil parametresi OS_UNICODE=uc'u ayarlayın.

* Hata yürütme SAPinst adım: dCheckGivenServer
  * Hata yürütme SAPinst adım: dCheckGivenServer" version="1.0" HATA: (Son \<hata adım tarafından bildirilen: p> Yükleme kullanıcı tarafından iptal edildi. \</p>
  * Çözüm  
    SWPM'nin profile erişimi olan bir kullanıcıyla birlikte çalıştığını unutmayın. Bu kullanıcı Application Server Installation sihirbazında yapılandırılabilir

* Hata yürütme SAPinst adımı: checkClient
  * Hata yürütme SAPinst adım: checkClient" version="1.0" HATA: (Son \<hata adım tarafından bildirilen: p> Yükleme kullanıcı tarafından iptal edildi. \</p>)
  * Çözüm  
    SQL Server için Microsoft ODBC sürücüsünün uygulama sunucusunu yüklemek istediğiniz sanal makineye yüklü olduğundan emin olun

* Hata yürütme SAPinst adım: copyScripts
  * Adımtarafından bildirilen son hata: Sistem çağrısı başarısız oldu. AYRINTILAR: Hata 13 (0x000000d) (İzin reddedildi) parametresi ile sistem\\çağrısı 'fopenU' yürütülmesinde ( \as1-ascs/sapmnt/AS1/SYS/exe/uc/NTAMD64/strdbs.cmd, w), satır (494) dosyada (\bas/bas/749_REL/bc_749_REL/src/ins/SAPINST/impl/src/syslib/filesystem/syxxcfstrm2.cpp), yığın izleme:  
  CThrThread.cpp: 85: CThrThread::threadFunction()  
  CSiServiceSet.cpp: 63: CSiServiceSet::executeService()  
  CSiStepExecute.cpp: 913: CSiStepExecute::execute()  
  EJSController.cpp: 179: EJSControllerImpl::executeScript()  
  JSExtension.hpp: 1136: CallFunctionBase::call()  
  iaxxcfile.cpp: 183: iastring CIaOsFileConnect::callMemberFunction(iastring const& adı, args_t const& args)  
  iaxxcfile.cpp: 1849: iastring CIaOsFileConnect::newFileStream(args_t const& _args)  
  iaxxbfile.cpp: 773: CIaOsFile::newFileStream_impl(4)  
  syxxcfile.cpp: 233: CSyFileImpl::openStream(ISyFile::eFileOpenMode)  
  syxxcfstrm.cpp: 29: CSyFileStreamImpl::CSyFileStreamImpl(CSyFileStream*,iastring,ISyFile::eFileOpenMode)  
  syxxcfstrm.cpp: 265: CSyFileStreamImpl::open()  
  syxxcfstrm2.cpp: 58: CSyFileStream2Impl::CSyFileStream2Impl(const CSyPath & \\\aw1-ascs/sapmnt/AW1/SYS/exe/uc/NTAMD64/strdbs.cmd, 0x4)  
  syxxcfstrm2.cpp: 456: CSyFileStream2Impl::open()
  * Çözüm  
    SWPM'nin profile erişimi olan bir kullanıcıyla birlikte çalıştığını unutmayın. Bu kullanıcı Application Server Installation sihirbazında yapılandırılabilir

* Hata yürütme SAPinst adım: askPasswords
  * Adımtarafından bildirilen son hata: Sistem çağrısı başarısız oldu. AYRINTILAR: Hata 5 (0x00000005) (Erişim reddedildi.) parametre (...), satır (359) dosya (\bas/bas/749_REL/bc_749_REL/src/ins/SAPINST/impl/src/syslib/account/synxcaccmg.cpp) ile 'NetValidatePasswordPolicy' sistem çağrısının yürütülmesinde:  
  CThrThread.cpp: 85: CThrThread::threadFunction()  
  CSiServiceSet.cpp: 63: CSiServiceSet::executeService()  
  CSiStepExecute.cpp: 913: CSiStepExecute::execute()  
  EJSController.cpp: 179: EJSControllerImpl::executeScript()  
  JSExtension.hpp: 1136: CallFunctionBase::call()  
  CSiStepExecute.cpp: 764: CSiStepExecute::invokeDialog()  
  DarkModeGuiEngine.cpp: 56: DarkModeGuiEngine::showDialogCalledByJs()  
  DarkModeDialog.cpp: 85: DarkModeDialog::submit()  
  EJSController.cpp: 179: EJSControllerImpl::executeScript()  
  JSExtension.hpp: 1136: CallFunctionBase::call()  
  iaxxcaccount.cpp: 107: iastring CIaOsAccountConnect::callMemberFunction(iastring const& adı, args_t const& args)  
  iaxxcaccount.cpp: 1186: iastring CIaOsAccountConnect::doğrulamaPasswordPolicy(args_t const& _args)  
  iaxxbaccount.cpp: 430: CIaOsAccount::validatePasswordPolicy_impl()  
  synxcaccmg.cpp: 297: ISyAccountMgt::PasswordValidationMessage CSyAccountMgtImpl::validatePasswordPolicy(saponazure,*****) const )
  * Çözüm  
    VM'den etki alanı denetleyicisine iletişim sağlamak için adım *Yalıtımına* ana bilgisayar kuralı eklediğinizden emin olun

## <a name="next-steps"></a>Sonraki adımlar
* [Azure işlemlerinde SAP HANA kılavuzu][hana-ops-guide]
* [AZURE Sanal Makineler SAP için planlama ve uygulama][planning-guide]
* [SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
