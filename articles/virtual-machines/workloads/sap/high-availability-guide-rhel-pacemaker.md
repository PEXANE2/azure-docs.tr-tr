---
title: Azure'da RHEL'de Kalp Pili Kurulumu | Microsoft Dokümanlar
description: Azure'da Red Hat Enterprise Linux'ta Pacemaker'ı ayarlama
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/17/2018
ms.author: radeltch
ms.openlocfilehash: 21c551721815847eea4cb1435298ea6f7bf37966
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264485"
---
# <a name="setting-up-pacemaker-on-red-hat-enterprise-linux-in-azure"></a>Azure'da Red Hat Enterprise Linux'ta Pacemaker'ı ayarlama

[planning-guide]:planning-guide.md
[deployment-guide]:deployment-guide.md
[dbms-guide]:dbms-guide.md
[sap-hana-ha]:sap-hana-high-availability.md
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2009879]:https://launchpad.support.sap.com/#/notes/2009879
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1999351]:https://launchpad.support.sap.com/#/notes/1999351

[virtual-machines-linux-maintenance]:../../maintenance-and-updates.md#maintenance-that-doesnt-require-a-reboot


Önce aşağıdaki SAP Notlarını ve bildirilerini okuyun:

* SAP Note [1928533], olan:
  * SAP yazılımının dağıtımı için desteklenen Azure VM boyutlarının listesi.
  * Azure VM boyutları için önemli kapasite bilgileri.
  * Desteklenen SAP yazılımı ve işletim sistemi (OS) ve veritabanı kombinasyonları.
  * Microsoft Azure'da Windows ve Linux için gerekli SAP çekirdeği sürümü.
