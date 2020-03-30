---
title: Log Analytics ağ geçidini kullanarak bilgisayarları bağlayın | Microsoft Dokümanlar
description: İnternet erişimi olmadığında Azure Otomasyonu ve Log Analytics hizmetine veri göndermek için Log Analytics ağ geçidini kullanarak cihazlarınızı ve Operasyon Yöneticisi tarafından izlenen bilgisayarlarınızı bağlayın.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 12/24/2019
ms.openlocfilehash: a92e96a835f24ac54fa55b05086a35b9a91d609e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80298347"
---
# <a name="connect-computers-without-internet-access-by-using-the-log-analytics-gateway-in-azure-monitor"></a>Azure Monitor'da Log Analytics ağ geçidini kullanarak internet erişimi olmayan bilgisayarları bağlayın

>[!NOTE]
>Microsoft Operations Management Suite (OMS) Microsoft Azure Monitor'a geçerken, terminoloji değişiyor. Bu makalede, Azure Günlük Analizi ağ geçidi olarak OMS Ağ Geçidi ifade eder. 
>

Bu makalede, doğrudan bağlı olan veya Operations Manager tarafından izlenen bilgisayarların Internet erişimi olmadığında Log Analytics ağ geçidini kullanarak Azure Automation ve Azure Monitor ile iletişimin nasıl yapılandırılabildiğini açıklanmaktadır. 

Log Analytics ağ geçidi, HTTP CONNECT komutunu kullanarak HTTP tünellerini destekleyen bir HTTP ileri proxy'sidir. Bu ağ geçidi, verileri Doğrudan Internet'e bağlanamayan bilgisayarlar adına Azure Automation'a ve Azure Monitor'daki Log Analytics çalışma alanına gönderir. 

Log Analytics ağ geçidi şunları destekler:

* Arkasındaki her aracı üzerinde yapılandırılan ve Azure Automation Hybrid Runbook Workers ile yapılandırılan aynı Log Analytics çalışma alanlarına raporlama.  
* Microsoft İzleme Aracısı'nın Azure Monitor'daki Bir Günlük Analizi çalışma alanına doğrudan bağlı olduğu Windows bilgisayarlar.
* Linux için bir Log Analytics aracının Azure Monitor'daki Log Analytics çalışma alanına doğrudan bağlı olduğu Linux bilgisayarları.  
* System Center Operations Manager 2012 SP1 with UR7, Operations Manager 2012 R2 with UR3 veya Operations Manager 2016 veya sonraki yıllarda Log Analytics ile entegre olan bir yönetim grubu.  

Bazı BT güvenlik ilkeleri ağ bilgisayarları için Internet bağlantısına izin vermez. Bu bağlantısız bilgisayarlar satış noktası (POS) aygıtları veya örneğin BT hizmetlerini destekleyen sunucular olabilir. Bu aygıtları yönetebilmeniz ve izleyebilmeniz için bu aygıtları Azure Otomasyonuna veya Log Analytics çalışma alanına bağlamak için, bunları doğrudan Log Analytics ağ geçidiyle iletişim kuracak şekilde yapılandırın. Log Analytics ağ geçidi, yapılandırma bilgilerini alabilir ve verileri onların adına iletebilir. Bilgisayarlar Log Analytics aracısıyla doğrudan bir Log Analytics çalışma alanına bağlanmak üzere yapılandırılırsa, bilgisayarlar bunun yerine Log Analytics ağ geçidiyle iletişim kurar.  

Log Analytics ağ geçidi, aracılardan gelen verileri doğrudan hizmete aktarıyor. Aktarımsırasındaki verilerin hiçbirini çözümlemiyor ve ağ geçidi hizmetle bağlantısını kaybettiğinde verileri önbelleğe almıyor. Ağ geçidi hizmetle iletişim kuramayınca, aracı çalıştırmaya devam eder ve toplanan verileri izlenen bilgisayarın diskinde sıraya alır. Bağlantı geri yüklendiğinde, aracı önbelleğe alınan verileri Azure Monitor'a gönderir.

Bir Operasyon Yöneticisi yönetim grubu Log Analytics ile entegre edildiğinde, yönetim sunucuları etkinleştirdiğiniz çözüme bağlı olarak yapılandırma bilgilerini almak ve toplanan verileri göndermek için Log Analytics ağ geçidine bağlanacak şekilde yapılandırılabilir .  Operations Manager aracıları yönetim sunucusuna bazı veriler gönderir. Örneğin, aracılar Operations Manager uyarıları, yapılandırma değerlendirme verileri, örnek alan verileri ve kapasite verileri gönderebilir. Internet Information Services (IIS) günlükleri, performans verileri ve güvenlik olayları gibi diğer yüksek hacimli veriler doğrudan Log Analytics ağ geçidine gönderilir. 

Bir çevre ağındaki veya yalıtılmış ağdaki güvenilmeyen sistemleri izlemek için bir veya daha fazla Operations Manager Gateway sunucusu dağıtılırsa, bu sunucular Bir Günlük Analizi ağ geçidiyle iletişim kuramaz.  Operations Manager Gateway sunucuları yalnızca bir yönetim sunucusuna rapor verebilir.  Bir Operasyon Yöneticisi yönetim grubu Log Analytics ağ geçidiyle iletişim kurmak üzere yapılandırıldığında, proxy yapılandırma bilgileri Azure Monitor için günlük verilerini toplamak üzere yapılandırılan aracı lar tarafından yönetilen her bilgisayara otomatik olarak dağıtılır, ayar boş olsa bile.

