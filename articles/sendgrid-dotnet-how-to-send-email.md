---
title: SendGrid e-posta hizmeti (.NET) nasıl kullanılır | Microsoft Dokümanlar
description: Azure'daki SendGrid e-posta hizmetiyle nasıl e-posta göndereceğinizi öğrenin. C# ile yazılmış kod örnekleri ve .NET API'yi kullanın.
services: ''
documentationcenter: .net
author: georgewallace
ms.author: gwallace
editor: ''
ms.assetid: 21bf4028-9046-476b-9799-3d3082a0f84c
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/15/2017
ms.reviewer: dx@sendgrid.com
ms.openlocfilehash: 33df6b5c8c5c16a6eb896944de05068affc2b407
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062209"
---
# <a name="how-to-send-email-using-sendgrid-with-azure"></a>Azure ile SendGrid Kullanarak E-posta Gönderme
## <a name="overview"></a>Genel Bakış
Bu kılavuz, Azure'daki SendGrid e-posta hizmetiyle ortak programlama görevlerinin nasıl gerçekleştirililebildiğini gösterir. Örnekler C\# ile yazılır ve .NET Standart 1.3'u destekler. Kapsanan senaryolar arasında e-posta oluşturma, e-posta gönderme, ek ekleme ve çeşitli posta ve izleme ayarlarını etkinleştirme yer alır. SendGrid ve e-posta gönderme hakkında daha fazla bilgi için [Sonraki adımlar][Next steps] bölümüne bakın.

## <a name="what-is-the-sendgrid-email-service"></a>SendGrid E-posta Hizmeti nedir?
SendGrid, özel entegrasyonu kolaylaştıran esnek API'lerin yanı sıra güvenilir [işlemsel e-posta teslimi,]ölçeklenebilirlik ve gerçek zamanlı analiz sağlayan bulut tabanlı bir [e-posta hizmetidir.] Sık kullanılan SendGrid kullanım örnekleri şunlardır:

* Müşterilere otomatik olarak makbuz veya satın alma onayları gönderme.
* Müşterilere aylık el ilanları ve promosyonlar göndermek için dağıtım listeleri yönetme.
* Engellenen e-posta ve müşteri etkileşimi gibi şeyler için gerçek zamanlı ölçümler toplama.
* Müşteri sorgularını iletmek.
* Gelen e-postaları işleme.

