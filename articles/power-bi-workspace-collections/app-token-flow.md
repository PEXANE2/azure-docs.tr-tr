---
title: Power BI çalışma alanı koleksiyonlarının kimliğini doğrulama ve yetkilendirme
description: Power BI çalışma alanı koleksiyonlarıyla kimlik doğrulaması ve yetkilendirme.
services: power-bi-workspace-collections
author: rkarlin
ms.author: rkarlin
ms.service: power-bi-embedded
ms.topic: article
ms.workload: powerbi
ms.date: 09/20/2017
ms.openlocfilehash: 8fcd7caffb041c57090d7256361421cb49a9a5fc
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75427104"
---
# <a name="authenticating-and-authorizing-with-power-bi-workspace-collections"></a>Power BI çalışma alanı koleksiyonlarında kimlik doğrulaması ve yetkilendirme

Power BI çalışma alanı koleksiyonları, açık son kullanıcı kimlik doğrulaması yerine kimlik doğrulama ve yetkilendirme için **anahtar** ve **uygulama belirteçlerini** kullanır. Bu modelde, uygulamanız son kullanıcılarınız için kimlik doğrulama ve yetkilendirmeyi yönetir. Gerektiğinde, uygulamanız, hizmetimizin istenen raporu işlemesini söyleyen uygulama belirteçlerini oluşturur ve gönderir. Bu tasarım, uygulamanızın kullanıcı kimlik doğrulaması ve yetkilendirme için Azure Active Directory kullanmasını gerektirmez, ancak yine de yapabilirsiniz.

> [!IMPORTANT]
> Power BI Çalışma Alanı Koleksiyonları kullanım dışı bırakılmıştır ve Haziran 2018'e kadar veya anlaşmanızda belirtilen süre boyunca kullanılabilecektir. Uygulamanızda kesinti yaşanmaması için Power BI Embedded'a geçirmeyi planlamanız önerilir. Verilerinizi Power BI Embedded'a nasıl taşıyacağınızı öğrenmek için bkz. [Power BI Çalışma Alanı Koleksiyonları'nı Power BI Embedded'a geçirme](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/).

## <a name="two-ways-to-authenticate"></a>Kimlik doğrulama için iki yol

**Anahtar** -tüm Power BI çalışma alanı koleksiyonları REST API çağrıları için anahtarlar kullanabilirsiniz. Anahtarlar, **Tüm ayarlar** ' ı ve ardından **anahtarlara erişim**' i seçerek **Microsoft Azure Portal** bulunabilir. Anahtarınızı bir parola gibi her zaman değerlendirin. Bu anahtarların belirli bir çalışma alanı koleksiyonunda herhangi bir REST API çağrısı yapma izni vardır.

REST çağrısında bir anahtar kullanmak için aşağıdaki yetkilendirme üst bilgisini ekleyin:

    Authorization: AppKey {your key}

**Uygulama belirteci** -uygulama belirteçleri tüm ekleme istekleri için kullanılır. Bunlar, istemci tarafı çalıştıracak şekilde tasarlanmıştır. Belirteç, tek bir raporla ve en iyi yöntemi bir sona erme saati ayarlamak için kısıtlanmıştır.

Uygulama belirteçleri, anahtarlarınızın biri tarafından imzalanmış bir JWT (JSON Web Token).

Uygulama belirteciniz aşağıdaki talepleri içerebilir:

