---
title: Azure Tanılama uzantısı 1,2 yapılandırma şeması
description: YALNıZCA Azure sanal makineler, sanal makine ölçek kümeleri, Service Fabric veya Cloud Services ile Azure SDK 2,5 kullanıyorsanız ilgilidir.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/15/2017
ms.author: robb
ms.subservice: diagnostic-extension
ms.openlocfilehash: dae74e730d6e175fa3e447150adce4caecd3d7a3
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "60237830"
---
# <a name="azure-diagnostics-12-configuration-schema"></a>Azure Tanılama 1,2 yapılandırma şeması
> [!NOTE]
> Azure Tanılama, Azure sanal makineler, sanal makine ölçek kümeleri, Service Fabric ve Cloud Services performans sayaçlarını ve diğer istatistikleri toplamak için kullanılan bileşendir.  Bu sayfa yalnızca bu hizmetlerden birini kullanıyorsanız ilgilidir.
>

Azure Tanılama, Application Insights ve Log Analytics dahil olmak üzere Azure Izleyici gibi diğer Microsoft tanılama ürünleriyle birlikte kullanılır.

Bu şema, tanılama İzleyicisi başladığında tanılama yapılandırma ayarlarını başlatmak için kullanabileceğiniz olası değerleri tanımlar.  


 Aşağıdaki PowerShell komutunu yürüterek ortak yapılandırma dosyası şema tanımını indirin:  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

 Azure Tanılama kullanma hakkında daha fazla bilgi için bkz. [Azure Cloud Services tanılamayı etkinleştirme](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-diagnostics/).  

## <a name="example-of-the-diagnostics-configuration-file"></a>Tanılama yapılandırma dosyası örneği  
 Aşağıdaki örnek tipik bir tanılama yapılandırma dosyası göstermektedir:  

```xml
<?xml version="1.0" encoding="utf-8"?>  
<PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">  
  <WadCfg>  
    <DiagnosticMonitorConfiguration overallQuotaInMB="10000">  
      <PerformanceCounters scheduledTransferPeriod="PT1M">  
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
        <EtwEventSourceProviderConfiguration provider="MyProviderClass" scheduledTransferPeriod="PT5M">  
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
      <CrashDumps containerName="wad-crashdumps" directoryQuotaPercentage="30" dumpType="Mini">  
        <CrashDumpConfiguration processName="mynewprocess.exe" />  
        <CrashDumpConfiguration processName="badapp.exe"/>  
      </CrashDumps>  
    </DiagnosticMonitorConfiguration>  
  </WadCfg>  
</PublicConfig>  

```  

## <a name="diagnostics-configuration-namespace"></a>Tanılama yapılandırması ad alanı  
 Tanılama yapılandırma dosyası için XML ad alanı:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="publicconfig-element"></a>PublicConfig öğesi  
 Tanılama yapılandırma dosyasının en üst düzey öğesi. Aşağıdaki tabloda yapılandırma dosyasının öğeleri açıklanmaktadır.  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**WadCfg**|Gerekli. Toplanacak Telemetri verilerinin yapılandırma ayarları.|  
|**StorageAccount**|Verilerin depolandığı Azure depolama hesabının adı. Bu, set-Azurezervicediagnokııbu cmdlet 'i yürütürken bir parametre olarak da belirtilebilir.|  
|**LocalResourceDirectory**|Olay verilerini depolamak için Izleme Aracısı tarafından kullanılacak sanal makinedeki dizin. Ayarlanmamışsa, varsayılan dizin kullanılır:<br /><br /> Bir çalışan/Web rolü için:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> Bir sanal makine için:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Gerekli öznitelikler şunlardır:<br /><br /> -                      **yol** -Azure tanılama tarafından kullanılacak sistem üzerindeki dizin.<br /><br /> -                      **Expandenvironment** -ortam değişkenlerinin yol adında genişletilip genişletilmediğini denetler.|  

