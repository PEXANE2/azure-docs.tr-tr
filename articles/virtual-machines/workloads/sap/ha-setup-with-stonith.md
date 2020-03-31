---
title: Azure'da SAP HANA için STONITH ile yüksek kullanılabilirlik ayarlandı (Büyük Örnekler)| Microsoft Dokümanlar
description: STONITH'i kullanarak SUSE'da Azure'da (Büyük Örnekler) SAP HANA için yüksek kullanılabilirlik oluşturma
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/21/2017
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4060dbe936af8ff1f9dd8c958f64834cb06525de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77615079"
---
# <a name="high-availability-set-up-in-suse-using-the-stonith"></a>STONITH kullanarak SUSE’de yüksek kullanılabilirlik ayarlama
Bu belge, STONITH cihazını kullanarak SUSE İşletim sisteminde Yüksek Kullanılabilirlik'i kurmak için adım adım ayrıntılı yönergeleri sağlar.

**Yasal Uyarı:** *Bu kılavuz, kurulumu başarıyla çalışan Microsoft HANA Büyük Örnekler ortamında sınanarak türetilmiştir. HANA Large Instances için Microsoft Hizmet Yönetimi ekibi İşletim sistemini desteklemediği için, işletim sistemi katmanında daha fazla sorun giderme veya açıklama için SUSE ile iletişime geçmeniz gerekebilir. Microsoft hizmet yönetimi ekibi STONITH aygıtını ayarlar ve STONITH aygıt sorunları için sorun giderme için tam olarak destekler ve dahil edilebilir.*
## <a name="overview"></a>Genel Bakış
SUSE kümeleme kullanarak Yüksek kullanılabilirliği ayarlamak için aşağıdaki ön koşulların karşılanması gerekir.
### <a name="pre-requisites"></a>Ön koşullar
- HANA Büyük Örnekleri sağlanmaktadır
- İşletim sistemi kayıtlı
- HANA Büyük Örnekler sunucuları yamalar /paketler almak için SMT sunucusuna bağlanır
- İşletim sistemi en son yamalar yüklü var
- NTP (zaman sunucusu) ayarlanır
- HA kurulumunda SUSE belgelerinin en son sürümünü okuyun ve anlayın

### <a name="setup-details"></a>Kurulum ayrıntıları
Bu kılavuzda aşağıdaki kurulum kullanır:
- İşletim Sistemi: SAP için SLES 12 SP1
- HANA Büyük Örnekleri: 2xS192 (dört soket, 2 TB)
- HANA Versiyon: HANA 2.0 SP1
- Sunucu Adları: sapprdhdb95 (node1) ve sapprdhdb96 (node2)
- STONITH Cihazı: iSCSI tabanlı STONITH cihazı
- Hana Büyük Örnek düğümlerinden birine kurulan NTP

HSR ile HANA Büyük Örnekleri ayarladığınızda, MICROSOFT Service Management ekibinden STONITH'i ayarlamasını isteyebilirsiniz. HANA Large Instances'ı zaten hazırlamış olan mevcut bir müşteriyseniz ve varolan bıçaklarınız için STONITH aygıtının ayarlatMası gerekiyorsa, aşağıdaki bilgileri hizmet istek formunda (SRF) Microsoft Service Management ekibine sağlamanız gerekir. Hana Büyük Örnek onboarding için Teknik Hesap Yöneticisi veya Microsoft İlgili Kişi aracılığıyla SRF formu isteyebilirsiniz. Yeni müşteriler, sağlama sırasında STONITH cihazını talep edebilirler. Girişler, sağlama isteği formunda mevcuttur.

- Sunucu Adı ve Sunucu IP adresi (örneğin, myhanaserver1, 10.35.0.1)
- Konum (örneğin, ABD Doğu)
- Müşteri Adı (örneğin, Microsoft)
- SID - HANA Sistem Tanımlayıcısı (örneğin, H11)

