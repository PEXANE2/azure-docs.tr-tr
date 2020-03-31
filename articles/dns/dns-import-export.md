---
title: Etki alanı bölgesi dosyalarını alma ve dışa aktarma - Azure CLI
titleSuffix: Azure DNS
description: Azure CLI'yi kullanarak Bir DNS bölge dosyasını Azure DNS'ye nasıl içe aktarıp dışa aktarın
services: dns
author: rohinkoul
ms.service: dns
ms.date: 4/3/2019
ms.author: rohink
ms.topic: conceptual
ms.openlocfilehash: a5c2fdde564eba2d95e7f14f4d47e4d381739d5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79365177"
---
# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli"></a>Azure CLI’yı kullanarak DNS bölge dosyasını içeri ve dışarı aktarma

Bu makale, Azure CLI'yi kullanarak Azure DNS için DNS bölge dosyalarını nasıl içe aktarabileceğinizi ve dışa aktarabileceğinizi size iletebilirsiniz.

## <a name="introduction-to-dns-zone-migration"></a>DNS bölge geçişine giriş

DNS bölge dosyası, bölgedeki her Etki Alanı Adı Sistemi (DNS) kaydının ayrıntılarını içeren bir metin dosyasıdır. DNS kayıtlarının DNS sistemleri arasında aktarılması için uygun hale getiren standart bir biçim izler. Bölge dosyasını kullanmak, Bir DNS bölgesini Azure DNS'ye veya Azure DNS'ye aktarmanın hızlı, güvenilir ve kullanışlı bir yoludur.

Azure DNS, Azure komut satırı arabirimini (CLI) kullanarak bölge dosyalarını alma ve dışa aktarmayı destekler. Bölge dosya alma işlemi şu anda Azure PowerShell veya Azure portalı üzerinden **desteklenmez.**

