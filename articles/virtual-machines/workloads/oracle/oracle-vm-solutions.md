---
title: Azure sanal makinelerde Oracle çözümleri | Microsoft Dokümanlar
description: Desteklenen yapılandırmalar ve Microsoft Azure'daki Oracle sanal makine görüntülerinin sınırlamaları hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: BorisB2015
manager: gwallace
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/23/2019
ms.author: borisb
ms.openlocfilehash: 0cee7c25960d567c75a14d8ad9ef95b3e7221862
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81683427"
---
# <a name="oracle-vm-images-and-their-deployment-on-microsoft-azure"></a>Oracle VM görüntüleri ve bunların Microsoft Azure'da dağıtımı

Bu makale, Oracle tarafından Azure Marketi'nde yayınlanan sanal makine resimlerine dayalı Oracle çözümleri hakkındaki bilgileri kapsar. Oracle Cloud Infrastructure ile çapraz bulut uygulama çözümleriyle ilgileniyorsanız, [Microsoft Azure ve Oracle Cloud Infrastructure'ı entegre eden Oracle uygulama çözümlerine](oracle-oci-overview.md)bakın.

Şu anda kullanılabilen görüntülerin listesini almak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az vm image list --publisher oracle -o table --all
```

Mayıs 2019 itibariyle aşağıdaki resimler mevcuttur:

```bash
Offer                   Publisher    Sku                     Urn                                                          Version
----------------------  -----------  ----------------------  -----------------------------------------------------------  -------------
Oracle-Database-Ee      Oracle       12.1.0.2                Oracle:Oracle-Database-Ee:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Ee      Oracle       12.2.0.1                Oracle:Oracle-Database-Ee:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Ee      Oracle       18.3.0.0                Oracle:Oracle-Database-Ee:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Database-Se      Oracle       12.1.0.2                Oracle:Oracle-Database-Se:12.1.0.2:12.1.20170220             12.1.20170220
Oracle-Database-Se      Oracle       12.2.0.1                Oracle:Oracle-Database-Se:12.2.0.1:12.2.20180725             12.2.20180725
Oracle-Database-Se      Oracle       18.3.0.0                Oracle:Oracle-Database-Se:18.3.0.0:18.3.20181213             18.3.20181213
Oracle-Linux            Oracle       6.10                    Oracle:Oracle-Linux:6.10:6.10.20190506                       6.10.20190506
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.20190506                         6.8.20190506
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.20190506                         6.9.20190506
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.20190506                         7.3.20190506
Oracle-Linux            Oracle       7.4                     Oracle:Oracle-Linux:7.4:7.4.20190506                         7.4.20190506
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20181207                         7.5.20181207
Oracle-Linux            Oracle       7.5                     Oracle:Oracle-Linux:7.5:7.5.20190506                         7.5.20190506
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.20181207                         7.6.20181207
Oracle-Linux            Oracle       7.6                     Oracle:Oracle-Linux:7.6:7.6.20190506                         7.6.20190506
Oracle-WebLogic-Server  Oracle       Oracle-WebLogic-Server  Oracle:Oracle-WebLogic-Server:Oracle-WebLogic-Server:12.1.2  12.1.2
```

Bu görüntüler "Kendi Lisansınızı Getir" olarak kabul edilir ve bu nedenle yalnızca bir VM çalıştırılmasından kaynaklanan işlem, depolama ve ağ maliyetleri için ücretlendirilirsiniz.  Oracle yazılımlarını kullanmak için uygun lisansa sahip olduğunuz ve Oracle ile geçerli bir destek anlaşmanız olduğu varsayılır. Oracle, şirket içi azure'a lisans hareketliliğini garanti etmiştir. Lisans hareketliliği yle ilgili ayrıntılar için yayınlanan [Oracle ve Microsoft](https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html) notuna bakın. 

Bireyler ayrıca, çözümlerini Azure'da sıfırdan oluşturdukları özel bir görüntüye dayandırmayı veya kendi tesislerindeki ortamlarından özel bir resim yüklemeyi de seçebilirler.

## <a name="oracle-database-vm-images"></a>Oracle veritabanı VM görüntüleri
Oracle, Oracle Linux'u temel alan sanal makine görüntülerinde Oracle Database 12.1 ve daha yüksek Standart ve Kurumsal sürümlerini Azure'da çalıştırmayı destekler.  Azure'daki Oracle Veritabanı'nın üretim iş yüklerinde en iyi performansı elde etmek için, VM görüntüsünü düzgün boyutlandırın ve Premium SSD veya Ultra SSD Yönetilen Diskler'i kullandığınızdan emin olun. Oracle tarafından yayınlanan VM görüntüsünü kullanarak Azure'da oracle veritabanını hızlı bir şekilde nasıl çalışır hale getirebilirsiniz, [Oracle Database Quickstart walkthrough'u deneyin.](oracle-database-quick-create.md)

### <a name="attached-disk-configuration-options"></a>Ekli disk yapılandırma seçenekleri

Ekli diskler Azure Blob depolama hizmetine güvenir. Her standart disk, saniyede yaklaşık 500 giriş/çıkış işlemi (IOPS) teorik olarak maksimum kapasiteye sahiptir. Premium disk teklifimiz yüksek performanslı veritabanı iş yükleri için tercih edilir ve disk başına 5000 IOps'a kadar ulaşabilirsiniz. Performans gereksinimlerinizi karşılıyorsa tek bir disk kullanabilirsiniz. Ancak, birden çok bağlı disk kullanıyorsanız, veritabanı verilerini bunlara yayıyorsanız ve ardından Oracle Automatic Storage Management (ASM) kullanıyorsanız etkili IOPS performansını artırabilirsiniz. Daha fazla Oracle ASM özel bilgi için [Oracle Otomatik Depolama genel bakış](https://www.oracle.com/technetwork/database/index-100339.html) bakın. Oracle ASM'nin Linux Azure VM'ye nasıl yüklenir ve [Installing and Configuring Oracle Automated Storage Management](configure-oracle-asm.md) yapılandırılır, bkz.

### <a name="shared-storage-configuration-options"></a>Paylaşılan depolama yapılandırma seçenekleri

Azure NetApp Files, buluttaki veritabanları gibi yüksek performanslı iş yüklerini çalıştırmanın temel gereksinimlerini karşılayacak şekilde tasarlanmıştır ve şunları sağlar;
- Oracle iş yüklerini VM yerel NFS istemcisi veya Oracle dNFS üzerinden çalıştırmak için Azure yerel paylaşılan NFS depolama hizmeti
- IOPS taleplerinin gerçek dünya yelpazesini yansıtan ölçeklenebilir performans katmanları
- Düşük gecikme gecikmesi
- Yüksek kullanılabilirlik, yüksek dayanıklılık ve ölçekte yönetilebilirlik, genellikle görev kritik kurumsal iş yükleri tarafından talep (SAP ve Oracle gibi)
- En agresif RTO ve RPO SLA'ları elde etmek için hızlı ve verimli yedekleme ve kurtarma

Azure NetApp Dosyaları, Azure veri merkezi ortamında çalışan tüm flash sistemler e-® Azure Yerel hizmeti olarak NetApp® ONTAP'ı temel aldığı için bu özellikler mümkündür. Sonuç, diğer Azure depolama seçenekleri gibi sağlanabilir ve tüketilebilen ideal bir veritabanı depolama teknolojisidir. Azure NetApp Files NFS ciltlerinin nasıl dağıtılanve erişilenhakkında daha fazla bilgi için [Azure NetApp Dosyaları belgelerine](https://docs.microsoft.com/azure/azure-netapp-files/) bakın. Azure [NetApp Dosyaları'nda](https://www.netapp.com/us/media/tr-4780.pdf) Oracle veritabanını işletmek için en iyi uygulama önerileri için Azure NetApp Dosyalarını Kullanarak Azure Dağıtım En İyi Uygulama Kılavuzu'nda Oracle'a bakın.


## <a name="licensing-oracle-database--software-on-azure"></a>Azure'da Oracle Veritabanı & yazılımı lisanslama
Microsoft Azure, Oracle Veritabanı'nı çalıştırmak için yetkili bir bulut ortamıdır. Oracle Core Factor tablosu, bulutta Oracle veritabanlarını lisanslarken geçerli değildir. Bunun yerine, Enterprise Edition veritabanları için etkinleştirilen Hyper-Threading Teknolojisine sahip VM'ler kullanırken, hiperiş parçacığı etkinse (ilke belgesinde belirtildiği gibi) iki vCPUs'u bir Oracle İşlemci lisansına eşdeğer olarak sayın. İlke ayrıntılarını [burada](http://www.oracle.com/us/corporate/pricing/cloud-licensing-070579.pdf)bulabilirsiniz.
Oracle veritabanları genellikle daha yüksek bellek ve IO gerektirir. Bu nedenle, [bellek optimize edilmiş VM'ler](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory) bu iş yükleri için önerilir. İş yüklerinizi daha da optimize etmek için, yüksek bellek, depolama ve G/Ç bant genişliği gerektiren ancak yüksek çekirdek sayısı gerektirmeyen Oracle Veritabanı iş yükleri için [Kısıtlı Çekirdek vCPUs'ları](https://docs.microsoft.com/azure/virtual-machines/linux/constrained-vcpu) önerilir.

Oracle yazılımlarını ve iş yüklerini şirket içinden Microsoft Azure'a geçirdiğinizde, Oracle Azure [SSS'de](https://www.oracle.com/cloud/technologies/oracle-azure-faq.html) belirtildiği gibi lisans mobilitesi sağlar


## <a name="oracle-real-application-cluster-oracle-rac"></a>Oracle Gerçek Uygulama Kümesi (Oracle RAC)
Oracle Real Application Cluster (Oracle RAC), şirket içi çok düğümlü küme yapılandırmasında tek bir düğümün arızasını azaltmak için tasarlanmıştır. Bu, hiper ölçekli genel bulut ortamlarına özgü olmayan iki şirket içi teknolojiye dayanır: ağ çok dökümlü ve paylaşılan disk. Veritabanı çözümünüz Azure'da Oracle RAC gerektiriyorsa, bu teknolojileri etkinleştirmek için üçüncü taraf yazılıma ihtiyacınız vardır. Oracle RAC hakkında daha fazla bilgi için [FlashGrid SkyCluster sayfasına](https://www.flashgrid.io/oracle-rac-in-azure/)bakın.

## <a name="high-availability-and-disaster-recovery-considerations"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma konuları
Azure'da Oracle veritabanlarını kullanırken, herhangi bir kapalı kalma süresini önlemek için yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümlerini uygulamaktan siz sorumlusunuz. 

Oracle Database Enterprise Edition için yüksek kullanılabilirlik ve olağanüstü durum kurtarma (Oracle RAC güvenmeden) Iki ayrı sanal makinelerde iki veritabanları ile [Veri Koruma, Active Data Guard](https://www.oracle.com/database/technologies/high-availability/dataguard.html)veya Oracle [GoldenGate](https://www.oracle.com/technetwork/middleware/goldengate)kullanarak Azure'da elde edilebilir. Her iki sanal makine de özel kalıcı IP adresi üzerinden birbirlerine erişebildiklerinden emin olmak için aynı [sanal ağda](https://azure.microsoft.com/documentation/services/virtual-network/) olmalıdır.  Ayrıca, sanal makineleri Azure'un ayrı hata etki adlarına yerleştirmesine ve etki alanlarını yükseltmesine olanak sağlamak için aynı kullanılabilirlik kümesine yerleştirmenizi öneririz. Coğrafi artıklığa sahip olmak istiyorsanız, iki veritabanlarını iki farklı bölge arasında çoğaltmak ve iki örneği VPN Ağ Geçidi ile bağlayacak şekilde ayarlayın.

[Azure'da Oracle Data Guard uygula](configure-oracle-dataguard.md) öğreticisi, Azure'daki temel kurulum yordamını size iletebilir.  

Oracle Data Guard ile, bir sanal makinede birincil veritabanı, başka bir sanal makinede ikincil (bekleme) veritabanı ve aralarında tek yönlü çoğaltma ile yüksek kullanılabilirlik sağlanabilir. Sonuç veritabanının kopyasına erişim okunur. Oracle GoldenGate ile, iki veritabanları arasında çift yönlü çoğaltma yapılandırabilirsiniz. Bu araçları kullanarak veritabanlarınız için yüksek kullanılabilirlik çözümlerini nasıl kurabilirsiniz öğrenmek için Oracle web sitesindeki [Active Data Guard](https://www.oracle.com/database/technologies/high-availability/dataguard.html) ve [GoldenGate](https://docs.oracle.com/goldengate/1212/gg-winux/index.html) belgelerine bakın. Veritabanının kopyasına okuma yazma erişimine ihtiyacınız varsa, [Oracle Active Data Guard'ı](https://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html)kullanabilirsiniz.

Oracle [GoldenGate'i Azure'da Uygulama](configure-oracle-golden-gate.md) öğreticisi, Azure'daki temel kurulum yordamını size iletebilir.

Azure'da bir HA ve DR çözümünüzün tasarlanmasına ek olarak, veritabanınızı geri yüklemek için bir yedekleme stratejiniz olmalıdır. Öğretici [Yedekleme ve bir Oracle Veritabanı kurtarmak](oracle-backup-recovery.md) tutarlı bir yedekleme kurmak için temel yordam ı size iletir.


## <a name="support-for-jd-edwards"></a>JD Edwards desteği
Oracle Destek notu [Doc ID 2178595.1](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=573435677515785&id=2178595.1&_afrWindowMode=0&_adf.ctrl-state=o852dw7d_4)göre, JD Edwards EnterpriseOne sürümleri 9.2 ve üzeri `Minimum Technical Requirements` kendi özel (MTR) karşılayan **herhangi bir genel bulut sunan** desteklenir.  İşletim sistemi ve yazılım uygulaması uyumluluğu için MTR belirtimlerini karşılayan özel görüntüler oluşturmanız gerekir. 


## <a name="oracle-weblogic-server-virtual-machine-images"></a>Oracle WebLogic Server sanal makine görüntüleri

* **Kümeleme yalnızca Enterprise Edition'da desteklenir.** WebLogic kümeleme'yi yalnızca Oracle WebLogic Server'ın Enterprise Edition'ını kullanırken kullanma lisansınız vardır. Oracle WebLogic Server Standard Edition ile kümeleme kullanmayın.
* **UDP multicast desteklenmez.** Azure, UDP tek döküm'ü destekler, ancak çok amaçlı veya yayını desteklemez. Oracle WebLogic Server, Azure UDP tek döküm özelliklerine güvenebilir. UDP unicast'e dayanan en iyi sonuçlar için, WebLogic küme boyutunun sabit tutulmasını veya en fazla 10 yönetilen sunucuyla tutulmasını öneririz.
* **Oracle WebLogic Server, ortak ve özel bağlantı noktalarının T3 erişimi için aynı olmasını bekler (örneğin, Enterprise JavaBeans kullanırken).** Bir hizmet katmanı (EJB) uygulamasının Iki veya daha fazla VM'den oluşan bir Oracle WebLogic Server kümesinde, *SLWLS*adlı sanal bir ağda çalıştığı çok katmanlı bir senaryo düşünün. İstemci katmanı, hizmet katmanında EJB'yi çağırmaya çalışan basit bir Java programı çalıştıran, aynı sanal ağda farklı bir alt ağda dır. Hizmet katmanını yüklemek gerektiğinden, Oracle WebLogic Server kümesindeki sanal makineler için ortak yük dengeli bir uç noktası oluşturulması gerekir. Belirttiğiniz özel bağlantı noktası ortak bağlantı noktasından farklıysa (örneğin, 7006:7008), aşağıdaki gibi bir hata oluşur:

       [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

       Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

   Bunun nedeni, herhangi bir uzaktan T3 erişimi için, Oracle WebLogic Server yük dengeleyici bağlantı noktası ve WebLogic yönetilen sunucu bağlantı noktası aynı olmasını bekliyor olmasıdır. Önceki durumda, istemci bağlantı noktası 7006 (yük dengeleyici bağlantı noktası) erişiyor ve yönetilen sunucu 7008 (özel bağlantı noktası) dinliyor. Bu kısıtlama sadece T3 erişimi için geçerlidir, HTTP için geçerli değildir.

   Bu sorunu önlemek için aşağıdaki geçici çözümlerden birini kullanın:

  * T3 erişimine adanmış yük dengeli uç noktaları için aynı özel ve ortak bağlantı noktası numaralarını kullanın.
  * Oracle WebLogic Server'ı başlatırken aşağıdaki JVM parametresini ekleyin:

    ```
    -Dweblogic.rjvm.enableprotocolswitch=true
    ```

İlgili bilgiler için bk. **madde 860340.1'** e <https://support.oracle.com>bakınız.

* **Dinamik kümeleme ve yük dengeleme sınırlamaları.** Oracle WebLogic Server'da dinamik bir küme kullanmak ve azure'da tek bir genel yük dengeli bitiş noktası aracılığıyla ortaya çıkarmak istediğinizi varsayalım. Bu, yönetilen sunucuların her biri için sabit bir bağlantı noktası numarası kullandığınız (bir aralıktan dinamik olarak atanmamış) ve yöneticinin izlediği makinelerden daha fazla yönetilen sunucu başlatmadığınız sürece yapılabilir. Diğer bir süre, sanal makine başına birden fazla yönetilen sunucu yoktur). Yapılandırmanız, sanal makinelerden daha fazla Oracle WebLogic Sunucusunun başlatılmasıyla sonuçlanırsa (diğer bir deyişle, birden çok Oracle WebLogic Server örneğinin aynı sanal makineyi paylaştığı yer), Oracle WebLogic Sunucularının bu örneklerinden birden fazlasının belirli bir bağlantı noktası numarasına bağlanması mümkün değildir. Sanal makinedeki diğerleri başarısız oluyor.

   Yönetici sunucuyu yönetilen sunucularına benzersiz bağlantı noktası numaralarını otomatik olarak atayacağı şekilde yapılandırırsanız, Azure bu yapılandırma için gerekli olduğu gibi tek bir ortak bağlantı noktasından birden çok özel bağlantı noktasına eşlemeyi desteklemediği için yük dengelemesi mümkün değildir.
* **Sanal bir makinede Oracle WebLogic Server birden fazla örneği.** Dağıtımınızın gereksinimlerine bağlı olarak, sanal makine yeterince büyükse, aynı sanal makinede birden çok Oracle WebLogic Server örneği çalıştırmayı düşünebilirsiniz. Örneğin, iki çekirdek içeren orta ölçekli bir sanal makinede, Oracle WebLogic Server'ın iki örneğini çalıştırmayı seçebilirsiniz. Ancak, oracle WebLogic Server'ın birden çok örneğini çalıştıran tek bir sanal makine kullandıysanız, mimarinize tek puan hata yapmaktan kaçınmanızı öneririz. En az iki sanal makine kullanarak daha iyi bir yaklaşım olabilir ve her sanal makine daha sonra Oracle WebLogic Server birden fazla örnekleri çalıştırabilir. Oracle WebLogic Server'ın her örneği hala aynı kümenin bir parçası olabilir. Ancak, Azure yük dengeleyicisi yük dengeli sunucuların benzersiz sanal makineler arasında dağıtılmasını gerektirdiğinden, şu anda aynı sanal makine içinde bu tür Oracle WebLogic Server dağıtımları tarafından ortaya çıkarılan uç noktaları yüklemek için Azure'u kullanmak mümkün değildir.

## <a name="oracle-jdk-virtual-machine-images"></a>Oracle JDK sanal makine görüntüleri
* **JDK 6 ve 7 son güncellemeler.** Azure, Java'nın en son genel, desteklenen sürümünü (şu anda Java 8) kullanmanızı öneririz, ancak JDK 6 ve 7 görüntülerini de kullanılabilir hale getirir. Bu, JDK 8'e yükseltilmeye henüz hazır olmayan eski uygulamalar için tasarlanmıştır. Önceki JDK görüntülerine yönelik güncelleştirmeler artık genel kullanıma sunulmamış olsa da, Oracle ile microsoft ortaklığı göz önüne alındığında, Azure tarafından sağlanan JDK 6 ve 7 görüntüleri, normalde Oracle tarafından yalnızca oracle'ın desteklenen belirli bir grup müşterisine sunulan daha yeni bir genel olmayan güncelleştirme içerecek şekilde tasarlanmıştır. JDK görüntülerinin yeni sürümleri, JDK 6 ve 7'nin güncellenmiş sürümleriyle zaman içinde kullanıma sunulacaktır.

   JDK 6 ve 7 görüntülerinde bulunan JDK ve bunlardan türetilen sanal makineler ve görüntüler yalnızca Azure'da kullanılabilir.
* **64 bit JDK.** Oracle WebLogic Server sanal makine görüntüleri ve Azure tarafından sağlanan Oracle JDK sanal makine görüntüleri hem Windows Server hem de JDK'nın 64 bit sürümlerini içerir.

## <a name="next-steps"></a>Sonraki adımlar
Artık Microsoft Azure'daki sanal makine resimlerine dayalı güncel Oracle çözümlerine genel bir bakış alabilirsiniz. Bir sonraki adımınız ilk Oracle veritabanınızı Azure'a dağıtmaktır.

> [!div class="nextstepaction"]
> [Azure'da oracle veritabanı oluşturma](oracle-database-quick-create.md)
