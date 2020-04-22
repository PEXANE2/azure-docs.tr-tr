---
title: OPC Publisher çalıştırın - Azure | Microsoft Dokümanlar
description: Bu makalede, OPC Publisher'ın nasıl çalıştırılıp hata ayıklanınca açıklanmaktadır. Ayrıca performans ve bellek hususlar ı da ele alıyor.
author: dominicbetts
ms.author: dobett
ms.date: 06/10/2019
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: c664d4859a306387b4eafa2f19ab5877ccf6eb1b
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686950"
---
# <a name="run-opc-publisher"></a>OPC Yayımcısını Çalıştırma

Bu makalede, reklam hata ayıklama OPC Publisher çalıştırmak için nasıl açıklanmaktadır. Ayrıca performans ve bellek hususlar ı da ele alıyor.

## <a name="command-line-options"></a>Komut satırı seçenekleri

Uygulama kullanımı `--help` komut satırı seçeneği kullanılarak aşağıdaki gibi gösterilir:

```sh/cmd
Current directory is: /appdata
Log file is: <hostname>-publisher.log
Log level is: info

OPC Publisher V2.3.0
Informational version: V2.3.0+Branch.develop_hans_methodlog.Sha.0985e54f01a0b0d7f143b1248936022ea5d749f9

Usage: opcpublisher.exe <applicationname> [<IoT Hubconnectionstring>] [<options>]

OPC Edge Publisher to subscribe to configured OPC UA servers and send telemetry to Azure IoT Hub.
To exit the application, just press CTRL-C while it is running.

applicationname: the OPC UA application name to use, required
                  The application name is also used to register the publisher under this name in the
                  IoT Hub device registry.

IoT Hubconnectionstring: the IoT Hub owner connectionstring, optional

There are a couple of environment variables which can be used to control the application:
_HUB_CS: sets the IoT Hub owner connectionstring
_GW_LOGP: sets the filename of the log file to use
_TPC_SP: sets the path to store certificates of trusted stations
_GW_PNFP: sets the filename of the publishing configuration file

Command line arguments overrule environment variable settings.

Options:
      --pf, --publishfile=VALUE
                              the filename to configure the nodes to publish.
                                Default: '/appdata/publishednodes.json'
      --tc, --telemetryconfigfile=VALUE
                              the filename to configure the ingested telemetry
                                Default: ''
  -s, --site=VALUE           the site OPC Publisher is working in. if specified
                                this domain is appended (delimited by a ':' to
                                the 'ApplicationURI' property when telemetry is
                                sent to IoT Hub.
                                The value must follow the syntactical rules of a
                                DNS hostname.
                                Default: not set
      --ic, --iotcentral     publisher will send OPC UA data in IoTCentral
                                compatible format (DisplayName of a node is used
                                as key, this key is the Field name in IoTCentral)
                                . you need to ensure that all DisplayName's are
                                unique. (Auto enables fetch display name)
                                Default: False
      --sw, --sessionconnectwait=VALUE
                              specify the wait time in seconds publisher is
                                trying to connect to disconnected endpoints and
                                starts monitoring unmonitored items
                                Min: 10
                                Default: 10
      --mq, --monitoreditemqueuecapacity=VALUE
                              specify how many notifications of monitored items
                                can be stored in the internal queue, if the data
                                can not be sent quick enough to IoT Hub
                                Min: 1024
                                Default: 8192
      --di, --diagnosticsinterval=VALUE
                              shows publisher diagnostic info at the specified
                                interval in seconds (need log level info).
                                -1 disables remote diagnostic log and diagnostic
                                output
                                0 disables diagnostic output
                                Default: 0
      --ns, --noshutdown=VALUE
                              same as runforever.
                                Default: False
      --rf, --runforever     publisher can not be stopped by pressing a key on
                                the console, but will run forever.
                                Default: False
      --lf, --logfile=VALUE  the filename of the logfile to use.
                                Default: './<hostname>-publisher.log'
      --lt, --logflushtimespan=VALUE
                              the timespan in seconds when the logfile should be
                                flushed.
                                Default: 00:00:30 sec
      --ll, --loglevel=VALUE the loglevel to use (allowed: fatal, error, warn,
                                info, debug, verbose).
                                Default: info
        --ih, --IoT Hubprotocol=VALUE
                              the protocol to use for communication with IoT Hub (
                                allowed values: Amqp, Http1, Amqp_WebSocket_Only,
                                  Amqp_Tcp_Only, Mqtt, Mqtt_WebSocket_Only, Mqtt_
                                Tcp_Only) or IoT EdgeHub (allowed values: Mqtt_
                                Tcp_Only, Amqp_Tcp_Only).
                                Default for IoT Hub: Mqtt_WebSocket_Only
                                Default for IoT EdgeHub: Amqp_Tcp_Only
      --ms, --IoT Hubmessagesize=VALUE
                              the max size of a message which can be send to
                                IoT Hub. when telemetry of this size is available
                                it will be sent.
                                0 will enforce immediate send when telemetry is
                                available
                                Min: 0
                                Max: 262144
                                Default: 262144
      --si, --IoT Hubsendinterval=VALUE
                              the interval in seconds when telemetry should be
                                send to IoT Hub. If 0, then only the
                                IoT Hubmessagesize parameter controls when
                                telemetry is sent.
                                Default: '10'
      --dc, --deviceconnectionstring=VALUE
                              if publisher is not able to register itself with
                                IoT Hub, you can create a device with name <
                                applicationname> manually and pass in the
                                connectionstring of this device.
                                Default: none
  -c, --connectionstring=VALUE
                              the IoT Hub owner connectionstring.
                                Default: none
      --hb, --heartbeatinterval=VALUE
                              the publisher is using this as default value in
                                seconds for the heartbeat interval setting of
                                nodes without
                                a heartbeat interval setting.
                                Default: 0
      --sf, --skipfirstevent=VALUE
                              the publisher is using this as default value for
                                the skip first event setting of nodes without
                                a skip first event setting.
                                Default: False
      --pn, --portnum=VALUE  the server port of the publisher OPC server
                                endpoint.
                                Default: 62222
      --pa, --path=VALUE     the enpoint URL path part of the publisher OPC
                                server endpoint.
                                Default: '/UA/Publisher'
      --lr, --ldsreginterval=VALUE
                              the LDS(-ME) registration interval in ms. If 0,
                                then the registration is disabled.
                                Default: 0
      --ol, --opcmaxstringlen=VALUE
                              the max length of a string opc can transmit/
                                receive.
                                Default: 131072
      --ot, --operationtimeout=VALUE
                              the operation timeout of the publisher OPC UA
                                client in ms.
                                Default: 120000
      --oi, --opcsamplinginterval=VALUE
                              the publisher is using this as default value in
                                milliseconds to request the servers to sample
                                the nodes with this interval
                                this value might be revised by the OPC UA
                                servers to a supported sampling interval.
                                please check the OPC UA specification for
                                details how this is handled by the OPC UA stack.
                                a negative value will set the sampling interval
                                to the publishing interval of the subscription
                                this node is on.
                                0 will configure the OPC UA server to sample in
                                the highest possible resolution and should be
                                taken with care.
                                Default: 1000
      --op, --opcpublishinginterval=VALUE
                              the publisher is using this as default value in
                                milliseconds for the publishing interval setting
                                of the subscriptions established to the OPC UA
                                servers.
                                please check the OPC UA specification for
                                details how this is handled by the OPC UA stack.
                                a value less than or equal zero will let the
                                server revise the publishing interval.
                                Default: 0
      --ct, --createsessiontimeout=VALUE
                              specify the timeout in seconds used when creating
                                a session to an endpoint. On unsuccessful
                                connection attemps a backoff up to 5 times the
                                specified timeout value is used.
                                Min: 1
                                Default: 10
      --ki, --keepaliveinterval=VALUE
                              specify the interval in seconds the publisher is
                                sending keep alive messages to the OPC servers
                                on the endpoints it is connected to.
                                Min: 2
                                Default: 2
      --kt, --keepalivethreshold=VALUE
                              specify the number of keep alive packets a server
                                can miss, before the session is disconneced
                                Min: 1
                                Default: 5
      --aa, --autoaccept     the publisher trusts all servers it is
                                establishing a connection to.
                                Default: False
      --tm, --trustmyself=VALUE
                              same as trustowncert.
                                Default: False
      --to, --trustowncert   the publisher certificate is put into the trusted
                                certificate store automatically.
                                Default: False
      --fd, --fetchdisplayname=VALUE
                              same as fetchname.
                                Default: False
      --fn, --fetchname      enable to read the display name of a published
                                node from the server. this will increase the
                                runtime.
                                Default: False
      --ss, --suppressedopcstatuscodes=VALUE
                              specifies the OPC UA status codes for which no
                                events should be generated.
                                Default: BadNoCommunication,
                                BadWaitingForInitialData
      --at, --appcertstoretype=VALUE
                              the own application cert store type.
                                (allowed values: Directory, X509Store)
                                Default: 'Directory'
      --ap, --appcertstorepath=VALUE
                              the path where the own application cert should be
                                stored
                                Default (depends on store type):
                                X509Store: 'CurrentUser\UA_MachineDefault'
                                Directory: 'pki/own'
      --tp, --trustedcertstorepath=VALUE
                              the path of the trusted cert store
                                Default: 'pki/trusted'
      --rp, --rejectedcertstorepath=VALUE
                              the path of the rejected cert store
                                Default 'pki/rejected'
      --ip, --issuercertstorepath=VALUE
                              the path of the trusted issuer cert store
                                Default 'pki/issuer'
      --csr                  show data to create a certificate signing request
                                Default 'False'
      --ab, --applicationcertbase64=VALUE
                              update/set this applications certificate with the
                                certificate passed in as bas64 string
      --af, --applicationcertfile=VALUE
                              update/set this applications certificate with the
                                certificate file specified
      --pb, --privatekeybase64=VALUE
                              initial provisioning of the application
                                certificate (with a PEM or PFX fomat) requires a
                                private key passed in as base64 string
      --pk, --privatekeyfile=VALUE
                              initial provisioning of the application
                                certificate (with a PEM or PFX fomat) requires a
                                private key passed in as file
      --cp, --certpassword=VALUE
                              the optional password for the PEM or PFX or the
                                installed application certificate
      --tb, --addtrustedcertbase64=VALUE
                              adds the certificate to the applications trusted
                                cert store passed in as base64 string (multiple
                                strings supported)
      --tf, --addtrustedcertfile=VALUE
                              adds the certificate file(s) to the applications
                                trusted cert store passed in as base64 string (
                                multiple filenames supported)
      --ib, --addissuercertbase64=VALUE
                              adds the specified issuer certificate to the
                                applications trusted issuer cert store passed in
                                as base64 string (multiple strings supported)
      --if, --addissuercertfile=VALUE
                              adds the specified issuer certificate file(s) to
                                the applications trusted issuer cert store (
                                multiple filenames supported)
      --rb, --updatecrlbase64=VALUE
                              update the CRL passed in as base64 string to the
                                corresponding cert store (trusted or trusted
                                issuer)
      --uc, --updatecrlfile=VALUE
                              update the CRL passed in as file to the
                                corresponding cert store (trusted or trusted
                                issuer)
      --rc, --removecert=VALUE
                              remove cert(s) with the given thumbprint(s) (
                                multiple thumbprints supported)
      --dt, --devicecertstoretype=VALUE
                              the IoT Hub device cert store type.
                                (allowed values: Directory, X509Store)
                                Default: X509Store
      --dp, --devicecertstorepath=VALUE
                              the path of the iot device cert store
                                Default Default (depends on store type):
                                X509Store: 'My'
                                Directory: 'CertificateStores/IoT Hub'
  -i, --install              register OPC Publisher with IoT Hub and then exits.
                                Default:  False
  -h, --help                 show this message and exit
      --st, --opcstacktracemask=VALUE
                              ignored, only supported for backward comaptibility.
      --sd, --shopfloordomain=VALUE
                              same as site option, only there for backward
                                compatibility
                                The value must follow the syntactical rules of a
                                DNS hostname.
                                Default: not set
      --vc, --verboseconsole=VALUE
                              ignored, only supported for backward comaptibility.
      --as, --autotrustservercerts=VALUE
                              same as autoaccept, only supported for backward
                                cmpatibility.
                                Default: False
      --tt, --trustedcertstoretype=VALUE
                              ignored, only supported for backward compatibility.
                                the trusted cert store will always reside in a
                                directory.
      --rt, --rejectedcertstoretype=VALUE
                              ignored, only supported for backward compatibility.
                                the rejected cert store will always reside in a
                                directory.
      --it, --issuercertstoretype=VALUE
                              ignored, only supported for backward compatibility.
                                the trusted issuer cert store will always
                                reside in a directory.
```

