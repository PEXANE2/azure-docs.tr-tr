---
title: Azure Site Kurtarma'yı kullanan çok katmanlı bir SharePoint uygulaması için olağanüstü durum kurtarma
description: Bu makalede, Azure Site Kurtarma özelliklerini kullanarak çok katmanlı bir SharePoint uygulaması için olağanüstü durum kurtarma nın nasıl ayarlanır.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 6/27/2019
ms.author: sutalasi
ms.openlocfilehash: d74e28ce470c23bbc8ee2081532a198c260ccea5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74706375"
---
# <a name="set-up-disaster-recovery-for-a-multi-tier-sharepoint-application-for-disaster-recovery-using-azure-site-recovery"></a>Azure Site Kurtarma'yı kullanarak olağanüstü durum kurtarma için çok katmanlı bir SharePoint uygulaması için olağanüstü durum kurtarma yı ayarlama

Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md)kullanarak bir SharePoint uygulamasının nasıl korunulacak ayrıntılı olarak açıklanmaktadır.


## <a name="overview"></a>Genel Bakış

Microsoft SharePoint, bir grubun veya departmanın bilgileri düzenlemesine, işbirliği yapmasına ve paylaşmaya yardımcı olabilecek güçlü bir uygulamadır. SharePoint intranet portalları, belge ve dosya yönetimi, işbirliği, sosyal ağlar, extranet'ler, web siteleri, kurumsal arama ve iş zekası sağlayabilir. Ayrıca sistem entegrasyonu, süreç entegrasyonu ve iş akışı otomasyonu yetenekleri ne de vardır. Genellikle, kuruluşlar bunu kapalı kalma süresine ve veri kaybına duyarlı bir Tier-1 uygulaması olarak kabul esastır.

Bugün, Microsoft SharePoint herhangi bir out-of-the-box olağanüstü durum kurtarma yetenekleri sağlamaz. Bir felaketin türü ve ölçeğine bakılmaksızın, kurtarma, çiftliği kurtarabileceğiniz bir bekleme veri merkezinin kullanımını içerir. Bekleme veri merkezleri, yerel gereksiz sistemlerin ve yedeklemelerin birincil veri merkezindeki kesintiden kurtarılamayacağı senaryolar için gereklidir.

İyi bir olağanüstü durum kurtarma çözümü, SharePoint gibi karmaşık uygulama mimarileri etrafında kurtarma planlarının modelalınmasına izin vermelidir. Ayrıca, çeşitli katmanlar arasında uygulama eşlemeleri işlemek ve dolayısıyla bir felaket durumunda daha düşük bir RTO ile tek tıklamayla başarısız olma sağlamak için özelleştirilmiş adımlar ekleme yeteneğine sahip olmalıdır.

Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md)kullanarak bir SharePoint uygulamasının nasıl korunulacak ayrıntılı olarak açıklanmaktadır. Bu makalede, üç katmanlı bir SharePoint uygulamasını Azure'a çoğaltmak için en iyi uygulamalar, olağanüstü durum kurtarma demlemesi nasıl yapabileceğiniz ve azure'a uygulama üzerinde nasıl başarısız olabileceğiniz yer alacaktır.

Azure'da çok katmanlı bir uygulamayı kurtarma yla ilgili aşağıdaki videoyu izleyebilirsiniz.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/Disaster-Recovery-of-load-balanced-multi-tier-applications-using-Azure-Site-Recovery/player]


## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakileri anladığınızdan emin olun:

