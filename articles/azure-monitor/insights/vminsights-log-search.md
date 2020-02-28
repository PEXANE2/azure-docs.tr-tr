---
title: VM'ler için Azure İzleyici günlüklerini sorgulama (Önizleme) | Microsoft Docs
description: VM'ler için Azure İzleyici çözümü ölçümleri ve günlük verilerini toplar ve bu makalede kayıtları açıklanmakta ve örnek sorgular yer almaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 12/19/2019
ms.openlocfilehash: e679345669d0954008e46f48d986930038a84c10
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77670721"
---
# <a name="how-to-query-logs-from-azure-monitor-for-vms-preview"></a>VM'ler için Azure İzleyici günlüklerini sorgulama (Önizleme)

VM'ler için Azure İzleyici performans ve bağlantı ölçümlerini, bilgisayar ve işlem envanter verilerini ve sistem durumu bilgilerini toplayıp Azure Izleyici 'de Log Analytics çalışma alanına iletir.  Bu veriler Azure Izleyici 'de [sorgu](../../azure-monitor/log-query/log-query-overview.md) için kullanılabilir. Bu verileri, geçiş planlama, Kapasite Analizi, bulma ve isteğe bağlı performans sorunlarını gidermeyle ilgili senaryolara uygulayabilirsiniz.

## <a name="map-records"></a>Eşleme kayıtları

Her benzersiz bilgisayar ve işlem için saat başına bir kayıt oluşturulur; bir işlem veya bilgisayar başladığında oluşturulan kayıtlara ek olarak, VM'ler için Azure İzleyici eşleme özelliği de eklenmediyse. Bu kayıtlar aşağıdaki tablolardaki özelliklere sahiptir. ServiceMapComputer_CL olaylardaki alanlar ve değerler, ServiceMap Azure Resource Manager API 'sindeki makine kaynağının alanlarıyla eşlenir. ServiceMapProcess_CL olaylardaki alanlar ve değerler, ServiceMap Azure Resource Manager API 'sindeki Işlem kaynağının alanlarıyla eşlenir. ResourceName_s alanı, karşılık gelen Kaynak Yöneticisi kaynağındaki ad alanıyla eşleşir. 

Benzersiz işlem ve bilgisayarları tanımlamak için kullanabileceğiniz, dahili olarak oluşturulan özellikler vardır:

- Bilgisayar: bir Log Analytics çalışma alanındaki bilgisayarı benzersiz şekilde tanımlamak için *RESOURCEID* veya *ResourceName_s* kullanın.
- İşlem: Log Analytics çalışma alanındaki bir işlemi benzersiz şekilde tanımlamak için *RESOURCEID* kullanın. *ResourceName_s* , işlemin üzerinde çalıştığı makinenin bağlamı içinde benzersizdir (MachineResourceName_s) 

Belirli bir zaman aralığında belirtilen bir işlem ve bilgisayar için birden çok kayıt olabileceğinden, sorgular aynı bilgisayar veya işlem için birden fazla kayıt döndürebilir. Yalnızca en son kaydı dahil etmek için sorguya `| summarize arg_max(TimeGenerated, *) by ResourceId` ekleyin.

### <a name="connections-and-ports"></a>Bağlantılar ve bağlantı noktaları

Bağlantı ölçümleri özelliği, Azure Izleyici günlükleri-VMConnection ve VMBoundPort 'ta iki yeni tablo tanıtır. Bu tablolar bir makineye yönelik bağlantılar (gelen ve giden) ve bunlar üzerinde açık/etkin olan sunucu bağlantı noktaları hakkında bilgi sağlar. Connectionmetric Ayrıca bir zaman penceresi sırasında belirli bir ölçümü elde etmek için bir yol sağlayan API 'Ler aracılığıyla da sunulur. Bir dinleme yuvasında *kabul edilmeden* kaynaklanan TCP bağlantıları gelen, belırlı bir IP ve bağlantı noktasına *bağlanarak* oluşturulanlar gidendir. Bir bağlantının yönü, **gelen** veya **giden**olarak ayarlanabilen Direction özelliği ile temsil edilir. 

