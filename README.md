# DemoPod
# Applylar Android SDK Integration

Appylar library for Android is a lightweight and easy-to-use Ad integration SDK provided by Appylar. SDK enables developers to integrate Appylar Ads in any type of application.

Appylar provides several types of Ads and enables you to set Ads wherever you want in the application.

The Ads provided by Appylar are like:

 - Banners
 - Insterstitials
 
## General Requirements
 - Targeted versions must be Android 5.0 (API level 21) [Lollipop] and later.
 
# Usage
The installation guide will lead you through the following steps:
 - <a href="https://github.com/5exceptions-rakeshdiwan/mine-personal/new/main?readme=1#appylar-android-sdk">Add Appylar to Gradle</a>
 - <a href="https://github.com/5exceptions-rakeshdiwan/mine-personal/new/main?readme=1#appylar-android-sdk">Setup the SDK configuration</a>
 - <a href="https://github.com/5exceptions-rakeshdiwan/mine-personal/new/main?readme=1#appylar-android-sdk">Add BannerView to the application</a>
 - <a href="https://github.com/5exceptions-rakeshdiwan/mine-personal/new/main?readme=1#appylar-android-sdk">Add Interstitial to the application</a>

# Step 1: Add Appylar to your Gradle
Make sure `MavenCentral` artifact repository is available in your project's root build.gradle (or settings.gradle if you are using settings repositories):
#### `build.gradle`

allprojects {
	repositories {
		...
		mavenCentral()
	}
}

#### `settings.gradle`

pluginManagement {
    repositories {
        ...
        mavenCentral()
    }
}
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        ...
        mavenCentral()
    }
}

Set dependency in your :app module's build.gradle: `[build.gradle (: app)]`
- *Groovy*

dependencies {
	implementation 'com.appylar:android.sdk:#latest-version'
}

      
- *Kotlin*

dependencies {
	implementation("com.appylar:android.sdk:#latest-version")
}      

# Step 2: Setup the configuration for your App and Listeners
1. Create arbitrary subclass of Application(), override it's onCreate() method and implement `Events` interface. You can, of course, use your Application subclass, if you already have one in your project.
#### Kotlin : `[AppylarApplication.kt]`

import com.appylar.android.sdk.Appylar
import com.appylar.android.sdk.interfaces.Events

class AppylarApplication : Application(), Events {

    override fun onCreate() {
        super.onCreate()
	Appylar.setEventListener(this) //attach callback listeners for SDK before initialization
	//Initialization
	...
    }
    
    override fun onInitialized() {
         //Callback for successful initialization
    }

    override fun onError(error: String) {
        //Callback for error thrown by SDK
    }
}

#### Java : `[AppylarApplication.java]`

import com.appylar.android.sdk.Appylar;
import com.appylar.android.sdk.interfaces.Events;

public class AppylarApplication extends Application implements Events {
    @Override
    public void onCreate() {
        super.onCreate();
	Appylar.setEventListener(this); //Attach callback listeners for SDK before initialization
	//Initialization
	...
    }
    @Override
    public void onInitialized() {
	//Callback for successful initialization
    }

    @Override
    public void onError(@NonNull String error) {
	//Callback for error thrown by SDK
    }
}

2. Initialize SDK with configuration:
#### Kotlin - `[AppylarApplication.kt]`

override fun onCreate() {
	super.onCreate()
	Appylar.setEventListener(this) //Attach callback listeners for SDK before initialization
	//Initialization
	 Appylar.init(
                this, //Application context 
                "<YOUR_APP_KEY>", 	//APP KEY provide by console for Development use ["jrctNFE1b-7IqHPShB-gKw"] 
                arrayOf(AdType.BANNER, AdType.INTERSTITIAL), 	//What type of Ads you want to integrate 
                arrayOf(Orientation.PORTRAIT, Orientation.LANDSCAPE), 	//Supported orientations for Ads
                true 	//Test Mode, [TRUE] for development & [FALSE] for production
            )
}

#### Java - `[AppylarApplication.java]`

@Override
public void onCreate() {
	super.onCreate();
	Appylar.setEventListener(this);//Attach callback listeners for SDK before initialization
	//Initialization
	Appylar.init(
		this, //application context 
		"<YOUR_APP_KEY>", 	//APP KEY provide by console for Development use ["jrctNFE1b-7IqHPShB-gKw"]
		new AdType[] {AdType.BANNER, AdType.INTERSTITIAL}, 	//What type Ads you want to integrate 
		new Orientation[] {Orientation.PORTRAIT, Orientation.LANDSCAPE}, 	//Supported orientations for Ads
                true 	//Test Mode, [TRUE] for development & [FALSE] for production
	); 
}

