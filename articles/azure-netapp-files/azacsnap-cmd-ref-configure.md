---
title: Azure NetApp Files için Azure uygulaması tutarlı anlık görüntü aracını yapılandırma | Microsoft Docs
description: Azure NetApp Files ile kullanabileceğiniz Azure uygulaması tutarlı anlık görüntü aracının Yapılandır komutunu çalıştırmaya yönelik bir kılavuz sağlar.
services: azure-netapp-files
documentationcenter: ''
author: Phil-Jensen
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 12/14/2020
ms.author: phjensen
ms.openlocfilehash: 0875aae8bb9049fc96377c1c98efa7391211d08f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "97633075"
---
# <a name="configure-azure-application-consistent-snapshot-tool-preview"></a>Azure uygulaması tutarlı anlık görüntü aracını yapılandırma (Önizleme)

Bu makale, Azure NetApp Files ile kullanabileceğiniz Azure uygulaması tutarlı anlık görüntü aracının Yapılandır komutunu çalıştırmaya yönelik bir kılavuz sağlar.

## <a name="introduction"></a>Giriş

Yapılandırma dosyası, komutu kullanılarak oluşturulabilir veya düzenlenebilir `azacsnap -c configure` .

## <a name="command-options"></a>Komut seçenekleri

`-c configure`Komut aşağıdaki seçeneklere sahiptir

- `--configuration new` Yeni bir yapılandırma dosyası oluşturmak için.

- `--configuration edit` Varolan bir yapılandırma dosyasını düzenlemek için.

- `[--configfile <config filename>]` , özel yapılandırma dosyası adlarına izin veren isteğe bağlı bir parametredir.

## <a name="configuration-file-for-snapshot-tools"></a>Snapshot araçları için yapılandırma dosyası

Bir yapılandırma dosyası çalıştırılarak oluşturulabilir `azacsnap -c configure --configuration new` .  Varsayılan olarak, yapılandırma dosya adı ' dir `azacsnap.json` .  Özel bir dosya adı parametresiyle birlikte kullanılabilir `--configfile=` (örneğin, `--configfile=<customname>.json` ) aşağıdaki örnek Azure büyük örnek yapılandırmasına yöneliktir:

```bash
azacsnap -c configure --configuration new
```

```output
Building new config file
Add comment to config file (blank entry to exit adding comments):This is a new config file for `azacsnap`
Add comment to config file (blank entry to exit adding comments):
Add database to config? (y/n) [n]: y
HANA SID (for example, H80): H80
HANA Instance Number (for example, 00): 00
HANA HDB User Store Key (for example, `hdbuserstore List`): AZACSNAP
HANA Server's Address (hostname or IP address): testing01
Add ANF Storage to database section? (y/n) [n]:
Add HLI Storage to database section? (y/n) [n]: y
Add DATA Volume to HLI Storage section of Database section? (y/n) [n]: y
Storage User Name (for example, clbackup25): clt1h80backup
Storage IP Address (for example, 192.168.1.30): 172.18.18.11
Storage Volume Name (for example, hana_data_h80_testing01_mnt00001_t250_vol): hana_data_h80_testing01_mnt00001_t020_vol
Add DATA Volume to HLI Storage section of Database section? (y/n) [n]:
Add OTHER Volume to HLI Storage section of Database section? (y/n) [n]:
Add HLI Storage to database section? (y/n) [n]:
Add database to config? (y/n) [n]:
Editing configuration complete, writing output to 'azacsnap.json'
```

## <a name="details-of-required-values"></a>Gerekli değerlerin ayrıntıları

Aşağıdaki bölümlerde yapılandırma dosyası için gereken çeşitli değerler hakkında ayrıntılı yönergeler sağlanmaktadır.

### <a name="sap-hana-values"></a>SAP HANA değerleri

Yapılandırmaya bir *veritabanı* eklenirken aşağıdaki değerler gereklidir:

- **Hana sunucusunun Address** = SAP HANA sunucusu konak adı veya IP adresi.
- **Hana SID** = SAP HANA sistem kimliği.
- **Hana örnek numarası** = SAP HANA örnek numarası.
- **Hana HDB kullanıcı deposu anahtarı** = SAP HANA Kullanıcı, veritabanı yedeklerini çalıştırma izinleriyle yapılandırılır.

- Tek düğüm: düğümün IP 'si ve ana bilgisayar adı
- STONITH ile HSR: düğümün IP 'si ve ana bilgisayar adı
- Ölçeği genişletme (N + N, N + e): geçerli ana düğüm IP 'si ve ana bilgisayar adı
- STONITH olmadan HSR: düğümün IP 'si ve ana bilgisayar adı
- Tek düğümde çoklu SID: Bu SID 'Leri barındıran düğümün ana bilgisayar adı ve IP 'si