Bu tablolardaki kayıtlar, Dependency Agent tarafından bildirilen verilerden oluşturulur. Her kayıt, 1 dakikalık bir zaman aralığı üzerindeki bir gözlemi temsil eder. TimeGenerated özelliği zaman aralığının başlangıcını gösterir. Her kayıt, ilgili varlığın, yani bağlantının veya bağlantı noktasının yanı sıra söz konusu varlıkla ilişkili ölçümleri belirlemek için bilgiler içerir. Şu anda yalnızca IPv4 üzerinden TCP kullanılarak gerçekleşen ağ etkinliği raporlanır. 

#### <a name="common-fields-and-conventions"></a>Ortak alanlar ve kurallar 

Aşağıdaki alanlar ve kurallar hem VMConnection hem de VMBoundPort için geçerlidir: 

- Bilgisayar: Raporlama makinesinin tam etki alanı adı 
- Bilgisayar kimliği: Log Analytics aracısına sahip bir makine için benzersiz tanımlayıcı  
- Makine: ServiceMap tarafından sunulan makinenin Azure Resource Manager kaynak adı. Bu, *Guid* ,, It TID Ile aynı GUID olan *z-{GUID}* biçimindedir  
- İşlem: ServiceMap tarafından sunulan işlemin Azure Resource Manager kaynak adı. Bu, *p-{hex dize}* biçimindedir. İşlem bir makine kapsamı içinde benzersizdir ve makineler arasında benzersiz bir işlem KIMLIĞI oluşturmak, makine ve Işlem alanlarını birleştirmek. 
- ProcessName: raporlama işleminin yürütülebilir adı.
- Tüm IP adresleri, IPv4 kurallı biçimindeki dizelerdir, örneğin, *13.107.3.160* 

Maliyet ve karmaşıklığı yönetmek için, bağlantı kayıtları tek tek fiziksel ağ bağlantılarını temsil etmez. Birden çok fiziksel ağ bağlantısı, daha sonra ilgili tabloya yansıtılmış bir mantıksal bağlantı halinde gruplandırılır.  Yani, *Vmconnection* tablosundaki kayıtlar, gözlemlenen ayrı fiziksel bağlantıları değil, mantıksal gruplandırmayı temsil eder. Belirli bir dakikalık Aralık sırasında aşağıdaki öznitelikler için aynı değeri paylaşan fiziksel ağ bağlantısı, *Vmconnection*'daki tek bir mantıksal kayıtta toplanır. 

| Özellik | Açıklama |
|:--|:--|
|Yön |Bağlantının yönü, değer *gelen* veya *giden* |
|Makine |Bilgisayar FQDN 'SI |
|İşleme |İşlemin veya işlem gruplarının kimliği, bağlantıyı başlatma/kabul etme |
|SourceIp |Kaynağın IP adresi |
|Hedef IP |Hedefin IP adresi |
|DestinationPort |Hedefin bağlantı noktası numarası |
|Protokol |Bağlantı için kullanılan protokol.  Değerler *TCP*'dir. |

Gruplandırmanın etkisini hesaba eklemek için, kaydın aşağıdaki özelliklerinde gruplanmış fiziksel bağlantı sayısıyla ilgili bilgiler verilmiştir:

| Özellik | Açıklama |
|:--|:--|
|LinksEstablished |Raporlama süresi penceresi sırasında kurulan fiziksel ağ bağlantısı sayısı |
|Bağlantı stermi |Raporlama süresi penceresi sırasında sonlandırılan fiziksel ağ bağlantısı sayısı |
|LinksFailed |Raporlama zaman penceresi sırasında başarısız olan fiziksel ağ bağlantısı sayısı. Bu bilgiler şu anda yalnızca giden bağlantılar için kullanılabilir. |
|LinksLive |Raporlama zamanı penceresinin sonunda açık olan fiziksel ağ bağlantısı sayısı|