3. There is another method available for parameters customization:
#### Kotlin - `[AppylarApplication.kt]`

override fun onCreate() {
	super.onCreate()
	//Hashmap is required for setParameter(), as mentioned below
	val parameters: MutableMap<String, Array<String>> = HashMap() 
        parameters.put("banner_height", arrayOf("90")) //for customized banner height
        parameters.put("age_restriction", arrayOf("18")) //to apply age restrictions
        Appylar.setParameters(parameters)
}

#### Java - `[AppylarApplication.java]`

@Override
public void onCreate() {
	super.onCreate();
	//Hashmap is required for setParameter(), as mentioned below
	HashMap<String, String[]> parameters = new HashMap();
        parameters.put("banner_height", new String[]{"90"}); //for customized banner height
        parameters.put("age_restriction", new String[]{"18"}); //to apply age restrictions
        Appylar.setParameters(parameters);
}

4. Add this new subclass to AndroidManifest.xml" inside <application> tag: - `[AndroidManifest.xml]`

<application
    android:name=".AppylarApplication"
    ...

# Step 3: Add BannerView to the application
1. To integrate the BannerView component in your design, prefer below snippet:

<com.appylar.android.sdk.bannerview.BannerView
	android:id="@+id/bannerView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
	  
2. Bind component to your activity and implement callback listeners.
###### Kotlin

import com.appylar.android.sdk.bannerview.BannerView

class KotlinActivity : AppCompatActivity() : BannerViewListener {
    private lateinit var bannerView: BannerView
    
    	override fun onCreate(savedInstanceState: Bundle?) {
        	super.onCreate(savedInstanceState)
        	setContentView(R.layout.activity_kotlin)
        	bannerView = findViewById(R.id.bannerView)
		bannerView.setEventListener(this) //Attach Event Listeners
    	}
	
	override fun onNoBanner() {
		//Callback if there are no banners available to show
	}

	override fun onBannerShown() {
		//Callback for Banner shown success
	}
}

###### Java

import com.appylar.android.sdk.bannerview.BannerView;

public class JavaActivity extends AppCompatActivity implements BannerViewListener {
	private BannerView bannerView; 
    
    	@Override
    	protected void onCreate(Bundle savedInstanceState) {
        	super.onCreate(savedInstanceState);
        	setContentView(R.layout.activity_java);
  		bannerView = findViewById(R.id.bannerView);
		bannerView.setEventListener(this); //Attach Event Listeners
    	}
	
	@Override
        public void onNoBanner() {
		//Callback if there are no banners available to show
	}
	
	@Override
        public void onBannerShown() {
		//Callback for Banner shown success
	}
}
	

3. Check Ad availablity and show the Ad.
###### Kotlin

bannerView.setEventListener(this) //Attach Event Listeners
if(bannerView.canShowAd()) {
	//BannerView to show Ad on the UI with optional customized placement
	//Leave blank if not required
	bannerView.showAd("<PLACEMENT_STRING>") //Placement string is an optional parameter
}


###### Java

bannerView.setEventListener(this); //Attach Event Listeners
if(bannerView.canShowAd()) { //Check Ad availability
	//BannerView to show Ad on the UI with optional customized placement
	//Leave blank if not required
	bannerView.showAd("<PLACEMENT_STRING>"); //Placement string is an optional parameter
}

	
4. For hiding the banner at the run time.
###### Kotlin

	bannerView.hideBanner()	

###### Java

	bannerView.hideBanner();


# Step 4: Add Interstitial to the application
1. Implement callback listeners for Interstitial.
###### Kotlin

import com.appylar.android.sdk.interstitial.Interstitial

class KotlinActivity : AppCompatActivity() : InterstitialListener {
    	override fun onCreate(savedInstanceState: Bundle?) {
        	super.onCreate(savedInstanceState)
        	setContentView(R.layout.activity_kotlin)
	
		Interstitial.setEventListener(this) //Attach Event Listeners
    	}
	
	override fun onNoInterstitial() {
        	//Callback if there are no interstitials available to show
	}

	override fun onInterstitialShown() {
		//Callback for interstitial shown success
	}
	
