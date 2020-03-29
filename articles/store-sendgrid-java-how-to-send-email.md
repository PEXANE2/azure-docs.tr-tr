---
title: SendGrid e-posta hizmeti (Java) nasıl kullanılır | Microsoft Dokümanlar
description: Azure'daki SendGrid e-posta hizmetiyle nasıl e-posta gönderebildiğini öğrenin. Java'da yazılan kod örnekleri.
services: ''
documentationcenter: java
author: thinkingserious
manager: sendgrid
editor: mollybos
ms.assetid: cc75c43e-ede9-492b-98c2-9147fcb92c21
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 10/30/2014
ms.author: erikre
ms.reviewer: elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork
ms.openlocfilehash: 8ae948e9c79cff4cd0c896b250743fd9dc521752
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67876521"
---
# <a name="how-to-send-email-using-sendgrid-from-java"></a>Java'dan SendGrid Kullanarak E-posta Nasıl Gönderilir?
Bu kılavuz, Azure'daki SendGrid e-posta hizmetiyle ortak programlama görevlerinin nasıl gerçekleştirililebildiğini gösterir. Örnekler Java ile yazılmıştır. Kapsanan senaryolar arasında **e-posta oluşturma,** **e-posta gönderme,** **e-posta ekleme,** **filtreleri kullanma**ve **özellikleri güncelleştirme**yer alıyor. SendGrid ve e-posta gönderme hakkında daha fazla bilgi için [Sonraki adımlar](#next-steps) bölümüne bakın.

## <a name="what-is-the-sendgrid-email-service"></a>SendGrid E-posta Hizmeti nedir?
SendGrid, özel entegrasyonu kolaylaştıran esnek API'lerin yanı sıra güvenilir [işlemsel e-posta teslimi,]ölçeklenebilirlik ve gerçek zamanlı analiz sağlayan bulut tabanlı bir [e-posta hizmetidir.] Sık kullanılan SendGrid kullanım senaryoları şunlardır:

* Müşterilere otomatik olarak makbuz gönderme
* Müşterilere aylık e-el ilanları ve özel teklifler göndermek için dağıtım listeleri nin yönetilmesi
* Engellenen e-posta ve müşteri yanıt verme gibi şeyler için gerçek zamanlı ölçümler toplama
* Eğilimleri belirlemeye yardımcı olacak raporlar oluşturma
* Müşteri sorgularını iletme
* Uygulamanızdan gelen e-posta bildirimleri

Daha fazla bilgi için bkz. <https://sendgrid.com>.

## <a name="create-a-sendgrid-account"></a>SendGrid hesabı oluşturma
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="how-to-use-the-javaxmail-libraries"></a>Nasıl kullanılır: javax.mail kitaplıklarını kullanın
Örneğin javax.mail kitaplıklarını <https://www.oracle.com/technetwork/java/javamail> edinin ve bunları kodunuza aktarın. Üst düzey bir düzeyde, SMTP kullanarak e-posta göndermek için javax.mail kitaplığını kullanma işlemi aşağıdakileri yapmaktır:

1. SendGrid için smtp.sendgrid.net smtp sunucusu da dahil olmak üzere SMTP değerlerini belirtin.

```
        import java.util.Properties;
        import javax.mail.*;
        import javax.mail.internet.*;

        public class MyEmailer {
           private static final String SMTP_HOST_NAME = "smtp.sendgrid.net";
           private static final String SMTP_AUTH_USER = "your_sendgrid_username";
           private static final String SMTP_AUTH_PWD = "your_sendgrid_password";

           public static void main(String[] args) throws Exception{
               new MyEmailer().SendMail();
           }

           public void SendMail() throws Exception
           {
              Properties properties = new Properties();
                 properties.put("mail.transport.protocol", "smtp");
                 properties.put("mail.smtp.host", SMTP_HOST_NAME);
                 properties.put("mail.smtp.port", 587);
                 properties.put("mail.smtp.auth", "true");
                 // …
```

