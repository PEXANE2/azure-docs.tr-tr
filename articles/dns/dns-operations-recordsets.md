---
title: Azure PowerShell'i kullanarak Azure DNS'deki DNS kayıtlarını yönetme | Microsoft Dokümanlar
description: Etki alanınızı Azure DNS'de barındırırken Azure DNS'de DNS kayıt kümelerini ve kayıtlarını yönetme. Kayıt kümeleri ve kayıtlardaki işlemler için tüm PowerShell komutları.
services: dns
documentationcenter: na
author: rohinkoul
manager: timlt
ms.assetid: 7136a373-0682-471c-9c28-9e00d2add9c2
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: rohink
ms.openlocfilehash: b9244d9b2bdc9cb20195bbc103c0b1eb48a9de63
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76932539"
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-azure-powershell"></a>Azure PowerShell'i kullanarak Azure DNS'deki DNS kayıtlarını ve kayıt kümelerini yönetme

> [!div class="op_single_selector"]
> * [Azure Portalı](dns-operations-recordsets-portal.md)
> * [Azure klasik CLI](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI](dns-operations-recordsets-cli.md)
> * [Powershell](dns-operations-recordsets.md)

Bu makalede, Azure PowerShell kullanarak DNS bölgeniz için DNS kayıtlarını nasıl yönetisiniz gösterilmektedir. DNS kayıtları, platformlar arası Azure [CLI](dns-operations-recordsets-cli.md) veya [Azure portalı](dns-operations-recordsets-portal.md)kullanılarak da yönetilebilir.

