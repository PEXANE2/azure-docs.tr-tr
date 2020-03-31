---
title: ASE v1'i yapılandır
description: App Service Environment v1'in yapılandırması, yönetimi ve izlenmesi. Bu doküman yalnızca eski v1 ASE'yi kullanan müşteriler için sağlanır.
author: ccompy
ms.assetid: b5a1da49-4cab-460d-b5d2-edd086ec32f4
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: b37708e27887b20604a1fe921f14e51387793737
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74687252"
---
# <a name="configuring-an-app-service-environment-v1"></a>Uygulama Hizmet Ortamını Yapılandırma v1

> [!NOTE]
> Bu makale, App Service Environment v1 hakkındadır.  Uygulama Hizmet Ortamı'nın kullanımı daha kolay olan ve daha güçlü altyapıda çalışan daha yeni bir sürümü vardır. Yeni sürüm hakkında daha fazla bilgi edinmek [için Uygulama Hizmet Ortamına Giriş](intro.md)ile başlayın.
> 

## <a name="overview"></a>Genel Bakış
Yüksek düzeyde, Azure Uygulama Hizmet Ortamı birkaç ana bileşenden oluşur:

* Barındırılan Hizmet, Uygulama Hizmet Ortamında çalışan kaynakları hesaplama
* Depolama
* Bir veritabanı
* Klasik(V1) veya Kaynak Yöneticisi(V2) Azure Sanal Ağı (VNet) 
* Uygulama Hizmet Ortamı'na sahip bir alt ağ, içinde çalışan hizmet barındırılan

### <a name="compute-resources"></a>Kaynak hesaplama
Dört kaynak havuzunuz için bilgi işlem kaynaklarını kullanırsınız.  Her Uygulama Hizmet Ortamı (ASE) ön uçları ve üç olası işçi havuzları kümesi vardır. Üç alt havuzuda da kullanmanız gerekmez- isterseniz, sadece bir veya iki tane kullanabilirsiniz.

Kaynak havuzlarında (ön uçlar ve çalışanlar) ana bilgisayarlara doğrudan kiracı erişilemez. Uzaktan Masaüstü Protokolü'nü (RDP) bunlara bağlanmak, sağlamalarını değiştirmek veya bunlar adak için yönetici lik yapmak için kullanamazsınız.

Kaynak havuzu miktarını ve boyutunu ayarlayabilirsiniz. BIR ASE'de, P1'den P4'e etiketlenmiş dört boyut seçeneğiniz vardır. Bu boyutlar ve fiyatlandırmaları hakkında ayrıntılı bilgi [için, Uygulama Hizmeti fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/)için bkz.
Miktarı veya boyutu değiştirme ölçek işlemi olarak adlandırılır.  Aynı anda yalnızca bir ölçek işlemi gerçeklebiliyor.

**Ön uçlar**: Ön uçlar, ASE'nizde bulunan uygulamalarınız için HTTP/HTTPS uç noktalarıdır. İş yüklerini ön uçlarda çalıştıramazsın.

* ASE, dev/test iş yükleri ve düşük düzeyli üretim iş yükleri için yeterli olan iki P2 ile başlar. Orta ve ağır üretim iş yükleri için P3'leri şiddetle tavsiye ediyoruz.
* Orta ve ağır üretim iş yükleri için, zamanlanmış bakım gerçekleştiğinde yeterli ön uçların çalışmadığından emin olmak için en az dört P3'e sahip olduğunuzu öneririz. Planlanmış bakım faaliyetleri bir seferde bir ön uç aşağı getirecektir. Bu, bakım faaliyetleri sırasında genel kullanılabilir ön uç kapasitesini azaltır.
* Ön uçların sağlanması bir saat kadar sürebilir. 
* Daha fazla ölçek ince ayar için, ön uç havuzu için CPU yüzdesi, Bellek yüzdesi ve Etkin İstekler ölçümlerini izlemeniz gerekir. Cpu veya Bellek yüzdeleri P3'leri çalıştırırken yüzde 70'in üzerindeyse, daha fazla ön uç ekleyin. Etkin İstekler değeri ön uç başına ortalama 15.000 ile 20.000 istek arasında ysa, daha fazla ön uç eklemeniz gerekir. Genel hedef, CPU ve Bellek yüzdelerini %70'in altında tutmak ve P3'leri çalıştırırken etkin istekler ön uç başına ortalama 15.000 istek tesbit etmektir.  

