---
title: Azure Media Services çıktı meta verileri şeması | Microsoft Docs
description: Bu makale, Azure Media Services çıkış meta verisi şemasına bir genel bakış sunar.
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
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74886821"
---
# <a name="output-metadata"></a>Çıkış meta verileri
## <a name="overview"></a>Genel Bakış
Bir kodlama işi, üzerinde bazı kodlama görevlerini gerçekleştirmek istediğiniz bir giriş varlığı (veya varlıkları) ile ilişkilendirilir. Örneğin, bir MP4 dosyasını H. küçük resim oluşturma; yer paylaşımları oluştur. Bir görevi tamamladıktan sonra bir çıkış varlığı üretilir.  Çıkış varlığı video, ses, küçük resim vb. içerir. Çıktı varlığı Ayrıca çıkış varlığı hakkında meta veriler içeren bir dosya içerir. Meta veri XML dosyasının adı şu biçimdedir: &lt;source_file_name&gt;_manifest. xml (örneğin, BigBuckBunny_manifest. xml).  

Media Services meta veri oluşturmak için giriş varlıklarını önceden üretemiyor. Giriş meta verileri yalnızca bir Iş içinde bir giriş varlığı işlendiğinde yapıt olarak üretilir. Bu nedenle, bu yapıt çıkış varlığına yazılır. Farklı araçlar, giriş varlıkları ve çıkış varlıkları için meta veriler oluşturmak için kullanılır. Bu nedenle, giriş meta verilerinin, çıkış meta verileri arasında biraz farklı bir şeması vardır.

Meta veri dosyasını incelemek istiyorsanız **SAS** Konumlandırıcı oluşturabilir ve dosyayı yerel bilgisayarınıza indirebilirsiniz.  

Bu makalede, çıkış metada (&lt;source_file_name&gt;_manifest. xml) temel aldığı XML şemasının öğeleri ve türleri açıklanmaktadır. Giriş varlığı hakkında meta veriler içeren dosya hakkında daha fazla bilgi için bkz. giriş meta verileri.  

Tüm şema kodunu ve XML örneğini Bu makalenin sonunda bulabilirsiniz.  

## <a name="AssetFiles"></a>AssetFiles kök öğesi
Kodlama işi için Assetdosya girişlerinin toplanması.  

### <a name="child-elements"></a>Alt öğeleri
| Adı | Açıklama |
| --- | --- |
| **AssetFile**<br/><br/> minOccurs = "0" maxOccurs = "1" |AssetFiles koleksiyonunun bir parçası olan bir AssetFile öğesi. |

