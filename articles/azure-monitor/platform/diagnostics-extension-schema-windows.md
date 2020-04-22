---
title: Windows tanılama uzantısı şeması
description: Azure Monitor'da Windows tanılama uzantısı (WAD) için yapılandırma şeması başvurusu.
ms.subservice: diagnostic-extension
ms.topic: reference
author: bwren
ms.author: bwren
ms.date: 01/20/2020
ms.openlocfilehash: c04fc82b8b04e474a656a0849177f7aa5d27b427
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81676428"
---
# <a name="windows-diagnostics-extension-schema"></a>Windows tanılama uzantısı şeması
Azure Tanılama uzantısı, Azure Monitor'da konuk işletim sisteminden izleme verileri ve Azure bilgi işlem kaynaklarının iş yüklerini toplayan bir aracıdır. Bu makalede, Windows sanal makinelerde ve diğer bilgi işlem kaynaklarında tanılama uzantısı yapılandırması için kullanılan şema ayrıntıları.

> [!NOTE]
> Bu makaledeki şema 1.3 ve daha yeni sürümler için geçerlidir (Azure SDK 2.4 ve daha yeni). Yeni yapılandırma bölümleri, hangi sürümde eklendiklerini göstermek için yorumlanır. Şema'nın 1.0 ve 1.2 sürümü arşivlendi ve artık kullanılamıyor. 

## <a name="public-configuration-file-schema"></a>Genel yapılandırma dosyası şeması

Aşağıdaki PowerShell komutunu çalıştırarak ortak yapılandırma dosyası şema tanımını indirin:  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  


