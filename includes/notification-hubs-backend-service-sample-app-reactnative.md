---
author: alexeystrakh
ms.author: alstrakh
ms.date: 06/11/2020
ms.service: notification-hubs
ms.topic: include
ms.openlocfilehash: c8271cc4de558ec5c97f41d6a070f71a9fc49cd2
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86060515"
---
### <a name="create-the-react-native-solution"></a>Tepki verme yerel çözümünü oluşturma

1. ' De `Terminal` , aşağıdaki komutları kullanarak, yerel olarak tepki vererek çalışmak için gereken ortam araçlarınızı güncelleştirin:

    ```bash
    # install node
    brew install node
    # or update
    brew update node
    # install watchman
    brew install watchman
    # or update
    brew upgrade watchman
    # install cocoapods
    sudo gem install cocoapods
    ```

1. İçinde `Terminal` , `React Native` kaldırmak için CLI yüklüyse, aşağıdaki komutu çalıştırın. `npx`Kullanılabilir en son tepki yerel CLI sürümüne otomatik olarak erişmek için kullanın:

    ```bash
    npm uninstall -g react-native-cli
    ```

    > [!NOTE]
    > Yerel olarak tepki verme yerleşik bir komut satırı arabirimine sahiptir. CLı 'nın belirli bir sürümünü genel olarak yükleyip yönetmek yerine, kullanarak çalışma zamanında geçerli sürüme erişmenizi öneririz `npx` . Bu, Node.js ile birlikte gelir. İle `npx react-native <command>` , CLI 'nın geçerli kararlı sürümü, komutun çalıştırıldığı sırada indirilir ve yürütülür.

1. Yeni uygulamayı oluşturmak istediğiniz projeler klasörünüze gidin. Parametresini belirterek TypeScript tabanlı şablonu kullanın `--template` :

    ```bash
    # init new project with npx
    npx react-native init PushDemo --template react-native-template-typescript
    ```

1. JavaScript paketleri oluşturan ve paketleri gerçek zamanlı olarak yenilemek üzere tüm kod güncelleştirmelerini izleyen Metro sunucusunu çalıştırın:

    ```bash
    cd PushDemo
    npx react-native start
    ```

1. Kurulumu doğrulamak için iOS uygulamasını çalıştırın. Aşağıdaki komutu yürütmeden önce bir iOS simülatörü veya bir iOS cihazı bağladığınızdan emin olun:

    ```bash
    npx react-native run-ios
    ```

1. Kurulumu doğrulamak için Android uygulamasını çalıştırın. Bir Android öykünücüsü veya cihazı, yanıt verme yerel Metro sunucusuna erişebilecek şekilde yapılandırmak için birkaç ek adım gerektirir. Aşağıdaki komutlar, Android için ilk JavaScript paketi oluşturur ve varlıklar klasörüne koyar.

    ```bash
    # create assets folder for the bundle
    mkdir android/app/scr/main/assets
    # build the bundle
    npx react-native bundle --platform android --dev true --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res
    # enable ability for sim to access the localhost
    adb reverse tcp:8081 tcp:8081
    ```

    Bu betik, uygulamanın ilk sürümü ile önceden dağıtılır. Dağıtıldıktan sonra, sunucu IP adresini belirterek öykünücü veya cihazınızı Metro sunucusuna erişecek şekilde yapılandırın. Android uygulamasını derlemek ve çalıştırmak için aşağıdaki komutu yürütün:

    ```bash
    npx react-native run-android
    ```

    Uygulamada bir kez, `CMD+M` Geliştirici ayarlarını doldurmak için (öykünücü) veya cihazı sallayın, şuraya gidin `Settings`  >  `Change Bundle Location` ve varsayılan bağlantı noktası ile Metro sunucusu IP adresini belirtin: `<metro-server-ip-address>:8081` .