**İşçiler**: Çalışanlar, uygulamalarınızın gerçekte çalıştığı yerdir. Uygulama Hizmeti planlarınızı ölçeklendirdiğinizde, ilişkili işçi havuzundaki çalışanları kullanır.

* Hemen işçi ekleyemezsiniz. Sağlanması bir saat kadar sürebilir.
* Herhangi bir havuz için bir bilgi işlem kaynağının boyutunu ölçekleme güncelleştirme etki alanı başına < 1 saat sürer. Bir ASE'de 20 güncelleştirme alanı vardır. Bir işçi havuzunun işlem boyutunu 10 örnekle ölçeklendirseniz, tamamlanması 10 saat kadar sürebilir.
* Bir alt havuzda kullanılan işlem kaynaklarının boyutunu değiştirirseniz, bu alt havuzda çalışan uygulamaların soğuk başlamasına neden olursunuz.

Herhangi bir uygulama çalıştırmayan bir çalışan havuzunun bilgi işlem kaynak boyutunu değiştirmenin en hızlı yolu:

* İşçi miktarını 2'ye küçültün.  Portaldaki minimum küçülme boyutu 2'dir. Örneklerinizi ele almak birkaç dakika nızı alacaktır. 
* Yeni işlem boyutunu ve örnek sayısını seçin. Buradan tamamlanması 2 saat kadar sürer.

Uygulamalarınız daha büyük bir bilgi işlem kaynağı boyutu gerektiriyorsa, önceki kılavuzdan yararlanamazsınız. Bu uygulamaları barındıran çalışan havuzunun boyutunu değiştirmek yerine, başka bir işçi havuzunu istenilen boyuttaki çalışanlarla doldurabilir ve uygulamalarınızı bu havuza taşıyabilirsiniz.

* Başka bir alt havuzda gerekli bilgi işlem boyutu ek örnekleri oluşturun. Bu sürenin tamamlanması bir saat kadar sürer.
* Yeni yapılandırılan alt havuza daha büyük boyut gerektiren uygulamaları barındıran Uygulama Hizmeti planlarınızı yeniden atayın. Bu, tamamlanması bir dakikadan az sürer hızlı bir işlemdir.  
* Artık kullanılmayan örneklere ihtiyacınız yoksa ilk çalışan havuzunu küçültün. Bu işlemin tamamlanması birkaç dakika sürer.

**Otomatik ölçekleme**: Bilgi işlem kaynak tüketiminizi yönetmenize yardımcı olabilecek araçlardan biri otomatik ölçeklemedir. Ön uç veya çalışan havuzları için otomatik ölçekleme kullanabilirsiniz. Sabahherhangi bir havuz türüörnekleri artırmak ve akşam azaltmak gibi şeyler yapabilirsiniz. Veya belki de bir işçi havuzunda bulunan işçi sayısı belirli bir eşiğin altına düştüğünde örnekler ekleyebilirsiniz.

