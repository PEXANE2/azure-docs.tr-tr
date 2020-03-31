---
title: VM'ler için Azure Monitöründen Günlükler Nasıl Sorgulanır?
description: Sanal Aygıtlar için Azure Monitor çözümü ölçümler toplar ve verileri kaydeder ve bu makalede kayıtlar açıklanır ve örnek sorgular içerir.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2020
ms.openlocfilehash: 71258b04bad9a7aec4e86564d51d1d6f3f8cac76
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80283812"
---
# <a name="how-to-query-logs-from-azure-monitor-for-vms"></a>VM'ler için Azure Monitor'dan günlükleri sorgulama

Sanal Taşıtlar için Azure Monitor performans ve bağlantı ölçümleri, bilgisayar ve süreç envanteri verileri ve sistem durumu bilgilerini toplar ve Azure Monitor'daki Log Analytics çalışma alanına iletir.  Bu [veriler, Azure](../../azure-monitor/log-query/log-query-overview.md) Monitor'da sorgu için kullanılabilir. Bu verileri geçiş planlaması, kapasite çözümlemesi, bulma ve isteğe bağlı performans sorun giderme gibi senaryolara uygulayabilirsiniz.

## <a name="map-records"></a>Kayıtları eşleme

Bir işlem veya bilgisayar başlatıldığında veya VMs Map özelliği için Azure Monitor'a binildiğinde oluşturulan kayıtlara ek olarak, her benzersiz bilgisayar ve işlem için saat başına bir kayıt oluşturulur. Bu kayıtlar aşağıdaki tablolarda özelliklere sahiptir. ServiceMapComputer_CL olaylardaki alanlar ve değerler, ServiceMap Azure Kaynak Yöneticisi API'deki Makine kaynağının alanlarıyla eşlenin. ServiceMapProcess_CL olaylardaki alanlar ve değerler, ServiceMap Azure Kaynak Yöneticisi API'deki İşlem kaynağının alanlarına eşlenir. ResourceName_s alanı, ilgili Kaynak Yöneticisi kaynağındaki ad alanıyla eşleşir. 

Benzersiz işlemleri ve bilgisayarları tanımlamak için kullanabileceğiniz dahili olarak oluşturulmuş özellikler vardır:

- Bilgisayar: Log Analytics çalışma alanı içindeki bir bilgisayarı benzersiz olarak tanımlamak için *ResourceId* veya *ResourceName_s* kullanın.
- İşlem: Log Analytics çalışma alanındaki bir işlemi benzersiz olarak tanımlamak için *ResourceId'yi* kullanın. *ResourceName_s,* işlemin yürütüldettiği makine bağlamında benzersizdir (MachineResourceName_s) 

Belirli bir işlem ve belirli bir zaman aralığındaki bilgisayar için birden çok kayıt bulunabildiği için, sorgular aynı bilgisayar veya işlem için birden fazla kayıt döndürebilir. Yalnızca en son kaydı eklemek `| summarize arg_max(TimeGenerated, *) by ResourceId` için sorguya ekleyin.

### <a name="connections-and-ports"></a>Bağlantılar ve bağlantı noktaları

Bağlantı Ölçümleri özelliği, Azure Monitor günlüklerinde iki yeni tablo sunar : VMConnection ve VMBoundPort. Bu tablolar, bir makinenin (gelen ve giden) bağlantılarının yanı sıra açık/etkin sunucu bağlantı noktaları hakkında bilgi sağlar. ConnectionMetrics, bir zaman penceresi boyunca belirli bir metrik elde etmek için araçlar sağlayan API'ler aracılığıyla da ortaya çıkarır. Dinleme soketinde *kabul* etmekten kaynaklanan TCP bağlantıları gelen, belirli bir IP ve bağlantı noktasına *bağlanarak* oluşturulanlar ise giden. Bir bağlantının yönü, **gelen** veya **giden**olarak ayarlanabilen Yön özelliği yle temsil edilir. 

Bu tablolardaki kayıtlar Bağımlılık Aracısı tarafından bildirilen verilerden oluşturulur. Her kayıt 1 dakikalık bir zaman aralığında bir gözlem temsil eder. TimeGenerated özelliği zaman aralığının başlangıcını gösterir. Her kayıt, ilgili varlığı, yani bağlantı veya bağlantı noktasını tanımlamak için bilgiler ve bu varlıkla ilişkili ölçümleri içerir. Şu anda, yalnızca IPv4 üzerinden TCP kullanılarak oluşan ağ etkinliği bildirilir. 

#### <a name="common-fields-and-conventions"></a>Ortak alanlar ve sözleşmeler 

Aşağıdaki alanlar ve konvansiyonlar hem VMConnection hem de VMBoundPort için geçerlidir: 

