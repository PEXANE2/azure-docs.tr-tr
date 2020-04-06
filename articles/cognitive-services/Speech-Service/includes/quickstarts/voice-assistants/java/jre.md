---
author: trrwilson
ms.service: cognitive-services
ms.topic: include
ms.date: 04/04/2020
ms.author: travisw
ms.openlocfilehash: 38e6bae69710dc9e1dbc8789ee9ccb636193c7f7
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80671889"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları unutmayın:

> [!div class="checklist"]
> * [Azure Konuşma kaynağı oluşturma](~/articles/cognitive-services/speech-service/get-started.md)
> * [Geliştirme ortamınızı ayarlayın ve boş bir proje oluşturun](~/articles/cognitive-services/speech-service/quickstarts/setup-platform.md?tabs=jre&pivots=programming-language-java)
> * Doğrudan Satır Konuşma [kanalına](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech) bağlı bir bot oluşturma
> * Ses çekimi için mikrofona erişebildiğinizden emin olun

  > [!NOTE]
  > Lütfen [sesli asistanlar için desteklenen bölgeler listesine](~/articles/cognitive-services/speech-service/regions.md#voice-assistants) bakın ve kaynaklarınızın bu bölgelerden birinde dağıtıldığından emin olun.

## <a name="create-and-configure-project"></a>Proje oluşturma ve yapılandırma

[!INCLUDE [](~/includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

Ayrıca, günlüğe kaydetmeyi etkinleştirmek _için, pom.xml_ dosyasını aşağıdaki bağımlılıkları içerecek şekilde güncelleştirin:

```xml
 <dependency>
     <groupId>org.slf4j</groupId>
     <artifactId>slf4j-simple</artifactId>
     <version>1.7.5</version>
 </dependency>
```

## <a name="add-sample-code"></a>Örnek kod ekleme

1. Java projenize yeni bir boş sınıf eklemek için**Yeni** > **Sınıf** **Dosyası'nı** > seçin.

1. Yeni **Java Sınıfı** penceresinde, **Paket** alanına _speechsdk.quickstart_ ve **Ad** alanına _Main_ girin.

   ![Yeni Java Sınıfı penceresinin ekran görüntüsü](~/articles/cognitive-services/speech-service/media/sdk/qs-java-jre-06-create-main-java.png)

1. Yeni oluşturulan `Main` sınıfı açın ve dosyanın `Main.java` içeriğini aşağıdaki başlangıç koduyla değiştirin:

   ```java
   package speechsdk.quickstart;

   import com.microsoft.cognitiveservices.speech.audio.AudioConfig;
   import com.microsoft.cognitiveservices.speech.audio.PullAudioOutputStream;
   import com.microsoft.cognitiveservices.speech.dialog.DialogServiceConfig;
   import com.microsoft.cognitiveservices.speech.dialog.DialogServiceConnector;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;

   import javax.sound.sampled.AudioFormat;
   import javax.sound.sampled.AudioSystem;
   import javax.sound.sampled.DataLine;
   import javax.sound.sampled.SourceDataLine;
   import java.io.InputStream;

   public class Main {
       final Logger log = LoggerFactory.getLogger(Main.class);

       public static void main(String[] args) {
           // New code will go here
       }

       private void playAudioStream(PullAudioOutputStream audio) {
           ActivityAudioStream stream = new ActivityAudioStream(audio);
           final ActivityAudioStream.ActivityAudioFormat audioFormat = stream.getActivityAudioFormat();
           final AudioFormat format = new AudioFormat(
                   AudioFormat.Encoding.PCM_SIGNED,
                   audioFormat.getSamplesPerSecond(),
                   audioFormat.getBitsPerSample(),
                   audioFormat.getChannels(),
                   audioFormat.getFrameSize(),
                   audioFormat.getSamplesPerSecond(),
                   false);
           try {
               int bufferSize = format.getFrameSize();
               final byte[] data = new byte[bufferSize];

               SourceDataLine.Info info = new DataLine.Info(SourceDataLine.class, format);
               SourceDataLine line = (SourceDataLine) AudioSystem.getLine(info);
               line.open(format);

               if (line != null) {
                   line.start();
                   int nBytesRead = 0;
                   while (nBytesRead != -1) {
                       nBytesRead = stream.read(data);
                       if (nBytesRead != -1) {
                           line.write(data, 0, nBytesRead);
                       }
                   }
                   line.drain();
                   line.stop();
                   line.close();
               }
               stream.close();

           } catch (Exception e) {
               e.printStackTrace();
           }
       }

   }
   ```

1. `main` Yöntemde, önce yapılandırır `DialogServiceConfig` ve bir `DialogServiceConnector` örnek oluşturmak için kullanabilirsiniz. Bu örnek, botunuzun etkileşimini sağlamak için Doğrudan Satır Konuşması kanalına bağlanır. Ses `AudioConfig` girişinin kaynağını belirtmek için bir örnek de kullanılır. Bu örnekte, varsayılan mikrofon `AudioConfig.fromDefaultMicrophoneInput()`.

   - Bu web `YourSubscriptionKey` [sitesinden](~/articles/cognitive-services/speech-service/get-started.md)alabileceğiniz abonelik anahtarıile dize değiştirin.
   - Dizeyi `YourServiceRegion` aboneliğinizle ilişkili [bölgeyle](~/articles/cognitive-services/speech-service/regions.md) değiştirin.

   > [!NOTE]
   > Lütfen [sesli asistanlar için desteklenen bölgeler listesine](~/articles/cognitive-services/speech-service/regions.md#voice-assistants) bakın ve kaynaklarınızın bu bölgelerden birinde dağıtıldığından emin olun.

   ```java
   final String subscriptionKey = "YourSubscriptionKey"; // Your subscription key
   final String region = "YourServiceRegion"; // Your speech subscription service region
   final BotFrameworkConfig botConfig = BotFrameworkConfig.fromSubscription(subscriptionKey, region);

   // Configure audio input from a microphone.
   final AudioConfig audioConfig = AudioConfig.fromDefaultMicrophoneInput();

   // Create a DialogServiceConnector instance.
   final DialogServiceConnector connector = new DialogServiceConnector(botConfig, audioConfig);
   ```

1. Bağlayıcı, `DialogServiceConnector` bot etkinliklerini, konuşma tanıma sonuçlarını ve diğer bilgileri iletmek için çeşitli etkinliklere güvenir. Sonraki bu olay dinleyicileri ekleyin.

   ```java
   // Recognizing will provide the intermediate recognized text while an audio stream is being processed.
   connector.recognizing.addEventListener((o, speechRecognitionResultEventArgs) -> {
       log.info("Recognizing speech event text: {}", speechRecognitionResultEventArgs.getResult().getText());
   });

   // Recognized will provide the final recognized text once audio capture is completed.
   connector.recognized.addEventListener((o, speechRecognitionResultEventArgs) -> {
       log.info("Recognized speech event reason text: {}", speechRecognitionResultEventArgs.getResult().getText());
   });

   // SessionStarted will notify when audio begins flowing to the service for a turn.
   connector.sessionStarted.addEventListener((o, sessionEventArgs) -> {
       log.info("Session Started event id: {} ", sessionEventArgs.getSessionId());
   });

   // SessionStopped will notify when a turn is complete and it's safe to begin listening again.
   connector.sessionStopped.addEventListener((o, sessionEventArgs) -> {
       log.info("Session stopped event id: {}", sessionEventArgs.getSessionId());
   });

   // Canceled will be signaled when a turn is aborted or experiences an error condition.
   connector.canceled.addEventListener((o, canceledEventArgs) -> {
       log.info("Canceled event details: {}", canceledEventArgs.getErrorDetails());
       connector.disconnectAsync();
   });

   // ActivityReceived is the main way your bot will communicate with the client and uses Bot Framework activities.
   connector.activityReceived.addEventListener((o, activityEventArgs) -> {
       final String act = activityEventArgs.getActivity().serialize();
           log.info("Received activity {} audio", activityEventArgs.hasAudio() ? "with" : "without");
           if (activityEventArgs.hasAudio()) {
               playAudioStream(activityEventArgs.getAudio());
           }
       });
   ```

1. Yöntemi `DialogServiceConnector` çağırarak Doğrudan Satır Konuşması'na bağlanın. `connectAsync()` Botunuzu test etmek için `listenOnceAsync` mikrofonunuzdan ses girişi göndermek için yöntem çağırabilirsiniz. Ayrıca, özel bir etkinliği `sendActivityAsync` seri leştirilmiş dize olarak göndermek için yöntemi de kullanabilirsiniz. Bu özel etkinlikler, robotunuzun konuşmada kullandığı ek veriler sağlayabilir.

   ```java
   connector.connectAsync();
   // Start listening.
   System.out.println("Say something ...");
   connector.listenOnceAsync();

   // connector.sendActivityAsync(...)
   ```

1. Dosyadaki değişiklikleri `Main` kaydedin.

1. Yanıt çalmayı desteklemek için, getAudio() API'sinden döndürülen PullAudioOutputStream nesnesini işleme kolaylığı için Java InputStream'e dönüştüren ek bir sınıf ekleyin. Bu, `ActivityAudioStream` Doğrudan Satır Konuşması kanalından ses yanıtını işleyen özel bir sınıftır. Erişimcilere, oynatma işlemlerini işlemek için gereken ses biçimi bilgilerini getirmelerini sağlar. Bunun için **Yeni** > **New** > Dosya**yı**seçin.

1. Yeni **Java Sınıfı** penceresinde, **Paket** alanına _speechsdk.quickstart_ ve **Ad** alanında _ActivityAudioStream_ girin.

1. Yeni oluşturulan `ActivityAudioStream` sınıfı açın ve aşağıdaki kodla değiştirin:

   ```java
   package com.speechsdk.quickstart;

   import com.microsoft.cognitiveservices.speech.audio.PullAudioOutputStream;

   import java.io.IOException;
   import java.io.InputStream;

    public final class ActivityAudioStream extends InputStream {
        /**
         * The number of samples played per second (16 kHz).
         */
        public static final long SAMPLE_RATE = 16000;
        /**
         * The number of bits in each sample of a sound that has this format (16 bits).
         */
        public static final int BITS_PER_SECOND = 16;
        /**
         * The number of audio channels in this format (1 for mono).
         */
        public static final int CHANNELS = 1;
        /**
         * The number of bytes in each frame of a sound that has this format (2).
         */
        public static final int FRAME_SIZE = 2;

        /**
         * Reads up to a specified maximum number of bytes of data from the audio
         * stream, putting them into the given byte array.
         *
         * @param b   the buffer into which the data is read
         * @param off the offset, from the beginning of array <code>b</code>, at which
         *            the data will be written
         * @param len the maximum number of bytes to read
         * @return the total number of bytes read into the buffer, or -1 if there
         * is no more data because the end of the stream has been reached
         */
        @Override
        public int read(byte[] b, int off, int len) {
            byte[] tempBuffer = new byte[len];
            int n = (int) this.pullStreamImpl.read(tempBuffer);
            for (int i = 0; i < n; i++) {
                if (off + i > b.length) {
                    throw new ArrayIndexOutOfBoundsException(b.length);
                }
                b[off + i] = tempBuffer[i];
            }
            if (n == 0) {
                return -1;
            }
            return n;
        }

        /**
         * Reads the next byte of data from the activity audio stream if available.
         *
         * @return the next byte of data, or -1 if the end of the stream is reached
         * @see #read(byte[], int, int)
         * @see #read(byte[])
         * @see #available
         * <p>
         */
        @Override
        public int read() {
            byte[] data = new byte[1];
            int temp = read(data);
            if (temp <= 0) {
                // we have a weird situation if read(byte[]) returns 0!
                return -1;
            }
            return data[0] & 0xFF;
        }

        /**
         * Reads up to a specified maximum number of bytes of data from the activity audio stream,
         * putting them into the given byte array.
         *
         * @param b the buffer into which the data is read
         * @return the total number of bytes read into the buffer, or -1 if there
         * is no more data because the end of the stream has been reached
         */
        @Override
        public int read(byte[] b) {
            int n = (int) pullStreamImpl.read(b);
            if (n == 0) {
                return -1;
            }
            return n;
        }

        /**
         * Skips over and discards a specified number of bytes from this
         * audio input stream.
         *
         * @param n the requested number of bytes to be skipped
         * @return the actual number of bytes skipped
         * @throws IOException if an input or output error occurs
         * @see #read
         * @see #available
         */
        @Override
        public long skip(long n) {
            if (n <= 0) {
                return 0;
            }
            if (n <= Integer.MAX_VALUE) {
                byte[] tempBuffer = new byte[(int) n];
                return read(tempBuffer);
            }
            long count = 0;
            for (long i = n; i > 0; i -= Integer.MAX_VALUE) {
                int size = (int) Math.min(Integer.MAX_VALUE, i);
                byte[] tempBuffer = new byte[size];
                count += read(tempBuffer);
            }
            return count;
        }

        /**
         * Closes this audio input stream and releases any system resources associated
         * with the stream.
         */
        @Override
        public void close() {
            this.pullStreamImpl.close();
        }

        /**
         * Fetch the audio format for the ActivityAudioStream. The ActivityAudioFormat defines the sample rate, bits per sample, and the # channels.
         *
         * @return instance of the ActivityAudioFormat associated with the stream
         */
        public ActivityAudioStream.ActivityAudioFormat getActivityAudioFormat() {
            return activityAudioFormat;
        }

        /**
         * Returns the maximum number of bytes that can be read (or skipped over) from this
         * audio input stream without blocking.
         *
         * @return the number of bytes that can be read from this audio input stream without blocking.
         * As this implementation does not buffer, this will be defaulted to 0
         */
        @Override
        public int available() {
            return 0;
        }

        public ActivityAudioStream(final PullAudioOutputStream stream) {
            pullStreamImpl = stream;
            this.activityAudioFormat = new ActivityAudioStream.ActivityAudioFormat(SAMPLE_RATE, BITS_PER_SECOND, CHANNELS, FRAME_SIZE, AudioEncoding.PCM_SIGNED);
        }

        private PullAudioOutputStream pullStreamImpl;

        private ActivityAudioFormat activityAudioFormat;

        /**
         * ActivityAudioFormat is an internal format which contains metadata regarding the type of arrangement of
         * audio bits in this activity audio stream.
         */
        static class ActivityAudioFormat {

            private long samplesPerSecond;
            private int bitsPerSample;
            private int channels;
            private int frameSize;
            private AudioEncoding encoding;

            public ActivityAudioFormat(long samplesPerSecond, int bitsPerSample, int channels, int frameSize, AudioEncoding encoding) {
                this.samplesPerSecond = samplesPerSecond;
                this.bitsPerSample = bitsPerSample;
                this.channels = channels;
                this.encoding = encoding;
                this.frameSize = frameSize;
            }

            /**
             * Fetch the number of samples played per second for the associated audio stream format.
             *
             * @return the number of samples played per second
             */
            public long getSamplesPerSecond() {
                return samplesPerSecond;
            }

            /**
             * Fetch the number of bits in each sample of a sound that has this audio stream format.
             *
             * @return the number of bits per sample
             */
            public int getBitsPerSample() {
                return bitsPerSample;
            }

            /**
             * Fetch the number of audio channels used by this audio stream format.
             *
             * @return the number of channels
             */
            public int getChannels() {
                return channels;
            }

            /**
             * Fetch the default number of bytes in a frame required by this audio stream format.
             *
             * @return the number of bytes
             */
            public int getFrameSize() {
                return frameSize;
            }

            /**
             * Fetch the audio encoding type associated with this audio stream format.
             *
             * @return the encoding associated
             */
            public AudioEncoding getEncoding() {
                return encoding;
            }
        }

        /**
         * Enum defining the types of audio encoding supported by this stream.
         */
        public enum AudioEncoding {
            PCM_SIGNED("PCM_SIGNED");

            String value;

            AudioEncoding(String value) {
                this.value = value;
            }
        }
    }

   ```

1. Dosyadaki değişiklikleri `ActivityAudioStream` kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

F11'i seçin veya**Hata Ayıklama'yı** **çalıştır'ı** > seçin.
Konsol "Bir şey söyle" iletisini görüntüler.
Bu noktada, botunuzun anlayabileceği İngilizce bir ifade veya cümle konuşun. Konuşmanız, botunuzun tanındığı ve işlendiği Doğrudan Hat Konuşması kanalı aracılığıyla botunuza iletilir. Yanıt bir etkinlik olarak döndürülür. Botunuz yanıtı olarak konuşmayı döndürürse, `AudioPlayer` ses sınıfı kullanılarak oynatılır.

![Başarılı tanıma sonrasında konsol çıktısının ekran görüntüsü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-08-console-output.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
