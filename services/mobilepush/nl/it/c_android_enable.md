    ---

copyright:
 years: 2015 2016

---


# Abilitazione delle applicazioni Android alla ricezione di {{site.data.keyword.mobilepushshort}}
{: #tag_based_notifications}
Ultimo aggiornamento: 16 agosto 2016
{: .last-updated}

Abilita le applicazioni Android a ricevere {{site.data.keyword.mobilepushshort}} e inviare {{site.data.keyword.mobilepushshort}} ai tuoi dispositivi.

## Installazione del Push SDK client con Gradle
{: #android_install}

Questa sezione descrive come installare e utilizzare il Push SDK client per sviluppare
    ulteriormente le tue applicazioni Android.

Il Push SDK dei servizi mobili Bluemix® può essere aggiunto utilizzando Gradle. Gradle
          scarica automaticamente le risorse dai repository e le rende disponibili alla tua applicazione Android. Assicurati di impostare correttamente
          Android Studio e Android Studio SDK. Per ulteriori informazioni su come impostare il tuo sistema, consulta la [panoramica di Android Studio](https://developer.android.com/tools/studio/index.html). Per informazioni su Gradle, consulta [Configuring Gradle Builds](http://developer.android.com/tools/building/configuring-gradle.html).

1. In Android Studio, dopo aver creato e aperto la tua applicazione mobile, apri il tuo file **build.gradle** dell'applicazione. 
2. Aggiungi le seguenti dipendenze alla tua applicazione mobile. Queste istruzioni di importazione sono necessarie per i frammenti di codice: 

	```
	import com.ibm.mobilefirstplatform.clientsdk.android.core.api.BMSClient;
	import com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPush;
	import com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushException;
	import com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushResponseListener;
	import com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushNotificationListener;
	import com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPSimplePushNotification;
	```


2. Aggiungi le seguenti dipendenze alla tua applicazione mobile. Le seguenti righe aggiungono l'SDK del client di push dei servizi Bluemix™ Mobile e l'SDK dei servizi Google Play alle tue dipendenze dell'ambito di compilazione. 

	```
	dependencies {
	  compile group: 'com.ibm.mobilefirstplatform.clientsdk.android', 
		name: 'push', 
		version: '2.+',
		ext: 'aar', 
		transitive: true
	}  
	```
3. Nel file **AndroidManifest.xml**, aggiungi le seguenti autorizzazioni. Per visualizzare un manifest di esempio, consulta [Applicazione di esempio Android helloPush](https://github.com/ibm-bluemix-mobile-services/bms-samples-android-hellopush/blob/master/helloPush/app/src/main/AndroidManifest.xml). Per visualizzare un file Gradle di esempio, consulta [Sample Build Gradle file](https://github.com/ibm-bluemix-mobile-services/bms-samples-android-hellopush/blob/master/helloPush/app/build.gradle).

	```
	<uses-permission android:name="android.permission.INTERNET"/>
	<uses-permission android:name="com.ibm.clientsdk.android.app.permission.C2D_MESSAGE" />
	<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
	<uses-permission android:name="android.permission.WAKE_LOCK" />
	<uses-permission android:name="android.permission.GET_ACCOUNTS" />
	<uses-permission android:name="android.permission.USE_CREDENTIALS" />
	<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
	<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>

	```
   Leggi ulteriori informazioni sulle [autorizzazioni Android](http://developer.android.com/guide/topics/security/permissions.html) qui.

4. Aggiungi le impostazioni di intento di notifica per l'attività. Questa impostazione avvia l'applicazione
          quando l'utente fa clic sulla notifica ricevuta dall'area di notifica.

	```
	<intent-filter>  
		<action android:name="<Your_Android_Package_Name.IBMPushNotification"/>   
		<category  android:name="android.intent.category.DEFAULT"/>
	</intent-filter>
	```
	**Nota**: sostituisci *Your_Android_Package_Name* nell'azione sopra indicata con il nome del pacchetto applicazione utilizzato nella tua applicazione.

5. Aggiungi il servizio di intento GCM (Google Cloud Messaging) e i filtri di intento per
          le notifiche di evento RECEIVE.

	```
	service android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.api.MFPPushIntentService" />

	<receiver
	    android:name="com.ibm.mobilefirstplatform.clientsdk.android.push.internal.MFPPushBroadcastReceiver"
	    android:permission="com.google.android.c2dm.permission.SEND">
	    <intent-filter>
	        <action android:name="com.google.android.c2dm.intent.RECEIVE" />

	        <category android:name="com.ibm.mobilefirstplatform.clientsdk.android.app" />
	    </intent-filter>
	    <intent-filter>
	        <action android:name="android.intent.action.BOOT_COMPLETED" />

	        <category android:name="com.ibm.mobilefirstplatform.clientsdk.android.app" />
	    </intent-filter>
	</receiver>
	```


## Inizializzazione di Push SDK per applicazioni Android
{: #android_initialize}

Un posto comune dove inserire il codice di inizializzazione è nel metodo onCreate dell'attività principale nella tua applicazione Android.

Per ottenere la rotta e il GUID dell'applicazione, seleziona l'opzione di configurazione nel pannello di navigazione per i servizi di push inizializzati e fai clic su **Opzioni mobili**.
Utilizza questi valori per la rotta e il GUID dell'applicazione. Modifica il frammento di codice per utilizzare la tua applicazione Bluemix appRoute e i parametri appGUID.


###Inizializza il Core SDK

```
// Inizializza l'SDK for Java (Android) con IBM Bluemix AppGUID e la rotta
BMSClient.getInstance().initialize(getApplicationContext(), appRoute , appGuid, bluemixRegionSuffix);

```


####appRoute
{: approute}

Specifica la rotta assegnato all'applicazione server creata in Bluemix.

####AppGUID
{: appguid}

Specifica la chiave univoca assegnata all'istanza del servizio {{site.data.keyword.mobilepushshort}} in Bluemix. Questo valore è
                sensibile al maiuscolo/minuscolo. Puoi ottenere questo valore dalle opzioni mobili nel dashboard Push.

####bluemixRegionSuffix
{: bluemixRegionSuffix}

Specifica l'ubicazione in cui è ospitata l'applicazione. Puoi utilizzare uno dei seguenti tre valori: 

- BMSClient.REGION_US_SOUTH
- BMSClient.REGION_UK
- BMSClient.REGION_SYDNEY

###Inizializza il Push SDK client

```
//Inizializza il Push SDK for Java client
MFPPush push = MFPPush.getInstance();
push.initialize(getApplicationContext(), "AppGUID");
```
####AppGUID
{: AppGUID}

Questa è la chiave AppGUID del servizio {{site.data.keyword.mobilepushshort}}.

## Registrazione di dispositivi Android
{: #android_register}

Utilizza l'API `MFPPush.register()` per registrare il dispositivo con il servizio {{site.data.keyword.mobilepushshort}}. Per registrare le periferiche Android, devi aggiungere le informazioni di GCM (Google Cloud Messaging) nel dashboard di configurazione del servizio {{site.data.keyword.mobilepushshort}} Bluemix. Per ulteriori informazioni, consulta [Configurazione delle credenziali per GCM (Google Cloud Messaging)](t_push_provider_android.html).

Copia e incolla i seguenti frammenti di codice nella tua applicazione mobile Android.

```
	//Registra dispositivi Android
	push.registerDevice(new MFPPushResponseListener<String>() {
	    @Override
	    public void onSuccess(String deviceId) {
	           //gestisce esiti positivi qui
	    }
	    @Override
	    public void onFailure(MFPPushException ex) {
	    //gestisce esiti negativi qui
	    }
	});
```

```
	//Gestisci la notifica quando arriva
	MFPPushNotificationListener notificationListener = new MFPPushNotificationListener() {
	    @Override
	    public void onReceive (final MFPSimplePushNotification message){
	      // Gestisci Push Notification
	    }
	};
```


## Ricezione di notifiche di push su dispositivi Android
{: #android_receive}

Per registrare l'oggetto  notificationListener con push, richiama il metodo **MFPPush.listen()**. Questo metodo viene di norma
                            richiamato dal metodo ** onResume() **dell'attività che
                            sta gestendo le notifiche di push.

1. Per registrare l'oggetto  notificationListener con push, richiama il metodo **listen()**. Questo metodo viene di norma
                            richiamato dal metodo ** onResume() **dell'attività che
                            sta gestendo le notifiche di push.

	```
	@Override
	protected void onResume(){
	   super.onResume();
	   if(push != null) {
	       push.listen(notificationListener);
	   }
	}
  ```
2. Crea il progetto ed eseguilo sul dispositivo o sull'emulatore. Quando viene richiamato il metodo onSuccess() per il listener di risposte nel metodo register(), ricevi una conferma che il dispositivo ha eseguito correttamente la registrazione con il servizio {{site.data.keyword.mobilepushshort}}. In questo momento puoi inviare un messaggio come descritto in Invio di notifiche di push di base.
3. Verifica che i tuoi dispositivi abbiano ricevuto la tua notifica. Se l'applicazione è in
            primo piano, la notifica viene gestita da **MFPPushNotificationListener**. Se l'applicazione è in background, viene visualizzato un messaggio nella barra di notifica.


## Invio di {{site.data.keyword.mobilepushshort}} di base
{: #send}

Dopo che hai sviluppato le tue applicazioni, puoi inviare delle notifiche di push di base (senza utilizzare tag, badge, payload aggiuntivi o file audio).

1. In **Choose the Audience**, seleziona uno dei seguenti destinatari:
      **All Devices**, oppure in base alla piattaforma: **Only iOS devices** o
      **Only Android devices**. 

	**Nota**: quando selezioni l'opzione **All Devices**, tutti i dispositivi che hanno sottoscritto le notifiche di push riceveranno le notifiche.

![Schermata notifiche](images/tag_notification.jpg)

2. In **Create your Notification**, immetti il tuo messaggio e quindi fai clic su **Send**.
3. Verifica che i tuoi dispositivi abbiano ricevuto la tua notifica. Il seguente screenshot mostra una casella di avviso che gestisce una notifica push
in primo piano su un dispositivo Android.

![Notifica push in primo piano su Android](images/Android_Screenshot.jpg)

Il seguente screenshot mostra una notifica push in background per Android.

![Notifica push in background su Android](images/background.jpg)



## Fasi successive
{: #next_steps_tags}

Dopo che hai correttamente configurato le notifiche di base, puoi configurare le notifiche
        basate sulle tag e le opzioni avanzate.

Aggiungi queste funzioni del Servizio push notifications alla tua applicazione.
Per utilizzare le notifiche basate sulle tag, vedi [Notifiche basate sulle tag](c_tag_basednotifications.html).
Per utilizzare le opzioni di notifica avanzate, vedi [Notifiche di push avanzate](t_advance_notifications.html).