1. *javax.mail.Authenticator* sınıfını genişletin ve *getPasswordAuthentication* yöntemini uygulanmasında SendGrid kullanıcı adınızı ve şifrenizi iade edin.  

       private class SMTPAuthenticator extends javax.mail.Authenticator {
       public PasswordAuthentication getPasswordAuthentication() {
          String username = SMTP_AUTH_USER;
          String password = SMTP_AUTH_PWD;
          return new PasswordAuthentication(username, password);
       }
2. *Javax.mail.Session* nesnesi üzerinden kimlik doğrulaması yapılan bir e-posta oturumu oluşturun.  

       Authenticator auth = new SMTPAuthenticator();
       Session mailSession = Session.getDefaultInstance(properties, auth);
3. İletinizi oluşturun ve **,** **From**, **Konu** ve içerik değerlerini atayın. Bu, Nasıl [Yapılır: E-posta oluşturma](#how-to-create-an-email) bölümünde gösterilir.
4. İletiyi *javax.mail.Transport* nesnesi üzerinden gönderin. Bu, [Nasıl Yapılır: E-posta gönderme #how-gönder-e-posta] bölümünde gösterilir.

## <a name="how-to-create-an-email"></a>Nasıl?
Aşağıda, bir e-postanın değerlerinin nasıl belirtilen gösterilmektedir.

    MimeMessage message = new MimeMessage(mailSession);
    Multipart multipart = new MimeMultipart("alternative");
    BodyPart part1 = new MimeBodyPart();
    part1.setText("Hello, Your Contoso order has shipped. Thank you, John");
    BodyPart part2 = new MimeBodyPart();
    part2.setContent(
        "<p>Hello,</p>
        <p>Your Contoso order has <b>shipped</b>.</p>
        <p>Thank you,<br>John</br></p>",
        "text/html");
    multipart.addBodyPart(part1);
    multipart.addBodyPart(part2);
    message.setFrom(new InternetAddress("john@contoso.com"));
    message.addRecipient(Message.RecipientType.TO,
       new InternetAddress("someone@example.com"));
    message.setSubject("Your recent order");
    message.setContent(multipart);

## <a name="how-to-send-an-email"></a>Nasıl yapılsın: E-posta gönderme
Aşağıda e-postanın nasıl gönderilen gösterilmektedir.

    Transport transport = mailSession.getTransport();
    // Connect the transport object.
    transport.connect();
    // Send the message.
    transport.sendMessage(message, message.getAllRecipients());
    // Close the connection.
    transport.close();

## <a name="how-to-add-an-attachment"></a>Nasıl yapılır: Ek ekleme
Aşağıdaki kod, ek eklemeyi gösterir.

    // Local file name and path.
    String attachmentName = "myfile.zip";
    String attachmentPath = "c:\\myfiles\\";
    MimeBodyPart attachmentPart = new MimeBodyPart();
    // Specify the local file to attach.
    DataSource source = new FileDataSource(attachmentPath + attachmentName);
    attachmentPart.setDataHandler(new DataHandler(source));
    // This example uses the local file name as the attachment name.
    // They could be different if you prefer.
    attachmentPart.setFileName(attachmentName);
    multipart.addBodyPart(attachmentPart);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>Nasıl kullanılır: Altlıkları, izlemeyi ve analizleri etkinleştirmek için filtreleri kullanın
SendGrid *filtreleri*kullanarak ek e-posta işlevselliği sağlar. Bunlar, tıklama izlemeyi etkinleştirme, Google analitiği, abonelik izleme gibi belirli işlevleri etkinleştirmek için bir e-posta iletisine eklenebilecek ayarlardır. Filtrelerin tam listesi için [Filtre Ayarları'na][Filter Settings]bakın.

* Aşağıda, gönderilen e-postanın alt kısmında görünen HTML metniyle sonuçlanan bir altbilgi filtresi nasıl ekilir gösterilmektedir.

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"footer\":
          {\"settings\":
          {\"enable\":1,\"text/html\":
          \"<html><b>Thank you</b> for your business.</html>\"}}}}");
* Filtrenin başka bir örneği de tıklama izlemedir. E-posta metninizin aşağıdaki gibi bir köprü içerdiğini ve tıklama oranını izlemek istediğinizi varsayalım:

      messagePart.setContent(
          "Hello,
          <p>This is the body of the message. Visit
          <a href='http://www.contoso.com'>http://www.contoso.com</a>.</p>
          Thank you.",
          "text/html");
* Tıklama izlemeyi etkinleştirmek için aşağıdaki kodu kullanın:

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"clicktrack\":
          {\"settings\":
          {\"enable\":1}}}}");

## <a name="how-to-update-email-properties"></a>Nasıl yapılır: E-posta özelliklerini güncelleştirme
Bazı e-posta **özellikleri, Özellik kümesi** kullanılarak üzerine yazılabilir veya **Özellik ekle**kullanılarak eklenebilir.

Örneğin, **Yanıtla** adreslerini belirtmek için aşağıdakileri kullanın:

    InternetAddress addresses[] =
        { new InternetAddress("john@contoso.com"),
          new InternetAddress("wendy@contoso.com") };

    message.setReplyTo(addresses);

**Cc** alıcısı eklemek için aşağıdakileri kullanın:

    message.addRecipient(Message.RecipientType.CC, new
    InternetAddress("john@contoso.com"));

## <a name="how-to-use-additional-sendgrid-services"></a>Nasıl kullanılır: Ek SendGrid hizmetlerini kullanma
SendGrid, Azure uygulamanızdan ek SendGrid işlevselliği nden yararlanmak için kullanabileceğiniz web tabanlı API'ler sunar. Tüm ayrıntılar için [SendGrid API belgelerine][SendGrid API documentation]bakın.

## <a name="next-steps"></a>Sonraki adımlar
SendGrid E-posta hizmetinin temellerini öğrendiğiniz için, daha fazla bilgi edinmek için bu bağlantıları takip edin.

* Bir Azure dağıtımında SendGrid'i kullandığını gösteren örnek: [Azure dağıtımında Java'dan SendGrid kullanarak e-posta gönderme](store-sendgrid-java-how-to-send-email-example.md)
* SendGrid Java SDK:<https://sendgrid.com/docs/Code_Examples/java.html>
* SendGrid API belgeleri:<https://sendgrid.com/docs/API_Reference/index.html>
* Azure müşterileri için SendGrid özel teklifi:<https://sendgrid.com/windowsazure.html>

[https://sendgrid.com]: https://sendgrid.com
[https://sendgrid.com/pricing.html]: https://sendgrid.com/pricing.html
[http://www.sendgrid.com/azure.html]: https://www.sendgrid.com/windowsazure.html
[https://sendgrid.com/features]: https://sendgrid.com/features
[https://www.oracle.com/technetwork/java/javamail]: https://www.oracle.com/technetwork/java/javamail/index.html
[Filter Settings]: https://sendgrid.com/docs/API_Reference/Web_API/filter_settings.html
[SendGrid API documentation]: https://sendgrid.com/docs/API_Reference/index.html
[https://sendgrid.com/azure.html]: https://sendgrid.com/windowsazure.html
[bulut tabanlı e-posta hizmeti]: https://sendgrid.com/email-solutions
[işlemsel e-posta teslimi]: https://sendgrid.com/transactional-email
