---
title: Ortak olay biçimi (CEF) anahtarı ve CommonSecurityLog alan eşleme
description: Bu makale, CEF anahtarlarını Azure Sentinel 'teki CommonSecurityLog içindeki ilgili alan adlarına eşler.
services: sentinel
author: batamig
ms.author: bagol
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: reference
ms.date: 03/16/2021
ms.openlocfilehash: 6c23fe86af030d371e12914062bb9558e8db3484
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "104776298"
---
# <a name="cef-and-commonsecuritylog-field-mapping"></a>CEF ve CommonSecurityLog alan eşleme

Aşağıdaki tablolar ortak olay biçimi (CEF) alanı adlarını Azure Sentinel 'in CommonSecurityLog içinde kullandıkları adlara eşler ve Azure Sentinel 'de bir CEF veri kaynağıyla çalışırken yararlı olabilir.

Daha fazla bilgi için bkz. [ortak olay biçimini kullanarak dış çözümünüzü bağlama](connect-common-event-format.md).

## <a name="a---c"></a>A - C

|CEF anahtar adı  |CommonSecurityLog alan adı  |Description  |
|---------|---------|---------|
| çalışanlarının    |    <a name="deviceaction"></a> DeviceAction     |  Olayda bahsedilen eylem.       |
|   Uygulama  |    ApplicationProtocol     |  Uygulamada kullanılan protokol (HTTP, HTTPS, SSHv2, Telnet, POP, ıMPA, ıMAPS vb.).   |
| •    |    EventCount     |  Aynı olayın kaç kez gözlemlendiği gösteren olayla ilişkili bir sayı.       |
| | | |

## <a name="d"></a>D

|CEF anahtar adı  |CommonSecurityLog adı  |Description  |
|---------|---------|---------|
|Cihaz satıcısı     |  Devicevençi       | Cihaz ürün ve sürüm tanımlarıyla birlikte, gönderen cihazın türünü benzersiz şekilde tanımlayan dize.       |
|Cihaz ürünü     |   DeviceProduct      |   Cihaz satıcısı ve sürüm tanımlarıyla birlikte, gönderen cihazın türünü benzersiz şekilde tanımlayan dize.        |
|Cihaz sürümü     |   DeviceVersion      |      Cihaz ürünü ve satıcı tanımlarına sahip olan dize, gönderme cihazının türünü benzersiz şekilde tanımlar.     |
|DeviceEventClassID     |   DeviceEventClassID     |   Olay türü başına benzersiz bir tanımlayıcı görevi gören dize veya tamsayı.      |
| Hedef DNSDomain    | Hedef DNSDomain        |   Tam etki alanı adının (FQDN) DNS bölümü.      |
| destinationServiceName | DestinationServiceName | Olayın hedeflediği hizmet. Örneğin, `sshd`.|
| Destinationtranslatedadddress | Destinationtranslatedadddress | Bir IP ağındaki olay tarafından başvurulan, IPv4 IP adresi olarak çevrilmiş hedefi tanımlar. |
| destinationTranslatedPort | DestinationTranslatedPort | Bir güvenlik duvarı gibi çevirinin ardından bağlantı noktası. <br>Geçerli bağlantı noktası numaraları: `0` - `65535` |
| deviceDirection | <a name="communicationdirection"></a> CommunicationDirection | Gözlemlenen iletişimin alındığı yönle ilgili herhangi bir bilgi. Geçerli değerler: <br>- `0` = Gelen <br>- `1` = Giden |
| deviceDnsDomain | DeviceDnsDomain | Tam nitelikli etki alanı adının (FQDN) DNS etki alanı bölümü |
| Deviceexternalıd | Deviceexternalıd | Olayı oluşturan cihazı benzersiz şekilde tanımlayan bir ad. |
| Devicetesis | Devicetesis | Olayı oluşturan tesis.|
| Deviceınboundınterface | Deviceınboundınterface |Paketin veya verilerin cihaza girdiği arabirim.  |
| deviceNtDomain | DeviceNtDomain | Cihaz adresinin Windows etki alanı |
| Deviceoutboundınterface | Deviceoutboundınterface |Paketin veya verilerin cihazı bıraktı arabirim. |
| Devicepayloadıd |Devicepayloadıd |Olayla ilişkili yük için benzersiz tanımlayıcı. |
| deviceProcessName | ProcessName | Olayla ilişkili işlem adı. <br><br>Örneğin, UNIX 'de Syslog girdisini oluşturan işlem. |
| Devicetranslatedadddress | Devicetranslatedadddress | Bir IP ağında, olayın başvurduğu çevrilmiş cihaz adresini belirler. <br><br>Biçim bir IPv4 adresidir. |
| dhost |Hedef konak adı | Olayın bir IP ağında başvurduğu hedef.  <br>Biçim, bir düğüm kullanılabilir olduğunda hedef düğümle ilişkili bir FQDN olmalıdır. Örneğin `host.domain.com` veya `host` olabilir. |
| DMAC | DestinationMacAddress | Hedef MAC adresi (FQDN) |
| dntdom | Hedef Ntdomain | Hedef adresin Windows etki alanı adı.|
| DPID | Destinationprocessıd |Olayla ilişkili hedef işlemin KIMLIĞI.|
| dprıv | DestinationUserPrivileges | Hedef kullanım ayrıcalıklarını tanımlar. <br>Geçerli değerler: `Admninistrator` , `User` , `Guest` |
| dproc | DestinationProcessName | Olay hedef işleminin adı, örneğin `telnetd` veya `sshd.` |
| DPT | DestinationPort | Hedef bağlantı noktası. <br>Geçerli değerler: `*0` - `65535` |
| HD | Hedef IP | Olayın bir IP ağında başvurduğu hedef IPv4 adresi. |
| DTZ | DeviceTimeZon | Olayı üreten cihazın saat dilimi |
| DUID |Destinationuserıd | Hedef kullanıcıyı KIMLIĞE göre tanımlar. |
| duser | Hedef Kullanıcı adı |Hedef kullanıcıyı ada göre tanımlar.|
| DVC | DeviceAddress | Olayı üreten cihazın IPv4 adresi. |
| dvchost | DeviceName | Bir düğüm kullanılabilir olduğunda cihaz düğümüyle ilişkili FQDN. Örneğin `host.domain.com` veya `host` olabilir.| 
| dvcmac | DeviceMacAddress | Olayı üreten cihazın MAC adresi. |
| dvcpıd | İşlem Kimliği | Olayı üreten cihazdaki işlemin KIMLIĞINI tanımlar. |

