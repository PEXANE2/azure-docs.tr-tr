---
title: Karma bağlantılar
description: Farklı ağlardaki kaynaklara erişmek için Azure Uygulama Hizmeti'nde karma bağlantılar oluşturma ve kullanma hakkında bilgi edinin.
author: ccompy
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.topic: article
ms.date: 06/06/2019
ms.author: ccompy
ms.custom: seodec18, fasttrack-edit
ms.openlocfilehash: ec842530f3cae26b869a649617f279d204b98fcc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80047769"
---
# <a name="azure-app-service-hybrid-connections"></a>Azure Uygulama Hizmeti Karma Bağlantıları

Karma Bağlantılar hem Azure'da bir hizmet hem de Azure Uygulama Hizmeti'nde bir özelliktir. Bir hizmet olarak, Uygulama Hizmeti'nde kullanılanların ötesinde kullanımlara ve özelliklere sahiptir. Karma Bağlantılar ve Uygulama Hizmeti dışındaki kullanımları hakkında daha fazla bilgi edinmek için [Azure Relay Karma Bağlantıları'na][HCService]bakın.

Uygulama Hizmeti içinde, Karma Bağlantılar diğer ağlardaki uygulama kaynaklarına erişmek için kullanılabilir. Uygulamanızdan uygulama bitiş noktasına erişim sağlar. Uygulamanıza erişmek için alternatif bir özellik etkinleştirmez. Uygulama Hizmeti'nde kullanıldığı gibi, her Karma Bağlantı tek bir TCP ana bilgisayar ve bağlantı noktası kombinasyonuyla ilişkilidir. Bu, Bir TCP dinleme bağlantı noktasına erişiyorsanız, Hibrit Bağlantı bitiş noktasının herhangi bir işletim sisteminde ve herhangi bir uygulamada olabileceği anlamına gelir. Karma Bağlantılar özelliği uygulama protokolünün ne olduğunu veya neye eriştediğinizi bilmiyor veya önemsemiyor. Sadece ağ erişimi sağlamaktadır.  


## <a name="how-it-works"></a>Nasıl çalışır? ##
Karma Bağlantılar özelliği, Azure Servis Veri Veri Günü'ne giden iki çağrıdan oluşur. Uygulamanızın App Service'te çalıştığı ana bilgisayardaki bir kitaplıktan bağlantı vardır. Hibrit Bağlantı Yöneticisi'nden (HCM) Servis Veri Böleği'ne bir bağlantı da vardır. HCM, erişmeye çalıştığınız kaynağı barındıran ağ içinde dağıttığınız bir geçiş hizmetidir. 

İki birleştirilmiş bağlantı sayesinde, uygulamanız HCM'nin diğer tarafındaki sabit bir ana bilgisayar bağlantı noktası birleşimine bir TCP tüneline sahiptir. Bağlantı, kimlik doğrulama ve yetkilendirme için güvenlik ve paylaşılan erişim imzası (SAS) anahtarları için TLS 1.2'yi kullanır.    

![Hibrit Bağlantı diyagramı üst düzey akış][1]

Uygulamanız yapılandırılmış karma bağlantı bitiş noktasıyla eşleşen bir DNS isteğinde bulunduğunda, giden TCP trafiği Karma Bağlantı üzerinden yönlendirilir.  

> [!NOTE]
> Bu, Karma Bağlantınız için her zaman bir DNS adı kullanmayı denemeniz gerektiği anlamına gelir. Bitiş noktası bunun yerine bir IP adresi kullanıyorsa, bazı istemci yazılımları DNS araması yapmaz. 
>

### <a name="app-service-hybrid-connection-benefits"></a>Uygulama Hizmeti Karma Bağlantı avantajları ###

Hibrit Bağlantılar özelliğinin bir dizi avantajı vardır:

- Uygulamalar şirket içi sistemlere ve hizmetlere güvenli bir şekilde erişebilir.
- Bu özellik interneterişimiolan bir bitiş noktası gerektirmez.
- Kurulumu hızlı ve kolaydır. 
- Her Karma Bağlantı, güvenlik için yararlı olan tek bir ana bilgisayar:bağlantı noktası kombinasyonuyla eşleşir.
- Normalde güvenlik duvarı delikleri gerektirmez. Bağlantıların tümü standart web bağlantı noktalarından çıkar.
- Özellik ağ düzeyi olduğundan, uygulamanız tarafından kullanılan dile ve bitiş noktası tarafından kullanılan teknolojiye agnostiktir.
- Tek bir uygulamadan birden çok ağda erişim sağlamak için kullanılabilir. 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Karma Bağlantılarla yapamayacağınız şeyler ###

