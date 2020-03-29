---
title: Azure Tanılama ile Bulut Hizmetleri Uygulamasında akışı izleme
titleSuffix: Azure Cloud Services
description: Hata ayıklama, performans ölçme, izleme, trafik analizi ve daha fazlasına yardımcı olmak için bir Azure uygulamasına izleme iletileri ekleyin.
services: cloud-services
documentationcenter: .net
author: tgore03
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.date: 02/20/2016
ms.author: tagore
ms.openlocfilehash: 47a33ba27dd6d2df626d93695c421303bace6a0b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75386519"
---
# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>Azure Tanılama ile Bulut Hizmetleri uygulamasının akışını izleme
İzleme, uygulamanız çalışırken yürütülmesini izlemeniz için bir yoldur. [System.Diagnostics.Trace,](/dotnet/api/system.diagnostics.trace) [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug)ve [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) sınıflarını kullanarak günlüklerde, metin dosyalarında veya daha sonraki aygıtlarda hata ve uygulama yürütmesi hakkındaki bilgileri kaydedebilirsiniz. İzleme hakkında daha fazla bilgi için [bkz.](/dotnet/framework/debug-trace-profile/tracing-and-instrumenting-applications)

## <a name="use-trace-statements-and-trace-switches"></a>İzleme ifadeleri ve izleme anahtarları kullanma
Uygulama yapılandırmasına [DiagnosticMonitorTraceListener'ı](/previous-versions/azure/reference/ee758610(v=azure.100)) ekleyerek ve uygulama kodunuzdaki System.Diagnostics.Trace.Trace veya System.Diagnostics.Debug'a çağrı lar yaparak Bulut Hizmetleri uygulamanızda izleme uygulayın. İşçi rolleri için configuration file *app.config'i* ve web rolleri için *web.config'i* kullanın. Visual Studio şablonu kullanarak yeni bir barındırılan hizmet oluşturduğunuzda, Azure Diagnostics projeye otomatik olarak eklenir ve DiagnosticMonitorTraceListener eklediğiniz roller için uygun yapılandırma dosyasına eklenir.

İzleme deyimleri yerleştirme hakkında bilgi için [bkz: Uygulama Koduna İzleme İfadeleri Ekleme](/dotnet/framework/debug-trace-profile/how-to-add-trace-statements-to-application-code).

Kodunuzda [İzleme Anahtarları](/dotnet/framework/debug-trace-profile/trace-switches) yerleştirerek, izlemenin gerçekleşip oluşmadığını ve ne kadar kapsamlı olduğunu denetleyebilirsiniz. Bu, uygulamanızın bir üretim ortamındaki durumunu izlemenize olanak tanır. Bu, özellikle birden çok bilgisayarda çalışan birden çok bileşen kullanan bir iş uygulamasında önemlidir. Daha fazla bilgi için [bkz: İzleme Anahtarlarını Yapılandırma.](/dotnet/framework/debug-trace-profile/how-to-create-initialize-and-configure-trace-switches)

## <a name="configure-the-trace-listener-in-an-azure-application"></a>Bir Azure uygulamasında izleme dinleyicisini yapılandırma
Trace, Debug ve TraceSource, gönderilen iletileri toplamak ve kaydetmek için "dinleyici" ayarlamanızı gerektirir. Dinleyiciler izleme iletilerini toplar, saklar ve yönlendirir. İzleme çıktısını günlük, pencere veya metin dosyası gibi uygun bir hedefe yönlendirirler. Azure [Diagnostics, DiagnosticMonitorTraceListener](/previous-versions/azure/reference/ee758610(v=azure.100)) sınıfını kullanır.

Aşağıdaki yordamı tamamlamadan önce Azure tanı monitörünü başlatmanız gerekir. Bunu yapmak için bkz. [Microsoft Azure'da Tanılamayı Etkinleştirme.](cloud-services-dotnet-diagnostics.md)

Visual Studio tarafından sağlanan şablonları kullanırsanız, dinleyicinin yapılandırmasının sizin için otomatik olarak eklandığını unutmayın.

### <a name="add-a-trace-listener"></a>İzleme dinleyicisi ekleme
1. Rolünüz için web.config veya app.config dosyasını açın.
2. Dosyaya aşağıdaki kodu ekleyin. Başvuruda bulunduğunuz derlemenin sürüm numarasını kullanmak için Sürüm özniteliğini değiştirin. Güncelleştirmeler olmadığı sürece, derleme sürümü her Azure SDK sürümünde mutlaka değişmez.
   
    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                    <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
   > [!IMPORTANT]
   > Microsoft.WindowsAzure.Diagnostics derlemesine bir proje başvurusu yaptığınızdan emin olun. Başvurulan Microsoft.WindowsAzure.Diagnostics derlemesinin sürümüyle eşleşecek şekilde yukarıdaki xml'deki sürüm numarasını güncelleştirin.
   > 
   > 
3. Config dosyasını kaydedin.

Dinleyiciler hakkında daha fazla bilgi için [Trace Dinleyicileri'ne](/dotnet/framework/debug-trace-profile/trace-listeners)bakın.

Dinleyiciyi eklemek için adımları tamamladıktan sonra, kodunuza izleme ifadeleri ekleyebilirsiniz.

### <a name="to-add-trace-statement-to-your-code"></a>Kodunuza izleme ifadesi eklemek için
1. Uygulamanız için bir kaynak dosya açın. Örneğin, işçi \<rolü veya web rolü için RoleName>.cs dosyası.
2. Daha önce eklenmediyse aşağıdaki yönergeyi ekleyin:
    ```
        using System.Diagnostics;
    ```
3. Uygulamanızın durumu hakkında bilgi yakalamak istediğiniz İzleme deyimleri ekleyin. İzleme deyiminin çıktısını biçimlendirmek için çeşitli yöntemler kullanabilirsiniz. Daha fazla bilgi için [bkz: Uygulama Koduna İzleme İfadeleri Ekleyin.](/dotnet/framework/debug-trace-profile/how-to-add-trace-statements-to-application-code)
4. Kaynak dosyayı kaydedin.