#### <a name="metrics"></a>Ölçümler

Bağlantı sayısı ölçümlerine ek olarak, belirli bir mantıksal bağlantı veya ağ bağlantı noktası üzerinde gönderilen ve alınan veri hacmi hakkındaki bilgiler, kaydın aşağıdaki özelliklerine de dahildir:

| Özellik | Açıklama |
|:--|:--|
|BytesSent |Raporlama süresi penceresi sırasında gönderilen toplam bayt sayısı |
|BytesReceived |Raporlama süresi penceresi sırasında alınan toplam bayt sayısı |
|Yanıtlar |Raporlama zaman penceresi sırasında gözlemlenen yanıt sayısı. 
|ResponseTimeMax |Raporlama zaman penceresi sırasında gözlenen en büyük yanıt süresi (milisaniye). Değer yoksa, özellik boştur.|
|ResponseTimeMin |Raporlama zamanı penceresi sırasında gözlenen en küçük yanıt süresi (milisaniye). Değer yoksa, özellik boştur.|
|ResponseTimeSum |Raporlama zamanı penceresi sırasında gözlenen tüm yanıt sürelerinin (milisaniye) toplamı. Değer yoksa, özellik boştur.|

Bildirilen üçüncü veri türü yanıt süresi-bir çağıran bir bağlantı üzerinden gönderilen bir isteğin, uzak uç nokta tarafından işlenmek ve yanıt vermek için ne kadar süre harcadığını bekliyor. Raporlanan yanıt süresi, temel uygulama protokolünün doğru yanıt süresinin bir tahmindir. Fiziksel bir ağ bağlantısının kaynak ve hedef sonu arasındaki veri akışının gözlemine bağlı olarak buluşsal yöntemler kullanılarak hesaplanır. Kavramsal olarak, bir isteğin son baytındaki zaman arasındaki fark, yanıtın son baytının geri ulaştığı zaman arasındaki farktır. Bu iki zaman damgası, belirli bir fiziksel bağlantıda istek ve yanıt olaylarını belirtmek için kullanılır. Aralarındaki fark, tek bir isteğin yanıt süresini temsil eder. 

Bu özelliğin ilk sürümünde, algoritmız, belirli bir ağ bağlantısı için kullanılan gerçek uygulama protokolüne bağlı olarak değişen başarı derecesine sahip olabilecek bir yaklaşık dır. Örneğin, geçerli yaklaşım HTTP (S) gibi istek yanıt tabanlı protokoller için iyi çalışır, ancak tek yönlü veya ileti kuyruğu tabanlı protokollerle çalışmaz.

Göz önünde bulundurmanız gereken bazı önemli noktaları aşağıda bulabilirsiniz:

1. Bir işlem aynı IP adresinde ancak birden çok ağ arabiriminden bağlantı kabul ederse, her arabirim için ayrı bir kayıt raporlanır. 
2. Joker IP içeren kayıtlar hiçbir etkinlik içermez. Bunlar, makinedeki bir bağlantı noktasının gelen trafiğe açık olduğunu göstermek için dahil edilmiştir.
3. Ayrıntı ve veri hacmini azaltmak için, belirli bir IP adresi ile eşleşen bir kayıt (aynı işlem, bağlantı noktası ve protokol) olduğunda joker IP 'si olan kayıtlar atlanır. Joker karakter bir IP kaydı atlandığında, belirli IP adresine sahip ıyaya Cardbind kaydı özelliği, bağlantı noktasının raporlama makinesinin her arabiriminden açığa çıkardığını göstermek için "true" olarak ayarlanır.
4. Yalnızca belirli bir arabirime bağlı olan bağlantı noktalarında ıyalar Cardbind *değeri false*olarak ayarlanmıştır.

#### <a name="naming-and-classification"></a>Adlandırma ve sınıflandırma

