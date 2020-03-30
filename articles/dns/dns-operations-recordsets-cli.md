---
title: Azure CLI'yi kullanarak Azure DNS'deki DNS kayıtlarını yönetme
description: Etki alanınızı Azure DNS'de barındırırken Azure DNS'de DNS kayıt kümelerini ve kayıtlarını yönetme.
author: rohinkoul
ms.assetid: 5356a3a5-8dec-44ac-9709-0c2b707f6cb5
ms.service: dns
ms.devlang: azurecli
ms.topic: conceptual
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 05/15/2018
ms.author: rohink
ms.openlocfilehash: 4e017dc940e1d32888ff279904e44d34db1fd5c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76936895"
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-the-azure-cli"></a>Azure CLI'yi kullanarak Azure DNS'deki DNS kayıtlarını ve kayıt kümelerini yönetme

> [!div class="op_single_selector"]
> * [Azure Portalı](dns-operations-recordsets-portal.md)
> * [Azure CLI](dns-operations-recordsets-cli.md)
> * [Powershell](dns-operations-recordsets.md)

Bu makalede, Windows, Mac ve Linux için kullanılabilen çapraz platform Azure CLI'yi kullanarak DNS bölgenizdeki DNS kayıtlarını nasıl yönetisiniz gösterilmektedir. DNS kayıtlarınızı [Azure PowerShell](dns-operations-recordsets.md) veya [Azure portalını](dns-operations-recordsets-portal.md)kullanarak da yönetebilirsiniz.