	override fun onInterstitialClosed() {
		//Callback for close event of interstitial
	}
}

###### Java

import com.appylar.android.sdk.interstitial.Interstitial;

public class JavaActivity extends AppCompatActivity implements InterstitialListener {
    
    	@Override
    	protected void onCreate(Bundle savedInstanceState) {
        	super.onCreate(savedInstanceState);
        	setContentView(R.layout.activity_java);
	
  		Interstitial.setEventListener(this); //Attach Event Listeners
    	}
	
	@Override
        public void onNoInterstitial() {
        	//Callback if there are no interstitials available to show
	}
	
	@Override
        public void onInterstitialShown() {
		//Callback for interstitial shown success
	}
	
	@Override
        public void onInterstitialClosed() {
		//Callback for close event of interstitial
	}
}
	
3. Check Ad availablity and show the Ad.
###### Kotlin

Interstitial.setEventListener(this) //Attach Event Listeners
if(Interstitial.canShowAd()) {
	//Interstitial to show Ad on the UI with context of activity and optional customized placement
	//Leave blank if not required
	Interstitial.showAd(this, "<PLACEMENT_STRING>") //Placement string is an optional parameter	
}

###### Java

Interstitial.setEventListener(this); //Attach Event Listeners
if(Interstitial.canShowAd()) { //Check Ad availability
	//Interstitial to show Ad on the UI with context of activity and optional customized placement
	//Leave blank if not required
	Interstitial.showAd(this, "<PLACEMENT_STRING>"); //Placement string is an optional parameter	
}


# Sample Codes
1. For initialization
	
#### Kotlin - [AppylarApplication.kt]

import android.app.Application
import android.util.Log
import com.appylar.android.sdk.Appylar
import com.appylar.android.sdk.enums.AdType
import com.appylar.android.sdk.enums.Orientation
import com.appylar.android.sdk.interfaces.Events

class AppylarApplication : Application(), Events {
    private val TAG = "AppylarApplication"

    override fun onCreate() {
        super.onCreate()
        Appylar.setEventListener(this) //Attach callback listeners for SDK before initialization
        Appylar.init(
            this, //Application context
            "jrctNFE1b-7IqHPShB-gKw",    //APP KEY provide by console for Development use ["jrctNFE1b-7IqHPShB-gKw"]
            arrayOf(AdType.BANNER, AdType.INTERSTITIAL),    //What type of Ads you want to integrate
            arrayOf(
                Orientation.PORTRAIT,
                Orientation.LANDSCAPE
            ),    //Supported orientations for Ads
            true    //Test Mode, [TRUE] for development & [FALSE] for production
        )
    }

    override fun onError(error: String) {
        Log.d(TAG, "onError: $error")
    }

    override fun onInitialized() {
        Log.d(TAG, "onInitialized: called")
    }
}

	
#### Java - [AppylarApplication.java]

import android.app.Application;
import android.util.Log;

import androidx.annotation.NonNull;

import com.appylar.android.sdk.Appylar;
import com.appylar.android.sdk.enums.AdType;
import com.appylar.android.sdk.enums.Orientation;
import com.appylar.android.sdk.interfaces.Events;

public class AppylarApplication extends Application implements Events {
    private static final String TAG = "AppylarApplication";

    @Override
    public void onCreate() {
        super.onCreate();
        Appylar.setEventListener(this); //Attach callback listeners for SDK before initialization
        Appylar.init(
                this, //application context
                "jrctNFE1b-7IqHPShB-gKw",    //APP KEY provide by console for Development use ["jrctNFE1b-7IqHPShB-gKw"]
                new AdType[]{AdType.BANNER, AdType.INTERSTITIAL},    //What type Ads you want to integrate
                new Orientation[]{Orientation.PORTRAIT, Orientation.LANDSCAPE},    //Supported orientations for Ads
                true    //Test Mode, [TRUE] for development & [FALSE] for production
        );
    }

    @Override
    public void onError(@NonNull String s) {
        Log.d(TAG, "onError: " + s);
    }

    @Override
    public void onInitialized() {
        Log.d(TAG, "onInitialized: called");
    }
}


			
2. Design file - [activity_main.xml]