Kolaylık olması için, bir bağlantının uzak ucunun IP adresi Remoteıp özelliğinde yer alır. Gelen bağlantılarda, Remoteıp, SourceIP ile aynıdır, ancak giden bağlantılar için de Destinationıp ile aynıdır. RemoteDnsCanonicalNames özelliği, Remoteıp için makine tarafından raporlanan DNS kurallı adlarını temsil eder. Remotednssorular ve RemoteClassification özellikleri ileride kullanılmak üzere ayrılmıştır. 

#### <a name="geolocation"></a>Konumu

*Vmconnection* Ayrıca, kaydın aşağıdaki özelliklerindeki her bir bağlantı kaydının uzak ucuna ait coğrafi konum bilgilerini de içerir: 

| Özellik | Açıklama |
|:--|:--|
|RemoteCountry |Remoteıp 'yi barındıran ülkenin/bölgenin adı.  Örneğin, *Birleşik Devletler* |
|Remoteenlem |Coğrafi konum enlem. Örneğin, *47,68* |
|Remoteboylam |Coğrafi konum boylam. Örneğin, *-122,12* |

#### <a name="malicious-ip"></a>Kötü amaçlı IP

*Vmconnection* tablosundaki her remoteıp özelliği, bilinen kötü amaçlı etkinliklerle bir IP kümesine karşı denetlenir. Remoteıp kötü amaçlı olarak tanımlanmışsa, kaydın aşağıdaki özelliklerinde aşağıdaki özellikler doldurulur (boş olan, IP kötü amaçlı olarak kabul edilmez).

| Özellik | Açıklama |
|:--|:--|
|MaliciousIp |Remoteıp adresi |
|Indicatorthreadtype |Algılanan tehdit göstergesi, *botnet*, *C2*, *cryptoaraştırma*, *koyu ağ*, *DDoS*, *MaliciousUrl*, *kötü amaçlı yazılım*, *kimlik avı*, *proxy*, *Pua*, *listem*değerlerinden biridir.   |
|Açıklama |Gözlemlenen tehdit açıklaması. |
|TLPLevel |Trafik ışığı Protokolü (TLP) düzeyi, tanımlı değerlerden biridir, *beyaz*, *yeşil* *,,* ve *kırmızı*. |
|Güven |Değerler *0 – 100*' dir. |
|Severity |Değerler *0 – 5*' dir; burada *5* en önemdir ve *0* , hiç önemli değildir. Varsayılan değer *3*' dir.  |
|FirstReportedDateTime |Sağlayıcı göstergeyi ilk kez raporladı. |
|LastReportedDateTime |Göstergenin Interflow tarafından en son görüldüğü zaman. |
|IsActive |Göstergelerin *true* veya *false* değeriyle devre dışı bırakıldığını gösterir. |
|ReportReferenceLink |Verilen bir observable ile ilgili raporların bağlantıları. |
|AdditionalInformation |, Gözlemlenen tehdit hakkında, varsa ek bilgiler sağlar. |

### <a name="ports"></a>Bağlantı Noktaları 

Gelen trafiği etkin bir şekilde kabul eden veya trafiği kabul edebilen, ancak raporlama zaman penceresi sırasında boşta olan bir makinedeki bağlantı noktaları VMBoundPort tablosuna yazılır.  

VMBoundPort içindeki her kayıt aşağıdaki alanlar tarafından tanımlanır: 

| Özellik | Açıklama |
|:--|:--|
|İşleme | Bağlantı noktasının ilişkilendirildiği işlemin kimliği (veya işlem grupları).|
|Ip | Bağlantı noktası IP adresi (joker IP, *0.0.0.0*olabilir) |
|Bağlantı noktası |Bağlantı noktası numarası |
|Protokol | Protokol.  Örnek, *TCP* veya *UDP* (yalnızca *TCP* Şu anda desteklenmektedir).|
 
Bir bağlantı noktasının kimliği, yukarıdaki beş alandan türetilir ve PortID özelliğinde saklanır. Bu özellik, belirli bir bağlantı noktası için zaman içinde kayıtları hızlı bir şekilde bulmak için kullanılabilir. 

