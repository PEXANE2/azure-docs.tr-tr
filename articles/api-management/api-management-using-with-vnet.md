---
title: Sanal ağlar ile Azure API Management’ı kullanma
description: Azure API Yönetimi'nde sanal ağa nasıl bağlantı kurabileceğinizi ve web hizmetlerine nasıl erişerek eriştin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 03/09/2020
ms.author: apimpm
ms.openlocfilehash: dcc2c38238f707a5d43cde03502c589add9461b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335919"
---
# <a name="how-to-use-azure-api-management-with-virtual-networks"></a>Sanal ağlar ile Azure API Management’ı kullanma
Azure Sanal Ağları (VNET’ler) Azure kaynaklarınızdan herhangi birini, erişimini denetlediğiniz İnternet tabanlı olmayan ve yönlendirilebilir bir ağa yerleştirmenizi sağlar. Bu ağlar daha sonra çeşitli VPN teknolojileri kullanılarak şirket içi ağlarınıza bağlanabilir. Azure Sanal Ağlar hakkında daha fazla bilgi edinmek için buradaki bilgilerle başlayın: [Azure Sanal Ağa Genel Bakış.](../virtual-network/virtual-networks-overview.md)

Azure API Yönetimi sanal ağ (VNET) içinde dağıtılabilir, böylece ağ içindeki arka uç hizmetlerine erişebilir. Geliştirici portalı ve API ağ geçidi, Internet'ten veya yalnızca sanal ağ içinde erişilebilir olacak şekilde yapılandırılabilir.

> [!NOTE]
> API alma belgesi URL'si, herkesin erişebileceği bir internet adresinde barındırılmalıdır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [premium-dev.md](../../includes/api-management-availability-premium-dev.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu makalede açıklanan adımları gerçekleştirmek için şunları yapmalısınız:

+ Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Bir APIM örneği. Daha fazla bilgi için [bkz.](get-started-create-service-instance.md)

## <a name="enable-vnet-connection"></a><a name="enable-vpn"> </a>VNET bağlantısını etkinleştirme

### <a name="enable-vnet-connectivity-using-the-azure-portal"></a>Azure portalını kullanarak VNET bağlantısını etkinleştirme

