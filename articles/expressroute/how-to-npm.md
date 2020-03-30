---
title: "Azure ExpressRoute: Devreler için NPM'yi yapılandır"
description: Azure ExpressRoute devreleri için bulut tabanlı ağ izlemeyi (NPM) yapılandırın. Bu, ExpressRoute özel eşleme ve Microsoft'un bakışları üzerinden izlemeyi kapsar.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 01/25/2019
ms.author: cherylmc
ms.openlocfilehash: 54fa3dcbfbbcb3153f81407a9bc9b52511405390
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74076601"
---
# <a name="configure-network-performance-monitor-for-expressroute"></a>ExpressRoute için Ağ Performansı İzleyicisi’ni Yapılandırma

Bu makale, ExpressRoute'u izlemek için bir Ağ Performans İzleyicisi uzantısı yapılandırmanıza yardımcı olur. Ağ Performansı İzleyicisi (NPM), Azure bulut dağıtımları ile şirket içi konumlar (Şube ofisleri vb.) arasındaki bağlantıyı izleyen, bulut tabanlı bir ağ izleme çözümüdür. NPM, Azure İzleyici günlüklerinin bir parçasıdır. NPM, ExpressRoute için özel eşleme veya Microsoft eşlemesi kullanmak üzere yapılandırılmış ExpressRoute bağlantı hatları üstünde ağ performansını izlemenizi sağlayan bir uzantı sunar. NPM’yi ExpressRoute için yapılandırdığınızda, ağ sorunlarını algılayabilir, tanımlayabilir ve giderebilirsiniz. Bu hizmet Azure Kamu Bulutu’nda da kullanılabilir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Şunları yapabilirsiniz:

* Çeşitli VNet'lerde kaybı ve gecikmeyi izleyin ve uyarıları ayarlayın

* Ağdaki tüm yolları (gereksiz yollar dahil) izleme

* Çoğaltılması zor olan geçici ve zaman içinde nokta ağ sorunlarını giderme

* Azalan performansla sorumlu ağdaki belirli bir kesimi belirlemeye yardımcı olun