STONITH aygıtı yapılandırıldıktan sonra, Microsoft Service Management ekibi size STONITH kurulumını yapılandırmak için kullanabileceğiniz iSCSI depolama alanının SBD aygıt adını ve IP adresini sağlar. 

STONITH kullanarak HA sonuna kadar ayarlamak için aşağıdaki adımları izlenmesi gerekir:

1.  SBD aygıtını tanımlama
2.  SBD aygıtını başlatma
3.  Kümeyi Yapılandırma
4.  Softdog Watchdog kurma
5.  Kümeye düğümkatılma
6.  Kümeyi doğrulama
7.  Kaynakları kümeye yapılandırma
8.  Başarısız süreci test edin

## <a name="1---identify-the-sbd-device"></a>1. SBD cihazını tanımlayın
Bu bölümde, Microsoft hizmet yönetimi ekibi STONITH'i yapılandırdıktan sonra kurulumunuz için SBD aygıtının nasıl belirlendiğini açıklamaktadır. **Bu bölüm yalnızca varolan müşteri için geçerlidir.** Yeni bir müşteriyseniz, Microsoft hizmet yönetim ekibi size SBD aygıt adı sağlar ve bu bölümü atlayabilirsiniz.

1.1 Değiştir */etc/iscsi/initiatorname.isci* için 
``` 
iqn.1996-04.de.suse:01:<Tenant><Location><SID><NodeNumber> 
```

Microsoft hizmet yönetimi bu dizeyi sağlar. Dosyayı **her iki** düğümde de değiştirin, ancak düğüm numarası her düğümde farklıdır.

![initiatorname.png](media/HowToHLI/HASetupWithStonith/initiatorname.png)

1.2 Değiştir */etc/iscsi/iscsid.conf*: Set *node.session.timeo.replacement_timeout=5* ve *node.startup = automatic*. Dosyayı **her iki** düğümde de değiştirin.

1.3 Bulma komutunu çalıştırın, dört oturum gösterir. Her iki düğümüzerinde çalıştırın.

```
iscsiadm -m discovery -t st -p <IP address provided by Service Management>:3260
```

![iSCSIadmDiscovery.png](media/HowToHLI/HASetupWithStonith/iSCSIadmDiscovery.png)

1.4 iSCSI aygıtında oturum açma komutunu uygulayın, dört oturum gösterir. **Her iki** düğümüzerinde çalıştırın.

```
iscsiadm -m node -l
```
![iSCSIadmLogin.png](media/HowToHLI/HASetupWithStonith/iSCSIadmLogin.png)

1.5 Rescan komut dosyasını çalıştırın: *rescan-scsi-bus.sh.*  Bu komut dosyası, sizin için oluşturulan yeni diskleri gösterir.  Her iki düğümüzerinde çalıştırın. Sıfırdan büyük bir LUN numarası görmeniz gerekir (örneğin: 1, 2 vb.)

```
rescan-scsi-bus.sh
```
![rescanscsibus.png](media/HowToHLI/HASetupWithStonith/rescanscsibus.png)

1.6 Cihaz adının fdisk komutunu çalıştırmasını sağlamak için *–l*. Her iki düğümüzerinde çalıştırın. **178 MiB**boyutunda cihazı seçin.

```
  fdisk –l
```

![fdisk-l.png](media/HowToHLI/HASetupWithStonith/fdisk-l.png)

## <a name="2---initialize-the-sbd-device"></a>2. SBD cihazını başlatma

2.1 SBD cihazını **her iki** düğümde de başlatma

```
sbd -d <SBD Device Name> create
```
![sbdcreate.png](media/HowToHLI/HASetupWithStonith/sbdcreate.png)

2.2 Cihaza yazılanları kontrol edin. **Her iki** düğümüzerinde yapın

```
sbd -d <SBD Device Name> dump
```