Genellikle, yalnızca uygulamanın ilk çalışmasında IoT Hub sahibi bağlantı dizesini belirtirsiniz. Bağlantı dizesi şifrelenir ve platform sertifika deposunda depolanır. Daha sonraki çalıştırmalarda, uygulama sertifika deposundan bağlantı dizesini okur. Her çalıştırmada bağlantı dizesini belirtirseniz, IoT Hub aygıt kayıt defterindeki uygulama için oluşturulan aygıt kaldırılır ve yeniden oluşturulur.

## <a name="run-natively-on-windows"></a>Windows'da yerel olarak çalıştırın

Visual Studio ile **opcpublisher.sln** projesini açın, çözümü oluşturun ve yayınlayın. Yayınladığınız **Hedef dizininde** uygulamayı aşağıdaki şekilde başlatabilirsiniz:

```cmd
dotnet opcpublisher.dll <applicationname> [<IoT Hubconnectionstring>] [options]
```

## <a name="use-a-self-built-container"></a>Kendi kendine inşa edilmiş bir kapsayıcı kullanma

Kendi kapsayıcınızı oluşturun ve aşağıdaki gibi başlatın:

```sh/cmd
docker run <your-container-name> <applicationname> [<IoT Hubconnectionstring>] [options]
```

## <a name="use-a-container-from-microsoft-container-registry"></a>Microsoft Kapsayıcı Kayıt Defteri'nden bir kapsayıcı kullanma

