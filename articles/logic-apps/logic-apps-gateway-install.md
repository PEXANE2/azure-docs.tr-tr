---
title: Şirket içi veri ağ geçidini (Azure Logic Apps) yükler | Microsoft Docs
description: Şirket içindeki verilere Azure Logic Apps erişmeden önce şirket içi veri ağ geçidini indirip yükleyin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: arthii, LADocs
ms.topic: article
ms.date: 10/01/2018
ms.openlocfilehash: 61a9b319b9ea44f766bc6f014b76bc48d15efc57
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68598461"
---
# <a name="install-on-premises-data-gateway-for-azure-logic-apps"></a>Azure Logic Apps için şirket içi veri ağ geçidini yükler

Azure Logic Apps 'den şirket içi veri kaynaklarına bağlanabilmeniz için, şirket içi veri ağ geçidini yerel bir bilgisayara indirip yükleyin. Ağ Geçidi, Şirket içindeki (bulutta değil) ve mantıksal uygulamalarınızın veri kaynakları arasında hızlı veri aktarımı ve şifreleme sağlayan bir köprü olarak çalışmaktadır. Bu makalede şirket içi veri ağ geçidinizin nasıl indirileceği, yüklenebileceğiniz ve ayarlayabileceğiniz gösterilmektedir. 