## <a name="e---i"></a>E-ı

|CEF anahtar adı  |CommonSecurityLog adı  |Description  |
|---------|---------|---------|
|end     |  EndTime       | Olayla ilgili etkinliğin sona erdiği zaman.        |
|externalId    |   ExternalID      | Kaynak cihaz tarafından kullanılan bir KIMLIK. Genellikle, bu değerler bir olayla ilişkili değerleri artırır.        |
|fileCreateTime     |  FileCreateTime      | Dosyanın oluşturulduğu zaman.        |
|Dosya karması     |   Dosya karması      |   Bir dosyanın karması.      |
|File     |   File      |INode gibi bir dosyayla ilişkili bir KIMLIK.         |
| Dosya Modificationtime | Dosya Modificationtime |Dosyanın son değiştirilme zamanı. |
| Null | Null | Dosya adı da dahil olmak üzere dosyanın tam yolu. Örneğin: `C:\ProgramFiles\WindowsNT\Accessories\wordpad.exe` veya `/usr/bin/zip` .|
| Dosya Izni |Dosya Izni |Dosyanın izinleri. |
| fileType | FileType | Dosya türü (kanal, yuva vb.).|
|fname | Kısaltın| Yol olmadan dosyanın adı. |
| fsize | İ | Dosya boyutu. |
|Ana bilgisayar    |  Bilgisayar       | Ana bilgisayar, syslog 'dan        |
|in     |  ReceivedBytes      |Gelen aktarılan bayt sayısı.         |
| | | |

## <a name="m---p"></a>M-P

|CEF anahtar adı  |CommonSecurityLog adı  |Description  |
|---------|---------|---------|
|msg   |  İleti       | Olayla ilgili daha fazla ayrıntı sağlayan bir ileti.        |
|Name     |  Etkinlik       |   Olayın okunabilir ve anlaşılır bir açıklamasını temsil eden bir dize.     |
|oldFileCreateTime     |  OldFileCreateTime       | Eski dosyanın oluşturulduğu zaman.        |
|oldFileHash     |   OldFileHash      |   Eski dosyanın karması.      |
|Oldfileıd     |   Oldfileıd     |   Ve eski dosyayla ilişkili inode gibi KIMLIK.      |
| oldFileModificationTime | OldFileModificationTime |Eski dosyanın son değiştirilme zamanı. |
| eskiDosya adı |  EskiDosya adı |Eski dosyanın adı. |
| oldFilePath | OldFilePath | Dosya adı da dahil olmak üzere eski dosyanın tam yolu. <br>Örneğin `C:\ProgramFiles\WindowsNT\Accessories\wordpad.exe` veya `/usr/bin/zip` olabilir.|
| oldFilePermission | OldFilePermission |Eski dosyanın izinleri. |
|oldFileSize | OldFileSize | Eski dosyanın boyutu.|
| oldFileType | OldFileType | Eski dosyanın kanal, yuva vb. gibi dosya türü.|
| out | SentBytes | Giden aktarılan bayt sayısı. |
| Sonuç | Sonuç | Etkinliğin sonucu, örneğin `success` veya `failure` .|
|proto    |  Protokol       | Kullanılan katman-4 protokolünü tanımlayan Aktarım Protokolü. <br><br>Olası değerler, veya gibi protokol adlarını içerir `TCP` `UDP` .        |
| | | |

## <a name="r---t"></a>R-T

