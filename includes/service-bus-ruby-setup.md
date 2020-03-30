---
author: spelluru
ms.service: service-bus
ms.topic: include
ms.date: 11/09/2018
ms.author: spelluru
ms.openlocfilehash: 16ce537a54fc77fc0f72b859d6d193501d86c1fc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "67188604"
---
## <a name="create-a-ruby-application"></a>Ruby uygulaması oluşturma
Talimatlar için [bkz.](../articles/virtual-machines/linux/classic/ruby-rails-web-app.md)

## <a name="configure-your-application-to-use-service-bus"></a>Uygulamanızı Hizmet Veri Aracı Kullanacak Şekilde Yapılandırın
Hizmet Veri Servisi'ni kullanmak için, depolama REST hizmetleriyle iletişim kuran bir dizi kolaylık kitaplığı içeren Azure Ruby paketini indirin ve kullanın.

### <a name="use-rubygems-to-obtain-the-package"></a>Paketi edinmek için RubyGems kullanma
1. **PowerShell** (Windows), **Terminal** (Mac) veya **Bash** (Unix) gibi bir komut satırı arabirimi kullanın.
2. Mücevheri ve bağımlılıkları yüklemek için komut penceresine "mücevher yükleme azure" yazın.

### <a name="import-the-package"></a>Paketi içeri aktarma
En sevdiğiniz metin düzenleyicisini kullanarak, depolama alanını kullanmak istediğiniz Ruby dosyasının üst bölümüne aşağıdakileri ekleyin:

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Servis Veri Servisi bağlantısı ayarlama
Ad alanı, anahtar, anahtar, imzalayan ve ana bilgisayar değerlerini ayarlamak için aşağıdaki kodu kullanın:

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

Ad alanı değerini, URL'nin tamamı yerine oluşturduğunuz değere ayarlayın. Örneğin, "yourexamplenamespace.servicebus.windows.net" değil, **"örnekname alanı"** kullanın.
