---
title: Azure Analysis Services istemci kitaplıkları | Microsoft Docs
description: İstemci uygulamaları ve araçları 'na bağlanmak için gereken istemci kitaplıklarını açıklar Azure Analysis Services
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 06/01/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 8c02be378febacc4db0b077a3be69339ff9710a0
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84300918"
---
# <a name="client-libraries-for-connecting-to-analysis-services"></a>Analysis Services bağlantı için istemci kitaplıkları

İstemci kitaplıkları, Analysis Services sunucularına bağlanmak için istemci uygulamalar ve araçlar için gereklidir. Visual Studio için Power BI Desktop, Excel, SQL Server Management Studio (SSMS) ve Analysis Services projeleri uzantısı gibi Microsoft istemci uygulamaları, her üç istemci kitaplığı da yükler ve düzenli uygulama güncelleştirmeleriyle birlikte günceller. Bazı durumlarda, istemci kitaplıklarının daha yeni sürümlerini yüklemeniz gerekebilir. Özel istemci uygulamaları da istemci kitaplıklarının yüklü olmasını gerektirir.

## <a name="download-the-latest-client-libraries-windows-installer"></a>En son istemci kitaplıklarını indir (Windows Installer)  

|İndir  |Ürün sürümü  | 
|---------|---------|
|[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)    |    15.1.42.26    |
|[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)     |     15.1.42.26       |
|[AMO](https://go.microsoft.com/fwlink/?linkid=829578)     |   19.2.0.2    |
|[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)     |    19.2.0.2     |

## <a name="amo-and-adomd-nuget-packages"></a>AMO ve ADOMD (NuGet paketleri)

Analysis Services Management Objects (AMO) ve ADOMD istemci kitaplıkları, [NuGet.org](https://www.nuget.org/)adresinden yüklenebilen paketler olarak sunulmaktadır. Windows Installer kullanmak yerine NuGet başvurularına geçiş yapmanız önerilir. 

|Paket  | Ürün sürümü  | 
|---------|---------|
|[AMO](https://www.nuget.org/packages/Microsoft.AnalysisServices.retail.amd64/)    |    19.2.0.2     |
|[ADOMD](https://www.nuget.org/packages/Microsoft.AnalysisServices.AdomdClient.retail.amd64/)     |   19.2.0.2      |

NuGet Paket derlemeleri AssemblyVersion anlam sürümü oluşturma: büyük. Bazı. Düzeltmesi. NuGet başvuruları GAC 'de farklı bir sürüm olsa bile beklenen sürümü yükler (MSI yüklemesi sonucu olur). Her sürüm için düzeltme eki artırılır. AMO ve ADOMD sürümleri eşitlenmiş olarak tutulur.

## <a name="understanding-client-libraries"></a>İstemci kitaplıklarını anlama

Analysis Services, veri sağlayıcıları olarak da bilinen üç istemci kitaplığı kullanır. ADOMD.NET ve Analysis Services Management Objects (AMO), yönetilen istemci kitaplıklarıdır. Analysis Services OLE DB Sağlayıcısı (MSOLAP DLL) yerel bir istemci kitaplığı. Genellikle, üçü de aynı anda yüklenir. **Azure Analysis Services, üç kitaplık için en son sürümleri gerektirir**. 

Power BI Desktop ve Excel gibi Microsoft istemci uygulamaları, her üç istemci kitaplığı da yükler ve yeni sürümler kullanılabilir olduğunda bunları güncelleştirir. Güncelleştirmelerin sürümüne veya sıklığına bağlı olarak, bazı istemci kitaplıkları Azure Analysis Services için gereken en son sürümler olmayabilir. Aynı durum AsCmd, TOM, ADOMD.NET gibi özel uygulamalar ve diğer arabirimler için de geçerlidir. Bu uygulamalar, kitaplıkları el ile veya program aracılığıyla yüklemeyi gerektirir. El ile yükleme için istemci kitaplıkları, dağıtılabilir paketler olarak SQL Server Özellik paketlerine dahil edilmiştir. Ancak, bu istemci kitaplıkları SQL Server sürüme bağlanır ve en son sürümü olmayabilir.  

İstemci bağlantıları için istemci kitaplıkları, bir Azure Analysis Services sunucusundan bir veri kaynağına bağlanmak için gereken veri sağlayıcılarından farklıdır. Veri kaynağı bağlantıları hakkında daha fazla bilgi için bkz. [veri kaynağı bağlantıları](analysis-services-datasource.md).

## <a name="client-library-types"></a>İstemci kitaplığı türleri

### <a name="analysis-services-ole-db-provider-msolap"></a>Analysis Services OLE DB Sağlayıcısı (MSOLAP) 

 Analysis Services OLE DB Sağlayıcısı (MSOLAP), Analysis Services veritabanı bağlantıları için yerel istemci kitaplığıdır. Bu, hem ADOMD.NET hem de AMO tarafından dolaylı olarak kullanılır, bu da veri sağlayıcısına bağlantı istekleri temsilcisi olarak yapılır. OLE DB sağlayıcıyı doğrudan uygulama kodundan da çağırabilirsiniz.  
  
 Analysis Services OLE DB Sağlayıcısı, Analysis Services veritabanlarına erişmek için kullanılan çoğu araç ve istemci uygulaması tarafından otomatik olarak yüklenir. Analysis Services verilerine erişmek için kullanılan bilgisayarlarda yüklü olmalıdır.  
  
 OLE DB sağlayıcılar genellikle bağlantı dizelerinde belirtilir. Analysis Services bağlantı dizesi, OLE DB sağlayıcısına başvurmak için farklı bir terminolojisi kullanır: MSOLAP. \<version> . dosyasını.

### <a name="amo"></a>AMO  

 AMO, sunucu yönetimi ve veri tanımı için kullanılan bir yönetilen istemci kitaplığıdır. Bu, Araçlar ve istemci uygulamaları tarafından yüklenir ve kullanılır. Örneğin, SQL Server Management Studio (SSMS) Analysis Services bağlanmak için AMO kullanır. AMO kullanan bir bağlantı genellikle en az bir ' den oluşur `"data source=\<servername>"` . Bir bağlantı kurulduktan sonra, veritabanı koleksiyonları ve ana nesnelerle çalışmak için API 'yi kullanırsınız. Hem Visual Studio hem de SSMS, bir Analysis Services örneğine bağlanmak için AMO kullanır.  

  
### <a name="adomd"></a>ADOMD

 ADOMD.NET, Analysis Services verileri sorgulamak için kullanılan bir yönetilen veri istemci kitaplığıdır. Bu, Araçlar ve istemci uygulamaları tarafından yüklenir ve kullanılır. 
  
 Bir veritabanına bağlanırken, üç kitaplıkların hepsi için bağlantı dizesi özellikleri benzerdir. ADOMD.NET için tanımladığınız neredeyse tüm bağlantı dizeleri [Microsoft. AnalysisServices. AdomdClient. AdomdConnection. ConnectionString](/dotnet/api/microsoft.analysisservices.adomdclient.adomdconnection.connectionstring#Microsoft_AnalysisServices_AdomdClient_AdomdConnection_ConnectionString) Ile de amo ve ANALYSIS SERVICES OLE DB sağlayıcısı (msolap) için de kullanılır. Daha fazla bilgi için bkz. [bağlantı dizesi özellikleri &#40;Analysis Services&#41;](https://docs.microsoft.com/analysis-services/instances/connection-string-properties-analysis-services).  

  
##  <a name="how-to-determine-client-library-version"></a><a name="bkmk_LibUpdate"></a>İstemci kitaplığı sürümünü belirleme   
  
### <a name="oleddb-msolap"></a>OLEDDB (MSOLAP)  
  
1.  `C:\Program Files\Microsoft Analysis Services\AS OLEDB\` kısmına gidin. Birden fazla klasörünüz varsa, daha yüksek bir sayı seçin.
  
2.  **MSOLAP. dll**  >  **özellikleri**  >  **ayrıntıları**' na sağ tıklayın. Dosya adı msolap140. dll ise, en son sürümden daha eski ve yükseltilmesi gerekir.
    
    ![İstemci kitaplığı ayrıntıları](media/analysis-services-data-providers/aas-msolap-details.png)
    
  
### <a name="amo"></a>AMO

1. `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices\` kısmına gidin. Birden fazla klasörünüz varsa, daha yüksek bir sayı seçin.
2. **Microsoft. AnalysisServices**  >  **özellikleri**  >  **ayrıntıları**' na sağ tıklayın.  

### <a name="adomd"></a>ADOMD

1. `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices.AdomdClient\` kısmına gidin. Birden fazla klasörünüz varsa, daha yüksek bir sayı seçin.
2. **Microsoft. AnalysisServices. AdomdClient**  >  **özellikleri**  >  **ayrıntıları**' na sağ tıklayın.  


## <a name="next-steps"></a>Sonraki adımlar
[Excel ile bağlanma](analysis-services-connect-excel.md)    
[Power BI ile bağlanma](analysis-services-connect-pbi.md)
