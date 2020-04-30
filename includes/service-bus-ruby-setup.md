---
author: spelluru
ms.service: service-bus
ms.topic: include
ms.date: 11/09/2018
ms.author: spelluru
ms.openlocfilehash: aec13c6beb8dbfcdd5f38e7f96b86bf03e42fa37
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80986804"
---
## <a name="create-a-ruby-application"></a>Ruby uygulaması oluşturma
Yönergeler için bkz. [Azure 'Da Ruby uygulaması oluşturma](../articles/virtual-machines/linux/classic/ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Uygulamanızı kullanmak için yapılandırma Service Bus
Service Bus kullanmak için, depolama REST hizmetleriyle iletişim kuran bir dizi kullanışlı kitaplık içeren Azure Ruby paketini indirip kullanın.

### <a name="use-rubygems-to-obtain-the-package"></a>Paketi edinmek için RubyGems kullanma
1. **PowerShell** (Windows), **Terminal** (Mac) veya **Bash** (Unix) gibi bir komut satırı arabirimi kullanın.
2. GED ve bağımlılıklarını yüklemek için komut penceresine "GEA Install Azure" yazın.

### <a name="import-the-package"></a>Paketi içeri aktarma
En sevdiğiniz metin düzenleyiciyi kullanarak, depolamayı kullanmayı düşündüğünüz Ruby dosyasının en üstüne aşağıdakileri ekleyin:

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Service Bus bağlantısını ayarlama
Ad alanı, anahtar, imzalayan ve konak adı değerlerini ayarlamak için aşağıdaki kodu kullanın:

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

Ad alanı değerini, URL 'nin tamamı yerine oluşturduğunuz değere ayarlayın. Örneğin, "yourexamplenamespace.servicebus.windows.net" değil, **"yourexamptanamespace"** kullanın.

Birden çok ad alanıyla çalışırken, nesneleri oluştururken `SharedAccessSigner` anahtarı ve adını oluşturucuya geçirebilirsiniz

```ruby
sb_namespace = '<your azure service bus namespace>'
sb_sas_key_name = '<your azure service bus access keyname>'
sb_sas_key = '<your azure service bus access key>'

signer = Azure::ServiceBus::Auth::SharedAccessSigner.new(sb_sas_key_name, sb_sas_key)
sb_host = "https://#{sb_namespace}.servicebus.windows.net"
```
