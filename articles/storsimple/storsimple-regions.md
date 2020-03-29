---
title: StorBasit bölge kullanılabilirliği
description: Çeşitli StorSimple aygıt modellerinin kullanılabildiği Azure bölgelerini açıklar.
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 11/16/2017
ms.author: alkohli
ms.openlocfilehash: a2f04d6faa7770eef7768437ae0e624b76713f98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76275120"
---
# <a name="available-regions-for-your-storsimple"></a>StorSimple'ınız için kullanılabilir bölgeler

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]

## <a name="overview"></a>Genel Bakış

Azure veri merkezleri, müşterinin veri konumuyla ilgili performans, gereksinim ve tercihtaleplerini karşılamak için dünyanın dört bir yanındaki birden fazla coğrafyada çalışır. Azure coğrafyası, dünyanın en az bir Azure Bölgesi içeren tanımlı bir alanıdır. Azure bölgesi, bir veya daha fazla veri merkezi içeren bir coğrafya içindeki bir alandır.

Azure bölgesi seçmek çok önemlidir ve bölge seçimi veri ikameti ve egemenlik, hizmet kullanılabilirliği, performans, maliyet ve artıklık gibi etkenlerden etkilenir. Bir bölgeyi nasıl seçeceğiniz hakkında daha fazla bilgi için hangi [Azure bölgesine gidin?](https://azure.microsoft.com/overview/datacenters/how-to-choose/)

StorSimple çözümü için bölge seçimi özellikle aşağıdaki etkenler tarafından belirlenir:

- StorSimple Device Manager hizmetinin kullanılabildiği bölgeler.
- StorSimple fiziksel, bulut veya sanal aygıtın kullanılabildiği ülkeler/bölgeler.
- Depolama Alanı verilerini depolayan depolama hesaplarının en iyi performans için bulunması gereken bölgeler.

Bu öğretici, StorSimple Device Manager hizmeti, şirket içi fiziksel ve bulut aygıtları için bölge kullanılabilirliğini açıklar. Bu makalede yer alan bilgiler StorSimple 8000 ve 1200 serisi cihazlar için geçerlidir.

## <a name="region-availability-for-storsimple-device-manager-service"></a>StorSimple Device Manager hizmeti için bölge durumu

StorSimple Device Manager hizmeti şu anda 12 genel bölgede ve 2 Azure Resmi bölgesinde desteklenir.

StorSimple Device Manager hizmetini ilk oluşturduğunuzda bir bölge veya konum tanımlarsınız. Genel olarak, aygıtın dağıtıldığı coğrafi bölgeye en yakın konum seçilir. Ancak aygıt ve hizmet farklı konumlarda da dağıtılabilir.

Burada, StorSimple Device Manager hizmetinin Azure genel bulutu için kullanılabildiği ve dağıtılabildiği bölgelerin listesi verebilirsiniz.

![storsimple-device-manager-service-regions](./media/storsimple-region/storsimple-device-manager-service-regions.png)

Azure Devlet bulutu için StorSimple Device Manager hizmeti ABD Gov Iowa ve ABD Gov Virginia veri merkezlerinde kullanılabilir.

## <a name="region-availability-for-data-stored-in-storsimple"></a>StorSimple'da depolanan veriler için bölge kullanılabilirliği

StorBasit veriler Azure depolama hesaplarında fiziksel olarak depolanır ve bu hesaplar tüm Azure bölgelerinde kullanılabilir. Bir Azure depolama hesabı oluşturduğunuzda, depolama hesabının birincil konumu seçilir ve bu da verilerin bulunduğu bölgeyi belirler.

Bir StorSimple Device Manager hizmetini ilk oluşturduğunuzda ve bir depolama hesabını onunla ilişkilendirdiğinizde, StorSimple Device Manager hizmetiniz ve Azure depolama alanınız iki ayrı konumda olabilir. Böyle bir durumda, StorSimple Cihaz Yöneticisi ve Azure Storage hesabını ayrı ayrı oluşturmanız gerekir.

Genel olarak, depolama hesabınız için servisinizin en yakın bölgesini seçin. Ancak, en yakın Microsoft Azure bölgesi aslında en düşük gecikme gecikmesi olan bölge olmayabilir. Ağ hizmeti performansını ve dolayısıyla çözümün performansını belirleyen gecikmedir. Bu nedenle, farklı bir bölgede bir depolama hesabı seçiyorsanız, hizmetinizle depolama hesabınızla ilişkili bölge arasındaki gecikmelerin ne olduğunu bilmek önemlidir.

StorSimple Cloud Appliance kullanıyorsanız, hizmetin ve ilişkili depolama hesabının aynı bölgede olduğunu öneririz. Farklı bir bölgedeki depolama hesapları düşük performansa neden olabilir.

## <a name="availability-of-storsimple-device"></a>StorSimple cihazının kullanılabilirliği

Modele bağlı olarak, StorSimple aygıtları farklı coğrafyalarda veya ülkelerde/bölgelerde kullanılabilir.

### <a name="storsimple-physical-device-models-81008600"></a>StorBasit fiziksel cihaz (Modeller 8100/8600)

StorSimple 8100 veya 8600 fiziksel aygıtı kullanıyorsanız, cihaz aşağıdaki ülkelerde/bölgelerde kullanılabilir.

| #  | Ülke/Bölge        | #  | Ülke/Bölge     | #  | Ülke/Bölge      | #  | Ülke/Bölge             |
|----|-----------------------|----|--------------------|----|---------------------|----|----------------------------|
| 1  | Avustralya             | 16 | Hong Kong ÖİB      | 31 | Yeni Zelanda         | 46 | Güney Afrika               |
| 2  | Avusturya               | 17 | Macaristan            | 32 | Nijerya             | 47 | Güney Kore                |
| 3  | Bahreyn               | 18 | İzlanda            | 33 | Norveç              | 48 | İspanya                      |
| 4  | Belçika               | 19 | Hindistan              | 34 | Peru                | 49 | Sri Lanka                  |
| 5  | Brezilya                | 20 | Endonezya          | 35 | Filipinler         | 50 | İsveç                     |
| 6  | Kanada                | 21 | İrlanda            | 36 | Polonya              | 51 | İsviçre                |
| 7  | Şili                 | 22 | İsrail             | 37 | Portekiz            | 52 | Tayvan                     |
| 8  | Kolombiya              | 23 | İtalya              | 38 | Porto Riko         | 53 | Tayland                   |
| 9  | Çek Cumhuriyeti        | 24 | Japonya              | 39 | Katar               | 54 | Türkiye                     |
| 10 | Danimarka               | 25 | Kenya              | 40 | Romanya             | 55 | Ukrayna                    |
| 11 | Mısır                 | 26 | Kuveyt             | 41 | Rusya              | 56 | Birleşik Arap Emirlikleri       |
| 12 | Finlandiya               | 27 | Makao ÖİB          | 42 | Suudi Arabistan        | 57 | Birleşik Krallık             |
| 13 | Fransa                | 28 | Malezya           | 43 | Singapur           | 58 | Amerika Birleşik Devletleri              |
| 14 | Almanya               | 29 | Meksika             | 44 | Slovakya            | 59 | Vietnam                    |
| 15 | Yunanistan                | 30 | Hollanda        | 45 | Slovenya            | 60 | Hırvatistan                    |

Bu liste, daha fazla ülke/bölge eklendikçe değişir. Coğrafyaların en güncel listesi için, [Ürün terimlerinde](https://www.microsoft.com/en-us/licensing/product-licensing/products)Depolama Dizi Terimleri Ekine gidin.

Microsoft, fiziksel donanım ı sevk edebilir ve StorSimple için donanım yedek parça değişimini önceki listedeki coğrafyalara sağlayabilir.

> [!IMPORTANT]
> StorSimple'ın desteklenmediği bir bölgeye StorSimple fiziksel aygıtı yerleştirmeyin. Microsoft, StorSimple'ın desteklenmediği ülkelere/bölgelere yedek parça gönderemez.

### <a name="storsimple-cloud-appliance-models-80108020"></a>StorSimple Bulut Aletleri (Modeller 8010/8020)

StorSimple Cloud Appliance 8010 veya 8020 kullanıyorsanız, aygıt desteklenir ve temel VM'nin desteklendiği tüm bölgelerde kullanılabilir. 8010, tüm Azure bölgelerinde desteklenen _Standard_A3_ bir VM kullanır.

8020, bir bulut cihazı oluşturmak için birinci sınıf depolama alanı ve _Standard_DS3_ VM kullanır. 8020, Premium Depolama'yı destekleyen ve _Standard_DS3_ Azure VM'Standard_DS3 Azure bölgelerinde desteklenir. Bölgenizde hem **Sanal Makineler > DS serisi** hem de **Depolama > Disk depolamanın** mevcut olup olmadığını görmek için [bu listeyi](https://azure.microsoft.com/regions/services/) kullanın.

### <a name="storsimple-virtual-array-model-1200"></a>StorSimple Sanal Dizi (Model 1200)

1200 serisi StorSimple Virtual Array kullanıyorsanız, sanal disk görüntüsü tüm Azure bölgelerinde desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Çeşitli StorSimple modellerinin fiyatlandırması](https://azure.microsoft.com/pricing/calculator/#storsimple2)hakkında daha fazla bilgi edinin.
* [StorSimple depolama hesabınızı yönetme](storsimple-8000-manage-storage-accounts.md)hakkında daha fazla bilgi edinin.
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini](storsimple-8000-manager-service-administration.md)nasıl kullanacağınız hakkında daha fazla bilgi edinin.
