---
title: Sanal makineyi önceden doğrulamak için istemciyi kendi kendine test etme | Azure Marketi
description: Azure Marketi için sanal bir makine görüntüsünü önceden doğrulamak için kendi kendini test eden bir istemci oluşturma.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 01/23/2018
ms.author: dsindona
ms.openlocfilehash: fb568400cb60f108303909353bfa703e98ab6157
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80286430"
---
# <a name="create-a-self-test-client-to-pre-validate-an-azure-virtual-machine-image"></a>Azure sanal makine görüntüsünü önceden doğrulamak için kendi kendine test eden bir istemci oluşturma

Kendi kendine test API tüketen bir istemci hizmeti oluşturmak için bir kılavuz olarak bu makaleyi kullanın. Sanal makineyi (VM) en son Azure Marketi yayımlama gereksinimlerini karşıladığından emin olmak için kendi kendini test eden API'yi önceden doğrulamak için kullanabilirsiniz. Bu istemci hizmeti, Microsoft sertifikası için teklifinizi göndermeden önce bir VM'yi test etmenizi sağlar.

## <a name="development-and-testing-overview"></a>Geliştirme ve teste genel bakış

Kendi kendine test işleminin bir parçası olarak, Azure Aboneliğinizde çalışan bir VM'yi doğrulamak için Azure Marketi'ne bağlanan yerel bir istemci oluşturursunuz. VM, Windows veya Linux işletim sistemini çalıştırıyor olabilir.

Yerel istemci, kendi kendini sınama API ile kimlik doğrulayan, bağlantı bilgilerini gönderen ve test sonuçlarını alan bir komut dosyası çalıştırır.

Kendi kendini sınayen bir istemci oluşturmak için üst düzey adımlar şunlardır:

1. Uygulamanız için Azure Etkin Dizin (AD) kiracısını seçin.
2. İstemci uygulamasını kaydedin.
3. İstemci Azure AD uygulaması için bir belirteç oluşturun.
4. Belirteci kendi kendini test eden API'ye geçirin.

İstemciyi oluşturduktan sonra VM'inizle karşısına test edebilirsiniz.

### <a name="self-test-client-authorization"></a>Kendi kendine test istemci yetkilendirme

Aşağıdaki diyagram, istemci kimlik bilgilerini (paylaşılan gizli veya sertifika) kullanarak hizmet çağrılarına hizmet için yetkilendirmenin nasıl çalıştığını gösterir.

![İstemci yetkilendirme süreci](./media/stclient-dev-process.png)

## <a name="the-self-test-client-api"></a>Kendi kendine test istemci API

Kendi kendine sınayın API yalnızca POST yöntemini destekleyen tek bir bitiş noktası içerir.  Aşağıdaki yapıya sahiptir.

```
Uri:             https://isvapp.azurewebsites.net/selftest-vm
Method:          Post
Request Header:  Content-Type: "application/json"
Authorization:   "Bearer xxxx-xxxx-xxxx-xxxxx"
Request body:    The Request body parameters should use the following JSON format:
                 {
                   "DNSName":"XXXX.westus.cloudapp.azure.com",
                   "User":"XXX",
                   "Password":"XXX@1234567",
                   "OS":"XXX",
                   "PortNo":"22",
                   "CompanyName":"ABCD",
                 }
```

Aşağıdaki tabloda API alanları açıklanmaktadır.


|      Alan         |    Açıklama    |
|  ---------------   |  ---------------  |
|  Yetkilendirme     |  "Bearer xxxx-xxxx-xxxx-xxxxx" dizesi, PowerShell kullanılarak oluşturulabilen Azure Active Directory (AD) istemci belirteci içerir.          |
|  DNSName           |  Test etmek için VM'nin DNS Adı    |
|  Kullanıcı              |  VM'de oturum açma için kullanıcı adı         |
|  Parola          |  VM'de oturum açma parolası          |
|  İşletim Sistemi                |  VM işletim sistemi: ya `Linux` da`Windows`          |
|  PortNo            |  VM'ye bağlanmak için bağlantı noktası numarasını açın. Bağlantı noktası numarası `22` genellikle Linux `5986` ve Windows içindir.          |
|  |  |

## <a name="consuming-the-api"></a>API'yi tüketme

PowerShell veya cURL kullanarak kendi kendine test API'yi tüketebilirsiniz.

