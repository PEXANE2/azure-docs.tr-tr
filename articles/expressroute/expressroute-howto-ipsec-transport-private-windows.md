---
title: 'Azure ExpressRoute özel eşleme: IPsec aktarım modunu yapılandırın - Windows ana bilgisayarları'
description: GDO'lar ve İşbur'lar kullanarak ExpressRoute özel bakışlama yoluyla Azure Windows VM'leri ile şirket içi Windows ana bilgisayarları arasında IPsec aktarım modunu etkinleştirme.
services: expressroute
author: fabferri
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/17/2018
ms.author: fabferri
ms.custom: seodec18
ms.openlocfilehash: 1bc33047d31262af443cddc418853fbacd88aec1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74022001"
---
# <a name="configure-ipsec-transport-mode-for-expressroute-private-peering"></a>ExpressRoute özel eşleme için IPsec aktarım modunu yapılandırın

Bu makale, Windows çalıştıran Azure VM'ler ile şirket içi Windows ana bilgisayarları arasında ExpressRoute özel bakışları üzerinden aktarım modunda IPsec tünelleri oluşturmanıza yardımcı olur. Bu makaledeki adımlar, grup ilkesi nesneleri kullanarak bu yapılandırmayı oluşturur. Bu yapılandırmayı kuruluş birimleri (İş) ve grup ilke nesnelerini (GPO) kullanmadan oluşturmak mümkün olsa da, İş ve GPO'ların birleşimi güvenlik ilkelerinizin denetimini basitleştirmeye yardımcı olur ve hızla ölçeklendirmenize olanak tanır. Bu makaledeki adımlar, zaten bir Active Directory yapılandırmanız olduğunu ve OS'ler ve GPO'lar kullanmaya aşina olduğunuzu varsayar.

## <a name="about-this-configuration"></a>Bu yapılandırma hakkında

Aşağıdaki adımlardaki yapılandırma, ExpressRoute özel eşlemeli tek bir Azure sanal ağı (VNet) kullanır. Ancak, bu yapılandırma daha fazla Azure VNet'ini ve şirket içi ağları kaplayabilir. Bu makale, bir IPsec şifreleme ilkesi tanımlamanıza ve bunu aynı OU'nun parçası olan bir grup Azure VM'sine ve şirket içi ana bilgisayarlara uygulamanıza yardımcı olur. Azure VM'leri (vm1 ve vm2) ile şirket içi ana bilgisayar1 arasında şifrelemeyi yalnızca hedef bağlantı noktası 8080 olan HTTP trafiği için yapılandırabilirsiniz. Gereksinimlerinize göre farklı türde IPsec ilkesi oluşturulabilir.

### <a name="working-with-ous"></a>OSB'lerle Çalışma 

OU ile ilişkili güvenlik ilkesi GPO üzerinden bilgisayarlara itilir. Tek bir ana bilgisayara ilkeler uygulamak yerine, OS'leri kullanmanın birkaç avantajı şunlardır:

* OU ile bir ilke ilişkilendirmek, aynı OU'ya ait bilgisayarların aynı ilkeleri elde etmesini garanti eder.
* OU ile ilişkili güvenlik ilkesinin değiştirilmesi, değişiklikleri OU'daki tüm ana bilgisayarlara uygular.

### <a name="diagrams"></a>Diyagramları

Aşağıdaki diyagram, ara bağlantıyı ve atanan IP adres alanını gösterir. Azure VM'leri ve şirket içi ana bilgisayar Windows 2016'yı çalıştırıyor. Azure VM'leri ve şirket içi ana bilgisayar1 aynı etki alanının bir parçasıdır. Azure VM'leri ve şirket içi ana bilgisayarlar DNS kullanarak adları düzgün şekilde çözebilir.

[![1.1.2.2.]][1]

Bu diyagram, ExpressRoute özel bakışlarında geçiş halindeki IPsec tünellerini gösterir.

[![4 4]][4]

### <a name="working-with-ipsec-policy"></a>IPsec ilkesiyle çalışma

Windows'da şifreleme, IPsec ilkesiyle ilişkilidir. IPsec ilkesi hangi IP trafiğinin güvenli olduğunu ve IP paketlerine uygulanan güvenlik mekanizmasını belirler.
**IPSec ilkeleri** aşağıdaki öğelerden oluşur: **Filtre Listeleri, Filtre** **Eylemleri**ve **Güvenlik Kuralları.**

