---
title: Nasıl Twilio (Java) | Microsoft Dokümanlar
description: Azure'daki bir Java uygulamasında Twilio'yu kullanan bir web sayfasından nasıl telefon görüşmesi yapacağınızı öğrenin.
services: ''
documentationcenter: java
author: georgewallace
ms.assetid: 0381789e-e775-41a0-a784-294275192b1d
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/25/2014
ms.author: gwallace
ms.openlocfilehash: 168ec65cfd0ff4e87c33324daa353b554111c8aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73838547"
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Azure'daki Bir Java Uygulamasında Twilio Kullanarak Telefon Görüşmesi Nasıl Yapılır?
Aşağıdaki örnek, Azure'da barındırılan bir web sayfasından arama yapmak için Twilio'yu nasıl kullanabileceğinizi gösterir. Ortaya çıkan uygulama, aşağıdaki ekran görüntüsünde gösterildiği gibi, kullanıcıdan telefon görüşmesi değerleri ister.

![Twilio ve Java kullanarak Azure Çağrı Formu][twilio_java]

Bu konuda kodu kullanmak için aşağıdakileri yapmanız gerekir:

1. Bir Twilio hesabı ve kimlik doğrulama belirteci edinin. Twilio ile başlamak için, [https://www.twilio.com/pricing][twilio_pricing]fiyatlandırmayı . [https://www.twilio.com/try-twilio][try_twilio]Kaydolabilirsiniz. Twilio tarafından sağlanan API hakkında [https://www.twilio.com/api][twilio_api]bilgi için bkz.
2. Twilio JAR'ı edinin. At [https://github.com/twilio/twilio-java][twilio_java_github], GitHub kaynakları indirebilir ve kendi JAR oluşturmak, ya da önceden oluşturulmuş bir JAR indirin (ya da bağımlılıkları olmadan).
   Bu konudaki kod önceden oluşturulmuş TwilioJava-3.3.8-bağımlılıkları JAR kullanılarak yazılmıştır.
3. JAR'ı Java yapı yolunuza ekleyin.
4. Bu Java uygulamasını oluşturmak için Eclipse kullanıyorsanız, Eclipse'in dağıtım montaj özelliğini kullanarak uygulama dağıtım dosyanıza (WAR) Twilio JAR'ı ekleyin. Eclipse'i bu Java uygulamasını oluşturmak için kullanmıyorsanız, Twilio JAR'ın Java uygulamanızla aynı Azure rolüne dahil olduğundan ve uygulamanızın sınıf yoluna eklenmiş olduğundan emin olun.
5. Önbellek keystore'unuzun MD5 parmak izi 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (seri numarası 35:DE:F4:CF ve SHA1 parmak izi D2:32:09:AD:23:D ile Equifax Güvenli Sertifika Yetkilisi sertifikasını içerdiğinden emin olun 3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A). Bu, Twilio API'lerini kullandığınızda çağrılan [https://api.twilio.com][twilio_api_service] hizmetin sertifika yetkilisi (CA) sertifikasıdır. Bu CA sertifikasını JDK'nızın cacert deposuna ekleme hakkında bilgi [için][add_ca_cert]bkz.

Ayrıca, Eclipse için Azure [Araç Kiti'ni Kullanarak Bir Merhaba Dünya Uygulaması Oluşturma'daki][azure_java_eclipse_hello_world]bilgilere veya Eclipse kullanmıyorsanız Azure'da Java uygulamalarını barındırmak için diğer tekniklere aşinalık şiddetle önerilir.

## <a name="create-a-web-form-for-making-a-call"></a>Arama yapmak için web formu oluşturma
Aşağıdaki kod, arama yapmak için kullanıcı verilerini almak için bir web formunun nasıl oluşturulurolduğunu gösterir. Bu örnek amacıyla, **TwilioCloud**adında yeni bir dinamik web projesi oluşturuldu ve **callform.jsp** jsp dosyası olarak eklendi.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "https://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Arama yapmak için kodu oluşturma
Kullanıcı callform.jsp tarafından görüntülenen formu tamamladığında çağrılan aşağıdaki kod, çağrı iletisini oluşturur ve çağrıyı oluşturur. Bu örnek için, JSP dosyası **makecall.jsp** olarak adlandırılır ve **TwilioCloud** projesine eklenmiştir. (Aşağıdaki kodda **accountSID** ve **authToken'e** atanan yer tutucu değerleri yerine Twilio hesabınızı ve kimlik doğrulama belirtecinizi kullanın.)

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "https://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";

         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");

         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");

         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");

         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");

         // Create a URL using the Twilio message and the user-entered text.
         String Url="https://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;

         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");

         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);

         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

Arama yapmanın yanı sıra, makecall.jsp Twilio bitiş noktasını, API sürümünü ve arama durumunu görüntüler. Bir örnek aşağıdaki ekran görüntüsüdür:

![Twilio ve Java kullanarak Azure Çağrı Yanıtı][twilio_java_response]

## <a name="run-the-application"></a>Uygulamayı çalıştırma
Uygulamanızı çalıştırmak için üst düzey adımlar şunlardır; Bu adımlara ilişkin ayrıntılar, [Eclipse için Azure Araç Setini Kullanarak Bir Merhaba Dünya Uygulaması Oluşturma'da][azure_java_eclipse_hello_world]bulunabilir.

1. TwilioCloud WAR'ınızı Azure **approot** klasörüne dışa aktarın. 
2. TwilioCloud WAR'ınızın zip'ini çıkarmak için **startup.cmd'yi** değiştirin.
3. Hesaplama emülatörü için başvurunuzu derle.
4. Dağıtımınızı işlem emülatöründe başlatın.
5. Bir tarayıcı açın `http://localhost:8080/TwilioCloud/callform.jsp`ve çalıştırın.
6. Formdaki değerleri girin, **bu aramayı yapın'ı**tıklatın ve ardından makecall.jsp'de sonuçları görün.

Azure'a dağıtmaya hazır olduğunuzda, buluta dağıtım için yeniden derlenin, Azure'a dağıtın ve tarayıcıda*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp http:// çalıştırın (your_hosted_name *için*değerinizi değiştirin).

## <a name="next-steps"></a>Sonraki adımlar
Bu kod, Azure'da Java'da Twilio kullanarak temel işlevleri göstermek için sağlanmıştır. Üretimde Azure'a dağıtılamadan önce, daha fazla hata işleme veya diğer özellikler eklemek isteyebilirsiniz. Örnek:

* Web formu kullanmak yerine, telefon numaralarını ve arama metinlerini depolamak için Azure depolama bloblarını veya SQL Veritabanı'nı kullanabilirsiniz. Java'da Azure depolama blobları kullanma hakkında daha fazla bilgi [için, Java'dan Blob Depolama Hizmeti nasıl kullanılır][howto_blob_storage_java]' a bakın. 
* Makecall.jsp'deki değerleri sert kodlamak yerine, Dağıtımınızın yapılandırma ayarlarından Twilio hesap kimliği ve kimlik doğrulama belirteci almak için **RoleEnvironment.getConfigurationSettings'i** kullanabilirsiniz. **RoleEnvironment** sınıfı hakkında daha fazla bilgi için [JSP'deki Azure Hizmet Çalışma Zamanı Kitaplığını Kullanma'ya][azure_runtime_jsp]bakın.
* Makecall.jsp kodu [https://twimlets.com/message][twimlet_message_url] **Url** değişkenine Twilio tarafından sağlanan bir URL atar. Bu URL, Twilio'ya çağrıya nasıl devam edileceği konusunda bilgi veren bir Twilio İşaretdili Dili (TwiML) yanıtı sağlar. Örneğin, döndürülen TwiML, çağrı ** &lt;&gt; ** alıcısına konuşulan metinle sonuçlanan bir Say fiili içerebilir. Twilio tarafından sağlanan URL'yi kullanmak yerine, Twilio'nun isteğine yanıt vermek için kendi hizmetinizi oluşturabilirsiniz; daha fazla bilgi için, [Java'da Ses ve SMS Özellikleri için Twilio Nasıl Kullanılır][howto_twilio_voice_sms_java]' a bakın. TwiML hakkında daha fazla [https://www.twilio.com/docs/api/twiml][twiml]bilgi bulunabilir , ve ** &lt;Say&gt; ** ve diğer Twilio [https://www.twilio.com/docs/api/twiml/say][twilio_say]fiiller hakkında daha fazla bilgi bulunabilir .
* Twilio güvenlik yönergelerini [https://www.twilio.com/docs/security][twilio_docs_security]okuyun.

Twilio hakkında daha fazla [https://www.twilio.com/docs][twilio_docs]bilgi için bkz.

## <a name="see-also"></a>Ayrıca Bkz.
* [Java'da Ses ve SMS Özellikleri için Twilio Nasıl Kullanılır?][howto_twilio_voice_sms_java]
* [Java CA Sertifika Mağazasına Sertifika Ekleme][add_ca_cert]

[twilio_pricing]: https://www.twilio.com/pricing
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_api]: https://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: https://github.com/twilio/twilio-java
[twimlet_message_url]: https://twimlets.com/message
[twiml]: https://www.twilio.com/docs/api/twiml
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app 
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: https://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: https://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: https://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[twilio_docs_security]: https://www.twilio.com/docs/security
[twilio_docs]: https://www.twilio.com/docs
[twilio_say]: https://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