| İste | Açıklama |    
| --- | --- |
| **ver** |Uygulama belirtecinin sürümü. 0.2.0 geçerli sürümdür. |
| **AUD** |Belirtecin amaçlanan alıcısı. Power BI çalışma alanı koleksiyonları için şunu kullanın: *https:\//Analysis.Windows.net/powerbi/api*. |
| **ğe** |Belirteci veren uygulamayı gösteren bir dize. |
| **type** |Oluşturulmakta olan uygulama belirtecinin türü. Geçerli tek desteklenen tür **ekleme**. |
| **WCN** |Belirtecin verildiği çalışma alanı koleksiyonu adı. |
| **WID** |Belirtecin verildiği çalışma alanı KIMLIĞI. |
| **Rid** |Belirteç için verilen rapor KIMLIĞI. |
| **Kullanıcı adı** (isteğe bağlı) |RLS ile birlikte kullanılan Kullanıcı adı, RLS kuralları uygulanırken kullanıcının tanımlanmasına yardımcı olabilecek bir dizedir. |
| **Roller** (isteğe bağlı) |Satır düzeyi güvenlik kuralları uygulanırken seçilecek rolleri içeren bir dize. Birden fazla rol geçirilirse, bu, bir toplama dizisi olarak geçirilmelidir. |
| **SCP** (isteğe bağlı) |İzin kapsamlarını içeren bir dize. Birden fazla rol geçirilirse, bu, bir toplama dizisi olarak geçirilmelidir. |
| **Exp** (isteğe bağlı) |Belirtecin süresinin dolacağı süreyi gösterir. Değer, Unix zaman damgaları olarak geçirilmelidir. |
| **NBF** (isteğe bağlı) |Belirtecin geçerli çalışmaya başladığı saati gösterir. Değer, Unix zaman damgaları olarak geçirilmelidir. |

