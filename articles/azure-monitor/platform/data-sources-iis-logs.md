---
title: Azure Izleyici 'de IIS günlükleri | Microsoft Docs
description: Internet Information Services (IIS), Azure Izleyici tarafından toplanabilecek günlük dosyalarındaki Kullanıcı etkinliklerini depolar.  Bu makalede, Azure Izleyici 'de oluşturdukları kayıtların IIS günlüklerinin ve ayrıntılarının nasıl yapılandırılacağı açıklanır.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: cec5ff0a-01f5-4262-b2e8-e3db7b7467d2
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2018
ms.author: bwren
ms.openlocfilehash: 05d9dc8f676589dcb301c19b0a2e80e9fd4c1fa0
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249752"
---
# <a name="collect-iis-logs-in-azure-monitor"></a>Azure Izleyici 'de IIS günlükleri toplama
Internet Information Services (IIS), Azure Izleyici tarafından toplanabilecek ve [günlük verileri](data-platform.md)olarak depolanan günlük dosyalarındaki Kullanıcı etkinliklerini depolar.

![IIS günlükleri](media/data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>IIS günlüklerini yapılandırma
Azure Izleyici, IIS tarafından oluşturulan günlük dosyalarından girişler toplar, bu nedenle [IIS 'yi günlüğe kaydetmek için yapılandırmanız](https://technet.microsoft.com/library/hh831775.aspx)gerekir.

Azure Izleyici yalnızca W3C biçiminde depolanan IIS günlük dosyalarını destekler ve özel alanları veya IIS gelişmiş günlüğe kaydetmeyi desteklemez. NCSA veya IIS yerel biçiminde Günlükler toplanmaz.

[Gelişmiş ayarlar menüsünden](agent-data-sources.md#configuring-data-sources)Azure IZLEYICI 'de IIS günlüklerini yapılandırın.  **W3C BIÇIM IIS günlük dosyalarını topla**seçeneğinin belirlenmesi dışında bir yapılandırma gerekmez.


## <a name="data-collection"></a>Veri toplama
Azure Izleyici, günlük zaman damgası her değiştiğinde veya yeni bir dosya oluşturulduğunda her bir aracıdan IIS günlük girişleri toplar. Günlük her 5 dakikada bir okunurdur. Yeni dosya oluşturma sıklığı, varsayılan olarak günde bir kez olan IIS sitesi için **günlük dosyası aktarma zamanlaması** ayarıyla denetlenir. Herhangi bir nedenden dolayı IIS, geçiş zamanından önce zaman damgasını güncelleştirmezse, ayar **saatlik**ise, Azure İzleyici günlüğü her saat toplar. Ayar **günlük**Ise Azure izleyici, günlüğü her 24 saatte bir toplar.


## <a name="iis-log-record-properties"></a>IIS günlük kaydı özellikleri
IIS günlük kayıtları bir tür **W3CIISLog** ve aşağıdaki tabloda bulunan özelliklere sahiptir:

| Özellik | Açıklama |
|:--- |:--- |
| Computer |Olayın toplandığı bilgisayarın adı. |
| CIP |İstemcinin IP adresi. |
| csMethod |İsteğin GET veya POST gibi yöntemi. |
| csReferer |Kullanıcının geçerli siteye bir bağlantıyı takip ettiği site. |
| csUserAgent |İstemcinin tarayıcı türü. |
| csUserName |Sunucuya erişen kimliği doğrulanmış kullanıcının adı. Anonim kullanıcılar bir tire ile belirtilir. |
| csUriStem |İsteğin hedefi bir Web sayfası. |
| csUriQuery |Varsa, istemcisinin gerçekleştirmeye çalıştığı sorgu. |
| ManagementGroupName |Operations Manager aracıları için yönetim grubunun adı.  Diğer aracılar için AOI - budur\<çalışma alanı kimliği\> |
| RemoteIPCountry |İstemcinin IP adresinin ülkesi/bölgesi. |
| RemoteIPLatitude |İstemci IP adresinin enlem. |
| RemoteIPLongitude |İstemci IP adresinin boylam. |
| scStatus |HTTP durum kodu. |
| Scalt durum |Alt durum hata kodu. |
| scWin32Status |Windows durum kodu. |
| Istemcinin |Web sunucusunun IP adresi. |
| SourceSystem |OpsMgr |
| Spor |İstemcinin bağlandığı sunucudaki bağlantı noktası. |
| sSiteName |IIS sitesinin adı. |
| TimeGenerated |Girişin günlüğe kaydedildiği tarih ve saat. |
| TimeTaken |İsteğin milisaniye cinsinden işlenmesi için geçen süre. |

## <a name="log-queries-with-iis-logs"></a>IIS günlükleri ile sorguları günlüğe kaydet
Aşağıdaki tabloda, IIS günlük kayıtlarını alan günlük sorgularının farklı örnekleri verilmiştir.

| Sorgu | Açıklama |
|:--- |:--- |
| W3CIISLog |Tüm IIS günlük kayıtları. |
| W3CIISLog &#124; burada scstatus = = 500 |Tüm IIS günlük kayıtları 500 dönüş durumuna sahiptir. |
| W3CIISLog &#124; özetleme sayısı (), CIP |İstemci IP adresine göre IIS günlük girişi sayısı. |
| W3CIISLog &#124; WHERE cshost = = "www\.contoso.com" &#124; Count () öğesini csUriStem ile özetleme |Host www\.contoso.com için URL 'ye göre IIS günlük girişi sayısı. |
| W3CIISLog &#124; özetleme toplamı (csbytes) bilgisayar &#124; tarafından 500000 al |Her IIS bilgisayarı tarafından alınan toplam bayt sayısı. |

## <a name="next-steps"></a>Sonraki adımlar
* Azure Izleyici 'yi analize yönelik diğer [veri kaynaklarını](agent-data-sources.md) toplayacak şekilde yapılandırın.
* Hakkında bilgi edinin [oturum sorguları](../log-query/log-query-overview.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için.