Ağ geçidi aracılığıyla bir Log Analytics çalışma alanıyla iletişim kurandoğrudan bağlı veya Operasyon Yönetimi grupları için yüksek kullanılabilirlik sağlamak için, trafiği birden çok ağ geçidi sunucusuna yönlendirmek ve dağıtmak için ağ yükü dengelemesini (NLB) kullanın. Bu şekilde, bir ağ geçidi sunucusu çökerse, trafik kullanılabilir başka bir düğüme yönlendirilir.  

Log Analytics ağ geçidini çalıştıran bilgisayar, giriş kapısının iletişim kurması gereken hizmet bitiş noktalarını tanımlaması için Log Analytics Windows aracısının olmasını gerektirir. Aracının, ağ geçidinin arkasındaki aracıların veya Operasyon Yöneticisi yönetim grubunun yapılandırıldığı aynı çalışma alanlarına rapor vermek için ağ geçidini yönlendirmesi de gerekir. Bu yapılandırma ağ geçidi ve aracının atanan çalışma alanıyla iletişim kurmasını sağlar.

Bir ağ geçidi en fazla dört çalışma alanı için birden fazla yuvaya darayabilir. Bu, bir Windows aracısının desteklediği toplam çalışma alanı sayısıdır.  

Aracıların ağ geçidine otomatik olarak veri aktarabilmesi ve ağ geçidinden veri aktarabilmesi için her aracının ağ bağlantısına sahip olması gerekir. Ağ geçidini etki alanı denetleyicisi üzerinde yüklemekten kaçının. Ağ geçidi sunucusunun arkasındaki Linux [bilgisayarları,](agent-linux.md#install-the-agent-using-wrapper-script) Linux için Log Analytics aracısını yüklemek için sarmalayıcı komut dosyası yükleme yöntemini kullanamaz. Aracı nın el ile indirilmesi, bilgisayara kopyalanması ve el ile yüklenmesi gerekir, çünkü ağ geçidi yalnızca daha önce bahsedilen Azure hizmetleriyle iletişimkurmayı destekler.

Aşağıdaki diyagram, doğrudan aracılardan ağ geçidine, Azure Otomasyonu ve Log Analytics'e akan verileri gösterir. Aracı proxy yapılandırması, Log Analytics ağ geçidinin yapılandırıldıkları bağlantı noktasıyla eşleşmelidir.  

![Hizmetlerle doğrudan aracı iletişimdiyagramı](./media/gateway/oms-omsgateway-agentdirectconnect.png)

Aşağıdaki diyagram, Bir Operasyon Yöneticisi yönetim grubundan Log Analytics'e veri akışını gösterir.   

![Log Analytics ile Operasyon Yöneticisi iletişim diyagramı](./media/gateway/log-analytics-agent-opsmgrconnect.png)

## <a name="set-up-your-system"></a>Sisteminizi ayarlama

Log Analytics ağ geçidini çalıştırmak üzere atanan bilgisayarların aşağıdaki yapılandırmaya sahip olması gerekir:

* Windows 10, Windows 8.1 veya Windows 7
* Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 veya Windows Server 2008
* Microsoft .NET Framework 4.5
* En az 4 çekirdekli işlemci ve 8 GB bellek 
* Windows için ağ geçidi üzerinden iletişim sağlayan aracılarla aynı çalışma alanına rapor vermek üzere yapılandırılan bir [Log Analytics aracısı](agent-windows.md)

### <a name="language-availability"></a>Dil kullanılabilirliği

Log Analytics ağ geçidi şu dillerde kullanılabilir:

- Çince (Basitleştirilmiş)
- seçenekleri yerine
- Çekçe
- Felemenkçe
- Türkçe
- Fransızca
- Almanca
- Macarca
- İtalyanca
- Japonca
- Korece
- Lehçe
- Portekizce (Brezilya)
- Portekizce (Portekiz)
- Rusça
- İspanyolca (Uluslararası)

### <a name="supported-encryption-protocols"></a>Desteklenen şifreleme protokolleri

Log Analytics ağ geçidi yalnızca Aktarım Katmanı Güvenliği (TLS) 1.0, 1.1 ve 1.2'yi destekler.  Güvenli Soket Katmanı'nı (SSL) desteklemez.  Log Analytics'e aktarım halindeki verilerin güvenliğini sağlamak için ağ geçidini en az TLS 1,2 kullanacak şekilde yapılandırın. TLS veya SSL'nin eski sürümleri savunmasızdır. Şu anda geriye dönük uyumluluğa izin vermelerine rağmen, bunları kullanmaktan kaçının.  

Daha fazla bilgi için [TLS 1.2'yi kullanarak veri gönderme'yi](../../azure-monitor/platform/data-security.md#sending-data-securely-using-tls-12)güvenli bir şekilde gözden geçirin. 

### <a name="supported-number-of-agent-connections"></a>Desteklenen aracı bağlantısı sayısı

Aşağıdaki tablo, bir ağ geçidi sunucusuyla yaklaşık olarak kaç aracının iletişim kurabileceğini gösterir. Destek, her 6 saniyede yaklaşık 200 KB veri yükleyen aracıları temel alıntır. Test edilen her aracı için veri hacmi günde yaklaşık 2,7 GB'dır.

|Ağ geçidi |Desteklenen aracılar (yaklaşık)|  
|--------|----------------------------------|  
|İşlemci: Intel Xeon İşlemci E5-2660 v3 \@ 2.6 GHz 2 Çekirdek<br> Bellek: 4 GB<br> Ağ bant genişliği: 1 Gbps| 600|  
|İşlemci: Intel Xeon İşlemci E5-2660 v3 \@ 2.6 GHz 4 Çekirdek<br> Bellek: 8 GB<br> Ağ bant genişliği: 1 Gbps| 1000|  

## <a name="download-the-log-analytics-gateway"></a>Log Analytics ağ geçidini indirin

Log Analytics ağ geçidi Kurulum dosyasının en son sürümünü Microsoft Download Center[(Download Link)](https://go.microsoft.com/fwlink/?linkid=837444)veya Azure portalından alın.

Azure portalından Log Analytics ağ geçidini almak için aşağıdaki adımları izleyin:

1. Hizmet listesine göz atın ve ardından **Günlük Analitiği'ni**seçin. 
1. Bir çalışma alanı seçin.
1. Çalışma alanı bıçak, **Genel**altında, **Hızlı Başlat**seçin. 
1. **Çalışma alanına bağlanmak için bir veri kaynağı seçin**, **Bilgisayarlar'ı**seçin.
1. Direct **Agent** blade'de, **Giriş Analizi ağ geçidini indir'i**seçin.
 
   ![Log Analytics ağ geçidini indirmek için adımların ekran görüntüsü](./media/gateway/download-gateway.png)

or 

1. Çalışma alanı bıçak, **Ayarlar**altında, **Gelişmiş ayarları**seçin.
1. Bağlı **Kaynaklar** > **Windows Sunucularına** gidin ve **Giriş Günlüğü Analizi ağ geçidini seçin.**

## <a name="install-log-analytics-gateway-using-setup-wizard"></a>Kurulum sihirbazı kullanarak Günlük Analizi ağ geçidini yükleme

Kurulum sihirbazını kullanarak bir ağ geçidi yüklemek için aşağıdaki adımları izleyin. 

1. Hedef klasörden, Log **Analytics gateway.msi'yi**çift tıklatın.
1. **Hoş Geldiniz** sayfasında, **İleri**’yi seçin.

   ![Ağ Geçidi Kurulumu sihirbazında Karşılama sayfasının ekran görüntüsü](./media/gateway/gateway-wizard01.png)

1. Lisans **Sözleşmesi** sayfasında, Microsoft Yazılım Lisans Koşullarını kabul etmek için **Lisans Sözleşmesi'ndeki şartları kabul ediyorum** ve sonra **İleri'yi**seçin.
1. Bağlantı **Noktası ve proxy adresi** sayfasında:

   a. Ağ geçidi için kullanılacak TCP bağlantı noktası numarasını girin. Kurulum, Windows Güvenlik Duvarı'nda gelen bir kuralı yapılandırmak için bu bağlantı noktası numarasını kullanır.  Varsayılan değer 8080'dir.
      Bağlantı noktası numarasının geçerli aralığı 1 ile 65535 arasındadır. Giriş bu aralıkta düşmezse, bir hata iletisi görüntülenir.

   b. Ağ geçidinin yüklü olduğu sunucunun bir proxy üzerinden iletişim kurması gerekiyorsa, ağ geçidinin bağlanması gereken proxy adresini girin. Örneğin, `http://myorgname.corp.contoso.com:80` girin.  Bu alanı boş bırakırsanız, ağ geçidi doğrudan internete bağlanmayı deneyecektir.  Proxy sunucunuz kimlik doğrulaması gerektiriyorsa, bir kullanıcı adı ve parola girin.

   c. **Sonraki'ni**seçin.

   ![Ağ geçidi proxy'si için yapılandırma ekran görüntüsü](./media/gateway/gateway-wizard02.png)

1. Microsoft Update etkin değilse, Microsoft Update sayfası görüntülenir ve etkinleştirmeyi seçebilirsiniz. Seçim yapın ve sonra **İleri'yi**seçin. Aksi halde, bir sonraki adıma devam edin.
1. Hedef **Klasör** sayfasında varsayılan klasörC:\Program Files\OMS Ağ Geçidi'ni bırakın veya ağ geçidini yüklemek istediğiniz konumu girin. Ardından **İleri'yi**seçin.
1. **Yüklemeye Hazır** sayfasında **Yükle'yi**seçin. Kullanıcı Hesabı Denetimi yükleme izni isterse **Evet'i**seçin.
1. Kurulum bittikten sonra **Finish'i**seçin. Hizmetin çalıştığını doğrulamak için services.msc snap-in'i açın ve **OMS Ağ Geçidi'nin** hizmetler listesinde göründüğünü ve durumunun **çalıştığını**doğrulayın.

   ![OMS Ağ Geçidi'nin çalıştığını gösteren yerel hizmetlerin ekran görüntüsü](./media/gateway/gateway-service.png)

## <a name="install-the-log-analytics-gateway-using-the-command-line"></a>Komut satırını kullanarak Log Analytics ağ geçidini yükleme

Ağ geçidi için indirilen dosya, komut satırından veya diğer otomatik yöntemlerden sessiz yüklemeyi destekleyen bir Windows Installer paketidir. Windows Yükleyici için standart komut satırı seçeneklerini bilmiyorsanız, [Komut satırı seçeneklerine](https://docs.microsoft.com/windows/desktop/Msi/command-line-options)bakın.
 
Aşağıdaki tabloda kurulum tarafından desteklenen parametreler vurgulanır.

|Parametreler| Notlar|
|----------|------| 
|Bağlantınoktasınumarası | Dinlemek için ağ geçidi için TCP bağlantı noktası numarası |
|Proxy | Proxy sunucusunun IP adresi |
|KURULUMDIR | Ağ geçidi yazılım dosyalarının yükleme dizinini belirtmek için tam nitelikli yol |
|USERNAME | Proxy sunucusu ile kimlik doğrulaması için kullanıcı kimliği |
|Parola | Proxy ile kimlik doğrulaması için kullanıcı kimliğinin şifresi |
|LicenseAccepted | Lisans sözleşmesini kabul ettiğinizi doğrulamak için **1** değeri belirtin |
|HASAUTH | KULLANICI ADI/Parola parametreleri belirtildiğinde **1** değerini belirtin |
|HASPROXY | **PROXY** parametresi için IP adresi belirtirken **1** değerini belirtin |

Ağ geçidini sessizce yüklemek ve belirli bir proxy adresi, bağlantı noktası numarası yla yapılandırmak için aşağıdakileri yazın:

```dos
Msiexec.exe /I "oms gateway.msi" /qn PORTNUMBER=8080 PROXY="10.80.2.200" HASPROXY=1 LicenseAccepted=1 
```

/qn komut satırı seçeneğini kullanarak kurulumu gizler, /qb sessiz yükleme sırasında kurulumu gösterir.  

Proxy ile kimlik doğrulamak için kimlik bilgileri sağlamanız gerekiyorsa, aşağıdakileri yazın:

```dos
Msiexec.exe /I "oms gateway.msi" /qn PORTNUMBER=8080 PROXY="10.80.2.200" HASPROXY=1 HASAUTH=1 USERNAME="<username>" PASSWORD="<password>" LicenseAccepted=1 
```

Kurulumdan sonra, aşağıdaki PowerShell cmdlets kullanarak ayarların kabul edildiğini (kullanıcı adı ve parola hariç) onaylayabilirsiniz:

- **Get-OMSGatewayConfig** – TCP Bağlantı Noktasını verir ağ geçidi dinleyecek şekilde yapılandırılır.
- **Get-OMSGatewayRelayProxy** – Iletişim kurmak için yapılandırdığınız proxy sunucusunun IP adresini verir.

## <a name="configure-network-load-balancing"></a>Ağ yük dengelemeyi yapılandırma

Ağ yük dengeleme (NLB) kullanarak ağ yük dengeleme (NLB) kullanarak yüksek kullanılabilirlik için ağ geçidini Microsoft [Ağ Yük Dengeleme (NLB)](https://docs.microsoft.com/windows-server/networking/technologies/network-load-balancing), [Azure Yük Dengeleyicisi](../../load-balancer/load-balancer-overview.md)veya donanım tabanlı yük dengeleyicileri kullanarak yapılandırabilirsiniz. Yük bakiyesi, istenen bağlantıları Log Analytics aracılarından veya Operasyon Yöneticisi yönetim sunucularından düğümlerine yönlendirerek trafiği yönetir. Bir Ağ Geçidi sunucusu çökerse, trafik diğer düğümlere yönlendirilir.

### <a name="microsoft-network-load-balancing"></a>Microsoft Ağ Yük Dengeleme

Windows Server 2016 ağ yük dengeleme kümesini nasıl tasarlayıp dağıtacağımı öğrenmek için [Ağ yük dengeleme](https://docs.microsoft.com/windows-server/networking/technologies/network-load-balancing)bölümüne bakın. Aşağıdaki adımlar, Microsoft ağ yük dengeleme kümesini nasıl yapılandırılabildiğini açıklar.  

1. NLB kümesinin bir üyesi olan Windows sunucusunda yönetim hesabı içeren oturum açın.  
2. Sunucu Yöneticisi'nde Ağ Yük Dengeleme Yöneticisi'ni açın, **Araçlar'ı**tıklatın ve ardından **Ağ Yük Dengeleme Yöneticisi'ni**tıklatın.
3. Bir Log Analytics ağ geçidi sunucusunu Microsoft Monitoring Agent yüklü olarak bağlamak için kümenin IP adresine sağ tıklayın ve ardından **Küme'ye Ana Bilgisayar Ekle'yi**tıklatın. 

    ![Ağ Yük Dengeleme Yöneticisi – Kümeye Ana Bilgisayar Ekle](./media/gateway/nlb02.png)
 
4. Bağlanmak istediğiniz ağ geçidi sunucusunun IP adresini girin. 

    ![Ağ Yük Dengeleme Yöneticisi – Kümeye Ana Bilgisayar Ekle: Bağlan](./media/gateway/nlb03.png) 

### <a name="azure-load-balancer"></a>Azure Load Balancer

Azure Yük Dengeleyicisi'ni nasıl tasarlayıp dağıtacağımı öğrenmek için [bkz.](../../load-balancer/load-balancer-overview.md) Temel bir yük dengeleyicisini dağıtmak için, **arka uç sunucuları oluştur**bölümünde özetlenen adımları hariç olmak üzere bu hızlı [başlatmada](../../load-balancer/quickstart-load-balancer-standard-public-portal.md) özetlenen adımları izleyin.   

> [!NOTE]
> **Temel SKU'yu**kullanarak Azure Yük Dengeleyicisini yapılandırmak, Azure sanal makinelerinin bir Kullanılabilirlik Kümesi'ne ait olmasını gerektirir. Kullanılabilirlik kümeleri hakkında daha fazla bilgi edinmek için [azure'daki Windows sanal makinelerinin kullanılabilirliğini yönet'e](../../virtual-machines/windows/manage-availability.md)bakın. Kullanılabilirlik kümesine varolan sanal makineleri eklemek için [Azure Kaynak Yöneticisi VM Kullanılabilirlik Kümesi'ni ayarla'ya](https://gallery.technet.microsoft.com/Set-Azure-Resource-Manager-f7509ec4)bakın.
> 

Yük dengeleyicisi oluşturulduktan sonra, trafiği bir veya daha fazla ağ geçidi sunucusuna dağıtan bir arka uç havuzu oluşturulması gerekir. Hızlı başlangıç makalesi bölümünde açıklanan adımları izleyin [Yük dengeleyicisi için kaynak oluştur.](../../load-balancer/quickstart-load-balancer-standard-public-portal.md)  

>[!NOTE]
>Sistem durumu sondasını yapılandırırken ağ geçidi sunucusunun TCP bağlantı noktasını kullanacak şekilde yapılandırılmalıdır. Sistem durumu sondası, sistem durumu denetimlerine verdikleri yanıta bağlı olarak ağ geçidi sunucularını yük dengeleyici rotasyonundan dinamik olarak ekler veya kaldırır. 
>

## <a name="configure-the-log-analytics-agent-and-operations-manager-management-group"></a>Log Analytics aracısı ve Operasyon Yöneticisi yönetim grubunu yapılandırma

Bu bölümde, Azure Otomasyonu veya Log Analytics ile iletişim kurmak için Doğrudan Bağlı Log Analytics aracılarını, Operasyon Yöneticisi yönetim grubunu veya Azure OtomasyonKarma Runbook İşçilerini Log Analytics ağ geçidiyle nasıl yapılandırabileceğinizi göreceksiniz.  

### <a name="configure-a-standalone-log-analytics-agent"></a>Bağımsız bir Log Analytics aracısı yapılandırma

Log Analytics aracısını yapılandırırken proxy sunucu değerini Log Analytics ağ geçidi sunucusunun IP adresi ve bağlantı noktası numarası ile değiştirin. Bir yük bakiyeleyicisinin arkasına birden çok ağ geçidi sunucusu dağıttıysanız, Log Analytics aracısı proxy yapılandırması yük bakiyeleyicisinin sanal IP adresidir.  

>[!NOTE]
>Giriş Analitiği aracısını ağ geçidine ve Doğrudan Log Analytics'e bağlanan Windows bilgisayarlarına yüklemek [için Bkz. Windows bilgisayarlarını Azure'daki Log Analytics hizmetine bağlayın.](agent-windows.md) Linux bilgisayarlarını bağlamak [için](agent-linux.md)bkz. 
>

Aracıyı ağ geçidi sunucusuna yükledikten sonra, ağ geçidiyle iletişim kuran çalışma alanına veya çalışma alanı aracılarına rapor vermek üzere yapılandırın. Giriş Analizi Windows aracısı ağ geçidine yüklenmezse, olay 300 OMS Ağ Geçidi olay günlüğüne yazılır ve aracının yüklenmesi gerektiğini belirtir. Aracı yüklüyse ancak bu aracılığıyla iletişim kuran aracılarla aynı çalışma alanına rapor vermek üzere yapılandırılmamışsa, olay 105 aynı günlüğe yazılır ve ağ geçidindeki aracının aracılarla aynı çalışma alanına raporlanması için yapılandırılması gerektiğini belirtir. ağ geçidi ile iletişim kurun.

Yapılandırmayı tamamladıktan sonra, değişiklikleri uygulamak için **OMS Ağ Geçidi** hizmetini yeniden başlatın. Aksi takdirde, ağ geçidi Log Analytics ile iletişim kurmaya çalışan aracıları reddeder ve OMS Ağ Geçidi olay günlüğünde olay 105'i bildirir. Bu, ağ geçidi sunucusundaki aracı yapılandırmasından bir çalışma alanı eklediğinizde veya kaldırdığınızda da gerçekleşir.

Otomasyon Karma Runbook Worker ile ilgili bilgi için, [Karma Runbook Worker'ı kullanarak veri merkezinizdeki veya bulutunuzdaki kaynakları otomatikleştir'e](../../automation/automation-hybrid-runbook-worker.md)bakın.

### <a name="configure-operations-manager-where-all-agents-use-the-same-proxy-server"></a>Tüm aracıların aynı proxy sunucusunu kullandığı İşlem yöneticisini yapılandırma

Operasyon Yöneticisi proxy yapılandırması, ayar boş olsa bile Operasyon Yöneticisi'ne rapor veren tüm aracılara otomatik olarak uygulanır.  

Operations Manager'ı desteklemek için OMS Ağ Geçidi'ni kullanmak için şunları yapmış olmalısınız:

* Microsoft İzleme Aracısı (sürüm 8.0.10900.0 veya sonraki sürüm) OMS Ağ Geçidi sunucusuna yüklenmiş ve yönetim grubunuzun rapor vermek üzere yapılandırıldığı aynı Log Analytics çalışma alanlarıyla yapılandırıldı.
* Internet bağlantısı. Alternatif olarak, OMS Ağ Geçidi internete bağlı bir proxy sunucusuna bağlı olmalıdır.

> [!NOTE]
> Ağ geçidi için değer belirtmezseniz, boş değerler tüm aracılara itilir.
>

Operasyon Yöneticisi yönetim grubunuz ilk kez bir Log Analytics çalışma alanına kaydoluyorsa, Operations konsolundaki yönetim grubu için proxy yapılandırmasını belirtme seçeneğini görmezsiniz. Bu seçenek yalnızca yönetim grubu hizmete kaydedilmişse kullanılabilir.  

Tümleştirmeyi yapılandırmak için, Operations konsolunu çalıştırdığınız sistemde ve yönetim grubundaki tüm yönetim sunucularında Netsh'i kullanarak sistem proxy yapılandırmasını güncelleştirin. Şu adımları uygulayın:

1. Yükseltilmiş bir komut istemini açın:

   a. **Başlat'ı** seçin ve **cmd**girin.  

   b. **Komut İstemi'ni** sağ tıklatın ve **yönetici olarak Çalıştır'ı**seçin.  

1. Aşağıdaki komutu girin:

   `netsh winhttp set proxy <proxy>:<port>`

Log Analytics ile tümleştirmeyi tamamladıktan `netsh winhttp reset proxy`sonra, çalıştırarak değişikliği kaldırın. Daha sonra, İşlemler konsolunda, Log Analytics ağ geçidi sunucusunu belirtmek için **Yapılandırılan proxy sunucusu** seçeneğini kullanın. 

1. Operations Manager konsolunda, **Operations Management Suite**altında **Bağlantı'yı**seçin ve ardından **Proxy Server'ı Yapılandır'ı**seçin.

   ![Seçimi Yapılaşı Proxy Server'ı gösteren Operasyon Yöneticisi ekran görüntüsü](./media/gateway/scom01.png)

1. **Operations Management Suite'e erişmek için proxy sunucusu kullan'ı** seçin ve ardından Log Analytics ağ geçidi sunucusunun IP adresini veya yük bakiyeleyicisinin sanal IP adresini girin. Önek `http://`ile başlamak için dikkatli olun.

   ![Proxy sunucu adresini gösteren Operations Manager ekran görüntüsü](./media/gateway/scom02.png)

1. **Bitiş'i**seçin. Operasyon Yöneticisi yönetim grubunuzun artık ağ geçidi sunucusu üzerinden Log Analytics hizmetine iletişim kurmak üzere yapılandırılmıştır.

### <a name="configure-operations-manager-where-specific-agents-use-a-proxy-server"></a>Belirli aracıların proxy sunucusu kullandığı İşlem yöneticisini yapılandırma

Büyük veya karmaşık ortamlar için, Log Analytics ağ geçidi sunucusunu yalnızca belirli sunucuların (veya grupların) kullanmasını isteyebilirsiniz.  Bu sunucular için, bu değer yönetim grubu için genel değer tarafından üzerine yazıldığından, Operasyon Yöneticisi aracısını doğrudan güncelleştiremezsiniz.  Bunun yerine, bu değerleri zorlamak için kullanılan kuralı geçersiz kılın.  

> [!NOTE] 
> Ortamınızda birden fazla Log Analytics ağ geçidi sunucusuna izin vermek istiyorsanız bu yapılandırma tekniğini kullanın.  Örneğin, belirli Log Analytics ağ geçidi sunucularının bölgesel bazda belirtilmesi ni talep edebilirsiniz.
>

Log Analytics ağ geçidi sunucusunu kullanacak belirli sunucuları veya grupları yapılandırmak için: 

1. Operations Manager konsolu'nu açın ve **Yazma** çalışma alanını seçin.  
1. Yazma çalışma alanında **Kurallar'ı**seçin. 
1. Operations Manager araç çubuğunda **Kapsam** düğmesini seçin. Bu düğme kullanılamıyorsa, **İzleme** bölmesinde klasör değil bir nesne seçtiğinizden emin olun. **Kapsam Yönetimi Paketi Nesneleri** iletişim kutusu, ortak hedeflenen sınıfların, grupların veya nesnelerin listesini görüntüler. 
1. Alanı **Ara'da,** **Sağlık Hizmeti'ni** girin ve listeden seçin. **Tamam'ı**seçin.  
1. Danışman **Proxy Ayar Kuralı'nı**ara. 
1. Operations Manager araç çubuğunda **Geçersiz Kılmalar'ı** seçin ve ardından sınıfın belirli bir nesnesi için Kuralı Geçersiz Kılma'yı işaret **edin: Sistem Durumu Hizmeti** ve listeden bir nesne seçin.  Veya bu geçersiz kılmayı uygulamak istediğiniz sunucuların sistem durumu hizmeti nesnesini içeren özel bir grup oluşturun. Ardından geçersiz kılmayı özel grubunuzun üzerine uygulayın.
1. Geçersiz **Kılma Özellikleri** iletişim kutusunda, **WebProxyAddress** parametresinin yanındaki **Geçersiz Kılma** sütununa bir onay işareti ekleyin.  Geçersiz **Kılma Değeri** alanına, Log Analytics ağ geçidi sunucusunun URL'sini girin. Önek `http://`ile başlamak için dikkatli olun.  

    >[!NOTE]
    > Kuralı etkinleştirmeniz gerekmez. Microsoft System Center Advisor Secure Reference Override yönetim paketinde Microsoft System Center Advisor Monitor MonitorIng Server Group'u hedefleyen geçersiz kılma yla otomatik olarak yönetilir.
    > 

1. **Hedef yönetim paketi** seç listesinden bir yönetim paketi seçin veya **Yeni'yi**seçerek yeni bir mühürsüz yönetim paketi oluşturun. 
1. İşiniz bittiğinde **Tamam**'a tıklayın. 

### <a name="configure-for-automation-hybrid-runbook-workers"></a>Otomasyon Hibrid Runbook İşçileri için yapılandırın

Ortamınızda Otomasyon Karma Runbook Çalışanları varsa, çalışanları desteklemek için ağ geçidini yapılandırmak için aşağıdaki adımları izleyin.

Her bölgenin URL'sini bulmak için Otomasyon belgelerinin [ağ bölümünüzü Yapılandır'a](../../automation/automation-hybrid-runbook-worker.md#network-planning) bakın.

Bilgisayarınız otomatik olarak Karma Runbook İşçisi olarak kayıtlıysa (örneğin, bir veya daha fazla VM için Güncelleştirme Yönetimi çözümü etkinse, aşağıdaki adımları izleyin:

1. İş Zamanı Veri URL'lerini Log Analytics ağ geçidinde İzin Verilen Ana Bilgisayar listesine ekleyin. Örneğin, `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
1. Aşağıdaki PowerShell cmdlet'i kullanarak Log Analytics ağ geçidi hizmetini yeniden başlatın:`Restart-Service OMSGatewayService`

Bilgisayarınız Karma Runbook Worker kayıt cmdlet'ini kullanarak Azure Otomasyonu'na katıldıysa aşağıdaki adımları izleyin:

1. Aracı hizmeti kayıt URL'sini, Log Analytics ağ geçidinde İzin Verilen Ana Bilgisayar listesine ekleyin. Örneğin, `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
1. İş Zamanı Veri URL'lerini Log Analytics ağ geçidinde İzin Verilen Ana Bilgisayar listesine ekleyin. Örneğin, `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
1. Log Analytics ağ geçidi hizmetini yeniden başlatın.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Faydalı PowerShell cmdlets

Log Analytics ağ geçidinin yapılandırma ayarlarını güncelleştirmek için görevleri tamamlamak için cmdlet'ler kullanabilirsiniz. Cmdlets kullanmadan önce şunları unutmayın:

1. Günlük Analitiği ağ geçidini (Microsoft Windows Installer) yükleyin.
1. PowerShell konsol pencereyi açın.
1. Bu komutu yazarak modülü içe aktarın:`Import-Module OMSGateway`
1. Önceki adımda hata oluştuysa, modül başarıyla içe aktarıldı ve cmdlets kullanılabilir. Girin`Get-Module OMSGateway`
1. Değişiklik yapmak için cmdlets kullandıktan sonra, OMS Ağ Geçidi hizmetini yeniden başlatın.

Adım 3'teki bir hata, modülün içeri aktarılamadığı anlamına gelir. PowerShell modülü buladığında hata oluşabilir. Modülü OMS Ağ Geçidi yükleme yolunda bulabilirsiniz: *C:\Program Files\Microsoft OMS Ağ Geçidi\PowerShell\OmsGateway*.

| **Cmdlet** | **Parametreler** | **Açıklama** | **Örnek** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Anahtar |Hizmetin yapılandırmasını alır |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Anahtar (gerekli) <br> Değer |Hizmetin yapılandırmasını değiştirir |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Röle (upstream) proxy adresini alır |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Adres<br> Kullanıcı adı<br> Parola (güvenli dize) |Röle (upstream) proxy adresi (ve kimlik bilgileri) ayarlar |1. Bir röle proxy ve kimlik numarası ayarlayın:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Kimlik doğrulaması gerektirmeyecek bir geçiş proxy'si ayarlayın:`Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Röle proxy ayarını temizleyin:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |Şu anda izin verilen ana bilgisayar (yalnızca yerel olarak yapılandırılan, izin verilen ana bilgisayarları otomatik olarak indirilmeyen ana bilgisayar) alır |`Get-OMSGatewayAllowedHost` | 
| `Add-OMSGatewayAllowedHost` |Ana bilgisayar (gerekli) |Ana bilgisayarı izin verilen listeye ekler |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Ana bilgisayar (gerekli) |Ana bilgisayarı izin verilen listeden kaldırır |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Konu (gerekli) |İstemci sertifikasını izin verilen listeye ekler |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Konu (gerekli) |İstemci sertifikası öznesini izin verilen listeden kaldırır |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Şu anda izin verilen istemci sertifikası öznelerini alır (yalnızca yerel olarak yapılandırılan izin verilen konular, otomatik olarak indirilmeyen izin verilen konular) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Sorun giderme

Ağ geçidi tarafından günlüğe kaydedilen olayları toplamak için Log Analytics aracısını yüklemelisiniz.

![Günlük Analizi ağ geçidi günlüğündeki Olay Görüntüleyicisi listesinin ekran görüntüsü](./media/gateway/event-viewer.png)

### <a name="log-analytics-gateway-event-ids-and-descriptions"></a>Günlük Analytics ağ geçidi olay tanımları ve açıklamaları

Aşağıdaki tabloda, Log Analytics ağ geçidi günlük etkinlikleri için olay tanımları ve açıklamaları gösterilmektedir.

| **Kimliği** | **Açıklama** |
| --- | --- |
| 400 |Belirli bir kimliği olmayan herhangi bir uygulama hatası. |
| 401 |Yanlış yapılandırma. Örneğin, bir toplamcı yerine listenPort = "metin". |
| 402 |TLS el sıkışma iletilerini ayrıştırmada özel durum. |
| 403 |Ağ hatası. Örneğin, hedef sunucuya bağlanamıyor. |
| 100 |Genel bilgiler. |
| 101 |Hizmet başladı. |
| 102 |Hizmet durduruldu. |
| 103 |İstemciden bir HTTP CONNECT komutu alındı. |
| 104 |HTTP CONNECT komutu değil. |
| 105 |Hedef sunucu izin verilenler listesinde değildir veya hedef bağlantı noktası güvenli değildir (443). <br> <br> OMS Ağ Geçidi sunucunuzdaki MMA aracısının ve OMS Ağ Geçidi ile iletişim sağlayan aracıların aynı Log Analytics çalışma alanına bağlı olduğundan emin olun. |
| 105 |HATA TcpConnection – Geçersiz İstemci sertifikası: CN=Ağ Geçidi. <br><br> OMS Ağ Geçidi sürüm 1.0.395.0 veya daha büyük kullandığınızdan emin olun. Ayrıca, OMS Ağ Geçidi sunucunuzdaki MMA aracısının ve OMS Ağ Geçidi ile iletişim kuran aracıların aynı Log Analytics çalışma alanına bağlı olduğundan emin olun. |
| 106 |Desteklenmeyen TLS/SSL protokolü sürümü.<br><br> Log Analytics ağ geçidi yalnızca TLS 1.0, TLS 1.1 ve 1.2'yi destekler. SSL'yi desteklemez.|
| 107 |TLS oturumu doğrulandı. |

### <a name="performance-counters-to-collect"></a>Toplanacak performans sayaçları

Aşağıdaki tablo, Log Analytics ağ geçidi için kullanılabilen performans sayaçlarını gösterir. Sayaçları eklemek için Performans Monitörü'nu kullanın.

| **Adı** | **Açıklama** |
| --- | --- |
| Günlük Analitik Ağ Geçidi/Etkin İstemci Bağlantısı |Etkin istemci ağı (TCP) bağlantı sayısı |
| Günlük Analizi Ağ Geçidi/Hata Sayısı |Hata sayısı |
| Günlük Analitik Ağ Geçidi/Bağlı İstemci |Bağlı istemci sayısı |
| Günlük Analitik Ağ Geçidi/Reddi Sayısı |TLS doğrulama hatası nedeniyle reddedilenlerin sayısı |

![Performans sayaçlarını gösteren Log Analytics ağ geçidi arabiriminin ekran görüntüsü](./media/gateway/counters.png)

## <a name="assistance"></a>Yardım

Azure portalında oturum açtığınızda, Log Analytics ağ geçidi veya başka bir Azure hizmeti veya özelliği hakkında yardım alabilirsiniz.
Yardım almak için, portalın sağ üst köşesindeki soru işareti simgesini seçin ve **Yeni destek isteği'ni**seçin. Ardından yeni destek istek formunu doldurun.

![Yeni bir destek isteğinin ekran görüntüsü](./media/gateway/support.png)

## <a name="next-steps"></a>Sonraki adımlar

Bağlı kaynaklardan veri toplamak için [veri kaynakları ekleyin](../../azure-monitor/platform/agent-data-sources.md) ve verileri Log Analytics çalışma alanınızda depolayın.
