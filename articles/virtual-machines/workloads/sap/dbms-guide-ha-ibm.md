---
title: IBM Db2 HADR'ı Azure sanal makinelerinde (VM) ayarlama | Microsoft Dokümanlar
description: Azure sanal makinelerde (VM) IBM Db2 LUW'un yüksek kullanılabilirliğini belirleyin.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: SAP
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/06/2020
ms.author: juergent
ms.openlocfilehash: 614ac8b651224a3b6ec605a6bffd520449a1d793
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78926738"
---
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[1612105]:https://launchpad.support.sap.com/#/notes/1612105

[sles-for-sap-bp]:https://www.suse.com/documentation/sles-for-sap-12/
[db2-hadr-11.1]:https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html
[db2-hadr-10.5]:https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html
[dbms-db2]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm
[sles-pacemaker]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker
[sap-instfind]:https://help.sap.com/viewer/9e41ead9f54e44c1ae1a1094b0f80712/ALL/en-US/576f5c1808de4d1abecbd6e503c9ba42.html
[nfs-ha]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs
[sles-ha-guide]:https://www.suse.com/releasenotes/x86_64/SLE-HA/12-SP4/
[ascs-ha]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md
[azr-sap-plancheck]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist



# <a name="high-availability-of-ibm-db2-luw-on-azure-vms-on-suse-linux-enterprise-server-with-pacemaker"></a>Pacemaker ile SUSE Linux Enterprise Server'da Azure VM'lerde IBM Db2 LUW'un yüksek kullanılabilirliği

[Yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR) yapılandırmasında,](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html) Linux, UNIX ve Windows (LUW) için IBM Db2, birincil veritabanı örneğini çalıştıran bir düğüm ve ikincil veritabanı örneğini çalıştıran en az bir düğümden oluşur. Birincil veritabanı örneğindeki değişiklikler, yapılandırmanıza bağlı olarak ikincil bir veritabanı örneğine eşzamanlı olarak veya eşzamanlı olarak çoğaltılır. 

Bu makalede, Azure sanal makinelerinin (VM) nasıl dağıtılanın ve yapılandırılabilmek, küme çerçevesini yüklemek ve HADR yapılandırması ile IBM Db2 LUW'u nasıl yükleyebilirsiniz. 

Makale, IBM Db2 LUW'un HADR veya SAP yazılım yüklemesi ile nasıl yüklenilip yapılandırılabildiğini kapsamaz. Bu görevleri gerçekleştirmenize yardımcı olmak için SAP ve IBM yükleme kılavuzlarına başvurular salıyoruz. Bu makalede, Azure ortamına özgü parçalar üzerinde duruluyor. 

