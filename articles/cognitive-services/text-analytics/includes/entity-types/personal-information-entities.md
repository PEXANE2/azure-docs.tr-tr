---
title: Kişisel bilgiler için adlandırılmış varlıklar
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/30/2020
ms.author: aahi
ms.openlocfilehash: dd7a8b94aefbf389afef30b327ffaa367a30dd51
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86108523"
---
> [!NOTE]
> Algılamak için `PHI` (korunan sistem durumu bilgilerini), `domain=phi` parametresini ve model sürümünü `2020-04-01` veya üstünü kullanın.
>
> Örneğin, `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.1/entities/recognition/pii?domain=phi&model-version=2020-04-01`
 
Uç noktaya istek gönderilirken aşağıdaki varlık kategorileri döndürülür `/v3.1-preview.1/entities/recognition/pii` .

| Kategori   | Subcategory | Description                          | Model sürümü başlatılıyor | Notlar |
|------------|-------------|--------------------------------------|------------------------|---|
| Kişi     | YOK         | Kişilerin adları.  | `2019-10-01`  | İle de döndürülür `domain=phi` . |
| Kişilik türü | YOK         | Bir kişi tarafından tutulan iş türleri veya roller. | `2020-02-01` | |
| PhoneNumber | YOK | Telefon numaraları (yalnızca ABD ve AB telefon numaraları). | `2019-10-01` | İle de döndürüldü`domain=phi` |
|Kuruluş  | YOK | Şirketler, siyatik gruplar, müzik bantları, spor sinek, kamu gövdeleri ve kamu kuruluşları.  | `2019-10-01` | Bu varlık türünde ülke almallikleri ve dini dahil değildir.  |
|Kuruluş | Birinin | Tıbbi şirketler ve gruplar. | `2020-04-01` | İle de döndürülür `domain=phi` . |
|Kuruluş | Stok alışverişi | Kambiyo senedi grupları. | `2020-04-01` | İle de döndürülür `domain=phi` . |
| Kuruluş | Spor | Spor ile ilgili kuruluşlar. | `2020-04-01` | İle de döndürülür `domain=phi` . |
| Adres | YOK | Tam posta adresleri.  | `2020-04-01` | İle de döndürülür `domain=phi` . |
| AB GPS koordinatları | YOK | Avrupa Birliği içindeki konumların GPS koordinatları.  | `2019-10-01` |  |
| E-posta | YOK | E-posta adresleri. | `2019-10-01` | İle de döndürülür `domain=phi` .   |
| URL | YOK | Web sitelerinin URL 'Leri. | `2019-10-01` | İle de döndürülür `domain=phi` . |
| IP | YOK | Ağ IP adresleri. | `2019-10-01` | |
| DateTime | YOK | Günün tarihleri ve saatleri. | `2019-10-01` |  | 
| DateTime | Tarih | Takvim tarihleri. | `2019-10-01` | İle de döndürülür `domain=phi` . |
| Miktar | YOK | Sayılar ve sayısal miktarlar. | `2019-10-01` |  |
| Miktar | Yaş | Geçirir. | `2019-10-01` | | |
| Derinlemesine bir uluslararası sınıflandırma (ICD-9-CM) | YOK | Uluslararası sınıflandırmayla ilgili varlıklar, dokuzuncu düzeltme.   | `2020-04-01` | |
| Derinlemesine bir uluslararası sınıflandırma (ICD-10-CM) | YOK | Uluslararası sınıflandırmayla ilgili varlıklar, onuncu düzeltme.    | `2020-04-01` | |

## <a name="azure-information"></a>Azure bilgileri

Bu varlık kategorisi, kimlik doğrulama bilgileri ve bağlantı dizeleri dahil olmak üzere tanımlanabilir Azure bilgilerini içerir. Model sürümünden itibaren kullanılabilir `2019-10-01` . `domain=phi`Parametresiyle döndürülmedi.

| Subcategory                           | Description                                                                 |
|---------------------------------------|-----------------------------------------------------------------------------|
| Azure DocumentDB kimlik doğrulama anahtarı             | Azure DocumentDB sunucusu için yetkilendirme anahtarı.                           |
| Azure ıAAS veritabanı bağlantı dizesi ve Azure SQL bağlantı dizesi | Bir Azure hizmet olarak altyapı (IaaS) veritabanı ve SQL bağlantı dizesi için bağlantı dizesi. |
| Azure SQL bağlantı dizesi           | Azure SQL veritabanı 'nda bir veritabanı için bağlantı dizesi.                                |
| Azure IoT bağlantı dizesi           | Azure nesnelerin Interneti (IoT) için bağlantı dizesi.                        |
| Azure yayımlama ayarı parolası        | Azure yayımlama ayarları için parola.                                        |
| Azure Redis Cache bağlantı dizesi   | Redsıs için bir Azure önbelleği bağlantı dizesi.                             |
| Azure SAS                             | Hizmet olarak Azure yazılım (SAS) için bağlantı dizesi.                     |
| Azure Service Bus bağlantı dizesi   | Azure Service Bus için bağlantı dizesi.                                 |
| Azure depolama hesabı anahtarı             | Bir Azure depolama hesabı için hesap anahtarı.                                   |
| Azure depolama hesabı anahtarı (genel)   | Bir Azure depolama hesabı için genel hesap anahtarı.                           |
| SQL Server bağlantı dizesi          | Bir SQL Server için bağlantı dizesi.                                         |

## <a name="identification"></a>Kimlik

[!INCLUDE [supported identification entities](./identification-entities.md)]