Bu makaledeki [örnekler, Azure PowerShell'i zaten yüklediğinizi, oturum açtettiğinizi ve bir DNS bölgesi oluşturduğunuzu](dns-operations-dnszones.md)varsayar.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="introduction"></a>Giriş

Azure DNS’de DNS kayıtlarını oluşturmadan önce Azure DNS’nin DNS kayıtlarını DNS kayıt kümeleri şeklinde nasıl düzenlediğini kavramanız gerekir.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Azure DNS’deki DNS kayıtları hakkında daha fazla bilgi için bkz. [DNS bölgeleri ve kayıtları](dns-zones-records.md).


## <a name="create-a-new-dns-record"></a>Yeni bir DNS kaydı oluşturma

Yeni kaydınız varolan bir kayıtla aynı ada ve türe [sahipse, varolan kayıt kümesine eklemeniz](#add-a-record-to-an-existing-record-set)gerekir. Yeni kaydınızın farklı bir adı ve varolan tüm kayıtlara türü varsa, yeni bir kayıt kümesi oluşturmanız gerekir. 

### <a name="create-a-records-in-a-new-record-set"></a>Yeni bir kayıt kümesinde 'A' kayıtları oluşturma

`New-AzDnsRecordSet` cmdlet’ini kullanarak kayıt kümeleri oluşturabilirsiniz. Bir kayıt kümesi oluştururken, kayıt kümesi adını, bölgeyi, yaşama süresini (TTL), kayıt türünü ve oluşturulacak kayıtları belirtmeniz gerekir.

Bir kayıt kümesine kayıt eklemeye yönelik parametreler, kayıt kümesinin türüne bağlı olarak farklılık gösterir. Örneğin, 'A' türünden bir kayıt kümesi kullanırken, parametreyi `-IPv4Address`kullanarak IP adresini belirtmeniz gerekir. Diğer kayıt türleri için diğer parametreler kullanılır. Ayrıntılar için ek kayıt türü örneklerine bakın.

Aşağıdaki örnek, DNS Bölgesi 'contoso.com' içinde 'www' göreli adı ile ayarlanmış bir kayıt oluşturur. Kayıt kümesinin tam nitelikli adı 'www.contoso.com'dır. Kayıt türü 'A'dır ve TTL 3600 saniyedir. Kayıt kümesi, IP adresi '1.2.3.4' olan tek bir kayıt içerir.

```powershell
New-AzDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address "1.2.3.4") 
```

Bir bölgenin 'tepe noktasında' ayarlanmış bir kayıt oluşturmak için (bu durumda, 'contoso.com'), '' (tırnak işaretleri hariç) kayıt kümesi adını\@kullanın:

```powershell
New-AzDnsRecordSet -Name "@" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address "1.2.3.4") 
```

Birden fazla kayıt içeren bir kayıt kümesi oluşturmanız gerekiyorsa, önce yerel bir dizi oluşturun `New-AzDnsRecordSet` ve kayıtları ekleyin, ardından diziyi aşağıdaki gibi geçirin:

```powershell
$aRecords = @()
$aRecords += New-AzDnsRecordConfig -IPv4Address "1.2.3.4"
$aRecords += New-AzDnsRecordConfig -IPv4Address "2.3.4.5"
New-AzDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName MyResourceGroup -Ttl 3600 -RecordType A -DnsRecords $aRecords
```

[Kayıt kümesi meta verileri,](dns-zones-records.md#tags-and-metadata) uygulamaya özgü verileri anahtar değer çiftleri olarak her kayıt kümesiyle ilişkilendirmek için kullanılabilir. Aşağıdaki örnek, 'dept=finance' ve 'environment=production' olmak üzere iki meta veri girişi içeren bir kayıt kümesinin nasıl oluşturulurulur gösterilmektedir.

```powershell
New-AzDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address "1.2.3.4") -Metadata @{ dept="finance"; environment="production" } 
```

Azure DNS, DNS kayıtları oluşturmadan önce bir DNS adını ayırmak için yer tutucu olarak hareket edebilen 'boş' kayıt kümelerini de destekler. Boş kayıt kümeleri Azure DNS denetim düzleminde görünür, ancak Azure DNS ad sunucularında görünür. Aşağıdaki örnek boş bir kayıt kümesi oluşturur:

```powershell
New-AzDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords @()
```

## <a name="create-records-of-other-types"></a>Diğer türlerin kayıtlarını oluşturma

'A' kayıtlarının nasıl oluşturulacaklarını ayrıntılı olarak gördükten sonra, aşağıdaki örnekler Azure DNS tarafından desteklenen diğer kayıt türlerinin kayıtlarının nasıl oluşturulacak olduğunu gösterir.

Her durumda, tek bir kayıt içeren bir kayıt kümesinin nasıl oluşturulabileceğimizi gösteririz. 'A' kayıtları için önceki örnekler, meta verilerle birden çok kayıt içeren diğer türlerin kayıt kümelerini oluşturmak veya boş kayıt kümeleri oluşturmak için uyarlanabilir.

SOA'lar her DNS bölgesiyle oluşturulup silindiğinden ve ayrı olarak oluşturulamadığından veya silinemediğinden, SOA kayıt kümesi oluşturmak için bir örnek vermeyiz. Ancak, [SOA daha sonraki bir örnekte gösterildiği gibi değiştirilebilir.](#to-modify-an-soa-record)

### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Tek kayıtlı bir AAAA kayıt kümesi oluşturma

```powershell
New-AzDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Ipv6Address "2607:f8b0:4009:1803::1005") 
```

### <a name="create-a-caa-record-set-with-a-single-record"></a>Tek bir kayıtla ayarlanmış bir CAA kaydı oluşturma

```powershell
New-AzDnsRecordSet -Name "test-caa" -RecordType CAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Caaflags 0 -CaaTag "issue" -CaaValue "ca1.contoso.com") 
```

### <a name="create-a-cname-record-set-with-a-single-record"></a>Tek kayıtlı bir CNAME kayıt kümesi oluşturma

> [!NOTE]
> DNS standartları, bir bölgenin tepesindeki CNAME kayıtlarına`-Name '@'`izin vermez ( ), ne de birden fazla kayıt içeren kayıt kümelerine izin vermez.
> 
> Daha fazla bilgi için [CNAME kayıtlarına](dns-zones-records.md#cname-records)bakın.


```powershell
New-AzDnsRecordSet -Name "test-cname" -RecordType CNAME -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Cname "www.contoso.com") 
```

### <a name="create-an-mx-record-set-with-a-single-record"></a>Tek kayıtlı bir MX kayıt kümesi oluşturma

Bu örnekte, bölge tepe noktasında\@bir MX kaydı oluşturmak için '' ' olarak kayıt kümesi adını kullanırız (bu durumda, 'contoso.com').


```powershell
New-AzDnsRecordSet -Name "@" -RecordType MX -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Exchange "mail.contoso.com" -Preference 5) 
```

### <a name="create-an-ns-record-set-with-a-single-record"></a>Tek kayıtlı bir NS kayıt kümesi oluşturma

```powershell
New-AzDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Nsdname "ns1.contoso.com") 
```

### <a name="create-a-ptr-record-set-with-a-single-record"></a>Tek kayıtlı bir PTR kaydı kümesi oluşturma

Bu durumda, 'my-arpa-zone.com' IP aralığınızı temsil eden ARPA ters arama bölgesini temsil eder. Bu bölgedeki her PTR kaydı bu IP aralığındaki bir IP adresine karşılık gelir. '10' kayıt adı, bu kayıtla temsil edilen bu IP aralığındaki IP adresinin son sekizlisidir.

```powershell
New-AzDnsRecordSet -Name 10 -RecordType PTR -ZoneName "my-arpa-zone.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Ptrdname "myservice.contoso.com") 
```

### <a name="create-an-srv-record-set-with-a-single-record"></a>Tek kayıtlı bir SRV kayıt kümesi oluşturma

[Bir SRV kayıt kümesi](dns-zones-records.md#srv-records)oluştururken, kayıt kümesi adında * \_hizmet* ve * \_protokolü* belirtin. Bölge tepe noktasında ayarlanan bir SRV kaydı oluştururken ' ' ' '\@i kayıt kümesi adına eklemeye gerek yoktur.

```powershell
New-AzDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Priority 0 -Weight 5 -Port 8080 -Target "sip.contoso.com") 
```


### <a name="create-a-txt-record-set-with-a-single-record"></a>Tek bir kayıtla ayarlanmış bir TXT kaydı oluşturma

Aşağıdaki örnekte, TXT kaydının nasıl oluşturuluşu gösterilmektedir. TXT kayıtlarında desteklenen maksimum dize uzunluğu hakkında daha fazla bilgi için Bkz. [TXT kayıtları.](dns-zones-records.md#txt-records)

```powershell
New-AzDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Value "This is a TXT record") 
```


## <a name="get-a-record-set"></a>Kayıt seti alma

Varolan bir kayıt kümesini almak için `Get-AzDnsRecordSet`. Bu cmdlet, Azure DNS'de ayarlanan kaydı temsil eden yerel bir nesneyi döndürür.

Olduğu `New-AzDnsRecordSet`gibi , verilen kayıt kümesi adı *göreli* bir ad olmalıdır, yani bölge adını dışlamalıdır. Ayrıca kayıt türünü ve kayıt kümesini içeren bölgeyi belirtmeniz gerekir.

Aşağıdaki örnekte, bir kayıt kümesinin nasıl alınılsüreceğini gösterilmektedir. Bu örnekte, bölge `-ZoneName` ve `-ResourceGroupName` parametreler kullanılarak belirtilir.

```powershell
$rs = Get-AzDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Alternatif olarak, parametre kullanılarak geçirilen bir bölge nesnesi `-Zone` kullanarak bölge belirtebilirsiniz.

```powershell
$zone = Get-AzDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs = Get-AzDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

## <a name="list-record-sets"></a>Liste kayıt kümeleri

Ayrıca, `Get-AzDnsZone` `-Name` ve/veya parametreleri atlayarak, bir bölgede kayıt `-RecordType` kümelerini listelemek için de kullanabilirsiniz.

Aşağıdaki örnek, bölgedeki tüm kayıt kümelerini döndürür:

```powershell
$recordsets = Get-AzDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Aşağıdaki örnek, kayıt kümesi adını atlarken kayıt türünü belirterek belirli bir türün tüm kayıt kümelerinin nasıl alınabileceğini gösterir:

```powershell
$recordsets = Get-AzDnsRecordSet -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Belirli bir ada sahip tüm kayıt kümelerini almak için, kayıt türleri arasında, tüm kayıt kümelerini almanız ve ardından sonuçları filtrelemeniz gerekir:

```powershell
$recordsets = Get-AzDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | where {$_.Name.Equals("www")}
```

Yukarıdaki örneklerin tümlerinde, bölge veya `-ZoneName` `-ResourceGroupName`parametreleri kullanarak (gösterildiği gibi) veya bir bölge nesnesi belirterek belirtilebilir:

```powershell
$zone = Get-AzDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$recordsets = Get-AzDnsRecordSet -Zone $zone
```

## <a name="add-a-record-to-an-existing-record-set"></a>Varolan bir kayıt kümesine kayıt ekleme

Varolan bir kayıt kümesine kayıt eklemek için aşağıdaki üç adımı izleyin:

1. Varolan kayıt kümesini alma

    ```powershell
    $rs = Get-AzDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. Yeni kaydı yerel kayıt kümesine ekleyin. Bu çevrimdışı bir operasyon.

    ```powershell
    Add-AzDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. Değişikliği Azure DNS hizmetine geri taşıyın. 

    ```powershell
    Set-AzDnsRecordSet -RecordSet $rs
    ```

Kullanma, `Set-AzDnsRecordSet` Azure DNS'de (ve içerdiği tüm kayıtların) varolan kayıt kümesinin yerine belirtilen kayıt kümesiyle *değiştirilir.* Eşzamanlı değişikliklerin üzerine yazılmadığından emin olmak için [Etag denetimleri](dns-zones-records.md#etags) kullanılır. Bu denetimleri `-Overwrite` bastırmak için isteğe bağlı anahtarı kullanabilirsiniz.

Bu işlem sırası da *borulu*olabilir, yani bir parametre olarak geçmek yerine boruyu kullanarak kayıt kümesi nesnesini geçersiniz:

```powershell
Get-AzDnsRecordSet -Name "www" –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Add-AzDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzDnsRecordSet
```

Yukarıdaki örnekler, varolan bir 'A' türü kayıt kümesine nasıl 'A' kaydı ekleyeceğiniz gösterilmektedir. Benzer bir işlem dizisi, her kayıt türüne özgü diğer parametrelerin `-Ipv4Address` parametresini `Add-AzDnsRecordConfig` ikame ederek diğer türlerdeki kayıt kümelerine kayıt eklemek için kullanılır. Her kayıt türü için parametreler, yukarıdaki `New-AzDnsRecordConfig` Ek kayıt türü örneklerinde gösterildiği gibi cmdlet ile aynıdır.

'CNAME' veya 'SOA' türü kayıt kümeleri birden fazla kayıt içeremez. Bu kısıtlama DNS standartlarından doğar. Bu, Azure DNS'nin bir sınırlaması değildir.

## <a name="remove-a-record-from-an-existing-record-set"></a>Varolan bir kayıt kümesinden kaydı kaldırma

Bir kaydı kayıt kümesinden kaldırma işlemi, varolan bir kayıt kümesine kayıt eklemek için yapılan işleme benzer:

1. Varolan kayıt kümesini alma

    ```powershell
    $rs = Get-AzDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. Kaydı yerel kayıt kümesi nesnesinden kaldırın. Bu çevrimdışı bir operasyon. Kaldırılan kayıt, tüm parametreler arasında varolan bir kayıtla tam olarak eşleşmelidir.

    ```powershell
    Remove-AzDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. Değişikliği Azure DNS hizmetine geri taşıyın. Eşzamanlı değişiklikler `-Overwrite` için [Etag denetimlerini](dns-zones-records.md#etags) bastırmak için isteğe bağlı anahtarı kullanın.

    ```powershell
    Set-AzDnsRecordSet -RecordSet $Rs
    ```

Son kaydı bir kayıt kümesinden kaldırmak için yukarıdaki sırayı kullanmak kayıt kümesini silmez, boş bir kayıt kümesi bırakır. Bir kayıt kümesini tamamen kaldırmak için [bkz.](#delete-a-record-set)

Bir kayıt kümesine kayıt eklemeye benzer şekilde, bir kayıt kümesini kaldırmak için işlem sırası da boruyla işletilebilir:

```powershell
Get-AzDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Remove-AzDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzDnsRecordSet
```

Uygun türe özgü parametreler `Remove-AzDnsRecordSet`. Her kayıt türü için parametreler, yukarıdaki `New-AzDnsRecordConfig` Ek kayıt türü örneklerinde gösterildiği gibi cmdlet ile aynıdır.


## <a name="modify-an-existing-record-set"></a>Varolan bir kayıt kümesini değiştirme

Varolan bir kayıt kümesini değiştirme adımları, kayıt kümesine kayıt eklerken veya kaldırırken attığınız adımlara benzer:

1. 'yi kullanarak `Get-AzDnsRecordSet`ayarlanan varolan kaydı alın.
2. Yerel kayıt kümesi nesnesini aşağıdaki leri yaparak değiştirin:
    * Kayıt ekleme veya kaldırma
    * Varolan kayıtların parametrelerini değiştirme
    * Kayıt kümesi meta verilerini ve yaşama süresini değiştirme (TTL)
3. `Set-AzDnsRecordSet` Cmdlet kullanarak değişikliklerinizi gerçekleştirin. *Bu,* Azure DNS'de ayarlanan varolan kaydın yerine belirtilen kayıt kümesini alır.

Kullanırken, `Set-AzDnsRecordSet`Eşzamanlı değişikliklerin üzerine yazılmadığından emin olmak için [Etag denetimleri](dns-zones-records.md#etags) kullanılır. Bu denetimleri `-Overwrite` bastırmak için isteğe bağlı anahtarı kullanabilirsiniz.

### <a name="to-update-a-record-in-an-existing-record-set"></a>Varolan bir kayıt kümesindeki kaydı güncelleştirmek için

Bu örnekte, varolan bir 'A' kaydının IP adresini değiştiririz:

```powershell
$rs = Get-AzDnsRecordSet -name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Ipv4Address = "9.8.7.6"
Set-AzDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-an-soa-record"></a>SOA kaydını değiştirmek için

Bölge tepe noktasında (teklif`-Name "@"`işaretleri dahil) otomatik olarak oluşturulan SOA kaydından kayıt ekleyemez veya kaldıramazsınız. Ancak, SOA kaydı ("Ana Bilgisayar" hariç) ve TTL kayıt kümesindeki parametrelerden herhangi birini değiştirebilirsiniz.

Aşağıdaki örnek, SOA kaydının *E-posta* özelliğinin nasıl değiştirilebildiğini gösterir:

```powershell
$rs = Get-AzDnsRecordSet -Name "@" -RecordType SOA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Email = "admin.contoso.com"
Set-AzDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-ns-records-at-the-zone-apex"></a>Bölge tepe noktasındaki NS kayıtlarını değiştirmek için

Bölge tepe noktasında ayarlanan NS kaydı, her DNS bölgesiyle otomatik olarak oluşturulur. Bölgeye atanan Azure DNS ad sunucularının adlarını içerir.

Birden fazla DNS sağlayıcısıyla birlikte barındırılabilen etki alanlarını desteklemek için bu NS kayıt kümesine ek ad sunucuları ekleyebilirsiniz. Bu kayıt kümesi için TTL ve meta verilerini de değiştirebilirsiniz. Ancak, önceden doldurulmuş Azure DNS ad sunucularını kaldıramaz veya değiştiremezsiniz.

Bunun yalnızca bölge tepe noktasında ayarlanan NS kaydı için geçerli olduğunu unutmayın. Bölgenizdeki diğer NS kayıt kümeleri (alt bölgeleri temsilcilendirmek için kullanılır) kısıtlama olmaksızın değiştirilebilir.

Aşağıdaki örnek, bölge tepe noktasında ayarlanan NS kaydına nasıl ek bir ad sunucusu ekleyeceğinigösterir:

```powershell
$rs = Get-AzDnsRecordSet -Name "@" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Add-AzDnsRecordConfig -RecordSet $rs -Nsdname ns1.myotherdnsprovider.com
Set-AzDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-record-set-metadata"></a>Kayıt kümesi meta verilerini değiştirmek için

[Kayıt kümesi meta verileri,](dns-zones-records.md#tags-and-metadata) uygulamaya özgü verileri anahtar değer çiftleri olarak her kayıt kümesiyle ilişkilendirmek için kullanılabilir.

Aşağıdaki örnek, varolan bir kayıt kümesinin meta verilerinin nasıl değiştirilebildiğini gösterir:

```powershell
# Get the record set
$rs = Get-AzDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"

# Add 'dept=finance' name-value pair
$rs.Metadata.Add('dept', 'finance') 

# Remove metadata item named 'environment'
$rs.Metadata.Remove('environment')  

# Commit changes
Set-AzDnsRecordSet -RecordSet $rs
```


## <a name="delete-a-record-set"></a>Kayıt kümesini silme

Kayıt setleri `Remove-AzDnsRecordSet` cmdlet kullanılarak silinebilir. Bir kayıt kümesini silmek, kayıt kümesindeki tüm kayıtları da siler.

> [!NOTE]
> Bölge tepe noktasındaki SOA ve NS kayıt`-Name '@'`kümelerini silemezsiniz .  Azure DNS bunları bölge oluşturulduğunda otomatik olarak oluşturdu ve bölge silindiğinde bunları otomatik olarak siler.

Aşağıdaki örnekte, bir kayıt kümesinin nasıl silinir olduğu gösterilmektedir. Bu örnekte, kayıt kümesi adı, kayıt kümesi türü, bölge adı ve kaynak grubunun her biri açıkça belirtilir.

```powershell
Remove-AzDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Alternatif olarak, kayıt kümesi ada ve türüne göre belirtilebilir ve bir nesne kullanılarak belirtilen bölge:

```powershell
$zone = Get-AzDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

Üçüncü bir seçenek olarak, kayıt kümesinin kendisi bir kayıt kümesi nesnesi kullanılarak belirtilebilir:

```powershell
$rs = Get-AzDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzDnsRecordSet -RecordSet $rs
```

Kayıt kümesi nesnesi kullanılarak silinecek kayıt kümesini belirttiğiniz zaman, eşzamanlı değişikliklerin silinmemesini sağlamak için [Etag denetimleri](dns-zones-records.md#etags) kullanılır. Bu denetimleri `-Overwrite` bastırmak için isteğe bağlı anahtarı kullanabilirsiniz.

Kayıt kümesi nesnesi parametre olarak geçirilmek yerine boruyla da işlenebilir:

```powershell
Get-AzDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | Remove-AzDnsRecordSet
```

## <a name="confirmation-prompts"></a>Onay istemleri

`New-AzDnsRecordSet`, `Set-AzDnsRecordSet` ve `Remove-AzDnsRecordSet` cmdlet’lerinin tamamı onay istemlerini destekler.

PowerShell tercih değişkeninin `$ConfirmPreference` `Medium` değeri düşük veya daha düşükse, her cmdlet onay ister. Varsayılan değer `$ConfirmPreference` olduğundan, `High`varsayılan PowerShell ayarları kullanırken bu istemler verilmez.

`-Confirm` parametresini kullanarak geçerli `$ConfirmPreference` ayarını geçersiz kılabilirsiniz. `-Confirm` veya `-Confirm:$True` belirtirseniz cmdlet, çalıştırılmadan önce size onay istemi görüntüler. `-Confirm:$False` belirtirseniz cmdlet size onay istemi görüntülemez. 

`-Confirm` ve `$ConfirmPreference` hakkında daha fazla bilgi için bkz. [Tercih Değişkenleri Hakkında](/powershell/module/microsoft.powershell.core/about/about_preference_variables).

## <a name="next-steps"></a>Sonraki adımlar

[Azure DNS'deki bölgeler ve kayıtlar](dns-zones-records.md)hakkında daha fazla bilgi edinin.
<br>
Azure DNS kullanırken [bölgelerinizi ve kayıtlarınızı](dns-protect-zones-recordsets.md) nasıl koruyacağınızı öğrenin.
<br>
Azure [DNS PowerShell başvuru belgelerini](/powershell/module/az.dns)gözden geçirin.
