---
title: Azure Medya Hizmetleri çıktı meta veri şeması | Microsoft Dokümanlar
description: Bu makalede, Azure Medya Hizmetleri çıktı meta veri şemasına genel bir bakış sunulur.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 1ed84c88-eea5-4a24-9c4f-f2428157d08a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 3f0c6b60e2be625d1f869c3eda4acb9dfd3c6e9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74886821"
---
# <a name="output-metadata"></a>Çıktı Meta verileri
## <a name="overview"></a>Genel Bakış
Kodlama işi, bazı kodlama görevlerini gerçekleştirmek istediğiniz bir giriş kıymeti (veya varlıklar) ile ilişkilidir. Örneğin, bir MP4 dosyasını H.264 MP4 uyarlanabilir bithızı kümelerine kodlayın; küçük resim oluşturun; bindirmeler oluşturun. Bir görev tamamlandıktan sonra, bir çıktı varlık üretilir.  Çıkış varlığı video, ses, küçük resimler, vb içerir. Çıktı varlığı, çıktı varlığı yla ilgili meta verileri içeren bir dosya da içerir. Meta data XML dosyasının adı aşağıdaki &lt;biçime sahiptir: source_file_name&gt;_manifest.xml (örneğin, BigBuckBunny_manifest.xml).  

Ortam Hizmetleri, meta veri oluşturmak için giriş Varlıklarını önceden tarayamaz. Giriş meta verileri, yalnızca bir giriş Kıymeti Bir İş'te işlendiğinde yapı olarak oluşturulur. Bu nedenle bu yapı çıkış Varlık yazılır. Giriş Varlıkları ve çıktı Varlıkları için meta veri oluşturmak için farklı araçlar kullanılır. Bu nedenle, giriş meta verileri çıktı meta verilerinden biraz daha farklı şema vardır.

Meta veri dosyasını incelemek istiyorsanız, bir **SAS** bulucu oluşturup dosyayı yerel bilgisayarınıza indirebilirsiniz.  

Bu makalede, çıkış metadasının (source_file_name&lt;&gt;_manifest.xml) dayandığı XML şemasının öğeleri ve türleri anlatılmaktadır. Giriş kıymeti hakkında meta veriler içeren dosya hakkında bilgi için Giriş Meta verileri'ne bakın.  

Bu makalenin sonunda şema kodunun ve XML örneğinin tamamını bulabilirsiniz.  

## <a name="assetfiles-root-element"></a><a name="AssetFiles"></a>AssetFiles kök öğesi
Kodlama işi için AssetFile girişlerinin toplanması.  

### <a name="child-elements"></a>Alt öğeleri
| Adı | Açıklama |
| --- | --- |
| **Varlık Dosyası**<br/><br/> minOccurs="0" maxOccurs="1" |Varlık Dosyaları koleksiyonunun bir parçası olan bir AssetFile öğesi. |