## <a name="common-attribute-types"></a>Ortak Öznitelik Türleri  
 **zamanlananTransferPeriod** özniteliği çeşitli öğelerde görüntülenir. Depolamaya zamanlanan aktarımlar arasındaki aralıken en yakın dakikaya yuvarlanır. Değer bir [XML "Süre Veri Türü."](https://www.w3schools.com/xml/schema_dtypes_date.asp)


## <a name="diagnosticsconfiguration-element"></a>TanılamaYapılandırma Öğesi  
 *Ağaç: Kök - TanılamaYapılandırma*

Sürüm 1.3'te eklendi.  

Tanılama yapılandırma dosyasının üst düzey öğesi.  

**Öznitelik** xmlns - Tanılama yapılandırma dosyası için XML ad alanı:  
`http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration`


|Alt Öğeler|Açıklama|  
|--------------------|-----------------|  
|**PublicConfig**|Gereklidir. Bu sayfanın başka bir yerinde ki açıklamaya bakın.|  
|**PrivateConfig**|İsteğe bağlı. Bu sayfanın başka bir yerinde ki açıklamaya bakın.|  
|**IsEnabled**|Boolean. Bu sayfanın başka bir yerinde ki açıklamaya bakın.|  

## <a name="publicconfig-element"></a>PublicConfig Element  
 *Ağaç: Kök - TeşhisYapılandırma - PublicConfig*

 Genel tanıyapılandırmasını açıklar.  

|Alt Öğeler|Açıklama|  
|--------------------|-----------------|  
|**WadCfg**|Gereklidir. Bu sayfanın başka bir yerinde ki açıklamaya bakın.|  
|**StorageAccount**|Verileri depolamak için Azure Depolama hesabının adı. Set-AzureServiceDiagnosticsExtension cmdlet'i çalıştırırken parametre olarak da belirtilebilir.|  
|**Depolama Türü**|*Tablo*, *Blob*, veya *TableAndBlob*olabilir . Tablo varsayılandır. TableAndBlob seçildiğinde, tanılama verileri iki kez yazılır -- her türe bir kez.|  
|**YerelKaynak Rehberi**|İzleme Aracısı'nın olay verilerini depoladığı sanal makinedeki dizin. Değilse, ayarlayın, varsayılan dizin kullanılır:<br /><br /> İşçi/web rolü için:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> Sanal Makine için:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Gerekli öznitelikler şunlardır:<br /><br /> - **yol** - Azure Diagnostics tarafından kullanılacak sistem dizini.<br /><br /> - **expandEnvironment** - Çevre değişkenlerinin yol adında genişletilip genişletilmediğini denetler.|  

## <a name="wadcfg-element"></a>WadCFG Elemanı  
 *Ağaç: Kök - TeşhisYapılandırma - PublicConfig - WadCFG*

 Toplanacak telemetri verilerini tanımlar ve yapılandırır.  


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration Elemanı
 *Ağaç: Kök - TeşhisYapılandırma - PublicConfig - WadCFG - DiagnosticMonitorConfiguration*

 Gerekli

|Öznitelikler|Açıklama|  
|----------------|-----------------|  
| **genelQuotaInMB** | Azure Tanılama tarafından toplanan çeşitli tanılama verileri tarafından tüketilebilecek maksimum yerel disk alanı miktarı. Varsayılan ayar 4096 MB'dır.<br />
|**useProxyServer** | Azure Tanılama'yı IE ayarlarında ayarlanan proxy sunucu ayarlarını kullanacak şekilde yapılandırın.|
|**Lavabo** | 1.5 olarak eklendi. İsteğe bağlı. Lavaboları destekleyen tüm alt öğeler için tanılama verileri göndermek için bir lavabo konumuna işaret eder. Lavabo örneği Uygulama Öngörüleri veya Olay Hub'larıdır.|  


<br /> <br />

|Alt Öğeler|Açıklama|  
|--------------------|-----------------|  
|**CrashDumps**|Bu sayfanın başka bir yerinde ki açıklamaya bakın.|  
|**TeşhisAltyapıGünlükleri**|Azure Diagnostics tarafından oluşturulan günlüklerin toplanmasını etkinleştirin. Tanılama altyapısı günlükleri, tanılama sisteminin kendisini sorun giderme için yararlıdır. İsteğe bağlı öznitelikler şunlardır:<br /><br /> - **scheduledTransferLogLevelFilter** - Toplanan günlüklerin minimum önem düzeyini yapılandırır.<br /><br /> - **scheduledTransferPeriod** - Depolamaya zamanlanan aktarımlar arasındaki aralık en yakın dakikaya yuvarlanır. Değer bir [XML "Süre Veri Türü."](https://www.w3schools.com/xml/schema_dtypes_date.asp) |  
|**Dizinler**|Bu sayfanın başka bir yerinde ki açıklamaya bakın.|  
|**EtwSağlayıcılar**|Bu sayfanın başka bir yerinde ki açıklamaya bakın.|  
|**Ölçümler**|Bu sayfanın başka bir yerinde ki açıklamaya bakın.|  
|**Performans Sayaçları**|Bu sayfanın başka bir yerinde ki açıklamaya bakın.|  
|**WindowsEventLog**|Bu sayfanın başka bir yerinde ki açıklamaya bakın.|
|**DockerKaynakları**|Bu sayfanın başka bir yerinde ki açıklamaya bakın. |



## <a name="crashdumps-element"></a>CrashDumps Öğesi  
 *Ağaç: Kök - TeşhisYapılandırma - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps*

 Kilitlenme dökümlerinin toplanmasını etkinleştirin.  

|Öznitelikler|Açıklama|  
|----------------|-----------------|  
|**containerName**|İsteğe bağlı. Kilitlenme dökümlerini depolamak için kullanılmak üzere Azure Depolama hesabınızdaki blob kapsayıcısının adı.|  
|**crashDumpType**|İsteğe bağlı.  Azure Tanılama'yı mini veya tam kilitlenme dökümleri toplamak üzere yapılandırır.|  
|**dizinQuotaPercentage**|İsteğe bağlı.  VM'deki kilitlenme dökümleri için ayrılacak **genel QuotaInMB** yüzdesini yapılandırır.|  

|Alt Öğeler|Açıklama|  
|--------------------|-----------------|  
|**CrashDumpYapılandırma**|Gereklidir. Her işlem için yapılandırma değerlerini tanımlar.<br /><br /> Aşağıdaki öznitelik de gereklidir:<br /><br /> **processName** - Azure Diagnostics'in bir kilitlenme dökümü toplamasını istediğiniz işlemin adı.|  

## <a name="directories-element"></a>Dizinler Öğesi
 *Ağaç: Kök - TeşhisYapılandırma - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Dizinler*

 Bir dizinin içeriğinin toplanmasını sağlar, IIS başarısız erişim istek günlükleri ve/veya IIS günlükleri.  

 İsteğe bağlı **zamanlananTransferPeriod** özniteliği. Daha önce açıklamaya bakın.  

|Alt Öğeler|Açıklama|  
|--------------------|-----------------|  
|**IISLogs**|Yapılandırmaya bu öğenin eklenmesi IIS günlüklerinin toplanmasını sağlar:<br /><br /> **containerName** - IIS günlüklerini depolamak için kullanılacak Azure Depolama hesabınızdaki blob kapsayıcısının adı.|   
|**FailedRequestLogs**|Yapılandırmaya bu öğenin eklenmesi, bir IIS sitesine veya uygulamaya başarısız isteklerle ilgili günlüklerin toplanmasını sağlar. Ayrıca sistem altında izleme seçeneklerini etkinleştirmeniz **gerekir. WebServer** **içinde Web.config**.|  
|**Datasources**|İzlenecek dizinlerin listesi.|




## <a name="datasources-element"></a>DataSources Öğesi  
 *Ağaç: Kök - TeşhisYapılandırma - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Dizinler - DataSources*

 İzlenecek dizinlerin listesi.  

|Alt Öğeler|Açıklama|  
|--------------------|-----------------|  
|**DizinYapılandırma**|Gereklidir. Gerekli öznitelik:<br /><br /> **containerName** - Azure Depolama hesabınızdaki günlük dosyalarını depolamak için kullanılacak blob kapsayıcısının adı.|  





## <a name="directoryconfiguration-element"></a>DizinYapılandırma Öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Dizinler - DataSources - DizinYapılandırma*

 **Mutlak** veya Yerel **Kaynak** öğesini içerebilir, ancak her ikisini de içermeyebilir.  

|Alt Öğeler|Açıklama|  
|--------------------|-----------------|  
|**Mutlak**|İzlenecek dizin için mutlak yol. Aşağıdaki öznitelikler gereklidir:<br /><br /> - **Yol** - Izlemek için dizin için mutlak yol.<br /><br /> - **expandEnvironment** - Yol'daki ortam değişkenlerinin genişletilip genişletilmediğini yapılandırır.|  
|**Yerel Kaynak**|İzlenecek yerel kaynağa göre yol. Gerekli öznitelikler şunlardır:<br /><br /> - **Ad** - İzni izleyen yerel kaynak<br /><br /> - **relativePath** - İzlenecek dizini içeren Ad'a göre yol|  



## <a name="etwproviders-element"></a>EtwProviders Öğesi  
 *Ağaç: Kök - TeşhisYapılandırma - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders*

 ETW olaylarının Toplanmasını EventSource ve/veya ETW Manifest tabanlı sağlayıcılardan yapılandırır.  

|Alt Öğeler|Açıklama|  
|--------------------|-----------------|  
|**EtwEventSourceProviderYapılandırma**|[EventSource Sınıfından](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx)oluşturulan olayların koleksiyonunu yapılandırır. Gerekli öznitelik:<br /><br /> **sağlayıcı** - EventSource etkinliğinin sınıf adı.<br /><br /> İsteğe bağlı öznitelikler şunlardır:<br /><br /> - **scheduledTransferLogLevelFilter** - Depolama hesabınıza aktarmak için minimum önem düzeyi.<br /><br /> - **scheduledTransferPeriod** - Depolamaya zamanlanan aktarımlar arasındaki aralık en yakın dakikaya yuvarlanır. Değer bir [XML "Süre Veri Türü."](https://www.w3schools.com/xml/schema_dtypes_date.asp) |  
|**EtwManifestSağlayıcıYapılandırma**|Gerekli öznitelik:<br /><br /> **sağlayıcı** - Olay sağlayıcısının GUID<br /><br /> İsteğe bağlı öznitelikler şunlardır:<br /><br /> - **scheduledTransferLogLevelFilter** - Depolama hesabınıza aktarmak için minimum önem düzeyi.<br /><br /> - **scheduledTransferPeriod** - Depolamaya zamanlanan aktarımlar arasındaki aralık en yakın dakikaya yuvarlanır. Değer bir [XML "Süre Veri Türü."](https://www.w3schools.com/xml/schema_dtypes_date.asp) |  



## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderYapılandırma Öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders- EtwEventSourceProviderConfiguration*

 [EventSource Sınıfından](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx)oluşturulan olayların koleksiyonunu yapılandırır.  

|Alt Öğeler|Açıklama|  
|--------------------|-----------------|  
|**Varsayılan Olaylar**|İsteğe bağlı öznitelik:<br/><br/> **eventDestination** - Olayları depolamak için tablonun adı|  
|**Olay**|Gerekli öznitelik:<br /><br /> **id** - Olayın kimliği.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **eventDestination** - Olayları depolamak için tablonun adı|  



## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderYapılandırma Öğesi  
 *Ağaç: Kök - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwManifestProviderConfiguration*

|Alt Öğeler|Açıklama|  
|--------------------|-----------------|  
|**Varsayılan Olaylar**|İsteğe bağlı öznitelik:<br /><br /> **eventDestination** - Olayları depolamak için tablonun adı|  
|**Olay**|Gerekli öznitelik:<br /><br /> **id** - Olayın kimliği.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **eventDestination** - Olayları depolamak için tablonun adı|  



## <a name="metrics-element"></a>Ölçümler Öğesi  
 *Ağaç: Kök - TeşhisYapılandırma - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Ölçümler*

 Hızlı sorgular için en iyi duruma getirilmiş bir performans sayacı oluşturmanıza olanak tanır. **PerformanceCounters** öğesinde tanımlanan her performans sayacı, Performans Sayacı tablosuna ek olarak Ölçümler tablosunda depolanır.  

 **ResourceId** özniteliği gereklidir.  Azure Tanılama'yı dağıtdığınız Sanal Makine veya Sanal Makine Ölçeği Kümesi'nin kaynak kimliği. [Azure portalından](https://portal.azure.com) **kaynak kimliğini** alın. **Kaynak Gruplarına**  ->  **Gözat** -> ** \><Adı'nı**seçin. **Özellikler** döşemesini tıklatın ve **kimlik** alanından değeri kopyalayın.  

|Alt Öğeler|Açıklama|  
|--------------------|-----------------|  
|**MetricAggregation**|Gerekli öznitelik:<br /><br /> **scheduledTransferPeriod** - Depolamaya zamanlanan aktarımlar arasındaki aralık en yakın dakikaya yuvarlanır. Değer bir [XML "Süre Veri Türü."](https://www.w3schools.com/xml/schema_dtypes_date.asp) |  



## <a name="performancecounters-element"></a>PerformanceCounters Öğesi  
 *Ağaç: Kök - TeşhisYapılandırma - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - PerformanceCounters*

 Performans sayaçlarının toplanmasını sağlar.  

 İsteğe bağlı öznitelik:  

 İsteğe bağlı **zamanlananTransferPeriod** özniteliği. Daha önce açıklamaya bakın.

|Alt Öğe|Açıklama|  
|-------------------|-----------------|  
|**Performans KarşıYapılandırma**|Aşağıdaki öznitelikler gereklidir:<br /><br /> - **counterSpecifier** - performans sayacının adı. Örneğin, `\Processor(_Total)\% Processor Time`. Ana bilgisayarınızda performans sayaçlarının listesini almak `typeperf`için komutu çalıştırın.<br /><br /> - **sampleRate** - Sayaç ne sıklıkta örneklenmelidir.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **birim** - Sayacın ölçü birimi. Değerler [UnitType Class'ta](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.sql.models.unittype?view=azure-dotnet) mevcuttur |
|**Lavabo** | 1.5 olarak eklendi. İsteğe bağlı. Tanılama verilerini göndermek için bir lavabo konumuna işaret edin. Örneğin, Azure Monitörü veya Etkinlik Hub'ları.|    




## <a name="windowseventlog-element"></a>WindowsEventLog Öğesi
 *Ağaç: Kök - TeşhisYapılandırma - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog*

 Windows Olay Günlükleri'nin toplanmasını sağlar.  

 İsteğe bağlı **zamanlananTransferPeriod** özniteliği. Daha önce açıklamaya bakın.  

|Alt Öğe|Açıklama|  
|-------------------|-----------------|  
|**Datasource**|Windows Event günlükleri toplamak için. Gerekli öznitelik:<br /><br /> **ad** - Toplanacak windows olaylarını açıklayan XPath sorgusu. Örneğin:<br /><br /> `Application!*[System[(Level <=3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level <= 3)]`<br /><br /> Tüm olayları toplamak için "*" |
|**Lavabo** | 1.5 olarak eklendi. İsteğe bağlı. Lavaboları destekleyen tüm alt öğeler için tanılama verileri göndermek için bir lavabo konumuna işaret eder. Lavabo örneği Uygulama Öngörüleri veya Olay Hub'larıdır.|  


## <a name="logs-element"></a>Günlükleri Öğesi  
 *Ağaç: Kök - TeşhisYapılandırma - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Günlükler*

 Sürüm 1.0 ve 1.1'de mevcut. 1.2'de kayıp. 1.3'te eklendi.  

 Temel Azure günlükleri için arabellek yapılandırmasını tanımlar.  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|**Unsignedınt**|İsteğe bağlı. Belirtilen veriler için kullanılabilen maksimum dosya sistemi depolama miktarını belirtir.<br /><br /> Varsayılan değer, 0'dur.|  
|**scheduledTransferLogLevelFilter**|**Dize**|İsteğe bağlı. Aktarılan günlük girişleri için minimum önem düzeyini belirtir. Varsayılan **değer,** tüm günlükleri aktaran Tanımsız'dır. Diğer olası değerler (en az bilgi sırasına göre) **Verbose,** **Bilgi**, **Uyarı**, **Hata**, ve **Kritik**.|  
|**scheduledTransferPeriod**|**Süre**|İsteğe bağlı. Zamanlanmış veri aktarımları arasındaki aralığı belirtir, en yakın dakikaya yuvarlanır.<br /><br /> Varsayılan pt0S olduğunu.|  
|**Lavabo** |**Dize**| 1.5 olarak eklendi. İsteğe bağlı. Tanılama verilerini göndermek için bir lavabo konumuna işaret edin. Örneğin, Uygulama Öngörüleri veya Olay Hub'ları.|  

## <a name="dockersources"></a>DockerKaynakları
 *Ağaç: Kök - TeşhisYapılandırma - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources*

 1.9 olarak eklendi.

|Öğe Adı|Açıklama|  
|------------------|-----------------|  
|**İstatistikler**|Docker konteynerleri için istatistikleri toplamak için sistem söyler|  

## <a name="sinksconfig-element"></a>SinksConfig Element  
 *Ağaç: Kök - TeşhisYapılandırma - PublicConfig - WadCFG - SinksConfig*

 Tanılama verilerini gönderecek konumların listesi ve bu konumlarla ilişkili yapılandırma.  

|Öğe Adı|Açıklama|  
|------------------|-----------------|  
|**Havuz**|Bu sayfanın başka bir yerinde ki açıklamaya bakın.|  

## <a name="sink-element"></a>Lavabo Elemanı
 *Ağaç: Kök - TeşhisYapılandırma - PublicConfig - WadCFG - SinksConfig - Lavabo*

 Sürüm 1.5'e eklendi.  

 Tanılama verilerini gönderecek konumları tanımlar. Örneğin, Application Insights hizmeti.  

|Öznitelik|Tür|Açıklama|  
|---------------|----------|-----------------|  
|**Adı**|string|Lavabo adını tanımlayan bir dize.|  

|Öğe|Tür|Açıklama|  
|-------------|----------|-----------------|  
|**Uygulama Bilgileri**|string|Yalnızca Application Insights'a veri gönderirken kullanılır. Erişebildiğiniz etkin bir Application Insights hesabı için Enstrümantasyon Anahtarını bulundurun.|  
|**Kanallar**|string|Bu akışı her ek filtreleme için bir|  

## <a name="channels-element"></a>Kanallar Öğesi  
 *Ağaç: Kök - TeşhisYapılandırma - PublicConfig - WadCFG - SinksConfig - Lavabo - Kanallar*

 Sürüm 1.5'e eklendi.  

 Lavabodan geçen günlük veri akışları için filtreler tanımlar.  

|Öğe|Tür|Açıklama|  
|-------------|----------|-----------------|  
|**Kanal**|string|Bu sayfanın başka bir yerinde ki açıklamaya bakın.|  

## <a name="channel-element"></a>Kanal Öğesi
 *Ağaç: Kök - TeşhisYapılandırma - PublicConfig - WadCFG - SinksConfig - Lavabo - Kanallar - Kanal*

 Sürüm 1.5'e eklendi.  

 Tanılama verilerini gönderecek konumları tanımlar. Örneğin, Application Insights hizmeti.  

|Öznitelikler|Tür|Açıklama|  
|----------------|----------|-----------------|  
|**Loglevel**|**Dize**|Aktarılan günlük girişleri için minimum önem düzeyini belirtir. Varsayılan **değer,** tüm günlükleri aktaran Tanımsız'dır. Diğer olası değerler (en az bilgi sırasına göre) **Verbose,** **Bilgi**, **Uyarı**, **Hata**, ve **Kritik**.|  
|**Adı**|**Dize**|Başvurmak için kanalın benzersiz bir adı|  


## <a name="privateconfig-element"></a>PrivateConfig Element
 *Ağaç: Kök - TeşhisYapılandırma - PrivateConfig*

 Sürüm 1.3'te eklendi.  

 İsteğe bağlı  

 Depolama hesabının özel ayrıntılarını (ad, anahtar ve bitiş noktası) depolar. Bu bilgiler sanal makineye gönderilir, ancak ondan alınamaz.  

|Alt Öğeler|Açıklama|  
|--------------------|-----------------|  
|**StorageAccount**|Kullanılacak depolama hesabı. Aşağıdaki öznitelikler gereklidir<br /><br /> - **adı** - Depolama hesabının adı.<br /><br /> - **anahtar** - depolama hesabının anahtarı.<br /><br /> - **endpoint** - Depolama hesabına erişmek için bitiş noktası. <br /><br /> -**sasToken** (ek 1.8.1)- Özel config'de depolama hesabı anahtarı yerine bir SAS belirteği belirtebilirsiniz. Sağlandığı takdirde, depolama hesabı anahtarı yoksayılır. <br />SAS Belirteci için gereksinimler: <br />- Yalnızca hesap SAS belirteci destekler <br />- *b*, *t* hizmet türleri gereklidir. <br /> - *a*, *c*, *u*, *w* izinleri gereklidir. <br /> - *c*, *o* kaynak türleri gereklidir. <br /> - Yalnızca HTTPS protokolünü destekler <br /> - Başlangıç ve son kullanma süresi geçerli olmalıdır.|  


## <a name="isenabled-element"></a>Etkin Öğe  
 *Ağaç: Kök - DiagnosticsConfiguration - IsEnabled*

 Boolean. Tanılamayı `true` etkinleştirmek `false` veya tanılamayı devre dışı kullanabilirsiniz.

## <a name="example-configuration"></a>Örnek yapılandırma
 Aşağıda, hem JSON hem de XML'de gösterilen Windows tanılama uzantısı için tam bir örnek yapılandırmadır.

 
### <a name="json"></a>JSON

Çoğu JSON kullanım durumunda farklı değişkenler olarak geçirildiği için *PublicConfig* ve *PrivateConfig* ayrılır. Bu servis talepleri Kaynak Yöneticisi şablonlarını, PowerShell'i ve Visual Studio'yı içerir.

> [!NOTE]
> Genel config Azure Monitor lavabo tanımı, *resourceId* ve *bölge*olmak üzere iki özelliğe sahiptir. Bunlar yalnızca Klasik VM'ler ve Klasik Bulut hizmetleri için gereklidir. Bu özellikler diğer kaynaklar için kullanılmamalıdır.

```json
"PublicConfig" {
    "WadCfg": {
        "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": 10000,
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "sinks": "AzureMonitorSink",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                        "sampleRate": "PT1M",
                        "unit": "percent"
                    }
                ]
            },
            "Directories": {
                "scheduledTransferPeriod": "PT5M",
                "IISLogs": {
                    "containerName": "iislogs"
                },
                "FailedRequestLogs": {
                    "containerName": "iisfailed"
                },
                "DataSources": [
                    {
                        "containerName": "mynewprocess",
                        "Absolute": {
                            "path": "C:\\MyNewProcess",
                            "expandEnvironment": false
                        }
                    },
                    {
                        "containerName": "badapp",
                        "Absolute": {
                            "path": "%SYSTEMDRIVE%\\BadApp",
                            "expandEnvironment": true
                        }
                    },
                    {
                        "containerName": "goodapp",
                        "LocalResource": {
                            "relativePath": "..\\PeanutButter",
                            "name": "Skippy"
                        }
                    }
                ]
            },
            "EtwProviders": {
                "sinks": "",
                "EtwEventSourceProviderConfiguration": [
                    {
                        "scheduledTransferPeriod": "PT5M",
                        "provider": "MyProviderClass",
                        "Event": [
                            {
                                "id": 0
                            },
                            {
                                "id": 1,
                                "eventDestination": "errorTable"
                            }
                        ],
                        "DefaultEvents": {
                        }
                    }
                ],
                "EtwManifestProviderConfiguration": [
                    {
                        "scheduledTransferPeriod": "PT2M",
                        "scheduledTransferLogLevelFilter": "Information",
                        "provider": "5974b00b-84c2-44bc-9e58-3a2451b4e3ad",
                        "Event": [
                            {
                                "id": 0
                            }
                        ],
                        "DefaultEvents": {
                        }
                    }
                ]
            },
            "WindowsEventLog": {
                "scheduledTransferPeriod": "PT5M",
                "DataSource": [
                    {
                        "name": "System!*[System[Provider[@Name='Microsoft Antimalware']]]"
                    },
                    {
                        "name": "System!*[System[Provider[@Name='NTFS'] and (EventID=55)]]"
                    },
                    {
                        "name": "System!*[System[Provider[@Name='disk'] and (EventID=7 or EventID=52 or EventID=55)]]"
                    }
                ]
            },
            "Logs": {
                "scheduledTransferPeriod": "PT1M",
                "scheduledTransferLogLevelFilter": "Verbose",
                "sinks": "ApplicationInsights.AppLogs"
            },
            "CrashDumps": {
                "directoryQuotaPercentage": 30,
                "dumpType": "Mini",
                "containerName": "wad-crashdumps",
                "CrashDumpConfiguration": [
                    {
                        "processName": "mynewprocess.exe"
                    },
                    {
                        "processName": "badapp.exe"
                    }
                ]
            }
        },
        "SinksConfig": {
            "Sink": [
                {
                    "name": "AzureMonitorSink",
                    "AzureMonitor":
                    {
                        "ResourceId": "{insert resourceId if a classic VM or cloud service, else property not needed}",
                        "Region": "{insert Azure region of resource if a classic VM or cloud service, else property not needed}"
                    }
                },
                {
                    "name": "ApplicationInsights",
                    "ApplicationInsights": "{Insert InstrumentationKey}",
                    "Channels": {
                        "Channel": [
                            {
                                "logLevel": "Error",
                                "name": "Errors"
                            },
                            {
                                "logLevel": "Verbose",
                                "name": "AppLogs"
                            }
                        ]
                    }
                },
                {
                    "name": "EventHub",
                    "EventHub": {
                        "Url": "https://myeventhub-ns.servicebus.windows.net/diageventhub",
                        "SharedAccessKeyName": "SendRule",
                        "usePublisherId": false
                    }
                },
                {
                    "name": "secondaryEventHub",
                    "EventHub": {
                        "Url": "https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub",
                        "SharedAccessKeyName": "SendRule",
                        "usePublisherId": false
                    }
                },
                {
                    "name": "secondaryStorageAccount",
                    "StorageAccount": {
                        "name": "secondarydiagstorageaccount",
                        "endpoint": "https://core.windows.net"
                    }
                }
            ]
        }
    },
    "StorageAccount": "diagstorageaccount",
    "StorageType": "TableAndBlob"
}
```

> [!NOTE]
> Özel config Azure Monitor lavabo tanımı, *PrincipalId* ve *Secret*olmak üzere iki özelliğe sahiptir. Bunlar yalnızca Klasik VM'ler ve Klasik Bulut hizmetleri için gereklidir. Bu özellikler diğer kaynaklar için kullanılmamalıdır.


```json
"PrivateConfig" {
    "storageAccountName": "diagstorageaccount",
    "storageAccountKey": "{base64 encoded key}",
    "storageAccountEndPoint": "https://core.windows.net",
    "storageAccountSasToken": "{sas token}",
    "EventHub": {
        "Url": "https://myeventhub-ns.servicebus.windows.net/diageventhub",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "{base64 encoded key}"
    },
    "AzureMonitorAccount": {
        "ServicePrincipalMeta": {
            "PrincipalId": "{Insert service principal client Id}",
            "Secret": "{Insert service principal client secret}"
        }
    },
    "SecondaryStorageAccounts": {
        "StorageAccount": [
            {
                "name": "secondarydiagstorageaccount",
                "key": "{base64 encoded key}",
                "endpoint": "https://core.windows.net",
                "sasToken": "{sas token}"
            }
        ]
    },
    "SecondaryEventHubs": {
        "EventHub": [
            {
                "Url": "https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub",
                "SharedAccessKeyName": "SendRule",
                "SharedAccessKey": "{base64 encoded key}"
            }
        ]
    }
}

```

### <a name="xml"></a>XML

```xml  
<?xml version="1.0" encoding="utf-8"?>  
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">   
  <PublicConfig>  
    <WadCfg>  
      <DiagnosticMonitorConfiguration overallQuotaInMB="10000">  

        <PerformanceCounters scheduledTransferPeriod="PT1M", sinks="AzureMonitorSink">  
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />  
        </PerformanceCounters>  

        <Directories scheduledTransferPeriod="PT5M">  
          <IISLogs containerName="iislogs" />  
          <FailedRequestLogs containerName="iisfailed" />  

          <DataSources>  
            <DirectoryConfiguration containerName="mynewprocess">  
              <Absolute path="C:\MyNewProcess" expandEnvironment="false" />  
            </DirectoryConfiguration>  
            <DirectoryConfiguration containerName="badapp">  
              <Absolute path="%SYSTEMDRIVE%\BadApp" expandEnvironment="true" />  
            </DirectoryConfiguration>  
            <DirectoryConfiguration containerName="goodapp">  
              <LocalResource name="Skippy" relativePath="..\PeanutButter"/>  
            </DirectoryConfiguration>  
          </DataSources>  

        </Directories>  

        <EtwProviders>  
          <EtwEventSourceProviderConfiguration   
                       provider="MyProviderClass"   
                       scheduledTransferPeriod="PT5M">  
            <Event id="0"/>  
            <Event id="1" eventDestination="errorTable"/>  
            <DefaultEvents />  
          </EtwEventSourceProviderConfiguration>  
          <EtwManifestProviderConfiguration provider="5974b00b-84c2-44bc-9e58-3a2451b4e3ad" scheduledTransferLogLevelFilter="Information" scheduledTransferPeriod="PT2M">  
            <Event id="0"/>  
            <DefaultEvents eventDestination="defaultTable"/>  
          </EtwManifestProviderConfiguration>  
        </EtwProviders>  

        <WindowsEventLog scheduledTransferPeriod="PT5M">  
          <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>  
          <DataSource name="System!*[System[Provider[@Name='NTFS'] and (EventID=55)]]" />  
          <DataSource name="System!*[System[Provider[@Name='disk'] and (EventID=7 or EventID=52 or EventID=55)]]" />  
        </WindowsEventLog>  

        <Logs  bufferQuotaInMB="1024"   
             scheduledTransferPeriod="PT1M"   
             scheduledTransferLogLevelFilter="Verbose"   
             sinks="ApplicationInsights.AppLogs"/>  <!-- sinks attribute added in 1.5 -->  

        <CrashDumps containerName="wad-crashdumps" directoryQuotaPercentage="30" dumpType="Mini">  
          <CrashDumpConfiguration processName="mynewprocess.exe" />  
          <CrashDumpConfiguration processName="badapp.exe"/>  
        </CrashDumps>  

        <DockerSources> <!-- Added in 1.9 -->
          <Stats enabled="true" sampleRate="PT1M" scheduledTransferPeriod="PT1M" />
        </DockerSources>

      </DiagnosticMonitorConfiguration>  

      <SinksConfig>   <!-- Added in 1.5 -->  
        <Sink name="AzureMonitorSink">
            <AzureMonitor> <!-- Added in 1.11 -->
                <resourceId>{insert resourceId}</ResourceId> <!-- Parameter only needed for classic VMs and Classic Cloud Services, exclude VMSS and Resource Manager VMs-->
                <Region>{insert Azure region of resource}</Region> <!-- Parameter only needed for classic VMs and Classic Cloud Services, exclude VMSS and Resource Manager VMs -->
            </AzureMonitor>
        </Sink>
        <Sink name="ApplicationInsights">   
          <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>   
          <Channels>   
            <Channel logLevel="Error" name="Errors"  />   
            <Channel logLevel="Verbose" name="AppLogs"  />   
          </Channels>   
        </Sink>   
        <Sink name="EventHub"> <!-- Added in 1.7 -->
          <EventHub Url="https://myeventhub-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" usePublisherId="false" />
        </Sink>
        <Sink name="secondaryEventHub"> <!-- Added in 1.7 -->
          <EventHub Url="https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub" SharedAccessKeyName="SendRule" usePublisherId="false" />
        </Sink>
        <Sink name="secondaryStorageAccount"> <!-- Added in 1.7 -->
          <StorageAccount name="secondarydiagstorageaccount" endpoint="https://core.windows.net" />
        </Sink>
   </SinksConfig>

  </WadCfg>  

  <StorageAccount>diagstorageaccount</StorageAccount>
  <StorageType>TableAndBlob</StorageType> <!-- Added in 1.8 -->  
  </PublicConfig>  

  <PrivateConfig>  <!-- Added in 1.3 -->  
    <StorageAccount name="" key="" endpoint="" sasToken="{sas token}"  />  <!-- sasToken in Private config added in 1.8.1 -->  
    <EventHub Url="https://myeventhub-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />

    <AzureMonitorAccount>
        <ServicePrincipalMeta> <!-- Added in 1.11; only needed for classic VMs and Classic cloud services -->
            <PrincipalId>{Insert service principal clientId}</PrincipalId>
            <Secret>{Insert service principal client secret}</Secret>
        </ServicePrincipalMeta>
    </AzureMonitorAccount>

    <SecondaryStorageAccounts>
       <StorageAccount name="secondarydiagstorageaccount" key="{base64 encoded key}" endpoint="https://core.windows.net" sasToken="{sas token}" />
    </SecondaryStorageAccounts>

    <SecondaryEventHubs>
       <EventHub Url="https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
    </SecondaryEventHubs>

  </PrivateConfig>  
  <IsEnabled>true</IsEnabled>  
</DiagnosticsConfiguration>  

```  
> [!NOTE]
> Genel config Azure Monitor lavabo tanımı, resourceId ve bölge olmak üzere iki özelliğe sahiptir. Bunlar yalnızca Klasik VM'ler ve Klasik Bulut hizmetleri için gereklidir. Bu özellikler Kaynak Yöneticisi Sanal Makineler veya Sanal Makine Ölçeği kümeleri için kullanılmamalıdır.
> Azure Monitor lavabosu için, Asıl Kimlik ve Gizli'de geçen ek bir Özel Config öğesi de vardır. Bu yalnızca Klasik VM'ler ve Klasik Bulut Hizmetleri için gereklidir. Kaynak Yöneticisi VM'leri ve VMSS için özel config öğesindeki Azure Monitör tanımı hariç tutulabilir.
>
