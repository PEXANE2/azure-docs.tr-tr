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
ms.openlocfilehash: cd17bf6c97cb89984bf407552330fbe9bec36e3f
ms.sourcegitcommit: bf99428d2562a70f42b5a04021dde6ef26c3ec3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85292199"
---
> [!NOTE]
> Algılamak için `PHI` , `domain=phi` parametresini ve model sürümünü `2020-04-01` veya üstünü kullanın.
>
> Örneğin, `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.1/entities/recognition/pii?domain=phi&model-version=2020-04-01`
 
Uç noktaya istek gönderilirken aşağıdaki varlık kategorileri döndürülür `/v3.1-preview.1/entities/recognition/pii` .

| Kategori   | Subcategory | Description                          | Model sürümü başlatılıyor | Notlar |
|------------|-------------|--------------------------------------|------------------------|---|
| Kişi     | Yok         | Kişilerin adları.  | `2019-10-01`  | İle de döndürülür `domain=phi` . |
| Kişilik türü | Yok         | Bir kişi tarafından tutulan iş türleri veya roller. | `2020-02-01` | |
| PhoneNumber | Yok | Telefon numaraları (yalnızca ABD ve AB telefon numaraları). | `2019-10-01` | İle de döndürüldü`domain=phi` |
|Kuruluş  | Yok | Şirketler, siyatik gruplar, müzik bantları, spor sinek, kamu gövdeleri ve kamu kuruluşları.  | `2019-10-01` | Bu varlık türünde ülke almallikleri ve dini dahil değildir.  |
|Kuruluş | Birinin | Tıbbi şirketler ve gruplar. | `2020-04-01` | İle de döndürülür `domain=phi` . |
|Kuruluş | Stok alışverişi | Kambiyo senedi grupları. | `2020-04-01` | İle de döndürülür `domain=phi` . |
| Kuruluş | Spor | Spor ile ilgili kuruluşlar. | `2020-04-01` | İle de döndürülür `domain=phi` . |
| Adres | Yok | Tam posta adresleri.  | `2020-04-01` | İle de döndürülür `domain=phi` . |
| AB GPS koordinatları | Yok | Avrupa Birliği içindeki konumların GPS koordinatları.  | `2019-10-01` |  |
| E-posta | Yok | E-posta adresleri. | `2019-10-01` | İle de döndürülür `domain=phi` .   |
| URL | Yok | Web sitelerinin URL 'Leri. | `2019-10-01` | İle de döndürülür `domain=phi` . |
| IP | Yok | Ağ IP adresleri. | `2019-10-01` | |
| DateTime | Yok | Günün tarihleri ve saatleri. | `2019-10-01` |  | 
| DateTime | Tarih | Takvim tarihleri. | `2019-10-01` | İle de döndürülür `domain=phi` . |
| Miktar | Yok | Sayılar ve sayısal miktarlar. | `2019-10-01` |  |
| Miktar | Yaş | Geçirir. | `2019-10-01` | | |
| Derinlemesine bir uluslararası sınıflandırma (ICD-9-CM) | Yok | Uluslararası sınıflandırmayla ilgili varlıklar, dokuzuncu düzeltme.   | `2020-04-01` | |
| Derinlemesine bir uluslararası sınıflandırma (ICD-10-CM) | Yok | Uluslararası sınıflandırmayla ilgili varlıklar, onuncu düzeltme.    | `2020-04-01` | |

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