Karma Bağlantılar ile yapamayacağınız şeyler şunlardır:

- Bir sürücü monte edin.
- UDP kullanın.
- FTP Pasif Modu veya Genişletilmiş Pasif Modu gibi dinamik bağlantı noktalarını kullanan TCP tabanlı hizmetlere erişin.
- UDP gerektirebilir, çünkü LDAP destek.
- Etkin Dizini destekleyin, çünkü bir Uygulama Hizmeti çalışanına katılamazsınız.

### <a name="prerequisites"></a>Ön koşullar ###
 - Windows App hizmeti gereklidir. Yalnızca Windows'da kullanılabilir.  

## <a name="add-and-create-hybrid-connections-in-your-app"></a>Uygulamanızda Karma Bağlantılar Ekleme ve Oluşturma ##

Karma Bağlantı oluşturmak için [Azure portalına][portal] gidin ve uygulamanızı seçin. **Ağ Yapılandırma'yı** > seçin**Karma Bağlantı uç noktalarınızı yapılandırın.** Burada uygulamanız için yapılandırılan Karma Bağlantıları görebilirsiniz.  

![Karma Bağlantı listesinin ekran görüntüsü][2]

Yeni bir Karma Bağlantı eklemek için **[+] Karma bağlantı ekle'yi**seçin.  Zaten oluşturduğunuz Karma Bağlantıların bir listesini görürsünüz. Uygulamanıza bunlardan bir veya daha fazlasını eklemek için, istediğiniz leri seçin ve **ardından seçili Karma Bağlantı Ekle'yi**seçin.  

![Hibrit Bağlantı portalının ekran görüntüsü][3]

Yeni bir Karma Bağlantı oluşturmak istiyorsanız, **yeni karma bağlantı oluştur'u**seçin. Belirtin: 

- Karma Bağlantı adı.
- Endpoint ana bilgisayar adı.
- Endpoint bağlantı noktası.
- Kullanmak istediğiniz Servis Veri Günü ad alanı.

![Yeni karma bağlantı iletişim kutusu oluşturma ekran görüntüsü][4]

Her Karma Bağlantı bir Hizmet Veri Servisi ad alanına bağlıdır ve her Hizmet Veri Mesuresi ad alanı bir Azure bölgesindedir. Ağ kaynaklı gecikmeyi önlemek için uygulamanızla aynı bölgede bir Hizmet Veri Servisi ad alanı kullanmaya çalışmak önemlidir.

Karma Bağlantınızı uygulamanızdan kaldırmak istiyorsanız, uygulamanızdan sağ tıklayın ve **Bağlantıyı Kesme'yi**seçin.  

Uygulamanıza Karma Bağlantı eklendiğinde, uygulamanın ayrıntılarını yalnızca seçerek görebilirsiniz. 

![Hibrit bağlantı ayrıntılarının ekran görüntüsü][5]

### <a name="create-a-hybrid-connection-in-the-azure-relay-portal"></a>Azure Röle portalında Karma Bağlantı Oluşturma ###

Uygulamanızın içinden gelen portal deneyimine ek olarak, Azure Röle portalı içinden Karma Bağlantılar oluşturabilirsiniz. Karma Bağlantının App Service tarafından kullanılabilmesi için şunları yapmalıdır:

* İstemci yetkilendirmesi gerektirir.
* Değer olarak ana bilgisayar:bağlantı noktası birleşimi içeren, bitiş noktası adlı bir meta veri öğesine sahip.

## <a name="hybrid-connections-and-app-service-plans"></a>Karma Bağlantılar ve Uygulama Hizmeti planları ##

Uygulama Hizmeti Karma Bağlantıları yalnızca Temel, Standart, Premium ve İzole fiyatlandırma SUK'larında kullanılabilir. Fiyatlandırma planına bağlı sınırlar vardır.  

| Fiyatlandırma planı | Planda kullanılabilir Karma Bağlantı Sayısı |
|----|----|
| Temel | 5 |
| Standart | 25 |
| Premium | 200 |
| Yalıtılmış | 200 |

Uygulama Hizmeti planı ui, kaç Karma Bağlantı kullanıldığını ve hangi uygulamalar tarafından kullanıldığını gösterir.  

![Uygulama Hizmeti planı özelliklerinin ekran görüntüsü][6]