Aynı ağ geçidi yüklemesini Power BI, Microsoft Flow, PowerApps ve Azure Analysis Services gibi diğer hizmetlerle kullanabilirsiniz. [Data Gateway 'in nasıl çalıştığı](#gateway-cloud-service)hakkında daha fazla bilgi edinin.

<a name="supported-connections"></a>

Ağ Geçidi, bu veri kaynakları için Azure Logic Apps [Şirket içi bağlayıcıları](../connectors/apis-list.md#on-premises-connectors) destekler:

*   BizTalk Server 2016
*   Dosya sistemi
*   IBM DB2  
*   IBM Informix
*   IBM MQ
*   MySQL
*   Oracle Database
*   PostgreSQL
*   SAP Uygulama Sunucusu 
*   SAP İleti Sunucusu
*   SharePoint Server
*   SQL Server
*   Teradata

Ağ geçidini diğer hizmetlerle kullanma hakkında daha fazla bilgi için şu makalelere bakın:

* [Microsoft Power BI şirket içi veri ağ geçidi](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
* [Şirket içi veri ağ geçidini Microsoft PowerApps](https://powerapps.microsoft.com/tutorials/gateway-management/)
* [Şirket içi veri ağ geçidini Microsoft Flow](https://flow.microsoft.com/documentation/gateway-manage/)
* [Şirket içi veri ağ geçidini Azure Analysis Services](../analysis-services/analysis-services-gateway.md)

<a name="requirements"></a>

## <a name="prerequisites"></a>Önkoşullar

* [Azure aboneliğine](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access) sahip bir [iş veya okul hesabı](../active-directory/fundamentals/sign-up-organization.md) 

  Ağ geçidi yüklemesi sırasında, ağ geçidi yüklemesini Azure aboneliğinizle ilişkilendirebilmeniz için bu hesapta oturum açın. 
  Daha sonra, Azure portal ağ geçidi yüklemeniz için bir Azure kaynağı oluştururken de aynı hesabı kullanırsınız. 
  Henüz bir Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Yerel bilgisayarınız için gereksinimler şunlardır:

  **Minimum gereksinimler**

  * .NET Framework 4.5.2
  * Windows 7 veya Windows Server 2008 R2 64-bit sürümü (veya üzeri)

  **Önerilen gereksinimler**

  * 8 çekirdekli CPU
  * 8 GB bellek
  * Windows Server 2012 R2 64-bit sürümü (veya üzeri)

* **Önemli konular**

  * Şirket içi veri ağ geçidini, etki alanı denetleyicisi değil yalnızca yerel bir bilgisayara yükleyebilirsiniz. Ancak, ağ geçidini veri kaynağınıza göre aynı bilgisayara yüklemenize gerek yoktur. Ayrıca, tüm veri kaynaklarınız için yalnızca bir ağ geçidine ihtiyacınız vardır, bu nedenle her bir veri kaynağı için ağ geçidini yüklemeniz gerekmez.

    > [!TIP]
    > Gecikme süresini en aza indirmek için, ağ geçidini veri kaynağınıza veya aynı bilgisayara mümkün olduğunca yakın bir şekilde yükleyebilirsiniz.

  * Ağ geçidini Internet 'e bağlı bir bilgisayara yükler, her zaman *açık ve uyku* moduna geçmez. Aksi takdirde, ağ geçidi çalıştırılamaz. 
  Ayrıca, performans kablosuz bir ağdan düşebilir.

  * Yükleme sırasında yalnızca Azure Active Directory (Azure AD) tarafından yönetilen bir [iş veya okul hesabıyla](../active-directory/sign-up-organization.md) oturum açabilirsiniz; örneğin @contoso.onmicrosoft.com, bir Azure B2B (konuk) hesabı veya @hotmail.com ya dagibikişiselMicrosofthesabıdeğildir.@outlook.com. 
  Bir ağ geçidi kaynağı oluşturarak ağ geçidi yüklemenizi Azure portal kaydederken aynı oturum açma hesabını kullandığınızdan emin olun. 
  Daha sonra, mantıksal uygulamanızdan şirket içi veri kaynağınıza bağlantı oluşturduğunuzda bu ağ geçidi kaynağını seçebilirsiniz. 
  [Neden bir Azure AD iş veya okul hesabı kullanmalıyım?](#why-azure-work-school-account)

  > [!TIP]
  > Office 365 teklifi için kaydolduysanız ve gerçek iş e-postanızı sağlamadıysanız Şu örneğe benzer bir oturum açma adresiniz olabilir:`username@domain.onmicrosoft.com` 
  >
  > [Visual Studio standart aboneliğine](https://visualstudio.microsoft.com/vs/pricing/)sahip bir Microsoft hesabı kullanmak için, önce [Azure Active Directory bir dizin (kiracı) oluşturun](../active-directory/develop/quickstart-create-new-tenant.md)veya Microsoft hesabı varsayılan dizini kullanın. 
  > Dizine bir parolası olan bir kullanıcı ekleyin ve bu kullanıcıya aboneliğinize erişim izni verin. 
  > Daha sonra bu Kullanıcı adı ve parolayla ağ geçidi yüklemesi sırasında oturum açabilirsiniz.

  * Ağ geçidinizin yüklemeniz için seçtiğiniz bölge, bir Azure kaynağı oluşturarak ağ geçidinizi daha sonra Azure 'da kaydettiğiniz konumu belirler. 
  Bu ağ geçidi kaynağını Azure 'da oluşturduğunuzda, ağ geçidi yüklemenizin *bulunduğu* konumu seçmeniz gerekir. Varsayılan bölge, Azure hesabınızı yöneten Azure AD kiracınızla aynı konumdadır ancak ağ geçidi yüklemesi sırasında konumu değiştirebilirsiniz.

  * 14.16.6317.4 sürümünden önceki bir yükleyiciyle ayarladığınız bir ağ geçidiniz zaten varsa, en son yükleyiciyi çalıştırarak ağ geçidinizin konumunu değiştiremezsiniz. Ancak, istediğiniz konuma sahip yeni bir ağ geçidi ayarlamak için en son yükleyiciyi kullanabilirsiniz.
  
    Sürüm 14.16.6317.4 ' den önceki bir ağ geçidi yükleyiciniz varsa ancak ağ geçidinizi henüz yüklemediyseniz, bunun yerine en son yükleyiciyi indirebilir ve kullanabilirsiniz.

## <a name="high-availability-support"></a>Yüksek kullanılabilirlik desteği

Şirket içi veri ağ geçidi, birden fazla ağ geçidi yüklemeniz ve bunları kümeler olarak ayarlamanız durumunda yüksek kullanılabilirliği destekler. Başka bir ağ geçidi oluşturmak için gittiğinizde mevcut bir ağ geçidiniz varsa, isteğe bağlı olarak yüksek kullanılabilirlik kümeleri oluşturabilirsiniz. Bu kümeler, ağ geçitlerini tek hata noktalarından kaçınmanıza yardımcı olabilecek gruplar halinde düzenler. Ayrıca, tüm şirket içi veri ağ geçidi bağlayıcıları artık yüksek kullanılabilirliği desteklemektedir.

Şirket içi veri ağ geçidini kullanmak için şu gereksinimleri ve konuları gözden geçirin:

* Birincil ağ geçidi ile aynı Azure aboneliğinde en az bir ağ geçidi yüklemeniz ve ilgili yükleme için kurtarma anahtarı olmalıdır. 

* Birincil ağ geçidinizin, Kasım 2017 veya sonraki bir sürümünün ağ geçidi güncelleştirmesini çalıştırmalıdır.

Bu gereksinimleri karşıladıktan sonra, bir sonraki ağ geçidinizi oluşturduğunuzda, **mevcut bir ağ geçidi kümesine ekle**' yi seçin, kümenizin birincil ağ geçidini seçin ve o birincil ağ geçidi için kurtarma anahtarını sağlayın.
Daha fazla bilgi için bkz. Şirket [içi veri ağ geçidi Için yüksek kullanılabilirlik kümeleri](https://docs.microsoft.com/power-bi/service-gateway-high-availability-clusters).

<a name="install-gateway"></a>

## <a name="install-data-gateway"></a>Veri ağ geçidi yükleme

1. [Ağ geçidi yükleyicisini yerel bir bilgisayarda indirin, kaydedin ve çalıştırın](https://aka.ms/on-premises-data-gateway-installer).

2. Varsayılan yükleme yolunu kabul edin veya bilgisayarınızda ağ geçidini yüklemek istediğiniz konumu belirtin.

3. Kullanım koşullarını ve gizlilik bildirimini gözden geçirin ve kabul edin ve ardından **Install**' ı seçin.

   ![Kullanım koşullarını ve gizlilik bildirimini kabul et](./media/logic-apps-gateway-install/accept-terms.png)

4. Ağ Geçidi başarıyla yüklendikten sonra, iş veya okul hesabınız için e-posta adresini girin ve **oturum aç**' ı seçin.

   ![İş veya okul hesabıyla oturum açın](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

5. Ağ Geçidi yüklemenizi [Ağ Geçidi Bulut hizmeti](#gateway-cloud-service)ile > kaydeden **,** **Bu bilgisayarda yeni bir ağ geçidi kaydet**' i seçin. 

   ![Ağ geçidini kaydetme](./media/logic-apps-gateway-install/register-new-gateway.png)

6. Ağ Geçidi yüklemeniz için şu bilgileri sağlayın:

   * Yüklemeniz için istediğiniz ad 

   * Oluşturmak istediğiniz kurtarma anahtarı, en az sekiz karakter içermelidir

     > [!IMPORTANT]
     > Kurtarma Anahtarınızı güvenli bir yerde kaydedin ve saklayın. Ağ geçidinin konumunu değiştirirken veya var olan bir ağ geçidini geçirdiğinizde, kurtardığınızda veya devralırken bu anahtara ihtiyacınız vardır.

   * Kurtarma anahtarınız için onay 

     ![Ağ geçidini ayarlama](./media/logic-apps-gateway-install/set-up-gateway.png)

7. Ağ Geçidi bulutu hizmeti için seçilen bölgeyi ve ağ geçidi yüklemeniz tarafından kullanılan Azure Service Bus denetleyin. 

   ![Bölgeyi denetle](./media/logic-apps-gateway-install/check-region.png)

   > [!IMPORTANT]
   > Ağ geçidinizi yüklemeyi bitirdikten sonra bu bölgeyi değiştirmek için bu ağ geçidi yüklemesi için kurtarma anahtarına ihtiyacınız vardır. Ayrıca, ağ geçidini kaldırmanız ve yeniden yüklemeniz gerekir. Daha fazla bilgi için bkz. [konumu değiştirme, geçirme, kurtarma veya mevcut ağ geçidini alma](#update-gateway-installation).

   *Ağ Geçidi yüklemenizin bölgesi neden değiştirilsin?* 

   Örneğin, gecikme süresini azaltmak için ağ geçidinizin bölgenizi mantıksal uygulamanızla aynı bölgeye göre değiştirebilirsiniz. 
   Ya da şirket içi veri kaynağınıza en yakın bölgeyi seçebilirsiniz. 
   *Azure 'daki ağ geçidi kaynağınızın* ve mantıksal uygulamanızın farklı konumları olabilir.

8. Varsayılan bölgeyi kabul etmek için **Yapılandır**' ı seçin. Ya da varsayılan bölgeyi değiştirmek için şu adımları izleyin:

   1. Geçerli bölgenin yanındaki **bölgeyi değiştir**' i seçin. 

      ![Bölge Değiştir](./media/logic-apps-gateway-install/change-region.png)

   2. Sonraki sayfada **Bölge seç** listesini açın, istediğiniz bölgeyi seçin ve **bitti**' yi seçin.

      ![Başka bir bölge seçin](./media/logic-apps-gateway-install/select-region-gateway-install.png)

9. Onay sayfası göründükten sonra **Kapat**' ı seçin. 

   Yükleyici, ağ geçidinizin çevrimiçi ve kullanıma hazır olduğunu onaylar.

   ![Ağ Geçidi tamamlandı](./media/logic-apps-gateway-install/finished-gateway-default-location.png)

10. Şimdi ağ geçidiniz [için bir Azure kaynağı oluşturarak](../logic-apps/logic-apps-gateway-connection.md)ağ geçidinizi Azure 'a kaydedin. 

<a name="update-gateway-installation"></a>

## <a name="change-location-migrate-restore-or-take-over-existing-gateway"></a>Konum değiştirme, geçirme, geri yükleme veya mevcut ağ geçidini alma

Ağ geçidinizin konumunu değiştirmeniz gerekiyorsa, ağ geçidi yüklemenizi yeni bir bilgisayara taşıyın, hasarlı bir ağ geçidini kurtarmanız veya mevcut bir ağ geçidi için sahiplik almanız gerekiyorsa, ağ geçidi yüklemesi sırasında sağlanmış olan kurtarma anahtarına ihtiyacınız vardır. Bu eylem, eski ağ geçidinin bağlantısını keser.

1. Bilgisayarınızın **denetim masasından** **Programlar ve Özellikler**' e gidin. Programlar listesinde, Şirket **içi veri ağ geçidi**' ni seçin ve ardından **Kaldır**' ı seçin.

2. Şirket [içi veri ağ geçidini yeniden yükleyin](https://aka.ms/on-premises-data-gateway-installer).

3. Yükleyici açıldıktan sonra, ağ geçidini yüklemek için daha önce kullanılan iş veya okul hesabıyla oturum açın.

4. **Var olan bir ağ geçidini geçir, geri yükle veya getir**' i seçin ve ardından **İleri**' yi seçin.

   !["Var olan bir ağ geçidini geçir, geri yükle veya getir" i seçin](./media/logic-apps-gateway-install/migrate-recover-take-over-gateway.png)

5. **Kullanılabilir ağ geçitleri** veya **kullanılabilir ağ geçidi kümeleri**altında, değiştirmek istediğiniz ağ geçidi yüklemesini seçin. Ağ geçidi yüklemesi için kurtarma anahtarını girin. 

   ![Birincil ağ geçidini seçin](./media/logic-apps-gateway-install/select-existing-gateway.png)

6. Bölgeyi değiştirmek için bölgeyi ve yeni bölgeyi **Değiştir** ' i seçin.

7. İşiniz bittiğinde **Yapılandır**' ı seçin.

## <a name="configure-proxy-or-firewall"></a>Proxy veya güvenlik duvarını yapılandırma

Şirket içi veri ağ geçidi [Azure Service Bus](https://azure.microsoft.com/services/service-bus/)giden bir bağlantı oluşturur. İş ortamınız internet 'e erişmek için bir proxy 'den gider gerektiriyorsa, bu kısıtlama veri ağ geçidinin ağ geçidi bulut hizmetine bağlanmasını engelleyebilir. Ağınızın bir proxy kullanıp kullanmadığını öğrenmek için superuser.com adresinde bu makaleye bakın: 

[Nasıl yaparım? hangi proxy sunucusunu kullandığını öğrenin. (SuperUser.com)](https://superuser.com/questions/346372/how-do-i-know-what-proxy-server-im-using) 

Ağ geçidinizin ara sunucu bilgilerini sağlamak için bkz. [proxy ayarlarını yapılandırma](https://docs.microsoft.com/power-bi/service-gateway-proxy). Proxy 'nizin veya güvenlik duvarının bağlantıları engelleyebileceğinden emin olmak için makinenizin Internet 'e ve [Azure Service Bus](https://azure.microsoft.com/services/service-bus/)gerçekten bağlanıp bağlanamadıklarını doğrulayın. Bir PowerShell isteminden şu komutu çalıştırın:

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

> [!NOTE]
> Bu komut yalnızca ağ bağlantısını ve Azure Service Bus bağlantısını sınar. Komut, kimlik bilgilerinizi ve ağ geçidi ayrıntılarını şifreleyen ve depolayan ağ geçidi veya ağ geçidi bulutu hizmeti ile herhangi bir şey yapmaz. 
>
> Ayrıca, bu komut yalnızca Windows Server 2012 R2 veya üzeri sürümlerde ve Windows 8.1 veya üzeri sürümlerde kullanılabilir. Daha önceki işletim sistemi sürümlerinde, bağlantıyı test etmek için Telnet ' i kullanabilirsiniz. [Azure Service Bus ve karma çözümler](../service-bus-messaging/service-bus-messaging-overview.md)hakkında daha fazla bilgi edinin.

Bu örnekte, **Tcptestsucceeded** **değeri true**olarak ayarlandığında sonuçlarınız şuna benzemelidir:

```text
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

**Tcptestsucceeded** **değeri true**olarak ayarlanmamışsa, ağ geçidiniz bir güvenlik duvarı tarafından engelleniyor olabilir. Kapsamlı olmasını istiyorsanız **ComputerName** ve **Port** değerlerini bu makaledeki [bağlantı noktalarını Yapılandır](#configure-ports) altında listelenen değerlerle değiştirin.

Güvenlik Duvarı, Azure Service Bus Azure veri merkezlerine yaptığı bağlantıları da engelleyebilir. Bu senaryo olursa, bölgenizdeki bu veri merkezlerinin tüm IP adreslerini onaylayın (engellemeyi kaldırın). Bu IP adresleri için [buradan Azure IP adresleri listesini alın](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="configure-ports"></a>Bağlantı noktalarını yapılandırma

Ağ Geçidi [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) ve giden bağlantı noktalarında iletişim kuran bir giden bağlantı oluşturur: TCP 443 (varsayılan), 5671, 5672, 9350 ile 9354. Ağ Geçidi, gelen bağlantı noktaları gerektirmez. [Azure Service Bus ve karma çözümler](../service-bus-messaging/service-bus-messaging-overview.md)hakkında daha fazla bilgi edinin.

Ağ Geçidi bu tam etki alanı adlarını kullanır:

| Etki alanı adları | Giden bağlantı noktaları | Açıklama | 
| ------------ | -------------- | ----------- | 
| *. analysis.windows.net | 443 | HTTPS | 
| *. core.windows.net | 443 | HTTPS | 
| *.frontend.clouddatahub.net | 443 | HTTPS | 
| *. login.windows.net | 443 | HTTPS | 
| *.microsoftonline-p.com | 443 | Yapılandırmaya bağlı olarak kimlik doğrulaması için kullanılır. | 
| *. msftncsi.com | 443 | Power BI hizmeti ağ geçidine erişilemiyorsa internet bağlantısını test etmek için kullanılır. | 
| *.servicebus.windows.net | 443, 9350-9354 | TCP üzerinden Service Bus geçişine yönelik dinleyiciler (Access Control belirteci alımı için 443 gerekir) | 
| *.servicebus.windows.net | 5671-5672 | Gelişmiş İleti Sıraya Alma Protokolü (AMQP) | 
| login.microsoftonline.com | 443 | HTTPS | 
||||

Bazı durumlarda, Azure Service Bus bağlantıları, tam etki alanı adları yerine IP adresleriyle yapılır. Bu nedenle, güvenlik duvarınızdaki veri bölgenizin IP adreslerini beyaz listeye almak isteyebilirsiniz. Etki alanları yerine IP adreslerini beyaz listeye almak için [Microsoft Azure veri MERKEZI IP aralıkları listesini](https://www.microsoft.com/download/details.aspx?id=41653)indirip kullanabilirsiniz. Bu listedeki IP adresleri, [sınıfsız etki alanları arası yönlendirme (CIDR)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösterimiyle bulunur.

### <a name="force-https-communication-with-azure-service-bus"></a>Azure Service Bus ile HTTPS iletişimini zorla

Bazı proxy 'ler yalnızca 80 ve 443 bağlantı noktalarında trafiğe izin verir. Varsayılan olarak, Azure Service Bus iletişim 443 dışındaki bağlantı noktalarında oluşur.
Ağ geçidini doğrudan TCP yerine HTTPS üzerinden Azure Service Bus iletişim kurmaya zorlayabilirsiniz, ancak bunu yapmak performansı önemli ölçüde azaltabilir. Bu görevi gerçekleştirmek için şu adımları izleyin:

1. Şirket içi veri ağ geçidi istemcisinin konumuna gidin ve genellikle burada bulabilirsiniz:```C:\Program Files\On-premises data gateway\Microsoft.PowerBI.EnterpriseGateway.exe```

   Aksi takdirde, istemci konumunu bulmak için aynı bilgisayarda Hizmetler konsolunu açın, Şirket **içi veri ağ geçidi hizmeti**' ni bulun ve **yürütülebilir özelliğin yolunu** görüntüleyin.

2. Bu *yapılandırma* dosyasını açın: **Microsoft. PowerBI. Datataşıması. Pipeline. GatewayCore. dll. config**

3. **Servicebussystemconnectivitymodestring** değerini **Otomatik Algıla** değerinden **https**olarak değiştirin:

   ```html
   <setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
      <value>Https</value>
   </setting>
   ```

<a name="windows-service-account"></a>

## <a name="windows-service-account"></a>Windows hizmeti hesabı

Şirket içi veri ağ geçidini yüklediğiniz bilgisayarda, ağ geçidi "Şirket içi veri ağ geçidi hizmeti" adlı bir Windows hizmet hesabı olarak çalışır. Ancak ağ geçidi, "oturum aç" hesabı kimlik bilgileri için "NT Servıce\pbıegwservice" adını kullanır. Varsayılan olarak ağ geçidinin, ağ geçidini yüklediğiniz bilgisayardaki "hizmet olarak oturum aç" izinleri vardır. Ağ geçidinin Windows hizmet hesabı genellikle şirket içi veri kaynaklarına bağlanmak için kullandığınız hesaptan ve bulut hizmetlerinde oturum açmak için kullandığınız iş veya okul hesabından farklıdır.

Azure portal ağ geçidini oluşturup koruyabilmeniz için, bu Windows hizmet hesabının en az **katkıda bulunan** izinlerine sahip olması gerekir. Bu izinleri denetlemek için bkz. [RBAC kullanarak erişimi yönetme ve Azure Portal](../role-based-access-control/role-assignments-portal.md). 

<a name="restart-gateway"></a>

## <a name="restart-gateway"></a>Ağ geçidini yeniden Başlat

Veri ağ geçidi bir pencere hizmeti olarak çalışır, bu nedenle diğer Windows Hizmetleri gibi, ağ geçidini çeşitli yollarla başlatabilir ve durdurabilirsiniz. Örneğin, ağ geçidinin çalıştığı bilgisayarda yükseltilmiş izinlerle bir komut istemi açabilir ve iki komutu çalıştırabilirsiniz:

* Hizmeti durdurmak için şu komutu çalıştırın:
  
  `net stop PBIEgwService`

* Hizmeti başlatmak için şu komutu çalıştırın:
  
  `net start PBIEgwService`

## <a name="tenant-level-administration"></a>Kiracı düzeyinde yönetim 

Şu anda, kiracı yöneticilerinin diğer kullanıcıların yüklediği ve yapılandırdığı tüm ağ geçitlerini yönetebileceği tek bir yer yoktur. Kiracı yöneticisiyseniz, kuruluşunuzdaki kullanıcıların, yükledikleri her ağ geçidi için sizi yönetici olarak eklemesini isteyebilirsiniz. Bu şekilde, ağ geçidi ayarları sayfası veya [PowerShell komutları](/data-integration/gateway/service-gateway-powershell-support)aracılığıyla kuruluşunuzdaki tüm ağ geçitlerini yönetebilirsiniz. 

<a name="gateway-cloud-service"></a>

## <a name="how-does-the-gateway-work"></a>Ağ geçidi nasıl çalışır?

Veri ağ geçidi, mantıksal uygulamanız, ağ geçidi bulut hizmeti ve şirket içi veri kaynağınız arasındaki hızlı ve güvenli iletişimi kolaylaştırır. Ağ Geçidi bulutu hizmeti, veri kaynağı kimlik bilgilerinizi ve ağ geçidi ayrıntılarını şifreler ve depolar. Hizmet Ayrıca sorguları ve bunların sonuçlarını mantıksal uygulamanız, şirket içi veri ağ geçidi ve şirket içi veri kaynağınız arasında yönlendirir. 

Ağ Geçidi, güvenlik duvarları ile birlikte çalışarak yalnızca giden bağlantıları kullanır. Tüm trafik ağ geçidi aracısından güvenli giden trafik olarak gelir. Ağ Geçidi, Azure Service Bus aracılığıyla şifrelenmiş kanallardaki şirket içi kaynaklardaki verileri geçirir. Bu hizmet veri yolu, ağ geçidi ile çağıran hizmet arasında bir kanal oluşturur, ancak herhangi bir veri depolamaz. Ağ Geçidi üzerinden taşınan tüm veriler şifrelenir.

![Şirket içi-veri-ağ geçidi-akış diyagramı](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

Bu adımlarda, buluttaki bir kullanıcı şirket içi veri kaynağınıza bağlı bir öğeyle etkileşim kurduğunda ne olacağı açıklanır:

1. Ağ Geçidi bulutu hizmeti, veri kaynağı için şifrelenmiş kimlik bilgileriyle birlikte bir sorgu oluşturur ve ağ geçidinin işlemesi için sorguyu kuyruğa gönderir.

2. Ağ Geçidi Bulut hizmeti sorguyu analiz eder ve isteği Azure Service Bus gönderir.

3. Şirket içi veri ağ geçidi, bekleyen istekler için Azure Service Bus yoklar.

4. Ağ Geçidi sorguyu alır, kimlik bilgilerinin şifresini çözer ve bu kimlik bilgileriyle veri kaynağına bağlanır.

5. Ağ Geçidi, yürütme için sorguyu veri kaynağına gönderir.

6. Sonuçlar veri kaynağından ağ geçidine ve sonra ağ geçidi bulut hizmetine gönderilir. Daha sonra ağ geçidi bulutu hizmeti sonuçları kullanır.

<a name="faq"></a>

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="general"></a>Genel

**S**: Buluttaki veri kaynakları için Azure SQL veritabanı gibi bir ağ geçidine ihtiyacım var mı? <br/>
Y: Hayır, ağ geçidi yalnızca şirket içi veri kaynaklarına bağlanır.

**S**: Ağ Geçidi, veri kaynağıyla aynı makineye mi yüklenmelidir? <br/>
Y: Hayır, ağ geçidi, girilen bağlantı bilgilerini kullanarak veri kaynağına bağlanır. Ağ geçidini bu anlamda bir istemci uygulaması olarak düşünün. Ağ geçidinin yalnızca sunulan sunucu adına bağlanması için gereken bir özelliğe ihtiyacı vardır.

<a name="why-azure-work-school-account"></a>

**S**: Oturum açmak için neden bir iş veya okul hesabı kullanmalıyım? <br/>
Y: Şirket içi veri ağ geçidini yüklerken yalnızca bir iş veya okul hesabı kullanabilirsiniz. Oturum açma hesabınız, Azure Active Directory (Azure AD) tarafından yönetilen bir kiracıda depolanır. Genellikle, Azure AD hesabınızın kullanıcı asıl adı (UPN) e-posta adresiyle eşleşir.

**S**: Kimlik bilgilerim nerede depolanır? <br/>
Y: Bir veri kaynağı için girdiğiniz kimlik bilgileri, ağ geçidi bulut hizmetinde şifrelenir ve saklanır. Şirket içi veri ağ geçidinde kimlik bilgilerinin şifresi çözülür.

**S**: Ağ bant genişliği için herhangi bir gereksinim var mı? <br/>
Y: Ağ bağlantınızın iyi bir işleme sahip olup olmadığını denetleyin. Her ortam farklıdır ve gönderilen veri miktarı sonuçları etkileyebilir. Şirket içi veri kaynağınız ile Azure veri merkezleri arasında bir verimlilik düzeyi sağlamak için [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/)' u deneyin. Aktarım hızını ölçmenize yardımcı olmak için Azure hız testi gibi bir dış araç deneyin.

**S**: Ağ geçidinden bir veri kaynağına sorgu çalıştırmaya yönelik gecikme nedir? En iyi mimari nedir? <br/>
Y: Ağ gecikmesini azaltmak için ağ geçidini, mümkün olduğunca veri kaynağına yakın bir şekilde yükler. Ağ geçidini gerçek veri kaynağına yükleyebiliyorsanız, bu yakınlık, tanıtılan gecikme süresini en aza indirir. Ayrıca, Azure veri merkezlerine yakınlık de göz önünde bulundurun. Örneğin, hizmetiniz Batı ABD veri merkezini kullanıyorsa ve bir Azure VM 'de barındırılan SQL Server varsa, Batı ABD bölgesindeki Azure VM 'nizi de isteyebilirsiniz. Bu yakınlık, gecikme süresini en aza indirir ve Azure VM 'de çıkış ücretlerini önler.

**S**: Sonuçlar buluta nasıl geri gönderilir? <br/>
Y: Sonuçlar Azure Service Bus aracılığıyla gönderilir.

**S**: Buluttan ağ geçidine yönelik herhangi bir gelen bağlantı var mı? <br/>
Y: Hayır, ağ geçidi Azure Service Bus için giden bağlantıları kullanır.

**S**: Giden bağlantıları engelsem ne yapmalıyım? Ne yapmam gerekir? <br/>
Y: Ağ geçidinin kullandığı bağlantı noktalarına ve konaklara bakın.

**S**: Asıl Windows hizmeti ne çağırdı? <br/>
Y: Görev Yöneticisi 'nin Hizmetler sekmesinde, hizmet adı "Pbıegwservice" ya da Enterprise Gateway Service Power BI. Hizmetler konsolunda, hizmet adı "Şirket içi veri ağ geçidi hizmeti" olur. Windows hizmeti, hizmet SID 'SI (SSID) olarak "NT Servıce\pbıegwservice" kullanır.

**S**: Ağ geçidi Windows hizmeti bir Azure Active Directory hesabıyla çalıştırılabilir mi? <br/>
Y: Hayır, Windows hizmetinin geçerli bir Windows hesabı olması gerekir.

### <a name="disaster-recovery"></a>Olağanüstü durum kurtarma

**S**: Olağanüstü durum kurtarma için hangi seçenekler kullanılabilir? <br/>
Y: Bir ağ geçidini geri yüklemek veya taşımak için kurtarma anahtarını kullanabilirsiniz. Ağ geçidini yüklerken kurtarma anahtarını belirtin.

**S**: Kurtarma anahtarının avantajı nedir? <br/>
Y: Kurtarma anahtarı, bir olağanüstü durum sonrasında ağ geçidi ayarlarınızı geçirmek veya kurtarmak için bir yol sağlar.

## <a name="troubleshooting"></a>Sorun giderme

Bu bölümde, şirket içi veri ağ geçidini ayarlarken ve kullanırken karşılaşabileceğiniz bazı yaygın sorunlar ele alınmaktadır.

**S**: Ağ Geçidi yüklemem neden başarısız oldu? <br/>
Y: Bu sorun, hedef bilgisayardaki virüsten koruma yazılımının güncel olmaması durumunda meydana gelebilir. Virüsten koruma yazılımını güncelleştirebilir ya da virüsten koruma yazılımını devre dışı bırakabilir, ancak yalnızca ağ geçidi yüklemesi sırasında yeniden etkinleştirebilirsiniz.

**S**: Azure 'da ağ geçidi kaynağını oluştururken ağ geçidi yüklememi neden göremiyorum? <br/>
Y: Bu sorun şu nedenlerden kaynaklanabilir:

* Ağ Geçidi yüklemeniz zaten kayıtlı ve Azure 'daki başka bir ağ geçidi kaynağı tarafından talep edildi. Ağ Geçidi yüklemeleri, ağ geçidi kaynakları oluşturulduktan sonra örnekler listesinde görünmez.
Azure portal ağ geçidi kayıtlarınızı denetlemek için tüm Azure *abonelikleri için* **Şirket Içi veri ağ geçitleri** türü ile tüm Azure kaynaklarınızı gözden geçirin. 

* Ağ geçidini yükleyen kişinin Azure AD kimliği, Azure portal oturum açan kişiden farklıdır. Ağ geçidini yükleyen kimlik ile oturum açtığınızdan emin olun.

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

**S**: Ağ Geçidi günlükleri nerede? <br/>
Y: Bu makalenin ilerleyen bölümlerindeki [ **Günlükler** bölümüne](#logs) bakın.

**S**: Şirket içi veri kaynağına hangi sorguların gönderildiğini nasıl görebilirim? <br/>
Y: Gönderilen sorguları içeren sorgu izlemeyi etkinleştirebilirsiniz. Sorun gidermeyi tamamladıktan sonra sorgu izlemeyi özgün değere değiştirmeyi unutmayın. Sorgu izlemenin açık bırakılması daha büyük Günlükler oluşturur.

Ayrıca, veri kaynağınızda sorguları izlemek için gereken araçlara bakabilirsiniz. Örneğin, SQL Server ve Analysis Services için genişletilmiş olayları ya da SQL Profiler 'ı kullanabilirsiniz.

### <a name="outdated-gateway-version"></a>Güncel olmayan ağ geçidi sürümü

Ağ Geçidi sürümü güncel hale geldiğinde birçok sorun ortaya bulunabilir. İyi bir genel uygulama olarak, en son sürüme sahip olduğunuzdan emin olun. Ağ geçidini bir ay veya daha uzun süredir güncelleştirmediyseniz ağ geçidinin en son sürümünü yüklemeyi düşünebilirsiniz ve sorunu yeniden oluşturup oluşturamadığınızı görebilirsiniz.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a>Hata: Gruba kullanıcı eklenemedi. (-2147463168 Pbıegwservice performans günlüğü kullanıcıları)

Ağ geçidini, desteklenmeyen bir etki alanı denetleyicisine yüklemeye çalışırsanız bu hatayı alabilirsiniz. Ağ geçidini, etki alanı denetleyicisi olmayan bir makineye dağıttığınızdan emin olun.

<a name="logs"></a>

### <a name="logs"></a>Günlükler

Sorun gidermenize yardımcı olmak için, ağ geçidi günlüklerini toplayıp inceleyerek her zaman başlayın. Günlükleri toplamak için çeşitli yöntemlere sahipsiniz, ancak ağ geçidini yükledikten sonra en basit seçenek ağ geçidi yükleyicisinin kullanıcı arabiriminden yapılır. 

1. Bilgisayarınızda, şirket içi veri ağ geçidi yükleyicisini açın.
2. Sol taraftaki menüden **Tanılama**' yı seçin.
3. **Ağ geçidi günlükleri**altında, **günlükleri dışarı aktar**' ı seçin.

   ![Ağ Geçidi yükleyicisinden günlükleri dışarı aktarma](./media/logic-apps-gateway-install/export-logs.png)

Çeşitli günlükleri bulabileceğiniz diğer konumlar aşağıda verilmiştir:

| Günlük türü | Location | 
|----------|----------| 
| **Yükleyici günlükleri** | %localappdata%\Temp\On-premises_data_gateway_ <*yyyymmdd*>. <*numarası*>. log | 
| **Yapılandırma günlükleri** | C:\Users\<*Kullanıcı adı*> \appdata\local\microsoft\on-premises Data gateway\gatewayconfigurator <*YYYYMMDD*>. <*numarası*>. log | 
| **Kurumsal ağ geçidi hizmeti günlükleri** | C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises Data gateway\Gateway <*yyyymmdd*>. <*numarası*>. log | 
||| 

**Olay günlükleri**

Ağ geçidinin olay günlüklerini bulmak için şu adımları izleyin:

1. Ağ Geçidi yüklemesiyle ilgili bilgisayarda **Olay Görüntüleyicisi**açın. 
2. **Olay Görüntüleyicisi (yerel)**  > **uygulamalar ve hizmet günlükleri '** ni genişletin. 
3. Şirket **içi veri ağ geçidi hizmeti '** ni seçin.

   ![Ağ Geçidi için olay günlüklerini görüntüleme](./media/logic-apps-gateway-install/event-viewer.png)

### <a name="review-slow-query-performance"></a>Yavaş sorgu performansını gözden geçirme

Ağ geçidinde yavaş çalışan sorgular bulursanız, sorguları ve bunların sürelerini veren ek günlüğe kaydetme özelliğini açabilirsiniz. Bu Günlükler hangi sorguların yavaş veya uzun süre çalıştığını bulmanıza yardımcı olur. Sorgu performansını ayarlamak için veri kaynağınızı değiştirmeniz gerekebilir; Örneğin, SQL Server sorguları için dizinleri ayarlayın.

Bir sorgunun süresini öğrenmek için şu adımları izleyin:

1. Ağ Geçidi istemcisiyle aynı konuma gidin ve genellikle burada bulabilirsiniz:```C:\Program Files\On-premises data gateway```

   Aksi takdirde, istemci konumunu bulmak için aynı bilgisayarda Hizmetler konsolunu açın, Şirket **içi veri ağ geçidi hizmeti**' ni bulun ve **yürütülebilir özelliğin yolunu** görüntüleyin.

2. Bu yapılandırma dosyalarını açın ve açıklandığı gibi düzenleyin:

   * **Microsoft. PowerBI. Datataşıması. Pipeline. GatewayCore. dll. config**

     Bu dosyada, ağ geçidiniz ağ geçidinden bir veri kaynağına gönderilen sorguları günlüğe kaydetmek için **EmitQueryTraces** değerini **false** değerinden **true** olarak değiştirin:

     ```html
     <setting name="EmitQueryTraces" serializeAs="String">
        <value>true</value>
     </setting>
     ```

     > [!IMPORTANT]
     > EmitQueryTraces ayarını açmak, ağ geçidi kullanımına göre günlük boyutunu önemli ölçüde artırabilir. Günlükleri incelemeyi bitirdikten sonra, uzun dönem için bu ayarı bırakmak yerine EmitQueryTraces 'i yeniden **yanlış** olarak sıfırladığınızdan emin olun.

   * **Microsoft. PowerBI. Datataşıması. Pipeline. Diagnostics. dll. config**

     Ağ geçidinizin, süreyi gösteren girişler de dahil olmak üzere ayrıntılı girişler sağlamak için, her iki adımı gerçekleştirerek **Tracingayrıntı düzeyi** değerlerini **4** ' ten **5** ' e değiştirin: 

     * Bu yapılandırma dosyasında, **Tracingayrıntı düzeyi** değerini **4** ' ten **5** ' e değiştirin 

       ```html
       <setting name="TracingVerbosity" serializeAs="String">
          <value>5</value>
       </setting>
       ```

     * Ağ Geçidi yükleyicisini açın, **Tanılama**' yı seçin, **ek günlüğü**açın ve ardından **Uygula**' yı seçin:

       ![Ek günlüğe kaydetmeyi aç](./media/logic-apps-gateway-install/turn-on-additional-logging.png)

     > [!IMPORTANT]
     > Tracingayrıntı düzeyi ayarını açmak, ağ geçidi kullanımına göre günlük boyutunu önemli ölçüde artırabilir. Günlükleri incelemeyi bitirdikten sonra, bu ayarı uzun vadede açmak yerine, ağ geçidi yükleyicisindeki **ek günlük kaydını** kapatmayı veya yapılandırma dosyasında Tracingayrıntı düzeyini yeniden **4** ' ü sıfırladığınızdan emin olun.

3. Bir sorgunun süresini bulmak için şu adımları izleyin:

   1. Ağ Geçidi günlüğünü [dışarı aktarıp](#logs) açın.

   2. Bir sorgu bulmak için bir etkinlik türü arayın, örneğin: 

      | Etkinlik türü | Açıklama | 
      |---------------|-------------| 
      | MGEQ | ADO.NET üzerinde çalışan sorgular. | 
      | MCOĞRAFI | OLEDB üzerinde çalışan sorgular. | 
      | MGED | Mashup altyapısından çalıştırılan sorgular. | 
      ||| 

   3. Istek KIMLIĞI olan ikinci GUID 'yi aklınızda edin.

   4. Süresi milisaniye olan "Fireactivitycompletedbaşarılı Fullyıevent" adlı bir giriş buluncaya kadar etkinlik türünü aramaya devam edin. 
   Girişin aynı Istek KIMLIĞINE sahip olduğunu onaylayın, örneğin:

      ```text 
      DM.EnterpriseGateway Verbose: 0 : 2016-09-26T23:08:56.7940067Z DM.EnterpriseGateway    baf40f21-2eb4-4af1-9c59-0950ef11ec4a    5f99f566-106d-c8ac-c864-c0808c41a606    MGEQ    21f96cc4-7496-bfdd-748c-b4915cb4b70c    B8DFCF12 [DM.Pipeline.Common.TracingTelemetryService] Event: FireActivityCompletedSuccessfullyEvent (duration=5004)
      ```

      > [!NOTE] 
      > "Fireactivitycompletedbaşarıyla Fullyıevent" girişi, ayrıntılı bir giriştir ve "Tracingayrıntı düzeyi" ayarı 5 düzeyinde olmadığı takdirde günlüğe kaydedilmez.

### <a name="trace-traffic-with-fiddler"></a>Fiddler ile trafiği izleme

[Fiddler](https://www.telerik.com/fiddler) , Telerik tarafından HTTP trafiğini izleyen ücretsiz bir araçtır. Bu trafiği istemci makinesinden Power BI hizmeti gözden geçirebilirsiniz. Bu hizmet hataları ve diğer ilgili bilgileri gösterebilir.

## <a name="next-steps"></a>Sonraki adımlar
    
* [Logic Apps 'ten şirket içi verilere bağlanma](../logic-apps/logic-apps-gateway-connection.md)
* [Kurumsal tümleştirme özellikleri](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Azure Logic Apps için Bağlayıcılar](../connectors/apis-list.md)
