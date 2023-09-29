# Table of contents

* [Android _PhotoPay_ integration instructions](#intro)
* [Quick Start](#quick-start)
    * [Quick start with the sample app](#quick-demo)
    * [SDK integration](#android-studio-integration)
* [Device requirements](#supportCheck)
* [_PhotoPay_ SDK integration levels](#ui-customizations)
    * [Built-in activities (`UISettings`)](#run-builtin-activity)
    * [Built-in fragment (`RecognizerRunnerFragment`)](#recognizer-runner-fragment)
    * [Custom UX with `RecognizerRunnerView`](#recognizer-runner-view)
    * [Direct API](#direct-api)
        * [Using Direct API for recognition of Android Bitmaps and custom camera frames](#direct-api-images)
        * [Using Direct API for `String` recognition (parsing)](#direct-api-strings)
        * [Understanding DirectAPI's state machine](#direct-api-state-machine)
        * [Using Direct API while RecognizerRunnerView is active](#direct-api-with-recognizer)
* [Available activities and overlays](#built-in-ui-components)
    * [`FieldByFieldUISettings` and `FieldByFieldOverlayController`](#fieldByFieldUiComponent)
    * [Translation and localization](#translation)
* [Handling processing events with `RecognizerRunner` and `RecognizerRunnerView`](#processing-events)
* [`Recognizer` concept and `RecognizerBundle`](#available-recognizers)
    * [The `Recognizer` concept](#recognizer-concept)
    * [`RecognizerBundle`](#recognizer-bundle)
        * [Passing `Recognizer` objects between activities](#intent-optimization)
* [List of available recognizers](#recognizer-list)
    * [Frame Grabber Recognizer](#frame-grabber-recognizer)
    * [Success Frame Grabber Recognizer](#success-frame-grabber-recognizer)
    * [BlinkInput recognizer](#blinkInputRecognizer)
* [`Field by field` scanning feature](#fieldByFieldFeature)
* [`Processor` and `Parser`](#processorsAndParsers)
    * [The `Processor` concept](#processorConcept)
    * [List of available processors](#processorList)
        * [Parser Group Processor](#parserGroupProcessor)
    * [The `Parser` concept](#parserConcept)
    * [List of available parsers](#parserList)
        * [Amount Parser](#amountParser)
        * [Date Parser](#dateParser)
        * [IBAN Parser](#ibanParser)
        * [Raw Parser](#rawParser)
    * [Barcode recognizer](#barcodeRecognizer)
* [Embedding _PhotoPay_ inside another SDK](#embed-aar)
* [Processor architecture considerations](#archConsider)
    * [Reducing the final size of your app](#reduceSize)
        * [Consequences of removing processor architecture](#archConsequences)
    * [Combining _PhotoPay_ with other native libraries](#combineNativeLibraries)
* [Troubleshooting](#troubleshoot)
* [FAQ and known issues](#faq)
* [Additional info](#info)
    * [PhotoPay SDK size](#size-report)
    * [API reference](#api-reference)
    * [Contact](#contact)

# <a name="intro"></a> Android _PhotoPay_ integration instructions

The package contains Android Archive (AAR) that contains everything you need to use _PhotoPay_ library. Besides AAR, package also contains a sample project that contains following modules:

- _PhotoPay-aMinimalSample_ demonstrates quick and simple integration of _PhotoPay_ library
- _PhotoPay-AllRecognizersSample_ demonstrates integration of almost all available features. This sample application is best for performing a quick test of supported features.
- _PhotoPay-CustomFieldByFieldScanSample_ demonstrates advanced integration of Field by Field feature within custom scan activity. It shows how to create a custom scan activity for scanning little text fields.
- _PhotoPay-CustomUISample_ demonstrates advanced integration within custom scan activity.
- _PhotoPay-DirectApiSample_ demonstrates how to perform scanning of [Android Bitmaps](https://developer.android.com/reference/android/graphics/Bitmap.html)
 
The source code of all sample apps is given to you to show you how to perform integration of _PhotoPay_ SDK into your app. You can use this source code and all resources as you wish. You can use sample apps as a basis for creating your own app, or you can copy/paste the code and/or resources from sample apps into your app and use them as you wish without even asking us for permission.

_PhotoPay_ is supported on Android SDK version 19 (Android 4.4) or later.

The list of all provided scan activities can be found in the [Built-in activities and overlays](#builtInUIComponents) section.

You can also create your own scanning UI - you just need to embed `RecognizerRunnerView` into your activity and pass activity's lifecycle events to it and it will control the camera and recognition process. For more information, see [Embedding `RecognizerRunnerView` into custom scan activity](#recognizerRunnerView).

# <a name="quick-start"></a> Quick Start

## <a name="quick-demo"></a> Quick start with the sample app

1. Open Android Studio.
2. In Quick Start dialog choose _Import project (Eclipse ADT, Gradle, etc.)_.
3. In File dialog select _PhotopaySample_ folder.
4. Wait for the project to load. If Android studio asks you to reload project on startup, select `Yes`.


## <a name="android-studio-integration"></a> SDK integration
#### Adding _PhotoPay_ dependency

1. Create a `libs` folder in your Android Studio project.
2. Download the _LibPhotoPay.aar_ file and move it to the created `libs` folder.
3. In your app's `build.gradle`, add a dependency to `LibPhotoPay` (by using a relative path to the _LibPhotoPay.aar_ file) and appcompat:

    ```
    dependencies {
        implementation files('../libs/LibPhotoPay.aar')
        implementation "androidx.appcompat:appcompat:1.4.0"
    }
    ```
    
#### Importing Javadoc

1. In Android Studio project sidebar, ensure [project view is enabled](https://developer.android.com/sdk/installing/studio-androidview.html)
2. Expand `External Libraries` entry (usually this is the last entry in project view)
3. Locate `LibPhotoPay-unspecified` entry, right click on it and select `Library Properties...`
4. A `Library Properties` pop-up window will appear
5. Click the `+` button in bottom left corner of the window
6. Window for choosing JAR file will appear
7. Find and select `LibPhotoPay-javadoc.jar` file which is located in root folder of the SDK distribution
8. Click `OK`


#### Performing your first scan
1. A valid license key is required to initialize scanning. You can request a free trial license key, after you register, at [Microblink Developer Hub](https://account.microblink.com/signin). License is bound to [package name](https://developer.android.com/studio/build/configure-app-module#set-application-id) of your app, so please make sure you enter the correct package name when asked. 

    Download your licence file and put it in your application's _assets_ folder. Make sure to set the license key before using any other classes from the SDK, otherwise you will get a runtime exception. 
    
    We recommend that you extend [Android Application class](https://developer.android.com/reference/android/app/Application.html) and set the license in [onCreate callback](https://developer.android.com/reference/android/app/Application.html#onCreate()) like this:

    ```java
    public class MyApplication extends Application {
        @Override
        public void onCreate() {
            MicroblinkSDK.setLicenseFile("path/to/license/file/within/assets/dir", this);
        }
    }
    ```

2. In your main activity, create recognizer objects that will perform image recognition, configure them and put them into [RecognizerBundle object](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/RecognizerBundle.html). You can see more information about available recognizers and `RecognizerBundle` [here](#availableRecognizers). 

    For example, to scan Croatian slip, configure your recognizer like this:

    ```java
    public class MyActivity extends Activity {
        private CroatiaSlipRecognizer mRecognizer;
        private RecognizerBundle mRecognizerBundle;
        
        @Override
        protected void onCreate(Bundle bundle) {
            super.onCreate(bundle);
            
            // setup views, as you would normally do in onCreate callback
            
            // create CroatiaSlipRecognizer
            mRecognizer = new CroatiaSlipRecognizer();
            
            // bundle recognizers into RecognizerBundle
            mRecognizerBundle = new RecognizerBundle(mRecognizer);
        }
    }
    ```

3. Start recognition process by creating `PhotopayUISettings` and calling [`ActivityRunner.startActivityForResult`](https://photopay.github.io/photopay-android/com/microblink/uisettings/ActivityRunner.html#startActivityForResult-android.app.Activity-int-com.microblink.uisettings.UISettings-):
    
    ```java
    // method within MyActivity from previous step
    public void startScanning() {
        // Settings for PhotopayActivity
        PhotopayUISettings settings = new PhotopayUISettings(mRecognizerBundle);
        
        // tweak settings as you wish
        
        // Start activity
        ActivityRunner.startActivityForResult(this, MY_REQUEST_CODE, settings);
    }
    ```
    
4. `onActivityResult` will be called in your activity after scanning is finished, here you can get the scanning results.

    ```java
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        
        if (requestCode == MY_REQUEST_CODE) {
            if (resultCode == Activity.RESULT_OK && data != null) {
                // load the data into all recognizers bundled within your RecognizerBundle
                mRecognizerBundle.loadFromIntent(data);
                
                // now every recognizer object that was bundled within RecognizerBundle
                // has been updated with results obtained during scanning session
                
                // you can get the result by invoking getResult on recognizer
                CroatiaSlipRecognizer.Result result = mRecognizer.getResult();
                if (result.getResultState() == Recognizer.Result.State.Valid) {
                    // result is valid, you can use it however you wish
                }
            }
        }
    }
    ```
    
    For more information about available recognizers and `RecognizerBundle`, see [RecognizerBundle and available recognizers](#availableRecognizers).

# <a name="supportCheck"></a> Device requirements

### Android Version

_PhotoPay_ requires Android API level **21** or newer. For best performance and compatibility, we recommend at least Android 5.0.

### Camera

Camera video preview resolution also matters. In order to perform successful scans, camera preview resolution must be at least 720p. Note that camera preview resolution is not the same as video recording resolution.

### Processor architecture

_PhotoPay_ is distributed with **ARMv7**, **ARM64**, **x86** and **x86_64** native library binaries.

_PhotoPay_ is a native library, written in C++ and available for multiple platforms. Because of this, _PhotoPay_ cannot work on devices with obscure hardware architectures. We have compiled _PhotoPay_ native code only for the most popular Android [ABIs](https://en.wikipedia.org/wiki/Application_binary_interface).

Even before setting the license key, you should check if the _PhotoPay_ is supported on the current device (see next section: *Compatibility check*). Attempting to call any method from the SDK that relies on native code, such as license check, on a device with unsupported CPU architecture will crash your app.

If you are combining _PhotoPay_ library with other libraries that contain native code into your application, make sure you match the architectures of all native libraries.

For example, if a third party library has got only ARMv7 and ARM64 versions, you must use exactly ARMv7 and ARM64 versions of _PhotoPay_ with that library, but not x86. Using these architectures will crash your app at the initialization step because JVM will try to load all its native dependencies in the same preferred architecture and will fail with `UnsatisfiedLinkError`. 

For more information, see [Processor architecture considerations](#archConsider) section.

### Compatibility check

Here's how you can check whether the _PhotoPay_ is supported on the device:

```java
// check if PhotoPay is supported on the device,
RecognizerCompatibilityStatus status = RecognizerCompatibility.getRecognizerCompatibilityStatus(this);
if (status == RecognizerCompatibilityStatus.RECOGNIZER_SUPPORTED) {
    Toast.makeText(this, "PhotoPay is supported!", Toast.LENGTH_LONG).show();
} else if (status == RecognizerCompatibilityStatus.NO_CAMERA) {
    Toast.makeText(this, "PhotoPay is supported only via Direct API!", Toast.LENGTH_LONG).show();
} else if (status == RecognizerCompatibilityStatus.PROCESSOR_ARCHITECTURE_NOT_SUPPORTED) {
    Toast.makeText(this, "PhotoPay is not supported on current processor architecture!", Toast.LENGTH_LONG).show();
} else {
    Toast.makeText(this, "PhotoPay is not supported! Reason: " + status.name(), Toast.LENGTH_LONG).show();
}
```

##### Kotlin
```kotlin
// check if _PhotoPay_ is supported on the device,
val status = RecognizerCompatibility.getRecognizerCompatibilityStatus(this)  
if (status == RecognizerCompatibilityStatus.RECOGNIZER_SUPPORTED) {  
  Toast.makeText(this, "_PhotoPay_ is supported!", Toast.LENGTH_LONG).show()  
} else if (status == RecognizerCompatibilityStatus.NO_CAMERA) {  
	  Toast.makeText(this, "_PhotoPay_ is supported only via Direct API!", 
		  Toast.LENGTH_LONG).show()  
} else if (status == RecognizerCompatibilityStatus.PROCESSOR_ARCHITECTURE_NOT_SUPPORTED) {  
	  Toast.makeText(this,"_PhotoPay_ is not supported on current processor architecture!",
		  Toast.LENGTH_LONG).show()  
} else {  
	  Toast.makeText(this,"_PhotoPay_ is not supported! Reason: " + status.name,  
	      Toast.LENGTH_LONG).show()
}
```
Some recognizers require camera with autofocus. If you try using them on a device that doesn't support autofocus, you will get an error. To prevent that, you can check whether a recognizer requires autofocus by calling its [requiresAutofocus](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/Recognizer.html#requiresAutofocus--) method.

If you already have an array of recognizers, you can easily filter out recognizers that require autofocus from array using the following code snippet:

##### Java

```java
Recognizer[] recArray = ...;
if(!RecognizerCompatibility.cameraHasAutofocus(CameraType.CAMERA_BACKFACE, this)) {
    recArray = RecognizerUtils.filterOutRecognizersThatRequireAutofocus(recArray);
}
```

##### Kotlin
```kotlin
var recArray: Array<Recognizer> = ...
if(!RecognizerCompatibility.cameraHasAutofocus(CameraType.CAMERA_BACKFACE, this)) {
    recArray = RecognizerUtils.filterOutRecognizersThatRequireAutofocus(recArray)
}
```

# <a name="ui-customizations"></a> _PhotoPay_ SDK integration levels

You can integrate _PhotoPay_ into your app in four different ways, depending on your use case and customisation needs:

1. Built-in activities (`UISettings`) - SDK handles everything and you just need to start our built-in activity and handle result, customisation options are limited
2. Built-in fragment (`RecognizerRunnerFragment`) - reuse scanning UX from our built-in activities in your own activity
3. Custom UX (`RecognizerRunnerView`) - SDK handles camera management while you have to implement completely custom scanning UX
4. Direct Api (`RecognizerRunner`) - SKD only handles recognition while you have to provide it with the images, either from camera or from a file

## <a name="run-builtin-activity"></a> Built-in activities (`UISettings`)

`UISettings` is a class that contains all the necessary settings for SDK's built-in scan activities. It configures scanning activity behaviour, strings, icons and other UI elements. 
As shown in the first scan example, you should use [`ActivityRunner `](https://photopay.github.io/photopay-android/com/microblink/uisettings/ActivityRunner.html) to start the scan activity configured by `UISettings`.

We provide multiple `UISettings` classes specialised for different scanning scenarios. Each `UISettings` object has properties which can be changed via appropriate setter methods. For example, you can customise camera settings with `setCameraSettings` metod. 

All available `UISettings` classes are listed [here](#built-in-ui-components).

## <a name="recognizer-runner-fragment"></a> Built-in fragment (`RecognizerRunnerFragment`)

If you want to reuse our built-in activity UX inside your own activity, use [`RecognizerRunnerFragment`](https://photopay.github.io/photopay-android/com/microblink/fragment/RecognizerRunnerFragment.html). Activity that will host `RecognizerRunnerFragment` must implement [`ScanningOverlayBinder`](https://photopay.github.io/photopay-android/com/microblink/fragment/RecognizerRunnerFragment.ScanningOverlayBinder.html) interface. Attempting to add `RecognizerRunnerFragment` to activity that does not implement that interface will result in `ClassCastException`.

The `ScanningOverlayBinder` is responsible for returning `non-null` implementation of [`ScanningOverlay`](https://photopay.github.io/photopay-android/com/microblink/fragment/overlay/ScanningOverlay.html) - class that will manage UI on top of `RecognizerRunnerFragment`. It is not recommended to create your own `ScanningOverlay` implementation, use one of our implementations listed [here](#built-in-ui-components) instead.

Here is the minimum example for activity that hosts the `RecognizerRunnerFragment`:

```java
public class MyActivity extends AppCompatActivity implements RecognizerRunnerFragment.ScanningOverlayBinder {
    private CroatiaSlipRecognizer mRecognizer;
    private RecognizerBundle mRecognizerBundle;
    private BasicOverlayController mScanOverlay;
    private RecognizerRunnerFragment mRecognizerRunnerFragment;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate();
        setContentView(R.layout.activity_my_activity);
        mScanOverlay = createOverlay();
        if (null == savedInstanceState) {
            // create fragment transaction to replace R.id.recognizer_runner_view_container with RecognizerRunnerFragment
            mRecognizerRunnerFragment = new RecognizerRunnerFragment();
            FragmentTransaction fragmentTransaction = getSupportFragmentManager().beginTransaction();
            fragmentTransaction.replace(R.id.recognizer_runner_view_container, mRecognizerRunnerFragment);
            fragmentTransaction.commit();
        } else {
            // obtain reference to fragment restored by Android within super.onCreate() call
            mRecognizerRunnerFragment = (RecognizerRunnerFragment) getSupportFragmentManager().findFragmentById(R.id.recognizer_runner_view_container);
        }
    }

    @Override
    @NonNull
    public ScanningOverlay getScanningOverlay() {
        return mScanOverlay;
    }

    private BasicOverlayController createOverlay() {
        // create CroatiaSlipRecognizer
        mRecognizer = new CroatiaSlipRecognizer();

        // bundle recognizers into RecognizerBundle
        mRecognizerBundle = new RecognizerBundle(mRecognizer);

        PhotopayUISettings settings = new PhotopayUISettings(mRecognizerBundle);

        return settings.createOverlayController(this, mScanResultListener);
    }

    private final ScanResultListener mScanResultListener = new ScanResultListener() {
        @Override
        public void onScanningDone(@NonNull RecognitionSuccessType recognitionSuccessType) {
            // pause scanning to prevent new results while fragment is being removed
            mRecognizerRunnerFragment.getRecognizerRunnerView().pauseScanning();

            // now you can remove the RecognizerRunnerFragment with new fragment transaction
            // and use result within mRecognizer safely without the need for making a copy of it

            // if not paused, as soon as this method ends, RecognizerRunnerFragments continues
            // scanning. Note that this can happen even if you created fragment transaction for
            // removal of RecognizerRunnerFragment - in the time between end of this method
            // and beginning of execution of the transaction. So to ensure result within mRecognizer
            // does not get mutated, ensure calling pauseScanning() as shown above.
        }
        @Override
        public void onUnrecoverableError(@NonNull Throwable throwable) {
        }
    };
    
}
```
Please refer to sample apps provided with the SDK for more detailed example and make sure your host activity's orientation is set to `nosensor` or has configuration changing enabled (i.e. is not restarted when configuration change happens). For more information, check [scan orientation section](#scan-orientation).

## <a name="recognizer-runner-view"></a> Custom UX with `RecognizerRunnerView`
This section discusses how to embed [RecognizerRunnerView](https://photopay.github.io/photopay-android/com/microblink/view/recognition/RecognizerRunnerView.html) into your scan activity and perform scan.

1. First make sure that `RecognizerRunnerView` is a member field in your activity. This is required because you will need to pass all activity's lifecycle events to `RecognizerRunnerView`.
2. It is recommended to keep your scan activity in one orientation, such as `portrait` or `landscape`. Setting `sensor` as scan activity's orientation will trigger full restart of activity whenever device orientation changes. This will provide very poor user experience because both camera and _PhotoPay_ native library will have to be restarted every time. There are measures against this behaviour that are discussed [later](#scan-orientation).
3. In your activity's `onCreate` method, create a new `RecognizerRunnerView`, set [RecognizerBundle](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/RecognizerBundle.html) containing recognizers that will be used by the view, define [CameraEventsListener](https://photopay.github.io/photopay-android/com/microblink/view/CameraEventsListener.html) that will handle mandatory camera events, define [ScanResultListener](https://photopay.github.io/photopay-android/com/microblink/view/recognition/ScanResultListener.html) that will receive call when recognition has been completed and then call its `create` method. After that, add your views that should be layouted on top of camera view.
4. Pass in your activity's lifecycle using `setLifecycle` method to enable automatic handling of lifeceycle events.
 
Here is the minimum example of integration of `RecognizerRunnerView` as the only view in your activity:

```java
public class MyScanActivity extends AppCompatActivity {
    private static final int PERMISSION_CAMERA_REQUEST_CODE = 42;
    private RecognizerRunnerView mRecognizerRunnerView;
    private CroatiaSlipRecognizer mRecognizer;
    private RecognizerBundle mRecognizerBundle;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // create CroatiaSlipRecognizer
        mRecognizer = new CroatiaSlipRecognizer();

        // bundle recognizers into RecognizerBundle
        mRecognizerBundle = new RecognizerBundle(mRecognizer);
        // create RecognizerRunnerView
        mRecognizerRunnerView = new RecognizerRunnerView(this);
        
        // set lifecycle to automatically call recognizer runner view lifecycle methods
        mRecognizerRunnerView.setLifecycle(getLifecycle());

        // associate RecognizerBundle with RecognizerRunnerView
        mRecognizerRunnerView.setRecognizerBundle(mRecognizerBundle);

        // scan result listener will be notified when scanning is complete
        mRecognizerRunnerView.setScanResultListener(mScanResultListener);
        // camera events listener will be notified about camera lifecycle and errors
        mRecognizerRunnerView.setCameraEventsListener(mCameraEventsListener);

        setContentView(mRecognizerRunnerView);
    }

    @Override
    public void onConfigurationChanged(Configuration newConfig) {
        super.onConfigurationChanged(newConfig);
        // changeConfiguration is not handled by lifecycle events so call it manually
        mRecognizerRunnerView.changeConfiguration(newConfig);
    }

    private final CameraEventsListener mCameraEventsListener = new CameraEventsListener() {
        @Override
        public void onCameraPreviewStarted() {
            // this method is from CameraEventsListener and will be called when camera preview starts
        }

        @Override
        public void onCameraPreviewStopped() {
            // this method is from CameraEventsListener and will be called when camera preview stops
        }

        @Override
        public void onError(Throwable exc) {
            /**
             * This method is from CameraEventsListener and will be called when
             * opening of camera resulted in exception or recognition process
             * encountered an error. The error details will be given in exc
             * parameter.
             */
        }

        @Override
        @TargetApi(23)
        public void onCameraPermissionDenied() {
            /**
             * Called in Android 6.0 and newer if camera permission is not given
             * by user. You should request permission from user to access camera.
             */
            requestPermissions(new String[]{Manifest.permission.CAMERA}, PERMISSION_CAMERA_REQUEST_CODE);
            /**
             * Please note that user might have not given permission to use
             * camera. In that case, you have to explain to user that without
             * camera permissions scanning will not work.
             * For more information about requesting permissions at runtime, check
             * this article:
             * https://developer.android.com/training/permissions/requesting.html
             */
        }

        @Override
        public void onAutofocusFailed() {
            /**
             * This method is from CameraEventsListener will be called when camera focusing has failed.
             * Camera manager usually tries different focusing strategies and this method is called when all
             * those strategies fail to indicate that either object on which camera is being focused is too
             * close or ambient light conditions are poor.
             */
        }

        @Override
        public void onAutofocusStarted(Rect[] areas) {
            /**
             * This method is from CameraEventsListener and will be called when camera focusing has started.
             * You can utilize this method to draw focusing animation on UI.
             * Areas parameter is array of rectangles where focus is being measured.
             * It can be null on devices that do not support fine-grained camera control.
             */
        }

        @Override
        public void onAutofocusStopped(Rect[] areas) {
            /**
             * This method is from CameraEventsListener and will be called when camera focusing has stopped.
             * You can utilize this method to remove focusing animation on UI.
             * Areas parameter is array of rectangles where focus is being measured.
             * It can be null on devices that do not support fine-grained camera control.
             */
        }
    };
    
    private final ScanResultListener mScanResultListener = new ScanResultListener() {
        @Override
        public void onScanningDone(@NonNull RecognitionSuccessType recognitionSuccessType) {
            // this method is from ScanResultListener and will be called when scanning completes
            // you can obtain scanning result by calling getResult on each
            // recognizer that you bundled into RecognizerBundle.
            // for example:

            CroatiaSlipRecognizer.Result result = mRecognizer.getResult();
            if (result.getResultState() == Recognizer.Result.State.Valid) {
                // result is valid, you can use it however you wish
            }

            // Note that mRecognizer is stateful object and that as soon as
            // scanning either resumes or its state is reset
            // the result object within mRecognizer will be changed. If you
            // need to create a immutable copy of the result, you can do that
            // by calling clone() on it, for example:

            CroatiaSlipRecognizer.Result immutableCopy = result.clone();

            // After this method ends, scanning will be resumed and recognition
            // state will be retained. If you want to prevent that, then
            // you should call:
            mRecognizerRunnerView.resetRecognitionState();
            // Note that reseting recognition state will clear internal result
            // objects of all recognizers that are bundled in RecognizerBundle
            // associated with RecognizerRunnerView.

            // If you want to pause scanning to prevent receiving recognition
            // results or mutating result, you should call:
            mRecognizerRunnerView.pauseScanning();
            // if scanning is paused at the end of this method, it is guaranteed
            // that result within mRecognizer will not be mutated, therefore you
            // can avoid creating a copy as described above

            // After scanning is paused, you will have to resume it with:
            mRecognizerRunnerView.resumeScanning(true);
            // boolean in resumeScanning method indicates whether recognition
            // state should be automatically reset when resuming scanning - this
            // includes clearing result of mRecognizer
        }
    };
    
}
```
#### <a name="scan-orientation"></a> Scan activity's orientation

If activity's `screenOrientation` property in `AndroidManifest.xml` is set to `sensor`, `fullSensor` or similar, activity will be restarted every time device changes orientation from portrait to landscape and vice versa. While restarting activity, its `onPause`, `onStop` and `onDestroy` methods will be called and then new activity will be created anew. This is a potential problem for scan activity because in its lifecycle it controls both camera and native library - restarting the activity will trigger both restart of the camera and native library. This is a problem because changing orientation from landscape to portrait and vice versa will be very slow, thus degrading a user experience. **We do not recommend such setting.**

For that matter, we recommend setting your scan activity to either `portrait` or `landscape` mode and handle device orientation changes manually. To help you with this, `RecognizerRunnerView` supports adding child views to it that will be rotated regardless of activity's `screenOrientation`. You add a view you wish to be rotated (such as view that contains buttons, status messages, etc.) to `RecognizerRunnerView` with [addChildView](#{javadocUrl}(com/microblink/view/CameraViewGroup.html#addChildView-android.view.View-boolean-)) method. The second parameter of the method is a boolean that defines whether the view you are adding will be rotated with device. To define allowed orientations, implement [OrientationAllowedListener](https://photopay.github.io/photopay-android/com/microblink/view/OrientationAllowedListener.html) interface and add it to `RecognizerRunnerView` with method `setOrientationAllowedListener`. **This is the recommended way of rotating camera overlay.**

However, if you really want to set `screenOrientation` property to `sensor` or similar and want Android to handle orientation changes of your scan activity, then we recommend to set `configChanges` property of your activity to `orientation|screenSize`. This will tell Android not to restart your activity when device orientation changes. Instead, activity's `onConfigurationChanged` method will be called so that activity can be notified of the configuration change. In your implementation of this method, you should call `changeConfiguration` method of `RecognizerView` so it can adapt its camera surface and child views to new configuration.
## <a name="direct-api"></a> Direct API

This section will describe how to use direct API to recognize android Bitmaps without the need for camera. You can use direct API anywhere from your application, not just from activities.

Image recognition performance highly depends on the quality of the input images. When our camera management is used (scanning from a camera), we do our best to get camera frames with the best possible quality for the used device. On the other hand, when Direct API is used, you need to provide high-quality images without blur and glare for successful recognition.

### <a name="direct-api-images"></a> Using Direct API for recognition of Android Bitmaps and custom camera frames

1. First, you need to obtain reference to [RecognizerRunner singleton](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html) using [getSingletonInstance](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html#getSingletonInstance--).
2. Second, you need to [initialize the recognizer runner](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html#initialize-android.content.Context-com.microblink.entities.recognizers.RecognizerBundle-com.microblink.directApi.DirectApiErrorListener-).
3. After initialization, you can use singleton to process:
 - **Still** Android `Bitmaps` obtained, for example, from the gallery. Use [recognizeBitmap](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html#recognizeBitmap-android.graphics.Bitmap-com.microblink.hardware.orientation.Orientation-com.microblink.geometry.Rectangle-com.microblink.view.recognition.ScanResultListener-) or [recognizeBitmapWithRecognizers](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html#recognizeBitmapWithRecognizers-android.graphics.Bitmap-com.microblink.hardware.orientation.Orientation-com.microblink.geometry.Rectangle-com.microblink.view.recognition.ScanResultListener-com.microblink.entities.recognizers.RecognizerBundle-).
 - **Video** `Images` that are [built from custom camera video frames](https://photopay.github.io/photopay-android/com/microblink/image/ImageBuilder.html), for example, when you use your own or third party camera management. Recognition will be optimized for speed and will rely on time-redundancy between consecutive video frames in order to yield best possible recognition result. Use [recognizeVideoImage](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html#recognizeVideoImage-com.microblink.image.Image-com.microblink.view.recognition.ScanResultListener-) or [recognizeVideoImageWithRecognizers](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html#recognizeVideoImageWithRecognizers-com.microblink.image.Image-com.microblink.view.recognition.ScanResultListener-com.microblink.entities.recognizers.RecognizerBundle-).
 - **Still** `Images` when you need thorough scanning of single or few images which are not part of the video stream and you want to get best possible results from the single `Image`. [Image](https://photopay.github.io/photopay-android/com/microblink/image/Image.html) type comes from our SDK or it can be created by using [ImageBuilder](https://photopay.github.io/photopay-android/com/microblink/image/ImageBuilder.html). Use [recognizeStillImage](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html#recognizeStillImage-com.microblink.image.Image-com.microblink.view.recognition.ScanResultListener-) or [recognizeStillImageWithRecognizers](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html#recognizeStillImage-com.microblink.image.Image-com.microblink.view.recognition.ScanResultListener-com.microblink.entities.recognizers.RecognizerBundle-). 

4. When you want to delete all cached data from multiple recognitions, for example when you want to scan other document and/or restart scanning, you need to [reset the recognition state](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html#resetRecognitionState--).
5. Do not forget to [terminate](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html#terminate--) the recognizer runner singleton after usage (it is a shared resource).

Here is the minimum example of usage of direct API for recognizing android Bitmap:

```java
public class DirectAPIActivity extends Activity {
    private RecognizerRunner mRecognizerRunner;
    private CroatiaSlipRecognizer mRecognizer;
    private RecognizerBundle mRecognizerBundle;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate();
        // initialize your activity here
        // create CroatiaSlipRecognizer
        mRecognizer = new CroatiaSlipRecognizer();

        // bundle recognizers into RecognizerBundle
        mRecognizerBundle = new RecognizerBundle(mRecognizer);

        try {
            mRecognizerRunner = RecognizerRunner.getSingletonInstance();
        } catch (FeatureNotSupportedException e) {
            Toast.makeText(this, "Feature not supported! Reason: " + e.getReason().getDescription(), Toast.LENGTH_LONG).show();
            finish();
            return;
        }

        mRecognizerRunner.initialize(this, mRecognizerBundle, new DirectApiErrorListener() {
            @Override
            public void onRecognizerError(Throwable t) {
                Toast.makeText(DirectAPIActivity.this, "There was an error in initialization of Recognizer: " + t.getMessage(), Toast.LENGTH_SHORT).show();
                finish();
            }
        });
    }

    @Override
    protected void onResume() {
        super.onResume();
        // start recognition
        Bitmap bitmap = BitmapFactory.decodeFile("/path/to/some/file.jpg");
        mRecognizerRunner.recognizeBitmap(bitmap, Orientation.ORIENTATION_LANDSCAPE_RIGHT, mScanResultListener);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        mRecognizerRunner.terminate();
    }

    private final ScanResultListener mScanResultListener = new ScanResultListener() {
        @Override
        public void onScanningDone(@NonNull RecognitionSuccessType recognitionSuccessType) {
            // this method is from ScanResultListener and will be called
            // when scanning completes
            // you can obtain scanning result by calling getResult on each
            // recognizer that you bundled into RecognizerBundle.
            // for example:

            CroatiaSlipRecognizer.Result result = mRecognizer.getResult();
            if (result.getResultState() == Recognizer.Result.State.Valid) {
                // result is valid, you can use it however you wish
            }
        }
    };

}
```

[ScanResultListener.onScanningDone](https://photopay.github.io/photopay-android/com/microblink/view/recognition/ScanResultListener.html#onScanningDone-RecognitionSuccessType-) method is called for each input image that you send to the recognition. You can call `RecognizerRunner.recognize*` method multiple times with different images of the same document for better reading accuracy until you get a successful result in the listener's `onScanningDone` method. This is useful when you are using your own or third-party camera management.

### <a name="direct-api-strings"></a> Using Direct API for `String` recognition (parsing)

Some recognizers support recognition from `String`. They can be used through Direct API to parse given `String` and return data just like when they are used on an input image. When recognition is performed on `String`, there is no need for the OCR. Input `String` is used in the same way as the OCR output is used when image is being recognized. 

Recognition from `String` can be performed in the same way as recognition from image, described in the [previous section](direct-api-images).

The only difference is that one of the [RecognizerRunner singleton](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html) methods for recognition from string should be called:

- [recognizeString](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html#recognizeString-java.lang.String-com.microblink.view.recognition.ScanResultListener-)
- [recognizeStringWithRecognizers](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html#recognizeStringWithRecognizers-java.lang.String-com.microblink.view.recognition.ScanResultListener-com.microblink.entities.recognizers.RecognizerBundle-)


### <a name="direct-api-state-machine"></a> Understanding DirectAPI's state machine

Direct API's `RecognizerRunner` singleton is a state machine that can be in one of 3 states: `OFFLINE`, `READY` and `WORKING`.

- When you obtain the reference to `RecognizerRunner` singleton, it will be in `OFFLINE` state. 
- You can initialize `RecognizerRunner` by calling [initialize](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html#initialize-android.content.Context-com.microblink.entities.recognizers.RecognizerBundle-com.microblink.directApi.DirectApiErrorListener-) method. If you call `initialize` method while `RecognizerRunner` is not in `OFFLINE` state, you will get `IllegalStateException`.
- After successful initialization, `RecognizerRunner` will move to `READY` state. Now you can call any of the `recognize*` methods.
- When starting recognition with any of the `recognize*` methods, `RecognizerRunner` will move to `WORKING` state. If you attempt to call these methods while `RecognizerRunner` is not in `READY` state, you will get `IllegalStateException`
- Recognition is performed on background thread so it is safe to call all `RecognizerRunner's` methods from UI thread
- When recognition is finished, `RecognizerRunner` first moves back to `READY` state and then calls the [onScanningDone](https://photopay.github.io/photopay-android/com/microblink/view/recognition/ScanResultListener.html#onScanningDone-RecognitionSuccessType-) method of the provided [`ScanResultListener`](https://photopay.github.io/photopay-android/com/microblink/view/recognition/ScanResultListener.html). 
- Please note that `ScanResultListener`'s [`onScanningDone`](https://photopay.github.io/photopay-android/com/microblink/view/recognition/ScanResultListener.html#onScanningDone-RecognitionSuccessType-) method will be called on background processing thread, so make sure you do not perform UI operations in this callback. Also note that until the `onScanningDone` method completes, `RecognizerRunner` will not perform recognition of another image or string, even if any of the `recognize*` methods have been called just after transitioning to `READY` state. This is to ensure that results of the recognizers bundled within `RecognizerBundle` associated with `RecognizerRunner` are not modified while possibly being used within `onScanningDone` method.
- By calling [`terminate`](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html#terminate--) method, `RecognizerRunner` singleton will release all its internal resources. Note that even after calling `terminate` you might receive `onScanningDone` event if there was work in progress when `terminate` was called.
- `terminate` method can be called from any `RecognizerRunner` singleton's state
- You can observe `RecognizerRunner` singleton's state with method [`getCurrentState`](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html#getCurrentState--)

### <a name="direct-api-with-recognizer"></a> Using Direct API while RecognizerRunnerView is active
Both [RecognizerRunnerView](#recognizer-runner-view) and `RecognizerRunner` use the same internal singleton that manages native code. This singleton handles initialization and termination of native library and propagating recognizers to native library. It is possible to use `RecognizerRunnerView` and `RecognizerRunner` together, as internal singleton will make sure correct synchronization and correct recognition settings are used. If you run into problems while using `RecognizerRunner` in combination with `RecognizerRunnerView`, [let us know](http://help.microblink.com)!

# <a name="built-in-ui-components"></a> Available activities and overlays
## <a name='photopayUIComponent'></a> `PhotopayUISettings`

[`PhotopayUISettings `](https://photopay.github.io/photopay-android/com/microblink/uisettings/PhotopayUISettings.html) launches activity that uses [`BasicOverlayController`](https://photopay.github.io/photopay-android/com/microblink/fragment/overlay/basic/BasicOverlayController.html) with UI best suited for scanning various payment slips.

## <a name='ocrLineUIComponent'></a> `OcrLineUISettings`

[`OcrLineUISettings `](https://photopay.github.io/photopay-android/com/microblink/uisettings/OcrLineUISettings.html) launches activity that uses [`BasicOverlayController`](https://photopay.github.io/photopay-android/com/microblink/fragment/overlay/basic/BasicOverlayController.html) with UI best suited for performing scanning of payment slips that have entire payment information encoded in OCR line in lower part of the slip. For example, payment slips in Kosovo, Netherlands, Switzerland and United Kingdom.
## <a name='barcodeUIComponent'></a> `BarcodeUISettings`

[`BarcodeUISettings `](https://photopay.github.io/photopay-android/com/microblink/uisettings/BarcodeUISettings.html) launches activity that uses [`BasicOverlayController`](https://photopay.github.io/photopay-android/com/microblink/fragment/overlay/basic/BasicOverlayController.html) with UI best suited for performing scanning of various barcodes.

## <a name="fieldByFieldUiComponent"></a> `FieldByFieldUISettings` and `FieldByFieldOverlayController`

[`FieldByFieldOverlayController`](https://photopay.github.io/photopay-android/com/microblink/fragment/overlay/fieldbyfield/FieldByFieldOverlayController.html) is best suited for performing scanning of small text fields, which are scanned in the predefined order, one by one. 

To launch a built-in activity that uses `FieldByFieldOverlayController ` use [`FieldByFieldUISettings `](https://photopay.github.io/photopay-android/com/microblink/uisettings/FieldByFieldUISettings.html).
## <a name="translation"></a> Translation and localization

Strings used within built-in activities and overlays can be localized to any language. If you are using `RecognizerRunnerView` ([see this chapter for more information](#recognizerRunnerView)) in your custom scan activity or fragment, you should handle localization as in any other Android app. `RecognizerRunnerView` does not use strings nor drawables, it only uses assets from `assets/microblink` folder. Those assets must not be touched as they are required for recognition to work correctly.

However, if you use our built-in activities or overlays, they will use resources packed within `LibPhotoPay.aar` to display strings and images on top of the camera view. We have already prepared strings for several languages which you can use out of the box. You can also [modify those strings](#string-changing), or you can [add your own language](#add-language).

To use a language, you have to enable it from the code:
        
* To use a certain language, on application startup, before opening any UI component from the SDK, you should call method `LanguageUtils.setLanguageAndCountry(language, country, context)`. For example, you can set language to Croatian like this:
    
    ```java
    // define PhotoPay language
    LanguageUtils.setLanguageAndCountry("hr", "", this);
    ```

#### <a name="addLanguage"></a> Adding new language

_PhotoPay_ can easily be translated to other languages. The `res` folder in `LibPhotoPay.aar` archive has folder `values` which contains `strings.xml` - this file contains english strings. In order to make e.g. croatian translation, create a folder `values-hr` in your project and put the copy of `strings.xml` inside it (you might need to extract `LibPhotoPay.aar` archive to access those files). Then, open that file and translate the strings from English into Croatian.

#### <a name="stringChanging"></a> Changing strings in the existing language
    
To modify an existing string, the best approach would be to:

1. Choose a language you want to modify. For example Croatian ('hr').
2. Find `strings.xml` in folder `res/values-hr` of the `LibPhotoPay.aar` archive
3. Choose a string key which you want to change. For example: ```<string name="MBBack">Back</string>```
4. In your project create a file `strings.xml` in the folder `res/values-hr`, if it doesn't already exist
5. Create an entry in the file with the value for the string which you want. For example: ```<string name="MBBack">Natrag</string>```
6. Repeat for all the string you wish to change

# <a name="processing-events"></a> Handling processing events with `RecognizerRunner` and `RecognizerRunnerView`

Processing events, also known as _Metadata callbacks_ are purely intended for giving processing feedback on UI or to capture some debug information during development of your app using _PhotoPay_ SDK. For that reason, built-in activities and fragments handle those events internally. If you need to handle those events yourself, you need to use either [RecognizerRunnerView](#recognizer-runner-view) or [RecognizerRunner](#direct-api).

Callbacks for all events are bundled into the [MetadataCallbacks](https://photopay.github.io/photopay-android/com/microblink/metadata/MetadataCallbacks.html) object. Both [RecognizerRunner](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html#setMetadataCallbacks-com.microblink.metadata.MetadataCallbacks-) and [RecognizerRunnerView](https://photopay.github.io/photopay-android/com/microblink/view/recognition/RecognizerRunnerView.html#setMetadataCallbacks-com.microblink.metadata.MetadataCallbacks-) have methods which allow you to set all your callbacks.

We suggest that you check for more information about available callbacks and events to which you can handle in the [javadoc for MetadataCallbacks class](https://photopay.github.io/photopay-android/com/microblink/metadata/MetadataCallbacks.html).

Please note that both those methods need to pass information about available callbacks to the native code and for efficiency reasons this is done at the time `setMetadataCallbacks` method is called and **not every time** when change occurs within the `MetadataCallbacks` object. This means that if you, for example, set `QuadDetectionCallback` to `MetadataCallbacks` **after** you already called `setMetadataCallbacks` method, the `QuadDetectionCallback` will not be registered with the native code and you will not receive its events.

Similarly, if you, for example, remove the `QuadDetectionCallback` from `MetadataCallbacks` object **after** you already called `setMetadataCallbacks` method, your app will crash with `NullPointerException` when our processing code attempts to invoke the method on removed callback (which is now set to `null`). We **deliberately** do not perform `null` check here because of two reasons:

- it is inefficient
- having `null` callback, while still being registered to native code is illegal state of your program and it should therefore crash

**Remember**, each time you make some changes to `MetadataCallbacks` object, you need to apply those changes to to your `RecognizerRunner` or `RecognizerRunnerView` by calling its `setMetadataCallbacks` method.

# <a name="available-recognizers"></a> `Recognizer` concept and `RecognizerBundle`

This section will first describe [what is a `Recognizer`](#recognizer-concept) and how it should be used to perform recognition of the images, videos and camera stream. Next, [we will describe how `RecognizerBundle`](#recognizer-bundle) can be used to tweak the recognition procedure and to transfer `Recognizer` objects between activities.

[RecognizerBundle](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/RecognizerBundle.html) is an object which wraps the [Recognizers](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/Recognizer.html) and defines settings about how recognition should be performed. Besides that, `RecognizerBundle` makes it possible to transfer `Recognizer` objects between different activities, which is required when using built-in activities to perform scanning, as described in first scan section, but is also handy when you need to pass `Recognizer` objects between your activities.

List of all available `Recognizer` objects, with a brief description of each `Recognizer`, its purpose and recommendations how it should be used to get best performance and user experience, can be found [here](#recognizer-list) .

## <a name="recognizer-concept"></a> The `Recognizer` concept

The [Recognizer](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/Recognizer.html) is the basic unit of processing within the _PhotoPay_ SDK. Its main purpose is to process the image and extract meaningful information from it. As you will see [later](#recognizer-list), the _PhotoPay_ SDK has lots of different `Recognizer` objects that have various purposes.

Each `Recognizer` has a `Result` object, which contains the data that was extracted from the image. The `Result` object is a member of corresponding `Recognizer` object and its lifetime is bound to the lifetime of its parent `Recognizer` object. If you need your `Result` object to outlive its parent `Recognizer` object, you must make a copy of it by calling its method [`clone()`](https://photopay.github.io/photopay-android/com/microblink/entities/Entity.Result.html#clone--).

Every `Recognizer` is a stateful object, that can be in two states: _idle state_ and _working state_. While in _idle state_, you can tweak `Recognizer` object's properties via its getters and setters. After you bundle it into a `RecognizerBundle` and use either [RecognizerRunner](https://photopay.github.io/photopay-android/com/microblink/directApi/RecognizerRunner.html) or [RecognizerRunnerView](https://photopay.github.io/photopay-android/com/microblink/view/recognition/RecognizerRunnerView.html) to _run_ the processing with all `Recognizer` objects bundled within `RecognizerBundle`, it will change to _working state_ where the `Recognizer` object is being used for processing. While being in _working state_, you cannot tweak `Recognizer` object's properties. If you need to, you have to create a copy of the `Recognizer` object by calling its [`clone()`](https://photopay.github.io/photopay-android/com/microblink/entities/Entity.html#clone--), then tweak that copy, bundle it into a new `RecognizerBundle` and use [`reconfigureRecognizers`](https://photopay.github.io/photopay-android/com/microblink/view/recognition/RecognizerRunnerView.html#reconfigureRecognizers-com.microblink.entities.recognizers.RecognizerBundle-) to ensure new bundle gets used on processing thread.

While `Recognizer` object works, it changes its internal state and its result. The `Recognizer` object's `Result` always starts in [Empty state](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/Recognizer.Result.State.html#Empty). When corresponding `Recognizer` object performs the recognition of given image, its `Result` can either stay in `Empty` state (in case `Recognizer` failed to perform recognition), move to [Uncertain state](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/Recognizer.Result.State.html#Uncertain) (in case `Recognizer` performed the recognition, but not all mandatory information was extracted), move to [StageValid state](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/Recognizer.Result.State.html#StageValid) (in case `Recognizer` successfully scanned one part/side of the document and there are more fields to extract) or move to [Valid state](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/Recognizer.Result.State.html#Valid) (in case `Recognizer` performed recognition and all mandatory information was successfully extracted from the image).

As soon as one `Recognizer` object's `Result` within `RecognizerBundle` given to `RecognizerRunner` or `RecognizerRunnerView` changes to `Valid` state, the [`onScanningDone`](https://photopay.github.io/photopay-android/com/microblink/view/recognition/ScanResultListener.html#onScanningDone-RecognitionSuccessType-) callback will be invoked on same thread that performs the background processing and you will have the opportunity to inspect each of your `Recognizer` objects' `Results` to see which one has moved to `Valid` state.

As already stated in [section about `RecognizerRunnerView`](#recognizerRunnerView), as soon as `onScanningDone` method ends, the `RecognizerRunnerView` will continue processing new camera frames with same `Recognizer` objects, unless [paused](https://photopay.github.io/photopay-android/com/microblink/view/recognition/RecognizerRunnerView.html#pauseScanning--). Continuation of processing or [resetting recognition](https://photopay.github.io/photopay-android/com/microblink/view/recognition/RecognizerRunnerView.html#resetRecognitionState--) will modify or reset all `Recognizer` objects's `Results`. When using built-in activities, as soon as `onScanningDone` is invoked, built-in activity pauses the `RecognizerRunnerView` and starts finishing the activity, while saving the `RecognizerBundle` with active `Recognizer` objects into `Intent` so they can be transferred back to the calling activities.


## <a name="recognizer-bundle"></a> `RecognizerBundle`

The [RecognizerBundle](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/RecognizerBundle.html) is wrapper around [Recognizers](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/Recognizer.html) objects that can be used to transfer `Recognizer` objects between activities and to give `Recognizer` objects to `RecognizerRunner` or `RecognizerRunnerView` for processing.

The `RecognizerBundle` is always [constructed with array](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/RecognizerBundle.html#RecognizerBundle-com.microblink.entities.recognizers.Recognizer:A-) of `Recognizer` objects that need to be prepared for recognition (i.e. their properties must be tweaked already). The _varargs_ constructor makes it easier to pass `Recognizer` objects to it, without the need of creating a temporary array.

The `RecognizerBundle` manages a chain of `Recognizer` objects within the recognition process. When a new image arrives, it is processed by the first `Recognizer` in chain, then by the second and so on, iterating until a `Recognizer` object's `Result` changes its state to `Valid` or all of the `Recognizer` objects in chain were invoked (none getting a `Valid` result state). If you want to invoke all `Recognizers` in the chain, regardless of whether some `Recognizer` object's `Result` in chain has changed its state to `Valid` or not, you can [allow returning of multiple results on a single image](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/RecognizerBundle.html#setAllowMultipleScanResultsOnSingleImage-boolean-).

You cannot change the order of the `Recognizer` objects within the chain - no matter the order in which you give `Recognizer` objects to `RecognizerBundle`, they are internally ordered in a way that provides best possible performance and accuracy. Also, in order for _PhotoPay_ SDK to be able to order `Recognizer` objects in recognition chain in the best way possible, it is not allowed to have multiple instances of `Recognizer` objects of the same type within the chain. Attempting to do so will crash your application.

### <a name="intent-optimization"></a> Passing `Recognizer` objects between activities

Besides managing the chain of `Recognizer` objects, `RecognizerBundle` also manages transferring bundled `Recognizer` objects between different activities within your app. Although each `Recognizer` object, and each its `Result` object implements [Parcelable interface](https://developer.android.com/reference/android/os/Parcelable.html), it is not so straightforward to put those objects into [Intent](https://developer.android.com/reference/android/content/Intent.html) and pass them around between your activities and services for two main reasons:

- `Result` object is tied to its `Recognizer` object, which manages lifetime of the native `Result` object.
- `Result` object often contains large data blocks, such as images, which cannot be transferred via `Intent` because of [Android's Intent transaction data limit](https://developer.android.com/reference/android/os/TransactionTooLargeException.html).

Although the first problem can be easily worked around by making a [copy](https://photopay.github.io/photopay-android/com/microblink/entities/Entity.Result.html#clone--) of the `Result` and transfer it independently, the second problem is much tougher to cope with. This is where, `RecognizerBundle's` methods [saveToIntent](https://photopay.github.io/photopay-android/com/microblink/intent/IntentTransferableBundle.html#saveToIntent-android.content.Intent-) and [loadFromIntent](https://photopay.github.io/photopay-android/com/microblink/intent/IntentTransferableBundle.html#loadFromIntent-android.content.Intent-) come to help, as they ensure the safe passing of `Recognizer` objects bundled within `RecognizerBundle` between activities according to policy defined with method [`setIntentDataTransferMode`](https://photopay.github.io/photopay-android/com/microblink/MicroblinkSDK.html#setIntentDataTransferMode-com.microblink.intent.IntentDataTransferMode-):

- if set to [`STANDARD`](https://photopay.github.io/photopay-android/com/microblink/intent/IntentDataTransferMode.html#STANDARD), the `Recognizer` objects will be passed via `Intent` using normal _Intent transaction mechanism_, which is limited by [Android's Intent transaction data limit](https://developer.android.com/reference/android/os/TransactionTooLargeException.html). This is same as manually putting `Recognizer` objects into `Intent` and is OK as long as you do not use `Recognizer` objects that produce images or other large objects in their `Results`.
- if set to [`OPTIMISED`](https://photopay.github.io/photopay-android/com/microblink/intent/IntentDataTransferMode.html#OPTIMISED), the `Recognizer` objects will be passed via internal singleton object and no serialization will take place. This means that there is no limit to the size of data that is being passed. This is also the fastest transfer method, but it has a serious drawback - if Android kills your app to save memory for other apps and then later restarts it and redelivers `Intent` that should contain `Recognizer` objects, the internal singleton that should contain saved `Recognizer` objects will be empty and data that was being sent will be lost. You can easily provoke that condition by choosing _No background processes_ under _Limit background processes_ in your device's _Developer options_, and then switch from your app to another app and then back to your app.
- if set to [`PERSISTED_OPTIMISED`](https://photopay.github.io/photopay-android/com/microblink/intent/IntentDataTransferMode.html#PERSISTED_OPTIMISED), the `Recognizer` objects will be passed via internal singleton object (just like in `OPTIMISED` mode) and will additionaly be serialized into a file in your application's private folder. In case Android restarts your app and internal singleton is empty after re-delivery of the `Intent`, the data will be loaded from file and nothing will be lost. The files will be automatically cleaned up when data reading takes place. Just like `OPTIMISED`, this mode does not have limit to the size of data that is being passed and does not have a drawback that `OPTIMISED` mode has, but some users might be concerned about files to which data is being written. 
    - These files **will** contain end-user's private data, such as image of the object that was scanned and the extracted data. Also these files **may** remain saved in your application's private folder until the next successful reading of data from the file. 
    - If your app gets restarted multiple times, only after first restart will reading succeed and will delete the file after reading. If multiple restarts take place, you must implement [`onSaveInstanceState`](https://developer.android.com/reference/android/app/Activity.html#onSaveInstanceState(android.os.Bundle)) and save bundle back to file by calling its [`saveState`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/RecognizerBundle.html#saveState--) method. Also, after saving state, you should ensure that you [clear saved state](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/RecognizerBundle.html#clearSavedState--) in your [`onResume`](https://developer.android.com/reference/android/app/Activity.html#onResume()), as [`onCreate`](https://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle)) may not be called if activity is not restarted, while `onSaveInstanceState` may be called as soon as your activity goes to background (before `onStop`), even though activity may not be killed at later time. 
    - If saving data to file in private storage is a concern to you, you should use either `OPTIMISED` mode to transfer large data and image between activities or create your own mechanism for data transfer. Note that your application's private folder is only accessible by your application and your application alone, unless the end-user's device is rooted.

# <a name="recognizer-list"></a> List of available recognizers

This section will give a list of all `Recognizer` objects that are available within _PhotoPay_ SDK, their purpose and recommendations how they should be used to get best performance and user experience.

## <a name="frame-grabber-recognizer"></a> Frame Grabber Recognizer

The [`FrameGrabberRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/framegrabber/FrameGrabberRecognizer.html) is the simplest recognizer in _PhotoPay_ SDK, as it does not perform any processing on the given image, instead it just returns that image back to its [`FrameCallback`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/framegrabber/FrameCallback.html). Its [Result](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/framegrabber/FrameGrabberRecognizer.Result.html) never changes state from [Empty](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/Recognizer.Result.State.html#Empty).

This recognizer is best for easy capturing of camera frames with [`RecognizerRunnerView`](#recognizerRunnerView). Note that [`Image`](https://photopay.github.io/photopay-android/com/microblink/image/Image.html) sent to [`onFrameAvailable`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/framegrabber/FrameCallback.html#onFrameAvailable-com.microblink.image.Image-boolean-double-) are temporary and their internal buffers all valid only until the `onFrameAvailable` method is executing - as soon as method ends, all internal buffers of `Image` object are disposed. If you need to store `Image` object for later use, you must create a copy of it by calling [`clone`](https://photopay.github.io/photopay-android/com/microblink/image/Image.html#clone--).

Also note that [`FrameCallback`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/framegrabber/FrameCallback.html) interface extends [Parcelable interface](https://developer.android.com/reference/android/os/Parcelable.html), which means that when implementing `FrameCallback` interface, you must also implement `Parcelable` interface. 

This is especially important if you plan to transfer `FrameGrabberRecognizer` between activities - in that case, keep in mind that the instance of your object may not be the same as the instance on which `onFrameAvailable` method gets called - the instance that receives `onFrameAvailable` calls is the one that is created within activity that is performing the scan.

## <a name="success-frame-grabber-recognizer"></a> Success Frame Grabber Recognizer

The [`SuccessFrameGrabberRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/successframe/SuccessFrameGrabberRecognizer.html) is a special `Recognizer` that wraps some other `Recognizer` and impersonates it while processing the image. However, when the `Recognizer` being impersonated changes its `Result` into `Valid` state, the `SuccessFrameGrabberRecognizer` captures the image and saves it into its own `Result` object.

Since `SuccessFrameGrabberRecognizer` impersonates its slave `Recognizer` object, it is not possible to give both concrete `Recognizer` object and `SuccessFrameGrabberRecognizer` that wraps it to same `RecognizerBundle` - doing so will have the same result as if you have given two instances of same `Recognizer` type to the `RecognizerBundle` - it will crash your application.

This recognizer is best for use cases when you need to capture the exact image that was being processed by some other `Recognizer` object at the time its `Result` became `Valid`. When that happens, `SuccessFrameGrabber's` [`Result`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/successframe/SuccessFrameGrabberRecognizer.Result.html) will also become `Valid` and will contain described image. That image can then be retrieved with [`getSuccessFrame()`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/successframe/SuccessFrameGrabberRecognizer.Result.html#getSuccessFrame--) method.

## <a name="photopay_recognizers"></a> PhotoPay recognizers

Unless stated otherwise for concrete recognizer, recognizers from this list can be used in any context, but they work best with the [`PhotopayActivity`](#photopayUIComponent), which has the UI best suited for scanning payment slips and payment barcodes.

### <a name="photopay_recognizers_sepa"></a> SEPA Payment QR code recognizer

The [`SepaQrCodePaymentRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/sepa/SepaQrCodePaymentRecognizer.html) is used for scanning payment information from SEPA (Single Euro Payments Area) payment QR codes. The recognizer support scanning payment QR codes that are encoded by [standard defined by European Payments Council](https://www.europeanpaymentscouncil.eu/document-library/guidance-documents/quick-response-code-guidelines-enable-data-capture-initiation).

## <a name="photopay_recognizers_countries"></a> Country-specific PhotoPay recognizers

### <a name="photopay_recognizers_austria"></a> Austria

#### <a name="austria_payslip"></a> Austrian payslip recognizer

The [`AustriaSlipRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/austria/slip/AustriaSlipRecognizer.html) is used for scanning payment information from austrian payment slips. It supports both SEPA and old version od the payment slip standard. This recognizer works only in landscape orientation, so we recommend using the [`PhotopayActivity`](#photopayUIComponent) with that recognizer, as it will automatically switch to landscape-only orientation when this recognizer is used.

#### <a name="austria_qr"></a> Austrian payment QR code recognizer

The [`AustriaQrCodeRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/austria/qr/AustriaQrCodeRecognizer.html) is used for scanning payment information from QR code usually found on SEPA payment slips in Austria.

### <a name="photopay_recognizers_belgium"></a> Belgium

#### <a name="belgium_payslip"></a> Belgian payslip recognizer

The ['BelgiumSlipRecognizer'](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/belgium/slip/BelgiumSlipRecognizer.html) is used for scanning payment information from belgian payment slips. This recognizer works only in landscape orientation, so we recommend using the [`PhotopayActivity`](#photopayUIComponent) with that recognizer, as it will automatically switch to landscape-only orientation when this recognizer is used.

### <a name="photopay_recognizers_croatia"></a> Croatia

#### <a name="croatia_payslip"></a> Croatian payslip recognizer

The [`CroatiaSlipRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/croatia/slip/CroatiaSlipRecognizer.html) is used for scanning payment information from croatian payment slips. It supports both HUB3 and old HUB1 version od the payment slip standard.

#### <a name="croatia_pdf417"></a> Croatian payment PDF417 2D barcode recognizer

The [`CroatiaPdf417PaymentRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/croatia/CroatiaPdf417PaymentRecognizer.html) is used for scanning payment information from PDF417 2D barcode usually found on payment slips. It supports both HUB3 and HUB1 2D barcode standards.

#### <a name="croatia_qr"></a> Croatian payment QR code recognizer

The [`CroatiaQrCodePaymentRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/croatia/CroatiaQrCodePaymentRecognizer.html) is used for scanning payment information from QR codes that have content encoded in same format as specified by HUB3 PDF417 2D barcode standard.

### <a name="photopay_recognizers_czechia"></a> Czechia

#### <a name="czechia_payslip"></a> Czech payslip recognizer

The [`CzechiaSlipRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/czechia/slip/CzechiaSlipRecognizer.html) is used for scanning payment information from czech payment slips.

#### <a name="czechia_qr"></a> Czech payment QR code recognizer

The [`CzechiaQrCodeRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/czechia/qr/CzechiaQrCodeRecognizer.html) is used for scanning payment information from payment QR codes that are usually found on czech payment slips.

### <a name="photopay_recognizers_germany"></a> Germany

#### <a name="germany_payslip"></a> German payslip recognizer

The [`GermanySlipRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/germany/slip/GermanySlipRecognizer.html) is used for scanning payment information from german payment slips. It supports both SEPA and old version od the payment slip standard. This recognizer works only in landscape orientation, so we recommend using the [`PhotopayActivity`](#photopayUIComponent) with that recognizer, as it will automatically switch to landscape-only orientation when this recognizer is used.

#### <a name="germany_qr"></a> German payment QR code recognizer

The [`GermanyQrCodeRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/germany/qr/GermanyQrCodeRecognizer.html) is used for scanning payment information from QR code usually found on SEPA payment slips in Germany.

### <a name="photopay_recognizers_hungary"></a> Hungary

#### <a name="hungary_payslip"></a> Hungarian payslip recognizer

The [`HungarySlipRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/hungary/slip/HungarySlipRecognizer.html) is used for scanning payment information from hungarian payment slips. It supports both yellow and white versions od the payment slip standard. This recognizer works only in landscape orientation, so we recommend using the [`PhotopayActivity`](#photopayUIComponent) with that recognizer, as it will automatically switch to landscape-only orientation when this recognizer is used.

### <a name="photopay_recognizers_kosovo"></a> Kosovo

#### <a name="kosovo_payslip"></a> Kosovo payslip recognizer

The [`KosovoSlipRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/kosovo/slip/KosovoSlipRecognizer.html) is used for scanning payment information from payment slips in Kosovo. This recognizer works by scanning OCR line from the lower part of the payment slip, so we recommend using the [`OcrLineScanActivity`](#ocrLineUIComponent) with that recognizer, as it has UI best suited for scanning OCR lines.

#### <a name="kosovo_code128"></a> Kosovo Code 128 recognizer

The [`KosovoCode128Recognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/kosovo/code128/KosovoCode128Recognizer.html) is used for scanning payment information from Code128 1D barcodes usually found on payment slips in Kosovo.

### <a name="photopay_recognizers_kosovo"></a> Netherlands

#### <a name="netherlands_payslip"></a> Dutch payslip recognizer

The [`NetherlandsSlipRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/netherlands/slip/NetherlandsSlipRecognizer.html) is used for scanning payment information from payment slips in Netherlands. This recognizer works by scanning OCR line from the lower part of the payment slip, so we recommend using the [`OcrLineScanActivity`](#ocrLineUIComponent) with that recognizer, as it has UI best suited for scanning OCR lines.

### <a name="photopay_recognizers_serbia"></a> Serbia

#### <a name="serbia_pdf417"></a> Serbian payment PDF417 2D barcode recognizer

The [`SerbiaPdf417PaymentRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/serbia/pdf417/SerbiaPdf417PaymentRecognizer.html) is used for scanning payment information from PDF417 2D barcode found on some serbian invoices. The Republic of Serbia does not have a national standard for payment slips nor payment barcodes. This recognizer supports scanning PDF417 2D barcodes that are modelled after Croatian HUB3 standard.

#### <a name="serbia_qr"></a> Serbian payment QR code recognizer

The [`SerbiaQrCodePaymentRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/serbia/qr/SerbiaQrCodePaymentRecognizer) is used for scanning payment information from QR code found on some serbian invoices. The Republic of Serbia does not have a national standard for payment slips nor payment barcodes. This recognizer supports scanning QR codes that are modelled after Croatian HUB3 standard.

### <a name="photopay_recognizers_slovakia"></a> Slovakia

#### <a name="slovakia_payslip"></a> Slovak payslip recognizer

The [`SlovakiaSlipRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/slovakia/slip/SlovakiaSlipRecognizer.html) is used for scanning payment information from payment slips in Slovakia. It supports only green payslips. You cannot use this recognizer to perform OCR of white slips. This recognizer works only in landscape orientation, so we recommend using the [`PhotopayActivity`](#photopayUIComponent) with that recognizer, as it will automatically switch to landscape-only orientation when this recognizer is used.


#### <a name="slovakia_code128"></a> Slovak payment Code 128 recognizer

The [`SlovakiaCode128PaymentRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/slovakia/SlovakiaCode128PaymentRecognizer.html) is used for scanning payment information from Code128 1D barcode usually found on both white and green payment slips in Slovakia.

#### <a name="slovakia_data_matrix"></a> Slovak payment Data Matrix Code recognizer

The [`SlovakiaDataMatrixPaymentRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/slovakia/SlovakiaDataMatrixPaymentRecognizer.html) is used for scanning payment information from Data Matrix 2D barcode usually found on some white payment slips in Slovakia.

#### <a name="slovakia_qr"></a> Slovak payBySquare QR code recognizer

The [`SlovakiaQrCodePaymentRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/slovakia/qr/SlovakiaQrCodePaymentRecognizer.html) is used for scanning payment information from [Slovak pyBySquare](https://bysquare.com/) payment QR code. This recognizer support only scanning the blue (PAY bySquare) QR codes. The orange (INVOICE bySquare) QR codes are not supported by this recognizer.

### <a name="photopay_recognizers_slovenia"></a> Slovenia

#### <a name="slovenia_payslip"></a> Slovenian payslip recognizer

The [`SloveniaSlipRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/slovenia/slip/SloveniaSlipRecognizer.html) is used for scanning payment information from slovenian [UPN payment slips](https://www.nlb.si/univerzalni-placilni-nalog). It performs the OCR of the left part of the slip (the _talon_) - it is not able to perform scanning of right part of the slip nor the OCR line.

#### <a name="slovenia_qr"></a> Slovenian payment QR code recognizer

The [`SloveniaQrCodePaymentRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/slovenia/SloveniaQrCodePaymentRecognizer.html) is used for scanning payment information from payment QR codes usually found on [UPN payment slips](https://www.nlb.si/univerzalni-placilni-nalog) in Slovenia.

### <a name="photopay_recognizers_switzerland"></a> Switzerland

#### <a name="switzerland_payslip"></a> Swiss payslip recognizer

The [`SwitzerlandSlipRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/switzerland/slip/SwitzerlandSlipRecognizer.html) is used for scanning payment information from payment slips in Switzerland. It supports scanning only OCR lines in orange slips. It does not support red payment slips.  This recognizer works by scanning OCR line from the lower part of the payment slip, so we recommend using the [`OcrLineScanActivity`](#ocrLineUIComponent) with that recognizer, as it has UI best suited for scanning OCR lines.

#### <a name="switzerland_qr"></a> Swiss payment QR code recognizer

The [`SwitzerlandQrCodePaymentRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/switzerland/SwitzerlandQrCodePaymentRecognizer.html) is used for scanning payment information from payment QR codes used in Switzerland.

### <a name="photopay_recognizers_uk"></a> United Kingdom

#### <a name="uk_payslip"></a> UK payslip recognizer

The [`UnitedKingdomSlipRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/uk/slip/UnitedKingdomSlipRecognizer.html) is used for scanning payment information from payment slips in United Kingdom. It supports scanning both OCR and MICR lines found in UK giro credit slips. This recognizer works by scanning OCR line from the lower part of the payment slip, so we recommend using the [`OcrLineScanActivity`](#ocrLineUIComponent) with that recognizer, as it has UI best suited for scanning OCR lines.

#### <a name="uk_qr"></a> UK payment QR code recognizer

The [`UnitedKingdomQrCodePaymentRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/photopay/unitedkingdom/UnitedKingdomQrCodePaymentRecognizer.html) is used for scanning payment information from payment QR codes in United Kingdom. Please [contact us](help.microblink.com) for more information about supported QR code standards.
## <a name="blinkInputRecognizer"></a> BlinkInput recognizer

The [`BlinkInputRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/blinkinput/BlinkInputRecognizer.html) is generic OCR recognizer used for scanning segments which enables specifying `Processors` that will be used for scanning. Most commonly used `Processor` within this recognizer is [`ParserGroupProcessor`](https://photopay.github.io/photopay-android/com/microblink/entities/processors/parserGroup/ParserGroupProcessor.html) that activates all `Parsers` in the group to extract data of interest from the OCR result.

This recognizer can be used in any context. It is used internally in the implementation of the provided [`FieldByFieldOverlayController`](https://photopay.github.io/photopay-android/com/microblink/fragment/overlay/FieldByFieldOverlayController.html).

`Processors` are explained in [The Processor concept](#processorConcept) section and you can find more about `Parsers` in [The Parser concept](#parserConcept) section.



# <a name="fieldByFieldFeature"></a> `Field by field` scanning feature

[`Field by field`](#fieldByFieldFeature) scanning feature is designed for scanning small text fields which are called scan elements. Elements are scanned in the predefined order. For each scan element, specific [`Parser`](#parserConcept) that will extract structured data of interest from the OCR result is defined. Focusing on the small text fields which are scanned one by one enables implementing support for the **free-form documents** because field detection is not required. The user is responsible for positioning the field of interest inside the scanning window and the scanning process guides him. When implementing support for the custom document, only fields of interest has to be defined.

`Field by field` scan can be performed by using provided [`FieldByFieldScanActivity` and `FieldByFieldOverlayController`](#fieldByFieldUiComponent).

For preparing the scan configuration, [`FieldByFieldBundle`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/config/fieldbyfield/FieldByFieldBundle.html) is used. It holds the array of `FieldByFieldElements` passed to its constructor and it is responsible for transferring them from one `Activity` to another, just like the [`RecognizerBundle`](#recognizerBundle) transfers `Recognizers`.
 
[`FieldByFieldElement`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/config/fieldbyfield/FieldByFieldElement.html) holds a combination of `Parser` used for data extraction, its title and message which are shown in the UI during the scan. For all available configuration options please see the [javadoc](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/config/fieldbyfield/FieldByFieldElement.html).

When `FieldByFieldBundle` is prepared, it should be used for creating the [`FieldByFieldUISettings`](https://photopay.github.io/photopay-android/com/microblink/uisettings/FieldByFieldUISettings.html) which accepts `FieldByFieldBundle` as a constructor argument and can be used to additionally tweak the scanning process and UI. For the list of all available configuration options, please see [javadoc](https://photopay.github.io/photopay-android/com/microblink/uisettings/FieldByFieldUISettings.html).
 
For starting the [`FieldByFieldScanActivity`](https://photopay.github.io/photopay-android/com/microblink/activity/FieldByFieldScanActivity.html), the [ActivityRunner.startActivityForResult](https://photopay.github.io/photopay-android/com/microblink/uisettings/ActivityRunner.html#startActivityForResult-android.app.Activity-int-com.microblink.uisettings.UISettings-) should be called with the prepared `FieldByFieldUISettings`.
 
When the scanning is done and control is returned to the calling activity, in `onActivityResult` method [FieldByFieldBundle.loadFromIntent](https://photopay.github.io/photopay-android/com/microblink/intent/IntentTransferableBundle.html#loadFromIntent-android.content.Intent-) should be called. `FieldByFieldBundle` will load the scanning results to the `Parser` instances held by its elements.
## <a name="quickScan_field_by_field"></a> Performing your first `field by field` scan

1. Before starting a recognition process, you need to obtain a license from [Microblink dashboard](https://microblink.com/login). After registering, you will be able to generate a trial license for your app. License is bound to [package name](http://tools.android.com/tech-docs/new-build-system/applicationid-vs-packagename) of your app, so please make sure you enter the correct package name when asked. 

    After creating a license, you will have the option to download the license as a file that you must place within your application's _assets_ folder. You must ensure that license key is set before instantiating any other classes from the SDK, otherwise you will get an exception at runtime. Therefore, we recommend that you extend [Android Application class](https://developer.android.com/reference/android/app/Application.html) and set the license in its [onCreate callback](https://developer.android.com/reference/android/app/Application.html#onCreate()) in the following way:

    ```java
    public class MyApplication extends Application {
        @Override
        public void onCreate() {
            MicroblinkSDK.setLicenseFile("path/to/license/file/within/assets/dir", this);
        }
    }
    ```

2. In your main activity create parser objects that will be used during recognition, configure them if needed, define scan elements and store them in [`FieldByFieldBundle`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/config/fieldbyfield/FieldByFieldBundle.html) object. For example, to scan three fields: amount, date and raw text, you can configure your recognizer object in the following way:

   ```java
    public class MyActivity extends Activity {
        // parsers are member variables because it will be used for obtaining results
        private AmountParser mAmountParser;
        private DateParser mDateParser;
        private RawParser mRawParser;

        /** Reference to bundle is kept, it is used later for loading results from intent */
        private FieldByFieldBundle mFieldByFieldBundle;
        
        @Override
        protected void onCreate(Bundle bundle) {
            super.onCreate(bundle);
            
            // setup views, as you would normally do in onCreate callback
            
            mAmountParser = new AmountParser();
            mDateParser = new DateParser();
            mRawParser = new RawParser();
            
            // prepare scan elements and put them in FieldByFieldBundle
            // we need to scan 3 items, so we will create bundle with 3 elements
            mFieldByFieldBundle = new FieldByFieldBundle(
                // each scan element contains two string resource IDs: string shown in title bar
                // and string shown in text field above scan box. Besides that, it contains parser
                // that will extract data from the OCR result.
                new FieldByFieldElement(R.string.amount_title, R.string.amount_msg, mAmountParser),
                new FieldByFieldElement(R.string.date_title, R.string.date_msg, mDateParser),
                new FieldByFieldElement(R.string.raw_title, R.string.raw_msg, mRawParser)
            );
        }
    }
    ```
    
3. You can start recognition process by starting [`FieldByFieldScanActivity`](https://photopay.github.io/photopay-android/com/microblink/activity/FieldByFieldScanActivity.html). You need to do that by creating [`FieldByFieldUISettings`](https://photopay.github.io/photopay-android/com/microblink/uisettings/FieldByFieldUISettings.html) and calling [`ActivityRunner.startActivityForResult`](https://photopay.github.io/photopay-android/com/microblink/uisettings/ActivityRunner.html#startActivityForResult-android.app.Activity-int-com.microblink.uisettings.UISettings-) method:

   ```java
    // method within MyActivity from previous step
    public void startFieldByFieldScanning() {
        // we use FieldByFieldUISettings - settings for FieldByFieldScanActivity
        FieldByFieldUISettings scanActivitySettings = new FieldByFieldUISettings(mFieldByFieldBundle);
        
        // tweak settings as you wish
        
        // Start activity
        ActivityRunner.startActivityForResult(this, MY_REQUEST_CODE, scanActivitySettings);
    }
    ```

4. After `FieldByFieldScanActivity` finishes the scan, it will return to the calling activity or fragment and will call its method `onActivityResult`. You can obtain the scanning results in that method.

    ```java
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        
        if (requestCode == MY_REQUEST_CODE) {
            if (resultCode == FieldByFieldScanActivity.RESULT_OK && data != null) {
                // load the data into all parsers bundled within your FieldByFieldBundle
                mFieldByFieldBundle.loadFromIntent(data);
                
                // now every parser object that was bundled within FieldByFieldBundle
                // has been updated with results obtained during scanning session
                
                // you can get the results by invoking getResult on each parser, and then
                // invoke specific getter for each concrete parser result type
                String amount = mAmountParser.getResult().getAmount();
                String date = mDateParser.getResult().getDate().toString();
                String rawText = mRawParser.getResult().getRawText();

                if (!amount.isEmpty()) {
                    // amount has been successfully parsed, you can use it however you wish
                }
                if (!date.isEmpty()) {
                    // date has been successfully parsed, you can use it however you wish
                }
                if (!rawText.isEmpty()) {
                    // raw text has been successfully returned, you can use it however you wish
                }
            }
        }
    }
    ``` 
    
# <a name="processorsAndParsers"></a> `Processor` and `Parser`

The `Processors` and `Parsers` are standard processing units within *PhotoPay* SDK used for data extraction from the input images. Unlike the [`Recognizer`](#recognizerConcept), `Processor` and `Parser` are not stand-alone processing units. `Processor` is always used within `Recognizer` and `Parser` is used within appropriate `Processor` to extract data from the OCR result.

## <a name="processorConcept"></a> The `Processor` concept

`Processor` is a processing unit used within some `Recognizer` which supports processors. It processes the input image prepared by the enclosing `Recognizer` in the way that is characteristic to the implementation of the concrete `Processor`.

For example, [`BlinkInputRecognizer`](#blinkInputRecognizer) encloses a collection of processors which are run on the input image to extract data. To perform the OCR of the input image, [`ParserGroupProcessor`](#parserGroupProcessor) is used. 

`Processor` architecture is similar to `Recognizer` architecture described in [The Recognizer concept](#recognizerConcept) section. Each instance also has associated inner `Result` object whose lifetime is bound to the lifetime of its parent `Processor` object and it is updated while `Processor` works. If you need your `Result` object to outlive its parent `Processor` object, you must make a copy of it by calling its method [`clone()`](https://photopay.github.io/photopay-android/com/microblink/entities/Entity.Result.html#clone--).

It also has its internal state and while it is in the *working state* during recognition process, it is not allowed to tweak `Processor` object's properties.

To support common use cases, there are several different `Processor` implementations available. They are listed in the next section.

## <a name="processorList"></a> List of available processors

This section will give a list of `Processor` types that are available within *PhotoPay* SDK and their purpose.

### <a name="parserGroupProcessor"></a> Parser Group Processor

The [`ParserGroupProcessor`](https://photopay.github.io/photopay-android/com/microblink/entities/processors/parserGroup/ParserGroupProcessor.html) is the type of the processor that performs the OCR (*Optical Character Recognition*) on the input image and lets all the parsers within the group to extract data from the OCR result. The concept of `Parser` is described in [the next](#parserConcept) section.

Before performing the OCR, the best possible OCR engine options are calculated by combining engine options needed by each `Parser` from the group. For example, if one parser expects and produces result from uppercase characters and other parser extracts data from digits, both uppercase characters and digits must be added to the list of allowed characters that can appear in the OCR result. This is a simplified explanation because OCR engine options contain many parameters which are combined by the `ParserGroupProcessor`.

Because of that, if multiple parsers and multiple parser group processors are used during the scan, it is very important to group parsers carefully.

Let's see this on an example: assume that we have two parsers at our disposal: `AmountParser` and `IbanParser`. `AmountParser` knows how to extract amount's from OCR result and requires from OCR only to recognize digits, periods and commas and ignore letters. On the other hand, `IbanParser` knows how to extract IBAN from OCR result and requires from OCR to recognize letters and digits.

If we put both `AmountParser` and `IbanParser` into the same `ParserGroupProcessor`, the merged OCR engine settings will require recognition of all letters, all digits, both period and comma. Such OCR result will contain all characters for `IbanParser` to properly parse IBAN, but might confuse `AmountParser` if OCR misclassifies some characters into digits.

If we put `AmountParser` in one `ParserGroupProcessor` and `IbanParser` in another `ParserGroupProcessor`, OCR will be performed for each parser group independently, thus preventing the `AmountParser` confusion, but two OCR passes of the image will be performed, which can have a performance impact.

`ParserGroupProcessor` is most commonly used `Processor`. It is used whenever the OCR is needed. After the OCR is performed and all parsers are run, parsed results can be obtained through parser objects that are enclosed in the group. `ParserGroupProcessor` instance also has associated inner `ParserGroupProcessor.Result` whose state is updated during processing and its method [`getOcrResult()`](https://photopay.github.io/photopay-android/com/microblink/entities/processors/parserGroup/ParserGroupProcessor.Result.html#getOcrResult--) can be used to obtain the raw [`OCRResult`](https://photopay.github.io/photopay-android/com/microblink/results/ocr/OcrResult.html) that was used for parsing data.

Take note that `OCRResult` is available only if it is allowed by the *PhotoPay* SDK license key. `OCRResult` structure contains information about all recognized characters and their positions on the image. To prevent someone to abuse that, obtaining of the `OCRResult` structure is allowed only by the premium license keys.

## <a name="parserConcept"></a> The `Parser` concept

`Parser` is a class of objects that are used to extract structured data from the raw OCR result. It must be used within `ParserGroupProcessor` which is responsible for performing the OCR, so `Parser` is not stand-alone processing unit.

Like [`Recognizer`](#recognizerConcept) and all other processing units, each `Parser` instance has associated inner `Result` object whose lifetime is bound to the lifetime of its parent `Parser` object and it is updated while `Parser` works. When parsing is done `Result` can be used for obtaining extracted data. If you need your `Result` object to outlive its parent `Parser` object, you must make a copy of it by calling its method [`clone()`](https://photopay.github.io/photopay-android/com/microblink/entities/Entity.Result.html#clone--).

It also has its internal state and while it is in the *working state* during recognition process, it is not allowed to tweak `Parser` object's properties.

There are a lot of different `Parsers` for extracting most common fields which appear on various documents. Also, most of them can be adjusted for specific use cases.

## <a name="parserList"></a> List of available parsers

### <a name="amountParser"></a> Amount Parser

[`AmountParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/amount/AmountParser.html) is used for extracting amounts from the OCR result. For available configuration options and result getters please check [javadoc](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/amount/AmountParser.html).

### <a name="dateParser"></a> Date Parser

[`DateParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/date/DateParser.html) is used for extracting dates in various formats from the OCR result. For available configuration options and result getters please check [javadoc](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/date/DateParser.html).

### <a name="ibanParser"></a> IBAN Parser

[`IbanParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/iban/IbanParser.html) is used for extracting IBAN (*International Bank Account Number*) from the OCR result. For available configuration options and result getters please check [javadoc](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/iban/IbanParser.html).

### <a name="rawParser"></a> Raw Parser

[`RawParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/raw/RawParser.html) is used for obtaining string version of raw OCR result, without performing any smart parsing operations. For available result getters please check [javadoc](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/raw/RawParser.html).

## <a name="parserList_photopay"></a> List of available parsers for payment information scanning by countries

### <a name="photopay_parsers_australia"></a> Australia

#### <a name="photopay_parsers_australia_abn"></a> Australian Business Number parser

The [`AustraliaAbnParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/australia/abn/AustraliaAbnParser.html) is used for scanning Australian Business Number.

#### <a name="photopay_parsers_australia_account"></a> Australian Account Number parser

The [`AustraliaAccountParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/australia/bankdetails/account/AustraliaAccountParser.html) is used for scanning Australian Account Number.

#### <a name="photopay_parsers_australia_bsb"></a> Australian Bank State Branch parser

The [`AustraliaBsbParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/australia/bankdetails/bsb/AustraliaBsbParser.html) is used for scanning Australian Bank State Branch.

#### <a name="photopay_parsers_australia_biller"></a> Australian Biller Code parser

The [`AustraliaBillerParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/australia/bpay/biller/AustraliaBillerParser.html) is used for scanning Australian Biller Code.

#### <a name="photopay_parsers_australia_reference"></a> Australian payment reference number parser

The [`AustraliaReferenceParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/australia/bpay/reference/AustraliaReferenceParser.html) is used for scanning Australian payment reference number.

### <a name="photopay_parsers_austria"></a> Austria

#### <a name="photopay_parsers_austria_reference"></a> Austrian payment reference number parser

The [`AustriaReferenceParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/austria/reference/AustriaReferenceParser.html) is used for scanning austrian 12-digit payment reference number (_Kundendaten_).

### <a name="photopay_parsers_bih"></a> Bosnia and Herzegovina

#### <a name="photopay_parsers_bih_account"></a> Account number from Bosnia and Herzegovina parser

The [`BosniaAndHerzegovinaAccountParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/bih/account/BosniaAndHerzegovinaAccountParser.html) is used for scanning account number from Bosnia and Herzegovina.

#### <a name="photopay_parsers_bih_reference"></a> Payment reference number from Bosnia and Herzegovina parser

The [`BosniaAndHerzegovinaReferenceParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/bih/reference/BosniaAndHerzegovinaReferenceParser.html) is used for scanning payment reference number from Bosnia and Herzegovina.

### <a name="photopay_parsers_croatia"></a> Croatia

#### <a name="photopay_parsers_croatia_amount"></a> Croatian amount parser

The [`CroatiaAmountParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/croatia/amount/CroatiaAmountParser.html) is used for scanning amounts from payment slips and invoices issued in Croatia.

#### <a name="photopay_parsers_croatia_reference"></a> Croatian payment reference number parser

The [`CroatiaReferenceParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/croatia/reference/CroatiaReferenceParser.html) is used for scanning Croatian payment reference numbers.

### <a name="photopay_parsers_czechia"></a> Czechia

#### <a name="photopay_parsers_czechia_account"></a> Czech account number parser

The [`CzechiaAccountParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/czechia/account/CzechiaAccountParser.html) is used for scanning Czech account numbers.

#### <a name="photopay_parsers_czechia_varsymbol"></a> Czech Variabilni Symbol parser

The [`CzechiaVariabilniSymbolParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/czechia/account/CzechiaVariabilniSymbolParser.html) is used for scanning Czech _variabilni symbol_ number.

### <a name="photopay_parsers_germany"></a> Germany

#### <a name="photopay_parsers_germany_reference"></a> German payment reference number parser

The [`GermanyReferenceParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/germany/reference/GermanyReferenceParser.html) is used for scanning both German 13-digit payment reference numbers with ISO 7064 check digits as well as RF payment reference numbers.

### <a name="photopay_parsers_hungary"></a> Hungary

#### <a name="photopay_parsers_hungary_account"></a> Hungarian account number parser

The [`HungaryAccountParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/hungary/account/HungaryAccountParser.html) is used for scanning hungarian account number.

#### <a name="photopay_parsers_hungary_payer_id"></a> Hungarian payer ID number parser

The [`HungaryPayerIdParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/hungary/payerid/HungaryPayerIdParser.html) is used for scanning hungarian payer ID number.

### <a name="photopay_parsers_macedonia"></a> Macedonia

#### <a name="photopay_parsers_macedonia_account"></a> Macedonian account number parser

The [`MacedoniaAccountParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/macedonia/account/MacedoniaAccountParser.html) is used for scanning macedonian account numbers.

#### <a name="photopay_parsers_macedonia_reference"></a> Macedonian payment reference number parser

The [`MacedoniaReferenceParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/macedonia/reference/MacedoniaReferenceParser.html) is used for scanning Macedonian payment reference numbers.

### <a name="photopay_parsers_montenegro"></a> Montenegro

#### <a name="photopay_parsers_montenegro_account"></a> Montenegro account number parser

The [`MontenegroAccountParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/montenegro/account/MontenegroAccountParser.html) is used for scanning account numbers in Montenegro.

#### <a name="photopay_parsers_montenegro_reference"></a> Montenegro payment reference number parser

The [`MontenegroReferenceParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/montenegro/reference/MontenegroReferenceParser.html) is used for scanning payment reference numbers in Montenegro.

### <a name="photopay_parsers_serbia"></a> Serbia

#### <a name="photopay_parsers_serbia_account"></a> Serbian account number parser

The [`SerbiaAccountParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/serbia/account/SerbiaAccountParser.html) is used for scanning serbian account numbers.

#### <a name="photopay_parsers_serbia_reference"></a> Serbian payment reference number parser

The [`SerbiaReferenceParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/serbia/reference/SerbiaReferenceParser.html) is used for scanning serbian payment reference numbers.

### <a name="photopay_parsers_slovenia"></a> Slovenia

#### <a name="photopay_parsers_slovenia_reference"></a> Slovenian payment reference number parser

The [`SloveniaReferenceParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/slovenia/reference/SloveniaReferenceParser.html) is used for scanning slovenian payment reference numbers.

### <a name="photopay_parsers_sweden"></a> Sweden

#### <a name="photopay_parsers_sweden_amount"></a> Swedish amount parser

The [`SwedenAmountParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/sweden/amount/SwedenAmountParser.html) is used for scanning amount on swedish payment slip.

#### <a name="photopay_parsers_sweden_giro"></a> Swedish Giro number parser

The [`SwedenGiroNumberParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/sweden/giro/SwedenGiroNumberParser.html) is used for scanning giro number on swedish payment slip.

#### <a name="photopay_parsers_sweden_reference"></a> Swedish payment reference number parser

The [`SwedenReferenceParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/sweden/reference/SwedenReferenceParser.html) is used for scanning payment reference number on swedish payment slip.

#### <a name="photopay_parsers_sweden_slipcode"></a> Swedish slip code number parser

The [`SwedenSlipCodeParser`](https://photopay.github.io/photopay-android/com/microblink/entities/parsers/photopay/sweden/slipcode/SwedenSlipCodeParser.html) is used for scanning slip code number on swedish payment slip.

## <a name="barcodeRecognizer"></a> Barcode recognizer

The [`BarcodeRecognizer`](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/blinkbarcode/barcode/BarcodeRecognizer.html) is recognizer specialised for scanning various types of barcodes. This recognizer should be your first choice when scanning barcodes as it supports lots of barcode symbologies, including the [PDF417 2D barcodes](https://en.wikipedia.org/wiki/PDF417), thus making [PDF417 recognizer](#pdf417Recognizer) possibly redundant, which was kept only for its simplicity.

As you can see from [javadoc](https://photopay.github.io/photopay-android/com/microblink/entities/recognizers/blinkbarcode/barcode/BarcodeRecognizer.html), you can enable multiple barcode symbologies within this recognizer, however keep in mind that enabling more barcode symbologies affects scanning performance - the more barcode symbologies are enabled, the slower the overall recognition performance. Also, keep in mind that some simple barcode symbologies that lack proper redundancy, such as [Code 39](https://en.wikipedia.org/wiki/Code_39), can be recognized within more complex barcodes, especially 2D barcodes, like [PDF417](https://en.wikipedia.org/wiki/PDF417).

This recognizer can be used in any context, but it works best with the [`BarcodeScanActivity`](https://photopay.github.io/photopay-android/com/microblink/activity/BarcodeScanActivity.html), which has UI best suited for barcode scanning.
# <a name="embed-aar"></a> Embedding _PhotoPay_ inside another SDK
	
You need to ensure that the final app gets all resources required by _PhotoPay_. At the time of writing this documentation, [Android does not have support for combining multiple AAR libraries into single fat AAR](https://stackoverflow.com/questions/20700581/android-studio-how-to-package-single-aar-from-multiple-library-projects/20715155#20715155). The problem is that resource merging is done while building application, not while building AAR, so application must be aware of all its dependencies. **There is no official Android way of "hiding" third party AAR within your AAR.**

This problem is usually solved with transitive Maven dependencies, i.e. when publishing your AAR to Maven you specify dependencies of your AAR so they are automatically referenced by app using your AAR. Besides this, there are also several other approaches you can try:

- you can ask your clients to reference _PhotoPay_ in their app when integrating your SDK
- since the problem lies in resource merging part you can try avoiding this step by ensuring your library will not use any component from _PhotoPay_ that uses resources (i.e. built-in activities, fragments and views, except `RecognizerRunnerView`). You can perform [custom UI integration](#recognizer-runner-view) while taking care that all resources (strings, layouts, images, ...) used are solely from your AAR, not from _PhotoPay_. Then, in your AAR you should not reference `LibPhotoPay.aar` as gradle dependency, instead you should unzip it and copy its assets to your AAR’s assets folder, its `classes.jar` to your AAR’s lib folder (which should be referenced by gradle as jar dependency) and contents of its jni folder to your AAR’s src/main/jniLibs folder.
- Another approach is to use [3rd party unofficial gradle script](https://github.com/adwiv/android-fat-aar) that aim to combine multiple AARs into single fat AAR. Use this script at your own risk and report issues to [its developers](https://github.com/adwiv/android-fat-aar/issues) - we do not offer support for using that script.
- There is also a [3rd party unofficial gradle plugin](https://github.com/Vigi0303/fat-aar-plugin) which aims to do the same, but is more up to date with latest updates to Android gradle plugin. Use this plugin at your own risk and report all issues with using to [its developers](https://github.com/Vigi0303/fat-aar-plugin/issues) - we do not offer support for using that plugin.

# <a name="archConsider"></a> Processor architecture considerations

_PhotoPay_ is distributed with **ARMv7**, **ARM64**, **x86** and **x86_64** native library binaries.

**ARMv7** architecture gives the ability to take advantage of hardware accelerated floating point operations and SIMD processing with [NEON](http://www.arm.com/products/processors/technologies/neon.php). This gives _PhotoPay_ a huge performance boost on devices that have ARMv7 processors. Most new devices (all since 2012.) have ARMv7 processor so it makes little sense not to take advantage of performance boosts that those processors can give. Also note that some devices with ARMv7 processors do not support NEON and VFPv4 instruction sets, most popular being those based on [NVIDIA Tegra 2](https://en.wikipedia.org/wiki/Tegra#Tegra_2), [ARM Cortex A9](https://en.wikipedia.org/wiki/ARM_Cortex-A9) and older. Since these devices are old by today's standard, _PhotoPay_ does not support them. For the same reason, _PhotoPay_ does not support devices with ARMv5 (`armeabi`) architecture.

**ARM64** is the new processor architecture that most new devices use. ARM64 processors are very powerful and also have the possibility to take advantage of new NEON64 SIMD instruction set to quickly process multiple pixels with a single instruction.

**x86** and **x86_64** architectures are used on very few devices today, most of them are manufactured before 2015, like [Asus Zenfone 4](http://www.gsmarena.com/asus_zenfone_4-5951.php) and they take about 1% of all devices, according to the Device catalog on Google Play Console. Some x86 and x86_64 devices have ARM emulator, but running the _PhotoPay_ on the emulator will give a huge performance penalty.

There are some issues to be considered:

- ARMv7 build of the native library cannot be run on devices that do not have ARMv7 compatible processor
- ARMv7 processors do not understand x86 instruction set
- x86 processors understand neither ARM64 nor ARMv7 instruction sets
- some x86 android devices ship with the builtin [ARM emulator](http://commonsware.com/blog/2013/11/21/libhoudini-what-it-means-for-developers.html) - such devices are able to run ARM binaries but with a performance penalty. There is also a risk that the builtin ARM emulator will not understand some specific ARM instruction and will crash.
- ARM64 processors understand ARMv7 instruction set, but ARMv7 processors do not understand ARM64 instructions. 
    - <a name="64bitNotice"></a> **NOTE:** as of the year 2018, some android devices that ship with ARM64 processors do not have full compatibility with ARMv7. This is mostly due to incorrect configuration of Android's 32-bit subsystem by the vendor, however Google decided that as of August 2019 all apps on PlayStore that contain native code need to have native support for 64-bit processors (this includes ARM64 and x86_64) - this is in anticipation of future Android devices that will support 64-bit code **only**, i.e. that will have ARM64 processors that do not understand ARMv7 instruction set.
- if ARM64 processor executes ARMv7 code, it does not take advantage of modern NEON64 SIMD operations and does not take advantage of 64-bit registers it has - it runs in emulation mode
- x86_64 processors understand x86 instruction set, but x86 processors do not understand x86_64 instruction set
- if x86_64 processor executes x86 code, it does not take advantage of 64-bit registers and use two instructions instead of one for 64-bit operations

`LibPhotoPay.aar` archive contains ARMv7, ARM64, x86 and x86_64 builds of the native library. By default, when you integrate _PhotoPay_ into your app, your app will contain native builds for all these processor architectures. Thus, _PhotoPay_ will work on ARMv7, ARM64, x86 and x86_64 devices and will use ARMv7 features on ARMv7 devices and ARM64 features on ARM64 devices. However, the size of your application will be rather large.

## <a name="reduceSize"></a> Reducing the final size of your app

We recommend that you distribute your app using [App Bundle](https://developer.android.com/platform/technology/app-bundle). This will defer apk generation to Google Play, allowing it to generate minimal APK for each specific device that downloads your app, including only required processor architecture support.

### Using APK splits

If you are unable to use App Bundle, you can create multiple flavors of your app - one flavor for each architecture. With gradle and Android studio this is very easy - just add the following code to `build.gradle` file of your app:

```
android {
  ...
  splits {
    abi {
      enable true
      reset()
      include 'x86', 'armeabi-v7a', 'arm64-v8a', 'x86_64'
      universalApk true
    }
  }
}
```

With that build instructions, gradle will build four different APK files for your app. Each APK will contain only native library for one processor architecture and one APK will contain all architectures. In order for Google Play to accept multiple APKs of the same app, you need to ensure that each APK has different version code. This can easily be done by defining a version code prefix that is dependent on architecture and adding real version code number to it in following gradle script:

```
// map for the version code
def abiVersionCodes = ['armeabi-v7a':1, 'arm64-v8a':2, 'x86':3, 'x86_64':4]

import com.android.build.OutputFile

android.applicationVariants.all { variant ->
    // assign different version code for each output
    variant.outputs.each { output ->
        def filter = output.getFilter(OutputFile.ABI)
        if(filter != null) {
            output.versionCodeOverride = abiVersionCodes.get(output.getFilter(OutputFile.ABI)) * 1000000 + android.defaultConfig.versionCode
        }
    }
}
```

For more information about creating APK splits with gradle, check [this article from Google](https://developer.android.com/studio/build/configure-apk-splits.html#configure-abi-split).

After generating multiple APK's, you need to upload them to Google Play. For tutorial and rules about uploading multiple APK's to Google Play, please read the [official Google article about multiple APKs](https://developer.android.com/google/play/publishing/multiple-apks.html).

### Removing processor architecture support

If you won't be distributing your app via Google Play or for some other reasons want to have single APK of smaller size, you can completely remove support for certain CPU architecture from your APK. **This is not recommended due to [consequences](#archConsequences)**.

To keep only some CPU architectures, for example `armeabi-v7a` and `arm64-v8a`, add the following statement to your `android` block inside `build.gradle`:

```
android {
    ...
    ndk {
        // Tells Gradle to package the following ABIs into your application
        abiFilters 'armeabi-v7a', 'arm64-v8a'
    }
}
```

This will remove other architecture builds for **all** native libraries used by the application.

To remove support for a certain CPU architecture only for _PhotoPay_, add the following statement to your `android` block inside `build.gradle`:

```
android {
    ...
    packagingOptions {
        exclude 'lib/<ABI>/libBlinkPhotoPay.so'
    }
}
```

where `<ABI>` represents the CPU architecture you want to remove:

- to remove ARMv7 support, use `exclude 'lib/armeabi-v7a/libBlinkPhotoPay.so'`
- to remove x86 support, use `exclude 'lib/x86/libBlinkPhotoPay.so'`
- to remove ARM64 support, use `exclude 'lib/arm64-v8a/libBlinkPhotoPay.so'`
    - **NOTE**: this is **not recommended**. See [this notice](#64bitNotice).
- to remove x86_64 support, use `exclude 'lib/x86_64/libBlinkPhotoPay.so'`

You can also remove multiple processor architectures by specifying `exclude` directive multiple times. Just bear in mind that removing processor architecture will have side effects on performance and stability of your app. Please read [this](#archConsequences) for more information.

### <a name="archConsequences"></a> Consequences of removing processor architecture

- Google decided that as of August 2019 all apps on Google Play that contain native code need to have native support for 64-bit processors (this includes ARM64 and x86_64). This means that you cannot upload application to Google Play Console that supports only 32-bit ABI and does not support corresponding 64-bit ABI.

- By removing ARMv7 support, _PhotoPay_ will not work on devices that have ARMv7 processors. 
- By removing ARM64 support, _PhotoPay_ will not use ARM64 features on ARM64 device
    - also, some future devices may ship with ARM64 processors that will not support ARMv7 instruction set. Please see [this note](#64bitNotice) for more information.
- By removing x86 support, _PhotoPay_ will not work on devices that have x86 processor, except in situations when devices have ARM emulator - in that case, _PhotoPay_ will work, but will be slow and possibly unstable
- By removing x86_64 support, _PhotoPay_ will not use 64-bit optimizations on x86_64 processor, but if x86 support is not removed, _PhotoPay_ should work


## <a name="combineNativeLibraries"></a> Combining _PhotoPay_ with other native libraries

If you are combining _PhotoPay_ library with other libraries that contain native code into your application, make sure you match the architectures of all native libraries. For example, if third party library has got only ARMv7 and x86 versions, you must use exactly ARMv7 and x86 versions of _PhotoPay_ with that library, but not ARM64. Using these architectures will crash your app at initialization step because JVM will try to load all its native dependencies in same preferred architecture and will fail with `UnsatisfiedLinkError`.
# <a name="troubleshoot"></a> Troubleshooting

### Integration difficulties

In case of problems with SDK integration, first make sure that you have followed [integration instructions](#android-studio-integration). If you're still having problems, please contact us at [help.microblink.com](http://help.microblink.com).

### Licensing issues

If you are getting "invalid license key" error or having other license-related problems (e.g. some feature is not enabled that should be or there is a watermark on top of camera), first check the ADB logcat. All license-related problems are logged to error log so it is easy to determine what went wrong.

When you have to determine what is the license-relate problem or you simply do not understand the log, you should contact us [help.microblink.com](http://help.microblink.com). When contacting us, please make sure you provide following information:

* exact package name of your app (from your `AndroidManifest.xml` and/or your `build.gradle` file)
* license that is causing problems
* please stress out that you are reporting problem related to Android version of _PhotoPay_ SDK
* if unsure about the problem, you should also provide excerpt from ADB logcat containing license error

**Keep in mind:** Versions 7.11.0 and above require an internet connection to work under our new License Management Program.

We’re only asking you to do this so we can validate your trial license key. Data extraction still happens offline, on the device itself.
Once the validation is complete, you can continue using the SDK in offline mode (or over a private network) until the next check. 

### Other problems

If you are having problems with scanning certain items, undesired behaviour on specific device(s), crashes inside _PhotoPay_ or anything unmentioned, please do as follows:

* enable logging to get the ability to see what is library doing. To enable logging, put this line in your application:

    ```java
    com.microblink.photopay.util.Log.setLogLevel(com.microblink.photopay.util.Log.LogLevel.LOG_VERBOSE);
    ```

    After this line, library will display as much information about its work as possible. Please save the entire log of scanning session to a file that you will send to us. It is important to send the entire log, not just the part where crash occurred, because crashes are sometimes caused by unexpected behaviour in the early stage of the library initialization.
    
* Contact us at [help.microblink.com](http://help.microblink.com) describing your problem and provide following information:
    * log file obtained in previous step
    * high resolution scan/photo of the item that you are trying to scan
    * information about device that you are using - we need exact model name of the device. You can obtain that information with any app like [this one](https://play.google.com/store/apps/details?id=ru.andr7e.deviceinfohw)
    * please stress out that you are reporting problem related to Android version of _PhotoPay_ SDK


# <a name="faq"></a> FAQ and known issues
#### <a name="feature-not-supported-by-license-key"></a> After switching from trial to production license I get `InvalidLicenseKeyException` when I construct specific `Recognizer` object

Each license key contains information about which features are allowed to use and which are not. This exception indicates that your production license does not allow using of specific `Recognizer` object. You should contact [support](http://help.microblink.com) to check if provided license is OK and that it really contains all features that you have purchased.

#### <a name="invalid-license-key"></a> I get `InvalidLicenseKeyException` with trial license key

Whenever you construct any `Recognizer` object or any other object that derives from [`Entity`](https://photopay.github.io/photopay-android/com/microblink/entities/Entity.html), a check whether license allows using that object will be performed. If license is not set prior constructing that object, you will get `InvalidLicenseKeyException`. We recommend setting license as early as possible in your app, ideally in `onCreate` callback of your [Application singleton](https://developer.android.com/reference/android/app/Application.html).

#### <a name="missing-resources"></a> When my app starts, I get exception telling me that some resource/class cannot be found or I get `ClassNotFoundException`

This usually happens when you perform integration into Eclipse project and you forget to add resources or native libraries into the project. You must alway take care that same versions of both resources, assets, java library and native libraries are used in combination. Combining different versions of resources, assets, java and native libraries will trigger crash in SDK. This problem can also occur when you have performed improper integration of _PhotoPay_ SDK into your SDK. Please read how to [embed _PhotoPay_ inside another SDK](#embed-aar).

#### <a name="unsatisfied-link-error"></a> When my app starts, I get `UnsatisfiedLinkError`

This error happens when JVM fails to load some native method from native library If performing integration into Android studio and this error happens, make sure that you have correctly combined _PhotoPay_ SDK with [third party SDKs that contain native code](#combine-native-libraries). If this error also happens in our integration sample apps, then it may indicate a bug in the SDK that is manifested on specific device. Please report that to our [support team](http://help.microblink.com).

#### <a name="late-metadata1"></a> I've added my callback to `MetadataCallbacks` object, but it is not being called

Make sure that after adding your callback to `MetadataCallbacks` you have applied changes to `RecognizerRunnerView` or `RecognizerRunner` as described in [this section](#processing-events).

#### <a name="late-metadata2"></a> I've removed my callback to `MetadataCallbacks` object, and now app is crashing with `NullPointerException`

Make sure that after removing your callback from `MetadataCallbacks` you have applied changes to `RecognizerRunnerView` or `RecognizerRunner` as described in [this section](#processing-events).

#### <a name="stateful-recognizer"></a> In my `onScanningDone` callback I have the result inside my `Recognizer`, but when scanning activity finishes, the result is gone

This usually happens when using `RecognizerRunnerView` and forgetting to pause the `RecognizerRunnerView` in your `onScanningDone` callback. Then, as soon as `onScanningDone` happens, the result is mutated or reset by additional processing that `Recognizer` performs in the time between end of your `onScanningDone` callback and actual finishing of the scanning activity. For more information about statefulness of the `Recognizer` objects, check [this section](#recognizer-concept).

#### <a name="transaction-too-large"></a> I am using built-in activity to perform scanning and after scanning finishes, my app crashes with `IllegalStateException` stating `Data cannot be saved to intent because its size exceeds intent limit`.

This usually happens when you use `Recognizer` that produces image or similar large object inside its `Result` and that object exceeds the Android intent transaction limit. You should enable different intent data transfer mode. For more information about this, [check this section](#intent-optimization). Also, instead of using built-in activity, you can use [`RecognizerRunnerFragment` with built-in scanning overlay](#recognizerRunnerFragment).

#### <a name="transaction-too-large2"></a> After scanning finishes, my app freezes

This usually happens when you attempt to transfer standalone `Result` that contains images or similar large objects via Intent and the size of the object exceeds Android intent transaction limit. Depending on the device, you will get either [TransactionTooLargeException](https://developer.android.com/reference/android/os/TransactionTooLargeException.html), a simple message `BINDER TRANSACTION FAILED` in log and your app will freeze or your app will get into restart loop. We recommend that you use `RecognizerBundle` and its API for sending `Recognizer` objects via Intent in a more safe manner ([check this section](#intent-optimization) for more information). However, if you really need to transfer standalone `Result` object (e.g. `Result` object obtained by cloning `Result` object owned by specific `Recognizer` object), you need to do that using global variables or singletons within your application. Sending large objects via Intent is not supported by Android.

#### <a name="direct-api-bad-performance"></a> Scanning with a camera works better than a recognition of images by using the `Direct API`

When automatic scanning of camera frames with our camera management is used (provided camera overlays or direct usage of `RecognizerRunnerView`), we use a stream of video frames and send multiple images to the recognition to boost reading accuracy. Also, we perform frame quality analysis and combine scanning results from multiple camera frames. On the other hand, when you are using the Direct API with a single image per document side, we cannot combine multiple images. We do our best to extract as much information as possible from that image. In some cases, when the quality of the input image is not good enough, for example, when the image is blurred or when glare is present, we are not able to successfully read the document.

#### <a name="network-required-error"></a> I am getting a ‘Network required’ error when I'm on a private network

Online trial licenses require a public network access for validation purposes. See [Licensing issues](#licensing-issues).

#### <a name="ocr-result-forbidden"></a> `onOcrResult()` method in my `OcrCallback` is never invoked and all `Result` objects always return `null` in their OCR result getters

In order to be able to obtain raw OCR result, which contains locations of each character, its value and its alternatives, you need to have a license that allows that. By default, licenses do not allow exposing raw OCR results in public API. If you really need that, please [contact us](https://help.microblink.com) and explain your use case.
# <a name="info"></a> Additional info

## <a name="size-report"></a> PhotoPay SDK size
You can find PhotoPay SDK size report for all supported ABIs [here](size-report/sdk_size_report.md).

## <a name="api-reference"></a> API reference
Complete API reference can be found in [Javadoc](https://photopay.github.io/photopay-android).

## <a name="contact"></a> Contact
For any other questions, feel free to contact us at [help.microblink.com](http://help.microblink.com).