Microsoft Kapsayıcı Kayıt Defteri'nde önceden oluşturulmuş bir kapsayıcı vardır. Aşağıdaki gibi başlatın:

```sh/cmd
docker run mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

Desteklenen işletim sistemlerini ve işlemci mimarilerini görmek için [Docker Hub'ı](https://hub.docker.com/_/microsoft-iotedge-opc-publisher) kontrol edin. İşletim sistemi ve CPU mimariniz desteklenirse, Docker doğru kapsayıcıyı otomatik olarak seçer.

## <a name="run-as-an-azure-iot-edge-module"></a>Azure IoT Edge modülü olarak çalıştırın

OPC Publisher, [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge) modülü olarak kullanılmaya hazırdır. OPC Publisher'ı IoT Edge modülü olarak kullandığınızda, desteklenen tek aktarım protokolleri **Amqp_Tcp_Only** ve **Mqtt_Tcp_Only.**

IoT Edge dağıtımınıza modül olarak OPC Publisher eklemek için Azure portalındaki IoT Hub ayarlarınıza gidin ve aşağıdaki adımları tamamlayın:

1. **IoT Edge'e** gidin ve IoT Edge cihazınızı oluşturun veya seçin.
1. **Modülleri Ayarlama**'yı seçin.
1. **Dağıtım Modülleri** Altında **Ekle'yi** ve ardından **IoT Edge Modülünü**seçin.
1. **Ad** alanına **yayımcı**girin.
1. Image **URI** alanına girin`mcr.microsoft.com/iotedge/opc-publisher:<tag>`
1. Mevcut etiketleri Docker [Hub'da](https://hub.docker.com/_/microsoft-iotedge-opc-publisher) bulabilirsiniz
1. Aşağıdaki JSON'u **Kapsayıcı Oluşturma Seçenekleri** alanına yapıştırın:

    ```json
    {
        "Hostname": "publisher",
        "Cmd": [
            "--aa"
        ]
    }
    ```

    Bu yapılandırma, IoT Edge'i OPC Publisher görüntüsünü kullanarak **yayımcı** adı verilen bir kapsayıcıyı başlatmak için yapılandırır. Kapsayıcının sisteminin ana bilgisayar adı **yayımcı**olarak ayarlanır. OPC Publisher aşağıdaki komut satırı bağımsız `--aa`değişkeni ile çağrılır: . Bu seçenekle OPC Publisher, bağlandığı OPC UA sunucularının sertifikalarına güvenir. Herhangi bir OPC Publisher komut satırı seçeneklerini kullanabilirsiniz. Tek sınırlama, IoT Edge tarafından desteklenen **Kapsayıcı Oluşturma Seçenekleri'nin** boyutudur.

1. Diğer ayarları değiştirmeden bırakın ve **Kaydet'i**seçin.
1. OPC Publisher çıktısını başka bir IoT Edge modülüyle yerel olarak işlemek istiyorsanız, **Modülleri Ayarla** sayfasına geri dön. Ardından Yolları **Belirt** sekmesine gidin ve aşağıdaki JSON'a benzeyen yeni bir rota ekleyin:

    ```json
    {
      "routes": {
        "processingModuleToIoT Hub": "FROM /messages/modules/processingModule/outputs/* INTO $upstream",
        "opcPublisherToProcessingModule": "FROM /messages/modules/publisher INTO BrokeredEndpoint(\"/modules/processingModule/inputs/input1\")"
      }
    }
    ```

1. **Modülleri Ayarla** sayfasında, yapılandırmanın son sayfasına ulaşana kadar **İleri'yi**seçin.
1. Yapılandırmanızı IoT Edge'e göndermek için **Gönder'i** seçin.
1. Kenar aygıtınızda IoT Edge'i başlattığınızda ve docker **konteyneryayımı** çalışıyorsa, opc Publisher'ın `docker logs -f publisher` günlük çıktısını kullanarak veya logfileyi kontrol ederek kontrol edebilirsiniz. Önceki örnekte, günlük dosyası `d:\iiotegde\publisher-publisher.log`yukarıda. Ayrıca [iot-edge-opc-publisher-diagnostics aracını](https://github.com/Azure-Samples/iot-edge-opc-publisher-diagnostics)da kullanabilirsiniz.

### <a name="make-the-configuration-files-accessible-on-the-host"></a>Yapılandırma dosyalarını ana bilgisayarda erişilebilir hale getirme

IoT Edge modül yapılandırma dosyalarını ana bilgisayar dosya sisteminde erişilebilir hale getirmek için aşağıdaki **Kapsayıcı Oluşturma Seçenekleri'ni**kullanın. Aşağıdaki örnek, Windows için Linux Kapsayıcıları'nı kullanan bir dağıtımdır:

```json
{
    "Hostname": "publisher",
    "Cmd": [
        "--pf=./pn.json",
        "--aa"
    ],
    "HostConfig": {
        "Binds": [
            "d:/iiotedge:/appdata"
        ]
    }
}
```

Bu seçeneklerle, OPC Publisher dosyadan `./pn.json` yayımlaması gereken düğümleri okur ve kapsayıcının `/appdata` çalışma dizini başlangıçta ayarlanır. Bu ayarlarla, OPC Publisher `/appdata/pn.json` yapılandırmasını almak için dosyayı kapsayıcıdan okur. `--pf` Seçenek olmadan, OPC Publisher varsayılan yapılandırma `./publishednodes.json`dosyasını okumaya çalışır.

Varsayılan adı `publisher-publisher.log`kullanarak günlük dosyası yazılır `/appdata` ve `CertificateStores` dizin de bu dizinde oluşturulur.

Tüm bu dosyaları ana bilgisayar dosya sisteminde kullanılabilir hale getirmek için, kapsayıcı yapılandırması bir bağlama montaj hacmi gerektirir. Bağlama, `d://iiotedge:/appdata` kapsayıcı başlatmadaki `/appdata`geçerli çalışma dizini olan dizini ana bilgisayar `d://iiotedge`dizinine eşler. Bu seçenek olmadan, kapsayıcı sonraki başlatıldığında hiçbir dosya verisi kalıcı dır.