### <a name="azure-large-instance-hli-storage-values"></a>Azure büyük örnek (HLI) depolama değerleri

Bir veritabanı bölümüne *hLi depolama* eklerken aşağıdaki değerler gereklidir:

- **Depolama Kullanıcı adı** = bu değer, depolama alanına SSH bağlantısı kurmak için kullanılan kullanıcı adıdır.
- **Depolama IP adresi** = depolama sisteminin adresi.
- **Depolama birimi adı** = anlık görüntünün birim adı.  Bu değer birden çok şekilde belirlenebilir, belki de en basit, aşağıdaki kabuk komutunu denemelidir:

    ```bash
    grep nfs /etc/fstab | cut -f2 -d"/" | sort | uniq
    ```

    ```output
    hana_data_p40_soldub41_mnt00001_t020_vol
    hana_log_backups_p40_soldub41_t020_vol
    hana_log_p40_soldub41_mnt00001_t020_vol
    hana_shared_p40_soldub41_t020_vol
    ```

### <a name="azure-netapp-files-anf-storage-values"></a>Azure NetApp Files (ANF) depolama değerleri

Bir veritabanı bölümüne *ANF depolama* eklerken, aşağıdaki değerler gereklidir:

- **Hizmet sorumlusu kimlik doğrulaması dosya adı** = bu, `authfile.json` Azure NetApp Files depolama ile iletişim yapılandırılırken Cloud Shell oluşturulan dosyadır.
- **Tam ANF depolama birimi kaynak kimliği** = anlık görüntü yapılacak birimin tam kaynak kimliği.  Bu, şu kaynaktan alınabilir: Azure portal – > ANF – > Volume – > ayarlar/Özellikler – > kaynak KIMLIĞI

## <a name="configuration-file-overview-azacsnapjson"></a>Yapılandırma dosyasına genel bakış ( `azacsnap.json` )

Aşağıdaki örnekte, `azacsnap.json` BIR SID ile yapılandırılır.

Parametre değerleri müşterinin özel SAP HANA ortamına ayarlanmalıdır.
**Azure büyük örnek** sistemi için, bu bilgiler ekleme/devreden Multipath çağrısı sırasında Microsoft hizmet yönetimi tarafından sağlanır ve devreden Multipath sırasında sağlanan bir Excel dosyasında kullanılabilir hale getirilir. Bu bilgileri yeniden istemeniz gerekiyorsa bir hizmet isteği açın.

Aşağıda yalnızca bir örnek verilmiştir. tüm değerleri uygun şekilde güncelleştirin.

```bash
cat azacsnap.json
```

```output
{
  "version": "5.0 Preview",
  "logPath": "./logs",
  "securityPath": "./security",
  "comments": [],
  "database": [
    {
      "hana": {
        "serverAddress": "sapprdhdb80",
        "sid": "H80",
        "instanceNumber": "00",
        "hdbUserStoreName": "SCADMIN",
        "savePointAbortWaitSeconds": 600,
        "hliStorage": [
          {
            "dataVolume": [
              {
                "backupName": "clt1h80backup",
                "ipAddress": "172.18.18.11",
                "volume": "hana_data_h80_azsollabbl20a31_mnt00001_t210_vol"
              },
              {
                "backupName": "clt1h80backup",
                "ipAddress": "172.18.18.11",
                "volume": "hana_shared_h80_azsollabbl20a31_t210_vol"
              }
            ],
            "otherVolume": [
              {
                "backupName": "clt1h80backup",
                "ipAddress": "172.18.18.11",
                "volume": "hana_log_backups_h80_azsollabbl20a31_t210_vol"
              }
            ]
          }
        ],
        "anfStorage": []
      }
    }
  ]
}
```

> [!NOTE]
> Dr sitesinde yedeklemelerin çalıştırılacağı bir DR senaryosunda, Dr sitesindeki Dr yapılandırma dosyasında (örneğin,) yapılandırılan HANA sunucu adı, `DR.json` üretim sunucusu adı ile aynı olmalıdır.

> [!NOTE]
> Azure büyük örneği için depolama IP adresiniz, sunucu havuzunuzdaki aynı alt ağda olmalıdır. Örneğin, bu durumda, sunucu havuzu alt ağımız 172 ' dir. 18. 18.0/24 ve atanan depolama IP 'si 172.18.18.11.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure uygulaması tutarlı anlık görüntü aracını test etme](azacsnap-cmd-ref-test.md)