Ayrıntıları görmek için Karma Bağlantı'yı seçin. Uygulama görünümünde gördüğünüz tüm bilgileri görebilirsiniz. Aynı plandaki diğer uygulamaların bu Karma Bağlantıyı kullandığını da görebilirsiniz.

Bir Uygulama Hizmeti planında kullanılabilecek Karma Bağlantı uç noktalarının sayısında bir sınır vardır. Ancak, kullanılan her Karma Bağlantı, bu plandaki herhangi bir sayıda uygulamada kullanılabilir. Örneğin, Bir Uygulama Hizmeti planında beş ayrı uygulamada kullanılan tek bir Karma Bağlantı, tek bir Karma Bağlantı olarak sayılır.

### <a name="pricing"></a>Fiyatlandırma ###

Bir Uygulama Hizmeti planı SKU gereksinimi olmasının yanı sıra, Karma Bağlantılar kullanmak için ek bir maliyet vardır. Hibrit Bağlantı tarafından kullanılan her dinleyici için bir ücret vardır. Dinleyici, Karma Bağlantı Yöneticisi'dir. İki Karma Bağlantı Yöneticisi tarafından desteklenen beş Karma Bağlantınız olsaydı, bu 10 dinleyici olurdu. Daha fazla bilgi için [Servis Veri Servisi fiyatlandırması'na][sbpricing]bakın.

## <a name="hybrid-connection-manager"></a>Karma Bağlantı Yöneticisi ##

Karma Bağlantılar özelliği, ağda Karma Bağlantı bitiş noktanızı barındıran bir geçiş aracısı gerektirir. Bu geçiş aracısı Hibrit Bağlantı Yöneticisi (HCM) olarak adlandırılır. [Azure portalındaki][portal]uygulamanızdan HCM'yi indirmek**için, Karma Bağlantı uç noktalarınızı Yapılandırma ağı'nı** **Networking** > seçin.  

Bu araç Windows Server 2012 ve sonraki sürümlerinde çalışır. HCM bir hizmet olarak çalışır ve 443 bağlantı noktasındaki Azure Röle'ye giden bağlantıya bağlanır.  

HCM yükledikten sonra, araç için UI kullanmak için HybridConnectionManagerUi.exe çalıştırabilirsiniz. Bu dosya Karma Bağlantı Yöneticisi yükleme dizinindedir. Windows 10'da, arama kutunuzda *Karma Bağlantı Yöneticisi UI'yi* arama nız da yapabilirsiniz.  

![Hibrit Bağlantı Yöneticisi ekran görüntüsü][7]

HCM UI'yi başlattığınızda, gördüğünüz ilk şey, HCM'nin bu örneğiyle yapılandırılan tüm Karma Bağlantıları listeleyen bir tablodur. Herhangi bir değişiklik yapmak istiyorsanız, önce Azure ile kimlik doğrulaması yapın. 

HCM'nize bir veya daha fazla Karma Bağlantı eklemek için:

1. HCM UI'yi başlatın.
2. **Başka bir Karma Bağlantı Yapıla'yı**seçin.
![Yeni Karma Bağlantıları Yapılandırma Ekran Görüntüsü][8]

1. Aboneliklerinizde Karma Bağlantılarınızı kullanılabilir hale getirmek için Azure hesabınızla oturum açın. HCM, Azure hesabınızı bunun ötesinde kullanmaya devam etmez. 
1. Bir abonelik seçin.
1. HCM'nin geçişini istediğiniz Karma Bağlantıları seçin.
![Hibrit Bağlantıların Ekran Görüntüsü][9]

1. **Kaydet'i**seçin.

Artık eklediğiniz Karma Bağlantıları görebilirsiniz. Ayrıntıları görmek için yapılandırılan Karma Bağlantı'yı da seçebilirsiniz.

![Karma Bağlantı Ayrıntılarının Ekran Görüntüsü][10]

Yapılandırıldığı Karma Bağlantıları desteklemek için HCM şunları gerektirir:

- TCP, 443 bağlantı noktası üzerinden Azure'a erişim.
- TCP'nin Karma Bağlantı bitiş noktasına erişimi.
- Uç nokta ana bilgisayarda ve Servis Veri Yolundas ad alanında DNS aramayapma özelliği.

> [!NOTE]
> Azure Röle, bağlantı için Web Soketlerine dayanır. Bu özellik yalnızca Windows Server 2012 veya sonraki bazılarında kullanılabilir. Bu nedenle, HCM Windows Server 2012 daha erken bir şey desteklenmez.
>

### <a name="redundancy"></a>Yedeklilik ###