#### <a name="metrics"></a>Ölçümler 

Bağlantı noktası kayıtları, bunlarla ilişkili bağlantıları temsil eden ölçümleri içerir. Şu anda, aşağıdaki ölçümler raporlanır (her ölçüm için Ayrıntılar önceki bölümde açıklanmıştır): 

- BytesSent ve BytesReceived 
- LinksEstablished, Linksterayırt edici, LinksLive 
- ResposeTime, ResponseTimeMin, ResponseTimeMax, ResponseTimeSum 

Göz önünde bulundurmanız gereken bazı önemli noktaları aşağıda bulabilirsiniz:

- Bir işlem aynı IP adresinde ancak birden çok ağ arabiriminden bağlantı kabul ederse, her arabirim için ayrı bir kayıt raporlanır.  
- Joker IP içeren kayıtlar hiçbir etkinlik içermez. Bunlar, makinedeki bir bağlantı noktasının gelen trafiğe açık olduğunu göstermek için dahil edilmiştir. 
- Ayrıntı ve veri hacmini azaltmak için, belirli bir IP adresi ile eşleşen bir kayıt (aynı işlem, bağlantı noktası ve protokol) olduğunda joker IP 'si olan kayıtlar atlanır. Joker karakter IP kaydı atlandığında, belirli IP adresine sahip kayıt için *ıyaya Cardbind* özelliği *true*olarak ayarlanır.  Bu, bağlantı noktasının raporlama makinesinin her arabiriminden açığa çıkardığını gösterir. 
- Yalnızca belirli bir arabirime bağlı olan bağlantı noktalarında ıyalar Cardbind *değeri false*olarak ayarlanmıştır. 

### <a name="vmcomputer-records"></a>VMComputer kayıtları