IPsec ilkesini yapılandırırken aşağıdaki IPsec ilkesi terminolojisini anlamak önemlidir:

* **IPsec ilkesi:** Kurallar koleksiyonu. Belirli bir zamanda yalnızca bir ilke etkin ("atanmış") olabilir. Her ilkenin bir veya daha fazla kuralı olabilir ve bunların tümü aynı anda etkin olabilir. Bir bilgisayara verilen anda yalnızca bir etkin IPsec ilkesi atanabilir. Ancak, IPsec ilkesi içinde, farklı durumlarda alınabilecek birden çok eylem tanımlayabilirsiniz. Her IPsec kuralı kümesi, kuralın uygulandığı ağ trafiği türünü etkileyen bir filtre listesiyle ilişkilidir.

* **Filtre listeleri:** Filtre listeleri bir veya daha fazla filtrenin demetidir. Bir liste birden çok filtre içerebilir. Filtre, IP adres aralıklarına, protokollere ve hatta belirli protokol bağlantı noktalarına göre iletişime izin verilip verilmediğini, güvenli hale alınıp alınıp alınıp alınamadığını tanımlar. Her filtre belirli bir koşul kümesiyle eşleşir; örneğin, belirli bir alt ağdan belirli bir hedef bağlantı noktasındaki belirli bir bilgisayara gönderilen paketler. Ağ koşulları bu filtrelerden biriyle veya daha fazlasıile eşleştiğinde, filtre listesi etkinleştirilir. Her filtre belirli bir filtre listesi içinde tanımlanır. Filtreler filtre listeleri arasında paylaşılamaz. Ancak, belirli bir filtre listesi birkaç IPsec ilkelerine dahil edilebilir. 

* **Filtre eylemleri:** Güvenlik yöntemi, IKE görüşmeleri sırasında bir bilgisayarın sunduğu bir dizi güvenlik algoritması, protokol ve anahtar tanımlar. Filtre eylemleri, tercih sırasına göre sıralanan güvenlik yöntemlerinin listeleridir.  Bir bilgisayar bir IPsec oturumu üzerinde anlaşma yaptığında, filtre eylemleri listesinde depolanan güvenlik ayarını temel alan teklifleri kabul eder veya gönderir.

* **Güvenlik kuralları:** Kurallar, bir IPsec ilkesinin iletişimi nasıl ve ne zaman koruyacağını yönetir. IPsec bağlantısını oluşturmak için bir IPsec kuralı oluşturmak için **filtre listesi** ve **filtre eylemleri** kullanır. Her ilkenin bir veya daha fazla kuralı olabilir ve bunların tümü aynı anda etkin olabilir. Her kural, IP filtrelerinin bir listesini ve bu filtre listesiyle eşleşen bir eşleme sırasında gerçekleşen güvenlik eylemleri koleksiyonunu içerir:
  * IP Filtre İşlemleri
  * Kimlik doğrulama yöntemleri
  * IP tünel ayarları
  * Bağlantı türleri

[![5 5,5]][5]

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki ön koşulları yerine getirebildiğinizden emin olun:

* Grup İlkesi ayarlarını uygulamak için kullanabileceğiniz işleyen bir Active Directory yapılandırmanız olmalıdır. GPO'lar hakkında daha fazla bilgi için [Grup İlkesi Nesneleri'ne](https://msdn.microsoft.com/library/windows/desktop/aa374162(v=vs.85).aspx)bakın.

* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir.
  * ExpressRoute devresi oluşturma hakkında daha fazla bilgi için [bkz.](expressroute-howto-circuit-arm.md) 
  * Devrenin bağlantı sağlayıcınız tarafından etkinleştirildiğinden doğrulayın. 
  * Devreniz için Azure özel eşleme yapılandırıldığınızdan doğrulayın. Yönlendirme yönergeleri için [yapılandırma yönlendirme](expressroute-howto-routing-arm.md) makalesine bakın. 
  * Bir VNet ve sanal ağ ağ ağ geçidi oluşturulduğunu ve tam olarak sağlanmış olduğunu doğrulayın. [ExpressRoute için sanal ağ ağ geçidi oluşturmak için](expressroute-howto-add-gateway-resource-manager.md)yönergeleri izleyin. ExpressRoute için bir sanal ağ ağ geçidi, VPN değil, GatewayType 'ExpressRoute' kullanır.

* ExpressRoute sanal ağ ağ geçidi ExpressRoute devresine bağlı olmalıdır. Daha fazla bilgi için [bkz.](expressroute-howto-linkvnet-arm.md)

* Azure Windows VM'lerinin VNet'e dağıtılmış olduğunu doğrulayın.

* Şirket içi ana bilgisayarlarla Azure VM'leri arasında bağlantı olduğunu doğrulayın.

* Azure Windows VM'lerinin ve şirket içi ana bilgisayarların adları düzgün bir şekilde çözmek için DNS'yi kullanabileceğini doğrulayın.

### <a name="workflow"></a>İş akışı

1. Bir GPO oluşturun ve OU ile ilişkilendirin.
2. Bir IPsec **Filtre Eylemi**tanımlayın.
3. Bir IPsec **Filtre Listesi**tanımlayın.
4. **Güvenlik Kuralları**ile bir IPsec İlkesi oluşturun.
5. Ou'ya IPsec GPO'yu atayın.

### <a name="example-values"></a>Örnek değerler

* **Alan Adı:** ipsectest.com

* **OU:** IPSecOU

* **Şirket içi Windows bilgisayar:** host1

* **Azure Windows VM'leri:** vm1, vm2

## <a name="1-create-a-gpo"></a><a name="creategpo"></a>1. GPO oluşturma

1. OU'ya bağlı yeni bir GPO oluşturmak için Grup İlkesi Yönetimi'ni açın ve GPO'nun bağlanacağı OU'yu bulun. Örnekte, OU **IPSecOU**olarak adlandırılır. 

   [![9 9]][9]
2. Grup İlkesi Yönetimi'nde OU'yu seçin ve sağ tıklatın. Açılan açılır dosyada "**Bu etki alanında bir GPO oluştur ve buraya bağla...**" seçeneğini tıklayın.

   [![10]][10]
3. GPO'yu daha sonra kolayca bulabilmeniz için sezgisel bir ad adlandırın. GPO oluşturmak ve bağlamak için **Tamam'ı** tıklatın.

   [![11 11]][11]

## <a name="2-enable-the-gpo-link"></a><a name="enablelink"></a>2. GPO bağlantısını etkinleştirme

GPO'yu OU'ya uygulamak için GPO'nun yalnızca OU'ya bağlı olmaması, aynı zamanda bağlantının da etkin leştirilmesi gerekir.

1. Oluşturduğunuz GPO'yu bulun, sağ tıklatın ve açılır yerden **Edit'i** seçin.
2. GPO'yu OU'ya uygulamak için **Etkin Bağlantı'yı**seçin.

   [![12 12]][12]

## <a name="3-define-the-ip-filter-action"></a><a name="filteraction"></a>3. IP filtresi eylemini tanımlayın

1. Açılan dosyadan, **Active Directory'de IP Güvenlik İlkesi'ni**sağ tıklatın ve ardından **IP filtre listelerini yönet'i ve eylemleri filtrele'yi tıklatın...**.

   [![15 15]][15]
2. "**Filtre Eylemlerini Yönet**" sekmesinde **Ekle'yi**tıklatın.

   [![16 16]][16]

3. IP **Güvenlik Filtresi Eylem sihirbazında,** **İleri'yi**tıklatın.

   [![17 17]][17]
4. Filtre eylemini daha sonra bulabilmeniz için sezgisel bir ad olarak adlandırın. Bu örnekte, filtre eylemi **myEncryption**olarak adlandırılır. Ayrıca bir açıklama da ekleyebilirsiniz. Ardından **İleri'yi**tıklatın.

   [![18 18]][18]
5. **Güvenlik görüşü,** iPsec başka bir bilgisayarla kurulamazsa davranışı tanımlamanızı sağlar. **Güvenliği Belirley'i**seçin, ardından **İleri'yi**tıklatın.

   [![19 19]][19]
6. **IPsec sayfasını desteklemeyen bilgisayarlarla iletişim** de, güvenli olmayan **iletişime izin verme'yi**seçin, ardından **İleri'yi**tıklatın.

   [![20]][20]
7. IP **Trafik ve Güvenlik** sayfasında **Özel'i**seçin ve **Ayarlar'ı tıklatın...**

   [![21]][21 21]
8. Özel **Güvenlik Yöntemi Ayarları** sayfasında **Veri bütünlüğü ve şifreleme (ESP): SHA1, 3DES'i**seçin. Ardından **Tamam'ı**tıklatın.

   [![22 22]][22]
9. Filtre **Eylemlerini Yönet** sayfasında, **myEncryption** filtresinin başarıyla eklandığını görebilirsiniz. **Kapat'ı**tıklatın.

   [![23]][23]

## <a name="4-define-an-ip-filter-list"></a><a name="filterlist1"></a>4. IP filtre listesi tanımlama

Hedef bağlantı noktası 8080 ile şifreli HTTP trafiğini belirten bir filtre listesi oluşturun.

1. Hangi trafik türlerinin şifrelemesi gerektiğini nitelemek için bir **IP filtre listesi**kullanın. IP **Filtresi Listelerini Yönet** sekmesinde, yeni bir IP filtre listesi eklemek için **Ekle'yi** tıklatın.

   [![24]][24]
2. **Ad:** alan, IP filtre listeniz için bir ad yazın. Örneğin, **azure-onpremises-HTTP8080**. Ardından **Ekle'yi**tıklatın.

   [![25 25]][25]
3. IP **Filtresi Açıklaması ve Yansıtılmış özellik** **sayfasında, Mirrored'ı**seçin. Aynalı ayar, iki yönlü iletişim sağlayan paketlerle eşleşir. Ardından **İleri**’ye tıklayın.

   [![26]][26]
4. IP **Trafik Kaynağı** sayfasında, **Kaynak adresinden:** açılır açılır, **belirli bir IP Adresi veya Subnet**seçin. 

   [![27]][27 27]
5. Ip trafiğinin kaynak adresini **IP Adresi veya Alt Net** belirtin, ardından **İleri'yi**tıklatın.

   [![28 28]][28]
6. Hedef **adresi belirtin:** IP Adresi veya Subnet. Ardından **İleri'yi**tıklatın.

   [![29 29]][29]
7. IP **Protokolü Türü** sayfasında **TCP'yi**seçin. Ardından **İleri'yi**tıklatın.

   [![30]][30]
8. IP **Protocol Bağlantı Noktası** sayfasında, **herhangi bir bağlantı noktasından** ve **bu bağlantı noktasına seçin:**. Metin **kutusuna 8080** yazın. Bu ayarlar, yalnızca hedef bağlantı noktası 8080'deki HTTP trafiğinin şifreleneceğini belirtir. Ardından **İleri'yi**tıklatın.

   [![31]][31 31]
9. IP filtre listesini görüntüleyin.  IP Filtre Listesi'nin yapılandırması **azure-onpremises-HTTP8080,** aşağıdaki ölçütlerle eşleşen tüm trafik için şifrelemeyi tetikler:

   * 10.0.1.0/24'teki tüm kaynak adresleri (Azure Subnet2)
   * 10.2.27.0/25'teki herhangi bir hedef adres (şirket içi alt ağ)
   * TCP protokolü
   * Hedef bağlantı noktası 8080

   [![32 32]][32]

## <a name="5-edit-the-ip-filter-list"></a><a name="filterlist2"></a>5. IP filtre listesini edin

Aynı trafik türünü ters yönde şifrelemek için (şirket içi ana bilgisayardan Azure VM'ye kadar) ikinci bir IP filtresine ihtiyacınız vardır. Yeni filtrenin ayarişlemi, ilk IP filtresini ayarlamak için kullandığınız işlemle aynıdır. Tek fark kaynak alt ağ ve hedef alt ağdır.

1. IP Filtre Listesine yeni bir IP filtresi eklemek için **Edit'i**seçin.

   [![33 33]][33]
2. IP **Filtre Listesi** sayfasında **Ekle'yi**tıklatın.

   [![34]][34 34]
3. Aşağıdaki örnekteki ayarları kullanarak ikinci bir IP filtresi oluşturun:

   [![35 35]][35]
4. İkinci IP filtresini oluşturduktan sonra IP filtre listesi aşağıdaki gibi görünür:

   [![36 36]][36]

Bir uygulamayı korumak için şirket içi bir konum la Azure alt ağı arasında şifreleme gerekiyorsa, varolan IP filtre listesini değiştirmek yerine yeni bir IP filtre listesi ekleyebilirsiniz. 2 IP filtre listesini aynı Ipsec ilkesiyle ilişkilendirmek, diğer IP filtre listelerini etkilemeden belirli bir IP filtre listesi herhangi bir zamanda değiştirilebilir veya kaldırılabilir olduğundan daha iyi esneklik sağlar.

## <a name="6-create-an-ipsec-security-policy"></a><a name="ipsecpolicy"></a>6. Bir IPsec güvenlik ilkesi oluşturma 

Güvenlik kuralları içeren bir IPsec ilkesi oluşturun.

1. OU ile ilişkili **Active dizininde IPSecurity İlkeleri'ni** seçin. Sağ tıklayın ve **IP Güvenlik İlkesi Oluştur'u**seçin.

   [![37 37]][37]
2. Güvenlik ilkesini adlandırın. Örneğin, **ilke-azure-onpremises**. Ardından **İleri'yi**tıklatın.

   [![38 38]][38]
3. Onay kutusunu seçmeden **İleri'yi** tıklatın.

   [![39 39]][39]
4. **Özellikleri Edit** onay kutusunun seçili olduğunu doğrulayın ve sonra **Bitir'i**tıklatın.

   [![40]][40]

## <a name="7-edit-the-ipsec-security-policy"></a><a name="editipsec"></a>7. IPsec güvenlik ilkesini edin

IPsec ilkesine, daha önce yapılandırdığınız **IP Filtre Listesi** ve Filtre **Eylemi'ni** ekleyin.

1. HTTP ilkesi Özellikleri **Kuralları** sekmesinde **Ekle'yi**tıklatın.

   [![41 41]][41]
2. Hoş Geldiniz sayfasında **İleri**'ye tıklayın.

   [![42 42]][42]
3. Kural, IPsec modunu tanımlama seçeneği sağlar: tünel modu veya aktarım modu.

   * Tünel modunda, orijinal paket bir IP üstbilgi kümesi tarafından kapsüllenir. Tünel modu, orijinal paketin IP üstbilgisini şifreleyerek dahili yönlendirme bilgilerini korur. Tünel modu, siteden siteye VPN senaryolarında ağ geçitleri arasında yaygın olarak uygulanır. Tünel modu, ana bilgisayarlar arasında uçtan uca şifreleme için kullanılan çoğu durumda kullanılır.

   * Aktarım modu yalnızca yükü ve ESP römorkunu şifreler; özgün paketin IP üstbilgisi şifrelenmez. Aktarım modunda, paketlerin IP kaynağı ve IP hedefi değişmez.

   Bu kuralı seçin **bir tünel belirtmez**ve sonra **İleri'yi**tıklatın.

   [![43 43]][43]
4. **Ağ Türü,** hangi ağ bağlantısının güvenlik ilkesiyle ilişkilendirinir tanımlar. **Tüm ağ bağlantılarını**seçin ve sonra **İleri'yi**tıklatın.

   [![44 44]][44]
5. Daha önce oluşturduğunuz IP filtre listesini seçin, **azure-onpremises-HTTP8080**, ve sonra **İleri'yi**tıklatın.

   [![45 45]][45]
6. Daha önce oluşturduğunuz varolan Filtre Eylemi **myEncryption'ı** seçin.

   [![46 46]][46]
7. Windows dört farklı kimlik doğrulama türünü destekler: Kerberos, sertifikalar, NTLMv2 ve önceden paylaşılan anahtar. Etki alanı birleştirilmiş ana bilgisayarlarla çalıştığımıziçin Active **Directory varsayılan (Kerberos V5 protokolü)** seçeneğini belirleyin ve ardından **İleri'yi**tıklatın.

   [![47 47]][47]
8. Yeni ilke güvenlik kuralını oluşturur: **azure-onpremises-HTTP8080**. **Tamam**'a tıklayın.

   [![48 48]][48]

IPsec ilkesi, hedef bağlantı noktası 8080'deki tüm HTTP bağlantılarının IPsec aktarım modunu kullanmasını gerektirir. HTTP açık bir metin protokolü olduğundan, güvenlik ilkesietkin olması, ExpressRoute özel bakış yoluyla aktarıldığında verilerin şifrelenmesini sağlar. Active Directory için IP Güvenlik ilkesi Gelişmiş Güvenlik ile Windows Güvenlik Duvarı daha yapılandırmak için daha karmaşıktır, ancak IPsec bağlantısı daha özelleştirme için izin verir.

## <a name="8-assign-the-ipsec-gpo-to-the-ou"></a><a name="assigngpo"></a>8. Ou'ya IPsec GPO'yu atama

1. İlkeyi görüntüleyin. Güvenlik grubu ilkesi tanımlanır, ancak henüz atanmamış.

   [![49 49]][49]
2. Güvenlik grubu ilkesini OU **IPSecOU'ya**atamak için güvenlik ilkesini sağ tıklatın ve **Ata'yı**seçti.
   Ou'ya ait her bilgisayar, güvenlik grubu ilkesine sahip olacaktır.

   [![50]][50]

## <a name="check-traffic-encryption"></a><a name="checktraffic"></a>Trafik şifrelemeyi denetleme

OU'da uygulanan şifrelemegine göz atın, tüm Azure VM'lerine ve ana bilgisayara IIS yükleyin1. Her IIS, 8080 portundaki HTTP isteklerine yanıt verecek şekilde özelleştirilmiştir.
Şifrelemeyi doğrulamak için OU'daki tüm bilgisayarlara bir ağ algılayıcısı (Wireshark gibi) yükleyebilirsiniz.
Powershell komut dosyası, bağlantı noktası 8080'de HTTP isteklerini oluşturmak için HTTP istemcisi olarak çalışır:

```powershell
$url = "http://10.0.1.20:8080"
while ($true) {
try {
[net.httpWebRequest]
$req = [net.webRequest]::create($url)
$req.method = "GET"
$req.ContentType = "application/x-www-form-urlencoded"
$req.TimeOut = 60000

$start = get-date
[net.httpWebResponse] $res = $req.getResponse()
$timetaken = ((get-date) - $start).TotalMilliseconds

Write-Output $res.Content
Write-Output ("{0} {1} {2}" -f (get-date), $res.StatusCode.value__, $timetaken)
$req = $null
$res.Close()
$res = $null
} catch [Exception] {
Write-Output ("{0} {1}" -f (get-date), $_.ToString())
}
$req = $null

# uncomment the line below and change the wait time to add a pause between requests
#Start-Sleep -Seconds 1
}

```
Aşağıdaki ağ yakalama, yalnızca şifrelenmiş trafiği eşleştirmek için ekran filtresi ESP ile şirket içi host1 sonuçlarını gösterir:

[![51 51]][51]

Powershell komut dosyasını premisies (HTTP istemcisi) üzerinde çalıştırDıysanız, Azure VM'deki ağ yakalama benzer bir izleme gösterir.

## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).

<!--Image References-->

[1]: ./media/expressroute-howto-ipsec-transport-private-windows/network-diagram.png "ExpressRoute üzerinden 1 ağ Diyagramı IPsec taşıma modu"
[4]: ./media/expressroute-howto-ipsec-transport-private-windows/ipsec-interesting-traffic.png "IPsec ilginç trafik"
[5]: ./media/expressroute-howto-ipsec-transport-private-windows/windows-ipsec.png "Windows IPsec ilkesi"
[9]: ./media/expressroute-howto-ipsec-transport-private-windows/ou.png "Grup Politikasında Organizasyon Birimi"
[10]: ./media/expressroute-howto-ipsec-transport-private-windows/create-gpo-ou.png "OU ile ilişkili bir GPO oluşturmak"
[11]: ./media/expressroute-howto-ipsec-transport-private-windows/gpo-name.png "OU ile ilişkili GPO bir ad atamak"
[12]: ./media/expressroute-howto-ipsec-transport-private-windows/edit-gpo.png "GPO'yu edin"
[15]: ./media/expressroute-howto-ipsec-transport-private-windows/manage-ip-filter-list-filter-actions.png "IP Filtre Listelerini ve Filtre Eylemlerini Yönet"
[16]: ./media/expressroute-howto-ipsec-transport-private-windows/add-filter-action.png "Filtre Eylemi ekle"
[17]: ./media/expressroute-howto-ipsec-transport-private-windows/action-wizard.png "Aksiyon Sihirbazı"
[18]: ./media/expressroute-howto-ipsec-transport-private-windows/filter-action-name.png "Filtre Eylem adı"
[19]: ./media/expressroute-howto-ipsec-transport-private-windows/filter-action.png "Filtre Eylemi"
[20]: ./media/expressroute-howto-ipsec-transport-private-windows/filter-action-no-ipsec.png "belirtin davranışı güvenli olmayan bir bağlantı kurulur"
[21]: ./media/expressroute-howto-ipsec-transport-private-windows/security-method.png "güvenlik mekanizması"
[22]: ./media/expressroute-howto-ipsec-transport-private-windows/custom-security-method.png "Özel güvenlik yöntemi"
[23]: ./media/expressroute-howto-ipsec-transport-private-windows/filter-actions-list.png "filtre eylem listesi"
[24]: ./media/expressroute-howto-ipsec-transport-private-windows/add-new-ip-filter.png "Yeni bir IP filtre listesi ekleme"
[25]: ./media/expressroute-howto-ipsec-transport-private-windows/ip-filter-http-traffic.png "IP filtresine HTTP trafiği ekleme"
Her iki yönde [26]: ./media/expressroute-howto-ipsec-transport-private-windows/match-both-direction.pngmaç "paketi"
Kaynak alt net [27]: ./media/expressroute-howto-ipsec-transport-private-windows/source-address.png "seçimi"
[28]: ./media/expressroute-howto-ipsec-transport-private-windows/source-network.png "Kaynak Ağ"
[29]: ./media/expressroute-howto-ipsec-transport-private-windows/destination-network.png "Hedef Ağ"
[30]: ./media/expressroute-howto-ipsec-transport-private-windows/protocol.png "Protokol"
[31]: ./media/expressroute-howto-ipsec-transport-private-windows/source-port-and-destination-port.png "kaynak bağlantı noktası ve hedef bağlantı noktası"
[32]: ./media/expressroute-howto-ipsec-transport-private-windows/ip-filter-list.png "filtre listesi"
HTTP trafiği ile [33]: ./media/expressroute-howto-ipsec-transport-private-windows/ip-filter-for-http.png "IP filtre listesi"
[34]: ./media/expressroute-howto-ipsec-transport-private-windows/ip-filter-add-second-entry.png "İkinci bir IP Filtresi Ekleme"
[35]: ./media/expressroute-howto-ipsec-transport-private-windows/ip-filter-second-entry.png "IP Filtresi listesi-ikinci giriş"
[36]: ./media/expressroute-howto-ipsec-transport-private-windows/ip-filter-list-2entries.png "IP Filtresi listesi-ikinci giriş"
[37]: ./media/expressroute-howto-ipsec-transport-private-windows/create-ip-security-policy.png "IP Güvenlik ilkesini oluşturun"
 "IPsec politikasının" [38]: ./media/expressroute-howto-ipsec-transport-private-windows/ipsec-policy-name.pngadı
[39]: ./media/expressroute-howto-ipsec-transport-private-windows/security-policy-wizard.png "IPsec ilkesi sihirbazı"
[40]: ./media/expressroute-howto-ipsec-transport-private-windows/edit-security-policy.png "IPsec politikasının 40'ı"
[41]: ./media/expressroute-howto-ipsec-transport-private-windows/add-new-rule.png "IPsec ilkesine yeni güvenlik kuralı ekleyin"
[42]: ./media/expressroute-howto-ipsec-transport-private-windows/create-security-rule.png "yeni bir güvenlik kuralı oluşturma"
[43]: ./media/expressroute-howto-ipsec-transport-private-windows/transport-mode.png "Taşıma Modu"
[44]: ./media/expressroute-howto-ipsec-transport-private-windows/network-type.png "Ağ türü"
 "Mevcut IP Filtre Listesinin" [45]: ./media/expressroute-howto-ipsec-transport-private-windows/selection-filter-list.pngseçimi
 "Mevcut Filtre Eylemi'nin" [46]: ./media/expressroute-howto-ipsec-transport-private-windows/selection-filter-action.pngseçimi
 "Kimlik doğrulama yönteminin" [47]: ./media/expressroute-howto-ipsec-transport-private-windows/authentication-method.pngseçimi
 "Güvenlik politikasının oluşturulması sürecinin" [48]: ./media/expressroute-howto-ipsec-transport-private-windows/security-policy-completed.pngsonu
 "GPO'ya bağlı ancak atanmamış" [49]: ./media/expressroute-howto-ipsec-transport-private-windows/gpo-not-assigned.pngIPsec politikası
 "GPO'ya atanan" [50]: ./media/expressroute-howto-ipsec-transport-private-windows/gpo-assigned.pngIPsec politikası
[51]: ./media/expressroute-howto-ipsec-transport-private-windows/encrypted-traffic.png "IPsec şifreli trafik yakalama"