Her HCM birden çok Karma Bağlantı'yı destekleyebilir. Ayrıca, herhangi bir Karma Bağlantı birden fazla HCM tarafından desteklenebilir. Varsayılan davranış, belirli bir bitiş noktası için yapılandırılan HCM'ler arasında trafiği yönlendirmektir. Ağınızdaki Karma Bağlantılarınızda yüksek kullanılabilirlik istiyorsanız, ayrı makinelerde birden fazla HCM çalıştırın. HHM'lere trafik dağıtmak için Röle hizmeti tarafından kullanılan yük dağıtım algoritması rasgele atamadır. 

### <a name="manually-add-a-hybrid-connection"></a>El ile Karma Bağlantı ekleme ###

Aboneliğiniz in dışındaki birinin belirli bir Karma Bağlantı için bir HCM örneğini barındırmasını sağlamak için, Karma Bağlantı için ağ geçidi bağlantı dizesini onlarla paylaşın. [Azure portalında][portal]Karma Bağlantı özelliklerinde ağ geçidi bağlantı dizesini görebilirsiniz. Bu dizeyi kullanmak için HCM'de **El Ile Gir'i** ve ağ geçidi bağlantı dizesine yapıştır'ı seçin.

![El ile Karma Bağlantı ekleme][11]

### <a name="upgrade"></a>Yükseltme ###

Sorunları gidermek veya iyileştirmeler sağlamak için Karma Bağlantı Yöneticisi'nde periyodik güncelleştirmeler vardır. Yükseltmeler yayımlandığında, HCM UI'de bir açılır pencere açılır. Yükseltmeyi uygulamak değişiklikleri uygular ve HCM'yi yeniden başlatacaktır. 

## <a name="adding-a-hybrid-connection-to-your-app-programmatically"></a>Uygulamanıza programlı olarak Karma Bağlantı Ekleme ##

Aşağıda belirtilen API'ler doğrudan uygulamalarınıza bağlı Karma Bağlantıları yönetmek için kullanılabilir. 

    /subscriptions/[subscription name]/resourceGroups/[resource group name]/providers/Microsoft.Web/sites/[app name]/hybridConnectionNamespaces/[relay namespace name]/relays/[hybrid connection name]?api-version=2016-08-01

Karma Bağlantı ile ilişkili JSON nesnesi aşağıdaki gibi görünür:

    {
      "name": "[hybrid connection name]",
      "type": "Microsoft.Relay/Namespaces/HybridConnections",
      "location": "[location]",
      "properties": {
        "serviceBusNamespace": "[namespace name]",
        "relayName": "[hybrid connection name]",
        "relayArmUri": "/subscriptions/[subscription id]/resourceGroups/[resource group name]/providers/Microsoft.Relay/namespaces/[namespace name]/hybridconnections/[hybrid connection name]",
        "hostName": "[endpoint host name]",
        "port": [port],
        "sendKeyName": "defaultSender",
        "sendKeyValue": "[send key]"
      }
    }

Bu bilgileri kullanmanın bir yolu armclient ile, [armclient][armclient] GitHub projeden alabilirsiniz. Uygulamanıza önceden varolan bir Karma Bağlantı ekleme yle ilgili bir örnek aşağıda verilmiştir. Yukarıdaki şema başına bir JSON dosyası oluşturun:

    {
      "name": "relay-demo-hc",
      "type": "Microsoft.Relay/Namespaces/HybridConnections",
      "location": "North Central US",
      "properties": {
        "serviceBusNamespace": "demo-relay",
        "relayName": "relay-demo-hc",
        "relayArmUri": "/subscriptions/ebcidic-asci-anna-nath-rak1111111/resourceGroups/myrelay-rg/providers/Microsoft.Relay/namespaces/demo-relay/hybridconnections/relay-demo-hc",
        "hostName": "my-wkstn.home",
        "port": 1433,
        "sendKeyName": "defaultSender",
        "sendKeyValue": "Th9is3is8a82lot93of3774stu887ff122235="
      }
    }

Bu API'yi kullanmak için gönder tuşu ve kaynak kimliğini aktarmanız gerekir. Bilgilerinizi hctest.json dosya adı ile kaydettiyseniz, Karma Bağlantınızı uygulamanıza eklemek için bu komutu sorun: 

    armclient login
    armclient put /subscriptions/ebcidic-asci-anna-nath-rak1111111/resourceGroups/myapp-rg/providers/Microsoft.Web/sites/myhcdemoapp/hybridConnectionNamespaces/demo-relay/relays/relay-demo-hc?api-version=2016-08-01 @hctest.json