Bir *Vmcomputer* türü olan kayıtlar, bağımlılık aracısına sahip sunucular için envanter verileri vardır. Bu kayıtlar aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Açıklama |
|:--|:--|
|TenantId | Çalışma alanı için benzersiz tanımlayıcı |
|SourceSystem | *İçgörüler* | 
|TimeGenerated | Kaydın zaman damgası (UTC) |
|Bilgisayar | Bilgisayar FQDN 'SI | 
|AgentId | Log Analytics aracısının benzersiz KIMLIĞI |
|Makine | ServiceMap tarafından sunulan makinenin Azure Resource Manager kaynağının adı. Bu, *Guid* ,, It TID Ile aynı GUID olan *z-{GUID}* biçimindedir. | 
|DisplayName | Görünen ad | 
|FullDisplayName | Tam ekran adı | 
|ana bilgisayar adı | Etki alanı adı olmayan makinenin adı |
|BootTime | Makinenin önyükleme saati (UTC) |
|TimeZone | Normalleştirilmiş Saat dilimi |
|VirtualizationState | *sanal*, *hiper yönetici*, *fiziksel* |
|Ipv4Addresses | IPv4 adresleri dizisi | 
|Ipv4SubnetMasks | IPv4 alt ağ maskelerinden oluşan dizi (Ipv4Addresses ile aynı sırada). |
|Ipv4DefaultGateways | IPv4 ağ geçitleri dizisi | 
|Ipv6Addresses | IPv6 adresleri dizisi | 
|MacAddresses | MAC adresleri dizisi | 
|DnsNames | Makineyle ilişkili DNS adları dizisi. |
|DependencyAgentVersion | Makinede çalışan bağımlılık aracısının sürümü. | 
|OperatingSystemFamily | *Linux*, *Windows* |
|OperatingSystemFullName | İşletim sisteminin tam adı | 
|PhysicalMemoryMB | Megabayt cinsinden fiziksel bellek | 
|'Lar | İşlemci sayısı | 
|CpuSpeed | MHz cinsinden CPU hızı | 
|VirtualMachineType | *hyperv*, *VMware*, *Xen* |
|VirtualMachineNativeId | Hiper Yöneticisi tarafından atanan VM KIMLIĞI | 
|VirtualMachineNativeName | VM 'nin adı |
|Virtualmachinehypervisorıd | VM 'yi barındıran hiper yöneticinin benzersiz tanımlayıcısı |
|Hipervizör türüne | *hyperv* |
|Hipervizorıd | Hiper yöneticinin benzersiz KIMLIĞI | 
|HostingProvider | *mavisi* |
|_ResourceId | Bir Azure kaynağı için benzersiz tanımlayıcı |
|Azuresubscriptionıd | Aboneliğinizi tanımlayan bir genel benzersiz tanımlayıcı | 
|AzureResourceGroup | Makinenin üyesi olduğu Azure Kaynak grubunun adı. |
|AzureResourceName | Azure kaynağının adı |
|AzureLocation | Azure kaynağının konumu |
|AzureUpdateDomain | Azure Update etki alanının adı |
|AzureFaultDomain | Azure hata etki alanının adı |
|Azurevmıd | Azure sanal makinesinin benzersiz tanımlayıcısı |
|AzureSize | Azure VM 'nin boyutu |
|AzureImagePublisher | Azure VM yayımcısının adı |
|AzureImageOffering | Azure VM teklif türünün adı | 
|AzureImageSku | Azure VM görüntüsünün SKU 'SU | 
|AzureImageVersion | Azure VM görüntüsünün sürümü | 
|Azurecses ServiceName | Azure bulut hizmeti 'nin adı |
|Azurecses hizmeti dağıtımı | Bulut hizmeti için dağıtım KIMLIĞI |
|Azurechoparlör Servicerolename | Bulut hizmeti rol adı |
|Azurecses Serviceroletype | Bulut hizmeti rol türü: *çalışan* veya *Web* |
|Azurecses hizmeti InstanceId | Bulut hizmeti rol örneği KIMLIĞI |
|AzureVmScaleSetName | Sanal makine ölçek kümesinin adı |
|AzureVmScaleSetDeployment | VM Ölçek kümesi dağıtım KIMLIĞI |
|AzureVmScaleSetResourceId | Sanal makine ölçek kümesi kaynağının benzersiz tanımlayıcısı.|
|AzureVmScaleSetInstanceId | Sanal makine ölçek kümesinin benzersiz tanımlayıcısı |
|AzureServiceFabricClusterId | Azure Service Fabric kümesinin benzersiz tanıtıcısı | 
|AzureServiceFabricClusterName | Azure Service Fabric kümesinin adı |

### <a name="vmprocess-record"></a>VMProcess kaydı

Bir *Vmprocess* türüne sahip kayıtlar, bağımlılık aracısına sahıp sunuculardaki TCP bağlantılı işlemler için envanter verileri vardır. Bu kayıtlar aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Açıklama |
|:--|:--|
|TenantId | Çalışma alanı için benzersiz tanımlayıcı |
|SourceSystem | *İçgörüler* | 
|TimeGenerated | Kaydın zaman damgası (UTC) |
|Bilgisayar | Bilgisayar FQDN 'SI | 
|AgentId | Log Analytics aracısının benzersiz KIMLIĞI |
|Makine | ServiceMap tarafından sunulan makinenin Azure Resource Manager kaynağının adı. Bu, *Guid* ,, It TID Ile aynı GUID olan *z-{GUID}* biçimindedir. | 
|İşleme | Hizmet Eşlemesi işleminin benzersiz tanımlayıcısı. *P-{GUID}* biçimindedir. 
|ExecutableName | İşlemin yürütülebilir dosyasının adı | 
|DisplayName | İşlem görünen adı |
|Rol | İşlem rolü: *WebSunucusu*, *appserver*, *DatabaseServer*, *ldapserver*, *SMBSERVER* |
|Grup | İşlem grubu adı. Aynı gruptaki süreçler mantıksal olarak ilişkilidir, ör. aynı ürün veya sistem bileşeninin bir parçasıdır. |
|StartTime | İşlem havuzu başlangıç zamanı |
|FirstPid | İşlem havuzundaki ilk PID |
|Açıklama | İşlem açıklaması |
|CompanyName | Şirketin adı |
|InternalName | İç ad |
|ProductName | Ürünün adı |
|ProductVersion | Ürünün sürümü |
|FileVersion | Dosyanın sürümü |
|ExecutablePath |Yürütülebilir dosyanın yolu |
|Komut satırı | Komut satırı |
|Başlangıç | Çalışma dizini |
|Hizmetler | İşlemin üzerinde yürütüldüğü hizmet dizisi |
|UserName | İşlemin üzerinde yürütüldüğü hesap |
|UserDomain | İşlemin üzerinde yürütüldüğü etki alanı |
|_ResourceId | Çalışma alanı içindeki bir işlemin benzersiz tanımlayıcısı |