- Bilgisayar: Raporlama makinesinin tam nitelikli alan adı 
- AgentId: Log Analytics temsilcisine sahip bir makineiçin benzersiz tanımlayıcı  
- Makine: ServiceMap tarafından açığa çıkarılan makine için Azure Kaynak Yöneticisi kaynağının adı. *M-{GUID}* formundadır, *GUID* AgentId ile aynı GUID  
- İşlem: ServiceMap tarafından ortaya çıkarılan işlem için Azure Kaynak Yöneticisi kaynağının adı. *P-{hex string}* şeklindedir. İşlem bir makine kapsamı içinde benzersizdir ve makineler arasında benzersiz bir işlem kimliği oluşturmak, Makine ve İşlem alanlarını birleştirmek için. 
- ProcessName: Raporlama işleminin yürütülebilir adı.
- Tüm IP adresleri IPv4 kanonik formatında dizeleri, örneğin *13.107.3.160* 

Maliyet ve karmaşıklığı yönetmek için bağlantı kayıtları tek tek fiziksel ağ bağlantılarını temsil etmez. Birden çok fiziksel ağ bağlantısı, daha sonra ilgili tabloya yansıtılan mantıksal bir bağlantı halinde gruplandırılır.  Anlamı, *VMConnection* tablosundaki kayıtlar, gözlemlenen tek tek fiziksel bağlantıları değil, mantıksal bir gruplamayı temsil eder. Belirli bir dakika aralığında aşağıdaki öznitelikler için aynı değeri paylaşan fiziksel ağ bağlantısı, *VMConnection'da*tek bir mantıksal kayda toplanır. 

| Özellik | Açıklama |
|:--|:--|
|Yön |Bağlantının yönü, değer *gelen* veya *giden* |
|Makine |Bilgisayar FQDN |
|İşleme |Sürecin veya süreç gruplarının kimliği, bağlantıyı başlatma/kabul etme |
|KaynakIp |Kaynağın IP adresi |
|HedefIp |Hedefin IP adresi |
|Hedef Port |Hedefin bağlantı noktası numarası |
|Protokol |Bağlantı için kullanılan protokol.  Değerler *tcp'dir.* |

Gruplandırmanın etkisini hesaba katmak için, gruplanmış fiziksel bağlantı sayısı hakkında bilgiler kaydın aşağıdaki özelliklerinde sağlanır:

| Özellik | Açıklama |
|:--|:--|
|LinklerKurulan |Raporlama süresi penceresi sırasında kurulan fiziksel ağ bağlantısı sayısı |
|LinklerSonlandırıldı |Raporlama süresi sırasında sonlandırılan fiziksel ağ bağlantısı sayısı |
|LinklerBaşarısız |Raporlama süresi penceresinde başarısız olan fiziksel ağ bağlantısı sayısı. Bu bilgiler şu anda yalnızca giden bağlantılar için kullanılabilir. |
|LinklerCanlı |Raporlama süresi penceresinin sonunda açık olan fiziksel ağ bağlantısı sayısı|

#### <a name="metrics"></a>Ölçümler

Bağlantı sayısı ölçümlerine ek olarak, belirli bir mantıksal bağlantı veya ağ bağlantı noktasına gönderilen ve alınan verilerin hacmi hakkındaki bilgiler de kaydın aşağıdaki özelliklerine dahil edilir:

| Özellik | Açıklama |
|:--|:--|
|Baytgönderildi |Raporlama süresi sırasında gönderilen toplam bayt sayısı |
|BaytAlınan |Raporlama süresi sırasında alınan toplam bayt sayısı |
|Yanıtlar |Raporlama süresi penceresinde gözlenen yanıt ların sayısı. 
|ResponseTimeMax |Raporlama süresi sırasında gözlenen en büyük yanıt süresi (milisaniye). Değer yoksa, özellik boştur.|
|ResponseTimeMin |Raporlama süresi sırasında gözlenen en küçük yanıt süresi (milisaniye). Değer yoksa, özellik boştur.|
|ResponseTimeSum |Raporlama süresi penceresinde gözlenen tüm yanıt sürelerinin (milisaniye) toplamı. Değer yoksa, özellik boştur.|

Bildirilen üçüncü veri türü yanıt süresidir - bir arayanın bir bağlantı üzerinden gönderilen bir isteği bekleyerek ne kadar zaman harcadığı ve uzak bitiş noktası tarafından yanıtlandığıdır. Bildirilen yanıt süresi, temel uygulama protokolünün gerçek yanıt süresinin tahminidir. Fiziksel ağ bağlantısının kaynak ve hedef sonu arasındaki veri akışının gözlemine dayalı sezgisel kullanılarak hesaplanır. Kavramsal olarak, bir isteğin son baytının gönderenden ayrıldığı saat ile yanıtın son baytının ona geri geldiği saat arasındaki farktır. Bu iki zaman damgası, belirli bir fiziksel bağlantıdaki istek ve yanıt olaylarını ifade etmek için kullanılır. Aralarındaki fark, tek bir isteğin yanıt süresini temsil eder. 

