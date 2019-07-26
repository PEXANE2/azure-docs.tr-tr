---
title: Akışı Azure Tanılama bir Cloud Services uygulamasında izleme | Microsoft Docs
description: Hata ayıklama, performans, izleme, trafik analizi ve daha fazlasını gidermek için bir Azure uygulamasına izleme iletileri ekleyin.
services: cloud-services
documentationcenter: .net
author: georgewallace
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.date: 02/20/2016
ms.author: gwallace
ms.openlocfilehash: e3e34ff9b5ce1c3a7b45468d22faddddf0c9a913
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68359136"
---
# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>Azure Tanılama bir Cloud Services uygulamasının akışını izleme
İzleme, çalışırken uygulamanızın yürütülmesini izlemenize yönelik bir yoldur. Daha sonra, günlüklerde, metin dosyalarında veya diğer cihazlarda hatalar ve uygulama yürütme bilgilerini kaydetmek için [System. Diagnostics. Trace](/dotnet/api/system.diagnostics.trace), [System. Diagnostics. Debug](/dotnet/api/system.diagnostics.debug)ve [System. Diagnostics. TraceSource](/dotnet/api/system.diagnostics.tracesource) sınıflarını kullanabilirsiniz çözümlemeleri. İzleme hakkında daha fazla bilgi için bkz. [uygulamaları izleme ve işaretleme](/dotnet/framework/debug-trace-profile/tracing-and-instrumenting-applications).

## <a name="use-trace-statements-and-trace-switches"></a>Trace deyimlerini ve izleme anahtarlarını kullanma
Uygulama yapılandırmasına [Diagnosticmonitortracelistener](/previous-versions/azure/reference/ee758610(v=azure.100)) ekleyerek ve uygulama kodunuzda System. Diagnostics. Trace veya System. Diagnostics. Debug için çağrılar yaparak Cloud Services uygulamanızda izlemeyi uygulayın. Çalışan rolleri için *app. config* yapılandırma dosyasını ve Web rolleri için *Web. config* ' i kullanın. Visual Studio şablonu kullanarak yeni bir barındırılan hizmet oluşturduğunuzda, Azure Tanılama projeye otomatik olarak eklenir ve DiagnosticMonitorTraceListener, eklediğiniz roller için uygun yapılandırma dosyasına eklenir.

Trace deyimlerini yerleştirme hakkında daha fazla bilgi için [bkz. nasıl yapılır: Uygulama koduna](/dotnet/framework/debug-trace-profile/how-to-add-trace-statements-to-application-code)Trace deyimlerini ekleyin.

Kodunuzda [Izleme anahtarları](/dotnet/framework/debug-trace-profile/trace-switches) yerleştirerek, izlemenin yapılıp yapılmayacağını ve ne kadar geniş olduğunu kontrol edebilirsiniz. Bu, uygulamanızın durumunu bir üretim ortamında izlemenize olanak sağlar. Bu, özellikle birden çok bilgisayarda çalışan birden çok bileşen kullanan bir iş uygulamasında önemlidir. Daha fazla bilgi için [nasıl yapılır: Izleme anahtarlarını](/dotnet/framework/debug-trace-profile/how-to-create-initialize-and-configure-trace-switches)yapılandırın.

## <a name="configure-the-trace-listener-in-an-azure-application"></a>Azure uygulamasında izleme dinleyicisini yapılandırma
Trace, Debug ve TraceSource, gönderilen iletileri toplamak ve kaydetmek için "dinleyicileri" ayarlamanızı gerektirir. Dinleyiciler toplama, depolama ve izleme iletilerini yönlendirme. İzleme çıkışını, günlük, pencere veya metin dosyası gibi uygun bir hedefe yönlendirir. Azure Tanılama [Diagnosticmonitortracelistener](/previous-versions/azure/reference/ee758610(v=azure.100)) sınıfını kullanır.

Aşağıdaki yordamı tamamlamadan önce Azure tanılama İzleyicisi 'ni başlatmalısınız. Bunu yapmak için bkz. [Microsoft Azure tanılamayı etkinleştirme](cloud-services-dotnet-diagnostics.md).

Visual Studio tarafından sunulan şablonları kullanırsanız, dinleyicinin yapılandırmasının sizin için otomatik olarak eklendiğini unutmayın.

### <a name="add-a-trace-listener"></a>İzleme dinleyicisi ekleme
1. Rolünüzün Web. config veya App. config dosyasını açın.
2. Dosyasına aşağıdaki kodu ekleyin. Sürüm özniteliğini, başvurduğunuz derlemenin sürüm numarasını kullanacak şekilde değiştirin. Derleme sürümü, güncelleştirme olmadığı müddetçe her bir Azure SDK sürümü ile değişmeyebilir.
   
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
   > Microsoft. WindowsAzure. Diagnostics derlemesine bir proje başvurunuz olduğundan emin olun. Yukarıdaki XML 'deki sürüm numarasını, başvurulan Microsoft. WindowsAzure. Diagnostics derlemesinin sürümüyle eşleşecek şekilde güncelleştirin.
   > 
   > 
3. Yapılandırma dosyasını kaydedin.

Dinleyiciler hakkında daha fazla bilgi için bkz. [Trace dinleyicileri](/dotnet/framework/debug-trace-profile/trace-listeners).

Dinleyiciyi ekleme adımlarını tamamladıktan sonra, kodunuza Trace deyimlerini ekleyebilirsiniz.

### <a name="to-add-trace-statement-to-your-code"></a>Kodunuza Trace ifadesini eklemek için
1. Uygulamanız için bir kaynak dosyası açın. Örneğin, \<roleName, çalışan rolü veya Web rolü için. cs dosyası >.
2. Önceden eklenmemişse Aşağıdaki using ifadesini ekleyin:
    ```
        using System.Diagnostics;
    ```
3. Uygulamanızın durumu hakkında bilgi yakalamak istediğiniz Izleme deyimlerini ekleyin. Trace ifadesinin çıkışını biçimlendirmek için çeşitli yöntemler kullanabilirsiniz. Daha fazla bilgi için [nasıl yapılır: Uygulama koduna](/dotnet/framework/debug-trace-profile/how-to-add-trace-statements-to-application-code)Trace deyimlerini ekleyin.
4. Kaynak dosyayı kaydedin.