Windows kapsayıcıları çalıştırıyorsanız, `Binds` parametresözdizimi farklıdır. Kapsayıcı başlatmada, çalışma dizini `c:\appdata`. Yapılandırma dosyasını ana bilgisayardaki `d:\iiotedge`dizine koymak için, bölümdeki aşağıdaki eşlemi belirtin: `HostConfig`

```json
"HostConfig": {
    "Binds": [
        "d:/iiotedge:c:/appdata"
    ]
}
```

Linux'ta Linux kapsayıcıları çalıştırıyorsanız, `Binds` parametresözdizimi yine farklıdır. Kapsayıcı başlatmada, çalışma dizini `/appdata`. Yapılandırma dosyasını ana bilgisayardaki `/iiotedge` dizine koymak için, bölümdeki aşağıdaki eşlemi belirtin: `HostConfig`

```json
"HostConfig": {
    "Binds": [
        "/iiotedge:/appdata"
    ]
}
```

## <a name="considerations-when-using-a-container"></a>Kapsayıcı kullanırken dikkat edilmesi gerekenler

Aşağıdaki bölümler, bir kapsayıcı kullanırken aklınızda bulundurmanız gereken bazı şeyleri listeleyir:

### <a name="access-to-the-opc-publisher-opc-ua-server"></a>OPC Publisher OPC UA sunucusuna erişim