Bilgi işlem kaynak havuzu ölçümleri etrafında otomatik ölçeklendirme kuralları ayarlamak istiyorsanız, sağlamanın gerektirdiği zamanı aklınızda bulundurun. Uygulama Hizmet Ortamlarını otomatik ölçeklendirme hakkında daha fazla bilgi için, [Bir Uygulama Hizmet Ortamında otomatik ölçeklendirmeyi nasıl yapılandırılatır'][ASEAutoscale]a bakın.

### <a name="storage"></a>Depolama
Her ASE 500 GB depolama alanı ile yapılandırılmıştır. Bu alan ASE'deki tüm uygulamalarda kullanılır. Bu depolama alanı ASE'nin bir parçasıdır ve şu anda depolama alanınızı kullanmak üzere değiştirilemez. Sanal ağ yönlendirmeveya güvenliğinizde ayarlamalar yapıyorsanız, Azure Depolama'ya erişime izin vermeniz gerekir veya ASE çalışamaz.

### <a name="database"></a>Database
Veritabanı, ortamı tanımlayan bilgilerin yanı sıra içinde çalışan uygulamalarla ilgili ayrıntıları da tutar. Bu da Azure tarafından tutulan aboneliğin bir parçasıdır. Bu doğrudan manipüle etme yeteneğine sahip olduğun bir şey değil. Sanal ağ yönlendirmeveya güvenliğinizde ayarlamalar yapıyorsanız, SQL Azure'a erişime izin vermeniz gerekir veya ASE çalışamaz.

### <a name="network"></a>Ağ
ASE'nizle birlikte kullanılan VNet, ASE'yi oluşturduğunuzda veya önceden yaptığınız bir tane yken yaptığınız bir tane olabilir. ASE oluşturma sırasında alt ağı oluşturduğunuzda, ASE'yi sanal ağla aynı kaynak grubunda olmaya zorlar. ASE'niz tarafından kullanılan kaynak grubunun VNet'inizinkinden farklı olması gerekiyorsa, bir kaynak yöneticisi şablonu kullanarak ASE'nizi oluşturmanız gerekir.

AsE için kullanılan sanal ağ üzerinde bazı kısıtlamalar vardır:

* Sanal ağ bölgesel bir sanal ağ olmalıdır.
* ASE'nin dağıtıldığı 8 veya daha fazla adrese sahip bir alt ağ olması gerekir.
* Bir ASE barındırmak için bir alt ağ kullanıldıktan sonra, alt ağın adres aralığı değiştirilemez. Bu nedenle, alt ağ gelecekteki ASE büyümesini karşılamak için en az 64 adres içerir öneririz.
* Alt ağda ASE'den başka bir şey olamaz.

ASE içeren barındırılan hizmetin aksine, [sanal ağ][virtualnetwork] ve alt ağ kullanıcı denetimi altındadır.  Sanal ağınızı Sanal Ağ UI veya PowerShell üzerinden yönetebilirsiniz.  Bir ASE, Klasik veya Kaynak Yöneticisi VNet'te dağıtılabilir.  Portal ve API deneyimleri Klasik ve Kaynak Yöneticisi VNets arasında biraz farklıdır, ancak ASE deneyimi aynıdır.

BIR ASE barındırmak için kullanılan VNet, özel RFC1918 IP adreslerini kullanabilir veya ortak IP adreslerini kullanabilir.  RFC1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) kapsamında olmayan bir IP aralığı kullanmak istiyorsanız, ASE oluşturma öncesinde ASE tarafından kullanılmak üzere VNet ve alt ağınızı oluşturmanız gerekir.

Bu özellik Azure Uygulama Hizmetini sanal ağınıza yerleştirdiği için, ASE'nizde barındırılan uygulamalarınızın artık ExpressRoute veya siteden siteye sanal özel ağlar (VPN'ler) aracılığıyla kullanılabilen kaynaklara erişebileceği anlamına gelir. Uygulama Hizmet Ortamınızda bulunan uygulamalar, Uygulama Hizmeti Ortamınızı barındıran sanal ağdaki kaynaklara erişmek için ek ağ özellikleri gerektirmez. Bu, sanal ağınızdaki kaynaklara ulaşmak veya sanal ağınıza bağlanmak için VNET Tümleştirmesini veya Hibrit Bağlantıları kullanmanız gerekolmadığı anlamına gelir. Sanal ağınıza bağlı olmayan ağlardaki kaynaklara erişmek için bu özelliklerin her ikisini de kullanmaya devam edebilirsiniz.