### <a name="use-powershell-to-consume-the-api-on-the-linux-os"></a>Linux işletim sistemi üzerindeki API'yi kullanmak için PowerShell'i kullanın

PowerShell'deki API'yi aramak için aşağıdaki adımları izleyin:

1. API'yi `Invoke-WebRequest` aramak için komutu kullanın.
2. Yöntem Post ve içerik türü JSON, aşağıdaki kod örneği ve ekran yakalama gösterildiği gibi.
3. Gövde parametrelerini JSON biçiminde belirtin.

Aşağıdaki kod örneği, API'ye bir PowerShell çağrısı gösterir.

```powershell
$accesstoken = "Get token for your Client AAD App"
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Authorization", "Bearer $accesstoken")
$Body = @{
    "DNSName" = "XXXX.westus.cloudapp.azure.com"
    "User" = "XXX"
    "Password" = "XXX@123456"
    "OS" = "Linux"
    "PortNo" = "22"
    "CompanyName" = "ABCD"

} | ConvertTo-Json
$res = Invoke-WebRequest -Method "Post" -Uri $uri -Body $Body -ContentType "application/json" -Headers $headers;
$Content = $res | ConvertFrom-Json
```
Aşağıdaki ekran yakalama PowerShell API arama için bir örnek gösterir.