Varsayılan olarak, OPC Publisher OPC UA sunucusu bağlantı noktası 62222'de dinler. Bu gelen bağlantı noktasını bir kapsayıcıda ortaya çıkarmak için aşağıdaki komutu kullanın:

```sh/cmd
docker run -p 62222:62222 mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="enable-intercontainer-name-resolution"></a>Konteynerler arası ad çözümlemesini etkinleştirme

Kapsayıcının içinden diğer kapsayıcılara ad çözümlemesini etkinleştirmek için, docker bridge network'ünü tanımlayan bir kullanıcı oluşturun ve kapsayıcıyı `--network` bu ağa bu ağa bağlama seçeneğini kullanarak. Ayrıca `--name` aşağıdaki seçeneği kullanarak kapsayıcı bir ad atayın:

```sh/cmd
docker network create -d bridge iot_edge
docker run --network iot_edge --name publisher mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

Kapsayıcıartık aynı ağdaki diğer `publisher` kapsayıcılar tarafından ad kullanılarak ulaşılabilir.

### <a name="access-other-systems-from-within-the-container"></a>Konteynerin içinden diğer sistemlere erişin

Diğer kapsayıcılara önceki bölümde açıklanan parametreler kullanılarak ulaşılabilir. Docker'ın barındırıldığı işletim sistemi DNS etkinse, DNS tarafından bilinen tüm sistemlere erişmek işe yarar.

NetBIOS ad çözümlemesi kullanan ağlarda, kapsayıcınızı `--add-host` opsiyonla başlatarak diğer sistemlere erişimi etkinleştirin. Bu seçenek, kapsayıcının ana bilgisayar dosyasına etkili bir giriş ekler:

```cmd/sh
docker run --add-host mydevbox:192.168.178.23  mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="assign-a-hostname"></a>Ana bilgisayar adı atama

OPC Publisher, sertifika ve son nokta oluşturma için çalıştırdAradığı makinenin ana bilgisayar adını kullanır. Docker, `-h` seçenek tarafından ayarlanan bir kişi değilse rasgele bir ana bilgisayar adı seçer. Aşağıdaki örnek, kapsayıcının iç ana bilgisayar adının `publisher`nasıl ayarlanır:

```sh/cmd
docker run -h publisher mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="use-bind-mounts-shared-filesystem"></a>Bağlama montajlarını kullanma (paylaşılan dosya sistemi)

Kapsayıcı dosya sistemini kullanmak yerine, yapılandırma bilgilerini ve günlük dosyalarını depolamak için ana bilgisayar dosya sistemini seçebilirsiniz. Bu seçeneği yapılandırmak için `-v` bind montaj modunda seçeneğini `docker run` kullanın.

## <a name="opc-ua-x509-certificates"></a>OPC UA X.509 sertifikaları

OPC UA, bir bağlantı kurduklarında OPC UA istemcisinin ve sunucusunun kimliğini doğrulamak ve aralarındaki iletişimi şifrelemek için X.509 sertifikalarını kullanır. OPC Publisher, tüm sertifikaları yönetmek için OPC UA yığını tarafından tutulan sertifika depolarını kullanır. Başlangıçta, OPC Publisher kendisi için bir sertifika olup olmadığını denetler. Sertifika deposunda sertifika yoksa ve komut satırında sertifika yoksa, OPC Publisher kendi imzalı bir sertifika oluşturur. Daha fazla bilgi için **InitApplicationSecurityAsync** yöntemine `OpcApplicationConfigurationSecurity.cs`bakın.