Örneğin, VNET Tümleştirme'yi aboneliğinizde bulunan ancak ASE'nizin içinde olduğu sanal ağa bağlı olmayan bir sanal ağla tümleştirmek için kullanabilirsiniz. Yine de, normalde kullanabileceğiniz gibi diğer ağlardaki kaynaklara erişmek için Karma Bağlantıları da kullanabilirsiniz.  

Sanal ağınızı ExpressRoute VPN ile yapılandırMışsanız, bir ASE'nin bazı yönlendirme gereksinimlerinin farkında olmalısınız. Ase ile uyumsuz bazı kullanıcı tanımlı rota (UDR) yapılandırmaları vardır. ExpressRoute ile sanal ağda ASE çalıştırma hakkında daha fazla bilgi için [ExpressRoute ile sanal ağda Uygulama Hizmeti Ortamı Çalıştırma'ya][ExpressRoute]bakın.

#### <a name="securing-inbound-traffic"></a>Gelen trafiği güvence altına alma
ASE'nize gelen trafiği denetlemek için iki ana yöntem vardır.  Bir [Uygulama Hizmet Ortamında gelen trafiği nasıl kontrol edebilirsiniz](app-service-app-service-environment-control-inbound-traffic.md) ve ayrıca bir İç Yük Dengeleyicisi (ILB) ile ASE'nizi yapılandırmak için, burada açıklandığı gibi HANGI IP adreslerinin ASE'nize erişebileceğini denetlemek için Ağ Güvenlik Grupları'nı (NSGs) kullanabilirsiniz.  NSG'leri kullanarak ILB ASE'nize erişimi kısıtlamak istiyorsanız, bu özellikler birlikte de kullanılabilir.

Bir ASE oluşturduğunuzda, VNet'inizde bir VIP oluşturur.  İki VIP türü vardır, dış ve iç.  Harici bir VIP ile bir ASE oluşturduğunuzda, ASE'nizdeki uygulamalara internet üzerinden bir IP adresi üzerinden erişebilirsiniz. Dahili'yi seçtiğinizde ASE'niz bir ILB ile yapılandırılacaktır ve doğrudan interneterişimine sahip olmayacaktır.  ILB ASE hala harici bir VIP gerektirir, ancak yalnızca Azure yönetimi ve bakım erişimi için kullanılır.  

ILB ASE oluşturma sırasında ILB ASE tarafından kullanılan alt etki alanını sağlarsınız ve belirttiğiniz alt etki alanı için kendi DNS'nizi yönetmek zorunda kalırsınız.  Alt alan adını ayarladığınızdan, HTTPS erişimi için kullanılan sertifikayı yönetmeniz gerekir.  ASE oluşturulduktan sonra sertifikayı sağlamanız istenir.  ILB ASE oluşturma ve kullanma hakkında daha fazla bilgi edinmek için [Uygulama Hizmet Ortamına sahip Dahili Yük Dengeleyicisi'ni][ILBASE]kullanma'yı okuyun. 

## <a name="portal"></a>Portal
Azure portalındaki Kullanıcı Arabirimi'ni kullanarak Uygulama Hizmeti Ortamınızı yönetebilir ve izleyebilirsiniz. BIR ASE'niz varsa, kenar çubuğunuzda Uygulama Hizmeti simgesini görme olasılığınız yüksektir. Bu sembol, Azure portalındaki Uygulama Hizmeti Ortamlarını temsil etmek için kullanılır:

![Uygulama Hizmet Ortamı simgesi][1]

