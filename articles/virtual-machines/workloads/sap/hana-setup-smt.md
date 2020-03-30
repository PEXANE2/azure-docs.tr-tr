---
title: Azure'da SAP HANA için SMT sunucusu nasıl ayarlanır (Büyük Örnekler) | Microsoft Dokümanlar
description: Azure'da SAP HANA için SMT sunucusu nasıl ayarlaylayın (Büyük Örnekler).
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: hermannd
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 699a8a69621642d07d3547c07bb20c0d32ca7686
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616984"
---
# <a name="set-up-smt-server-for-suse-linux"></a>SUSE Linux için SMT sunucusu ayarlama
BÜYÜK SAP HANA Örnekleri internete doğrudan bağlantı yok. Böyle bir birimi işletim sistemi sağlayıcısına kaydetmek ve güncelleştirmeleri karşıdan yükleyip uygulamak kolay bir işlem değildir. SUSE Linux için bir çözüm, bir Azure sanal makinede bir SMT sunucusu kurmaktır. Sanal makineyi HANA Büyük Örneği'ne bağlı bir Azure sanal ağında barındırın. Böyle bir SMT sunucusu yla, HANA Büyük Örnek birimi güncelleştirmeleri kaydedebilir ve karşıdan yükleyebilir. 

SUSE hakkında daha fazla belge için, [SLES 12 SP2 için Abonelik Yönetim Aracı'na](https://www.suse.com/documentation/sles-12/pdfdoc/book_smt/book_smt.pdf)bakın. 

HANA Büyük Örnekleri için görevi yerine getiren bir SMT sunucusu yüklemek için ön koşullar şunlardır:

- HANA Büyük Örnek ExpressRoute devresine bağlı bir Azure sanal ağı.
- Bir kuruluşla ilişkili bir SUSE hesabı. Kuruluşun geçerli bir SUSE aboneliği olmalıdır.

## <a name="install-smt-server-on-an-azure-virtual-machine"></a>Azure sanal makinesine SMT sunucusu yükleme

İlk olarak, [SUSE Müşteri Merkezi'nde](https://scc.suse.com/)oturum açın.

**Kuruluş** > **Kuruluş Kimlik Bilgileri'ne**gidin. Bu bölümde, SMT sunucusunu ayarlamak için gerekli kimlik bilgilerini bulmanız gerekir.

Ardından, Azure sanal ağına bir SUSE Linux VM yükleyin. Sanal makineyi dağıtmak için Azure'un SLES 12 SP2 galeri görüntüsünü alın (BYOS SUSE görüntüsünü seçin). Dağıtım işleminde, bir DNS adı tanımlamayın ve statik IP adresleri kullanmayın.

![SMT sunucusu için sanal makine dağıtımının ekran görüntüsü](./media/hana-installation/image3_vm_deployment.png)

Dağıtılan sanal makine daha küçüktür ve 10.34.1.4'lük Azure sanal ağındaki dahili IP adresini aldı. Sanal makinenin adı *smtserver*olduğunu. Kurulumdan sonra HANA Büyük Örnek birimine veya birimlerine bağlantı denetlenir. Ad çözümlemeyi nasıl düzenlediğinize bağlı olarak, Azure sanal makinesinin vb/ana bilgisayarlarındaki HANA Büyük Örnek birimlerinin çözünürlüğünü yapılandırmanız gerekebilir. 

Sanal makineye bir disk ekleyin. Güncelleştirmeleri tutmak için bu diski kullanırsınız ve önyükleme diskinin kendisi çok küçük olabilir. Burada, disk aşağıdaki ekran görüntüsünde gösterildiği gibi /srv/www/htdocs'a monte edilmiştir. 100 GB'lık bir disk yeterli olacaktır.

![SMT sunucusu için sanal makine dağıtımının ekran görüntüsü](./media/hana-installation/image4_additional_disk_on_smtserver.PNG)

HANA Büyük Örnek birimi veya birimlerinde oturum açın, /etc/hosts'ı koruyun ve SMT sunucusunu ağ üzerinden çalıştırması gereken Azure sanal makinesine erişip erişemeyeceğiniz kontrol edin.

Bu denetimden sonra, SMT sunucusunu çalıştırması gereken Azure sanal makinesinde oturum açın. Sanal makinede oturum açtıracak macun kullanıyorsanız, bash pencerenizde bu komut dizisini çalıştırın:

```
cd ~
echo "export NCURSES_NO_UTF8_ACS=1" >> .bashrc
```

Ayarları etkinleştirmek için bash yeniden başlatın. O zaman YAST'ı başlat.

VM'nizi (smtserver) SUSE sitesine bağlayın.

```
smtserver:~ # SUSEConnect -r <registration code> -e s<email address> --url https://scc.suse.com
Registered SLES_SAP 12.2 x86_64
To server: https://scc.suse.com
Using E-Mail: email address
Successfully registered system.
```

Sanal makine SUSE sitesine bağlandıktan sonra smt paketlerini yükleyin. smt paketlerini yüklemek için aşağıdaki macun komutunu kullanın.

```
smtserver:~ # zypper in smt
Refreshing service 'SUSE_Linux_Enterprise_Server_for_SAP_Applications_12_SP2_x86_64'.
Loading repository data...
Reading installed packages...
Resolving package dependencies...
```


SMT paketlerini yüklemek için YAST aracını da kullanabilirsiniz. YAST'da, **Yazılım Bakımı'na**gidin ve smt'yi arayın. Yast2-smt'ye otomatik olarak geçiş yapan **smt'yi**seçin.

![YAST'de SMT ekran görüntüsü](./media/hana-installation/image5_smt_in_yast.PNG)


Smtserver'da yükleme için seçimi kabul edin. Yükleme tamamlandıktan sonra SMT sunucu yapılandırmasına gidin. Daha önce aldığınız SUSE Müşteri Merkezi'nden kuruluş kimlik bilgilerini girin. Ayrıca Azure sanal makine ana adınızı SMT Server URL'si olarak girin. Bu gösteride, https:\//smtserver' dır.

![SMT sunucu yapılandırmasının ekran görüntüsü](./media/hana-installation/image6_configuration_of_smtserver1.png)

Şimdi SUSE Müşteri Merkezi bağlantısının çalışıp çalışmadığını test edin. Aşağıdaki ekran görüntüsünde gördüğünüz gibi, bu gösteri durumda, işe yetti.

![SUSE Müşteri Merkezi'ne bağlantı test ekran görüntüsü](./media/hana-installation/image7_test_connect.png)

SMT kurulumu başladıktan sonra bir veritabanı parolası sağlayın. Yeni bir yükleme olduğundan, bu parolayı aşağıdaki ekran görüntüsünde gösterildiği gibi tanımlamanız gerekir.

![Veritabanı için parola tanımlama ekran görüntüsü](./media/hana-installation/image8_define_db_passwd.PNG)

Bir sonraki adım bir sertifika oluşturmaktır.

![SMT sunucusu için sertifika oluşturma ekran görüntüsü](./media/hana-installation/image9_certificate_creation.PNG)

Yapılandırmanın sonunda, eşitleme denetimini çalıştırmak birkaç dakika sürebilir. SMT sunucusunun kurulumu ve yapılandırması sonrasında, /srv/www/htdocs/ montaj noktasının altında dizin reposu bulmalısınız. Repo kapsamında bazı alt dizinler de vardır. 

SMT sunucusunu ve ilgili hizmetlerini bu komutlarla yeniden başlatın.

```
rcsmt restart
systemctl restart smt.service
systemctl restart apache2
```

## <a name="download-packages-onto-smt-server"></a>Paketleri SMT sunucusuna indirin

Tüm hizmetler yeniden başlatıldıktan sonra, YAST kullanarak SMT Management'da uygun paketleri seçin. Paket seçimi HANA Büyük Örnek sunucusunun işletim sistemi görüntüsüne bağlıdır. Paket seçimi, SMT sunucusunu çalıştıran sanal makinenin SLES sürümüne veya sürümüne bağlı değildir. Aşağıdaki ekran görüntüsü seçim ekranının bir örneğini gösterir.

![Paketleri seçme ekran görüntüsü](./media/hana-installation/image10_select_packages.PNG)

Ardından, seçtiğiniz paketlerin ilk kopyasını ayarladığınız SMT sunucusuna başlatın. Bu kopya smt-mirror komutu kullanılarak kabukta tetiklenir.

![Paketleri SMT sunucusuna indirme ekran görüntüsü](./media/hana-installation/image11_download_packages.PNG)

Paketler , /srv/www/htdocs montaj noktası altında oluşturulan dizinlere kopyalanmalıdır. Bu işlem, seçtiğiniz pakete bağlı olarak bir saat veya daha uzun sürebilir. Bu işlem tamamlanınırken, SMT istemci kurulumuna geçin. 

## <a name="set-up-the-smt-client-on-hana-large-instance-units"></a>HANA Büyük Örnek birimlerinde SMT istemcisini ayarlama

Bu durumda istemci veya istemciler HANA Büyük Örnek birimleridir. SMT sunucu kurulumu komut dosyası clientSetup4SMT.sh Azure sanal makinesine kopyaladı. Bu komut dosyasını SMT sunucunuza bağlanmak istediğiniz HANA Büyük Örnek birimine kopyalayın. Komut dosyasını -h seçeneğiyle başlatın ve SMT sunucunuzun adını parametre olarak verin. Bu örnekte, adı *smtserver*olduğunu.

![SMT istemcisini yapılandırma ekran görüntüsü](./media/hana-installation/image12_configure_client.PNG)

Sertifikanın istemci tarafından sunucudan yüklenmesi başarılı olabilir, ancak aşağıdaki ekran görüntüsünde gösterildiği gibi kayıt başarısız olur.

![İstemci kayıt hatasının ekran görüntüsü](./media/hana-installation/image13_registration_failed.PNG)

Kayıt başarısız olursa, [SUSE destek belgesine](https://www.suse.com/de-de/support/kb/doc/?id=7006024)bakın ve burada açıklanan adımları çalıştırın.

> [!IMPORTANT] 
> Sunucu adı için, tam nitelikli etki alanı adı olmadan sanal makinenin adını (bu durumda, *smtserver)* sağlayın. 

Bu adımları çalıştırdıktan sonra, HANA Büyük Örnek biriminde aşağıdaki komutu çalıştırın:

```
SUSEConnect –cleanup
```

> [!Note] 
> O adımdan sonra birkaç dakika bekle. Hemen clientSetup4SMT.sh çalıştırın, bir hata alabilirsiniz.

SUSE makalesinin adımlarına göre düzeltmeniz gereken bir sorunla karşılaşırsanız, HANA Büyük Örnek biriminde clientSetup4SMT.sh yeniden başlatın. Şimdi başarılı bir şekilde bitmeli.

![İstemci kayıt başarısının ekran görüntüsü](./media/hana-installation/image14_finish_client_config.PNG)

HANA Büyük Örnek biriminin SMT istemcisini Azure sanal makinesine yüklediğiniz SMT sunucusuna bağlanacak şekilde yapılandırıldınız. Artık HANA Büyük Örnekleri işletim sistemi güncelleştirmeleri yüklemek için 'zypper up' veya 'zypper' alabilir veya ek paketler yükleyebilirsiniz. Yalnızca daha önce indirdiğiniz güncelleştirmeleri SMT sunucusundan alabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [HLI'da HANA Kurulumu](hana-example-installation.md).