Güvenilir bir CA tarafından imzalanmadıkları için kendi imzalı sertifikalar herhangi bir güvenlik sağlamaz.

OPC Publisher komut satırı seçenekleri sağlar:

- OPC Publisher tarafından kullanılan geçerli uygulama sertifikasının CSR bilgilerini alın.
- CA imzalı sertifikaya sahip OPC Publisher'ı sağlama.
- Yeni bir anahtar çifti ve eşleşen CA imzalı sertifika ile Sağlama OPC Publisher.
- Güvenilir bir eş veya güvenilir veren sertifika deposuna sertifika ekleyin.
- CRL ekleyin.
- Sertifikayı güvenilir eş veya güvenilir verenler sertifika deposundan kaldırın.

Tüm bu seçenekler, dosyaları veya base64 kodlanmış dizeleri kullanarak parametreleri geçiş sağlar.

Tüm sertifika depoları için varsayılan mağaza türü, komut satırı seçeneklerini kullanarak değiştirebileceğiniz dosya sistemidir. Kapsayıcı dosya sisteminde kalıcı depolama sağlamadığından, farklı bir depo türü seçmeniz gerekir. Sertifika depolarını ana dosya sisteminde veya Docker biriminde sürdürmek için Docker `-v` seçeneğini kullanın. Docker birimi kullanıyorsanız, base64 kodlanmış dizeleri kullanarak sertifikaları geçirebilirsiniz.

Çalışma zamanı ortamı, sertifikaların nasıl kalıcı olduğunu etkiler. Uygulamayı her çalıştırdığınızda yeni sertifika depoları oluşturmaktan kaçının:

- Windows'da yerel olarak çalışan, özel anahtara erişim `Directory` başarısız olduğundan, tür bir uygulama sertifikası deposu kullanamazsınız. Bu durumda, seçeneğini `--at X509Store`kullanın.
- Linux docker konteyner olarak çalışan, docker çalıştır seçeneği `-v <hostdirectory>:/appdata`ile ana dosya sistemi için sertifika depoları eşleyebilirsiniz. Bu seçenek, sertifikayı uygulama çalıştırmaları arasında kalıcı hale getirir.
- Linux docker konteyner olarak çalışan ve uygulama sertifikası için bir X509 mağaza `-v x509certstores:/root/.dotnet/corefx/cryptography/x509stores` kullanmak istiyorsanız, docker çalıştır seçeneği ve uygulama seçeneği kullanın`--at X509Store`

## <a name="performance-and-memory-considerations"></a>Performans ve bellek hususları

Bu bölümde bellek ve performansı yönetme seçenekleri anlatılmaktadır:

### <a name="command-line-parameters-to-control-performance-and-memory"></a>Performansı ve belleği kontrol etmek için komut satırı parametreleri

OPC Publisher'ı çalıştırdığınızda, performans gereksinimleriniz ve ev sahibinizde bulunan bellek kaynaklarının farkında olmanız gerekir.

Bellek ve performans birbirine bağlıdır ve her ikisi de yayımlamak için yapılandırdığınız düğümün yapılandırmasına bağlıdır. Aşağıdaki parametrelerin gereksinimlerinizi karşıladığını sağlayın:

- IoT Hub aralığı gönderir:`--si`
- IoT Hub ileti `1`boyutu (varsayılan):`--ms`
- İzlenen öğeler kuyruk kapasitesi:`--mq`

`--mq` Parametre, tüm OPC düğüm değeri değişiklik bildirimlerini arabellekten iç sıranın kapasitesinin üst sınırını denetler. OPC Publisher IoT Hub'a yeterince hızlı ileti gönderemezse, bu sıra bildirimleri arabellekte tutar. Parametre arabelleğe alınabilecek bildirim sayısını ayarlar. Bu kuyruktaki öğe sayısının test çalışırınızda arttığını görürseniz, iletileri kaybetmemek için şunları yapmalısınız:

- IoT Hub gönderme aralığını azaltın
- IoT Hub ileti boyutunu artırma

Parametre, `--si` OPC Publisher'ı belirtilen aralıkta IoT Hub'ına ileti göndermeye zorlar. OPC Publisher, `--ms` parametrede belirtilen ileti boyutuna ulaşılır ulaşmaz veya `--si` parametrede belirtilen araya erişilir ulaşmaz bir ileti gönderir. İleti boyutu seçeneğini devre dışı `--ms 0`kullanabilirsiniz. Bu durumda, OPC Publisher toplu veri için 256 kB mümkün olan en büyük IoT Hub ileti boyutunu kullanır.