Bu özelliğin bu ilk sürümünde, algoritmamız, belirli bir ağ bağlantısı için kullanılan gerçek uygulama protokolüne bağlı olarak değişen başarı dereceleriyle çalışabilecek bir yaklaşımdır. Örneğin, geçerli yaklaşım HTTP(S) gibi istek yanıtı tabanlı protokoller için iyi çalışır, ancak tek yönlü veya ileti sıra tabanlı iletişim ile çalışmaz.

Göz önünde bulundurulması gereken bazı önemli noktalar şunlardır:

1. Bir işlem aynı IP adresindeki ancak birden çok ağ arabirimi üzerindeki bağlantıları kabul ederse, her arabirim için ayrı bir kayıt bildirilir. 
2. Joker karakter IP'si içeren kayıtlar etkinlik içermez. Bunlar, makinedeki bir bağlantı noktasının gelen trafiğe açık olduğu gerçeğini temsil etmek için dahildir.
3. Ayrıntılı bilgi ve veri hacmini azaltmak için, belirli bir IP adresiyle eşleşen bir kayıt (aynı işlem, bağlantı noktası ve protokol için) olduğunda joker KARAKTER IP'li kayıtlar atlanır. Bir joker IP kaydı atlandığında, belirli IP adresine sahip IsWildcardBind kayıt özelliği, bağlantı noktasının raporlama makinesinin her arabirimi üzerinde açıkta olduğunu belirtmek için "True" olarak ayarlanır.
4. Yalnızca belirli bir arabirimde bağlı olan bağlantı noktaları False *olarak*ayarlanmış IsWildcardBind'e sahiptir.

#### <a name="naming-and-classification"></a>Adlandırma ve Sınıflandırma

Kolaylık sağlamak için, bir bağlantının uzak ucunun IP adresi RemoteIp özelliğine dahildir. Gelen bağlantılar için RemoteIp SourceIp ile aynıdır, giden bağlantılar için ise DestinationIp ile aynıdır. RemoteDnsCanonicalNames özelliği, RemoteIp için makine tarafından bildirilen DNS kanonik adlarını temsil eder. RemoteDnsQuestions ve RemoteClassification özellikleri ileride kullanılmak üzere ayrılmıştır. 

#### <a name="geolocation"></a>Coğrafi Konum

*VMConnection* ayrıca, her bağlantı kaydının uzak ucuiçin aşağıdaki kayıtlardaki coğrafi konum bilgilerini de içerir: 

| Özellik | Açıklama |
|:--|:--|
|Uzak Ülke |RemoteIp'e ev sahipliği yapan ülke/bölgenin adı.  Örneğin, *Amerika Birleşik Devletleri* |
|Uzaktan Latitude |Coğrafi konum enlemi. Örneğin, *47,68* |
|Uzaktan Boy |Coğrafi konum boylam. Örneğin, *-122,12* |

#### <a name="malicious-ip"></a>Kötü niyetli IP

*VMConnection* tablosundaki her RemoteIp özelliği, bilinen kötü amaçlı etkinlik le ilgili bir ip kümesiyle karşılaştırılır. RemoteIp kötü niyetli olarak tanımlanırsa, aşağıdaki özellikler kaydın aşağıdaki özelliklerinde doldurulur (IP kötü amaçlı olarak kabul edilmezse boşturlar)

| Özellik | Açıklama |
|:--|:--|
|Kötü Niyetli Ip |RemoteIp adresi |
|GöstergeThreadType |Tespit edilen tehdit göstergesi aşağıdaki değerlerden biridir, *Botnet*, *C2*, *CryptoMining*, *Darknet*, *DDos*, *MalwareUrl*, *Malware*, *Phishing*, *Proxy*, *PUA*, *İzleme Listesi*.   |
|Açıklama |Gözlenen tehdidin tanımı. |
|TLPLevel |Trafik Işık Protokolü (TLP) Düzeyi tanımlanan değerlerden biridir, *Beyaz,* *Yeşil,* *Kehribar,* *Kırmızı*. |
|Güvenilirlik |Değerler *0 - 100 arasındadır.* |
|Severity |Değerler *0 - 5,* *5* en şiddetli ve *0* hiç şiddetli değildir. Varsayılan değer *3*3'dür.  |
|İlk RaporTarihi |Sağlayıcı göstergeyi ilk kez rapor etti. |
|LastReportedDateTime |Gösterge interflow tarafından en son görüldüğünde. |
|ısactive |Göstergelerin *True* veya *False* değeriyle devre dışı bırakıldığını gösterir. |
|ReportReferenceLink |Belirli bir gözlemlenebilir ile ilgili raporlara bağlantılar. |
|Ek Bilgi |Varsa, gözlenen tehdit hakkında ek bilgi sağlar. |