![Linux OS için PowerShell ile API'yi arayın](./media/stclient-call-api-ps-linuxvm.png)

Önceki örneği kullanarak, JSON'ı alabilir ve aşağıdaki ayrıntıları almak için ayrıştın:

```powershell
$testresult = ConvertFrom-Json -InputObject (ConvertFrom-Json -InputObject $res)

  Write-Host "OSName: $($testresult.OSName)"
  Write-Host "OSVersion: $($testresult.OSVersion)"
  Write-Host "Overall Test Result: $($testresult.TestResult)"

For ($i=0; $i -lt $testresult.Tests.Length; $i++)
{
    Write-Host "TestID: $($testresult.Tests[$i].TestID)"
    Write-Host "TestCaseName: $($testresult.Tests[$i].TestCaseName)"
    Write-Host "Description: $($testresult.Tests[$i].Description)"
    Write-Host "Result: $($testresult.Tests[$i].Result)"
    Write-Host "ActualValue: $($testresult.Tests[$i].ActualValue)"
}
```

Aşağıdaki ekran yakalama, `$res.Content`hangi gösterir , JSON formatında test sonuçlarının ayrıntılarını verir.

![Linux PowerShell arama JSON sonuçları](./media/stclient-pslinux-rescontent-json.png)

Aşağıdaki ekran yakalama, çevrimiçi JSON görüntüleyicisinde görüntülenen JSON test sonuçlarının bir örneğini gösterir (örneğin, [Kod Güzelleştirme](https://codebeautify.org/jsonviewer) veya [JSON Görüntüleyici).](https://jsonformatter.org/json-viewer)

![Linux VM PowerShell arama JSON sonuçları](./media/stclient-consume-api-pslinux-json.png)

### <a name="use-powershell-to-consume-the-api-on-the-windows-os"></a>Windows işletim sistemi üzerindeki API'yi kullanmak için PowerShell'i kullanın

PowerShell'deki API'yi aramak için aşağıdaki adımları izleyin:

1. API'yi `Invoke-WebRequest` aramak için komutu kullanın.
2. Yöntem Post ve içerik türü JSON, aşağıdaki kod örneği ve ekran yakalama gösterildiği gibi.
3. JSON formatında Gövde parametrelerini oluşturun.

Aşağıdaki kod örneği, API'ye bir PowerShell çağrısı gösterir.

```powershell
$accesstoken = "Get token for your Client AAD App"
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Authorization", "Bearer $accesstoken")
$Body = @{
    "DNSName" = "XXXX.westus.cloudapp.azure.com"
    "User" = "XXX"
    "Password" = "XXX@123456"
    "OS" = "Windows"
    "PortNo" = "5986"
    "CompanyName" = "ABCD"

} | ConvertTo-Json
$res = Invoke-WebRequest -Method "Post" -Uri $uri -Body $Body -ContentType "application/json" -Headers $headers;
$Content = $res | ConvertFrom-Json
```

Aşağıdaki ekran yakalama PowerShell API arama için bir örnek gösterir.

![Windows VM için PowerShell ile API'yi arayın](./media/stclient-call-api-ps-windowsvm.png)

Önceki örneği kullanarak, JSON'ı alabilir ve aşağıdaki ayrıntıları almak için ayrıştın:

```powershell
$testresult = ConvertFrom-Json -InputObject (ConvertFrom-Json -InputObject $res)

  Write-Host "OSName: $($testresult.OSName)"
  Write-Host "OSVersion: $($testresult.OSVersion)"
  Write-Host "Overall Test Result: $($testresult.TestResult)"

For ($i=0; $i -lt $testresult.Tests.Length; $i++)
{
    Write-Host "TestID: $($testresult.Tests[$i].TestID)"
    Write-Host "TestCaseName: $($testresult.Tests[$i].TestCaseName)"
    Write-Host "Description: $($testresult.Tests[$i].Description)"
    Write-Host "Result: $($testresult.Tests[$i].Result)"
    Write-Host "ActualValue: $($testresult.Tests[$i].ActualValue)"
}
```

Aşağıdaki ekran yakalama, `$res.Content`hangi gösterir , JSON formatında test sonuçlarının ayrıntılarını verir.

![Windows'a PowerShell aramaJSON sonuçları](./media/stclient-pswindows-rescontent-json.png)

Aşağıdaki ekran yakalama, çevrimiçi bir JSON görüntüleyicisinde görüntülenen test sonuçlarını gösterir.
(örneğin, [Kod Güzelleştirmek](https://codebeautify.org/jsonviewer), [JSON Görüntüleyici](https://jsonformatter.org/json-viewer))

![Windows VM PowerShell arama JSON sonuçları](./media/stclient-consume-api-pswindows-json.png)

### <a name="use-curl-to-consume-the-api-on-the-linux-os"></a>Linux OS'deki API'yi kullanmak için cURL'yi kullanın

API'yi cURL ile aramak için aşağıdaki adımları izleyin:

1. API'yi aramak için kıvırma komutunu kullanın.
2. Yöntem Post ve içerik türü JSON, aşağıdaki kod snippet gösterildiği gibi.

```
CURL POST -H "Content-Type:application/json"
-H "Authorization: Bearer XXXXXX-Token-XXXXXXXX"
https://isvapp.azurewebsites.net/selftest-vm
-d '{ "DNSName":"XXXX.westus.cloudapp.azure.com", "User":"XXX", "Password":"XXXX@123456", "OS":"Linux", "PortNo":"22", "CompanyName":"ABCD"}'
```

Aşağıdaki ekranda API aramak için kıvırma kullanarak bir örnek gösterilmektedir.

![Kıvırma komutunu kullanarak API'yi arayın](./media/stclient-consume-api-curl.png)

Aşağıdaki ekran yakalama kıvırma çağrısından JSON sonuçlarını gösterir.

![Kıvırma çağrısından JSON sonuçları](./media/stclient-consume-api-curl-json.png)


## <a name="choose-the-azure-ad-tenant-for-the-app"></a>Uygulama için Azure AD kiracısını seçin

Uygulamanızı oluşturmak istediğiniz Azure AD kiracısını seçmek için aşağıdaki adımları kullanın.

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Üst menü çubuğunda hesabınızı seçin ve Dizin listesinin altında, başvurunuzu kaydetmek istediğiniz Aktif Dizin kiracısını seçin. Veya Global abonelik filtresini görmek için **Dizin + Abonelik** simgesini seçin. Aşağıdaki ekran yakalama bu filtrenin bir örneğini gösterir.

   ![Abonelik filtresini seçin](./media/stclient-subscription-filter.png)

3. Sol daki gezinme çubuğunda **Tüm hizmetler'i** seçin ve ardından **Azure Etkin Dizini'ni**seçin.

   Aşağıdaki adımlarda, kiracı adı (veya dizin adı) veya kiracı kimliği (veya dizin kimliği) gerekebilir.

   **Kiracı bilgilerini almak için:**

   **Azure Etkin Dizine Genel Bakış'ta**"Özellikler"i arayın ve ardından **Özellikler'i**seçin. Örnek olarak aşağıdaki ekran yakalamayı kullanma:

   - **Ad** - Kiracı adı veya dizin adı
   - **Dizin Kimliği** - Kiracı kimliği veya dizin kimliği veya Özellikleri bulmak için kaydırma çubuğunu kullanın.

   ![Azure Etkin Dizin özellikleri sayfası](./media/stclient-aad-properties.png)

## <a name="register-the-client-app"></a>İstemci uygulamasını kaydetme

İstemci uygulamasını kaydetmek için aşağıdaki adımları kullanın.

1. Sol daki gezinti çubuğunda **Tüm hizmetleri** seçin ve ardından **Uygulama kayıtlarını**seçin.
2. **Uygulama kayıtları**altında , seçin + Yeni **uygulama kaydı**.
3. **Create**altında, aşağıdaki alanlar için gerekli bilgileri sağlayın:

   - **Ad** - Uygulama için dostça bir ad girin. Örneğin, "SelfTestClient".
   - **Uygulama türü** - **Web App/API'yi** seçin
   - **Oturum açma URL'si** \/- "https: /isvapp.azurewebsites.net/selftest-vm" yazın

4. **Oluştur'u**seçin.
5. **Uygulama kayıtları** veya **Kayıtlı uygulama**altında, Uygulama **Kimliği**kopyalayın.

   ![Uygulama kimliğini alma](./media/stclient-app-id.png)

6. Kayıtlı uygulama araç çubuğunda **Ayarlar'ı**seçin.
7. Uygulamanız için izinleri yapılandırmak için **Gerekli İzinleri** seçin.
8. **Gerekli izinler**altında , seçin **+ Ekle**.
9. **API ekle erişimi**altında, **bir API seçin.**
10. **API'yi seçin'in**altında, API'yi aramak için "Windows Azure klasik dağıtım modeli" yazın.
11. Arama sonuçlarında, **Windows Azure klasik dağıtım modelini** seçin ve sonra **Seç'i**tıklatın.

    ![Uygulama için çok kiracılı yapılandırma](./media/stclient-select-api.png)

12. **API erişimi ekle**altında, **İzinseç'i**seçin.
13. **Access "Windows Azure Hizmet Yönetimi API"yi**seçin.

    ![Uygulama için API erişimini etkinleştirme](./media/stclient-enable-api-access.png)

14. **Seç'i**tıklatın.
15. **Done** (Bitti) öğesini seçin.
16. **Ayarlar** bölümünde **Özellikler**’i seçin.
17. **Özellikleri**altında, **Çok kiracılı**aşağı kaydırın. **Evet'i**seçin.

    ![Uygulama için çok kiracılı yapılandırma](./media/stclient-yes-multitenant.png)

18. **Kaydet'i**seçin.
19. **Ayarlar** **altında, Tuşlar'ı**seçin.
20. Anahtar **TANIMI** metin kutusunu seçerek gizli bir anahtar oluşturun. Aşağıdaki alanları yapılandırın:

    - Anahtar adı yazın. Örneğin, "selftestclient"
    - SONA **ERME** açılır listesinde "1 yıl içinde" seçeneğini belirleyin.
    - Anahtarı oluşturmak için **Kaydet'i** seçin.
    - **VALUE**altında, anahtarı kopyalayın.

      >[!Important]
      >**Anahtarlar** formundan çıktıktan sonra anahtar değerini göremezsin.

    ![Anahtar değer formu](./media/stclient-create-key.png)

## <a name="create-the-token-for-the-client-app"></a>İstemci uygulaması için belirteç oluşturma

OAuth REST API'sini kullanarak bir belirteç oluşturmak ve almak için aşağıdaki programlardan herhangi birini kullanabilirsiniz:

- Postman
- Linux'ta cURL
- C&#35;
- PowerShell

### <a name="to-create-and-get-a-token-using-postman"></a>Postacı kullanarak bir belirteç oluşturmak ve almak için

 Auth0'dan yetkili uygulamalarınızdan herhangi biri için belirteçler istemek [https://login.microsoftonline.com/common/oauth2/token](https://login.microsoftonline.com/common/oauth2/token) için, aşağıdaki biçimde bir yük içeren bitiş noktasına kadar bir POST işlemi gerçekleştirin:

```
Method Type : POST
Base Url: https://login.microsoftonline.com/common/oauth2/token
```

İstek gövdesinde aşağıdaki parametreleri geçirin:

```
Body Content-Type: x-www-form-urlencoded
client_id: XXX (Paste your Application ID of Web App/API Type client AD App)
grant_type: client_credentials
client_secret: XXX (Paste your Secret Key of Web App/API Type client AD App)
resource: https://management.core.windows.net
```

İstek üstbilgisinde aşağıdaki parametreleri geçirin:

```
Content-Type: application/x-www-form-urlencoded
```

Aşağıdaki ekran yakalama bir belirteç almak için Postacı kullanarak bir örnek gösterir.

![Postacı ile belirteç alın](./media/stclient-postman-get-token.png)

### <a name="to-create-and-get-a-token-using-curl-in-linux"></a>Linux'ta cURL kullanarak bir belirteç oluşturmak ve almak için

Auth0'dan yetkili uygulamalarınızdan herhangi biri için belirteçler istemek [https://login.microsoftonline.com/common/oauth2/token](https://login.microsoftonline.com/common/oauth2/token) için, aşağıdaki biçimde bir yük içeren bitiş noktasına kadar bir POST işlemi gerçekleştirin:

```
Request:

curl --request POST \
                --url 'https://login.microsoftonline.com/common/oauth2/token' \
                --header "Content-Type:application/x-www-form-urlencoded" \
                --data "grant_type=client_credentials&client_id=XXXXX-XXXX-XXXX-XXXXXXX&client_secret=XXXXXXXXX&resource=https://management.core.windows.net"

Response:

{"token":"UClCUUKxUlkdbhE1cHLz3kyjbIZYVh9eB34A5Q21Y3FPqKGSJs","expires":"2014-02-17 18:46:08"}
```

Aşağıdaki ekran yakalama, bir belirteç almak için kıvırma komutunu kullanma nın bir örneğini gösterir.

![Kıvırma komutu yla belirteç alın](./media/stclient-curl-get-token.png)

### <a name="to-create-and-get-a-token-using-c35"></a>C&#35; kullanarak bir belirteç oluşturmak ve almak için

Auth0'dan yetkili uygulamalarınızın herhangi biri için belirteçleri istemek için,\/aşağıdaki biçimde bir yük içeren https: /soamtenant.auth0.com/oauth/token uç noktasına bir POST işlemi gerçekleştirin:

```csharp
string clientId = "Your Application Id";
string clientSecret = "Your Application Secret";
string audience = "https://management.core.windows.net";
string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, "https://login.microsoftonline.com/common/oauth2/token");
string grantType = "client_credentials";
var client = new RestClient(authority);
var request = new RestRequest(Method.POST);
       request.AddHeader("content-type", "application/x-www-form- urlencoded");

string requestBody = "grant_type=" + grantType + "&" + "client_id=" +   clientId + "&" + "client_secret=" + clientSecret + "&" + "resource=" +  audience;

request.AddParameter("application/x-www-form-urlencoded", requestBody,  ParameterType.RequestBody);

IRestResponse response = client.Execute(request);
var content = response.Content;
var token = JObject.Parse(content)["access_token"];
```

### <a name="to-create-and-get-a-token-using-powershell"></a>PowerShell kullanarak bir belirteç oluşturmak ve almak için

Auth0'dan yetkili uygulamalarınızın herhangi biri için belirteçleri istemek için,\/aşağıdaki biçimde bir yük içeren https: /soamtenant.auth0.com/oauth/token uç noktasına bir POST işlemi gerçekleştirin:

```powershell
$clientId = "Application Id of AD Client APP";
$clientSecret = "Secret Key of AD Client APP "
$audience = "https://management.core.windows.net";
$authority = "https://login.microsoftonline.com/common/oauth2/token"
$grantType = "client_credentials";

$requestBody = "grant_type=" + $grantType + "&" + "client_id=" +  $clientId   + "&" + "client_secret=" + $clientSecret + "&" + "resource=" + $audience;

$headers = New-Object  "System.Collections.Generic.Dictionary[[String],[String]]"
             $headers.Add("ContentType", "application/x-www-form-urlencoded")
resp = Invoke-WebRequest -Method Post -Uri $authority -Headers $headers -ContentType 'application/x-www-form-urlencoded' -Body $requestBody

$token = $resp.Content | ConvertFrom-Json
$token.AccessToken
```

## <a name="pass-the-client-app-token-to-the-api"></a>İstemci uygulaması belirteci api'ye geçirin

Yetkilendirme üstbilgisinde aşağıdaki kodu kullanarak belirteci kendi kendini sınayARAK API'ye geçirin:

```powershell
$redirectUri = 'https://isvapp.azurewebsites.net/selftest-vm'
$accesstoken = 'place your token here'

$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Authorization", "Bearer $accesstoken")
$Body =
@{
      'DNSName'="XXXX.cloudapp.azure.com";
      'User'="XXX";
      'Password'="XXXX@12345";
      'OS'="Linux";
      'PortNo'="22"
  } | ConvertTo-Json

$result=Invoke-WebRequest -Method Post -Uri $redirectUri -Headers $headers -ContentType 'application/json' -Body $Body
$result
Write-Output 'Test Results:'
$result.Content
```

## <a name="test-your-self-test-client"></a>Kendi kendini test eden istemcinizi test edin

İstemciyi sınamak için aşağıdaki adımları izleyin:

1. Test etmek istediğiniz VM'yi dağıtın.
2. Yetkilendirme için istemci uygulama belirtecinizi kullanarak kendi kendini test eden API'yi arayın.
3. Test sonuçlarını JSON formatında alın.

### <a name="test-result-examples"></a>Test sonucu örnekleri

Aşağıdaki parçacıklar test sonuçlarını JSON formatında gösterir.

**Windows VM için test sonuçları:**

```json
{
  "SchemaVersion": 1,
  "AppCertificationCategory": "Microsoft Single VM Certification",
  "ProviderID": "050DE427-2A99-46D4-817C-5354D3BF2AE8",
  "OSName": "Microsoft Windows Server 2016 Datacenter",
  "OSVersion": "10.0.14393",
  "CreatedDate": "06/01/2018 07:48:04",
  "TestResult": "Pass",
  "APIVersion": "1.1",
  "Tests": [
    {
      "Tes tID": "1.1.4",
      "TestCaseName": "OS Architecture",
      "Description": "Azure supports 64bit Operating System Only.",
      "Result": "Passed",
      "ActualValue": "64 Bit",
      "Re quiredValue": "OS Should be 64 bit"
    },
    {
      "TestID": "1.1.5",
      "TestCaseName": "User account dependency",
      "Description": "Application execution should not have de pendency on administrator account.",
      "Result": "Passed",
      "ActualValue": "isv",
      "RequiredValue": "Logged in user should not be an administrator"
    },
    {
      "TestID": " 1.1.6",
      "TestCaseName": "Failover Cluster",
      "Description": "Windows Server Failover Clustering feature is not yet supported, Application should not have dependency on this feature.",
      "Result": "Passed",
      "ActualValue": "Disabled",
      "RequiredValue": "Failover Clustering feature is disabled"
    },
```

**Linux VM için test sonuçları:**

```json
{
  "SchemaVersion": 1,
  "AppCertificationCategory": "Microsoft Single VM Certification",
  "ProviderID": "050DE427-2A99-46D4-817C-5354D3BF2AE8",
  "OSName": "Ubuntu  16.04",
  "OSVersion": "16.04",
  "CreatedDate": "06/01/2018 07:04:24",
  "TestResult": "Pass",
  "APIVersion": "1.1",
  "Tests": [
    {
      "TestID": "48",
      "TestCaseName": "Bash Hi story",
      "Description": "Bash history files should be cleared before creating the VM image.",
      "Result": "Passed",
      "ActualValue": "No file Exist",
      "RequiredVal ue": "1024"
    },
    {
      "TestID": "39",
      "TestCaseName": "Linux Agent Version",
      "Description": "Azure Linux Agent 2.2.10 and above should be installed. ",
      "Result": "Pas sed",
      "ActualValue": "2.2.20",
      "RequiredValue": "2.2.10"
    },
    {
      "TestID": "40",
      "TestCaseName": "Required Kernel Parameters",
      "Description": "Verifies the following  kernel parameters are set console=ttyS0, earlyprintk=ttyS0, rootdelay=300",
      "Result": "Passed",
      "ActualValue": "Matched Parameter: console=ttyS0,earlypri ntk=ttyS0,rootdelay=300",
      "RequiredValue": "console=ttyS0#earlyprintk=ttyS0#rootdelay=300"
    },
```

## <a name="next-steps"></a>Sonraki adımlar

Azure sanal makinenizi başarıyla test ettikten sonra [teklifi yayımlayabilirsiniz.](./cpp-publish-offer.md)
