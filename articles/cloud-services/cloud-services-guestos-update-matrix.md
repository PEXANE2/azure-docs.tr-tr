---
title: En son Azure Konuk İşletim Sistemi Sürümleri | Microsoft Dokümanlar
description: Azure Bulut Hizmetleri Konuk İşletim Sistemi için en son sürüm haberleri ve SDK uyumluluğu.
services: cloud-services
documentationcenter: na
author: raiye
editor: ''
ms.assetid: 6306cafe-1153-44c7-8554-623b03d59a34
ms.service: cloud-services
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 4/2/2020
ms.author: raiye
ms.openlocfilehash: 95c27cd906717d52a232b5ed85eba2b93bc6569d
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618285"
---
# <a name="azure-guest-os-releases-and-sdk-compatibility-matrix"></a>Azure Konuk İşletim Sistemi sürümleri ve SDK uyumluluk matrisi
Bulut Hizmetleri için en son Azure Konuk İşletim Sistemi sürümleri hakkında güncel bilgiler sağlar. Bu bilgiler, Konuk İşletim Sistemi devre dışı bırakılmadan önce yükseltme yolunuzu planlamanıza yardımcı olur. Rollerinizi [Azure Konuk İşletim Sistemi Güncelleme Ayarları'nda][Azure Guest OS Update Settings]açıklandığı gibi *otomatik* Konuk İşletim Sistemi güncelleştirmelerini kullanacak şekilde yapılandırırsanız, bu sayfayı okumanız hayati önem vermez.

> [!IMPORTANT]
> Bu sayfa, Konuk İşletim Sistemi'nin üstünde çalışan Bulut Hizmetleri web'i ve çalışan rolleri için geçerlidir. IaaS Sanal Makineler için **geçerli değildir.**
>
>


> [!TIP]
>  Tüm Konuk İşletim Sistemi değişiklikleri hakkında en zamanında bildirim almak için [Konuk İşletim Sistemi Update RSS Feed'e] abone olun.
>
>

> [!IMPORTANT]
> Azure portalında yalnızca Konuk İşletim Sistemi'nin en son 2 sürümü desteklenir ve kullanılabilir.
>
>

Konuk işletim sisteminizi nasıl güncelleştireceğinden emin değil misiniz? [Şuna][cloud updates] bak.

## <a name="news-updates"></a>Haber güncellemeleri

###### <a name="april-2-2020"></a>**2 Nisan 2020**
March Guest OS yayımladı. 

###### <a name="march-5-2020"></a>**5 Mart 2020**
Şubat Guest OS yayımladı. 

###### <a name="january-24-2020"></a>**24 Ocak 2020**
Ocak Guest OS yayımladı. 

###### <a name="january-8-2020"></a>**8 Ocak 2020**
Aralık Guest OS yayımladı.

###### <a name="december-5-2019"></a>**5 Aralık 2019**
Kasım Guest OS yayımladı.

###### <a name="november-1-2019"></a>**1 Kasım 2019**
Ekim Guest OS yayımladı.

###### <a name="october-7-2019"></a>**Ekim 7, 2019**
Eylül Guest OS yayımladı.

###### <a name="september-4-2019"></a>**Eylül 4, 2019**
Ağustos Guest OS yayımladı.

###### <a name="july-26-2019"></a>**Temmuz 26, 2019**
Temmuz Guest OS yayımladı.

###### <a name="july-8-2019"></a>**Temmuz 8, 2019**
Haziran Guest OS yayımladı.

###### <a name="june-6-2019"></a>**6 Haziran 2019**
Mayıs Guest OS yayımladı.

###### <a name="may-7-2019"></a>**Mayıs 7, 2019**
Nisan Guest OS yayımladı.

###### <a name="march-26-2019"></a>**Mart 26, 2019**
March Guest OS yayımladı.

###### <a name="march-12-2019"></a>**Mart 12, 2019**
Şubat Guest OS yayımladı.

###### <a name="february-5-2019"></a>**Şubat 5, 2019**
Ocak Guest OS yayımladı.

###### <a name="january-24-2019"></a>**24 Ocak 2019**
Family 6 Guest OS (Windows Server 2019) yayımladı.

###### <a name="january-7-2019"></a>**7 Ocak 2019**
Aralık Guest OS yayımladı.

###### <a name="december-14-2018"></a>**14 Aralık 2018**
Kasım Guest OS yayımladı.

###### <a name="november-8-2018"></a>**8 Kasım 2018**
Ekim Guest OS yayımladı.

###### <a name="october-12-2018"></a>**Ekim 12, 2018**
Eylül Guest OS yayımladı.

## <a name="releases"></a>Yayınlar

## <a name="family-6-releases"></a>Aile 6 bültenleri
**Windows Server 2019**

.NET Framework yüklü: 3.5, 4.7.2

> [!NOTE]
> .NET - 3.0 için Windows Azure SDK [buradan][Windows Azure SDK]indirebilirsiniz.
>
>Yükleme adımları:
>1. Lütfen MicrosoftAzureAuthoringTools*.msi'nin eski sürümlerini kaldırın
>2. [.NET için Azure SDK'yı][Windows Azure SDK] yükleyin - 3.0
>3. Makinenizi yeniden başlatın
>4. Yeni bir Bulut Hizmeti projesi oluşturun ve tek bir İşçi Rolü ekleyin
>5. İşletim Sistemi Ailesini 6 ile değiştirin ve bir paket oluşturun
>6. Azure portalını veya Visual Studio'yu kullanarak paketi Azure'a dağıtma
>
>Guest OS Family 6 sürümü, TLS 1.0 ve 1.1'i açıkça devre dışı bırakarak ve belirli bir şifre paketi kümesi ni tanımlayarak TLS 1.2'yi uygular. [Daha fazla]bilgi edinin.


| Yapılandırma dizesi | Sürüm tarihi | Tarihi devre dışı |
| --- | --- | --- |
|  WA-GUEST-OS-6.17_202003-01  |  2 Nisan 2020  |  Mesaj 6.19  |  
|  WA-GUEST-OS-6.16_202002-01  |  5 Mart 2020  |  Mesaj 6.18  |  
|~~WA-GUEST-OS-6.15_202001-01~~|  24 Ocak 2020  |  2 Nisan 2020  |  
|~~WA-GUEST-OS-6.14_201912-01~~| 8 Ocak 2020 | 5 Mart 2020 |  
|~~WA-GUEST-OS-6.13_201911-01~~| 5 Aralık 2019 | 24 Ocak 2020 |  
|~~WA-GUEST-OS-6.12_201910-01~~| 1 Kasım 2019 | 8 Ocak 2020 |  
|~~WA-GUEST-OS-6.11_201909-01~~| Ekim 7, 2019 | 5 Aralık 2019 |  
|~~WA-GUEST-OS-6.10_201908-01~~| Ağustos 4, 2019 | 1 Kasım 2019  |  
|~~WA-GUEST-OS-6.9_201907-0~~|Temmuz 26, 2019 | Ekim 7, 2019 |
|~~WA-GUEST-OS-6.8_201906-01~~|Temmuz 8, 2019 |Ağustos 4, 2019 |
|~~WA-GUEST-OS-6.7_201905-01~~ |6 Haziran 2019 |Temmuz 26, 2019 |
|~~WA-GUEST-OS-6.6_201904-01~~ |Mayıs 7, 2019 |Temmuz 8, 2019 |
|~~WA-GUEST-OS-6.5_201903-01~~ |Mart 26, 2019 |6 Haziran 2019 |
|~~WA-GUEST-OS-6.4_201902-01~~ |12 Mart 2019 |Mayıs 7, 2019 |
|~~WA-GUEST-OS-6.3_201901-01~~ |5 Şubat 2019 |Mart 26, 2019 |
|~~WA-GUEST-OS-6.2_201812-01~~ |24 Ocak 2019 |12 Mart 2019 |
|~~WA-GUEST-OS-6.1_201811-01~~ |24 Ocak 2019 |5 Şubat 2019 |

## <a name="family-5-releases"></a>Aile 5 bültenleri
**Windows Server 2016**

.NET Framework yüklü: 3.5, 4.6

> [!NOTE]
> OS ailesi 5 için RDP parolası en az 10 karakter olmalıdır.
>


| Yapılandırma dizesi | Sürüm tarihi | Tarihi devre dışı |
| --- | --- | --- |
|  WA-GUEST-OS-5.41_202003-01  |  2 Nisan 2020  |  Mesaj 5.43  |  
|  WA-GUEST-OS-5.40_202002-01  |  5 Mart 2020  |  Mesaj 5.42  |  
|~~WA-GUEST-OS-5.39_202001-01~~|  24 Ocak 2020  |  2 Nisan 2020  |  
|~~WA-GUEST-OS-5.38_201912-01~~| 8 Ocak 2020 | 5 Mart 2020 |  
|~~WA-GUEST-OS-5.37_201911-01~~| 5 Aralık 2019 | 24 Ocak 2020 |  
|~~WA-GUEST-OS-5.36_201910-01~~| 1 Kasım 2019 | 8 Ocak 2020 |  
|~~WA-GUEST-OS-5.35_201909-01~~| Ekim 7, 2019 | 5 Aralık 2019 |  
|~~WA-GUEST-OS-5.34_201908-01~~|  Ağustos 4, 2019  | 1 Kasım 2019 |  
|~~WA-GUEST-OS-5.33_201907-01~~| Temmuz 26, 2019 | Ekim 7, 2019 |  
|~~WA-GUEST-OS-5.32_201906-01~~|Temmuz 8, 2019 |Ağustos 4, 2019 |
|~~WA-GUEST-OS-5.31_201905-01~~ |6 Haziran 2019 |Temmuz 26, 2019 |
|~~WA-GUEST-OS-5.30_201904-01~~ |Mayıs 7, 2019 |Temmuz 8, 2019 |
|~~WA-GUEST-OS-5.29_201903-01~~ |Mart 26, 2019 |6 Haziran 2019 |
|~~WA-GUEST-OS-5.28_201902-01~~ |12 Mart 2019 |Mayıs 7, 2019 |
|~~WA-GUEST-OS-5.27_201901-01~~ |5 Şubat 2019 |Mart 26, 2019 |
|~~WA-GUEST-OS-5.26_201812-01~~ |7 Ocak 2019 |12 Mart 2019 |
|~~WA-GUEST-OS-5.25_201811-01~~ |14 Aralık 2018 |5 Şubat 2019 |
|~~WA-GUEST-OS-5.24_201810-01~~ |8 Kasım 2018 |7 Ocak 2019 |
|~~WA-GUEST-OS-5.23_201809-01~~ |Ekim 12, 2018 |14 Aralık 2018 |

## <a name="family-4-releases"></a>Aile 4 bültenleri
**Windows Server 2012 R2**

.NET Framework yüklü: 3.5, 4.5.1, 4.5.2

| Yapılandırma dizesi | Sürüm tarihi | Tarihi devre dışı |
| --- | --- | --- |
|  WA-GUEST-OS-4.76_202003-01  |  2 Nisan 2020  |  Mesaj 4.78  |  
|  WA-GUEST-OS-4.75_202002-01  |  5 Mart 2020  |  Mesaj 4.77  |  
|~~WA-GUEST-OS-4.74_202001-01~~|  24 Ocak 2020  |  2 Nisan 2020  |  
|~~WA-GUEST-OS-4.73_201912-01~~| 8 Ocak 2020 | 5 Mart 2020 |  
|~~WA-GUEST-OS-4.72_201911-01~~| 5 Aralık 2019 | 24 Ocak 2020 |  
|~~WA-GUEST-OS-4.71_201910-01~~| 1 Kasım 2019 | 8 Ocak 2020 |  
|~~WA-GUEST-OS-4.70_201909-01~~| Ekim 7, 2019 | 5 Aralık 2019 |  
|~~WA-GUEST-OS-4.69_201908-01~~| Ağustos 4, 2019 | 1 Kasım 2019 |  
|~~WA-GUEST-OS-4.68_201907-01~~| Temmuz 26, 2019  | Ekim 7, 2019 |
|~~WA-GUEST-OS-4.67_201906-01~~| Temmuz 8, 2019 |Ağustos 4, 2019 |
|~~WA-GUEST-OS-4.66_201905-01~~ |6 Haziran 2019 |Temmuz 26, 2019 |
|~~WA-GUEST-OS-4.65_201904-01~~ |Mayıs 7, 2019 |Temmuz 8, 2019 |
|~~WA-GUEST-OS-4.64_201903-01~~ |Mart 26, 2019 |6 Haziran 2019 |
|~~WA-GUEST-OS-4.63_201902-01~~ |12 Mart 2019 |Mayıs 7, 2019 |
|~~WA-GUEST-OS-4.62_201901-01~~ |5 Şubat 2019 |Mart 26, 2019 |
|~~WA-GUEST-OS-4.61_201812-01~~ |7 Ocak 2019 |12 Mart 2019 |
|~~WA-GUEST-OS-4.60_201811-01~~ |14 Aralık 2018 |5 Şubat 2019 |
|~~WA-GUEST-OS-4.59_201810-01~~ |8 Kasım 2018 |7 Ocak 2019 |
|~~WA-GUEST-OS-4.58_201809-01~~ |Ekim 12, 2018 |14 Aralık 2018 |

## <a name="family-3-releases"></a>Aile 3 bültenleri
**Windows Server 2012**

.NET Framework yüklü: 3.5, 4.5

| Yapılandırma dizesi | Sürüm tarihi | Tarihi devre dışı |
| --- | --- | --- |
|  WA-GUEST-OS-3.83_202003-01  |  2 Nisan 2020  |  Mesaj 3.85  |  
|  WA-GUEST-OS-3.82_202002-01  |  5 Mart 2020  |  Mesaj 3.84  |  
|~~WA-GUEST-OS-3.81_202001-01~~|  24 Ocak 2020  |  2 Nisan 2020  |  
|~~WA-GUEST-OS-3.80_201912-01~~| 8 Ocak 2020 | 5 Mart 2020 |  
|~~WA-GUEST-OS-3.79_201911-01~~| 5 Aralık 2019 | 24 Ocak 2020 |  
|~~WA-GUEST-OS-3.78_201910-01~~| 1 Kasım 2019 | 8 Ocak 2020 |  
|~~WA-GUEST-OS-3.77_201909-01~~| Ekim 7, 2019 | 5 Aralık 2019 |  
|~~WA-GUEST-OS-3.76_201908-01~~|  Ağustos 4, 2019  |  1 Kasım 2019  |  
|~~WA-GUEST-OS-3.75_201907-01~~| Temmuz 26, 2019 | Ekim 7, 2019 |
|~~WA-GUEST-OS-3.74_201906-01~~| Temmuz 8, 2019 |Ağustos 4, 2019 |
|~~WA-GUEST-OS-3.73_201905-01~~ |6 Haziran 2019 |Temmuz 26, 2019 |
|~~WA-GUEST-OS-3.72_201904-01~~ |Mayıs 7, 2019 |Temmuz 8, 2019 |
|~~WA-GUEST-OS-3.71_201903-01~~ |Mart 26, 2019 |6 Haziran 2019 |
|~~WA-GUEST-OS-3.70_201902-01~~ |12 Mart 2019 |Mayıs 7, 2019 |
|~~WA-GUEST-OS-3.69_201901-01~~ |5 Şubat 2019 |Mart 26, 2019 |
|~~WA-GUEST-OS-3.68_201812-01~~ |7 Ocak 2019 |12 Mart 2019 |
|~~WA-GUEST-OS-3.67_201811-01~~ |14 Aralık 2018 |5 Şubat 2019 |
|~~WA-GUEST-OS-3.66_201810-01~~ |8 Kasım 2018 |7 Ocak 2019 |
|~~WA-GUEST-OS-3.65_201809-01~~ |Ekim 12, 2018 |14 Aralık 2018 |

## <a name="family-2-releases"></a>Aile 2 bültenleri
**Windows Server 2008 R2 SP1**

.NET Framework yüklü: 3.5 (2.0 ve 3.0 içerir), 4.5

| Yapılandırma dizesi | Sürüm tarihi | Tarihi devre dışı |
| --- | --- | --- |
|  WA-GUEST-OS-2.96_202003-01  |  2 Nisan 2020  |  Mesaj 2.98  |  
|  WA-GUEST-OS-2.95_202002-01  |  5 Mart 2020  |  Mesaj 2.97  |  
|~~WA-GUEST-OS-2.94_202001-01~~|  24 Ocak 2020  |  2 Nisan 2020  |  
|~~WA-GUEST-OS-2.93_201912-01~~| 8 Ocak 2020 | 5 Mart 2020 |  
|~~WA-GUEST-OS-2.92_201911-01~~| 5 Aralık 2019 | 24 Ocak 2020 |  
|~~WA-GUEST-OS-2.91_201910-01~~| 1 Kasım 2019 | 8 Ocak 2020 |  
|~~WA-GUEST-OS-2.90_201909-01~~| Ekim 7, 2019 | 5 Aralık 2019 |  
|~~WA-GUEST-OS-2.89_201908-01~~| Ağustos 4, 2019 | 1 Kasım 2019 |  
|~~WA-GUEST-OS-2.88_201907-01~~| Temmuz 26, 2019 | Ekim 7, 2019 |
|~~WA-GUEST-OS-2.87_201906-01~~|Temmuz 8, 2019 | Ağustos 4, 2019 |
|~~WA-GUEST-OS-2.86_201905-01~~ |6 Haziran 2019 |Temmuz 26, 2019 |
|~~WA-GUEST-OS-2.85_201904-01~~ |Mayıs 7, 2019 |Temmuz 8, 2019 |
|~~WA-GUEST-OS-2.84_201903-01~~ |Mart 26, 2019 |6 Haziran 2019 |
|~~WA-GUEST-OS-2.83_201902-01~~ |12 Mart 2019 |Mayıs 7, 2019 |
|~~WA-GUEST-OS-2.82_201901-01~~ |5 Şubat 2019 |Mart 26, 2019 |
|~~WA-GUEST-OS-2.81_201812-01~~ |7 Ocak 2019 |12 Mart 2019 |
|~~WA-GUEST-OS-2.80_201811-01~~ |14 Aralık 2018 |5 Şubat 2019 |
|~~WA-GUEST-OS-2.79_201810-01~~ |8 Kasım 2018 |7 Ocak 2019 |
|~~WA-GUEST-OS-2.78_201809-01~~ |Ekim 12, 2018 |14 Aralık 2018 |

## <a name="msrc-patch-updates"></a>MSRC yama güncellemeleri
Her aylık Konuk İşletim Sistemi sürümüne dahil olan yamalar [listesine buradan][patches]ulaşabilirsiniz.

## <a name="sdk-support"></a>SDK desteği
[Azure SDK'nın emeklilik ilkesi][retire policy sdk] yalnızca 2,2'nin üzerindeki sürümlerin desteklenmediğini belirtse de, belirli Konuk İşletim Sistemi aileleri önceki sürümleri kullanmanıza olanak tanır. Her zaman en son desteklenen SDK kullanmalısınız.

| Konuk İşletim Sistemi ailesi | Uyumlu SDK sürümleri |
| --- | --- |
| 6 |Sürüm 2.9.6+ |
| 5 |Sürüm 2.9.5.1+ |
| 4 |Sürüm 2.1+ |
| 3 |Sürüm 1.8+ |
| 2 |Sürüm 1.3+ |
| 1 |Sürüm 1.0+ |

## <a name="guest-os-release-information"></a>Konuk İşletim Sistemi sürüm bilgileri
Konuk İşletim Sistemi sürümleri için önemli olan üç tarih vardır: **çıkış** tarihi, **devre dışı bırakma** tarihi ve son **kullanma** tarihi. Konuk İşletim Sistemi, Portal'da olduğunda kullanılabilir olarak kabul edilir ve hedef Konuk İşletim Sistemi olarak seçilebilir. Konuk İşletim Sistemi **devre dışı bırakıldığı** tarihe ulaştığında Azure'dan kaldırılır. Ancak, Konuk İşletim Sistemi'ni hedefleyen herhangi bir Bulut Hizmeti yine de normal olarak çalışır.

Devre dışı **bırakılan** tarih ile **son kullanma** tarihi arasındaki pencere, bir Konuk Işletim Sistemi'nden yeni bir işletim sistemi için kolayca geçiş yapmak için bir arabellek sağlar. Otomatik *olarak* Misafir işletim sisteminizi kullanıyorsanız, her zaman en son sürümde olacaksınız ve süresi nin dolması konusunda endişelenmenize gerek kalmaz.

Son **kullanma** tarihi geçtiğinde, konuk işletim sistemi kullanan herhangi bir Bulut Hizmeti durdurulur, silinir veya yükseltmeye zorlanacaktır. [Burada][retirepolicy]emeklilik politikası hakkında daha fazla bilgi edinebilirsiniz.

## <a name="guest-os-family-version-explanation"></a>Konuk İşletim Sistemi aile sürümü açıklaması
Konuk İşletim Sistemi aileleri, Microsoft Windows Server'ın yayımlanmış sürümlerini temel almaktadır. Konuk İşletim Sistemi, Azure Bulut Hizmetleri'nin çalıştığı temel işletim sistemidir. Her Konuk İşletim Sistemi'nin bir ailesi, sürümü ve yayın numarası vardır.

* **Konuk İşletim Sistemi ailesi**  
  Guest OS'nin temel alan bir Windows Server işletim sistemi sürümü. Örneğin, *aile 3* Windows Server 2012'yi temel almaktadır.
* **Konuk İşletim Sistemi sürümü**  
  Konuk İşletim Sistemi aile resmine ve yeni Konuk İşletim Sistemi sürümünün üretildiği tarihte kullanılabilen ilgili [Microsoft Güvenlik Yanıt Merkezi (MSRC)][msrc] düzeltme emaları'na özeldir. Tüm düzeltme emaları dahil edilebilir.

    Sayılar 0'dan başlar ve her yeni güncelleştirme kümesi ekleninde 1 artış. Sondaki sıfırlar yalnızca önemliyse gösterilir. Yani, sürüm 2.10 sürüm 2.1 daha farklı, çok daha sonra sürümüdür.
* **Konuk İşletim Sistemi sürümü**  
  Guest OS sürümünün yeniden yayımı. Microsoft sınama sırasında sorunlar bulursa yeniden yayımlar oluşur; değişiklik gerektiren. En son sürüm her zaman önceki sürümleri, kamu ya da yerini. Azure portalı, kullanıcıların yalnızca belirli bir sürüm için en son sürümü seçmelerine olanak tanır. Önceki sürümde çalışan dağıtımlar genellikle hatanın önem derecesine bağlı olarak yükseltilen zorlama değildir.

Aşağıdaki örnekte, 2 aile, 12 sürüm ve "rel2" serbest olduğunu.

**Konuk Işletim Sistemi sürümü** - 2.12 rel2

**Bu sürüm için yapılandırma dizesi** - WA-GUEST-OS-2.12_201208-02

Konuk Işletim Sistemi için yapılandırma dizesi, bu sürüm için hangi MSRC düzeltme ekinin dikkate alındığını gösteren bir tarihle birlikte aynı bilgilere sahiptir. Bu örnekte, Windows Server 2008 R2 için Ağustos 2012'ye kadar üretilen MSRC yamaları dahil edilmeye çalışıldı. Yalnızca Windows Server'ın bu sürümüne özel olarak uygulanan düzeltme emaları dahildir. Örneğin, bir MSRC düzeltme eki Microsoft Office'e uygulanıyorsa, bu ürün Windows Server temel görüntüsünün bir parçası olmadığından dahil edilmez.

## <a name="guest-os-system-update-process"></a>Konuk İşletim Sistemi sistem güncelleme süreci
Bu sayfa, yaklaşan Konuk İşletim Sistemi Sürümleri hakkında bilgi içerir. Müşteriler, bulut hizmeti rollerinin "Otomatik" güncelleştirme olarak ayarlandığında yeniden başlatılacakları için bir sürümün ne zaman oluştuğunu bilmek istediklerini belirttiler. Konuk İşletim Sistemi sürümleri genellikle her ayın ikinci Salı günü gerçekleşen MSRC güncelleştirme sürümünden 2-3 hafta sonra gerçekleşir. Yeni sürümler, her Konuk İşletim Sistemi ailesi için ilgili tüm MSRC yamaları içerir.

Microsoft Azure sürekli güncelleştirmeler yayımlamaktadır. Guest OS, ardışık hatlar üzerinde bu tür bir güncelleştirmeden yalnızca biridir. Bir sürüm birçok faktörden burada listelemek için çok sayıda etkilenebilir. Buna ek olarak, Azure kelimenin tam anlamıyla yüz binlerce makineüzerinde çalışır. Bu, rolünüzün yeniden başlatılacak ları kesin bir tarih ve saat vermenin imkansız olduğu anlamına gelir. Biz sınırlamak veya zaman yeniden başlatmayı sınırlamak için bir plan üzerinde çalışıyoruz.

Konuk İşletim Sistemi'nin yeni bir sürümü yayımlandığında, Azure genelinde tam olarak yayılması zaman alabilir. Hizmetler yeni Konuk İşletim Sistemi'ne güncelleştirildikçe, güncelleştirme etki alanlarını onurlandırarak yeniden başlatılırlar. "Otomatik" güncelleştirmeleri kullanmak üzere ayarlanmış hizmetler ilk olarak bir sürüm alır. Güncelleştirmeden sonra, Azure portalında hizmetiniz için listelenen yeni Konuk İşletim Sistemi sürümünü görürsünüz. Bu dönemde yeniden yayımlar oluşabilir. Bazı sürümler daha uzun süreler boyunca dağıtılabilir ve otomatik yükseltme yeniden başlatmaları resmi çıkış tarihinden sonraki haftalar boyunca oluşmayabilir. Konuk İşletim Sistemi kullanıma sunulduktan sonra, bu sürümü portaldan veya yapılandırma dosyanızdan açıkça seçebilirsiniz.

Konuk ve Host OS güncellemelerinin daha fazla bilgi teknik ayrıntıları için yeniden başlatmalar ve işaretçiler hakkında çok sayıda değerli bilgi için, [Os Yükseltmeleri Nedeniyle Rol Örneği Yeniden Başlatma][restarts]başlıklı MSDN blog gönderisine bakın.

Konuk İşletim Sisteminizi el ile güncellerseniz, ek bilgi için [Konuk İşletim Sistemi emeklilik politikasına][retirepolicy] bakın.

## <a name="guest-os-supportability-and-retirement-policy"></a>Konuk İşletim Sistemi desteklenebilirlik ve emeklilik politikası
Konuk Işletim Sistemi desteklenebilirlik ve emeklilik politikası [burada][retirepolicy]açıklanmıştır.

[cloud updates]: https://docs.microsoft.com/azure/cloud-services/cloud-services-update-azure-service
[Konuk İşletim Sistemi RSS Feed güncelleme]: https://raw.githubusercontent.com/MicrosoftDocs/azure-cloud-services-files/master/GuestOS/GuestOSFeed.xml
[Install .NET on a Cloud Service Role]: https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-install-dotnet/?WT.mc_id=azurebg_email_Trans_963_RevisedNET_Update
[Azure Guest OS Update Settings]: cloud-services-how-to-configure-portal.md
[ssl3 announcement]: https://azure.microsoft.com/blog/2014/12/09/azure-security-ssl-3-0-update/
[Microsoft Security Advisory 3009008]: https://technet.microsoft.com/library/security/3009008.aspx
[ssl3-fixit]: https://go.microsoft.com/?linkid=9863266
[MS14-066]: https://technet.microsoft.com/library/security/ms14-066.aspx
[MS14-046]: https://technet.microsoft.com/library/security/ms14-046.aspx
[retire policy sdk]: https://msdn.microsoft.com/library/dn479282.aspx
[server and gos]: https://msdn.microsoft.com/library/dn775043.aspx
[azuresupport]: https://azure.microsoft.com/support/options/
[net install pkg]: https://www.microsoft.com/download/details.aspx?id=42643
[msrc]: https://technet.microsoft.com/security/dn440717.aspx
[update guest os portal]: https://msdn.microsoft.com/library/gg433101.aspx
[update guest os svc]: https://msdn.microsoft.com/library/gg456324.aspx
[restarts]: https://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx
[patches]: cloud-services-guestos-msrc-releases.md
[retirepolicy]: cloud-services-guestos-retirement-policy.md
[fam1retire]: cloud-services-guestos-family1-retirement.md
[fix]: https://technet.microsoft.com/library/security/ms17-010.aspx
[Windows Azure SDK]: https://www.microsoft.com/en-us/download/details.aspx?id=54917
[-nda daha fazla]: https://docs.microsoft.com/azure/cloud-services/applications-dont-support-tls-1-2  
