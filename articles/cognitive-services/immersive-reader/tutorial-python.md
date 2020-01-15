---
title: 'Öğretici: Python kullanarak modern okuyucu başlatma'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, tam ekran okuyucuyu Başlatan bir Python uygulaması oluşturacaksınız.
services: cognitive-services
author: dylankil
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 08/02/2019
ms.author: dylankil
ms.openlocfilehash: 3293c4ea76010e5f39c793a1faee14d9a74226a0
ms.sourcegitcommit: 49e14e0d19a18b75fd83de6c16ccee2594592355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75945222"
---
# <a name="tutorial-launch-the-immersive-reader-using-the-python-sample-project"></a>Öğretici: Python örnek projesini kullanarak modern okuyucu başlatma

[Genel bakışta](./overview.md), derinlikli okuyucu ne olduğunu ve dil öğrenimi, gelişmekte olan okuyucular ve öğrenme farklılığı olan öğrenciler için okuma kavraışını geliştirmek üzere kendini kanıtlamış tekniklerin nasıl uyguladığını öğrendiniz. Bu öğreticide, tam ekran okuyucuyu Başlatan bir Python web uygulamasının nasıl oluşturulacağı ele alınmaktadır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Örnek bir proje kullanarak PIP, Flask, Jınja ve virtualalenv ile bir Python web uygulaması oluşturma
> * Erişim belirteci alma
> * Örnek içerikle modern okuyucu başlatma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Azure Active Directory kimlik doğrulaması için yapılandırılmış bir tam ekran okuyucu kaynağı. Kurulumunu yapmak için [Bu yönergeleri](./how-to-create-immersive-reader.md) izleyin. Ortam özellikleri yapılandırılırken burada oluşturulan bazı değerler gerekir. Daha sonra başvurmak üzere oturumunuzun çıkışını bir metin dosyasına kaydedin.
* [Git](https://git-scm.com/)
* [Modern Okuyucu SDK 'Sı](https://github.com/microsoft/immersive-reader-sdk)
* [Python](https://www.python.org/downloads/) ve [PIP](https://docs.python.org/3/installing/index.html). Python 3,4 ' den başlayarak, PIP Python ikili yükleyicilerine göre varsayılan olarak dahil edilir.
* [Flask](https://flask.palletsprojects.com/en/1.0.x/)
* [Jınja](http://jinja.pocoo.org/docs/2.10/)
* [virtualalenv](https://virtualenv.pypa.io/en/latest/) ve [virtualalenvwrapper-](https://pypi.org/project/virtualenvwrapper-win/) [OSX için Windows veya virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/) için Win
* [istekler modülü](https://pypi.org/project/requests/2.7.0/)
* [Visual Studio Code](https://code.visualstudio.com/) gıbı bir IDE

## <a name="configure-authentication-credentials"></a>Kimlik doğrulama kimlik bilgilerini yapılandırma

_. Env_adlı yeni bir dosya oluşturun ve bu dosyaya aşağıdaki kodu yapıştırarak, derinlikli okuyucu kaynağını oluştururken verilen değerleri sağlayabilirsiniz.

```text
TENANT_ID={YOUR_TENANT_ID}
CLIENT_ID={YOUR_CLIENT_ID}
CLIENT_SECRET={YOUR_CLIENT_SECRET}
SUBDOMAIN={YOUR_SUBDOMAIN}
```

Ortak olmaması gereken gizli dizileri içerdiğinden, bu dosyayı kaynak denetimine yürütmemeyi unutmayın.

**Getimmersivereadertoken** API uç noktası, yetkisiz kullanıcıların, bir dizi kimlik doğrulaması (örneğin, [OAuth](https://oauth.net/2/)) arkasında güvenli hale gelmelidir Bu iş, Bu öğreticinin kapsamı dışındadır.

## <a name="create-a-python-web-app-on-windows"></a>Windows 'da Python web uygulaması oluşturma

Windows üzerinde `flask` kullanarak bir Python web uygulaması oluşturun.

[Git](https://git-scm.com/)'i yükleyin.

Git yüklendikten sonra bir komut Istemi açın ve tam ekran okuyucu SDK Git deposunu bilgisayarınızdaki bir klasöre ' kopyalayın '

```cmd
git clone https://github.com/microsoft/immersive-reader-sdk.git
```

[Python](https://www.python.org/downloads/)'ı yükleyin.

YOLA Python ekleme kutusunu işaretleyin.

![Python Windows yüklemesi Iletişim kutusu 1. adım](./media/pythoninstallone.jpg)

Kutuları işaretleyerek Isteğe bağlı özellikler ekleyin ve ardından ' Ileri ' düğmesine tıklayın.

![Python Windows yüklemesi Iletişim kutusu 2. adım](./media/pythoninstalltwo.jpg)

' Özel yükleme ' öğesini seçin ve yükleme yolunu kök klasörünüz olarak ayarlayın, örneğin, `C:\Python37-32\` ' yükleme ' düğmesine tıklayın.

![Python Windows yüklemesi Iletişim kutusu 3. adım](./media/pythoninstallthree.jpg)

Python yüklemesi tamamlandıktan sonra, bir komut Istemi açın ve Python betikleri klasörüne `cd`.

```cmd
cd C:\Python37-32\Scripts
```

Flask 'yi yükler.

```cmd
pip install flask
```

Jinja2 'i yükler. Python için tam özellikli bir şablon altyapısı.

```cmd
pip install jinja2
```

Virtualenv 'yi yükler. Yalıtılmış Python ortamları oluşturmaya yönelik bir araç.

```cmd
pip install virtualenv
```

Virtualalenvwrapper-WIN 'yi yükler. Virtualalenvwrapper arkasındaki fikir virtualalenv kullanımını kolaylaştırmaya yönelik.

```cmd
pip install virtualenvwrapper-win
```

İstekler modülünü yükler. İstekler, Python 'da yazılmış bir apache2 lisanslı HTTP kitaplığı.

```cmd
pip install requests
```

Python-dotenv modülünü yükler. Bu modül,. env dosyasından anahtar-değer çiftini okur ve bunları ortam değişkenine ekler.

```cmd
pip install python-dotenv
```

Sanal ortam oluşturma

```cmd
mkvirtualenv advanced-python
```

örnek proje kök klasörüne `cd`.

```cmd
cd C:\immersive-reader-sdk\js\samples\advanced-python
```

Örnek projeyi ortamla bağlayın. Bu, yeni oluşturulan sanal ortamı örnek proje kök klasörüne eşler.

```cmd
setprojectdir .
```

Sanal ortamı etkinleştirin.

```cmd
activate
```

Proje artık etkin olmalıdır ve komut Isteminde `(advanced-python) C:\immersive-reader-sdk\js\samples\advanced-python>` gibi bir şey görürsünüz.

Ortamı devre dışı bırakın.

```cmd
deactivate
```

Artık ortamın devre dışı bırakılmakta olduğu için `(advanced-python)` ön eki ilerlemelidir.

`workon advanced-python` ortamı yeniden etkinleştirmek için örnek proje kök klasöründen çalıştırın.

```cmd
workon advanced-python
```

### <a name="launch-the-immersive-reader-with-sample-content"></a>Örnek içerikle modern okuyucu başlatma

Ortam etkin olduğunda, örnek proje kök klasöründen `flask run` girerek örnek projeyi çalıştırın.

```cmd
flask run
```

Tarayıcınızı açın ve _http://localhost:5000_ gidin.

## <a name="create-a-python-web-app-on-osx"></a>OSX üzerinde bir Python web uygulaması oluşturma

OSX üzerinde `flask` kullanarak bir Python web uygulaması oluşturun.

[Git](https://git-scm.com/)'i yükleyin.

Git yüklendikten sonra, açık terminal ' i açın ve bilgisayarınızdaki bir klasöre ' kopyalayın ' ve modern Okuyucu SDK Git deposunu

```bash
git clone https://github.com/microsoft/immersive-reader-sdk.git
```

[Python](https://www.python.org/downloads/)'ı yükleyin.

Python kök klasörü gibi `Python37-32`, şimdi uygulamalar klasöründe olmalıdır.

Python yüklemesi tamamlandıktan sonra, Terminal ve `cd` Python betikleri klasörüne açın.

```bash
cd immersive-reader-sdk/js/samples/advanced-python
```

Pip yükleyin.

```bash
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
```

Ardından, izin sorunlarından kaçınmak için şu anda oturum açmış olan kullanıcı için PIP 'yi yüklemek üzere aşağıdakileri çalıştırın.

```bash
python get-pip.py --user
```

```bash
sudo nano /etc/paths
```

- İstendiğinde parolanızı girin.
- PIP yüklemenizin yolunu PATH değişkenine ekleyin.
- Dosyanın sonuna gidin ve listenin son öğesi olarak eklemek istediğiniz yolu girin, örneğin `PATH=$PATH:/usr/local/bin`.
- Çıkmak için Control-x ' i vurun.
- Değiştirilen arabelleği kaydetmek için `Y` girin.
- İşte bu kadar! Test etmek için yeni Terminal penceresinde şunu yazın: `echo $PATH`.

Flask 'yi yükler.

```bash
pip install flask --user
```

Jinja2 'i yükler. Python için tam özellikli bir şablon altyapısı.

```bash
pip install Jinja2 --user
```

Virtualenv 'yi yükler. Yalıtılmış Python ortamları oluşturmaya yönelik bir araç.

```bash
pip install virtualenv --user
```

Virtualalenvwrapper 'ı yükler. Virtualalenvwrapper arkasındaki fikir virtualalenv kullanımını kolaylaştırmaya yönelik.

```bash
pip install virtualenvwrapper --user
```

İstekler modülünü yükler. İstekler, Python 'da yazılmış bir apache2 lisanslı HTTP kitaplığı.

```bash
pip install requests --user
```

Python-dotenv modülünü yükler. Bu modül,. env dosyasından anahtar-değer çiftini okur ve bunları ortam değişkenine ekler.

```bash
pip install python-dotenv --user
```

Sanal ortamlarınızı tutmak istediğiniz klasörü seçin ve bu komutu çalıştırın

```bash
mkdir ~/.virtualenvs
```

Tam ekran okuyucu SDK 'Sı Python örnek uygulama klasörü `cd`.

```bash
cd immersive-reader-sdk/js/samples/advanced-python
```

Sanal ortam oluşturma

```bash
mkvirtualenv -p /usr/local/bin/python3 advanced-python
```

Örnek projeyi ortamla bağlayın. Bu, yeni oluşturulan sanal ortamı örnek proje kök klasörüne eşler.

```bash
setprojectdir .
```

Sanal ortamı etkinleştirin.

```bash
activate
```

Proje artık etkin olmalıdır ve komut Isteminde `(advanced-python) /immersive-reader-sdk/js/samples/advanced-python>` gibi bir şey görürsünüz.

Ortamı devre dışı bırakın.

```bash
deactivate
```

Artık ortamın devre dışı bırakılmakta olduğu için `(advanced-python)` ön eki ilerlemelidir.

`workon advanced-python` ortamı yeniden etkinleştirmek için örnek proje kök klasöründen çalıştırın.

```bash
workon advanced-python
```

## <a name="launch-the-immersive-reader-with-sample-content"></a>Örnek içerikle modern okuyucu başlatma

Ortam etkin olduğunda, örnek proje kök klasöründen `flask run` girerek örnek projeyi çalıştırın.

```bash
flask run
```

Tarayıcınızı açın ve _http://localhost:5000_ gidin.

## <a name="next-steps"></a>Sonraki adımlar

* [Modern Okuyucu SDK 'sını](https://github.com/microsoft/immersive-reader-sdk) ve [tam ekran okuyucu SDK başvurusunu](./reference.md) keşfet
* [GitHub](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/) 'daki kod örneklerini görüntüle
