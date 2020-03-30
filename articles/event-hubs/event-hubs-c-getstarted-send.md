---
title: "Hızlı başlangıç: C - Azure Etkinlik Hub'larını kullanarak etkinlik gönderme"
description: "Hızlı başlangıç: Bu makalede, Azure Etkinlik Hub'larına olay gönderen bir C uygulaması oluşturmak için bir iz yol sağlar."
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: c
ms.devlang: csharp
ms.topic: quickstart
ms.custom: seodec18
ms.date: 11/05/2019
ms.author: shvija
ms.openlocfilehash: 5bd4bb66b7e3c3ec37724f8684105befbc9132ff
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73720667"
---
# <a name="quickstart-send-events-to-azure-event-hubs-using-c"></a>Hızlı başlangıç: C kullanarak etkinlikleri Azure Etkinlik Hub'larına gönderme

## <a name="introduction"></a>Giriş
Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu öğretici, C'deki bir konsol uygulamasını kullanarak olayları bir olay merkezine nasıl gönderilen açıklar. 

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* C geliştirme ortamı. Bu öğretici, Ubuntu 14.04 ile Azure Linux VM'deki gcc yığınını varsayar.
* [Microsoft Visual Studio](https://www.visualstudio.com/).
* **Olay Hub'ları ad alanı ve olay hub'ı oluşturun.** Etkinlik Hub'larının ad alanını oluşturmak ve uygulamanızın etkinlik merkeziyle iletişim kurmak için ihtiyaç duyduğu yönetim kimlik bilgilerini elde etmek için [Azure portalını](https://portal.azure.com) kullanın. Ad alanı ve olay hub'ı oluşturmak için [bu makaledeki](event-hubs-create.md)yordamı izleyin. Makaledeki yönergeleri izleyerek olay hub'ı için erişim anahtarının değerini alın: [Bağlantı dizesini alın.](event-hubs-get-connection-string.md#get-connection-string-from-the-portal) Bu öğreticide daha sonra yazdığınız koddaki erişim anahtarını kullanırsınız. Varsayılan anahtar adı: **RootManageSharedAccessKey**.

## <a name="write-code-to-send-messages-to-event-hubs"></a>Olay Hub'larına ileti göndermek için kod yazma
Bu bölümde, etkinlik merkezinize olay göndermek için bir C uygulaması nasıl yazılanın gösterilmektedir. Kod, [Apache Qpid projesindeki](https://qpid.apache.org/)Proton AMQP kitaplığını kullanır. Bu, [bu örnekte](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504)gösterildiği gibi Servis Veri Servisi sıralarını ve C'den AMQP içeren konuları kullanmaya benzer. Daha fazla bilgi için [Qpid Proton belgelerine](https://qpid.apache.org/proton/index.html)bakın.

1. [Qpid AMQP Messenger sayfasından,](https://qpid.apache.org/proton/messenger.html)ortamınıza bağlı olarak Qpid Proton'u yüklemek için talimatları izleyin.
2. Proton kitaplığını derlemek için aşağıdaki paketleri yükleyin:
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. [Qpid Proton kitaplığını](https://qpid.apache.org/proton/index.html)indirin ve ayıklayın, örneğin:
   
    ```shell
    wget https://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. Yapı dizini oluşturun, derleve ve yükleyin:
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. Çalışma dizininizde, aşağıdaki kodu içeren **gönderen.c** adlı yeni bir dosya oluşturun. SAS anahtar/ad, olay merkezi adı ve ad alanı değerlerini değiştirmeyi unutmayın. Ayrıca, daha önce oluşturulan **SendRule** için anahtarın URL kodlanmış bir sürümünü de değiştirmelisiniz. [URL'yi burada](https://www.w3schools.com/tags/ref_urlencode.asp)kodlayabilirsiniz.
   
    ```c
    #include "proton/message.h"
    #include "proton/messenger.h"
   
    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>
   
    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }
   
    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  
   
    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }
   
    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://{SAS Key Name}:{SAS key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";
   
        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();
   
        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);
   
        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));
   
        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);
   
        pn_message_free(message);
    }
   
    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");
   
        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);
   
        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }
   
        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);
   
        return 0;
    }
    ```
6. **GCC**varsayarak, dosyayı derlemek:
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > Bu kod, iletileri mümkün olan en kısa sürede dışarı zorlamak için giden 1 penceresini kullanır. Uygulamanızın iş bilgililiği artırmak için toplu iletileri toplu olarak denemeniz önerilir. Qpid Proton kitaplığını bu ve diğer ortamlarda ve bağlamaların sağlandığı platformlardan (şu anda Perl, PHP, Python ve Ruby) nasıl kullanılacağı hakkında bilgi almak için [Qpid AMQP Messenger sayfasına](https://qpid.apache.org/proton/messenger.html) bakın.

Olay merkezine ileti göndermek için uygulamayı çalıştırın. 

Tebrikler! Bir olay hub'ına ileti gönderdiniz.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleleri okuyun:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Azure Etkinlik Hub'larında özellikler ve terminoloji.](event-hubs-features.md)


<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