### <a name="ports"></a>Bağlantı Noktaları 

Gelen trafiği etkin olarak kabul eden veya trafiği kabul edebilecek, ancak raporlama süresi penceresi sırasında boşta olan bir makinedeki bağlantı noktaları VMBoundPort tablosuna yazılır.  

VMBoundPort'taki her kayıt aşağıdaki alanlar tarafından tanımlanır: 

| Özellik | Açıklama |
|:--|:--|
|İşleme | Bağlantı noktasının ilişkili olduğu işlemin (veya işlem gruplarının) kimliği.|
|ıp | Bağlantı noktası IP adresi (joker karakter IP, *0.0.0.0*olabilir) |
|Bağlantı noktası |Bağlantı noktası numarası |
|Protokol | Protokol.  Örneğin, *tcp* veya *udp* (şu anda yalnızca *tcp* desteklenir).|
 
Bir bağlantı noktasının kimliği yukarıdaki beş alandan türetilmiştir ve PortId özelliğinde depolanır. Bu özellik, zaman içinde belirli bir bağlantı noktasının kayıtlarını hızla bulmak için kullanılabilir. 

#### <a name="metrics"></a>Ölçümler 

Bağlantı noktası kayıtları, kendileriyle ilişkili bağlantıları temsil eden ölçümleri içerir. Şu anda, aşağıdaki ölçümler rapor edilir (her metrik için ayrıntılar önceki bölümde açıklanmıştır): 

- BaytlarGönderilen ve BaytAlınan 
- LinksKurulan, LinklerSon, LinksLive 
- ResposeTime, ResponseTimeMin, ResponseTimeMax, ResponseTimeSum 

Göz önünde bulundurulması gereken bazı önemli noktalar şunlardır:

- Bir işlem aynı IP adresindeki ancak birden çok ağ arabirimi üzerindeki bağlantıları kabul ederse, her arabirim için ayrı bir kayıt bildirilir.  
- Joker karakter IP'si içeren kayıtlar etkinlik içermez. Bunlar, makinedeki bir bağlantı noktasının gelen trafiğe açık olduğu gerçeğini temsil etmek için dahildir. 
- Ayrıntılı bilgi ve veri hacmini azaltmak için, belirli bir IP adresiyle eşleşen bir kayıt (aynı işlem, bağlantı noktası ve protokol için) olduğunda joker KARAKTER IP'li kayıtlar atlanır. Bir joker IP kaydı atlandığında, belirli IP adresine sahip kayıt için *IsWildcardBind* özelliği *True*olarak ayarlanır.  Bu, bağlantı noktasının raporlama makinesinin her arabirimi üzerinde açıkta olduğunu gösterir. 
- Yalnızca belirli bir arabirimde bağlı olan bağlantı noktaları False *olarak*ayarlanmış IsWildcardBind'e sahiptir. 

### <a name="vmcomputer-records"></a>VMComputer kayıtları

*VMComputer* türüne sahip kayıtlar, Bağımlılık aracısı olan sunucular için envanter verilerine sahiptir. Bu kayıtların özellikleri aşağıdaki tabloda dır:

| Özellik | Açıklama |
|:--|:--|
|TenantId | Çalışma alanı için benzersiz tanımlayıcı |
|SourceSystem | *İçgörüler* | 
|TimeGenerated | Kaydın zaman damgası (UTC) |
|Bilgisayar | Bilgisayar FQDN | 
|AgentId | Log Analytics aracısının benzersiz kimliği |
|Makine | ServiceMap tarafından açığa çıkarılan makine için Azure Kaynak Yöneticisi kaynağının adı. *GUID'in* AgentId ile aynı GUID olduğu *m-{GUID}* biçimindedir. | 
|DisplayName | Görünen ad | 
|FullDisplayName | Tam ekran adı | 
|Ana bilgisayar adı | Etki alanı adı olmayan makinenin adı |
|Önyükleme Süresi | Makine önyükleme süresi (UTC) |
|TimeZone | Normalleştirilmiş saat dilimi |
|SanallaştırmaDevlet | *sanal*, *hipervizör*, *fiziksel* |
|Ipv4Adresler | IPv4 adresleri dizisi | 
|Ipv4SubnetMaskeler | IPv4 alt ağ maskeleri dizisi (Ipv4Addresses ile aynı sırada). |
|Ipv4DefaultAğ Geçidi | IPv4 ağ geçitleri dizisi | 
|Ipv6Adresler | IPv6 adresleri dizisi | 
|MacAdresleri | MAC adresleri dizisi | 
|DnsNames | Makineyle ilişkili DNS adları dizisi. |
|BağımlılıkAgentVersion | Makinede çalışan Bağımlılık aracısının sürümü. | 
|İşletim SistemiAile | *Linux*, *Windows* |
|İşletim SistemiFullName | İşletim sisteminin tam adı | 
|FizikselBellekMB | Megabaytfiziksel bellek | 
|Cpu | İşlemci sayısı | 
|CpuSpeed | MHz'de CPU hızı | 
|VirtualMachineType | *hyperv*, *vmware*, *xen* |
|VirtualMachineNativeid | Hypervisor tarafından atanan VM Kimliği | 
|VirtualMachineNativeName | VM adı |
|VirtualMachineHypervisorId | VM'ye ev sahipliği yapan hipervizörün benzersiz tanımlayıcısı |
|Hipervizör Tipi | *hyperv* |
|HipervisorId | Hipervizörün benzersiz kimliği | 
|Hosting Sağlayıcısı | *Azure* |
|_ResourceId | Azure kaynağı için benzersiz tanımlayıcı |
|AzureSubscriptionId | Aboneliğinizi tanımlayan genel olarak benzersiz bir tanımlayıcı | 
|AzureResourceGroup | Makinenin üyesi olduğu Azure kaynak grubunun adı. |
|AzureResourceName | Azure kaynağının adı |
|AzureKonum | Azure kaynağının konumu |
|AzureUpdateDomain | Azure güncelleştirme etki alanının adı |
|AzureFaultDomain | Azure hata etki alanının adı |
|AzureVmId | Azure sanal makinesinin benzersiz tanımlayıcısı |
|AzureBoyut | Azure VM'nin boyutu |
|AzureImagePublisher | Azure VM yayımcısının adı |
|AzureImageOffering | Azure VM teklif türünün adı | 
|AzureImageSku | Azure VM görüntüsünün SKU'su | 
|AzureImageSürüm | Azure VM görüntüsünün sürümü | 
|AzureCloudServiceName | Azure bulut hizmetinin adı |
|AzureCloudServiceDeployment | Bulut Hizmeti için Dağıtım Kimliği |
|AzureCloudServiceRoleName | Bulut Hizmeti rol adı |
|AzureCloudServiceRoleType | Bulut Hizmeti rol türü: *işçi* veya *web* |
|AzureCloudServiceInstanceId | Bulut Hizmeti rol örneği kimliği |
|AzureVmScaleSetName | Sanal makine ölçek kümesinin adı |
|AzureVmScaleSetDeployment | Sanal makine ölçeği ayarlama dağıtım kimliği |
|AzureVmScaleSetResourceId | Sanal makine ölçeğinin benzersiz tanımlayıcısı kaynak kümesi.|
|AzureVmScaleSetInstanceId | Sanal makine ölçek kümesinin benzersiz tanımlayıcısı |
|AzureServiceFabricClusterId | Azure Hizmet Kumaşı kümesinin benzersiz identiferi | 
|AzureServiceFabricClusterName | Azure Hizmet Kumaşı kümesinin adı |

### <a name="vmprocess-records"></a>VMProcess kayıtları

*VMProcess* türüne sahip kayıtlar, Bağımlılık aracısı ile sunucularda TCP'ye bağlı işlemler için envanter verilerine sahiptir. Bu kayıtların özellikleri aşağıdaki tabloda dır:

| Özellik | Açıklama |
|:--|:--|
|TenantId | Çalışma alanı için benzersiz tanımlayıcı |
|SourceSystem | *İçgörüler* | 
|TimeGenerated | Kaydın zaman damgası (UTC) |
|Bilgisayar | Bilgisayar FQDN | 
|AgentId | Log Analytics aracısının benzersiz kimliği |
|Makine | ServiceMap tarafından açığa çıkarılan makine için Azure Kaynak Yöneticisi kaynağının adı. *GUID'in* AgentId ile aynı GUID olduğu *m-{GUID}* biçimindedir. | 
|İşleme | Hizmet Haritası işleminin benzersiz tanımlayıcısı. *P-{GUID}* şeklindedir. 
|ExecutableName | Çalıştırılabilen işlemin adı | 
|DisplayName | İşlem ekran adı |
|Rol | İşlem rolü: *webserver*, *appServer*, *databaseServer*, *ldapServer*, *smbServer* |
|Grup | İşlem grubu adı. Aynı gruptaki işlemler mantıksal olarak ilişkilidir, örneğin, aynı ürün veya sistem bileşeninin bir parçası. |
|StartTime | İşlem havuzu başlangıç saati |
|FirstPid | İşlem havuzundaki ilk PID |
|Açıklama | İşlem açıklaması |
|CompanyName | Şirketin adı |
|InternalName | Dahili ad |
|ProductName | Ürünün adı |
|ProductVersion | Ürünün sürümü |
|Fileversion | Dosyanın sürümü |
|Çalıştırılabilir Yol |Yürütülebilir yolun |
|Commandline | Komut satırı |
|Başlangıç | Çalışma dizini |
|Hizmetler | İşlemin yürütüldettiği bir dizi hizmet |
|UserName | İşlemin yürütüldettiği hesap |
|UserDomain | İşlemin yürütüldettiği etki alanı |
|_ResourceId | Çalışma alanı içindeki bir işlem için benzersiz tanımlayıcı |