* Sanal ağ başına iş başı elde edin (Her VNet'te aracılar yüklüyse)

* ExpressRoute sistem durumunu önceki bir noktadan görme

## <a name="workflow"></a><a name="workflow"></a>Iş akışı

İzleme aracıları, hem şirket içinde hem de Azure'da birden çok sunucuda yüklenir. Aracılar birbirleriyle iletişim kurar, ancak veri göndermez, TCP el sıkışma paketleri gönderirler. Aracılar arasındaki iletişim, Azure'un ağ topolojisini ve trafiğin yol açabileceğini haritalamasına olanak tanır.

1. Bir NPM Çalışma Alanı oluşturun. Bu, Log Analytics çalışma alanıyla aynıdır.
2. Yazılım aracılarını yükleyin ve yapılandırır. (Yalnızca Microsoft Peering üzerinden izlemek istiyorsanız, yazılım aracılarını yüklemeniz ve yapılandırmanız gerekmez.): 
    * İzleme aracılarını şirket içi sunucularda ve Azure VM'lere (özel izleme için) yükleyin.
    * İzleme aracısı sunucularında ayarları, izleme aracısının iletişim kurmasına izin verecek şekilde yapılandırın. (Güvenlik duvarı bağlantı noktalarını aç, vb.)
3. Ağ güvenlik grubu (NSG) kurallarını, Azure VM'lerine yüklenen izleme aracısının şirket içi izleme aracılarıyla iletişim kurmasına izin verecek şekilde yapılandırın.
4. İzlemeyi ayarlama: NPM'de hangi ağların görünür olduğunu otomatik olarak keşfedin ve yönetin.

Diğer nesneleri veya hizmetleri izlemek için zaten Ağ Performans İzleyicisi kullanıyorsanız ve desteklenen bölgelerden birinde zaten Çalışma Alanı'nız varsa, Adım 1 ve Adım 2'yi atlayabilir ve yapılandırmanızı Adım 3 ile başlatabilirsiniz.

## <a name="step-1-create-a-workspace"></a><a name="configure"></a>Adım 1: Çalışma Alanı Oluşturma

Abonelikte ExpressRoute devresine(ler) VNets bağlantısı olan bir çalışma alanı oluşturun.

1. Azure [portalında,](https://portal.azure.com)ExpressRoute devrenize bakan VNET'lere sahip Abonelik'i seçin. Ardından, 'Ağ Performans Monitörü' için **Market'teki** hizmetlerin listesini arayın. Dönüşte, Ağ Performans **İzleyicisi** sayfasını açmak için tıklatın.

   >[!NOTE]
   >Yeni bir çalışma alanı oluşturabilir veya varolan bir çalışma alanı kullanabilirsiniz. Varolan bir çalışma alanı kullanmak istiyorsanız, çalışma alanının yeni sorgu diline geçirilmiş olduğundan emin olmalısınız. [Daha fazla bilgi...](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-search-upgrade)
   >

   ![portal](./media/how-to-npm/3.png)<br><br>
2. Ana Ağ Performans **İzleyicisi** sayfasının alt kısmında, Ağ Performans Monitörü'nu açmak için **Oluştur** 'u tıklatın - Yeni çözüm sayfası **oluşturun.** **Günlük Analizi Çalışma Alanı 'nı** tıklatın - Çalışma Alanları sayfasını açmak için bir çalışma alanı seçin. Çalışma Alanı sayfasını açmak için **+ Yeni Çalışma Alanı Oluştur'u** tıklatın.
3. Günlük **Analizi çalışma alanı** sayfasında Yeni **Oluştur'u**seçin ve ardından aşağıdaki ayarları yapılandırın:

   * Günlük Analitiği Çalışma Alanı - Çalışma alanınız için bir ad yazın.
   * Abonelik - Birden çok aboneliğiniz varsa, yeni Çalışma Alanı ile ilişkilendirmek istediğiniz aboneliği seçin.
   * Kaynak grubu - Kaynak grubu oluşturun veya varolan bir grup kullanın.
   * Konum - Bu konum, aracı bağlantı günlükleri için kullanılan depolama hesabının konumunu belirtmek için kullanılır.
   * Fiyatlandırma katmanı - Fiyatlandırma katmanını seçin.
  
     >[!NOTE]
     >ExpressRoute devresi dünyanın herhangi bir yerinde olabilir. Çalışma Alanı ile aynı bölgede olmak zorunda değildir.
     >
  
     ![çalışma alanı](./media/how-to-npm/4.png)<br><br>
4. Ayarlar şablonu kaydetmek ve dağıtmak için **Tamam'ı** tıklatın. Şablon doğrulandıktan sonra Çalışma Alanını dağıtmak için **Oluştur'u** tıklatın.
5. Çalışma alanı dağıtıldıktan sonra, oluşturduğunuz **NetworkMonitoring(ad)** kaynağına gidin. Ayarları doğrulayın, ardından **Çözüm'e**tıklayın ek yapılandırma gerektirir.

   ![ek yapılandırma](./media/how-to-npm/5.png)

## <a name="step-2-install-and-configure-agents"></a><a name="agents"></a>Adım 2: Aracıları yükleme ve yapılandırma

### <a name="21-download-the-agent-setup-file"></a><a name="download"></a>2.1: Aracı kurulum dosyasını indirin

1. Kaynağınız için **Ağ Performans İzleyicisi Yapılandırma** sayfasının Ortak **Ayarlar** sekmesine gidin. **Log Analytics Agents** bölümünü yükleyin ve kurulum dosyasını indirin.
2. Ardından, **Çalışma Alanı Kimliğini** ve Birincil **Anahtarı** Not Defteri'ne kopyalayın.
3. TCP protokolü bölümünü **kullanarak izleme için Yapılandırılan Log Analytics Aracılarından** Powershell Script'i indirin. PowerShell komut dosyası, TCP işlemleri için ilgili güvenlik duvarı bağlantı noktasını açmanıza yardımcı olur.

   ![PowerShell betiği](./media/how-to-npm/7.png)

### <a name="22-install-a-monitoring-agent-on-each-monitoring-server-on-each-vnet-that-you-want-to-monitor"></a><a name="installagent"></a>2.2: Her izleme sunucusuna bir izleme aracısı yükleyin (izlemek istediğiniz her VNET'e)

Artıklık için ExpressRoute bağlantısının her iki tarafına en az iki aracı yüklemenizi öneririz (örneğin, şirket içi Azure VNET'ler). Aracı nın bir Windows Server'a (2008 SP1 veya sonrası) yüklenmesi gerekir. Windows Desktop OS ve Linux OS kullanarak ExpressRoute devrelerinin izlenmesi desteklenmez. Aracıları yüklemek için aşağıdaki adımları kullanın:
   
  >[!NOTE]
  >SCOM tarafından itilen aracılar [(MMA](https://technet.microsoft.com/library/dn465154(v=sc.12).aspx)dahil) Azure'da barındırılan aracılar konumlarını sürekli olarak algılayamayabilir. ExpressRoute'u izlemek için bu aracıları Azure VNETs'te kullanmamanızı öneririz.
  >

1. ExpressRoute'u izlemek için kullanmak istediğiniz her sunucuya aracıyı yüklemek için **Kurulum'u** çalıştırın. İzleme için kullandığınız sunucu vm veya şirket içi olabilir ve Internet erişimine sahip olmalıdır. Azure'da izlemek istediğiniz her ağ segmentine şirket içinde en az bir aracı ve bir aracı yüklemeniz gerekir.
2. **Hoş Geldiniz** sayfasında **İleri**'ye tıklayın.
3. Lisans **Koşulları** sayfasında, lisansı okuyun ve sonra **Kabul Ediyorum'u**tıklatın.
4. Hedef **Klasör** sayfasında varsayılan yükleme klasörünü değiştirin veya tutun ve sonra **İleri'yi**tıklatın.
5. Aracı **Kurulum Seçenekleri** sayfasında aracıyı Azure Monitor günlüklerine veya Operasyon Yöneticisi'ne bağlamayı seçebilirsiniz. Veya aracıyı daha sonra yapılandırmak istiyorsanız seçenekleri boş bırakabilirsiniz. Seçiminizi yaptıktan sonra **İleri'yi**tıklatın.

   * **Azure Log Analytics'e**bağlanmayı seçtiyseniz, önceki bölümde Notepad'e kopyaladığınız **Çalışma Alanı Kimliği** ve Çalışma **Alanı Anahtarını** (Birincil Anahtar) yapıştırın. Ardından **İleri'yi**tıklatın.

     ![Kimlik ve Anahtar](./media/how-to-npm/8.png)
   * **Yönetim Grubu Yapılandırma** sayfasında Operasyon **Yöneticisi'ne**bağlanmayı seçtiyseniz, Yönetim **Grubu Adı,** **Yönetim Sunucusu**ve Yönetim Sunucusu **Bağlantı Noktası'nı**yazın. Ardından **İleri'yi**tıklatın.

     ![Operations Manager](./media/how-to-npm/9.png)
   * Aracı **Eylem Hesabı** sayfasında, **Yerel Sistem** hesabını veya Etki Alanı veya Yerel **Bilgisayar Hesabı'nı**seçin. Ardından **İleri'yi**tıklatın.

     ![Hesap](./media/how-to-npm/10.png)
6. **Yüklemeye Hazır** sayfasında, seçimlerinizi gözden geçirin ve sonra **Yükle'yi**tıklatın.
7. **Yapılandırma başarıyla tamamlandı** sayfasında **Son**'a tıklayın.
8. Tamamlandığında, Microsoft İzleme Aracısı Denetim Masası'nda görünür. Yapılandırmanızı buradan inceleyebilir ve aracının Azure Monitor günlüklerine bağlı olduğunu doğrulayabilirsiniz. Bağlı olduğunuzda, aracı şu belirten bir ileti görüntüler: **Microsoft İzleme Aracısı Microsoft Operations Management Suite hizmetine başarıyla bağlanmıştır.**

9. İzlenmeniz gereken her VNET için bu yordamı tekrarlayın.

### <a name="23-configure-proxy-settings-optional"></a><a name="proxy"></a>2.3: Proxy ayarlarını yapılandırma (isteğe bağlı)

Internet'e erişmek için bir web proxy kullanıyorsanız, Microsoft İzleme Aracısı için proxy ayarlarını yapılandırmak için aşağıdaki adımları kullanın. Her sunucu için bu adımları gerçekleştirin. Yapılandırmanız gereken birden çok sunucu olması durumunda, bu işlemi otomatikleştirmek için bir betik kullanmak sizin için daha kolay olabilir. Bu ysa, [bkz.](../log-analytics/log-analytics-windows-agent.md)

Denetim Masası'nı kullanarak Microsoft İzleme Aracısı için proxy ayarlarını yapılandırmak için:

1. Kontrol **Panelini**açın.
2. **Microsoft İzleme Aracısı**'nı açın.
3. **Ara Sunucu Ayarları** sekmesine tıklayın.
4. **Proxy sunucusu kullan'ı** seçin ve gerekirse URL ve bağlantı noktası numarasını yazın. Ara sunucunuz kimlik doğrulaması gerektiriyorsa ara sunucuya erişmek için kullanıcı adını ve parolayı yazın.

   ![proxy](./media/how-to-npm/11.png)

### <a name="24-verify-agent-connectivity"></a><a name="verifyagent"></a>2.4: Aracı bağlantısını doğrulayın

Aracılarınızın iletişim kurup kurmadığını kolayca doğrulayabilirsiniz.

1. İzleme aracısı olan bir sunucuda **Denetim Paneli'ni**açın.
2. Microsoft **İzleme Aracısını**açın.
3. Azure **Günlük Analizi** sekmesini tıklatın.
4. **Durum** sütununda, aracının Azure Monitor günlüklerine başarıyla bağladığını görmeniz gerekir.

   ![durum](./media/how-to-npm/12.png)

### <a name="25-open-the-firewall-ports-on-the-monitoring-agent-servers"></a><a name="firewall"></a>2.5: İzleme aracısı sunucularında güvenlik duvarı bağlantı noktalarını açın

TCP protokolünü kullanmak için, izleme aracıları iletişim kurabilir emin olmak için güvenlik duvarı bağlantı noktalarını açmanız gerekir.

Ağ Performans İzleyicisi tarafından gerekli olan kayıt defteri anahtarlarını oluşturmak için bir PowerShell komut dosyası çalıştırabilirsiniz. Bu komut dosyası, izleme aracılarının birbiriyle TCP bağlantıları oluşturmasına izin vermek için Windows Güvenlik Duvarı kurallarını da oluşturur. Komut dosyası tarafından oluşturulan kayıt defteri anahtarları, hata ayıklama günlüklerini ve günlükler dosyasının yolunu günlüğe kaydedip günlüğe kaydetmemeyi belirtir. Ayrıca iletişim için kullanılan tcp bağlantı noktasını tanımlar. Bu anahtarların değerleri komut dosyası tarafından otomatik olarak ayarlanır. Bu anahtarları el ile değiştirmemelisiniz.

Bağlantı noktası 8084 varsayılan olarak açılır. Komut dosyasına 'portNumber' parametresini sağlayarak özel bir bağlantı noktası kullanabilirsiniz. Ancak, bunu yaparsanız, komut dosyasını çalıştırdığınız tüm sunucular için aynı bağlantı noktasını belirtmeniz gerekir.

>[!NOTE]
>'EnableRules' PowerShell komut dosyası, Windows Güvenlik Duvarı kurallarını yalnızca komut dosyasının çalıştırıldığı sunucuda yapılandırır. Bir ağ güvenlik duvarınız varsa, Ağ Performans İzleyicisi tarafından kullanılan TCP bağlantı noktası için mukadder olan trafiğe izin verdiğinden emin olmalısınız.
>
>

Aracı sunucularında, yönetim ayrıcalıklarına sahip bir PowerShell penceresi açın. [EnableRules](https://aka.ms/npmpowershellscript) PowerShell komut dosyasını çalıştırın (daha önce indirdiğiniz). Herhangi bir parametre kullanmayın.

![PowerShell_Script](./media/how-to-npm/script.png)

## <a name="step-3-configure-network-security-group-rules"></a><a name="opennsg"></a>Adım 3: Ağ güvenlik grubu kurallarını yapılandırma

Azure'da bulunan aracı sunucularını izlemek için, npm tarafından sentetik işlemler için kullanılan bir bağlantı noktasında TCP trafiğine izin verecek şekilde ağ güvenlik grubu (NSG) kurallarını yapılandırmanız gerekir. Varsayılan bağlantı noktası 8084'dür. Bu, Azure VM'ye yüklü bir izleme aracının şirket içi bir izleme aracısıyla iletişim kurmasına olanak tanır.

NSG hakkında daha fazla bilgi için [Ağ Güvenlik Grupları'na](../virtual-network/virtual-networks-create-nsg-arm-portal.md)bakın.

>[!NOTE]
>Aracıları (şirket içi sunucu aracısı ve Azure sunucu aracısı) yüklediğinizden ve bu adıma geçmeden önce PowerShell komut dosyasını çalıştırdığınızdan emin olun.
>

## <a name="step-4-discover-peering-connections"></a><a name="setupmonitor"></a>Adım 4: Eşleme bağlantılarını keşfedin

1. **Tüm Kaynaklar** sayfasına giderek Ağ Performans İzleme Denetimi'ne genel bakış döşemesine gidin ve ardından beyaz listedeki NPM Çalışma Alanı'nı tıklatın.

   ![npm çalışma alanı](./media/how-to-npm/npm.png)
2. Panoyu açmak için **Ağ Performans Monitörü** genel ayartmasını tıklatın. Pano, ExpressRoute'un 'yapılandırılmamış durumda' olduğunu gösteren bir ExpressRoute sayfası içerir. Ağ Performans Monitörü yapılandırma sayfasını açmak için **Özellik Kurulumu'nu** tıklatın.

   ![özellik kurulumu](./media/how-to-npm/npm2.png)
3. Yapılandırma sayfasında, sol taraftaki panelde bulunan 'ExpressRoute Peerings' sekmesine gidin. Ardından, **Şimdi Keşfet'i**tıklatın.

   ![Keşfetmek](./media/how-to-npm/13.png)
4. Bulma tamamlandığında, aşağıdaki öğeleri içeren bir liste görürsünüz:
   * ExpressRoute devresindeki(ler) bu abonelikle ilişkili tüm Microsoft bakan bağlantıları.
   * Bu abonelikle ilişkili VNets'e bağlanan tüm özel bakan bağlantıları.
            
## <a name="step-5-configure-monitors"></a><a name="configmonitor"></a>Adım 5: Monitörleri yapılandır

Bu bölümde, monitörleri yapılandırırsınız. İzlemek istediğiniz eşleme türü için adımları izleyin: **özel akranlama**veya **Microsoft eşleme.**

### <a name="private-peering"></a>Özel eşleme

Özel görünüm için, keşif tamamlandığında, benzersiz Devre **Adı** ve **VNet Adı**için will kuralları bakın. Başlangıçta, bu kurallar devre dışı bırakılır.

![rules](./media/how-to-npm/14.png)

1. Bu **bakan** onay kutusunu Monitör'ü kontrol edin.
2. **Bu eşleme için Sistem Durumu İzlemeyi Etkinleştir**onay kutusunu seçin.
3. İzleme koşullarını seçin. Eşik değerlerini yazarak sistem durumu olayları oluşturmak için özel eşikler ayarlayabilirsiniz. Koşulun değeri seçili ağ/alt ağ çifti için seçili eşiğinin üzerine çıktığında, bir sistem durumu olayı oluşturulur.
4. Özel akran bağlantısını izlemek istediğiniz şirket içi sunucuları eklemek için ON-PREM AGENTS **Add Agents** düğmesini tıklatın. Yalnızca Adım 2 bölümünde belirttiğiniz Microsoft hizmet bitiş noktasına bağlantısı olan aracıları seçtiğinizden emin olun. Şirket içi aracılar ExpressRoute bağlantısını kullanarak bitiş noktasına ulaşabilmeli.
5. Ayarları kaydedin.
6. Kuralları etkinleştirdikten ve izlemek istediğiniz değerleri ve aracıları seçtikten sonra, değerlerin doldurulmaya başlaması ve **ExpressRoute İzleme** kutucuklarının kullanılabilir hale gelmesi için yaklaşık 30-60 dakika bekleyin.

### <a name="microsoft-peering"></a>Microsoft eşlemesi

Microsoft'un görüntülenmesi için, izlemek istediğiniz Microsoft peering connection(lar)'ı tıklatın ve ayarları yapılandırın.

1. Bu **bakan** onay kutusunu Monitör'ü kontrol edin. 
2. (İsteğe bağlı) Hedef Microsoft hizmet bitiş noktasını değiştirebilirsiniz. Varsayılan olarak, NPM hedef olarak bir Microsoft hizmet bitiş noktası seçer. NPM, şirket içi sunucularınızdan ExpressRoute aracılığıyla bu hedef uç noktasına bağlantı izler. 
    * Bu hedef bitiş noktasını değiştirmek için Hedef altındaki **(edit)** bağlantısını tıklatın **ve**URL'ler listesinden başka bir Microsoft hizmeti hedef bitiş noktası seçin.
      ![hedefi edin](./media/how-to-npm/edit_target.png)<br>

    * Özel bir URL veya IP Adresi kullanabilirsiniz. Azure Depolama, SQL veritabanları ve genel IP adreslerinde sunulan Web siteleri gibi Azure PaaS hizmetlerine bağlantı kurmak için Microsoft'u kullanıyorsanız, bu seçenek özellikle alakalıdır. Bunu yapmak için URL listesinin altındaki bağlantıyı **(Bunun yerine özel URL veya IP Adresi kullanın)** tıklatın ve ardından ExpressRoute Microsoft eşleme seçimleri yoluyla bağlantılı Azure PaaS hizmetinizin ortak bitiş noktasını girin.
    ![özel URL](./media/how-to-npm/custom_url.png)<br>

    * Bu isteğe bağlı ayarları kullanıyorsanız, burada yalnızca Microsoft hizmet bitiş noktasının seçildiğinden emin olun. Bitiş noktası ExpressRoute'a bağlı olmalı ve şirket içi aracılar tarafından erişilebilmelidir.
3. **Bu eşleme için Sistem Durumu İzlemeyi Etkinleştir**onay kutusunu seçin.
4. İzleme koşullarını seçin. Eşik değerlerini yazarak sistem durumu olayları oluşturmak için özel eşikler ayarlayabilirsiniz. Koşulun değeri seçili ağ/alt ağ çifti için seçili eşiğinin üzerine çıktığında, bir sistem durumu olayı oluşturulur.
5. Microsoft peering bağlantısını izlemek istediğiniz şirket içi sunucuları eklemek için ON-PREM ARACıLARI **Ekle** düğmesini tıklatın. Yalnızca Adım 2 bölümünde belirttiğiniz Microsoft hizmet uç noktalarına bağlantısı olan aracıları seçtiğinizden emin olun. Şirket içi aracılar ExpressRoute bağlantısını kullanarak bitiş noktasına ulaşabilmeli.
6. Ayarları kaydedin.
7. Kuralları etkinleştirdikten ve izlemek istediğiniz değerleri ve aracıları seçtikten sonra, değerlerin doldurulmaya başlaması ve **ExpressRoute İzleme** kutucuklarının kullanılabilir hale gelmesi için yaklaşık 30-60 dakika bekleyin.

## <a name="step-6-view-monitoring-tiles"></a><a name="explore"></a>Adım 6: İzleme kutucuklarını görüntüleme

İzleme kutularını gördüğünüzde, ExpressRoute devreleriniz ve bağlantı kaynaklarınız NPM tarafından izlenir. Microsoft Peering bağlantılarının sistem durumunu ayrıntılı olarak gözden çıkarmak için Microsoft Peering döşemesini tıklatabilirsiniz.

![izleme karoları](./media/how-to-npm/15.png)

### <a name="network-performance-monitor-page"></a><a name="dashboard"></a>Ağ Performans Monitörü sayfası

NPM sayfası, ExpressRoute devrelerinin ve bakışlarının sağlığına genel bir bakış gösteren ExpressRoute için bir sayfa içerir.

![Pano](./media/how-to-npm/dashboard.png)

### <a name="list-of-circuits"></a><a name="circuits"></a>Devreler listesi

İzlenen tüm ExpressRoute devrelerinin listesini görüntülemek için **ExpressRoute devreleri** döşemesini tıklatın. Bir devre seçebilir ve sistem durumu durumunu, paket kaybı, bant genişliği kullanımı ve gecikme sonu için eğilim grafiklerini görüntüleyebilirsiniz. Grafikler etkileşimli. Grafikleri çizmek için özel bir zaman penceresi seçebilirsiniz. Fareyi, yakınlaştırmak ve ince taneli veri noktalarını görmek için grafikteki bir alanın üzerine sürükleyebilirsiniz.

![circuit_list](./media/how-to-npm/circuits.png)

#### <a name="trend-of-loss-latency-and-throughput"></a><a name="trend"></a>Kayıp Eğilimi, Gecikme ve İş İlerletme

Bant genişliği, gecikme ve kayıp grafikleri etkileşimlidir. Fare denetimlerini kullanarak bu grafiklerin herhangi bir bölümüne yakınlaştırabilirsiniz. Sol üstteki Eylemler düğmesinin altında bulunan **Tarih/Saat'i**tıklatarak diğer aralıklar için bant genişliği, gecikme süresi ve kayıp verilerini de görebilirsiniz.

![Eğilim](./media/how-to-npm/16.png)

### <a name="peerings-list"></a><a name="peerings"></a>Eşlemeler listesi

Özel görünüm üzerinden sanal ağlara yapılan tüm bağlantıların listesini görüntülemek için panodaki **Özel Görünümler** döşemesini tıklatın. Burada, sanal ağ bağlantısı seçebilir ve sistem durumu durumunu, paket kaybı, bant genişliği kullanımı ve gecikme sonu için eğilim grafiklerini görüntüleyebilirsiniz.

![devre listesi](./media/how-to-npm/peerings.png)

### <a name="nodes-view"></a><a name="nodes"></a>Düğümler görünümü

Seçilen ExpressRoute eşleme bağlantısı için şirket içi düğümler ile Azure VM/Microsoft hizmet bitiş noktaları arasındaki tüm bağlantıların listesini görüntülemek için **düğüm bağlantılarını görüntüle'yi**tıklatın. Her bağlantının sistem durumu durumunu ve bunlarla ilişkili kayıp ve gecikme eğilimini görüntüleyebilirsiniz.

![düğümgörünümü](./media/how-to-npm/nodes.png)

### <a name="circuit-topology"></a><a name="topology"></a>Devre topolojisi

Devre topolojisini görüntülemek için **Topoloji** karosu'nu tıklatın. Bu seçili devre veya bakan topoloji görünümüne götürür. Topoloji diyagramı ağdaki her kesim için gecikme yi sağlar. Her katman 3 atlama diyagramın bir düğüm ile temsil edilir. Bir atlama tıklayarak atlama hakkında daha fazla bilgi ortaya koymaktadır.

Kaydırıcı çubuğunu **Filtreler'in**altına taşıyarak şirket içi atlamaları içerecek şekilde görünürlük düzeyini artırabilirsiniz. Kaydırıcı çubuğunu sola veya sağa taşımak, topoloji grafiğindeki atlama sayısını artırır/azaltır. Her segmentteki gecikme, ağınızdaki yüksek gecikmeli kesimlerin daha hızlı yalıtılmış olarak yalıtMasına olanak tanır.

![filtreler](./media/how-to-npm/topology.png)

#### <a name="detailed-topology-view-of-a-circuit"></a>Bir devrenin ayrıntılı Topoloji görünümü

Bu görünüm VNet bağlantılarını gösterir.
![ayrıntılı topoloji](./media/how-to-npm/17.png)