1. API yönetimi örneğini bulmak için [Azure portalına](https://portal.azure.com) gidin. **API Yönetimi hizmetlerini**arayın ve seçin.

2. API Yönetimi örneğini seçin.

3. **Sanal ağı**seçin.
4. SANAL ağ içinde dağıtılacak API Yönetimi örneğini yapılandırın.

    ![API Yönetimi sanal ağ menüsü][api-management-using-vnet-menu]
5. İstenilen erişim türünü seçin:

    * **Kapalı**: Bu varsayılandır. API Yönetimi sanal bir ağa dağıtılanmaz.

    * **Harici**: API Yönetimi ağ geçidi ne de geliştirici portalına harici yük dengeleyicisi aracılığıyla genel internetten erişilebilir. Ağ geçidi sanal ağ içindeki kaynaklara erişebilir.

        ![Ortak eşleme][api-management-vnet-public]

    * **Dahili**: API Yönetimi ağ geçidine ve geliştirici portalına yalnızca sanal ağ içinden bir dahili yük dengeleyicisi aracılığıyla erişilebilir. Ağ geçidi sanal ağ içindeki kaynaklara erişebilir.

        ![Özel eşleme][api-management-vnet-private]

6. **Harici** veya **Dahili'yi**seçtiyseniz, API Yönetimi hizmetinizin sağlandığı tüm bölgelerin listesini görürsünüz. Bir **Konum**seçin ve ardından **Sanal ağ** ve **Subnet'ini**seçin. Sanal ağ listesi, yapılandırma yaptığınız bölgede ayarlanan Azure aboneliklerinizde bulunan hem klasik hem de Kaynak Yöneticisi sanal ağlarıyla doldurulur.

    > [!IMPORTANT]
    > Bir Kaynak Yöneticisi VNET'e Azure API Yönetimi örneğini dağıtırken, hizmetin Azure API Yönetimi örnekleri dışında başka kaynak içermeyen özel bir alt ağda olması gerekir. Azure API Yönetimi örneğini diğer kaynakları içeren kaynak yöneticisi VNET alt ağına dağıtma girişiminde bulunulması durumunda, dağıtım başarısız olur.

    Ardından **Uygula**'yı seçin. API Yönetimi örneğinizin **Sanal ağ** sayfası yeni sanal ağınız ve alt ağ seçeneklerinizle güncelleştirilir.

    ![VPN'i seçin][api-management-setup-vpn-select]

7. Üstteki gezinti çubuğunda **Kaydet'i**ve ardından **ağ yapılandırmasını uygula'yı**seçin.

> [!NOTE]
> API Yönetimi örneğinin VIP adresi, VNET her etkinleştirildiğinde veya devre dışı bırakıldığında değişecektir.
> VIP adresi, API Yönetimi **Dıştan** **İçe**veya Tam tersi bir şekilde taşındığında da değişecektir.
>

> [!IMPORTANT]
> API Yönetimi'ni bir VNET'ten kaldırırsanız veya dağıtılan ı değiştirirseniz, daha önce kullanılan VNET altı saate kadar kilitli kalabilir. Bu süre zarfında VNET'i silmek veya ona yeni bir kaynak dağıtmak mümkün olmayacaktır. Bu davranış api sürümü 2018-01-01 ve daha önceki kullanan istemciler için geçerlidir. Api sürümü 2019-01-01 ve daha sonra kullanan istemciler, ilişkili API Yönetimi hizmeti silinir silinmez VNET serbest bırakılır.

## <a name="enable-vnet-connection-using-powershell-cmdlets"></a><a name="enable-vnet-powershell"> </a>PowerShell cmdlets kullanarak VNET bağlantısını etkinleştirme
PowerShell cmdlets'i kullanarak VNET bağlantısını da etkinleştirebilirsiniz.

* **VNET içinde bir API Yönetimi hizmeti oluşturun**: Bir VNET içinde Azure API Yönetimi hizmeti oluşturmak için cmdlet [New-AzApiManagement'ı](/powershell/module/az.apimanagement/new-azapimanagement) kullanın.

* **VNET içinde varolan bir API Yönetimi hizmetini dağıtın**: Varolan bir Azure API Yönetimi hizmetini Sanal Ağ'a taşımak için cmdlet [Update-AzApiManagementRegion'u](/powershell/module/az.apimanagement/update-azapimanagementregion) kullanın.

## <a name="connect-to-a-web-service-hosted-within-a-virtual-network"></a><a name="connect-vnet"> </a>Sanal Ağ içinde barındırılan bir web hizmetine bağlanma
API Yönetimi hizmetiniz VNET'e bağlandıktan sonra, arka uç hizmetlerine erişmek kamu hizmetlerine erişmekten farklı değildir. Yeni bir API oluştururken veya varolan bir api düzenlerken web hizmetinizin yerel IP adresini veya ana bilgisayar adını (VNET için bir DNS sunucusu yapılandırılmışsa) **Web hizmeti NIN URL** alanına yazmanız gerekir.

![VPN'den API ekleme][api-management-setup-vpn-add-api]

## <a name="common-network-configuration-issues"></a><a name="network-configuration-issues"> </a>Ortak Ağ Yapılandırma Sorunları
Aşağıda, API Yönetimi hizmetini Sanal Ağa dağıtırken oluşabilecek yaygın yanlış yapılandırma sorunlarının bir listesi vereyim.

* **Özel DNS sunucu kurulumu**: API Yönetimi hizmeti birkaç Azure hizmetine bağlıdır. API Yönetimi, özel bir DNS sunucusuna sahip bir VNET'te barındırıldığında, bu Azure hizmetlerinin ana bilgisayar adlarını çözmesi gerekir. Lütfen özel DNS kurulumu ile ilgili [bu](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) kılavuzu izleyin. Aşağıdaki bağlantı noktaları tablosuna ve başvuru için diğer ağ gereksinimlerine bakın.

> [!IMPORTANT]
> VNET için özel bir DNS Server(lar) kullanmayı planlıyorsanız, içine bir API Yönetimi hizmeti dağıtmadan **önce** bu sunucuyu ayarlamanız gerekir. Aksi takdirde, [Ağ Yapılandırma İşlemini Çalıştırarak](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/ApiManagementService/ApplyNetworkConfigurationUpdates) DNS Server(ları) her değiştirinizde API Yönetimi hizmetini güncelleştirmeniz gerekir

* **API Yönetimi için gerekli bağlantı noktaları**: API Yönetimi'nin dağıtıldığı Subnet'e gelen ve giden trafik Ağ Güvenlik [Grubu][Network Security Group]kullanılarak denetlenebilir. Bu bağlantı noktalarından herhangi biri kullanılamıyorsa, API Yönetimi düzgün çalışmayabilir ve erişilemez hale gelebilir. Bu bağlantı noktalarından birinin veya daha fazlasının engellenmesi, Bir VNET ile API Yönetimi kullanırken başka bir yaygın yanlış yapılandırma sorunudur.

<a name="required-ports"> </a> Bir API Yönetimi hizmeti örneği bir VNET'te barındırıldığında, aşağıdaki tablodaki bağlantı noktaları kullanılır.

| Kaynak / Hedef Bağlantı Noktası(lar) | Yön          | Ulaşım protokolü |   [Servis Etiketleri](../virtual-network/security-overview.md#service-tags) <br> Kaynak / Hedef   | Amaç (*)                                                 | Sanal Ağ türü |
|------------------------------|--------------------|--------------------|---------------------------------------|-------------------------------------------------------------|----------------------|
| * / [80], 443                  | Gelen            | TCP                | İnternet / VIRTUAL_NETWORK            | API Yönetimine müşteri iletişimi                      | Dış             |
| * / 3443                     | Gelen            | TCP                | ApiManagement / VIRTUAL_NETWORK       | Azure portalı ve Powershell için yönetim bitiş noktası         | Dış & İç  |
| * / 443                  | Giden           | TCP                | VIRTUAL_NETWORK / Depolama             | **Azure Depolamaya Bağımlılık**                             | Dış & İç  |
| * / 443                  | Giden           | TCP                | VIRTUAL_NETWORK / AzureActiveDirectory | [Azure Etkin Dizini](api-management-howto-aad.md) (varsa)                   | Dış & İç  |
| * / 1433                     | Giden           | TCP                | VIRTUAL_NETWORK / SQL                 | **Azure SQL uç noktalarına erişim**                           | Dış & İç  |
| * / 5671, 5672, 443          | Giden           | TCP                | VIRTUAL_NETWORK / EventHub            | [Olay Hub ilkesi](api-management-howto-log-event-hubs.md) ve izleme aracısı için Günlük Bağımlılık | Dış & İç  |
| * / 445                      | Giden           | TCP                | VIRTUAL_NETWORK / Depolama             | [GIT](api-management-configuration-repository-git.md) için Azure Dosya Paylaşımına Bağımlılık                      | Dış & İç  |
| * / 1886                     | Giden           | TCP                | VIRTUAL_NETWORK / AzureCloud            | Kaynak Durumu'na Sağlık durumunu yayınlamak için gerekli          | Dış & İç  |
| * / 443                     | Giden           | TCP                | VIRTUAL_NETWORK / AzureMonitor         | [Tanılama Günlükleri ve Ölçümleri](api-management-howto-use-azure-monitor.md) Yayımla                       | Dış & İç  |
| * / 25                       | Giden           | TCP                | VIRTUAL_NETWORK / İnternet            | E-posta göndermek için SMTP Rölesine bağlanın                    | Dış & İç  |
| * / 587                      | Giden           | TCP                | VIRTUAL_NETWORK / İnternet            | E-posta göndermek için SMTP Rölesine bağlanın                    | Dış & İç  |
| * / 25028                    | Giden           | TCP                | VIRTUAL_NETWORK / İnternet            | E-posta göndermek için SMTP Rölesine bağlanın                    | Dış & İç  |
| * / 6381 - 6383              | Gelen & Giden | TCP                | VIRTUAL_NETWORK / VIRTUAL_NETWORK     | Makineler arasındaki [Oran Sınırı](api-management-access-restriction-policies.md#LimitCallRateByKey) ilkeleri için Redis Hizmetine erişin         | Dış & İç  |
| * / *                        | Gelen            | TCP                | AZURE_LOAD_BALANCER / VIRTUAL_NETWORK | Azure Altyapı Yük Dengeleyicisi                          | Dış & İç  |

>[!IMPORTANT]
> *API* Yönetimi hizmetinin başarıyla dağıtılabilmesi için Amacın **kalın** olduğu bağlantı noktaları gereklidir. Ancak diğer bağlantı noktalarının engellenmesi, çalışan hizmeti kullanma ve izleme yeteneğinde bozulmaya neden olur.

+ **TLS işlevselliği**: TLS/SSL sertifika zinciri oluşturma ve doğrulama yı etkinleştirmek için API Management hizmetinin ocsp.msocsp.com, mscrl.microsoft.com ve crl.microsoft.com giden ağ bağlantısına ihtiyacı vardır. API Yönetimi'ne yüklediğiniz herhangi bir sertifika CA köküne tam zincir içeriyorsa, bu bağımlılık gerekli değildir.

+ **DNS Access : DNS**sunucuları ile iletişim için bağlantı noktası 53'te giden erişim gereklidir. VPN ağ geçidinin diğer ucunda özel bir DNS sunucusu varsa, DNS sunucusuna API Yönetimi barındırma alt ağlarından erişilebilmelidir.

+ **Ölçümler ve Sistem Durumu İzleme**: Aşağıdaki etki alanları altında çözüme kavuşturulan Azure İzleme uç noktalarına giden ağ bağlantısı:

    | Azure Ortamı | Uç Noktalar                                                                                                                                                                                                                                                                                                                                                              |
    |-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Azure Genel      | <ul><li>gcs.prod.monitoring.core.windows.net (**yeni**)</li><li>prod.warmpath.msftcloudes.com (**amortismana alınacak)**</li><li>shoebox2.metrics.microsoftmetrics.com (**yeni**)</li><li>shoebox2.metrics.nsatc.net (**amortismana alınacak)**</li><li>prod3.metrics.microsoftmetrics.com (**yeni**)</li><li>prod3.metrics.nsatc.net (**amortismana alınacak**)</li><li>prod3-black.prod3.metrics.microsoftmetrics.com (**yeni**)</li><li>prod3-black.prod3.metrics.nsatc.net (**amortismana alınacak)**</li><li>prod3-red.prod3.metrics.microsoftmetrics.com (**yeni**)</li><li>prod3-red.prod3.metrics.nsatc.net **(amortismana alınacak)**</li><li>prod.warm.ingestion.msftcloudes.com</li><li>`azure region`.warm.ingestion.msftcloudes.com `East US 2` nerede eastus2.warm.ingestion.msftcloudes.com</li></ul> |
    | Azure Kamu  | <ul><li>fairfax.warmpath.usgovcloudapi.net</li><li>shoebox2.metrics.microsoftmetrics.com (**yeni**)</li><li>shoebox2.metrics.nsatc.net (**amortismana alınacak)**</li><li>prod3.metrics.microsoftmetrics.com (**yeni**)</li><li>prod3.metrics.nsatc.net (**amortismana alınacak**)</li><li>prod5.prod.microsoftmetrics.com</li></ul>                                                                                                                                                                                                                                                |
    | Azure Çin 21Vianet     | <ul><li>mooncake.warmpath.chinacloudapi.cn</li><li>shoebox2.metrics.microsoftmetrics.com (**yeni**)</li><li>shoebox2.metrics.nsatc.net (**amortismana alınacak)**</li><li>prod3.metrics.microsoftmetrics.com (**yeni**)</li><li>prod3.metrics.nsatc.net (**amortismana alınacak**)</li><li>prod5.prod.microsoftmetrics.com</li></ul>                                                                                                                                                                                                                                                |

>[!IMPORTANT]
> DNS bölgesi **.nsatc.net** ile **.microsoftmetrics.com'a** yukarıdaki kümelerin değişimi çoğunlukla bir DNS Değişikliğidir. Kümenin IP Adresi değişmez.

+ **SMTP Rölesi**: SMTP Rölesi için giden `smtpi-co1.msn.com` `smtpi-ch1.msn.com`ağ `smtpi-db3.msn.com` `smtpi-sin.msn.com` bağlantısı, ana bilgisayar altında çözülür , , ve`ies.global.microsoft.com`

+ **Geliştirici portalı CAPTCHA**: Geliştirici portalının CAPTCHA'sı için giden ağ `client.hip.live.com` bağlantısı, ana bilgisayarların altında çözülür ve `partner.hip.live.com`.

+ **Azure portalı Tanılama**: Sanal Ağ içinden API Yönetimi uzantısını kullanırken Azure portalından tanılama günlüklerinin akışını etkinleştirmek için, `dc.services.visualstudio.com` 443 no'lu bağlantı noktasına giden erişim gereklidir. Bu, uzantıyı kullanırken karşılaşabileceğiniz sorun giderme sorunlarına yardımcı olur.

+ Express Route veya Network Virtual Appliance kullanarak Trafiği **Ön-Prem'e ZorLamak**: Ortak bir müşteri yapılandırması, API Yönetimi'nin devrettiği alt ağdaki tüm trafiği şirket içi bir güvenlik duvarından veya Ağ sanal cihazından akmaya zorlayan kendi varsayılan rotalarını (0.0.0.0/0) tanımlamaktır. Giden trafik şirket içinde engellenmiş olduğundan veya artık çeşitli Azure uç noktalarıyla çalışmayan tanınmaz bir adres kümesine NAT'd olduğundan, bu trafik akışı Azure API Yönetimi ile bağlantıyı her zaman kırar. Çözüm birkaç şey yapmanız gerektiğini:

  * API Yönetimi hizmetinin dağıtıldığı alt ağda hizmet uç noktalarını etkinleştirin. Azure Sql, Azure Depolama, Azure EventHub ve Azure ServiceBus için [Hizmet Bitiş Noktalarının][ServiceEndpoints] etkinleştirilmesi gerekir. Uç noktaları doğrudan API Yönetimi'nin bu hizmetlere devredilen alt ağlarından etkinleştirmek, hizmet trafiği için en iyi yönlendirmeyi sağlayan Microsoft Azure omurga ağını kullanmalarına olanak tanır. Zorla tünellenmiş Api Yönetimi ile Hizmet Bitiş Noktaları kullanıyorsanız, yukarıdaki Azure hizmetleri trafiği tünele atılmaz. Diğer API Yönetimi hizmet bağımlılık trafiği tünele zorlanır ve kaybedilemez veya API Yönetimi hizmeti düzgün çalışmaz.
    
  * Internet'ten API Yönetimi hizmetinizin yönetim bitiş noktasına kadar olan tüm kontrol uçağı trafiği, API Yönetimi tarafından barındırılan belirli bir Gelen IP'ler kümesi aracılığıyla yönlendirilir. Trafik zorla tünel kazıldığında, yanıtlar bu Gelen kaynak IP'lerine simetrik olarak eşlenmez. Sınırlamayı aşmak için, bu ana bilgisayar yollarının hedefini "Internet"e ayarlayarak trafiği Azure'a yönlendirmek için aşağıdaki kullanıcı tanımlı yolları[(ÜDR)][UDRs]eklememiz gerekir. Kontrol uçak trafiği için Gelen IP'ler kümesi Belgelenmiştir [Kontrol Uçağı IP Adresleri](#control-plane-ips)

  * Zorla tünele giren diğer API Yönetimi hizmet bağımlılıkları için, ana bilgisayar adını çözmenin ve bitiş noktasına ulaşmanın bir yolu olmalıdır. Bunlar arasında
      - Ölçümler ve Sağlık İzleme
      - Azure portalı Tanılama
      - SMTP Röle
      - Geliştirici portalı CAPTCHA

## <a name="troubleshooting"></a><a name="troubleshooting"> </a>Sorun giderme
* **İlk Kurulum**: API Yönetimi hizmetinin bir alt ağa ilk dağıtımı başarılı olmadığında, önce aynı alt ağa sanal bir makine dağıtması önerilir. Sanal makineye sonraki uzak masaüstü ve azure aboneliğinizde aşağıdaki her kaynaktan birine bağlantı olduğunu doğrulayın
    * Azure Depolama blob
    * Azure SQL Veritabanı
    * Azure Depolama Tablosu

  > [!IMPORTANT]
  > Bağlantıyı doğruladıktan sonra, API Yönetimi'ni alt ağa dağıtmadan önce alt ağda dağıtılan tüm kaynakları kaldırdığınızdan emin olun.

* **Artımlı Güncelleştirmeler**: Ağınızda değişiklik yaparken, API Yönetimi hizmetinin bağlı olduğu kritik kaynaklardan herhangi birinde erişimini kaybetmediğini doğrulamak için [NetworkStatus API'ye](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/networkstatus)bakın. Bağlantı durumu her 15 dakikada bir güncelleştirilmelidir.

* **Kaynak Gezinme Bağlantıları**: Kaynak Yöneticisi stili vnet alt ağına dağıtılırken, API Yönetimi bir kaynak gezinti bağlantısı oluşturarak alt ağı ayırır. Alt ağ zaten farklı bir sağlayıcıdan bir kaynak içeriyorsa, dağıtım **başarısız**olur. Benzer şekilde, bir API Yönetimi hizmetini farklı bir alt ağa taşıdığınızda veya sildiğinizde, bu kaynak gezinti bağlantısını kaldırırız.

## <a name="subnet-size-requirement"></a><a name="subnet-size"> </a> Alt Ağ Boyutu Gereksinimi
Azure her alt ağda bazı IP adresleri ayırır ve bu adresler kullanılamaz. Alt ağların ilk ve son IP adresleri protokol uyumluluğu için ayrılmış ve Azure hizmetleri için kullanılan üç adres daha vardır. Daha fazla bilgi için, bu [alt ağlar içinde IP adresleri nin kullanılmasıyla ilgili herhangi bir kısıtlama var mı?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Azure VNET altyapısı tarafından kullanılan IP adreslerine ek olarak, alt netteki her Api Yönetimi örneği Premium SKU birimi başına iki IP adresi veya Geliştirici SKU için bir IP adresi kullanır. Her örnek, harici yük dengeleyicisi için ek bir IP adresi saklı tutar. Dahili sanal ağa dağıtılırken, dahili yük dengeleyicisi için ek bir IP adresi gerektirir.

API Yönetimi'nin dağıtılabildiği alt netin minimum boyutunun üzerindeki hesaplama /29'dur ve bu üç kullanılabilir IP adresi verir.

API Yönetimi'nin her ek ölçek birimi için iki IP adresi daha gerekirse.

## <a name="routing"></a><a name="routing"> </a> Yönlendirme
+ Tüm hizmet bitiş noktalarına erişim sağlamak için yük dengeli genel IP adresi (VIP) ayrılmıştır.
+ Vnet içindeki kaynaklara erişmek için bir alt ağ IP aralığından (DIP) bir IP adresi ve vnet dışındaki kaynaklara erişmek için ortak bir IP adresi (VIP) kullanılacaktır.
+ Yük dengeli genel IP adresi, Azure portalındaki Genel Bakış/Essentials bıçak üzerinde bulunabilir.

## <a name="limitations"></a><a name="limitations"> </a>Sınırlamalar
* API Yönetimi örnekleri içeren bir alt ağ başka Azure kaynak türleri içeremez.
* Alt ağ ve API Yönetimi hizmeti aynı abonelikte olmalıdır.
* API Yönetimi örneklerini içeren bir alt ağ abonelikler arasında taşınamaz.
* İç sanal ağ modunda yapılandırılan çok bölgeli API Yönetimi dağıtımları için, kullanıcılar yönlendirmeye sahip oldukları için birden çok bölgede yük dengelemesini yönetmekten sorumludur.
* Başka bir bölgedeki genel olarak görüntülenen VNET'teki bir kaynaktan İç moddaki API Yönetimi hizmetine bağlantı, platform sınırlaması nedeniyle çalışmaz. Daha fazla bilgi için bkz: [Tek bir sanal ağdaki Kaynaklar, bakan sanal ağdaki Azure dahili yük dengeleyicisiyle iletişim kuramıyor](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints)

## <a name="control-plane-ip-addresses"></a><a name="control-plane-ips"> </a> Kontrol Düzlemi IP Adresleri

IP Adresleri **Azure Ortamı'na**bölünür. **Global** ile işaretlenmiş gelen isteklere izin verirken, **Bölgeye** özgü IP Adresi ile birlikte beyaz listeye alınmalıdır.

| **Azure Ortamı**|   **Bölge**|  **IP adresi**|
|-----------------|-------------------------|---------------|
| Azure Genel| Güney Orta ABD (Küresel)| 104.214.19.224|
| Azure Genel| Kuzey Orta ABD (Küresel)| 52.162.110.80|
| Azure Genel| Orta Batı ABD| 52.253.135.58|
| Azure Genel| Güney Kore - Orta| 40.82.157.167|
| Azure Genel| Batı Birleşik Krallık| 51.137.136.0|
| Azure Genel| Batı Japonya| 40.81.185.8|
| Azure Genel| Orta Kuzey ABD| 40.81.47.216|
| Azure Genel| Güney Birleşik Krallık| 51.145.56.125|
| Azure Genel| Batı Hindistan| 40.81.89.24|
| Azure Genel| Doğu ABD| 52.224.186.99|
| Azure Genel| Batı Avrupa| 51.145.179.78|
| Azure Genel| Doğu Japonya| 52.140.238.179|
| Azure Genel| Orta Fransa| 40.66.60.111|
| Azure Genel| Doğu Kanada| 52.139.80.117|
| Azure Genel| BAE Kuzey| 20.46.144.85|
| Azure Genel| Güney Brezilya| 191.233.24.179|
| Azure Genel| Güneydoğu Asya| 40.90.185.46|
| Azure Genel| Güney Afrika Kuzey| 102.133.130.197|
| Azure Genel| Orta Kanada| 52.139.20.34|
| Azure Genel| Güney Kore - Güney| 40.80.232.185|
| Azure Genel| Orta Hindistan| 13.71.49.1|
| Azure Genel| Batı ABD| 13.64.39.16|
| Azure Genel| Güneydoğu Avustralya| 20.40.160.107|
| Azure Genel| Orta Avustralya| 20.37.52.67|
| Azure Genel| Güney Hindistan| 20.44.33.246|
| Azure Genel| Orta ABD| 13.86.102.66|
| Azure Genel| Doğu Avustralya| 20.40.125.155|
| Azure Genel| Batı ABD 2| 51.143.127.203|
| Azure Genel| Doğu ABD 2 EUAP| 52.253.229.253|
| Azure Genel| ORTA ABD EUAP| 52.253.159.160|
| Azure Genel| Orta Güney ABD| 20.188.77.119|
| Azure Genel| Doğu ABD 2| 20.44.72.3|
| Azure Genel| Kuzey Avrupa| 52.142.95.35|
| Azure Genel| Doğu Asya| 52.139.152.27|
| Azure Genel| Güney Fransa| 20.39.80.2|
| Azure Genel| İsviçre Batı| 51.107.96.8|
| Azure Genel| Orta Avustralya 2| 20.39.99.81|
| Azure Genel| BAE Merkez| 20.37.81.41|
| Azure Genel| İsviçre Kuzey| 51.107.0.91|
| Azure Genel| Güney Afrika Batı| 102.133.0.79|
| Azure Genel| Almanya Batı Orta| 51.116.96.0|
| Azure Genel| Almanya Kuzey| 51.116.0.0|
| Azure Genel| Norveç Doğu| 51.120.2.185|
| Azure Genel| Norveç Batı| 51.120.130.134|
| Azure Çin 21Vianet| Çin Kuzey (Küresel)| 139.217.51.16|
| Azure Çin 21Vianet| Çin Doğu (Küresel)| 139.217.171.176|
| Azure Çin 21Vianet| Kuzey Çin| 40.125.137.220|
| Azure Çin 21Vianet| Doğu Çin| 40.126.120.30|
| Azure Çin 21Vianet| Çin Kuzey 2| 40.73.41.178|
| Azure Çin 21Vianet| Çin Doğu 2| 40.73.104.4|
| Azure Kamu| USGov Virginia (Küresel)| 52.127.42.160|
| Azure Kamu| USGov Texas (Küresel)| 52.127.34.192|
| Azure Kamu| USGov Virginia| 52.227.222.92|
| Azure Kamu| USGov Iowa| 13.73.72.21|
| Azure Kamu| USGov Arizona| 52.244.32.39|
| Azure Kamu| USGov Texas| 52.243.154.118|
| Azure Kamu| USDoD Merkez| 52.182.32.132|
| Azure Kamu| USDoD Doğu| 52.181.32.192|

## <a name="related-content"></a><a name="related-content"> </a>İlgili içerik
* [Vpn Ağ Geçidi'ni kullanarak sanal ağı arka uca bağlama](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti)
* [Sanal Ağı farklı dağıtım modellerinden bağlama](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
* [Azure API Yönetimi'ndeki çağrıları izlemek için API Denetçisi nasıl kullanılır?](api-management-howto-api-inspector.md)
* [Sanal Ağ Sık Sorulan Sorular](../virtual-network/virtual-networks-faq.md)
* [Hizmet etiketleri](../virtual-network/security-overview.md#service-tags)

[api-management-using-vnet-menu]: ./media/api-management-using-with-vnet/api-management-menu-vnet.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-using-with-vnet/api-management-using-vnet-add-api.png
[api-management-vnet-private]: ./media/api-management-using-with-vnet/api-management-vnet-internal.png
[api-management-vnet-public]: ./media/api-management-using-with-vnet/api-management-vnet-external.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[UDRs]: ../virtual-network/virtual-networks-udr-overview.md
[Network Security Group]: ../virtual-network/security-overview.md
[ServiceEndpoints]: ../virtual-network/virtual-network-service-endpoints-overview.md
[ServiceTags]: ../virtual-network/security-overview.md#service-tags