Parametre, IoT Hub'a `--ms` gönderilen toplu iletileri toplu olarak göndermenizi sağlar. Kullandığınız protokol, IoT Hub'a ileti göndermenin ek yükünün yükün gerçek gönderme süresiyle karşılaştırıldığında yüksek olup olmadığını belirler. Senaryonuz, IoT Hub tarafından veri sorulduğunda gecikmeye izin veriyorsa, OPC Publisher'ı 256 kB'lik en büyük ileti boyutunu kullanacak şekilde yapılandırın.

Üretim senaryolarında OPC Publisher'ı kullanmadan önce, performans ve bellek kullanımını üretim koşullarında test edin. `--di` OPC Publisher'ın tanılama bilgilerini yazdığı aralığı saniyeler içinde belirtmek için parametreyi kullanabilirsiniz.

### <a name="test-measurements"></a>Test ölçümleri

Aşağıdaki örnek tanılama, 1 saniyelik Bir OPC yayımlama aralığı ile 500 düğüm yayımlama için `--si` farklı değerler ve `--ms` parametrelerile ölçümler gösterir.  Test, Windows 10'da 120 saniye boyunca yerel olarak bir OPC Publisher hata ayıklama yapısı kullandı. IoT Hub protokolü varsayılan MQTT protokolüydü.

#### <a name="default-configuration---si-10---ms-262144"></a>Varsayılan yapılandırma (--si 10 --ms 262144)

```log
==========================================================================
OpcPublisher status @ 26.10.2017 15:33:05 (started @ 26.10.2017 15:31:09)
---------------------------------
OPC sessions: 1
connected OPC sessions: 1
connected OPC subscriptions: 5
OPC monitored items: 500
---------------------------------
monitored items queue bounded capacity: 8192
monitored items queue current items: 0
monitored item notifications enqueued: 54363
monitored item notifications enqueue failure: 0
monitored item notifications dequeued: 54363
---------------------------------
messages sent to IoT Hub: 109
last successful msg sent @: 26.10.2017 15:33:04
bytes sent to IoT Hub: 12709429
avg msg size: 116600
msg send failures: 0
messages too large to sent to IoT Hub: 0
times we missed send interval: 0
---------------------------------
current working set in MB: 90
--si setting: 10
--ms setting: 262144
--ih setting: Mqtt
==========================================================================
```

Varsayılan yapılandırma, her 10 saniyede bir IoT Hub'ına veya IoT Hub'ının yutması için 256 kB veri kullanılabilir olduğunda veri gönderir. Bu yapılandırma, yaklaşık 10 saniyelik orta bir gecikme gecikmesi ekler, ancak büyük ileti boyutu nedeniyle veri kaybetme olasılığı en düşüktür. Tanılama çıktısı, kayıp OPC düğümü güncelleştirmesi olmadığını gösterir: `monitored item notifications enqueue failure: 0`.

#### <a name="constant-send-interval---si-1---ms-0"></a>Sabit gönderme aralığı (--si 1 --ms 0)

```log
==========================================================================
OpcPublisher status @ 26.10.2017 15:35:59 (started @ 26.10.2017 15:34:03)
---------------------------------
OPC sessions: 1
connected OPC sessions: 1
connected OPC subscriptions: 5
OPC monitored items: 500
---------------------------------
monitored items queue bounded capacity: 8192
monitored items queue current items: 0
monitored item notifications enqueued: 54243
monitored item notifications enqueue failure: 0
monitored item notifications dequeued: 54243
---------------------------------
messages sent to IoT Hub: 109
last successful msg sent @: 26.10.2017 15:35:59
bytes sent to IoT Hub: 12683836
avg msg size: 116365
msg send failures: 0
messages too large to sent to IoT Hub: 0
times we missed send interval: 0
---------------------------------
current working set in MB: 90
--si setting: 1
--ms setting: 0
--ih setting: Mqtt
==========================================================================
```

İleti boyutu 0 olarak ayarlandığında, OPC Publisher 256 kB olan en büyük desteklenen IoT Hub ileti boyutunu kullanarak verileri dahili olarak toplu olarak alır. Tanılama çıktısı ortalama ileti boyutunun 115.019 bayt olduğunu gösterir. Bu yapılandırmada OPC Publisher herhangi bir OPC düğüm değer güncelleştirmelerini kaybetmez ve varsayılanla karşılaştırıldığında daha düşük gecikme gecikmesine sahiptir.

### <a name="send-each-opc-node-value-update---si-0---ms-0"></a>Her OPC düğüm değeri güncelleştirmesi gönderin (--si 0 --ms 0)

```log
==========================================================================
OpcPublisher status @ 26.10.2017 15:39:33 (started @ 26.10.2017 15:37:37)
---------------------------------
OPC sessions: 1
connected OPC sessions: 1
connected OPC subscriptions: 5
OPC monitored items: 500
---------------------------------
monitored items queue bounded capacity: 8192
monitored items queue current items: 8184
monitored item notifications enqueued: 54232
monitored item notifications enqueue failure: 44624
monitored item notifications dequeued: 1424
---------------------------------
messages sent to IoT Hub: 1423
last successful msg sent @: 26.10.2017 15:39:33
bytes sent to IoT Hub: 333046
avg msg size: 234
msg send failures: 0
messages too large to sent to IoT Hub: 0
times we missed send interval: 0
---------------------------------
current working set in MB: 96
--si setting: 0
--ms setting: 0
--ih setting: Mqtt
==========================================================================
```