Örnek bir uygulama belirteci şöyle görünür:

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ2ZXIiOiIwLjIuMCIsInR5cGUiOiJlbWJlZCIsIndjbiI6Ikd1eUluQUN1YmUiLCJ3aWQiOiJkNGZlMWViMS0yNzEwLTRhNDctODQ3Yy0xNzZhOTU0NWRhZDgiLCJyaWQiOiIyNWMwZDQwYi1kZTY1LTQxZDItOTMyYy0wZjE2ODc2ZTNiOWQiLCJzY3AiOiJSZXBvcnQuUmVhZCIsImlzcyI6IlBvd2VyQklTREsiLCJhdWQiOiJodHRwczovL2FuYWx5c2lzLndpbmRvd3MubmV0L3Bvd2VyYmkvYXBpIiwiZXhwIjoxNDg4NTAyNDM2LCJuYmYiOjE0ODg0OTg4MzZ9.v1znUaXMrD1AdMz6YjywhJQGY7MWjdCR3SmUSwWwIiI
```

Kodu çözülerek şöyle bir şey görünür:

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

SDK 'larda uygulama belirteçleri oluşturmayı kolaylaştıran yöntemler mevcuttur. Örneğin, .NET için [Microsoft. PowerBI. Security. PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) sınıfına ve [Createreportembedtoken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN) yöntemlerine bakabilirsiniz.

.NET SDK için [kapsamlara](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes)başvurabilirsiniz.

## <a name="scopes"></a>Kapsamlar

Ekleme belirteçlerini kullanırken, erişim verdiğiniz kaynakların kullanımını kısıtlamak isteyebilirsiniz. Bu nedenle, kapsamlı izinlerle bir belirteç oluşturabilirsiniz.

Power BI çalışma alanı koleksiyonları için kullanılabilir kapsamlar aşağıda verilmiştir.

|Kapsam|Açıklama|
|---|---|
|DataSet. Read|Belirtilen veri kümesini okumak için izin sağlar.|
|DataSet. Write|Belirtilen veri kümesine yazmak için izin sağlar.|
|DataSet. ReadWrite|Belirtilen veri kümesini okumak ve yazmak için izin sağlar.|
|Rapor. Read|Belirtilen raporu görüntülemek için izin sağlar.|
|Report. ReadWrite|Belirtilen raporu görüntüleme ve düzenleme izni sağlar.|
|Workspace. Report. Create|Belirtilen çalışma alanı içinde yeni bir rapor oluşturmak için izin sağlar.|
|Workspace. Report. Copy|Belirtilen çalışma alanı içinde varolan bir raporu kopyalamak için izin sağlar.|

Aşağıdaki gibi kapsamlar arasında boşluk kullanarak birden çok kapsam sağlayabilirsiniz.

```csharp
string scopes = "Dataset.Read Workspace.Report.Create";
```

**Gerekli talepler-kapsamlar**

SCP: {scopesClaim} scopesClaim bir dize veya dizeler dizisi olabilir ve çalışma alanı kaynaklarına izin verilen izinleri (rapor, veri kümesi, vb.) ifade edebilir

Tanımlı kapsamlar içeren kodu çözülmüş belirteç şuna benzer:

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "scp": "Report.Read",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

### <a name="operations-and-scopes"></a>İşlemler ve kapsamlar

|İşlem|Hedef kaynak|Belirteç izinleri|
|---|---|---|
|Bir veri kümesini temel alan yeni bir rapor oluşturun (bellek içi).|Veri kümesi|DataSet. Read|
|Bir veri kümesini temel alan yeni bir rapor oluşturun (bellek içi) ve raporu kaydedin.|Veri kümesi|* DataSet. Read<br>* Çalışma alanı. Report. Create|
|Var olan bir raporu görüntüleyin ve bulun/düzenleyin (bellek içi). Report. Read veri kümesi. Read anlamına gelir. Report. Read, düzenlemelerin kaydedilmesine izin vermez.|Rapor|Rapor. Read|
|Var olan bir raporu düzenleyin ve kaydedin.|Rapor|Report. ReadWrite|
|Raporun bir kopyasını kaydedin (farklı Kaydet).|Rapor|* Rapor. Read<br>* Workspace. Report. Copy|

## <a name="heres-how-the-flow-works"></a>Akışın nasıl çalıştığı
1. API anahtarlarını uygulamanıza kopyalayın. Anahtarları **Azure Portal**alabilirsiniz.
   
    ![Azure portal API anahtarlarının nerede bulunacağı](media/get-started-sample/azure-portal.png)
1. Belirteç bir talebi onaylar ve sona erme saati içerir.
   
    ![Uygulama belirteci akış belirteci onaylama talebi](media/get-started-sample/token-2.png)
1. Belirteç bir API erişim anahtarlarıyla imzalanır.
   
    ![Uygulama belirteci akışı-belirteç imzalı](media/get-started-sample/token-3.png)
1. Bir raporu görüntülemek için Kullanıcı istekleri.
   
    ![Uygulama belirteci akışı-bir raporu görüntülemek için Kullanıcı istekleri](media/get-started-sample/token-4.png)
1. Belirteç bir API erişim anahtarlarıyla onaylanır.
   
   ![Uygulama belirteci akışı belirteci doğrulanan](media/get-started-sample/token-5.png)
1. Power BI çalışma alanı koleksiyonları kullanıcıya bir rapor gönderir.
   
   ![Uygulama belirteci akışı-hizmet kullanıcıya rapor gönder](media/get-started-sample/token-6.png)

**Power BI çalışma alanı koleksiyonları** kullanıcıya bir rapor gönderdiğinde, Kullanıcı raporu özel uygulamanızda görüntüleyebilir. Örneğin, [analiz Sales Data PBIX örneğini](https://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix)içeri aktardıysanız örnek Web uygulaması şöyle görünür:

![Uygulamaya katıştırılmış rapor örneği](media/get-started-sample/sample-web-app.png)

## <a name="see-also"></a>Ayrıca Bkz.

[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN)  
[Microsoft Power BI çalışma alanı koleksiyonları örneği ile çalışmaya başlama](get-started-sample.md)  
[Ortak Microsoft Power BI çalışma alanı koleksiyonları senaryoları](scenarios.md)  
[Microsoft Power BI çalışma alanı koleksiyonları ile çalışmaya başlama](get-started.md)  
[PowerBI-CSharp git deposu](https://github.com/Microsoft/PowerBI-CSharp)

Başka sorunuz mu var? [Power BI Topluluğu'nu deneyin](https://community.powerbi.com/)
