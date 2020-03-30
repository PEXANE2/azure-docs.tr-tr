---
title: Azure Analiz Hizmetleri istemci kitaplıkları | Microsoft Dokümanlar
description: Azure Analiz Hizmetleri'ni bağlamak için istemci uygulamaları ve araçları için gerekli istemci kitaplıklarını açıklar
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c5914c7987d5a54a6bcc779231287309517f5121
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80129218"
---
# <a name="client-libraries-for-connecting-to-analysis-services"></a>Analiz Hizmetlerine bağlanmak için istemci kitaplıkları

İstemci kitaplıkları, istemci uygulamalarının ve araçlarının Çözümleme Hizmetleri sunucularına bağlanması için gereklidir. Visual Studio için Power BI Desktop, Excel, SQL Server Management Studio (SSMS) ve Analysis Services proje uzantısı gibi Microsoft istemci uygulamaları üç istemci kitaplıklarını da yükler ve düzenli uygulama güncelleştirmeleriyle birlikte güncelleştirir. Bazı durumlarda, istemci kitaplıklarının daha yeni sürümlerini yüklemeniz gerekebilir. Özel istemci uygulamaları da istemci kitaplıkları yüklü gerektirir.

## <a name="download-the-latest-client-libraries-windows-installer"></a>En son istemci kitaplıklarını (Windows Installer) indirin  