Bu yapılandırma, her OPC düğüm değeri için ioT Hub'a bir ileti değişikliği gönderir. Tanılama, ortalama ileti boyutunun 234 bayt olduğunu gösterir, ki bu küçüktür. Bu yapılandırmanın avantajı, OPC Publisher'ın herhangi bir gecikme sonu eklememesidir. Kayıp OPC düğüm değer güncelleştirmelerinin`monitored item notifications enqueue failure: 44624`sayısı yüksektir, bu da bu yapılandırmayı yüksek hacimli telemetri içeren senaryolar için uygun hale getirir.

### <a name="maximum-batching---si-0---ms-262144"></a>Maksimum toplu iş (--si 0 --ms 262144)

```log
==========================================================================
OpcPublisher status @ 26.10.2017 15:42:55 (started @ 26.10.2017 15:41:00)
---------------------------------
OPC sessions: 1
connected OPC sessions: 1
connected OPC subscriptions: 5
OPC monitored items: 500
---------------------------------
monitored items queue bounded capacity: 8192
monitored items queue current items: 0
monitored item notifications enqueued: 54137
monitored item notifications enqueue failure: 0
monitored item notifications dequeued: 54137
---------------------------------
messages sent to IoT Hub: 48
last successful msg sent @: 26.10.2017 15:42:55
bytes sent to IoT Hub: 12565544
avg msg size: 261782
msg send failures: 0
messages too large to sent to IoT Hub: 0
times we missed send interval: 0
---------------------------------
current working set in MB: 90
--si setting: 0
--ms setting: 262144
--ih setting: Mqtt
==========================================================================
```

Bu yapılandırma, mümkün olduğunca çok Sayıda OPC düğüm değeri güncelleştirmesi toplu. Maksimum IoT Hub ileti boyutu 256 kB'dir ve burada yapılandırılır. İstenilen gönderme aralığı yoktur, bu da IoT Hub'ın yutması gereken veri miktarının gecikmeyi belirlediği anlamına gelir. Bu yapılandırma, herhangi bir OPC düğüm değerlerini kaybetme olasılığı en düşüktür ve çok sayıda düğüm yayımlama için uygundur. Bu yapılandırmayı kullandığınızda, 256 kB ileti boyutuna ulaşılmazsa senaryonuzun yüksek gecikme nin başlandığı koşullar olmadığından emin olun.

## <a name="debug-the-application"></a>Uygulamada hata ayıklama

Uygulamayı hata ayıklamak için Visual Studio ile **opcpublisher.sln** çözüm dosyasını açın ve Visual Studio hata ayıklama araçlarını kullanın.

OPC Publisher'daki OPC UA sunucusuna erişmeniz gerekiyorsa, güvenlik duvarınızın sunucunun dinlediği bağlantı noktasına erişime izin verdiğinden emin olun. Varsayılan bağlantı noktası: 62222.

## <a name="control-the-application-remotely"></a>Uygulamayı uzaktan denetleme

Düğümleri yayımlayacak şekilde yapılandırma, IoT Hub doğrudan yöntemleri kullanılarak yapılabilir.

OPC Publisher okumak için birkaç ek IoT Hub doğrudan yöntem çağrıları uygular:

- Genel bilgiler.
- OPC oturumları, abonelikler ve izlenen öğeler hakkında tanılama bilgileri.
- IoT Hub iletileri ve olayları hakkında tanılama bilgileri.
- Başlangıç günlüğü.
- Günlüğün son 100 satırı.
- Başvuruyu kapatın.

Aşağıdaki GitHub depoları, [tanılama bilgilerini](https://github.com/Azure-Samples/iot-edge-opc-publisher-diagnostics)yayımlamak ve okumak için [düğümleri yapılandırmak için](https://github.com/Azure-Samples/iot-edge-opc-publisher-nodeconfiguration) araçlar içerir. Her iki araç da Docker Hub'da kapsayıcı olarak mevcuttur.

## <a name="use-a-sample-opc-ua-server"></a>Örnek bir OPC UA sunucusu kullanma

Gerçek bir OPC UA sunucunuz yoksa, başlamak için [örnek OPC UA PLC'yi](https://github.com/Azure-Samples/iot-edge-opc-plc) kullanabilirsiniz. Bu örnek PLC, Docker Hub'da da mevcuttur.

Rasgele veri ve anomalilerle etiketler oluşturan bir dizi etiket uygular. Ek etiket değerlerini simüle etmek gerekiyorsa, örneği genişletebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık OPC Publisher'ı çalıştırmayı öğrendiğiniz için, önerilen sonraki adımlar [OPC Twin](overview-opc-twin.md) ve [OPC Vault](overview-opc-vault.md)hakkında bilgi edinmektir.
