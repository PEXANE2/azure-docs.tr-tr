---
title: Kişisel bilgiler için adlandırılmış varlıklar
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 02/17/2021
ms.author: aahi
ms.openlocfilehash: 97167485dae155670f0eb83fc3ef9cb658952251
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750561"
---
> [!NOTE]
> Korumalı sistem durumu bilgilerini (FI) algılamak için, `domain=phi` parametresini ve model sürümünü `2020-04-01` veya üstünü kullanın.
>
> Örnek: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.3/entities/recognition/pii?domain=phi&model-version=2021-01-15`
 
Uç noktaya istek gönderdiğinizde aşağıdaki varlık kategorileri döndürülür `/v3.1-preview.3/entities/recognition/pii` .


| Kategori   |  Açıklama                          |
|------------|-------------|
| [Kişi](#category-person)      |  Kişilerin adları.  |
| [Kişilik türü](#category-persontype) | Bir kişi tarafından tutulan iş türleri veya roller. |
| [Telefon numarası](#category-phonenumber) |Telefon numaraları (yalnızca ABD ve AB telefon numaraları). |
| [Kuruluş](#category-organization) |  Şirketler, gruplar, kamu gövdeleri ve diğer kuruluşlar.  |
| [Adres](#category-address) | Tam posta adresleri.  |
| [E-posta](#category-email) | E-posta adresleri.   |
| [URL](#category-url) | Web sitelerinin URL 'Leri.  |
| [IP](#category-ip) | Ağ IP adresleri.  |
| [Tarih Saat](#category-datetime) | Günün tarihleri ve saatleri. | 
| [Miktar](#category-quantity) | Sayılar ve sayısal miktarlar.  |
| [Azure bilgileri](#azure-information) | Kimlik doğrulama bilgileri gibi tanımlanabilir Azure bilgileri.  |
| [Kimlik](#identification) | Mali ve ülkeye özgü tanımlama.  |

### <a name="category-person"></a>Kategori: kişi

Bu kategori aşağıdaki varlığı içerir:

:::row:::
    :::column span="":::
        **Varlık**

        Kişi

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        Kişilerin adları. İle de döndürülür `domain=phi` .
      
    :::column-end:::
:::row-end:::

### <a name="category-persontype"></a>Kategori: PersonType

Bu kategori aşağıdaki varlığı içerir:


:::row:::
    :::column span="":::
        **Varlık**

        Kişilik türü

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        Bir kişi tarafından tutulan iş türleri veya roller.
      
    :::column-end:::
:::row-end:::

### <a name="category-phonenumber"></a>Kategori: PhoneNumber

Bu kategori aşağıdaki varlığı içerir:

:::row:::
    :::column span="":::
        **Varlık**

        PhoneNumber

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        Telefon numaraları (yalnızca ABD ve AB telefon numaraları). İle de döndürülür `domain=phi` .
      
    :::column-end:::
:::row-end:::


### <a name="category-organization"></a>Kategori: kuruluş

Bu kategori aşağıdaki varlığı içerir:

:::row:::
    :::column span="":::
        **Varlık**

        Kuruluş

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        Şirketler, siyatik gruplar, müzik bantları, spor sinek, kamu gövdeleri ve kamu kuruluşları. Bu varlık türünde ülke almallikleri ve dini dahil değildir.
      
    :::column-end:::
:::row-end:::

#### <a name="subcategories"></a>Kategor

Bu kategorideki varlık aşağıdaki alt kategorilere sahip olabilir.

:::row:::
    :::column span="":::
        **Varlık alt kategorisi**

        Birinin

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        Tıbbi şirketler ve gruplar.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Stok alışverişi

    :::column-end:::
    :::column span="2":::

        Kambiyo senedi grupları. 
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Spor

    :::column-end:::
    :::column span="2":::

        Spor ile ilgili kuruluşlar.
      
    :::column-end:::

:::row-end:::


### <a name="category-address"></a>Kategori: adres

Bu kategori aşağıdaki varlığı içerir:

:::row:::
    :::column span="":::
        **Varlık**

        Adres

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        Tam posta adresi.
      
    :::column-end:::
:::row-end:::

### <a name="category-email"></a>Kategori: e-posta

Bu kategori aşağıdaki varlığı içerir:

:::row:::
    :::column span="":::
        **Varlık**

        E-posta

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        E-posta adresleri.
      
    :::column-end:::
:::row-end:::

### <a name="category-url"></a>Kategori: URL

Bu kategori aşağıdaki varlığı içerir:

:::row:::
    :::column span="":::
        **Varlık**

        URL

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        Web sitelerinin URL 'Leri. 
      
    :::column-end:::
:::row-end:::

### <a name="category-ip"></a>Kategori: IP

Bu kategori aşağıdaki varlığı içerir:

:::row:::
    :::column span="":::
        **Varlık**

        IP

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        ağ IP adresleri. 
      
    :::column-end:::
:::row-end:::

### <a name="category-datetime"></a>Kategori: DateTime

Bu kategori aşağıdaki varlıkları içerir:

:::row:::
    :::column span="":::
        **Varlık**

        DateTime

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        Günün tarihleri ve saatleri. 
      
    :::column-end:::
:::row-end:::

#### <a name="subcategories"></a>Kategor

Bu kategorideki varlık aşağıdaki alt kategorilere sahip olabilir.

:::row:::
    :::column span="":::
        **Varlık alt kategorisi**

        Tarih

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        Takvim tarihleri.
      
    :::column-end:::
:::row-end:::

### <a name="category-quantity"></a>Kategori: miktar

Bu kategori aşağıdaki varlıkları içerir:

:::row:::
    :::column span="":::
        **Varlık**

        Miktar

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        Sayılar ve sayısal miktarlar.
      
    :::column-end:::
:::row-end:::

#### <a name="subcategories"></a>Kategor

Bu kategorideki varlık aşağıdaki alt kategorilere sahip olabilir.

:::row:::
    :::column span="":::
        **Varlık alt kategorisi**

        Yaş

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        Geçirir.
      
    :::column-end:::
:::row-end:::

### <a name="azure-information"></a>Azure bilgileri

Bu varlık kategorileri, kimlik doğrulama bilgileri ve bağlantı dizeleri dahil olmak üzere tanınabilir Azure bilgilerini içerir. `domain=phi`Parametresiyle döndürülmedi.

:::row:::
    :::column span="":::
        **Varlık**

        Azure DocumentDB kimlik doğrulama anahtarı 

    :::column-end:::
    :::column span="2":::
        **Ayrıntılar**

        Azure Cosmos DB sunucusu için yetkilendirme anahtarı.   
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Azure ıAAS veritabanı bağlantı dizesi ve Azure SQL bağlantı dizesi

    :::column-end:::
    :::column span="2":::

        Bir Azure hizmet olarak altyapı (IaaS) veritabanı ve SQL bağlantı dizesi için bağlantı dizesi.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Azure SQL bağlantı dizesi

    :::column-end:::
    :::column span="2":::

        Azure SQL veritabanı 'nda bir veritabanı için bağlantı dizesi.
      
    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Azure IoT bağlantı dizesi  

    :::column-end:::
    :::column span="2":::

        Azure IoT için bağlantı dizesi. 
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Azure yayımlama ayarı parolası  

    :::column-end:::
    :::column span="2":::

        Azure yayımlama ayarları için parola.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Azure Redis Cache bağlantı dizesi 

    :::column-end:::
    :::column span="2":::

        Redsıs önbelleği için bağlantı dizesi.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Azure SAS 

    :::column-end:::
    :::column span="2":::

        Azure hizmet olarak yazılım (SaaS) için bağlantı dizesi.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Azure Service Bus bağlantı dizesi

    :::column-end:::
    :::column span="2":::

        Azure Service Bus için bağlantı dizesi.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Azure depolama hesabı anahtarı 

    :::column-end:::
    :::column span="2":::

       Bir Azure depolama hesabı için hesap anahtarı. 
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Azure depolama hesabı anahtarı (genel)

    :::column-end:::
    :::column span="2":::

       Bir Azure depolama hesabı için genel hesap anahtarı.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        SQL Server bağlantı dizesi 

    :::column-end:::
    :::column span="2":::

       SQL Server çalıştıran bir bilgisayar için bağlantı dizesi.
      
    :::column-end:::
:::row-end:::

### <a name="identification"></a>Kimlik

[!INCLUDE [supported identification entities](./identification-entities.md)]
