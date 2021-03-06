---

copyright:
  years: 2016

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}

# Erste Schritte mit {{site.data.keyword.mobileanalytics_short}} (Experimentell)  

{: #gettingstartedtemplate}
*Letzte Aktualisierung: 01. August 2016*
{: .last-updated}

Verwenden Sie den {{site.data.keyword.mobileanalytics_full}}-Service zum Messen von Status, Verhalten und Kontext der mobilen Anwendungen, Benutzer und Geräte.
{: shortdesc}

Führen Sie die folgenden Schritte aus, damit der {{site.data.keyword.mobileanalytics_short}}-Service schnell betriebsbereit ist:

1. Nach der [Instanzerstellen](https://console.{DomainName}/docs/services/reqnsi.html#req_instance) des {{site.data.keyword.mobileanalytics_short}}-Service können Sie auf die {{site.data.keyword.mobileanalytics_short}}-Konsole zugreifen; klicken Sie hierzu auf die entsprechende Kachel im Abschnitt 'Services' des {{site.data.keyword.Bluemix}}-Dashboards.

  **Wichtig:** Wenn Sie den neu erstellen Mobile Analytics-Service zum ersten Mal öffnen, wird möglicherweise ein Fenster angezeigt, in dem Sie bestätigen sollen, dass dem Service von {{site.data.keyword.Bluemix_notm}} die erforderlichen Informationen über Sie bereitgestellt werden sollen, damit Ihre Identität vom Service überprüft werden kann. Klicken Sie auf **Bestätigung**, um mit der {{site.data.keyword.mobileanalytics_short}}-Konsole fortzufahren. Wenn Sie abbrechen, wir die {{site.data.keyword.mobileanalytics_short}}-Konsole nicht geöffnet.
2. Installieren Sie die {{site.data.keyword.mobileanalytics_short}} [Client-SDKs](install-client-sdk.html).
3. Importieren Sie die Client-SDKs und initialisieren Sie sie mit dem folgenden Codeausschnitt, um die Nutzungsanalysedaten aufzuzeichnen.
  #### Android
  {: #android-initialize}
  1. Importieren Sie das Client-SDK:
		
		```
		import com.ibm.mobilefirstplatform.clientsdk.android.core.api.*;
		import com.ibm.mobilefirstplatform.clientsdk.android.analytics.api.*;
		```

  2. Rufen Sie den Wert für Ihren [Zugriffsschlüssel](sdk.html#analytics-clientkey) ab.
  3. Initialisieren Sie das Client-SDK im Anwendungscode, um Nutzungsanalysedaten und Anwendungssitzungen aufzuzeichnen.
		
		```Java
		try {
		        BMSClient.getInstance().initialize(this.getApplicationContext(), "", "", BMSClient.REGION_US_SOUTH);
			}
			catch (MalformedURLException e) {
		        Log.e("your_app_name","URL should not be malformed:  " + e.getLocalizedMessage());
		    }
		   Analytics.init(getApplication(), "your_app_name", "your_access_key", Analytics.DeviceEvent.LIFECYCLE);
		```
    Der Parameter **bluemixRegion** gibt an, welche Bluemix-Bereitstellung Sie verwenden, zum Beispiel `BMSClient.REGION_US_SOUTH`, `BMSClient.REGION_UK` oder `BMSClient.REGION_SYDNEY`.

  #### iOS
  {: #ios-initialize}
  1. Importieren Sie die Frameworks `BMSCore` und `BMSAnalytics`:

    ```
    import BMSCore
    import BMSAnalytics
    ```

  2. Rufen Sie den Wert für Ihren [Zugriffsschlüssel](sdk.html#analytics-clientkey) ab.

  3. Initialisieren Sie das Client-SDK im Anwendungscode, um Nutzungsanalysedaten und Anwendungssitzungen aufzuzeichnen.
	
	```Swift
	BMSClient.sharedInstance.initializeWithBluemixAppRoute("nil",bluemixAppGUID: "nil", bluemixRegion: BMSClient.REGION_US_SOUTH) //You can change the region
	Analytics.initializeWithAppName("your_app_name", accessKey: "your_access_key", deviceEvents: DeviceEvent.LIFECYCLE)
	```

    Der Parameter **bluemixRegion** gibt an, welche Bluemix-Bereitstellung Sie verwenden, zum Beispiel `BMSClient.REGION_US_SOUTH`, `BMSClient.REGION_UK` oder `BMSClient.REGION_SYDNEY`.

4. Senden Sie die aufgezeichneten Nutzungsanalysedaten an den Mobile Analytics-Service. Eine Einfache Möglichkeit zum Testen der Analyse ist das Ausführen des folgenden Codes, wenn die Anwendung gestartet wird:

	#### Android
	{: #android-send}
	
	Sie können die Methode `Analytics.send()` in der Methode `onCreate` der Hauptaktivität in der Android-Anwendung oder an einer Position hinzufügen, die für Ihr Projekt am besten geeignet ist.
	
	```
	Analytics.send(new ResponseListener() {
	    @Override
	    public void onSuccess(Response response) {
	        Log.d("your_app_name", "Successfully sent analytics: " + response.toString());
	    }
		
	    @Override
	    public void onFailure(Response response, Throwable throwable, JSONObject jsonObject) {
	        Log.e("your_app_name", "Failed to send analytics: ");
	        if (response != null) {
	            Log.e("your_app_name", response.toString());
	        }
	        if (throwable != null) {
	            Log.e("your_app_name","Stack trace: ", throwable);
	        }
	    }
	});
	```
	
	#### iOS
	{: #ios-send}
	
	
	Verwenden Sie die Methode `Analytics.send` zum Senden der Analysedaten an den Server. Versetzen Sie die Methode `Analytics.send` in die Methode `application(_:didFinishLaunchingWithOptions:)` des Anwendungsdelegierten oder an eine Position, die sich für das Projekt am besten eignet. 
		
	```
	Analytics.send { (response: Response?, error: NSError?) in
	  if response?.statusCode == 201 {
	      print("Successfully sent analytics: \(response?.responseText)")
	  }
	  else {
	      print("Failed to send analytics: \(response?.responseText). Error: \(error?.localizedDescription)")
	  }
	}
	```
Lesen Sie den Abschnitt [Anwendung instrumentieren](sdk.html).
5. Kompilieren Sie die Anwendung und führen Sie sie im Emulator oder einem Gerät aus.

6. Wechseln Sie zum {{site.data.keyword.mobileanalytics_short}} **Dashboard**, um die Analysedaten anzuzeigen, zum Beispiel neue Geräte oder die Gesamtsumme der Geräte, von denen die Anwendung verwendet wird. Sie können die App auch durch <!-- [creating custom charts](app-monitoring.html#custom-charts), --> [Festlegen von Alerts](app-monitoring.html#alerts) und [Überwachen von App-Abstürzen](app-monitoring.html#monitor-app-crash) überwachen. 


# Zugehörige Links

## SDK
* [Android-SDK](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-android-analytics){: new_window}  
* [iOS-SDK](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-analytics){: new_window}