## <a name="3---configuring-the-cluster"></a>3. Kümeyi Yapılandırma
Bu bölümde, SUSE HA kümesini ayarlamak için adımlar açıklanmaktadır.
### <a name="31-package-installation"></a>3.1 Paket montajı
3.1.1 Lütfen ha_sles ve SAPHanaSR-doc desenlerinin yüklü olup olmadığını kontrol edin. Yüklenmezse, yükleyin. **Her iki** düğüme de yükleyin.
```
zypper in -t pattern ha_sles
zypper in SAPHanaSR SAPHanaSR-doc
```
![zypperpatternha_sles.png](media/HowToHLI/HASetupWithStonith/zypperpatternha_sles.png)
![zypperpatternSAPHANASR-doc.png](media/HowToHLI/HASetupWithStonith/zypperpatternSAPHANASR-doc.png)

### <a name="32-setting-up-the-cluster"></a>3.2 Kümenin ayarlanması
3.2.1 *Ha-cluster-init* komutunu kullanabilir veya kümeyi ayarlamak için yast2 sihirbazını kullanabilirsiniz. Bu durumda, yast2 sihirbazı kullanılır. Bu adımı **yalnızca Birincil düğümde**gerçekleştirirsiniz.

yast2> Yüksek Kullanılabilirlik > Cluster ![yast-control-center.png](media/HowToHLI/HASetupWithStonith/yast-control-center.png)
![yast-hawk-install.png izleyin](media/HowToHLI/HASetupWithStonith/yast-hawk-install.png)

Halk2 paketi zaten yüklendiğinden **iptal'i** tıklatın.

![yast-hawk-continue.png](media/HowToHLI/HASetupWithStonith/yast-hawk-continue.png)

**Devam Et'i** tıklatın

Beklenen değer=Dağıtılan ![düğüm sayısı (bu durumda 2) yast-Cluster-Security.png](media/HowToHLI/HASetupWithStonith/yast-Cluster-Security.png) Click **Next**
![yast-cluster-configure-csync2.png](media/HowToHLI/HASetupWithStonith/yast-cluster-configure-csync2.png) Düğüm adlarını ekle ve ardından "Önerilen dosyaları ekle"yi tıklatın

"Csync2'yi AÇ" seçeneğini tıklayın

"Önceden Paylaşılan Anahtarları Oluştur"a tıklayın, pop-up'ın altında

![yast-key-file.png](media/HowToHLI/HASetupWithStonith/yast-key-file.png)

**Tamam'ı** tıklatın

Kimlik doğrulama, Csync2'deki IP adresleri ve önceden paylaşılan tuşlar kullanılarak gerçekleştirilir. Anahtar dosyası csync2 -k /etc/csync2/key_hagroup ile oluşturulur. Dosya key_hagroup oluşturulduktan sonra kümenin tüm üyelerine el ile kopyalanmalıdır. **Dosyayı düğüm 1'den düğüm2'ye kopyaladığından emin olun.**

![yast-küme-conntrackd.png](media/HowToHLI/HASetupWithStonith/yast-cluster-conntrackd.png)