Tüm Uygulama Hizmet Ortamlarınızı listeleyen UI'yi açmak için simgeyi kullanabilir veya Uygulama Hizmet Ortamlarını seçmek için kenar çubuğunun altındaki köşeli (>" simgesini) seçebilirsiniz. Listelenen AsEs birini seçerek, izlemek ve yönetmek için kullanılan Kullanıcı Aracı'nı açarsınız.

![Uygulama Hizmet Ortamınızı izlemek ve yönetmek için kullanıcı arabirimi][2]

Bu ilk bıçak, kaynak havuzu başına bir metrik grafikle birlikte ASE'nizin bazı özelliklerini gösterir. **Essentials** bloğunda gösterilen özelliklerden bazıları, bıçağın ilişkili olduğu şekilde açılacak köprülerdir. Örneğin, ASE'nizin içinde çalıştığınız sanal ağla ilişkili Kullanıcı Arabirimi'ni açmak için **Sanal Ağ** adını seçebilirsiniz. **Uygulama Hizmeti planları** ve **Uygulamalarıher** ase içinde bu öğeleri listeleyen bıçakları açılır.  

### <a name="monitoring"></a>İzleme
Grafikler, her kaynak havuzunda çeşitli performans ölçümleri görmenizi sağlar. Ön uç havuzu için ortalama CPU ve belleği izleyebilirsiniz. Alt havuzlar için, kullanılan miktarı ve kullanılabilir miktarı izleyebilirsiniz.

Birden çok Uygulama Hizmeti planı, çalışanların bir işçi havuzunda kullanılmasını sağlayabilir. İş yükü ön uç sunucularla aynı şekilde dağıtılmaz, bu nedenle CPU ve bellek kullanımı yararlı bilgiler konusunda çok fazla bilgi sağlamaz. Özellikle bu sistemi başkalarının kullanması için yönetiyorsanız, kaç işçi kullandığınızı ve kullanılabilir olduğunuzu izlemek daha önemlidir.  

Uyarıları ayarlamak için grafiklerde izlenebilen tüm ölçümleri de kullanabilirsiniz. Burada uyarı lar ayarlamak, App Service'in diğer yerlerinde olduğu gibi çalışır. **Uyarılar** UI bölümünden veya herhangi bir ölçüm ui'sini delmekten ve **Uyarı Ekle'yi**seçerek bir uyarı ayarlayabilirsiniz.

![Ölçümler UI][3]

Az önce tartışılan ölçümler, Uygulama Hizmeti Ortamı ölçümleridir. Uygulama Hizmeti planı düzeyinde kullanılabilen ölçümler de vardır. Cpu ve bellek izleme çok mantıklı olduğu yerdir.

Bir ASE'de, tüm Uygulama Hizmeti planları özel App Service planlarıdır. Bu, bu Uygulama Hizmeti planına tahsis edilen ana bilgisayarlarda çalışan tek uygulamanın, bu Uygulama Hizmeti planındaki uygulamalar olduğu anlamına gelir. Uygulama Hizmeti planınızdaki ayrıntıları görmek için, ASE UI'deki listelerden veya **Uygulama Hizmetine Gözat planlarından** (hepsini listeleyen) Uygulama Hizmeti planınızı getirin.   

### <a name="settings"></a>Ayarlar
ASE bıçağının içinde, birkaç önemli özellik içeren bir **Ayarlar** bölümü vardır:

**Ayarlar** > **Özellikleri**: ASE bıçağınızı açtığınızda **Ayarlar** bıçağı otomatik olarak açılır. Üstte **Properties**olduğunu. **Burada Essentials**ne gördüğünüze gereksiz öğeleri bir dizi vardır, ama ne çok yararlı **Sanal IP Adresi**, hem de Giden IP **Adresleri**.

![Ayarlar blade ve Özellikleri][4]

