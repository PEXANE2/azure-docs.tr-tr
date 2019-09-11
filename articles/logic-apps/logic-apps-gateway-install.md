---
title: Şirket içi veri ağ geçidini (Azure Logic Apps) yükler
description: Şirket içindeki verilere Azure Logic Apps erişmeden önce şirket içi veri ağ geçidini indirip yükleyin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: arthii, LADocs
ms.topic: article
ms.date: 07/01/2019
ms.openlocfilehash: 657bc704e33e89b1646dffa6123a27169e6c317a
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70860673"
---
# <a name="install-on-premises-data-gateway-for-azure-logic-apps"></a>Azure Logic Apps için şirket içi veri ağ geçidini yükler

Azure Logic Apps 'den şirket içi veri kaynaklarına bağlanabilmeniz için, şirket içi veri ağ geçidini yerel bir bilgisayara indirip yükleyin. Ağ Geçidi, Şirket içindeki (bulutta değil) ve mantıksal uygulamalarınızın veri kaynakları arasında hızlı veri aktarımı ve şifreleme sağlayan bir köprü olarak çalışmaktadır. Aynı ağ geçidi yüklemesini, Power BI, Microsoft Flow, PowerApps ve Azure Analysis Services gibi diğer bulut hizmetleriyle birlikte kullanabilirsiniz. Bu hizmetlerle ağ geçidini kullanma hakkında daha fazla bilgi için şu makalelere bakın:

