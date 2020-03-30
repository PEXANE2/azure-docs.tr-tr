---
title: Azure Monitör'de IIS günlükleri | Microsoft Dokümanlar
description: Internet Information Services (IIS), Azure Monitor tarafından toplanabilen günlük dosyalarında kullanıcı etkinliğini depolar.  Bu makalede, IIS günlükleri koleksiyonu ve Azure Monitor'da oluşturdukları kayıtların ayrıntılarını nasıl yapılandırılabildikleri açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/28/2018
ms.openlocfilehash: 1b3ae6295a639c3d59643b106b920cb606572e0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670585"
---
# <a name="collect-iis-logs-in-azure-monitor"></a>Azure Monitör'de IIS günlüklerini topla
Internet Information Services (IIS), kullanıcı etkinliğini Azure Monitor tarafından toplanabilen ve [günlük verileri](data-platform.md)olarak depolanabilen günlük dosyalarında depolar.

![IIS günlükleri](media/data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>IIS günlüklerini yapılandırma
Azure Monitor, IIS tarafından oluşturulan günlük dosyalarından girişleri toplar, bu nedenle [günlüğe kaydetmek için IIS'yi yapılandırmanız](https://technet.microsoft.com/library/hh831775.aspx)gerekir.

Azure Monitor yalnızca W3C biçiminde depolanan IIS günlük dosyalarını destekler ve özel alanları veya IIS Advanced Logging'u desteklemez. NCSA veya IIS yerel biçiminde günlükleri toplamaz.

[Gelişmiş Ayarlar menüsünden](agent-data-sources.md#configuring-data-sources)Azure Monitör'deki IIS günlüklerini yapılandırın.  **W3C formatı IIS günlük dosyalarını topla'yı**seçmek dışında yapılandırma gerekmez.


## <a name="data-collection"></a>Veri toplama
Azure Monitor, günlük zaman damgası her değiştiğinde her aracıdan IIS günlük girişlerini toplar. Günlük her **5 dakikada**bir okunur. Herhangi bir nedenle IIS, yeni bir dosya oluşturulduğu devir zamanından önce zaman damgasını güncelleştirmezse, girişler yeni dosyanın oluşturulmasından sonra toplanır. Yeni dosya oluşturma sıklığı, varsayılan olarak günde bir kez olan IIS sitesi için **Günlük Dosya Rollover Zamanlama** ayarı tarafından denetlenir. Ayar **Saatlik**ise, Azure Monitor günlüğü her saat toplar. Ayar **Günlük**ise, Azure Monitor günlüğü her 24 saatte bir toplar.


## <a name="iis-log-record-properties"></a>IIS günlük kayıt özellikleri
IIS günlük kayıtları bir **W3CIISLog** türüne sahiptir ve aşağıdaki tabloda özelliklere sahiptir:

| Özellik | Açıklama |
|:--- |:--- |
| Bilgisayar |Olayın toplandığı bilgisayarın adı. |
| Cıp |İstemcinin IP adresi. |
| csMethod |GET veya POST gibi istek yöntemi. |
| csReferer |Kullanıcının geçerli siteye bir bağlantı izlediği site. |
| csUserAgent |İstemcinin tarayıcı türü. |
| csUserName |Sunucuya erişen kimliği doğrulanmış kullanıcının adı. Anonim kullanıcılar bir tire işaretiyle gösterilir. |
| csUriStem |Web sayfası gibi isteğin hedefi. |
| csUriQuery |Varsa, istemcinin gerçekleştirmeye çalıştığı sorgu. |
| ManagementGroupName |Operasyon Yöneticisi aracıları için yönetim grubunun adı.  Diğer aracılar için bu AOI-\<çalışma alanı kimliğidir\> |
| RemoteIPCountry |İstemcinin IP adresinin ülke/bölgesi. |
| RemoteIPLatitude |İstemci IP adresinin enlemi. |
| RemoteIPLongitude |İstemci IP adresinin boylam. |
| scDurum |HTTP durum kodu. |
| scSubStatus |Alt durum hata kodu. |
| scWin32Durum |Windows durum kodu. |
| Sıp |Web sunucusunun IP adresi. |
| SourceSystem |Opsmgr |
| Spor |İstemcinin bağlı olduğu sunucuda bağlantı noktası. |
| sSiteAdı |IIS sitesinin adı. |
| TimeGenerated |Girişin günlüğe kaydedildiği tarih ve saat. |
| Zaman Dilimi |İsteğin milisaniye cinsinden işlemesi için gereken süre. |

## <a name="log-queries-with-iis-logs"></a>IIS günlükleriyle günlük sorguları
Aşağıdaki tablo, IIS günlük kayıtlarını alan günlük sorgularının farklı örneklerini sağlar.

| Sorgu | Açıklama |
|:--- |:--- |
| W3CIISLog |Tüm IIS günlük kayıtları. |
| W3CIISLog &#124; nerede scStatus==500 |500 iade durumu olan tüm IIS günlük kayıtları. |
| W3CIISLog &#124; cIP tarafından sayım() özetlemek |İstemci IP adresine göre IIS günlük girişlerinin sayısı. |
| W3CIISLog &#124; nerede csHost\.=="www contoso.com" &#124; csUriStem tarafından sayım özetlemek |Ev sahibi www\.contoso.com için URL'ye göre IIS günlük girişlerinin sayısı. |
| W3CIISLog &#124; 500000 almak &#124; Bilgisayar tarafından toplamı (csBytes) özetlemek |Her IIS bilgisayarı tarafından alınan toplam bayt. |

## <a name="next-steps"></a>Sonraki adımlar
* Azure Monitor'u analiz için diğer [veri kaynaklarını](agent-data-sources.md) toplamak üzere yapılandırın.
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin.