|CEF anahtar adı  |CommonSecurityLog adı  |Description  |
|---------|---------|---------|
|Nedeni     |  Nedeni      |Bir denetim olayının oluşturulma nedeni. <br><br>Örneğin `Bad password` veya `Unknown user` olabilir.         |
|İstek     |   RequestURL      | Protokol dahil olmak üzere bir HTTP isteği için erişilen URL. Örneğin, `http://www/secure.com`        |
|requestClientApplication     |   RequestClientApplication      |   İstekle ilişkili Kullanıcı Aracısı.      |
| Öğesinden | Öğesinden | HTTP başvuran gibi, isteğin kaynaklandığı içeriği açıklar. |
| requestCookies | RequestCookies |İstekle ilişkili tanımlama bilgileri. |
| requestMethod | RequestMethod | Bir URL 'ye erişmek için kullanılan yöntem. <br><br>Geçerli değerler,, vb. gibi yöntemleri içerir `POST` `GET` . |
| ş | Dönem tSaati | Etkinlikle ilgili olayın alındığı zaman. |
|Önem derecesi     |  <a name="logseverity"></a> Logönem derecesi       |  Olayın önemini açıklayan bir dize veya tamsayı.<br><br> Geçerli dize değerleri: `Unknown` , `Low` , `Medium` , `High` , `Very-High` <br><br>Geçerli tamsayı değerleri şunlardır: `0` - `3` = Low, `4` - `6` = Medium, `7` - `8` = yüksek, `9` - `10` = Very-High |
| Shost    | SourceHostName        |Olayın başvurduğu kaynağı bir IP ağında tanımlar. Biçim, bir düğüm kullanılabilir olduğunda kaynak düğümle ilişkili tam etki alanı adı (DQDN) olmalıdır. Örneğin `host` veya `host.domain.com` olabilir. |
| SMAC | SourceMacAddress | Kaynak MAC adresi. |
| sntdom | SourceNTDomain | Kaynak adresi için Windows etki alanı adı. |
| sourceDnsDomain | SourceDnsDomain | Tam FQDN 'nin DNS etki alanı kısmı. |
| sourceServiceName | SourceServiceName | Olayı oluşturmadan sorumlu hizmet. |
| Sourcetranslatedaddresi | Sourcetranslatedaddresi | Bir IP ağında, olayın başvurduğu çevrilmiş kaynağı tanımlar. |
| sourceTranslatedPort | SourceTranslatedPort | Bir güvenlik duvarı gibi çeviri sonrasında kaynak bağlantı noktası. <br>Geçerli bağlantı noktası numaraları şunlardır `0`  -  `65535` . |
| tamamladığını | SourceProcessId | Olayla ilişkili kaynak işlemin KIMLIĞI.|
| spriv | SourceUserPrivileges | Kaynak kullanıcının ayrıcalıkları. <br><br>Geçerli değerler şunlardır: `Administrator` , `User` , `Guest` |
| sproc | SourceProcessName | Olayın kaynak işleminin adı.|
| SPT | SourcePort | Kaynak bağlantı noktası numarası. <br>Geçerli bağlantı noktası numaraları şunlardır `0`  -  `65535` . |
| src | SourceIP |Bir olayın bir IP ağında başvurduğu kaynak, bir IPv4 adresi olarak. |
| start | StartTime | Olayın başvurduğu etkinliğin başladığı zaman. |
| SUID | Sourceuserıd | Kaynak kullanıcıyı KIMLIĞE göre tanımlar. |
| tür | Olay türü | Olay türü. Değer değerleri şunlardır: <br>- `0`: temel olay <br>- `1`: toplanmış <br>- `2`: bağıntı olayı <br>- `3`: eylem olayı <br><br>**Note**: Bu olay, temel olaylar için atlanabilir. |
| | | |

## <a name="unmapped-fields"></a>Eşlenmemiş alanlar

Aşağıdaki **Commonsecuritylog** alan adlarında CEF anahtarlarında eşlemeler yoktur:


|CommonSecurityLog alan adı  |Description  |
|---------|---------|
|**Originallogönem derecesi**     |  Her zaman boş, Cıcoasa tümleştirmesi için desteklenir. <br>Günlük önem derecesi değerleri hakkında daha fazla bilgi için bkz. [Logönem derecesi](#logseverity) alanı.       |
|**RemoteIP**     |     Uzak IP adresi. <br>Bu değer, mümkünse, [Communicationdirection](#communicationdirection) alanını temel alır.     |
|**RemotePort**     |   Uzak bağlantı noktası. <br>Bu değer, mümkünse, [Communicationdirection](#communicationdirection) alanını temel alır.      |
|**SimplifiedDeviceAction**     |   [Devaction değerini bir](#deviceaction) statik değer kümesine basitleştirir, ancak özgün değeri [deviceaction](#deviceaction) alanında tutarken. <br>Örneğin: `Denied`  >  `Deny` .      |
|     |         |


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [ortak olay biçimini kullanarak dış çözümünüzü bağlama](connect-common-event-format.md).