Bu makaledeki [örnekler, Azure CLI'yi zaten yüklediğinizi, oturum açttık ve bir DNS bölgesi oluşturduğunuzu](dns-operations-dnszones-cli.md)varsayar.

## <a name="introduction"></a>Giriş

Azure DNS’de DNS kayıtlarını oluşturmadan önce Azure DNS’nin DNS kayıtlarını DNS kayıt kümeleri şeklinde nasıl düzenlediğini kavramanız gerekir.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Azure DNS’deki DNS kayıtları hakkında daha fazla bilgi için bkz. [DNS bölgeleri ve kayıtları](dns-zones-records.md).

## <a name="create-a-dns-record"></a>DNS kaydı oluşturma

Bir DNS kaydı oluşturmak `az network dns record-set <record-type> add-record` için, `<record-type>` komutu kullanın (kayıt türü nerede, yani a, srv, txt, vb.) Yardım için `az network dns record-set --help`bkz.

Bir kayıt oluştururken kaynak grubu adını, bölge adını, kaynak kümesi adını, kaynak türünü ve oluşturulan kaynağın ayrıntılarını belirtmeniz gerekir. Verilen kayıt kümesi adı *göreli* bir ad olmalıdır, yani bölge adını dışlamalıdır.

Kayıt kümesi mevcut değilse bu komutla oluşturulur. Kayıt kümesi mevcutsa bu komut belirttiğiniz kaydı var olan kayıt kümesine ekler.

Yeni bir kayıt kümesi oluşturuluyorsa yaşam süresi (TTL) olarak varsayılan 3600 değeri kullanılır. Farklı TTL'lerin nasıl kullanılacağına ilişkin talimatlar [için](#create-a-dns-record-set)bkz.

Aşağıdaki örnek *www* adlı A kaydını *contoso.com* bölgesinde ve *MyResourceGroup* kaynak grubu içinde oluşturur. A kaydının IP adresi *1.2.3.4* olarak belirtilmiştir.

```azurecli
az network dns record-set a add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

Bölgenin tepesinde ayarlanmış bir kayıt oluşturmak için (bu durumda, "contoso.com"),\@tırnak işaretleri de dahil olmak üzere " " kaydıadını kullanın:

```azurecli
az network dns record-set a add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --ipv4-address 1.2.3.4
```

## <a name="create-a-dns-record-set"></a>DNS kayıt kümesi oluşturma

Yukarıdaki örneklerde, DNS kaydı varolan bir kayıt kümesine eklendi veya kayıt kümesi *örtülü olarak*oluşturuldu. Ayrıca, kayıt eklemeden önce *açıkça* kayıt kümesini oluşturabilirsiniz. Azure DNS, DNS kayıtları oluşturmadan önce bir DNS adını ayırmak için yer tutucu olarak hareket edebilen 'boş' kayıt kümelerini destekler. Boş kayıt kümeleri Azure DNS denetim düzleminde görünür, ancak Azure DNS ad sunucularında görünmez.

Kayıt kümeleri komutu `az network dns record-set <record-type> create` kullanılarak oluşturulur. Yardım için bkz. `az network dns record-set <record-type> create --help`.

Kayıt kümesini oluşturmak, [Zaman-To-Live (TTL)](dns-zones-records.md#time-to-live) ve meta veriler gibi kayıt kümesi özelliklerini belirtmenize olanak tanır. [Kayıt kümesi meta verileri,](dns-zones-records.md#tags-and-metadata) uygulamaya özgü verileri anahtar değer çiftleri olarak her kayıt kümesiyle ilişkilendirmek için kullanılabilir.

Aşağıdaki örnek, `--ttl` parametre (kısa form) `-l`kullanarak 60 saniyelik TTL ile 'A' türüboş bir kayıt kümesi oluşturur:

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --name www --ttl 60
```

Aşağıdaki örnek, `--metadata` parametreyi kullanarak iki meta veri girişi olan "dept=finance" ve "environment=production" ile bir kayıt kümesi oluşturur:

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --name www --metadata "dept=finance" "environment=production"
```

Boş bir kayıt kümesi oluşturduktan sonra, kayıtlar `azure network dns record-set <record-type> add-record` [DNS kaydı oluştur'da](#create-a-dns-record)açıklandığı gibi kullanılarak eklenebilir.

## <a name="create-records-of-other-types"></a>Diğer türlerin kayıtlarını oluşturma

'A' kayıtlarının nasıl oluşturulacaklarını ayrıntılı olarak gördükten sonra, aşağıdaki örnekler Azure DNS tarafından desteklenen diğer kayıt türlerinin kaydının nasıl oluşturulacak olduğunu gösterir.

Kayıt verilerini belirtmek için kullanılan parametreler, kayıt türüne bağlı olarak değişiklik gösterir. Örneğin "A" türünde bir kayıt için IPv4 adresini `--ipv4-address <IPv4 address>` parametresiyle belirtirsiniz. Her kayıt türü için parametreler `az network dns record-set <record-type> add-record --help`kullanılarak listelenebilir.

Her durumda, tek bir kaydın nasıl oluşturulabildiğini gösteririz. Kayıt varolan kayıt kümesine veya örtülü olarak oluşturulan bir kayıt kümesine eklenir. Kayıt kümeleri oluşturma ve kayıt kümesi parametresi tanımlama hakkında daha fazla bilgi için [bkz.](#create-a-dns-record-set)

SOA'lar her DNS bölgesiyle oluşturulup silindiğinden ve ayrı olarak oluşturulamadığından veya silinemediğinden, SOA kayıt kümesi oluşturmak için bir örnek vermeyiz. Ancak, [SOA daha sonraki bir örnekte gösterildiği gibi değiştirilebilir.](#to-modify-an-soa-record)

### <a name="create-an-aaaa-record"></a>AAAA kaydı oluşturma

```azurecli
az network dns record-set aaaa add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-aaaa --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-an-caa-record"></a>CAA kaydı oluşturma

```azurecli
az network dns record-set caa add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-caa --flags 0 --tag "issue" --value "ca1.contoso.com"
```

### <a name="create-a-cname-record"></a>CNAME kaydı oluşturma

> [!NOTE]
> DNS standartları, bir bölgenin tepesindeki CNAME kayıtlarına`--Name "@"`izin vermez ( ), ne de birden fazla kayıt içeren kayıt kümelerine izin vermez.
> 
> Daha fazla bilgi için [CNAME kayıtlarına](dns-zones-records.md#cname-records)bakın.

```azurecli
az network dns record-set cname set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-cname --cname www.contoso.com
```

### <a name="create-an-mx-record"></a>MX kaydı oluşturma

Bu örnekte, bölge tepe noktasında\@MX kaydı oluşturmak için " " " " adlı kayıt kümesi adını kullanırız (bu durumda, "contoso.com").

```azurecli
az network dns record-set mx add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a>NS kaydı oluşturma

```azurecli
az network dns record-set ns add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-ns --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a>PTR kaydı oluşturma

Bu durumda, 'my-arpa-zone.com' IP aralığınızı temsil eden ARPA bölgesini temsil eder. Bu bölgedeki her PTR kaydı bu IP aralığındaki bir IP adresine karşılık gelir.  '10' kayıt adı, bu kayıtla temsil edilen bu IP aralığındaki IP adresinin son sekizlisidir.

```azurecli
az network dns record-set ptr add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name my-arpa.zone.com --ptrdname myservice.contoso.com
```

### <a name="create-an-srv-record"></a>SRV kaydı oluşturma

[Bir SRV kayıt kümesi](dns-zones-records.md#srv-records)oluştururken, kayıt kümesi adında * \_hizmet* ve * \_protokolü* belirtin. Bölge tepe noktasında bir\@SRV kaydı oluşturulurken " " " ikime " eklemenize gerek yoktur.

```azurecli
az network dns record-set srv add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name _sip._tls --priority 10 --weight 5 --port 8080 --target sip.contoso.com
```

### <a name="create-a-txt-record"></a>TXT kaydı oluşturma

Aşağıdaki örnekte, TXT kaydının nasıl oluşturuluşu gösterilmektedir. TXT kayıtlarında desteklenen maksimum dize uzunluğu hakkında daha fazla bilgi için Bkz. [TXT kayıtları.](dns-zones-records.md#txt-records)

```azurecli
az network dns record-set txt add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-txt --value "This is a TXT record"
```

## <a name="get-a-record-set"></a>Kayıt seti alma

Varolan bir kayıt kümesini almak için `az network dns record-set <record-type> show`. Yardım için bkz. `az network dns record-set <record-type> show --help`.

Bir kayıt veya kayıt kümesi oluştururken olduğu gibi, verilen kayıt kümesi adı *göreli* bir ad olmalıdır, yani bölge adını dışlamalıdır. Ayrıca kayıt türünü, kayıt kümesini içeren bölgeyi ve bölgeyi içeren kaynak grubunu belirtmeniz gerekir.

Aşağıdaki örnek, *MyResourceGroup*kaynak grubunda *contoso.com* a tipi nin *kaydını* alır:

```azurecli
az network dns record-set a show --resource-group myresourcegroup --zone-name contoso.com --name www
```

## <a name="list-record-sets"></a>Liste kayıt kümeleri

Komutu kullanarak `az network dns record-set list` bir DNS bölgesindeki tüm kayıtları listeleyebilirsiniz. Yardım için bkz. `az network dns record-set list --help`.

Bu örnek, ad veya kayıt türüne bakılmaksızın, kaynak grubu *MyResourceGroup'ta* *contoso.com*alandaki tüm kayıt kümelerini döndürür:

```azurecli
az network dns record-set list --resource-group myresourcegroup --zone-name contoso.com
```

Bu örnek, verilen kayıt türüyle eşleşen tüm kayıt kümelerini döndürür (bu durumda, 'A' kayıtları):

```azurecli
az network dns record-set a list --resource-group myresourcegroup --zone-name contoso.com 
```

## <a name="add-a-record-to-an-existing-record-set"></a>Varolan bir kayıt kümesine kayıt ekleme

Her ikisini `az network dns record-set <record-type> add-record` de yeni bir kayıt kümesinde kayıt oluşturmak veya varolan bir kayıt kümesine kayıt eklemek için kullanabilirsiniz.

Daha fazla bilgi için [bkz.](#create-a-dns-record) [Create records of other types](#create-records-of-other-types)

## <a name="remove-a-record-from-an-existing-record-set"></a>Varolan bir kayıt kümesinden bir kayıt kaldırın.

Varolan bir kayıt kümesinden Bir DNS kaydını kaldırmak için `az network dns record-set <record-type> remove-record`. Yardım için bkz. `az network dns record-set <record-type> remove-record -h`.

Bu komut, bir DNS kaydını bir kayıt kümesinden siler. Bir kayıt kümesindeki son kayıt silinirse, kayıt kümesinin kendisi de silinir. Bunun yerine boş kayıt kümesini `--keep-empty-record-set` tutmak için seçeneği kullanın.

Silinecek kaydı ve silinmesi gereken bölgeyi belirtmeniz `az network dns record-set <record-type> add-record`gerekir. Bu [parametreler, bir DNS kaydı oluştur](#create-a-dns-record) ve yukarıdaki diğer türlerin kayıtlarını [oluşturma'da](#create-records-of-other-types) açıklanmıştır.

Aşağıdaki örnek, *MyResourceGroup*kaynak grubunda *contoso.com* *contoso.com, www* adlı kayıt kümesinden '1.2.3.4' değerindeki A kaydını siler.

```azurecli
az network dns record-set a remove-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "www" --ipv4-address 1.2.3.4
```

## <a name="modify-an-existing-record-set"></a>Varolan bir kayıt kümesini değiştirme

Her kayıt kümesi bir [zaman-to-live (TTL)](dns-zones-records.md#time-to-live), [meta veri](dns-zones-records.md#tags-and-metadata)ve DNS kayıtları içerir. Aşağıdaki bölümlerde bu özelliklerin her biri nasıl değiştirilen açıklanmıştır.

### <a name="to-modify-an-a-aaaa-caa-mx-ns-ptr-srv-or-txt-record"></a>A, AAAA, CAA, MX, NS, PTR, SRV veya TXT kaydını değiştirmek için

A, AAAA, CAA, MX, NS, PTR, SRV veya TXT türündeki varolan bir kaydı değiştirmek için önce yeni bir kayıt eklemeniz ve ardından varolan kaydı silmeniz gerekir. Kayıtların nasıl silinip eklence ilgili ayrıntılı talimatlar için bu makalenin önceki bölümlerine bakın.

Aşağıdaki örnek, IP adresi 1.2.3.4'ten IP adresi 5.6.7.8'e kadar bir 'A' kaydının nasıl değiştirilebildiğini gösterir:

```azurecli
az network dns record-set a add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 5.6.7.8
az network dns record-set a remove-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

Bölge tepe noktasında (teklif`--Name "@"`işaretleri dahil) otomatik olarak oluşturulan NS kaydındaki kayıtları ekleyemez, kaldıramaz veya değiştiremezsiniz. Bu kayıt kümesi için, yalnızca izin verilen değişiklikler kayıt kümesi TTL ve meta verilerini değiştirmektir.

### <a name="to-modify-a-cname-record"></a>CNAME kaydını değiştirmek için

Diğer kayıt türlerinin aksine, CNAME kayıt kümesi yalnızca tek bir kayıt içerebilir.  Bu nedenle, diğer kayıt türlerinde olduğu gibi yeni bir kayıt ekleyerek ve varolan kaydı kaldırarak geçerli değeri değiştiremezsiniz.

Bunun yerine, bir CNAME `az network dns record-set cname set-record`kaydını değiştirmek için. Yardım için bkz. `az network dns record-set cname set-record --help`

Örnek, kaynak grubu *MyResourceGroup'ta* *www* *contoso.com,* varolan değeri yerine 'www.fabrikam.net' işaret etmek için cname rekorunu contoso.com olarak değiştirir:

```azurecli
az network dns record-set cname set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-cname --cname www.fabrikam.net
``` 

### <a name="to-modify-an-soa-record"></a>SOA kaydını değiştirmek için

Diğer kayıt türlerinin aksine, CNAME kayıt kümesi yalnızca tek bir kayıt içerebilir.  Bu nedenle, diğer kayıt türlerinde olduğu gibi yeni bir kayıt ekleyerek ve varolan kaydı kaldırarak geçerli değeri değiştiremezsiniz.

Bunun yerine, SOA kaydını `az network dns record-set soa update`değiştirmek için. Yardım için bkz. `az network dns record-set soa update --help`.

Aşağıdaki örnek, *MyResourceGroup*kaynak grubunda *contoso.com* bölge için SOA kaydının 'e-posta' özelliğinin nasıl ayarlanır:

```azurecli
az network dns record-set soa update --resource-group myresourcegroup --zone-name contoso.com --email admin.contoso.com
```

### <a name="to-modify-ns-records-at-the-zone-apex"></a>Bölge tepe noktasındaki NS kayıtlarını değiştirmek için

Bölge tepe noktasında ayarlanan NS kaydı, her DNS bölgesiyle otomatik olarak oluşturulur. Bölgeye atanan Azure DNS ad sunucularının adlarını içerir.

Birden fazla DNS sağlayıcısıyla birlikte barındırılabilen etki alanlarını desteklemek için bu NS kayıt kümesine ek ad sunucuları ekleyebilirsiniz. Bu kayıt kümesi için TTL ve meta verilerini de değiştirebilirsiniz. Ancak, önceden doldurulmuş Azure DNS ad sunucularını kaldıramaz veya değiştiremezsiniz.

Bunun yalnızca bölge tepe noktasında ayarlanan NS kaydı için geçerli olduğunu unutmayın. Bölgenizdeki diğer NS kayıt kümeleri (alt bölgeleri temsilcilendirmek için kullanılır) kısıtlama olmaksızın değiştirilebilir.

Aşağıdaki örnek, bölge tepe noktasında ayarlanan NS kaydına nasıl ek bir ad sunucusu ekleyeceğinigösterir:

```azurecli
az network dns record-set ns add-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --nsdname ns1.myotherdnsprovider.com 
```

### <a name="to-modify-the-ttl-of-an-existing-record-set"></a>Varolan bir kayıt kümesinin TTL'sini değiştirmek için

Varolan bir kayıt kümesinin TTL'sini değiştirmek için `azure network dns record-set <record-type> update`. Yardım için bkz. `azure network dns record-set <record-type> update --help`.

Aşağıdaki örnekte, bu durumda bir kayıt kümesi TTL'nin nasıl değiştirilen 60 saniyeye kadar değiştirilebildiğini gösterir:

```azurecli
az network dns record-set a update --resource-group myresourcegroup --zone-name contoso.com --name www --set ttl=60
```

### <a name="to-modify-the-metadata-of-an-existing-record-set"></a>Varolan bir kayıt kümesinin meta verilerini değiştirmek için

[Kayıt kümesi meta verileri,](dns-zones-records.md#tags-and-metadata) uygulamaya özgü verileri anahtar değer çiftleri olarak her kayıt kümesiyle ilişkilendirmek için kullanılabilir. Varolan bir kayıt kümesinin meta `az network dns record-set <record-type> update`verilerini değiştirmek için . Yardım için bkz. `az network dns record-set <record-type> update --help`.

Aşağıdaki örnek, "dept=finance" ve "environment=production" olmak üzere iki meta veri girişi içeren bir kayıt kümesinin nasıl değiştirilebildiğini gösterir. Varolan meta verilerin verilen değerlerle *değiştirileceğini* unutmayın.

```azurecli
az network dns record-set a update --resource-group myresourcegroup --zone-name contoso.com --name www --set metadata.dept=finance metadata.environment=production
```

## <a name="delete-a-record-set"></a>Kayıt kümesini silme

Kayıt kümeleri `az network dns record-set <record-type> delete` komutu kullanılarak silinebilir. Yardım için bkz. `azure network dns record-set <record-type> delete --help`. Bir kayıt kümesini silmek, kayıt kümesindeki tüm kayıtları da siler.

> [!NOTE]
> Bölge tepe noktasındaki SOA ve NS kayıt`--name "@"`kümelerini silemezsiniz .  Bunlar, bölge oluşturulduğunda otomatik olarak oluşturulur ve bölge silindiğinde otomatik olarak silinir.

Aşağıdaki örnek, Kaynak Grubu *MyResourceGroup'taki* *contoso.com* A tipi *www* adlı kayıt kümesini siler:

```azurecli
az network dns record-set a delete --resource-group myresourcegroup --zone-name contoso.com --name www
```

Silme işlemini onaylamanız istenir. Bu istemi bastırmak için `--yes` anahtarı kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure DNS'deki bölgeler ve kayıtlar](dns-zones-records.md)hakkında daha fazla bilgi edinin.
<br>
Azure DNS kullanırken [bölgelerinizi ve kayıtlarınızı](dns-protect-zones-recordsets.md) nasıl koruyacağınızı öğrenin.
