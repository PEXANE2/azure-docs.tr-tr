---
title: CRS kural grupları ve kuralları
titleSuffix: Azure Web Application Firewall
description: Bu sayfa, web uygulaması güvenlik duvarı CRS kural grupları ve kuralları hakkında bilgi sağlar.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 11/14/2019
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: 59409c428aba938c49fc37647db82f30d783a629
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81730568"
---
# <a name="web-application-firewall-crs-rule-groups-and-rules"></a>Web Uygulama Güvenlik Duvarı CRS kural grupları ve kuralları

Uygulama Ağ Geçidi web uygulaması güvenlik duvarı (WAF), web uygulamalarını sık karşılaşılan güvenlik açıklarından ve açıklardan korur. Bu, OWASP temel kuralı 3.1, 3.0 veya 2.2.9 kümelerine dayalı olarak tanımlanan kurallar aracılığıyla yapılır. Bu kurallar kural bazında devre dışı edilebilir. Bu makalede, sunulan geçerli kural ve kural kümeleri içerir.

## <a name="core-rule-sets"></a>Çekirdek kural kümeleri

Uygulama Ağ Geçidi WAF varsayılan olarak CRS 3.0 ile önceden yapılandırılmış olarak gelir. Ancak crs 3.1 veya CRS 2.2.9 yerine kullanmayı seçebilirsiniz. CRS 3.1, Java enfeksiyonlarına, dosya yükleme denetimlerinin ilk kümesine, sabit yanlış pozitif lere ve daha fazlasına karşı savunma yapan yeni kural kümeleri sunar. CRS 3.0, CRS 2.2.9 ile karşılaştırıldığında düşük yanlış pozitif sonuçlar sunar. Ayrıca, [gereksinimlerinize uygun kuralları özelleştirebilirsiniz.](application-gateway-customize-waf-rules-portal.md)

> [!div class="mx-imgBorder"]
> ![Kuralları yönetir](../media/application-gateway-crs-rulegroups-rules/managed-rules-01.png)

WAF aşağıdaki web güvenlik açıklarına karşı korur:

- SQL enjeksiyon saldırıları
- Site arası komut dosyası saldırıları
- Komut enjeksiyonu, HTTP istek kaçakçılığı, HTTP yanıt bölme ve uzaktan dosya ekleme gibi diğer yaygın saldırılar
- HTTP protokol ihlalleri
- Eksik ana bilgisayar aracısı ve üstbilgi kabul gibi HTTP protokolü anomalileri
- Botlar, gezginler ve tarayıcıları
- Yaygın uygulama yanlış yapılandırmaları (örneğin, Apache ve IIS)

### <a name="owasp-crs-31"></a>OWASP CRS 3.1

CRS 3.1, aşağıdaki tabloda gösterildiği gibi 13 kural grubu içerir. Her grup devre dışı tutulabilir birden çok kural içerir.

