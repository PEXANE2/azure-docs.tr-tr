---
title: Azure Medya Hizmetleri hata kodları | Microsoft Dokümanlar
description: Medya Hizmetlerinde desteklenmeyen eylemlere zaman aşımına uğrarsa kimlik doğrulama belirteçleri gibi sorunlara bağlı olarak hizmetten HTTP hata kodları alabilirsiniz. Bu makalede, Azure Medya Hizmetleri v2 API hata kodlarına genel bir bakış sunulur.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: d3a62a64-7608-4b17-8667-479b26ba0d6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: f5a2dd68d86a7a38fc7f2942351c42c84742d104
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74887097"
---
# <a name="azure-media-services-error-codes"></a>Azure Medya Hizmetleri hata kodları
Microsoft Azure Medya Hizmetleri'ni kullanırken, Medya Hizmetleri'nde desteklenmeyen eylemlere zaman aşımı veren kimlik doğrulama belirteçleri gibi sorunlara bağlı olarak hizmetten HTTP hata kodları alabilirsiniz. Aşağıda, Medya Hizmetleri tarafından döndürülebilecek **HTTP hata kodlarının** listesi ve bunların olası nedenleri yer almaktadır.  

## <a name="400-bad-request"></a>400 Kötü İstek
İstek geçersiz bilgiler içerir ve aşağıdaki nedenlerden biri nedeniyle reddedilir:

* Desteklenmeyen bir API sürümü belirtilir. En güncel sürüm [için, Medya Hizmetleri REST API Geliştirme için Kurulum'a](media-services-rest-how-to-use.md)bakın.
* Medya Hizmetleri'nin API sürümü belirtilmemiştir. API sürümünü nasıl belirteceğimiz hakkında bilgi için [Medya Hizmetleri İşlemleri REST API Başvurusu'na](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)bakın.
  
  > [!NOTE]
  > Medya Hizmetlerine bağlanmak için .NET veya Java SDK'larını kullanıyorsanız, Medya Hizmetleri'ne karşı bazı eylemler gerçekleştirmeye çalıştığınızda API sürümü sizin için belirtilir.
  > 
  > 
