---
title: TLS karşılıklı kimlik doğrulamayı yapılandırma
description: TLS üzerinde istemci sertifikalarının nasıl doğrulandığını öğrenin. Azure App Service, istemci sertifikasını doğrulama için uygulama kodu için kullanılabilir hale getirir.
ms.assetid: cd1d15d3-2d9e-4502-9f11-a306dac4453a
ms.topic: article
ms.date: 10/01/2019
ms.custom: seodec18
ms.openlocfilehash: 44bf98ea0bd82bd261fa565fa068382ecc2a0ba7
ms.sourcegitcommit: a100e3d8b0697768e15cbec11242e3f4b0e156d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681149"
---
# <a name="configure-tls-mutual-authentication-for-azure-app-service"></a>Azure App Service için TLS karşılıklı kimlik doğrulamasını yapılandırma

Azure App Service uygulamanıza erişimi, farklı kimlik doğrulama türlerini etkinleştirerek kısıtlayabilirsiniz. Bunu yapmanın bir yolu, istemci isteği TLS/SSL üzerinden olduğunda ve sertifikayı doğrulayacağından istemci sertifikası isteğidir. Bu mekanizmaya TLS karşılıklı kimlik doğrulaması veya istemci sertifikası kimlik doğrulaması adı verilir. Bu makalede, uygulamanızın istemci sertifikası kimlik doğrulamasını kullanmak üzere nasıl ayarlanacağı gösterilmektedir.

> [!NOTE]
> Sitenize HTTPS değil, HTTP üzerinden eriştiğinizde hiçbir istemci sertifikası almazsınız. Bu nedenle, uygulamanız için istemci sertifikaları gerekiyorsa, HTTP üzerinden uygulamanıza yönelik isteklere izin vermeniz gerekir.
>

## <a name="enable-client-certificates"></a>İstemci sertifikalarını etkinleştir

Uygulamanızı istemci sertifikaları gerektirecek şekilde ayarlamak için uygulamanızın `clientCertEnabled` ayarını `true`olarak ayarlamanız gerekir. Ayarı ayarlamak için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın.

```azurecli-interactive
az webapp update --set clientCertEnabled=true --name <app_name> --resource-group <group_name>
```

## <a name="exclude-paths-from-requiring-authentication"></a>Kimlik doğrulama gerektirmesinden dışlanan yollar

Uygulamanız için karşılıklı kimlik doğrulamasını etkinleştirdiğinizde, uygulamanızın kökündeki tüm yollar erişim için bir istemci sertifikası gerektirir. Bazı yolların anonim erişim için açık kalmasını sağlamak için, uygulama yapılandırmanızın bir parçası olarak dışlama yolları tanımlayabilirsiniz.

Dışlama yolları **yapılandırma** > **Genel ayarlar** seçilerek ve bir dışlama yolu tanımlayarak yapılandırılabilir. Bu örnekte, uygulamanız için `/public` yolu altındaki her şey bir istemci sertifikası istemeyebilir.

![Sertifika dışlama yolları][exclusion-paths]


## <a name="access-client-certificate"></a>İstemci sertifikasına erişin

App Service, isteğin SSL sonlandırması ön uç yük dengeleyicisinde gerçekleşir. [İstemci sertifikaları etkinken](#enable-client-certificates)isteği uygulama kodunuza iletirken, istemci sertifikasıyla bir `X-ARR-ClientCert` isteği üst bilgisini App Service çıkartır. App Service, bu istemci sertifikası ile uygulamanıza iletilmesinin dışında hiçbir şey yapmaz. Uygulama kodunuz, istemci sertifikasını doğrulamaktan sorumludur.

ASP.NET için, istemci sertifikasına **HttpRequest. ClientCertificate** özelliği aracılığıyla erişilebilir.

Diğer uygulama yığınları (node. js, PHP, vb.) için, istemci sertifikası `X-ARR-ClientCert` istek üst bilgisinde Base64 kodlamalı bir değer aracılığıyla uygulamanızda kullanılabilir.

## <a name="aspnet-sample"></a>ASP.NET örneği

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

## <a name="nodejs-sample"></a>Node. js örneği

Aşağıdaki Node. js örnek kodu `X-ARR-ClientCert` üstbilgisini alır ve Base64 kodlamalı PEM dizesini bir sertifika nesnesine dönüştürmek ve doğrulamak için [node-Forge](https://github.com/digitalbazaar/forge) kullanır:

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

Aşağıdaki Java sınıfı `X-ARR-ClientCert` sertifikayı bir `X509Certificate` örneğine kodluyor. `certificateIsValid()`, sertifikanın parmak izinin oluşturucuda verilen ile eşleştiğini ve sertifikanın süresi dolmadığından emin olarak doğrular.


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