Azure CLI, Azure hizmetlerini yönetmek için kullanılan bir çapraz platform komut satırı aracıdır. [Azure indirme sayfasından](https://azure.microsoft.com/downloads/)Windows, Mac ve Linux platformları için kullanılabilir. En yaygın ad sunucusu yazılımı [OLAN BIND](https://www.isc.org/downloads/bind/)genellikle Linux'ta çalıştığından, platform ötesi destek bölge dosyalarını almak ve dışa aktarmak için önemlidir.

## <a name="obtain-your-existing-dns-zone-file"></a>Varolan DNS bölge dosyanızı edinin

Azure DNS'ye bir DNS bölge dosyası almadan önce bölge dosyasının bir kopyasını almanız gerekir. Bu dosyanın kaynağı, DNS bölgesinin şu anda nerede barındırıldığına bağlıdır.

* DNS bölgeniz bir iş ortağı hizmeti (etki alanı kayıt şirketi, özel DNS barındırma sağlayıcısı veya alternatif bulut sağlayıcısı gibi) tarafından barındırılırsa, bu hizmet In DNS bölge dosyasını karşıdan yükleme olanağı sağlamalıdır.
* DNS bölgeniz Windows DNS'de barındırılırsa, bölge dosyaları için varsayılan klasör **%systemroot%\system32\dns'dir.** Her bölge dosyasına giden tam yol, DNS konsolunun **Genel** sekmesinde de gösterir.
* DNS bölgeniz BIND kullanılarak barındırılırsa, her bölge için bölge dosyasının **konumu,.conf adlı**BIND yapılandırma dosyasında belirtilir.

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Azure DNS'ye Bir DNS bölge dosyası alma

Bir bölge dosyası nın içe aktarılması, zaten yoksa Azure DNS'de yeni bir bölge oluşturur. Bölge zaten varsa, bölge dosyasındaki kayıt kümeleri varolan kayıt kümeleriyle birleştirilmelidir.

### <a name="merge-behavior"></a>Birleştirme davranışı

* Varsayılan olarak, varolan ve yeni kayıt kümeleri birleştirilir. Birleştirilmiş kayıt kümesiiçindeki özdeş kayıtlar çoğaltılır.
* Kayıt kümeleri birleştirildiğinde, önceden varolan kayıt kümelerinin yaşama süresi (TTL) kullanılır.
* Yetki Başlangıcı (SOA) parametreleri (hariç) `host`her zaman alınan bölge dosyasından alınır. Benzer şekilde, bölge tepe noktasında ayarlanan ad sunucusu kaydı için, TTL her zaman alınan bölge dosyasından alınır.
* İçe aktarılan bir CNAME kaydı, varolan bir CNAME kaydının aynı ada göre değiştirilmez.  
* CNAME kaydı ile aynı adı taşıyan ancak farklı türde başka bir kayıt arasında bir çakışma ortaya çıktığında (varolan veya yeni olan) varolan kayıt korunur. 

### <a name="additional-information-about-importing"></a>Alma hakkında ek bilgiler

Aşağıdaki notlar, bölge alma işlemi hakkında ek teknik ayrıntılar sağlar.

* Yönerge `$TTL` isteğe bağlıdır ve desteklenir. Yönerge `$TTL` verilmediğinde, açık TTL'si olmayan kayıtlar 3600 saniye lik varsayılan TTL'ye ayarlanır. Aynı kayıt kümesindeki iki kayıt farklı TTL'ler belirttiğinde, daha düşük değer kullanılır.
* Yönerge `$ORIGIN` isteğe bağlıdır ve desteklenir. Hayır `$ORIGIN` ayarlandığında, kullanılan varsayılan değer komut satırında belirtildiği gibi bölge adıdır (artı sonlandırıcı ".").
* Ve `$INCLUDE` `$GENERATE` direktifler desteklenmez.
* Bu kayıt türleri desteklenir: A, AAAA, CAA, CNAME, MX, NS, SOA, SRV ve TXT.
* Bir bölge oluşturulduğunda SOA kaydı Azure DNS tarafından otomatik olarak oluşturulur. Bir bölge dosyasını içe aktardığınızda, `host` parametre *dışındaki* tüm SOA parametreleri bölge dosyasından alınır. Bu parametre, Azure DNS tarafından sağlanan değeri kullanır. Bunun nedeni, bu parametrenin Azure DNS tarafından sağlanan birincil ad sunucusuna başvurması gerektiğidir.
* Bölge tepe noktasında ayarlanan ad sunucusu kaydı, bölge oluşturulduğunda Azure DNS tarafından otomatik olarak oluşturulur. Yalnızca bu kayıt kümesinin TTL'si alınır. Bu kayıtlar, Azure DNS tarafından sağlanan ad sunucu adlarını içerir. Kayıt verileri, içe aktarılan bölge dosyasında bulunan değerler tarafından üzerine yazılmaz.
* Azure DNS, Genel Önizleme sırasında yalnızca tek dizeli TXT kayıtlarını destekler. Multistring TXT kayıtları concatenated ve 255 karakter kesilir.

### <a name="cli-format-and-values"></a>CLI biçimi ve değerleri

Bir DNS bölgesini almak için Azure CLI komutunun biçimi:

```azurecli
az network dns zone import -g <resource group> -n <zone name> -f <zone file name>
```

Değerler:

* `<resource group>`Azure DNS'deki bölge için kaynak grubunun adıdır.
* `<zone name>`bölgenin adıdır.
* `<zone file name>`alınacak bölge dosyasının yolu/adıdır.

Kaynak grubunda bu ada sahip bir bölge yoksa, sizin için oluşturulur. Bölge zaten varsa, alınan kayıt kümeleri varolan kayıt kümeleriyle birleştirilir. 

### <a name="step-1-import-a-zone-file"></a>1. Adım. Bölge dosyanı alma

Bölge **contoso.com**için bir bölge dosyası almak için.

1. Zaten bir kaynağınız yoksa, bir Kaynak Yöneticisi kaynak grubu oluşturmanız gerekir.

    ```azurecli
    az group create --group myresourcegroup -l westeurope
    ```

2. Bölge **contoso.com** dosyadan **contoso.com.txt** kaynak **grubunda**yeni bir DNS bölgesine almak için, `az network dns zone import`komutu çalıştırırsınız.<BR>Bu komut bölge dosyasını yükler ve ayrıştirır. Komut, bölge ve bölgedeki tüm kayıt kümelerini oluşturmak için Azure DNS hizmetinde bir dizi komut uyguluyor. Komut, konsol penceresinde ki ilerlemeyi ve hataları veya uyarıları bildirir. Kayıt kümeleri seri olarak oluşturulduğundan, büyük bir bölge dosyasını almak birkaç dakika sürebilir.

    ```azurecli
    az network dns zone import -g myresourcegroup -n contoso.com -f contoso.com.txt
    ```

### <a name="step-2-verify-the-zone"></a>2. Adım Bölgeyi doğrula

Dosyayı içeri aktardıktan sonra DNS bölgesini doğrulamak için aşağıdaki yöntemlerden birini kullanabilirsiniz:

* Aşağıdaki Azure CLI komutunu kullanarak kayıtları listeleyebilirsiniz:

    ```azurecli
    az network dns record-set list -g myresourcegroup -z contoso.com
    ```

* Azure CLI komutunu `az network dns record-set ns list`kullanarak kayıtları listeleyebilirsiniz.
* Kayıtların `nslookup` ad çözümlemesi doğrulamak için kullanabilirsiniz. Bölge henüz devredilmediklerinden, doğru Azure DNS ad sunucularını açıkça belirtmeniz gerekir. Aşağıdaki örnek, bölgeye atanan ad sunucusu adlarının nasıl alınır gösteriş gösterir. Bu da kullanarak `nslookup`"www" kaydısorgulamak için nasıl gösterir.

    ```azurecli
    az network dns record-set ns list -g myresourcegroup -z contoso.com  --output json 
    ```

    ```json
    [
      {
       .......
       "name": "@",
        "nsRecords": [
          {
            "additionalProperties": {},
            "nsdname": "ns1-03.azure-dns.com."
          },
          {
            "additionalProperties": {},
            "nsdname": "ns2-03.azure-dns.net."
          },
          {
            "additionalProperties": {},
            "nsdname": "ns3-03.azure-dns.org."
          },
          {
            "additionalProperties": {},
            "nsdname": "ns4-03.azure-dns.info."
          }
        ],
        "resourceGroup": "myresourcegroup",
        "ttl": 86400,
        "type": "Microsoft.Network/dnszones/NS"
      }
    ]
    ```

    ```cmd
    nslookup www.contoso.com ns1-03.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221
    ```

### <a name="step-3-update-dns-delegation"></a>3. Adım DNS delegasyonuna güncelleştir

Bölgenin doğru şekilde alındığını doğruladıktan sonra, Azure DNS ad sunucularını işaret edecek şekilde DNS delegasyonuna güncelleştirmeniz gerekir. Daha fazla bilgi için makaleye bakın [DNS delegasyonu güncelleştirin.](dns-domain-delegation.md)

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Azure DNS'den bir DNS bölge dosyası dışa aktarma

Bir DNS bölgesini dışa aktarmak için Azure CLI komutunun biçimi:

```azurecli
az network dns zone export -g <resource group> -n <zone name> -f <zone file name>
```

Değerler:

* `<resource group>`Azure DNS'deki bölge için kaynak grubunun adıdır.
* `<zone name>`bölgenin adıdır.
* `<zone file name>`dışa aktar edilecek bölge dosyasının yolu/adıdır.

Bölge alma işleminde olduğu gibi, önce oturum açmanız, aboneliğinizi seçmeniz ve Kaynak Yöneticisi modunu kullanacak şekilde Azure CLI'yi yapılandırmanız gerekir.

### <a name="to-export-a-zone-file"></a>Bölge dosyasını dışa aktarmak için

Kaynak **grubu myresource group'taki** varolan Azure DNS **bölgesini** **contoso.com.txt** dosyasına `azure network dns zone export`(geçerli klasörde) contoso.com dışa aktarmak için çalıştırın. Bu komut, Azure DNS hizmetini, bölgede bulunan kayıt kümelerini sayısala dizmek ve sonuçları BIND uyumlu bir bölge dosyasına aktarmak için çağırır.

```azurecli
az network dns zone export -g myresourcegroup -n contoso.com -f contoso.com.txt
```

## <a name="next-steps"></a>Sonraki adımlar

* DNS bölgenizdeki [kayıt kümelerini ve kayıtları](dns-getstarted-create-recordset-cli.md) nasıl yöneteceklerini öğrenin.

* [Etki alanınızı Azure DNS'ye nasıl devredin](dns-domain-delegation.md)öğrenin.