**Ayarlar** > **IP Adresleri**: ASE'nizde bir IP Secure Sockets Layer (SSL) uygulaması oluşturduğunuzda bir IP SSL adresine ihtiyacınız vardır. Bir tane elde etmek için, ASE'nizin sahip olduğu ve tahsis edilebilen IP SSL adreslerine ihtiyacı vardır. Bir ASE oluşturulduğunda, bu amaç için bir IP SSL adresi vardır, ancak daha fazlasını ekleyebilirsiniz. [App Service fiyatlandırmasında][AppServicePricing] (SSL bağlantıları bölümünde) gösterildiği gibi ek IP SSL adresleri için bir ücret alınr. Ek fiyat IP SSL fiyatıdır.

**Ayarlar** > **Ön Uç Havuz** / **Çalışma Havuzları**: Bu kaynak havuzu bıçaklarının her biri, bu kaynak havuzunu tam olarak ölçeklendirmek için denetim sağlamanın yanı sıra yalnızca bu kaynak havuzundaki bilgileri görme olanağı sunar.  

Her kaynak havuzu için temel bıçak, bu kaynak havuzu için ölçümleriçeren bir grafik sağlar. Ase bıçağındaki grafiklerde olduğu gibi, grafiğe girip istediğiniz uyarıları ayarlayabilirsiniz. Belirli bir kaynak havuzu için ASE bıçağından bir uyarı ayarlamak, kaynak havuzundan yapmakla aynı şeyi yapar. İşçi havuzu **Ayarları** bıçağından, bu alt havuzda çalışan tüm Uygulamalar veya Uygulama Hizmeti planlarına erişebilirsiniz.

![İşçi havuzu ayarları UI][5]

### <a name="portal-scale-capabilities"></a>Portal ölçeği özellikleri
Üç ölçekli işlem vardır:

* ASE'de IP SSL kullanımı için kullanılabilen IP adreslerinin sayısını değiştirme.
* Kaynak havuzunda kullanılan bilgi işlem kaynağının boyutunu değiştirme.
* Kaynak havuzunda el ile veya otomatik olarak kullanılan bilgi işlem kaynaklarının sayısını değiştirme.

Portalda, kaynak havuzlarınızda kaç sunucu olduğunu denetlemenin üç yolu vardır:

* Üstteki ana ASE bıçağından bir ölçek işlemi. Ön uç ve alt havuzlarıiçin birden çok ölçek yapılandırma değişiklikleri yapabilirsiniz. Hepsi tek bir işlem olarak uygulanır.
* **Ayarlar**altında olan tek tek kaynak havuzu **Ölçek** lendirme bıçağından el ile ölçeklendirme işlemi.
* Tek tek kaynak havuzu **Ölçek** bıçaktan ayarladığınız otomatik ölçeklendirme.

ASE bıçağındaki ölçek işlemini kullanmak için kaydırıcıyı istediğiniz miktara sürükleyin ve kaydedin. Bu UI, boyutu değiştirmeyi de destekler.  

![Ölçek UI][6]

Belirli bir kaynak havuzunda el kitabı veya otomatik ölçeklendirme özelliklerini kullanmak için, uygun şekilde **Ayarlar** > **Ön Uç Havuzu** / **Alt Havuzları'na** gidin. Sonra değiştirmek istediğiniz havuzu açın. **Ayarlar** > **Ölçeğine** gidin veya **Ayarlar** > **Ölçeklendirin.** **Scale Out** blade, örnek miktarını kontrol etmenizi sağlar. **Ölçeklendirme,** kaynak boyutunu denetlemenize olanak tanır.  

![Ölçek ayarları UI][7]

## <a name="fault-tolerance-considerations"></a>Hata toleransı hususları
Bir Uygulama Hizmet Ortamını en fazla 55 toplam bilgi işlem kaynağı kullanacak şekilde yapılandırabilirsiniz. Bu 55 işlem kaynağından yalnızca 50'si iş yüklerini barındırmak için kullanılabilir. Bunun nedeni iki yönlüdür. En az 2 ön uç işlem kaynağı vardır.  Bu da işçi havuzu tahsisini desteklemek için 53'e kadar kalıyor. Hata toleransı sağlamak için, aşağıdaki kurallara göre ayrılmış ek bir bilgi işlem kaynağına sahip olmanız gerekir:

