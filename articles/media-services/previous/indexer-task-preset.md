---
title: Azure Media Indexer için görev ön kümesi
description: Bu konu, Azure Medya Hizmetleri Medya Dizini için görev ön kümesine genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Asolanki
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: 29753759af341f82429f12b6710ae9c32dcb4103
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74896035"
---
# <a name="task-preset-for-azure-media-indexer"></a>Azure Media Indexer için görev ön kümesi 

Azure Media Indexer, aşağıdaki görevleri gerçekleştirmek için kullandığınız bir Medya İşlemcisidir: medya dosyalarını ve içeriğini aranabilir hale getirme, kapalı altyazı parçaları ve anahtar kelimeler oluşturma, kıymetinizin bir parçası olan varlık dosyalarını dizinleme.

Bu konu, dizin oluşturma işinize geçmeniz gereken görev ön kümesini açıklar. Örneğin, Azure [Media Indexer ile ortam dosyalarını dizine](media-services-index-content.md)ekleme'ye bakın.

## <a name="azure-media-indexer-configuration-xml"></a>Azure Media Indexer YapılandırmaxML

Aşağıdaki tabloda, XML yapılandırmasının öğeleri ve öznitelikleri açıklanmaktadır.

|Adı|Gerektirme|Açıklama|
|---|---|---|
|Giriş|true|Dizinlemek istediğiniz varlık dosyası(lar).<br/>Azure Media Indexer aşağıdaki ortam dosya biçimlerini destekler: MP4, MOV, WMV, MP3, M4A, WMA, AAC, WAV. <br/><br/>**Giriş** öğesinin **adı** veya **liste** özniteliği (aşağıda gösterildiği gibi) dosya adını (ler) belirtebilirsiniz. Hangi varlık dosyasının dizin ekinine belirtmezseniz, birincil dosya seçilir. Birincil kıymet dosyası ayarlanmazsa, giriş kıymetindeki ilk dosya dizine işlenir.<br/><br/>Varlık dosya adını açıkça belirtmek için şunları yapın:<br/>```<input name="TestFile.wmv" />```<br/><br/>Aynı anda birden çok varlık dosyasını (en fazla 10 dosya) dizine ekebilirsiniz. Bunu yapmak için:<br/>- Bir metin dosyası (bildirim dosyası) oluşturun ve bir .lst uzantısı verin.<br/>- Bu bildirim dosyasına giriş kıymetinizdeki tüm varlık dosya adlarının listesini ekleyin.<br/>- Bildirim dosyasını kıymete ekleyin (yükleyin).<br/>- Girişin liste özniteliğinde bildirim dosyasının adını belirtin.<br/>```<input list="input.lst">```<br/><br/>**Not:** Bildirim dosyasına 10'dan fazla dosya eklerseniz, dizin oluşturma işi 2006 hata koduyla başarısız olur.|
|meta veriler|yanlış|Belirtilen varlık dosyası(lar) için meta veriler.<br/>```<metadata key="..." value="..." />```<br/><br/>Önceden tanımlanmış anahtarlar için değerler sağlayabilirsiniz. <br/><br/>Şu anda, aşağıdaki tuşlar desteklenir:<br/><br/>**başlık** ve **açıklama** - konuşma tanıma doğruluğunu artırmak için dil modelini güncelleştirmek için kullanılır.<br/>```<metadata key="title" value="[Title of the media file]" /><metadata key="description" value="[Description of the media file]" />```<br/><br/>**kullanıcı adı** ve **şifre** - http veya https üzerinden internet dosyalarını indirirken kimlik doğrulama için kullanılır.<br/>```<metadata key="username" value="[UserName]" /><metadata key="password" value="[Password]" />```<br/>Kullanıcı adı ve parola değerleri giriş bildirimindeki tüm medya URL'leri için geçerlidir.|
|özellikler<br/><br/>Sürüm 1.2'de eklendi. Şu anda desteklenen tek özellik konuşma tanıma ("ASR").|yanlış|Konuşma Tanıma özelliği aşağıdaki ayarlar ayarı tuşlarına sahiptir:<br/><br/>Dil:<br/>- Multimedya dosyasında tanınacak doğal dil.<br/>- İngilizce, İspanyolca<br/><br/>CaptionFormats:<br/>- istenilen çıktı resim yazısı biçimlerinin yarı sütunlu ayrılmış listesi (varsa)<br/>- ttml;webvtt<br/><br/><br/>Anahtar Kelimeler Oluşturur:<br/>- Bir anahtar kelime XML dosyası gerekli olup olmadığını belirten bir boolean bayrak.<br/>- Doğru; False.|

## <a name="azure-media-indexer-configuration-xml-example"></a>Azure Media Indexer yapılandırması XML örneği

``` 
<?xml version="1.0" encoding="utf-8"?>  
<configuration version="2.0">  
  <input>  
    <metadata key="title" value="[Title of the media file]" />  
    <metadata key="description" value="[Description of the media file]" />  
  </input>  
  <settings>  
  </settings>  
  
  <features>  
    <feature name="ASR">    
      <settings>  
        <add key="Language" value="English"/>  
        <add key="GenerateKeywords" value ="true" />  
      </settings>  
    </feature>  
  </features>  
  
</configuration>  
```
  
## <a name="next-steps"></a>Sonraki adımlar

[Bkz. Azure Media Indexer ile medya dosyalarını dizine ekleme.](media-services-index-content.md)

