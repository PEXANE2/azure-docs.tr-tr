---
title: IBM Db2 HADR'ı RHEL'de Azure sanal makinelerinde (VM) ayarlama | Microsoft Dokümanlar
description: Azure sanal makinelerde (VM) RHEL'de IBM Db2 LUW'un yüksek kullanılabilirliğini belirleyin.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: SAP
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/13/2020
ms.author: juergent
ms.openlocfilehash: c6a230f6abeab45c56aab2db40b8b1defcc06d90
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77598706"
---
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[1612105]:https://launchpad.support.sap.com/#/notes/1612105
[2694118]:https://launchpad.support.sap.com/#/notes/2694118


[db2-hadr-11.1]:https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html
[db2-hadr-10.5]:https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html
[dbms-db2]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm

[sap-instfind]:https://help.sap.com/viewer/9e41ead9f54e44c1ae1a1094b0f80712/ALL/en-US/576f5c1808de4d1abecbd6e503c9ba42.html
[rhel-ha-addon]:https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index
[rhel-ha-admin]:https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index
[rhel-ha-ref]:https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index
[rhel-azr-supp]:https://access.redhat.com/articles/3131341
[rhel-azr-inst]:https://access.redhat.com/articles/3252491
[rhel-db2-supp]:https://access.redhat.com/articles/3144221
[ascs-ha-rhel]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel
[glusterfs]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-glusterfs
[rhel-pcs-azr]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker
[anf-rhel]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md
[azr-sap-plancheck]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist



# <a name="high-availability-of-ibm-db2-luw-on-azure-vms-on-red-hat-enterprise-linux-server"></a>Red Hat Enterprise Linux Server üzerinde Azure VM’lerindeki IBM Db2 LUW’a yönelik yüksek kullanılabilirlik

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
| [2002167] | Red Hat Enterprise Linux 7.x: Kurulum ve Yükseltme |
| [2694118] | Azure'da Red Hat Enterprise Linux HA Eklentisi |
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
| [Red Hat Enterprise Linux 7 için Yüksek Kullanılabilirlik Eklentisi'ne Genel Bakış][rhel-ha-addon] |
| [Yüksek Kullanılabilirlik Eklenti Yönetimi][rhel-ha-admin] |
| [Yüksek Kullanılabilirlik Eklenti Başvurusu][rhel-ha-ref] |
| [RHEL Yüksek Kullanılabilirlik Kümeleri için Destek İlkeleri - Küme Üyesi Olarak Microsoft Azure Sanal Makineleri][rhel-azr-supp]
| [Microsoft Azure'da Red Hat Enterprise Linux 7.4 (ve sonrası) Yüksek Kullanılabilirlik Kümesi'ni yükleme ve yapılandırma][rhel-azr-inst]
| [SAP iş yükü için IBM Db2 Azure Sanal Makineler DBMS dağıtımı][dbms-db2] |
| [IBM Db2 HADR 11.1][db2-hadr-11.1] |
| [IBM Db2 HADR 10.5][db2-hadr-10.5] |
| [RHEL Yüksek Kullanılabilirlik Kümeleri için Destek Politikası - Linux, Unix ve Windows için IBM Db2'nin Kümede Yönetimi][rhel-db2-supp]



## <a name="overview"></a>Genel Bakış
Yüksek kullanılabilirlik elde etmek için, HADR'li IBM Db2 LUW, Azure [kullanılabilirlik kümesinde](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) veya [Azure Kullanılabilirlik Bölgeleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ha-availability-zones)arasında dağıtılan en az iki Azure sanal makinesine yüklenir. 

Aşağıdaki grafikler, iki veritabanı sunucusu Azure VM'nin kurulumlarını görüntüler. Her iki veritabanı sunucusu Azure VM'lerin de kendi depolama alanı vardır ve çalışır durumdadır. HADR'de, Azure VM'lerinden birinde bulunan bir veritabanı örneği birincil örnek rolüne sahiptir. Tüm istemciler birincil örneğe bağlıdır. Veritabanı hareketlerinde yapılan tüm değişiklikler Db2 hareket günlüğünde yerel olarak devam etti. Hareket günlüğü kayıtları yerel olarak kalıcı olduğundan, kayıtlar TCP/IP üzerinden ikinci veritabanı sunucusundaki veritabanı örneğine, bekleme sunucusuna veya bekleme örneğine aktarılır. Bekleme örneği, aktarılan hareket günlüğü kayıtlarını ileterek yerel veritabanını güncelleştirir. Bu şekilde, bekleme sunucusu birincil sunucu yla eşit tutulur.

HADR yalnızca bir çoğaltma işlevidir. Hiçbir arıza algılama ve hiçbir otomatik devralma veya arıza tesisleri vardır. Bekleme sunucusuna devralma veya aktarma, bir veritabanı yöneticisi tarafından el ile başlatılmalıdır. Otomatik devralma ve hata algılaması elde etmek için Linux Pacemaker kümeleme özelliğini kullanabilirsiniz. Pacemaker iki veritabanı sunucusu örneklerini izler. Birincil veritabanı sunucusu örneği çöktüğinde, Pacemaker bekleme sunucusu tarafından *otomatik* bir HADR devralma başlatır. Pacemaker ayrıca sanal IP adresinin yeni birincil sunucuya atanmasını sağlar.

![IBM Db2 yüksek kullanılabilirlik genel bakış](./media/high-availability-guide-rhel-ibm-db2-luw/ha-db2-hadr-lb-rhel.png)

SAP uygulama sunucuların birincil veritabanına bağlanması için sanal bir ana bilgisayar adı ve sanal IP adresi gerekir. Bir hata durumunda, SAP uygulama sunucuları yeni birincil veritabanı örneğine bağlanır. Azure ortamında, bir [Azure yük dengeleyicisinin,](https://microsoft.sharepoint.com/teams/WAG/AzureNetworking/Wiki/Load%20Balancing.aspx) IBM Db2'nin HADR'ı için gerekli olan şekilde sanal bir IP adresi kullanması gerekir. 

HADR ve Pacemaker ile IBM Db2 LUW'un son derece kullanılabilir bir SAP sistem kurulumuna nasıl uyduğunu tam olarak anlamanıza yardımcı olmak için, aşağıdaki resim IBM Db2 veritabanına dayalı bir SAP sisteminin son derece kullanılabilir kurulumuna genel bir bakış sunar. Bu makale yalnızca IBM Db2'yi kapsar, ancak sap sisteminin diğer bileşenlerinin nasıl ayarlanıla ilgili diğer makalelere başvurular sağlar.

![IBM DB2 yüksek kullanılabilirlik tam ortama genel bakış](./media/high-availability-guide-rhel-ibm-db2-luw/end-2-end-ha-rhel.png)


### <a name="high-level-overview-of-the-required-steps"></a>Gerekli adımlara üst düzey genel bakış
IBM Db2 yapılandırmasını dağıtmak için aşağıdaki adımları izlemeniz gerekir:

  + Çevrenizi planlayın.
  + VM'leri dağıtın.
  + RHEL Linux'u güncelleyin ve dosya sistemlerini yapılandırın.
  + Pacemaker'ı yükleyin ve yapılandırın.
  + [Kurulum glusterfs kümesi][glusterfs] veya Azure [NetApp Dosyaları][anf-rhel]
  + [ASCS/ERS'i ayrı bir kümeye][ascs-ha-rhel]yükleyin.
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
| Azure eskrim | Beyin durumlarını önlemek için yöntem önlenir. |
| Azure Load Balancer | Temel veya Standart kullanımı (önerilir), Db2 veritabanı için prob bağlantı noktası (tavsiyemiz 62500) **prob-port.** |
| Ad çözümlemesi| Ad çözümlemesi ortamda nasıl çalışır? DNS hizmeti şiddetle tavsiye edilir. Yerel ana bilgisayarlar dosyası kullanılabilir. |
    
Azure'da Linux Pacemaker hakkında daha fazla bilgi için [Azure'da Red Hat Enterprise Linux'ta Pacemaker'ı ayarlama 'ya][rhel-pcs-azr]bakın.

## <a name="deployment-on-red-hat-enterprise-linux"></a>Red Hat Enterprise Linux'ta Dağıtım

IBM Db2 LUW'un kaynak aracısı Red Hat Enterprise Linux Server HA Addon'a dahildir. Bu belgede açıklanan kurulum için SAP için Red Hat Enterprise Linux kullanmanız gerekir. Azure Marketi, Red Hat Enterprise Linux 7.4 için SAP veya daha yüksek bir resim içerir ve yeni Azure sanal makinelerini dağıtmak için kullanabilirsiniz. Azure VM Marketplace'te bir VM görüntüsü seçtiğinizde Azure Marketi aracılığıyla Red Hat tarafından sunulan çeşitli destek veya hizmet modellerine dikkat edin.

### <a name="hosts-dns-updates"></a>Ana bilgisayarlar: DNS güncelleştirmeleri
Sanal ana bilgisayar adları da dahil olmak üzere tüm ana bilgisayar adlarının bir listesini yapın ve uygun IP adresini ana bilgisayar adı çözümlemesi için etkinleştirmek için DNS sunucularınızı güncelleştirin. Bir DNS sunucusu yoksa veya DNS girişlerini güncelleştirip oluşturamıyorsanız, bu senaryoya katılan tek tek VM'lerin yerel ana bilgisayar dosyalarını kullanmanız gerekir. Ana bilgisayar dosyaları girişlerini kullanıyorsanız, girişlerin SAP sistem ortamındaki tüm VM'lere uygulandığından emin olun. Ancak, ideal olarak Azure'a yayılan DNS'nizi kullanmanızı öneririz


### <a name="manual-deployment"></a>El ile dağıtım

Ibm Db2 LUW için seçilen işletim sistemi IBM/SAP tarafından desteklendirildikten emin olun. Azure VM'ler ve Db2 sürümleri için desteklenen işletim sistemi sürümlerinin listesi SAP note [1928533'te]mevcuttur. Bireysel Db2 sürümüne göre işletim sistemi sürümlerinin listesi SAP Ürün Kullanılabilirlik Matrisi'nde mevcuttur. Bu veya daha sonraki Red Hat Enterprise Linux sürümlerindeki Azure ile ilgili performans iyileştirmeleri nedeniyle SAP için en az Red Hat Enterprise Linux 7.4 öneririz.

1. Bir kaynak grubu oluşturun veya seçin.
1. Sanal ağ ve alt ağ oluşturun veya seçin.
1. Bir Azure kullanılabilirlik kümesi oluşturun veya bir kullanılabilirlik bölgesi dağıtın.
    + Kullanılabilirlik kümesi için, maksimum güncelleştirme etki alanlarını 2 olarak ayarlayın.
1. Sanal Makine 1 oluşturun.
    + Azure Marketi'nde SAP görüntüsü için Red Hat Enterprise Linux'u kullanın.
    + Adım 3'te oluşturduğunuz Azure kullanılabilirlik kümesini veya Kullanılabilirlik Bölgesi'ni seçin.
1.  Sanal Makine 2 oluşturun.
    + Azure Marketi'nde SAP görüntüsü için Red Hat Enterprise Linux'u kullanın.
    + Adım 3'te oluşturulan Azure kullanılabilirlik kümesini seçin veya Kullanılabilirlik Bölgesi'ni seçin (adım 3'tekiyle aynı bölge değil).
1. VM'lere veri diskleri ekleyin ve ardından SAP iş yükü [için IBM Db2 Azure Sanal Makineler DBMS dağıtımı][dbms-db2]makalesinde dosya sistemi kurulumu önerisini kontrol edin.

## <a name="create-the-pacemaker-cluster"></a>Kalp Pili kümesini oluşturma
    
Bu IBM Db2 sunucusu için temel bir Kalp Pili kümesi oluşturmak için [Azure'da Red Hat Enterprise Linux'ta Pacemaker'ı ayarlama'ya][rhel-pcs-azr]bakın. 

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

#### <a name="red-hat-firewall-rules"></a>Red Hat güvenlik duvarı kuralları
Red Hat Enterprise Linux varsayılan olarak etkin güvenlik duvarı vardır. 

<pre><code>#Allow access to SWPM tool. Rule is not permanent.
sudo firewall-cmd --add-port=4237/tcp</code></pre>

### <a name="installation-hints-for-setting-up-ibm-db2-luw-with-hadr"></a>HADR ile IBM Db2 LUW kurulumu için kurulum ipuçları

Birincil IBM Db2 LUW veritabanı örneğini ayarlamak için:

- Yüksek kullanılabilirlik veya dağıtılmış seçeneği kullanın.
- SAP ASCS/ERS ve Veritabanı örneğini yükleyin.
- Yeni yüklenen veritabanının yedeğini alın.

> [!IMPORTANT] 
> Yükleme sırasında ayarlanan "Veritabanı İletişimi bağlantı noktasını" yazın. Her iki veritabanı örneği için aynı bağlantı noktası numarası olmalıdır.
>![SAP SWPM Bağlantı Noktası Tanımı](./media/high-availability-guide-rhel-ibm-db2-luw/hadr-swpm-db2-port.png)

### <a name="ibm-db2-hadr-settings-for-azure"></a>Azure için IBM Db2 HADR ayarları

   Bir Azure Pacemaker eskrim aracısı kullandığınızda, aşağıdaki parametreleri ayarlayın:

   - HADR eş pencere süresi (saniye) (HADR_PEER_WINDOW) = 240  
   - HADR zaman çıkış değeri (HADR_TIMEOUT) = 45

İlk arıza/devralma testine dayalı olarak önceki parametreleri öneririz. Bu parametre ayarlarıyla başarısız ve devralmanın doğru işlevselliğini test etmek zorunludur. Tek tek yapılandırmalar değişebildiği için, parametreler ayarlama gerektirebilir. 

> [!NOTE]
> Normal başlangıç ile HADR yapılandırması ile IBM Db2'ye özgü: Birincil veritabanı örneğini başlatmadan önce ikincil veya bekleme veritabanı örneği çalışır durumda olmalıdır.

   
> [!NOTE]
> Azure ve Kalp Pili'ne özel kurulum ve yapılandırma için: SAP Yazılım Sağlama Yöneticisi aracılığıyla yükleme işlemi sırasında IBM Db2 LUW için yüksek kullanılabilirlik hakkında açık bir soru vardır:
>+ **IBM Db2 pureScale'i**seçmeyin.
>+ **Çoklu platformlar için IBM Tivoli Sistem Otomasyonu Yükle'yi seçmeyin.**
>+ **Küme yapılandırma dosyalarını oluştur'u**seçmeyin.
>![SAP SWPM - DB2 HA seçenekleri](./media/high-availability-guide-rhel-ibm-db2-luw/swpm-db2ha-opt.png)


SAP homojen sistem kopyalama yordamını kullanarak Standby veritabanı sunucusunu kurmak için aşağıdaki adımları uygulayın:

1. **Hedef sistemleri** > **Dağıtılmış** > **Veritabanı örneği**> Sistem **kopyalama** seçeneğini seçin.
1. Kopyalama yöntemi olarak, yedek sunucu örneğinde yedeklemeyi geri yüklemek için yedeklemeyi kullanabilmeniz için **Homojen Sistem'i** seçin.
1. Homojen sistem kopyası için veritabanını geri yüklemek için çıkış adımına ulaştığınızda, yükleyiciden çıkın. Birincil ana bilgisayar yedeklemesinden veritabanını geri yükleyin. Sonraki tüm yükleme aşamaları zaten birincil veritabanı sunucusunda yürütülmüştür.

#### <a name="red-hat-firewall-rules-for-db2-hadr"></a>DB2 HADR için Red Hat güvenlik duvarı kuralları
DB2'ye ve HADR'ın çalışması için DB2 arasında trafik sağlamak için güvenlik duvarı kuralları ekleyin:
+ Veritabanı iletişim bağlantı noktası. Bölümleri kullanıyorsanız, bu bağlantı noktalarını da ekleyin.
+ HADR bağlantı noktası (DB2 parametre sinin değeri HADR_LOCAL_SVC)
+ Azure sonda bağlantı noktası
<pre><code>sudo firewall-cmd --add-port=&lt;port&gt;/tcp --permanent
sudo firewall-cmd --reload</code></pre>

#### <a name="ibm-db2-hadr-check"></a>IBM Db2 HADR denetimi
Gösteri amaçları ve bu makalede açıklanan yordamlar için, veritabanı SID **ID2**olduğunu.

HADR'ı yapılandırdıktan ve durum BIRINCIL ve bekleme düğümlerinde PEER ve CONNECTED olduktan sonra aşağıdaki denetimi yapın:

<pre><code>
Execute command as db2&lt;sid&gt; db2pd -hadr -db &lt;SID&gt;

#Primary output:
Database Member 0 -- Database ID2 -- Active -- Up 1 days 15:45:23 -- Date 2019-06-25-10.55.25.349375

                            <b>HADR_ROLE = PRIMARY
                          REPLAY_TYPE = PHYSICAL
                        HADR_SYNCMODE = NEARSYNC
                           STANDBY_ID = 1
                        LOG_STREAM_ID = 0
                           HADR_STATE = PEER
                           HADR_FLAGS =
                  PRIMARY_MEMBER_HOST = az-idb01
                     PRIMARY_INSTANCE = db2id2
                       PRIMARY_MEMBER = 0
                  STANDBY_MEMBER_HOST = az-idb02
                     STANDBY_INSTANCE = db2id2
                       STANDBY_MEMBER = 0
                  HADR_CONNECT_STATUS = CONNECTED</b>
             HADR_CONNECT_STATUS_TIME = 06/25/2019 10:55:05.076494 (1561460105)
          HEARTBEAT_INTERVAL(seconds) = 7
                     HEARTBEAT_MISSED = 5
                   HEARTBEAT_EXPECTED = 52
                HADR_TIMEOUT(seconds) = 30
        TIME_SINCE_LAST_RECV(seconds) = 5
             PEER_WAIT_LIMIT(seconds) = 0
           LOG_HADR_WAIT_CUR(seconds) = 0.000
    LOG_HADR_WAIT_RECENT_AVG(seconds) = 598.000027
   LOG_HADR_WAIT_ACCUMULATED(seconds) = 598.000
                  LOG_HADR_WAIT_COUNT = 1
SOCK_SEND_BUF_REQUESTED,ACTUAL(bytes) = 0, 46080
SOCK_RECV_BUF_REQUESTED,ACTUAL(bytes) = 0, 369280
            PRIMARY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
            STANDBY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
                  HADR_LOG_GAP(bytes) = 132242668
     STANDBY_REPLAY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
       STANDBY_RECV_REPLAY_GAP(bytes) = 0
                     PRIMARY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
                     STANDBY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
              STANDBY_REPLAY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
         STANDBY_RECV_BUF_SIZE(pages) = 2048
             STANDBY_RECV_BUF_PERCENT = 0
           STANDBY_SPOOL_LIMIT(pages) = 1000
                STANDBY_SPOOL_PERCENT = 0
                   STANDBY_ERROR_TIME = NULL
                 PEER_WINDOW(seconds) = 300
                      PEER_WINDOW_END = 06/25/2019 11:12:03.000000 (1561461123)
             READS_ON_STANDBY_ENABLED = N



#Secondary output:
Database Member 0 -- Database ID2 -- Standby -- Up 1 days 15:45:18 -- Date 2019-06-25-10.56.19.820474

                            <b>HADR_ROLE = STANDBY
                          REPLAY_TYPE = PHYSICAL
                        HADR_SYNCMODE = NEARSYNC
                           STANDBY_ID = 0
                        LOG_STREAM_ID = 0
                           HADR_STATE = PEER
                           HADR_FLAGS =
                  PRIMARY_MEMBER_HOST = az-idb01
                     PRIMARY_INSTANCE = db2id2
                       PRIMARY_MEMBER = 0
                  STANDBY_MEMBER_HOST = az-idb02
                     STANDBY_INSTANCE = db2id2
                       STANDBY_MEMBER = 0
                  HADR_CONNECT_STATUS = CONNECTED</b>
             HADR_CONNECT_STATUS_TIME = 06/25/2019 10:55:05.078116 (1561460105)
          HEARTBEAT_INTERVAL(seconds) = 7
                     HEARTBEAT_MISSED = 0
                   HEARTBEAT_EXPECTED = 10
                HADR_TIMEOUT(seconds) = 30
        TIME_SINCE_LAST_RECV(seconds) = 1
             PEER_WAIT_LIMIT(seconds) = 0
           LOG_HADR_WAIT_CUR(seconds) = 0.000
    LOG_HADR_WAIT_RECENT_AVG(seconds) = 598.000027
   LOG_HADR_WAIT_ACCUMULATED(seconds) = 598.000
                  LOG_HADR_WAIT_COUNT = 1
SOCK_SEND_BUF_REQUESTED,ACTUAL(bytes) = 0, 46080
SOCK_RECV_BUF_REQUESTED,ACTUAL(bytes) = 0, 367360
            PRIMARY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
            STANDBY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
                  HADR_LOG_GAP(bytes) = 0
     STANDBY_REPLAY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
       STANDBY_RECV_REPLAY_GAP(bytes) = 0
                     PRIMARY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
                     STANDBY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
              STANDBY_REPLAY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
         STANDBY_RECV_BUF_SIZE(pages) = 2048
             STANDBY_RECV_BUF_PERCENT = 0
           STANDBY_SPOOL_LIMIT(pages) = 1000
                STANDBY_SPOOL_PERCENT = 0
                   STANDBY_ERROR_TIME = NULL
                 PEER_WINDOW(seconds) = 1000
                      PEER_WINDOW_END = 06/25/2019 11:12:59.000000 (1561461179)
             READS_ON_STANDBY_ENABLED = N

</code></pre>



## <a name="db2-pacemaker-configuration"></a>Db2 Kalp Pili yapılandırması

Bir düğüm hatası durumunda otomatik arıza için Pacemaker kullandığınızda, Db2 örneklerinizi ve Kalp Pilinizi buna göre yapılandırmanız gerekir. Bu bölümde bu tür bir yapılandırma açıklanmaktadır.

Aşağıdaki öğeler aşağıdakilerden biri ile önceden belirlenmiştir:

- **[A]**: Tüm düğümler için geçerlidir
- **[1]**: Yalnızca düğüm 1 için geçerlidir 
- **[2]**: Yalnızca düğüm 2 için geçerlidir

**[A]** Pacemaker yapılandırması için ön koşul:
1. Db2stop ile> kullanıcı\<db2 sid ile her iki veritabanı sunucularını kapatın.
1. db2\<sid> kullanıcısı için kabuk ortamını */bin/ksh*olarak değiştirin:
<pre><code># Install korn shell:
sudo yum install ksh
# Change users shell:
sudo usermod -s /bin/ksh db2&lt;sid&gt;</code></pre>
   

### <a name="pacemaker-configuration"></a>Kalp pili yapılandırması

**[1]** IBM Db2 HADR'a özel Pacemaker yapılandırması:
<pre><code># Put Pacemaker into maintenance mode
sudo pcs property set maintenance-mode=true 
</code></pre>

**[1]** IBM Db2 kaynakları oluşturun:
<pre><code># Replace <b>bold strings</b> with your instance name db2sid, database SID, and virtual IP address/Azure Load Balancer.
sudo pcs resource create Db2_HADR_<b>ID2</b> db2 instance='<b>db2id2</b>' dblist='<b>ID2</b>' master meta notify=true resource-stickiness=5000

#Configure resource stickiness and correct cluster notifications for master resoruce
sudo pcs resource update Db2_HADR_<b>ID2</b>-master meta notify=true resource-stickiness=5000

# Configure virtual IP - same as Azure Load Balancer IP
sudo pcs resource create vip_<b>db2id2</b>_<b>ID2</b> IPaddr2 ip='<b>10.100.0.40</b>'

# Configure probe port for Azure load Balancer
sudo pcs resource create nc_<b>db2id2</b>_<b>ID2</b> azure-lb port=<b>62500</b>

#Create a group for ip and Azure loadbalancer probe port
sudo pcs resource group add g_ipnc_<b>db2id2</b>_<b>ID2</b> vip_<b>db2id2</b>_<b>ID2</b> nc_<b>db2id2</b>_<b>ID2</b>

#Create colocation constrain - keep Db2 HADR Master and Group on same node
sudo pcs constraint colocation add g_ipnc_<b>db2id2</b>_<b>ID2</b> with master Db2_HADR_<b>ID2</b>-master

#Create start order constrain
sudo pcs constraint order promote Db2_HADR_<b>ID2</b>-master then g_ipnc_<b>db2id2</b>_<b>ID2</b>
</code></pre>

**[1]** IBM Db2 kaynaklarını başlatın:
* Pacemaker'ı bakım modundan çıkar.
<pre><code># Put Pacemaker out of maintenance-mode - that start IBM Db2
sudo pcs property set maintenance-mode=false</pre></code>

**[1]** Küme durumunun iyi olduğundan ve tüm kaynakların başlatıldıklarından emin olun. Kaynakların hangi düğümüzerinde çalıştığının önemli olmadığı önemli değildir.
<pre><code>sudo pcs status</code>
2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Kaynakların tam listesi:

 rsc_st_azure (stonith:fence_azure_arm): Az-idb01 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2] Ustalar: [ az-idb01 ] Köleler: [ az-idb02 ] Kaynak Grubu: g_ipnc_db2id2_ID2 vip_db2id2_ID2 (ocf:iPaddr2): Az-idb01 nc_db2id2_ID2 başladı (ocf:heartbeat:azure-lb): Az-idb01 başladı

Daemon Durum: corosync: aktif / devre dışı kalp pili: aktif / devre dışı pcsd: aktif / etkin
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

**[A]** Sonda bağlantı noktası için güvenlik duvarı kuralı ekleyin:
<pre><code>sudo firewall-cmd --add-port=<b><probe-port></b>/tcp --permanent
sudo firewall-cmd --reload</code></pre>

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
 
1. J2EE örneğinin birincil uygulama sunucusunda oturum açın ve çalıştırın:
    
    <pre><code>sudo /usr/sap/*SID*/*Instance*/j2ee/configtool/configtool.sh</code></pre>  
    
1. Sol çerçevede, **güvenlik deposu**seçin.
1. Sağ çerçevede, anahtarı `jdbc/pool/\<SAPSID>/url`seçin.
1. JDBC URL'deki ana bilgisayar adını sanal ana bilgisayar adı ile değiştirin.
    
    <pre><code>jdbc:db2://db-virt-hostname:5912/TSP:deferPrepares=0</code></pre>  
    
1. **Ekle'yi**seçin.
1. Değişikliklerinizi kaydetmek için sol üstteki disk simgesini seçin.
1. Yapılandırma aracını kapatın.
1. Java örneğini yeniden başlatın.

## <a name="configure-log-archiving-for-hadr-setup"></a>HADR kurulumu için günlük arşivleme yapılandırma
HADR kurulumu için Db2 günlük arşivlemesini yapılandırmak için, tüm günlük arşiv konumlarından otomatik günlük alma özelliğine sahip olmak üzere hem birincil hem de bekleme veritabanını yapılandırmanızı öneririz. Hem birincil hem de bekleme veritabanı, veritabanı örneklerinden birinin günlük dosyalarını arşivleyebileceği tüm günlük arşivi konumlarından günlük arşivi dosyalarını alabilmesi gerekir. 

Günlük arşivleme yalnızca birincil veritabanı tarafından gerçekleştirilir. Veritabanı sunucularının HADR rollerini değiştirirseniz veya bir hata oluşursa, günlük arşivlemeden yeni birincil veritabanı sorumludur. Birden çok günlük arşiv konumu ayarladıysanız, günlükleriniz iki kez arşivlenebilir. Yerel veya uzaktan yakalama durumunda, arşivlenmiş günlükleri eski birincil sunucudan yeni birincil sunucunun etkin günlük konumuna el ile kopyalamanız da gerekebilir.

Günlüklerin her iki düğümden yazıldığı ortak bir NFS paylaşımını veya GlusterFS'yi yapılandırmanızı öneririz. NFS payı veya GlusterFS son derece kullanılabilir olmalıdır. 

Mevcut yüksek kullanılabilirNN NFS hisselerini veya GlusterFS'yi taşımalar veya profil dizini için kullanabilirsiniz. Daha fazla bilgi için bkz.

- [SAP NetWeaver için Red Hat Enterprise Linux üzerinde Azure Sanal Makineler'de GlusterFS][glusterfs] 
- [SAP Uygulamaları için Azure NetApp Dosyaları ile Red Hat Enterprise Linux'ta Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik][anf-rhel]
- [Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) (NFS paylaşımları oluşturmak için)

## <a name="test-the-cluster-setup"></a>Küme kurulumını test edin

Bu bölümde Db2 HADR kurulumunuzu nasıl test edebilirsiniz. Her test IBM Db2 birincil *az-idb01* sanal makine üzerinde çalışıyor varsayar. Sudo ayrıcalıkları veya kökü olan (önerilmeyen) kullanıcı kullanılmalıdır.

Tüm test çalışmalarının başlangıç durumu burada açıklanmıştır: (crm_mon -r veya pcs durumu)

- **pcs durumu** yürütme sırasında Pacemaker durumunun bir anlık görüntüsüdür 
- **crm_mon -r,** Kalp Pili durumunun sürekli çıktısi

<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb01
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb01 ]
     Slaves: [ az-idb02 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb01
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb01

Daemon Status:
  corosync: active/disabled
  pacemaker: active/disabled
  pcsd: active/enabled
</code></pre>

SAP sistemindeki özgün durum, aşağıdaki resimde gösterildiği gibi İşlem DBACOCKPIT > Yapılandırma > Genel Bakış'ta belgelenmiştir:

![DBACockpit - Ön Göç](./media/high-availability-guide-rhel-ibm-db2-luw/hadr-sap-mgr-org-rhel.png)




### <a name="test-takeover-of-ibm-db2"></a>IBM Db2'nin test devralması


> [!IMPORTANT] 
> Teste başlamadan önce şunları yapın:
> * Kalp pili herhangi bir başarısız eylemleri (pcs durumu) yok.
> * Konum kısıtlaması yok (geçiş testiartıkları)
> * IBM Db2 HADR senkronizasyonu çalışıyor. Kullanıcı db2\<sid> ile kontrol edin <pre><code>db2pd -hadr -db \<DBSID></code></pre>


Aşağıdaki komutu çalıştırarak birincil Db2 veritabanını çalıştıran düğümü geçirin:
<pre><code>sudo pcs resource move Db2_HADR_<b>ID2</b>-master</code></pre>

Geçiş yapıldıktan sonra crm durum çıktısı gibi görünür:
<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb01
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Stopped: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02

</code></pre>

SAP sistemindeki özgün durum, aşağıdaki resimde gösterildiği gibi İşlem DBACOCKPIT > Yapılandırma > Genel Bakış'ta belgelenmiştir:

![DBACockpit - Geçiş Sonrası](./media/high-availability-guide-rhel-ibm-db2-luw/hadr-sap-mgr-post-rhel.png)

"Bilgisayarlar kaynak taşıma" ile kaynak geçişi konum kısıtlamaları oluşturur. Bu durumda konum kısıtlamaları az-idb01 IBM Db2 örneğinin çalıştırAn engelliyor. Konum kısıtlamaları silinmezse, kaynak geri alınamaz.

Konum kısıtlamasını kaldırın ve bekleme düğümü az-idb01'de başlatılacaktır.
<pre><code>sudo pcs resource clear Db2_HADR_<b>ID2</b>-master</code></pre>
Ve küme durumu şu şekilde değişir:
<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb01
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Slaves: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02</code></pre>

![DBACockpit - Kaldırılan konum kısıtlaması](./media/high-availability-guide-rhel-ibm-db2-luw/hadr-sap-mgr-clear-rhel.png)


Kaynağı *az-idb01'e* geri geçirin ve konum kısıtlamalarını temizleyin
<pre><code>sudo pcs resource move Db2_HADR_<b>ID2</b>-master az-idb01
sudo pcs resource clear Db2_HADR_<b>ID2</b>-master
</code></pre>

- **pcs kaynak \<taşımak <host>res_name> :** Konum kısıtlamaları oluşturur ve devralma yla ilgili sorunlara neden olabilir
- **pcs kaynak \<açık res_name>**: Konum kısıtlamaları temizler
- **pcs kaynak \<temizleme res_name>**: Kaynağın tüm hatalarını temizler

### <a name="test-a-manual-takeover"></a>Manuel devralmayı test edin

*Az-idb01* düğümünde Pacemaker hizmetini durdurarak manuel devralmayı test edebilirsiniz:
<pre><code>systemctl stop pacemaker</code></pre>

*az-ibdb02'de* durum
<pre><code>2 nodes configured
5 resources configured

Node az-idb01: pending
Online: [ az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Stopped: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02

Daemon Status:
  corosync: active/disabled
  pacemaker: active/disabled
  pcsd: active/enabled</code></pre>

Başarısız olduktan sonra, *az-idb01*tekrar hizmet başlayabilirsiniz.
<pre><code>systemctl start  pacemaker</code></pre>


### <a name="kill-the-db2-process-on-the-node-that-runs-the-hadr-primary-database"></a>HADR birincil veritabanını çalıştıran düğümdeki Db2 işlemini öldürme

<pre><code>#Kill main db2 process - db2sysc
[sapadmin@az-idb02 ~]$ sudo ps -ef|grep db2sysc
db2ptr    34598  34596  8 14:21 ?        00:00:07 db2sysc 0
[sapadmin@az-idb02 ~]$ sudo kill -9 34598
</code></pre>

Db2 örneği başarısız olacak ve Pacemaker ana düğümü hareket ettirecek ve aşağıdaki durumu rapor:

<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Stopped: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02

Failed Actions:
* Db2_HADR_ID2_demote_0 on az-idb01 'unknown error' (1): call=49, status=complete, exitreason='none',
    last-rc-change='Wed Jun 26 09:57:35 2019', queued=0ms, exec=362ms</code></pre>

Pacemaker aynı düğüm üzerinde Db2 birincil veritabanı örneğini yeniden başlatacak veya ikincil veritabanı örneğini çalıştıran düğüm üzerinde başarısız olur ve bir hata bildirilir.

### <a name="kill-the-db2-process-on-the-node-that-runs-the-secondary-database-instance"></a>İkincil veritabanı örneğini çalıştıran düğümdeki Db2 işlemini öldürme

<pre><code>[sapadmin@az-idb02 ~]$ sudo ps -ef|grep db2sysc
db2id2    23144  23142  2 09:53 ?        00:00:13 db2sysc 0
[sapadmin@az-idb02 ~]$ sudo kill -9 23144
</code></pre>

Düğüm başarısız belirtilen içine alır ve hata bildirilen
<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb01 ]
     Slaves: [ az-idb02 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb01
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb01

Failed Actions:
* Db2_HADR_ID2_monitor_20000 on az-idb02 'not running' (7): call=144, status=complete, exitreason='none',
    last-rc-change='Wed Jun 26 10:02:09 2019', queued=0ms, exec=0ms</code></pre>

Db2 örneği, daha önce atadığı ikincil rolde yeniden başlatılır.

### <a name="stop-db-via-db2stop-force-on-the-node-that-runs-the-hadr-primary-database-instance"></a>HADR birincil veritabanı örneğini çalıştıran düğümüzerinde db2stop kuvveti ile DB'yi durdurun

Kullanıcı db2\<sid> komut db2stop kuvvet yürütmek gibi:
<pre><code>az-idb01:db2ptr> db2stop force</code></pre>

Hata algılandı:

<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Slaves: [ az-idb02 ]
     Stopped: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Stopped
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Stopped

Failed Actions:
* Db2_HADR_ID2_demote_0 on az-idb01 'unknown error' (1): call=110, status=complete, exitreason='none',
    last-rc-change='Wed Jun 26 14:03:12 2019', queued=0ms, exec=355ms</code></pre>

Db2 HADR ikincil veritabanı örneği birincil role terfi var.
<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Slaves: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02

Failed Actions:
* Db2_HADR_ID2_demote_0 on az-idb01 'unknown error' (1): call=110, status=complete, exitreason='none',
    last-rc-change='Wed Jun 26 14:03:12 2019', queued=0ms, exec=355ms</pre></code>


### <a name="crash-the-vm-that-runs-the-hadr-primary-database-instance-with-halt"></a>HADR birincil veritabanı örneğini "durdurma" ile çalıştıran VM'yi kilitleyin

<pre><code>#Linux kernel panic. 
sudo echo b > /proc/sysrq-trigger</code></pre>

Böyle bir durumda, Pacemaker birincil veritabanı örneğini çalıştıran düğümün yanıt vermediğini algılar.

<pre><code>2 nodes configured
5 resources configured

Node az-idb01: UNCLEAN (online)
Online: [ az-idb02 ]

Full list of resources:

rsc_st_azure    (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb01 ]
     Slaves: [ az-idb02 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb01
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb01</code></pre>

Bir sonraki adım bölünmüş *beyin* durumu kontrol etmektir. Kalan düğüm, birincil veritabanı örneğini en son çalıştıran düğümün aşağı olduğunu belirledikten sonra, kaynakların bir başarısızlığı gerçekleştirilir.

<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb02 ]
OFFLINE: [ az-idb01 ]

Full list of resources:

rsc_st_azure    (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Stopped: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02 </code></pre>


Çekirdek paniği durumunda, başarısız düğüm eskrim ajanı tarafından yeniden yıldızlanır. Başarısız düğüm tekrar çevrimiçi olduktan sonra, kalp pili kümesini
<pre><code>sudo pcs cluster start</code></pre> db2 örneğini ikincil role başlatır.

<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

rsc_st_azure    (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Slaves: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02</code></pre>

## <a name="next-steps"></a>Sonraki adımlar
- [SAP NetWeaver için yüksek kullanılabilirlik mimarisi ve senaryoları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios)
- [Azure'da Red Hat Enterprise Linux'ta Pacemaker'ı ayarlama][rhel-pcs-azr]