* Tanımlanmamış bir özellik belirtilmiş. Özellik adı hata iletisindedir. Yalnızca belirli bir varlığın üyesi olan özellikler belirtilebilir. Varlıkların ve özelliklerinin listesi için [Azure Media Services REST API Başvurusu'na](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference) bakın.
* Geçersiz bir özellik değeri belirtildi. Özellik adı hata iletisindedir. Geçerli özellik türleri ve değerleri için önceki bağlantıya bakın.
* Özellik değeri eksik ve gereklidir.
* Belirtilen URL'nin bir bölümü kötü bir değer içerir.
* Bir WriteOnce özelliğini güncelleştirmegirişiminde bulunuldu.
* Birincil Varlık Dosyası belirtilmeyen veya belirlenemeyen bir giriş Kıymeti olan bir İş oluşturma girişiminde bulunuldu.
* Bir SAS Yer Bulucu'su güncelleştirmegirişiminde bulunuldu. SAS bulucuları yalnızca oluşturulabilir veya silinebilir. Akış bulucuları güncellenebilir. Daha fazla bilgi için [Bkz.](https://docs.microsoft.com/rest/api/media/operations/locator)
* Desteklenmeyen bir işlem veya sorgu gönderildi.

## <a name="401-unauthorized"></a>401 Yetkisiz
İstek aşağıdaki nedenlerden biri nedeniyle (yetkilendirilmeden önce) doğrulanamadı:

* Eksik kimlik doğrulama üstbilgisi.
* Kötü kimlik doğrulama üstbilgi değeri.
  * Belirteç süresi doldu. 
  * Belirteç geçersiz bir imza içerir.

## <a name="403-forbidden"></a>403 Yasak
Aşağıdaki nedenlerden biri nedeniyle istek kabul edilmez:

* Medya Hizmetleri hesabı bulunamıyor veya silinemiyor.
* Medya Hizmetleri hesabı devre dışı bırakılır ve istek türü HTTP GET değildir. Hizmet işlemleri de 403 yanıtı döndürecektir.
* Kimlik doğrulama belirteci, kullanıcının kimlik bilgilerini içermez: AccountName ve/veya SubscriptionId. Bu bilgileri, Medya Hizmetleri hesabınıziçin Medya Hizmetleri Kullanıcı Arabirimi uzantısında Azure Yönetim Portalı'nda bulabilirsiniz.
* Kaynağa erişilemiyor.
  
  * Medya Hizmetleri hesabınıziçin kullanılamayan bir MediaProcessor kullanmaya çalışıldı.
  * Medya Hizmetleri tarafından tanımlanan bir İş Şablonu güncelleştirme girişiminde bulunuldu.
  * Başka bir Medya Hizmetleri hesabının Yer Bulucu'su üzerine yazma girişiminde bulunuldu.
  * Başka bir Medya Hizmetleri hesabının ContentKey'inin üzerine yazma girişiminde bulunuldu.
* Kaynak, Medya Hizmetleri hesabı için ulaşılan bir hizmet kotası nedeniyle oluşturulamadı. Hizmet kotaları hakkında daha fazla bilgi için [Kotalar ve Sınırlamalar](media-services-quotas-and-limitations.md)bölümüne bakın.

## <a name="404-not-found"></a>404 Bulunamadı
Aşağıdaki nedenlerden biri nedeniyle kaynakta isteke izin verilmez:

* Var olmayan bir varlığı güncelleştirmek için bir girişimde bulunuldu.
* Var olmayan bir varlığı silme girişiminde bulunuldu.
* Var olmayan bir varlığa bağlantı veren bir varlık oluşturma girişiminde bulunuldu.
* Var olmayan bir varlığı ALMAK için bir girişimde bulunuldu.
* Medya Hizmetleri hesabıyla ilişkili olmayan bir depolama hesabı belirtme girişiminde bulunuldu.  

## <a name="409-conflict"></a>409 Çatışma
Aşağıdaki nedenlerden biri nedeniyle istek kabul edilmez:

* Birden fazla Varlık Dosyası, Varlık içinde belirtilen ada sahiptir.
* Varlık içinde ikinci bir birincil Varlık Dosyası oluşturma girişiminde bulunuldu.
* Zaten kullanılan belirtilen Kimlik ile bir ContentKey oluşturmak için bir girişimde bulunuldu.
* Önceden kullanılan belirtilen Kimlik ile bir Konum belirleyici oluşturmak için bir girişimde bulunuldu.
* Birden fazla IngestManifestFile IngestManifest içinde belirtilen ada sahiptir.
* İkinci bir depolama şifrelemesi ContentKey'i depolama şifrelenmiş Kıymete bağlama girişiminde bulunuldu.
* Aynı ContentKey'i Varlık'a bağlamak için bir girişimde bulunuldu.
* Depolama kapsayıcısı eksik olan veya artık Varlıkla ilişkilendirilmeyen bir Kıymetin yer bulucusu oluşturma girişiminde bulunuldu.
* Halihazırda kullanımda olan 5 yer bulucusu olan bir Varlığın yer bulucusu oluşturma girişiminde bulunuldu. (Azure Depolama, bir depolama kapsayıcısı üzerinde beş paylaşılan erişim ilkesi sınırını zorlar.)
* Bir Varlığın depolama hesabını bir IngestManifestAsset'e bağlamak, üst IngestManifest tarafından kullanılan depolama hesabıyla aynı değildir.  

## <a name="500-internal-server-error"></a>500 İç Sunucu Hatası
İsteğin işlenmesi sırasında, Medya Hizmetleri işlemin devam etmesini engelleyen bir hatayla karşılaşır. Bunun nedeni, aşağıdakilerden biri olabilir:

* Medya Hizmetleri hesabının hizmet kotası bilgileri geçici olarak kullanılamadığından Varlık veya İş oluşturmak başarısız olur.
* Hesabın depolama hesabı bilgileri geçici olarak kullanılamadığından, Varlık veya IngestManifest blob depolama kapsayıcısı oluşturmak başarısız olur.
* Diğer beklenmeyen hata.

## <a name="503-service-unavailable"></a>503 Hizmet Kullanılamıyor
Sunucu şu anda istek alamıyor. Bu hata, hizmete yapılan aşırı isteklerden kaynaklanabilir. Ortam Hizmetleri azaltma mekanizması, hizmete aşırı istekte bulunduran uygulamalar için kaynak kullanımını kısıtlar.

> [!NOTE]
> 503 hatasını alma nedeniniz hakkında daha ayrıntılı bilgi almak için hata iletisini ve hata kodu dizesini denetleyin. Bu hata her zaman azaltma anlamına gelmez.
> 
> 

Olası durum açıklamaları şunlardır:

* "Sunucu meşgul. Bu tür bir isteğin önceki {0} çalıştırmaları saniyeden fazla sürdü."
* "Sunucu meşgul. Saniyedeki {0} isteklerden fazlası azaltılabilir."
* "Sunucu meşgul. Saniyeler {0} içinde {1} isteklerden fazlası azaltılabilir."

Bu hatayı işlemek için üstel geri tefekbaşa yeniden deneme mantığını kullanmanızı öneririz. Bu, ardışık hata yanıtları için yeniden denemeler arasında giderek daha uzun beklemeler kullanmak anlamına gelir.  Daha fazla bilgi için [Geçici Hata İşleme Uygulama Bloğu'na](https://msdn.microsoft.com/library/hh680905.aspx)bakın.

> [!NOTE]
> [.Net için Azure Medya Hizmetleri SDK](https://github.com/Azure/azure-sdk-for-media-services/tree/master)kullanıyorsanız, 503 hatasının yeniden deneme mantığı SDK tarafından uygulanmıştır.  
> 
> 

## <a name="see-also"></a>Ayrıca Bkz.
[Medya Hizmetleri Yönetimi Hata Kodları](https://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