* Her alt havuz, iş yükü atanmak için kullanılamayan en az 1 ek bilgi işlem kaynağına ihtiyaç duyar.
* Bir alt havuzdaki bilgi işlem kaynaklarının miktarı belirli bir değerin üzerine çıktığında, hata toleransı için başka bir işlem kaynağı gerekir. Ön uç havuzunda durum böyle değil.

Herhangi bir tek işçi havuzunda, hata toleransı gereksinimleri, bir işçi havuzuna atanan X kaynaklarının belirli bir değeri için şunlardır:

* X 2 ile 20 arasındaysa, iş yükleri için kullanabileceğiniz kullanılabilir hesaplama kaynaklarının miktarı X-1'dir.
* X 21 ile 40 arasındaysa, iş yükleri için kullanabileceğiniz kullanılabilir işlem kaynaklarının miktarı X-2'dir.
* X 41 ile 53 arasındaysa, iş yükleri için kullanabileceğiniz kullanılabilir işlem kaynaklarının miktarı X-3'tür.

Minimum ayak izi 2 ön uç sunucuve 2 işçi vardır.  Yukarıdaki ifadeler ile daha sonra, burada açıklığa kavuşturmak için birkaç örnek vardır:  

* Tek bir havuzda 30 çalışanVarsa, bunların 28'i iş yüklerini barındırmak için kullanılabilir.
* Tek bir havuzda 2 çalışanVarsa, iş yüklerini barındırmak için 1 kullanılabilir.
* Tek bir havuzda 20 çalışanVarsa, iş yüklerini barındırmak için 19 çalışan kullanılabilir.  
* Tek bir havuzda 21 çalışanVarsa, iş yüklerini barındırmak için yalnızca 19 kişi kullanılabilir.  

Hata toleransı yönü önemlidir, ancak belirli eşiklerin üzerinde ölçeklendirilirken bunu aklınızda tutmanız gerekir. 20 örnekten daha fazla kapasite eklemek istiyorsanız, 21 daha fazla kapasite eklemediği için 22 veya daha yüksek bir kapasiteye gidin. Aynı şey 40'ın üzerine çıkarsa, kapasite katan bir sonraki sayı 42'dir.  

## <a name="deleting-an-app-service-environment"></a>Uygulama Hizmet Ortamını Silme
Bir Uygulama Hizmet Ortamını silmek istiyorsanız, App Service Environment bıyıcunun üst kısmındaki **Silme** eylemini kullanmanız yeterlidir. Bunu yaptığınızda, bunu gerçekten yapmak istediğinizi doğrulamak için Uygulama Hizmet Ortamınızın adını girmeniz istenir. Bir Uygulama Hizmet Ortamını sildiğinizde, içindeki tüm içeriği de sildiğinizi unutmayın.  

![Uygulama Hizmeti Ortamı Kullanıcı Yı Silme][9]  

## <a name="getting-started"></a>Başlarken
Uygulama Hizmet Ortamlarına başlamak için [Uygulama Hizmeti Ortamı'nı nasıl oluşturabilirsiniz'a](app-service-web-how-to-create-an-app-service-environment.md)bakın.

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[Appserviceplans]: ../overview-hosting-plans.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[HowtoScale]: app-service-web-scale-a-web-app-in-an-app-service-environment.md
[ControlInbound]: app-service-app-service-environment-control-inbound-traffic.md
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/
[ASEAutoscale]: app-service-environment-auto-scale.md
[ExpressRoute]: app-service-app-service-environment-network-configuration-expressroute.md
[ILBASE]: app-service-environment-with-internal-load-balancer.md