1. `App.tsx`Dosyasında, sayfa düzeninde herhangi bir değişikliği uygulayın, kaydedin ve değişikliğin hem iOS hem de Android uygulamalarında otomatik olarak yansıtıldığından emin olun.

    > [!NOTE]
    > Ayrıntılı geliştirme ortamı Kurulum Kılavuzu [resmi belgelerde](https://reactnative.dev/docs/environment-setup) mevcuttur

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

Bu örneğin çalışması için aşağıdaki üç pakete ihtiyacınız vardır:

1. [Yerel anında Iletme bildirimleri iOS](https://www.npmjs.com/package/@react-native-community/push-notification-ios)  -  'a tepki verme [Proje GitHub](https://github.com/react-native-community/push-notification-ios)

    Bu paket, PushNotificationIOS 'ın, yanıt verme 'nin çekirdeğinizden bölündüğü zaman oluşturulmuştur. Paket, iOS için anında iletme bildirimleri uygular ve ona erişmek için tepki temelli yerel arabirim sağlar. Paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```bash
    yarn add @react-native-community/push-notification-ios
    ```

1. [Yerel anında Iletme bildirimleri platformlar arası tepki verme](https://www.npmjs.com/package/react-native-push-notification)

    Bu paket, iOS ve Android 'de platformlar arası bir şekilde yerel ve uzak bildirimler uygular. Paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```bash
    yarn add react-native-push-notification
    ```

1. [Cihaz bilgi paketi](https://www.npmjs.com/package/react-native-device-info) Paket, çalışma zamanında bir cihaz hakkında bilgi sağlar. Anında iletme bildirimine kaydolmak için kullanılan bir cihaz tanımlayıcısı tanımlamak için bunu kullanın. Paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```bash
    yarn add react-native-device-info
    ```

### <a name="implement-the-cross-platform-components"></a>Platformlar arası bileşenleri uygulama

1. Oluştur ve Uygula `DemoNotificationHandler` :

    ```typescript
    import PushNotification from 'react-native-push-notification';

    class DemoNotificationHandler {
      private _onRegister: any;
      private _onNotification: any;

      onNotification(notification: any) {
        console.log('NotificationHandler:', notification);

        if (typeof this._onNotification === 'function') {
          this._onNotification(notification);
        }
      }

      onRegister(token: any) {
        console.log('NotificationHandler:', token);

        if (typeof this._onRegister === 'function') {
          this._onRegister(token);
        }
      }

      attachTokenReceived(handler: any) {
        this._onRegister = handler;
      }

      attachNotificationReceived(handler: any) {
        this._onNotification = handler;
      }
    }

    const handler = new DemoNotificationHandler();

    PushNotification.configure({
      onRegister: handler.onRegister.bind(handler),
      onNotification: handler.onNotification.bind(handler),
      permissions: {
        alert: true,
        badge: true,
        sound: true,
      },
      popInitialNotification: true,
      requestPermissions: true,
    });

    export default handler;
    ```

1. Oluştur ve Uygula `DemoNotificationService` :

    ```typescript
    import PushNotification from 'react-native-push-notification';
    import DemoNotificationHandler from './DemoNotificationHandler';

    export default class DemoNotificationService {
      constructor(onTokenReceived: any, onNotificationReceived: any) {
        DemoNotificationHandler.attachTokenReceived(onTokenReceived);
        DemoNotificationHandler.attachNotificationReceived(onNotificationReceived);
        PushNotification.getApplicationIconBadgeNumber(function(number: number) {
          if(number > 0) {
            PushNotification.setApplicationIconBadgeNumber(0);
          }
        });
      }

      checkPermissions(cbk: any) {
        return PushNotification.checkPermissions(cbk);
      }

      requestPermissions() {
        return PushNotification.requestPermissions();
      }

      cancelNotifications() {
        PushNotification.cancelLocalNotifications();
      }

      cancelAll() {
        PushNotification.cancelAllLocalNotifications();
      }

      abandonPermissions() {
        PushNotification.abandonPermissions();
      }
    }
    ```

1. Oluştur ve Uygula `DemoNotificationRegistrationService` :

    ```typescript
    export default class DemoNotificationService {
        constructor(
            readonly apiUrl: string,
            readonly apiKey: string) {
        }

    async registerAsync(request: any): Promise<Response> {
            const method = 'PUT';
            const registerApiUrl = `${this.apiUrl}/notifications/installations`;
            const result = await fetch(registerApiUrl, {
                method: method,
                headers: {
                    Accept: 'application/json',
                    'Content-Type': 'application/json',
                    'apiKey': this.apiKey
                },
                body: JSON.stringify(request)
            });

            this.validateResponse(registerApiUrl, method, request, result);
            return result;
        }

        async deregisterAsync(deviceId: string): Promise<Response> {
            const method = 'DELETE';
            const deregisterApiUrl = `${this.apiUrl}/notifications/installations/${deviceId}`;
            const result = await fetch(deregisterApiUrl, {
                method: method,
                headers: {
                    Accept: 'application/json',
                    'Content-Type': 'application/json',
                    'apiKey': this.apiKey
                }
            });

            this.validateResponse(deregisterApiUrl, method, null, result);
            return result;
        }

        private validateResponse(requestUrl: string, method: string, requestPayload: any, response: Response) {
            console.log(`Request: ${method} ${requestUrl} => ${JSON.stringify(requestPayload)}\nResponse: ${response.status}`);
            if (!response || response.status != 200) {
                throw `HTTP error ${response.status}: ${response.statusText}`;
            }
        }
    }
    ```

1. Uygulamayı yapılandırın. Öğesini açın `package.json` ve aşağıdaki betik tanımını ekleyin:

    ```json
    "configure": "cp .app.config.tsx src/config/AppConfig.tsx"
    ```

    Ardından, varsayılan yapılandırmayı klasörüne kopyalayacaksınız ve bu betiği yürütün `config` .

    ```bash
    yarn configure
    ```

    Son adım, önceki adımda kopyaladığınız yapılandırma dosyasını API erişim bilgileriyle güncelleştirmedir. `apiKey`Ve `apiUrl` parametrelerini belirtin:

    ```typescript
    module.exports = {
        appName: "PushDemo",
        env: "production",
        apiUrl: "https://<azure-push-notifications-api-url>/api/",
        apiKey: "<api-auth-key>",
    };
    ```

### <a name="implement-the-cross-platform-ui"></a>Platformlar arası kullanıcı arabirimini uygulama

1. Sayfa düzeni tanımla

    ```typescript
    <View style={styles.container}>
      {this.state.isBusy &&
        <ActivityIndicator></ActivityIndicator>
      }
      <View style={styles.button}>
        <Button title="Register" onPress={this.onRegisterButtonPress.bind(this)} disabled={this.state.isBusy} />
      </View>
      <View style={styles.button}>
        <Button title="Deregister" onPress={this.onDeregisterButtonPress.bind(this)} disabled={this.state.isBusy} />
      </View>
    </View>
    ```

1. Stilleri Uygula

    ```css
    const styles = StyleSheet.create({
      container: {
        flex: 1,
        alignItems: "center",
        justifyContent: 'flex-end',
        margin: 50,
      },
      button: {
        margin: 5,
        width: "100%",
      }
    });
    ```

1. Sayfa bileşenini başlatma

    ```typescript
      state: IState;
      notificationService: DemoNotificationService;
      notificationRegistrationService: DemoNotificationRegistrationService;
      deviceId: string;

      constructor(props: any) {
        super(props);
        this.deviceId = DeviceInfo.getUniqueId();
        this.state = {
          status: "Push notifications registration status is unknown",
          registeredOS: "",
          registeredToken: "",
          isRegistered: false,
          isBusy: false,
        };

        this.notificationService = new DemoNotificationService(
          this.onTokenReceived.bind(this),
          this.onNotificationReceived.bind(this),
        );

        this.notificationRegistrationService = new DemoNotificationRegistrationService(
          Config.apiUrl,
          Config.apiKey,
        );
      }
    ```

1. Tıklama tıklaı tıklama işleyiciler

    ```typescript
      async onRegisterButtonPress() {
        if (!this.state.registeredToken || !this.state.registeredOS) {
          Alert.alert("The push notifications token wasn't received.");
          return;
        }

        let status: string = "Registering...";
        let isRegistered = this.state.isRegistered;
        try {
          this.setState({ isBusy: true, status });
          const pnPlatform = this.state.registeredOS == "ios" ? "apns" : "fcm";
          const pnToken = this.state.registeredToken;
          const request = {
            installationId: this.deviceId,
            platform: pnPlatform,
            pushChannel: pnToken,
            tags: []
          };
          const response = await this.notificationRegistrationService.registerAsync(request);
          status = `Registered for ${this.state.registeredOS} push notifications`;
          isRegistered = true;
        } catch (e) {
          status = `Registration failed: ${e}`;
        }
        finally {
          this.setState({ isBusy: false, status, isRegistered });
        }
      }

      async onDeregisterButtonPress() {
        if (!this.notificationService)
          return;

        let status: string = "Deregistering...";
        let isRegistered = this.state.isRegistered;
        try {
          this.setState({ isBusy: true, status });
          await this.notificationRegistrationService.deregisterAsync(this.deviceId);
          status = "Deregistered from push notifications";
          isRegistered = false;
        } catch (e) {
          status = `Deregistration failed: ${e}`;
        }
        finally {
          this.setState({ isBusy: false, status, isRegistered });
        }
      }
    ```

1. Alınan belirteç kayıtlarını ve anında iletme bildirimlerini işle

    ```typescript
      onTokenReceived(token: any) {
        console.log(`Received a notification token on ${token.os}`);
        this.setState({ registeredToken: token.token, registeredOS: token.os, status: `The push notifications token has been received.` });

        if (this.state.isRegistered && this.state.registeredToken && this.state.registeredOS) {
          this.onRegisterButtonPress();
        }
      }

      onNotificationReceived(notification: any) {
        console.log(`Received a push notification on ${this.state.registeredOS}`);
        this.setState({ status: `Received a push notification...` });

        if (notification.data.message) {
          Alert.alert(AppConfig.appName, `${notification.data.action} action received`);
        }
      }
    };
    ```