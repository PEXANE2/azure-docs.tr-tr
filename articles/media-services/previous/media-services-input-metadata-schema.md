---
title: Azure Medya Hizmetleri giriş meta veri şeması | Microsoft Dokümanlar
description: Bu makalede, Azure Medya Hizmetleri giriş meta veri şemasına genel bir bakış sunulur.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: a81d6edfd887dc935a53742b7bc1492651c9bda5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74887128"
---
# <a name="input-metadata"></a>Giriş Meta verileri 

Kodlama işi, bazı kodlama görevlerini gerçekleştirmek istediğiniz bir giriş kıymeti (veya varlıklar) ile ilişkilidir.  Bir görev tamamlandıktan sonra, bir çıktı varlık üretilir.  Çıkış varlığı video, ses, küçük resimler, manifesto, vb içerir. Çıktı varlığı, giriş kıymeti hakkında meta verileri içeren bir dosya da içerir. Meta data XML dosyasının adı aşağıdaki &lt;biçime sahiptir: asset_id&gt;_metadata.xml (örneğin, 41114ad3-eb5e-4c57-8d92-5354e2b7d4a4_metadata.xml), asset_id &lt;&gt; giriş varlığının AssetId değeridir.  

Ortam Hizmetleri, meta veri oluşturmak için giriş Varlıklarını önceden tarayamaz. Giriş meta verileri, yalnızca bir giriş Kıymeti Bir İş'te işlendiğinde yapı olarak oluşturulur. Bu nedenle bu yapı çıkış Varlık yazılır. Giriş Varlıkları ve çıktı Varlıkları için meta veri oluşturmak için farklı araçlar kullanılır. Bu nedenle, giriş meta verileri çıktı meta verilerinden biraz daha farklı şema vardır.

Meta veri dosyasını incelemek istiyorsanız, bir **SAS** bulucu oluşturup dosyayı yerel bilgisayarınıza indirebilirsiniz. Medya [Hizmetleri .NET SDK Uzantılarını Kullanarak](media-services-dotnet-get-started.md)Bir SAS bulucu su nasıl oluşturulup dosyayı indirebileceğinize ilişkin bir örnek bulabilirsiniz.  

Bu makalede, giriş metada (asset_id&lt;&gt;_metadata.xml) dayandığı XML şemasının öğeleri ve türleri anlatılmaktadır.  Çıktı varlığı hakkında meta veri içeren dosya hakkında bilgi için [çıktı Meta verileri](media-services-output-metadata-schema.md)bölümüne bakın.  