## <a name="secure-your-hybrid-connections"></a>Karma Bağlantılarınızı Emniyete ##

Mevcut Karma Bağlantı, temel Azure Hizmet Veri Veri Günü'nde yeterli izine sahip olan herhangi bir kullanıcı tarafından diğer Uygulama Hizmeti Web Apps'a eklenebilir. Bu, başkalarının aynı Karma Bağlantıyı yeniden kullanmasını engellemeniz gerekiyorsa (örneğin, hedef kaynak yetkisiz erişimi önlemek için herhangi bir ek güvenlik önlemi olmayan bir hizmetse), Azure erişimini kilitlemeniz gerektiği anlamına gelir Servis Otobüs Rölesi.

Röle erişimi olan `Reader` herkes, Azure Portalı'ndaki Web Uygulamasına eklemeye çalışırken Karma Bağlantıyı _görebilir,_ ancak röle bağlantısını kurmak için kullanılan bağlantı dizesini almak için izinleri olmadığı için bu bağlantıyı _ekleyemeyecektir._ Karma Bağlantı'yı başarıyla ekleyebilmek için `listKeys` izinalmış olmaları gerekir.`Microsoft.Relay/namespaces/hybridConnections/authorizationRules/listKeys/action` Relay'deki `Contributor` bu izni içeren rol veya başka bir rol, kullanıcıların Karma Bağlantıyı kullanmasına ve bunları kendi Web Uygulamalarına eklemesine olanak tanır.

## <a name="troubleshooting"></a>Sorun giderme ##

"Bağlı" durumu, en az bir HCM'nin bu Karma Bağlantı ile yapılandırıldığı ve Azure'a ulaşabildiği anlamına gelir. Karma Bağlantınızın durumu **Bağlı**olarak belirtmiyorsa, Karma Bağlantınız Azure erişimi olan herhangi bir HCM'de yapılandırılmamıştır.

İstemcilerin bitiş noktalarına bağlanamamasının birincil nedeni, bitiş noktasının DNS adı yerine bir IP adresi kullanılarak belirtilmiş olmasıdır. Uygulamanız istenilen bitiş noktasına ulaşamıyorsa ve bir IP adresi kullandıysanız, HCM'nin çalıştığı ana bilgisayarda geçerli olan bir DNS adı kullanmaya geçin. Ayrıca, DNS adının HCM'nin çalıştığı ana bilgisayarda düzgün şekilde çözülüp çözüldüğünü denetleyin. HCM'nin Çalıştığı ana bilgisayardan Hibrit Bağlantı bitiş noktasına bağlantı olduğunu doğrulayın.  

App Service'de, gelişmiş araçlar (Kudu) konsolundan **tpping** komut satırı aracı çağrılabilir. Bu araç, bir TCP bitiş noktasına erişiminiz olup olmadığını size söyleyebilir, ancak karma bağlantı bitiş noktasına erişiminiz olup olmadığını söylemez. Konsoldaki aracı Karma Bağlantı bitiş noktasına karşı kullandığınızda, yalnızca ana bilgisayar:bağlantı noktası kombinasyonu kullandığını onaylarsınız.  

Bitiş noktanız için bir komut satırı istemciniz varsa, uygulama konsolundan bağlantı test edebilirsiniz. Örneğin, kıvırma kullanarak web sunucusu uç noktalarına erişimi test edebilirsiniz.

## <a name="biztalk-hybrid-connections"></a>BizTalk Hibrid Bağlantılar ##

Bu özelliğin erken formu BizTalk Hibrid Bağlantıları olarak adlandırıldı. Bu yetenek 31 Mayıs 2018'de Yaşamın Sonu'na gitti ve faaliyetlerini durdurdu. BizTalk hibrit bağlantıları tüm uygulamalardan kaldırılmıştır ve portal veya API üzerinden erişilemez. Hibrit Bağlantı Yöneticisi'nde yapılandırılan bu eski bağlantılarınız varsa, Sürek'li bir durum görür ve alt kısmında bir Yaşam Sonu deyimi görüntülersiniz.

![HCM BizTalk Hibrid Bağlantılar][12]


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png
[11]: ./media/app-service-hybrid-connections/hybridconn-manual.png
[12]: ./media/app-service-hybrid-connections/hybridconn-bt.png

<!--Links-->
[HCService]: https://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: https://portal.azure.com/
[oldhc]: https://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: https://azure.microsoft.com/pricing/details/service-bus/
[armclient]: https://github.com/projectkudu/ARMClient/