1. [Sanal makineyi Azure'a çoğaltma](site-recovery-vmware-to-azure.md)
2. Kurtarma [ağı](site-recovery-network-design.md) nasıl tasarlar?
3. [Azure'da test başarısız olması](site-recovery-test-failover-to-azure.md)
4. [Azure'da başarısız olmak](site-recovery-failover.md)
5. Etki [alanı denetleyicisi nasıl çoğaltılır?](site-recovery-active-directory.md)
6. SQL Server nasıl [çoğaltılır](site-recovery-sql.md)

## <a name="sharepoint-architecture"></a>SharePoint mimarisi

SharePoint, belirli amaç ve hedeflere uygun bir çiftlik tasarımı uygulamak için katmanlı topolojiler ve sunucu rolleri kullanılarak bir veya daha fazla sunucuda dağıtılabilir. Çok sayıda eşzamanlı kullanıcıyı destekleyen tipik bir büyük, yüksek talep alan SharePoint sunucu çiftliği ve çok sayıda içerik öğesi ölçeklenebilirlik stratejilerinin bir parçası olarak hizmet gruplandırmayı kullanır. Bu yaklaşım, özel sunucularda hizmetleri çalıştırmayı, bu hizmetleri birlikte gruplandırmayı ve sunucuları grup olarak ölçekletmeyi içerir. Aşağıdaki topoloji, üç katmanlı SharePoint sunucu çiftliğiiçin hizmet ve sunucu gruplandırmasını göstermektedir. Farklı SharePoint topolojileri hakkında ayrıntılı rehberlik için lütfen SharePoint belgelerine ve ürün satırı mimarilerine bakın. [Bu belgede](https://technet.microsoft.com/library/cc303422.aspx)SharePoint 2013 dağıtımı hakkında daha fazla bilgi bulabilirsiniz.



![Dağıtım Deseni 1](./media/site-recovery-sharepoint/sharepointarch.png)


## <a name="site-recovery-support"></a>Site Recovery desteği

Site Kurtarma uygulama-agnostik ve desteklenen bir makinede çalışan SharePoint herhangi bir sürümü ile çalışması gerekir. Bu makaleyi oluşturmak için, Windows Server 2012 R2 Enterprise ile VMware sanal makineler kullanılmıştır. SharePoint 2013 Enterprise edition ve SQL server 2014 Enterprise sürümü kullanılmıştır.

### <a name="source-and-target"></a>Kaynak ve hedef

**Senaryo** | **İkincil siteye** | **Azure'a**
--- | --- | ---
**Hyper-V** | Evet | Evet
**VMware** | Evet | Evet
**Fiziksel sunucu** | Evet | Evet
**Azure** | NA | Evet


### <a name="things-to-keep-in-mind"></a>Akılda tutulması gereken noktalar

Uygulamanızda herhangi bir katman olarak paylaşılan disk tabanlı küme kullanıyorsanız, bu sanal makineleri çoğaltmak için Site Kurtarma çoğaltmasını kullanamazsınız. Uygulama tarafından sağlanan yerel çoğaltma yı kullanabilirsiniz ve ardından tüm katmanları n için başarısız olmak için bir [kurtarma planı](site-recovery-create-recovery-plans.md) kullanabilirsiniz.

## <a name="replicating-virtual-machines"></a>Sanal makineleri çoğaltma

Sanal makineyi Azure'a çoğaltmaya başlamak için [bu kılavuzu](site-recovery-vmware-to-azure.md) izleyin.

* Çoğaltma tamamlandıktan sonra, her katmandaki her sanal makineye gittiğinizden ve 'Çoğaltılan öğe > Ayarlar > > Özellikler > Bilgi İşlem ve Ağ' kümesinde aynı kullanılabilirliği seçtiğinizden emin olun. Örneğin, web katmanınızda 3 VM varsa, tüm 3 VM'lerin Azure'da ayarlanan aynı kullanılabilirliğin bir parçası olacak şekilde yapılandırıldığından emin olun.

    ![Set-Kullanılabilirlik-Set](./media/site-recovery-sharepoint/select-av-set.png)

* Active Directory ve DNS'nin korunması yla ilgili rehberlik için [Active Directory ve DNS belgesini koru'ya](site-recovery-active-directory.md) bakın.

* SQL sunucusunda çalışan veritabanı katmanını koruma kılavuzu için SQL Server belgesini [Koru'ya](site-recovery-sql.md) bakın.

## <a name="networking-configuration"></a>Ağ yapılandırması

### <a name="network-properties"></a>Ağ özellikleri

* Uygulama ve Web katmanı VM'leri için, Azure portalındaki ağ ayarlarını, vm'lerin başarısız olduktan sonra doğru DR ağına bağlanması için yapılandırın.

    ![Ağ Seçin](./media/site-recovery-sharepoint/select-network.png)


* Statik bir IP kullanıyorsanız, sanal makinenin **Hedef IP** alanında almasını istediğiniz IP'yi belirtin

    ![Statik IP'yi ayarla](./media/site-recovery-sharepoint/set-static-ip.png)

### <a name="dns-and-traffic-routing"></a>DNS ve Trafik Yönlendirme

İnternete bakan siteler için Azure aboneliğinde ['Öncelik' türünden bir Trafik Yöneticisi profili oluşturun.](../traffic-manager/traffic-manager-create-profile.md) Ve ardından DNS ve Trafik Yöneticisi profilinizi aşağıdaki şekilde yapılandırın.


| **Nerede** | **Kaynak** | **Hedef**|
| --- | --- | --- |
| Genel DNS | SharePoint siteleri için genel DNS <br/><br/> Örnek: sharepoint.contoso.com | Traffic Manager <br/><br/> contososharepoint.trafficmanager.net |
| Şirket içi DNS | sharepointonprem.contoso.com | Şirket içi çiftlikte halka açık IP |


Trafik Yöneticisi profilinde birincil [ve kurtarma bitiş noktalarını oluşturun.](../traffic-manager/traffic-manager-configure-priority-routing-method.md) Şirket içi bitiş noktası için harici bitiş noktasını ve Azure bitiş noktası için genel IP'yi kullanın. Önceliğin şirket içi bitiş noktasına daha yüksek ayarlandığından emin olun.

Trafik Yöneticisi'nin kullanılabilirlik hatasını otomatik olarak algılaması için SharePoint web katmanında belirli bir bağlantı noktasında (örneğin, 800) bir test sayfası barındırın. Bu, SharePoint sitelerinizden hiçbirinde anonim kimlik doğrulamasını etkinleştiremiyorsanız bir geçici çözümdür.

Trafik Yöneticisi profilini aşağıdaki ayarlarla [yapılandırın.](../traffic-manager/traffic-manager-configure-priority-routing-method.md)

* Yönlendirme yöntemi - 'Öncelik'
* DNS'nin yaşama süresi (TTL) - '30 saniye'
* Endpoint monitör ayarları - Anonim kimlik doğrulamasını etkinleştirebilirseniz, belirli bir web sitesi bitiş noktası verebilirsiniz. Veya, belirli bir bağlantı noktasında bir test sayfası kullanabilirsiniz (örneğin, 800).

## <a name="creating-a-recovery-plan"></a>Kurtarma planı oluşturma

Kurtarma planı, çok katmanlı bir uygulamada çeşitli katmanların başarısızlığını sıralamaya ve dolayısıyla uygulama tutarlılığını korumaya olanak tanır. Çok katmanlı bir web uygulaması için bir kurtarma planı oluştururken aşağıdaki adımları izleyin. [Kurtarma planı oluşturma hakkında daha fazla bilgi edinin.](site-recovery-runbook-automation.md#customize-the-recovery-plan)

### <a name="adding-virtual-machines-to-failover-groups"></a>Başarısız gruplara sanal makineler ekleme

1. App ve Web katmanı VM'lerini ekleyerek bir kurtarma planı oluşturun.
2. VM'leri gruplandırmak için 'Özelleştir'e tıklayın. Varsayılan olarak, tüm VM'ler 'Grup 1'in bir parçasıdır.

    ![RP'yi özelleştirin](./media/site-recovery-sharepoint/rp-groups.png)

3. Başka bir Grup (Grup 2) oluşturun ve Web katmanı VM'lerini yeni gruba taşıyın. Uygulama katmanı VM'leriniz 'Grup 1'in bir parçası olmalı ve Web katmanı VM'leri 'Grup 2'nin bir parçası olmalıdır. Bu, App tier VM'lerin önce Web katmanı VM'leri tarafından başlatılmasını sağlamak içindir.


### <a name="adding-scripts-to-the-recovery-plan"></a>Kurtarma planına komut dosyaları ekleme

En sık kullanılan Azure Site Kurtarma komut dosyalarını Aşağıdaki 'Azure'a Dağıt' düğmesini tıklayarak Otomasyon hesabınıza dağıtabilirsiniz. Yayımlanmış herhangi bir komut dosyası kullanırken, komut dosyasındaki kılavuzu izlediğinizi sağlayın.

[![Azure'a Dağıt](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)

1. SQL Kullanılabilirlik grubunu başarısız etmek için 'Grup 1'e eylem öncesi komut dosyası ekleyin. Örnek komut dosyalarında yayınlanan 'ASR-SQL-FailoverAG' komut dosyasını kullanın. Komut dosyasındaki kılavuzu takip ettiğinizden ve komut dosyasında gerekli değişiklikleri uygun şekilde yaptığınızdan emin olun.

    ![Ekle-AG-Komut Dosyası-Adım-1](./media/site-recovery-sharepoint/add-ag-script-step1.png)

    ![Ekle-AG-Komut Dosyası-Adım-2](./media/site-recovery-sharepoint/add-ag-script-step2.png)

2. Web katmanının (Grup 2) başarısız sanal makineleri üzerinde başarısız bir yük dengeleyici eklemek için bir post action komut dosyası ekleyin. Örnek komut dosyalarında yayınlanan 'ASR-AddSingleLoadBalancer' komut dosyasını kullanın. Komut dosyasındaki kılavuzu takip ettiğinizden ve komut dosyasında gerekli değişiklikleri uygun şekilde yaptığınızdan emin olun.

    ![Ekle-LB-Script-Adım-1](./media/site-recovery-sharepoint/add-lb-script-step1.png)

    ![Ekle-LB-Komut Dosyası-Adım-2](./media/site-recovery-sharepoint/add-lb-script-step2.png)

3. Azure'daki yeni çiftliğe işaret etmek için DNS kayıtlarını güncelleştirmek için el ile bir adım ekleyin.

    * Internet bakan siteler için, hiçbir DNS güncellemeleri sonrası failover gereklidir. Trafik Yöneticisi'ni yapılandırmak için 'Ağ kılavuzu' bölümünde açıklanan adımları izleyin. Trafik Yöneticisi profili önceki bölümde açıklandığı gibi ayarlanmışsa, Azure VM'de sahte bağlantı noktasını (örnekte 800) açmak için bir komut dosyası ekleyin.

    * Dahili siteler için, yeni Web katmanı VM'nin yük dengeleyici IP'sine işaret etmek için DNS kaydını güncelleştirmek için manuel bir adım ekleyin.

4. Arama uygulamasını yedeklemeden geri yüklemek veya yeni bir arama hizmeti başlatmak için el ile adım ekleyin.

5. Arama hizmeti uygulamasını bir yedeklemeden geri atmak için aşağıdaki adımları izleyin.

    * Bu yöntem, arama hizmeti uygulamasının bir yedeklemesinin felaket olaydan önce gerçekleştirildiğini ve yedeklemenin DR sitesinde kullanılabildiğini varsayar.
    * Bu, yedeklemeyi zamanlayarak (örneğin, günde bir kez) ve yedeklemeyi DR sitesine yerleştirmek için bir kopyalama yordamı kullanılarak kolayca elde edilebilir. Kopyalama yordamları, AzCopy (Azure Copy) veya DFSR (Dağıtılmış Dosya Hizmetleri Çoğaltma) kurulumu gibi komut dosyası programları içerebilir.
    * SharePoint çiftliği çalışmaya devam ettiğine göre, Merkezi Yönetim'de gezinmek, 'Yedekleme ve Geri yükleme' ve Geri Yükle'yi seçin. Geri yükleme belirtilen yedekleme konumunu sorgular (değeri güncelleştirmeniz gerekebilir). Geri yüklemek istediğiniz Arama Hizmeti Uygulaması yedeklemesini seçin.
    * Arama geri yüklendi. Geri yüklemenin aynı topolojiyi (aynı sayıda sunucu) ve bu sunuculara atanan aynı sabit disk harflerini bulmayı beklediğini unutmayın. Daha fazla bilgi için bkz: ['SharePoint 2013'te Arama hizmeti uygulamasını geri yükle'](https://technet.microsoft.com/library/ee748654.aspx) belge.


6. Yeni bir Arama hizmeti uygulamasıyla başlamak için aşağıdaki adımları izleyin.

    * Bu yöntem, "Arama Yönetimi" veritabanının bir yedekleme DR sitesinde kullanılabilir varsayar.
    * Diğer Arama Hizmeti Uygulaması veritabanları çoğaltılamadığından, yeniden oluşturulması gerekir. Bunu yapmak için Merkezi Yönetim'e gidin ve Arama Hizmeti Uygulamasını silin. Arama Dizini'ni barındıran tüm sunucularda dizin dosyalarını silin.
    * Arama Hizmeti Uygulamasını yeniden oluşturun ve bu veritabanlarını yeniden oluşturur. GUI üzerinden tüm eylemleri gerçekleştirmek mümkün olmadığından, bu hizmet uygulamasını yeniden oluşturan hazırlanmış bir komut dosyasına sahip olması önerilir. Örneğin, dizin sürücü konumunu ayarlamak ve arama topolojisini yapılandırmak yalnızca SharePoint PowerShell cmdlets kullanılarak mümkündür. Windows PowerShell cmdlet Restore-SPEnterpriseSearchServiceApplication'ı kullanın ve Search_Service__DB tarafından gönderilen ve çoğaltılan Arama Yönetimi veritabanını belirtin. Bu cmdlet arama yapılandırması, şema, yönetilen özellikleri, kuralları ve kaynakları verir ve diğer bileşenlerin varsayılan kümesini oluşturur.
    * Arama Hizmeti Uygulaması yeniden oluşturulduktan sonra, Arama Hizmeti'ni geri yüklemek için her içerik kaynağı için tam bir tarama başlatmanız gerekir. Şirket içi çiftlikteki arama önerileri gibi bazı analiz bilgilerini kaybedersiniz.

7. Tüm adımlar tamamlandıktan sonra, kurtarma planını kaydedin ve son kurtarma planı aşağıdaki gibi görünecektir.

    ![Kaydedilen RP](./media/site-recovery-sharepoint/saved-rp.png)

## <a name="doing-a-test-failover"></a>Bir test başarısız yapma
Bir test başarısız yapmak için [bu kılavuzu](site-recovery-test-failover-to-azure.md) izleyin.

1.  Azure portalına gidin ve Kurtarma Hizmeti kasanızı seçin.
2.  SharePoint uygulaması için oluşturulan kurtarma planına tıklayın.
3.  'Test Failover'ı tıklayın.
4.  Test başarısız lık işlemini başlatmak için kurtarma noktası ve Azure sanal ağını seçin.
5.  İkincil ortam dolduktan sonra doğrulamalarınızı gerçekleştirebilirsiniz.
6.  Doğrulamalar tamamlandıktan sonra, kurtarma planında 'Temizleme testi başarısız' seçeneğini tıklatabilirsiniz ve test başarısız ortamı temizlenir.

AD ve DNS için test başarısızlığına ilişkin rehberlik için, [AD ve DNS belgesi için test başarısızları hususuna](site-recovery-active-directory.md#test-failover-considerations) bakın.

SQL Always ON kullanılabilirlik grupları için test başarısızlığı yapma kılavuzu için, [Azure Site Kurtarma ile Uygulama DR Gerçekleştirme ve Test failover](site-recovery-sql.md#disaster-recovery-of-an-application) belgesi yapma'a bakın.

## <a name="doing-a-failover"></a>Bir failover yapma
Bir failover yapmak için [bu kılavuzu](site-recovery-failover.md) izleyin.

1.  Azure portalına gidin ve Kurtarma Hizmetleri kasanızı seçin.
2.  SharePoint uygulaması için oluşturulan kurtarma planına tıklayın.
3.  'Failover'a tıklayın.
4.  Başarısız lama işlemini başlatmak için kurtarma noktasını seçin.

## <a name="next-steps"></a>Sonraki adımlar
Site Kurtarma'yı kullanarak [diğer uygulamaları çoğaltma](site-recovery-workload.md) hakkında daha fazla bilgi edinebilirsiniz.
