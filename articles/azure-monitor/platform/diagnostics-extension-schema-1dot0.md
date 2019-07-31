---
title: Azure Tanılama 1,0 yapılandırma şeması
description: YALNıZCA Azure sanal makineler, sanal makine ölçek kümeleri, Service Fabric veya Cloud Services ile Azure SDK 2,4 ve aşağıdaki durumlarda geçerlidir.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/15/2017
ms.author: robb
ms.subservice: diagnostic-extension
ms.openlocfilehash: ac2b79d670b803573a359dfc9f8738f972f2d9b5
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "60237850"
---
# <a name="azure-diagnostics-10-configuration-schema"></a>Azure Tanılama 1,0 yapılandırma şeması
> [!NOTE]
> Azure Tanılama, Azure sanal makineler, sanal makine ölçek kümeleri, Service Fabric ve Cloud Services performans sayaçlarını ve diğer istatistikleri toplamak için kullanılan bileşendir.  Bu sayfa yalnızca bu hizmetlerden birini kullanıyorsanız ilgilidir.
>

Azure Tanılama, Application Insights ve Log Analytics dahil olmak üzere Azure Izleyici gibi diğer Microsoft tanılama ürünleriyle birlikte kullanılır.

Azure Tanılama yapılandırma dosyası, tanılama Izleyicisini başlatmak için kullanılan değerleri tanımlar. Bu dosya, tanılama İzleyicisi başladığında tanılama yapılandırma ayarlarını başlatmak için kullanılır.  

 Varsayılan olarak, Azure tanılama yapılandırma şema dosyası `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` dizine yüklenir. `<version>` [Azure SDK](https://www.windowsazure.com/develop/downloads/)'nın yüklü sürümüyle değiştirin.  

> [!NOTE]
>  Tanılama yapılandırma dosyası genellikle, tanılama verilerinin başlangıç sürecinde daha önce toplanmasını gerektiren başlangıç görevleriyle birlikte kullanılır. Azure Tanılama kullanma hakkında daha fazla bilgi için bkz. [Azure Tanılama kullanarak günlük verilerini toplama](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).  

## <a name="example-of-the-diagnostics-configuration-file"></a>Tanılama yapılandırma dosyası örneği  
 Aşağıdaki örnek tipik bir tanılama yapılandırma dosyası göstermektedir:  

```xml  
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"  
      configurationChangePollInterval="PT1M"  
      overallQuotaInMB="4096">  
   <DiagnosticInfrastructureLogs bufferQuotaInMB="1024"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M" />  
   <Logs bufferQuotaInMB="1024"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M" />  

   <Directories bufferQuotaInMB="1024"   
      scheduledTransferPeriod="PT1M">  

      <!-- These three elements specify the special directories   
           that are set up for the log types -->  
      <CrashDumps container="wad-crash-dumps" directoryQuotaInMB="256" />  
      <FailedRequestLogs container="wad-frq" directoryQuotaInMB="256" />  
      <IISLogs container="wad-iis" directoryQuotaInMB="256" />  

      <!-- For regular directories the DataSources element is used -->  
      <DataSources>  
         <DirectoryConfiguration container="wad-panther" directoryQuotaInMB="128">  
            <!-- Absolute specifies an absolute path with optional environment expansion -->  
            <Absolute expandEnvironment="true" path="%SystemRoot%\system32\sysprep\Panther" />  
         </DirectoryConfiguration>  
         <DirectoryConfiguration container="wad-custom" directoryQuotaInMB="128">  
            <!-- LocalResource specifies a path relative to a local   
                 resource defined in the service definition -->  
            <LocalResource name="MyLoggingLocalResource" relativePath="logs" />  
         </DirectoryConfiguration>  
      </DataSources>  
   </Directories>  

   <PerformanceCounters bufferQuotaInMB="512" scheduledTransferPeriod="PT1M">  
      <!-- The counter specifier is in the same format as the imperative   
           diagnostics configuration API -->  
      <PerformanceCounterConfiguration   
         counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT5S" />  
   </PerformanceCounters>  

   <WindowsEventLog bufferQuotaInMB="512"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M">  
      <!-- The event log name is in the same format as the imperative   
           diagnostics configuration API -->  
      <DataSource name="System!*" />  
   </WindowsEventLog>  
</DiagnosticMonitorConfiguration>  
```  

## <a name="diagnosticsconfiguration-namespace"></a>DiagnosticsConfiguration ad alanı  
 Tanılama yapılandırma dosyası için XML ad alanı:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="schema-elements"></a>Şema öğeleri  
 Tanılama yapılandırma dosyası aşağıdaki öğeleri içerir.


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration öğesi  
Tanılama yapılandırma dosyasının en üst düzey öğesi.  

Öznitelikler:

|Öznitelik  |Type   |Gerekli| Varsayılan | Açıklama|  
|-----------|-------|--------|---------|------------|  
|**Configurationchangepollınterval**|duration|İsteğe Bağlı | PT1M| Tanılama izleyicisinin tanılama yapılandırması değişikliklerini yokladığı aralığı belirtir.|  
|**overallQuotaInMB**|unsignedInt|İsteğe Bağlı| 4000 MB. Bir değer sağlarsanız, bu miktarı aşmaması gerekir |Tüm günlük arabellekleri için ayrılan toplam dosya sistemi depolama miktarı.|  

## <a name="diagnosticinfrastructurelogs-element"></a>DiagnosticInfrastructureLogs öğesi  
Temel alınan tanılama altyapısı tarafından oluşturulan Günlükler için arabellek yapılandırmasını tanımlar.

Üst öğe: DiagnosticMonitorConfiguration öğesi.  

Öznitelikler:

|Öznitelik|Type|Açıklama|  
|---------|----|-----------------|  
|**bufferQuotaInMB**|unsignedInt|İsteğe bağlı. Belirtilen veriler için kullanılabilen en fazla dosya sistemi depolama miktarını belirtir.<br /><br /> Varsayılan değer 0 ' dır.|  
|**scheduledTransferLogLevelFilter**|dize|İsteğe bağlı. Aktarılan günlük girişlerinin en düşük önem derecesini belirtir. Varsayılan değer **tanımsızdır**. Diğer olası değerler **ayrıntılı**, **bilgi**, **Uyarı**, **hata**ve **kritik öneme**sahiptir.|  
|**scheduledTransferPeriod**|duration|İsteğe bağlı. Zamanlanan veri aktarımları arasındaki aralığı, en yakın dakikaya yuvarlayarak belirtir.<br /><br /> Varsayılan değer PT0S ' dir.|  

## <a name="logs-element"></a>Logs öğesi  
 Temel Azure günlükleri için arabellek yapılandırmasını tanımlar.

 Üst öğe: DiagnosticMonitorConfiguration öğesi.  

Öznitelikler:  

|Öznitelik|Type|Açıklama|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|İsteğe bağlı. Belirtilen veriler için kullanılabilen en fazla dosya sistemi depolama miktarını belirtir.<br /><br /> Varsayılan değer 0 ' dır.|  
|**scheduledTransferLogLevelFilter**|dize|İsteğe bağlı. Aktarılan günlük girişlerinin en düşük önem derecesini belirtir. Varsayılan değer **tanımsızdır**. Diğer olası değerler **ayrıntılı**, **bilgi**, **Uyarı**, **hata**ve **kritik öneme**sahiptir.|  
|**scheduledTransferPeriod**|duration|İsteğe bağlı. Zamanlanan veri aktarımları arasındaki aralığı, en yakın dakikaya yuvarlayarak belirtir.<br /><br /> Varsayılan değer PT0S ' dir.|  

## <a name="directories-element"></a>Dizinler öğesi  
Tanımlayabilmeniz için dosya tabanlı Günlükler için arabellek yapılandırmasını tanımlar.

Üst öğe: DiagnosticMonitorConfiguration öğesi.  


Öznitelikler:  

|Öznitelik|Type|Açıklama|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|İsteğe bağlı. Belirtilen veriler için kullanılabilen en fazla dosya sistemi depolama miktarını belirtir.<br /><br /> Varsayılan değer 0 ' dır.|  
|**scheduledTransferPeriod**|duration|İsteğe bağlı. Zamanlanan veri aktarımları arasındaki aralığı, en yakın dakikaya yuvarlayarak belirtir.<br /><br /> Varsayılan değer PT0S ' dir.|  

## <a name="crashdumps-element"></a>Crashdökümler öğesi  
 Kilitlenme dökümü dizinini tanımlar.

 Üst öğe: Dizinler öğesi.  

Öznitelikler:  

|Öznitelik|Type|Açıklama|  
|---------------|----------|-----------------|  
|**kapsayıcı**|dize|Dizinin içeriklerinin aktarılacağı kapsayıcının adı.|  
|**Directoryquocontainer MB**|unsignedInt|İsteğe bağlı. Dizinin en büyük boyutunu megabayt cinsinden belirtir.<br /><br /> Varsayılan değer 0 ' dır.|  

## <a name="failedrequestlogs-element"></a>FailedRequestLogs öğesi  
 Başarısız istek günlüğü dizinini tanımlar.

 Üst öğe dizinleri öğesi.  

Öznitelikler:  

|Öznitelik|Type|Açıklama|  
|---------------|----------|-----------------|  
|**kapsayıcı**|dize|Dizinin içeriklerinin aktarılacağı kapsayıcının adı.|  
|**Directoryquocontainer MB**|unsignedInt|İsteğe bağlı. Dizinin en büyük boyutunu megabayt cinsinden belirtir.<br /><br /> Varsayılan değer 0 ' dır.|  

##  <a name="iislogs-element"></a>Iıslogs öğesi  
 IIS günlük dizinini tanımlar.

 Üst öğe dizinleri öğesi.  

Öznitelikler:  

|Öznitelik|Type|Açıklama|  
|---------------|----------|-----------------|  
|**kapsayıcı**|dize|Dizinin içeriklerinin aktarılacağı kapsayıcının adı.|  
|**Directoryquocontainer MB**|unsignedInt|İsteğe bağlı. Dizinin en büyük boyutunu megabayt cinsinden belirtir.<br /><br /> Varsayılan değer 0 ' dır.|  

## <a name="datasources-element"></a>DataSources öğesi  
 Sıfır veya daha fazla ek günlük dizinini tanımlar.

 Üst öğe: Dizinler öğesi.

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration öğesi  
 İzlenecek günlük dosyalarının dizinini tanımlar.

 Üst öğe: DataSources öğesi.

Öznitelikler:

|Öznitelik|Type|Açıklama|  
|---------------|----------|-----------------|  
|**kapsayıcı**|dize|Dizinin içeriklerinin aktarılacağı kapsayıcının adı.|  
|**Directoryquocontainer MB**|unsignedInt|İsteğe bağlı. Dizinin en büyük boyutunu megabayt cinsinden belirtir.<br /><br /> Varsayılan değer 0 ' dır.|  

## <a name="absolute-element"></a>Mutlak öğe  
 İsteğe bağlı ortam genişletmesiyle izlenecek dizinin mutlak yolunu tanımlar.

 Üst öğe: DirectoryConfiguration öğesi.  

Öznitelikler:  

|Öznitelik|Type|Açıklama|  
|---------------|----------|-----------------|  
|**Yolu**|dize|Gerekli. İzlenecek dizinin mutlak yolu.|  
|**expandEnvironment**|boolean|Gerekli. **True**olarak ayarlanırsa yoldaki ortam değişkenleri genişletilir.|  

## <a name="localresource-element"></a>LocalResource öğesi  
 Hizmet tanımında tanımlanan bir yerel kaynakla ilişkili bir yol tanımlar.

 Üst öğe: DirectoryConfiguration öğesi.  

Öznitelikler:  

|Öznitelik|Type|Açıklama|  
|---------------|----------|-----------------|  
|**name**|dize|Gerekli. İzlenecek dizini içeren yerel kaynağın adı.|  
|**relativePath**|dize|Gerekli. İzlenecek yerel kaynakla ilgili yol.|  

## <a name="performancecounters-element"></a>PerformanceCounters öğesi  
 Toplanacak performans sayacının yolunu tanımlar.

 Üst öğe: DiagnosticMonitorConfiguration öğesi.


 Öznitelikler:  

|Öznitelik|Type|Açıklama|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|İsteğe bağlı. Belirtilen veriler için kullanılabilen en fazla dosya sistemi depolama miktarını belirtir.<br /><br /> Varsayılan değer 0 ' dır.|  
|**scheduledTransferPeriod**|duration|İsteğe bağlı. Zamanlanan veri aktarımları arasındaki aralığı, en yakın dakikaya yuvarlayarak belirtir.<br /><br /> Varsayılan değer PT0S ' dir.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration öğesi  
 Toplanacak performans sayacını tanımlar.

 Üst öğe: PerformanceCounters öğesi.  

 Öznitelikler:  

|Öznitelik|Type|Açıklama|  
|---------------|----------|-----------------|  
|**Onay Belirleyicisi**|dize|Gerekli. Toplanacak performans sayacının yolu.|  
|**Örnekleray**|duration|Gerekli. Performans sayacının toplanması gereken oran.|  

## <a name="windowseventlog-element"></a>WindowsEventLog öğesi  
 İzlenecek olay günlüklerini tanımlar.

 Üst öğe: DiagnosticMonitorConfiguration öğesi.

  Öznitelikler:

|Öznitelik|Type|Açıklama|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|unsignedInt|İsteğe bağlı. Belirtilen veriler için kullanılabilen en fazla dosya sistemi depolama miktarını belirtir.<br /><br /> Varsayılan değer 0 ' dır.|  
|**scheduledTransferLogLevelFilter**|dize|İsteğe bağlı. Aktarılan günlük girişlerinin en düşük önem derecesini belirtir. Varsayılan değer **tanımsızdır**. Diğer olası değerler **ayrıntılı**, **bilgi**, **Uyarı**, **hata**ve **kritik öneme**sahiptir.|  
|**scheduledTransferPeriod**|duration|İsteğe bağlı. Zamanlanan veri aktarımları arasındaki aralığı, en yakın dakikaya yuvarlayarak belirtir.<br /><br /> Varsayılan değer PT0S ' dir.|  

## <a name="datasource-element"></a>DataSource öğesi  
 İzlenecek olay günlüğünü tanımlar.

 Üst öğe: WindowsEventLog öğesi.  

 Öznitelikler:

|Öznitelik|Type|Açıklama|  
|---------------|----------|-----------------|  
|**name**|dize|Gerekli. Toplanacak günlüğü belirten bir XPath ifadesi.|  