## <a name="assetfile-element"></a><a name="AssetFile"></a>AssetFile öğesi
Bir XML örneği [XML örneği](#xml)bulabilirsiniz.  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Adı**<br/><br/> Gerekli |**xs:string** |Ortam varlık dosya adı. |
| **Boyut**<br/><br/> minInclusive ="0"<br/><br/> Gerekli |**xs:uzun** |Kıymet dosyasının baytboyutu. |
| **Süre**<br/><br/> Gerekli |**xs:süre** |İçerik oynatma süresi. |

### <a name="child-elements"></a>Alt öğeleri
| Adı | Açıklama |
| --- | --- |
| **Kaynak** |Bu Varlık Dosyasını üretmek için işlenen giriş/kaynak ortam dosyalarının toplanması. Daha fazla bilgi için Kaynak öğesine bakın. |
| **Video Parçaları**<br/><br/> minOccurs="0" maxOccurs="1" |Her fiziksel AssetFile içinde sıfır veya daha fazla video parçaları uygun bir kapsayıcı biçimine interleaved içerebilir. Daha fazla bilgi için VideoTracks öğesine bakın. |
| **Sesli Parçalar**<br/><br/> minOccurs="0" maxOccurs="1" |Her fiziksel AssetFile, uygun bir kapsayıcı biçimine ayrılan sıfır veya daha fazla ses parçası içerebilir. Bu tüm o ses parçalarının koleksiyonu. Daha fazla bilgi için AudioTracks öğesine bakın. |

## <a name="sources-element"></a><a name="Sources"></a>Kaynaklar öğesi
Bu Varlık Dosyasını üretmek için işlenen giriş/kaynak ortam dosyalarının toplanması.  

Bir XML örneği [XML örneği](#xml)bulabilirsiniz.  

### <a name="child-elements"></a>Alt öğeleri
| Adı | Açıklama |
| --- | --- |
| **Kaynak**<br/><br/> minOccurs="1" maxOccurs="sınırsız" |Bu varlığı oluştururken kullanılan bir giriş/kaynak dosyası. Daha fazla bilgi için Kaynak öğesine bakın. |

## <a name="source-element"></a><a name="Source"></a>Kaynak öğesi
Bu varlığı oluştururken kullanılan bir giriş/kaynak dosyası.  

Bir XML örneği [XML örneği](#xml)bulabilirsiniz.  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Adı**<br/><br/> Gerekli |**xs:string** |Giriş kaynak dosya adı. |

## <a name="videotracks-element"></a><a name="VideoTracks"></a>VideoTracks öğesi
Her fiziksel AssetFile içinde sıfır veya daha fazla video parçaları uygun bir kapsayıcı biçimine interleaved içerebilir. **VideoTracks** öğesi tüm video parçalarının bir koleksiyonunu temsil eder.  

Bir XML örneği [XML örneği](#xml)bulabilirsiniz.  

### <a name="child-elements"></a>Alt öğeleri
| Adı | Açıklama |
| --- | --- |
| **VideoTrack**<br/><br/> minOccurs="1" maxOccurs="sınırsız" |Üst AssetFile'da belirli bir video izleme. Daha fazla bilgi için Bkz. VideoTrack öğesi. |

## <a name="videotrack-element"></a><a name="VideoTrack"></a>VideoTrack öğesi
Üst AssetFile'da belirli bir video izleme.  

Bir XML örneği [XML örneği](#xml)bulabilirsiniz.  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Kimlik**<br/><br/> minInclusive ="0"<br/><br/> Gerekli |**xs:int** |Bu video parçasının sıfır tabanlı dizini. **Not:**  Bu **Kimlik,** mp4 dosyasında kullanılan TrackID olması gerekmez. |
| **Fourcc**<br/><br/> Gerekli |**xs:string** |Video codec FourCC kodu. |
| **Profil** |**xs:string** |H264 profili (sadece H264 codec için geçerlidir). |
| **Düzey** |**xs:string** |H264 düzeyi (sadece H264 codec için geçerlidir). |
| **Genişlik**<br/><br/> minInclusive ="0"<br/><br/> Gerekli |**xs:int** |Piksellerde kodlanmış video genişliği. |
| **Height**<br/><br/> minInclusive ="0"<br/><br/> Gerekli |**xs:int** |Piksellerde kodlanmış video yüksekliği. |
| **DisplayAspectRatioNumerator**<br/><br/> minInclusive ="0"<br/><br/> Gerekli |**xs:çift** |Video ekran boy oranı rakamı. |
| **DisplayAspectRatioDenominator**<br/><br/> minInclusive ="0"<br/><br/> Gerekli |**xs:çift** |Video ekran en boy oranı payda. |
| **Framerate**<br/><br/> minInclusive ="0"<br/><br/> Gerekli |**xs:ondalık** |0,3f formatında ölçülen video kare hızı. |
| **TargetFramerate**<br/><br/> minInclusive ="0"<br/><br/> Gerekli |**xs:ondalık** |Hedef kare hızını 0,3f formatında önceden ayarlayın. |
| **Bitrate**<br/><br/> minInclusive ="0"<br/><br/> Gerekli |**xs:int** |AssetFile'dan hesaplandığı gibi, saniyede kilobit cinsinden ortalama video bit hızı. Yalnızca temel akış yükünü sayar ve ambalaj yükünü içermez. |
| **TargetBitrate**<br/><br/> minInclusive ="0"<br/><br/> Gerekli |**xs:int** |Bu video parçası için hedef ortalama bit hızı, kodlama ön kümesi aracılığıyla istendiği gibi, saniyede kilobit. |
| **MaxGOPBitrate**<br/><br/> minInclusive ="0" |**xs:int** |Max GOP bu video parça için ortalama bithızı, saniyede kilobit. |

## <a name="audiotracks-element"></a><a name="AudioTracks"></a>AudioTracks öğesi
Her fiziksel AssetFile, uygun bir kapsayıcı biçimine ayrılan sıfır veya daha fazla ses parçası içerebilir. **AudioTracks** öğesi tüm bu ses parçalarının bir koleksiyonunu temsil eder.  

Bir XML örneği [XML örneği](#xml)bulabilirsiniz.  

### <a name="child-elements"></a>Alt öğeleri
| Adı | Açıklama |
| --- | --- |
| **Sesli Parça**<br/><br/> minOccurs="1" maxOccurs="sınırsız" |Üst AssetFile'da belirli bir ses kaydı. Daha fazla bilgi için AudioTrack öğesine bakın. |

## <a name="audiotrack-element"></a><a name="AudioTrack"></a>AudioTrack öğesi
Üst AssetFile'da belirli bir ses kaydı.  

Bir XML örneği [XML örneği](#xml)bulabilirsiniz.  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Kimlik**<br/><br/> minInclusive ="0"<br/><br/> Gerekli |**xs:int** |Bu ses parçasının sıfır tabanlı dizini. **Not:**  Bu, bir MP4 dosyasında kullanılan TrackID olması gerekmez. |
| **Codec** |**xs:string** |Ses parça codec dize. |
| **EncoderVersion** |**xs:string** |EAC3 için gerekli olan isteğe bağlı kodlayıcı sürüm dizesi. |
| **Kanallar**<br/><br/> minInclusive ="0"<br/><br/> Gerekli |**xs:int** |Ses kanallarının sayısı. |
| **Örnekleme Hızı**<br/><br/> minInclusive ="0"<br/><br/> Gerekli |**xs:int** |Örneklerde/sn veya Hz'de ses örnekleme hızı. |
| **Bitrate**<br/><br/> minInclusive ="0"<br/><br/> Gerekli |**xs:int** |AssetFile'dan hesaplanan saniyede bit cinsinden ortalama ses bit hızı. Yalnızca temel akış yükünü sayar ve ambalaj yükünü içermez. |
| **BitSPerSample**<br/><br/> minInclusive ="0"<br/><br/> Gerekli |**xs:int** |wFormatTag biçim türü için örnek başına bitler. |

### <a name="child-elements"></a>Alt öğeleri
| Adı | Açıklama |
| --- | --- |
| **YükseklikÖlçümSonuç Parametreleri**<br/><br/> minOccurs="0" maxOccurs="1" |Yükseklik ölçüm sonuç parametreleri. Daha fazla bilgi için Bkz. LoudnessMeteringResultParameters öğesi. |

## <a name="loudnessmeteringresultparameters-element"></a><a name="LoudnessMeteringResultParameters"></a>YükseklikÖlçerSonuç Parametreleri elemanı
Yükseklik ölçüm sonuç parametreleri.  

Bir XML örneği [XML örneği](#xml)bulabilirsiniz.  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **DPLMVersionInformation** |**xs:string** |**Dolby** profesyonel yükseklik ölçüm geliştirme kiti sürümü. |
| **İletişimNormalleştirme**<br/><br/> minInclusive="-31" maxInclusive="-1"<br/><br/> Gerekli |**xs:int** |DPLM ile oluşturulan DialogNormalization, LoudnessMetering ayarlandığında gerekli |
| **IntegratedLoudness**<br/><br/> minInclusive="-70" maxInclusive="10"<br/><br/> Gerekli |**xs:float** |Entegre yükseklik |
| **Entegre YükseklikBirimi**<br/><br/> Gerekli |**xs:string** |Entegre ses birimi. |
| **EntegreYükseklikGatingYöntemi**<br/><br/> Gerekli |**xs:string** |Gating tanımlayıcısı |
| **Entegre YükseklikKonuşma Yüzdesi**<br/><br/> minInclusive ="0" maxInclusive="100" |**xs:float** |Yüzde olarak program üzerinden konuşma içeriği. |
| **Örnek Tepe Noktası**<br/><br/> Gerekli |**xs:float** |Sıfırlamadan veya en son temizlenmeden bu yana kanal başına en yüksek mutlak örneklem değeri.  Birimler dBFS vardır. |
| **SamplePeakUnit**<br/><br/> sabit="dBFS"<br/><br/> Gerekli |**xs:anySimpleType** |Örnek tepe birimi. |
| **TruePeak**<br/><br/> Gerekli |**xs:float** |ItU-R BS.1770-2'ye göre, sıfırlandığından veya en son temizlenmeden bu yana kanal başına maksimum gerçek tepe değeri. Birimler dBTP vardır. |
| **TruePeakUnit**<br/><br/> sabit="dBTP"<br/><br/> Gerekli |**xs:anySimpleType** |Gerçek tepe birimi. |

## <a name="schema-code"></a>Şema Kodu
    <?xml version="1.0" encoding="utf-8"?>  
    <xs:schema xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" version="1.2"  
               xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata"  
               targetNamespace="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata"  
               elementFormDefault="qualified">  
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
                  <xs:element name="Sources">  
                    <xs:annotation>  
                      <xs:documentation>Collection of input/source media files, that was processed in order to produce this AssetFile</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="Source" minOccurs="1" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>An input/source file used when generating this asset</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
                            <xs:attribute name="Name" type="xs:string" use="required">  
                              <xs:annotation>  
                                <xs:documentation>input source file name</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                          </xs:complexType>  
                        </xs:element>  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="VideoTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format. This is the collection of all those video tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="VideoTrack" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>A specific video track in the parent AssetFile</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
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
                            <xs:attribute name="FourCC" type="xs:string" use="required">  
                              <xs:annotation>  
                                <xs:documentation>video codec FourCC code</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Profile" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>H264 profile (only appliable for H264 codec)</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Level" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>H264 level (only appliable for H264 codec)</xs:documentation>  
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
                            <xs:attribute name="Framerate" use="required">  
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
                            <xs:attribute name="TargetFramerate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>preset target video frame rate in .3f format</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:decimal">  
                                  <xs:minInclusive value="0"/>  
                                  <xs:fractionDigits value="3"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Bitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>average video bit rate in kilobits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="TargetBitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>target average bitrate for this video track, as requested via the encoding preset, in kilobits per second</xs:documentation>  
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
                          </xs:complexType>  
                        </xs:element>  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="AudioTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format. This is the collection of all those audio tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="AudioTrack" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>a specific audio track in the parent AssetFile</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
                            <xs:sequence>  
                              <xs:element name="LoudnessMeteringResultParameters" minOccurs="0" maxOccurs="1">  
                                <xs:annotation>  
                                  <xs:documentation>Loudness Metering Result Parameters</xs:documentation>  
                                </xs:annotation>  
                                <xs:complexType>  
                                  <xs:attribute name="DPLMVersionInformation" type="xs:string">  
                                    <xs:annotation>  
                                      <xs:documentation>Dolby Professional Loudness Metering Development Kit Version</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="DialogNormalization" use="required">  
                                    <xs:annotation>  
                                      <xs:documentation> DialogNormalization generated through DPLM, required when LoudnessMetering is set</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:int">  
                                        <xs:minInclusive value="-31"/>  
                                        <xs:maxInclusive value="-1"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudness" use="required">  
                                    <xs:annotation>  
                                      <xs:documentation>Integrated loudness</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:float">  
                                        <xs:minInclusive value="-70"/>  
                                        <xs:maxInclusive value="10"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessUnit" use="required" type="xs:string">  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessGatingMethod" use="required" type="xs:string">  
                                    <xs:annotation>  
                                      <xs:documentation>Gating identifier</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessSpeechPercentage">  
                                    <xs:annotation>  
                                      <xs:documentation>Speech content over the program, as a percentage.</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:float">  
                                        <xs:minInclusive value="0"/>  
                                        <xs:maxInclusive value="100"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="SamplePeak" use="required" type="xs:float">  
                                    <xs:annotation>  
                                      <xs:documentation>Peak absolute sample value, since reset or since it was last cleared, per channel.  Units are dBFS.</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="SamplePeakUnit" use="required" fixed="dBFS">  
                                  </xs:attribute>  
                                  <xs:attribute name="TruePeak" use="required" type="xs:float">  
                                    <xs:annotation>  
                                      <xs:documentation>Maximum True Peak value, as per ITU-R BS.1770-2, since reset or since it was last cleared, per channel.  Units are dBTP.</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="TruePeakUnit" use="required" fixed="dBTP">  
                                  </xs:attribute>  
                                </xs:complexType>  
                              </xs:element>  
                            </xs:sequence>  
                            <xs:attribute name="Id" use="required">  
                              <xs:annotation>  
                                <xs:documentation>zero-based index of this audio track. Note: this is not necessarily the TrackID as used in an MP4 file</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Codec" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>audio track codec string</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="EncoderVersion" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>optional encoder version string, required for EAC3</xs:documentation>  
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
                            <xs:attribute name="Bitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>average audio bit rate in bits per second, as calculated from the AssetFile. Counts only the elementary stream payload, and does not include the packaging overhead</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="BitsPerSample" use="required">  
                              <xs:annotation>  
                                <xs:documentation>Bits per sample for the wFormatTag format type</xs:documentation>  
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
                <xs:attribute name="Duration" use="required">  
                  <xs:annotation>  
                    <xs:documentation>content play back duration</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:duration"/>  
                  </xs:simpleType>  
                </xs:attribute>  
              </xs:complexType>  
            </xs:element>  
          </xs:sequence>  
        </xs:complexType>  
      </xs:element>  
    </xs:schema>  



## <a name="xml-example"></a><a name="xml"></a>XML örneği

Aşağıdaki XML, Çıktı meta veri dosyasının bir örneğidir.  

    <AssetFiles xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="https://www.w3.org/2001/XMLSchema"   
                xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata">  
      <AssetFile Name="BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4" Size="4646283" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.2" Width="1280" Height="720" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="4250" TargetBitrate="3400" MaxGOPBitrate="5514"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4" Size="3166728" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.1" Width="960" Height="540" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="2846" TargetBitrate="2250" MaxGOPBitrate="3630"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4" Size="2205095" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.1" Width="960" Height="540" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="1932" TargetBitrate="1500" MaxGOPBitrate="2513"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4" Size="1508567" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.0" Width="640" Height="360" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="1271" TargetBitrate="1000" MaxGOPBitrate="1527"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4" Size="1057155" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.0" Width="640" Height="360" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="843" TargetBitrate="650" MaxGOPBitrate="1086"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4" Size="699262" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="1.3" Width="320" Height="180" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="503" TargetBitrate="400" MaxGOPBitrate="661"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_AAC_und_ch2_96kbps.mp4" Size="166780" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_AAC_und_ch2_56kbps.mp4" Size="124576" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="53" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
    </AssetFiles>  

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