## <a name="sample-log-searches"></a>Örnek günlük aramaları

### <a name="list-all-known-machines"></a>Tüm bilinen makineleri Listele

```kusto
VMComputer | summarize arg_max(TimeGenerated, *) by _ResourceId
```

### <a name="when-was-the-vm-last-rebooted"></a>VM son kez yeniden başlatıldı

```kusto
let Today = now(); VMComputer | extend DaysSinceBoot = Today - BootTime | summarize by Computer, DaysSinceBoot, BootTime | sort by BootTime asc
```

### <a name="summary-of-azure-vms-by-image-location-and-sku"></a>Görüntü, konum ve SKU 'ya göre Azure VM 'lerinin Özeti

```kusto
VMComputer | where AzureLocation != "" | summarize by ComputerName, AzureImageOffering, AzureLocation, AzureImageSku
```

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Tüm yönetilen bilgisayarların fiziksel bellek kapasitesini listeleyin

```kusto
VMComputer | summarize arg_max(TimeGenerated, *) by _ResourceId | project PhysicalMemoryMB, Computer
```

### <a name="list-computer-name-dns-ip-and-os"></a>Bilgisayar adını, DNS, IP ve işletim sistemini Listele

```kusto
VMComputer | summarize arg_max(TimeGenerated, *) by _ResourceId | project Computer, OperatingSystemFullName, DnsNames, Ipv4Addresses
```

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Komut satırında "SQL" ile tüm süreçler bul

```kusto
VMComputer | where CommandLine contains_cs "sql" | summarize arg_max(TimeGenerated, *) by _ResourceId
```

### <a name="find-a-machine-most-recent-record-by-resource-name"></a>Kaynak adına göre bir makine (en son kayıt) bulun

```kusto
search in (VMComputer) "m-4b9c93f9-bc37-46df-b43c-899ba829e07b" | summarize arg_max(TimeGenerated, *) by _ResourceId
```

### <a name="find-a-machine-most-recent-record-by-ip-address"></a>IP adresine göre bir makine (en son kayıt) bulun

```kusto
search in (VMComputer) "10.229.243.232" | summarize arg_max(TimeGenerated, *) by _ResourceId
```

### <a name="list-all-known-processes-on-a-specified-machine"></a>Belirtilen makinedeki tüm bilinen işlemlerin listesini Listele

```kusto
VMProcess | where Machine == "m-559dbcd8-3130-454d-8d1d-f624e57961bc" | summarize arg_max(TimeGenerated, *) by _ResourceId
```

### <a name="list-all-computers-running-sql-server"></a>SQL Server çalıştıran tüm bilgisayarları listeleme

```kusto
VMComputer | where AzureResourceName in ((search in (VMProcess) "\*sql\*" | distinct Machine)) | distinct Computer
```

### <a name="list-all-unique-product-versions-of-curl-in-my-datacenter"></a>Veri merkezindeki tüm benzersiz ürün sürümlerini listeleyin