## <a name="AssetFile"></a>AssetFile öğesi
XML örnek [XML örneği](#xml)bulabilirsiniz.  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Adı**<br/><br/> Gereklidir |**xs: String** |Medya varlık dosya adı. |
| **Boyut**<br/><br/> minInclusive = "0"<br/><br/> Gereklidir |**xs: Long** |Varlık dosyasının bayt cinsinden boyutu. |
| **Süresi**<br/><br/> Gereklidir |**xs:duration** |İçerik kayıttan yürütme süresi. |

### <a name="child-elements"></a>Alt öğeleri
| Adı | Açıklama |
| --- | --- |
| **Ğına** |Bu Assetdosyasını üretmek için işlenen giriş/kaynak medya dosyalarının koleksiyonu. Daha fazla bilgi için bkz. kaynak öğesi. |
| **Videoizler**<br/><br/> minOccurs = "0" maxOccurs = "1" |Her fiziksel Assetdosyası bu dosyada sıfır veya daha fazla video ile ilgili bir kapsayıcı biçiminde araya eklenebilir. Daha fazla bilgi için bkz. Videoiz öğesi. |
| **Ses Izleri**<br/><br/> minOccurs = "0" maxOccurs = "1" |Her fiziksel Assetdosyası bu dosyada sıfır veya daha fazla ses parçası ile uygun bir kapsayıcı biçimiyle bulunabilir. Bu, tüm ses izlemelerinin koleksiyonudur. Daha fazla bilgi için bkz. Audioiz öğesi. |

## <a name="Sources"></a>Sources öğesi
Bu Assetdosyasını üretmek için işlenen giriş/kaynak medya dosyalarının koleksiyonu.  

XML örnek [XML örneği](#xml)bulabilirsiniz.  

### <a name="child-elements"></a>Alt öğeleri
| Adı | Açıklama |
| --- | --- |
| **Kaynak**<br/><br/> minOccurs = "1" maxOccurs = "sınırsız" |Bu varlık oluşturulurken kullanılan bir giriş/kaynak dosyası. Daha fazla bilgi için bkz. kaynak öğesi. |

## <a name="Source"></a>Kaynak öğe
Bu varlık oluşturulurken kullanılan bir giriş/kaynak dosyası.  

XML örnek [XML örneği](#xml)bulabilirsiniz.  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Adı**<br/><br/> Gereklidir |**xs: String** |Giriş kaynak dosya adı. |

## <a name="VideoTracks"></a>Videoiz öğesi
Her fiziksel Assetdosyası bu dosyada sıfır veya daha fazla video ile ilgili bir kapsayıcı biçiminde araya eklenebilir. **Videoiz** öğesi tüm video izlemelerinin bir koleksiyonunu temsil eder.  

XML örnek [XML örneği](#xml)bulabilirsiniz.  

### <a name="child-elements"></a>Alt öğeleri
| Adı | Açıklama |
| --- | --- |
| **VideoTrack**<br/><br/> minOccurs = "1" maxOccurs = "sınırsız" |Ana Assetdosyasında belirli bir video parçası. Daha fazla bilgi için bkz. VideoTrack öğesi. |

## <a name="VideoTrack"></a>VideoTrack öğesi
Ana Assetdosyasında belirli bir video parçası.  

XML örnek [XML örneği](#xml)bulabilirsiniz.  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Kimlik**<br/><br/> minInclusive = "0"<br/><br/> Gereklidir |**xs: int** |Bu video izlemenin sıfır tabanlı dizini. **Note:**  Bu **kimliğin** bir MP4 dosyasında kullanılan TrackID olması gerekmez. |
| **FourCC**<br/><br/> Gereklidir |**xs: String** |Video codec FourCC kodu. |
| **Profil** |**xs: String** |H264 profili (yalnızca H264 codec için geçerlidir). |
| **Düzey** |**xs: String** |H264 düzeyi (yalnızca H264 codec için geçerlidir). |
| **Genişlik**<br/><br/> minInclusive = "0"<br/><br/> Gereklidir |**xs: int** |Piksel cinsinden kodlanmış video genişliği. |
| **Yükseklik**<br/><br/> minInclusive = "0"<br/><br/> Gereklidir |**xs: int** |Piksel cinsinden kodlanmış video yüksekliği. |
| **DisplayAspectRatioNumerator**<br/><br/> minInclusive = "0"<br/><br/> Gereklidir |**xs: Double** |Video görüntüleme en boy oranı payı. |
| **Displayaspectratiopayda**<br/><br/> minInclusive = "0"<br/><br/> Gereklidir |**xs: Double** |Video görüntüleme en boy oranı paydası. |
| **Kare hızı değerini**<br/><br/> minInclusive = "0"<br/><br/> Gereklidir |**xs: Decimal** |.3F biçiminde ölçülen video kare hızı. |
| **Targetkare hızı**<br/><br/> minInclusive = "0"<br/><br/> Gereklidir |**xs: Decimal** |.3F biçimindeki hedef video kare hızını önceden ayarlayın. |
| **Bit hızı**<br/><br/> minInclusive = "0"<br/><br/> Gereklidir |**xs: int** |Assetdosyasından hesaplanan saniye başına kilobit cinsinden ortalama video bit hızı. Yalnızca öğesel akış yükünü sayar ve paketleme ek yükünü içermez. |
| **Targetbit hızı**<br/><br/> minInclusive = "0"<br/><br/> Gereklidir |**xs: int** |Kodlama ön ayarı aracılığıyla istenen, saniye başına kilobit olarak bu video izlemenin Ortalama bit hızını hedefleyin. |
| **MaxGOPBitrate**<br/><br/> minInclusive = "0" |**xs: int** |Bu video izlemesi için saniyedeki maksimum GOP Ortalama bit hızı, saniye başına kilobit cinsinden. |

## <a name="AudioTracks"></a>Audioiz öğesi
Her fiziksel Assetdosyası bu dosyada sıfır veya daha fazla ses parçası ile uygun bir kapsayıcı biçimiyle bulunabilir. **Audioizler** öğesi tüm ses izlemelerinin bir koleksiyonunu temsil eder.  

XML örnek [XML örneği](#xml)bulabilirsiniz.  

### <a name="child-elements"></a>Alt öğeleri
| Adı | Açıklama |
| --- | --- |
| **Ses Izi**<br/><br/> minOccurs = "1" maxOccurs = "sınırsız" |Ana Assetdosyasında belirli bir ses parçası. Daha fazla bilgi için bkz. AudioTrack öğesi. |

## <a name="AudioTrack"></a>AudioTrack öğesi
Ana Assetdosyasında belirli bir ses parçası.  

XML örnek [XML örneği](#xml)bulabilirsiniz.  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Kimlik**<br/><br/> minInclusive = "0"<br/><br/> Gereklidir |**xs: int** |Bu ses parçasının sıfır tabanlı dizini. **Note:**  Bu, bir MP4 dosyasında kullanılan TrackID olması gerekmez. |
| **Bileşeni** |**xs: String** |Ses izi codec bileşeni dizesi. |
| **EncoderVersion** |**xs: String** |EAC3 için gerekli olan isteğe bağlı kodlayıcı sürüm dizesi. |
| **Kanallar**<br/><br/> minInclusive = "0"<br/><br/> Gereklidir |**xs: int** |Ses kanalı sayısı. |
| **SamplingRate**<br/><br/> minInclusive = "0"<br/><br/> Gereklidir |**xs: int** |Örnek/sn veya Hz 'teki ses örnekleme oranı. |
| **Bit hızı**<br/><br/> minInclusive = "0"<br/><br/> Gereklidir |**xs: int** |Assetdosyasından hesaplanan, saniye başına bit cinsinden ortalama ses bit hızı. Yalnızca öğesel akış yükünü sayar ve paketleme ek yükünü içermez. |
| **BitsPerSample**<br/><br/> minInclusive = "0"<br/><br/> Gereklidir |**xs: int** |WFormatTag biçim türü için örnek başına bit sayısı. |

### <a name="child-elements"></a>Alt öğeleri
| Adı | Açıklama |
| --- | --- |
| **Ses Neskokteringresultparameters**<br/><br/> minOccurs = "0" maxOccurs = "1" |Ses kullanımı ölçüm sonucu parametreleri. Daha fazla bilgi için bkz. ses Neskokteringresultparameters öğesi. |

## <a name="LoudnessMeteringResultParameters"></a>Ses Neskokteringresultparameters öğesi
Ses kullanımı ölçüm sonucu parametreleri.  

XML örnek [XML örneği](#xml)bulabilirsiniz.  

### <a name="attributes"></a>Öznitelikler
| Adı | Tür | Açıklama |
| --- | --- | --- |
| **Dplmversionınformation** |**xs: String** |**Dolby** Professional ses kullanımı ölçüm geliştirme seti sürümü. |
| **Iletişimnormalleştirme**<br/><br/> minInclusive = "-31" maxInclusive = "-1"<br/><br/> Gereklidir |**xs: int** |DPLM aracılığıyla oluşturulan Dialognormalleştirme, Nessmetering ayarlandığında gereklidir |
| **Tümleştirenlik genişliği**<br/><br/> minInclusive = "-70" maxInclusive = "10"<br/><br/> Gereklidir |**xs: float** |Tümleşik ses kullanımı |
| **Tümleştirenit Nessunit**<br/><br/> Gereklidir |**xs: String** |Tümleşik ses kullanımı birimi. |
| **Integratedstonessgatingmethod**<br/><br/> Gereklidir |**xs: String** |Başarı tanıtıcısı |
| **IntegratedLoudnessSpeechPercentage**<br/><br/> minInclusive = "0" maxInclusive = "100" |**xs: float** |Bir yüzde olarak program üzerinde konuşma içeriği. |
| **Sampletepe**<br/><br/> Gereklidir |**xs: float** |En yüksek mutlak örnek değeri, sıfırlama veya son temizlenme bu yana kanal başına.  Birimler dBFS. |
| **SamplePeakUnit**<br/><br/> Fixed = "dBFS"<br/><br/> Gereklidir |**xs: anySimpleType** |Örnek tepe birimi. |
| **Truetepe**<br/><br/> Gereklidir |**xs: float** |En yüksek gerçek tepe değeri, ITU-R BS. 1770-2 itibariyle, sıfırlama veya son temizlenme sonrasında, kanal başına. Birimler dBTP. |
| **TruePeakUnit**<br/><br/> Fixed = "dBTP"<br/><br/> Gereklidir |**xs: anySimpleType** |Gerçek tepe birimi. |

## <a name="schema-code"></a>Şema kodu
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



## <a name="xml"></a>XML örneği

Aşağıdaki XML, çıkış meta veri dosyasına bir örnektir.  

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

## <a name="provide-feedback"></a>Geri bildirim sağlayın
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