* SAP Note [2015553,] Azure'da SAP destekli SAP yazılım dağıtımları için ön koşulları listeler.
* SAP Note [2002167] Red Hat Enterprise Linux için işletim sistemi ayarlarını tavsiye etti
* SAP Not [2009879] Red Hat Enterprise Linux için SAP HANA Yönergeleri vardır
* SAP Note [2178632,] Azure'da SAP için bildirilen tüm izleme ölçümleri hakkında ayrıntılı bilgilere sahiptir.
* SAP Note [2191498,] Azure'da Linux için gerekli SAP Host Agent sürümüne sahiptir.
* SAP Note [2243692,] Azure'da Linux'ta SAP lisanslama hakkında bilgi edinmiştir.
* SAP Note [1999351,] SAP için Azure Gelişmiş İzleme Uzantısı için ek sorun giderme bilgilerine sahiptir.
* [SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) Linux için gerekli tüm SAP Notları'na sahiptir.
* [Linux'ta SAP için Azure Sanal Makineler planlaması ve uygulaması][planning-guide]
* [Linux'ta SAP için Azure Sanal Makineler dağıtımı (bu makale)][deployment-guide]
* [Linux'ta SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* [Kalp pili kümesinde SAP HANA sistem replikasyonu](https://access.redhat.com/articles/3004101)
* Genel RHEL belgeleri
  * [Yüksek Kullanılabilirlik Eklentisi Genel Bakış](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index)
  * [Yüksek Kullanılabilirlik Eklenti Yönetimi](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index)
  * [Yüksek Kullanılabilirlik Eklenti Başvurusu](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index)
  * [RHEL Yüksek Kullanılabilirlik Kümeleri için Destek İlkeleri - sbd ve fence_sbd](https://access.redhat.com/articles/2800691)
* Azure'a özel RHEL belgeleri:
  * [RHEL Yüksek Kullanılabilirlik Kümeleri için Destek İlkeleri - Küme Üyesi Olarak Microsoft Azure Sanal Makineleri](https://access.redhat.com/articles/3131341)
  * [Microsoft Azure'da Red Hat Enterprise Linux 7.4 (ve sonrası) Yüksek Kullanılabilirlik Kümesi'ni yükleme ve yapılandırma](https://access.redhat.com/articles/3252491)
  * [SAP S/4HANA ASCS/ERS'i RHEL 7.6'da Pacemaker'da Bağımsız Enqueue Server 2 (ENSA2) ile yapılandırın](https://access.redhat.com/articles/3974941)

## <a name="cluster-installation"></a>Küme yükleme

![PACEMaker üzerinde RHEL genel bakış](./media/high-availability-guide-rhel-pacemaker/pacemaker-rhel.png)

> [!NOTE]
> Red Hat yazılımla taklit edilmiş izleme köpeğini desteklemiyor. Red Hat bulut platformlarında SBD'yi desteklemez. Ayrıntılar için [RHEL Yüksek Kullanılabilirlik Kümeleri için Destek İlkeleri - sbd ve fence_sbd](https://access.redhat.com/articles/2800691).
> Azure'daki Pacemaker Red Hat Enterprise Linux kümeleri için desteklenen tek eskrim mekanizması Azure çit aracısIdır.  

Aşağıdaki öğeler, tüm düğümler için geçerli olan **[A]** ile önceden belirlenmiştir, **[1] -** yalnızca düğüm 1 veya **[2]** için geçerlidir - yalnızca düğüm 2 için geçerlidir.

1. **[A]** Kayıt

   Sanal makinelerinizi kaydedin ve RHEL 7 için depolar içeren bir havuza takın.

   <pre><code>sudo subscription-manager register
   # List the available pools
   sudo subscription-manager list --available --matches '*SAP*'
   sudo subscription-manager attach --pool=&lt;pool id&gt;
   </code></pre>

   Bir Azure Marketi PAYG RHEL resmine bir havuz ekleyerek, RHEL kullanımınız için etkili bir şekilde çift faturalı olacağınızı unutmayın: PAYG görüntüsü için bir kez, eklediğiniz havuzdaki RHEL hakkı için bir kez. Bunu azaltmak için Azure artık BYOS RHEL görüntüleri sağlar. Daha fazla bilgi [burada](../redhat/byos.md)bulabilirsiniz.

1. SAP depoları için RHEL'i **etkinleştirme**

   Gerekli paketleri yüklemek için aşağıdaki depoları etkinleştirin.

   <pre><code>sudo subscription-manager repos --disable "*"
   sudo subscription-manager repos --enable=rhel-7-server-rpms
   sudo subscription-manager repos --enable=rhel-ha-for-rhel-7-server-rpms
   sudo subscription-manager repos --enable=rhel-sap-for-rhel-7-server-rpms
   sudo subscription-manager repos --enable=rhel-ha-for-rhel-7-server-eus-rpms
   </code></pre>

1. **[A]** INSTALL RHEL HA Eklentisi

   <pre><code>sudo yum install -y pcs pacemaker fence-agents-azure-arm nmap-ncat
   </code></pre>

   > [!IMPORTANT]
   > Bir kaynak durdurma başarısız olursa veya küme düğümleri artık birbirini iletişim kuramıyorsa, müşterilerin zaman içinde daha hızlı bir arızadan yararlanmaları için Azure Fence aracısının (veya daha sonraki) aşağıdaki sürümlerini öneririz:  
   > RHEL 7.6: çit-ajanlar-4.2.1-11.el7_6.8  
   > RHEL 7.5: çit-ajanlar-4.0.11-86.el7_5.8  
   > RHEL 7.4: çit-ajanlar-4.0.11-66.el7_4.12  
   > Daha fazla bilgi için, [RhEL Yüksek Kullanılabilirlik küme üyesi olarak çalışan Azure VM'nin çitle çevrilmesi çok uzun zaman alır veya Çitleme,VM kapanmadan önce arıza /kez devre dışı](https://access.redhat.com/solutions/3408711)kalır.

   Azure çit aracısının sürümünü kontrol edin. Gerekirse, yukarıda belirtilene eşit veya daha sonra bir sürüme güncelleyin.

   <pre><code># Check the version of the Azure Fence Agent
    sudo yum info fence-agents-azure-arm
   </code></pre>

   > [!IMPORTANT]
   > Azure Fence aracısını güncelleştirmeniz gerekiyorsa ve özel rol kullanıyorsanız, özel rolü eylem **powerOff'u**içerecek şekilde güncelleştirdiğinizden emin olun. Ayrıntılar [için bkz: Çit aracısı için özel bir rol oluştur.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker#1-create-a-custom-role-for-the-fence-agent)  

1. **[A]** Kurulum ana bilgisayar ad çözümü

   Bir DNS sunucusu kullanabilir veya tüm düğümlerde /etc/hosts'ı değiştirebilirsiniz. Bu örnek, /etc/hosts dosyasının nasıl kullanılacağını gösterir.
   Aşağıdaki komutlarda IP adresini ve ana bilgisayar adını değiştirin. /etc/hosts kullanmanın yararı, kümenizin DNS'den bağımsız hale gelmesidir, bu da tek bir hata noktası olabilir.

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   /etc/hosts'a aşağıdaki satırları ekleyin. IP adresini ve ana bilgisayar adını ortamınıza uyacak şekilde değiştirme

   <pre><code># IP address of the first cluster node
   <b>10.0.0.6 prod-cl1-0</b>
   # IP address of the second cluster node
   <b>10.0.0.7 prod-cl1-1</b>
   </code></pre>

1. **[A]** Hacluster parolasını aynı parolayla değiştirme

   <pre><code>sudo passwd hacluster
   </code></pre>

1. **[A]** Kalp pili için güvenlik duvarı kuralları ekleme

   Küme düğümleri arasındaki tüm küme iletişimine aşağıdaki güvenlik duvarı kurallarını ekleyin.

   <pre><code>sudo firewall-cmd --add-service=high-availability --permanent
   sudo firewall-cmd --add-service=high-availability
   </code></pre>

1. **[A]** Temel küme hizmetlerini etkinleştirme

   Pacemaker hizmetini etkinleştirmek ve başlatmak için aşağıdaki komutları çalıştırın.

   <pre><code>sudo systemctl start pcsd.service
   sudo systemctl enable pcsd.service
   </code></pre>

1. **[1]** Kalp Pili kümesi ni oluştur

   Düğümlerin kimliğini doğrulamak ve kümeoluşturmak için aşağıdaki komutları çalıştırın. Belleğin bakımını korumasını sağlamak için belirteci 30000 olarak ayarlayın. Daha fazla bilgi [için, Linux için bu makaleye][virtual-machines-linux-maintenance]bakın.

   <pre><code>sudo pcs cluster auth <b>prod-cl1-0</b> <b>prod-cl1-1</b> -u hacluster
   sudo pcs cluster setup --name <b>nw1-azr</b> <b>prod-cl1-0</b> <b>prod-cl1-1</b> --token 30000
   sudo pcs cluster start --all

   # Run the following command until the status of both nodes is online
   sudo pcs status

   # Cluster name: nw1-azr
   # WARNING: no stonith devices and stonith-enabled is not false
   # Stack: corosync
   # Current DC: <b>prod-cl1-1</b> (version 1.1.18-11.el7_5.3-2b07d5c5a9) - partition with quorum
   # Last updated: Fri Aug 17 09:18:24 2018
   # Last change: Fri Aug 17 09:17:46 2018 by hacluster via crmd on <b>prod-cl1-1</b>
   #
   # 2 nodes configured
   # 0 resources configured
   #
   # Online: [ <b>prod-cl1-0</b> <b>prod-cl1-1</b> ]
   #
   # No resources
   #
   #
   # Daemon Status:
   #   corosync: active/disabled
   #   pacemaker: active/disabled
   #   pcsd: active/enabled
   </code></pre>

1. **[A]** Set Beklenen Oylar

   <pre><code>sudo pcs quorum expected-votes 2
   </code></pre>

## <a name="create-stonith-device"></a>STONITH cihazı nı oluşturma

STONITH aygıtı, Microsoft Azure'a karşı yetkilendirme yapmak için bir Hizmet Sorumlusu kullanır. Hizmet Sorumlusu oluşturmak için aşağıdaki adımları izleyin.

1. Şuraya gidin: <https://portal.azure.com>
1. Azure Active Directory bıçağını açın  
   Özellikler'e gidin ve Dizin Kimliğini yazın. Bu **kiracı kimliği.**
1. Uygulama kayıtlarını tıklatın
1. Yeni Kayıt'ı tıklatın
1. Bir Ad girin, "Yalnızca bu kuruluş dizinindeki hesaplar" seçeneğini belirleyin 
2. Uygulama Türü "Web"i seçin, oturum açma URL'si girin (örneğin http:\//localhost) ve Ekle'yi tıklatın  
   Oturum açma URL'si kullanılmaz ve geçerli bir URL olabilir
1. Sertifikalar ve Sırlar'ı seçin ve ardından Yeni istemci sırrını tıklatın
1. Yeni bir anahtar için açıklama girin, "Asla süresi dolmaz" seçeneğini belirleyin ve Ekle'yi tıklatın
1. Değeri yazın. Hizmet Sorumlusunun **şifresi** olarak kullanılır
1. Genel Bakış’ı seçin. Başvuru Kimliğini yazın. Hizmet Sorumlusunun kullanıcı adı (aşağıdaki adımlardaki**giriş kimliği)** olarak kullanılır.

### <a name="1-create-a-custom-role-for-the-fence-agent"></a>**[1]** Çit aracısı için özel bir rol oluşturun

Hizmet Sorumlusunun Varsayılan olarak Azure kaynaklarına erişim izni yoktur. Kümedeki tüm sanal makineleri başlatmak ve durdurmak (güç kapatma) için Hizmet Sorumlusuna izin vermeniz gerekir. Özel rolü zaten oluşturmadıysanız, [PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell) veya Azure [CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli) kullanarak oluşturabilirsiniz

Giriş dosyası için aşağıdaki içeriği kullanın. İçeriği aboneliklerinize uyarlamanız, c276fc76-9cd4-44c9-99a7-4fd71546436e ve e91d47c4-76f3-4271-a796-21b4ecfe3624'ü aboneliğinizin kimlikleriyle değiştirmeniz gerekmektedir. Yalnızca bir aboneliğiniz varsa, AssignableScopes'taki ikinci girişi kaldırın.

```json
{
  "Name": "Linux Fence Agent Role",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows to power-off and start virtual machines",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/powerOff/action",
    "Microsoft.Compute/virtualMachines/start/action"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```

### <a name="a-assign-the-custom-role-to-the-service-principal"></a>**[A]** Özel rolü Hizmet Sorumlusuna atama

Son bölümde oluşturulan özel rolü "Linux Fence Agent Role"yi Hizmet Sorumlusuna atayın. Artık Sahibi rolünü kullanmayın!

1. Şuraya gidin: https://portal.azure.com
1. Tüm kaynaklar bıçağını açın
1. İlk küme düğümünün sanal makinesini seçin
1. Erişim denetimine (IAM) tıklayın
1. Rol Atama ekle'yi tıklatın
1. "Linux Fence Agent Role" rolünü seçin
1. Yukarıda oluşturduğunuz uygulamanın adını girin
1. Kaydet’e tıklayın.

İkinci küme düğümü için yukarıdaki adımları yineleyin.

### <a name="1-create-the-stonith-devices"></a>**[1]** STONITH aygıtlarını oluşturun

Sanal makinelerin izinlerini düzenledikten sonra kümedeki STONITH aygıtlarını yapılandırabilirsiniz.

<pre><code>
sudo pcs property set stonith-timeout=900
</code></pre>

Çit aygıtını yapılandırmak için aşağıdaki komutu kullanın.

> [!NOTE]
> RHEL ana bilgisayar adları ve Azure düğüm adları aynı değilse, yalnızca komutta 'pcmk_host_map' seçeneği gereklidir. Komuttaki kalın bölüme bakın.

<pre><code>sudo pcs stonith create rsc_st_azure fence_azure_arm login="<b>login ID</b>" passwd="<b>password</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" subscriptionId="<b>subscription id</b>" <b>pcmk_host_map="prod-cl1-0:10.0.0.6;prod-cl1-1:10.0.0.7"</b> power_timeout=240 pcmk_reboot_timeout=900</code></pre>

### <a name="1-enable-the-use-of-a-stonith-device"></a>**[1]** BIR STONITH cihazının kullanımını etkinleştirme

<pre><code>sudo pcs property set stonith-enabled=true
</code></pre>

## <a name="next-steps"></a>Sonraki adımlar

* [AZURE Sanal Makineler SAP için planlama ve uygulama][planning-guide]
* [SAP için Azure Sanal Makineler dağıtımı][deployment-guide]
* [SAP için Azure Sanal Makineler DBMS dağıtımı][dbms-guide]
* Azure Sanal M'lerde SAP HANA'nın yüksek kullanılabilirlik oluşturmasını ve olağanüstü kurtarma yı planlamayı öğrenmek için Azure [Sanal Makinelerde (VM) SAP HANA'nın Yüksek Kullanılabilirliği bölümüne][sap-hana-ha] bakın