```kusto
VMProcess | where ExecutableName == "curl" | distinct ProductVersion
```

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>CentOS çalıştıran tüm bilgisayarların bilgisayar grubunu oluşturma

```kusto
VMComputer | where OperatingSystemFullName contains_cs "CentOS" | distinct ComputerName
```

### <a name="bytes-sent-and-received-trends"></a>Gönderilen ve alınan bayt eğilimleri

```kusto
VMConnection | summarize sum(BytesSent), sum(BytesReceived) by bin(TimeGenerated,1hr), Computer | order by Computer desc | render timechart
```

### <a name="which-azure-vms-are-transmitting-the-most-bytes"></a>En fazla bayt ileten Azure VM 'Leri

```kusto
VMConnection | join kind=fullouter(VMComputer) on $left.Computer == $right.Computer | summarize count(BytesSent) by Computer, AzureVMSize | sort by count_BytesSent desc
```

### <a name="link-status-trends"></a>Bağlantı durumu eğilimleri

```kusto
VMConnection | where TimeGenerated >= ago(24hr) | where Computer == "acme-demo" | summarize dcount(LinksEstablished), dcount(LinksLive), dcount(LinksFailed), dcount(LinksTerminated) by bin(TimeGenerated, 1h) | render timechart
```

### <a name="connection-failures-trend"></a>Bağlantı hatalarının eğilimi

```kusto
VMConnection | where Computer == "acme-demo" | extend bythehour = datetime_part("hour", TimeGenerated) | project bythehour, LinksFailed | summarize failCount = count() by bythehour | sort by bythehour asc | render timechart
```

### <a name="bound-ports"></a>Bağlantılı bağlantı noktaları

```kusto
VMBoundPort
| where TimeGenerated >= ago(24hr)
| where Computer == 'admdemo-appsvr'
| distinct Port, ProcessName
```

### <a name="number-of-open-ports-across-machines"></a>Makinelerde açık bağlantı noktası sayısı

```kusto
VMBoundPort
| where Ip != "127.0.0.1"
| summarize by Computer, Machine, Port, Protocol
| summarize OpenPorts=count() by Computer, Machine
| order by OpenPorts desc
```

### <a name="score-processes-in-your-workspace-by-the-number-of-ports-they-have-open"></a>Çalışma alanınızdaki işlem, açık olan bağlantı noktası sayısıyla puan

```kusto
VMBoundPort
| where Ip != "127.0.0.1"
| summarize by ProcessName, Port, Protocol
| summarize OpenPorts=count() by ProcessName
| order by OpenPorts desc
```

### <a name="aggregate-behavior-for-each-port"></a>Her bağlantı noktası için Birleşik davranış

Bu sorgu daha sonra etkinliğe göre bağlantı noktalarına puan vermek için kullanılabilir, örneğin, en çok gelen/giden trafiğe sahip bağlantı noktaları, en çok bağlantı noktası bağlantı noktaları
```kusto
// 
VMBoundPort
| where Ip != "127.0.0.1"
| summarize BytesSent=sum(BytesSent), BytesReceived=sum(BytesReceived), LinksEstablished=sum(LinksEstablished), LinksTerminated=sum(LinksTerminated), arg_max(TimeGenerated, LinksLive) by Machine, Computer, ProcessName, Ip, Port, IsWildcardBind
| project-away TimeGenerated
| order by Machine, Computer, Port, Ip, ProcessName
```

### <a name="summarize-the-outbound-connections-from-a-group-of-machines"></a>Bir makine grubundan giden bağlantıları özetleme

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

## <a name="next-steps"></a>Sonraki adımlar

* Azure Izleyici 'de günlük sorguları yazmaya yeni çalışıyorsanız, günlük sorgularını yazmak için Azure portal [Log Analytics nasıl kullanacağınızı](../../azure-monitor/log-query/get-started-portal.md) inceleyin.

* [Arama sorguları yazma](../../azure-monitor/log-query/search-queries.md)hakkında bilgi edinin.