## <a name="sample-map-queries"></a>Örnek harita sorguları

### <a name="list-all-known-machines"></a>Bilinen tüm makineleri listele

```kusto
VMComputer | summarize arg_max(TimeGenerated, *) by _ResourceId
```

### <a name="when-was-the-vm-last-rebooted"></a>VM en son ne zaman yeniden başlatıldı?

```kusto
let Today = now(); VMComputer | extend DaysSinceBoot = Today - BootTime | summarize by Computer, DaysSinceBoot, BootTime | sort by BootTime asc
```

### <a name="summary-of-azure-vms-by-image-location-and-sku"></a>Azure VM'lerinin görüntü, konum ve SKU'ya göre özeti

```kusto
VMComputer | where AzureLocation != "" | summarize by ComputerName, AzureImageOffering, AzureLocation, AzureImageSku
```

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Yönetilen tüm bilgisayarların fiziksel bellek kapasitesini listele

```kusto
VMComputer | summarize arg_max(TimeGenerated, *) by _ResourceId | project PhysicalMemoryMB, Computer
```

### <a name="list-computer-name-dns-ip-and-os"></a>Bilgisayar adı, DNS, IP ve Işletim Sistemi'ni listele

```kusto
VMComputer | summarize arg_max(TimeGenerated, *) by _ResourceId | project Computer, OperatingSystemFullName, DnsNames, Ipv4Addresses
```

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Komut satırında "sql" olan tüm işlemleri bulun

```kusto
VMComputer | where CommandLine contains_cs "sql" | summarize arg_max(TimeGenerated, *) by _ResourceId
```

### <a name="find-a-machine-most-recent-record-by-resource-name"></a>Kaynak adına göre bir makine (en son kayıt) bulma

```kusto
search in (VMComputer) "m-4b9c93f9-bc37-46df-b43c-899ba829e07b" | summarize arg_max(TimeGenerated, *) by _ResourceId
```

### <a name="find-a-machine-most-recent-record-by-ip-address"></a>IP adresine göre bir makine (en son kayıt) bulma

```kusto
search in (VMComputer) "10.229.243.232" | summarize arg_max(TimeGenerated, *) by _ResourceId
```

### <a name="list-all-known-processes-on-a-specified-machine"></a>Bilinen tüm işlemleri belirli bir makinede listele

```kusto
VMProcess | where Machine == "m-559dbcd8-3130-454d-8d1d-f624e57961bc" | summarize arg_max(TimeGenerated, *) by _ResourceId
```

### <a name="list-all-computers-running-sql-server"></a>SQL Server çalıştıran tüm bilgisayarları listele

```kusto
VMComputer | where AzureResourceName in ((search in (VMProcess) "\*sql\*" | distinct Machine)) | distinct Computer
```

### <a name="list-all-unique-product-versions-of-curl-in-my-datacenter"></a>Curl'ün tüm benzersiz ürün sürümlerini veri merkezimde listele

```kusto
VMProcess | where ExecutableName == "curl" | distinct ProductVersion
```

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>CentOS çalıştıran tüm bilgisayarlardan oluşan bir bilgisayar grubu oluşturma

```kusto
VMComputer | where OperatingSystemFullName contains_cs "CentOS" | distinct ComputerName
```

### <a name="bytes-sent-and-received-trends"></a>Bayt lar gönderildi ve trendleri aldı

```kusto
VMConnection | summarize sum(BytesSent), sum(BytesReceived) by bin(TimeGenerated,1hr), Computer | order by Computer desc | render timechart
```

### <a name="which-azure-vms-are-transmitting-the-most-bytes"></a>Hangi Azure VM'ler en çok bayt iletiyor

```kusto
VMConnection | join kind=fullouter(VMComputer) on $left.Computer == $right.Computer | summarize count(BytesSent) by Computer, AzureVMSize | sort by count_BytesSent desc
```

### <a name="link-status-trends"></a>Bağlantı durumu eğilimleri