## <a name="wadcfg-element"></a>WadCFG öğesi  
Toplanacak Telemetri verilerinin yapılandırma ayarlarını tanımlar. Aşağıdaki tabloda alt öğeler açıklanmaktadır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**DiagnosticMonitorConfiguration**|Gerekli. İsteğe bağlı öznitelikler şunlardır:<br /><br /> -                     **Overallquocontainer MB** -Azure tanılama tarafından toplanan çeşitli tanılama verileri türleri tarafından tüketilen en fazla yerel disk alanı miktarı. Varsayılan ayar 5120MB 'tır.<br /><br /> -                     **Useproxyserver** -Azure tanılama IE ayarlarında ayarlandığı şekilde proxy sunucu ayarlarını kullanacak şekilde yapılandırın.|  
|**Formattaki**|Kilitlenme dökümlerinin toplanmasını etkinleştirin. İsteğe bağlı öznitelikler şunlardır:<br /><br /> -                     **ContainerName** -kilitlenme dökümlerini depolamak Için kullanılacak Azure Depolama hesabınızdaki blob kapsayıcısının adı.<br /><br /> -                     **Crashdumptype** -mini veya tam kilitlenme dökümleri toplamak için Azure tanılama yapılandırır.<br /><br /> -                     **Directoryquotapercentage**-VM 'deki kilitlenme dökümleri Için ayrılan **Overallquocontainer MB** yüzdesini yapılandırır.|  
|**DiagnosticInfrastructureLogs**|Azure Tanılama tarafından oluşturulan günlüklerin toplanmasını etkinleştirin. Tanılama altyapısı günlükleri, tanılama sisteminin kendisi için sorun gidermeye yarar. İsteğe bağlı öznitelikler şunlardır:<br /><br /> -                     **Scheduledtransferloglevelfilter** -toplanan günlüklerin en düşük önem derecesi düzeyini yapılandırır.<br /><br /> -                     **Scheduledtransferperiod** -depolamaya en yakın dakikaya yuvarlayarak zamanlanan aktarımlar arasındaki Aralık. Değer bir [XML "Duration veri türüdür."](https://www.w3schools.com/xml/schema_dtypes_date.asp)|  
|**Dizinler**|Bir dizin içeriğinin toplanmasını, IIS, istek günlüklerine ve/veya IIS günlüklerine erişim başarısız oldu. İsteğe bağlı öznitelik:<br /><br /> **Scheduledtransferperiod** -depolamaya en yakın dakikaya yuvarlayarak zamanlanan aktarımlar arasındaki Aralık. Değer bir [XML "Duration veri türüdür."](https://www.w3schools.com/xml/schema_dtypes_date.asp)|  
|**EtwProviders 'lar**|EventSource ve/veya ETW bildirim tabanlı sağlayıcılardan ETW olaylarının toplanmasını yapılandırır.|  
|**Ölçümler**|Bu öğe hızlı sorgular için iyileştirilmiş bir performans sayacı tablosu oluşturmanıza olanak sağlar. **PerformanceCounters** öğesinde tanımlanan her performans sayacı, performans sayacı tablosuna ek olarak ölçümler tablosunda depolanır. Gerekli öznitelik:<br /><br /> **RESOURCEID** -Azure tanılama dağıttığınız sanal makınenın kaynak kimliğidir. [Azure Portal](https://portal.azure.com) **RESOURCEID** 'yi alın. Ada ->  ->  **< kaynakgruplarınagözatamıyorum'ıseçin.\>** **** **Özellikler** kutucuğuna tıklayın ve değeri **kimlik** alanından kopyalayın.|  
|**PerformanceCounters**|Performans sayaçlarının toplanmasını mümkün. İsteğe bağlı öznitelik:<br /><br /> **Scheduledtransferperiod** -depolamaya en yakın dakikaya yuvarlayarak zamanlanan aktarımlar arasındaki Aralık. Değer bir [XML "Duration veri türü" dir.](https://www.w3schools.com/xml/schema_dtypes_date.asp)|  
|**WindowsEventLog**|Windows olay günlükleri koleksiyonunu etkinleştirilir. İsteğe bağlı öznitelik:<br /><br /> **Scheduledtransferperiod** -depolamaya en yakın dakikaya yuvarlayarak zamanlanan aktarımlar arasındaki Aralık. Değer bir [XML "Duration veri türü" dir.](https://www.w3schools.com/xml/schema_dtypes_date.asp)|  

## <a name="crashdumps-element"></a>Crashdökümler öğesi  
 Kilitlenme dökümlerinin toplanmasını etkinleştirilir. Aşağıdaki tabloda alt öğeler açıklanmaktadır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**CrashDumpConfiguration**|Gerekli. Gerekli öznitelik:<br /><br /> **ProcessName** -Azure tanılama kilitlenme dökümü toplamasını istediğiniz işlemin adı.|  
|**crashDumpType**|Mini veya tam kilitlenme dökümleri toplamak için Azure Tanılama yapılandırır.|  
|**directoryQuotaPercentage**|VM 'deki kilitlenme dökümlerinin ayrılması için **Overallquocontainer MB** yüzdesini yapılandırır.|  

## <a name="directories-element"></a>Dizinler öğesi  
 Bir dizin içeriğinin toplanmasını, IIS, istek günlüklerine ve/veya IIS günlüklerine erişim başarısız oldu. Aşağıdaki tabloda alt öğeler açıklanmaktadır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**Kaynağı**|İzlenecek dizinlerin listesi.|  
|**FailedRequestLogs**|Bu öğenin yapılandırmasına dahil edilmesi, bir IIS sitesine veya uygulamasına başarısız istekler hakkında günlüklerin toplanmasını sağlar. Ayrıca sistem altında izleme seçeneklerini etkinleştirmeniz gerekir **.**  **Web. config 'de Web**sunucusu.|  
|**Iıslogs**|Bu öğenin yapılandırmasına dahil edilmesi, IIS günlüklerinin toplanmasını sunar:<br /><br /> **ContainerName** -IIS günlüklerini depolamak Için kullanılacak Azure Depolama hesabınızdaki blob kapsayıcısının adı.|  

## <a name="datasources-element"></a>DataSources öğesi  
 İzlenecek dizinlerin listesi. Aşağıdaki tabloda alt öğeler açıklanmaktadır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**DirectoryConfiguration**|Gerekli. Gerekli öznitelik:<br /><br /> **ContainerName** -günlük dosyalarını depolamak Için kullanılacak Azure Depolama hesabınızdaki blob kapsayıcısının adı.|  

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration öğesi  
 **Directoryconfiguration** **mutlak** ya da **localresource** öğesi içerebilir, ancak ikisini birden içeremez. Aşağıdaki tabloda alt öğeler açıklanmaktadır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**Salt**|İzlenecek dizinin mutlak yolu. Aşağıdaki öznitelikler gereklidir:<br /><br /> -                     **Yol** -izlenecek dizinin mutlak yolu.<br /><br /> -                      **Expandenvironment** -yoldaki ortam değişkenlerinin genişletilip genişletilmediğini yapılandırır.|  
|**LocalResource**|İzlenecek yerel bir kaynağa göreli yol. Gerekli öznitelikler şunlardır:<br /><br /> -                     **Ad** -izlenecek dizini içeren yerel kaynak<br /><br /> -                     **relativePath** -izlenecek dizini içeren ada göre yol|  

## <a name="etwproviders-element"></a>EtwProviders öğesi  
 EventSource ve/veya ETW bildirim tabanlı sağlayıcılardan ETW olaylarının toplanmasını yapılandırır. Aşağıdaki tabloda alt öğeler açıklanmaktadır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|[EventSource sınıfından](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx)oluşturulan olayların koleksiyonunu yapılandırır. Gerekli öznitelik:<br /><br /> **provider** -EventSource olayının sınıf adı.<br /><br /> İsteğe bağlı öznitelikler şunlardır:<br /><br /> -                     **Scheduledtransferloglevelfilter** -depolama hesabınıza aktarılacak en düşük önem düzeyi.<br /><br /> -                     **Scheduledtransferperiod** -depolamaya en yakın dakikaya yuvarlayarak zamanlanan aktarımlar arasındaki Aralık. Değer bir [XML Duration veri türüdür](https://www.w3schools.com/xml/schema_dtypes_date.asp).|  
|**EtwManifestProviderConfiguration**|Gerekli öznitelik:<br /><br /> **sağlayıcı** -olay sağlayıcısının GUID 'si<br /><br /> İsteğe bağlı öznitelikler şunlardır:<br /><br /> - **Scheduledtransferloglevelfilter** -depolama hesabınıza aktarılacak en düşük önem düzeyi.<br /><br /> -                     **Scheduledtransferperiod** -depolamaya en yakın dakikaya yuvarlayarak zamanlanan aktarımlar arasındaki Aralık. Değer bir [XML Duration veri türüdür](https://www.w3schools.com/xml/schema_dtypes_date.asp).|  

## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration öğesi  
 [EventSource sınıfından](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx)oluşturulan olayların koleksiyonunu yapılandırır. Aşağıdaki tabloda alt öğeler açıklanmaktadır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**DefaultEvents**|İsteğe bağlı öznitelik:<br /><br /> **Eventdestination** -olayların depolandığı tablonun adı|  
|**Olay**|Gerekli öznitelik:<br /><br /> **ID** -olayın kimliği.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **Eventdestination** -olayların depolandığı tablonun adı|  

## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration öğesi  
 Aşağıdaki tabloda alt öğeler açıklanmaktadır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**DefaultEvents**|İsteğe bağlı öznitelik:<br /><br /> **Eventdestination** -olayların depolandığı tablonun adı|  
|**Olay**|Gerekli öznitelik:<br /><br /> **ID** -olayın kimliği.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **Eventdestination** -olayların depolandığı tablonun adı|  

## <a name="metrics-element"></a>Ölçüm öğesi  
 Hızlı sorgular için iyileştirilmiş bir performans sayacı tablosu oluşturmanıza olanak sağlar. Aşağıdaki tabloda alt öğeler açıklanmaktadır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**Metrictoplamasını**|Gerekli öznitelik:<br /><br /> **Scheduledtransferperiod** -depolamaya en yakın dakikaya yuvarlayarak zamanlanan aktarımlar arasındaki Aralık. Değer bir [XML Duration veri türüdür](https://www.w3schools.com/xml/schema_dtypes_date.asp).|  

## <a name="performancecounters-element"></a>PerformanceCounters öğesi  
 Performans sayaçlarının toplanmasını mümkün. Aşağıdaki tabloda alt öğeler açıklanmaktadır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**PerformanceCounterConfiguration**|Aşağıdaki öznitelikler gereklidir:<br /><br /> -                     **onay belirticisi** -performans sayacının adı. Örneğin: `\Processor(_Total)\% Processor Time`. Ana bilgisayarınızdaki performans sayaçlarının listesini almak için komutunu `typeperf`çalıştırın.<br /><br /> -                     **örnekler,** sayacın ne sıklıkta örnekleneceği.<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **Unit** -sayacın ölçü birimi.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration öğesi  
 Aşağıdaki tabloda alt öğeler açıklanmaktadır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**ek açıklama**|Gerekli öznitelik:<br /><br /> **DisplayName** -sayaç için görünen ad<br /><br /> İsteğe bağlı öznitelik:<br /><br /> **yerel ayar** -sayaç adı görüntülenirken kullanılacak yerel ayar|  

## <a name="windowseventlog-element"></a>WindowsEventLog öğesi  
 Aşağıdaki tabloda alt öğeler açıklanmaktadır:  

|Öğe adı|Açıklama|  
|------------------|-----------------|  
|**DataSource**|Toplanacak Windows olay günlükleri. Gerekli öznitelik:<br /><br /> **ad** -toplanacak Windows olaylarını açıklayan XPath sorgusu. Örneğin:<br /><br /> `Application!*[System[(Level >= 3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level >= 3]]`<br /><br /> Tüm olayları toplamak için "*" belirtin.|

