---
title: Twilio (.NET) telefon görüşmesi nasıl yapılır | Microsoft Dokümanlar
description: Azure'daki Twilio API hizmetiyle nasıl telefon görüşmesi yapacağınızı ve SMS mesajı göndermeyi öğrenin. .NET'te yazılan kod örnekleri.
services: ''
documentationcenter: .net
author: georgewallace
editor: ''
ms.assetid: 789185ad-69dc-4e9e-a936-42e0a25315c8
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/04/2016
ms.author: gwallace
ms.openlocfilehash: 27b4f3cdd8f622a97cfc0853f79bb77d76673dcf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69636146"
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Azure'daki bir web rolünde Twilio'su kullanarak telefon görüşmesi yapma
Bu kılavuz, Azure'da barındırılan bir web sayfasından arama yapmak için Twilio'nun nasıl kullanılacağını gösterir. Ortaya çıkan uygulama, kullanıcıdan aşağıdaki ekran görüntüsünde gösterildiği gibi verilen numara ve iletiyle birlikte bir arama yapmalarını ister.

![Twilio ve ASP.NET kullanarak Azure çağrı formu][twilio_dotnet_basic_form]

## <a name="prerequisites"></a><a name="twilio-prereqs"></a>Ön koşullar
Bu konuda kodu kullanmak için aşağıdakileri yapmanız gerekir:

1. Twilio [Konsolundan][twilio_console]bir Twilio hesabı ve kimlik doğrulama belirteci edinin. Twilio ile başlamak için, [https://www.twilio.com/try-twilio][try_twilio]kaydolun . Fiyatlandırmayı [https://www.twilio.com/pricing][twilio_pricing]. Twilio tarafından sağlanan API hakkında [https://www.twilio.com/voice/api][twilio_api]bilgi için bkz.
2. *Twilio .NET kitaplığını* web rolünüze ekleyin. Bkz. **Twilio kitaplıklarını web rol projenize eklemek için,** daha sonra bu konuda.

Azure'da temel bir [Web Rolü][azure_webroles_get_started]oluşturmaya aşina olmalısınız.

## <a name="how-to-create-a-web-form-for-making-a-call"></a><a name="howtocreateform"></a>Nasıl yapilir: Arama yapmak için web formu oluşturma
<a id="use_nuget"></a>Twilio kitaplıklarını web rol projenize eklemek için:

1. Visual Studio'da çözümünüzü açın.
2. Sağ tıklatma **Başvuruları**.
3. **NuGet Paketlerini Yönet'i**tıklatın.
4. **Online'ı**tıklatın.
5. Arama çevrimiçi kutusunda, *twilio*yazın.
6. Twilio paketini **yükle'yi** tıklatın.

Aşağıdaki kod, arama yapmak için kullanıcı verilerini almak için bir web formunun nasıl oluşturulurolduğunu gösterir. Bu örnekte, **TwilioCloud** adında ASP.NET bir Web Rolü oluşturulur.

```aspx
<%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
    AutoEventWireup="true" CodeBehind="Default.aspx.cs"
    Inherits="WebRole1._Default" %>

<asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
</asp:Content>
<asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
    <div>
        <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
        </asp:BulletedList>
    </div>
    <div>
        <p>Fill in all fields and click <b>Make this call</b>.</p>
        <div>
            To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
            Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
            <asp:Button ID="callpage" runat="server" Text="Make this call"
                onclick="callpage_Click" />
        </div>
    </div>
</asp:Content>
```

## <a name="how-to-create-the-code-to-make-the-call"></a><a id="howtocreatecode"></a>Nasıl yapılır: Arama yapmak için kodu oluşturma
Kullanıcı formu tamamladığında çağrılan aşağıdaki kod, çağrı iletisini oluşturur ve aramayı oluşturur. Bu örnekte, kod formdaki düğmenin onclick olay işleyicisinde çalıştırılır. (Aşağıdaki kodda `accountSID` ve yer tutucu değerleri yerine Twilio hesabınızı `authToken` ve kimlik doğrulama belirtecinizi kullanın.)

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using Twilio;
using Twilio.Http;
using Twilio.Types;
using Twilio.Rest.Api.V2010;

namespace WebRole1
{
    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {

        }

        protected void callpage_Click(object sender, EventArgs e)
        {
            // Call processing happens here.

            // Use your account SID and authentication token instead of
            // the placeholders shown here.
            var accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
            var authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

            // Instantiate an instance of the Twilio client.
            TwilioClient.Init(accountSID, authToken);

            // Retrieve the account, used later to retrieve the
            var account = AccountResource.Fetch(accountSID);

            this.varDisplay.Items.Clear();

            if (this.toNumber.Text == "" || this.message.Text == "")
            {
                this.varDisplay.Items.Add(
                        "You must enter a phone number and a message.");
            }
            else
            {
                // Retrieve the values entered by the user.
                var to = PhoneNumber(this.toNumber.Text);
                var from = new PhoneNumber("+14155992671");
                var myMessage = this.message.Text;

                // Create a URL using the Twilio message and the user-entered
                // text. You must replace spaces in the user's text with '%20'
                // to make the text suitable for a URL.
                var url = $"https://twimlets.com/message?Message%5B0%5D={myMessage.Replace(" ", "%20")}";
                var twimlUri = new Uri(url);

                // Display the endpoint, API version, and the URL for the message.
                this.varDisplay.Items.Add($"Using Twilio endpoint {
                }");
                this.varDisplay.Items.Add($"Twilioclient API Version is {apiVersion}");
                this.varDisplay.Items.Add($"The URL is {url}");

                // Place the call.
                var call = CallResource.create(to, from, url: twimlUri);
                this.varDisplay.Items.Add("Call status: " + call.Status);
            }
        }
    }
}
```

Arama yapılır ve Twilio bitiş noktası, API sürümü ve arama durumu görüntülenir. Aşağıdaki ekran görüntüsü, örnek bir çalıştırmanın çıktısını gösterir.

![Twilio ve ASP.NET kullanarak Azure arama yanıtı][twilio_dotnet_basic_form_output]

TwiML hakkında daha fazla [https://www.twilio.com/docs/api/twiml][twiml]bilgi bulabilirsiniz. Say &lt;&gt; ve diğer Twilio fiilleri hakkında [https://www.twilio.com/docs/api/twiml/say][twilio_say]daha fazla bilgiyi .

## <a name="next-steps"></a><a id="nextsteps"></a>Sonraki adımlar
Bu kod, Azure'daki ASP.NET bir web rolünde Twilio'u kullanarak temel işlevleri göstermek için sağlanmıştır. Üretimde Azure'a dağıtılamadan önce, daha fazla hata işleme veya diğer özellikler eklemek isteyebilirsiniz. Örnek:

* Web formu kullanmak yerine, telefon numaralarını depolamak ve metin aramak için Azure Blob depolama alanını veya Azure SQL Veritabanı örneğini kullanabilirsiniz. Azure'da Blobs kullanma hakkında daha fazla bilgi için [.NET'te Azure Blob depolama hizmetini nasıl kullanacağınız bilgisini][howto_blob_storage_dotnet]öğrenin. SQL Veritabanı'nı kullanma hakkında daha fazla bilgi için [.NET uygulamalarında Azure SQL Veritabanı'nın nasıl kullanılacağına][howto_sql_azure_dotnet]bakın.
* Twilio hesap kimliği ve kimlik doğrulama belirteci, formunuzdaki değerleri zor kodlamak yerine dağıtımınızın yapılandırma ayarlarından almak için kullanabilirsiniz. `RoleEnvironment.getConfigurationSettings` `RoleEnvironment` Sınıf hakkında daha fazla bilgi için [Microsoft.WindowsAzure.ServiceRuntime Namespace'e][azure_runtime_ref_dotnet]bakın.
* Twilio Güvenlik Yönergeleri'ni [https://www.twilio.com/docs/security][twilio_docs_security]okuyun.
* Twilio hakkında daha [https://www.twilio.com/docs][twilio_docs]fazla bilgi edinin.

## <a name="see-also"></a><a name="seealso"></a>Ayrıca bakınız
* [Azure'dan Ses ve SMS özellikleri için Twilio nasıl kullanılır?](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_console]: https://www.twilio.com/console
[twilio_pricing]: https://www.twilio.com/pricing
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_api]: https://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: https://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: https://www.twilio.com/docs/security
[twilio_docs]: https://www.twilio.com/docs
[twilio_say]: https://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx
[azure_webroles_get_started]: https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-get-started