Daha fazla bilgi [https://sendgrid.com](https://sendgrid.com) için SendGrid'in [C# kitaplığı][sendgrid-csharp] GitHub repo'yu ziyaret edin veya gönderin.

## <a name="create-a-sendgrid-account"></a>SendGrid Hesabı Oluşturma
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a>SendGrid .NET Sınıf Kitaplığı'na Başvurun
[SendGrid NuGet paketi,](https://www.nuget.org/packages/Sendgrid) SendGrid API'sini almanın ve uygulamanızı tüm bağımlılıklarla yapılandırmanın en kolay yoludur. NuGet, Microsoft Visual Studio 2015 ve üzeri ile birlikte verilen ve kitaplıkları ve araçları yüklemeyi ve güncellemeyi kolaylaştıran bir Visual Studio uzantısıdır.

> [!NOTE]
> Visual Studio 2015'ten önce Visual Studio'nun bir sürümünü [https://www.nuget.org](https://www.nuget.org)çalıştırıyorsanız NuGet'i yüklemek için NuGet'i ziyaret edin ve **NuGet'i Yükle** düğmesini tıklayın.
>
>

SendGrid NuGet paketini uygulamanızda yüklemek için aşağıdakileri yapın:

1. Yeni **Proje'ye** tıklayın ve bir **Şablon**seçin.

   ![Yeni bir proje oluşturma][create-new-project]
2. **Çözüm Gezgini'nde,** **Başvurular'ı**sağ tıklatın, ardından **NuGet Paketlerini Yönet'i**tıklatın.

   ![SendGrid NuGet paketi][SendGrid-NuGet-package]
3. **SendGrid'i** arayın ve sonuç listesindeki **SendGrid** öğesini seçin.
4. Bu makalede gösterilen nesne modeli ve API'lerle çalışabilmek için sürüm açılır tarafından Nuget paketinin en son kararlı sürümünü seçin.

   ![SendGrid paketi][sendgrid-package]
5. Yüklemeyi tamamlamak için **Yükle'yi** tıklatın ve sonra bu iletişim kutusunu kapatın.

SendGrid'in .NET sınıf kitaplığı **SendGrid**olarak adlandırılır. Aşağıdaki ad alanlarını içerir:

* SendGrid'in API'si ile iletişim kurmak için **SendGrid.**
* E-posta göndermenin nasıl yapılacağını belirten SendGridMessage nesnelerini kolayca oluşturmak için yardımcı yöntemler için **SendGrid.Helpers.Mail** gönderin.

SendGrid e-posta hizmetine programlı olarak erişmek istediğiniz c# dosyasının üst bölümüne aşağıdaki kod ad alanı bildirimlerini ekleyin.

    using SendGrid;
    using SendGrid.Helpers.Mail;

## <a name="how-to-create-an-email"></a>Nasıl?
E-posta iletisi oluşturmak için **SendGridMessage** nesnesini kullanın. İleti nesnesi oluşturulduktan sonra, e-posta gönderen, e-posta alıcısı ve e-postanın konusu ve gövdesi dahil olmak üzere özellikleri ve yöntemleri ayarlayabilirsiniz.

Aşağıdaki örnek, tam olarak doldurulan bir e-posta nesnesinin nasıl oluşturulabildiğini gösterir:

    var msg = new SendGridMessage();

    msg.SetFrom(new EmailAddress("dx@example.com", "SendGrid DX Team"));

    var recipients = new List<EmailAddress>
    {
        new EmailAddress("jeff@example.com", "Jeff Smith"),
        new EmailAddress("anna@example.com", "Anna Lidman"),
        new EmailAddress("peter@example.com", "Peter Saddow")
    };
    msg.AddTos(recipients);

    msg.SetSubject("Testing the SendGrid C# Library");

    msg.AddContent(MimeType.Text, "Hello World plain text!");
    msg.AddContent(MimeType.Html, "<p>Hello World!</p>");

**SendGrid** türü tarafından desteklenen tüm özellikler ve yöntemler hakkında daha fazla bilgi için GitHub'daki [sendgrid-csharp'a][sendgrid-csharp] bakın.

## <a name="how-to-send-an-email"></a>Nasıl Yapılsın: E-posta Gönder
Bir e-posta iletisi oluşturduktan sonra SendGrid'in API'sini kullanarak gönderebilirsiniz. Alternatif olarak, [kullanabilirsiniz. NET kütüphanede inşa edilmiştir.][NET-library]

E-posta göndermek, SendGrid API Anahtarınızı sağlamanızı gerektirir. API Anahtarlarını yapılandırma hakkında ayrıntılara ihtiyacınız varsa, lütfen SendGrid'in API Anahtarları [belgelerini][documentation]ziyaret edin.

Bu kimlik bilgilerini Azure portalınız üzerinden Uygulama ayarlarını tıklatarak ve Uygulama ayarlarının altına anahtar/değer çiftleri ekleyerek depolayabilirsiniz.

 ![Azure uygulama ayarları][azure_app_settings]

 Ardından, bunlara aşağıdaki gibi erişebilirsiniz:

    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
    var client = new SendGridClient(apiKey);

Aşağıdaki örnekler, konsol uygulamasıyla SendGrid Web API'sini kullanarak nasıl e-posta iletisi gönderilen leri gösterir.

    using System;
    using System.Threading.Tasks;
    using SendGrid;
    using SendGrid.Helpers.Mail;

    namespace Example
    {
        internal class Example
        {
            private static void Main()
            {
                Execute().Wait();
            }

            static async Task Execute()
            {
                var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
                var client = new SendGridClient(apiKey);
                var msg = new SendGridMessage()
                {
                    From = new EmailAddress("test@example.com", "DX Team"),
                    Subject = "Hello World from the SendGrid CSharp SDK!",
                    PlainTextContent = "Hello, Email!",
                    HtmlContent = "<strong>Hello, Email!</strong>"
                };
                msg.AddTo(new EmailAddress("test@example.com", "Test User"));
                var response = await client.SendEmailAsync(msg);
            }
        }
    }
    
## <a name="how-to-send-email-from-asp-net-core-api-using-mailhelper-class"></a>Nasıl? MailHelper sınıfını kullanarak ASP .NET Core API' den e- posta gönder

Aşağıdaki örnek, ad alanı `MailHelper` sınıfını `SendGrid.Helpers.Mail` kullanarak ASP .NET Core API'dan birden çok kişiye tek bir e-posta göndermek için kullanılabilir. Bu örnekte ASP .NET Core 1.0 kullanıyoruz. 

Bu örnekte, API anahtarı, yukarıdaki `appsettings.json` örneklerde gösterildiği gibi Azure portalından geçersiz kılınabilen dosyada depolanmıştır.

Dosyanın `appsettings.json` içeriği aşağıdakilere benzer olmalıdır:

    {
       "Logging": {
       "IncludeScopes": false,
       "LogLevel": {
       "Default": "Debug",
       "System": "Information",
       "Microsoft": "Information"
         }
       },
     "SENDGRID_API_KEY": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    }

Öncelikle .NET Core API projesinin dosyasına `Startup.cs` aşağıdaki kodu eklememiz gerekir. Bu, API denetleyicisinde `SENDGRID_API_KEY` bağımlılık `appsettings.json` enjeksiyonu kullanarak dosyadan erişebilmemiz için gereklidir. Arayüz `IConfiguration` aşağıdaki `ConfigureServices` yöntemde ekledikten sonra denetleyicinin oluşturucusu enjekte edilebilir. Gerekli kodu `Startup.cs` ekledikten sonra dosyanın içeriği aşağıdaki gibi görünür:

        public IConfigurationRoot Configuration { get; }

        public void ConfigureServices(IServiceCollection services)
        {
            // Add mvc here
            services.AddMvc();
            services.AddSingleton<IConfiguration>(Configuration);
        }

Denetleyicide, `IConfiguration` arabirimi enjekte ettikten sonra, `CreateSingleEmailToMultipleRecipients` birden çok `MailHelper` alıcıya tek bir e-posta göndermek için sınıfın yöntemini kullanabiliriz. Yöntem, ". `showAllRecipients` Bu parametre, e-posta alıcılarının e-posta üstbilgisinin To bölümübölümünde birbirlerinin e-posta adresini görüp göremeyeceğini denetlemek için kullanılabilir. Denetleyici için örnek kod aşağıdaki gibi olmalıdır 

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using SendGrid;
    using SendGrid.Helpers.Mail;
    using Microsoft.Extensions.Configuration;

    namespace SendgridMailApp.Controllers
    {
        [Route("api/[controller]")]
        public class NotificationController : Controller
        {
           private readonly IConfiguration _configuration;

           public NotificationController(IConfiguration configuration)
           {
             _configuration = configuration;
           }      
        
           [Route("SendNotification")]
           public async Task PostMessage()
           {
              var apiKey = _configuration.GetSection("SENDGRID_API_KEY").Value;
              var client = new SendGridClient(apiKey);
              var from = new EmailAddress("test1@example.com", "Example User 1");
              List<EmailAddress> tos = new List<EmailAddress>
              {
                  new EmailAddress("test2@example.com", "Example User 2"),
                  new EmailAddress("test3@example.com", "Example User 3"),
                  new EmailAddress("test4@example.com","Example User 4")
              };
            
              var subject = "Hello world email from Sendgrid ";
              var htmlContent = "<strong>Hello world with HTML content</strong>";
              var displayRecipients = false; // set this to true if you want recipients to see each others mail id 
              var msg = MailHelper.CreateSingleEmailToMultipleRecipients(from, tos, subject, "", htmlContent, false);
              var response = await client.SendEmailAsync(msg);
          }
       }
    }
    
## <a name="how-to-add-an-attachment"></a>Nasıl yapılır: Ek ekleme
Ekler, **AddAttachment** yöntemini arayarak ve eklemek istediğiniz dosya adını ve Base64 kodlanmış içeriği en az şekilde belirterek iletiye eklenebilir. Eklemek istediğiniz her dosya için bu yöntemi bir kez arayarak veya **Add Attachments** yöntemini kullanarak birden çok ek ekleyebilirsiniz. Aşağıdaki örnek, iletiye ek eklemeyi gösterir:

    var banner2 = new Attachment()
    {
        Content = Convert.ToBase64String(raw_content),
        Type = "image/png",
        Filename = "banner2.png",
        Disposition = "inline",
        ContentId = "Banner 2"
    };
    msg.AddAttachment(banner2);

## <a name="how-to-use-mail-settings-to-enable-footers-tracking-and-analytics"></a>Nasıl yapılır: Altlayıcıları, izlemeyi ve analitiği etkinleştirmek için posta ayarlarını kullanma
SendGrid, posta ayarları ve izleme ayarları nın kullanımı yoluyla ek e-posta işlevselliği sağlar. Bu ayarlar, tıklama izleme, Google analitiği, abonelik izleme gibi belirli işlevleri etkinleştirmek için bir e-posta iletisine eklenebilir. Uygulamaların tam listesi için [Ayarlar Belgeleri'ne][settings-documentation]bakın.

Uygulamalar **SendGridMessage** sınıfının bir parçası olarak uygulanan yöntemler kullanılarak **SendGrid** e-posta iletilerine uygulanabilir. Aşağıdaki örnekler altbilgi ve tıklama izleme filtreleri gösterir:

Aşağıdaki örnekler altbilgi ve tıklama izleme filtreleri gösterir:

### <a name="footer-settings"></a>Altbilgi ayarları
    msg.SetFooterSetting(
                         true,
                         "Some Footer HTML",
                         "<strong>Some Footer Text</strong>");

### <a name="click-tracking"></a>İzlemeyi tıklatın
    msg.SetClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Nasıl Kullanılır: Ek SendGrid Hizmetlerini Kullanın
SendGrid, Azure uygulamanızda ek işlevlerden yararlanmak için kullanabileceğiniz birkaç API ve web hook sunar. Daha fazla bilgi için [SendGrid API Başvurusu'na][SendGrid API documentation]bakın.

## <a name="next-steps"></a>Sonraki adımlar
SendGrid E-posta hizmetinin temellerini öğrendiğiniz için, daha fazla bilgi edinmek için bu bağlantıları takip edin.

* SendGrid\# C kitaplık repo: [sendgrid-csharp][sendgrid-csharp]
* SendGrid API belgeleri:<https://sendgrid.com/docs>

[Next steps]: #next-steps
[What is the SendGrid Email Service?]: #whatis
[Create a SendGrid Account]: #createaccount
[Reference the SendGrid .NET Class Library]: #reference
[How to: Create an Email]: #createemail
[How to: Send an Email]: #sendemail
[How to: Add an Attachment]: #addattachment
[How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
[How to: Use Additional SendGrid Services]: #useservices

[create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/new-project.png
[SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/reference.png
[sendgrid-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid-package.png
[azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/azure-app-settings.png
[sendgrid-csharp]: https://github.com/sendgrid/sendgrid-csharp
[SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
[App Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs/API_Reference/api_v3.html
[NET-library]: https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html#-Using-NETs-Builtin-SMTP-Library
[documentation]: https://sendgrid.com/docs/Classroom/Send/api_keys.html
[settings-documentation]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html

[bulut tabanlı e-posta hizmeti]: https://sendgrid.com/solutions
[işlemsel e-posta teslimi]: https://sendgrid.com/use-cases/transactional-email

