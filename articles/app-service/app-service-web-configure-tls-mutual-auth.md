---
title: TLS karşılıklı kimlik doğrulamayı yapılandırma
description: TLS'de istemci sertifikalarının nasıl doğrulanmış olduğunu öğrenin. Azure Uygulama Hizmeti, istemci sertifikasını doğrulama için uygulama kodunda kullanılabilir hale getirebilir.
ms.assetid: cd1d15d3-2d9e-4502-9f11-a306dac4453a
ms.topic: article
ms.date: 10/01/2019
ms.custom: seodec18
ms.openlocfilehash: 357ea2cc598bca3e008a74f021895e1e45a3874f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78301021"
---
# <a name="configure-tls-mutual-authentication-for-azure-app-service"></a>Azure Uygulama Hizmeti için TLS karşılıklı kimlik doğrulamasını yapılandırma

Azure Uygulama Hizmeti uygulamanıza erişimi, farklı türde kimlik doğrulamalarını etkinleştirerek kısıtlayabilirsiniz. Bunu yapmanın bir yolu, istemci isteği TLS/SSL'nin üzerinde yken istemci sertifikası istemek ve sertifikayı doğrulamaktır. Bu mekanizmaya TLS karşılıklı kimlik doğrulama veya istemci sertifikası kimlik doğrulaması denir. Bu makalede, istemci sertifikası kimlik doğrulamasını kullanmak üzere uygulamanızın nasıl ayarlır?

> [!NOTE]
> Sitenize HTTPS üzerinden değil DE HTTP üzerinden erişirseniz, herhangi bir istemci sertifikası almazsınız. Bu nedenle, başvurunuz istemci sertifikaları gerektiriyorsa, http üzerinden başvurunuziçin isteklere izin vermemelisiniz.
>

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

## <a name="enable-client-certificates"></a>İstemci sertifikalarını etkinleştirme