```kusto
VMConnection | where TimeGenerated >= ago(24hr) | where Computer == "acme-demo" | summarize dcount(LinksEstablished), dcount(LinksLive), dcount(LinksFailed), dcount(LinksTerminated) by bin(TimeGenerated, 1h) | render timechart
```

### <a name="connection-failures-trend"></a>Bağlantı hataları eğilimi

```kusto
VMConnection | where Computer == "acme-demo" | extend bythehour = datetime_part("hour", TimeGenerated) | project bythehour, LinksFailed | summarize failCount = count() by bythehour | sort by bythehour asc | render timechart
```

### <a name="bound-ports"></a>Bağlı Bağlantı Noktaları

```kusto
VMBoundPort
| where TimeGenerated >= ago(24hr)
| where Computer == 'admdemo-appsvr'
| distinct Port, ProcessName
```

### <a name="number-of-open-ports-across-machines"></a>Makineler arasında açık bağlantı noktası sayısı

```kusto
VMBoundPort
| where Ip != "127.0.0.1"
| summarize by Computer, Machine, Port, Protocol
| summarize OpenPorts=count() by Computer, Machine
| order by OpenPorts desc
```

### <a name="score-processes-in-your-workspace-by-the-number-of-ports-they-have-open"></a>Çalışma alanınızdaki süreçleri açtıkları bağlantı noktası sayısına göre puan

```kusto
VMBoundPort
| where Ip != "127.0.0.1"
| summarize by ProcessName, Port, Protocol
| summarize OpenPorts=count() by ProcessName
| order by OpenPorts desc
```

### <a name="aggregate-behavior-for-each-port"></a>Her bağlantı noktası için toplu davranış

Bu sorgu daha sonra, en çok gelen/giden trafiğin olduğu bağlantı noktaları, en çok bağlantıya sahip bağlantı noktaları gibi, etkinliklere göre bağlantı noktalarını puanlamak için kullanılabilir
```kusto
// 
VMBoundPort
| where Ip != "127.0.0.1"
| summarize BytesSent=sum(BytesSent), BytesReceived=sum(BytesReceived), LinksEstablished=sum(LinksEstablished), LinksTerminated=sum(LinksTerminated), arg_max(TimeGenerated, LinksLive) by Machine, Computer, ProcessName, Ip, Port, IsWildcardBind
| project-away TimeGenerated
| order by Machine, Computer, Port, Ip, ProcessName
```

### <a name="summarize-the-outbound-connections-from-a-group-of-machines"></a>Bir grup makineden giden bağlantıları özetle

```kusto
// the machines of interest
let machines = datatable(m: string) ["m-82412a7a-6a32-45a9-a8d6-538354224a25"];
// map of ip to monitored machine in the environment
let ips=materialize(VMComputer
| summarize ips=makeset(todynamic(Ipv4Addresses)) by MonitoredMachine=AzureResourceName
| mvexpand ips to typeof(string));
// all connections to/from the machines of interest
let out=materialize(VMConnection
| where Machine in (machines)
| summarize arg_max(TimeGenerated, *) by ConnectionId);
// connections to localhost augmented with RemoteMachine
let local=out
| where RemoteIp startswith "127."
| project ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=Machine;
// connections not to localhost augmented with RemoteMachine
let remote=materialize(out
| where RemoteIp !startswith "127."
| join kind=leftouter (ips) on $left.RemoteIp == $right.ips
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine=MonitoredMachine);
// the remote machines to/from which we have connections
let remoteMachines = remote | summarize by RemoteMachine;
// all augmented connections
(local)
| union (remote)
//Take all outbound records but only inbound records that come from either //unmonitored machines or monitored machines not in the set for which we are computing dependencies.
| where Direction == 'outbound' or (Direction == 'inbound' and RemoteMachine !in (machines))
| summarize by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol, RemoteIp, RemoteMachine
// identify the remote port
| extend RemotePort=iff(Direction == 'outbound', DestinationPort, 0)
// construct the join key we'll use to find a matching port
| extend JoinKey=strcat_delim(':', RemoteMachine, RemoteIp, RemotePort, Protocol)
// find a matching port
| join kind=leftouter (VMBoundPort 
| where Machine in (remoteMachines) 
| summarize arg_max(TimeGenerated, *) by PortId 
| extend JoinKey=strcat_delim(':', Machine, Ip, Port, Protocol)) on JoinKey
// aggregate the remote information
| summarize Remote=makeset(iff(isempty(RemoteMachine), todynamic('{}'), pack('Machine', RemoteMachine, 'Process', Process1, 'ProcessName', ProcessName1))) by ConnectionId, Direction, Machine, Process, ProcessName, SourceIp, DestinationIp, DestinationPort, Protocol
```