|Kural grubu|Açıklama|
|---|---|
|**[Genel](#general-31)**|Genel grup|
|**[REQUEST-911-METHOD-ENFORCEMENT](#crs911-31)**|Kilitleme yöntemleri (PUT, PATCH)|
|**[REQUEST-913-SCANNER-DETECTION](#crs913-31)**|Bağlantı noktası ve çevre tarayıcılarına karşı koruma|
|**[REQUEST-920-PROTOCOL-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs920-31)**|Protokol ve kodlama sorunlarına karşı koruma|
|**[REQUEST-921-PROTOCOL-ATTACK](#crs921-31)**|Başlık enjeksiyonuna karşı koruyun, kaçakçılık isteğinde bulunun ve yanıt bölünmesi|
|**[REQUEST-930-APPLICATION-ATTACK-LFI](#crs930-31)**|Dosya ve yol saldırılarına karşı koruma|
|**[REQUEST-931-APPLICATION-ATTACK-RFI](#crs931-31)**|Uzak dosya ekleme (RFI) saldırılarına karşı koruma|
|**[REQUEST-932-APPLICATION-ATTACK-RCE](#crs932-31)**|Yine uzak kod yürütme saldırılarını koruma|
|**[REQUEST-933-APPLICATION-ATTACK-PHP](#crs933-31)**|PHP enjeksiyon saldırılarına karşı koruyun|
|**[REQUEST-941-APPLICATION-ATTACK-XSS](#crs941-31)**|Siteler arası komut dosyası saldırılarına karşı koruma|
|**[REQUEST-942-APPLICATION-ATTACK-SQLI](#crs942-31)**|SQL enjeksiyon saldırılarına karşı koruyun|
|**[REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION](#crs943-31)**|Oturum fiksasyon saldırılarına karşı koruyun|
|**[İstek-944-UYGULAMA-SALDıRı-OTURUM-JAVA](#crs944-31)**|JAVA saldırılarına karşı koruyun|

### <a name="owasp-crs-30"></a>OWASP CRS 3.0

CRS 3.0, aşağıdaki tabloda gösterildiği gibi 12 kural grubu içerir. Her grup devre dışı tutulabilir birden çok kural içerir.

|Kural grubu|Açıklama|
|---|---|
|**[Genel](#general-30)**|Genel grup|
|**[REQUEST-911-METHOD-ENFORCEMENT](#crs911-30)**|Kilitleme yöntemleri (PUT, PATCH)|
|**[REQUEST-913-SCANNER-DETECTION](#crs913-30)**|Bağlantı noktası ve çevre tarayıcılarına karşı koruma|
|**[REQUEST-920-PROTOCOL-ENFORCEMENT](application-gateway-crs-rulegroups-rules.md#crs920-30)**|Protokol ve kodlama sorunlarına karşı koruma|
|**[REQUEST-921-PROTOCOL-ATTACK](#crs921-30)**|Başlık enjeksiyonuna karşı koruyun, kaçakçılık isteğinde bulunun ve yanıt bölünmesi|
|**[REQUEST-930-APPLICATION-ATTACK-LFI](#crs930-30)**|Dosya ve yol saldırılarına karşı koruma|
|**[REQUEST-931-APPLICATION-ATTACK-RFI](#crs931-30)**|Uzak dosya ekleme (RFI) saldırılarına karşı koruma|
|**[REQUEST-932-APPLICATION-ATTACK-RCE](#crs932-30)**|Yine uzak kod yürütme saldırılarını koruma|
|**[REQUEST-933-APPLICATION-ATTACK-PHP](#crs933-30)**|PHP enjeksiyon saldırılarına karşı koruyun|
|**[REQUEST-941-APPLICATION-ATTACK-XSS](#crs941-30)**|Siteler arası komut dosyası saldırılarına karşı koruma|
|**[REQUEST-942-APPLICATION-ATTACK-SQLI](#crs942-30)**|SQL enjeksiyon saldırılarına karşı koruyun|
|**[REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION](#crs943-30)**|Oturum fiksasyon saldırılarına karşı koruyun|

### <a name="owasp-crs-229"></a>OWASP CRS 2.2.9

CRS 2.2.9, aşağıdaki tabloda gösterildiği gibi 10 kural grubu içerir. Her grup devre dışı tutulabilir birden çok kural içerir.

|Kural grubu|Açıklama|
|---|---|
|**[crs_20_protocol_violations](#crs20)**|Protokol ihlallerine karşı koruma (geçersiz karakterler veya istek gövdesi ile GET gibi)|
|**[crs_21_protocol_anomalies](#crs21)**|Yanlış üstbilgi bilgilerine karşı koruma|
|**[crs_23_request_limits](#crs23)**|Sınırlamaları aşan bağımsız değişkenler veya dosyalara karşı koruma|
|**[crs_30_http_policy](#crs30)**|Kısıtlı yöntemlere, üstbilgi ve dosya türlerine karşı koruyun|
|**[crs_35_bad_robots](#crs35)**|Web tarayıcılarına ve tarayıcılara karşı koruma|
|**[crs_40_generic_attacks](#crs40)**|Genel saldırılara karşı koruyun (oturum sabitleme, uzaktan dosya ekleme ve PHP enjeksiyonu gibi)|
|**[crs_41_sql_injection_attacks](#crs41sql)**|SQL enjeksiyon saldırılarına karşı koruyun|
|**[crs_41_xss_attacks](#crs41xss)**|Siteler arası komut dosyası saldırılarına karşı koruma|
|**[crs_42_tight_security](#crs42)**|Yol geçişi saldırılarına karşı koruyun|
|**[crs_45_trojans](#crs45)**|Arka kapı truva atlarına karşı koruyun|

Aşağıdaki kural grupları ve kurallar, Uygulama Ağ Geçidi'nde Web Uygulama Güvenlik Duvarı'nı kullanırken kullanılabilir.

# <a name="owasp-31"></a>[OWASP 3.1](#tab/owasp31)

## <a name="rule-sets"></a><a name="owasp31"></a>Kural kümeleri

### <a name="p-x-ms-format-detectionnonegeneralp"></a><a name="general-31"></a> <p x-ms-format-detection="none">Genel</p>

|RuleId|Açıklama|
|---|---|
|200004|Olası Çok Parçalı Eşleşmemiş Sınır.|

### <a name="p-x-ms-format-detectionnonerequest-911-method-enforcementp"></a><a name="crs911-31"></a> <p x-ms-format-detection="none">REQUEST-911-METHOD-ENFORCEMENT</p>

|RuleId|Açıklama|
|---|---|
|911100|Yöntem ilke tarafından izin verilmez|


### <a name="p-x-ms-format-detectionnonerequest-913-scanner-detectionp"></a><a name="crs913-31"></a> <p x-ms-format-detection="none">REQUEST-913-SCANNER-DETECTION</p>

|RuleId|Açıklama|
|---|---|
|913100|Güvenlik tarayıcısıyla ilişkili Kullanıcı Aracısı bulundu|
|913101|Komut dosyası/genel HTTP istemcisi ile ilişkili Kullanıcı Aracısı bulundu|
|913102|Web tarayıcısı/botu ile ilişkili Kullanıcı Aracısı bulundu|
|913110|Güvenlik tarayıcısıyla ilişkili istek üstbilgisi bulundu|
|913120|Güvenlik tarayıcısıyla ilişkili istek dosya adı/bağımsız değişkeni bulundu|


### <a name="p-x-ms-format-detectionnonerequest-920-protocol-enforcementp"></a><a name="crs920-31"></a> <p x-ms-format-detection="none">REQUEST-920-PROTOCOL-ENFORCEMENT</p>

|RuleId|Açıklama|
|---|---|
|920100|Geçersiz HTTP İstek Satırı|
|920120|Çok parçalı/form-veri bypass girişimi|
|920121|Çok parçalı/form-veri bypass girişimi|
|920130|İstek gövdesini ayrışdırmak için başarısız oldu.|
|920140|Çok parçalı istek gövdesi katı doğrulama başarısız|
|920160|İçerik Uzunluğu HTTP üstbilgi sayısal değildir.|
|920170|Vücut İçeriği ile GET veya HEAD İstek.|
|920171|Transfer-Kodlama ile GET veya HEAD İstek.|
|920180|POST isteği eksik İçerik Uzunluğu Üstbilgi.|
|920190|Aralık = Geçersiz Son Bayt Değeri.|
|920200|Aralık = Çok fazla alan (6 veya daha fazla)|
|920201|Aralık = Pdf isteği için çok fazla alan (35 veya daha fazla)|
|920202|Aralık = Pdf isteği için çok fazla alan (6 veya daha fazla)|
|920210|Çoklu/Çakışan Bağlantı Üstbilgisi Verisi Bulundu.|
|920220|URL Kodlama Kötüye Saldırı Girişimi|
|920230|Birden Çok URL Kodlama Sıyrık Algılandı|
|920240|URL Kodlama Kötüye Saldırı Girişimi|
|920250|UTF8 Kodlama Kötüye Saldırı Girişimi|
|920260|Unicode Tam/Yarım Genişlik kötüye saldırı girişimi|
|920270|İstekte geçersiz karakter (null karakter)|
|920271|İstekteki geçersiz karakter (yazdırılmaz karakterler)|
|920272|İstekte geçersiz karakter (ascii 127'nin altındaki yazdırılabilir karakter dışında)|
|920273|İstekte geçersiz karakter (çok sıkı bir küme dışında)|
|920274|İstek üstbilgilerinde geçersiz karakter (çok sıkı kümenin dışında)|
|920280|Ana Bilgisayar Üstbilginin Eksik İsten|
|920290|Boş Ana Bilgisayar Üstbilgi|
|920300|Kabul Üstbilginin Eksik İsten|
|920310|İstek Boş Kabul Üstbilgisi Var|
|920311|İstek Boş Kabul Üstbilgisi Var|
|920320|Eksik Kullanıcı Aracısı Üstbilgi|
|920330|Boş Kullanıcı Aracısı Üstbilgi|
|920340|İçerik İçeren Ancak Eksik İçerik Türü üstbilgi İstek|
|920341|İçerik içeren istek İçerik Türü üstbilgi gerektirir|
|920350|Ana bilgisayar üstbilgi sayısal bir IP adresidir|
|920360|Bağımsız değişken adı çok uzun|
|920370|Bağımsız değişken değeri çok uzun|
|920380|İstekte çok fazla bağımsız değişken|
|920390|Toplam bağımsız değişken boyutu aşıldı|
|920400|Yüklenen dosya boyutu çok büyük|
|920410|Toplam yüklenen dosya boyutu çok büyük|
|920420|İstek içerik türü ilke tarafından izin verilmez|
|920430|HTTP protokolü sürümü ilke tarafından izin verilmez|
|920440|URL dosya uzantısı ilke ile sınırlıdır|
|920450|HTTP üstbilgi ilkesi yle sınırlıdır (%@{MATCHED_VAR})|
|920460|Anormal Kaçış Karakterleri|
|920470|Yasadışı İçerik Türü üstbilgi|
|920480|İçerik türü üstbilgiiçinde charset parametresini kısıtlama|

### <a name="p-x-ms-format-detectionnonerequest-921-protocol-attackp"></a><a name="crs921-31"></a> <p x-ms-format-detection="none">REQUEST-921-PROTOCOL-ATTACK</p>

|RuleId|Açıklama|
|---|---|
|921110|HTTP İstek Kaçakçılık Saldırısı|
|921120|HTTP Yanıt Bölme Saldırısı|
|921130|HTTP Yanıt Bölme Saldırısı|
|921140|Üstbilgi ile HTTP Başlık Enjeksiyon Saldırısı|
|921150|YÜK YOLUYLA HTTP Başlık Enjeksiyon Saldırısı (CR/LF algılandı)|
|921151|YÜK YOLUYLA HTTP Başlık Enjeksiyon Saldırısı (CR/LF algılandı)|
|921160|Taşıma yükü ile HTTP Üstbilgi Enjeksiyon Saldırısı (CR/LF ve üstbilgi adı algılandı)|
|921170|HTTP Parametre Kirliliği|
|921180|HTTP Parametre Kirliliği (%{TX.1})|

### <a name="p-x-ms-format-detectionnonerequest-930-application-attack-lfip"></a><a name="crs930-31"></a> <p x-ms-format-detection="none">REQUEST-930-APPLICATION-ATTACK-LFI</p>

|RuleId|Açıklama|
|---|---|
|930100|Yol Geçiş Saldırısı (/.... /)|
|930110|Yol Geçiş Saldırısı (/.... /)|
|930120|İşletim Sistemi Dosya Erişim Girişimi|
|930130|Kısıtlı Dosya Erişim Denemesi|

### <a name="p-x-ms-format-detectionnonerequest-931-application-attack-rfip"></a><a name="crs931-31"></a> <p x-ms-format-detection="none">REQUEST-931-APPLICATION-ATTACK-RFI</p>

|RuleId|Açıklama|
|---|---|
|931100|Olası Uzaktan Dosya Ekleme (RFI) Saldırısı = IP Adresini Kullanarak URL Parametresi|
|931110|Olası Uzak Dosya Ekleme (RFI) Saldırısı = W/URL Yükü kullanılan Ortak RFI Güvenlik Açığı Olan Parametre Adı|
|931120|Olası Uzak Dosya Ekleme (RFI) Saldırı = URL Payload Kullanılan w / Trailing Soru İşareti Karakteri (?)|
|931130|Olası Uzak Dosya Ekleme (RFI) Saldırısı = Etki Alanı Dışı Başvuru/Bağlantı|

### <a name="p-x-ms-format-detectionnonerequest-932-application-attack-rcep"></a><a name="crs932-31"></a> <p x-ms-format-detection="none">REQUEST-932-APPLICATION-ATTACK-RCE</p>

|RuleId|Açıklama|
|---|---|
|932100|Uzaktan Komut Yürütme: Unix Komut Enjeksiyon|
|932105|Uzaktan Komut Yürütme: Unix Komut Enjeksiyon|
|932106|Uzaktan Komut Yürütme: Unix Komut Enjeksiyon|
|932110|Uzaktan Komut Yürütme: Windows Komut Enjeksiyon|
|932115|Uzaktan Komut Yürütme: Windows Komut Enjeksiyon|
|932120|Uzaktan Komut Yürütme = Windows PowerShell Komutu Bulundu|
|932130|Uzaktan Komut Yürütme = Unix Kabuk İfadesi Bulundu|
|932140|Uzaktan Komut Yürütme = Windows FOR/IF Komutu Bulundu|
|932150|Uzaktan Komut Yürütme: Doğrudan Unix Komut Yürütme|
|932160|Uzaktan Komut Yürütme = Unix Kabuk Kodu Bulundu|
|932170|Uzaktan Komut Yürütme = Shellshock (CVE-2014-6271)|
|932171|Uzaktan Komut Yürütme = Shellshock (CVE-2014-6271)|
|932180|Kısıtlı Dosya Yükleme Girişimi|
|932190|Uzaktan Komut Yürütme: Joker bypass tekniği girişimi|

### <a name="p-x-ms-format-detectionnonerequest-933-application-attack-phpp"></a><a name="crs933-31"></a> <p x-ms-format-detection="none">REQUEST-933-APPLICATION-ATTACK-PHP</p>

|RuleId|Açıklama|
|---|---|
|933100|PHP Enjeksiyon Atağı = Açma/Kapanış Etiketi Bulundu|
|933110|PHP Enjeksiyon Saldırısı = PHP Script Dosya Yükleme bulundu|
|933111|PHP Enjeksiyon Saldırısı: PHP Script Dosya Yükleme bulundu|
|933120|PHP Enjeksiyon Atağı = Yapılandırma Yönergesi Bulundu|
|933130|PHP Enjeksiyon Atağı = Bulunan Değişkenler|
|933131|PHP Enjeksiyon Saldırısı: Değişkenler Bulundu|
|933140|PHP Enjeksiyon Saldırısı: I/O Stream Bulundu|
|933150|PHP Enjeksiyon Atağı = Yüksek Riskli PHP Fonksiyon Adı Bulundu|
|933151|PHP Enjeksiyon Atağı: Orta Riskli PHP Fonksiyon Adı Bulundu|
|933160|PHP Enjeksiyon Atağı = Yüksek Riskli PHP Fonksiyonu Araması Bulundu|
|933161|PHP Enjeksiyon Atağı: Düşük Değerli PHP Fonksiyonu Çağrısı Bulundu|
|933170|PHP Enjeksiyon Saldırısı: Serileştirilmiş Nesne Enjeksiyonu|
|933180|PHP Enjeksiyon Atağı = Değişken Fonksiyonlu Çağrı Bulundu|
|933190|PHP Enjeksiyon Saldırısı: PHP Kapanış Etiketi Bulundu|

### <a name="p-x-ms-format-detectionnonerequest-941-application-attack-xssp"></a><a name="crs941-31"></a> <p x-ms-format-detection="none">REQUEST-941-APPLICATION-ATTACK-XSS</p>

|RuleId|Açıklama|
|---|---|
|941100|Libinjection Ile XSS Saldırısı Algılandı|
|941101|Libinjection Ile XSS Saldırısı Algılandı|
|941110|XSS Filtresi - Kategori 1 = Komut Dosyası Etiket Vektörü|
|941130|XSS Filtresi - Kategori 3 = Öznitelik Vektörü|
|941140|XSS Filtresi - Kategori 4 = Javascript URI Vektör|
|941150|XSS Filtresi - Kategori 5 = İzin Verilmeyen HTML Öznitelikleri|
|941160|NoScript XSS Enjeksiyon KontrolÖrü: HTML Enjeksiyon|
|941170|NoScript XSS Enjeksiyon KontrolÖrü: Öznitelik Enjeksiyon|
|941180|Düğüm-Validator Kara Liste Anahtar Kelimeler|
|941190|Stil sayfaları kullanarak XSS|
|941200|VML çerçeveleri kullanan XSS|
|941210|XSS obfuscated Javascript kullanarak|
|941220|XSS, obfuscated VB Script kullanarak|
|941230|XSS 'gömme' etiketini kullanarak|
|941240|XSS 'alma' veya 'uygulama' özniteliği kullanarak|
|941250|IE XSS Filtreler - Saldırı Algılandı|
|941260|XSS 'meta' etiketini kullanarak|
|941270|XSS kullanarak 'bağlantı' href|
|941280|XSS 'base' etiketini kullanarak|
|941290|XSS 'applet' etiketini kullanarak|
|941300|XSS 'nesne' etiketini kullanarak|
|941310|US-ASCII Malformasyonlu Kodlama XSS Filtresi - Saldırı Algılandı.|
|941320|Olası XSS Saldırısı Algılandı - HTML Etiket Işleyicisi|
|941330|IE XSS Filtreler - Saldırı Algılandı.|
|941340|IE XSS Filtreler - Saldırı Algılandı.|
|941350|UTF-7 Kodlama IE XSS - Saldırı Algılandı.|


### <a name="p-x-ms-format-detectionnonerequest-942-application-attack-sqlip"></a><a name="crs942-31"></a> <p x-ms-format-detection="none">REQUEST-942-APPLICATION-ATTACK-SQLI</p>

|RuleId|Açıklama|
|---|---|
|942100|LIBINJECTION Ile Algılanan SQL Enjeksiyon Saldırısı|
|942110|SQL Enjeksiyon Saldırısı: Ortak Enjeksiyon Testi Algılandı|
|942120|SQL Enjeksiyon Saldırısı: SQL Operatörü Algılandı|
|942130|SQL Enjeksiyon Saldırısı: SQL Tautology Algılandı.|
|942140|SQL Injection Attack = Ortak DB Adları Algılandı|
|942150|SQL Enjeksiyon Saldırısı|
|942160|Uyku() veya benchmark() kullanarak kör sqli testlerini algılar.|
|942170|Koşullu sorgular da dahil olmak üzere SQL kıyaslama ve uyku enjeksiyon uyrama girişimlerini algılar|
|942180|Temel SQL kimlik doğrulama yan laştırma denemelerini algılar 1/3|
|942190|MSSQL kod yürütme ve bilgi toplama girişimlerini algılar|
|942200|MySQL yorum-/boşluk obfuscated enjeksiyonları ve backtick sonlandırma algılar|
|942210|Zincirli SQL enjeksiyon girişimlerini algılar 1/2|
|942220|3.0.00738585072 hariç, tümsedo taşma saldırıları arıyorsunuz, bu skipfish alınır|
|942230|Koşullu SQL enjeksiyon girişimlerini algılar|
|942240|MySQL charset anahtarını algılar ve MSSQL DoS denemelerini|
|942250|MATCH AGAINST, MERGE ve HEMEN ENJEKSIYONLARI İnFAZ ALGıLAR|
|942251|Enjeksiyonları algılar|
|942260|Temel SQL kimlik doğrulama yan laştırma denemelerini algılar 2/3|
|942270|Temel sql enjeksiyon arıyorsunuz. mysql oracle ve diğerleri için ortak saldırı dizesi|
|942280|Postgres pg_sleep enjeksiyonu algılar, gecikme saldırılarını bekler ve veritabanı kapatma girişimlerini bekler|
|942290|Temel MongoDB SQL enjeksiyon denemelerini bulur|
|942300|MySQL yorumlarını, koşullarını ve ch(a)r enjeksiyonlarını algılar|
|942310|Zincirli SQL enjeksiyon girişimlerini algılar 2/2|
|942320|MySQL ve PostgreSQL depolanan yordam/fonksiyon enjeksiyonlarını algılar|
|942330|Klasik SQL enjeksiyon sondalarını algılar|
|942340|Temel SQL kimlik doğrulama yan laştırma girişimlerini algılar 3/3|
|942350|MySQL UDF enjeksiyonu ve diğer veri/yapı işleme girişimlerini algılar|
|942360|Concatenated basic SQL enjeksiyonunu ve SQLLFI girişimlerini algılar|
|942361|Anahtar kelime değiştirme veya birleşime dayalı temel SQL enjeksiyonu algılar|
|942370|Klasik SQL enjeksiyon sondalarını algılar 2/2|
|942380|SQL Enjeksiyon Saldırısı|
|942390|SQL Enjeksiyon Saldırısı|
|942400|SQL Enjeksiyon Saldırısı|
|942410|SQL Enjeksiyon Saldırısı|
|942420|Sınırlı SQL Karakter Anomali Algılama (çerezler): # özel karakter (8) aştı|
|942421|Sınırlı SQL Karakter Anomali Algılama (çerezler): # özel karakter (3) aştı|
|942430|Kısıtlı SQL Karakter Anomali Algılama (args): # özel karakter (12) aştı|
|942431|Kısıtlı SQL Karakter Anomali Algılama (args): # özel karakter (6) aştı|
|942432|Kısıtlı SQL Karakter Anomali Algılama (args): # özel karakter (2) aştı|
|942440|SQL Yorum Sırası Algılandı.|
|942450|SQL Hex Kodlama Tanımlı|
|942460|Meta-Karakter Anomali Algılama Uyarısı - Tekrarlayan Non-Word Karakterler|
|942470|SQL Enjeksiyon Saldırısı|
|942480|SQL Enjeksiyon Saldırısı|
|942490|Klasik SQL enjeksiyon sondalarını algılar 3/3|

### <a name="p-x-ms-format-detectionnonerequest-943-application-attack-session-fixationp"></a><a name="crs943-31"></a> <p x-ms-format-detection="none">REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION</p>

|RuleId|Açıklama|
|---|---|
|943100|Olası Oturum Sabitleme Atağı = HTML'de Çerez Değerlerini Ayarlama|
|943110|Olası Oturum Fiksasyon Saldırısı = Off-Domain Referrer ile SessionID Parametre Adı|
|943120|Olası Oturum Sabitleme Atağı = OturumKIMLIĞI Parametre Adı|

### <a name="p-x-ms-format-detectionnonerequest-944-application-attack-session-javap"></a><a name="crs944-31"></a> <p x-ms-format-detection="none">İstek-944-UYGULAMA-SALDıRı-OTURUM-JAVA</p>

|RuleId|Açıklama|
|---|---|
|944120|Olası yük yürütme ve uzaktan komut yürütme|
|944130|Şüpheli Java sınıfları|
|944200|Java deserialization Apache Commons sömürü|

# <a name="owasp-30"></a>[OWASP 3.0](#tab/owasp30)

## <a name="rule-sets"></a><a name="owasp30"></a>Kural kümeleri

### <a name="p-x-ms-format-detectionnonegeneralp"></a><a name="general-30"></a> <p x-ms-format-detection="none">Genel</p>

|RuleId|Açıklama|
|---|---|
|200004|Olası Çok Parçalı Eşleşmemiş Sınır.|

### <a name="p-x-ms-format-detectionnonerequest-911-method-enforcementp"></a><a name="crs911-30"></a> <p x-ms-format-detection="none">REQUEST-911-METHOD-ENFORCEMENT</p>

|RuleId|Açıklama|
|---|---|
|911100|Yöntem ilke tarafından izin verilmez|


### <a name="p-x-ms-format-detectionnonerequest-913-scanner-detectionp"></a><a name="crs913-30"></a> <p x-ms-format-detection="none">REQUEST-913-SCANNER-DETECTION</p>

|RuleId|Açıklama|
|---|---|
|913100|Güvenlik tarayıcısıyla ilişkili Kullanıcı Aracısı bulundu|
|913110|Güvenlik tarayıcısıyla ilişkili istek üstbilgisi bulundu|
|913120|Güvenlik tarayıcısıyla ilişkili istek dosya adı/bağımsız değişkeni bulundu|
|913101|Komut dosyası/genel HTTP istemcisi ile ilişkili Kullanıcı Aracısı bulundu|
|913102|Web tarayıcısı/botu ile ilişkili Kullanıcı Aracısı bulundu|

### <a name="p-x-ms-format-detectionnonerequest-920-protocol-enforcementp"></a><a name="crs920-30"></a> <p x-ms-format-detection="none">REQUEST-920-PROTOCOL-ENFORCEMENT</p>

|RuleId|Açıklama|
|---|---|
|920100|Geçersiz HTTP İstek Satırı|
|920130|İstek gövdesini ayrışdırmak için başarısız oldu.|
|920140|Çok parçalı istek gövdesi katı doğrulama başarısız|
|920160|İçerik Uzunluğu HTTP üstbilgi sayısal değildir.|
|920170|Vücut İçeriği ile GET veya HEAD İstek.|
|920180|POST isteği eksik İçerik Uzunluğu Üstbilgi.|
|920190|Aralık = Geçersiz Son Bayt Değeri.|
|920210|Çoklu/Çakışan Bağlantı Üstbilgisi Verisi Bulundu.|
|920220|URL Kodlama Kötüye Saldırı Girişimi|
|920240|URL Kodlama Kötüye Saldırı Girişimi|
|920250|UTF8 Kodlama Kötüye Saldırı Girişimi|
|920260|Unicode Tam/Yarım Genişlik kötüye saldırı girişimi|
|920270|İstekte geçersiz karakter (null karakter)|
|920280|Ana Bilgisayar Üstbilginin Eksik İsten|
|920290|Boş Ana Bilgisayar Üstbilgi|
|920310|İstek Boş Kabul Üstbilgisi Var|
|920311|İstek Boş Kabul Üstbilgisi Var|
|920330|Boş Kullanıcı Aracısı Üstbilgi|
|920340|İçerik İçeren Ancak Eksik İçerik Türü üstbilgi İstek|
|920350|Ana bilgisayar üstbilgi sayısal bir IP adresidir|
|920380|İstekte çok fazla bağımsız değişken|
|920360|Bağımsız değişken adı çok uzun|
|920370|Bağımsız değişken değeri çok uzun|
|920390|Toplam bağımsız değişken boyutu aşıldı|
|920400|Yüklenen dosya boyutu çok büyük|
|920410|Toplam yüklenen dosya boyutu çok büyük|
|920420|İstek içerik türü ilke tarafından izin verilmez|
|920430|HTTP protokolü sürümü ilke tarafından izin verilmez|
|920440|URL dosya uzantısı ilke ile sınırlıdır|
|920450|HTTP üstbilgi ilkesi yle sınırlıdır (%@{MATCHED_VAR})|
|920200|Aralık = Çok fazla alan (6 veya daha fazla)|
|920201|Aralık = Pdf isteği için çok fazla alan (35 veya daha fazla)|
|920230|Birden Çok URL Kodlama Sıyrık Algılandı|
|920300|Kabul Üstbilginin Eksik İsten|
|920271|İstekteki geçersiz karakter (yazdırılmaz karakterler)|
|920320|Eksik Kullanıcı Aracısı Üstbilgi|
|920272|İstekte geçersiz karakter (ascii 127'nin altındaki yazdırılabilir karakter dışında)|
|920202|Aralık = Pdf isteği için çok fazla alan (6 veya daha fazla)|
|920273|İstekte geçersiz karakter (çok sıkı bir küme dışında)|
|920274|İstek üstbilgilerinde geçersiz karakter (çok sıkı kümenin dışında)|
|920460|Anormal kaçış karakterleri|

### <a name="p-x-ms-format-detectionnonerequest-921-protocol-attackp"></a><a name="crs921-30"></a> <p x-ms-format-detection="none">REQUEST-921-PROTOCOL-ATTACK</p>

|RuleId|Açıklama|
|---|---|
|921100|HTTP İstek Kaçakçılık Saldırı.|
|921110|HTTP İstek Kaçakçılık Saldırısı|
|921120|HTTP Yanıt Bölme Saldırısı|
|921130|HTTP Yanıt Bölme Saldırısı|
|921140|Üstbilgi ile HTTP Başlık Enjeksiyon Saldırısı|
|921150|YÜK YOLUYLA HTTP Başlık Enjeksiyon Saldırısı (CR/LF algılandı)|
|921160|Taşıma yükü ile HTTP Üstbilgi Enjeksiyon Saldırısı (CR/LF ve üstbilgi adı algılandı)|
|921151|YÜK YOLUYLA HTTP Başlık Enjeksiyon Saldırısı (CR/LF algılandı)|
|921170|HTTP Parametre Kirliliği|
|921180|HTTP Parametre Kirliliği (%@{TX.1})|

### <a name="p-x-ms-format-detectionnonerequest-930-application-attack-lfip"></a><a name="crs930-30"></a> <p x-ms-format-detection="none">REQUEST-930-APPLICATION-ATTACK-LFI</p>

|RuleId|Açıklama|
|---|---|
|930100|Yol Geçiş Saldırısı (/.... /)|
|930110|Yol Geçiş Saldırısı (/.... /)|
|930120|İşletim Sistemi Dosya Erişim Girişimi|
|930130|Kısıtlı Dosya Erişim Denemesi|

### <a name="p-x-ms-format-detectionnonerequest-931-application-attack-rfip"></a><a name="crs931-30"></a> <p x-ms-format-detection="none">REQUEST-931-APPLICATION-ATTACK-RFI</p>

|RuleId|Açıklama|
|---|---|
|931100|Olası Uzaktan Dosya Ekleme (RFI) Saldırısı = IP Adresini Kullanarak URL Parametresi|
|931110|Olası Uzak Dosya Ekleme (RFI) Saldırısı = W/URL Yükü kullanılan Ortak RFI Güvenlik Açığı Olan Parametre Adı|
|931120|Olası Uzak Dosya Ekleme (RFI) Saldırı = URL Payload Kullanılan w / Trailing Soru İşareti Karakteri (?)|
|931130|Olası Uzak Dosya Ekleme (RFI) Saldırısı = Etki Alanı Dışı Başvuru/Bağlantı|

### <a name="p-x-ms-format-detectionnonerequest-932-application-attack-rcep"></a><a name="crs932-30"></a> <p x-ms-format-detection="none">REQUEST-932-APPLICATION-ATTACK-RCE</p>

|RuleId|Açıklama|
|---|---|
|932120|Uzaktan Komut Yürütme = Windows PowerShell Komutu Bulundu|
|932130|Uzaktan Komut Yürütme = Unix Kabuk İfadesi Bulundu|
|932140|Uzaktan Komut Yürütme = Windows FOR/IF Komutu Bulundu|
|932160|Uzaktan Komut Yürütme = Unix Kabuk Kodu Bulundu|
|932170|Uzaktan Komut Yürütme = Shellshock (CVE-2014-6271)|
|932171|Uzaktan Komut Yürütme = Shellshock (CVE-2014-6271)|

### <a name="p-x-ms-format-detectionnonerequest-933-application-attack-phpp"></a><a name="crs933-30"></a> <p x-ms-format-detection="none">REQUEST-933-APPLICATION-ATTACK-PHP</p>

|RuleId|Açıklama|
|---|---|
|933100|PHP Enjeksiyon Atağı = Açma/Kapanış Etiketi Bulundu|
|933110|PHP Enjeksiyon Saldırısı = PHP Script Dosya Yükleme bulundu|
|933120|PHP Enjeksiyon Atağı = Yapılandırma Yönergesi Bulundu|
|933130|PHP Enjeksiyon Atağı = Bulunan Değişkenler|
|933150|PHP Enjeksiyon Atağı = Yüksek Riskli PHP Fonksiyon Adı Bulundu|
|933160|PHP Enjeksiyon Atağı = Yüksek Riskli PHP Fonksiyonu Araması Bulundu|
|933180|PHP Enjeksiyon Atağı = Değişken Fonksiyonlu Çağrı Bulundu|
|933151|PHP Enjeksiyon Atağı = Orta Riskli PHP Fonksiyon Adı Bulundu|
|933131|PHP Enjeksiyon Atağı = Bulunan Değişkenler|
|933161|PHP Enjeksiyon Atağı = Düşük Değerli PHP Fonksiyonu Çağrısı Bulundu|
|933111|PHP Enjeksiyon Saldırısı = PHP Script Dosya Yükleme bulundu|

### <a name="p-x-ms-format-detectionnonerequest-941-application-attack-xssp"></a><a name="crs941-30"></a> <p x-ms-format-detection="none">REQUEST-941-APPLICATION-ATTACK-XSS</p>

|RuleId|Açıklama|
|---|---|
|941100|Libinjection Ile XSS Saldırısı Algılandı|
|941110|XSS Filtresi - Kategori 1 = Komut Dosyası Etiket Vektörü|
|941130|XSS Filtresi - Kategori 3 = Öznitelik Vektörü|
|941140|XSS Filtresi - Kategori 4 = Javascript URI Vektör|
|941150|XSS Filtresi - Kategori 5 = İzin Verilmeyen HTML Öznitelikleri|
|941180|Düğüm-Validator Kara Liste Anahtar Kelimeler|
|941190|Stil sayfaları kullanarak XSS|
|941200|VML çerçeveleri kullanan XSS|
|941210|XSS obfuscated Javascript kullanarak|
|941220|XSS, obfuscated VB Script kullanarak|
|941230|XSS 'gömme' etiketini kullanarak|
|941240|XSS 'alma' veya 'uygulama' özniteliği kullanarak|
|941260|XSS 'meta' etiketini kullanarak|
|941270|XSS kullanarak 'bağlantı' href|
|941280|XSS 'base' etiketini kullanarak|
|941290|XSS 'applet' etiketini kullanarak|
|941300|XSS 'nesne' etiketini kullanarak|
|941310|US-ASCII Malformasyonlu Kodlama XSS Filtresi - Saldırı Algılandı.|
|941330|IE XSS Filtreler - Saldırı Algılandı.|
|941340|IE XSS Filtreler - Saldırı Algılandı.|
|941350|UTF-7 Kodlama IE XSS - Saldırı Algılandı.|
|941320|Olası XSS Saldırısı Algılandı - HTML Etiket Işleyicisi|

### <a name="p-x-ms-format-detectionnonerequest-942-application-attack-sqlip"></a><a name="crs942-30"></a> <p x-ms-format-detection="none">REQUEST-942-APPLICATION-ATTACK-SQLI</p>

|RuleId|Açıklama|
|---|---|
|942100|LIBINJECTION Ile Algılanan SQL Enjeksiyon Saldırısı|
|942110|SQL Enjeksiyon Saldırısı: Ortak Enjeksiyon Testi Algılandı|
|942130|SQL Enjeksiyon Saldırısı: SQL Tautology Algılandı.|
|942140|SQL Injection Attack = Ortak DB Adları Algılandı|
|942160|Uyku() veya benchmark() kullanarak kör sqli testlerini algılar.|
|942170|Koşullu sorgular da dahil olmak üzere SQL kıyaslama ve uyku enjeksiyon uyrama girişimlerini algılar|
|942190|MSSQL kod yürütme ve bilgi toplama girişimlerini algılar|
|942200|MySQL yorum-/boşluk obfuscated enjeksiyonları ve backtick sonlandırma algılar|
|942230|Koşullu SQL enjeksiyon girişimlerini algılar|
|942260|Temel SQL kimlik doğrulama yan laştırma denemelerini algılar 2/3|
|942270|Temel sql enjeksiyon arıyorsunuz. mysql oracle ve diğerleri için ortak saldırı dizesi.|
|942290|Temel MongoDB SQL enjeksiyon denemelerini bulur|
|942300|MySQL yorumlarını, koşullarını ve ch(a)r enjeksiyonlarını algılar|
|942310|Zincirli SQL enjeksiyon girişimlerini algılar 2/2|
|942320|MySQL ve PostgreSQL depolanan yordam/fonksiyon enjeksiyonlarını algılar|
|942330|Klasik SQL enjeksiyon sondalarını algılar|
|942340|Temel SQL kimlik doğrulama yan laştırma girişimlerini algılar 3/3|
|942350|MySQL UDF enjeksiyonu ve diğer veri/yapı işleme girişimlerini algılar|
|942360|Concatenated basic SQL enjeksiyonunu ve SQLLFI girişimlerini algılar|
|942370|Klasik SQL enjeksiyon sondalarını algılar 2/2|
|942150|SQL Enjeksiyon Saldırısı|
|942410|SQL Enjeksiyon Saldırısı|
|942430|Kısıtlı SQL Karakter Anomali Algılama (args): # özel karakter (12) aştı|
|942440|SQL Yorum Sırası Algılandı.|
|942450|SQL Hex Kodlama Tanımlı|
|942251|Enjeksiyonları algılar|
|942460|Meta-Karakter Anomali Algılama Uyarısı - Tekrarlayan Non-Word Karakterler|

### <a name="p-x-ms-format-detectionnonerequest-943-application-attack-session-fixationp"></a><a name="crs943-30"></a> <p x-ms-format-detection="none">REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION</p>

|RuleId|Açıklama|
|---|---|
|943100|Olası Oturum Sabitleme Atağı = HTML'de Çerez Değerlerini Ayarlama|
|943110|Olası Oturum Fiksasyon Saldırısı = Off-Domain Referrer ile SessionID Parametre Adı|
|943120|Olası Oturum Sabitleme Atağı = OturumKIMLIĞI Parametre Adı|

# <a name="owasp-229"></a>[OWASP 2.2.9](#tab/owasp2)

## <a name="rule-sets"></a><a name="owasp229"></a>Kural kümeleri

### <a name="crs_20_protocol_violations"></a><a name="crs20"></a>crs_20_protocol_violations

|RuleId|Açıklama|
|---|---|
|960911|Geçersiz HTTP İstek Satırı|
|981227|Apache Hatası = İstek'te Geçersiz URI.|
|960912|İstek gövdesini ayrışdırmak için başarısız oldu.|
|960914|Çok parçalı istek gövdesi katı doğrulama başarısız|
|960915|Çok parçalı ayrıştırıcı olası bir eşleşmez sınır algıladı.|
|960016|İçerik Uzunluğu HTTP üstbilgi sayısal değildir.|
|960011|Vücut İçeriği ile GET veya HEAD İstek.|
|960012|POST isteği eksik İçerik Uzunluğu Üstbilgi.|
|960902|Kimlik Kodlamasının Geçersiz Kullanımı.|
|960022|HTTP 1.0 için Başlık İzin Verilmez bekleyin.|
|960020|Pragma Üstbilgi, HTTP/1.1 istekleri için Önbellek Denetimi Üstbilgisi gerektirir.|
|958291|Aralık = alan var ve 0 ile başlar.|
|958230|Aralık = Geçersiz Son Bayt Değeri.|
|958295|Çoklu/Çakışan Bağlantı Üstbilgisi Verisi Bulundu.|
|950107|URL Kodlama Kötüye Saldırı Girişimi|
|950109|Birden Çok URL Kodlama Sıyrık Algılandı|
|950108|URL Kodlama Kötüye Saldırı Girişimi|
|950801|UTF8 Kodlama Kötüye Saldırı Girişimi|
|950116|Unicode Tam/Yarım Genişlik kötüye saldırı girişimi|
|960901|İstekte geçersiz karakter|
|960018|İstekte geçersiz karakter|

### <a name="crs_21_protocol_anomalies"></a><a name="crs21"></a>crs_21_protocol_anomalies

|RuleId|Açıklama|
|---|---|
|960008|Ana Bilgisayar Üstbilginin Eksik İsten|
|960007|Boş Ana Bilgisayar Üstbilgi|
|960015|Kabul Üstbilginin Eksik İsten|
|960021|İstek Boş Kabul Üstbilgisi Var|
|960009|Kullanıcı Aracısı Üstbilginin Eksik Olduğunu İsteyin|
|960006|Boş Kullanıcı Aracısı Üstbilgi|
|960904|İçerik İçeren Ancak Eksik İçerik Türü üstbilgi İstek|
|960017|Ana bilgisayar üstbilgi sayısal bir IP adresidir|

### <a name="crs_23_request_limits"></a><a name="crs23"></a>crs_23_request_limits

|RuleId|Açıklama|
|---|---|
|960209|Bağımsız değişken adı çok uzun|
|960208|Bağımsız değişken değeri çok uzun|
|960335|İstekte çok fazla bağımsız değişken|
|960341|Toplam bağımsız değişken boyutu aşıldı|
|960342|Yüklenen dosya boyutu çok büyük|
|960343|Toplam yüklenen dosya boyutu çok büyük|

### <a name="crs_30_http_policy"></a><a name="crs30"></a>crs_30_http_policy

|RuleId|Açıklama|
|---|---|
|960032|Yöntem ilke tarafından izin verilmez|
|960010|İstek içerik türü ilke tarafından izin verilmez|
|960034|HTTP protokolü sürümü ilke tarafından izin verilmez|
|960035|URL dosya uzantısı ilke ile sınırlıdır|
|960038|HTTP üstbilgi ilke ile sınırlıdır|

### <a name="crs_35_bad_robots"></a><a name="crs35"></a>crs_35_bad_robots

|RuleId|Açıklama|
|---|---|
|990002|İstek Siteyi Taranan Bir Güvenlik Tarayıcısını Gösterir|
|990901|İstek Siteyi Taranan Bir Güvenlik Tarayıcısını Gösterir|
|990902|İstek Siteyi Taranan Bir Güvenlik Tarayıcısını Gösterir|
|990012|Rogue web sitesi tarayıcısı|

### <a name="crs_40_generic_attacks"></a><a name="crs40"></a>crs_40_generic_attacks

|RuleId|Açıklama|
|---|---|
|960024|Meta-Karakter Anomali Algılama Uyarısı - Tekrarlayan Non-Word Karakterler|
|950008|Belgesiz ColdFusion Etiketler Enjeksiyon|
|950010|LDAP Enjeksiyon Saldırısı|
|950011|SSI enjeksiyon Saldırısı|
|950018|Evrensel PDF XSS URL algılandı.|
|950019|E-posta Enjeksiyon Saldırı|
|950012|HTTP İstek Kaçakçılık Saldırı.|
|950910|HTTP Yanıt Bölme Saldırısı|
|950911|HTTP Yanıt Bölme Saldırısı|
|950117|Uzaktan Dosya İçerme Saldırısı|
|950118|Uzaktan Dosya İçerme Saldırısı|
|950119|Uzaktan Dosya İçerme Saldırısı|
|950120|Olası Uzak Dosya Ekleme (RFI) Saldırısı = Etki Alanı Dışı Başvuru/Bağlantı|
|981133|Kural 981133|
|981134|Kural 981134|
|950009|Oturum Fiksasyon Saldırısı|
|950003|Oturum Fiksasyonu|
|950000|Oturum Fiksasyonu|
|950005|Uzaktan Dosya Erişim Girişimi|
|950002|Sistem KomutU Erişimi|
|950006|Sistem Komut Enjeksiyonu|
|959151|PHP Enjeksiyon Saldırısı|
|958976|PHP Enjeksiyon Saldırısı|
|958977|PHP Enjeksiyon Saldırısı|

### <a name="crs_41_sql_injection_attacks"></a><a name="crs41sql"></a>crs_41_sql_injection_attacks

|RuleId|Açıklama|
|---|---|
|981231|SQL Yorum Sırası Algılandı.|
|981260|SQL Hex Kodlama Tanımlı|
|981320|SQL Injection Attack = Ortak DB Adları Algılandı|
|981300|Kural 981300|
|981301|Kural 981301|
|981302|Kural 981302|
|981303|Kural 981303|
|981304|Kural 981304|
|981305|Kural 981305|
|981306|Kural 981306|
|981307|Kural 981307|
|981308|Kural 981308|
|981309|Kural 981309|
|981310|Kural 981310|
|981311|Kural 981311|
|981312|Kural 981312|
|981313|Kural 981313|
|981314|Kural 981314|
|981315|Kural 981315|
|981316|Kural 981316|
|981317|SQL SELECT Bildirimi Anomali Algılama Uyarısı|
|950007|Kör SQL Enjeksiyon Saldırısı|
|950001|SQL Enjeksiyon Saldırısı|
|950908|SQL Enjeksiyon Saldırısı.|
|959073|SQL Enjeksiyon Saldırısı|
|981272|Uyku() veya benchmark() kullanarak kör sqli testlerini algılar.|
|981250|Koşullu sorgular da dahil olmak üzere SQL kıyaslama ve uyku enjeksiyon uyrama girişimlerini algılar|
|981241|Koşullu SQL enjeksiyon girişimlerini algılar|
|981276|Temel sql enjeksiyon arıyorsunuz. mysql oracle ve diğerleri için ortak saldırı dizesi.|
|981270|Temel MongoDB SQL enjeksiyon denemelerini bulur|
|981253|MySQL ve PostgreSQL depolanan yordam/fonksiyon enjeksiyonlarını algılar|
|981251|MySQL UDF enjeksiyonu ve diğer veri/yapı işleme girişimlerini algılar|

### <a name="crs_41_xss_attacks"></a><a name="crs41xss"></a>crs_41_xss_attacks

|RuleId|Açıklama|
|---|---|
|973336|XSS Filtresi - Kategori 1 = Komut Dosyası Etiket Vektörü|
|973338|XSS Filtresi - Kategori 3 = Javascript URI Vektör|
|981136|Kural 981136|
|981018|Kural 981018|
|958016|Site arası Komut Dosyası (XSS) Saldırısı|
|958414|Site arası Komut Dosyası (XSS) Saldırısı|
|958032|Site arası Komut Dosyası (XSS) Saldırısı|
|958026|Site arası Komut Dosyası (XSS) Saldırısı|
|958027|Site arası Komut Dosyası (XSS) Saldırısı|
|958054|Site arası Komut Dosyası (XSS) Saldırısı|
|958418|Site arası Komut Dosyası (XSS) Saldırısı|
|958034|Site arası Komut Dosyası (XSS) Saldırısı|
|958019|Site arası Komut Dosyası (XSS) Saldırısı|
|958013|Site arası Komut Dosyası (XSS) Saldırısı|
|958408|Site arası Komut Dosyası (XSS) Saldırısı|
|958012|Site arası Komut Dosyası (XSS) Saldırısı|
|958423|Site arası Komut Dosyası (XSS) Saldırısı|
|958002|Site arası Komut Dosyası (XSS) Saldırısı|
|958017|Site arası Komut Dosyası (XSS) Saldırısı|
|958007|Site arası Komut Dosyası (XSS) Saldırısı|
|958047|Site arası Komut Dosyası (XSS) Saldırısı|
|958410|Site arası Komut Dosyası (XSS) Saldırısı|
|958415|Site arası Komut Dosyası (XSS) Saldırısı|
|958022|Site arası Komut Dosyası (XSS) Saldırısı|
|958405|Site arası Komut Dosyası (XSS) Saldırısı|
|958419|Site arası Komut Dosyası (XSS) Saldırısı|
|958028|Site arası Komut Dosyası (XSS) Saldırısı|
|958057|Site arası Komut Dosyası (XSS) Saldırısı|
|958031|Site arası Komut Dosyası (XSS) Saldırısı|
|958006|Site arası Komut Dosyası (XSS) Saldırısı|
|958033|Site arası Komut Dosyası (XSS) Saldırısı|
|958038|Site arası Komut Dosyası (XSS) Saldırısı|
|958409|Site arası Komut Dosyası (XSS) Saldırısı|
|958001|Site arası Komut Dosyası (XSS) Saldırısı|
|958005|Site arası Komut Dosyası (XSS) Saldırısı|
|958404|Site arası Komut Dosyası (XSS) Saldırısı|
|958023|Site arası Komut Dosyası (XSS) Saldırısı|
|958010|Site arası Komut Dosyası (XSS) Saldırısı|
|958411|Site arası Komut Dosyası (XSS) Saldırısı|
|958422|Site arası Komut Dosyası (XSS) Saldırısı|
|958036|Site arası Komut Dosyası (XSS) Saldırısı|
|958000|Site arası Komut Dosyası (XSS) Saldırısı|
|958018|Site arası Komut Dosyası (XSS) Saldırısı|
|958406|Site arası Komut Dosyası (XSS) Saldırısı|
|958040|Site arası Komut Dosyası (XSS) Saldırısı|
|958052|Site arası Komut Dosyası (XSS) Saldırısı|
|958037|Site arası Komut Dosyası (XSS) Saldırısı|
|958049|Site arası Komut Dosyası (XSS) Saldırısı|
|958030|Site arası Komut Dosyası (XSS) Saldırısı|
|958041|Site arası Komut Dosyası (XSS) Saldırısı|
|958416|Site arası Komut Dosyası (XSS) Saldırısı|
|958024|Site arası Komut Dosyası (XSS) Saldırısı|
|958059|Site arası Komut Dosyası (XSS) Saldırısı|
|958417|Site arası Komut Dosyası (XSS) Saldırısı|
|958020|Site arası Komut Dosyası (XSS) Saldırısı|
|958045|Site arası Komut Dosyası (XSS) Saldırısı|
|958004|Site arası Komut Dosyası (XSS) Saldırısı|
|958421|Site arası Komut Dosyası (XSS) Saldırısı|
|958009|Site arası Komut Dosyası (XSS) Saldırısı|
|958025|Site arası Komut Dosyası (XSS) Saldırısı|
|958413|Site arası Komut Dosyası (XSS) Saldırısı|
|958051|Site arası Komut Dosyası (XSS) Saldırısı|
|958420|Site arası Komut Dosyası (XSS) Saldırısı|
|958407|Site arası Komut Dosyası (XSS) Saldırısı|
|958056|Site arası Komut Dosyası (XSS) Saldırısı|
|958011|Site arası Komut Dosyası (XSS) Saldırısı|
|958412|Site arası Komut Dosyası (XSS) Saldırısı|
|958008|Site arası Komut Dosyası (XSS) Saldırısı|
|958046|Site arası Komut Dosyası (XSS) Saldırısı|
|958039|Site arası Komut Dosyası (XSS) Saldırısı|
|958003|Site arası Komut Dosyası (XSS) Saldırısı|
|973300|Olası XSS Saldırısı Algılandı - HTML Etiket Işleyicisi|
|973301|XSS Saldırısı Algılandı|
|973302|XSS Saldırısı Algılandı|
|973303|XSS Saldırısı Algılandı|
|973304|XSS Saldırısı Algılandı|
|973305|XSS Saldırısı Algılandı|
|973306|XSS Saldırısı Algılandı|
|973307|XSS Saldırısı Algılandı|
|973308|XSS Saldırısı Algılandı|
|973309|XSS Saldırısı Algılandı|
|973311|XSS Saldırısı Algılandı|
|973313|XSS Saldırısı Algılandı|
|973314|XSS Saldırısı Algılandı|
|973331|IE XSS Filtreler - Saldırı Algılandı.|
|973315|IE XSS Filtreler - Saldırı Algılandı.|
|973330|IE XSS Filtreler - Saldırı Algılandı.|
|973327|IE XSS Filtreler - Saldırı Algılandı.|
|973326|IE XSS Filtreler - Saldırı Algılandı.|
|973346|IE XSS Filtreler - Saldırı Algılandı.|
|973345|IE XSS Filtreler - Saldırı Algılandı.|
|973324|IE XSS Filtreler - Saldırı Algılandı.|
|973323|IE XSS Filtreler - Saldırı Algılandı.|
|973348|IE XSS Filtreler - Saldırı Algılandı.|
|973321|IE XSS Filtreler - Saldırı Algılandı.|
|973320|IE XSS Filtreler - Saldırı Algılandı.|
|973318|IE XSS Filtreler - Saldırı Algılandı.|
|973317|IE XSS Filtreler - Saldırı Algılandı.|
|973329|IE XSS Filtreler - Saldırı Algılandı.|
|973328|IE XSS Filtreler - Saldırı Algılandı.|

### <a name="crs_42_tight_security"></a><a name="crs42"></a>crs_42_tight_security

|RuleId|Açıklama|
|---|---|
|950103|Yol Geçiş Saldırısı|

### <a name="crs_45_trojans"></a><a name="crs45"></a>crs_45_trojans

|RuleId|Açıklama|
|---|---|
|950110|Arka kapı erişimi|
|950921|Arka kapı erişimi|
|950922|Arka kapı erişimi|

---

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalını kullanarak Web Uygulaması Güvenlik Duvarı kurallarını özelleştirme](application-gateway-customize-waf-rules-portal.md)