<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <com.appylar.android.sdk.bannerview.BannerView
        android:id="@+id/bannerView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:layout_margin="16dp"
        android:orientation="vertical">

        <Button
            android:id="@+id/btnShowBanner"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Show Banner" />

        <Button
            android:id="@+id/btnHideBanner"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Hide Banner" />

        <Button
            android:id="@+id/btnShowInterstitial"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Show Interstitial" />
    </LinearLayout>

</RelativeLayout>

			
3. Implementation for both type of ads
	
###### Kotlin

import android.os.Bundle
import android.util.Log
import android.widget.Button
import androidx.appcompat.app.AppCompatActivity
import com.appylar.android.sdk.bannerview.BannerView
import com.appylar.android.sdk.bannerview.BannerViewListener
import com.appylar.android.sdk.interstitial.Interstitial
import com.appylar.android.sdk.interstitial.InterstitialListener

class KotlinActivity : AppCompatActivity(), BannerViewListener, InterstitialListener {
    private val TAG = "KotlinActivity"
    private lateinit var bannerView: BannerView
    private lateinit var btnShowBanner: Button
    private lateinit var btnHideBanner: Button
    private lateinit var btnShowInterstitial: Button

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        bannerView = findViewById(R.id.bannerView)
        btnShowBanner = findViewById(R.id.btnShowBanner)
        btnHideBanner = findViewById(R.id.btnHideBanner)
        btnShowInterstitial = findViewById(R.id.btnShowInterstitial)

        bannerView.setEventListener(this) //Attach Banner Event Listeners
        btnShowBanner.setOnClickListener {
            if (bannerView.canShowAd()) {
                bannerView.showAd("")
            }
        }
        btnHideBanner.setOnClickListener {
            bannerView.showAd("")
        }

        Interstitial.setEventListener(this) //Attach Interstitial Event Listeners
        btnShowInterstitial.setOnClickListener {
            if (Interstitial.canShowAd()) {
                Interstitial.showAd(this, "")
            }
        }
    }
    
    override fun onBannerShown() {
        Log.d(TAG, "onBannerShown: called")
    }

    override fun onNoBanner() {
        Log.d(TAG, "onNoBanner: called")
    }

    override fun onInterstitialShown() {
        Log.d(TAG, "onInterstitialShown: called")
    }

    override fun onInterstitialClosed() {
        Log.d(TAG, "onInterstitialClosed: called")
    }

    override fun onNoInterstitial() {
        Log.d(TAG, "onNoInterstitial: called")
    }
}


###### Java

import android.os.Bundle;
import android.util.Log;
import android.widget.Button;

import androidx.appcompat.app.AppCompatActivity;

import com.appylar.android.sdk.bannerview.BannerView;
import com.appylar.android.sdk.bannerview.BannerViewListener;
import com.appylar.android.sdk.interstitial.Interstitial;
import com.appylar.android.sdk.interstitial.InterstitialListener;

public class JavaActivity extends AppCompatActivity implements BannerViewListener, InterstitialListener {
    private static final String TAG = "JavaActivity";
    private BannerView bannerView;
    private Button btnShowBanner;
    private Button btnHideBanner;
    private Button btnShowInterstitial;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        bannerView = findViewById(R.id.bannerView);

        btnShowBanner = findViewById(R.id.btnShowBanner);
        btnHideBanner = findViewById(R.id.btnHideBanner);
        btnShowInterstitial = findViewById(R.id.btnShowInterstitial);

        bannerView.setEventListener(this); //Attach Banner Event Listeners
        btnShowBanner.setOnClickListener(v -> {
            if (bannerView.canShowAd()) {
                bannerView.showAd("");
            }
        });
        btnHideBanner.setOnClickListener(v -> {
            bannerView.hideBanner();
        });

        Interstitial.setEventListener(this); //Attach Interstitial Event Listeners
        btnShowInterstitial.setOnClickListener(v -> {
            if (Interstitial.canShowAd()) {
                Interstitial.showAd(this, "");
            }
        });
    }

    @Override
    public void onBannerShown() {
        Log.d(TAG, "onBannerShown: called");
    }

    @Override
    public void onNoBanner() {
        Log.d(TAG, "onNoBanner: called");
    }
    
    @Override
    public void onInterstitialShown() {
        Log.d(TAG, "onInterstitialShown: called");
    }

    @Override
    public void onNoInterstitial() {
        Log.d(TAG, "onNoInterstitial: called");
    }

    @Override
    public void onInterstitialClosed() {
        Log.d(TAG, "onInterstitialClosed: called");
    }
}
	