**Next**
Sonraki![yast-cluster-service.png'yi tıklatın](media/HowToHLI/HASetupWithStonith/yast-cluster-service.png)

Varsayılan seçenekte, Önyükleme kapalıydı, "a"' olarak değiştirin, böylece kalp pili önyüklemeye başlanır. Seçimi kurulum gereksinimlerinize göre yapabilirsiniz.
**İleri'yi** tıklatın ve küme yapılandırması tamamlandı.

## <a name="4---setting-up-the-softdog-watchdog"></a>4. Softdog Watchdog kurma
Bu bölümde watchdog (softdog) yapılandırmasını açıklar.

4.1 Her **iki** düğümde *de /etc/init.d/boot.local'a* aşağıdaki satırı ekleyin.
```
modprobe softdog
```
![modprobe-softdog.png](media/HowToHLI/HASetupWithStonith/modprobe-softdog.png)

4.2 Dosyayı */etc/sysconfig/sbd'yi* **her iki** düğümde de aşağıdaki gibi güncelleyin:
```
SBD_DEVICE="<SBD Device Name>"
```
![sbd-device.png](media/HowToHLI/HASetupWithStonith/sbd-device.png)

4.3 Çekirdek modüllerini aşağıdaki komutu çalıştırarak **her iki** düğüme de yükleyin
```
modprobe softdog
```
![modprobe-softdog-command.png](media/HowToHLI/HASetupWithStonith/modprobe-softdog-command.png)

4.4 Softdog'un **her iki** düğümde de aşağıdaki gibi çalıştığını kontrol edin ve kontrol edin:
```
lsmod | grep dog
```
![lsmod-grep-dog.png](media/HowToHLI/HASetupWithStonith/lsmod-grep-dog.png)

4.5 SBD cihazını **her iki** düğümde de başlatın
```
/usr/share/sbd/sbd.sh start
```
![sbd-sh-start.png](media/HowToHLI/HASetupWithStonith/sbd-sh-start.png)

4.6 SBD daemon'u **her iki** düğümüzerinde de test edin. **Her iki** düğümde de yapılandırıldıktan sonra iki giriş görürsünüz
```
sbd -d <SBD Device Name> list
```
![sbd-list.png](media/HowToHLI/HASetupWithStonith/sbd-list.png)

4.7 Düğümlerinizden **birine** test iletisi gönderme
```
sbd  -d <SBD Device Name> message <node2> <message>
```
![sbd-list.png](media/HowToHLI/HASetupWithStonith/sbd-list.png)

4.8 **İkinci** düğümde (düğüm2) ileti durumunu kontrol edebilirsiniz
```
sbd  -d <SBD Device Name> list
```
![sbd-list-message.png](media/HowToHLI/HASetupWithStonith/sbd-list-message.png)

4.9 Sbd config'i benimsemek için *dosyayı /etc/sysconfig/sbd'yi* aşağıdaki şekilde güncelleyin. Her **iki** düğümdeki dosyayı güncelleştirme
```
SBD_DEVICE=" <SBD Device Name>" 
SBD_WATCHDOG="yes" 
SBD_PACEMAKER="yes" 
SBD_STARTMODE="clean" 
SBD_OPTS=""
```
4.10 **Birincil düğümdeki** kalp pili hizmetini başlatın (düğüm1)
```
systemctl start pacemaker
```
![başlangıç-pacemaker.png](media/HowToHLI/HASetupWithStonith/start-pacemaker.png)

Kalp pili hizmeti *başarısız olursa,* *Senaryo 5'e bakın: Kalp Pili hizmeti başarısız*

## <a name="5---joining-the-cluster"></a>5. Kümeye katılma
Bu bölümde kümeye düğüm nasıl birleştirilen açıklanır.

### <a name="51-add-the-node"></a>5.1 Düğümü ekleme
Düğüm2 kümeye katılmasına izin vermek için **düğüm2'de** aşağıdaki komutu çalıştırın.
```
ha-cluster-join
```
Kümeye katılma sırasında bir *hata* alırsanız, *Bkz. Senaryo 6: Düğüm 2 kümeye katılamaz.*

## <a name="6---validating-the-cluster"></a>6. Kümenin doğrulanması

### <a name="61-start-the-cluster-service"></a>6.1 Küme hizmetini başlatma
Kümeyi **her iki** düğümde de ilk kez denetlemek ve isteğe bağlı olarak başlatmak için.
```
systemctl status pacemaker
systemctl start pacemaker
```
![systemctl-durum-pacemaker.png](media/HowToHLI/HASetupWithStonith/systemctl-status-pacemaker.png)
### <a name="62-monitor-the-status"></a>6.2 Durumu izleme
**Her iki** düğümün de çevrimiçi olduğundan emin olmak için *crm_mon* komutu çalıştırın. Kümenin **düğümlerinden herhangi birinde** çalıştırabilirsiniz
```
crm_mon
```
![crm-mon.png](media/HowToHLI/HASetupWithStonith/crm-mon.png) Ayrıca küme durumunu kontrol etmek için hawk giriş yapabilirsiniz *https://\<düğüm IP>:7630*. Varsayılan kullanıcı hacluster ve şifre linux olduğunu. Gerekirse, *passwd* komutunu kullanarak parolayı değiştirebilirsiniz.

## <a name="7-configure-cluster-properties-and-resources"></a>7. Küme Özelliklerini ve Kaynaklarını Yapılandırma 
Bu bölümde küme kaynaklarını yapılandırmak için adımlar açıklanmaktadır.
Bu örnekte, aşağıdaki kaynağı ayarlayın, geri kalanı (gerekirse) SUSE HA kılavuzuna başvurarak yapılandırılabilir. Config'i yalnızca **düğümlerden birinde gerçekleştirin.** Birincil düğümde yapın.

- Küme bootstrap
- STONITH Cihazı
- Sanal IP Adresi


### <a name="71-cluster-bootstrap-and-more"></a>7.1 Küme bootstrap ve daha fazlası
Küme bootstrap ekleyin. Dosyayı oluşturun ve metni aşağıdaki gibi ekleyin:
```
sapprdhdb95:~ # vi crm-bs.txt
# enter the following to crm-bs.txt
property $id="cib-bootstrap-options" \
no-quorum-policy="ignore" \
stonith-enabled="true" \
stonith-action="reboot" \
stonith-timeout="150s"
rsc_defaults $id="rsc-options" \
resource-stickiness="1000" \
migration-threshold="5000"
op_defaults $id="op-options" \
timeout="600"
```
Yapılandırmayı kümeye ekleyin.
```
crm configure load update crm-bs.txt
```
![crm-configure-crmbs.png](media/HowToHLI/HASetupWithStonith/crm-configure-crmbs.png)

### <a name="72-stonith-device"></a>7.2 STONITH cihazı
Kaynak STONITH ekleyin. Dosyayı oluşturun ve metni aşağıdaki gibi ekleyin.
```
# vi crm-sbd.txt
# enter the following to crm-sbd.txt
primitive stonith-sbd stonith:external/sbd \
params pcmk_delay_max="15"
```
Yapılandırmayı kümeye ekleyin.
```
crm configure load update crm-sbd.txt
```

### <a name="73-the-virtual-ip-address"></a>7.3 Sanal IP adresi
Kaynak sanal IP ekleyin. Dosyayı oluşturun ve metni aşağıdaki gibi ekleyin.
```
# vi crm-vip.txt
primitive rsc_ip_HA1_HDB10 ocf:heartbeat:IPaddr2 \
operations $id="rsc_ip_HA1_HDB10-operations" \
op monitor interval="10s" timeout="20s" \
params ip="10.35.0.197"
```
Yapılandırmayı kümeye ekleyin.
```
crm configure load update crm-vip.txt
```

### <a name="74-validate-the-resources"></a>7.4 Kaynakları doğrulama

*komut crm_mon*çalıştırdığınızda, orada iki kaynak görebilirsiniz.
![crm_mon_command.png](media/HowToHLI/HASetupWithStonith/crm_mon_command.png)

Ayrıca, *https://\<düğüm IP adresi>:7630/cib/live/state* adresinde ki durumu görebilirsiniz

![hawlk-durum-page.png](media/HowToHLI/HASetupWithStonith/hawlk-status-page.png)

## <a name="8-testing-the-failover-process"></a>8. Başarısızlık işlemini test etme
Başarısız lık işlemini test etmek için, düğümdeki kalp pili hizmetini durdurun1 ve kaynaklar düğüm2'ye geçilemeyip başarısız oldu.
```
Service pacemaker stop
```
Şimdi, **düğüm2** üzerinde kalp pili hizmetini durdurun ve kaynaklar düğüm üzerinde başarısız **oldu1**

**Başarısız olmadan önce**  
![Before-failover.png](media/HowToHLI/HASetupWithStonith/Before-failover.png)  

**Başarısız olduktan sonra**  
![sonra-failover.png](media/HowToHLI/HASetupWithStonith/after-failover.png)  
![crm-mon-sonrası-failover.png](media/HowToHLI/HASetupWithStonith/crm-mon-after-failover.png)  


## <a name="9-troubleshooting"></a>9. Sorun giderme
Bu bölümde, kurulum sırasında karşılaşılabilen birkaç hata senaryosu açıklanmaktadır. Mutlaka bu sorunlarla karşı karşıya olmayabilir.

### <a name="scenario-1-cluster-node-not-online"></a>Senaryo 1: Küme düğümü çevrimiçi değil
Düğümlerden herhangi biri küme yöneticisinde çevrimiçi görünmüyorsa, çevrimiçi olarak getirmek için aşağıdakileri deneyebilirsiniz.

iSCSI hizmetini başlatın
```
service iscsid start
```

Ve şimdi bu iSCSI düğüm oturum açabilmeli
```
iscsiadm -m node -l
```
Beklenen çıktı aşağıdaki gibi görünüyor
```
sapprdhdb45:~ # iscsiadm -m node -l
Logging in to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.11,3260] (multiple)
Logging in to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.12,3260] (multiple)
Logging in to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.22,3260] (multiple)
Logging in to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.21,3260] (multiple)
Login to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.11,3260] successful.
Login to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.12,3260] successful.
Login to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.22,3260] successful.
Login to [iface: default, target: iqn.1992-08.com.netapp:hanadc11:1:t020, portal: 10.250.22.21,3260] successful.
```
### <a name="scenario-2-yast2-does-not-show-graphical-view"></a>Senaryo 2: yast2 grafik görünümü göstermez
Yast2 grafik ekranı, bu belgedeki Yüksek Kullanılabilirlik kümesini ayarlamak için kullanılır. Yast2 gösterildiği gibi grafik penceresi ile açılmıyorsa ve Qt hatası atmak, aşağıdaki adımları gerçekleştirin. Grafik penceresi ile açılırsa, adımları atlayabilirsiniz.

**Hata**

![yast2-qt-gui-error.png](media/HowToHLI/HASetupWithStonith/yast2-qt-gui-error.png)

**Beklenen Çıktı**

![yast-kontrol-center.png](media/HowToHLI/HASetupWithStonith/yast-control-center.png)

Yast2 grafik görünümüyle açılmıyorsa, aşağıdaki adımları izleyin.

Gereken paketleri yükleyin. Kullanıcı "root" olarak oturum açmanız ve paketleri indirmek/yüklemek için SMT'yi ayarlamanız gerekir.

Paketleri yüklemek için yazılım>Yazılım Yönetimi>Bağımlılıklar> seçeneği>"Önerilen paketleri yükleyin..." seçeneğini kullanın. Aşağıdaki ekran görüntüsü beklenen ekranları gösterir.
>[!NOTE]
>Her iki düğümden de yast2 grafik görünümüne erişebilmeniz için her iki düğümdeki adımları gerçekleştirmeniz gerekir.

![yast-sofwaremanagement.png](media/HowToHLI/HASetupWithStonith/yast-sofwaremanagement.png)

Bağımlılıklar altında, "Önerilen Paketleri ![Yükle" yast-dependencies.png'yi seçin](media/HowToHLI/HASetupWithStonith/yast-dependencies.png)

Değişiklikleri gözden geçirin ve Tamam'a vurun

![yast](media/HowToHLI/HASetupWithStonith/yast-automatic-changes.png)

Paket yükleme ![gelirleri yast-performing-installation.png](media/HowToHLI/HASetupWithStonith/yast-performing-installation.png)

İleri’ye tıklayın

![yast-enstalasyon-report.png](media/HowToHLI/HASetupWithStonith/yast-installation-report.png)

Bitiş'e tıklayın

Ayrıca libqt4 ve libyui-qt paketleri yüklemeniz gerekir.
```
zypper -n install libqt4
```
![zypper-install-libqt4.png](media/HowToHLI/HASetupWithStonith/zypper-install-libqt4.png)
```
zypper -n install libyui-qt
```
![zypper-install-ligyui.png](media/HowToHLI/HASetupWithStonith/zypper-install-ligyui.png)
![zypper-install-ligyui_part2.png](media/HowToHLI/HASetupWithStonith/zypper-install-ligyui_part2.png) Yast2 burada gösterildiği gibi şimdi grafik görünümü açmak gerekir.
![yast2-kontrol-center.png](media/HowToHLI/HASetupWithStonith/yast2-control-center.png)

### <a name="scenario-3-yast2-does-not-high-availability-option"></a>Senaryo 3: yast2 Yüksek Kullanılabilirlik seçeneği değil
Yüksek Kullanılabilirlik seçeneğinin yast2 kontrol merkezinde görülebilmesi için ek paketleri yüklemeniz gerekir.

Yast2>Yazılım>Yazılım yönetimini kullanma>Aşağıdaki desenleri seçin

- SAP HANA sunucu tabanı
- C/C++ Derleyici ve araçları
- Yüksek kullanılabilirlik
- SAP Uygulama sunucu tabanı

Aşağıdaki ekranda desenleri yüklemek için adımları gösterir.

Yast2 > Yazılım > Yazılım Yönetimi

![yast2-kontrol-center.png](media/HowToHLI/HASetupWithStonith/yast2-control-center.png)

Desenleri seçin

![yast-pattern1.png](media/HowToHLI/HASetupWithStonith/yast-pattern1.png)
![yast-pattern2.png](media/HowToHLI/HASetupWithStonith/yast-pattern2.png)

**Kabul Et'i** tıklatın

![yast-changed-packages.png](media/HowToHLI/HASetupWithStonith/yast-changed-packages.png)

**Devam Et'i** tıklatın

![yast2-performans-installation.png](media/HowToHLI/HASetupWithStonith/yast2-performing-installation.png)

Yükleme tamamlandığında **İleri'yi** tıklatın

![yast2-kurulum-report.png](media/HowToHLI/HASetupWithStonith/yast2-installation-report.png)

### <a name="scenario-4-hana-installation-fails-with-gcc-assemblies-error"></a>Senaryo 4: HANA Kurulumu gcc montajları hatası ile başarısız
HANA yüklemeaşağıdaki hata ile başarısız olur.

![Hana-kurulum-error.png](media/HowToHLI/HASetupWithStonith/Hana-installation-error.png)

Sorunu gidermek için kitaplıkları (libgcc_sl ve libstdc++6) aşağıdaki gibi yüklemeniz gerekir.

![zypper-install-lib.png](media/HowToHLI/HASetupWithStonith/zypper-install-lib.png)

### <a name="scenario-5-pacemaker-service-fails"></a>Senaryo 5: Kalp pili hizmeti başarısız olur

Aşağıdaki sorun kalp pili hizmeti nin başlatılması sırasında oluştu.

```
sapprdhdb95:/ # systemctl start pacemaker
A dependency job for pacemaker.service failed. See 'journalctl -xn' for details.
```
```
sapprdhdb95:/ # journalctl -xn
-- Logs begin at Thu 2017-09-28 09:28:14 EDT, end at Thu 2017-09-28 21:48:27 EDT. --
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync configuration map
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [QB    ] withdrawing server sockets
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync configuration ser
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [QB    ] withdrawing server sockets
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync cluster closed pr
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [QB    ] withdrawing server sockets
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync cluster quorum se
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [SERV  ] Service engine unloaded: corosync profile loading s
Sep 28 21:48:27 sapprdhdb95 corosync[68812]: [MAIN  ] Corosync Cluster Engine exiting normally
Sep 28 21:48:27 sapprdhdb95 systemd[1]: Dependency failed for Pacemaker High Availability Cluster Manager
-- Subject: Unit pacemaker.service has failed
-- Defined-By: systemd
-- Support: https://lists.freedesktop.org/mailman/listinfo/systemd-devel
--
-- Unit pacemaker.service has failed.
--
-- The result is dependency.
```
```
sapprdhdb95:/ # tail -f /var/log/messages
2017-09-28T18:44:29.675814-04:00 sapprdhdb95 corosync[57600]:   [QB    ] withdrawing server sockets
2017-09-28T18:44:29.676023-04:00 sapprdhdb95 corosync[57600]:   [SERV  ] Service engine unloaded: corosync cluster closed process group service v1.01
2017-09-28T18:44:29.725885-04:00 sapprdhdb95 corosync[57600]:   [QB    ] withdrawing server sockets
2017-09-28T18:44:29.726069-04:00 sapprdhdb95 corosync[57600]:   [SERV  ] Service engine unloaded: corosync cluster quorum service v0.1
2017-09-28T18:44:29.726164-04:00 sapprdhdb95 corosync[57600]:   [SERV  ] Service engine unloaded: corosync profile loading service
2017-09-28T18:44:29.776349-04:00 sapprdhdb95 corosync[57600]:   [MAIN  ] Corosync Cluster Engine exiting normally
2017-09-28T18:44:29.778177-04:00 sapprdhdb95 systemd[1]: Dependency failed for Pacemaker High Availability Cluster Manager.
2017-09-28T18:44:40.141030-04:00 sapprdhdb95 systemd[1]: [/usr/lib/systemd/system/fstrim.timer:8] Unknown lvalue 'Persistent' in section 'Timer'
2017-09-28T18:45:01.275038-04:00 sapprdhdb95 cron[57995]: pam_unix(crond:session): session opened for user root by (uid=0)
2017-09-28T18:45:01.308066-04:00 sapprdhdb95 CRON[57995]: pam_unix(crond:session): session closed for user root
```

Düzeltmek için aşağıdaki satırı dosya */usr/lib/systemd/system/fstrim.timer'dan* silin

```
Persistent=true
```

![Persistent.png](media/HowToHLI/HASetupWithStonith/Persistent.png)

### <a name="scenario-6-node-2-unable-to-join-the-cluster"></a>Senaryo 6: Düğüm 2 kümeye katılamıyor

Node2'yi *ha-cluster-join* komutunu kullanarak varolan kümeye katıldığında aşağıdaki hata oluştu.

```
ERROR: Can’t retrieve SSH keys from <Primary Node>
```

![ha-cluster-join-error.png](media/HowToHLI/HASetupWithStonith/ha-cluster-join-error.png)

Düzeltmek için, her iki düğümde de aşağıdakileri çalıştırın

```
ssh-keygen -q -f /root/.ssh/id_rsa -C 'Cluster Internal' -N ''
cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
```

![ssh-keygen-node1. Png](media/HowToHLI/HASetupWithStonith/ssh-keygen-node1.PNG)

![ssh-keygen-node2. Png](media/HowToHLI/HASetupWithStonith/ssh-keygen-node2.PNG)

Önceki düzeltmeden sonra, düğüm2 kümeye eklenmelidir

![ha-cluster-join-fix.png](media/HowToHLI/HASetupWithStonith/ha-cluster-join-fix.png)

## <a name="10-general-documentation"></a>10. Genel Dokümantasyon
Aşağıdaki makalelerde SUSE HA kurulumu hakkında daha fazla bilgi bulabilirsiniz: 

- [SAP HANA SR Performans Optimize Senaryo](https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf )
- [Depolama tabanlı eskrim](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_storage_protect_fencing.html)
- [Blog - SAP HANA için Kalp Pili Küme kullanma- Bölüm 1](https://blogs.sap.com/2017/11/19/be-prepared-for-using-pacemaker-cluster-for-sap-hana-part-1-basics/)
- [Blog - SAP HANA için Pacemaker Küme kullanma- Bölüm 2](https://blogs.sap.com/2017/11/19/be-prepared-for-using-pacemaker-cluster-for-sap-hana-part-2-failure-of-both-nodes/)