## <a name="performance-records"></a>Performans kayıtları
Bir tür *InsightsMetrics'e* sahip kayıtlar, sanal makinenin konuk işletim sisteminden performans verilerine sahiptir. Bu kayıtların özellikleri aşağıdaki tabloda dır:


| Özellik | Açıklama |
|:--|:--|
|TenantId | Çalışma alanı için benzersiz tanımlayıcı |
|SourceSystem | *İçgörüler* | 
|TimeGenerated | Değerin toplanma zamanı (UTC) |
|Bilgisayar | Bilgisayar FQDN | 
|Kaynak | *vm.azm.ms* |
|Ad Alanı | Performans sayacının kategorisi | 
|Adı | Performans sayacının adı |
|Val | Toplanan değer | 
|Etiketler | Kayıt la ilgili ayrıntılar. Farklı kayıt türleri ile kullanılan etiketler için aşağıdaki tabloya bakın.  |
|AgentId | Her bilgisayarın aracısı için benzersiz tanımlayıcı |
|Tür | *ÖngörülerÖlçümler* |
|_ResourceId_ | Sanal makinenin kaynak kimliği |

Şu anda *InsightsMetrics* tablosunda toplanan performans sayaçları aşağıdaki tabloda listelenmiştir:

| Ad Alanı | Adı | Açıklama | Birim | Etiketler |
|:---|:---|:---|:---|:---|
| Bilgisayar    | Sinyal             | Bilgisayar Kalp Atışı                        | | |
| Bellek      | Kullanılabilir MB           | Bellek Kullanılabilir Baytlar                    | Bayt          | memorySizeMB - Toplam bellek boyutu|
| Ağ     | YazmaBytesPerSecond   | Ağ Saniyede Bayt Yaz            | BaytPerİkinci | NetworkDeviceId - Cihazın kimliği<br>bayt - Toplam gönderilen baytlar |
| Ağ     | OkumaBytesPerSecond    | Ağ Saniyede Bayt Okuma             | BaytPerİkinci | networkDeviceId - Cihazın kimliği<br>bayt - Toplam alınan baytlar |
| İşlemci   | Kullanım Yüzdesi | İşlemci Kullanım Yüzdesi          | Yüzde        | totalCpus - Toplam CPU |
| Logicaldisk | Yazma PerSecond       | Mantıksal Disk Saniyede Yazar            | CountPerSecond | mountId - Cihazın Montaj Kimliği |
| Logicaldisk | WriteLatencyMs        | Mantıksal Disk Yazma Gecikme Milisaniye    | Milisaniye   | mountId - Cihazın Montaj Kimliği |
| Logicaldisk | YazmaBytesPerSecond   | Mantıksal Disk Saniyede Bayt Yazma       | BaytPerİkinci | mountId - Cihazın Montaj Kimliği |
| Logicaldisk | TransfersPerSecond    | Saniyede Mantıksal Disk Aktarımları         | CountPerSecond | mountId - Cihazın Montaj Kimliği |
| Logicaldisk | TransferLatencyMs     | Mantıksal Disk Aktarım Gecikme Milisaniye | Milisaniye   | mountId - Cihazın Montaj Kimliği |
| Logicaldisk | OkumaPersaniye        | Mantıksal Disk Saniyede Okur             | CountPerSecond | mountId - Cihazın Montaj Kimliği |
| Logicaldisk | ReadLatencyMs         | Mantıksal Disk Okuma Gecikme Milisaniye     | Milisaniye   | mountId - Cihazın Montaj Kimliği |
| Logicaldisk | OkumaBytesPerSecond    | Mantıksal Disk Saniyede Bayt Okuma        | BaytPerİkinci | mountId - Cihazın Montaj Kimliği |
| Logicaldisk | Boş Alan Yüzdesi   | Mantıksal Disk Boş Alan Yüzdesi        | Yüzde        | mountId - Cihazın Montaj Kimliği |
| Logicaldisk | Ücretsiz SpaceMB           | Mantıksal Disk Boş Alan Baytları             | Bayt          | mountId - Cihazın Montaj Kimliği<br>diskSizeMB - Toplam disk boyutu |
| Logicaldisk | BaytPerİkinci        | Saniyede Mantıksal Disk Baytları             | BaytPerİkinci | mountId - Cihazın Montaj Kimliği |


## <a name="next-steps"></a>Sonraki adımlar

* Azure Monitor'da günlük sorguları yazmada yeniyseniz, günlük sorguları yazmak için Azure portalında [Log Analytics'in nasıl kullanılacağını](../../azure-monitor/log-query/get-started-portal.md) gözden geçirin.

* Arama [sorguları yazma](../../azure-monitor/log-query/search-queries.md)hakkında bilgi edinin.