|İndirme  |Ürün sürümü  | 
|---------|---------|
|[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)    |    15.1.29.25    |
|[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)     |     15.1.29.25       |
|[Çyn](https://go.microsoft.com/fwlink/?linkid=829578)     |   18.4.8.0    |
|[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)     |    18.4.8.0     |

## <a name="amo-and-adomd-nuget-packages"></a>AMO ve ADOMD (NuGet paketleri)

Analiz Hizmetleri Yönetimi Nesneleri (AMO) ve ADOMD istemci kitaplıkları [NuGet.org](https://www.nuget.org/)yüklenebilir paketler olarak mevcuttur. Windows Installer kullanmak yerine NuGet başvurularına geçiş yapmamanız önerilir. 

|Paket  | Ürün sürümü  | 
|---------|---------|
|[Çyn](https://www.nuget.org/packages/Microsoft.AnalysisServices.retail.amd64/)    |    18.4.8.0     |
|[ADOMD](https://www.nuget.org/packages/Microsoft.AnalysisServices.AdomdClient.retail.amd64/)     |   18.4.8.0      |

NuGet paket derlemeleri AssemblyVersion semantik sürüm izleyin: MAJOR. Küçük. Yama. NuGet referansları, GAC'de farklı bir sürüm olsa bile (MSI yüklemesinden kaynaklanan) beklenen sürümü yükler. PATCH her sürüm için artımlı. AMO ve ADOMD sürümleri senkronize tutulur.

## <a name="understanding-client-libraries"></a>İstemci kitaplıklarını anlama

Çözümleme Hizmetleri, veri sağlayıcıları olarak da bilinen üç istemci kitaplıklarını kullanır. ADOMD.NET ve Analiz Hizmetleri Yönetim Nesneleri (AMO) istemci kitaplıkları yönetilir. Analiz Hizmetleri OLE DB Sağlayıcısı (MSOLAP DLL) yerel bir istemci kitaplığıdır. Genellikle, her üç aynı anda yüklenir. **Azure Çözümleme Hizmetleri, üç kitaplık tan en son sürümlerini gerektirir.** 

Power BI Desktop ve Excel gibi Microsoft istemci uygulamaları üç istemci kitaplıklarını da yükler ve yeni sürümler kullanılabilir olduğunda güncelleştirin. Güncelleştirmelerin sürümüne veya sıklığına bağlı olarak, bazı istemci kitaplıkları Azure Çözümleme Hizmetleri tarafından gerekli olan en son sürümler olmayabilir. Aynı durum AsCmd, TOM, ADOMD.NET gibi özel uygulamalar ve diğer arabirimler için de geçerlidir. Bu uygulamalar kitaplıkları el ile veya programlı bir şekilde yüklemeyi gerektirir. Manuel yükleme için istemci kitaplıkları, dağıtılabilir paketler olarak SQL Server özellik paketlerine dahildir. Ancak, bu istemci kitaplıkları SQL Server sürümüne bağlıdır ve en son olmayabilir.  

İstemci bağlantıları için istemci kitaplıkları, Bir Azure Analiz Hizmetleri sunucusundan bir veri kaynağına bağlanmak için gereken veri sağlayıcılarından farklıdır. Veri kaynağı bağlantıları hakkında daha fazla bilgi edinmek için [Bkz. Veri kaynağı bağlantıları.](analysis-services-datasource.md)

## <a name="client-library-types"></a>İstemci kitaplığı türleri

### <a name="analysis-services-ole-db-provider-msolap"></a>Analiz Hizmetleri OLE DB Sağlayıcısı (MSOLAP) 

 Analiz Hizmetleri OLE DB Sağlayıcısı (MSOLAP), Çözümhizmetleri veritabanı bağlantıları için yerel istemci kitaplığıdır. Hem ADOMD.NET hem de AMO tarafından dolaylı olarak kullanılır ve bağlantı isteklerini veri sağlayıcısına devretin. OLE DB Sağlayıcısını doğrudan uygulama kodundan da arayabilirsiniz.  
  
 Analiz Hizmetleri OLE DB Sağlayıcısı, Analiz Hizmetleri veritabanlarına erişmek için kullanılan çoğu araç ve istemci uygulaması tarafından otomatik olarak yüklenir. Analiz Hizmetleri verilerine erişmek için kullanılan bilgisayarlara yüklenmesi gerekir.  
  
 OLE DB sağlayıcıları genellikle bağlantı dizelerinde belirtilir. Çözümleme Hizmetleri bağlantı dizesi, OLE DB Sağlayıcısı: MSOLAP'a başvurmak için farklı bir adlandırma kullanır. \<sürüm>.dll.

### <a name="amo"></a>Çyn  

 AMO, sunucu yönetimi ve veri tanımı için kullanılan yönetilen bir istemci kitaplığıdır. Araçlar ve istemci uygulamaları tarafından yüklenir ve kullanılır. Örneğin, SQL Server Management Studio (SSMS) Analiz Hizmetleri'ne bağlanmak için AMO kullanır. AMO kullanan bir bağlantı genellikle en `"data source=\<servername>"`az düzeydedir ve bu bağlantı . Bir bağlantı kurulduktan sonra, veritabanı koleksiyonları ve ana nesnelerle çalışmak için API'yi kullanırsınız. Hem Visual Studio hem de SSMS, Analiz Hizmetleri örneğine bağlanmak için AMO'yu kullanır.  

  
### <a name="adomd"></a>ADOMD

 ADOMD.NET, Çözümleme Hizmetleri verilerini sorgulamak için kullanılan yönetilen bir veri istemcisi kitaplığıdır. Araçlar ve istemci uygulamaları tarafından yüklenir ve kullanılır. 
  
 Bir veritabanına bağlanırken, her üç kitaplık için bağlantı dize özellikleri benzer. [Microsoft.AnalysisServices.AdomdClient.AdomdConnection.ConnectionString](/dotnet/api/microsoft.analysisservices.adomdclient.adomdconnection.connectionstring#Microsoft_AnalysisServices_AdomdClient_AdomdConnection_ConnectionString) kullanarak ADOMD.NET için tanımladığınız hemen hemen tüm bağlantı dizeleri, AMO ve Analiz Hizmetleri OLE DB Sağlayıcısı (MSOLAP) için de çalışır. Daha fazla bilgi için, [Bağlantı dize özellikleri &#40;Analiz Hizmetleri&#41;](https://docs.microsoft.com/analysis-services/instances/connection-string-properties-analysis-services)bakın.  

  
##  <a name="how-to-determine-client-library-version"></a><a name="bkmk_LibUpdate"></a>İstemci kitaplığı sürümü nasıl belirlenir?   
  
### <a name="oleddb-msolap"></a>OLEDDB (MSOLAP)  
  
1.  `C:\Program Files\Microsoft Analysis Services\AS OLEDB\` kısmına gidin. Birden fazla klasörünüz varsa, daha yüksek sayıyı seçin.
  
2.  Sağ tıklayın **msolap.dll** > **Özellikler** > **Ayrıntılar**. Dosya adı msolap140.dll ise, en son sürümden eskidir ve yükseltilmelidir.
    
    ![İstemci kitaplığı ayrıntıları](media/analysis-services-data-providers/aas-msolap-details.png)
    
  
### <a name="amo"></a>Çyn

1. `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices\` kısmına gidin. Birden fazla klasörünüz varsa, daha yüksek sayıyı seçin.
2. **Microsoft.AnalysisServices** > **Özellikleri** > **Ayrıntıları**sağ tıklatın.  

### <a name="adomd"></a>ADOMD

1. `C:\Windows\Microsoft.NET\assembly\GAC_MSIL\Microsoft.AnalysisServices.AdomdClient\` kısmına gidin. Birden fazla klasörünüz varsa, daha yüksek sayıyı seçin.
2. **Microsoft.AnalysisServices.AdomdClient** > **Özellikleri** > **Ayrıntıları**sağ tıklayın.  


## <a name="next-steps"></a>Sonraki adımlar
[Excel ile bağlan](analysis-services-connect-excel.md)    
[Power BI ile bağlanma](analysis-services-connect-pbi.md)