Uygulamanızı istemci sertifikaları gerektirecek şekilde ayarlamak için uygulamanızın ayarını `clientCertEnabled` `true`' da ayarlamanız gerekir. Ayarı ayarlamak için [Bulut Kabuğu'nda](https://shell.azure.com)aşağıdaki komutu çalıştırın.

```azurecli-interactive
az webapp update --set clientCertEnabled=true --name <app_name> --resource-group <group_name>
```

## <a name="exclude-paths-from-requiring-authentication"></a>Kimlik doğrulama gerektiren yolları hariç tutma

Uygulamanız için karşılıklı auth etkinleştirdiğinizde, uygulamanızın kökü altında tüm yollar erişim için bir istemci sertifikası gerektirir. Belirli yolların anonim erişim için açık kalmasına izin vermek için, dışlama yollarını uygulama yapılandırmanızın bir parçası olarak tanımlayabilirsiniz.

Dışlama yolları **Yapılandırma** > **Genel Ayarları** seçilerek ve bir dışlama yolu tanımlayarak yapılandırılabilir. Bu örnekte, `/public` uygulamanız için yol altındaki herhangi bir şey istemci sertifikası istenmez.

![Sertifika Dışlama Yolları][exclusion-paths]


## <a name="access-client-certificate"></a>Müşteri sertifikasına erişin

App Service'de, isteğin SSL sonlandırılması ön uç yük dengeleyicisinde gerçekleşir. [İstemci sertifikaları etkinken](#enable-client-certificates)isteği uygulama kodunuza iletilirken, Uygulama Hizmeti istemci sertifikasıile birlikte bir `X-ARR-ClientCert` istek üstbilgisini ekler. Uygulama Hizmeti, bu istemci sertifikasıyla uygulamanıza iletmekten başka bir şey yapmaz. Uygulama kodunuz istemci sertifikasını doğrulamadan sorumludur.

ASP.NET için istemci sertifikası **httprequest.clientcertificate** özelliği aracılığıyla kullanılabilir.

Diğer uygulama yığınları (Node.js, PHP, vb.) için istemci sertifikası, `X-ARR-ClientCert` uygulamanızda istek üstbilgisinde base64 kodlanmış bir değer aracılığıyla kullanılabilir.

## <a name="aspnet-sample"></a>ASP.NET örnek

```csharp
    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class Cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;

                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;

                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                return true;
            }
        }
    }
```

## <a name="nodejs-sample"></a>Düğüm.js örneği

Aşağıdaki Node.js örnek kodu `X-ARR-ClientCert` üstbilgi alır ve base64 kodlanmış PEM dizesini bir sertifika nesnesine dönüştürmek ve doğrulamak için [düğüm-forge](https://github.com/digitalbazaar/forge) kullanır:

```javascript
import { NextFunction, Request, Response } from 'express';
import { pki, md, asn1 } from 'node-forge';

export class AuthorizationHandler {
    public static authorizeClientCertificate(req: Request, res: Response, next: NextFunction): void {
        try {
            // Get header
            const header = req.get('X-ARR-ClientCert');
            if (!header) throw new Error('UNAUTHORIZED');

            // Convert from PEM to pki.CERT
            const pem = `-----BEGIN CERTIFICATE-----${header}-----END CERTIFICATE-----`;
            const incomingCert: pki.Certificate = pki.certificateFromPem(pem);

            // Validate certificate thumbprint
            const fingerPrint = md.sha1.create().update(asn1.toDer(pki.certificateToAsn1(incomingCert)).getBytes()).digest().toHex();
            if (fingerPrint.toLowerCase() !== 'abcdef1234567890abcdef1234567890abcdef12') throw new Error('UNAUTHORIZED');

            // Validate time validity
            const currentDate = new Date();
            if (currentDate < incomingCert.validity.notBefore || currentDate > incomingCert.validity.notAfter) throw new Error('UNAUTHORIZED');

            // Validate issuer
            if (incomingCert.issuer.hash.toLowerCase() !== 'abcdef1234567890abcdef1234567890abcdef12') throw new Error('UNAUTHORIZED');

            // Validate subject
            if (incomingCert.subject.hash.toLowerCase() !== 'abcdef1234567890abcdef1234567890abcdef12') throw new Error('UNAUTHORIZED');

            next();
        } catch (e) {
            if (e instanceof Error && e.message === 'UNAUTHORIZED') {
                res.status(401).send();
            } else {
                next(e);
            }
        }
    }
}
```

## <a name="java-sample"></a>Java örneği

Aşağıdaki Java sınıfı sertifikayı `X-ARR-ClientCert` bir `X509Certificate` örnekten kodlar. `certificateIsValid()`sertifikanın parmak izinin oluşturucuda verilenle eşleştiğini ve sertifikanın süresinin dolmadığını doğrular.


```java
import java.io.ByteArrayInputStream;
import java.security.NoSuchAlgorithmException;
import java.security.cert.*;
import java.security.MessageDigest;

import sun.security.provider.X509Factory;

import javax.xml.bind.DatatypeConverter;
import java.util.Base64;
import java.util.Date;

public class ClientCertValidator { 

    private String thumbprint;
    private X509Certificate certificate;

    /**
     * Constructor.
     * @param certificate The certificate from the "X-ARR-ClientCert" HTTP header
     * @param thumbprint The thumbprint to check against
     * @throws CertificateException If the certificate factory cannot be created.
     */
    public ClientCertValidator(String certificate, String thumbprint) throws CertificateException {
        certificate = certificate
                .replaceAll(X509Factory.BEGIN_CERT, "")
                .replaceAll(X509Factory.END_CERT, "");
        CertificateFactory cf = CertificateFactory.getInstance("X.509");
        byte [] base64Bytes = Base64.getDecoder().decode(certificate);
        X509Certificate X509cert =  (X509Certificate) cf.generateCertificate(new ByteArrayInputStream(base64Bytes));

        this.setCertificate(X509cert);
        this.setThumbprint(thumbprint);
    }

    /**
     * Check that the certificate's thumbprint matches the one given in the constructor, and that the
     * certificate has not expired.
     * @return True if the certificate's thumbprint matches and has not expired. False otherwise.
     */
    public boolean certificateIsValid() throws NoSuchAlgorithmException, CertificateEncodingException {
        return certificateHasNotExpired() && thumbprintIsValid();
    }

    /**
     * Check certificate's timestamp.
     * @return Returns true if the certificate has not expired. Returns false if it has expired.
     */
    private boolean certificateHasNotExpired() {
        Date currentTime = new java.util.Date();
        try {
            this.getCertificate().checkValidity(currentTime);
        } catch (CertificateExpiredException | CertificateNotYetValidException e) {
            return false;
        }
        return true;
    }

    /**
     * Check the certificate's thumbprint matches the given one.
     * @return Returns true if the thumbprints match. False otherwise.
     */
    private boolean thumbprintIsValid() throws NoSuchAlgorithmException, CertificateEncodingException {
        MessageDigest md = MessageDigest.getInstance("SHA-1");
        byte[] der = this.getCertificate().getEncoded();
        md.update(der);
        byte[] digest = md.digest();
        String digestHex = DatatypeConverter.printHexBinary(digest);
        return digestHex.toLowerCase().equals(this.getThumbprint().toLowerCase());
    }

    // Getters and setters

    public void setThumbprint(String thumbprint) {
        this.thumbprint = thumbprint;
    }

    public String getThumbprint() {
        return this.thumbprint;
    }

    public X509Certificate getCertificate() {
        return certificate;
    }

    public void setCertificate(X509Certificate certificate) {
        this.certificate = certificate;
    }
}
```

[exclusion-paths]: ./media/app-service-web-configure-tls-mutual-auth/exclusion-paths.png