Desteklenen IBM Db2 sürümleri SAP not [1928533'te]belgelenen 10,5 ve daha sonraki sürümlerdir.

Yüklemeye başlamadan önce aşağıdaki SAP notlarına ve belgelerine bakın:

| SAP notu | Açıklama |
| --- | --- |
| [1928533] | Azure'daki SAP uygulamaları: Desteklenen ürünler ve Azure VM türleri |
| [2015553] | Azure'da SAP: Ön koşulları destekleyin |
| [2178632] | Azure'da SAP için temel izleme ölçümleri |
| [2191498] | Azure ile Linux'ta SAP: Gelişmiş izleme |
| [2243692] | Linux on Azure (IaaS) VM: SAP lisans sorunları |
| [1984787] | SUSE LINUX Enterprise Server 12: Kurulum notları |
| [1999351] | SAP için gelişmiş Azure izleme sorun giderme |
| [2233094] | DB6: Linux, UNIX ve Windows için IBM Db2 kullanan Azure'daki SAP uygulamaları - ek bilgiler |
| [1612105] | DB6: HADR ile Db2'de SSS |


| Belgeler | 
| --- |
| [SAP Community Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes): Linux için gerekli tüm SAP Notları |
| [Linux kılavuzunda SAP için Azure Sanal Makineler planlama ve uygulama][planning-guide] |
| [Linux'ta SAP için Azure Sanal Makineler dağıtımı][deployment-guide] (bu makale) |
| [Linux kılavuzunda SAP için Azure Virtual Machines veritabanı yönetim sistemi (DBMS) dağıtımı][dbms-guide] |
| [Azure planlama ve dağıtım denetim listesinde SAP iş yükü][azr-sap-plancheck] |
| [SAP Applications 12 SP4 en iyi uygulamalar kılavuzları için SUSE Linux Enterprise Server][sles-for-sap-bp] |
| [SUSE Linux Enterprise Yüksek Kullanılabilirlik Uzantısı 12 SP4][sles-ha-guide] |
| [SAP iş yükü için IBM Db2 Azure Sanal Makineler DBMS dağıtımı][dbms-db2] |
| [IBM Db2 HADR 11.1][db2-hadr-11.1] |
| [IBM Db2 HADR R 10.5][db2-hadr-10.5] |

## <a name="overview"></a>Genel Bakış
Yüksek kullanılabilirlik elde etmek için, HADR'li IBM Db2 LUW, Azure [kullanılabilirlik kümesinde](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) veya [Azure Kullanılabilirlik Bölgeleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ha-availability-zones)arasında dağıtılan en az iki Azure sanal makinesine yüklenir. 

Aşağıdaki grafikler, iki veritabanı sunucusu Azure VM'nin kurulumlarını görüntüler. Her iki veritabanı sunucusu Azure VM'lerin de kendi depolama alanı vardır ve çalışır durumdadır. HADR'de, Azure VM'lerinden birinde bulunan bir veritabanı örneği birincil örnek rolüne sahiptir. Tüm istemciler bu birincil örne bağlıdır. Veritabanı hareketlerinde yapılan tüm değişiklikler Db2 hareket günlüğünde yerel olarak devam etti. Hareket günlüğü kayıtları yerel olarak kalıcı olduğundan, kayıtlar TCP/IP üzerinden ikinci veritabanı sunucusundaki veritabanı örneğine, bekleme sunucusuna veya bekleme örneğine aktarılır. Bekleme örneği, aktarılan hareket günlüğü kayıtlarını ileterek yerel veritabanını güncelleştirir. Bu şekilde, bekleme sunucusu birincil sunucu yla eşit tutulur.

HADR yalnızca bir çoğaltma işlevidir. Hiçbir arıza algılama ve hiçbir otomatik devralma veya arıza tesisleri vardır. Bekleme sunucusuna devralma veya aktarma, bir veritabanı yöneticisi tarafından el ile başlatılmalıdır. Otomatik devralma ve hata algılaması elde etmek için Linux Pacemaker kümeleme özelliğini kullanabilirsiniz. Pacemaker iki veritabanı sunucusu örneklerini izler. Birincil veritabanı sunucusu örneği çöktüğinde, Pacemaker bekleme sunucusu tarafından *otomatik* bir HADR devralma başlatır. Pacemaker ayrıca sanal IP adresinin yeni birincil sunucuya atanmasını sağlar.

![IBM Db2 yüksek kullanılabilirlik genel bakış](./media/dbms-guide-ha-ibm/ha-db2-hadr-lb.png)

SAP uygulama sunucuların birincil veritabanına bağlanması için sanal bir ana bilgisayar adı ve sanal IP adresi gerekir. Bir hata durumunda, SAP uygulama sunucuları yeni birincil veritabanı örneğine bağlanır. Azure ortamında, bir [Azure yük dengeleyicisinin,](https://microsoft.sharepoint.com/teams/WAG/AzureNetworking/Wiki/Load%20Balancing.aspx) IBM Db2'nin HADR'ı için gerekli olan şekilde sanal bir IP adresi kullanması gerekir. 

HADR ve Pacemaker ile IBM Db2 LUW'un son derece kullanılabilir bir SAP sistem kurulumuna nasıl uyduğunu tam olarak anlamanıza yardımcı olmak için, aşağıdaki resim IBM Db2 veritabanına dayalı bir SAP sisteminin son derece kullanılabilir kurulumuna genel bir bakış sunar. Bu makale yalnızca IBM Db2'yi kapsar, ancak sap sisteminin diğer bileşenlerinin nasıl ayarlanıla ilgili diğer makalelere başvurular sağlar.

![IBM DB2 yüksek kullanılabilirlik tam ortama genel bakış](.//media/dbms-guide-ha-ibm/end-2-end-ha.png)


### <a name="high-level-overview-of-the-required-steps"></a>Gerekli adımlara üst düzey genel bakış
IBM Db2 yapılandırmasını dağıtmak için aşağıdaki adımları izlemeniz gerekir:

  + Çevrenizi planlayın.
  + VM'leri dağıtın.
  + SUSE Linux'u güncelleyin ve dosya sistemlerini yapılandırın.
  + Pacemaker'ı yükleyin ve yapılandırın.
  + [Son derece kullanılabilir NFS yükleyin.][nfs-ha]
  + [ASCS/ERS'i ayrı bir kümeye][ascs-ha]yükleyin.
  + Dağıtılmış/Yüksek Kullanılabilirlik seçeneği (SWPM) ile IBM Db2 veritabanını yükleyin.
  + İkincil bir veritabanı düğümü ve örneği yükleyin ve oluşturun ve HADR'yi yapılandırın.
  + HADR'ın çalıştığını doğrulayın.
  + IBM Db2'yi denetlemek için Pacemaker yapılandırmasını uygulayın.
  + Azure Yük Dengeleyicisi'ni yapılandırın.
  + Birincil ve iletişim uygulama sunucularını yükleyin.
  + SAP uygulama sunucularının yapılandırmasını denetleyin ve uyarlanın.
  + Başarısız ve devralma testleri gerçekleştirin.



## <a name="plan-azure-infrastructure-for-hosting-ibm-db2-luw-with-hadr"></a>HADR ile IBM Db2 LUW barındırma için Azure altyapı planı

Dağıtımı gerçekleştirmeden önce planlama işlemini tamamlayın. Planlama, Azure'da HADR ile Db2 yapılandırmasını dağıtmanın temelini oluşturur. IMB Db2 LUW (SAP ortamının veritabanı parçası) için planlamanın bir parçası olması gereken temel öğeler aşağıdaki tabloda listelenmiştir:

| Konu başlığı | Kısa açıklama |
| --- | --- |
| Azure kaynak gruplarını tanımlama | VM, VNet, Azure Yük Bakiyesi ve diğer kaynakları dağıttığınız kaynak grupları. Varolan veya yeni olabilir. |
| Sanal ağ / Subnet tanımı | IBM Db2 ve Azure Yük Dengeleyicisi için VM'lerin dağıtıldığı yer. Varolabilir veya yeni oluşturulabilir. |
| IBM Db2 LUW barındıran sanal makineler | VM boyutu, depolama, ağ, IP adresi. |
| IBM Db2 veritabanı için sanal ana bilgisayar adı ve sanal IP| SAP uygulama sunucularının bağlanması için kullanılan sanal IP veya ana bilgisayar adı. **db-virt-hostname**, **db-virt-ip**. |
| Azure eskrim | Azure eskrim veya SBD eskrim (şiddetle önerilir). Beyin durumlarını bölmek için yöntem. |
| SBD VM | SBD sanal makine boyutu, depolama, ağ. |
| Azure Load Balancer | Temel veya Standart kullanımı (önerilir), Db2 veritabanı için prob bağlantı noktası (tavsiyemiz 62500) **prob-port.** |
| Ad çözümlemesi| Ad çözümlemesi ortamda nasıl çalışır? DNS hizmeti şiddetle tavsiye edilir. Yerel ana bilgisayarlar dosyası kullanılabilir. |
    
Azure'da Linux Pacemaker hakkında daha fazla bilgi için [Azure'da SUSE Linux Enterprise Server'da Pacemaker'ı kur'a](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)bakın.

## <a name="deployment-on-suse-linux"></a>SUSE Linux'ta Dağıtım

IBM Db2 LUW'un kaynak aracısı SAP Uygulamaları için SUSE Linux Enterprise Server'a dahildir. Bu belgede açıklanan kurulum için SAP Uygulamaları için SUSE Linux Server'ı kullanmanız gerekir. Azure Marketi, SAP Applications 12 için SUSE Enterprise Server için yeni Azure sanal makineleri dağıtmak için kullanabileceğiniz bir görüntü içerir. Azure VM Marketplace'te bir VM görüntüsü seçtiğinizde Azure Marketi aracılığıyla SUSE tarafından sunulan çeşitli destek veya hizmet modellerine dikkat edin. 

### <a name="hosts-dns-updates"></a>Ana bilgisayarlar: DNS güncelleştirmeleri
Sanal ana bilgisayar adları da dahil olmak üzere tüm ana bilgisayar adlarının bir listesini yapın ve uygun IP adresini ana bilgisayar adı çözümlemesi için etkinleştirmek için DNS sunucularınızı güncelleştirin. Bir DNS sunucusu yoksa veya DNS girişlerini güncelleştirip oluşturamıyorsanız, bu senaryoya katılan tek tek VM'lerin yerel ana bilgisayar dosyalarını kullanmanız gerekir. Ana bilgisayar dosyaları girişlerini kullanıyorsanız, girişlerin SAP sistem ortamındaki tüm VM'lere uygulandığından emin olun. Ancak, ideal olarak Azure'a yayılan DNS'nizi kullanmanızı öneririz


### <a name="manual-deployment"></a>El ile dağıtım

Ibm Db2 LUW için seçilen işletim sistemi IBM/SAP tarafından desteklendirildikten emin olun. Azure VM'ler ve Db2 sürümleri için desteklenen işletim sistemi sürümlerinin listesi SAP note [1928533'te]mevcuttur. Bireysel Db2 sürümüne göre işletim sistemi sürümlerinin listesi SAP Ürün Kullanılabilirlik Matrisi'nde mevcuttur. Bu veya daha sonraki SUSE Linux sürümlerinde Azure ile ilgili performans iyileştirmeleri nedeniyle en az SLES 12 SP4 öneririz.

1. Bir kaynak grubu oluşturun veya seçin.
1. Sanal ağ ve alt ağ oluşturun veya seçin.
1. Bir Azure kullanılabilirlik kümesi oluşturun veya bir kullanılabilirlik bölgesi dağıtın.
    + Kullanılabilirlik kümesi için, maksimum güncelleştirme etki alanlarını 2 olarak ayarlayın.
1. Sanal Makine 1 oluşturun.
    + Azure Marketi'nde SAP görüntüsü için SLES'i kullanın.
    + Adım 3'te oluşturduğunuz Azure kullanılabilirlik kümesini veya Kullanılabilirlik Bölgesi'ni seçin.
1.  Sanal Makine 2 oluşturun.
    + Azure Marketi'nde SAP görüntüsü için SLES'i kullanın.
    + Adım 3'te oluşturulan Azure kullanılabilirlik kümesini seçin veya Kullanılabilirlik Bölgesi'ni seçin (adım 3'tekiyle aynı bölge değil).
1. VM'lere veri diskleri ekleyin ve ardından SAP iş yükü [için IBM Db2 Azure Sanal Makineler DBMS dağıtımı][dbms-db2]makalesinde dosya sistemi kurulumu önerisini kontrol edin.

## <a name="create-the-pacemaker-cluster"></a>Kalp Pili kümesini oluşturma
    
Bu IBM Db2 sunucusu için temel bir Kalp Pili kümesi oluşturmak için [Azure'da SUSE Linux Enterprise Server'da Pacemaker'ı kur'a][sles-pacemaker]bakın. 

## <a name="install-the-ibm-db2-luw-and-sap-environment"></a>IBM Db2 LUW ve SAP ortamını yükleme

IBM Db2 LUW tabanlı bir SAP ortamının yüklenmesine başlamadan önce aşağıdaki belgeleri gözden geçirin:

+ Azure belgeleri
+ SAP dokümantasyonu
+ IBM dokümantasyonu

Bu dokümantasyona bağlantılar bu makalenin giriş bölümünde verilmiştir.

IBM Db2 LUW'a NetWeaver tabanlı uygulamalar yükleme yle ilgili SAP yükleme kılavuzlarını kontrol edin.

Sap [Kurulum Kılavuzu Bulucu'yu][sap-instfind]kullanarak kılavuzları SAP Yardım portalında bulabilirsiniz.

Aşağıdaki filtreleri ayarlayarak portalda görüntülenen kılavuz sayısını azaltabilirsiniz:

- Ben istiyorum: "Yeni bir sistem yükleyin"
- Veritabanım: "Linux, Unix ve Windows için IBM Db2"
- SAP NetWeaver sürümleri, yığın yapılandırması veya işletim sistemi için ek filtreler

### <a name="installation-hints-for-setting-up-ibm-db2-luw-with-hadr"></a>HADR ile IBM Db2 LUW kurulumu için kurulum ipuçları

Birincil IBM Db2 LUW veritabanı örneğini ayarlamak için:

- Yüksek kullanılabilirlik veya dağıtılmış seçeneği kullanın.
- SAP ASCS/ERS ve Veritabanı örneğini yükleyin.
- Yeni yüklenen veritabanının yedeğini alın.


> [!IMPORTANT] 
> Yükleme sırasında ayarlanan "Veritabanı İletişimi bağlantı noktasını" yazın. Her iki veritabanı örneği için aynı bağlantı noktası numarası olmalıdır

SAP homojen sistem kopyalama yordamını kullanarak Standby veritabanı sunucusunu kurmak için aşağıdaki adımları uygulayın:

1. **Hedef sistemleri** > **Dağıtılmış** > **Veritabanı örneği**> Sistem **kopyalama** seçeneğini seçin.
1. Kopyalama yöntemi olarak, yedek sunucu örneğinde yedeklemeyi geri yüklemek için yedeklemeyi kullanabilmeniz için **Homojen Sistem'i** seçin.
1. Homojen sistem kopyası için veritabanını geri yüklemek için çıkış adımına ulaştığınızda, yükleyiciden çıkın. Birincil ana bilgisayar yedeklemesinden veritabanını geri yükleyin. Sonraki tüm yükleme aşamaları zaten birincil veritabanı sunucusunda yürütülmüştür.
1. IBM Db2 için HADR'ı ayarlayın.

   > [!NOTE]
   > Azure ve Kalp Pili'ne özel kurulum ve yapılandırma için: SAP Yazılım Sağlama Yöneticisi aracılığıyla yükleme işlemi sırasında IBM Db2 LUW için yüksek kullanılabilirlik hakkında açık bir soru vardır:
   >+ **IBM Db2 pureScale'i**seçmeyin.
   >+ **Çoklu platformlar için IBM Tivoli Sistem Otomasyonu Yükle'yi seçmeyin.**
   >+ **Küme yapılandırma dosyalarını oluştur'u**seçmeyin.

   Linux Pacemaker için bir SBD cihazı kullandığınızda, aşağıdaki Db2 HADR parametrelerini ayarlayın:
   + HADR eş pencere süresi (saniye) (HADR_PEER_WINDOW) = 300  
   + HADR zaman çıkış değeri (HADR_TIMEOUT) = 60

   Bir Azure Pacemaker eskrim aracısı kullandığınızda, aşağıdaki parametreleri ayarlayın:
   + HADR eş pencere süresi (saniye) (HADR_PEER_WINDOW) = 900  
   + HADR zaman çıkış değeri (HADR_TIMEOUT) = 60

İlk arıza/devralma testine dayalı olarak önceki parametreleri öneririz. Bu parametre ayarlarıyla başarısız ve devralmanın doğru işlevselliğini test etmek zorunludur. Tek tek yapılandırmalar değişebildiği için, parametreler ayarlama gerektirebilir. 

> [!IMPORTANT]
> Normal başlangıç ile HADR yapılandırması ile IBM Db2'ye özgü: Birincil veritabanı örneğini başlatmadan önce ikincil veya bekleme veritabanı örneği çalışır durumda olmalıdır.

Gösteri amaçları ve bu makalede açıklanan yordamlar için veritabanı SID **PTR**olduğunu.

#### <a name="ibm-db2-hadr-check"></a>IBM Db2 HADR denetimi
HADR'ı yapılandırdıktan ve durum BIRINCIL ve bekleme düğümlerinde PEER ve CONNECTED olduktan sonra aşağıdaki denetimi yapın:

<pre><code>
Execute command as db2&lt;sid&gt; db2pd -hadr -db &lt;SID&gt;

#Primary output:
# Database Member 0 -- Database PTR -- Active -- Up 1 days 01:51:38 -- Date 2019-02-06-15.35.28.505451
# 
#                             <b>HADR_ROLE = PRIMARY
#                           REPLAY_TYPE = PHYSICAL
#                         HADR_SYNCMODE = NEARSYNC
#                            STANDBY_ID = 1
#                         LOG_STREAM_ID = 0
#                            HADR_STATE = PEER
#                            HADR_FLAGS = TCP_PROTOCOL
#                   PRIMARY_MEMBER_HOST = azibmdb02
#                      PRIMARY_INSTANCE = db2ptr
#                        PRIMARY_MEMBER = 0
#                   STANDBY_MEMBER_HOST = azibmdb01
#                      STANDBY_INSTANCE = db2ptr
#                        STANDBY_MEMBER = 0
#                   HADR_CONNECT_STATUS = CONNECTED</b>
#              HADR_CONNECT_STATUS_TIME = 02/05/2019 13:51:47.170561 (1549374707)
#           HEARTBEAT_INTERVAL(seconds) = 15
#                      HEARTBEAT_MISSED = 0
#                    HEARTBEAT_EXPECTED = 6137
#                 HADR_TIMEOUT(seconds) = 60
#         TIME_SINCE_LAST_RECV(seconds) = 13
#              PEER_WAIT_LIMIT(seconds) = 0
#            LOG_HADR_WAIT_CUR(seconds) = 0.000
#     LOG_HADR_WAIT_RECENT_AVG(seconds) = 0.000025
#    LOG_HADR_WAIT_ACCUMULATED(seconds) = 434.595
#                   LOG_HADR_WAIT_COUNT = 223713
# SOCK_SEND_BUF_REQUESTED,ACTUAL(bytes) = 0, 46080
# SOCK_RECV_BUF_REQUESTED,ACTUAL(bytes) = 0, 374400
#             PRIMARY_LOG_FILE,PAGE,POS = S0000280.LOG, 15571, 27902548040
#             STANDBY_LOG_FILE,PAGE,POS = S0000280.LOG, 15571, 27902548040
#                   HADR_LOG_GAP(bytes) = 0
#      STANDBY_REPLAY_LOG_FILE,PAGE,POS = S0000280.LOG, 15571, 27902548040
#        STANDBY_RECV_REPLAY_GAP(bytes) = 0
#                      PRIMARY_LOG_TIME = 02/06/2019 15:34:39.000000 (1549467279)
#                      STANDBY_LOG_TIME = 02/06/2019 15:34:39.000000 (1549467279)
#               STANDBY_REPLAY_LOG_TIME = 02/06/2019 15:34:39.000000 (1549467279)
#          STANDBY_RECV_BUF_SIZE(pages) = 2048
#              STANDBY_RECV_BUF_PERCENT = 0
#            STANDBY_SPOOL_LIMIT(pages) = 0
#                 STANDBY_SPOOL_PERCENT = NULL
#                    STANDBY_ERROR_TIME = NULL
#                  PEER_WINDOW(seconds) = 300
#                       PEER_WINDOW_END = 02/06/2019 15:40:25.000000 (1549467625)
#              READS_ON_STANDBY_ENABLED = N

#Secondary output:
# Database Member 0 -- Database PTR -- Standby -- Up 1 days 01:46:43 -- Date 2019-02-06-15.38.25.644168
# 
#                             <b>HADR_ROLE = STANDBY
#                           REPLAY_TYPE = PHYSICAL
#                         HADR_SYNCMODE = NEARSYNC
#                            STANDBY_ID = 0
#                         LOG_STREAM_ID = 0
#                            HADR_STATE = PEER
#                            HADR_FLAGS = TCP_PROTOCOL
#                   PRIMARY_MEMBER_HOST = azibmdb02
#                      PRIMARY_INSTANCE = db2ptr
#                        PRIMARY_MEMBER = 0
#                   STANDBY_MEMBER_HOST = azibmdb01
#                      STANDBY_INSTANCE = db2ptr
#                        STANDBY_MEMBER = 0
#                   HADR_CONNECT_STATUS = CONNECTED</b>
#              HADR_CONNECT_STATUS_TIME = 02/05/2019 13:51:47.205067 (1549374707)
#           HEARTBEAT_INTERVAL(seconds) = 15
#                      HEARTBEAT_MISSED = 0
#                    HEARTBEAT_EXPECTED = 6186
#                 HADR_TIMEOUT(seconds) = 60
#         TIME_SINCE_LAST_RECV(seconds) = 5
#              PEER_WAIT_LIMIT(seconds) = 0
#            LOG_HADR_WAIT_CUR(seconds) = 0.000
#     LOG_HADR_WAIT_RECENT_AVG(seconds) = 0.000023
#    LOG_HADR_WAIT_ACCUMULATED(seconds) = 434.595
#                   LOG_HADR_WAIT_COUNT = 223725
# SOCK_SEND_BUF_REQUESTED,ACTUAL(bytes) = 0, 46080
# SOCK_RECV_BUF_REQUESTED,ACTUAL(bytes) = 0, 372480
#             PRIMARY_LOG_FILE,PAGE,POS = S0000280.LOG, 15574, 27902562173
#             STANDBY_LOG_FILE,PAGE,POS = S0000280.LOG, 15574, 27902562173
#                   HADR_LOG_GAP(bytes) = 0
#      STANDBY_REPLAY_LOG_FILE,PAGE,POS = S0000280.LOG, 15574, 27902562173
#        STANDBY_RECV_REPLAY_GAP(bytes) = 155
#                      PRIMARY_LOG_TIME = 02/06/2019 15:37:34.000000 (1549467454)
#                      STANDBY_LOG_TIME = 02/06/2019 15:37:34.000000 (1549467454)
#               STANDBY_REPLAY_LOG_TIME = 02/06/2019 15:37:34.000000 (1549467454)
#          STANDBY_RECV_BUF_SIZE(pages) = 2048
#              STANDBY_RECV_BUF_PERCENT = 0
#            STANDBY_SPOOL_LIMIT(pages) = 0
#                 STANDBY_SPOOL_PERCENT = NULL
#                    STANDBY_ERROR_TIME = NULL
#                  PEER_WINDOW(seconds) = 300
#                       PEER_WINDOW_END = 02/06/2019 15:43:19.000000 (1549467799)
#              READS_ON_STANDBY_ENABLED = N
</code></pre>



## <a name="db2-pacemaker-configuration"></a>Db2 Kalp Pili yapılandırması

Bir düğüm hatası durumunda otomatik arıza için Pacemaker kullandığınızda, Db2 örneklerinizi ve Kalp Pilinizi buna göre yapılandırmanız gerekir. Bu bölümde bu tür bir yapılandırma açıklanmaktadır.

Aşağıdaki öğeler aşağıdakilerden biri ile önceden belirlenmiştir:

- **[A]**: Tüm düğümler için geçerlidir
- **[1]**: Yalnızca düğüm 1 için geçerlidir 
- **[2]**: Yalnızca düğüm 2 için geçerlidir

**[A]** Pacemaker yapılandırması için ön koşullar:
1. Db2stop ile> kullanıcı\<db2 sid ile her iki veritabanı sunucularını kapatın.
1. Db2\<sid> kullanıcısı için kabuk ortamını */bin/ksh*olarak değiştirin. Yast aracını kullanmanızı öneririz. 


### <a name="pacemaker-configuration"></a>Kalp pili yapılandırması

> [!IMPORTANT]
> Son testler, netcat'in biriktirme listesi ve yalnızca bir bağlantı taşıma sınırlaması nedeniyle isteklere yanıt vermeyi bıraktığı durumları ortaya çıkardı. Netcat kaynağı Azure Yük dengeleyici isteklerini dinlemeyi durdurur ve kayan IP kullanılamaz hale gelir.  
> Mevcut Pacemaker kümeleri için, geçmişte netcat'i socat ile değiştirmemizi tavsiye ettik. Şu anda, paket kaynak aracılarının bir parçası olan azure-lb kaynak aracısını aşağıdaki paket sürüm gereksinimleriyle birlikte kullanmanızı öneririz:
> - SLES 12 SP4/SP5 için, sürüm en az kaynak-aracılar-4.3.018.a7fb5035-3.30.1 olmalıdır.  
> - SLES 15/15 SP1 için, sürüm en az kaynak-ajanlar-4.3.0184.6ee15eb2-4.13.1 olmalıdır.  
>
> Değişikliğin kısa bir kapalı kalma süresi gerektireceğini unutmayın.  
> Varolan Pacemaker kümeleri için yapılandırma Azure [Load-Balancer Detection Hardening'de](https://www.suse.com/support/kb/doc/?id=7024128)açıklandığı gibi socat kullanmak üzere zaten değiştirildiyse, hemen azure-lb kaynak aracısına geçmeniz gerek yoktur.

**[1]** IBM Db2 HADR'a özel Pacemaker yapılandırması:
<pre><code># Put Pacemaker into maintenance mode
sudo crm configure property maintenance-mode=true
</code></pre>

**[1]** IBM Db2 kaynakları oluşturun:
<pre><code># Replace **bold strings** with your instance name db2sid, database SID, and virtual IP address/Azure Load Balancer.

sudo crm configure primitive rsc_Db2_db2ptr_<b>PTR</b> db2 \
        params instance="<b>db2ptr</b>" dblist="<b>PTR</b>" \
        op start interval="0" timeout="130" \
        op stop interval="0" timeout="120" \
        op promote interval="0" timeout="120" \
        op demote interval="0" timeout="120" \
        op monitor interval="30" timeout="60" \
        op monitor interval="31" role="Master" timeout="60"

# Configure virtual IP - same as Azure Load Balancer IP
sudo crm configure primitive rsc_ip_db2ptr_<b>PTR</b> IPaddr2 \
        op monitor interval="10s" timeout="20s" \
        params ip="<b>10.100.0.10</b>"

# Configure probe port for Azure load Balancer
sudo crm configure primitive rsc_nc_db2ptr_<b>PTR</b> azure-lb port=<b>62500</b>

sudo crm configure group g_ip_db2ptr_<b>PTR</b> rsc_ip_db2ptr_<b>PTR</b> rsc_nc_db2ptr_<b>PTR</b>

sudo crm configure ms msl_Db2_db2ptr_<b>PTR</b> rsc_Db2_db2ptr_<b>PTR</b> \
        meta target-role="Started" notify="true"

sudo crm configure colocation col_db2_db2ptr_<b>PTR</b> inf: g_ip_db2ptr_<b>PTR</b>:Started msl_Db2_db2ptr_<b>PTR</b>:Master

sudo crm configure order ord_db2_ip_db2ptr_<b>PTR</b> inf: msl_Db2_db2ptr_<b>PTR</b>:promote g_ip_db2ptr_<b>PTR</b>:start

sudo crm configure rsc_defaults resource-stickiness=1000
sudo crm configure rsc_defaults migration-threshold=5000
</code></pre>

**[1]** IBM Db2 kaynaklarını başlatın:
* Pacemaker'ı bakım modundan çıkar.
<pre><code># Put Pacemaker out of maintenance-mode - that start IBM Db2
sudo crm configure property maintenance-mode=false</pre></code>

**[1]** Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıklarından emin olun. Kaynakların hangi düğümüzerinde çalıştığının önemli olmadığı önemli değildir.
<pre><code>sudo crm status</code>

# <a name="2-nodes-configured"></a>2 düğüm yapılandırıldı
# <a name="5-resources-configured"></a>5 kaynak yapılandırıldı

# <a name="online--azibmdb01-azibmdb02-"></a>Online: [ azibmdb01 azibmdb02 ]

# <a name="full-list-of-resources"></a>Kaynakların tam listesi:

#  <a name="stonith-sbd----stonithexternalsbd-started-azibmdb02"></a>stonith-sbd (stonith:external/sbd): azibmdb02 başladı
#  <a name="resource-group-g_ip_db2ptr_ptr"></a>Kaynak Grubu: g_ip_db2ptr_PTR
#      <a name="rsc_ip_db2ptr_ptr--ocfheartbeatipaddr2-------started-azibmdb02"></a>rsc_ip_db2ptr_PTR (ocf::kalp atışı:iPaddr2): Azibmdb02 başladı
#      <a name="rsc_nc_db2ptr_ptr--ocfheartbeatazure-lb------started-azibmdb02"></a>rsc_nc_db2ptr_PTR (ocf::heartbeat:azure-lb): azibmdb02 başladı
#  <a name="masterslave-set-msl_db2_db2ptr_ptr-rsc_db2_db2ptr_ptr"></a>Master / Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
#      <a name="masters--azibmdb02-"></a>Ustalar: [ azibmdb02 ]
#      <a name="slaves--azibmdb01-"></a>Köleler: [ azibmdb01 ]
</pre>

> [!IMPORTANT]
> Pacemaker araçları kullanarak Pacemaker kümelenmiş Db2 örneğini yönetmelisiniz. Db2stop gibi db2 komutlarını kullanırsanız, Pacemaker eylemi kaynağın başarısızlığı olarak algılar. Bakım yapıyorsanız, düğümleri veya kaynakları bakım moduna alabilirsiniz. Kalp pili izleme kaynaklarını askıya alıyor ve daha sonra normal db2 yönetim komutlarını kullanabilirsiniz.


### <a name="configure-azure-load-balancer"></a>Azure Load Balancer'ı yapılandırma
Azure Yük Dengeleyicisini yapılandırmak için [Azure Standart Yük Dengeleyicisi SKU'yu](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) kullanmanızı ve ardından aşağıdakileri yapmanızı öneririz;

> [!NOTE]
> Standart Yük Dengeleyici SKU' su, Yük Dengeleyicisi'nin altındaki düğümlerden genel IP adreslerine erişme kısıtlamalarına sahiptir. SAP [yüksek kullanılabilirlik senaryolarında Azure Standart Yük Dengeleyicisini kullanan Sanal Makineler için genel uç nokta bağlantısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-standard-load-balancer-outbound-connections) makalesi, bu düğümlerin ortak IP adreslerine erişmesini nasıl etkinleştirmenin yollarını anlatıyor

1. Ön uç IP havuzu oluşturun:

   a. Azure portalında Azure Yük Bakiyesini açın, **ön uç IP havuzunu**seçin ve sonra **Ekle'yi**seçin.

   b. Yeni ön uç IP havuzunun adını girin (örneğin, **Db2 bağlantısı).**

   c. **Atamayı** **Statik**olarak ayarlayın ve başlangıçta tanımlanan IP adresini **Sanal-IP** girin.

   d. **Tamam'ı**seçin.

   e. Yeni ön uç IP havuzu oluşturulduktan sonra, havuz IP adresine dikkat edin.

1. Arka uç havuzu oluşturun:

   a. Azure portalında Azure Yük Bakiyesini açın, **arka uç havuzlarını**seçin ve sonra **Ekle'yi**seçin.

   b. Yeni arka uç havuzunun adını girin (örneğin, **Db2 arka uç).**

   c. **Sanal makine ekle'yi**seçin.

   d. Önceki adımda oluşturulan kullanılabilirlik kümesini veya IBM Db2 veritabanını barındıran sanal makineleri seçin.

   e. IBM Db2 kümesinin sanal makinelerini seçin.

   f. **Tamam'ı**seçin.

1. Bir sistem durumu sondası oluşturun:

   a. Azure portalında Azure Yük Bakiyesini açın, **sistem durumu sondalarını**seçin ve **Ekle'yi**seçin.

   b. Yeni sistem durumu sondasının adını girin (örneğin, **Db2-hp).**

   c. Protokol olarak **TCP'yi** ve **62500**bağlantı noktasını seçin. **Aralık** değerini **5**olarak ayarlayın ve **Sağlıksız eşik** değerini **2**olarak ayarlayın.

   d. **Tamam'ı**seçin.

1. Yük dengeleme kurallarını oluşturun:

   a. Azure portalında Azure Yük Bakiyesi'ni açın, **Yük dengeleme kurallarını**seçin ve sonra **Ekle'yi**seçin.

   b. Yeni Yük Dengeleyici kuralının adını girin (örneğin, **Db2-SID).**

   c. Ön uç IP adresini, arka uç havuzunu ve daha önce oluşturduğunuz sistem durumu sondasını (örneğin, **Db2-frontend)** seçin.

   d. **Protokol'ün** **TCP**olarak ayarlı olmasını ve bağlantı noktası *Veritabanı İletişimi bağlantı noktasını*girin.

   e. **Boşta kalma süresini** 30 dakikaya çıkarın.

   f. **Kayan IP'yi etkinleştirdiğinden**emin olun.

   g. **Tamam'ı**seçin.


### <a name="make-changes-to-sap-profiles-to-use-virtual-ip-for-connection"></a>Bağlantı için sanal IP kullanmak için SAP profillerinde değişiklik yapma
HADR yapılandırmasının birincil örneğine bağlanmak için SAP uygulama katmanının Azure Yük Dengeleyicisi için tanımladığınız ve yapılandırdığınız sanal IP adresini kullanması gerekir. Aşağıdaki değişiklikler gereklidir:

/sapmnt/\<SID>/profil/VARSAYıLAN. PFL
<pre><code>SAPDBHOST = db-virt-hostname
j2ee/dbhost = db-virt-hostname
</code></pre>

/sapmnt/\<SID>/global/db6/db2cli.ini
<pre><code>Hostname=db-virt-hostname
</code></pre>



## <a name="install-primary-and-dialog-application-servers"></a>Birincil ve iletişim uygulama sunucularını yükleme

Birincil ve iletişim uygulama sunucularını Db2 HADR yapılandırmasına karşı yüklediğinizde, yapılandırma için seçtiğiniz sanal ana bilgisayar adını kullanın. 

Yüklemeyi Db2 HADR yapılandırmasını oluşturmadan önce gerçekleştirdiyseniz, önceki bölümde açıklandığı gibi ve SAP Java yığınları için aşağıdaki değişiklikleri yapın.

### <a name="abapjava-or-java-stack-systems-jdbc-url-check"></a>ABAP+Java veya Java yığın sistemleri JDBC URL denetimi

JDBC URL'sini kontrol etmek veya güncellemek için J2EE Config aracını kullanın. J2EE Config aracı bir grafik aracı olduğundan, X sunucusunun yüklü olması gerekir:
 
1. J2EE örneğinin birincil uygulama sunucusunda oturum açın ve çalıştırın:`sudo /usr/sap/*SID*/*Instance*/j2ee/configtool/configtool.sh`
1. Sol çerçevede, **güvenlik deposu**seçin.
1. Sağ çerçevede jdbc/pool/\<SAPSID>/url tuşlarını seçin.
1. JDBC URL'deki ana bilgisayar adını sanal ana bilgisayar adı ile değiştirin.
     `jdbc:db2://db-virt-hostname:5912/TSP:deferPrepares=0`
1. **Ekle'yi**seçin.
1. Değişikliklerinizi kaydetmek için sol üstteki disk simgesini seçin.
1. Yapılandırma aracını kapatın.
1. Java örneğini yeniden başlatın.

## <a name="configure-log-archiving-for-hadr-setup"></a>HADR kurulumu için günlük arşivleme yapılandırma
HADR kurulumu için Db2 günlük arşivlemesini yapılandırmak için, tüm günlük arşiv konumlarından otomatik günlük alma özelliğine sahip olmak üzere hem birincil hem de bekleme veritabanını yapılandırmanızı öneririz. Hem birincil hem de bekleme veritabanı, veritabanı örneklerinden birinin günlük dosyalarını arşivleyebileceği tüm günlük arşivi konumlarından günlük arşivi dosyalarını alabilmesi gerekir. 

Günlük arşivleme yalnızca birincil veritabanı tarafından gerçekleştirilir. Veritabanı sunucularının HADR rollerini değiştirirseniz veya bir hata oluşursa, günlük arşivlemeden yeni birincil veritabanı sorumludur. Birden çok günlük arşiv konumu ayarladıysanız, günlükleriniz iki kez arşivlenebilir. Yerel veya uzaktan yakalama durumunda, arşivlenmiş günlükleri eski birincil sunucudan yeni birincil sunucunun etkin günlük konumuna el ile kopyalamanız da gerekebilir.

Günlüklerin her iki düğümden yazıldığı ortak bir NFS paylaşımını yapılandırmanızı öneririz. NFS payı son derece kullanılabilir olmalıdır. 

Mevcut yüksek kullanılabilirNKns paylaşımlarını taşımalar veya profil dizini için kullanabilirsiniz. Daha fazla bilgi için bkz.

- [SUSE Linux Enterprise Server'da Azure VM'lerde NFS için yüksek kullanılabilirlik][nfs-ha] 
- [SAP Uygulamaları için Azure NetApp Dosyaları ile SUSE Linux Enterprise Server'da Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files)
- [Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) (NFS paylaşımları oluşturmak için)


## <a name="test-the-cluster-setup"></a>Küme kurulumını test edin

Bu bölümde Db2 HADR kurulumunuzu nasıl test edebilirsiniz. *Her test, kullanıcı kökü olarak oturum açtığınızı ve* IBM Db2 birincil *inazibmdb01* sanal makinede çalıştığını varsayar.

Tüm test çalışmalarının başlangıç durumu burada açıklanmıştır: (crm_mon -r veya crm durumu)

- **crm durumu** yürütme sırasında Pacemaker durumunun bir anlık görüntüsüdür 
- **crm_mon -r,** Kalp Pili durumunun sürekli çıktısi

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Stopped
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:azure-lb):      Stopped
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     rsc_Db2_db2ptr_PTR      (ocf::heartbeat:db2):   Promoting azibmdb01
     Slaves: [ azibmdb02 ]
</code></pre>

SAP sistemindeki özgün durum, aşağıdaki resimde gösterildiği gibi İşlem DBACOCKPIT > Yapılandırma > Genel Bakış'ta belgelenmiştir:

![DBACockpit - Ön Göç](./media/dbms-guide-ha-ibm/hadr-sap-mgr-org.png)




### <a name="test-takeover-of-ibm-db2"></a>IBM Db2'nin test devralması


> [!IMPORTANT] 
> Teste başlamadan önce şunları yapın:
> * Kalp pilinin herhangi bir başarısız eylemi yoktur (crm durumu).
> * Konum kısıtlaması yok (geçiş testiartıkları)
> * IBM Db2 HADR senkronizasyonu çalışıyor. Kullanıcı db2\<sid> ile kontrol edin <pre><code>db2pd -hadr -db \<DBSID></code></pre>


Aşağıdaki komutu çalıştırarak birincil Db2 veritabanını çalıştıran düğümü geçirin:
<pre><code>crm resource migrate msl_<b>Db2_db2ptr_PTR</b> azibmdb02</code></pre>

Geçiş yapıldıktan sonra crm durum çıktısı gibi görünür:
<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:azure-lb):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Slaves: [ azibmdb01 ]
</code></pre>

SAP sistemindeki özgün durum, aşağıdaki resimde gösterildiği gibi İşlem DBACOCKPIT > Yapılandırma > Genel Bakış'ta belgelenmiştir:

![DBACockpit - Geçiş Sonrası](./media/dbms-guide-ha-ibm/hadr-sap-mgr-post.png)

"crm kaynak geçir" ile kaynak geçişi konum kısıtlamaları oluşturur. Konum kısıtlamaları silinmelidir. Konum kısıtlamaları silinmezse, kaynak geri alınamaz veya istenmeyen devralmalarla karşılaşabilirsiniz. 

Kaynağı *azibmdb01'e* geri geçirin ve konum kısıtlamalarını temizleyin
<pre><code>crm resource migrate msl_<b>Db2_db2ptr_PTR</b> azibmdb01
crm resource clear msl_<b>Db2_db2ptr_PTR</b>
</code></pre>

- **crm kaynak \<> \<> res_name geçirin:** Konum kısıtlamaları oluşturur ve devralma yla ilgili sorunlara neden olabilir
- **crm kaynak \<açık res_name>**: Konum kısıtlamalarını temizler
- **crm kaynak \<temizleme res_name>**: Kaynağın tüm hatalarını temizler

### <a name="test-the-fencing-agent"></a>Eskrim aracısını test edin

Bu durumda, SUSE Linux'u kullandığınızda yapmanızı tavsiye ettiğimiz SBD eskrimtest ediyoruz.

<pre><code>
azibmdb01:~ # ps -ef|grep sbd
root       2374      1  0 Feb05 ?        00:00:17 sbd: inquisitor
root       2378   2374  0 Feb05 ?        00:00:40 sbd: watcher: /dev/disk/by-id/scsi-36001405fbbaab35ee77412dacb77ae36 - slot: 0 - uuid: 27cad13a-0bce-4115-891f-43b22cfabe65
root       2379   2374  0 Feb05 ?        00:01:51 sbd: watcher: Pacemaker
root       2380   2374  0 Feb05 ?        00:00:18 sbd: watcher: Cluster

azibmdb01:~ # kill -9 2374
</code></pre>

Küme düğümü *azibmdb01* yeniden başlatılmalıdır. IBM Db2 birincil HADR rolü *azibmdb02*taşınacak. *Azibmdb01* tekrar çevrimiçi olduğunda, Db2 örneği ikincil bir veritabanı örneği rolünde hareket edecek. 

Pacemaker hizmeti yeniden başlatılan eski birincil otomatik olarak başlatılmıyorsa, el ile başlattığından emin olun:

<code><pre>sudo service pacemaker start</code></pre>

### <a name="test-a-manual-takeover"></a>Manuel devralmayı test edin

Pacemaker hizmetini *azibmdb01* düğümünde durdurarak manuel devralmayı test edebilirsiniz:
<pre><code>service pacemaker stop</code></pre>

*azibmdb02'de* durum
<pre><code>
2 nodes configured
5 resources configured

Online: [ azibmdb02 ]
OFFLINE: [ azibmdb01 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:azure-lb):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Stopped: [ azibmdb01 ]
</code></pre>

Başarısız olduktan sonra, *azibmdb01*tekrar hizmet başlayabilirsiniz.
<pre><code>service pacemaker start</code></pre>


### <a name="kill-the-db2-process-on-the-node-that-runs-the-hadr-primary-database"></a>HADR birincil veritabanını çalıştıran düğümdeki Db2 işlemini öldürme

<pre><code>#Kill main db2 process - db2sysc
azibmdb01:~ # ps -ef|grep db2s
db2ptr    34598  34596  8 14:21 ?        00:00:07 db2sysc 0

azibmdb01:~ # kill -9 34598
</code></pre>

Db2 örneği başarısız olacak ve Pacemaker aşağıdaki durumu rapor edecektir:

<pre><code>
2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Stopped
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:azure-lb):      Stopped
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Slaves: [ azibmdb02 ]
     Stopped: [ azibmdb01 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_demote_0 on azibmdb01 'unknown error' (1): call=157, status=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:28:19 2019', queued=40ms, exec=223ms

</code></pre>

Pacemaker aynı düğüm üzerinde Db2 birincil veritabanı örneğini yeniden başlatacak veya ikincil veritabanı örneğini çalıştıran düğüm üzerinde başarısız olur ve bir hata bildirilir.

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:azure-lb):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_demote_0 on azibmdb01 'unknown error' (1): call=157, status=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:28:19 2019', queued=40ms, exec=223ms
</code></pre>


### <a name="kill-the-db2-process-on-the-node-that-runs-the-secondary-database-instance"></a>İkincil veritabanı örneğini çalıştıran düğümdeki Db2 işlemini öldürme

<pre><code>azibmdb02:~ # ps -ef|grep db2s
db2ptr    65250  65248  0 Feb11 ?        00:09:27 db2sysc 0

azibmdb02:~ # kill -9</code></pre>

Düğüm başarısız belirtilen içine alır ve hata bildirilen
<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:azure-lb):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     rsc_Db2_db2ptr_PTR      (ocf::heartbeat:db2):   FAILED azibmdb02
     Masters: [ azibmdb01 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_monitor_30000 on azibmdb02 'not running' (7): call=144, status=complete, exitreason='',
last-rc-change='Tue Feb 12 14:36:59 2019', queued=0ms, exec=0ms</code></pre>

Db2 örneği, daha önce atadığı ikincil rolde yeniden başlatılır.

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:azure-lb):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_monitor_30000 on azibmdb02 'not running' (7): call=144, status=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:36:59 2019', queued=0ms, exec=0ms</code></pre>



### <a name="stop-db-via-db2stop-force-on-the-node-that-runs-the-hadr-primary-database-instance"></a>HADR birincil veritabanı örneğini çalıştıran düğümüzerinde db2stop kuvveti ile DB'yi durdurun

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:azure-lb):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]</code></pre>

Kullanıcı db2\<sid> komut db2stop kuvvet yürütmek gibi:
<pre><code>azibmdb01:~ # su - db2ptr
azibmdb01:db2ptr> db2stop force</code></pre>

Hata algılandı
<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Stopped
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:azure-lb):      Stopped
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     rsc_Db2_db2ptr_PTR      (ocf::heartbeat:db2):   FAILED azibmdb01
     Slaves: [ azibmdb02 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_demote_0 on azibmdb01 'unknown error' (1): call=201, status=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:45:25 2019', queued=1ms, exec=150ms</code></pre>

Db2 HADR ikincil veritabanı örneği birincil role terfi var
<pre><code> nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:azure-lb):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Stopped: [ azibmdb01 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_start_0 on azibmdb01 'unknown error' (1): call=205, stat
us=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:45:27 2019', queued=0ms, exec=865ms</pre></code>


### <a name="crash-vm-with-restart-on-the-node-that-runs-the-hadr-primary-database-instance"></a>HADR birincil veritabanı örneğini çalıştıran düğümde yeniden başlatma ile Kilitlenme VM

<pre><code>#Linux kernel panic - with OS restart
azibmdb01:~ # echo b > /proc/sysrq-trigger</code></pre>

Kalp pili ikincil örneği birincil örnek rolüne yükseltecektir. Eski birincil örnek VM sonra ikincil role taşınır ve tüm hizmetler tamamen VM yeniden başlatıldıktan sonra geri yüklenir:

<pre><code> nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:azure-lb):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]</code></pre>



### <a name="crash-the-vm-that-runs-the-hadr-primary-database-instance-with-halt"></a>HADR birincil veritabanı örneğini "durdurma" ile çalıştıran VM'yi kilitleyin

<pre><code>#Linux kernel panic - halts OS
azibmdb01:~ # echo b > /proc/sysrq-trigger</code></pre>

Böyle bir durumda, Pacemaker birincil veritabanı örneğini çalıştıran düğümün yanıt vermediğini algılar.

<pre><code>2 nodes configured
5 resources configured

Node azibmdb01: UNCLEAN (online)
Online: [ azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:azure-lb):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]</code></pre>

Bir sonraki adım bölünmüş *beyin* durumu kontrol etmektir. Kalan düğüm, birincil veritabanı örneğini en son çalıştıran düğümün aşağı olduğunu belirledikten sonra, kaynakların bir başarısızlığı gerçekleştirilir.
<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb02 ]
OFFLINE: [ azibmdb01 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:azure-lb):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Stopped: [ azibmdb01 ] </code></pre>


Düğümün "durdurulması" durumunda, başarısız düğüm Azure Yönetimi araçları (Azure portalı, PowerShell veya Azure CLI'de) aracılığıyla yeniden başlatılmalıdır. Başarısız düğüm yeniden çevrimiçi olduktan sonra, db2 örneğini ikincil role başlar.

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:azure-lb):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Slaves: [ azibmdb01 ]</code></pre>

## <a name="next-steps"></a>Sonraki adımlar
- [SAP NetWeaver için yüksek kullanılabilirlik mimarisi ve senaryoları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios)
- [Azure'da SUSE Linux Enterprise Server'da Pacemaker'ı ayarlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)