Bu makalenin sonunda [Şema Kodu'nu](media-services-input-metadata-schema.md#code) bir [XML örneği](media-services-input-metadata-schema.md#xml) bulabilirsiniz.  
 

## <a name="assetfiles-element-root-element"></a><a name="AssetFiles"></a>AssetFiles öğesi (kök öğesi)
Kodlama işi için [AssetFile öğesi](media-services-input-metadata-schema.md#AssetFile)nin bir koleksiyonunu içerir.  

Bu makalenin sonunda bir XML örneğine bakın: [XML örneği.](media-services-input-metadata-schema.md#xml)  

| Adı | Açıklama |
| --- | --- |
| **Varlık Dosyası**<br /><br /> minOccurs="1" maxOccurs="sınırsız" |Tek bir alt öğe. Daha fazla bilgi için [AssetFile öğesine](media-services-input-metadata-schema.md#AssetFile)bakın. |

## <a name="assetfile-element"></a><a name="AssetFile"></a>AssetFile öğesi
 Varlık dosyalarını açıklayan öznitelikleri ve öğeleri içerir.  

 Bu makalenin sonunda bir XML örneğine bakın: [XML örneği.](media-services-input-metadata-schema.md#xml)  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Adı**<br /><br /> Gerekli |**xs:string** |Varlık dosya adı. |
| **Boyut**<br /><br /> Gerekli |**xs:uzun** |Kıymet dosyasının baytboyutu. |
| **Süre**<br /><br /> Gerekli |**xs:süre** |İçerik oynatma süresi. Örnek: Süre="PT25M37.757S". |
| **NumberOfStreams**<br /><br /> Gerekli |**xs:int** |Varlık dosyasındaki akış sayısı. |
| **Biçim Adları**<br /><br /> Gerekli |**xs: dize** |Adları biçimlendirin. |
| **BiçimVerboseİsimler**<br /><br /> Gerekli |**xs: dize** |Ayrıntılı adları biçimlendirin. |
| **Starttime** |**xs:süre** |İçerik başlangıç saati. Örnek: Başlangıç Zamanı="PT2.669S". |
| **Genel Bit Hızı** |**xs: int** |KBPs'deki varlık dosyasının ortalama bithızı. |

> [!NOTE]
> Aşağıdaki dört alt öğe bir sırada görünmelidir.  
> 
> 

### <a name="child-elements"></a>Alt öğeleri
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Programlar**<br /><br /> minOccurs="0" | |Varlık dosyası MPEG-TS biçimindeyken tüm [Programlar öğesinin](media-services-input-metadata-schema.md#Programs) toplanması. |
| **Video Parçaları**<br /><br /> minOccurs="0" | |Her fiziksel varlık dosyası, uygun bir kapsayıcı biçimine ayrılan sıfır veya daha fazla video parçası içerebilir. Bu öğe, varlık dosyasının bir parçası olan tüm [VideoTracks](media-services-input-metadata-schema.md#VideoTracks) bir koleksiyon içerir. |
| **Sesli Parçalar**<br /><br /> minOccurs="0" | |Her fiziksel varlık dosyası, uygun bir kapsayıcı biçimine ayrılan sıfır veya daha fazla ses parçası içerebilir. Bu öğe, varlık dosyasının bir parçası olan tüm [AudioTracks](media-services-input-metadata-schema.md#AudioTracks) bir koleksiyon içerir. |
| **Meta veriler**<br /><br /> minOccurs="0" maxOccurs="sınırsız" |[MetadataType](media-services-input-metadata-schema.md#MetadataType) |Varlık dosyasının meta verileri anahtar\değer dizeleri olarak temsil edilir. Örnek:<br /><br /> **&lt;Metadata key="dil" değeri="eng" /&gt;** |

## <a name="tracktype"></a><a name="TrackType"></a>TrackType
Bu makalenin sonunda bir XML örneğine bakın: [XML örneği.](media-services-input-metadata-schema.md#xml)  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Kimlik**<br /><br /> Gerekli |**xs:int** |Bu ses veya video parçasının sıfır tabanlı dizini.<br /><br /> Bu, bir MP4 dosyasında kullanılan TrackID'nin olması gerekmez. |
| **Codec** |**xs:string** |Video parça codec dize. |
| **CodecLongName** |**xs: dize** |Ses veya video parça codec uzun adı. |
| **Zaman Tabanı**<br /><br /> Gerekli |**xs:string** |Zaman tabanı. Örnek: TimeBase="1/48000" |
| **NumberOfFrames** |**xs:int** |Kare sayısı (video parçaları için hazır). |
| **Starttime** |**xs: süre** |Pist başlangıç saati. Örnek: Başlangıç Zamanı="PT2.669S" |
| **Süre** |**xs:süre** |Süreyi takip et. Örnek: Süre="PTSampleFormat M37.757S". |

> [!NOTE]
> Aşağıdaki iki alt öğe bir sıra halinde görünmelidir.  
> 
> 

### <a name="child-elements"></a>Alt öğeleri
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Disposition**<br /><br /> minOccurs="0" maxOccurs="1" |[StreamDispositionType](media-services-input-metadata-schema.md#StreamDispositionType) |Sunu bilgilerini içerir (örneğin, belirli bir ses parçasının görme engelli görüntüleyenler için olup olmadığı). |
| **Meta veriler**<br /><br /> minOccurs="0" maxOccurs="sınırsız" |[MetadataType](media-services-input-metadata-schema.md#MetadataType) |Çeşitli bilgileri tutmak için kullanılabilecek genel anahtar/değer dizeleri. Örneğin, key="language", ve value="eng". |

## <a name="audiotracktype-inherits-from-tracktype"></a><a name="AudioTrackType"></a>AudioTrackType (TrackType'ten devralır)
 **AudioTrackType** [TrackType'tan](media-services-input-metadata-schema.md#TrackType)devralan küresel karmaşık bir türüdür.  

 Tür, varlık dosyasındaki belirli bir ses parçasını temsil eder.  

 Bu makalenin sonunda bir XML örneğine bakın: [XML örneği.](media-services-input-metadata-schema.md#xml)  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Örnek Format** |**xs:string** |Örnek biçimi. |
| **Kanal Düzeni** |**xs: dize** |Kanal düzeni. |
| **Kanallar**<br /><br /> Gerekli |**xs:int** |Ses kanallarının sayısı (0 veya daha fazla). |
| **Örnekleme Hızı**<br /><br /> Gerekli |**xs:int** |Örneklerde/sn veya Hz'de ses örnekleme hızı. |
| **Bitrate** |**xs:int** |Kıymet dosyasından hesaplandığı gibi, saniyedeki bit cinsinden ortalama ses bithızı. Yalnızca temel akış yükü sayılır ve ambalaj yükü bu sayıma dahil edilmez. |
| **BitSPerSample** |**xs:int** |wFormatTag biçim türü için örnek başına bitler. |

## <a name="videotracktype-inherits-from-tracktype"></a><a name="VideoTrackType"></a>VideoTrackType (TrackType'ten devralır)
**VideoTrackType** [TrackType'tan](media-services-input-metadata-schema.md#TrackType)devralınan küresel karmaşık bir türüdür.  

Tür, varlık dosyasındaki belirli bir video parçasını temsil eder.  

Bu makalenin sonunda bir XML örneğine bakın: [XML örneği.](media-services-input-metadata-schema.md#xml)  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Fourcc**<br /><br /> Gerekli |**xs:string** |Video codec FourCC kodu. |
| **Profil** |**xs: dize** |Video parçasının profili. |
| **Düzey** |**xs: dize** |Video parçası seviyesi. |
| **Pixelformat** |**xs: dize** |Video parçanın piksel biçimi. |
| **Genişlik**<br /><br /> Gerekli |**xs:int** |Piksellerde kodlanmış video genişliği. |
| **Height**<br /><br /> Gerekli |**xs:int** |Piksellerde kodlanmış video yüksekliği. |
| **DisplayAspectRatioNumerator**<br /><br /> Gerekli |**xs: çift** |Video ekran boy oranı rakamı. |
| **DisplayAspectRatioDenominator**<br /><br /> Gerekli |**xs:çift** |Video ekran en boy oranı payda. |
| **DisplayAspectRatioDenominator**<br /><br /> Gerekli |**xs: çift** |Video örnek boy oranı rakamı. |
| **SampleAspectRatioNumerator** |**xs: çift** |Video örnek boy oranı rakamı. |
| **SampleAspectRatioNumerator** |**xs:çift** |Video örnek en boy oranı payda. |
| **Framerate**<br /><br /> Gerekli |**xs:ondalık** |0,3f formatında ölçülen video kare hızı. |
| **Bitrate** |**xs:int** |Varlık dosyasından hesaplandığı gibi, saniyedeki kilobit cinsinden ortalama video bit hızı. Yalnızca temel akış yükü sayılır ve ambalaj yükü dahil edilmez. |
| **MaxGOPBitrate** |**xs: int** |Max GOP bu video parça için ortalama bithızı, saniyede kilobit. |
| **HasBFrames** |**xs:int** |B karelerinin video izleme sayısı. |

## <a name="metadatatype"></a><a name="MetadataType"></a>MetadataType
**MetadataType,** varlık dosyasının meta verilerini anahtar/değer dizeleri olarak açıklayan genel karmaşık bir türdür. Örneğin, key="language", ve value="eng".  

Bu makalenin sonunda bir XML örneğine bakın: [XML örneği.](media-services-input-metadata-schema.md#xml)  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **anahtar**<br /><br /> Gerekli |**xs:string** |Anahtar/değer çiftindeki anahtar. |
| **value**<br /><br /> Gerekli |**xs:string** |Anahtar/değer çiftindeki değer. |

## <a name="programtype"></a><a name="ProgramType"></a>ProgramTürü
**ProgramType,** bir programı açıklayan genel karmaşık bir türdür.  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Programid**<br /><br /> Gerekli |**xs:int** |Program Kimliği |
| **Sayı programları**<br /><br /> Gerekli |**xs:int** |Program sayısı. |
| **PmtPid**<br /><br /> Gerekli |**xs:int** |Program Harita Tabloları (PMTs) programlar hakkında bilgi içerir.  Daha fazla bilgi için [Bkz. PMt.](https://en.wikipedia.org/wiki/MPEG_transport_stream#PMT) |
| **PcrPid**<br /><br /> Gerekli |**xs: int** |Kod çözücü tarafından kullanılır. Daha fazla bilgi için [PCR'ye](https://en.wikipedia.org/wiki/MPEG_transport_stream#PCR) bakın |
| **Başlangıç PTS** |**xs: uzun** |Sunum süresi damgası başlangıç. |
| **EndPTS** |**xs: uzun** |Sunu zaman damgasını sona erdirme. |

## <a name="streamdispositiontype"></a><a name="StreamDispositionType"></a>StreamDispositionType
**StreamDispositionType** akışı açıklayan genel karmaşık bir türüdür.  

Bu makalenin sonunda bir XML örneğine bakın: [XML örneği.](media-services-input-metadata-schema.md#xml)  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Varsayılan**<br /><br /> Gerekli |**xs: int** |Bu özniteliği varsayılan sunu olduğunu belirtmek için 1 olarak ayarlayın. |
| **Dub**<br /><br /> Gerekli |**xs:int** |Bu özniteliği, dublajlı sunu olduğunu belirtmek için 1 olarak ayarlayın. |
| **Özgün**<br /><br /> Gerekli |**xs: int** |Bu özniteliği özgün sunu olduğunu belirtmek için 1 olarak ayarlayın. |
| **Yorum**<br /><br /> Gerekli |**xs:int** |Bu parçanın yorum içerdiğini belirtmek için bu özniteliği 1 olarak ayarlayın. |
| **Şarkı**<br /><br /> Gerekli |**xs:int** |Bu parçanın şarkı sözlerini içerdiğini belirtmek için bu özniteliği 1 olarak ayarlayın. |
| **Karaoke**<br /><br /> Gerekli |**xs:int** |Bu özniteliği, karaoke parçasını (arka plan müziği, vokal yok) temsil ettiğini belirtmek için 1 olarak ayarlayın. |
| **Zorla**<br /><br /> Gerekli |**xs:int** |Bu özniteliği, zorunlu sunu olduğunu belirtmek için 1 olarak ayarlayın. |
| **İşitme Engelliler**<br /><br /> Gerekli |**xs:int** |Bu parçanın işitme zorları için olduğunu belirtmek için bu özniteliği 1 olarak ayarlayın. |
| **Görme Engelli**<br /><br /> Gerekli |**xs:int** |Bu parçanın görme engelliler için olduğunu belirtmek için bu özniteliği 1 olarak ayarlayın. |
| **Temizleme Efektleri**<br /><br /> Gerekli |**xs: int** |Bu parçanın temiz etkileri olduğunu belirtmek için bu özniteliği 1 olarak ayarlayın. |
| **Ekli Resim**<br /><br /> Gerekli |**xs: int** |Bu parçanın resimleri olduğunu belirtmek için bu özniteliği 1 olarak ayarlayın. |

## <a name="programs-element"></a><a name="Programs"></a>Programlar öğesi
Birden çok **Program** öğesi tutan sarma elemanı.  

### <a name="child-elements"></a>Alt öğeleri
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Program**<br /><br /> minOccurs="0" maxOccurs="sınırsız" |[ProgramTürü](media-services-input-metadata-schema.md#ProgramType) |MPEG-TS biçiminde olan varlık dosyaları için varlık dosyasındaki programlar hakkında bilgi içerir. |

## <a name="videotracks-element"></a><a name="VideoTracks"></a>VideoTracks öğesi
 Birden çok **VideoTrack** öğesi tutan sarma elemanı.  

 Bu makalenin sonunda bir XML örneğine bakın: [XML örneği.](media-services-input-metadata-schema.md#xml)  

### <a name="child-elements"></a>Alt öğeleri
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **VideoTrack**<br /><br /> minOccurs="0" maxOccurs="sınırsız" |[VideoTrackType (TrackType'ten devralır)](media-services-input-metadata-schema.md#VideoTrackType) |Varlık dosyasındaki video parçaları hakkında bilgi içerir. |

## <a name="audiotracks-element"></a><a name="AudioTracks"></a>AudioTracks öğesi
 Birden çok **AudioTrack** öğesini tutan sarma elemanı.  

 Bu makalenin sonunda bir XML örneğine bakın: [XML örneği.](media-services-input-metadata-schema.md#xml)  

### <a name="elements"></a>öğeler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Sesli Parça**<br /><br /> minOccurs="0" maxOccurs="sınırsız" |[AudioTrackType (TrackType'ten devralır)](media-services-input-metadata-schema.md#AudioTrackType) |Varlık dosyasındaki ses parçaları hakkında bilgi içerir. |

## <a name="schema-code"></a><a name="code"></a>Şema Kodu
    <?xml version="1.0" encoding="utf-8"?>  
    <xs:schema xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" version="1.0"  
               xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata"  
               targetNamespace="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata"  
               elementFormDefault="qualified">  

      <xs:complexType name="MetadataType">  
        <xs:attribute name="key"   type="xs:string" use="required"/>  
        <xs:attribute name="value" type="xs:string" use="required"/>  
      </xs:complexType>  

      <xs:complexType name="ProgramType">  
        <xs:attribute name="ProgramId" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>Program Id</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="NumberOfPrograms" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>Number of programs</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="PmtPid" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>pmt pid</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="PcrPid" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>pcr pid</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="StartPTS" type="xs:long">  
          <xs:annotation>  
            <xs:documentation>start pts</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="EndPTS" type="xs:long">  
          <xs:annotation>  
            <xs:documentation>end pts</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
      </xs:complexType>  

      <xs:complexType name="StreamDispositionType">  
        <xs:attribute name="Default"          type="xs:int" use="required" />  
        <xs:attribute name="Dub"              type="xs:int" use="required" />  
        <xs:attribute name="Original"         type="xs:int" use="required" />  
        <xs:attribute name="Comment"          type="xs:int" use="required" />  
        <xs:attribute name="Lyrics"           type="xs:int" use="required" />  
        <xs:attribute name="Karaoke"          type="xs:int" use="required" />  
        <xs:attribute name="Forced"           type="xs:int" use="required" />  
        <xs:attribute name="HearingImpaired"  type="xs:int" use="required" />  
        <xs:attribute name="VisualImpaired"   type="xs:int" use="required" />  
        <xs:attribute name="CleanEffects"     type="xs:int" use="required" />  
        <xs:attribute name="AttachedPic"      type="xs:int" use="required" />  
      </xs:complexType>  

      <xs:complexType name="TrackType" abstract="true">  
        <xs:sequence>  
          <xs:element name="Disposition" type="StreamDispositionType" minOccurs="0" maxOccurs="1"/>  
          <xs:element name="Metadata" type="MetadataType" minOccurs="0" maxOccurs="unbounded"/>  
        </xs:sequence>  
        <xs:attribute name="Id" use="required">  
          <xs:annotation>  
            <xs:documentation>zero-based index of this video track. Note: this is not necessarily the TrackID as used in an MP4 file</xs:documentation>  
          </xs:annotation>  
          <xs:simpleType>  
            <xs:restriction base="xs:int">  
              <xs:minInclusive value="0"/>  
            </xs:restriction>  
          </xs:simpleType>  
        </xs:attribute>  
        <xs:attribute name="Codec" type="xs:string">  
          <xs:annotation>  
            <xs:documentation>video track codec string</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="CodecLongName" type="xs:string">  
          <xs:annotation>  
            <xs:documentation>video track codec long name</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="TimeBase"  type="xs:string" use="required">  
          <xs:annotation>  
            <xs:documentation>Time base. Example: TimeBase="1/48000"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="NumberOfFrames">  
          <xs:annotation>  
            <xs:documentation>number of frames</xs:documentation>  
          </xs:annotation>  
          <xs:simpleType>  
            <xs:restriction base="xs:int">  
              <xs:minInclusive value="0"/>  
            </xs:restriction>  
          </xs:simpleType>  
        </xs:attribute>  
        <xs:attribute name="StartTime" type="xs:duration">  
          <xs:annotation>  
            <xs:documentation>Track start time. Example: StartTime="PT2.669S"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="Duration" type="xs:duration">  
          <xs:annotation>  
            <xs:documentation>Track duration. Example: Duration="PT25M37.757S"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
      </xs:complexType>  

      <xs:complexType name="VideoTrackType">  
        <xs:annotation>  
          <xs:documentation>A specific video track in the parent AssetFile</xs:documentation>  
        </xs:annotation>  
        <xs:complexContent>  
          <xs:extension base="TrackType">  
            <xs:attribute name="FourCC" type="xs:string" use="required">  
              <xs:annotation>  
                <xs:documentation>video codec FourCC code</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Profile" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>profile</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Level" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>level</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="PixelFormat" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>Video track's pixel format</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Width" use="required">  
              <xs:annotation>  
                <xs:documentation>encoded video width in pixels</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="Height" use="required">  
              <xs:annotation>  
                <xs:documentation>encoded video height in pixels</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="DisplayAspectRatioNumerator" use="required">  
              <xs:annotation>  
                <xs:documentation>video display aspect ratio numerator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="DisplayAspectRatioDenominator" use="required">  
              <xs:annotation>  
                <xs:documentation>video display aspect ratio denominator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SampleAspectRatioNumerator">  
              <xs:annotation>  
                <xs:documentation>video sample aspect ratio numerator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SampleAspectRatioDenominator">  
              <xs:annotation>  
                <xs:documentation>video sample aspect ratio denominator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="FrameRate" use="required">  
              <xs:annotation>  
                <xs:documentation>measured video frame rate in .3f format</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:decimal">  
                  <xs:minInclusive value="0"/>  
                  <xs:fractionDigits value="3"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="Bitrate">  
              <xs:annotation>  
                <xs:documentation>average video bit rate in kilobits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="MaxGOPBitrate">  
              <xs:annotation>  
                <xs:documentation>Max GOP average bitrate for this video track, in kilobits per second</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="HasBFrames" type="xs:int">  
              <xs:annotation>  
                <xs:documentation>video track number of B frames</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
          </xs:extension>  
        </xs:complexContent>  
      </xs:complexType>  

      <xs:complexType name="AudioTrackType">  
        <xs:annotation>  
          <xs:documentation>a specific audio track in the parent AssetFile</xs:documentation>  
        </xs:annotation>  
        <xs:complexContent>  
          <xs:extension base="TrackType">  
            <xs:attribute name="SampleFormat"  type="xs:string">  
              <xs:annotation>  
                <xs:documentation>sample format</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="ChannelLayout"  type="xs:string">  
              <xs:annotation>  
                <xs:documentation>channel layout</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Channels" use="required">  
              <xs:annotation>  
                <xs:documentation>number of audio channels</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SamplingRate" use="required">  
              <xs:annotation>  
                <xs:documentation>audio sampling rate in samples/sec or Hz</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="Bitrate">  
              <xs:annotation>  
                <xs:documentation>average audio bit rate in bits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="BitsPerSample">  
              <xs:annotation>  
                <xs:documentation>Bits per sample for the wFormatTag format type</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
          </xs:extension>  
        </xs:complexContent>  
      </xs:complexType>  

      <xs:element name="AssetFiles">  
        <xs:annotation>  
          <xs:documentation>Collection of AssetFile entries for the encoding job</xs:documentation>  
        </xs:annotation>  
        <xs:complexType>  
          <xs:sequence>  
            <xs:element name="AssetFile" minOccurs="1" maxOccurs="unbounded">  
              <xs:annotation>  
                <xs:documentation>asset file</xs:documentation>  
              </xs:annotation>  
              <xs:complexType>  
                <xs:sequence>  
                  <xs:element name="Programs" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>This is the collection of all programs when file is MPEG-TS</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="Program" type="ProgramType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="VideoTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format. This is the collection of all those video tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="VideoTrack" type="VideoTrackType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="AudioTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format. This is the collection of all those audio tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="AudioTrack" type="AudioTrackType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="Metadata" type="MetadataType" minOccurs="0" maxOccurs="unbounded" />  
                </xs:sequence>  
                <xs:attribute name="Name" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>the media asset file name</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="Size" use="required">  
                  <xs:annotation>  
                    <xs:documentation>size of file in bytes</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:long">  
                      <xs:minInclusive value="0"/>  
                    </xs:restriction>  
                  </xs:simpleType>  
                </xs:attribute>  
                <xs:attribute name="Duration" type="xs:duration" use="required">  
                  <xs:annotation>  
                    <xs:documentation>content play back duration. Example: Duration="PT25M37.757S"</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="NumberOfStreams" type="xs:int" use="required">  
                  <xs:annotation>  
                    <xs:documentation>number of streams in asset file</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="FormatNames" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>format names</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="FormatVerboseName" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>format verbose names</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="StartTime" type="xs:duration">  
                  <xs:annotation>  
                    <xs:documentation>content start time. Example: StartTime="PT2.669S"</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="OverallBitRate">  
                  <xs:annotation>  
                    <xs:documentation>average bitrate of the asset file in kbps</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:int">  
                      <xs:minInclusive value="0"/>  
                    </xs:restriction>  
                  </xs:simpleType>  
                </xs:attribute>  
              </xs:complexType>  
            </xs:element>  
          </xs:sequence>  
        </xs:complexType>  
      </xs:element>  
    </xs:schema>  


## <a name="xml-example"></a><a name="xml"></a>XML örneği
Aşağıda Giriş meta veri dosyasının bir örneği verilmiştir.  

    <?xml version="1.0" encoding="utf-8"?>  
    <AssetFiles xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata">  
      <AssetFile Name="bear.mp4" Size="1973733" Duration="PT12.678S" NumberOfStreams="2" FormatNames="mov,mp4,m4a,3gp,3g2,mj2" FormatVerboseName="QuickTime / MOV" StartTime="PT0S" OverallBitRate="1245">  
        <VideoTracks>  
          <VideoTrack Id="1" Codec="h264" CodecLongName="H.264 / AVC / MPEG-4 AVC / MPEG-4 part 10" TimeBase="1/29970" NumberOfFrames="375" StartTime="PT0.034S" Duration="PT12.645S" FourCC="avc1" Profile="High" Level="4.1" PixelFormat="yuv420p" Width="512" Height="384" DisplayAspectRatioNumerator="4" DisplayAspectRatioDenominator="3" SampleAspectRatioNumerator="1" SampleAspectRatioDenominator="1" FrameRate="29.656" Bitrate="1043" HasBFrames="1">  
            <Disposition Default="1" Dub="0" Original="0" Comment="0" Lyrics="0" Karaoke="0" Forced="0" HearingImpaired="0" VisualImpaired="0" CleanEffects="0" AttachedPic="0" />  
            <Metadata key="creation_time" value="2010-03-10 16:11:56" />  
            <Metadata key="language" value="eng" />  
            <Metadata key="handler_name" value="Mainconcept MP4 Video Media Handler" />  
          </VideoTrack>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="aac" CodecLongName="AAC (Advanced Audio Coding)" TimeBase="1/44100" NumberOfFrames="546" StartTime="PT0S" Duration="PT12.678S" SampleFormat="fltp" ChannelLayout="stereo" Channels="2" SamplingRate="44100" Bitrate="156" BitsPerSample="0">  
            <Disposition Default="1" Dub="0" Original="0" Comment="0" Lyrics="0" Karaoke="0" Forced="0" HearingImpaired="0" VisualImpaired="0" CleanEffects="0" AttachedPic="0" />  
            <Metadata key="creation_time" value="2010-03-10 16:11:56" />  
            <Metadata key="language" value="eng" />  
            <Metadata key="handler_name" value="Mainconcept MP4 Sound Media Handler" />  
          </AudioTrack>  
        </AudioTracks>  
        <Metadata key="major_brand" value="mp42" />  
        <Metadata key="minor_version" value="0" />  
        <Metadata key="compatible_brands" value="mp42mp41" />  
        <Metadata key="creation_time" value="2010-03-10 16:11:53" />  
        <Metadata key="comment" value="Courtesy of National Geographic.  Used by Permission." />  
      </AssetFile>  
    </AssetFiles>  

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

