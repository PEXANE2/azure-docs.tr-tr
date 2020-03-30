---
title: Azure DNS'de ters DNS arama bölgelerini barındır
description: IP aralıklarınız için ters DNS arama bölgelerini barındırmak için Azure DNS'yi nasıl kullanacağınızı öğrenin
author: rohinkoul
ms.service: dns
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: rohink
ms.openlocfilehash: 78fc3428274be5e1998abe9189bea996f15e278c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79454270"
---
# <a name="host-reverse-dns-lookup-zones-in-azure-dns"></a>Azure DNS'de ters DNS arama bölgelerini barındır

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalede, Azure DNS'de atanan IP aralıkları için ters DNS arama bölgeleri nasıl barındırılacak açıklanmaktadır. Ters arama bölgeleri tarafından temsil edilen IP aralıkları, genellikle ISS'niz tarafından kuruluşunuza atanmalıdır.

Azure hizmetinize atanmış bir Azure'a ait BIR IP adresi için ters [DNS](dns-reverse-dns-for-azure-services.md)yapılandırmak için bkz.

Bu makaleyi okumadan önce, [Azure'da ters DNS ve desteğe genel bakışa](dns-reverse-dns-overview.md)aşina olmalısınız.

Bu makale, azure portalı, Azure PowerShell, Azure klasik CLI veya Azure CLI'yi kullanarak ilk ters arama DNS bölgenizi oluşturmak ve kaydetmek için gereken adımlara doğru ilerler.

## <a name="create-a-reverse-lookup-dns-zone"></a>Ters arama DNS bölgesi oluşturma

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. **Hub** menüsünde **Yeni** > **Ağ'ı**seçin ve ardından **DNS bölgesini**seçin.

   !["DNS bölgesi" seçimi](./media/dns-reverse-dns-hosting/figure1.png)

1. **DNS bölge oluştur** bölmesinde, DNS bölgenizi adlandırın. Bölgenin adı IPv4 ve IPv6 önekleri için farklı şekilde işlenmiştir. Bölgenizi adlandırmak için [IPv4](#ipv4) veya [IPv6](#ipv6) yönergelerini kullanın. İşiniz bittiğinde, bölgeyi oluşturmak için **Oluştur'u** seçin.

### <a name="ipv4"></a>IPv4

IPv4 ters arama bölgesinin adı temsil ettiği IP aralığına dayanır. Aşağıdaki biçimde olmalıdır: `<IPv4 network prefix in reverse order>.in-addr.arpa`. Örneğin, [Azure'da ters DNS ve desteğe genel bakış bölümüne](dns-reverse-dns-overview.md#ipv4)bakın.

> [!NOTE]
> Azure DNS'de sınıfsız ters DNS arama bölgeleri oluştururken, bölge`-`adında bir iletme`/`( ) yerine tire kullanmanız gerekir.
>
> Örneğin, IP aralığı 192.0.2.128/26 için, `128-26.2.0.192.in-addr.arpa` bölge adı yerine `128/26.2.0.192.in-addr.arpa`kullanmanız gerekir.
>
> DNS standartları her iki yöntemi de desteklese de, Azure DNS ileri`/`eğik çizgi ( ) karakteri içeren DNS bölge adlarını desteklemez.

Aşağıdaki örnek, Azure portalı aracılığıyla Azure DNS'de adı verilen `2.0.192.in-addr.arpa` C sınıfı ters DNS bölgesinin nasıl oluşturulurolduğunu gösterir:

 !["DNS bölgesi oluşturma" bölmesi, kutular dolu](./media/dns-reverse-dns-hosting/figure2.png)

**Kaynak grubu konumu** kaynak grubunun konumunu tanımlar. DNS bölgesi üzerinde hiçbir etkisi yoktur. DNS bölgesinin konumu her zaman “genel” şeklindedir ve gösterilmez.

Aşağıdaki örnekler, Azure PowerShell ve Azure CLI'yi kullanarak bu görevin nasıl tamamlanabildiğini gösterir.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
azure network dns zone create MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
az network dns zone create -g MyResourceGroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

IPv6 ters arama bölgesinin adı aşağıdaki formda olmalıdır: `<IPv6 network prefix in reverse order>.ip6.arpa`.  Örneğin, [Azure'da ters DNS ve desteğe genel bakış bölümüne](dns-reverse-dns-overview.md#ipv6)bakın.


Aşağıdaki örnek, Azure portalı aracılığıyla Azure DNS'de `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` adı verilen bir IPv6 ters DNS arama bölgesinin nasıl oluşturulabildiğini gösterir:

 !["DNS bölgesi oluşturma" bölmesi, kutular dolu](./media/dns-reverse-dns-hosting/figure3.png)

**Kaynak grubu konumu** kaynak grubunun konumunu tanımlar. DNS bölgesi üzerinde hiçbir etkisi yoktur. DNS bölgesinin konumu her zaman “genel” şeklindedir ve gösterilmez.

Aşağıdaki örnekler, Azure PowerShell ve Azure CLI'yi kullanarak bu görevin nasıl tamamlanabildiğini gösterir.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
azure network dns zone create MyResourceGroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
az network dns zone create -g MyResourceGroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a>Ters DNS arama bölgesini temsilciolarak

Artık ters DNS arama bölgenizi oluşturduğunuza göre, bölgenin üst bölgeden ataolduğundan emin olmalısınız. DNS delegasyonu, Ters DNS arama bölgenizi barındıran ad sunucularını bulmak için DNS ad çözümleme işlemini sağlar. Bu ad sunucuları daha sonra adres aralığınızdaki IP adresleri için DNS ters sorgularını yanıtlayabilir.

İleriye dönük arama bölgeleri için, bir DNS bölgesi atama işlemi [etki alanınızın Azure DNS'sine](dns-delegate-domain-azure-dns.md)devredin'de açıklanmıştır. Ters arama bölgeleri için delegasyon aynı şekilde çalışır. Tek fark, etki alanı adı kayıt şirketi yerine IP aralığınızı sağlayan ISS ile ad sunucularını yapılandırmanız gerektiğidir.

## <a name="create-a-dns-ptr-record"></a>DNS PTR kaydı oluşturma

### <a name="ipv4"></a>IPv4

Aşağıdaki örnek, Azure DNS'de ters dns bölgesinde bir PTR kaydı oluşturma işleminde size yol açabilirsiniz. Diğer kayıt türleri ve var olan kayıtların değiştirilmesi hakkında bilgi için bkz. [Azure portalı kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets-portal.md).

1. **DNS bölge** bölmesinin üst kısmında , Ekle kayıt **kümesi** bölmesini açmak için **ayarla + Kaydet kümesini** seçin.

   ![Kayıt kümesi oluşturmak için düğme](./media/dns-reverse-dns-hosting/figure4.png)

1. Bir PTR kaydı için ayarlanan kaydın adının ters sırada IPv4 adresinin geri kalanı olması gerekir. 

   Bu örnekte, ilk üç sekizli zaten bölge adının bir parçası olarak doldurulur (.2.0.192). Bu nedenle, **Ad** kutusunda yalnızca son sekizli verilir. Örneğin, IP adresi 192.0.2.15 olan bir kaynak için kayıt kümenizi **15** olarak adlandırabilirsiniz.  
1. **Türü**için, **PTR**seçin.  
1. **DOMAIN NAME**için IP'yi kullanan kaynağın tam nitelikli alan adını (FQDN) girin.
1. DNS kaydını oluşturmak için bölmenin altındaki **Tamam'ı** seçin.

   !["Kayıt kümesi ekle" bölmesi, kutular dolu](./media/dns-reverse-dns-hosting/figure5.png)

Aşağıdaki örnekler, PowerShell veya Azure CLI kullanarak bu görevin nasıl tamamlanabildiğini gösterir.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
azure network dns record-set add-record MyResourceGroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
az network dns record-set ptr add-record -g MyResourceGroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a>IPv6

Aşağıdaki örnek, yeni PTR kaydı oluşturma sürecinde size yol alar. Diğer kayıt türleri ve var olan kayıtların değiştirilmesi hakkında bilgi için bkz. [Azure portalı kullanarak DNS kayıtlarını ve kayıt kümelerini yönetme](dns-operations-recordsets-portal.md).

1. **DNS bölge** bölmesinin üst kısmında , Ekle kayıt **kümesi** bölmesini açmak için **ayarla + Kaydet kümesini** seçin.

   ![Kayıt kümesi oluşturmak için düğme](./media/dns-reverse-dns-hosting/figure6.png)

2. Bir PTR kaydı için ayarlanan kaydın adının ters sırada IPv6 adresinin geri kalanı olması gerekir. Herhangi bir sıfır sıkıştırma içermemelidir. 

   Bu örnekte, IPv6'nın ilk 64 biti zaten bölge adının bir parçası olarak doldurulur (0.0.0.c.d.b.a.a.8.b.d.0.1.0.0.2.ip6.arpa). Bu nedenle, **Ad** kutusunda yalnızca son 64 bit sağlanır. IP adresinin son 64 biti ters sırada girilir ve her hexadecimal sayı arasındaki sınırlayıcı olarak bir dönem verilir. Örneğin, IP adresi 2001:0db8:abdc:0000:f524:10bc:1af9:405e olan bir kaynak için kayıt setinize **e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f** adını alabilirsiniz.  
3. **Türü**için, **PTR**seçin.  
4. **DOMAIN NAME**için IP kullanan kaynağın FQDN'sini girin.
5. DNS kaydını oluşturmak için bölmenin altındaki **Tamam'ı** seçin.

!["Kayıt kümesi ekle" bölmesi, kutular dolu](./media/dns-reverse-dns-hosting/figure7.png)

Aşağıdaki örnekler, PowerShell veya Azure CLI kullanarak bu görevin nasıl tamamlanabildiğini gösterir.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
azure network dns record-set add-record MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azure-cli"></a>Azure CLI

```azurecli
az network dns record-set ptr add-record -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a>Kayıtları görüntüleme

Oluşturduğunuz kayıtları görüntülemek için Azure portalındaki DNS bölgenize göz atın. **DNS bölge** bölmesinin alt kısmında, DNS bölgesinin kayıtlarını görebilirsiniz. Varsayılan NS ve SOA kayıtlarını ve oluşturduğunuz yeni kayıtları görmeniz gerekir. NS ve SOA kayıtları her bölgede oluşturulur. 

### <a name="ipv4"></a>IPv4

**DNS bölge** bölmesi IPv4 PTR kayıtlarını gösterir:

![IPv4 kayıtları ile "DNS bölge" bölmesi](./media/dns-reverse-dns-hosting/figure8.png)

Aşağıdaki örnekler, PowerShell veya Azure CLI kullanarak PTR kayıtlarının nasıl görüntülenebildiğini gösterir.

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
azure network dns record-set list MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
az network dns record-set list -g MyResourceGroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

**DNS bölge** bölmesi IPv6 PTR kayıtlarını gösterir:

![IPv6 kayıtları ile "DNS bölge" bölmesi](./media/dns-reverse-dns-hosting/figure9.png)

Aşağıdaki örnekler, PowerShell veya Azure CLI kullanarak kayıtların nasıl görüntülenebildiğini gösterir.

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-classic-cli"></a>Azure klasik CLI

```azurecli
azure network dns record-set list MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
az network dns record-set list -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a>SSS

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Azure DNS'de ISS'ye atanmış IP bloklarım için ters DNS arama bölgelerini barındırabilir miyim?

Evet. Azure DNS'de kendi IP aralıklarınız için ters arama (ARPA) bölgelerini barındırmak tamamen desteklenir.

Bu makalede açıklandığı gibi Azure DNS'de ters arama bölgesini oluşturun ve [ardından bölgeyi temsilciolarak](dns-domain-delegation.md)ISS'nizle birlikte çalışın. Daha sonra, her ters arama için PTR kayıtlarını diğer kayıt türleri ile aynı şekilde yönetebilirsiniz.

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a>Ne kadar benim ters DNS arama bölgesi maliyeti barındırma nedir?

Azure DNS'de ISS'ye atanmış IP bloğunuzun ters DNS arama bölgesini [barındırma, standart Azure DNS oranlarıüzerinden](https://azure.microsoft.com/pricing/details/dns/)ücretlendirilir.

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Azure DNS'de hem IPv4 hem de IPv6 adresleri için ters DNS arama bölgelerini barındırabilir miyim?

Evet. Bu makalede, Azure DNS'de Hem IPv4 hem de IPv6 ters DNS arama bölgeleri nasıl oluşturulacak açıklanmaktadır.

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a>Varolan bir ters DNS arama bölgesini içe aktarabilir miyim?

Evet. Varolan DNS bölgelerini Azure DNS'ye almak için Azure CLI'yi kullanabilirsiniz. Bu yöntem hem ileri arama bölgeleri hem de ters arama bölgeleri için çalışır.

Daha fazla bilgi için Azure [CLI kullanarak bir DNS bölge dosyası alma ve dışa aktarma'ya](dns-import-export.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Ters DNS hakkında daha fazla bilgi [için, Vikipedi'de ters DNS arama](https://en.wikipedia.org/wiki/Reverse_DNS_lookup)bakın.
<br>
[Azure hizmetleriniz için ters DNS kayıtlarını nasıl yöneteceğinizi](dns-reverse-dns-for-azure-services.md)öğrenin.