* [Microsoft Power BI şirket içi veri ağ geçidi](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
* [Şirket içi veri ağ geçidini Microsoft PowerApps](https://powerapps.microsoft.com/tutorials/gateway-management/)
* [Şirket içi veri ağ geçidini Microsoft Flow](https://flow.microsoft.com/documentation/gateway-manage/)
* [Şirket içi veri ağ geçidini Azure Analysis Services](../analysis-services/analysis-services-gateway.md)

Bu makalede şirket içi veri ağ geçidinizi indirme, yükleme ve kurma işlemlerinin yanı sıra Azure Logic Apps ' dan şirket içi veri kaynaklarına erişebilirsiniz. Bu konunun ilerleyen kısımlarında [Data Gateway 'in nasıl çalıştığı](#gateway-cloud-service) hakkında daha fazla bilgi edinebilirsiniz.

<a name="supported-connections"></a>

Ağ Geçidi, bu veri kaynakları için Azure Logic Apps [Şirket içi bağlayıcıları](../connectors/apis-list.md#on-premises-connectors) destekler:

* BizTalk Server 2016
* Dosya sistemi
* IBM DB2  
* IBM Informix
* IBM MQ
* MySQL
* Oracle Database
* PostgreSQL
* SAP
* SharePoint Server
* SQL Server
* Teradata

Ağ Geçidi tek başına ek ücret uygulamaz, ancak [Logic Apps fiyatlandırma modeli](../logic-apps/logic-apps-pricing.md) bu bağlayıcılar ve Azure Logic Apps diğer işlemler için geçerlidir.

<a name="requirements"></a>

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

  * Ağ geçidini yüklemek ve yönetmek için aynı Azure hesabını kullanmanız gerekir. Yükleme sırasında, bilgisayarınızdaki ağ geçidini bir Azure aboneliğiyle ilişkilendirmek için bu Azure hesabını kullanırsınız. Daha sonra, ağ geçidi yüklemeniz için Azure portal Azure kaynağı oluştururken aynı Azure hesabını kullanırsınız. 

  * Bir *kuruluş* hesabı `username@contoso.com`olarak da bilinen bir iş hesabı veya okul hesabıyla oturum açmanız gerekir. Azure B2B (konuk) hesaplarını veya @hotmail.com veya @outlook.comgibi kişisel Microsoft hesaplarını kullanamazsınız.

    > [!TIP]
    > Office 365 teklifi için kaydolduysanız ve iş e-posta adresinizi sağlamadıysanız adresiniz gibi `username@domain.onmicrosoft.com`görünebilir. Hesabınız bir Azure Active Directory kiracı içinde depolanır (Azure AD). Çoğu durumda, Azure AD hesabınız için Kullanıcı asıl adı (UPN) e-posta adresiniz ile aynıdır.
    >
    > Microsoft hesabı ilişkili bir [Visual Studio standart aboneliğini](https://visualstudio.microsoft.com/vs/pricing/) kullanmak için, önce [Azure AD 'de bir kiracı oluşturun](../active-directory/develop/quickstart-create-new-tenant.md)ya da varsayılan dizini kullanın. Dizine bir parolası olan bir kullanıcı ekleyin ve bu kullanıcıya aboneliğinize erişim izni verin. 
    > Daha sonra bu Kullanıcı adı ve parolayla ağ geçidi yüklemesi sırasında oturum açabilirsiniz.

* Yerel bilgisayarınız için gereksinimler şunlardır:

  **Minimum gereksinimler**

  * .NET Framework 4.6
  * Windows 7 veya Windows Server 2008 R2 64-bit sürümü (veya üzeri)

  **Önerilen gereksinimler**

  * 8 çekirdekli CPU
  * 8 GB bellek
  * Windows Server 2012 R2 veya üzeri 64 bit sürümü
  * Biriktirme için katı hal sürücüsü (SSD) depolaması

  > [!NOTE]
  > Ağ Geçidi, Windows Server 2016 çekirdeğini desteklemez.

* **İlgili konular**

  * Şirket içi veri ağ geçidini, etki alanı denetleyicisi değil yalnızca yerel bir bilgisayara yükleyebilirsiniz. Ancak, ağ geçidini veri kaynağınıza göre aynı bilgisayara yüklemenize gerek yoktur. Tüm veri kaynaklarınız için yalnızca bir ağ geçidine ihtiyacınız vardır. bu nedenle, her veri kaynağı için ağ geçidini yüklemeniz gerekmez.

    > [!TIP]
    > Gecikme süresini en aza indirmek için, ağ geçidini veri kaynağınıza veya aynı bilgisayara mümkün olduğunca yakın bir şekilde yükleyebilirsiniz.

  * Ağ geçidini kablolu ağ üzerinde bulunan, internet 'e bağlı, her zaman açık olan ve uyku moduna geçmeyen bir bilgisayara yükler. Aksi takdirde, ağ geçidi çalıştırılamaz ve performans kablosuz bir ağdan düşebilir.

  * Windows kimlik doğrulamasını kullanmayı planlıyorsanız, ağ geçidini, veri kaynaklarınızla aynı Active Directory ortamına üye olan bir bilgisayara yüklediğinizden emin olun.

  * Ağ geçidinizin yüklemeniz için seçtiğiniz bölge, daha sonra mantıksal uygulamanız için Azure Gateway kaynağını oluştururken seçmeniz gereken konumdur. Bu bölge, varsayılan olarak Azure hesabınızı yöneten Azure AD kiracınızla aynı konumdadır. Ancak, ağ geçidi yüklemesi sırasında konumu değiştirebilirsiniz.

  * Ağ geçidinde iki mod vardır: yalnızca Power BI için geçerli olan standart mod ve kişisel mod. Aynı bilgisayarda aynı modda çalışan birden fazla ağ geçidi olamaz.

<a name="install-gateway"></a>

## <a name="install-data-gateway"></a>Veri ağ geçidi yükleme

1. [Ağ geçidi yükleyicisini yerel bir bilgisayarda indirip çalıştırın](https://aka.ms/on-premises-data-gateway-installer).

1. Yükleyici açıldıktan sonra **İleri**' yi seçin.

   ![Yükleyici girişi](./media/logic-apps-gateway-install/gateway-intro-screen.png)

1. Standart mod olan **Şirket içi veri ağ geçidini (önerilen)** seçin ve ardından **İleri**' yi seçin.

   ![Ağ Geçidi modunu seçin](./media/logic-apps-gateway-install/select-gateway-mode.png)

1. En düşük gereksinimleri gözden geçirin, varsayılan yükleme yolunu koruyun, kullanım koşullarını kabul edin ve ardından **yükleme**' yi seçin.

   ![Gereksinimleri gözden geçirin ve kullanım koşullarını kabul edin](./media/logic-apps-gateway-install/accept-terms.png)

1. Ağ Geçidi başarıyla yüklendikten sonra, kuruluş hesabınızın e-posta adresini girin ve **oturum aç**' ı seçin, örneğin:

   ![İş veya okul hesabıyla oturum açın](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

   Artık hesabınızda oturum açtınız.

1.  > **Sonra** **Bu bilgisayarda yeni bir ağ geçidi Kaydet '** i seçin. Bu adım ağ geçidi yükleme cihazınızı [ağ geçidi bulut hizmetine](#gateway-cloud-service)kaydeder.

   ![Ağ geçidini kaydetme](./media/logic-apps-gateway-install/register-gateway.png)

1. Ağ Geçidi yüklemeniz için şu bilgileri sağlayın:

   * Azure AD kiracınız genelinde benzersiz olan bir ağ geçidi adı
   * Kullanmak istediğiniz en az sekiz karakter olması gereken kurtarma anahtarı
   * Kurtarma anahtarınız için onay

   ![Ağ geçidini ayarlama](./media/logic-apps-gateway-install/set-up-gateway.png)

   > [!IMPORTANT]
   > Kurtarma Anahtarınızı güvenli bir yerde kaydedin ve saklayın. Konumu değiştirmek, taşımak, kurtarmak veya bir ağ geçidi yüklemesini almak istiyorsanız bu anahtara ihtiyacınız vardır.

   [Yüksek kullanılabilirlik senaryoları](#high-availability)için ek ağ geçitleri yüklerken seçtiğiniz **mevcut bir ağ geçidi kümesine ekleme**seçeneğini göz önünde bulabilirsiniz.

1. Ağ Geçidi Bulut hizmeti için bölgeyi ve ağ geçidi yüklemeniz tarafından kullanılan [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) denetleyin. Bu bölge, varsayılan olarak Azure hesabınız için Azure AD kiracısı ile aynı konumdadır.

   ![Bölgeyi denetle](./media/logic-apps-gateway-install/check-region.png)

1. Varsayılan bölgeyi kabul etmek için **Yapılandır**' ı seçin. Bununla birlikte, varsayılan bölge size en yakın olan bölge değilse, bölgeyi değiştirebilirsiniz.

   *Ağ Geçidi yüklemenizin bölgesi neden değiştirilsin?*

   Örneğin, gecikme süresini azaltmak için ağ geçidinizin bölgenizi mantıksal uygulamanızla aynı bölgeye göre değiştirebilirsiniz. Ya da şirket içi veri kaynağınıza en yakın bölgeyi seçebilirsiniz. *Azure 'daki ağ geçidi kaynağınızın* ve mantıksal uygulamanızın farklı konumları olabilir.

   1. Geçerli bölgenin yanındaki **bölgeyi değiştir**' i seçin.

      ![Bölge Değiştir](./media/logic-apps-gateway-install/change-region.png)

   1. Sonraki sayfada **Bölge seç** listesini açın, istediğiniz bölgeyi seçin ve **bitti**' yi seçin.

      ![Başka bir bölge seçin](./media/logic-apps-gateway-install/select-region-gateway-install.png)

1. Son onay penceresindeki bilgileri gözden geçirin. Bu örnek, Logic Apps, Power BI, PowerApps ve Microsoft Flow için aynı hesabı kullanır, bu nedenle ağ geçidi tüm bu hizmetler için kullanılabilir. Hazırsanız, **Kapat**' ı seçin.

   ![Ağ Geçidi tamamlandı](./media/logic-apps-gateway-install/finished-gateway-default-location.png)

1. Şimdi [ağ geçidi yüklemeniz Için Azure kaynağını oluşturun](../logic-apps/logic-apps-gateway-connection.md).

<a name="high-availability"></a>

## <a name="high-availability-support"></a>Yüksek kullanılabilirlik desteği

Şirket içi veri erişimi için tek hata noktalarından kaçınmak için, farklı bir bilgisayarda birden çok ağ geçidi yüklemesi (yalnızca standart mod) olabilir ve bunları bir küme veya grup olarak ayarlayabilirsiniz. Bu şekilde, birincil ağ geçidi kullanılamıyorsa, veri istekleri ikinci ağ geçidine yönlendirilir ve bu şekilde devam eder. Bir bilgisayara yalnızca bir standart ağ geçidi yükleyebildiğinden, kümedeki her ek ağ geçidini farklı bir bilgisayara yüklemelisiniz. Şirket içi veri ağ geçidiyle çalışan tüm bağlayıcılar yüksek kullanılabilirliği destekler. 

* Birincil ağ geçidi ile aynı Azure aboneliğinde en az bir ağ geçidi yüklemeniz ve ilgili yükleme için kurtarma anahtarı olmalıdır.

* Birincil ağ geçidinizin, Kasım 2017 veya sonraki bir sürümünün ağ geçidi güncelleştirmesini çalıştırmalıdır.

Birincil ağ geçidinizi ayarladıktan sonra, başka bir ağ geçidi yüklemeye gittiğinizde, **mevcut bir ağ geçidi kümesine ekle**' yi seçin, yüklediğiniz ilk ağ geçidi olan birincil ağ geçidini seçin ve bu ağ geçidi için kurtarma anahtarını sağlayın. Daha fazla bilgi için bkz. Şirket [içi veri ağ geçidi Için yüksek kullanılabilirlik kümeleri](https://docs.microsoft.com/data-integration/gateway/service-gateway-install#add-another-gateway-to-create-a-cluster).

<a name="update-gateway-installation"></a>

## <a name="change-location-migrate-restore-or-take-over-existing-gateway"></a>Konum değiştirme, geçirme, geri yükleme veya mevcut ağ geçidini alma

Ağ geçidinizin konumunu değiştirmeniz gerekiyorsa, ağ geçidi yüklemenizi yeni bir bilgisayara taşıyın, hasarlı bir ağ geçidini kurtarmanız veya mevcut bir ağ geçidi için sahiplik almanız gerekiyorsa, ağ geçidi yüklemesi sırasında sağlanmış olan kurtarma anahtarına ihtiyacınız vardır.

1. Ağ Geçidi yükleyicisini mevcut ağ geçidine sahip olan bilgisayarda çalıştırın. En son ağ geçidi yükleyiciniz yoksa, [en son ağ geçidi sürümünü indirin](https://aka.ms/on-premises-data-gateway-installer).

   > [!NOTE]
   > Ağ geçidini özgün ağ geçidi yüklemesi olan bilgisayara geri yüklemeden önce, önce o bilgisayardaki ağ geçidini kaldırmanız gerekir. Bu eylem, özgün ağ geçidinin bağlantısını keser.
   > Herhangi bir bulut hizmeti için bir ağ geçidi kümesini kaldırır veya silerseniz, bu kümeyi geri alamazsınız.

1. Yükleyici açıldıktan sonra, ağ geçidini yüklemek için kullanılan Azure hesabıyla oturum açın.

1.  >  **Var olan bir ağ geçidini geçir, geri yükle veya** **İleri**' yi seçin, örneğin:

   !["Var olan bir ağ geçidini geçir, geri yükle veya getir" i seçin](./media/logic-apps-gateway-install/migrate-recover-take-over-gateway.png)

1. Kullanılabilir kümeler ve ağ geçitleri arasından seçim yapın ve seçilen ağ geçidi için kurtarma anahtarını girin, örneğin:

   ![Ağ geçidini seçin](./media/logic-apps-gateway-install/select-existing-gateway.png)

1. Bölgeyi değiştirmek için **bölgeyi değiştir**' i seçin ve yeni bölgeyi seçin.

1. Hazırsanız, görevinizi tamamlayabilmeniz için **Yapılandır** ' ı seçin.

## <a name="configure-proxy-or-firewall"></a>Proxy veya güvenlik duvarını yapılandırma

İş ortamınız internet 'e erişmek için bir proxy 'den gider gerektiriyorsa, bu kısıtlama şirket içi veri ağ geçidinin ağ geçidi bulut hizmetine bağlanmasını engelleyebilir ve [Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md). Daha fazla bilgi için bkz. Şirket [içi veri ağ geçidi için ara sunucu ayarlarını yapılandırma](https://docs.microsoft.com/power-bi/service-gateway-proxy).

Proxy 'nizin veya güvenlik duvarının bağlantıları engelleyebileceğinden emin olmak için, bilgisayarınızın İnternet 'e gerçekten bağlanıp Azure Service Bus bağlanıp bağlanamadıklarını doğrulayın. Bir PowerShell isteminden şu komutu çalıştırın:

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

<a name="configure-ports"></a>

## <a name="configure-ports"></a>Bağlantı noktalarını yapılandırma

Ağ Geçidi Azure Service Bus ve giden bağlantı noktalarında iletişim kuran bir giden bağlantı oluşturur: TCP 443 (varsayılan), 5671, 5672, 9350 ile 9354. Ağ Geçidi, gelen bağlantı noktaları gerektirmez. [Azure Service Bus ve karma çözümler](../service-bus-messaging/service-bus-messaging-overview.md)hakkında daha fazla bilgi edinin.

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

Bazı durumlarda, Azure Service Bus bağlantıları, tam etki alanı adları yerine IP adresleriyle yapılır. Bu nedenle, güvenlik duvarınızdaki veri bölgeniz için IP adreslerinin engelini kaldırmak isteyebilirsiniz. Etki alanları yerine IP adreslerine erişime izin vermek için [Microsoft Azure veri MERKEZI IP aralıkları listesini](https://www.microsoft.com/download/details.aspx?id=41653)indirip kullanabilirsiniz. Bu listedeki IP adresleri, [sınıfsız etki alanları arası yönlendirme (CIDR)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösterimiyle bulunur.

### <a name="force-https-communication-with-azure-service-bus"></a>Azure Service Bus ile HTTPS iletişimini zorla

Bazı proxy 'ler yalnızca 80 ve 443 bağlantı noktalarında trafiğe izin verir. Varsayılan olarak, Azure Service Bus iletişim 443 dışındaki bağlantı noktalarında oluşur. Ağ geçidini doğrudan TCP yerine HTTPS üzerinden Azure Service Bus iletişim kurmaya zorlayabilirsiniz, ancak bunu yapmak performansı önemli ölçüde azaltabilir. Daha fazla bilgi için bkz. [Azure Service Bus Ile https Iletişimini zorlama](https://docs.microsoft.com/data-integration/gateway/service-gateway-communication#force-https-communication-with-azure-service-bus).

<a name="windows-service-account"></a>

## <a name="windows-service-account"></a>Windows hizmeti hesabı

Varsayılan olarak, yerel bilgisayarınızdaki ağ geçidi yüklemesi "Şirket içi veri ağ geçidi hizmeti" adlı bir Windows hizmet hesabı olarak çalışır. Ancak ağ geçidi yüklemesi, "oturum `NT SERVICE\PBIEgwService` aç" hesabı kimlik bilgileri için adı kullanır ve "hizmet olarak oturum aç" izinlerine sahiptir.

> [!NOTE]
> Windows hizmet hesabınız, şirket içi veri kaynaklarına bağlanmak için kullanılan hesaptan ve bulut hizmetlerinde oturum açarken kullandığınız Azure hesabından farklılık gösterir.

<a name="restart-gateway"></a>

## <a name="restart-gateway"></a>Ağ geçidini yeniden Başlat

Veri ağ geçidi bir pencere hizmeti olarak çalışır, bu nedenle diğer Windows Hizmetleri gibi, ağ geçidini çeşitli yollarla başlatabilir ve durdurabilirsiniz. Daha fazla bilgi için bkz. Şirket [içi veri ağ geçidini yeniden başlatma](https://docs.microsoft.com/data-integration/gateway/service-gateway-restart).

## <a name="tenant-level-administration"></a>Kiracı düzeyinde yönetim

Bir Azure AD kiracısındaki tüm şirket içi veri ağ geçitlerine ilişkin görünürlük almak için, söz konusu Kiracıdaki Genel Yöneticiler, [Power Platform Yönetim merkezinde](https://powerplatform.microsoft.com) kiracı yöneticisi olarak oturum açabilir ve **veri ağ geçitleri** seçeneğini seçebilir. Daha fazla bilgi için bkz. Şirket [içi veri ağ geçidi Için kiracı düzeyinde yönetim](https://docs.microsoft.com/data-integration/gateway/service-gateway-tenant-level-admin).

<a name="gateway-cloud-service"></a>

## <a name="how-the-gateway-works"></a>Ağ geçidinin çalışması

Veri ağ geçidi, mantıksal uygulamanız, ağ geçidi bulut hizmeti ve şirket içi veri kaynağınız arasındaki hızlı ve güvenli iletişimi kolaylaştırır. Ağ Geçidi bulutu hizmeti, veri kaynağı kimlik bilgilerinizi ve ağ geçidi ayrıntılarını şifreler ve depolar. Hizmet Ayrıca sorguları ve bunların sonuçlarını mantıksal uygulamanız, şirket içi veri ağ geçidi ve şirket içi veri kaynağınız arasında yönlendirir.

Ağ Geçidi, güvenlik duvarları ile birlikte çalışarak yalnızca giden bağlantıları kullanır. Tüm trafik ağ geçidi aracısından güvenli giden trafik olarak gelir. Ağ Geçidi, şifreli kanallardaki şirket içi kaynaklardaki verileri Azure Service Bus aracılığıyla geçirir. Bu hizmet veri yolu, ağ geçidi ile çağıran hizmet arasında bir kanal oluşturur, ancak herhangi bir veri depolamaz. Ağ Geçidi üzerinden taşınan tüm veriler şifrelenir.

![Şirket içi veri ağ geçidi mimarisi](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

Bu adımlarda, buluttaki bir kullanıcı şirket içi veri kaynağınıza bağlı bir öğeyle etkileşim kurduğunda ne olacağı açıklanır:

1. Ağ Geçidi bulutu hizmeti, veri kaynağı için şifrelenmiş kimlik bilgileriyle birlikte bir sorgu oluşturur ve ağ geçidinin işlemesi için sorguyu kuyruğa gönderir.

1. Ağ Geçidi Bulut hizmeti sorguyu analiz eder ve isteği Azure Service Bus gönderir.

1. Şirket içi veri ağ geçidi, bekleyen istekler için Azure Service Bus yoklar.

1. Ağ Geçidi sorguyu alır, kimlik bilgilerinin şifresini çözer ve bu kimlik bilgileriyle veri kaynağına bağlanır.

1. Ağ Geçidi, yürütme için sorguyu veri kaynağına gönderir.

1. Sonuçlar veri kaynağından ağ geçidine ve sonra ağ geçidi bulut hizmetine gönderilir. Daha sonra ağ geçidi bulutu hizmeti sonuçları kullanır.

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

* Ağ Geçidi yüklemeniz zaten kayıtlı ve Azure 'daki başka bir ağ geçidi kaynağı tarafından talep edildi. Ağ Geçidi yüklemeleri, ağ geçidi kaynakları oluşturulduktan sonra örnekler listesinde görünmez. Azure portal ağ geçidi kayıtlarınızı denetlemek *için tüm Azure abonelikleri için* **Şirket Içi veri ağ geçitleri** türü ile tüm Azure kaynaklarınızı gözden geçirin.

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

1. Sol taraftaki menüden **Tanılama**' yı seçin.

1. **Ağ geçidi günlükleri**altında, **günlükleri dışarı aktar**' ı seçin.

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

1. **Olay Görüntüleyicisi (yerel)**  > **uygulamalar ve hizmet günlükleri '** ni genişletin.

1. Şirket **içi veri ağ geçidi hizmeti '** ni seçin.

   ![Ağ Geçidi için olay günlüklerini görüntüleme](./media/logic-apps-gateway-install/event-viewer.png)

### <a name="review-slow-query-performance"></a>Yavaş sorgu performansını gözden geçirme

Ağ geçidinde yavaş çalışan sorgular bulursanız, sorguları ve bunların sürelerini veren ek günlüğe kaydetme özelliğini açabilirsiniz. Bu Günlükler hangi sorguların yavaş veya uzun süre çalıştığını bulmanıza yardımcı olur. Sorgu performansını ayarlamak için veri kaynağınızı değiştirmeniz gerekebilir; Örneğin, SQL Server sorguları için dizinleri ayarlayın.

Bir sorgunun süresini öğrenmek için şu adımları izleyin:

1. Ağ Geçidi istemcisiyle aynı konuma gidin ve genellikle burada bulabilirsiniz:```C:\Program Files\On-premises data gateway```

   Aksi takdirde, istemci konumunu bulmak için aynı bilgisayarda Hizmetler konsolunu açın, Şirket **içi veri ağ geçidi hizmeti**' ni bulun ve **yürütülebilir özelliğin yolunu** görüntüleyin.

1. Bu yapılandırma dosyalarını açın ve açıklandığı gibi düzenleyin:

   * **Microsoft. PowerBI. Datataşıması. Pipeline. GatewayCore. dll. config**

     Bu dosyada, ağ geçidiniz ağ geçidinden bir veri kaynağına gönderilen sorguları günlüğe kaydetmek için **EmitQueryTraces** değerini **false** **olarak değiştirin** :

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

1. Bir sorgunun süresini bulmak için şu adımları izleyin:

   1. Ağ Geçidi günlüğünü [dışarı aktarıp](#logs) açın.

   1. Bir sorgu bulmak için bir etkinlik türü arayın, örneğin:

      | Etkinlik türü | Açıklama |
      |---------------|-------------|
      | MGEQ | ADO.NET üzerinde çalışan sorgular |
      | MCOĞRAFI | OLEDB üzerinde çalışan sorgular |
      | MGED | Karma altyapıdan çalışan sorgular |
      |||

   1. Istek KIMLIĞI olan ikinci GUID 'yi aklınızda edin.

   1. Süresi milisaniye olan "Fireactivitycompletedbaşarılı Fullyıevent" adlı bir giriş buluncaya kadar etkinlik türünü aramaya devam edin. Girişin aynı Istek KIMLIĞINE sahip olduğunu onaylayın, örneğin:

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
